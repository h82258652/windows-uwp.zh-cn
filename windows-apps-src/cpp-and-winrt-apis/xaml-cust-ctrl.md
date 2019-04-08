---
description: 本主题引导你完成使用 C++/WinRT 创建简单的自定义控件的步骤。 你可以基于此处的信息创建你自己的功能丰富且可自定义的 UI 控件。
title: XAML 自定义（模板化）控件与 C++/WinRT
ms.date: 10/03/2018
ms.topic: article
keywords: windows 10、 uwp、 标准版、 c + +、 cpp、 winrt、 投影、 XAML，自定义的模板化控件
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: ce4f7eea074233c625a2cc92ef773f0b06c2be9f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57635142"
---
# <a name="xaml-custom-templated-controls-with-cwinrt"></a>XAML 自定义（模板化）控件与 C++/WinRT

> [!IMPORTANT]
> 为帮助您了解如何使用和创作与运行时类的基本概念和术语[C + + WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)，请参阅[使用的 Api 使用 C + + WinRT](consume-apis.md)和[作者 Api 使用 C + + /cliWinRT](author-apis.md)。

通用 Windows 平台 (UWP) 的功能最强大功能之一是能够灵活的用户界面 (UI) 堆栈提供的用于创建基于 XAML 的自定义控件[**控制**](/uwp/api/windows.ui.xaml.controls.control)类型。 XAML UI 框架提供的功能，例如[自定义依赖属性](/windows/uwp/xaml-platform/custom-dependency-properties)并[附加属性](/windows/uwp/xaml-platform/custom-attached-properties)，并[控件模板](/windows/uwp/design/controls-and-patterns/control-templates)，这使它可以轻松创建功能丰富和可自定义控件。 本主题将指导你逐步完成创建自定义 （模板化） 控件和 C + + WinRT。

## <a name="create-a-blank-app-bglabelcontrolapp"></a>创建一个空白应用 (BgLabelControlApp)
首先在 Microsoft Visual Studio 中创建新项目。 创建**Visual c + +** > **Windows Universal** > **空白应用 (C + + WinRT)** 项目，并将其命名*BgLabelControlApp*. 本主题的后面部分，将会被定向以生成项目 （不生成在此之前）。

我们将创建一个新类来表示自定义 （模板化） 控件。 我们正在同一编译单元内创作和使用该类。 但我们想要能够实例化此类从 XAML 标记，以及原因就运行时类。 而且我们将使用 C++/WinRT 来创作和使用它。

创作新的运行时类的第一步是将新的 **Midl 文件(.idl)** 项添加到项目。 将其命名为 `BgLabelControl.idl`。 删除 `BgLabelControl.idl` 的默认内容，然后粘贴到此运行时类声明中。

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

上面的列表显示时声明依赖项属性 (DP) 遵循的模式。 有两个组成部分每个分发点。 首先，声明类型的只读静态属性[ **DependencyProperty**](/uwp/api/windows.ui.xaml.dependencyproperty)。 它具有名称加上你 DP*属性*。 在实现中，将使用此静态属性。 第二，你将读写实例属性声明具有类型和在分发点的名称。 如果你想要创作*附加属性*（而不是 DP)，然后请参阅中的代码示例[自定义附加属性](/windows/uwp/xaml-platform/custom-attached-properties)。

> [!NOTE]
> 如果你想使用浮点类型的分发点，然后使其成为`double`(`Double`中[MIDL 3.0](/uwp/midl-3/))。 声明和实现类型的 DP `float` (`Single` MIDL 中)，并且将值设置为在 XAML 标记中，该分发点会导致错误*未能创建 Windows.Foundation.Single 从文本<NUMBER>'*.

保存文件并生成项目。 在生成过程中，`midl.exe` 工具会运行以创建描述该运行时类的 Windows 运行时元数据文件 (`\BgLabelControlApp\Debug\BgLabelControlApp\Unmerged\BgLabelControl.winmd`)。 然后，`cppwinrt.exe` 工具运行以生成源代码文件，从而为你在创作和使用运行时类时提供支持。 这些文件包括存根 （stub） 若要开始实现**BgLabelControl**在 IDL 中声明的运行时类。 这些存根是 `\BgLabelControlApp\BgLabelControlApp\Generated Files\sources\BgLabelControl.h` 和 `BgLabelControl.cpp`。

将这些存根文件 `BgLabelControl.h` 和 `BgLabelControl.cpp` 从 `\BgLabelControlApp\BgLabelControlApp\Generated Files\sources\` 复制到项目文件夹中，即 `\BgLabelControlApp\BgLabelControlApp\`。 在 **“解决方案资源管理器”** 中，请确保将 **“显示所有文件”** 切换为打开。 右键单击已复制的存根文件，然后单击**包括在项目中**。

## <a name="implement-the-bglabelcontrol-custom-control-class"></a>实现**BgLabelControl**自定义控件类
现在，让我们打开 `\BgLabelControlApp\BgLabelControlApp\BgLabelControl.h` 和 `BgLabelControl.cpp` 并实现运行时类。 在`BgLabelControl.h`，更改构造函数来设置的默认样式项，请实现**标签**并**LabelProperty**，添加名为静态事件处理程序**OnLabelChanged**到处理对该依赖属性的值的更改并将添加私有成员来存储的支持字段**LabelProperty**。

在添加之后, 你`BgLabelControl.h`如下所示。

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

在`BgLabelControl.cpp`，定义此类的静态成员。

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

在本演练中，我们不会使用**OnLabelChanged**。 但它的存在，以便您可以看到如何使用属性更改回调注册依赖属性。 实现**OnLabelChanged**还演示了如何获取基本的投影类型派生的投影的类型 (基的投影的类型是**DependencyObject**，在这种情况下)。 它演示了如何然后获取实现提取的类型的类型的指针。 该第二个操作将自然地仅有可能实现的投影的类型 （即，实现运行时类的项目） 的项目中。

> [!NOTE]
> 如果你尚未安装 Windows SDK 版本 10.0.17763.0 (Windows 10，版本 1809年) 或更高版本，然后您需要调用[ **winrt::from_abi** ](/uwp/cpp-ref-for-winrt/from-abi)中更高版本的依赖关系属性已更改的事件处理程序而不是[ **winrt::get_self**](/uwp/cpp-ref-for-winrt/get-self)。

## <a name="design-the-default-style-for-bglabelcontrol"></a>设计的默认样式为**BgLabelControl**

其构造函数中**BgLabelControl**为自身设置默认样式键。 但什么*是*默认样式？ 自定义 （模板化） 控件需要具有默认样式&mdash;包含默认控件模板&mdash;它可以用于在控件的使用者不会将样式和/或模板的情况下呈现自身使用。 在本部分中我们要将标记文件添加到包含我们默认样式的项目。

在你的项目节点，创建一个新文件夹并将其命名为"主题"。 下`Themes`，添加新项的类型**Visual c + +** > **XAML** > **XAML 视图**，并将其命名为"Generic.xaml"。 文件夹和文件名称必须位于如下 XAML 框架以查找自定义控件的默认样式。 删除的默认内容`Generic.xaml`，并粘贴下面的标记。

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

在这种情况下，唯一的默认样式设置的属性是控件模板。 模板中包含的正方形 (其背景绑定到**背景**属性的所有实例的 XAML [**控制**](/uwp/api/windows.ui.xaml.controls.control)类型具有)，和一个文本元素 （其绑定到文本框**BgLabelControl::Label**依赖项属性)。

## <a name="add-an-instance-of-bglabelcontrol-to-the-main-ui-page"></a>添加的实例**BgLabelControl**到 UI 的主页

打开 `MainPage.xaml`，其中包含主 UI 页面的 XAML 标记。 后立即**按钮**元素 (内**StackPanel**)，添加以下标记。

```xaml
<local:BgLabelControl Background="Red" Label="Hello, World!"/>
```

此外，添加以下包含指令`MainPage.h`，以便**MainPage**类型 （编译 XAML 标记和命令性代码的组合） 都不知道**BgLabelControl**自定义控件类型。 如果你想要使用**BgLabelControl**从另一个 XAML 页面，请添加这个相同太 include 指令的标头文件的该页面。 或者，或者，只需将单个指令文件中包含预编译标头。

```cppwinrt
// MainPage.h
...
#include "BgLabelControl.h"
...
```

立即生成并运行该项目。 你将看到的默认控件模板绑定到背景画笔和标签的**BgLabelControl**标记中的实例。

本演练演示了一个简单的自定义 （模板化） 控件示例，在 C + + WinRT。 任意丰富、 功能完备，您可以将您自己自定义控件。 例如，自定义控件可能需要内容为可编辑数据网格、 视频播放器或三维几何图形的可视化工具一样复杂的形式。

## <a name="implementing-overridable-functions-such-as-measureoverride-and-onapplytemplate"></a>实现*可重写*函数，如**MeasureOverride**和**OnApplyTemplate**

派生自定义控件从[**控制**](/uwp/api/windows.ui.xaml.controls.control)运行时类，该类本身进一步派生自基本运行时类。 有的可重写方法和**控制**， [ **FrameworkElement**](/uwp/api/windows.ui.xaml.frameworkelement)，以及[ **UIElement** ](/uwp/api/windows.ui.xaml.uielement)你可以在派生类中重写。 下面是代码示例，演示如何执行该操作。

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

*可重写*函数本身以不同方式提供不同的语言投影中。 在C#，例如，可重写函数通常显示为受保护的虚拟函数。 在 C + + / WinRT，它们既不虚拟，也不受保护，但仍可以重写它们，并提供您自己的实现，如上所示。

## <a name="important-apis"></a>重要的 API
* [控件类](/uwp/api/windows.ui.xaml.controls.control)
* [DependencyProperty 类](/uwp/api/windows.ui.xaml.dependencyproperty)
* [FrameworkElement 类](/uwp/api/windows.ui.xaml.frameworkelement)
* [UIElement 类](/uwp/api/windows.ui.xaml.uielement)

## <a name="related-topics"></a>相关主题
* [控件模板](/windows/uwp/design/controls-and-patterns/control-templates)
* [自定义依赖属性](/windows/uwp/xaml-platform/custom-dependency-properties)
