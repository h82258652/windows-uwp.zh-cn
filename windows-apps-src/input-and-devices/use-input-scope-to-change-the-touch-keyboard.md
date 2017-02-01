---
author: Karl-Bridge-Microsoft
Description: "若要帮助用户使用触摸键盘或软输入面板 (SIP) 输入数据，你可以将文本控件的输入范围设置为与期望用户输入的数据类型匹配。"
MS-HAID: dev\_ctrl\_layout\_txt.use\_input\_scope\_to\_change\_the\_touch\_keyboard
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: "使用输入范围更改触摸键盘"
ms.assetid: 6E5F55D7-24D6-47CC-B457-B6231EDE2A71
template: detail.hbs
keywords: "键盘、辅助功能、导航、焦点、文本、输入、用户交互"
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: b258771c887d4422433522344b11130b7e9ed1e6
ms.openlocfilehash: caaa6228f2d5b2bb6566ccb285d90a396a1caf01

---

# <a name="use-input-scope-to-change-the-touch-keyboard"></a>使用输入范围更改触摸键盘
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

若要帮助用户使用触摸键盘或软输入面板 (SIP) 输入数据，你可以将文本控件的输入范围设置为与期望用户输入的数据类型匹配。

<div class="important-apis" >
<b>重要的 API</b><br/>
<ul>
<li>[**InputScope**](https://msdn.microsoft.com/library/windows/apps/hh702632)</li>
<li>[**InputScopeNameValue**](https://msdn.microsoft.com/library/windows/apps/hh702028)</li>
</ul>
</div>


当应用在具有触摸屏的设备上运行时，触摸键盘可用于文本输入。 当用户点击可编辑的输入字段（如 [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) 或 [**RichEditBox**](https://msdn.microsoft.com/library/windows/apps/br227548)）时，系统会调用触摸键盘。 通过将文本控件的*输入范围*设置为与你期望用户输入的数据类型匹配，可以让用户在应用中更快捷地输入数据。 输入范围会针对控件所预期的文本输入类型向系统提供提示，以便系统可以为该输入类型提供专用的触摸键盘布局。

例如，如果文本框仅用于输入一个 4 位数的 PIN，请将 [**InputScope**](https://msdn.microsoft.com/library/windows/apps/hh702632) 属性设置为 **Number**。 这将通知系统显示数字键盘布局，以便于用户输入 PIN。

> **重要提示**&nbsp;&nbsp;
- 此信息仅适用于 SIP。 它不适用于硬件键盘或 Windows“轻松使用”选项中提供的屏幕键盘。
- 输入范围不会导致任何输入验证的执行，并且不会阻止用户通过硬件键盘或其他输入设备提供任何输入。 你仍然负责按需在代码中验证输入。

## <a name="changing-the-input-scope-of-a-text-control"></a>更改文本控件的输入范围

可用于你的应用的输入范围是 [**InputScopeNameValue**](https://msdn.microsoft.com/library/windows/apps/hh702028) 枚举的成员。 你可以将 [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) 或 [**RichEditBox**](https://msdn.microsoft.com/library/windows/apps/br227548) 的 **InputScope** 属性设置为以下值之一。

> **重要提示**&nbsp;&nbsp;[**PasswordBox**](https://msdn.microsoft.com/library/windows/apps/br227519) 上的 [**InputScope**](https://msdn.microsoft.com/library/windows/apps/dn996570) 属性仅支持 **Password** 和 **NumericPin** 值。 将忽略任何其他值。

在这里，你可以更改多个文本框的输入范围以匹配每个文本框的预期数据范围。

**更改 XAML 中的输入范围**

1.  在页面的 XAML 文件中，找到需要更改的文本标记。
2.  将 [**InputScope**](https://msdn.microsoft.com/library/windows/apps/hh702632) 属性应用到标记并指定与预期输入匹配的 [**InputScopeNameValue**](https://msdn.microsoft.com/library/windows/apps/hh702028) 值。

    下面是一些可能会出现在常用客户联系信息表中的文本框。 设置 [**InputScope**](https://msdn.microsoft.com/library/windows/apps/hh702632) 后，将针对每个文本框显示数据布局合理的触摸键盘。

    ```xaml
    <StackPanel Width="300">
        <TextBox Header="Name" InputScope="Default"/>
        <TextBox Header="Email Address" InputScope="EmailSmtpAddress"/>
        <TextBox Header="Telephone Number" InputScope="TelephoneNumber"/>
        <TextBox Header="Web site" InputScope="Url"/>
    </StackPanel>
    ```

**更改代码中的输入范围**

1.  在页面的 XAML 文件中，找到需要更改的文本标记。 如果未设置，请设置 [x:Name 属性](https://msdn.microsoft.com/library/windows/apps/mt204788)以便可以在代码中引用控件。

    ```csharp
    <TextBox Header="Telephone Number" x:Name="phoneNumberTextBox"/>
    ```

2.  实例化新的 [**InputScope**](https://msdn.microsoft.com/library/windows/apps/hh702025) 对象。

    ```csharp
    InputScope scope = new InputScope();
    ```

3.  实例化新的 [**InputScopeName**](https://msdn.microsoft.com/library/windows/apps/hh702027) 对象。
    
    ```csharp
    InputScopeName scopeName = new InputScopeName();
    ```

4.  将 [**InputScopeName**](https://msdn.microsoft.com/library/windows/apps/hh702032) 对象的 [**NameValue**](https://msdn.microsoft.com/library/windows/apps/hh702027) 属性设置为 [**InputScopeNameValue**](https://msdn.microsoft.com/library/windows/apps/hh702028) 枚举的值。

    ```csharp
    scopeName.NameValue = InputScopeNameValue.TelephoneNumber;
    ```

5.  将 [**InputScopeName**](https://msdn.microsoft.com/library/windows/apps/hh702027) 对象添加到 [**InputScope**](https://msdn.microsoft.com/library/windows/apps/hh702034) 对象的 [**Names**](https://msdn.microsoft.com/library/windows/apps/hh702025) 集合。

    ```csharp
    scope.Names.Add(scopeName);
    ```

6.  将 [**InputScope**](https://msdn.microsoft.com/library/windows/apps/hh702025) 对象设置为文本控件的 [**InputScope**](https://msdn.microsoft.com/library/windows/apps/hh702632) 属性的值。

    ```csharp
    phoneNumberTextBox.InputScope = scope;
    ```

下面是全部代码。

```CSharp
InputScope scope = new InputScope();
InputScopeName scopeName = new InputScopeName();
scopeName.NameValue = InputScopeNameValue.TelephoneNumber;
scope.Names.Add(scopeName);
phoneNumberTextBox.InputScope = scope;
```

相同的步骤可以压缩为此简写代码。

```CSharp
phoneNumberTextBox.InputScope = new InputScope() 
{
    Names = {new InputScopeName(InputScopeNameValue.TelephoneNumber)}
};
```

## <a name="text-prediction-spell-checking-and-auto-correction"></a>文本预测、拼写检查和自动更正

[**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) 和 [**RichEditBox**](https://msdn.microsoft.com/library/windows/apps/br227548) 控件具有多个属性，这些属性会影响 SIP 的行为。 若要为你的用户提供最佳体验，则了解在使用触摸键盘时这些属性如何影响文本输入非常重要。

-   [**IsSpellCheckEnabled**](https://msdn.microsoft.com/library/windows/apps/br209688)：为文本控件启用拼写检查后，该控件将与系统的拼写检查引擎进行交互，以标记无法识别的单词。 点击一个单词即可查看建议的更正单词列表。 默认情况下，拼写检查处于启用状态。

    在 **Default** 输入范围中，使用此属性还可以使句子中第一个单词的首字母自动大写，并自动更正所键入的单词。 在其他输入范围中可能会禁用这些自动更正功能。 有关详细信息，请参阅本主题后面的表格。

-   [**IsTextPredictionEnabled**](https://msdn.microsoft.com/library/windows/apps/br209690)：为文本控件启用文本预测后，系统将显示你可能会键入的单词的列表。 你可以从该列表中进行选择，因此你无需键入整个单词。 默认情况下，文本预测处于启用状态。

    如果输入范围的属性不为 **Default**，将禁用文本预测，即使 [**IsTextPredictionEnabled**](https://msdn.microsoft.com/library/windows/apps/br209690) 属性为 **true** 也是如此。 有关详细信息，请参阅本主题后面的表格。

    **注意**&nbsp;&nbsp;在移动设备系列上，文本预测和拼写更正将显示在键盘上方区域的 SIP 中。 如果 [**IsTextPredictionEnabled**](https://msdn.microsoft.com/library/windows/apps/br209690) 设置为 **false**，则将隐藏 SIP 的此部分，并禁用自动更正，即使 [**IsSpellCheckEnabled**](https://msdn.microsoft.com/library/windows/apps/br209688) 为 **true** 也是如此。

-   [**PreventKeyboardDisplayOnProgrammaticFocus**](https://msdn.microsoft.com/library/windows/apps/dn299273)：若此属性为 **true**，则系统在采用编程方式在文本控件上设置焦点时，不显示 SIP。 而仅在用户与控件交互时才显示该键盘。

## <a name="touch-keyboard-index-for-windows-and-windows-phone"></a>适用于 Windows 和 Windows Phone 的触摸键盘索引

这些表显示了针对常见输入范围值的桌面设备和移动设备上的软输入面板 (SIP) 布局。 针对每个输入范围列出了输入范围对 **IsSpellCheckEnabled** 和 **IsTextPredictionEnabled** 属性所启用的功能的影响。 这并非是可用输入范围的完整列表。

> **注意**&nbsp;&nbsp;移动设备上的 SIP 较小，因此对移动应用来说，设置正确的输入范围尤其重要。 正如此处所示，Windows Phone 提供更多类型的专用键盘布局。 当文本字段不需要在 Windows 应用商店应用中设置自己的输入范围时，在 Windows Phone 应用商店应用中设置输入范围可能更有益。

> **提示**&nbsp;&nbsp;大多数触摸键盘可在字母布局与数字和符号布局之间进行切换。 在 Windows 上，切换“&amp;123”****键。 在 Windows Phone 上，按“&amp; 123”****键可切换到数字和符号布局，而按“abcd”****键可切换到字母布局。

### <a name="default"></a>默认值

`<TextBox InputScope="Default"/>`

默认键盘。

| Windows                                                    | Windows Phone                                                    |
|------------------------------------------------------------|------------------------------------------------------------------|
| ![默认 Windows 触摸键盘](images/input-scopes/kbdpcdefault.png) | ![默认 Windows Phone 触摸键盘](images/input-scopes/kbdwpdefault.png) |

功能的可用性：

-   拼写检查：如果 **IsSpellCheckEnabled** = **true**，则处于启用状态；如果 **IsSpellCheckEnabled** = **false**，则处于禁用状态
-   自动更正：如果 **IsSpellCheckEnabled** = **true**，则处于启用状态；如果 **IsSpellCheckEnabled** = **false**，则处于禁用状态
-   首字母自动大写：如果 **IsSpellCheckEnabled** = **true**，则处于启用状态；如果 **IsSpellCheckEnabled** = **false**，则处于禁用状态
-   文本预测：如果 **IsTextPredictionEnabled** = **true**，则处于启用状态；如果 **IsTextPredictionEnabled** = **false**，则处于禁用状态

### <a name="currencyamountandsymbol"></a>CurrencyAmountAndSymbol

`<TextBox InputScope="CurrencyAmountAndSymbol"/>`

默认数字和符号键盘布局。

| Windows                                                    | Windows Phone                                                    |
|------------------------------------------------------------|------------------------------------------------------------------|
| ![可输入货币字符的 Windows 触摸键盘](images/input-scopes/kbdpccurrencyamountandsymbol.png)<br>还包含向左翻页/向右翻页键以显示更多的符号。| ![可输入货币字符的 Windows Phone 触摸键盘](images/input-scopes/kbdwpcurrencyamountandsymbol.png) |
|功能的可用性：<ul><li>拼写检查：默认情况下处于启用状态，可以禁用</li><li>自动更正：始终处于禁用状态</li><li>首字母自动大写：始终处于禁用状态</li><li>文本预测：始终处于禁用状态</li></ul>等同于 **Number** 和 **TelephoneNumber**。 | 功能的可用性：<ul><li>拼写检查：默认情况下处于启用状态，可以禁用</li><li>自动更正：默认情况下处于启用状态，可以禁用</li><li>首字母自动大写：始终处于禁用状态</li><li>文本预测：默认情况下处于启用状态，可以禁用</li>| 

### <a name="url"></a>URL

`<TextBox InputScope="Url"/>`

包括**“.com”**键和![“转到”](images/input-scopes/kbdgokey.png)键。 长按“.com”****键可以显示其他选项（**.org**、**.net** 以及特定于区域的后缀）。

| Windows                                                    | Windows Phone                                                    |
|------------------------------------------------------------|------------------------------------------------------------------|
| ![可输入 URL 的 Windows 触摸键盘](images/input-scopes/kbdpcurl.png)<br>还包括“:”****、“-”****和“/”****键。| ![可输入 URL 的 Windows Phone 触摸键盘](images/input-scopes/kbdwpurl.png)<br>长按句号键可以显示其他选项 ( - + &quot; / &amp; : , )。 |
|功能的可用性：<ul><li>拼写检查：默认情况下处于启用状态，可以禁用</li><li>自动更正：默认情况下处于启用状态，可以禁用</li><li>首字母自动大写：始终处于禁用状态</li><li>文本预测：始终处于禁用状态</li></ul> | 功能的可用性：<ul><li>拼写检查：默认情况下处于禁用状态，可以启用</li><li>自动更正：默认情况下处于禁用状态，可以启用</li><li>首字母自动大写：默认情况下处于禁用状态，可以启用</li><li>文本预测：默认情况下处于禁用状态，可以启用</li></ul> |

### <a name="emailsmtpaddress"></a>EmailSmtpAddress

`<TextBox InputScope="EmailSmtpAddress"/>`

包括“@”****和“.com”****键。 长按“.com”****键可以显示其他选项（**.org**、**.net** 以及特定于区域的后缀）。

| Windows                                                    | Windows Phone                                                    |
|------------------------------------------------------------|------------------------------------------------------------------|
| ![可输入电子邮件地址的 Windows 触摸键盘](images/input-scopes/kbdpcemailsmtpaddress.png)<br>还包括“_”****和“-”****键。| ![可输入电子邮件地址的 Windows Phone 触摸键盘](images/input-scopes/kbdwpemailsmtpaddress.png)<br>长按句号键可以显示其他选项 (- _ , ;)。 |
|功能的可用性：<ul><li>拼写检查：默认情况下处于启用状态，可以禁用</li><li>自动更正：默认情况下处于启用状态，可以禁用</li><li>首字母自动大写：始终处于禁用状态</li><li>文本预测：始终处于禁用状态</li></ul> | 功能的可用性：<ul><li>拼写检查：默认情况下处于禁用状态，可以启用</li><li>自动更正：默认情况下处于禁用状态，可以启用</li><li>首字母自动大写：默认情况下处于禁用状态，可以启用</li><li>文本预测：默认情况下处于禁用状态，可以启用</li></ul> |

### <a name="number"></a>编号

`<TextBox InputScope="Number"/>`

| Windows                                                    | Windows Phone                                                    |
|------------------------------------------------------------|------------------------------------------------------------------|
| ![可输入数字的 Windows 触摸键盘](images/input-scopes/kbdpccurrencyamountandsymbol.png)| ![可输入数字的 Windows Phone 触摸键盘](images/input-scopes/kbdwpnumber.png)<br>键盘包含数字和小数点。 长按小数点键可以显示其他选项 (, -)。 |
|等同于 **CurrencyAmountAndSymbol** 和 **TelephoneNumber**。 | 功能的可用性：<ul><li>拼写检查：始终处于禁用状态</li><li>自动更正：始终处于禁用状态</li><li>首字母自动大写：始终处于禁用状态</li><li>文本预测：始终处于禁用状态</li></ul> |

### <a name="telephonenumber"></a>TelephoneNumber

`<TextBox InputScope="TelephoneNumber"/>`

| Windows                                                    | Windows Phone                                                    |
|------------------------------------------------------------|------------------------------------------------------------------|
| ![可输入电话号码的 Windows 触摸键盘](images/input-scopes/kbdpccurrencyamountandsymbol.png)| ![可输入电话号码的 Windows Phone 触摸键盘](images/input-scopes/kbdwptelephonenumber.png)<br>键盘可模拟电话数字小键盘。 长按句号键可以显示其他选项 ( , ( ) X。 )。 长按 0 键可以输入 +。 |
|等同于 **CurrencyAmountAndSymbol** 和 **TelephoneNumber**。 | 功能的可用性：<ul><li>拼写检查：始终处于禁用状态</li><li>自动更正：始终处于禁用状态</li><li>首字母自动大写：始终处于禁用状态</li><li>文本预测：始终处于禁用状态</li></ul> |

### <a name="search"></a>搜索

`<TextBox InputScope="Search"/>`

包括“搜索”****键，而不是“输入”****键。

| Windows                                                    | Windows Phone                                                    |
|------------------------------------------------------------|------------------------------------------------------------------|
| ![用于搜索的 Windows 触摸键盘](images/input-scopes/kbdpcsearch.png)| ![用于搜索的 Windows Phone 触摸键盘](images/input-scopes/kbdwpsearch.png)|
|功能的可用性：<ul><li>拼写检查：默认情况下处于启用状态，可以禁用</li><li>自动更正：始终处于禁用状态</li><li>首字母自动大写：始终处于禁用状态</li><li>文本预测：默认情况下处于启用状态，可以禁用</li></ul> | 功能的可用性：<ul><li>拼写检查：默认情况下处于启用状态，可以禁用</li><li>自动更正：默认情况下处于启用状态，可以禁用</li><li>首字母自动大写：始终处于禁用状态</li><li>文本预测：默认情况下处于启用状态，可以禁用</li></ul> |

### <a name="searchincremental"></a>SearchIncremental

`<TextBox InputScope="SearchIncremental"/>`

| Windows                                                    | Windows Phone                                                    |
|------------------------------------------------------------|------------------------------------------------------------------|
| ![默认 Windows 触摸键盘](images/input-scopes/kbdpcdefault.png)<br>与 **Default** 的布局相同。| ![默认 Windows Phone 触摸键盘](images/input-scopes/kbdwpdefault.png)|
|功能的可用性：<ul><li>拼写检查：默认情况下处于禁用状态，可以启用</li><li>自动更正：始终处于禁用状态</li><li>首字母自动大写：始终处于禁用状态</li><li>文本预测：始终处于禁用状态</li></ul> | 等同于 **Default**。 |

### <a name="formula"></a>公式

`<TextBox InputScope="Formula"/>`

包括“=”****键。

| Windows                                                    | Windows Phone                                                    |
|------------------------------------------------------------|------------------------------------------------------------------|
| ![可输入电话公式的 Windows 触摸键盘](images/input-scopes/kbdpcformula.png)<br>还包括“%”****、“$”****和“+”****键。| ![可输入公式的 Windows Phone 触摸键盘](images/input-scopes/kbdwpformula.png)<br>长按句号键可以显示其他选项 (- ! ? , )。 长按“=”****键可以显示其他选项 ( ( ) : &lt; &gt; )。 |
|功能的可用性：<ul><li>拼写检查：默认情况下处于禁用状态，可以启用</li><li>自动更正：始终处于禁用状态</li><li>首字母自动大写：始终处于禁用状态</li><li>文本预测：始终处于禁用状态</li></ul> | 功能的可用性：<ul><li>拼写检查：默认情况下处于启用状态，可以禁用</li><li>自动更正：默认情况下处于启用状态，可以禁用</li><li>首字母自动大写：始终处于禁用状态</li><li>文本预测：默认情况下处于启用状态，可以禁用</li></ul> |

### <a name="chat"></a>Chat

`<TextBox InputScope="Chat"/>`

| Windows                                                    | Windows Phone                                                    |
|------------------------------------------------------------|------------------------------------------------------------------|
| ![默认 Windows 触摸键盘](images/input-scopes/kbdpcdefault.png)<br>与 **Default** 的布局相同。| ![默认 Windows Phone 触摸键盘](images/input-scopes/kbdwpdefault.png)<br>与 **Default** 的布局相同。|
|功能的可用性：<ul><li>拼写检查：默认情况下处于禁用状态，可以启用</li><li>自动更正：始终处于禁用状态</li><li>首字母自动大写：始终处于禁用状态</li><li>文本预测：始终处于禁用状态</li></ul> | 功能的可用性：<ul><li>拼写检查：默认情况下处于启用状态，可以禁用</li><li>自动更正：默认情况下处于启用状态，可以禁用</li><li>首字母自动大写：默认情况下处于启用状态，可以禁用</li><li>文本预测：默认情况下处于启用状态，可以禁用</li></ul> |

### <a name="nameorphonenumber"></a>NameOrPhoneNumber

`<TextBox InputScope="NameOrPhoneNumber"/>`

| Windows                                                    | Windows Phone                                                    |
|------------------------------------------------------------|------------------------------------------------------------------|
| ![默认 Windows 触摸键盘](images/input-scopes/kbdpcdefault.png)<br>与 **Default** 的布局相同。| ![可输入名称或电话号码的 Windows Phone 触摸键盘](images/input-scopes/kbdwpnameorphonenumber.png)<br>包括“;”****和“@”****键。 用“123”****键来替换“&amp;123”****键，前者可用于打开电话数字小键盘（请参阅 **TelephoneNumber**）。|
|功能的可用性：<ul><li>拼写检查：默认情况下处于启用状态，可以禁用</li><li>自动更正：始终处于禁用状态</li><li>首字母自动大写：始终处于启用状态</li><li>文本预测：始终处于禁用状态</li></ul> | 功能的可用性：<ul><li>拼写检查：默认情况下处于禁用状态，可以启用</li><li>自动更正：默认情况下处于禁用状态，可以启用</li><li>首字母自动大写：默认情况下处于禁用状态，可以启用。 每个单词的首字母均大写。</li><li>文本预测：默认情况下处于禁用状态，可以启用</li></ul> |



<!--HONumber=Dec16_HO3-->


