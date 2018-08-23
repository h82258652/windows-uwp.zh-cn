---
author: stevewhims
description: 本主题将引导您完成步骤创建一个简单的自定义控件，使用 C + + / WinRT。 您可以生成 info 此处创建您自己的功能的富客户端和可自定义 UI 控件上。
title: XAML 自定义 （模板） 控件与 C + + / WinRT
ms.author: stwhi
ms.date: 08/01/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10 uwp、 标准、 c + +、 cpp、 winrt、 投影，XAML，自定义的模板化控件
ms.localizationpriority: medium
ms.openlocfilehash: c108175c66d27b2cdbd910a0f7653ca1befb68e9
ms.sourcegitcommit: 9c79fdab9039ff592edf7984732d300a14e81d92
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/23/2018
ms.locfileid: "2823079"
---
# <a name="xaml-custom-templated-controls-with-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>使用 XAML 自定义 （模板） 控件[C + + / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)

一个最强大的功能的通用 Windows 平台 (UWP) 是在用户界面 (UI) 堆栈提供来创建基于 XAML[控件](/uwp/api/windows.ui.xaml.controls.control)类型的自定义控件的灵活性。 XAML UI 框架提供[自定义依赖项属性](/windows/uwp/xaml-platform/custom-dependency-properties)和附加的属性和[控件模板](/windows/uwp/design/controls-and-patterns/control-templates)，其能够轻松创建功能富客户端和可自定义控件等功能。 本主题将引导您完成步骤创建自定义 （模板） 控件具有 C + + / WinRT。

## <a name="create-a-blank-app-bglabelcontrolapp"></a>创建一个空白的应用程序 (BgLabelControlApp)
首先在 Microsoft Visual Studio 中创建新项目。 创建**Visual c + + 空白应用程序 (C + + / WinRT)** 项目，并将其命名为*BgLabelControlApp*。

我们将编写一个新的类来表示自定义 （模板） 控件。 我们正在同一编译单元内创作和使用该类。 但我们希望能够进行实例化此类从 XAML 标记和原因它将一个运行时类。 而且我们将使用 C++/WinRT 来创作和使用它。

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

上方的列表演示了在声明依赖项 (DP) 时遵循的模式。 有两个部分每个 DP。 首先，您声明类型[DependencyProperty](/uwp/api/windows.ui.xaml.dependencyproperty)的只读的静态属性。 具有您 DP 以及*属性*的名称。 您将在实现中使用此静态属性。 第二，您使用的类型和名称您 DP 声明的读 / 写实例属性。

> [!NOTE]
> 如果您希望 DP 与浮点类型，然后使其`double`(`Double` [MIDL 3.0](/uwp/midl-3/)中)。 声明和实现类型 DP `float` (`Single` MIDL 中)，然后为该 DP XAML 标记中设置值将导致错误和*未能从文本中创建 Windows.Foundation.Single'<NUMBER>*。

保存文件并生成项目。 在生成过程中，`midl.exe` 工具会运行以创建描述该运行时类的 Windows 运行时元数据文件 (`\BgLabelControlApp\Debug\BgLabelControlApp\Unmerged\BgLabelControl.winmd`)。 然后，`cppwinrt.exe` 工具运行以生成源代码文件，从而为你在创作和使用运行时类时提供支持。 这些文件包括您入门实现您 IDL 中声明的**BgLabelControl**运行时类的存根。 这些存根是 `\BgLabelControlApp\BgLabelControlApp\Generated Files\sources\BgLabelControl.h` 和 `BgLabelControl.cpp`。

将这些存根文件 `BgLabelControl.h` 和 `BgLabelControl.cpp` 从 `\BgLabelControlApp\BgLabelControlApp\Generated Files\sources\` 复制到项目文件夹中，即 `\BgLabelControlApp\BgLabelControlApp\`。 在**解决方案资源管理器**中，确保将**显示所有文件**切换为打开。 右键单击已复制的存根文件，然后单击**包括在项目中**。

## <a name="implement-the-bglabelcontrol-custom-control-class"></a>实现**BgLabelControl**自定义控件类
现在，让我们打开 `\BgLabelControlApp\BgLabelControlApp\BgLabelControl.h` 和 `BgLabelControl.cpp` 并实现运行时类。 在`BgLabelControl.h`、 更改要设置的默认样式密钥、**标签**和**LabelProperty**实现的构造函数、 添加名为**OnLabelChanged**处理对依赖项属性的值更改静态事件处理程序和添加一个私有成员若要存储**LabelProperty**支持字段。

在本演练中，我们不会使用**OnLabelChanged**。 但它存在，以便您可以了解如何注册依赖项属性的属性更改回调。

添加这些之后, 将`BgLabelControl.h`如下所示。

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

    static void OnLabelChanged(Windows::UI::Xaml::DependencyObject const& d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs const& e);

private:
    static Windows::UI::Xaml::DependencyProperty m_labelProperty;
};
...
```

在`BgLabelControl.cpp`，定义如下所示的静态成员。

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

void BgLabelControl::OnLabelChanged(Windows::UI::Xaml::DependencyObject const& d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs const& e) {}
...
```

## <a name="design-the-default-style-for-bglabelcontrol"></a>设计的默认样式**BgLabelControl** （英文）

在其构造函数， **BgLabelControl**本身的设置的默认样式键。 什么*是*，但默认样式？ 自定义 （模板） 控件需要具有默认样式&mdash;包含的默认控件模板&mdash;其可用于呈现本身与控件的使用者不将样式和/或模板的情况下。 本节中，我们将包含我们默认样式的项目添加标记文件。

在您的项目节点下创建一个新文件夹并将其命名为"主题"。 在`Themes`，添加新项的类型**Visual c + +** > **XAML** > **XAML 视图**，并将其命名为"Generic.xaml"。 文件夹和文件名称必须是如下所示顺序的 XAML 框架，用来查找自定义控件的默认样式。 删除的默认内容`Generic.xaml`，并将其粘贴在下面的标记。

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

在这种情况下，仅属性的默认样式设置为的控件模板。 模板包含一个方形 （其背景绑定到 XAML[控件](/uwp/api/windows.ui.xaml.controls.control)类型的所有实例都具有**Background**属性），以及文本元素 （其文本绑定到的**BgLabelControl::Label**依赖项）。

## <a name="add-an-instance-of-bglabelcontrol-to-the-main-ui-page"></a>**BgLabelControl**的实例添加到 UI 主页

打开 `MainPage.xaml`，其中包含主 UI 页面的 XAML 标记。 **Button**元素 （内**StackPanel**) 后立即, 添加以下标记。

```xaml
<local:BgLabelControl Background="Red" Label="Hello, World!"/>
```

此外，添加以下包括到指令`MainPage.h`，以便**MainPage**类型 （编译 XAML 标记和命令性代码的组合） 已知道**BgLabelControl**自定义控件类型。

```cppwinrt
// MainPage.h
...
#include "BgLabelControl.h"
...
```

立即生成并运行该项目。 您将看到的默认控件模板绑定到背景画笔，和标记中的**BgLabelControl**实例的标签。

本演练演示了一个简单的自定义 （模板） 控件示例，在 C + + / WinRT。 任意丰富而全功能，您可以进行自定义控件。 例如，自定义控件可能需要为可编辑数据网格、 视频播放器或 3D 几何的可视化工具某些内容复杂的窗体。

## <a name="important-apis"></a>重要的 API
* [控件](/uwp/api/windows.ui.xaml.controls.control)
* [DependencyProperty](/uwp/api/windows.ui.xaml.dependencyproperty)

## <a name="related-topics"></a>相关主题
* [控件模板](/windows/uwp/design/controls-and-patterns/control-templates)
* [自定义依赖属性](/windows/uwp/xaml-platform/custom-dependency-properties)
