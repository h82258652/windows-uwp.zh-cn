---
author: Xansky
ms.assetid: cc24ba75-a185-4488-b70c-fd4078bc4206
description: 了解如何使用 AdScheduler 类在视频内容中显示广告。
title: 在视频内容中显示广告
ms.author: mhopkins
ms.date: 03/22/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, 广告, 视频, scheduler, javascript
ms.localizationpriority: medium
ms.openlocfilehash: cc5dd40ca3d9fe6e20f5e79c95b59cef3bea9a34
ms.sourcegitcommit: c4d3115348c8b54fcc92aae8e18fdabc3deb301d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/22/2018
ms.locfileid: "5407424"
---
# <a name="show-ads-in-video-content"></a>在视频内容中显示广告

本演练演示了如何在使用 JavaScript 与 HTML 编写的通用 Windows 平台 (UWP) 应用中，使用 **AdScheduler** 类在视频内容中显示广告。

> [!NOTE]
> 此功能当前仅受使用 JavaScript 与 HTML 编写的 UWP 应用支持。

**AdScheduler** 同时适用于渐进式和流媒体，并使用 IAB 标准的视频广告服务模板 (VAST) 2.0/3.0 和 VMAP 负载格式。 通过使用标准，**AdScheduler** 不需要知道与之交互的广告服务。

视频内容的广告根据程序短于十分钟（简短形式）还是长于十分钟（较长形式）而有所不同。 虽然后者在服务上设置时更为复杂，但实际上在用户编写客户端代码方式方面没有任何不同。 如果 **AdScheduler** 接收具有单个广告（而不是清单）的 VAST 负载，它将被视为针对单个前导广告调用的清单（在 00:00 时中断一次）。

## <a name="prerequisites"></a>先决条件

* 使用 Visual Studio 2015 安装 [Microsoft 广告 SDK](http://aka.ms/ads-sdk-uwp) 或更高版本。

* 你的项目必须使用 [MediaPlayer](https://github.com/Microsoft/TVHelpers/wiki/MediaPlayer-Overview) 控件，提供将计划广告的视频内容。 可在 GitHub 上的 Microsoft 中提供的 [TVHelpers](https://github.com/Microsoft/TVHelpers) 库集合中获取此控件。

  以下示例演示如何使用 HTML 标记声明 [MediaPlayer](https://github.com/Microsoft/TVHelpers/wiki/MediaPlayer-Overview)。 通常，此标记属于 index.html 文件（或另一个适用于你项目的 html 文件）中的 `<body>` 部分。

  ``` html
  <div id="MediaPlayerDiv" data-win-control="TVJS.MediaPlayer">
    <video src="URL to your content">
    </video>
  </div>
  ```

  以下示例演示如何使用 JavaScript 代码建立 [MediaPlayer](https://github.com/Microsoft/TVHelpers/wiki/MediaPlayer-Overview)。

  [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet1)]

## <a name="how-to-use-the-adscheduler-class-in-your-code"></a>如何在你的代码中使用 AdScheduler 类

1. 在 Visual Studio 中，打开项目或创建新项目。

2. 如果你的项目面向**任何 CPU**，请更新你的项目以使用特定于体系结构的生成输出（例如，**x86**）。 如果你的项目面向**任何 CPU**，你将无法在以下步骤中成功添加对 Microsoft Advertising 库的引用。 有关详细信息，请参阅[项目中由面向任何 CPU 引起的引用错误](known-issues-for-the-advertising-libraries.md#reference_errors)。

3. 将对**适用于 JavaScript 的 Microsoft 广告 SDK** 库的引用添加到你的项目。

    1. 在**解决方案资源管理器**窗口中，右键单击**引用**，然后选择**添加引用...**
    2. 在**引用管理器**中，展开**通用 Windows**、单击**扩展**，然后选中**适用于 JavaScript 的 Microsoft 广告 SDK**（版本 10.0）旁边的复选框。
    3. 在**引用管理器**中，单击“确定”。

4.  将 AdScheduler.js 文件添加到你的项目：

    1. 在 Visual Studio 中，依次单击**项目**和**管理 NuGet 包**。
    2. 在搜索框中，键入 **Microsoft.StoreServices.VideoAdScheduler** 并安装 Microsoft.StoreServices.VideoAdScheduler 程序包。 AdScheduler.js 文件将添加到你的项目中的 ../js 子目录。

5.  打开 index.html 文件（或其他适用于你项目的 html 文件）。 在 `<head>` 部分中，在项目的 JavaScript 引用 default.css 和 main.js 之后，添加对 ad.js 和 adscheduler.js 的引用。

    ``` html
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
    <script src="/js/adscheduler.js"></script>
    ```

    > [!NOTE]
    > 在包含了 main.js 之后，此行必须放置在 `<head>` 部分；否则，当你生成项目时将遇到错误。

6.  在你的项目的 main.js 文件中，添加可创建新 **AdScheduler** 对象的代码。 传入可托管视频内容的 **MediaPlayer**。 必须放置代码，以便它在 [WinJS.UI.processAll](https://docs.microsoft.com/en-us/previous-versions/windows/apps/hh440975) 后运行。

    [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet2)]

7.  使用 **AdScheduler** 对象的 **requestSchedule** 或 **requestScheduleByUrl** 方法从服务器请求广告计划，并将其插入到 **MediaPlayer** 时间线，然后播放视频媒体。

    * 如果你是已接收从 Microsoft 广告服务器请求广告计划权限的 Microsoft 合作伙伴，请使用 **requestSchedule** 并指定 Microsoft 代表已向你提供的应用程序 ID 和广告单元 ID。

        此方法采用 [Promise](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-universal-windows-platform-apps#asynchronous-patterns-in-uwp-using-javascript)形式，这是一个异步构造，在其中传递两个函数指针：一个是 **onComplete** 函数的指针，在承诺成功完成时调用，另一个是 **onError** 函数的指针，在遇到错误时调用。 在 **onComplete** 函数中，开始播放视频内容。 广告将在计划的时间开始播放。 在 **onError** 函数中，处理错误，然后开始播放视频。 视频内容将无广告播放。 **onError** 函数的参数是一个包含以下成员的对象。

        [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet3)]

    * 若要从非 Microsoft 广告服务器请求广告计划，请使用 **requestScheduleByUrl**，然后传入服务器 URI 中。 此方法也采用 **Promise** 形式，它会接受 **onComplete** 和 **onError** 函数的指针。 从服务器返回的广告负载必须遵从视频广告服务模板 (VAST) 或视频多广告播放列表 (VMAP) 负载格式。

        [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet4)]

    > [!NOTE]
    > 在 **MediaPlayer** 中开始播放主要视频内容之前，应该等待 **requestSchedule** 或 **requestScheduleByUrl** 返回。 在 **requestSchedule** 返回之前开始播放媒体（就前导广告而言），前导广告的播放将中断主要视频内容。 即使函数失败，也必须调用 **play**，因为 **AdScheduler** 将告诉 **MediaPlayer** 跳过广告，直接转到内容。 你可能具有不同的业务要求，例如插入内置广告（如果无法成功远程获取广告）。

8.  在播放期间，你可以处理让你的应用跟踪进度和/或在初始广告匹配进程后可能发生的错误的其他事件。 以下代码显示了部分事件，包括 **onPodStart**、**onPodEnd**、**onPodCountdown**、**onAdProgress**、**onAllComplete** 和 **onErrorOccurred**。

    [!code-javascript[TrialVersion](./code/AdvertisingSamples/AdSchedulerSamples/js/js/main.js#Snippet5)]

## <a name="adscheduler-members"></a>AdScheduler 成员

此部分提供有关 **AdScheduler** 对象成员的一些详细信息。 有关这些成员的更多信息，请参阅你的项目中 AdScheduler.js 文件中的注释和定义。

### <a name="requestschedule"></a>requestSchedule

此方法从 Microsoft 广告服务器请求广告计划，并将其插入传递到 **AdScheduler** 构造函数的 **MediaPlayer** 时间线。

可选的第三个参数 (*adTags*) 是名称/值对的 JSON 集合，这些名称/值对可用于具有高级目标设定的应用。 例如，一个播放各种自动相关视频的应用可能会使用所展示汽车的品牌和型号来补充广告单元 ID。 此参数只能由 Microsoft 批准使用广告标签的合作伙伴使用。

引用 *adTags* 时应注意以下几点：

* 此参数是很少使用的选项。 在使用 adTags 之前，发布者必须与 Microsoft 密切合作。
* 名称和值都必须通过广告服务预先确定。 广告标签并非开放性的搜索词或关键字。
* 支持的标签数最多为 10 个。
* 标签名称最长只能包含 16 个字符。
* 标签值最多只能包含 128 个字符。

### <a name="requestschedulebyuri"></a>requestScheduleByUri

此方法从 URI 中指定的非 Microsoft 广告服务器请求广告计划，并将其插入传递到 **AdScheduler** 构造函数的 **MediaPlayer** 时间线。 从广告服务器返回的广告负载必须遵从视频广告服务模板 (VAST) 或视频多广告播放列表 (VMAP) 负载格式。

### <a name="mediatimeout"></a>mediaTimeout

此属性获取或设置媒体必须可播放的毫秒数。 值为 0 时，将通知系统永不超时。 默认值为 30000 毫秒（30 秒）。

### <a name="playskippedmedia"></a>playSkippedMedia

此属性获取或设置 **Boolean** 值，用于指示当用户跳到超过计划开始时间的点时，是否播放计划的媒体。

广告客户端和媒体播放器将在主要视频内容快进和倒带时，强制执行广告操作规则。 在大多数情况下，应用开发人员都不允许跳过整个广告，而又要想为用户提供合理的体验。 大多数开发人员需要以下两个选项：

1. 允许最终用户随意跳过广告荚。
2. 允许用户跳过广告荚，但播放恢复时播放最新的广告荚。

**PlaySkippedMedia** 属性具有以下条件：

* 一旦广告开始，就不能跳过或快进。
* 一旦一个广告荚开始播放，其中的所有广告都要播放。
* 一个广告只要播放完毕，在主要内容（影片、剧集等）播放期间就不会再次播放；广告标记将标记为已播放或已移除。
* 前导广告不能跳过。

恢复包含广告的内容时，将 **playSkippedMedia** 设置为 **false** 可跳过前导广告，并会阻止最近的广告断点播放。 然后，内容开始播放后，将 **playSkippedMedia** 设置为 **true** 以确保用户无法快进后面的广告。

> [!NOTE]
> 一个广告荚是一组按顺序播放的广告，如在商业广告时段播放的一组广告。 有关更多详细信息，请参阅 IAB 数字视频广告服务模板 (VAST) 规范。

### <a name="requesttimeout"></a>requestTimeout

此属性获取或设置广告请求响应超时前等待的毫秒数。其值为 0 时将通知系统永不超时。 默认值为 30000 毫秒（30 秒）。

### <a name="schedule"></a>schedule

此属性获取从广告服务器检索到的计划数据。 此对象包含对应于视频广告服务模板 (VAST) 或视频多广告播放列表 (VMAP) 负载结构的完整数据结构层次。

### <a name="onadprogress"></a>onAdProgress  

当广告播放到四分之一检查点时引发此事件。 事件处理程序的第二个参数 (*eventInfo*) 是一个具有以下成员的 JSON 对象：

* **progress**：广告播放状态（AdScheduler.js 中定义的 **MediaProgress** 枚举值之一）。
* **clip**：正在播放的视频剪辑。 此对象不适用于你的代码。
* **adPackage**：一个对象，表示当前播放广告在广告负载中的对应部分。 此对象不适用于你的代码。

### <a name="onallcomplete"></a>onAllComplete  

当主要内容播放完毕并且计划的所有后续广告也都播放完时，引发此事件。

### <a name="onerroroccurred"></a>onErrorOccurred  

当 **AdScheduler** 遇到错误时引发此事件。 有关错误代码值的详细信息，请参阅[错误代码](https://docs.microsoft.com/uwp/api/microsoft.advertising.errorcode)。

### <a name="onpodcountdown"></a>onPodCountdown

当广告正在播放时触发此事件并指示当前广告荚剩余时间。 事件处理程序的第二个参数 (*eventData*) 是一个具有以下成员的 JSON 对象：

* **remainingAdTime**：当前广告的剩余秒数。
* **remainingPodTime**：当前广告荚的剩余秒数。

> [!NOTE]
> 一个广告荚是一组按顺序播放的广告，如在商业广告时段播放的一组广告。 有关更多详细信息，请参阅 IAB 数字视频广告服务模板 (VAST) 规范。

### <a name="onpodend"></a>onPodEnd  

广告荚结束时引发此事件。 事件处理程序的第二个参数 (*eventData*) 是一个具有以下成员的 JSON 对象：

* **startTime**：广告荚的开始时间（以秒计）。
* **pod**：代表广告荚的对象。 此对象不适用于你的代码。

### <a name="onpodstart"></a>onPodStart

广告荚开始时引发此事件。 事件处理程序的第二个参数 (*eventData*) 是一个具有以下成员的 JSON 对象：

* **startTime**：广告荚的开始时间（以秒计）。
* **pod**：代表广告荚的对象。 此对象不适用于你的代码。
