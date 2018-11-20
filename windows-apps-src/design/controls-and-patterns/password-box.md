---
author: Jwmsft
Description: A password box is a text input box that conceals the characters typed into it for the purpose of privacy.
title: 密码框指南
ms.assetid: 332B04D6-4FFE-42A4-8B3D-ABE8266C7C18
dev.assetid: 4BFDECC6-9BC5-4FF5-8C63-BB36F6DDF2EF
label: Password box
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: miguelrb
design-contact: ksulliv
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: c9e283c8f5c116e300b98d7e4078d91e4dac207e
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2018
ms.locfileid: "7425526"
---
# <a name="password-box"></a>密码框

 

密码框是指出于隐私目的隐藏所键入的字符的文本输入框。 密码框的外观类似文本框，区别在于它在已输入文本的位置呈现占位符。 可配置占位符。

> **重要 API**：[PasswordBox 类](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.passwordbox.aspx)，[Password 属性](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.passwordbox.password.aspx)，[PasswordChar 属性](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.passwordbox.passwordchar.aspx)，[PasswordRevealMode 属性](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.passwordbox.passwordrevealmode.aspx)，[PasswordChanged 事件](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.passwordbox.passwordchanged.aspx)

默认情况下，密码框向用户提供了查看其密码的方法，即按下显示按钮。 你可以禁用显示按钮，或提供显示密码的替代机制，如复选框。

## <a name="is-this-the-right-control"></a>这是正确的控件吗？

使用 **PasswordBox** 控件收集密码或其他隐私数据，如身份证号。

有关选择正确文本控件的详细信息，请参阅[文本控件](text-controls.md)文章。

## <a name="examples"></a>示例

<table>
<th align="left">XAML 控件库<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果已安装 <strong style="font-weight: semi-bold">XAML 控件库</strong>应用，请单击此处<a href="xamlcontrolsgallery:/item/PasswordBox">打开此应用，了解 PasswordBox 的实际应用</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

密码框具有多种状态，包括以下明显的状态。

处于闲置状态的密码框可以显示提示文本，以使用户知道它的用途：

![带有提示文本的处于闲置状态的密码框](images/passwordbox-rest-hinttext.png)

当用户在密码框中键入时，默认行为是显示隐藏正在输入的文本的项目符号：

![正在键入文本的密码框焦点状态](images/passwordbox-focus-typing.png)

按右侧的“显示”按钮可粗略地看到正在输入的密码：

![显示文本的密码框](images/passwordbox-text-reveal.png)

## <a name="create-a-password-box"></a>创建密码框

使用 [Password](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.passwordbox.password.aspx) 属性获取或设置 PasswordBox 的内容。 你可以在 [PasswordChanged](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.passwordbox.passwordchanged.aspx) 事件的处理程序中执行此操作，以便在用户输入密码时执行验证。 或者，你可以使用其他事件（如按钮 [Click](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.primitives.buttonbase.click.aspx)），以在用户完成文本输入后执行验证。

下面是密码框控件的 XAML，演示了 PasswordBox 的默认外观。 当用户输入密码时，通过检查以判断它是否为文本值“Password”。 如果是，则向用户显示一条消息。

```xaml
<StackPanel>  
  <PasswordBox x:Name="passwordBox" Width="200" MaxLength="16"
             PasswordChanged="passwordBox_PasswordChanged"/>

  <TextBlock x:Name="statusText" Margin="10" HorizontalAlignment="Center" />
</StackPanel>   
```

```csharp
private void passwordBox_PasswordChanged(object sender, RoutedEventArgs e)
{
    if (passwordBox.Password == "Password")
    {
        statusText.Text = "'Password' is not allowed as a password.";
    }
    else
    {
        statusText.Text = string.Empty;
    }
}
```
下面是运行此代码且用户输入“Password”时的结果。

![带有验证消息的密码框](images/passwordbox-revealed-validation.png)

### <a name="password-character"></a>密码字符

通过设置 [PasswordChar](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.passwordbox.passwordchar.aspx) 属性，你可以更改用于屏蔽密码的字符。 此处使用星号替换默认项目符号。

```xaml
<PasswordBox x:Name="passwordBox" Width="200" PasswordChar="*"/>
```

结果如下所示。

![带有自定义字符的密码框](images/passwordbox-custom-char.png)

### <a name="headers-and-placeholder-text"></a>标头和占位符文本

你可以使用 [Header](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.passwordbox.header.aspx) 和 [PlaceholderText](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.passwordbox.placeholdertext.aspx) 属性为 PasswordBox 提供上下文。 这在你拥有多个框时（例如在更改密码的窗体上）非常有用。

```xaml
<PasswordBox x:Name="passwordBox" Width="200" Header="Password" PlaceholderText="Enter your password"/>
```

![带有提示文本的处于闲置状态的密码框](images/passwordbox-rest-hinttext.png)

### <a name="maximum-length"></a>最大长度。

通过设置 [MaxLength](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.passwordbox.maxlength.aspx) 属性，指定用户可输入的最大字符数。 不存在指定最小长度的属性，但你可以在应用代码中检查密码长度并执行任何其他验证。

## <a name="password-reveal-mode"></a>密码显示模式

PasswordBox 具有内置按钮，用户按下该按钮可显示密码文本。 下面是用户操作的结果。 当用户释放该按钮时，密码会自动重新隐藏。

![显示文本的密码框](images/passwordbox-text-reveal.png)

### <a name="peek-mode"></a>速览模式

默认情况下，会显示密码显示按钮（或“速览”按钮）。 用户必须持续按住按钮来查看密码，以便保持较高级别的安全性。

[PasswordRevealMode](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.passwordbox.passwordrevealmode.aspx) 属性值不是确定用户能否看到密码显示按钮的唯一因素。 其他因素包括控件所示宽度是否大于最小值、PasswordBox 是否具有焦点以及文本输入字段是否至少包含一个字符。 密码显示按钮仅在 PasswordBox 首次接收焦点并且在用户输入一个字符后显示。 如果 PasswordBox 失去焦点后又重新获得焦点，除非已清除密码并从头开始输入字符，否则显示按钮将不会再次显示。

> **注意**&nbsp;&nbsp;在 Windows 10 之前，密码显示按钮在默认情况下不显示。 如果应用安全要求始终掩盖密码，请确保将 PasswordRevealMode 设置为 Hidden。

### <a name="hidden-and-visible-modes"></a>隐藏和可见模式

其他 [PasswordRevealMode](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.passwordrevealmode.aspx) 枚举值（**Hidden** 和 **Visible**）会隐藏密码显示按钮，并允许你以编程方式管理是否掩盖密码。

若要始终掩盖密码，请将 PasswordRevealMode 设置为 Hidden。 如果你不需要始终掩盖密码，可以提供自定义 UI，使用户在 Hidden 和 Visible 之间切换 PasswordRevealMode。

在以前版本的 Windows Phone 中，PasswordBox 使用复选框切换是否掩盖密码。 你可以为你的应用创建类似的 UI，如以下示例所示。 你还可以使用其他控件（例如 [ToggleButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.primitives.togglebutton.aspx)）以使用户切换模式。

此示例展示了如何使用 [CheckBox](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.checkbox.aspx) 使用户切换 PasswordBox 的显示模式。

```xaml
<StackPanel Width="200">
    <PasswordBox Name="passwordBox1"
                 PasswordRevealMode="Hidden"/>
    <CheckBox Name="revealModeCheckBox" Content="Show password"
              IsChecked="False"
              Checked="CheckBox_Changed" Unchecked="CheckBox_Changed"/>
</StackPanel>
```

```csharp
private void CheckBox_Changed(object sender, RoutedEventArgs e)
{
    if (revealModeCheckBox.IsChecked == true)
    {
        passwordBox1.PasswordRevealMode = PasswordRevealMode.Visible;
    }
    else
    {
        passwordBox1.PasswordRevealMode = PasswordRevealMode.Hidden;
    }
}
```

此 PasswordBox 的外观如下所示。

![带有自定义显示按钮的密码框](images/passwordbox-custom-reveal.png)

## <a name="choose-the-right-keyboard-for-your-text-control"></a>为文本控件选择正确的键盘

若要帮助用户使用触摸键盘或软输入面板 (SIP) 输入数据，你可以将文本控件的输入范围设置为与期望用户输入的数据类型匹配。 PasswordBox 仅支持 **Password** 和 **NumericPin** 输入范围值。 将忽略任何其他值。

有关如何使用输入范围的详细信息，请参阅[使用输入范围更改触摸键盘](https://msdn.microsoft.com/library/windows/apps/mt280229)。

## <a name="recommendations"></a>建议

-   如果密码框的用途不甚清楚，请使用标签或占位符文本。 无论文本输入框是否具有值，标签都可见。 占位符文本显示在文本输入框内，并在输入值后立即消失。
-   针对可输入值的范围，为密码框提供适当的宽度。 单词长度因语言而异，因此如果希望应用世界通用，请将本地化考虑在内。
-   不要紧挨密码输入框放置另一个控件。 密码框具有密码显示按钮，供用户验证已键入的密码。紧挨密码输入框放置其他控件会使用户在尝试与其他控件交互时，不小心显示其密码。 为了防止出现这种情况，请在输入框中的密码和另一个控件之间设置一些间距，或者将另一个控件放在下一行中。
-   请考虑为帐户创建显示两个密码框：一个用于新密码，第二个用于确认新密码。
-   仅为登录显示单个密码框。
-   当密码框用于输入 PIN 时，请考虑在输入最后一个数字后立即提供即时响应，而不是使用确认按钮。

## <a name="get-the-sample-code"></a>获取示例代码

- [XAML 控件库示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 以交互式格式查看所有 XAML 控件。

## <a name="related-articles"></a>相关文章

[文本控件](text-controls.md)

- [拼写检查指南](text-controls.md)
- [添加搜索](https://msdn.microsoft.com/library/windows/apps/hh465231)
- [文本输入指南](text-controls.md)
- [TextBox 类](https://msdn.microsoft.com/library/windows/apps/br209683)
- [Windows.UI.Xaml.Controls PasswordBox 类](https://msdn.microsoft.com/library/windows/apps/br227519)
- [String.Length 属性](https://msdn.microsoft.com/library/system.string.length(v=vs.110).aspx)
