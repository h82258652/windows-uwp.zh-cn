---
title: Xbox Live 跟踪分析器
author: KevinAsgari
description: 了解如何使用 Xbox Live 跟踪分析器来查看游戏进行的服务调用。
ms.assetid: b4490fae-d554-403d-bbbc-601af38af0ef
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, 服务调用, 测试, 跟踪分析器
ms.localizationpriority: medium
ms.openlocfilehash: 196505a9ed3fb62ef609415c292e98850588102f
ms.sourcegitcommit: 1938851dc132c60348f9722daf994b86f2ead09e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/03/2018
ms.locfileid: "4262387"
---
# <a name="xbox-live-trace-analyzer"></a>Xbox Live 跟踪分析器

现在，游戏开发人员可以通过 Xbox Live 服务 API 捕获所有服务调用，然后对其进行离线分析，以了解调用模式中是否存在任何违规。 可以使用 xbtrace 命令行工具中的新功能或者通过协议激活更高级方案来激活服务调用跟踪。 此外，也支持通过游戏代码直接激活服务调用跟踪。 从 Xbox Live 工具程序包的一部分找不到离线分析工具，调用 Xbox Live 跟踪分析器，XBLTraceAnalyzer.exe) [https://aka.ms/xboxliveuwptools](https://aka.ms/xboxliveuwptools)。


## <a name="gather-logs-and-analyze-the-service-calls"></a>收集日志并分析服务调用

必须执行以下步骤才能收集包含服务调用记录的日志并使用 Xbox Live 跟踪分析器对其进行分析。

1.  使用 2015 年 7 月或更新版本的 Xbox 开发人员工具包 (XDK) 中包含的 Xbox Live 服务 API 构建游戏。
2.  按照如下所述，修改游戏以启用跟踪。
3.  部署你的游戏。
4.  启动游戏并且至少调用一次 Xbox Live 服务，以便初始化 Xbox Live 服务 API。
5.  从游戏中你想要分析的位置开始跟踪。
6.  停止跟踪。
7.  在开发电脑上运行 Xbox Live 跟踪分析器工具并查看输出。

## <a name="starting-and-stopping-tracing"></a>开始和停止跟踪

可以通过三种方式开始和停止跟踪：

1.  你可以直接通过游戏调用一组 Xbox Live 服务 API。
2.  你可以使用 *xbtrace* 命令行工具。
3.  你可以使用*应用程序管理 (xbapp.exe)* 命令行工具中的协议激活。


### <a name="starting-and-stopping-tracing-directly-from-your-title"></a>直接通过游戏开始和停止跟踪

若要直接通过游戏开始跟踪，你必须执行以下操作：

1.  在 `Microsoft::Xbox::Services::Experimental` 命名空间中，将 `ServiceCallTrackerSettings` 类的 `EnableServiceCallTracking` 属性设置为 true。
2.  调用 `StartServiceCallTracking()` 以开始跟踪服务调用。
3.  调用 `StopServiceCallTracking()` 以停止跟踪服务调用。
4.  跟踪停止后，使用*文件复制 (xbcp.exe)* 或 *Xbox One Neighborhood* 将结果跟踪文件从主机上的开发人员暂存驱动器复制回电脑，以便使用 Xbox Live 跟踪分析器进行分析。

### <a name="starting-and-stopping-tracing-by-using-the-xbtrace-command-line-tool"></a>使用 xbtrace 命令行工具开始和停止跟踪

最方便、简单的开始跟踪方法是使用跟踪类型为 xboxliveservices 的 xbtrace 命令行工具。 使用 xbTrace 时，系统会为你将结果跟踪文件复制回电脑。

使用 xbtrace 开始和停止跟踪依赖于协议激活。 使用 xbtrace 开始和停止跟踪之前，你必须通过调用 `ServiceCallTrackerSettings` 类上的 `RegisterForProtcolActivation` 方法初始化协议激活。

以下示例展示了如何使用 xbTrace 开始和停止 Xbox Live 服务跟踪：

    xbtrace start xboxliveservices
    xbtrace stop


请记住，在通过 xbtrace 开始和停止跟踪之前，必须先运行游戏并且初始化协议激活。 跟踪停止后，xbtrace 会将跟踪文件复制回你的开发电脑并将其置于名称包含“xbtrace”及其时间戳的目录中。 可以使用 xbtrace 的 \[etlfile\] 选项覆盖此目录的名称。

<a name="starting-and-stopping-tracing-by-using-protocol-activation"></a>使用协议激活开始和停止跟踪
----------------------------------------------------------

也可以使用“xbApp 启动”的协议激活功能控制跟踪。 你必须知道游戏的 titleid 才能通过协议激活开始和停止跟踪。 你可以在游戏的清单文件中找到游戏 id。 通过包含“serviceCallTracking”参数的 URI 控制跟踪。 以下示例展示了如何对游戏 id 为 12345678 的游戏开始和停止跟踪：

    xbapp launch "ms-xbl-12345678://serviceCallTracking?state=start"
    xbapp launch "ms-xbl-12345678://serviceCallTracking?state=stop"

使用协议激活时，结果跟踪文件将存储于主机上的开发人员暂存驱动器上。 你需要使用 xbcp 或 Xbox One Neighborhood 将文件复制回电脑。 文件不会自动复制回电脑，这与使用 xbtrace 时不同。

你可以通过协议激活设置其他的跟踪参数，如详细级别。 支持四种详细级别：静默、诊断、详细和最低。 以下示例展示了如何设置详细级别：

    xbapp launch "ms-xbl-12345678://serviceCallTracking?verbosity=diagnostic"

## <a name="analyze-the-trace-file"></a>分析跟踪文件

跟踪文件复制回电脑之后，你可以使用 GNDP 上的 Xbox Live 跟踪分析器来分析游戏对 Xbox Live 服务的使用情况。 请参阅游戏开发人员网络上的 Xbox Live 跟踪分析器工具中所含的文档，了解如何调用此工具和解释其输出。 此外，你还可以运行命令行选项为 -? 或 -h 的 XBLTraceAnalyzer.exe，以查看命令行帮助。
