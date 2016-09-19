---
author: GrantMeStrength
ms.assetid: CFB3601D-3459-465F-80E2-520F57B88F62
title: "创建“Hello, world”应用 (JS)"
description: "本教程指导你如何使用 JavaScript 和 HTML 创建一个简单的“Hello, world”应用，该应用面向 Windows 10 上的通用 Windows 平台 (UWP)。"
translationtype: Human Translation
ms.sourcegitcommit: 2e0965f964f6f2e10b895d99244b66458eb15903
ms.openlocfilehash: 6c81b24f7fa9abe036d4ccd22ee8fa24c011fe77

---
# 创建“Hello, world”应用 (JS)

本教程指导你如何使用 JavaScript 和 HTML 创建一个简单的“Hello, world”应用，该应用面向 Windows 10 上的通用 Windows 平台 (UWP)。 通过 Microsoft Visual Studio 中的单个项目，你可以生成可在任何 Windows 10 设备上运行的应用。

在此处，你将了解如何：

-   创建面向 **Windows 10** 和 **UWP** 的新 **Visual Studio 2015** 项目。
-   向起始页中添加 HTML 内容
-   处理触控、笔以及鼠标输入
-   在 Visual Studio 中，在本地桌面和手机仿真器中运行该项目
-   使用 Windows JavaScript 库控件

## 开始之前...

-   [什么是通用 Windows 应用](whats-a-uwp.md)？
-   [Windows 10 中的新增功能](https://dev.windows.com/whats-new-windows-10-dev-preview)
-   若要完成本教程，你需要 Windows 10 和 Visual Studio 2015。 [准备工作](get-set-up.md)。
-   我们还假设你使用的是 Visual Studio 中的默认窗口布局。 如果要更改默认布局，你可以在“窗口”****菜单中，通过使用“重置窗口布局”****命令来重置它。

## 步骤 1：在 Visual Studio 中创建新项目

我们来创建名为 `HelloWorld` 的新应用。 以下是操作方法：
1.  启动 Visual Studio 2015。

2.  在“文件”****菜单中，依次选择“新建”&gt;“项目...”****以打开“新建项目”**对话框。

3.  在左侧的模板列表中，依次展开“已安装”&gt;“模板”&gt;“JavaScript”&gt;“Windows”****，然后选择“通用”****以查看 UWP 项目模板列表。 选择“WinJS App(通用 Windows)”****。

    ![“新建项目”窗口 ](images/winjs-tut-newproject.png)

    在本教程中，我们使用“WinJS App”****模板。 此模板创建一个最基本的 UWP 应用，该应用能够编译和运行，但不包含用户界面控件或数据。 本教程中的课程将指导你向应用中添加控件和数据。

   （如果你没有看到这些选项，请确保已安装通用 Windows 应用开发工具。 有关详细信息，请参阅[准备工作](get-set-up.md)。）

4.  在“名称”****文本框中，键入“HelloWorld”。
5.  单击“确定”****以创建项目。
6.  系统将要求你选择 Windows 支持的“目标版本”****和“最低版本”****。 默认设置都合适，因此单击“确定”****。

    Visual Studio 将创建项目并在“解决方案资源管理器”****中显示该项目。

    ![HelloWorld 项目的 Visual Studio 解决方案资源管理器](images/winjs-tut-helloworld.png)

尽管“WinJS App”****是最基本的模板，但该模板仍包含少量文件：

-   介绍应用（它的名称、说明、磁贴、起始页、初始屏幕等等）并列出应用包含的文件的清单文件 (package.appxmanifest)。
-   用于在“开始”菜单中显示的一组徽标图像（images/Square150x150Logo.scale-200.png、images/Square44x44Logo.scale-200.png 和 images/Wide310x150Logo.scale-200.png）。
-   用于在 Windows 应用商店中表示应用的图像 (images/StoreLogo.png)。
-   用于在应用启动时显示的初始屏幕 (images/SplashScreen.scale-200.png)。
-   用于在应用启动时运行的起始页 (index.html) 和附带的 JavaScript 文件 (main.js)。

若要查看和编辑文件，请双击“解决方案资源管理器”****中的文件。

这些文件是所有使用 JavaScript 的 UWP 应用必不可少的文件。 在 Visual Studio 中创建的所有项目都包含这些文件。

## 步骤 2：启动应用


此时，你已创建了一个非常简单的应用。 现在是构建、部署和启动应用并查看其外观的好时机。 你可以在本地计算机上、模拟器或仿真器中或者在远程设备上调试应用。 下面是 Visual Studio 中的目标设备菜单。

![用于调试应用的设备目标下拉列表](images/uap-debug.png)

### 在桌面设备上启动应用

默认情况下，应用在本地计算机上运行。 目标设备菜单提供用于在桌面设备系列中的设备上调试应用的多个选项。

-   **模拟器**
-   **本地计算机**
-   **远程计算机**

**在本地计算机上开始调试**

1.  在**“标准”**工具栏上的目标设备菜单（![“开始调试”菜单](images/startdebug-full.png)）中，确保已选中**“本地计算机”**。 （它是默认选择。）
2.  单击工具栏中的**“开始调试”**按钮（![“开始调试”按钮](images/startdebug-sm.png)）。

   -或者-

   在“调试”****菜单中，单击“开始调试”****。

   -或者-

   按 F5。

应用将在窗口中打开，并且将首先显示默认初始屏幕。 初始屏幕由一个图像 \(SplashScreen.png\) 和背景色（在应用的清单文件中指定）定义。

初始屏幕会消失，然后会出现你的应用。 它包含带有文本“内容在此处”的黑屏。

![电脑上的 HelloWorld 应用](images/helloworld-1-winjs.png)

按 Windows 键以打开**“开始”**菜单，然后显示所有应用。 请注意，本地部署应用会将其磁贴添加到**“开始”**菜单。 若要再次运行该应用（不是在调试模式下），请在**“开始”**菜单中点击或单击其磁贴。

它还无法执行很多操作，但祝贺你已构建了第一个 UWP 应用！

**停止调试**

-   单击工具栏中的**“停止调试”**按钮（![“停止调试”按钮](images/stopdebug.png)）。

   -或者-

   在“调试”****菜单中，单击“停止调试”****。

   -或者-

   关闭应用窗口。

### 在移动设备仿真器上启动该应用

你的应用可在任何 Windows 10 设备上运行，让我们看一下它在 Windows Phone 上的情况如何。

除了在桌面设备上执行调试的选项，Visual Studio 还提供用于在连接到计算机的物理移动设备上或移动设备仿真器上部署和调试应用的选项。 你可以为带有不同内存和显示配置的设备在仿真器中进行选择。

-   **设备**
-   **仿真器 <SDK version> WVGA 4 英寸 512MB**
-   **仿真器 <SDK version> WVGA 4 英寸 1GB**
-   等（采用其他配置的各种仿真器）

（如果你没有看到这些仿真器，请确保已安装通用 Windows 应用开发工具。 有关详细信息，请参阅[准备工作](get-set-up.md)。）

最好在具有小型屏幕和有限内存的设备上测试应用，因此请使用“仿真器 10.0.14393.0 WVGA 4 英寸 512MB”****选项。

**在移动设备仿真器上开始调试**

1.  在“标准”****工具栏上的目标设备菜单（![“开始调试”菜单](images/startdebug-full.png)）中，选取“仿真器 10.0.14393.0 WVGA 4 英寸 512MB”****。
2.  单击工具栏中的“开始调试”****按钮（![“开始调试”按钮](images/startdebug-sm.png)）。

   -或者-

   在**“调试”**菜单中，单击**“开始调试”**。


Visual Studio 将启动选定的仿真器，然后部署和启动你的应用。 在初始启动时，仿真器可能需要一些时间来启动。 你可能会看到有关 HyperV 的错误，单击“重试”****应该可以解决此问题。 在移动设备仿真器上，应用外观如下所示。

![移动设备上的初始应用屏幕](images/helloworld-1-winjs-phone.png)

## 步骤 3：修改你的起始页

Visual Studio 为你创建了一个 **index.html** 文件，它是应用的起始页。 应用运行时，会显示其起始页的内容。 起始页还包含对应用的代码文件和样式表的引用。 以下是 Visual Studio 为你创建的起始页：

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title>HelloWorld</title>

    <!-- WinJS references -->
    <link href="lib/winjs-4.0.1/css/ui-light.css" rel="stylesheet" />
    <script src="lib/winjs-4.0.1/js/base.js"></script>
    <script src="lib/winjs-4.0.1/js/ui.js"></script>

    <!-- HelloWorld references -->
    <link href="/css/default.css" rel="stylesheet" />
    <script src="/js/main.js"></script>
</head>
<body class="win-type-body">
    <div>Content goes here!</div>
</body>
</html>
```

让我们向 default.html 文件中添加一些新内容。 正如你向任何其他 HTML 文件添加内容一样，在 [body](https://msdn.microsoft.com/library/windows/apps/Hh453011) 元素内添加内容。 你可以使用 HTML5 元素创建应用（[少数情况例外](https://msdn.microsoft.com/library/windows/apps/Hh465380)）。 这表示你可以使用 HTML5 元素，如 [h1](https://msdn.microsoft.com/library/windows/apps/Hh441078)、[p](https://msdn.microsoft.com/library/windows/apps/Hh453431)、[button](https://msdn.microsoft.com/library/windows/apps/Hh453017)、[div](https://msdn.microsoft.com/library/windows/apps/Hh453133) 以及 [img](https://msdn.microsoft.com/library/windows/apps/Hh466114)。

**编辑起始页**

1.  使用以下内容替代 **body** 元素中的现有内容：显示“Hello, world!”的一级标题、询问用户名的一些文本、用于接受用户名的 **input** 元素、**button** 以及 **div** 元素。 向 **input**、**button** 和 **div** 分配 ID。

 ```html
    <body class="win-type-body">
        <h1>Hello, world!</h1>
        <p>What's your name?</p>
        <input id="nameInput" type="text" />
        <button id="helloButton">Say "Hello"</button>
        <div id="greetingOutput"></div>
    </body>
 ```

2.  在本地计算机上运行应用。 它的外观如下所示。

![带有新内容的 HelloWorld 应用](images/helloworld-2-winjs.png)

   你可以在 **input** 元素中键入内容，但现在单击 **button** 不会执行任何操作。 某些对象（例如 **button**）可以在发生特定事件时发送消息。 这些事件消息为你提供了可以采取一些操作响应事件的机会。 将用于响应事件的代码放在事件处理程序方法中。

   在后续步骤中，我们为显示个性化问候的 **button** 创建事件处理程序。 我们向 main.js 文件添加事件处理程序代码。

## 步骤 4：创建事件处理程序

创建新项目后，Visual Studio 为我们创建了 /js/main.js 文件。 此文件包含用于处理应用生命周期的代码。 你还可以在此文件中编写为 index.html 文件提供交互性的其他代码。

打开 main.js 文件。

在开始添加自己的代码之前，我们来看一下该文件中代码的头几行和最后几行：

```javascript
(function () {
    "use strict";

     // Omitted code

 })();
```

你可能会对此处发生的情况感到疑惑。 代码的这些行覆盖了自我执行匿名函数中 main.js 代码的其他位置。 自我执行匿名函数使避免命名冲突或意外修改原本无意修改的值的情况变得更简单。 此操作还可防止全局命名空间中出现不需要的标识符，这有助于提高性能。 它看上去有一点奇怪，但却是良好的编程实践。

代码的下一行为 JavaScript 代码打开了[严格模式](https://msdn.microsoft.com/library/windows/apps/br230269.aspx)。 严格模式为代码提供了额外的错误检查。 例如，它防止你使用隐式声明的变量或为只读属性分配值。

查看 main.js 中代码的剩余部分。 它处理应用的 [activated](https://msdn.microsoft.com/library/windows/apps/BR212679) 和 [checkpoint](https://msdn.microsoft.com/library/windows/apps/BR229839) 事件。 我们在以后讨论这些事件的更多详细信息。 现在，只要了解启动应用时将引发 **activated** 事件。

```javascript
   (function () {
    "use strict";

    var app = WinJS.Application;
    var activation = Windows.ApplicationModel.Activation;
    var isFirstActivation = true;

    app.onactivated = function (args) {
          if (args.detail.kind === activation.ActivationKind.voiceCommand) {
            // TODO: Handle relevant ActivationKinds. For example, if your app can be started by voice commands,
            // this is a good place to decide whether to populate an input field or choose a different initial view.
        }
          else if (args.detail.kind === activation.ActivationKind.launch) {
            // A Launch activation happens when the user launches your app via the tile
            // or invokes a toast notification by clicking or tapping on the body.
              if (args.detail.arguments) {
                // TODO: If the app supports toasts, use this value from the toast payload to determine where in the app
                // to take the user in response to them invoking a toast notification.
              }
              else if (args.detail.previousExecutionState === activation.ApplicationExecutionState.terminated) {
                // TODO: This application had been suspended and was then terminated to reclaim memory.
                // To create a smooth user experience, restore application state here so that it looks like the app never stopped running.
                // Note: You may want to record the time when the app was last suspended and only restore state if they've returned after a short period.
            }
        }

        if (!args.detail.prelaunchActivated) {
            // TODO: If prelaunchActivated were true, it would mean the app was prelaunched in the background as an optimization.
            // In that case it would be suspended shortly thereafter.
            // Any long-running operations (like expensive network or disk I/O) or changes to user state which occur at launch
            // should be done here (to avoid doing them in the prelaunch case).
            // Alternatively, this work can be done in a resume or visibilitychanged handler.
        }

        if (isFirstActivation) {
            // TODO: The app was activated and had not been running. Do general startup initialization here.
            document.addEventListener("visibilitychange", onVisibilityChanged);
            args.setPromise(WinJS.UI.processAll());
        }

        isFirstActivation = false;
    };
```

让我们为 [button](https://msdn.microsoft.com/library/windows/apps/Hh453017) 定义事件处理程序。 新事件处理程序从 `nameInput` [input](https://msdn.microsoft.com/library/windows/apps/Hh453271) 控件获取用户名并使用该用户名向在上一部分中创建的 `greetingOutput` **div** 元素输出问候语。

### 使用用于触控、鼠标和笔输入的事件

在 UWP 应用中，你无需担心触控、鼠标与其他指针输入形式之间的区别。 你只需使用你了解的事件（如 [click](https://msdn.microsoft.com/library/windows/apps/Hh441312)），这些事件适用于所有输入形式。

**提示** 应用还可以使用新的 *MSPointer\** 和 *MSGesture\** 事件，这些事件适用于触控、鼠标以及笔输入，并可以提供有关触发事件的设备的其他信息。 有关详细信息，请参阅[响应用户交互](https://msdn.microsoft.com/library/windows/apps/Hh700412)和[手势、操作以及交互](https://msdn.microsoft.com/library/windows/apps/Hh761498)。

我们继续并创建事件处理程序。

**创建事件处理程序**

1.  在 main.js 中，在 [**app.oncheckpoint**](https://msdn.microsoft.com/library/windows/apps/BR229839) 事件处理程序之后且对 [**app.start**](https://msdn.microsoft.com/library/windows/apps/BR229705) 的调用之前，创建名为 `buttonClickHandler` 的 [**click**](https://msdn.microsoft.com/library/windows/apps/Hh441312) 事件处理程序函数，该函数接受名为 `eventInfo` 的单个参数。
```javascript
    function buttonClickHandler(eventInfo) {

        }
```

2.  在事件处理程序内，从 `nameInput` [**input**](https://msdn.microsoft.com/library/windows/apps/Hh453271) 控件检索用户名并使用该用户名创建问候语。 使用 `greetingOutput` [**div**](https://msdn.microsoft.com/library/windows/apps/Hh453133) 显示相关结果。
```javascript
    function buttonClickHandler(eventInfo) {
            var userName = document.getElementById("nameInput").value;
            var greetingString = "Hello, " + userName + "!";
            document.getElementById("greetingOutput").innerText = greetingString;
        }
 ```

你已将事件处理程序添加到 main.js。 现在你需要注册该处理程序。

## 步骤 5：在应用启动时注册事件处理程序


此时，你只需向该按钮注册该事件处理程序。 注册事件处理程序的建议方法是从代码中调用 [addEventListener](https://msdn.microsoft.com/library/windows/apps/Hh441145)。 注册事件处理程序的较好时机是在激活应用之时。 幸运的是，正如你所见，Visual Studio 为我们在 main.js 文件中生成了一些代码，可处理应用激活。


在 [onactivated](https://msdn.microsoft.com/library/windows/apps/BR212679) 处理程序内，该代码会检查发生了何种类型的激活。 存在多种不同类型的激活。 例如，当用户启动应用和用户希望打开与应用关联的文件时会激活应用。 （有关详细信息，请参阅[应用生命周期](https://msdn.microsoft.com/library/windows/apps/Mt243287)。）

我们对 [launch](https://msdn.microsoft.com/library/windows/apps/BR224693) 激活感兴趣。 只要应用未在运行而后由用户激活，就会*启动*该应用。 不论应用过去是否关闭，或者是否属于首次启动，激活都会调用 [WinJS.UI.processAll](https://msdn.microsoft.com/library/windows/apps/Hh440975)。 **WinJS.UI.processAll** 包含在对 [setPromise](https://msdn.microsoft.com/library/windows/apps/JJ215609) 方法的调用中，这样可确保初始屏幕不会在应用页面准备就绪前停止。

**提示** **WinJS.UI.processAll** 函数扫描 default.html 文件中是否存在 WinJS 控件并初始化这些控件。 到目前为止，我们尚未添加其中任何控件，但最好保留此代码，以便在以后需要时进行添加。

为非 WinJS 控件注册事件处理程序的较好时机是在调用 **WinJS.UI.processAll** 之后。

**注册事件处理程序**

-   在 main.js 的 [**onactivated**](https://msdn.microsoft.com/library/windows/apps/BR212679) 事件处理程序中，检索 `helloButton` 并使用 [**addEventListener**](https://msdn.microsoft.com/library/windows/apps/Hh441145) 为 [**click**](https://msdn.microsoft.com/library/windows/apps/Hh441312) 事件注册事件处理程序。 在调用 [**WinJS.UI.processAll**](https://msdn.microsoft.com/library/windows/apps/Hh440975) 之后添加此代码。

```javascript
   app.onactivated = function (args) {
           // Omitted code
           if (isFirstActivation) {
              document.addEventListener("visibilitychange", onVisibilityChanged);
              args.setPromise(WinJS.UI.processAll());

              // Add your code to retrieve the button and register the event handler.
              var helloButton = document.getElementById("helloButton");
              helloButton.addEventListener("click", buttonClickHandler, false);
            }

```    



运行应用。 当你在文本框中输入姓名并单击按钮时，应用会显示个性化问候。

**注意** 如果你想知道我们为何在代码中使用 [addEventListener](https://msdn.microsoft.com/library/windows/apps/Hh441145) 注册事件而不是在 HTML 中设置 [onclick](https://msdn.microsoft.com/library/windows/apps/Hh441312) 事件，请参阅[基本应用编码](https://msdn.microsoft.com/library/windows/apps/Hh780660)以获得详细说明。

## 步骤 6：添加 Windows JavaScript 库控件


除了标准 HTML 控件外，你的应用还可以使用 [Windows JavaScript 库](https://msdn.microsoft.com/library/windows/apps/BR229782)中的任何控件，例如 [WinJS.UI.DatePicker](https://msdn.microsoft.com/library/windows/apps/BR211681)、[WinJS.UI.FlipView](https://msdn.microsoft.com/library/windows/apps/BR211711)、[WinjS.UI.ListView](https://msdn.microsoft.com/library/windows/apps/BR211837) 和 [WinJS.UI.Rating](https://msdn.microsoft.com/library/windows/apps/BR211895) 控件。

与 HTML 控件不同，WinJS 控件没有专用的标记元素：例如，你不能通过添加 `<rating />` 元素创建 [Rating](https://msdn.microsoft.com/library/windows/apps/BR211895) 控件。 若要添加 WinJS 控件，可以创建 **div** 元素并使用 [data-win-control](https://msdn.microsoft.com/library/windows/apps/Hh440969) 属性指定所需控件的类型。 若要添加 **Rating** 控件，请将该属性设置为“WinJS.UI.Rating”。

**将 Rating 控件添加到应用。**

1.  在 index.html 文件中，在 `greetingOutput` **div** 之后添加 [label](https://msdn.microsoft.com/library/windows/apps/Hh453321) 和 [Rating](https://msdn.microsoft.com/library/windows/apps/BR211895) 控件。

    ```html
    <body class="win-type-body">
        <h1>Hello, world!</h1>
        <p>What's your name?</p>
        <input id="nameInput" type="text" />
        <button id="helloButton">Say "Hello"</button>
        <div id="greetingOutput"></div>
        <label for="ratingControlDiv">
            Rate this greeting:
        </label>
        <div id="ratingControlDiv" data-win-control="WinJS.UI.Rating">
        </div>
    </body>
    ```

2.  在本地计算机上运行应用。 请注意新的 [**Rating**](https://msdn.microsoft.com/library/windows/apps/BR211895) 控件。

   ![具有 Windows JavaScript 库控件的 Hello, world 应用](images/helloworld-4-winjs.png)

> 对于要加载的 **Rating**，你的页面必须调用 [WinJS.UI.processAll](https://msdn.microsoft.com/library/windows/apps/Hh440975)。 由于我们的应用使用的是 Visual Studio 模板之一，你的 main.js 已包含对 **WinJS.UI.processAll** 的调用，如前面所述，因此你无需添加任何代码。

现在，单击 **Rating** 控件可更改分级（但它不会执行任何其他操作）。 当用户更改分级时，让我们使用事件处理程序来执行一些操作。

## 步骤 7：为 Windows JavaScript 库控件注册事件处理程序


为 WinJS 控件注册事件处理程序的方法与为标准 HTML 控件注册事件处理程序稍有不同。 如前所述，**onactivated** 事件处理程序调用 **WinJS.UI.processAll** 方法来初始化标记中的 WinJS。 **WinJS.UI.processAll** 调用包含在对 **setPromise** 方法的调用中，如下所示：

```javascript
            args.setPromise(WinJS.UI.processAll());           
```

如果 **Rating** 是标准 HTML 控件，则你可以在对 **WinJS.UI.processAll** 的调用之后添加事件处理程序。 但是，对于类似于我们的 **Rating** 的 WinJS 控件，情况稍复杂一些。 由于 **WinJS.UI.processAll** 为我们创建 **Rating** 控件，因此我们无法向 **Rating** 添加事件处理程序，直到 **WinJS.UI.processAll** 完成其处理。

如果 **WinJS.UI.processAll** 是典型方法，我们可以在调用该方法后立即注册 **Rating** 事件处理程序。 但是，**WinJS.UI.processAll** 方法是异步的，因此它后面的任何代码都可能在 **WinJS.UI.processAll** 完成之前运行。 那么我们该怎么办？ 我们使用 [Promise](https://msdn.microsoft.com/library/windows/apps/BR211867) 对象在 **WinJS.UI.processAll** 完成时接收通知。

与所有异步 WinJS 方法类似，**WinJS.UI.processAll** 会返回一个 **Promise** 对象。 **Promise** 是对某件事将在将来发生的“承诺”，当该事件发生时，表示 **Promise** 已完成。

[Promise](https://msdn.microsoft.com/library/windows/apps/BR211867) 对象具有 [then](https://msdn.microsoft.com/library/windows/apps/BR229728) 方法，该方法接受“completed”函数作为参数。 **Promise** 在完成时调用此函数。

通过向“completed”函数添加代码并将其传递到 **Promise** 对象的 **then** 方法，你可以确保代码在 **WinJS.UI.processAll** 完成后执行。

**输出用户选择的 Rating 值**

1.  在 index.html 文件中，创建一个 [**div**](https://msdn.microsoft.com/library/windows/apps/Hh453133) 元素以显示分级值，并向其提供 **id**“ratingOutput”。

    ```html
        <body class="win-type-body">
        <h1>Hello, world!</h1>
        <p>What's your name?</p>
        <input id="nameInput" type="text" />
        <button id="helloButton">Say "Hello"</button>
        <div id="greetingOutput"></div>
        <label for="ratingControlDiv">
            Rate this greeting:
        </label>
        <div id="ratingControlDiv" data-win-control="WinJS.UI.Rating">
        </div>
        <div id="ratingOutput"></div>
    </body>
```

2.  在 main.js 文件中，为 **Rating** 控件的 [change](https://msdn.microsoft.com/library/windows/apps/BR211891) 事件创建一个名为 `ratingChanged` 的事件处理程序。 [eventInfo](https://msdn.microsoft.com/library/windows/apps/Hh465776) 参数包含 **detail.tentativeRating** 属性，该属性可提供新的用户分级。 检索该值并在输出 **div** 中显示该值。

    ```javascript
        function ratingChanged(eventInfo) {

            var ratingOutput = document.getElementById("ratingOutput");
            ratingOutput.innerText = eventInfo.detail.tentativeRating;
        }
```

3.  更新调用 [WinJS.UI.processAll](https://msdn.microsoft.com/library/windows/apps/Hh440975) 的 [onactivated](https://msdn.microsoft.com/library/windows/apps/BR212679) 事件处理程序中的代码，方法是添加对 [then](https://msdn.microsoft.com/library/windows/apps/BR229728) 方法的调用并将其传递给 `completed` 函数。 在 `completed` 函数中，检索托管 [Rating](https://msdn.microsoft.com/library/windows/apps/BR211895) 控件的 `ratingControlDiv` 元素。 然后使用 [winControl](https://msdn.microsoft.com/library/windows/apps/Hh770814) 属性来检索实际的 **Rating** 控件。 （此示例定义 `completed` 内联函数。）

    ```javascript
           args.setPromise(WinJS.UI.processAll().then(function completed() {

                    // Retrieve the div that hosts the Rating control.
                    var ratingControlDiv = document.getElementById("ratingControlDiv");

                    // Retrieve the actual Rating control.
                    var ratingControl = ratingControlDiv.winControl;

                    // Register the event handler.
                    ratingControl.addEventListener("change", ratingChanged, false);

                }));
```

4.  可以在调用 [**WinJS.UI.processAll**](https://msdn.microsoft.com/library/windows/apps/Hh440975) 之后为 HTML 控件注册事件处理程序，也可在 `completed` 函数中注册它们。 为简单起见，我们继续将所有事件处理程序注册移到 [**then**](https://msdn.microsoft.com/library/windows/apps/BR229728) 事件处理程序内。

    下面是更新后的 [**onactivated**](https://msdn.microsoft.com/library/windows/apps/BR212679) 事件处理程序：

    ```javascript
    (function () {
    "use strict";

    var app = WinJS.Application;
    var activation = Windows.ApplicationModel.Activation;
    var isFirstActivation = true;

    app.onactivated = function (args) {
        if (args.detail.kind === activation.ActivationKind.voiceCommand) {
            // TODO: Handle relevant ActivationKinds. For example, if your app can be started by voice commands,
            // this is a good place to decide whether to populate an input field or choose a different initial view.
        }
        else if (args.detail.kind === activation.ActivationKind.launch) {
            // A Launch activation happens when the user launches your app via the tile
            // or invokes a toast notification by clicking or tapping on the body.
            if (args.detail.arguments) {
                // TODO: If the app supports toasts, use this value from the toast payload to determine where in the app
                // to take the user in response to them invoking a toast notification.
            }
            else if (args.detail.previousExecutionState === activation.ApplicationExecutionState.terminated) {
                // TODO: This application had been suspended and was then terminated to reclaim memory.
                // To create a smooth user experience, restore application state here so that it looks like the app never stopped running.
                // Note: You may want to record the time when the app was last suspended and only restore state if they've returned after a short period.
            }
        }

        if (!args.detail.prelaunchActivated) {
            // TODO: If prelaunchActivated were true, it would mean the app was prelaunched in the background as an optimization.
            // In that case it would be suspended shortly thereafter.
            // Any long-running operations (like expensive network or disk I/O) or changes to user state which occur at launch
            // should be done here (to avoid doing them in the prelaunch case).
            // Alternatively, this work can be done in a resume or visibilitychanged handler.
        }

        if (isFirstActivation) {
            // TODO: The app was activated and had not been running. Do general startup initialization here.
            document.addEventListener("visibilitychange", onVisibilityChanged);

            args.setPromise(WinJS.UI.processAll().then(function completed() {
                var ratingControlDiv = document.getElementById("ratingControlDiv");
                var ratingControl = ratingControlDiv.winControl;
                ratingControl.addEventListener("change",ratingChanged, false);
            }));

            var helloButton = document.getElementById("helloButton");
            helloButton.addEventListener("click", buttonClickHandler, false);

        }

        isFirstActivation = false;
    };

    ```        

    运行应用。 选择评级值时，它将在 [**Rating**](https://msdn.microsoft.com/library/windows/apps/BR211895) 控件下方输出数值。

![电脑上已完成的 Hello world 应用](images/helloworld-5-winjs.png)

## 摘要

祝贺你，你已使用 JavaScript 和 HTML创建了你的第一款适用于 Windows 10 和 UWP 的应用！



<!--HONumber=Aug16_HO3-->


