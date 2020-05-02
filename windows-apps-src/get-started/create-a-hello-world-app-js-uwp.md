---
ms.assetid: 3a17e682-40be-41b4-8bd3-fbf0b15259d6
title: 创建“Hello, world”应用 (JS)
description: 本教程指导你如何使用 JavaScript 和 HTML 创建一个简单的&\#0034;Hello, world&\#0034;应用，该应用面向 Windows 10 上的通用 Windows 平台 (UWP)。
ms.date: 09/12/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a161a5e26bd647a37b06a270ef6714d54bc16d80
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "74467417"
---
# <a name="create-a-hello-world-app-js"></a>创建“Hello, world”应用 (JS)

本教程指导你如何使用 JavaScript 和 HTML 创建一个简单的“Hello, world”应用，该应用面向 Windows 10 上的通用 Windows 平台 (UWP)。 通过 Microsoft Visual Studio 中的单个项目，可以生成可在任何 Windows 10 设备上运行的应用。

> [!NOTE]
> 本教程使用 Visual Studio Community 2017。 如果使用的是不同版本的 Visual Studio，则其外观可能稍有不同。

> [!WARNING]
> Visual Studio 2019 不支持 Javascript UWP 应用开发。 必须使用 Visual Studio 2017 开发 Javascript UWP 应用。

在本文中，你将了解如何：

-   创建面向 **Windows 10** 和 **UWP** 的新 **Visual Studio 2017** 项目。
-   添加 HTML 和 JavaScript 内容。
-   在 Visual Studio 中，在本地桌面中运行该项目。

## <a name="before-you-start"></a>开始之前

-   [UWP 应用是什么？](universal-application-platform-guide.md)
-   若要完成本教程，需要 Windows 10 和 Visual Studio。 [准备工作](get-set-up.md)。
-   我们还假设你使用的是 Visual Studio 中的默认窗口布局。 若要更改默认布局，可以在“窗口”菜单中，使用“重置窗口布局”命令来重置它   。

## <a name="step-1-create-a-new-project-in-visual-studio"></a>步骤 1：在 Visual Studio 中创建新项目

1.  启动 Visual Studio 2017。

2.  在“文件”菜单中选择“新建”>“项目”以打开“创建新项目”对话框    。

3.  选择“空白应用(通用 Windows) JavaScript”，然后选择“下一步”。  

    （如果未看到任何通用模板，可能是缺少用于创建 UWP 应用的组件。 可以在“创建新项目”对话框中单击“打开 Visual Studio 安装程序”来重复安装流程并添加 UWP 支持   。 请参阅[准备工作](get-set-up.md)。

4.  在“配置新项目”对话框中，输入“HelloWorld”作为“项目名称”，然后选择“创建”     。

> [!NOTE]
> 如果你是首次使用 Visual Studio，则可能会看到要求启用“开发人员模式”的“设置”对话框  。 开发人员模式是一种用于启用某些功能（如允许直接运行应用，而不是只能从 Store 运行）的特殊设置。 有关更多信息，请阅读[启用设备进行开发](enable-your-device-for-development.md)。 若要继续使用本指南，请依次选择“开发人员模式”、“是”，然后关闭对话框   。

 ![激活“开发人员模式”对话框](images/win10-cs-00.png)

5.  将显示“目标版本/最低版本”对话框。 默认设置均适用于本教程，因此请选择“确定”以创建项目  。

    ![“解决方案资源管理器”窗口](images/win10-cs-02.png)

6.  新项目打开时，项目文件将显示在右侧的“解决方案资源管理器”窗格中  。 若要查看文件，可能需要选择“解决方案资源管理器”选项卡而不是“属性”选项卡   。

    ![“解决方案资源管理器”窗口](images/win10-js-02.png)

尽管“空白应用(通用 Windows)”为最基本的模板，但该模板仍包含很多文件  。 这些文件是所有使用 JavaScript 的 UWP 应用必不可少的文件。 在 Visual Studio 中创建的每一个项目都包含这些文件。


### <a name="whats-in-the-files"></a>文件中包含哪些内容？

若要查看和编辑项目中的文件，请双击“解决方案资源管理器”中的文件  。

*default.css*

-  应用默认使用的样式表。

*main.js*

- 默认 JavaScript 文件。 index.html 文件将会引用它。

*index.html*

- 启动应用时加载和显示的应用网页。

*一组徽标图像*
-   Assets/Square150x150Logo.scale-200.png 表示“开始”菜单中的应用  。
-   Assets/StoreLogo.png 表示 Microsoft Store 中的应用。
-   Assets/SplashScreen.scale-200.png 是应用启动时显示的初始屏幕。

## <a name="step-2-adding-a-button"></a>步骤 2：添加按钮

选择 **index.html** 以在编辑器中将其选中，然后更改所包含的 HTML，使其如下所示。

```html
<!DOCTYPE html>
<html>

<head>
    <meta charset="utf-8" />
    <title>Hello World</title>
    <script src="js/main.js"></script>
    <link href="css/default.css" rel="stylesheet" />
</head>

<body>
    <p>Click the button..</p>
    <button id="button">Hello world!</button>
</body>

</html>
```

它应如下所示。

 ![项目的 HTML](images/win10-js-03.png)

此 HTML 可以引用包含 JavaScript 的 *main.js*，然后为网页主体添加单行文本和单个按钮。 该按钮具有给定的 *ID*，因此 JavaScript 能够引用它。


## <a name="step-3-adding-some-javascript"></a>步骤 3:添加一些 JavaScript

现在，我们将添加 JavaScript。 选择 **main.js** 以将其选中，然后添加以下内容。

```javascript
// Your code here!

window.onload = function () {
    document.getElementById("button").onclick = function (evt) {
        sayHello()
    }
}


function sayHello() {
    var messageDialog = new Windows.UI.Popups.MessageDialog("Hello, world!", "Alert");
    messageDialog.showAsync();
}

```

它应如下所示。

 ![项目的 JavaScript](images/win10-js-04.png)

此 JavaScript 用于声明两个函数。 显示 *index.html* 时，将会自动调用 *window.onload* 函数。 它用于查找按钮（使用声明的 ID）和添加 onclick 处理程序：单击此按钮时将调用此方式。

第二个函数 *sayHello()* 用于创建和显示对话。 这与你从先前的 JavaScript 开发中了解的 *Alert()* 函数非常类似。


## <a name="step-4-run-the-app"></a>步骤 4：运行应用！

现在，可以按 F5 运行该应用。 此时将会加载该应用，并显示网页。 选中该按钮，此时会弹出消息对话框。

 ![运行项目](images/win10-js-05.png)



## <a name="summary"></a>摘要


祝贺你，现已创建一个适用于 Windows 10 和 UWP 的 JavaScript 应用！ 这是一个非常简单的示例，不过，你现在即可开始添加喜欢的 JavaScript 库和框架，以创建自己的应用。 它是一个 UWP 应用，因此，可将其发布到 Store。 有关如何添加第三方框架的示例，请参阅以下项目：

* [Microsoft Store 中用 JavaScript 和 CreateJS 编写的简单 2D UWP 游戏](get-started-tutorial-game-js2d.md)
* [适用于 Microsoft Store、用 JavaScript 和 threeJS 编写的 3D UWP 游戏](get-started-tutorial-game-js3d.md)
