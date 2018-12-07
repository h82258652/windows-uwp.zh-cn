---
title: 调用 Xbox Live 的最佳实践
description: 了解调用 Xbox Live API 的最佳实践。
ms.assetid: f4c7156b-7736-41e5-9b3d-e87cc8dd2531
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, 最佳实践
ms.localizationpriority: medium
ms.openlocfilehash: 55e05ef7de2e2981f9f5af86623a8d8413ce2c99
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/07/2018
ms.locfileid: "8782879"
---
# <a name="best-practices-for-calling-xbox-live"></a>调用 Xbox Live 的最佳实践

可以通过两种主要方式调用 Xbox Live 服务：使用 Xbox 服务 API (XSAPI)，或者直接调用 REST 端点。 无论代码如何调用 Xbox Live，必须具有正确的调用模式和重试逻辑。

若要了解如何编写正确的重试逻辑，必须了解两类 REST 端点 - **幂等**和**非幂等**。 我们将在下面分别讨论这两类端点
 
## <a name="non-idempotent-endpoints"></a>非幂等端点

在重复调用时会产生副作用的 HTTP 方法被视为是**非幂等**。 这意味着，如果客户端将调用此端点且发生网络超时，则重试此方法不安全，因为资源可能已更新，但网络无法通知已成功的调用方。 发生错误时，客户端必须先查询以确定调用是否成功，而不是重试。 仅当调用不成功时才应重试。

在 Xbox 服务 API 中，某些 API 已在内部标记为调用非幂等端点。 这意味着，如果在调用这些端点时发生故障，则 API 不会自动重试端点。

完整的非幂等 API 列表如下：

* game\_server\_platform\_service::allocate\_cluster()
<br>
* game\_server\_platform\_service::allocate\_cluster\_inline()
<br>
* game\_server\_platform\_service::allocate\_session\_host()
<br>
* matchmaking\_service::create\_match\_ticket()
<br>
* multiplayer\_service::write\_session()
<br>
* multiplayer\_service::write\_session\_by\_handle()
<br>
* multiplayer\_service::send\_invites()
<br>
* reputation\_service::submit\_batch\_reputation\_feedback()
<br>
* reputation\_service::submit\_reputation\_feedback()
 

## <a name="idempotent-methods"></a>幂等方法

**幂等**从另一方面来说，HTTP 方法没有副作用。 这反过来意味着可以安全地重试。 在 Xbox 服务 API 中，在特定条件下，将会自动重试所有幂等方法。

完整的幂等 API 列表为上面未列为非幂等 API 的所有 API。


## <a name="retry-logic-best-practices"></a>重新逻辑最佳实践

对于幂等调用，应该会自动重试这些条件：

* 所有网络错误
* 401：未授权
* 408：请求超时
* 429：请求过多
* 500：内部错误
* 502：网关错误
* 503：服务不可用
* 504：网关超时


在 UWP 上，401：对未授权将会特殊处理。 这表示 Xbox Live 身份验证令牌已过期，因此 Xbox 服务 API 将调用操作系统以刷新令牌并作为单次重试执行。

执行重试时，在达到“重试后”标头时间之前，最好不好调用此服务。 XSAPI 现在实施此最佳实践。 如果 HTTP 状态代码发生故障且任何 API 返回“重试后”标头，则在重试后时间之前对此相同 API 的其他调用将会立即返回原始错误，且不会使用此服务。

重试调用时，最佳实践是通过随机抖动执行指数后退，以便将负载扩展到该服务。 XSAPI 以 2 秒默认延迟开始，默认延迟使用 xbox\_live\_context\_settings::set\_http\_retry\_delay() 来控制。 这意味着，默认情况下，每次重试以 2、4、8 秒（以此类推）的指数后退，并且将根据响应时间抖动当前与下一次后退值之间的延迟，以便将负载进一步扩展到尝试重试的设备集中。

游戏应控制重试调用所花的时间。 使用 XSAPI 时，开发人员通过使用函数 xbox\_live\_context\_settings::set\_http\_timeout\_window() 直接对此进行控制。 默认情况下，此值设为 20 秒。 将此值设置为 0 秒将有效关闭重试逻辑。 XSAPI 现在还可以根据 http\_timeout\_window() 中所剩的时间来动态调整内部 HTTP 超时。 内部 HTTP 超时控制操作系统在中止之前进行 HTTP 网络操作所花的时间。 除非 http\_timeout\_window() 中所剩的时间至少为 5 秒，否则不会重试调用，这样就提供有足够合理的时间来完成调用。 此规则不适用于第一次调用，因此将 http\_timeout\_window() 设为 0 可接受，并且将发生单次调用。 此逻辑的作用是当 API 调用返回时 http\_timeout\_window() 更具确定性。 如果返回“重试后”标头，则在达到“重试后”时间之前不会进行任何重试。 如果“重试后”时间在 http\_timeout\_window() 之后，则调用将在 http\_timeout\_window() 结束时返回。


## <a name="error-handling"></a>错误处理

游戏开发人员在**每次**服务调用时应**始终**使用正确的错误处理，并且需要确保正确处理故障响应。
 
许多的实际情况可能会导致请求 Xbox Live 返回故障代码，如

1.  网络不可用。 例如，设备丢失 4G、丢失 WLAN 或网络关闭。
2.  服务上的多个负载过载 (503)
3.  服务上发生故障 (500)
4.  发送给服务的请求过多 (429)
5.  写操作冲突 (412)。 例如，多人游戏会话中的另一个玩家先提交了更改
6.  用户已被禁止或没有权限
7.  用户已注销

正确的错误处理程序对于确保游戏在这些条件下正确运行非常重要。

XSAPI 具有两类错误处理模式。 通过 C++/CX、C\# 或 Javascript 使用 WinRT API 时运用一类模式，使用新 C++ API 时运用另一类模式。 有关错误处理最佳实践的完整详细信息，请参阅 Xbox Live 文档页“错误处理”，有关介绍此内容的视频，请参阅 [*Xfest 2015 视频*](https://developer.xboxlive.com/en-us/platform/documentlibrary/events/Pages/Xfest2015.aspx) 中名为 *XSAPI：C++，无异常！* 的访谈。


## <a name="best-calling-patterns"></a>最佳调用模式

### <a name="usebatching-requests"></a>Usebatching 请求

某些端点支持将一组请求批处理或聚合为单次调用。 例如，借助 Xbox Live 个人资料服务，你可以请求单个用户的个人资料或一组用户的个人资料。 因此，如果你需要一组用户的个人资料，那么，一次调用每个用户个人资料的一个端点或 API 的效率就非常低。 每次调用都会增加大量身份验证开销。 相反，将你需要其相关信息的所有用户一次传递至 API，使得端点可以同时处理所有用户个人资料并返回单个响应。

### <a name="use-the-real-time-activity-rta-service-instead-of-polling"></a>使用实时活动 (RTA) 服务而不是轮询

另一个最佳实践是使用实时活动 (RTA) 服务而不是定期轮询。 此服务将会公开一个 Websocket，后者会在服务上的目标资源更改时向客户端发送通知。 RTA 服务将提供与在线状态更改、统计更改、多人游戏会话文档更改和社会关系更改相关的通知。 若要了解客户端感兴趣的信息，客户端必须先通过 Websocket 订阅项目。 这可避免通过轮询服务检测更改，因为系统会告知你项目更改的准确时间。

XSAPI 将 RTA 服务作为客户端可使用的一组订阅 API 公开。 这些 API 每个均具有对应的 \*\_changed\_handler API，它们使用项目更改时将会调用的回调函数。

* presence\_service::subscribe\_to\_device\_presence\_change
<br>
* presence\_service::subscribe\_to\_title\_presence\_change
<br>
* user\_statistics\_service::subscribe\_to\_statistic\_change
<br>
* social\_service::subscribe\_to\_social\_relationship\_change<br>
 

## <a name="use-xbox-live-client-side-managers"></a>使用 Xbox Live 客户端侧管理器

作为 XSAPI 中的一项新功能，我们现在可提供一组管理器，用作在特定情况下负责繁重工作的缓存和状态机。


### <a name="social-manager"></a>社交管理器

社交管理器负责与好友列表和个人资料相关的所有繁重工作。 它将使用 RTA 服务使你的好友列表、好友个人资料及其在线状态数据保持最新。 该管理器将公开一个对游戏引擎非常友好的同步 API。 游戏可以频繁调用其 API，因为它可以通过服务维持内存内缓存的最新信息。 有关更多信息，请参阅 Xbox Live 文档页“社交管理器简介”

### <a name="multiplayer-manager"></a>多人游戏管理器

对于多人游戏会话管理，多人游戏管理器是一个适合于传统多人游戏的普适型解决方案。 多人游戏管理器 API 包括玩家名单和会话管理、处理游戏邀请、加入游戏、比赛和插入现有的网络解决方案。 它负责与实施传统多人游戏流相关的繁重工作。 有关更多信息，请参阅 Xbox Live 文档页“多人游戏管理器简介”


## <a name="throttling-fine-grained-rate-limiting"></a>节流（细化速率限制）

Xbox Live 服务具有适当的节流机制，用于防止单个设备对服务施加极端负载。 必须了解游戏被节流的时间。 你可以通过不同的方式确定游戏是否已被节流：


### <a name="monitor-for-http-status-code-429"></a>监视是否存在 HTTP 状态代码 429

你可以使用 Fiddler 并监视是否返回 HTTP 状态代码 429。 JSON 响应将包含与端点如何被节流相关的详细信息。 例如：

```json
{
  "version":1,
  "currentRequests":13,
  "maxRequests":10,
  "periodInSeconds":120,
  "limitType":"Rate"
}
```

如果你使用 XSAPI，则 API 将返回 http\_status\_429\_too\_many\_requests 错误并设置错误消息以说明与 API 节流方法相关的详细信息。

### <a name="debug-asserts"></a>调试断言

使用 XSAPI 时，如果调用在处于开发人员沙盒中时被节流且使用的是调试版本的游戏，则它将断言立即让开发人员知悉已发生节流。 这是为了避免因不正确的编写代码而意外丢失 429 节流错误。 如果你想要在不修复问题代码的情况下禁用这些断言以继续工作，则你可以调用此 API：


> xboxLiveContext-&gt;settings()-&gt;disable\_asserts\_for\_xbox\_live\_throttling\_in\_dev\_sandboxes( xbox\_live\_context\_throttle\_setting::this\_code\_needs\_to\_be\_changed\_to\_avoid\_throttling )；

但是，请注意，此 API 不会阻止游戏被节流。 你的游戏仍将被节流。 这只会在使用调试版本时在开发人员沙盒中禁用断言。 

### <a name="xbox-live-trace-analyzer-tool"></a>Xbox Live 分析跟踪器工具

另一个选择是记录 Xbox Live 调用的轨迹，然后使用 [Xbox Live 跟踪分器其工具](https://docs.microsoft.com/windows/uwp/xbox-live/tools/analyze-service-calls)分析该轨迹。

若要记录轨迹，你可以使用 Fiddler 记录 .SAZ 文件，或使用 XSAPI 内置轨迹日志记录。 有关如何在 XSAPI 中打开跟踪的更多信息，请参阅 Xbox Live 文档页“分析 Xbox Live 服务调用”。 获得轨迹之后，Xbox Live 跟踪分析器工具将在检测到节流的调用时发出警告。

## <a name="is-xbox-live-up"></a>Xbox Live 是否已启动？

Xbox Live 是一系列微服务集合，用于公开 Xbox Live 功能，如个人资料、好友和在线状态、统计数据、排行榜、成就、多人游戏和比赛。 没有单个服务器或端点用于定义 Xbox Live 是否已启动。 当某个服务器关闭时，其余的 Xbox Live 微服务在很大程度上是独立的并且应该可以正常运行。

如果单个服务遇到临时中断，则必须知道此服务调用对你的游戏来说是否是任务关键型。 虽然存在间歇性网络或服务问题，但我们仍尝试提供合理体验。 例如，如果在线状态服务返回故障，则该调用对你的游戏来说可能不是任务关键型。 因此，只需向用户报告最近一次已知的在线状态即可，无需报告 Xbox Live 已关闭。

Xbox Live 也遵循最终一致性的一致性模型。 这意味着，如果没有新的更新，则对该资源的所有最终请求将报告最后更新的值。 这实际上意味着数据传播时在一小段时间内信息是过时的。
