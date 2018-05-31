---
author: jwmsft
title: 条件 XAML
description: 在保持与以前版本的兼容性的同时在 XAML 标记中使用新 API
ms.author: jimwalk
ms.date: 10/10/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10，uwp
ms.localizationpriority: medium
ms.openlocfilehash: bb295bfb203a09d64491563bec7ba01cd31b4227
ms.sourcegitcommit: 6618517dc0a4e4100af06e6d27fac133d317e545
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
ms.locfileid: "1689593"
---
# <a name="conditional-xaml"></a>条件 XAML

*条件 XAML* 提供在 XAML 标记中使用 [ApiInformation.IsApiContractPresent](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation.isapicontractpresent) 方法的一种途径。 你可以基于 API 的存在在标记中设置属性和实例化对象，无需使用背后的代码。 它选择性地分析元素或属性来确定在运行时是否可用。 条件语句在运行时进行评估，如果评估为 **true**，则对使用条件 XAML 标记进行限定的元素进行分析；否则忽略。

条件 XAML 从创意者更新（版本 1703，内部版本 15063）开始提供。 若要使用条件 XAML，Visual Studio 项目的最低版本必须设置为内部版本 15063（创意者更新）或更高版本，且目标版本设置为比最低版本更高的版本。 请参阅[版本自适应应用](version-adaptive-apps.md)以获取有关配置 Visual Studio 项目的详细信息。

> [!NOTE]
> 若要使用低于内部版本 15063 的最低版本创建版本自适应应用，则必须使用[版本自适应代码](version-adaptive-code.md)，而不是 XAML。

有关 ApiInformation 和 API 协定的重要背景信息，请参阅[版本自适应应用](version-adaptive-apps.md)。

## <a name="conditional-namespaces"></a>条件命名空间

若要在 XAML 中使用条件方法，首先必须在页面顶部声明条件 [XAML 命名空间](../xaml-platform/xaml-namespaces-and-namespace-mapping.md)。 下面介绍条件命名空间的仿真代码示例：

```xaml
xmlns:myNamespace="schema?conditionalMethod(parameter)"
```

条件命名空间可以分为两部分，用“?”分隔符分隔。 

- 分隔符前面的内容指示命名空间或架构含有被引用的 API。 
- 分隔符“?”后的内容表示条件方法，该方法确定是否将条件命名空间评估为 **true** 或 **false**。

在大多数情况下，架构将是默认的 XAML 命名空间：

```xaml
xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
```

条件 XAML 支持以下条件方法：

方法 | 反转
------ | -------
IsApiContractPresent(ContractName, VersionNumber) | IsApiContractNotPresent(ContractName, VersionNumber)
IsTypePresent(ControlType) | IsTypeNotPresent(ControlType)
IsPropertyPresent(ControlType, PropertyName) | IsPropertyNotPresent(ControlType, PropertyName)

我们将在本文后面的部分中进一步讨论这些方法。

> [!NOTE]
> 我们建议你使用 IsApiContractPresent 和 IsApiContractNotPresent。 其他条件在 Visual Studio 设计体验中不完全受支持。

## <a name="create-a-namespace-and-set-a-property"></a>创建命名空间并设置一个属性

在此示例中，如果在 Fall Creators Update 或更高版本上运行应用，将显示“Hello, Conditional XAML”作为文本块的内容，如果在以前的版本上运行，则默认显示无内容。

首先，使用前缀“contract5Present”定义自定义命名空间并使用默认 XAML 命名空间 (http://schemas.microsoft.com/winfx/2006/xaml/presentation) 作为含有 [TextBlock.Text](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.Text) 属性的架构。 若要使其成为一个条件命名空间，添加“?” 架构后的分隔符。

然后定义在运行 Fall Creators Update 或更高版本的设备上返回 **true** 的条件。 使用 ApiInformation 方法 **IsApiContractPresent** 来检查 UniversalApiContract 的第 5 个版本。 UniversalApiContract 版本 5 是和 Fall Creators Update (SDK 16299) 一起发布的。

```xaml
xmlns:contract5Present="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractPresent(Windows.Foundation.UniversalApiContract,5)"
```

定义命名空间后，将命名空间前缀添加到 TextBox 的 Text 属性的前面，将它限定为应该在运行时进行条件性设置的属性。

```xaml
<TextBlock contract5Present:Text="Hello, Conditional XAML"/>
```

下面是完整的 XAML。

```xaml
<Page
    x:Class="ConditionalTest.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:contract5Present="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractPresent(Windows.Foundation.UniversalApiContract,5)">

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <TextBlock contract5Present:Text="Hello, Conditional XAML"/>
    </Grid>
</Page>
```

在 Fall Creators Update 上运行此示例时，显示文本“Hello, Conditional XAML”；在 Creators Update 上运行时，不显示文本。

条件 XAML 可以让你改为在标记中执行可针对代码执行的 API 检查。 下面介绍此检查的等效代码。

```csharp
if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 5))
{
    textBlock.Text = "Hello, Conditional XAML";
}
```

请注意，即使 IsApiContractPresent 方法带有一个表示 *contractName* 参数的字符串，也不应将它放在 XAML 命名空间声明的引号 (" ") 中。

## <a name="use-ifelse-conditions"></a>使用 if/else 条件

在上面的示例中，仅当在 Fall Creators Update 上运行应用时设置 Text 属性。 但如果在不同文本在创意者更新上运行时想要显示这些文本，应该怎么做？ 你可以尝试不使用条件限定符设置 Text 属性，就像这样。

```xaml
<!-- DO NOT USE -->
<TextBlock Text="Hello, World" contract5Present:Text="Hello, Conditional XAML"/>
```

当它在 Creators Update 上运行时会有效，但当在 Fall Creators Update 上运行时，你会收到错误消息，指出多次设置 Text 属性。

若要设置在不同版本的 Windows 10 上运行应用时的不同文本，你需要另一个条件。 条件 XAML 提供每种受支持的 ApiInformation 方法的反转，让你创建像这样的 if/else 条件方案。

如果当前设备包含指定合同和版本号，IsApiContractPresent 方法返回 **true**。 例如，假设在创意者更新上运行应用，它具有通用 API 协定第 4 版。

对 IsApiContractPresent 的不同调用将产生以下结果：

- IsApiContractPresent(Windows.Foundation.UniversalApiContract, 5) = **false**
- IsApiContractPresent(Windows.Foundation.UniversalApiContract, 4) = true
- IsApiContractPresent(Windows.Foundation.UniversalApiContract, 3) = true
- IsApiContractPresent(Windows.Foundation.UniversalApiContract, 2) = true
- IsApiContractPresent(Windows.Foundation.UniversalApiContract, 1) = true。

IsApiContractNotPresent 返回 IsApiContractPresent 的反转。 调用 IsApiContractNotPresent 将产生以下结果：

- IsApiContractNotPresent(Windows.Foundation.UniversalApiContract, 5) = **true**
- IsApiContractNotPresent(Windows.Foundation.UniversalApiContract, 4) = false
- IsApiContractNotPresent(Windows.Foundation.UniversalApiContract, 3) = false
- IsApiContractNotPresent(Windows.Foundation.UniversalApiContract, 2) = false
- IsApiContractNotPresent(Windows.Foundation.UniversalApiContract, 1) = false

若要使用反转条件，你可以创建第二个使用 **IsApiContractNotPresent** 条件的条件 XAML 命名空间。 在这里，其中包含前缀“contract5NotPresent”。

```xaml
xmlns:contract5NotPresent="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractNotPresent(Windows.Foundation.UniversalApiContract,5)"

xmlns:contract5Present="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractPresent(Windows.Foundation.UniversalApiContract,5)"
```

定义两个命名空间后，可以将 Text 属性设置两次，前提是在它们前面添加限定符，确保在运行时仅使用一个属性设置，如：

```xaml
<TextBlock contract5NotPresent:Text="Hello, World"
           contract5Present:Text="Hello, Fall Creators Update"/>
```

下面介绍了设置按钮背景的另一个示例。 [亚克力材料](../design/style/acrylic.md) 功能从 Fall Creators Update 开始提供，因此当在 Fall Creators Update 上运行应用时，你将为背景使用亚克力。 它在更早的版本中不可用，因此在这些情况下，要将背景设置为红色。

```xaml
<Button Content="Button"
        contract5NotPresent:Background="Red"
        contract5Present:Background="{ThemeResource SystemControlAcrylicElementBrush}"/>
```

## <a name="create-controls-and-bind-properties"></a>创建控件和绑定属性

到目前为止，你已了解如何使用条件 XAML 设置属性，但你也可基于在运行时可用的 API 协定条件性地实例化控件。

在这里，当在控件可用的 Fall Creators Update 上运行应用时，对 [ColorPicker](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.colorpicker) 进行实例化。 ColorPicker 在 Fall Creators Update 之前不可用，因此在较早的版本上运行应用时，使用 [ComboBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.combobox) 为用户提供简化的颜色选项。

```xaml
<contract5Present:ColorPicker x:Name="colorPicker"
                              Grid.Column="1"
                              VerticalAlignment="Center"/>

<contract5NotPresent:ComboBox x:Name="colorComboBox"
                              PlaceholderText="Pick a color"
                              Grid.Column="1"
                              VerticalAlignment="Center">
```

你可以使用具有不同形式的 [XAML 属性语法](../xaml-platform/xaml-syntax-guide.md) 的条件限定符。 在这里，使用用于 Fall Creators Update 的属性元素句法和用于较早版本的属性句法设置矩形的 Fill 属性。

```xaml
<Rectangle x:Name="colorRectangle" Width="200" Height="200"
           contract5NotPresent:Fill="{x:Bind ((SolidColorBrush)((FrameworkElement)colorComboBox.SelectedItem).Tag), Mode=OneWay}">
    <contract5Present:Rectangle.Fill>
        <SolidColorBrush contract5Present:Color="{x:Bind colorPicker.Color, Mode=OneWay}"/>
    </contract5Present:Rectangle.Fill>
</Rectangle>
```

将属性绑定到取决于条件命名空间的另一个属性时，你必须对这两个属性使用相同的条件。 在这里，`colorPicker.Color` 取决于“contract5Present”条件命名空间，因此你必须将“contract5Present”前缀放在 SolidColorBrush.Color 属性的前面。 （或者将“contract5Present”前缀放在 SolidColorBrush 的前面，而不是放在 Color 属性的前面。）否则，你将收到编译时间错误。

```xaml
<SolidColorBrush contract5Present:Color="{x:Bind colorPicker.Color, Mode=OneWay}"/>
```

下面是演示这些方案的完整 XAML。 此示例中包含一个矩形以及一个让你设置矩形颜色的 UI。

在 Fall Creators Update 上运行应用时，使用 ColorPicker 让用户设置颜色。 ColorPicker 在 Fall Creators Update 之前不可用，因此在较早的版本上运行应用时，使用组合框为用户提供简化的颜色选项。

```xaml
<Page
    x:Class="ConditionalTest.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:contract5Present="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractPresent(Windows.Foundation.UniversalApiContract,5)"
    xmlns:contract5NotPresent="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractNotPresent(Windows.Foundation.UniversalApiContract,5)">

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.ColumnDefinitions>
            <ColumnDefinition/>
            <ColumnDefinition/>
        </Grid.ColumnDefinitions>

        <Rectangle x:Name="colorRectangle" Width="200" Height="200"
                   contract5NotPresent:Fill="{x:Bind ((SolidColorBrush)((FrameworkElement)colorComboBox.SelectedItem).Tag), Mode=OneWay}">
            <contract5Present:Rectangle.Fill>
                <SolidColorBrush contract5Present:Color="{x:Bind colorPicker.Color, Mode=OneWay}"/>
            </contract5Present:Rectangle.Fill>
        </Rectangle>

        <contract5Present:ColorPicker x:Name="colorPicker"
                                      Grid.Column="1"
                                      VerticalAlignment="Center"/>

        <contract5NotPresent:ComboBox x:Name="colorComboBox"
                                      PlaceholderText="Pick a color"
                                      Grid.Column="1"
                                      VerticalAlignment="Center">
            <ComboBoxItem>Red
                <ComboBoxItem.Tag>
                    <SolidColorBrush Color="Red"/>
                </ComboBoxItem.Tag>
            </ComboBoxItem>
            <ComboBoxItem>Blue
                <ComboBoxItem.Tag>
                    <SolidColorBrush Color="Blue"/>
                </ComboBoxItem.Tag>
            </ComboBoxItem>
            <ComboBoxItem>Green
                <ComboBoxItem.Tag>
                    <SolidColorBrush Color="Green"/>
                </ComboBoxItem.Tag>
            </ComboBoxItem>
        </contract5NotPresent:ComboBox>
    </Grid>
</Page>
```

## <a name="related-articles"></a>相关文章

- [UWP 应用指南](https://msdn.microsoft.com/windows/uwp/get-started/universal-application-platform-guide)
- [使用 API 协定动态检测功能](https://blogs.windows.com/buildingapps/2015/09/15/dynamically-detecting-features-with-api-contracts-10-by-10/)
- [API 协定](https://channel9.msdn.com/Events/Build/2015/3-733)（版本 2015 视频）