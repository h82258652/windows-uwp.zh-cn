---
author: Jwmsft
Description: A text entry box that provides suggestions as the user types.
title: 自动建议框指南
ms.assetid: 1F608477-F795-4F33-92FA-F200CC243B6B
dev.assetid: 54F8DB8A-120A-4D79-8B5A-9315A3764C2F
label: Auto-suggest box
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: miguelrb
design-contact: ksulliv
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: efb6c8a9324419ea099041f34c341152351575f6
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2018
ms.locfileid: "5815198"
---
# <a name="auto-suggest-box"></a>自动建议框

使用 AutoSuggestBox 提供建议列表，以便用户在键入时从中进行选择。

> **重要 API**：[AutoSuggestBox 类](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestbox.aspx)、[TextChanged 事件](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestbox.textchanged.aspx)、[SuggestionChose 事件](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestbox.suggestionchosen.aspx)、[QuerySubmitted 事件](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestbox.querysubmitted.aspx)

![自动建议框](images/controls/auto-suggest-box-open.png)

## <a name="is-this-the-right-control"></a>这是正确的控件吗？

如果你希望允许进行文本搜索且附带建议列表的简单、可自定义的控件，请选择自动建议框。

有关选择正确文本控件的详细信息，请参阅 [文本控件](text-controls.md) 文章。

## <a name="examples"></a>示例

<table>
<th align="left">XAML 控件库<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果已安装 <strong style="font-weight: semi-bold">XAML 控件库</strong>应用，请单击此处<a href="xamlcontrolsgallery:/item/AutoSuggestBox">打开此应用，了解 AutoSuggestBox 的实际应用</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Groove 音乐应用中的自动建议框。

![Groove 音乐应用中的自动建议框](images/control-examples/auto-suggest-box-groove.png)

## <a name="anatomy"></a>结构
自动建议框的入口点由一个可选的标头和一个带有可选提示文本的文本框构成：

![自动建议控件的入口点示例](images/controls_autosuggest_entrypoint.png)

用户开始输入文本后，自动建议结果将自动列出填充内容。 结果列表可能出现在文本输入框的上方或下方。 将显示“清除所有”按钮：

![展开的自动建议控件示例](images/controls_autosuggest_expanded01.png)

## <a name="create-an-auto-suggest-box"></a>创建自动建议框

若要使用 AutoSuggestBox，你需要响应 3 个用户操作。

- 更改的文本 - 当用户输入文本时，更新建议列表。
- 选择的建议 - 当用户在建议列表中选择某个建议时，更新文本框。
- 提交的查询 - 当用户提交某个查询时，显示查询结果。

### <a name="text-changed"></a>更改的文本

每当更新文本框的内容时，都将引发 [TextChanged](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestbox.textchanged.aspx) 事件。 使用事件参数 [Reason](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestboxtextchangedeventargs.reason.aspx) 属性确定更改是否缘自用户输入。 如果更改原因是 **UserInput**，则根据输入筛选你的数据。 然后，将筛选的数据设置为 AutoSuggestBox 的 [ItemsSource](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemssource.aspx) 以更新建议列表。

若要控制项目在建议列表中的显示方式，你可以使用 [DisplayMemberPath](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.displaymemberpath.aspx) 或 [ItemTemplate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemtemplate.aspx)。

- 若要显示数据项的单一属性的文本，则设置 DisplayMemberPath 属性以选择对象中的哪个属性要显示在建议列表中。
- 若要为列表中的每个项目定义自定义外观，请使用 ItemTemplate 属性。

### <a name="suggestion-chosen"></a>选择的建议

当用户使用键盘导航浏览建议列表时，你需要更新文本框中的文本以进行匹配。

你可以设置 [TextMemberPath](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestbox.textmemberpath.aspx) 属性以选择数据对象中的哪个属性要显示在文本框中。 如果指定 TextMemberPath，文本框将自动更新。 通常应该为 DisplayMemberPath 和 TextMemberPath 指定相同的值，以便使建议列表和文本框中的文本相同。

如果你需要显示多个简单属性，请处理 [SuggestionChosen](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestbox.suggestionchosen.aspx) 事件以根据所选项目使用自定义文本填充文本框。

### <a name="query-submitted"></a>提交的查询

处理 [QuerySubmitted](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestbox.querysubmitted.aspx) 事件以执行适用于你的应用的查询操作，并向用户显示结果。

当用户提交查询字符串时，将发生 QuerySubmitted 事件。 用户可以采用以下方式之一提交查询：
- 当焦点位于文本框中时，按 Enter 或单击查询图标。 事件参数 [ChosenSuggestion](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestboxquerysubmittedeventargs.chosensuggestion.aspx) 属性是 **null**。
- 当焦点位于建议列表中时，按 Enter，单击或点击某个项目。 事件参数 ChosenSuggestion 属性包含已从列表中选择的项目。

在所有情况下，事件参数 [QueryText](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestboxquerysubmittedeventargs.querytext.aspx) 属性都包含文本框中的文本。

下面是带有所需事件处理程序的简单 AutoSuggestBox。

```xaml
<AutoSuggestBox PlaceholderText="Search" QueryIcon="Find" Width="200"
                TextChanged="AutoSuggestBox_TextChanged"
                QuerySubmitted="AutoSuggestBox_QuerySubmitted"
                SuggestionChosen="AutoSuggestBox_SuggestionChosen"/>
```

```csharp
private void AutoSuggestBox_TextChanged(AutoSuggestBox sender, AutoSuggestBoxTextChangedEventArgs args)
{
    // Only get results when it was a user typing,
    // otherwise assume the value got filled in by TextMemberPath
    // or the handler for SuggestionChosen.
    if (args.Reason == AutoSuggestionBoxTextChangeReason.UserInput)
    {
        //Set the ItemsSource to be your filtered dataset
        //sender.ItemsSource = dataset;
    }
}


private void AutoSuggestBox_SuggestionChosen(AutoSuggestBox sender, AutoSuggestBoxSuggestionChosenEventArgs args)
{
    // Set sender.Text. You can use args.SelectedItem to build your text string.
}


private void AutoSuggestBox_QuerySubmitted(AutoSuggestBox sender, AutoSuggestBoxQuerySubmittedEventArgs args)
{
    if (args.ChosenSuggestion != null)
    {
        // User selected an item from the suggestion list, take an action on it here.
    }
    else
    {
        // Use args.QueryText to determine what to do.
    }
}
```

## <a name="use-autosuggestbox-for-search"></a>使用 AutoSuggestBox 进行搜索

使用 AutoSuggestBox 提供建议列表，以便用户在键入时从中进行选择。

默认情况下，文本输入框中不会显示查询按钮。 你可以设置 [QueryIcon](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestbox.queryicon.aspx) 属性以在文本框的右侧添加附带指定图标的按钮。 例如，若要使 AutoSuggestBox 看起来像一个典型的搜索框，可添加一个“查找”图标，如下所示。

```xaml
<AutoSuggestBox QueryIcon="Find"/>
```

下面是附带“查找”图标的 AutoSuggestBox。

![自动建议控件的入口点示例](images/controls_autosuggest_entrypoint.png)

## <a name="dos-and-donts"></a>应做事项和禁止事项

-   在使用自动建议框执行搜索并且输入的文本不存在任何搜索结果时，请显示一行“没有结果”的消息作为结果，以便用户知道已执行了他们的搜索请求：

    ![无任何搜索结果的自动建议框示例](images/controls_autosuggest_noresults.png)

<!--
<div class="microsoft-internal-note">
**Globalization and localization checklist**

<table>
<tr>
<th>Vertical spacing</th><td>Use non-Latin characters for vertical spacing to ensure non-Latin scripts will display properly, including numbers.</td>
</tr>
<tr>
<th>Scrolling</th><td>When auto suggest text is selected, user should be able to scroll to end of string.</td>
</tr>
</table>
</div>
-->

## <a name="get-the-sample-code"></a>获取示例代码

- [XAML 控件库示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 以交互式格式查看所有 XAML 控件。
- [AutoSuggestBox 示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlAutoSuggestBox)

## <a name="related-articles"></a>相关文章

- [文本控件](text-controls.md)
- [拼写检查](text-controls.md)
- [搜索](search.md)
- [TextBox 类](https://msdn.microsoft.com/library/windows/apps/br209683)
- [Windows.UI.Xaml.Controls PasswordBox 类](https://msdn.microsoft.com/library/windows/apps/br227519)
- [String.Length 属性](https://msdn.microsoft.com/library/system.string.length.aspx)
