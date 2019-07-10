---
Description: 若要帮助用户使用触摸键盘或软输入面板 (SIP) 输入数据，你可以将文本控件的输入范围设置为与期望用户输入的数据类型匹配。
MS-HAID: dev\_ctrl\_layout\_txt.use\_input\_scope\_to\_change\_the\_touch\_keyboard
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: 使用输入范围更改触摸键盘
ms.assetid: 6E5F55D7-24D6-47CC-B457-B6231EDE2A71
template: detail.hbs
keywords: 键盘、辅助功能、导航、焦点、文本、输入、用户交互
ms.date: 02/08/2017
ms.topic: article
ms.openlocfilehash: c522e21c45a3edd08a14b081cc227a83f19a3ea0
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2019
ms.locfileid: "66365354"
---
# <a name="use-input-scope-to-change-the-touch-keyboard"></a>使用输入范围更改触摸键盘

若要帮助用户使用触摸键盘或软输入面板 (SIP) 输入数据，你可以将文本控件的输入范围设置为与期望用户输入的数据类型匹配。

### <a name="important-apis"></a>重要的 API
- [InputScope](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.inputscope)
- [InputScopeNameValue](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.InputScopeNameValue)


当你的应用在具有触摸屏的设备上运行时，触摸键盘可用于文本输入。 当用户点击可编辑的输入字段（如 **[TextBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox)** 或 **[RichEditBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichEditBox)** ）时，系统会调用触摸键盘。 通过将文本控件的*输入范围*设置为与你期望用户输入的数据类型匹配，可以让用户在应用中更快捷地输入数据。 输入范围会针对控件所预期的文本输入类型向系统提供提示，以便系统可以为该输入类型提供专用的触摸键盘布局。

例如，如果文本框仅用于输入一个 4 位数的 PIN，请将 [**InputScope**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.inputscope) 属性设置为 **Number**。 这将通知系统显示数字键盘布局，以便于用户输入 PIN。

> [!IMPORTANT]
> - 此信息仅适用于 SIP。 它不适用于硬件键盘或 Windows“轻松使用”选项中提供的屏幕键盘。
> - 输入范围不会导致任何输入验证的执行，并且不会阻止用户通过硬件键盘或其他输入设备提供任何输入。 你仍然负责按需在代码中验证输入。

## <a name="changing-the-input-scope-of-a-text-control"></a>更改文本控件的输入范围

可用于你的应用的输入范围是 **[InputScopeNameValue](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.InputScopeNameValue)** 枚举的成员。 可以设置**InputScope**的属性 **[文本框](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox)** 或者 **[RichEditBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichEditBox)** 到其中一种值。

> [!IMPORTANT]
> **[InputScope](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.passwordbox.inputscope)** 属性上的 **[PasswordBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.PasswordBox)** 仅支持 **密码**和 **NumericPin** 值。 将忽略任何其他值。

在这里，你可以更改多个文本框的输入范围以匹配每个文本框的预期数据范围。

**若要更改在 XAML 中的输入的范围**

1.  在页面的 XAML 文件中，找到需要更改的文本标记。
2.  将 [**InputScope**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.inputscope) 属性应用到标记并指定与预期输入匹配的 [**InputScopeNameValue**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.InputScopeNameValue) 值。

    下面是一些可能会出现在常用客户联系信息表中的文本框。 设置 [**InputScope**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.inputscope) 后，将针对每个文本框显示数据布局合理的触摸键盘。

    ```xaml
    <StackPanel Width="300">
        <TextBox Header="Name" InputScope="Default"/>
        <TextBox Header="Email Address" InputScope="EmailSmtpAddress"/>
        <TextBox Header="Telephone Number" InputScope="TelephoneNumber"/>
        <TextBox Header="Web site" InputScope="Url"/>
    </StackPanel>
    ```

**若要更改在代码中的输入的范围**

1.  在页面的 XAML 文件中，找到需要更改的文本标记。 如果未设置，请设置 [x:Name 属性](https://docs.microsoft.com/windows/uwp/xaml-platform/x-name-attribute)以便可以在代码中引用控件。

    ```csharp
    <TextBox Header="Telephone Number" x:Name="phoneNumberTextBox"/>
    ```

2.  实例化新的 [**InputScope**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.InputScope) 对象。

    ```csharp
    InputScope scope = new InputScope();
    ```

3.  实例化新的 [**InputScopeName**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.InputScopeName) 对象。
    
    ```csharp
    InputScopeName scopeName = new InputScopeName();
    ```

4.  将 [**InputScopeName**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.inputscopename.namevalue) 对象的 [**NameValue**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.InputScopeName) 属性设置为 [**InputScopeNameValue**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.InputScopeNameValue) 枚举的值。

    ```csharp
    scopeName.NameValue = InputScopeNameValue.TelephoneNumber;
    ```

5.  将 [**InputScopeName**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.InputScopeName) 对象添加到 [**InputScope**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.inputscope.names) 对象的 [**Names**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.InputScope) 集合。

    ```csharp
    scope.Names.Add(scopeName);
    ```

6.  将 [**InputScope**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.InputScope) 对象设置为文本控件的 [**InputScope**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.inputscope) 属性的值。

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

[**TextBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) 和 [**RichEditBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichEditBox) 控件具有多个属性，这些属性会影响 SIP 的行为。 若要为你的用户提供最佳体验，则了解在使用触摸键盘时这些属性如何影响文本输入非常重要。

-   [**IsSpellCheckEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.isspellcheckenabled)— 控件的文本控件启用拼写检查时，与系统的标记无法识别的单词的拼写检查引擎交互。 点击一个单词即可查看建议的更正单词列表。 默认情况下，拼写检查处于启用状态。

    在 **Default** 输入范围中，使用此属性还可以使句子中第一个单词的首字母自动大写，并自动更正所键入的单词。 在其他输入范围中可能会禁用这些自动更正功能。 有关详细信息，请参阅本主题后面的表格。

-   [**IsTextPredictionEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.istextpredictionenabled)— 文本控件启用文本预测时，系统显示了您可能开始键入的单词列表。 你可以从该列表中进行选择，因此你无需键入整个单词。 默认情况下，文本预测处于启用状态。

    如果输入范围的属性不为 **Default**，将禁用文本预测，即使 [**IsTextPredictionEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.istextpredictionenabled) 属性为 **true** 也是如此。 有关详细信息，请参阅本主题后面的表格。

-   [**PreventKeyboardDisplayOnProgrammaticFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.preventkeyboarddisplayonprogrammaticfocus)— 当此属性是**true**，它会阻止系统显示 SIP 焦点时以编程方式设置文本控件上。 而仅在用户与控件交互时才显示该键盘。

## <a name="touch-keyboard-index-for-windows"></a>Windows 触控键盘索引

这些表显示了为常见的输入的范围值 Windows 软输入面板 (SIP) 布局。 针对每个输入范围列出了输入范围对 **IsSpellCheckEnabled** 和 **IsTextPredictionEnabled** 属性所启用的功能的影响。 这并非是可用输入范围的完整列表。

> [!Tip] 
> 您可以通过按下切换字母布局和数字和符号布局之间的大多数触摸键盘 **& 123**项以更改到数字和符号布局，并按**abcd**密钥将更改为字母的布局。

### <a name="default"></a>默认

`<TextBox InputScope="Default"/>`

默认 Windows 触摸键盘。

![默认 Windows 触摸键盘](images/input-scopes/default.png)
- 拼写检查：如果 **IsSpellCheckEnabled** = **true**，则处于启用状态；如果 **IsSpellCheckEnabled** = **false**，则处于禁用状态
- 自动更正：如果 **IsSpellCheckEnabled** = **true**，则处于启用状态；如果 **IsSpellCheckEnabled** = **false**，则处于禁用状态
- 首字母自动大写：如果 **IsSpellCheckEnabled** = **true**，则处于启用状态；如果 **IsSpellCheckEnabled** = **false**，则处于禁用状态
- 文本预测：如果 **IsTextPredictionEnabled** = **true**，则处于启用状态；如果 **IsTextPredictionEnabled** = **false**，则处于禁用状态

### <a name="currencyamountandsymbol"></a>CurrencyAmountAndSymbol

`<TextBox InputScope="CurrencyAmountAndSymbol"/>`

默认数字和符号键盘布局。

![可输入货币字符的 Windows 触摸键盘](images/input-scopes/currencyamountandsymbol.png)

- 包含页左/右键以显示更多的符号
- 拼写检查：默认情况下处于启用状态，可以禁用
- 自动更正：默认情况下处于启用状态，可以禁用
- 首字母自动大写：始终处于禁用状态
- 文本预测：默认情况下处于启用状态，可以禁用
 
### <a name="url"></a>url

`<TextBox InputScope="Url"/>`

![可输入 URL 的 Windows 触摸键盘](images/input-scopes/url.png)

- 包括 **“.com”** 键和![“转到”](images/input-scopes/kbdgokey.png)键。 按下并保持 **.com**键以显示其他选项 ( **.org**， **.net**，和特定于区域的后缀)
- 包括 **:** ， **-** ，并且 **/** 密钥
- 拼写检查：默认情况下处于禁用状态，可以启用
- 自动更正：默认情况下处于禁用状态，可以启用
- 首字母自动大写：默认情况下处于禁用状态，可以启用
- 文本预测：默认情况下处于禁用状态，可以启用


### <a name="emailsmtpaddress"></a>EmailSmtpAddress

`<TextBox InputScope="EmailSmtpAddress"/>`

![可输入电子邮件地址的 Windows 触摸键盘](images/input-scopes/emailsmtpaddress.png)
- 包括“@”  和“.com”  键。 按下并保持 **.com**键以显示其他选项 ( **.org**， **.net**，和特定于区域的后缀)
- 包括 **_** 并 **-** 密钥
- 拼写检查：默认情况下处于禁用状态，可以启用
- 自动更正：默认情况下处于禁用状态，可以启用
- 首字母自动大写：默认情况下处于禁用状态，可以启用
- 文本预测：默认情况下处于禁用状态，可以启用


### <a name="number"></a>编号

`<TextBox InputScope="Number"/>`

![可输入数字的 Windows 触摸键盘](images/input-scopes/number.png)
- 拼写检查：默认情况下处于启用状态，可以禁用
- 自动更正：默认情况下处于启用状态，可以禁用
- 首字母自动大写：始终处于禁用状态
- 文本预测：默认情况下处于启用状态，可以禁用

### <a name="telephonenumber"></a>TelephoneNumber

`<TextBox InputScope="TelephoneNumber"/>`

![可输入电话号码的 Windows 触摸键盘](images/input-scopes/telephonenumber.png)
- 拼写检查：默认情况下处于启用状态，可以禁用
- 自动更正：默认情况下处于启用状态，可以禁用
- 首字母自动大写：始终处于禁用状态
- 文本预测：默认情况下处于启用状态，可以禁用

### <a name="search"></a>搜索

`<TextBox InputScope="Search"/>`

![用于搜索的 Windows 触摸键盘](images/input-scopes/search.png)
- 包括**搜索**而不是关键**Enter**密钥
- 拼写检查：默认情况下处于启用状态，可以禁用
- 自动更正：默认情况下处于启用状态，可以禁用
- 首字母自动大写：始终处于禁用状态
- 文本预测：默认情况下处于启用状态，可以禁用

### <a name="searchincremental"></a>SearchIncremental

`<TextBox InputScope="SearchIncremental"/>`

![Windows 触摸键盘的渐进式搜索](images/input-scopes/searchincremental.png)
- 与相同的布局**默认**
- 拼写检查：默认情况下处于禁用状态，可以启用
- 自动更正：始终处于禁用状态
- 首字母自动大写：始终处于禁用状态
- 文本预测：始终处于禁用状态

### <a name="formula"></a>公式

`<TextBox InputScope="Formula"/>`

![Windows 触摸键盘的公式](images/input-scopes/formula.png)
- 包括 **=** 密钥
- 此外包括 **%** ， **$** ，以及 **+** 密钥
- 拼写检查：默认情况下处于启用状态，可以禁用
- 自动更正：默认情况下处于启用状态，可以禁用
- 首字母自动大写：始终处于禁用状态
- 文本预测：默认情况下处于启用状态，可以禁用

### <a name="chat"></a>Chat

`<TextBox InputScope="Chat"/>`

![默认 Windows 触摸键盘](images/input-scopes/default.png)
- 与相同的布局**默认**
- 拼写检查：默认情况下处于启用状态，可以禁用
- 自动更正：默认情况下处于启用状态，可以禁用
- 首字母自动大写：默认情况下处于启用状态，可以禁用
- 文本预测：默认情况下处于启用状态，可以禁用

### <a name="nameorphonenumber"></a>NameOrPhoneNumber

`<TextBox InputScope="NameOrPhoneNumber"/>`

![默认 Windows 触摸键盘](images/input-scopes/default.png)
- 与相同的布局**默认**
- 拼写检查：默认情况下处于禁用状态，可以启用
- 自动更正：默认情况下处于禁用状态，可以启用
- 自动大写： 关闭默认情况下，可以启用 （每个单词的首字母大写）
- 文本预测：默认情况下处于禁用状态，可以启用
