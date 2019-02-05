---
Description: This topic provides answers to frequently-asked questions and issues related to the Multilingual App Toolkit (MAT) 4.0.
title: 多语言应用工具包常见问题解答
template: detail.hbs
ms.date: 11/13/2017
ms.topic: article
keywords: windows 10, uwp, 全球化, 可本地化性, 本地化
ms.localizationpriority: medium
ms.openlocfilehash: 2e27256fbf19ed31a7b087e94dea9e5514db516f
ms.sourcegitcommit: bf600a1fb5f7799961914f638061986d55f6ab12
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/05/2019
ms.locfileid: "9050590"
---
# <a name="multilingual-app-toolkit-40-faq--troubleshooting"></a>多语言应用工具包 4.0 常见问题和疑难解答

本主题提供有关多语言应用工具包 (MAT) 4.0 的常见问题解答。

另请参阅[使用多语言应用工具包 4.0](use-mat.md)。

**请注意**此工具包支持 .resw (XAML) 和 .resjson (JavaScript) 文件。 但本主题将仅涉及 .resw 文件。 .resw 文件叫做资源文件。 它包含默认语言或翻译为其他语言的字符串。 包含 .resw 文件的文件夹通常以语言标记的值命名。

## <a name="do-i-need-resw-files-in-multiple-languages"></a>我是否需要多种语言的 .resw 文件？

不需要。 此工具包的关键优势之一是无需多种语言的 .resw 文件。 工具包通过使用 .xlf 文件管理并同步你的应用的资源。 这将消除保持跨多个 .resw 文件同步内容的难题。

包含匹配的 .resw 和 .xlf 文件的项目将导致忽略 .xlf 文件中的译文。 如果发生此情况，则在构建时将显示一条警告，告诉你在最终应用中未包含 .xlf 译文。 当 .resw 文件和 .xlf 文件具有包含相同语言代码的目标语言时，这两种文件相匹配。 一个匹配的配对示例为 `Strings\de-DE\Resources.resw` 和 `<project-name>.de-DE.xlf` 文件（包含 `target-language="de-DE"`）。

## <a name="can-i-have-resw-files-in-multiple-languages"></a>我是否可以具有多种语言的 .resw 文件？

可以，但我们不建议这样做。 如果想要在项目中包含多种语言的 .resw 文件和使用此工具包，请确保没有匹配的 .resw 和 .xlf 文件。

## <a name="i-dont-see-an-option-in-the-tools-menu-to-enable-the-multilingual-app-toolkit"></a>我未在工具菜单中看到启用多语言应用工具包的选项

请尝试以下步骤。

- 请确保选择项目节点而不是解决方案节点，然后再打开**工具**菜单。
- 确认使用 Visual Studio 扩展管理器安装了工具包扩展。
- 确认你的项目为 UWP 项目。

## <a name="when-i-build-my-project-i-dont-see-a-message-saying-that-a-multilingual-app-toolkit-build-has-started"></a>在生成项目时，我未看到已启动多语言应用工具包生成的消息

确认你已针对项目启用了 MAT。 在**工具**菜单上，选择**多语言应用工具包** > **启用选择**。 如果使用之前版本启用项目，请使用**工具**菜单禁用重新启用 MAT。 这将更新项目，以使用新的工具包版本进行处理。

确保已安装“针对所有 Visual Studio 版本生成任务”组件。 此生成组件与扩展一起安装，但可在安装期间手动取消选择扩展。 此组件需要更新 .xlf 文件，并将译文添加到 PRI 文件。 在此组件已安装并正常工作后，你将看到这些生成消息。

```dosbatch
1> Multilingual App Toolkit build started.
1> Multilingual App Toolkit build completed successfully.
```

## <a name="the-toolkit-is-reporting-that-it-didnt-locate-any-xliff-language-files-during-the-build"></a>工具包报告在生成期间无法找到任何 XLIFF 语言文件

```dosbatch
No XLIFF language files were found. The app will not contain any localized resources.
```

当工具包无法在扩展名为 .xlf 的项目中找到任何文件时，将显示此消息。 默认情况下，工具包将生成这些文件，并将其保存在 `MultilingualResources` 文件夹中。 可以移动这些文件；但最好将其保留在该文件夹中，因为这将允许多语言编辑器查找相关的元数据文件。

## <a name="my-xlf-file-is-not-included-in-the-list-of-files-processed-by-the-toolkit-during-build"></a>在生成期间工具包处理的文件的列表中不包括我的 .xlf 文件

如果先从项目中手动排除 .xlf 文件，然后再重新包括该文件，则文件类型元素设置可能不正确。 在 Visual Studio 中，选择文件，然后选中属性窗口。 文件的生成操作应设置为 XliffResource，复制到输出目录应设置为不要复制。 这是参考应在项目文件中的显示方式。

```xml
<XliffResource Include="MultilingualResources\<project-name>.fr-FR.xlf" />
```

## <a name="ive-added-xlf-based-languages-where-are-my-strings"></a>我已添加基于 .xlf 的语言。 我的字符串位于何处？

你的默认语言资源文件 (.resw) 就是你的应用所使用的字符串的规范“架构”。 已翻译的资源文件可包含所有这些字符串或这些字符串的一个子集。

生成项目时，将同步你的资源文件和 .xlf 文件。

- 已更新 .xlf 文件，以反映任何添加或删除的字符串或添加或删除的资源文件。
- 已更新资源文件，以反映 .xlf 文件中任何翻译的字符串。

有关此情况的详细信息，请参阅[使用多语言应用工具包 4.0](use-mat.md)。

## <a name="when-i-build-my-project-the-xlf-files-remain-empty"></a>生成项目时，.xlf 文件保持为空白

你的应用需要可本地化，然后才能有效使用 MAT。 有关此情况的详细信息，请参阅[使用多语言应用工具包 4.0](use-mat.md)。

## <a name="what-is-microsoft-translator"></a>什么是 Microsoft Translator？

Microsoft Translator 是提供基于机器翻译的基于云的服务。 在无法合理获得人工翻译时，机器翻译是获得译文的理想选择。 若要了解详细信息，请参阅 [Microsoft Translator](https://go.microsoft.com/fwlink/p/?LinkId=258220)。

工具包使用 Microsoft Translator 服务向你提供翻译建议。 当 Microsoft Translator 图标显示在翻译语言对话框中时，可以看到 Microsoft Translator 支持的语言。

通过选择某一字符串，并单击**翻译**，可使用多语言编辑器中的 Microsoft Translator 快速翻译你的应用。

## <a name="what-is-pseudo-language-and-what-are-pseudo-resource-trackers"></a>什么是伪语言，什么是伪资源跟踪器？

伪语言是一种对旨在模拟真实语言本地化的软件产品的人为修改。 有关伪语言和伪资源跟踪器的详细信息，请参阅[使用多语言应用工具包 4.0](use-mat.md)。

## <a name="how-do-i-set-my-language-preference-to-pseudo-language-so-that-i-can-test-my-pseudo-locd-strings"></a>如何将语言首选项设置为伪语言，以便测试伪本地化的字符串？

有关此情况的解释，请参阅[使用多语言应用工具包 4.0](use-mat.md)。

## <a name="what-kind-of-localizability-issues-can-i-find-using-pseudo-language"></a>可以找到使用伪语言的哪些类型的本地化问题？

有关此情况的解释，请参阅[使用多语言应用工具包 4.0](use-mat.md)。

## <a name="im-not-seeing-any-translations-when-i-launch-my-app-or-my-app-is-only-partially-translated"></a>启动应用后发现其中的内容全部未翻译或仅翻译了一部分

在多语言编辑器中打开 .xlf 文件，查看是否存在译文。 当显式更改默认语言 .resw 文件中的字符串时，将从 .xlf 文件中删除任何相应译文。 这是为了确保译文匹配其源字符串。 翻译 .xlf 文件中的字符串，并重新生成字符串，以更新非默认语言 .resw 文件。

如果已在 .xlf 文件中翻译字符串，但这些字符串未显示在你的应用中，请重新生成项目以更新非默认语言 .resw 文件。 Visual Studio 优化生成命令，以仅生成自最后一次生成开始更改的文件。

检查你的语言首选项顺序。 确保你想要测试的语言列出在**设置**中语言首选项列表的顶部。

## <a name="the-toolkit-is-reporting-error--0x80004004-in-the-build-output"></a>工具包在生成输出中报告错误 0x80004004

```dosbatch
Merge of Loc PRI file failed calling makepri.exe: "0x80004004"
```

当区域格式与工具包生成操作冲突时，将显示此消息。 解决方法是在生成时在**设置**中将语言更改为 en-US。


## <a name="the-toolkit-is-reporting-error--0x80004005-in-the-build-output"></a>工具包在生成输出中报告错误 0x80004005

```dosbatch
Merge of Loc PRI file failed calling makepri.exe: "0x80004005"
```

当 .xlf 文件包含不受支持的目标语言时，将显示此消息。 例如，“zh-cht”不正确（将其更改为“zh-hant”），并且“zh-chs”也不正确（将其更改为“zh-hans”）。

## <a name="is-there-a-way-to-find-out-more-information-about-the-errors-im-seeing"></a>是否存在某种方法可以找到所看到的错误的详细信息？

是的，可以打开 Visual Studio 中的详细日志。 单击**工具** > **选项** > **项目和解决方案** > **生成并运行**。 将**MSBuild 项目生成输出详细程度**从最小更改为正常或更高。

从命令行运行 MSBuild 还可能生成额外的消息。

```dosbatch
msbuild /t:rebuild <project-name>
```

## <a name="import-translation-failed"></a>导入译文失败

导入过程在导入之前会执行基本的验证。 这将确保正在导入的文件中的目标文化信息与现有 .xlf 文件中的目标文化信息相匹配。 在多语言编辑器中打开 .xlf 文件，并确保文化信息相匹配。

## <a name="what-if-my-translator-doesnt-have-windows-10-andor-visual-studio-andor-the-multilingual-app-toolkit-installed"></a>如果我的 Translator 未安装 Windows 10 和/或 Visual Studio 和/或多语言应用工具包会怎样？

当选择导出字符串资源对话框中的**输出：邮件收件人**时，电子邮件包含下载和安装多语言应用工具包 (MAT) 4.0 的链接。 即使没有 Windows 10 或 Visual Studio，你的 Translator 仍可安装 MAT 4.0 独立多语言编辑器工具。

有关详细信息，请参阅[使用多语言应用工具包 4.0](use-mat.md)。

## <a name="what-happened-to-the-markuprulesxml-and-resourceslocksxml-files"></a>`MarkupRules.xml` 和 `ResourcesLocks.xml` 文件会作何处理？

多语言应用工具包 4.0 不使用专有资源锁定文件。 相反，直接向 .xlf 文件添加 XLIFF 1.2 标记 `<mrk>`，以识别在机器翻译期间未修改的字符串。 这使得 XLIFF 文件为独立文件，并允许基于每个文件进行资源锁定。

这些额外的支持文件已经不再需要，所以你可以放心将其删除。

## <a name="what-happened-to-the-tpx-file"></a>.tpx 文件会作何处理？

.tpx 文件提供了一种简单的方法，用于在发送 .xlf 文件以进行翻译时包括 `MarkupRules.xml` 和 `ResourcesLocks.xml` 文件。 不再需要此功能。

如果在需要检索的 .tpx 文件中具有译文，则仅需将 .tpx 文件扩展名重命名为 .zip。 这将允许通过文件资源管理器或任何 .zip 兼容工具打开和解压缩内容。

## <a name="i-think-ive-done-everything-right-but-it-still-isnt-working"></a>我认为我已正确完成所有操作，但却仍无法工作

请尝试以下步骤。

1. 使用上面介绍的方法之一添加译文。
2. 转储 .pri 文件（请参阅 [MakePri.exe 命令行选项](../../app-resources/makepri-exe-command-options.md)），查看译文是否位于 .pri 文件中。 将显示带语言代码和已翻译的值（如下所示）的译文。
   ```xml
   <Candidate qualifiers="Language-QPS-PLOC" type="String">
       <Value>[!!_Ŝéãřćĥ_!!]</Value>
   </Candidate>
   ```
3. 从命令提示符处生成；结果错误可能具有比生成输出中报告的信息更多的详细信息。

## <a name="my-app-failed-certification-to-the-microsoft-store"></a>我的应用无法向 Microsoft Store 进行认证

在开始 Microsoft Store 认证过程之前，必须从项目中排除 `<project-name>.qps-ploc.xlf` 文件。 伪语言用于检测潜在的本地化问题或错误，但它不是有效的 Microsoft Store 语言。 如果未将其删除，则你的应用将在 Microsoft Store 认证过程中失败。

## <a name="related-topics"></a>相关主题

* [使用多语言应用工具包 4.0](use-mat.md)
* [Microsoft Translator](https://go.microsoft.com/fwlink/p/?LinkId=258220)
* [MakePri.exe 命令行选项](../../app-resources/makepri-exe-command-options.md)
