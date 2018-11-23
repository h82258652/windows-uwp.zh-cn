---
title: Multiplayer 2015 常见问题和疑难解答
author: KevinAsgari
description: 有关 Xbox Live 多人游戏 2015 的常见问题和疑难解答。
ms.assetid: 75823f10-b342-4e20-b885-e5ad4392bc3d
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, 多人游戏
ms.localizationpriority: medium
ms.openlocfilehash: e9c6778ebc90d917a883a437d905112cf32a6829
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2018
ms.locfileid: "7559239"
---
# <a name="multiplayer-2015-faq-and-troubleshooting"></a>Multiplayer 2015 常见问题和疑难解答

-   我正在开发新作品。 我应该使用哪些多人游戏 API 元素？
-   我如何从服务访问新的多人游戏 API？
-   我的作品可以为多个会话订阅更改么？
-   如果用户丢失了网络连接或关闭了主机，他/她会被立即删除么？
-   如何查找要使用的 SCID、会话模板和沙盒？
-   是否有一个请求正文的示例，我可以将自己作品的请求正文与其相比？
-   调用 MPSD 时我收到了 HTTP/403 代码。
-   调用 MPSD 时我收到了 HTTP/404 代码。
-   为何我在调用 **MultiplayerService.WriteSessionByHandleAsync** 或 **MultiplayerService.GetCurrentSessionByHandleAsync** 时收到了 HTTP/404 代码？
-   调用 MPSD 时我收到了 HTTP/412 代码。
-   调用 MPSD 时我收到了 HTTP/400、405、409、503 等代码。
-   可以或应当在会话模板中为我的作品做哪些修改？
-   我收到一条指明我的会话未进行初始化的错误。
-   我的会话未创建且我收到了 HTTP/204 代码。
-   我应在何时轮询 MPSD？
-   如果为会话保留或被邀请加入会话的玩家未加入会怎样？
-   为何创建的会话不会通过匹配找到？
-   2015 多人游戏正确使用派对的方式与 2014 多人游戏中的使用方式之间的主要区别是什么？
-   如果游戏会话有玩家空位且支持进行中加入，为何用户在会话开始后无法找到会话？
-   如果游戏会话是开放的，刚加入游戏的用户可以只是加入会话就开始玩游戏，而无需等待保留么？
-   当大型游戏会话在我的游戏中进行时，为何不是所有成员都能看到游戏邀请 toast？
-   我看到了不一致的游戏行为，并收到了引用游戏派对功能的协议激活信息。
-   我已经配置了 v107 会话模板，为何我还会在我的跟踪中看到 v105 会话文档的语法？


### <a name="i-am-developing-a-new-title-which-multiplayer-api-elements-should-i-use"></a>我正在开发新作品。 我应该使用哪些多人游戏 API 元素？

2014 多人游戏功能将继续适用于现有游戏，但关联的 API 元素很有可能被弃用。 在 2015 中准备客户端发布时，我们强烈建议使用 2015 多人游戏。


### <a name="how-can-i-access-the-new-multiplayer-api-from-a-service"></a>我如何从服务访问新的多人游戏 API？

请参阅[调用 MPSD](multiplayer-session-directory.md)。


### <a name="can-my-title-subscribe-to-changes-for-more-than-one-session"></a>我的作品可以为多个会话订阅更改么？

可以，作品可以订阅接收每个连接最多 10 个会话的更改。


### <a name="will-a-user-be-removed-immediately-if-heshe-loses-a-network-connection-or-turns-off-the-console"></a>如果用户丢失了网络连接或关闭了主机，他/她会被立即删除么？

Web 套接字连接允许 MPSD 快速检测客户端的断开连接情况，并将客户端设置为非活动。 会话成员将在非活动删除超时到期后被立即删除。 有关详细信息，请参阅[会话超时](mpsd-session-details.md)。


### <a name="how-do-i-find-out-what-scid-session-template-and-sandbox-to-use"></a>如何查找要使用的 SCID、会话模板和沙盒？

如果你未进行作品的初始注册，在创建此信息时，你将没有访问此信息的权限。 请联系你的开发者帐户管理员，他可以为你获取此数据。


### <a name="is-there-an-example-of-a-request-body-that-i-can-compare-to-the-one-for-my-title"></a>是否有一个请求正文的示例，我可以将自己作品的请求正文与其相比？

请参见 **MultiplayerSessionRequest (JSON)** 中的请求结构


### <a name="i-am-getting-an-http403-code-when-calling-mpsd"></a>调用 MPSD 时我收到了 HTTP/403 代码。

这通常是权限或范围问题。 收集 Fiddler 跟踪以获取详细信息，然后查看作为 HttpResponse 正文的一部分返回的消息了解常见的 403 消息：

-   无法访问请求的服务配置。
    1.  确认你所使用的帐户是否有权访问沙盒。
    2.  确认你是否位于正确的沙盒内。
     3.  如果你正在使用证书身份验证并收到此错误，请联系你的开发者帐户管理员。   无法访问请求的会话。 调用用户必须具有多人游戏的权限，私人会话只能由会话成员读取。

    你无法看到该会话，可能是因为你尝试访问的会话的可见性为“私人”。 如果将可见性设为“私人”，只有该会话的成员才可以读取会话文档。

| 注意                                                                                                                                                  |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 用户必须有金会员帐户才能够注册新会话。 游戏用户没有金会员帐户权限，注册新会话的请求将返回 HTTP/403。 |

-   请求正文无法包含现有成员引用，除非身份验证主体包括服务器。

    你无法代表用户加入其他用户进行会话。 你只能邀请。 将索引设置为 "reserve\_&lt;number&gt;" 来邀请玩家。


### <a name="i-am-getting-an-http404-code-when-calling-mpsd"></a>调用 MPSD 时我收到了 HTTP/404 代码。

收集 Fiddler 跟踪以获取详细信息，然后执行下列操作：

1.  查看作为适用于常见 404 消息的 HttpResponse 正文的一部分返回的消息：
    -   请求的服务配置无效，或不是为会话配置的。 确保使用中的 SCID 是正确的。
    -   未找到请求的会话。 在检索会话前，确保会话已创建且会话模板是正确的。 你可以使用 PUT 调用创建会话。

2.  查看你所使用的 URI。
3.  重新启动主机或使用新用户尝试。
4.  查看游戏开发人员论坛了解错误代码或其他解决方案。
5.  确认会话未由于变空被删除。 会话可能由于用户超时而变空。如果发生这种情况，通常是因为所有会话成员都处于应用超时的状态，如“准备”或“非活动”。 请参阅[会话用户状态](mpsd-session-details.md)了解用户状态的详细信息。


### <a name="why-am-i-getting-an-http404-code-when-calling-multiplayerservicewritesessionbyhandleasync-or-multiplayerservicegetcurrentsessionbyhandleasync"></a>为何我在调用 **MultiplayerService.WriteSessionByHandleAsync** 或 **MultiplayerService.GetCurrentSessionByHandleAsync** 时收到了 HTTP/404 代码？

如果你的作品为响应包含句柄 ID 的加入协议激活通过句柄访问 MPSD 会话，协议激活中的句柄 ID 可能由于以下原因之一过期：

-   作品从中发起加入的 Xbox shell 中的 UI 视图可能已过期。 某些 UI 视图（例如，用户的个人资料卡）在打开时不会自动刷新加入句柄。 若要避免收到 HTTP/404 代码，作品应先关闭然后重新打开视图以在加入之前刷新数据。
-   作品正在加入的用户可能已在作品从 Xbox shell UI 选择了加入操作后切换了活动会话。 HTTP/404 代码的这个原因应该很少见。

在这两种情况下，作品代码都应该向加入用户显示一条错误消息，指出加入失败。


### <a name="i-am-getting-an-http412-code-when-calling-mpsd"></a>调用 MPSD 时我收到了 HTTP/412 代码。

如果会话已存在，以下请求将返回 HTTP/412：

    PUT /serviceconfigs/00000000-0000-0000-0000-000000000000/sessiontemplates/quick/sessions/foo HTTP/1.1
    Content-Type: application/json
    If-None-Match: *


如果会话 etag 与 If-Match 标头不匹配，以下请求将返回 HTTP/412：

    PUT /serviceconfigs/00000000-0000-0000-0000-000000000000/sessiontemplates/quick/sessions/foo HTTP/1.1
    Content-Type: application/json
    If-Match: 9555A7DE-8B91-40E4-8CFB-0629312C9C7D


请参阅[会话更新的同步](multiplayer-session-directory.md)了解详细信息。


### <a name="i-am-getting-an-http400-405-409-503-etc-code-when-calling-mpsd"></a>调用 MPSD 时我收到了 HTTP/400、405、409、503 等代码。

收集 Fiddler 跟踪以获取详细信息，然后查看作为 HttpResponse 正文的一部分返回的消息。 这应该为你提供了足够的信息，用来识别并修复错误，或者你可以搜索开发人员论坛寻找解决方案。

如果你使用的是 XSAPI，你还可以获取响应正文，如 [Xbox Live 服务 API 疑难解答](../../using-xbox-live/troubleshooting/troubleshooting-the-xbox-live-services-api.md)中所述。 或者，你的代码可以使用 **XboxLiveContextSettings.EnableServiceCallRoutedEvents** 属性将输出发送到你的作品 UI。


### <a name="what-can-or-should-i-change-in-the-session-templates-for-my-title"></a>可以或应当在会话模板中为我的作品做哪些修改？

会话模板是用于你的会话的模式，你无法替代模板中已经设置的常量。 但是，你可以将新属性添加到模板中。 请参阅 [MPSD 会话模板](multiplayer-session-directory.md)了解详细信息。


### <a name="im-getting-an-error-that-is-saying-my-session-isnt-initialized"></a>我收到一条指明我的会话未进行初始化的错误。

响应错误示例：

400 - \[ResponseBody\]：此会话是为需要至少 2 个成员才能开始的托管初始化配置的。

会话无法创建，因为请求中没有包括足够的将“初始化”字段设置为 true 的会话成员保留。 你的代码可以使用 **MultiplayerSession.AddMemberReservation** 方法或 **MultiplayerSession.Join** 方法的 *initializeRequested* 参数为成员设置此字段。

在你的会话模板中指定初始化时，请确保为足够的成员保留设置 "initialize":"true" 以传递匹配 QoS。 有关详细信息，请参阅[目标会话初始化和 QoS](smartmatch-matchmaking.md)。


### <a name="my-session-is-not-being-created-and-im-getting-an-http204-code"></a>我的会话未创建且我收到了 HTTP/204 代码。

此状态代码指示当你创建会话时，没有将用户添加到会话。 如果会话在创建时没有用户，且会话空超时为零（默认），则不会创建会话。

请确保在创建会话时至少包含一个玩家。 对于专用服务器场景，获取正在尝试创建匹配或需要创建匹配的玩家，并让该玩家成为匹配中的初始玩家。 或者，你可以更改或删除会话空超时。 有关详细信息，请参阅[会话超时](mpsd-session-details.md)。


### <a name="when-should-i-poll-mpsd"></a>我应在何时轮询 MPSD？

你的作品必须避免轮询 MPSD。 如果作品必须发现对 MPSD 会话的更改，它应该订阅会话更改事件。 有关详细信息，请参阅[操作方法：订阅 MPSD 会话更改通知](multiplayer-how-tos.md)。


### <a name="what-happens-if-a-player-who-was-reserved-or-invited-to-the-session-does-not-join-it"></a>如果为会话保留或被邀请加入会话的玩家未加入会怎样？

这取决于当玩家收到游戏会话已准备好的通知时游戏是否在运行。 如果玩家在游戏中，并且不接受来自游戏 UI 的游戏会话通知，则由游戏决定是否使用 **MultiplayerSession.Leave** 方法从游戏会话中删除该玩家。

如果游戏受限制或未运行，shell 将提供通知玩家空位可用的通知。 如果玩家拒绝或忽略系统通知，MPSD 将从游戏会话中删除此人。


### <a name="why-would-a-created-session-not-be-found-by-matchmaking"></a>为何创建的会话不会通过匹配找到？

在 Xbox One 上，只是创建会话还不足以让匹配找到新的会话。 你必须创建匹配票证来开始向匹配服务播发会话。 请参阅 [SmartMatch 匹配](smartmatch-matchmaking.md)。


### <a name="what-is-the-key-difference-between-the-way-parties-are-properly-used-by-2015-multiplayer-and-the-way-they-were-used-in-2014-multiplayer"></a>2015 多人游戏正确使用派对的方式与 2014 多人游戏中的使用方式之间的主要区别是什么？

在 2015 多人游戏中，多人游戏 API 不定义系统级游戏派对，仅定义聊天派对。 不使用游戏派对，游戏使用会话来控制加入、邀请及相关功能。 对于 2014 多人游戏，Xbox One 上的多人游戏 API 突出使用游戏派对概念（**Party** 类），它实际上实现了系统级加入大厅而不是游戏邀请。


### <a name="if-a-game-session-has-open-player-slots-and-supports-join-in-progress-why-would-users-not-be-able-to-find-the-session-once-it-has-started"></a>如果游戏会话有玩家空位且支持进行中加入，为何用户在会话开始后无法找到会话？

当游戏会话开始时，它不再在匹配服务中播发。 如果会话内的空位可用，并且仲裁程序（主机）想要吸引新玩家，那么仲裁程序必须使用 **MatchmakingService.CreateMatchTicketAsync** 方法为进行中的会话创建新的匹配票证。 此票证将再次播发会话，并查找更多玩家。 请参阅 [SmartMatch 匹配](smartmatch-matchmaking.md)。


### <a name="if-a-game-session-is-open-can-a-user-who-has-just-joined-a-game-simply-join-the-session-and-start-playing-without-having-to-wait-for-the-reservation"></a>如果游戏会话是开放的，刚加入游戏的用户可以只是加入会话就开始玩游戏，而无需等待保留么？

是的。 如果你的作品使用多个会话来跟踪游戏会话内玩家的子组，这尤其有用。 加入用户可以加入表示他/她的组的会话，然后需要加入更大的游戏会话。


### <a name="when-large-game-sessions-are-playing-in-my-title-why-arent-all-session-members-seeing-the-game-invite-toast"></a>当大型游戏会话在我的游戏中进行时，为何不是所有成员都能看到游戏邀请 toast？

当你的作品通过加入将用户添加到会话中时，游戏始终会将 **MultiplayerSessionMember.InitializeRequested** 属性设置为 true。 这会告知 MPSD 在将游戏移出初始化阶段之前等待其余会话成员。 否则，等待用户加入的时间段将非常短，用户可能错过 toast 和会话更改通知。


### <a name="i-am-seeing-inconsistent-game-behavior-and-have-received-protocol-activation-information-referencing-game-party-features"></a>我看到了不一致的游戏行为，并收到了引用游戏派对功能的协议激活信息。

这表明你在将 2014 多人游戏与 2015 多人游戏的功能混合。 2015 多人游戏的 API 永远不要使用为 2014 多人游戏编写的代码。


### <a name="why-am-i-seeing-the-syntax-for-v105-session-documents-in-my-traces-although-i-have-configured-a-v107-session-template"></a>我已经配置了 v107 会话模板，为何我还会在我的跟踪中看到 v105 会话文档的语法？

MPSD 根据客户端请求在会话文档版本之间执行自动转换。 目前，所有 Xbox 服务 API 均为 MPSD 的请求使用 v105。 这可能导致 v107 会话模板和 v105 跟踪之间的语法不同，但没有其他功能上的影响。 会话模板应该配置为 v107。

© 2015 Microsoft Corporation。 保留所有权利。
在 <https://forums.xboxlive.com/spaces/22/index.html> 上提交反馈。
版本：2.0.90731.0
