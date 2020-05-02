---
Description: Numberbox 是一个控件，可以用来显示和编辑数字。
title: 数字框
template: detail.hbs
ms.date: 11/27/2019
ms.topic: article
keywords: windows 10, uwp
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: d0e4a290da0d67560715f80bf20fc6ae4d44a8f6
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "81266955"
---
# <a name="number-box"></a>数字框

表示一个可以用来显示和编辑数字的控件。 它支持验证、递增步进以及对基本等式进行内联计算，例如加减乘除。

**获取 Windows UI 库**

|  |  |
| - | - |
| ![WinUI 徽标](images/winui-logo-64x64.png) |  NumberBox 控件需要 Windows UI 库，该库是一个 NuGet 包，其中包含适用于 UWP 应用的新控件和 UI 功能。 有关详细信息（包括安装说明），请参阅 [Windows UI 库概述](https://docs.microsoft.com/uwp/toolkits/winui/)。 |

**Windows UI 库 API：** [NumberBox 类](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.NumberBox)

> [!TIP]
> 在本文档中，我们使用 XAML 中的 **muxc** 别名表示我们已包含在项目中的 Windows UI 库 API。 我们已将此项添加到我们的[页](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page)元素：`xmlns:muxc="using:Microsoft.UI.Xaml.Controls"`
>
>在后面的代码中，我们还使用 C# 中的 **muxc** 别名表示我们已包含在项目中的 Windows UI 库 API。 我们在文件顶部添加了此 **using** 语句：`using muxc = Microsoft.UI.Xaml.Controls;`

## <a name="is-this-the-right-control"></a>这是正确的控件吗？

可以使用 NumberBox 控件来捕获和显示数学输入。 如果需要一个可编辑文本框来接受数字以外的输入，请使用 [TextBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) 控件。 如果需要一个可编辑文本框来接受密码或其他敏感输入，请参阅 [PasswordBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.passwordbox)。 如果需要一个文本框来输入搜索词，请参阅 [AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox)。 如果需要输入或编辑带格式的文本，请参阅 [RichEditBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richeditbox)。

## <a name="examples"></a>示例

<table>
<th align="left">XAML 控件库<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果已安装 <strong style="font-weight: semi-bold">XAML 控件库</strong>应用，请单击此处<a href="xamlcontrolsgallery:/item/TextBox">打开此应用，了解 NumberBox 的实际应用</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

### <a name="create-a-simple-numberbox"></a>创建一个简单的 NumberBox

下面是基本 NumberBox 的 XAML，演示了默认外观。 请使用 [x:Bind](/windows/uwp/xaml-platform/x-bind-markup-extension#property-path) 来确保显示给用户的数据始终与存储在应用中的数据同步。

```xaml
<muxc:NumberBox Value="{x:Bind Path=ViewModel.NumberBoxValue, Mode=TwoWay}" />
```

![一个聚焦的输入字段，显示 0。](images/numberbox-basic.png)

### <a name="labeling-numberbox"></a>标记 NumberBox

如果 NumberBox 的用途不明确，请使用 `Header` 或 `PlaceholderText`。 无论 NumberBox 是否有值，`Header` 都可见。

```xaml
<muxc:NumberBox Header="Enter a number:"
    Value="{x:Bind Path=ViewModel.NumberBoxValue, Mode=TwoWay}" />
```

![NumberBox 上方一个显示内容为“请输入表达式:”的标头。](images/numberbox-header.png)

`PlaceholderText` 显示在 NumberBox 中，并且只会在 `Value` 已设置为 NaN 或输入已由用户清除的情况下显示。

```xaml
<muxc:NumberBox PlaceholderText="1+2^2"
    Value="{x:Bind Path=ViewModel.NumberBoxValue, Mode=TwoWay}" />
```

![一个 NumberBox，包含的占位符文本显示“A + B”。](images/numberbox-placeholder-text.png)

### <a name="enable-calculation-support"></a>启用计算支持

将 `AcceptsExpression` 属性设置为 true 即可让 NumberBox 对基本的内联表达式进行计算，例如按标准运算顺序进行加减乘除。 当失去焦点或用户按“Enter”键时，会触发计算。 计算某个表达式以后，系统不保留该表达式的原始形式。

```xaml
<muxc:NumberBox Value="{x:Bind Path=ViewModel.NumberBoxValue, Mode=TwoWay}"
    AcceptsExpression="True" />
```

### <a name="increment-and-decrement-stepping"></a>递增和递减步进

请使用 `SmallChange` 属性来配置当 NumberBox 处于焦点位置且用户执行以下操作时 NumberBox 中值的变化情况：

- 滚动
- 按向上键
- 按向下键

请使用 `LargeChange` 属性来配置当 NumberBox 处于焦点位置且用户按 PageUp 或 PageDown 键时 NumberBox 中值的变化情况。

请使用 `SpinButtonPlacementMode` 属性来启用特定按钮，这些按钮在单击后即可将 NumberBox 中的值递增或递减 `SmallChange` 属性所指定的量。 如果再执行一个步骤就会超出最大值或最小值，则会禁用这些按钮。

将 `SpinButtonPlacementMode` 设置为 `Inline`，这些按钮就会显示在控件旁边。

```xaml
<muxc:NumberBox Value="{x:Bind Path=ViewModel.NumberBoxValue, Mode=TwoWay}"
    SmallChange="10"
    LargeChange="100"
    SpinButtonPlacementMode="Inline" />
```

![一个 NumberBox，其旁边有一个向下箭头按钮和一个向上箭头按钮。](images/numberbox-spinbutton-inline.png)

将 `SpinButtonPlacementMode` 设置为 `Compact`，则只有在 NumberBox 处于焦点位置时，这些按钮才会作为浮出控件显示。

```xaml
<muxc:NumberBox Value="{x:Bind Path=ViewModel.NumberBoxValue, Mode=TwoWay}"
    SmallChange="10"
    LargeChange="100"
    SpinButtonPlacementMode="Compact" />
```

![一个 NumberBox，其中有一个小的图标，显示一个向上箭头和一个向下箭头。](images/numberbox-spinbutton-compact-non-visible.png)

![一个 NumberBox，在提升层中有一个向下箭头按钮和一个向上箭头按钮浮出到旁边。](images/numberbox-spinbutton-compact-visible.png)

### <a name="enabling-input-validation"></a>启用输入验证

将 `ValidationMode` 设置为 `InvalidInputOverwritten`，则当因为失去焦点或按“Enter”键而触发计算时，NumberBox 就会覆盖不是数字的或与上一个有效值的合法格式不符的无效输入。

```xaml
<muxc:NumberBox Header="Quantity"
    Value="{x:Bind Path=ViewModel.NumberBoxValue, Mode=TwoWay}"
    ValidationMode="InvalidInputOverwritten" />
```

将 `ValidationMode` 设置为 `Disabled` 即可配置自定义输入验证。

对于小数点和逗号，用户使用的格式设置会被替换为为 NumberBox 配置的格式设置。 不会触发输入验证错误。

### <a name="formatting-input"></a>对输入进行格式设置

[数字格式设置](/uwp/api/windows.globalization.numberformatting)可以用来设置 Numberbox 值的格式，只需配置格式设置类的一个实例并将其分配给 `NumberFormatter` 属性即可。 小数、货币、百分比和有效数字是可用的数字格式设置类的一部分。 请注意，舍入也由数字格式设置属性来定义。

下面是一个示例，演示了如何使用 DecimalFormatter 来设置 NumberBox 的值的格式，使之具有一个整数位、两个小数位并向上舍入到最接近的 0.25：

```xaml
<muxc:NumberBox  x:Name="FormattedNumberBox"
    Value="{x:Bind Path=ViewModel.NumberBoxValue, Mode=TwoWay}" />
```

```csharp
private void SetNumberBoxNumberFormatter()
{
    IncrementNumberRounder rounder = new IncrementNumberRounder();
    rounder.Increment = 0.25;
    rounder.RoundingAlgorithm = RoundingAlgorithm.RoundUp;

    DecimalFormatter formatter = new DecimalFormatter();
    formatter.IntegerDigits = 1;
    formatter.FractionDigits = 2;
    formatter.NumberRounder = rounder;
    FormattedNumberBox.NumberFormatter = formatter;
}
```

![一个 NumberBox，显示一个值 0.00。](images/numberbox-formatted.png)

对于小数点和逗号，用户使用的格式设置会被替换为为 NumberBox 配置的格式设置。 不会触发输入验证错误。

## <a name="remarks"></a>备注

### <a name="input-scope"></a>输入范围

`Number` 将用于[输入范围](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.InputScopeNameValue)。 此输入范围适用于数字 0-9。 此项可以覆盖，但系统不会显式支持替代的 InputScope 类型。

### <a name="not-a-number"></a>不是数字

清除 NumberBox 的输入以后，`Value` 会被设置为 `NaN`，表示不存在数字值。

### <a name="expression-evaluation"></a>表达式计算

NumberBox 使用中缀表示法来计算表达式。 允许使用的运算符的优先级顺序如下：

* ^
* */
* +-

请注意，可以使用括号来重写优先规则。

## <a name="recommendations"></a>建议

* 可以使用 `Text` 和 `Value` 轻松地将 NumberBox 的值捕获为字符串或双精度型，不需在不同类型之间转换值。 以编程方式更改 NumberBox 的值时，建议通过 `Value` 属性来那样做。 `Value` 会覆盖初始设置中的 `Text`。 完成初始设置以后，对其中一项的更改会传播到另一项中，但通过 `Value` 以一致方式进行编程式更改有助于避免任何概念上的误解，那就是：NumberBox 会通过 `Text` 接受非数字字符。
* 使用 `Header` 或 `PlaceholderText` 告知用户：NumberBox 仅接受数字字符作为输入。 数字的拼写表示形式（例如“one”）不会解析为可接受的值。
