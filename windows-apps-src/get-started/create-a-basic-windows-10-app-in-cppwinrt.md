---
title: 使用 C++/WinRT 创建“Hello, World!” 应用
description: 本主题将指导你使用 C++/WinRT 创建 Windows 10 UWP“Hello, World!” 应用。 该应用的 UI 是使用 Extensible Application Markup Language (XAML) 定义的。
ms.date: 07/11/2020
ms.topic: article
keywords: windows 10, uwp, cppwinrt, C++/WinRT
ms.localizationpriority: medium
ms.openlocfilehash: e2f4e6b808d0169f4c9f8f7142f218c00f124ae3
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493696"
---
# <a name="create-a-hello-world-app-using-cwinrt"></a>使用 C++/WinRT 创建“Hello, World!” 应用

本主题将指导你使用 C++/WinRT 创建 Windows 10 通用 Windows 平台 (UWP)“Hello, World!” 应用。 该应用的用户界面 (UI) 是使用 Extensible Application Markup Language (XAML) 定义的。

C++/WinRT 是适用于 Windows 运行时 (WinRT) API 的完全标准的新式 C++17 语言投影。 有关详细信息、更多演练和代码示例，请参阅 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/) 文档。 最好先学习 [C++/WinRT 入门](/windows/uwp/cpp-and-winrt-apis/get-started) 这一主题。

## <a name="set-up-visual-studio-for-cwinrt"></a>针对 C++/WinRT 设置 Visual Studio

有关设置 Visual Studio 以进行 C++/WinRT 部署的信息 &mdash; 包括安装和使用 C++/WinRT Visual Studio 扩展 (VSIX) 和 NuGet 包（两者共同提供项目模板，并生成支持）的信息 &mdash; 请参阅[适用于 C++/WinRT 的 Visual Studio 支持](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)。

若要下载 Visual Studio，请参阅[下载](https://visualstudio.microsoft.com/downloads/)。

有关 XAML 的简介，请参阅 [XAML 概述](/windows/uwp/xaml-platform/xaml-overview)

## <a name="create-a-blank-app-helloworldcppwinrt"></a>创建空白应用 (HelloWorldCppWinRT)

我们的第一个应用是“Hello, World!” 应用，它演示了交互性、布局和样式的一些基本功能。

首先在 Microsoft Visual Studio 中创建新项目。 创建“空白应用(C++/WinRT)”项目，然后将其命名为 HelloWorldCppWinRT。 请确保未选中“将解决方案和项目放在同一目录中”。 面向 Windows SDK 的最新正式发布（非预览）版本。

本主题的后续部分将指导你如何生成项目（但在那之前暂时不要生成）。

### <a name="about-the-project-files"></a>关于项目文件

通常在项目文件夹中，每个 `.xaml`（XAML 标记）文件都有相应的 `.idl`、`.h` 和 `.cpp` 文件。 这些文件一起编译为 XAML 页面类型。

你可以修改 XAML 标记文件以创建 UI 元素，并且可以将这些元素绑定到数据源（此任务称为[数据绑定](/windows/uwp/data-binding/)）。 你可以修改 `.h` 和 `.cpp` 文件（有时为 `.idl` 文件），为 XAML 页面添加自定义逻辑 &mdash; 例如事件处理程序。

下面是一些项目文件。

- `App.idl`、`App.xaml`、`App.h` 和 `App.cpp`。 这些文件表示应用对 [Windows::UI::Xaml::Application](/uwp/api/windows.ui.xaml.application) 类的专业化，其中包括应用的入口点。 `App.xaml` 不包含任何特定于页面的标记，但你可以在其中添加用户界面元素样式，还可以添加你希望可从所有页面访问的任何其他元素。 `.h` 和 `.cpp` 文件包含各种应用程序生命周期事件的处理程序。 通常，你在此处添加自定义代码，以在应用启动时初始化应用并在它暂停或终止时执行清理。
- `MainPage.idl`、`MainPage.xaml`、`MainPage.h` 和 `MainPage.cpp`。 包含应用中默认主（启动）页面类型的 XAML 标记和实现，即 MainPage 运行时类。 MainPage 没有导航支持，但它提供了一些默认的 UI 和一个事件处理程序来帮助你入门。
- `pch.h` 和 `pch.cpp`。 这些文件表示项目的预编译头文件。 在 `pch.h` 中，包含不经常更改的任何头文件，然后将 `pch.h` 包含在项目中的其他文件中。

## <a name="a-first-look-at-the-code"></a>代码一览

### <a name="runtime-classes"></a>运行时类

你可能知道，使用 C# 编写的通用 Windows 平台 (UWP) 应用中的所有类都是 Windows 运行时类型。 但在 C++/WinRT 应用程序中创作类型时，可以选择将该类型设为 Windows 运行时类型还是常规 C++ 类/结构/枚举。

项目中的任何 XAML 页面类型都需要是 Windows 运行时类型。 因此 MainPage 是一种 Windows 运行时类型。 具体而言，它是运行时类。 XAML 页面使用的任何类型也需要是 Windows 运行时类型。 当你编写 [Windows 运行时组件](/windows/uwp/winrt-components/create-a-windows-runtime-component-in-cppwinrt)时，如果想要创作可从其他应用中使用的类型，可创作一个 Windows 运行时类型。 在其他情况下，你的类型可以是常规 C++ 类型。 一般而言，可通过任何 Windows 运行时语言使用 Windows 运行时类型。

要了解类型是否是 Windows 运行时类型，可查看它是否是在接口定义语言 (`.idl`) 文件中使用 [Microsoft 接口定义语言 (MIDL)](/uwp/midl-3/) 定义的，这是一种良好的指示。 我们以 MainPage 为例。

```idl
// MainPage.idl
namespace HelloWorldCppWinRT
{
    [default_interface]
    runtimeclass MainPage : Windows.UI.Xaml.Controls.Page
    {
        MainPage();
        Int32 MyProperty;
    }
}
```

下面是实现 MainPage 运行时类及其激活工厂的基本结构，如 `MainPage.h` 中所示。

```cppwinrt
// MainPage.h
...
namespace winrt::HelloWorldCppWinRT::implementation
{
    struct MainPage : MainPageT<MainPage>
    {
        MainPage();

        int32_t MyProperty();
        void MyProperty(int32_t value);
        ...
    };
}

namespace winrt::HelloWorldCppWinRT::factory_implementation
{
    struct MainPage : MainPageT<MainPage, implementation::MainPage>
    {
    };
}
```    

若要更详细地了解是否应针对给定类型创作运行时类，请参阅主题：[使用 C++/WinRT 创作 API](/windows/uwp/cpp-and-winrt-apis/author-apis)。 而有关运行时类和 IDL（`.idl` 文件）之间的连接的详细信息，可以参阅主题 [XAML 控件；绑定到 C++/WinRT 属性](/windows/uwp/cpp-and-winrt-apis/binding-property)。 本主题分步介绍创作新运行时类的过程，第一步是将新的 Midl 文件 (.idl) 项添加到项目。

接下来向 HelloWorldCppWinRT 项目添加一些功能。

## <a name="step-1-modify-your-startup-page"></a>步骤 1： 修改启动页

在“解决方案资源管理器”中，打开 `MainPage.xaml`，以便创作构成用户界面 (UI) 的控件。

删除已存在的 StackPanel 及其内容。 在其位置粘贴以下 XAML。

```xaml
<StackPanel x:Name="contentPanel" Margin="120,30,0,0">
    <TextBlock HorizontalAlignment="Left" Text="Hello, World!" FontSize="36"/>
    <TextBlock Text="What's your name?"/>
    <StackPanel x:Name="inputPanel" Orientation="Horizontal" Margin="0,20,0,20">
        <TextBox x:Name="nameInput" Width="300" HorizontalAlignment="Left"/>
        <Button x:Name="inputButton" Content="Say &quot;Hello&quot;"/>
    </StackPanel>
    <TextBlock x:Name="greetingOutput"/>
</StackPanel>
```

这个新的 [StackPanel](/uwp/api/Windows.UI.Xaml.Controls.StackPanel) 具有一个询问用户名的 [TextBlock](/uwp/api/Windows.UI.Xaml.Controls.TextBlock)、一个接受用户名的 [TextBox](/uwp/api/Windows.UI.Xaml.Controls.TextBox)、一个 [Button](/uwp/api/Windows.UI.Xaml.Controls.Button) 以及另一个 TextBlock 元素    。

由于已删除名为 myButton 的 Button，因此必须从代码中删除对它的引用。 因此，在 `MainPage.cpp` 中删除 MainPage::ClickHandler 函数内的相应代码行。

此时，你已创建一个非常基本的通用 Windows 应用。 若要查看 UWP 应用的外观，请生成并运行应用。

![带有控件的 UWP 应用屏幕](images/xaml-hw-app2.png)

在应用中，你可以在文本框中键入内容。 但单击按钮还不会执行任何操作。

## <a name="step-2-add-an-event-handler"></a>步骤 2： 添加事件处理程序

在 `MainPage.xaml` 中，找到名为 inputButton 的 Button，并为其 [ButtonBase::Click](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) 事件声明一个事件处理程序。 Button 的标记现应如下所示。

```xaml
<Button x:Name="inputButton" Content="Say &quot;Hello&quot;" Click="inputButton_Click"/>
```

按如下所示实现该事件处理程序。

```cppwinrt
// MainPage.h
struct MainPage : MainPageT<MainPage>
{
    ...
    void inputButton_Click(
        winrt::Windows::Foundation::IInspectable const& sender,
        winrt::Windows::UI::Xaml::RoutedEventArgs const& e);
};

// MainPage.cpp
namespace winrt::HelloWorldCppWinRT::implementation
{
    ...
    void MainPage::inputButton_Click(
        winrt::Windows::Foundation::IInspectable const& sender,
        winrt::Windows::UI::Xaml::RoutedEventArgs const& e)
    {
        greetingOutput().Text(L"Hello, " + nameInput().Text() + L"!");
    }
}
```

有关详细信息，请参阅[使用代理来处理事件](/windows/uwp/cpp-and-winrt-apis/handle-events)。

该实现从文本框中检索用户名称，使用该名称创建问候语，并在 greetingOutput 文本块中显示该问候语。

构建并运行应用。 在文本框中键入名称，然后单击按钮。 应用将显示个性化的问候语。

![显示消息的应用屏幕](images/xaml-hw-app3.png)

## <a name="step-3-style-the-startup-page"></a>步骤 3： 设置启动页的样式

### <a name="choose-a-theme"></a>选择主题

轻松自定义应用的外观。 默认情况下，应用使用具有浅色样式的资源。 系统资源还包含深色主题。

若要尝试使用深色主题，请编辑 `App.xaml`，并为 [Application::RequestedTheme](/uwp/api/windows.ui.xaml.application.requestedtheme) 添加一个值。

```xaml
<Application
    ...
    RequestedTheme="Dark">

</Application>
```

对于主要显示图像或视频的应用，我们建议使用深色主题；对于包含大量文本的应用，我们建议使用浅色主题。 如果你使用的是自定义配色方案，则请使用最适合应用外观和感觉的主题。

> [!NOTE]
> 在应用启动时应用主题。 无法在应用运行时更改主题。

### <a name="use-system-styles"></a>使用系统样式

在本节中，更改文本的外观（例如将字号变大）。

在 `MainPage.xaml` 中，找到“What's your name?” TextBlock。 将其 [Style](/uwp/api/windows.ui.xaml.style) 属性设置为对 BaseTextBlockStyle 系统资源键的引用。

```xaml
<TextBlock Text="What's your name?" Style="{ThemeResource BaseTextBlockStyle}"/>
```

BaseTextBlockStyle 是在 `\Program Files (x86)\Windows Kits\10\DesignTime\CommonConfiguration\Neutral\UAP\<version>\Generic\generic.xaml` 处的 [ResourceDictionary](/uwp/api/Windows.UI.Xaml.ResourceDictionary) 中定义的资源键。 下面是该样式设置的属性值。

```xaml
<Style x:Key="BaseTextBlockStyle" TargetType="TextBlock">
    <Setter Property="FontFamily" Value="XamlAutoFontFamily" />
    <Setter Property="FontWeight" Value="SemiBold" />
    <Setter Property="FontSize" Value="14" />
    <Setter Property="TextTrimming" Value="None" />
    <Setter Property="TextWrapping" Value="Wrap" />
    <Setter Property="LineStackingStrategy" Value="MaxHeight" />
    <Setter Property="TextLineBounds" Value="Full" />
</Style>
```

同样，在 `MainPage.xaml` 中找到名为 `greetingOutput` 的 TextBlock。 也将其 Style 设置为 BaseTextBlockStyle。 如果你现在生成并运行应用，则可看到这两个文本块的外观已更改（例如字号现在变大了）。

## <a name="step-4-have-the-ui-adapt-to-different-window-sizes"></a>步骤 4： 使 UI 适应不同的窗口大小

现在，我们将使 UI 动态地适应不断变化的窗口大小，使其在屏幕较小的设备上的显示效果不错。 为此，需要将 [VisualStateManager](/uwp/api/Windows.UI.Xaml.VisualStateManager) 部分添加到 `MainPage.xaml` 中。 为不同的窗口大小定义不同的可视状态，然后设置属性以应用于每个可视状态。

### <a name="adjust-the-ui-layout"></a>调整 UI 布局

将此 XAML 块添加为根 StackPanel 元素的第一个子元素。

```xaml
<StackPanel ...>
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
                    <Setter Target="contentPanel.Margin" Value="20,30,0,0"/>
                    <Setter Target="inputPanel.Orientation" Value="Vertical"/>
                    <Setter Target="inputButton.Margin" Value="0,4,0,0"/>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    ...
</StackPanel>
```

构建并运行应用。 请注意，UI 外观与以前相同，除非将窗口调整为窄于 641 与设备无关的像素 (DIP)。 此时，应用 narrowState 可视状态，同时应用为该状态定义的所有属性 Setter。

名为 `wideState` 的 [**VisualState**](/uwp/api/Windows.UI.Xaml.VisualState) 具有一个 [**AdaptiveTrigger**](/uwp/api/Windows.UI.Xaml.AdaptiveTrigger)，并且其 [**MinWindowWidth**](/uwp/api/windows.ui.xaml.adaptivetrigger.minwindowwidth) 属性设置为 641。 这意味着仅在窗口宽度不小于 641 DIP 的最小值时应用该状态。 你没有为此状态定义任何 [Setter](/uwp/api/Windows.UI.Xaml.Setter) 对象，因此它会将你在 XAML 中定义的布局属性用于页面内容。

名为 `narrowState` 的第二个 [**VisualState**](/uwp/api/Windows.UI.Xaml.VisualState) 具有一个 [**AdaptiveTrigger**](/uwp/api/Windows.UI.Xaml.AdaptiveTrigger)，其 [**MinWindowWidth**](/uwp/api/windows.ui.xaml.adaptivetrigger.minwindowwidth) 属性设置为 0。 当窗口宽度大于 0 但小于 641 DIP 时，应用此状态。 正好为 641 DIP 时，`wideState` 将生效。 在 `narrowState` 中，定义 [**Setter**](/uwp/api/Windows.UI.Xaml.Setter) 对象以更改 UI 中控件的布局属性。

- 将 contentPanel 元素的左边距从 120 降低为 20。
- 将 inputPanel 元素的 [Orientation](/uwp/api/windows.ui.xaml.controls.stackpanel.orientation) 从 Horizontal 更改为 Vertical 。
- 将 4 DIP 的上边距添加到 inputButton 元素。

## <a name="summary"></a>摘要

本演练展示了如何将内容添加到 Windows 通用应用、如何添加交互功能，以及如何更改 UI 外观。
