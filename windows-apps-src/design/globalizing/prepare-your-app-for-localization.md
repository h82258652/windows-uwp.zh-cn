---
Description: A localized app is one that can be localized to other markets, languages, or regions without uncovering any functional defects in the app. The most essential property of a localizable app is that its executable code has been cleanly separated from its localizable resources.
title: 对应用进行可本地化处理
ms.assetid: 06E1D4BB-59EA-4D71-99AC-7CB93D2A58A7
template: detail.hbs
ms.date: 11/07/2017
ms.topic: article
keywords: windows 10, uwp, 全球化, 可本地化性, 本地化
ms.localizationpriority: medium
ms.openlocfilehash: c0df06458bf70599be657fe2812b2fb3e2b44ed6
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2018
ms.locfileid: "8898935"
---
# <a name="make-your-app-localizable"></a>对应用进行可本地化处理

本地化应用是一种可针对其他市场、语言或地区进行本地化且未发现应用中的任何功能性缺陷的应用。 可本地化应用最重要的属性是其可执行代码与其可本地化资源完全分隔。 所以，你应确定哪些应用的资源需要进行本地化。 问问你自己，如果应用要针对其他市场进行本地化，需要作出哪些更改？

我们还建议你熟悉[全球化指南](guidelines-and-checklist-for-globalizing-your-app.md)。

## <a name="put-your-strings-into-resources-files-resw"></a>将字符串置于资源文件 (.resw) 中

请不要对强制性代码、XAML 标注或应用程序包清单中的字符串文本进行硬编码。 相反，将字符串放入资源文件 (.resw) 中，以便它们适合独立于你应用的生成二进制文件的不同本地市场。 有关详细信息，请参阅[本地化 UI 和应用程序包清单中的字符串](../../app-resources/localize-strings-ui-manifest.md)。

该主题还展示如何向你的默认资源文件 (.resw) 中添加注释。 例如，如果你要采用非正式语音或语调，请确保在注释中解释此情况。 此外，为最大程度降低费用，请确认仅向翻译人员提供需要翻译的字符串。

在应用程序包清单源文件（`Package.appxmanifest` 文件）中适当地设置应用的默认语言。 默认语言确定当用户的首选语言不匹配应用的所有支持语言时要使用的语言。 使用语言（甚至是默认语言中的某种语言，例如 `\Assets\en-us\Logo.png`）标记所有资源，以便系统可以通知资源所使用的是哪种语言以及在特定情况下如何使用该语言。

## <a name="tailor-your-images-and-other-file-resources-for-language"></a>针对语言定制图像和其他文件资源

理想情况下，你将能够对图像进行全球化&mdash;让其与特定文化无关。 对于不能进行全球化的任何图像和其他文件资源，根据需要创建它们的多个不同变体，并将相应语言限定符置于它们的文件或文件夹名称中。 若要了解详细信息，请参阅[针对语言、缩放、高对比度和其他限定符定制资源](../../app-resources/tailor-resources-lang-scale-contrast.md)。

为了最大程度降低本地化成本，请不要将文字或文化敏感性材料置于开始的图像。 适合你自己文化的图像可能在其他文化中具有冒犯性或会被曲解。 避免使用文化特定的图像，例如世界上不通用的邮箱。 避免使用特定于宗教符号、动物、政治或性别的图像。 展示肉体、身体部位或手势也可能是敏感主题。 如果无法避免所有这些，则需要细致周到地进行图像本地化。 如果要本地化为与你本身语言的读取方向不同的语言，请使用对称图像和效果，以便可以更容易地支持镜像。

另请避免在图像中使用文字，以及在音频/视频文件中使用语音。

## <a name="the-use-of-color-in-your-app"></a>在应用中使用颜色

使用颜色时请注意。 使用与国旗或政治运动关联的颜色组合可能有问题。 颜色选择可能需要由文化专家进行审阅。 使用颜色还存在辅助功能问题。 如果使用颜色传达含义，则还应通过其他方法（如大小、形状或标签）传达相同信息。

## <a name="consider-factoring-your-strings-into-sentences"></a>在句子中考虑字符串因素

使用大小适当的字符串。 短字符串更易于翻译，且可循环利用其翻译（这可节省费用，因为不会将相同字符串多次发送给本地化人员）。 此外，本地化工具可能不支持过长的字符串。

但是，与本指南相冲突的是会产生在不同上下文中重复使用某一字符串的风险。 即使是 &quot;on&quot; 和 &quot;off&quot; 等简单字词也可能会基于上下文产生不同的翻译结果。 在英语中，“on”和“off”可用于飞行模式、蓝牙和设备的切换。 但在意大利语中，翻译取决于要打开和关闭的内容的上下文。 可能需要为每个上下文创建一对字符串。 在两个上下文相同的情况下可以重复使用字符串。 例如，你可以针对声音效果音量和音乐音量重复使用字符串“Volume”，因为二者均指声音的强度。 但不应该在指硬盘卷时使用同一字符串，因为二者的上下文和含义是不同的，因此该字词可能会具有不同的翻译。

此外，诸如“text”或“fax”之类的字符串在英语中可用作动词和名词，在翻译过程中这可能会造成混淆。 因此，为动词和名词格式创建单独的字符串。 在不确定上下文是否相同时，为稳妥起见使用不同的字符串。

简而言之，将字符串分解为在所有上下文中均起作用的几个部分。 将出现需要将某一字符串作为整个句子的情况。

请考虑以下字符串:"{0}无法同步。"

大量的字词可能会替换{0}，如"约会"、"任务"或"文档"。 虽然此示例适用于英语，但它绝不适用于德语等的相应语句。 请注意以下德语语句，模板字符串中的某些字词（“Der”、“Die”、“Das”）需要与参数化的字词匹配：

| 英语                                    | 德语                                           |
|:------------------------------------------ |:------------------------------------------------ |
| The appointment could not be synchronized. | Der Termin konnte nicht synchronisiert werden.   |
| The task could not be synchronized.        | Die Aufgabe konnte nicht synchronisiert werden.  |
| The document could not be synchronized.    | Das Dokument konnte nicht synchronisiert werden. |

在另一个示例中，考虑语句"提醒我在{0}分钟。" “minute(s)”适用于英语，但其他语言可能会使用不同的术语。 例如，波兰语使用“minuta”、“minuty”或“minut”，具体取决于上下文。

若要解决此问题，应本地化整个语句，而不应只本地化单个字词。 这么做看似增加了额外工作量且是个不明智的解决方案，但其实是最佳解决方案，原因如下：

-   会针对所有语言显示一个语法正确的消息。
-   翻译无需询问该字符串将会被什么词替代。
-   当应用完成后出现类似于此图面的问题时，也无需实施高成本的代码修复。

## <a name="other-considerations-for-strings"></a>字符串的其他注意事项

避免在以默认语言编写的字符串中使用俗语或隐喻。 特定于某种人群的语言（如文化和年龄）很难理解或翻译，因为只有属于该类人群的人才会使用该语言。 同样，比喻可能对于一个人而言有意义，而对其他人而言没有意义。 例如，&quot;蓝鸟&quot;对于了解滑雪文化的人而言表示特定意义，但对于不了解该文化的人而言则没有意义。

请勿使用技术行话、缩写或缩略语。 技术语言很有可能不会被来自其他文化或区域的非技术用户或人员所理解，并且也很难翻译。 人们在日常对话中不会采用这样的说法。 技术语言通常显示在识别硬件和软件问题的错误消息中，但在*仅当用户需要此级别的信息，并且可以对其进行操作或找到可以对其进行操作的人*时才应将字符串化为技术语言。

在字符串中使用非正式语音或语调是一种有效的选择。 可以在默认资源文件 (.resw) 中使用注释来表明该目的。

## <a name="pseudo-localization"></a>伪本地化

伪本地化你的应用，找出任何本地化问题。 伪本地化是一种本地化预演或问题揭露测试。 生成一组未真正翻译的资源；它们看起来就是那样子的。 例如，你的字符串大约比默认语言长 40%，并且字符串中具有分隔符，以便你可以一眼就看出它们在 UI 中是否被截断。

## <a name="geopolitical-awareness"></a>地缘政治意识

在地图中或在涉及地理区域时避免政治冒犯。 地图可能包含有争议的地理区域或国界，并且这是引起政治冲突的常见原因。 请务必小心用于选择国家/地区的任何 UI，应将其称为“国家/地区”。 将有争议的领土列于标记为“国家/地区”的列表（例如地址表）中可能会冒犯某些用户。

## <a name="language--and-region-changed-events"></a>更改了语言和地区的事件

订阅在系统的语言和地区设置更改时引发的事件。 执行此操作，以便在适当情况下可以重新加载资源。 有关详细信息，请参阅[响应限定符值更改事件更新字符串](../../app-resources/localize-strings-ui-manifest.md#updating-strings-in-response-to-qualifier-value-change-events)和[响应限定符值更改事件更新图像](../../app-resources/images-tailored-for-scale-theme-contrast.md#updating-images-in-response-to-qualifier-value-change-events)。

## <a name="ensure-the-correct-parameter-order-when-formatting-strings"></a>设置字符串格式时，请确保正确的参数顺序

不要假设所有语言都以相同顺序表示参数。 例如，请考虑以下格式。

```csharp
    string.Format("Every {0} {1}", monthName, dayNumber); // For example, "Every April 1".
```

此示例中的格式字符串适用于英语(美国)。 但不适用于德语(德国)，例如，其中的日期和月份以相反顺序显示。 确保 translator 知道每个参数的意图，以便它们可以反向格式中的项顺序的格式字符串 (例如，"{1} {0}") 为适合于目标语言。

## <a name="dont-over-localize"></a>不要过度本地化

仅向翻译提供自然语言；不要提供编程语言或标注。 `<link>` 标记不是自然语言。 看看下面的示例。

| 不要将其进行本地化                   | 将其进行本地化 |
|:--------------------------------------- |:-------------------------- |
| &lt;link&gt;使用条款&lt;/link&gt;   | 使用条款               |
| &lt;link&gt;隐私策略&lt;/link&gt; | 隐私策略             |

在资源文件 (.resw) 中包括 `<link>` 标记也意味着很有可能要进行翻译。 这将导致标记无效。 如果具有需要包括标注以便维护上下文并确保排序的长字符串，请在注释中明确说明无需进行翻译的内容。

## <a name="choose-an-appropriate-translation-approach"></a>选择适当的翻译方法

在字符串分为资源文件后，就可以对其进行翻译了。 翻译字符串的理想时间为在项目中的字符串敲定后，这通常发生在项目的后期。 可以采用多种方法完成翻译流程。 这可能要取决于待翻译的字符串的量、待翻译的语言数以及如何完成翻译（例如，内部翻译或雇用外部供应商）。

不妨请考虑以下几个选项。

- **直接在项目中打开资源文件，即可对其进行翻译。** 对于需要翻译成两种或三种语言的字符串数量较少的项目，此方法比较适用。 在开发人员使用多种语言并且愿意处理翻译过程的情况下，可以使用这种方法。 这种方法的优势在于快速、无需工具并且误译的风险最小。 但这种方法不可扩展。 特别是，不同语言中的资源很容易不同步，这会导致不好的用户体验和维护困难。
- **字符串资源文件采用 XML 或 ResJSON 文本格式，因此可以使用任何文本编辑器交付它们以供翻译。 然后，再将已翻译的文件复制回项目中。** 此方法存在翻译人员意外编辑 XML 标记的风险，但它允许在 Microsoft Visual Studio 项目外进行翻译工作。 对于需要翻译成少数几种语言的项目，此方法可能比较适用。 XLIFF 格式是专门用于本地化的 XML 格式，应该可以很好地受到一些本地化供应商或本地化工具的支持。 你可以使用[多语言应用工具包](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/jj572370.aspx)从其他资源文件中（如 .resw 或 .resjson）生成 XLIFF 文件。

对于其他文件（例如，图像或音频文件），可能需要交付给本地化人员。

此外，请考虑以下建议。

- **使用本地化工具。** 有很多本地化工具可用于解析资源文件，并仅允许翻译人员编辑可翻译的字符串。 这种方法减少了翻译人员意外编辑 XML 标记的风险。 但它的缺点是向本地化流程中引入了新的工具和流程。 本地化工具适合具有大量字符串但需要翻译为少数语言的项目。 若要了解详细信息，请参阅[如何使用多语言应用工具包](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/jj572370.aspx)。
- **使用本地化供应商。** 如果项目包含大量字符串并需要翻译为多种语言，请考虑使用本地化供应商。 本地化供应商可提供有关工具和流程的建议，并可翻译你的资源文件。 这是一种理想的解决方案，但也是花费最大的选项，并且会增加翻译内容的检查时间。

## <a name="keep-access-keys-and-labels-consistent"></a>使访问键和标签保持一致

将用于辅助功能的访问键与本地化的访问键的显示“同步”比较困难，因为这两个字符串资源被分类为两个单独的部分。 请确保为标签字符串提供注释，如： `Make sure that the emphasized shortcut key  is synchronized with the access key.`

## <a name="support-furigana-for-japanese-strings-that-can-be-sorted"></a>支持可进行排序的日语字符串的假名注音

日语汉字字符具有根据字词和使用它们的上下文具有多个发音的属性。 这在尝试排序日语命名对象（例如，应用程序名称、文件、歌曲等）时会引发问题。 以前，日语日文汉字通常采用名为 XJIS 的计算机可理解的顺序进行排序。 但是，由于此排序顺序是非注音的，因此使用并不人性化。

*汉字注音*允许用户或创建者指定所使用字符的注音，因而可解决此问题。 如果使用以下过程向应用名称添加假名注音，则可以确保该名称排序在应用列表的适当位置。 如果应用名称包含日文汉字字符并且没有提供假名注音，则当用户的 UI 语言或排序顺序设置为日语时，Windows 将尽量生成适当的发音。 然而，也可能会依据较常见的读音对包含少见或独特读音的应用名称进行排序。 因此，用于日语应用程序（尤其是名称中包含日文汉字字符的应用程序）的最佳做法是：在日语本地化过程中提供其应用名称的汉字注音版本。

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

## <a name="related-topics"></a>相关主题

* [全球化指南](guidelines-and-checklist-for-globalizing-your-app.md)
* [对 UI 和应用包清单中的字符串实施本地化](../../app-resources/localize-strings-ui-manifest.md)
* [定制语言、比例、高对比度和其他限定符的资源](../../app-resources/tailor-resources-lang-scale-contrast.md)
* [调整布局和字体并支持 RTL](adjust-layout-and-fonts--and-support-rtl.md)
* [响应限定符值更改事件更新图像](../../app-resources/images-tailored-for-scale-theme-contrast.md#updating-images-in-response-to-qualifier-value-change-events)

## <a name="samples"></a>示例

* [应用程序资源和本地化示例](http://go.microsoft.com/fwlink/p/?linkid=254478)
