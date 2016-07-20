---
author: martinekuan
ms.assetid: 03A74239-D4B6-4E41-B2FA-6C04F225B844
title: "创建“Hello, world”应用 \\(XAML\\)"
description: "本教程指导你如何使用 Extensible Application Markup Language \\(XAML\\) 和 C# 创建一个面向 Windows 10 上通用 Windows 平台 (UWP) 的简单“Hello, world”应用。"
translationtype: Human Translation
ms.sourcegitcommit: 3de603aec1dd4d4e716acbbb3daa52a306dfa403
ms.openlocfilehash: 0a524d51f713c37ce2069b4e750bf3ed20fe19ab

---

# 创建“Hello, world”应用 \(XAML\)

本教程指导你如何使用 Extensible Application Markup Language \(XAML\) 和 C# 创建一个面向 Windows 10 上通用 Windows 平台 \(UWP\) 的简单“Hello, world”应用。 借助 Microsoft Visual Studio 中的单个项目，你可以生成可在任何 Windows 10 设备上运行的应用。 这里我们侧重于创建可在桌面和移动设备上正常运行的应用。

**重要提示** 本教程适用于 Microsoft Visual Studio 2015 和 Windows 10。 它在早期版本中无法正常运行。

此处你将了解如何：

-   创建面向 Windows 10 和 UWP 的新 Visual Studio 项目。
-   将 XAML 内容添加到起始页。
-   处理触控、笔以及鼠标输入。
-   在 Visual Studio 中，在本地桌面上和在手机仿真器中运行该项目。
-   使 UI 适应不同的屏幕大小。

## 开始之前...


-   我们将直接跳到你用于创建简单通用应用的步骤。 因此我们强烈建议你在开始本教程前，阅读并了解 [Windows 10 中的新增功能](https://dev.windows.com/whats-new-windows-10-dev-preview)以及[什么是通用 Windows 应用](whats-a-uwp.md)中的概述信息。
-   若要完成本教程，你需要 Windows 10 和 Visual Studio 2015。 有关详细信息，请参阅[准备工作](get-set-up.md)。
-   我们假设你已基本了解 [XAML 概述](https://msdn.microsoft.com/library/windows/apps/Mt185595)中的 XAML 和概念。
-   我们还假设你正使用 Visual Studio 中的默认窗口布局。 如果要更改默认布局，你可以在“窗口”****菜单中，通过使用“重置窗口布局”****命令来重置它。

##  第 1 步：在 Visual Studio 中创建新项目


1.  启动 Visual Studio 2015。

   将出现 Visual Studio 2015 起始页。 （从现在开始，我们将 Visual Studio 2015 简称为 Visual Studio。）

2.  在“文件”****菜单上，依次选择“新建”**** > “项目”****。

   会出现“新建项目”****对话框。 可以在对话框的左侧窗格中选择要显示模板的类型。

3.  在左侧窗格中，依次展开“已安装”&gt;“模板”&gt;“Visual C#”&gt;“Windows”****，然后选取“通用”****模板组。 对话框的中心窗格会显示一系列用于通用 Windows 平台 \(UWP\) 应用的项目模板。

   ![“新建项目”窗口 ](images/newproject-cs.png)
   
   （如果你没有看到这些选项，请确保已安装通用 Windows 应用开发工具。 有关详细信息，请参阅[准备工作](get-set-up.md)。）

4.  在中心窗格中，选择“空白应用(通用 Windows)”****模板。

   “空白应用”****模板会创建一个最基本的 UWP 应用，该应用可以编译和运行，但不包含任何用户界面控件或数据。 本教程将指导你向该应用添加控件。

5.  在**“名称”**文本框中，键入“HelloWorld”。
6.  单击“确定”****以创建项目。

   Visual Studio 会创建项目并在“解决方案资源管理器”****中显示该项目。

   ![适用于 HelloWorld 项目的 Visual Studio 解决方案资源管理器](images/solutionexplorer-cs.png)

尽管“空白应用”****为最基本的模板，但该模板仍包含很多文件：

-   清单文件 \(Package.appxmanifest\)，介绍应用（其名称、描述、磁贴、起始页等等）并列出应用包含的文件。
-   用于在“开始”菜单中显示的一组徽标图像（Assets/Square150x150Logo.scale-200.png、Assets/Square44x44Logo.scale-200.png 和 Assets/Wide310x150Logo.scale-200.png）。
-   表示应用位于 Windows 应用商店的图像 \(Assets/StoreLogo.png\)。
-   用于在应用启动时显示的初始屏幕 \(Assets/SplashScreen.scale-200.png\)。
-   应用的 XAML 和代码文件（App.xaml 和 App.xaml.cs）。
-   起始页 \(MainPage.xaml\) 和附带的代码文件 \(MainPage.xaml.cs\)，这些文件在应用启动时运行。

这些文件是使用 C# 的所有 UWP 应用必不可少的文件。 在 Visual Studio 中创建的每一个项目都包含这些文件。

## 第 2 步：修改起始页


### 文件中包含哪些内容？

若要查看和编辑项目中的文件，请双击“解决方案资源管理器”****中的文件。 默认情况下，你可以像展开文件夹一样展开 XAML 文件以查看其相关联的代码文件。 XAML 文件在拆分视图中打开，可同时显示设计界面和 XAML 编辑器。

在本教程中，你可以只使用少量以前列出的文件：App.xaml、MainPage.xaml 和 MainPage.xaml.cs。

### App.xaml 和 App.xaml.cs

App.xaml 是你声明应用中所使用的资源的位置。 App.xaml.cs 是 App.xaml 的代码隐藏文件。 代码隐藏是与 XAML 页的部分类结合的代码。 XAML 与代码隐藏一同组成完整的类。 App.xaml.cs 是应用的入口点。 与所有代码隐藏页面一样，它包含一个调用 `InitializeComponent` 方法的构造函数。 你不必编写 `InitializeComponent` 方法。 该方法由 Visual Studio 生成，其主要作用是初始化在 XAML 文件中声明的元素。 App.xaml.cs 还包含一些处理应用激活和挂起的方法。

### MainPage.xaml

在 MainPage.xaml 中，为应用定义 UI。 你可以直接使用 XAML 标记添加元素，也可以使用 Visual Studio 提供的设计工具。 MainPage.xaml.cs 是 MainPage.xaml 的代码隐藏页面。 你可以在其中添加应用逻辑和事件处理程序。

这两个文件一起定义称为 `MainPage` 的新类，该类继承自 `HelloWorld` 命名空间中的 [**Page**](https://msdn.microsoft.com/library/windows/apps/BR227503)。

MainPage.xaml

```xml
    <Page
    x:Class="HelloWorld.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:HelloWorld"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    </Grid>
</Page>
```

MainPage.xaml.cs

```csharp
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;

namespace HelloWorld
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
        }
    }
}
```

### 修改起始页

现在，让我们来向应用添加一些内容。

**修改起始页**

1.  在“解决方案资源管理器”****中双击 MainPage.xaml 以将其打开。
2.  在 XAML 编辑器中，为 UI 添加控件。

   在根 [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704) 中，添加此 XAML。 它包含一个标题为 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652) 的 [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/BR209635)、一个询问用户名称的 **TextBlock**、一个用于接受用户名称的 [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683) 元素、一个 [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265)，以及另一个用于显示问候的 **TextBlock**。 其中一些控件有名称，因此稍后你可以在代码中引用它们。

```xml    
    <StackPanel x:Name="contentPanel" Margin="8,32,0,0">
        <TextBlock Text="Hello, world!" Margin="0,0,0,40"/>
        <TextBlock Text="What' s your name?"/>
        <StackPanel x:Name="inputPanel" Orientation="Horizontal" Margin="0,20,0,20">
            <TextBox x:Name="nameInput" Width="280" HorizontalAlignment="Left"/>
            <Button x:Name="inputButton" Content="Say &quot;Hello&quot;"/>
        </StackPanel>
        <TextBlock x:Name="greetingOutput"/>
    </StackPanel>
```    

    The controls that you added in the XAML editor show up in the design view.

## 第 3 步：启动应用


至此，你已创建了一个非常简单的应用。 现在是构建、部署和启动应用并查看其外观的好时机。 你可以在本地计算机上、模拟器或仿真器中或者在远程设备上调试应用。 下面是 Visual Studio 中的目标设备菜单。

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

初始屏幕会消失，然后会出现你的应用。 它的外观如下所示。

![初始应用屏幕](images/helloworld-1-cs.png)

按 Windows 键以打开“开始”****菜单，然后显示所有应用。 请注意，本地部署应用会将其磁贴添加到**“开始”**菜单。 若要再次运行该应用（不是在调试模式下），请在**“开始”**菜单中点击或单击其磁贴。

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

最好在具有小型屏幕和有限内存的设备上测试应用，因此请使用“仿真器 10.0.10240.0 WVGA 4 英寸 512MB”****选项。
**在移动设备仿真器上开始调试**

1.  在“标准”****工具栏上的目标设备菜单（![“开始调试”菜单](images/startdebug-full.png)）中，选取“仿真器 10.0.10240.0 WVGA 4 英寸 512MB”****。
2.  单击工具栏中的“开始调试”****按钮（![“开始调试”按钮](images/startdebug-sm.png)）。

   -或者-

   在“调试”****菜单中，单击“开始调试”****。

   -或者-

   按 F5。

Visual Studio 将启动选定的仿真器，然后部署和启动你的应用。 在移动设备仿真器中，应用的外观如下所示。

![移动设备上的初始应用屏幕](images/helloworld-1-cs-phone.png)

你首先会注意到，在屏幕较小的移动设备上，该按钮被推送至屏幕之外。 在本教程的后面部分中，你将了解如何使 UI 适应不同的屏幕大小，以使应用始终保持良好外观。

你可能还会注意到，可以在 [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683) 中键入内容，但此时单击或点击 [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265) 不会起任何作用。 在接下来的步骤中，你将为按钮的 [**Click**](https://msdn.microsoft.com/library/windows/apps/BR227737) 事件创建事件处理程序，以显示个性化的问候。 将事件处理程序代码添加到 MainPage.xaml.cs 文件。

## 第 4 步：创建事件处理程序


XAML 元素可以在出现某些事件时发送消息。 这些事件消息为你提供了可以采取一些操作响应事件的机会。 将用于响应事件的代码放在事件处理程序方法中。 多个应用中最常见事件之一为用户单击 [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265)。

让我们为按钮的 [**Click**](https://msdn.microsoft.com/library/windows/apps/BR227737) 事件创建事件处理程序。 事件处理程序会从 `nameInput` [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683) 控件获取用户名并使用该用户名向 `greetingOutput` [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652) 输出问候语。

### 使用用于触控、鼠标和笔输入的事件

你应处理什么事件？ 由于它们可以在各种设备上运行，请在设计 Windows 应用商店应用时牢记触摸输入。 应用还必须能够处理来自鼠标或触笔的输入。 幸运的是，诸如 [**Click**](https://msdn.microsoft.com/library/windows/apps/BR227737) 和 [**DoubleTapped**](https://msdn.microsoft.com/library/windows/apps/BR208922) 的事件与设备无关。 如果你熟悉 Microsoft .NET 编程，则可以看到鼠标、触控和触笔输入的单独事件，如 **MouseMove**、**TouchMove** 和 **StylusMove**。 在 Windows 应用商店应用中，这些单独的事件使用单个 [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/BR208970) 事件替换，该事件同样适用于触摸、鼠标以及触笔输入。

**添加事件处理程序**

1.  在 XAML 或设计视图中，选择已添加到 MainPage.xaml 的“Say Hello”[**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265)。
2.  在“属性窗口”****中，单击“事件”按钮（![“事件”按钮](images/eventsbutton.png)）。
3.  在事件列表的顶部查找 [**Click**](https://msdn.microsoft.com/library/windows/apps/BR227737) 事件。 在事件的文本框中，键入处理 **Click** 事件的函数名称。 对于本示例，请键入“Button\_Click”。

   ![属性窗口中的事件列表](images/xaml-hw-event.png)

4.  按 Enter 键。 事件处理程序方法在代码编辑器中创建和打开，因此你可以添加要在事件发生时执行的代码。

    在 XAML 编辑器中，更新 [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265) 的 XAML 以声明 [**Click**](https://msdn.microsoft.com/library/windows/apps/BR227737) 事件处理程序，如下所示。

```xml   
   <Button x:Name="inputButton" Content="Say &quot;Hello&quot;" Click="Button_Click"/>
```    

5.  向在代码隐藏页面中创建的事件处理程序添加代码。 在事件处理程序中，从 `nameInput` [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683) 控件检索用户名并使用该用户名创建问候语。 使用 `greetingOutput` [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652) 显示相关结果。
    
```csharp    
    private void Button_Click(object sender, RoutedEventArgs e)
    {
        greetingOutput.Text = "Hello, " + nameInput.Text + "!";
    }
```    

6.  在本地计算机上调试应用。 当你在文本框中输入姓名并单击按钮后，应用会显示个性化问候。

## 第 5 步：使 UI 适应不同的窗口大小


现在我们将使 UI 适应不同的屏幕大小，以使其在移动设备上外观良好。 若要执行此操作，添加 [**VisualStateManager**](https://msdn.microsoft.com/library/windows/apps/BR209021) 并设置应用于不同视觉状态的属性。

**调整 UI 布局**

1.  在 XAML 编辑器中，在根 [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704) 元素的开始标记后添加此 XAML 块。

```xml    
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup>
            <VisualState x:Name="wideState">
                <VisualState.StateTriggers>
                    <AdaptiveTrigger MinWindowWidth="641" />
                </VisualState.StateTriggers>
            </VisualState>
            <VisualState x:Name="narrowState">
                <VisualState.StateTriggers>
                    <AdaptiveTrigger MinWindowWidth="0" />
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Target="inputPanel.Orientation" Value="Vertical"/>
                    <Setter Target="inputButton.Margin" Value="0,4,0,0"/>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
```    

2.  在本地计算机上调试应用。 请注意，UI 外观与以前相同，除非窗口变得窄于 641 像素。
3.  在移动设备仿真器上调试应用。 请注意 UI 使用你在 `narrowState` 中定义的属性并正确显示在小屏幕上。

![移动应用屏幕](images/helloworld-2-cs-phone.png)

如果你在以前版本的 XAML 中使用过 [**VisualStateManager**](https://msdn.microsoft.com/library/windows/apps/BR209021)，你可能会注意到 XAML 在此处使用简化的语法。

名为 `wideState` 的 [**VisualState**](https://msdn.microsoft.com/library/windows/apps/BR209007) 具有一个 [**AdaptiveTrigger**](https://msdn.microsoft.com/library/windows/apps/Dn890382)，并且其 [**MinWindowWidth**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.adaptivetrigger.minwindowwidth) 属性设置为 641。 这意味着仅在窗口宽度不小于 641 像素的最小值时应用该状态。 你没有为此状态定义任何 [**Setter**](https://msdn.microsoft.com/library/windows/apps/BR208817) 对象，因此它会将你在 XAML 中定义的布局属性用于页面内容。

名为 `narrowState` 的第二个 [**VisualState**](https://msdn.microsoft.com/library/windows/apps/BR209007) 具有一个 [**AdaptiveTrigger**](https://msdn.microsoft.com/library/windows/apps/Dn890382)，其 [**MinWindowWidth**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.adaptivetrigger.minwindowwidth) 属性设置为 0。 当窗口宽度大于 0 但小于 641 像素时，应用此状态。 （在 641 像素时，应用 `wideState`。）在此状态下，定义一些 [**Setter**](https://msdn.microsoft.com/library/windows/apps/BR208817) 对象以更改 UI 中控件的布局属性：

-   将 `inputPanel` 元素的 [**Orientation**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.orientation) 从 **Horizontal** 更改为 **Vertical**。
-   将 4 的顶部边距添加到 `inputButton` 元素。

## 摘要


祝贺你，你已创建了自己的第一个适用于 Windows 10 和 UWP 的应用！



<!--HONumber=Jul16_HO2-->


