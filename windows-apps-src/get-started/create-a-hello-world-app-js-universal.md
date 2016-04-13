---
ms.assetid: CFB3601D-3459-465F-80E2-520F57B88F62
创建“Hello, world”应用 (JS)
本教程指导你如何使用 JavaScript 和 HTML 创建一个简单的“Hello, world”应用，该应用面向 Windows 10 上的通用 Windows 平台 (UWP)。
---
# 创建“Hello, world”应用 (JS)

本教程指导你如何使用 JavaScript 和 HTML 创建一个简单的“Hello, world”应用，该应用面向 Windows 10 上的通用 Windows 平台 (UWP)。 通过 Microsoft Visual Studio 中的单个项目，你可以生成可在任何 Windows 10 设备上运行的应用。 这里我们侧重于创建可在桌面和移动设备上正常运行的应用。

**重要提示** 本教程适用于 Microsoft Visual Studio 2015 和 Windows 10。 它在早期版本中无法正常运行。

在此处，你将了解如何：

-   创建一个新项目
-   向起始页中添加 HTML 内容
-   处理触控、笔以及鼠标输入
-   在 Visual Studio 中，在本地桌面和手机仿真器中运行该项目。
-   创建你自己的自定义样式
-   使用 Windows JavaScript 库控件

##开始之前...


-   我们将直接跳到你用于创建简单通用应用的步骤。 因此我们强烈建议你在开始本教程前，阅读并了解 [Windows 10 中的新增功能](https://dev.windows.com/whats-new-windows-10-dev-preview)以及[什么是通用 Windows 应用](whats-a-uwp.md)中的概述信息。
-   若要完成本教程，你需要 Windows 10 和 Visual Studio 2015。 有关详细信息，请参阅[准备工作](get-set-up.md)。
-   我们还假设你使用的是 Visual Studio 中的默认窗口布局。 如果要更改默认布局，你可以在“窗口”****菜单中，通过使用“重置窗口布局”****命令来重置它。

##步骤 1：在 Visual Studio 中创建新项目


我们来创建名为 `HelloWorld` 的新应用。 以下是操作方法：

1.  启动 Visual Studio 2015。

    Visual Studio 2015 开始屏幕将显示。

    （从现在开始，我们将 Visual Studio 2015 简称为 Visual Studio 。）

2.  在“文件”****菜单上，选择“新建”****>“项目”****。

    “新建项目”****对话框将显示。 可在对话框的左侧窗格中选择要显示的模板的类型。

3.  在左侧窗格中，展开“已安装>模板> JavaScript > Windows”****，然后选取“通用”****模板组。 对话框的中心窗格会显示适用于通用 Windows 平台 (UWP) 应用的项目模板的列表。

    ![“新建项目”窗口 ](images/js-tut-newproject.png)

    在本教程中，我们使用 **Blank App** 模板。 此模板创建一个最基本的 UWP 应用，该应用能够编译和运行，但不包含用户界面控件或数据。 本教程将指导你向应用中添加控件和数据。

4.  在中心窗格中，选择“空白应用（通用 Windows）”****模板。

    “空白应用”****模板会创建一个最基本的 UWP 应用，该应用可以编译和运行，但不包含任何用户界面控件或数据。 本教程将指导你向该应用添加控件。

5.  在**“名称”**文本框中，键入“HelloWorld”。
6.  单击**“确定”**以创建项目。

    Visual Studio 会创建项目并在“解决方案资源管理器”****中显示该项目。

    ![HelloWorld 项目的 Visual Studio 解决方案资源管理器](images/js-tut-helloworld.png)

尽管 **Blank App** 是最基本的模板，但该模板仍包含少量文件：

-   清单文件 (package.appxmanifest) 介绍了应用（它的名称、介绍、磁贴、起始页、初始屏幕等等）并列出了应用包含的文件。
-   用于在“开始”菜单中显示的一组徽标图像（images/Square150x150Logo.scale-200.png、images/Square44x44Logo.scale-200.png 和 images/Wide310x150Logo.scale-200.png）。
-   用于在 Windows 应用商店中表示应用的图像 (images/StoreLogo.png)。
-   用于在应用启动时显示的初始屏幕 (images/SplashScreen.scale-200.png)。
-   用于在应用启动时运行的起始页 (default.html) 和附带的 JavaScript 文件 (default.js)。

若要查看和编辑文件，请双击**“解决方案资源管理器”**中的文件。

这些文件是所有使用 JavaScript 的 UWP 应用必不可少的文件。 在 Visual Studio 中创建的所有项目都包含这些文件。

##步骤 2：启动应用


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

   在**“调试”**菜单中，单击**“开始调试”**。

   -或者-

   按 F5。

应用将在窗口中打开，并且将首先显示默认初始屏幕。 初始屏幕由一个图像 (SplashScreen.png) 和背景色（在应用的清单文件中指定）定义。

初始屏幕会消失，然后会出现你的应用。 它包含带有文本“内容在此处”的黑屏。

![电脑上的 HelloWorld 应用](images/helloworld-1-js.png)

按 Windows 键以打开**“开始”**菜单，然后显示所有应用。 请注意，本地部署应用会将其磁贴添加到**“开始”**菜单。 若要再次运行该应用（不是在调试模式下），请在**“开始”**菜单中点击或单击其磁贴。

它还无法执行很多操作，但祝贺你已构建了第一个 UWP 应用！

**停止调试**

-   单击工具栏中的**“停止调试”**按钮（![“停止调试”按钮](images/stopdebug.png)）。

   -或者-

   在**“调试”**菜单中，单击**“停止调试”**。

   -或者-

   关闭应用窗口。

### 在移动设备仿真器上启动该应用

你的应用可在任何 Windows 10 设备上运行，让我们看一下它在 Windows Phone 上的情况如何。

除了在桌面设备上执行调试的选项，Visual Studio 还提供用于在连接到计算机的物理移动设备上或移动设备仿真器上部署和调试应用的选项。 你可以为带有不同内存和显示配置的设备在仿真器中进行选择。

-   **设备**
-   **仿真器 <SDK version> WVGA 4 英寸 512MB**
-   **仿真器 <SDK version> WVGA 4 英寸 1GB**
-   等（采用其他配置的各种仿真器）

最好在带有小型屏幕和有限内存的设备上测试应用，因此请使用**“仿真器 10.0.10240.0 WVGA 4 英寸 512MB”**选项。
**在移动设备仿真器上开始调试**

1.  在**“标准”**工具栏上的目标设备菜单（![“开始调试”菜单](images/startdebug-full.png)）中，选取**仿真器 10.0.10240.0 WVGA 4 英寸 512MB**。
2.  单击工具栏中的**“开始调试”**按钮（![“开始调试”按钮](images/startdebug-sm.png)）。

   -或者-

   在**“调试”**菜单中，单击**“开始调试”**。

   
Visual Studio 将启动选定的仿真器，然后部署和启动你的应用。 在移动设备仿真器中，应用的外观如下所示。

![移动设备上的初始应用屏幕](images/helloworld-1-js-phone.png)

## 步骤 3：修改你的起始页

Visual Studio 为你创建的文件之一是 default.html，应用的起始页。 应用运行时，会显示其起始页的内容。 起始页还包含对应用的代码文件和样式表的引用。 以下是 Visual Studio 为你创建的起始页：

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title>HelloWorld</title>

    <!-- WinJS references -->
    <link href="WinJS/css/ui-dark.css" rel="stylesheet" />
    <script src="WinJS/js/base.js"></script>
    <script src="WinJS/js/ui.js"></script>

    <!-- HelloWorld references -->
    <link href="/css/default.css" rel="stylesheet" />
    <script src="/js/default.js"></script>
</head>
<body class="win-type-body">
    <p>Content goes here</p>
</body>
</html>
```

我们来向 default.html 文件中添加一些新内容。 正如你向任何其他 HTML 文件中添加内容一样，你在 [**body**](https://msdn.microsoft.com/library/windows/apps/Hh453011) 元素内添加内容。 你可以使用 HTML5 元素创建应用（具有[少数例外](https://msdn.microsoft.com/library/windows/apps/Hh465380)）。 这表示你可以使用 HTML5 元素，如 [**h1**](https://msdn.microsoft.com/library/windows/apps/Hh441078)、[**p**](https://msdn.microsoft.com/library/windows/apps/Hh453431)、[**button**](https://msdn.microsoft.com/library/windows/apps/Hh453017)、[**div**](https://msdn.microsoft.com/library/windows/apps/Hh453133) 以及 [**img**](https://msdn.microsoft.com/library/windows/apps/Hh466114)。

**修改起始页**

1.  使用以下内容替代 [**body**](https://msdn.microsoft.com/library/windows/apps/Hh453011) 元素中的现有内容：显示“Hello, world!”的首级标题、询问用户名的一些文本、用于接受用户名的 [**input**](https://msdn.microsoft.com/library/windows/apps/Hh453271) 元素、[**button**](https://msdn.microsoft.com/library/windows/apps/Hh453017) 以及 [**div**](https://msdn.microsoft.com/library/windows/apps/Hh453133) 元素。 向 **input**、**button** 和 **div** 分配 ID。

 ```html
    <body class="win-type-body">
        <h1>Hello, world!</h1>
        <p>What' s your name?</p>
        <input id="nameInput" type="text" />
        <button id="helloButton">Say "Hello"</button>
        <div id="greetingOutput"></div>
    </body>
 ```

2.  在本地计算机上运行应用。 它的外观如下所示。

![带有新内容的 HelloWorld 应用](images/helloworld-2-js.png)

   你可以在 [**input**](https://msdn.microsoft.com/library/windows/apps/Hh453271) 元素中进行键入，但现在单击 [**button**](https://msdn.microsoft.com/library/windows/apps/Hh453017) 不会执行任何操作。 某些对象（例如 **button**）可以在发生特定事件时发送消息。 这些事件消息为你提供了可以采取一些操作响应事件的机会。 将用于响应事件的代码放在事件处理程序方法中。

   在接下来的步骤中，我们为显示个性化问候的 [**button**](https://msdn.microsoft.com/library/windows/apps/Hh453017) 创建事件处理程序。 我们向 default.js 文件添加事件处理程序代码。

##第 4 步：创建事件处理程序

创建新项目时，Visual Studio 为我们创建了 /js/default.js 文件。 此文件包含用于处理应用生命周期的代码。 你还可以在此文件中编写为 default.html 文件提供交互性的其他代码。

打开 default.js 文件。

在我们开始添加自己的代码之前，我们来看一下该文件中代码的头几行和最后几行：

```javascript
(function () {
    "use strict";

     // Omitted code 

 })(); 
```

你可能会对此处发生的情况感到疑惑。 代码的这些行覆盖了自我执行匿名函数中 default.js 代码的其他位置。 自我执行匿名函数使避免冲突或意外修改原本无意修改的值的情况变得更简单。 此操作还可防止全局命名空间中出现不需要的标识符，这有助于提高性能。 它看上去有一点奇怪，但却是良好的编程实践。

代码的下一行为 JavaScript 代码打开了[严格模式](https://msdn.microsoft.com/en-us/library/windows/apps/br230269.aspx)。 严格模式为代码提供了额外的错误检查。 例如，它防止你使用隐式声明的变量或为只读属性分配值。

查看 default.js 中代码的剩余部分。 它处理了应用的 [**activated**](https://msdn.microsoft.com/library/windows/apps/BR212679) 和 [**checkpoint**](https://msdn.microsoft.com/library/windows/apps/BR229839) 事件。 我们以后会深入了解这些事件的详细信息。 现在，只要了解启动应用时会触发 **activated** 事件。

```javascript
   (function () {
    "use strict";

    var app = WinJS.Application;
    var activation = Windows.ApplicationModel.Activation;

    app.onactivated = function (args) {
        if (args.detail.kind === activation.ActivationKind.launch) {
            if (args.detail.previousExecutionState !== activation.ApplicationExecutionState.terminated) {
                // TODO: This application has been newly launched. Initialize your application here.
            } else {
                // TODO: This application was suspended and then terminated.
                // To create a smooth user experience, restore application state here so that it looks like the app never stopped running.
            }
            args.setPromise(WinJS.UI.processAll());
        }
    };

    app.oncheckpoint = function (args) {
        // TODO: This application is about to be suspended. Save any state that needs to persist across suspensions here.
        // You might use the WinJS.Application.sessionState object, which is automatically saved and restored across suspension.
        // If you need to complete an asynchronous operation before your application is suspended, call args.setPromise().
    };

    app.start();
})();
```

我们来为 [**button**](https://msdn.microsoft.com/library/windows/apps/Hh453017) 定义事件处理程序。 新的事件处理程序会从 `nameInput`[**input**](https://msdn.microsoft.com/library/windows/apps/Hh453271) 控件获取用户名并使用该用户名向在上一部分中创建的 `greetingOutput`[**div**](https://msdn.microsoft.com/library/windows/apps/Hh453133) 元素输出问候。

### 使用用于触控、鼠标和笔输入的事件

在 UWP 应用中，你无须担心触控、鼠标与其他指针输入形式之间的区别。 你只需使用你了解的事件（如 [**click**](https://msdn.microsoft.com/library/windows/apps/Hh441312)），这些事件适用于所有输入形式。

**提示** 应用还可以使用新的 *MSPointer\** 和 *MSGesture\** 事件，这些事件适用于触控输入、鼠标输入以及笔输入，并可以提供有关触发事件的设备的其他信息。 有关详细信息，请参阅[响应用户交互](https://msdn.microsoft.com/library/windows/apps/Hh700412)和[手势、操作以及交互](https://msdn.microsoft.com/library/windows/apps/Hh761498)。

我们继续并创建事件处理程序。

**创建事件处理程序**

1.  在 default.js 中，在 [**app.oncheckpoint**](https://msdn.microsoft.com/library/windows/apps/BR229839) 事件处理程序之后且对 [**app.start**](https://msdn.microsoft.com/library/windows/apps/BR229705) 的调用之前，创建名为 `buttonClickHandler` 的 [**click**](https://msdn.microsoft.com/library/windows/apps/Hh441312) 事件处理程序函数，该函数接受名为 `eventInfo` 的单个参数。
```javascript
    function buttonClickHandler(eventInfo) {
     
        }
    ```

2.  Inside our event handler, retrieve the user's name from the `nameInput` [**input**](https://msdn.microsoft.com/library/windows/apps/Hh453271) control and use it to create a greeting. Use the `greetingOutput` [**div**](https://msdn.microsoft.com/library/windows/apps/Hh453133) to display the result.
```javascript
    function buttonClickHandler(eventInfo) {
            var userName = document.getElementById("nameInput").value;
            var greetingString = "Hello, " + userName + "!";
            document.getElementById("greetingOutput").innerText = greetingString; 
        }
 ```

你已将事件处理程序添加到 default.js。 现在你需要注册该处理程序。

## 步骤 5：在应用启动时注册事件处理程序


现在，你只需向该按钮注册该事件处理程序。 注册事件处理程序的建议方法是从代码中调用 [**addEventListener**](https://msdn.microsoft.com/library/windows/apps/Hh441145)。 注册事件处理程序的较好时机是在激活应用之时。 幸运的是，Visual Studio 为我们在 default.js 文件中生成了一些代码，可处理应用的激活：[**app.onactivated**](https://msdn.microsoft.com/library/windows/apps/BR212679) 事件处理程序。 我们来看看此代码。

```javascript
    var app = WinJS.Application;
    var activation = Windows.ApplicationModel.Activation;

    app.onactivated = function (args) {
        if (args.detail.kind === activation.ActivationKind.launch) {
            if (args.detail.previousExecutionState !== activation.ApplicationExecutionState.terminated) {
                // TODO: This application has been newly launched. Initialize your application here.
            } else {
                // TODO: This application was suspended and then terminated.
                // To create a smooth user experience, restore application state here so that it looks like the app never stopped running.
            }
            args.setPromise(WinJS.UI.processAll());
        }
    };
```

在 [**onactivated**](https://msdn.microsoft.com/library/windows/apps/BR212679) 处理程序内，该代码会检查发生了何种类型的激活。 存在多种不同类型的激活。 例如，当用户启动应用和用户希望打开与应用关联的文件时会激活应用。 （有关详细信息，请参阅[应用生命周期](https://msdn.microsoft.com/library/windows/apps/Mt243287)。）

我们关注 [**launch**](https://msdn.microsoft.com/library/windows/apps/BR224693) 激活。 只要应用未在运行而后由用户激活，就会*启动*该应用。

```javascript
    app.onactivated = function (args) {
        if (args.detail.kind === activation.ActivationKind.launch) {
```

如果激活为启动激活，则会检查代码以查看应用上次在运行时如何被关闭。

```javascript
            if (args.detail.previousExecutionState !== activation.ApplicationExecutionState.terminated) {
                // TODO: This application has been newly launched. Initialize your application here.
            } else {
                // TODO: This application was suspended and then terminated.
                // To create a smooth user experience, restore application state here so that it looks like the app never stopped running.
            }
```

然后激活会调用 [**WinJS.UI.processAll**](https://msdn.microsoft.com/library/windows/apps/Hh440975)。

```javascript
            args.setPromise(WinJS.UI.processAll());
        }
    };
```    

不论应用过去是否关闭，或者是否属于首次启动，激活都会调用 [**WinJS.UI.processAll**](https://msdn.microsoft.com/library/windows/apps/Hh440975)。 **WinJS.UI.processAll** 包含在对 [**setPromise**](https://msdn.microsoft.com/library/windows/apps/JJ215609) 方法的调用中，这样可确保初始屏幕不会在应用的页面准备好前停止。

**提示** [**WinJS.UI.processAll**](https://msdn.microsoft.com/library/windows/apps/Hh440975) 函数会扫描 default.html 文件中是否存在 WinJS 控件并初始化这些控件。 到目前为止，我们尚未添加其中任何控件，但最好保留此代码，以便以后需要时要添加它们。

为非 WinJS 控件注册事件处理程序的较好时机是在调用 [**WinJS.UI.processAll**](https://msdn.microsoft.com/library/windows/apps/Hh440975) 之后。

**注册事件处理程序**

-   在 default.js 的 [**onactivated**](https://msdn.microsoft.com/library/windows/apps/BR212679) 事件处理程序中，检索 `helloButton` 并使用 [**addEventListener**](https://msdn.microsoft.com/library/windows/apps/Hh441145) 为 [**click**](https://msdn.microsoft.com/library/windows/apps/Hh441312) 事件注册事件处理程序。 在调用 [**WinJS.UI.processAll**](https://msdn.microsoft.com/library/windows/apps/Hh440975) 之后添加此代码。

```javascript
   app.onactivated = function (args) {
            if (args.detail.kind === activation.ActivationKind.launch) {
                if (args.detail.previousExecutionState !== activation.ApplicationExecutionState.terminated) {
                    // TODO: This application has been newly launched. Initialize your application here.
                } else {
                    // TODO: This application was suspended and then terminated.
                    // To create a smooth user experience, restore application state here so that it looks like the app never stopped running.
                }
                args.setPromise(WinJS.UI.processAll());

             // Retrieve the button and register our event handler. 
                var helloButton = document.getElementById("helloButton");
                helloButton.addEventListener("click", buttonClickHandler, false);
            }
        };
```    

以下是更新的 default.js 文件的完整代码：

```javascript
   (function () {
    "use strict";

    var app = WinJS.Application;
    var activation = Windows.ApplicationModel.Activation;

    app.onactivated = function (args) {
        if (args.detail.kind === activation.ActivationKind.launch) {
            if (args.detail.previousExecutionState !== activation.ApplicationExecutionState.terminated) {
                // TODO: This application has been newly launched. Initialize your application here.
            } else {
                // TODO: This application was suspended and then terminated.
                // To create a smooth user experience, restore application state here so that it looks like the app never stopped running.
            }
            args.setPromise(WinJS.UI.processAll());

            // Retrieve the button and register our event handler. 
            var helloButton = document.getElementById("helloButton");
            helloButton.addEventListener("click", buttonClickHandler, false);
        }
    };

    app.oncheckpoint = function (args) {
        // TODO: This application is about to be suspended. Save any state that needs to persist across suspensions here.
        // You might use the WinJS.Application.sessionState object, which is automatically saved and restored across suspension.
        // If you need to complete an asynchronous operation before your application is suspended, call args.setPromise().
    };

    function buttonClickHandler(eventInfo) {
        var userName = document.getElementById("nameInput").value;
        var greetingString = "Hello, " + userName + "!";
        document.getElementById("greetingOutput").innerText = greetingString;
    }

    app.start();
})();
```

运行应用。 当你在文本框中输入姓名并单击按钮时，应用会显示个性化问候。 下面是应用在本地计算机和仿真器中的外观。

![HelloWorld 应用中的个性化问候](images/helloworld-3-js.png)

![HelloWorld 应用中的个性化问候](images/helloworld-3-js-phone.png)

**注意** 如果你想知道我们为何在代码中使用 [**addEventListener**](https://msdn.microsoft.com/library/windows/apps/Hh441145) 来注册事件而不是在 HTML 中设置 [**onclick**](https://msdn.microsoft.com/library/windows/apps/Hh441312) 事件，请参阅[基本应用编码](https://msdn.microsoft.com/library/windows/apps/Hh780660)以获得详细说明。

## 步骤 6：添加 Windows JavaScript 库控件


除了标准 HTML 控件外，你的应用还可以使用 Windows JavaScript 库中的任何控件，例如 [**WinJS.UI.DatePicker**](https://msdn.microsoft.com/library/windows/apps/BR211681)、[**WinJS.UI.FlipView**](https://msdn.microsoft.com/library/windows/apps/BR211711)、[**WinjS.UI.ListView**](https://msdn.microsoft.com/library/windows/apps/BR211837) 和 [**WinJS.UI.Rating**](https://msdn.microsoft.com/library/windows/apps/BR211895) 控件。

与 HTML 控件不同的是，WinJS 控件没有专用的标记元素：例如，你不能通过添加 `<rating />` 元素来创建 [**Rating**](https://msdn.microsoft.com/library/windows/apps/BR211895) 控件。 若要添加 WinJS 控件，可以创建 [**div**](https://msdn.microsoft.com/library/windows/apps/Hh453133) 元素并使用 [**data-win-control**](https://msdn.microsoft.com/library/windows/apps/Hh440969) 属性指定所需的控件类型。 若要添加 **Rating** 控件，请将该属性设置为“WinJS.UI.Rating”。

我们来将一个 [**Rating**](https://msdn.microsoft.com/library/windows/apps/BR211895) 控件添加到应用。

1.  在你的 default.html 文件中，在 `greetingOutput` [**div**](https://msdn.microsoft.com/library/windows/apps/Hh453133) 之后添加 [**label**](https://msdn.microsoft.com/library/windows/apps/Hh453321) 和 [**Rating**](https://msdn.microsoft.com/library/windows/apps/BR211895) 控件。

```html
        <body class="win-type-body">
        <h1>Hello, world!</h1>
        <p>What' s your name?</p>
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

    For the [**Rating**](https://msdn.microsoft.com/library/windows/apps/BR211895) to load, your page must call [**WinJS.UI.processAll**](https://msdn.microsoft.com/library/windows/apps/Hh440975). Because our app is using one of the Visual Studio templates, your default.js already includes a call to **WinJS.UI.processAll**, as described earlier, so you don't have to add any code.

2.  在本地计算机上运行应用。 请注意新的 [**Rating**](https://msdn.microsoft.com/library/windows/apps/BR211895) 控件。

   ![具有适用于 JavaScript 的 Windows 库控件的 Hello, world 应用](images/helloworld-4-JS.png)

现在，单击 [**Rating**](https://msdn.microsoft.com/library/windows/apps/BR211895) 控件来更改评级（但它不会执行任何其他操作）。 当用户更改评级时，让我们使用事件处理程序来执行一些操作。

## 步骤 7：为 Windows JavaScript 库控件注册事件处理程序


为 WinJS 控件注册事件处理程序的方法与为标准 HTML 控件注册事件处理程序稍有不同。 如前所述，我们提到 [**onactivated**](https://msdn.microsoft.com/library/windows/apps/BR212679) 事件处理程序调用 [**WinJS.UI.processAll**](https://msdn.microsoft.com/library/windows/apps/Hh440975) 方法来初始化标记中的 WinJS。 **WinJS.UI.processAll** 包含在对 [**setPromise**](https://msdn.microsoft.com/library/windows/apps/JJ215609) 方法的调用中。

```javascript
            args.setPromise(WinJS.UI.processAll());           
```

假如 [**Rating**](https://msdn.microsoft.com/library/windows/apps/BR211895) 是标准 HTML 控件，则你可以在对 [**WinJS.UI.processAll**](https://msdn.microsoft.com/library/windows/apps/Hh440975) 的调用之后添加事件处理程序。 但是，对于 WinJS 控件，类似于我们的 **Rating**，情况稍复杂一些。 由于 **WinJS.UI.processAll** 为我们创建了 **Rating** 控件，因此我们无法向 **Rating** 添加事件处理程序，直到 **WinJS.UI.processAll** 完成其处理。

假如 [**WinJS.UI.processAll**](https://msdn.microsoft.com/library/windows/apps/Hh440975) 是常用方法，则我们可以在调用该方法后立即注册 [**Rating**](https://msdn.microsoft.com/library/windows/apps/BR211895) 事件处理程序。 但是，**WinJS.UI.processAll** 方法是异步的，因此它后面的任何代码都可能在 **WinJS.UI.processAll** 完成之前运行。 那么我们该怎么办？ 我们使用 [**Promise**](https://msdn.microsoft.com/library/windows/apps/BR211867) 对象在 **WinJS.UI.processAll** 完成时接收通知。

与所有异步 WinJS 方法类似，[**WinJS.UI.processAll**](https://msdn.microsoft.com/library/windows/apps/Hh440975) 会返回一个 [**Promise**](https://msdn.microsoft.com/library/windows/apps/BR211867) 对象。 **Promise** 是对某件事会在将来发生的“承诺”，当该事件发生时，表示 **Promise** 已完成。

[
            **Promise**](https://msdn.microsoft.com/library/windows/apps/BR211867) 对象具有 [**then**](https://msdn.microsoft.com/library/windows/apps/BR229728) 方法，该方法接受“completed”函数作为参数。 完成时 **Promise** 会调用此函数。

通过向“completed”函数加代码并将其传递到 [**Promise**](https://msdn.microsoft.com/library/windows/apps/BR211867) 对象的 [**then**](https://msdn.microsoft.com/library/windows/apps/BR229728) 方法，你可以确保代码在 [**WinJS.UI.processAll**](https://msdn.microsoft.com/library/windows/apps/Hh440975) 完成后执行。

1.  当用户选择评级时，输出该评级值。 在 default.html 文件中，创建一个 [**div**](https://msdn.microsoft.com/library/windows/apps/Hh453133) 元素以显示评级值，并提供其 **id**“ratingOutput”。
```html
        <body class="win-type-body">
        <h1>Hello, world!</h1>
        <p>What' s your name?</p>
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

2.  在 default.js 文件中，为 [**Rating**](https://msdn.microsoft.com/library/windows/apps/BR211895) 控件的 [**change**](https://msdn.microsoft.com/library/windows/apps/BR211891) 事件创建一个名为 `ratingChanged` 的事件处理程序。 [
            **eventInfo**](https://msdn.microsoft.com/library/windows/apps/Hh465776) 参数包含 **detail.tentativeRating** 属性，该属性可提供新的用户评级。 检索该值并在输出 [**div**](https://msdn.microsoft.com/library/windows/apps/Hh453133) 中显示该值。

```javascript
        function ratingChanged(eventInfo) {

            var ratingOutput = document.getElementById("ratingOutput");
            ratingOutput.innerText = eventInfo.detail.tentativeRating; 
        }
    ```

3.  Update the code in the [**onactivated**](https://msdn.microsoft.com/library/windows/apps/BR212679) event handler that calls [**WinJS.UI.processAll**](https://msdn.microsoft.com/library/windows/apps/Hh440975) by adding a call to the [**then**](https://msdn.microsoft.com/library/windows/apps/BR229728) method and passing it a `completed` function. In the `completed` function, retrieve the `ratingControlDiv` element that hosts the [**Rating**](https://msdn.microsoft.com/library/windows/apps/BR211895) control. Then use the [**winControl**](https://msdn.microsoft.com/library/windows/apps/Hh770814) property to retrieve the actual **Rating** control. (This example defines the `completed` function inline.)

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

4.  While it's fine to register event handlers for HTML controls after the call to [**WinJS.UI.processAll**](https://msdn.microsoft.com/library/windows/apps/Hh440975), it's also OK to register them inside your `completed` function. For simplicity, let's go ahead and move all your event handler registrations inside the [**then**](https://msdn.microsoft.com/library/windows/apps/BR229728) event handler.

Here's the updated [**onactivated**](https://msdn.microsoft.com/library/windows/apps/BR212679) event handler:

```javascript
       app.onactivated = function (args) {
            if (args.detail.kind === activation.ActivationKind.launch) {
                if (args.detail.previousExecutionState !== activation.ApplicationExecutionState.terminated) {
                    // TODO: This application has been newly launched. Initialize your application here.
                } else {
                    // TODO: This application was suspended and then terminated.
                    // To create a smooth user experience, restore application state here so that it looks like the app never stopped running.
                }
                args.setPromise(WinJS.UI.processAll().then(function completed() {

                    // Retrieve the div that hosts the Rating control.
                    var ratingControlDiv = document.getElementById("ratingControlDiv");

                    // Retrieve the actual Rating control.
                    var ratingControl = ratingControlDiv.winControl;

                    // Register the event handler. 
                    ratingControl.addEventListener("change", ratingChanged, false);

                    // Retrieve the button and register our event handler. 
                    var helloButton = document.getElementById("helloButton");
                    helloButton.addEventListener("click", buttonClickHandler, false);

                }));

            }
        };
```        

5.  运行应用。 选择评级值时，它将在 [**Rating**](https://msdn.microsoft.com/library/windows/apps/BR211895) 控件下方输出数值。

![电脑上已完成的 Hello world 应用](images/helloworld-5-js.png)

![手机上已完成的 Hello world 应用](images/helloworld-5-js-phone.png)

## 摘要

祝贺你，你已使用 JavaScript 和 HTML创建了你的第一款适用于 Windows 10 和 UWP 的应用！



<!--HONumber=Mar16_HO1-->


