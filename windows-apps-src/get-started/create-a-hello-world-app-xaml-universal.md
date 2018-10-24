---
author: GrantMeStrength
ms.assetid: 03A74239-D4B6-4E41-B2FA-6C04F225B844
title: 了解如何创建“Hello, world”应用 (XAML)
description: 使用 Extensible Application Markup Language (XAML) 和 C# 创建一个面向 Windows 10 上通用 Windows 平台 (UWP) 的简单“Hello, world”应用。
ms.author: jken
ms.date: 03/06/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, 第一个应用, Hello World
ms.localizationpriority: medium
ms.openlocfilehash: 950b2f3fac44c8350a51fd5c1b7071f05c92d746
ms.sourcegitcommit: 4b97117d3aff38db89d560502a3c372f12bb6ed5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/23/2018
ms.locfileid: "5434852"
---
# <a name="create-a-hello-world-app-xaml"></a>创建“Hello, world”应用 \(XAML\)

本教程指导你如何使用 XAML 和 C# 创建一个简单的“Hello, world”应用，该应用面向 Windows10 上的通用 Windows 平台 (UWP)。 通过 Microsoft Visual Studio 中的单个项目，可以生成可在任何 Windows10 设备上运行的应用。

在此处，你将了解如何：

-   创建面向 **Windows10** 和 **UWP** 的新 **Visual Studio 2017** 项目。
-   编写 XAML 即可更改起始页上的 UI。
-   在 Visual Studio 中，在本地桌面中运行该项目。
-   使用 SpeechSynthesizer，可在你按下某个按钮时使应用说话。


## <a name="before-you-start"></a>开始之前...

-   [什么是通用 Windows 应用？](universal-application-platform-guide.md)
-   [下载 Visual Studio 2017（和 Windows 10）](https://developer.microsoft.com/windows/downloads)。 如果需要帮助，请了解如何[设置](get-set-up.md)。
-   我们还假设你使用的是 Visual Studio 中的默认窗口布局。 如果要更改默认布局，你可以在**窗口**菜单中，通过使用**重置窗口布局**命令来重置它。

> [!NOTE]
> 本教程使用 Visual Studio Community 2017。 如果使用的是不同版本的 Visual Studio，则其外观可能稍有不同。

## <a name="video-summary"></a>视频摘要

<iframe src="https://channel9.msdn.com/Blogs/One-Dev-Minute/Writing-Your-First-Windows-10-App/player" width="640" height="360" allowFullScreen frameBorder="0"></iframe>



## <a name="step-1-create-a-new-project-in-visual-studio"></a>步骤 1：在 Visual Studio 中创建新项目。

1.  启动 Visual Studio 2017。

2.  从**文件**菜单中，选择**新建 > 项目**以打开*新建项目*对话框。

3.  从左侧的模板列表中，选择**已安装 > Visual C# > Windows 通用**以查看 UWP 项目模板列表。

    （如果未看到任何通用模板，可能是缺少用于创建 UWP 应用的组件。 可以通过在**新建项目**对话框中单击*打开 Visual Studio 安装程序*来重复安装流程并添加 UWP 支持。 请参阅[准备工作](get-set-up.md)。）

    ![如何重复安装流程](images/win10-cs-install.png)

4.  选择**空白应用（通用 Windows）** 模板，然后输入“HelloWorld”作为**名称**。 选择**确定**。

    ![“新建项目”窗口](images/win10-cs-01.png)

> [!NOTE]
> 如果你是首次使用 Visual Studio，则可能会看到要求启用**开发人员模式**的“设置”对话框。 开发人员模式是一种用于启用某些功能（如允许直接运行应用，而不是只能从应用商店运行）的特殊设置。 有关更多信息，请阅读[启用设备进行开发](enable-your-device-for-development.md)。 若要继续使用本指南，请选择**开发人员模式**，然后单击**是**并关闭对话框。

 ![激活“开发人员模式”对话框](images/win10-cs-00.png)

5.  将显示“目标版本/最低版本”对话框。 默认设置均适用于本教程，因此选择**确定**以创建项目。

    ![“解决方案资源管理器”窗口](images/win10-cs-02.png)

6.  新项目打开时，项目文件将显示在右侧的**解决方案资源管理器**窗格中。 若要查看你的文件，可能需要选择**解决方案资源管理器**选项卡，而不是**属性**选项卡。

    ![“解决方案资源管理器”窗口](images/win10-cs-03.png)

尽管**空白应用(通用 Windows)** 为最基本的模板，但该模板仍包含很多文件。 这些文件是使用 C# 的所有 UWP 应用必不可少的文件。 在 Visual Studio 中创建的每一个项目都包含这些文件。


### <a name="whats-in-the-files"></a>文件中包含哪些内容？

若要查看和编辑项目中的文件，请双击**解决方案资源管理器**中的文件。 像展开文件夹一样展开 XAML 文件即可查看其关联的代码文件。 XAML 文件在拆分视图中打开，可同时显示设计界面和 XAML 编辑器。
> [!NOTE]
> 什么是 XAML？ Extensible Application Markup Language (XAML) 是用于定义应用的用户界面的语言。 可以手动输入，也可以使用 Visual Studio 设计工具创建。 一个 .xaml 文件具有一个包含逻辑的 .xaml.cs 代码隐藏文件。 XAML 与代码隐藏一同组成完整的类。 有关详细信息，请参阅 [XAML 概述](https://msdn.microsoft.com/library/windows/apps/Mt185595)。

*App.xaml 和 App.xaml.cs*

-   App.xaml 是你声明应用中所使用的资源的位置。
-   App.xaml.cs 是 App.xaml 的代码隐藏文件。 与所有代码隐藏页面一样，它包含一个调用 `InitializeComponent` 方法的构造函数。 你不必编写 `InitializeComponent` 方法。 该方法由 Visual Studio 生成，其主要作用是初始化在 XAML 文件中声明的元素。
-   App.xaml.cs 是应用的入口点。
-   App.xaml.cs 还包含一些处理应用激活和挂起的方法。

*MainPage.xaml*

-   在 MainPage.xaml 中，为应用定义 UI。 你可以直接使用 XAML 标记添加元素，也可以使用 Visual Studio 提供的设计工具。
-   MainPage.xaml.cs 是 MainPage.xaml 的代码隐藏页面。 你可以在其中添加应用逻辑和事件处理程序。
-   这两个文件一起定义称为 `MainPage` 的新类，该类继承自 `HelloWorld` 命名空间中的 [**Page**](https://msdn.microsoft.com/library/windows/apps/BR227503)。

*Package.appxmanifest*
-   描述应用的清单文件：应用的名称、描述、磁贴、起始页等等。
-   包括应用包含的文件列表。

*一组徽标图像*
-   Assets/Square150x150Logo.scale-200.png 表示“开始”菜单中的应用。
-   Assets/StoreLogo.png 表示 Microsoft Store 中的应用。
-   Assets/SplashScreen.scale-200.png 是应用启动时显示的初始屏幕。

## <a name="step-2-adding-a-button"></a>步骤 2：添加按钮

### <a name="using-the-designer-view"></a>使用设计器视图

让我们向我们的页面中添加一个按钮。 在本教程中，你可以只使用少量以前列出的文件：App.xaml、MainPage.xaml 和 MainPage.xaml.cs。

1.  双击 **MainPage.xaml** 即可在设计视图中打开它。

    你将发现图形视图位于屏幕顶部，而 XAML 代码视图位于底部。 可以对任一视图进行更改，但现在我们将使用图形视图。

    ![“解决方案资源管理器”窗口](images/win10-cs-04.png)

2.  单击左侧垂直的**工具箱**，打开 UI 控件列表。 （可以单击“工具箱”的标题栏中的固定图标使其始终可见。）

    ![“解决方案资源管理器”窗口](images/win10-cs-05.png)

3.  展开**常见 XAML 控件**，然后将 **Button** 拖动到设计画面的中间。

    ![“解决方案资源管理器”窗口](images/win10-cs-06.png)

    如果查看 XAML 代码窗口，你会发现 Button 已添加到此窗口中：

 ```XAML
<Button x:name="button" Content="Button" HorizontalAlignment="Left" Margin = "152,293,0,0" VerticalAlignment="Top"/>
 ```

4.  更改按钮的文本。

    在 XAML 代码视图中单击一下，然后将内容从“Button”更改为“Hello, world!”。

```XAML
<Button x:name="button" Content="Hello, world!" HorizontalAlignment="Left" Margin = "152,293,0,0" VerticalAlignment="Top"/>
```

注意设计画布中显示的按钮如何更新显示新文本。

![“解决方案资源管理器”窗口](images/win10-cs-07.png)

## <a name="step-3-start-the-app"></a>步骤 3：启动应用


至此，你已创建了一个非常简单的应用。 现在是构建、部署和启动应用并查看其外观的好时机。 你可以在本地计算机上、模拟器或仿真器中或者在远程设备上调试应用。 下面是 Visual Studio 中的目标设备菜单。

![用于调试应用的设备目标下拉列表](images/uap-debug.png)

### <a name="start-the-app-on-a-desktop-device"></a>在桌面设备上启动应用

默认情况下，应用在本地计算机上运行。 目标设备菜单提供用于在桌面设备系列中的设备上调试应用的多个选项。

-   **模拟器**
-   **本地计算机**
-   **远程计算机**

**在本地计算机上开始调试**

1.  在 **“标准”** 工具栏上的目标设备菜单（![“开始调试”菜单](images/startdebug-full.png)）中，确保已选中 **“本地计算机”**。 （它是默认选择。）
2.  单击工具栏中的 **“开始调试”** 按钮（![“开始调试”按钮](images/startdebug-sm.png)）。

   -或者-

   在**调试**菜单中，单击**开始调试**。

   -或者-

   按 F5。

应用将在窗口中打开，并且将首先显示默认初始屏幕。 初始屏幕由一个图像 \(SplashScreen.png\) 和背景色（在应用的清单文件中指定）定义。

初始屏幕会消失，然后会出现你的应用。 它的外观如下所示。

![初始应用屏幕](images/win10-cs-08.png)

按 Windows 键以打开**开始**菜单，然后显示所有应用。 请注意，本地部署应用会将其磁贴添加到**开始**菜单。 若要稍后再次运行该应用（不是在调试模式下），请在**开始**菜单中点击或单击其磁贴。

它还无法执行很多操作，但祝贺你已生成了第一个 UWP 应用！

**停止调试**

   单击工具栏中的 **“停止调试”** 按钮（![“停止调试”按钮](images/stopdebug.png)）。

   -或者-

   在**调试**菜单中，单击**停止调试**。

   -或者-

   关闭应用窗口。

## <a name="step-4-event-handlers"></a>步骤 4：事件处理程序

“事件处理程序”听起来很复杂，但它只是事件发生（如用户单击按钮）时调用的代码的另一个名称。

1.  如果你尚未这样做，请停止应用运行。

2.  双击设计画布中的按钮控件，让 Visual Studio 为该按钮创建事件处理程序。

  当然，你可以手动创建所有代码。 也可以单击按钮选中它，然后访问右下方的**属性**窗格。 如果切换到**事件**（小闪电球），可以添加事件处理程序的名称。

3.  编辑 *MainPage.xaml.cs*（即代码隐藏页面）中的事件处理程序代码。 这是事情变得有趣的所在之处。 默认的事件处理程序如下所示：

```C#
private void Button_Click(object sender, RoutedEventArgs e)
{

}
```

  让我们更改它，使它如下所示：

```C#
private async void Button_Click(object sender, RoutedEventArgs e)
        {
            MediaElement mediaElement = new MediaElement();
            var synth = new Windows.Media.SpeechSynthesis.SpeechSynthesizer();
            Windows.Media.SpeechSynthesis.SpeechSynthesisStream stream = await synth.SynthesizeTextToStreamAsync("Hello, World!");
            mediaElement.SetSource(stream, stream.ContentType);
            mediaElement.Play();
        }
```

此外，确保包含了 **async** 关键字，否则尝试运行应用时会出错。

### <a name="what-did-we-just-do"></a>我们刚才做了什么？

此代码使用一些 Windows API 创建一个语音合成对象，然后提供给该对象一些要说的文本。 （有关使用 SpeechSynthesis 的详细信息，请参阅 [SpeechSynthesis 命名空间](https://msdn.microsoft.com/library/windows/apps/windows.media.speechsynthesis.aspx)文档。）

运行该应用并单击按钮时，计算机（或手机）会逐字地说出“Hello, World!”。


## <a name="summary"></a>小结

祝贺你，你已创建了自己的第一个适用于 Windows10 和 UWP 的应用！

若要了解如何使用 XAML 来布置你的应用将使用的控件，请尝试[网格教程](../design/layout/grid-tutorial.md)，或直接跳至[下一步](learn-more.md)。

## <a name="see-also"></a>另请参阅

* [你的第一个应用](your-first-app.md)
* [发布 UWP 应用](https://developer.microsoft.com/store/publish-apps)。
* [有关开发 UWP 应用的操作方法文章](https://developer.microsoft.com/windows/apps/develop)
* [适用于 UWP 开发人员的代码示例](https://developer.microsoft.com/windows/samples)
* [什么是通用 Windows 应用？](universal-application-platform-guide.md)
* [注册 Windows 帐户](sign-up.md)
