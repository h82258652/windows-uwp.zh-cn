---
author: DelfCo
Description: 准备将应用本地化到其他市场、语言或地区。
title: 准备将应用本地化
ms.assetid: 06E1D4BB-59EA-4D71-99AC-7CB93D2A58A7
label: Prepare your app for localization
template: detail.hbs
---

# 准备将应用本地化





准备将应用本地化到其他市场、语言或地区。 在开始使用前，请务必阅读[应做事项和禁止事项](guidelines-and-checklist-for-globalizing-your-app.md)。

## <span id="use_resource_files_and_qualifiers."></span><span id="USE_RESOURCE_FILES_AND_QUALIFIERS."></span>使用资源文件和限定符。


请确保在资源文件中指定你的应用的 UI 字符串，而不是将其放置在代码中。 有关详细信息，请参阅[将 UI 字符串放入资源中](put-ui-strings-into-resources.md)。

在其文件或文件夹中指定带有相应语言标记的图像或其他文件资源。 请注意，本地化图像、音频和视频需要占用大量的系统资源，因此最好是在可能的情况下尽量使用中性媒体资产。 若要了解详细信息，请参阅[如何使用限定符命名资源](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324)。

## <span id="add_contextual_comments."></span><span id="ADD_CONTEXTUAL_COMMENTS."></span>添加上下文注释。


将本地化注释添加到应用资源文件。 本地化人员可以看到注释，并且注释应提供用于帮助本地化人员准确翻译资源的上下文信息。 注释还应提供有关资源的充足约束信息，以便翻译不会破坏软件。 可使用 Makepri.exe 工具对注释进行日志记录（可选）。

**XAML：**Resw 文件（在 Visual Studio 中为使用 XAML 的应用创建的资源）具有注释元素。 例如：

```XML
<data name="String1">
    <value>Hello World</value>
    <comment>A greeting (This is a comment to the localizer)</comment>
</data>
```

**HTML：**Resjson 文件（在 Visual Studio 中为使用 HTML 的应用创建的资源）允许在以下划线开头的字段中使用元数据，如注释：

```json
{
    "String1"  : "Hello World",
    "_String1.comment" : "A greeting (This is a comment to the localizer)"
}
```

## <span id="localize_sentences_instead_of_words."></span><span id="LOCALIZE_SENTENCES_INSTEAD_OF_WORDS."></span>本地化句子而不是字词。


请考虑以下字符串：“{0} 无法进行同步。”

大量的字词都可能会替换 {0}，如约会、任务或文档。 虽然此示例似乎适用于英语，但它绝不适用于德语的相应语句。 请注意以下德语语句，模板字符串中的某些字词（“Der”、“Die”、“Das”）需要与参数化的字词匹配：

| 英语                                    | 德语                                           |
|:------------------------------------------ |:------------------------------------------------ |
| The appointment could not be synchronized. | Der Termin konnte nicht synchronisiert werden.   |
| The task could not be synchronized.        | Die Aufgabe konnte nicht synchronisiert werden.  |
| The document could not be synchronized.    | Das Dokument konnte nicht synchronisiert werden. |

 

在另一个示例中，考虑语句“在 {0} 分钟后提醒我。” 虽然“minute(s)”适用于英语，但其他语言可能会使用不同的术语。 例如，波兰语使用“minuta”、“minuty”或“minut”，具体取决于上下文。

若要解决此问题，应本地化整个语句，而不应只本地化单个字词。 这么做看似增加了额外工作量且是个不明智的解决方案，但其实是最佳解决方案，原因如下：

-   会针对所有语言显示一个清晰明确的错误消息。
-   本地化人员无需询问该字符串将会被什么词替代。
-   当应用完成后出现类似于此界面的问题时，也无需实施高成本的代码修复。

## <span id="ensure_the_correct_parameter_order."></span><span id="ENSURE_THE_CORRECT_PARAMETER_ORDER."></span>确保正确的参数顺序。


不要假设所有语言都以相同顺序使用参数。 例如，考虑字符串“Every %s %s”，其中第一个 %s 将由月份的名称替代，而第二个 %s 将有月份的日期替代。 此示例适用于英语，但在应用本地化为德语时会遇到问题，其中日期和月份是按相反顺序显示的。

若要解决此问题，请将字符串更改为“Every %1 %2”，以便顺序根据语言进行交换。

## <span id="don_t_over_localize."></span><span id="DON_T_OVER_LOCALIZE."></span>不要过度本地化。


本地化特定字符串，而不是标记。 请考虑以下示例：

| 过度本地化的字符串                   | 正确本地化的字符串 |
|:--------------------------------------- |:-------------------------- |
| &lt;link&gt;使用条款&lt;/link&gt;   | 使用条款               |
| &lt;link&gt;隐私策略&lt;/link&gt; | 隐私策略             |

 

在资源中包括以上 &lt;link&gt; 标记表示它也将被本地化。 这样会使标记无效。 应仅本地化字符串自身。 通常，应将标记视为应与可本地化内容分离的代码。 但是，某些长字符串应包含标记以保留上下文并确保顺序。

## <span id="do_not_use_the_same_strings_in_dissimilar_contexts."></span><span id="DO_NOT_USE_THE_SAME_STRINGS_IN_DISSIMILAR_CONTEXTS."></span>不要在不同的上下文中使用相同的字符串。


重复使用字符串看起来像是最佳的解决方案，但如果同一字词或短语具有不同的含义和上下文，则会导致本地化问题。

在两个上下文相同的情况下可以重复使用字符串。 例如，你可以针对声音效果音量和音乐音量重复使用字符串“Volume”，因为二者均指声音的强度。 但不应该在指硬盘卷时使用同一字符串，因为二者的上下文和含义是不同的，因此该字词可能会具有不同的翻译。

另一示例是字符串“on”和“off”的使用。 在英语中，“on”和“off”可用于飞行模式、蓝牙和设备的切换。 但在意大利语中，翻译取决于要打开和关闭的内容的上下文。 可能需要为每个上下文创建一对字符串。

此外，诸如“text”或“fax”之类的字符串在英语中可用作动词和名词，在翻译过程中这可能会造成混淆。 因此，为动词和名词格式创建单独的字符串。 在不确定上下文是否相同时，应稳妥起见使用不同的字符串。

## <span id="identify_resources_with_unique_attributes."></span><span id="IDENTIFY_RESOURCES_WITH_UNIQUE_ATTRIBUTES."></span>使用唯一属性标识资源。


资源标识符区分大小写，并且对于每个资源文件应是唯一的。 在访问一个资源时，应使用资源标识符，而不是实际的资源值。 资源标识符不会更改，但实际的资源值会根据语言进行更改。

请确保使用有意义的资源标识符，为翻译提供额外的上下文。

请勿在字符串资源送去翻译后更改资源标识符。 本地化团队使用资源标识符来跟踪资源内的添加、删除和更新。 资源标识符中的更改（也称为“资源标识符转换”）需要翻译字符串，因为尽管字符串可能会删除并且会添加其他字符串，但会显示这些字符串。

## <span id="choose_an_appropriate_translation_approach."></span><span id="CHOOSE_AN_APPROPRIATE_TRANSLATION_APPROACH."></span>选择相应的翻译方法。


在字符串分为资源文件后，就可以对其进行翻译了。 翻译字符串的理想时间为在项目中的字符串敲定后，这通常发生在项目的后期。 可以采用多种方法完成翻译流程。 这可能要取决于待翻译的字符串的量、待翻译的语言数以及如何完成翻译（例如，内部翻译或雇用外部供应商）。

请考虑以下选项：

-   **直接在项目中打开资源文件，即可对其进行翻译。** 对于字符串数量较少而且需要翻译成两种或三种语言的项目，此方法比较适用。 在开发人员使用多种语言并且愿意处理翻译过程的情况下，可以使用这种方法。 这种方法的优势在于快速、无需工具并且误译的风险最小，但这种方法不可扩展。 特别是，不同语言中的资源很容易不同步，这会导致不好的用户体验和维护困难。
-   **字符串资源文件采用 XML 或 ResJSON 文本格式，因此可以使用任何文本编辑器交付它们以供翻译。 然后，再将已翻译的文件复制回项目中。** 此方法存在翻译人员意外编辑 XML 标记的风险，但它允许在 Microsoft Visual Studio 项目外进行翻译工作。 对于需要翻译成少数几种语言的项目，此方法可能比较适用。 XLIFF 格式是专门用于本地化的 XML 格式，应该可以很好地受到一些本地化供应商或本地化工具的支持。 你可以使用[多语言应用工具包](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/jj572370.aspx)从其他资源文件中（如 .resw 或 .resjson）生成 XLIFF 文件。

对于其他文件（例如，图像或音频文件），可能需要交付给本地化人员。 通常，我们不建议创建依赖于文化的文件，因为这会给进行本地化带来困难。

此外，请考虑以下建议：

-   **使用本地化工具。** 有很多本地化工具可用于解析资源文件，并仅允许翻译人员编辑可翻译的字符串。 这种方法减少了翻译人员意外编辑 XML 标记的风险。 但它的缺点是向本地化流程中引入了新的工具和流程。 本地化工具适合具有大量字符串但需要翻译为少数语言的项目。 若要了解详细信息，请参阅[如何使用 多语言应用工具包](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/jj572370.aspx)。
-   **使用本地化供应商。** 如果项目包含大量字符串并需要翻译为多种语言，请考虑使用本地化供应商。 本地化供应商可提供有关工具和流程的建议，并可翻译你的资源文件。 这是一种理想的解决方案，但也是花费最大的选项，并且会增加翻译内容的检查时间。
-   **使本地化人员了解相关信息。** 通知本地化人员有关可视为名词或动词的字符串。 使用术语工具向本地化人员解释生造词。 尽量使字符串在语法上正确、无歧义，并尽量使用非技术术语以避免混淆。

## <span id="keep_access_keys_and_labels_consistent."></span><span id="KEEP_ACCESS_KEYS_AND_LABELS_CONSISTENT."></span>使访问键和标签保持一致。


将用于辅助功能的访问键与本地化的访问键的显示“同步”比较困难，因为这两个字符串资源被分类为两个单独的部分。 请确保为标签字符串提供注释，如： `Make sure that the emphasized shortcut key  is synchronized with the access key.`

**HTML：**

你可以按照下面显示的实现进行操作。 再一次，请确保正确注释标签字符串以将其链接到访问键定义。

```HTML
<label id="theLabel" data-win-res="{accessKey: 'theLabelAccessKey'}" for="xPrinterRedirection" accessKey="L">The <u>L</u>abel</label>
<input type="checkbox" value="OFF" id="xPrinterRedirection" name="xPrinterRedirection" />
```

## <span id="support_furigana_for_japanese_strings_that_can_be_sorted."></span><span id="SUPPORT_FURIGANA_FOR_JAPANESE_STRINGS_THAT_CAN_BE_SORTED."></span>支持可存储的日语字符串的假名注音。


日语汉字字符具有独特的属性，即根据字词和使用它们的上下文具有多个发音。 这在尝试排序日语命名对象（例如，应用程序名称、文件、歌曲等）时会引发问题。 以前，日语日文汉字通常采用名为 XJIS 的计算机可理解的顺序进行排序。 但是，由于此排序顺序是非注音的，因此使用并不人性化。

汉字注音允许用户或创建者指定所使用字符的注音，因而可解决此问题。 如果使用以下过程向应用名称添加假名注音，则可以确保该名称排序在应用列表的适当位置。 如果应用名称包含日文汉字字符并且没有提供假名注音，则当用户的 UI 语言或排序顺序设置为日语时，Windows 将尽量生成适当的发音。 然而，也可能会依据较常见的读音对包含少见或独特读音的应用名称进行排序。 因此，用于日语应用程序（尤其是名称中包含日文汉字字符的应用程序）的最佳做法是：在日语本地化过程中提供其应用名称的汉字注音版本。

1.  添加“ms-resource:Appname”作为程序包显示名称和应用程序显示名称。
2.  在字符串下创建 ja-JP 文件夹并添加两个资源文件，如下所示：

    ``` syntax
    strings\
        en-us\
        ja-jp\
            Resources.altform-msft-phonetic.resw
            Resources.resw
    ```

3.  在用于常规 ja-JP 的 Resources.resw 中：添加用于应用名称“希蒼”的字符串资源
4.  在用于日语汉字注音资源的 Resources.altform-msft-phonetic.resw 中：添加用于应用名称“のあ”的汉字注音值

用户可以搜索应用名称“希蒼”，方法是使用汉字注音值“のあ”(noa)，也可以使用注音值（通过输入法编辑器 (IME) 使用 **GetPhonetic** 功能）“まれあお”(mare-ao)。

按照**区域控制面板**格式排序：

-   在日语用户区域设置下，
    -   如果启用汉字注音，则“希蒼”排序在“の”下。
    -   如果没有汉字注音，则“希蒼”排序在“ま”下。
-   在非日语用户区域设置下，
    -   如果启用汉字注音，则“希蒼”排序在“の”下。
    -   如果没有汉字注音，则“希蒼”排序在“漢字”下。

## <span id="related_topics"></span>相关主题


* [全球化和本地化的应做事项和禁止事项](guidelines-and-checklist-for-globalizing-your-app.md)
* [将 UI 字符串放入资源中](put-ui-strings-into-resources.md)
* [如何使用限定符命名资源](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324)
 

 





<!--HONumber=May16_HO2-->


