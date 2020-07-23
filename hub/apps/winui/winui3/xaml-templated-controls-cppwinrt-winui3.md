---
description: 本文介绍如何使用 C++/WinRT 创建适用于 WinUI 3 的 XAML 模板化控件。
title: 使用 C++/WinRT 的 UWP 和 WinUI 3 应用模板化 XAML 控件
ms.date: 07/09/2020
ms.topic: article
keywords: windows 10, uwp, 自定义控件, 模板化控件, winui, C++/WinRT
ms.author: drewbat
author: drewbatgit
ms.localizationpriority: high
ms.custom: 19H1
ms.openlocfilehash: 7e2d69390ac9a0f564c99a94151dd967759477a3
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493897"
---
# <a name="templated-xaml-controls-for-uwp-and-winui-3-apps-with-cwinrt"></a>使用 C++/WinRT 的 UWP 和 WinUI 3 应用模板化 XAML 控件

本文介绍如何使用 C++/WinRT 创建适用于 WinUI 3 的模板化 XAML 控件。 模板化控件继承自 Microsoft.UI.Xaml.Controls.Control，并且具有可使用 XAML 控件模板自定义的可视结构和可视行为。 本文介绍的场景与[使用 C++/WinRT 的 XAML 自定义（模板）控件](/windows/uwp/cpp-and-winrt-apis/xaml-cust-ctrl)一文中所介绍的相同，只是已改编为使用 WinUI 3。

在执行本文的操作步骤之前，你应确保已配置开发环境以创建 WinUI 3 应用。 有关设置信息，请参阅[适用于桌面应用的 WinUI 3 入门](/windows/apps/winui/winui3/get-started-winui3-for-desktop)。

## <a name="create-a-blank-app-bglabelcontrolapp"></a>创建空白应用 (BgLabelControlApp)

首先在 Microsoft Visual Studio 中创建新项目。 创建一个“空白应用(UWP 版 WinUI)”项目，将其名称设置为 BgLabelControlApp。 将“目标版本”设置为 Windows 10 版本 1903（内部版本 18362），并将“最低版本”设置为 Windows 10 版本 1803（内部版本 17134） 。 本演练还适用于使用“打包的空白应用(桌面版 WinUI)”项目模板创建的桌面应用，只需确保执行“BgLabelControlApp (桌面)”项目中的所有步骤即可 。

![空白应用项目模板](images/WinUI-cpp-newproject-UWP.png)

由于该类将从 XAML 标记进行实例化，因此它将成为一个运行时类。 要创作新的运行时类，第一步是将新的 Midl 文件(.idl) 项添加到项目。 从“项目”菜单中，选择“添加新项…”，然后在搜索框中键入“MIDL”以查找 .idl 文件项 。 将新文件命名为 `BgLabelControl.idl`，以便该名称与本文中的步骤一致。 删除 `BgLabelControl.idl` 的默认内容，然后粘贴到此运行时类声明中。

```cppwinrt
// BgLabelControl.idl
namespace BgLabelControlApp
{
    runtimeclass BgLabelControl : Microsoft.UI.Xaml.Controls.Control
    {
        BgLabelControl();
        static Microsoft.UI.Xaml.DependencyProperty LabelProperty{ get; };
        String Label;
    }
}
```

上表展示在声明依赖项属性 (DP) 时采用的模式。 每个 DP 有两个部分。 首先，声明类型为 DependencyProperty 的只读静态属性。 它的名称是你的 DP + 属性。 你将在实现中使用此静态属性。 其次，使用 DP 的类型和名称声明读写实例属性。 如果想要创作附加属性（而不是 DP），请参阅[自定义附加属性](/windows/uwp/xaml-platform/custom-attached-properties)中的代码示例。

请注意，以上代码中引用的 XAML 类位于 Microsoft.UI.Xaml 命名空间中。 这就是它们作为 WinUI 控件与在 Windows.UI.XAML 命名空间中定义的 UWP·XAML 控件的区别。

保存新的 .idl 文件后，下一步是为用于实现模板化控件的 .cpp 和 .h 实现文件生成 Windows 运行时元数据文件 (.winmd) 和存根。 通过生成解决方案来生成这些文件，这将使 MIDL 编译器 (midl.exe) 编译创建的 .idl 文件。 请注意，该解决方案将无法成功生成，并且 Visual Studio 将在输出窗口中显示生成错误，但是必要文件可生成。

将 \BgLabelControlApp\BgLabelControlApp\Generated Files\sources\ 中的存根文件 BgLabelControl.h 和 BgLabelControl.cpp 复制到项目文件夹中。 在“解决方案资源管理器”中，请确保将“显示所有文件”切换为打开。 右键单击已复制的存根文件，然后单击“包括在项目中”。

编译器在 BgLabelControl.h 和 BgLabelControl.cpp 的顶部放置 static_assert 行，以防编译生成的文件。 实施控件时，你应从放置在项目目录中的文件中删除这些行。 对于本演练，只需使用下面提供的代码覆盖文件的全部内容即可。

## <a name="implement-the-bglabelcontrol-custom-control-class"></a>实现 BgLabelControl 自定义控件类

在下面的步骤中，更新项目目录中的 BgLabelControl.h 和 BgLabelControl.cpp 文件中的代码以实现运行时类。 

用以下代码替换 BgLabelControl.h 的内容。

```cppwinrt
// BgLabelControl.h
#pragma once
#include "BgLabelControl.g.h"

namespace winrt::BgLabelControlApp::implementation
{
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

        static Microsoft::UI::Xaml::DependencyProperty LabelProperty() { return m_labelProperty; }

        static void OnLabelChanged(Microsoft::UI::Xaml::DependencyObject const&, Microsoft::UI::Xaml::DependencyPropertyChangedEventArgs const&);

    private:
        static Microsoft::UI::Xaml::DependencyProperty m_labelProperty;
    };
}
namespace winrt::BgLabelControlApp::factory_implementation
{
    struct BgLabelControl : BgLabelControlT<BgLabelControl, implementation::BgLabelControl>
    {
    };
}
```

上面显示的代码实现了 Label 和 LabelProperty 属性，添加了一个名为 OnLabelChanged 的静态事件处理程序以处理对依赖项属性值的更改，并添加了一个私有成员以存储 LabelProperty 的备份字段   。

同样要注意的是，头文件中引用的 XAML 类位于属 WinUI 3 框架的 Microsoft.UI.Xaml 命名空间中，而不是 UWP UI 框架使用的 Windows.UI.Xaml 命名空间。


接下来，用以下代码替换 BgLabelControl.cpp 的内容。

```cppwinrt
// BgLabelControl.cpp
#include "pch.h"
#include "BgLabelControl.h"
#include "BgLabelControl.g.cpp"

namespace winrt::BgLabelControlApp::implementation
{
    Microsoft::UI::Xaml::DependencyProperty BgLabelControl::m_labelProperty =
        Microsoft::UI::Xaml::DependencyProperty::Register(
            L"Label",
            winrt::xaml_typename<winrt::hstring>(),
            winrt::xaml_typename<BgLabelControlApp::BgLabelControl>(),
            Microsoft::UI::Xaml::PropertyMetadata{ winrt::box_value(L"default label"), Microsoft::UI::Xaml::PropertyChangedCallback{ &BgLabelControl::OnLabelChanged } }
    );

    void BgLabelControl::OnLabelChanged(Microsoft::UI::Xaml::DependencyObject const& d, Microsoft::UI::Xaml::DependencyPropertyChangedEventArgs const& /* e */)
    {
        if (BgLabelControlApp::BgLabelControl theControl{ d.try_as<BgLabelControlApp::BgLabelControl>() })
        {
            // Call members of the projected type via theControl.

            BgLabelControlApp::implementation::BgLabelControl* ptr{ winrt::get_self<BgLabelControlApp::implementation::BgLabelControl>(theControl) };
            // Call members of the implementation type via ptr.
        }
    }
}
```
本演练不会使用 OnLabelChanged 回调，但提供它是为了让你可了解如何使用属性已更改的回调注册依赖项属性。 OnLabelChanged 的实现还展示了如何从基本投影类型（本例中，基本投影类型为 DependencyObject）中获取派生投影类型。 它展示之后如何获取指针，该指针指向实现投影类型的类型。 第二个操作自然只能在实现投影类型的项目（即实现运行时类的项目）中执行。

[xaml_typename](/uwp/cpp-ref-for-winrt/xaml-typename) 函数由 Windows.UI.Xaml.Interop 命名空间提供，WinUI 3 项目模板默认不包含该命名空间。 添加一行到项目 `pch.h` 的预编译头文件中，以包含与此命名空间关联的头文件。

```cppwinrt
// pch.h
...
#include <winrt/Windows.UI.Xaml.Interop.h>
...
```



## <a name="design-the-default-style-for-bglabelcontrol"></a>为 BgLabelControl 设计默认样式

在其构造函数中，BgLabelControl 为自身设置默认样式密钥。 模板化控件需要具有包含默认控件模板的默认样式，在控件的使用者未设置样式和/或模板的情况下时，可使用它进行呈现。 在本节中，我们将向包含默认样式的项目添加标记文件。

确保“显示所有文件”仍处于打开状态（在“解决方案资源管理器”中）。  在项目节点下，新建文件夹（不是筛选器，而是文件夹），并将其命名为“Themes”。 在 `Themes` 下，添加类型为“Visual C++”>“XAML”>“XAML 视图”的新项，并将其命名为“Generic.xaml”。 文件夹和文件的名称必须与此类似，以便 XAML 框架查找模块化控件的默认样式。 删除 Generic.xaml 的默认内容，然后粘贴下方的标记。

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

在本例中，默认样式所设置的唯一的属性是控件模板。 该模板由一个正方形（其背景绑定到 Background 属性，所有 XAML Control 类型的实例都具有该属性）和一个文本元素（其文本绑定到 BgLabelControl::Label 依赖项属性）组成  。

## <a name="add-an-instance-of-bglabelcontrol-to-the-main-ui-page"></a>将 BgLabelControl 的实例添加到主 UI 页面

打开 `MainPage.xaml`，其中包含主 UI 页面的 XAML 标记。 紧接在 Button 元素（StackPanel 内）之后，添加以下标记 。

```xaml
<local:BgLabelControl Background="Red" Label="Hello, World!"/>
```

此外，将以下 include 指令添加到 `MainPage.h`，以便 MainPage 类型（编译 XAML 标记和命令式代码的组合）意识到 BgLabelControl 模板控件类型 。 如果想要使用另一个 XAML 页面的 BgLabelControl，则也可以将上述 include 指令添加到该页面的头文件。 或者，也可以直接在预编译的头文件中放入一个 include 指令。

```cppwinrt
//MainPage.h
...
#include "BgLabelControl.h"
...
```

立即生成并运行该项目。 你将看到，默认控件模板绑定到标记中 BgLabelControl 实例的背景刷和标签。


## <a name="implementing-overridable-functions-such-as-measureoverride-and-onapplytemplate"></a>实现 MeasureOverride 和 OnApplyTemplate 等可替代的函数 

可以从 Control 运行时类派生模板化控件，而该运行时类本身派生自基本运行时类。 以下是一些可在派生类中替代的 Control、FrameworkElement 和 UIElement 的可替代方法  。 下面的代码示例演示了如何执行该操作。

```cppwinrt
// Control overrides.
void OnPointerPressed(Microsoft::UI::Xaml::Input::PointerRoutedEventArgs const& /* e */) const { ... };

// FrameworkElement overrides.
Windows::Foundation::Size MeasureOverride(Windows::Foundation::Size const& /* availableSize */) const { ... };
void OnApplyTemplate() const { ... };

// UIElement overrides.
Microsoft::UI::Xaml::Automation::Peers::AutomationPeer OnCreateAutomationPeer() const { ... };
```

可覆盖函数在不同语言投影中呈现出不同形式。 例如，在 C# 中可覆盖函数通常呈现为受保护的虚拟函数。 在 C++/WinRT 中，它们既不是虚拟的也不是受保护的，但你仍可以覆盖它们并提供你自己的实现，如上所示。