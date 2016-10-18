---
author: mcleanbyron
ms.assetid: cc24ba75-a185-4488-b70c-fd4078bc4206
description: "了解如何使用 AdScheduler 类向视频内容添加广告。"
title: "使用 HTML 5 和 JavaScript 向视频内容添加广告"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 5fd097d978b1960ed957a7e12ab7eece012c5b41

---

# 使用 HTML 5 和 JavaScript 向视频内容添加广告


本演练演示了如何在使用 JavaScript 与 HTML 编写的通用 Windows 平台 (UWP) 应用中，使用 [AdScheduler](https://msdn.microsoft.com/library/windows/apps/mt732197.aspx) 类向视频内容添加广告。

>**注意**&nbsp;&nbsp;此功能当前仅受使用 JavaScript 与 HTML 编写的 UWP 应用支持。

[AdScheduler](https://msdn.microsoft.com/library/windows/apps/mt732197.aspx) 同时适用于渐进式和流媒体，并使用 IAB 标准的视频广告服务模板 (VAST) 2.0/3.0 和 VMAP 负载格式。 通过使用标准，[AdScheduler](https://msdn.microsoft.com/library/windows/apps/mt732197.aspx) 不需要知道与之交互的广告服务。

视频内容的广告根据程序短于十分钟（简短形式）还是长于十分钟（较长形式）而有所不同。 虽然后者在服务上设置时更为复杂，但实际上在用户编写客户端代码方式方面没有任何不同。 如果 [AdScheduler](https://msdn.microsoft.com/library/windows/apps/mt732197.aspx) 接收具有单个广告（而不是清单）的 VAST 负载，它将被视为针对单个前导广告调用的清单（在 00:00 时中断一次）。

## 先决条件

* 使用 Visual Studio 2015 安装 [Microsoft Store Services SDK](http://aka.ms/store-em-sdk)。

* 你的项目必须使用 [MediaPlayer](https://github.com/Microsoft/TVHelpers/wiki/MediaPlayer-Overview) 控件，提供将计划广告的视频内容。 可在 GitHub 上的 Microsoft 中提供的 [TVHelpers](https://github.com/Microsoft/TVHelpers) 库集合中获取此控件。

  以下示例演示了如何使用 HTML 标记声明 [MediaPlayer](https://github.com/Microsoft/TVHelpers/wiki/MediaPlayer-Overview)。 通常，此标记属于 default.html 文件中的 `<body>` 部分（或其他适用于你的项目的 html 文件）。
  ``` html
  <div id="MediaPlayerDiv" data-win-control="TVJS.MediaPlayer">
    <video src="URL to your content">
    </video>
  </div>
  ```

  以下示例演示了如何使用 JavaScript 代码建立 [MediaPlayer](https://github.com/Microsoft/TVHelpers/wiki/MediaPlayer-Overview)。
  ``` javascript
  var mediaPlayerDiv = document.createElement("div");
  document.body.appendChild(mediaPlayerDiv);

  var videoElement = document.createElement("video");
  videoElement.src = "<URL to your content>";
  mediaPlayerDiv.appendChild(videoElement);

  var mediaPlayer = new TVJS.MediaPlayer(mediaPlayerDiv);
  ```

## 如何在你的代码中使用 AdScheduler 类

1. 在 Visual Studio 中，打开项目或创建新项目。

2. 如果你的项目面向**任何 CPU**，请更新你的项目以使用特定于体系结构的生成输出（例如，**x86**）。 如果你的项目面向**任何 CPU**，你将无法在以下步骤中成功添加对 Microsoft Advertising 库的引用。 有关详细信息，请参阅[项目中由面向任何 CPU 引起的引用错误](known-issues-for-the-advertising-libraries.md#reference_errors)。

3. 将对**适用于 JavaScript 的 Microsoft Advertising SDK** 库的引用添加到你的项目。

  a. 在“解决方案资源管理器”****窗口中，右键单击“引用”****，然后选择“添加引用...”****

  b. 在“引用管理器”****中，展开“通用 Windows”****、单击“扩展”****，然后选中“适用于 JavaScript 的 Microsoft Advertising SDK”****（版本 10.0）旁边的复选框。

  c. 在“引用管理器”****中，单击“确定”。

4.  将 AdScheduler.js 文件添加到你的项目：

  a.  在 Visual Studio 中，依次单击“项目”****和“管理 NuGet 包”****。

  b.  在搜索框中，键入 **Microsoft.StoreServices.VideoAdScheduler** 并安装 Microsoft.StoreServices.VideoAdScheduler 程序包。 AdScheduler.js 文件将添加到你的项目中的 ../js 子目录。

5.  打开 default.html 文件（或其他适用于你的项目的 html 文件）。 在 `<head>` 部分中，在项目的 JavaScript 引用 default.css 和 default.js 之后，添加对 ad.js 和 adscheduler.js 的引用。

    ``` html
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
    <script src="/js/adscheduler.js"></script>
    ```

    > **注意** 在包含了 default.js 之后，此行必须放置在 `<head>` 部分；否则，当你生成项目时将遇到错误。

6.  在你的项目的 default.js 文件中，将添加可创建新 [AdScheduler](https://msdn.microsoft.com/library/windows/apps/mt732197.aspx) 对象的代码。 传入可托管视频内容的 **MediaPlayer**。 必须放置代码，以便它在 [WinJS.UI.processAll](https://msdn.microsoft.com/library/windows/apps/hh440975.aspx) 后运行。

    ``` javascript
    var myMediaPlayer = document.getElementById("MediaPlayerDiv");
    var myAdScheduler = new MicrosoftNSJS.Advertising.AdScheduler(myMediaPlayer);
    ```

7.  使用 [requestSchedule](https://msdn.microsoft.com/library/windows/apps/mt732208.aspx) 或 [requestScheduleByUrl](https://msdn.microsoft.com/library/windows/apps/mt732210.aspx) 方法从服务器请求一个广告计划，并将其插入到 **MediaPlayer** 时间线，然后播放视频媒体。

  * 如果你是已接收从 Microsoft 广告服务器请求广告计划权限的 Microsoft 合作伙伴，请使用 [requestSchedule](https://msdn.microsoft.com/library/windows/apps/mt732208.aspx) 并指定 Microsoft 代表已向你提供的应用程序 ID 和广告单元 ID。 此方法采用 **Promise** 形式，这是可传递两个函数指针以分别处理成功和失败案例的异步构造。 有关详细信息，请参阅[使用 JavaScript 的 UWP 中的异步模式](https://msdn.microsoft.com/windows/uwp/threading-async/asynchronous-programming-universal-windows-platform-apps#asynchronous-patterns-in-uwp-using-javascript)。

      ``` javascript
  myAdScheduler.requestSchedule("your application ID", "your ad unit ID").then(
        function promiseSuccessHandler(schedule) {
           // Success: play the video content with the scheduled ads.
           myMediaPlayer.tvControl.play();
        },
        function promiseErrorHandler(err) {
           // Error: play the video content without the ads.
           myMediaPlayer.tvControl.play();
        });
  ```

  * 若要从非 Microsoft 广告服务器请求广告计划，请使用 [requestScheduleByUrl](https://msdn.microsoft.com/library/windows/apps/mt732210.aspx)，然后传入服务器 URL 中。 此方法也采用 **Promise** 形式。

      ``` javascript
  myAdScheduler.requestScheduleByUrl("your URL").then(
        function promiseSuccessHandler(schedule) {
            // Success: play the video content with the scheduled ads.
            myMediaPlayer.winControl.play();
        },
        function promiseErrorHandler(evt) {
            // Error: play the video content without the ads.
             myMediaPlayer.winControl.play();
        });
  ```

  >**注意**&nbsp;&nbsp;即使该函数失败，也必须调用 **play**，因为 [AdScheduler](https://msdn.microsoft.com/library/windows/apps/mt732197.aspx) 将告诉 **MediaPlayer** 跳过广告，直接转到内容。 你可能具有不同的业务要求，例如插入内置广告（如果无法成功远程获取广告）。

8.  在播放期间，可能存在让你的应用跟踪进度和/或在初始广告匹配进程后可能发生的错误的其他事件。 以下代码显示其中一些事件。
  * [onPodStart](https://msdn.microsoft.com/library/windows/apps/mt732206.aspx)：

      ```javascript
      // Raised when an ad pod starts. Make the countdown timer visible.
      myAdScheduler.onPodStart = function (sender, data) {
          myCounterDiv.style.visibility= "visible";
      }  
      ```

  * [onPodEnd](https://msdn.microsoft.com/library/windows/apps/mt732205.aspx)：

      ```javascript
      // Raised when an ad pod ends. Hide the countdown timer.
      myAdScheduler.onPodEnd = function (sender, data) {
          myCounterDiv.style.visibility= "hidden";
      }
      ```

  * [onPodCountdown](https://msdn.microsoft.com/library/windows/apps/mt732204.aspx)：

      ```javascript
      // Raised when an ad is playing and indicates how many seconds remain in the current pod of ads. This is useful when the app wants to show a visual countdown until the video content resumes.
      myAdScheduler.onPodCountdown = function (sender, data) {
            myCounterText     = "Content resumes in: " +
            Math.ceil(data.remainingPodTime) + " seconds.";
      }  
      ```

  * [onAdProgress](https://msdn.microsoft.com/library/windows/apps/mt732201.aspx)：

      ```javascript
      // Raised during each quartile of progress through the ad clip.
      myAdScheduler.onAdProgress = function (sender, data) {
      }
      ```

  * [onAllComplete](https://msdn.microsoft.com/library/windows/apps/mt732202.aspx)：

      ```javascript
      // Raised when the ads and content are complete.
      myAdScheduler.onAllComplete = function (sender) {
      }
      ```

  * [onErrorOccurred](https://msdn.microsoft.com/library/windows/apps/mt732203.aspx)：

      ```javascript
      // Raised when an error occurs during playback after successfully scheduling.
      myAdScheduler.onErrorOccurred = function (sender, data) {
      }
      ```



<!--HONumber=Aug16_HO5-->


