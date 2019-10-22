---
description: 本主题引导你完成使用 C++/WinRT 创建简单的自定义控件的步骤。 你可以基于此处的信息创建你自己的功能丰富且可自定义的 UI 控件。
title: XAML 自定义（模板化）控件与 C++/WinRT
ms.date: 04/24/2019
ms.topic: article
keywords: windows 10, uwp, 标准, c++, cpp, winrt, 投影, XAML, 自定义, 模板化, 控件
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 6acbd62a8fa75eefb39598dd5bbb6ec1270388c4
ms.sourcegitcommit: cc9f5a16386be78c12821a975e43497a0693abba
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/18/2019
ms.locfileid: "72578175"
---
# <a name="xaml-custom-templated-controls-with-cwinrt"></a>XAML 自定义（模板化）控件与 C++/WinRT

> [!IMPORTANT]
> 有关支持你了解如何利用 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 来使用和创作运行时类的基本概述和术语，请参阅[通过 C++/WinRT 使用 API](consume-apis.md) 和[通过 C++/WinRT 创作 API](author-apis.md)。

通用 Windows 平台 (UWP) 最强大的功能之一是，用户界面 (UI) 堆栈所提供基于 XAML [控件](/uwp/api/windows.ui.xaml.controls.control)类型创建自定义控件的灵活性  。 XAML UI 框架提供[自定义依赖项属性](/windows/uwp/xaml-platform/custom-dependency-properties)[附加属性](/windows/uwp/xaml-platform/custom-attached-properties)以及[控件模板](/windows/uwp/design/controls-and-patterns/control-templates)等功能，使用这些功能可以更轻松地创建功能丰富且可自定义的控件。 本主题引导你完成使用 C++/WinRT 创建自定义（模板化）控件的步骤。

## <a name="create-a-blank-app-bglabelcontrolapp"></a>创建空白应用 (BgLabelControlApp)
首先在 Microsoft Visual Studio 中创建新项目。 创建“空白应用 (C++/WinRT)”项目，然后将其命名为 BgLabelControlApp   。 本主题的后续部分将指导你如何生成项目（在此之前暂时不要生成）。

> [!NOTE]
> 有关设置 Visual Studio 以进行 C++/WinRT 部署的信息 &mdash; 包括安装和使用 C++/WinRT Visual Studio 扩展 (VSIX) 和 NuGet 包（两者共同提供项目模板，并生成支持）的信息 &mdash; 请参阅[适用于 C++/WinRT 的 Visual Studio 支持](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)。

我们将创作新类来表示自定义的（模板化）控件。 我们要在同一编译单元内创作和使用该类。 但我们希望能够从 XAML 标记实例化此类，因此，它将成为一个运行时类。 而且我们将使用 C++/WinRT 来创作和使用它。

创作新的运行时类的第一步是将新的 Midl 文件(.idl) 项添加到项目  。 将其命名为 `BgLabelControl.idl`。 删除 `BgLabelControl.idl` 的默认内容，然后粘贴到此运行时类声明中。

```idl
// BgLabelControl.idl
namespace BgLabelControlApp
{
    runtimeclass BgLabelControl : Windows.UI.Xaml.Controls.Control
    {
        BgLabelControl();
        static Windows.UI.Xaml.DependencyProperty LabelProperty{ get; };
        String Label;
    }
}
```

上表展示在声明依赖项属性 (DP) 时采用的模式。 每个 DP 有两个部分。 首先，声明类型 [DependencyProperty](/uwp/api/windows.ui.xaml.dependencyproperty) 的只读静态属性  。 它具有 DP 的名称和属性  。 你将在实现中使用此静态属性。 其次，使用 DP 的类型和名称声明读写实例属性。 如果想要创作附加属性（而不是 DP），请参阅[自定义附加属性](/windows/uwp/xaml-platform/custom-attached-properties)中的代码示例  。

> [!NOTE]
> 如果想要创作具有浮点类型的 DP，则将其设置为 `double`（[MIDL 3.0](/uwp/midl-3/) 中的 `Double`）。 声明并实现类型 `float`（MIDL 中的 `Single`）的 DP，则在 XAML 标记中为该 DP 设置值将导致发生错误，即无法从文本“<NUMBER>”创建“Windows.Foundation.Single”  。

保存文件并生成项目。 在生成过程中，`midl.exe` 工具会运行以创建 Windows 运行时元数据文件 (`\BgLabelControlApp\Debug\BgLabelControlApp\Unmerged\BgLabelControl.winmd`) 来描述该运行时类。 然后，`cppwinrt.exe` 工具运行以生成源代码文件，从而为你在创作和使用运行时类时提供支持。 这些文件包含让你开始实现已在 IDL 中声明的 BgLabelControl 运行时类的存根  。 这些存根是 `\BgLabelControlApp\BgLabelControlApp\Generated Files\sources\BgLabelControl.h` 和 `BgLabelControl.cpp`。

将这些存根文件 `BgLabelControl.h` 和 `BgLabelControl.cpp` 从 `\BgLabelControlApp\BgLabelControlApp\Generated Files\sources\` 复制到项目文件夹中，即 `\BgLabelControlApp\BgLabelControlApp\`。 在 **“解决方案资源管理器”** 中，请确保将 **“显示所有文件”** 切换为打开。 右键单击已复制的存根文件，然后单击“包括在项目中”  。

## <a name="implement-the-bglabelcontrol-custom-control-class"></a>实现 BgLabelControl 自定义控件类 
现在，让我们打开 `\BgLabelControlApp\BgLabelControlApp\BgLabelControl.h` 和 `BgLabelControl.cpp` 并实现运行时类。 在 `BgLabelControl.h` 中，更改构造函数以设置默认样式密钥、实现 Label 和 LabelProperty，添加名为 OnLabelChanged 的静态事件处理程序以处理对依赖项属性值的更改，并添加私有成员以存储 LabelProperty 的备份字段     。

在添加这些内容后，`BgLabelControl.h` 将如下所示。

```cppwinrt
// BgLabelControl.h
...
struct BgLabelControl : BgLabelControlT<BgLabelControl>
{
    BgLabelControl() { DefaultStyleKey(winrt::box_value(L"BgLabelControlApp.BgLabelControl")); }

    winrt::hstring Label()
    {
        return winrt::unbox_value<winrt::hstring>(GetValue(m_labelProperty));
    }

    void Label(winrt::hstring const& value)
    {
        SetValue(m_labelProperty, winrt::box_value(value));
    }

    static Windows::UI::Xaml::DependencyProperty LabelProperty() { return m_labelProperty; }

    static void OnLabelChanged(Windows::UI::Xaml::DependencyObject const&, Windows::UI::Xaml::DependencyPropertyChangedEventArgs const&);

private:
    static Windows::UI::Xaml::DependencyProperty m_labelProperty;
};
...
```

在 `BgLabelControl.cpp` 中定义静态成员，如下所示。

```cppwinrt
// BgLabelControl.cpp
...
Windows::UI::Xaml::DependencyProperty BgLabelControl::m_labelProperty =
    Windows::UI::Xaml::DependencyProperty::Register(
        L"Label",
        winrt::xaml_typename<winrt::hstring>(),
        winrt::xaml_typename<BgLabelControlApp::BgLabelControl>(),
        Windows::UI::Xaml::PropertyMetadata{ winrt::box_value(L"default label"), Windows::UI::Xaml::PropertyChangedCallback{ &BgLabelControl::OnLabelChanged } }
);

void BgLabelControl::OnLabelChanged(Windows::UI::Xaml::DependencyObject const& d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs const& /* e */)
{
    if (BgLabelControlApp::BgLabelControl theControl{ d.try_as<BgLabelControlApp::BgLabelControl>() })
    {
        // Call members of the projected type via theControl.

        BgLabelControlApp::implementation::BgLabelControl* ptr{ winrt::get_self<BgLabelControlApp::implementation::BgLabelControl>(theControl) };
        // Call members of the implementation type via ptr.
    }
}
...
```

本演练中，我们不会使用 OnLabelChanged  。 但在此处将使用，以便你理解如何为属性更改回调注册依赖项属性。 OnLabelChanged 的实现还展示如何从基本投影类型获取派生投影类型（本例中，基本投影类型为 DependencyObject）   。 它展示之后如何获取指针，该指针指向实现投影类型的类型。 第二个操作自然只能在实现投影类型的项目（即实现运行时类的项目）中执行。

> [!NOTE]
> 如果你尚未安装 Windows SDK 版本 10.0.17763.0（Windows 10 版本 1809）或更高版本，则需在上述依赖项属性更改事件处理器中调用 [winrt::from_abi](/uwp/cpp-ref-for-winrt/from-abi)，而不是 [winrt::get_self](/uwp/cpp-ref-for-winrt/get-self)   。

## <a name="design-the-default-style-for-bglabelcontrol"></a>为 BgLabelControl 设计默认样式 

在其构造函数中，BgLabelControl 为自身设置默认样式密钥  。 但默认样式是什么  ？ 自定义（模板化）控件需要具有包含默认控件模板的默认样式，在控件的使用者未设置样式和/或模板时，可用于呈现。 在本节中，我们将向包含默认样式的项目添加标记文件。

在项目节点下，新建文件夹（不是筛选器，而是文件夹），并将其命名为“Themes”。 在 `Themes` 下，添加类型为“Visual C++” > “XAML” > “XAML View”的新项目，并将其命名为“Generic.xaml”    。 文件夹和文件的名称必须与如下类似，以便 XAML 框架查找自定义控件的默认样式。 删除 `Generic.xaml` 的默认内容，并粘贴到下方标记中。

```xaml
<!-- \Themes\Generic.xaml -->
<ResourceDictionary
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:BgLabelControlApp">

    <Style TargetType="local:BgLabelControl" >
        <Setter Property="Template">
            <Setter.Value>
                <ControlTemplate TargetType="local:BgLabelControl">
                    <Grid Width="100" Height="100" Background="{TemplateBinding Background}">
                        <TextBlock HorizontalAlignment="Center" VerticalAlignment="Center" Text="{TemplateBinding Label}"/>
                    </Grid>
                </ControlTemplate>
            </Setter.Value>
        </Setter>
    </Style>
</ResourceDictionary>
```

在本例中，默认样式所设置的唯一的属性是控件模板。 该模板由一个正方形（其背景绑定到 Background 属性，所有 XAML [Control](/uwp/api/windows.ui.xaml.controls.control) 类型的实例都具有该属性）和一个文本元素（其文本绑定到 BgLabelControl::Label 依赖项属性）组成    。

## <a name="add-an-instance-of-bglabelcontrol-to-the-main-ui-page"></a>将 BgLabelControl 的实例添加到主 UI 页面 

打开 `MainPage.xaml`，其中包含主 UI 页面的 XAML 标记。 紧接在 Button 元素（StackPanel 内）之后，添加以下标记   。

```xaml
<local:BgLabelControl Background="Red" Label="Hello, World!"/>
```

此外，将以下 include 指令添加到 `MainPage.h`，以便 MainPage 类型（编译 XAML 标记和命令式代码的组合）意识到 BgLabelControl 自定义控件类型   。 如果想要使用另一个 XAML 页面的 BgLabelControl，则也可以将上述 include 指令添加到该页面的头文件  。 或者，也可以直接在预编译的头文件中放入一个 include 指令。

```cppwinrt
// MainPage.h
...
#include "BgLabelControl.h"
...
```

立即生成并运行该项目。 你将看到，默认控件模板绑定到标记中 BgLabelControl 实例的背景刷和标签  。

本演练展示了 C++/WinRT 中自定义（模板化）控件的一个简单示例。 你可以任意将自己的自定义控件变得功能丰富、特色十足。 例如，自定义控件可以采用可编辑数据网格、视频播放器或 3D 几何可视化器等复杂形式。

## <a name="implementing-overridable-functions-such-as-measureoverride-and-onapplytemplate"></a>实现 MeasureOverride 和 OnApplyTemplate 等可覆盖的函数   

可以从 [Control](/uwp/api/windows.ui.xaml.controls.control) 运行时类派生自定义控件，而该运行时类本身派生自基本运行时类  。 以下是 [Control、](/uwp/api/windows.ui.xaml.frameworkelement) 和 [UIElement](/uwp/api/windows.ui.xaml.uielement) 的一些可覆盖方法，可在派生类中覆盖    。 下面的代码示例演示了如何执行该操作。

```cppwinrt
struct BgLabelControl : BgLabelControlT<BgLabelControl>
{
...
    // Control overrides.
    void OnPointerPressed(Windows::UI::Xaml::Input::PointerRoutedEventArgs const& /* e */) const { ... };

    // FrameworkElement overrides.
    Windows::Foundation::Size MeasureOverride(Windows::Foundation::Size const& /* availableSize */) const { ... };
    void OnApplyTemplate() const { ... };

    // UIElement overrides.
    Windows::UI::Xaml::Automation::Peers::AutomationPeer OnCreateAutomationPeer() const { ... };
...
};
```

可覆盖函数在不同语言投影中呈现出不同形式  。 例如，在 C# 中可覆盖函数通常呈现为受保护的虚拟函数。 在 C++/WinRT 中，它们既不是虚拟的也不是受保护的，但你仍可以覆盖它们并提供你自己的实现，如上所示。

## <a name="important-apis"></a>重要的 API
* [Control 类](/uwp/api/windows.ui.xaml.controls.control)
* [DependencyProperty 类](/uwp/api/windows.ui.xaml.dependencyproperty)
* [FrameworkElement 类](/uwp/api/windows.ui.xaml.frameworkelement)
* [UIElement 类](/uwp/api/windows.ui.xaml.uielement)

## <a name="related-topics"></a>相关主题
* [控件模板](/windows/uwp/design/controls-and-patterns/control-templates)
* [自定义依赖属性](/windows/uwp/xaml-platform/custom-dependency-properties)
