---
author: stevewhims
Description: The Multilingual App Toolkit (MAT) 4.0 integrates with Microsoft Visual Studio 2017 to provide UWP apps with translation support, translation file management, and editor tools.
title: 使用多语言应用工具包
template: detail.hbs
ms.author: stwhi
ms.date: 01/23/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 全球化, 可本地化性, 本地化
ms.localizationpriority: medium
ms.openlocfilehash: 39e002247cabb6389ddf23860499ebf1f166b03a
ms.sourcegitcommit: fbdc9372dea898a01c7686be54bea47125bab6c0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/08/2018
ms.locfileid: "4431006"
---
# <a name="use-the-multilingual-app-toolkit-40"></a>使用多语言应用工具包 4.0

多语言应用工具包 (MAT) 4.0 与 Microsoft Visual Studio 2017 集成，为 UWP 应用提供翻译支持、翻译文件管理和编辑工具。 下面是一些此工具包的值建议。

- 帮助你在开发期间管理资源更改和翻译状态。
- 提供 UI，可基于配置的翻译提供程序来选择语言。
- 支持本地化行业标准 XLIFF 文件格式。
- 提供伪语言引擎，帮助在开发期间识别翻译问题。
- 与 Microsoft 语言门户连接，轻松访问翻译的字符串和术语。
- 与 Microsoft Translator 连接，获取快速翻译建议。

## <a name="how-to-use-the-toolkit"></a>如何使用此工具包

### <a name="step-1-design-your-app-for-globalization-and-localization"></a>步骤 1： 针对全球化和本地化设计你的应用

你的应用需要可本地化，然后才能有效使用 MAT。 具体来说，你的项目应包含一个或多个资源文件 (.resw)，其中包含有以默认语言显示的应用的字符串。 有关详细信息，请参阅[本地化 UI 和应用程序包清单中的字符串](../../app-resources/localize-strings-ui-manifest.md)。 完成此操作后，此工具包即可快速、轻松添加其他语言。

有关全球化和本地化的值建议，以及术语**全球化**、**可本地化性**和**本地化**的定义，请参阅[全球化和本地化](globalizing-portal.md)。

另请参阅[全球化指南](guidelines-and-checklist-for-globalizing-your-app.md)和[使你的应用可本地化](prepare-your-app-for-localization.md)。

### <a name="step-2-download-and-install-the-multilingual-app-toolkit-40"></a>步骤 2： 下载并安装多语言应用工具包 4.0

多语言应用工具包 4.0 (MAT 4.0) 具有两个部分，每个部分都有其自己的安装程序。

- [适用于 Visual Studio 2017 的多语言应用工具包 4.0 扩展](https://marketplace.visualstudio.com/items?itemName=MultilingualAppToolkit.MultilingualAppToolkit-18308)。 它包含 .vsix 安装程序形式的适用于 Visual Studio 2017 的 MAT 4.0 扩展。
- [多语言应用工具包 4.0 编辑器](https://developer.microsoft.com/en-us/windows/develop/multilingual-app-toolkit)。 它包含 .msi 安装程序形式的 MAT 4.0 独立多语言编辑器工具。 此外，它还包含适用于 Visual Studio 2015 和 Visual Studio 2013 的 MAT 4.0 扩展。

如果使用 Visual Studio 2017，请逐个下载并运行这两种安装程序。 如果使用 Visual Studio 2015 或 Visual Studio 2013，请下载并运行 .msi 安装程序。

### <a name="step-3-enable-the-multilingual-app-toolkit-for-your-project"></a>步骤 3： 针对项目启用多语言应用工具包

必须先针对项目启用 MAT，然后才能开始本地化应用。 以下是启用工具包的方法。

- 在 Visual Studio 中打开项目解决方案。
- 在解决方案资源管理器中，选择所需项目。
- 在**工具**菜单上，选择**多语言应用工具包** > **启用选择**。 

在输出窗口（从多语言应用工具包中显示输出）中，注意消息 `Project '<project-name>' was enabled. The project's source culture is '<language-tag>' <language-name>`。 如果显示此消息，即可使用 MAT。

### <a name="step-4-add-languages-to-your-project"></a>步骤 4： 向项目添加语言

按照以下步骤向你的项目添加语言。

1. 在解决方案资源管理器中，右键单击项目节点。
2. 单击**多语言应用工具包** > **添加翻译语言…**。
3. 在“翻译语言”对话框中，选择想要支持的语言，然后单击“确定”。

工具包将作出响应，执行这些操作。

- 对于已添加的各种语言，将创建一个得名于该语言的 [BCP 47 语言标记](http://go.microsoft.com/fwlink/p/?linkid=227302)的新文件夹。 该文件夹内创建了新的资源文件 (.resw)，以匹配包含默认语言字符串的文件。
- 如果这是你首次添加语言，则将向项目添加一个名为 `MultilingualResources` 的新文件夹。 该文件夹内添加了各种语言的 .xlf 文件。 .xlf 文件包含项目的每个资源文件 (.resw) 中每个字符串的翻译单元。
- 输出窗口确认对所添加语言的添加。

每当添加/删除默认语言资源文件 (.resw) 或添加/删除默认语言资源文件 (.resw) 中的字符串时，重新生成项目以重新同步 .xlf 文件。 这将确保 .xlf 文件包含以默认语言显示的字符串并集。

安装的 [Microsoft 语言门户](http://go.microsoft.com/fwlink/p/?LinkId=330295)和 [Microsoft Translator](http://go.microsoft.com/fwlink/p/?LinkId=258220) 等翻译提供程序可用于翻译应用的资源。 当提供程序支持某一特定语言时，该提供程序的图标将显示在“翻译语言”对话框中语言名称的旁边。

在“翻译语言”对话框中，工具包所发现的任何基于 .xlf 的现有语言的选项框均已预勾选，指示项目中已包含该语言。

向项目中添加某一语言后，无法通过取消勾选“翻译语言”对话框中的框来删除该语言。 若要删除某一语言，右键单击语言特定的 .xlf 文件，然后选择**删除**。 如果确认，这还将删除相应资源文件 (.resw)。

### <a name="step-5-test-your-app-using-pseudo-language"></a>步骤 5： 使用伪语言测试你的应用

伪语言是一种对旨在模拟真实语言本地化的软件产品人工修改，但对母语人士仍具可读性。 伪翻译替换字符并展开资源字符串长度，以便在项目早期和开始本地化之前认真检测出潜在的可本地化性问题或错误。

请按照以下步骤对项目进行伪本地化并测试。

1. 使用“翻译语言”对话框向项目添加伪语言 [qps-ploc]。
2. 右键单击解决方案资源管理器中的 `<project-name>.qps-ploc.xlf` 文件，然后单击**多语言应用工具包** > **生成机器翻译**。
3. 在**设置** > **时间和语言** > **区域和语言** > **语言**中，单击**添加语言**。
5. 在搜索框中，键入 `qps-ploc`。
6. 单击 `English (qps-ploc)` 进行添加。
7. 在语言列表中，选择 `English (qps-ploc)` 并单击**设为默认值**。
8. 测试伪本地化的应用。 例如，查找其中未显示某一字符串的所有部分（字符串被截断）的 UI 布局问题，或未翻译（但已硬编码）的字符串。

除了字符替换和扩展，伪引擎还提供每个资源的唯一跟踪标识符。 此跟踪符预置到每个字符串的开头，并由括号括起 `[xxxxx]`。 在可视 UI 检查测试期间，可以使用这些跟踪符。 它们可以帮助跟踪产品中的特定资源，尤其是在多个资源具有类似或重复文本的情况下。

在此“Hello, World!” 文本示例中，伪翻译展开并占据 30% 以上的屏幕空间，然后应用资源跟踪符。

```
"Hello World" -> "Ĥèĺļõ Ŵòŗłđ" -> "[!!_Ĥèĺļõ Ŵòŗłđ_!!]" -> "[hJ8s1][!!_Ĥèĺļõ Ŵòŗłđ_!!]"
```

### <a name="step-6-translate-your-app-into-selected-languages"></a>步骤 6： 将应用翻译为选定语言

多语言应用工具包已集成到生成过程中。 在生成过程中，已更新的字符串将自动添加到各种语言 .xlf 文件。
使用伪语言测试应用后，可通过三个选项将你的应用翻译为其他语言进行发布。

#### <a name="option-1-translate-the-strings-yourself"></a>选项 1 自行翻译字符串

可以使用多语言编辑器单独翻译字符串。 前面已经提到，[.msi 安装程序](https://developer.microsoft.com/en-us/windows/develop/multilingual-app-toolkit)中包含此操作。

- 右键单击想要翻译的 .xlf 文件。
- 单击**打开方式…** 并选择多语言编辑器。 可以根据需要单击**设为默认值**。
- 对于每个字符串，**源**以默认语言显示原始字符串。 在**翻译**中，针对正在编辑的 .xlf 文件键入翻译为相应语言的字符串。
- 完成此操作后，保存并关闭该文件。

重新生成项目，以便系统将已翻译的字符串复制到与你刚刚编辑的 .xlf 文件相对应的资源文件 (.resw) 中。

还可以通过以下操作启动多语言编辑器。 转到“开始”，显示所有应用，打开多语言应用工具包文件夹，然后单击多语言编辑器进行启动。

#### <a name="option-2-send-the-xlf-files-to-a-third-party-for-translation"></a>选项 2 将 .xlf 文件发送给第三方进行翻译

若要将翻译和编辑工作外包给本地化人员，在解决方案资源管理器中选择所需 .xlf 文件，右键单击这些文件，然后单击**多语言应用工具包** > **导出翻译...**。

选择“导出字符串资源”对话框中的**输出: 邮件收件人**，然后单击“确定”，你的文件将被压缩并附加到新电子邮件。 选择**输出: 文件文件夹位置**，浏览文件夹，然后单击“确定”，还可根据需要选择压缩此文件，并再次单击“确定”，你的文件将被（压缩并）保存在所选位置处得名于项目的新文件夹内。

在本地化人员完成翻译工作并向你发送已翻译的 .xlf 文件后，便可以将这些文件导入项目。 在解决方案资源管理器中选择所需 .xlf 文件，右键单击这些文件，然后单击**多语言应用工具包** > **导入/循环使用翻译...**。单击**添加**，导航到 .xlf 或 .zip 文件，然后单击**导入**。

**注意**导入过程在导入之前会执行基本的验证。 这将确保正在导入的文件中的目标文化信息与现有 .xlf 文件中的目标文化信息相匹配。

重新生成项目，以便系统将已翻译的字符串复制到与你刚刚导入的 .xlf 文件相对应的资源文件 (.resw) 中。

以下第三方提供商提供本地化服务，可能对你有所帮助。

- [Elanex](https://www.elanex.com/)
- [Keywords Studios](https://www.keywordsstudios.com/)
- [Lionbridge](https://www.lionbridge.com)
- [Moravia](https://www.moravia.com/)
- [SDL](https://www.sdl.com/languagecloud/managed-translation/ilp/instantquote)
- [Welocalize](https://www.welocalize.com/)

> [!NOTE]
> 以上列表仅用于提供信息，并非某种认可。 Microsoft 对这些供应商或其服务不作任何表示或担保，并且在任何情况下，Microsoft 对你使用这些供应商或服务不承担任何责任。 任何有关这些供应商或其服务的问题、投诉或索赔均必须指向适当的供应商。

#### <a name="option-3-use-the-integrated-translation-services"></a>选项 3 使用集成的翻译服务

翻译服务已集成到 Visual Studio IDE 和多语言编辑器中。 在开发产品和本地化资源时，这将提供对翻译服务的轻松访问。 对于此服务，将需要 Azure 帐户订阅，如 [Microsoft Translator 移动到 Azure 门户](https://multilingualapptoolkit.uservoice.com/knowledgebase/articles/1167898-microsoft-translator-moves-to-the-azure-portal)中所述。

若要访问 Visual Studio 内的翻译服务，在解决方案资源管理器中选择并右键单击的一个或多个 .xlf 文件，然后单击**生成机器翻译**。

多语言编辑器提供相同的翻译支持，还添加交互式翻译建议，允许你选择最适合资源字符串的翻译。 提供翻译建议后，可以针对翻译风格微调字符串。

多语言应用工具包随附两个提供程序。

- [Microsoft 语言门户](http://go.microsoft.com/fwlink/p/?LinkId=330295)提供程序基于 Microsoft 产品和服务的用户界面文本翻译启用翻译循环使用和术语匹配支持。
- [Microsoft Translator](http://go.microsoft.com/fwlink/p/?LinkId=258220) 提供程序启用按需机器翻译服务。

你和你的翻译人员可以管理多语言编辑器中的翻译状态，以便稍后查看不确定的翻译。 可以在**属性**选项卡中设置每个字符串的状态。状态值为：**新**、**需审阅**、**已翻译**、**最终**和**签核**。 位于行左侧的指示器显示状态。 当多语言编辑器中的所有行都显示为绿色时，即表示翻译工作已完成。

重新生成项目，以便系统将已翻译的字符串复制到与你刚刚编辑的 .xlf 文件相对应的资源文件 (.resw) 中。

### <a name="step-7-upload-your-app-to-the-microsoft-store"></a>步骤 7： 将你的应用上传到 Microsoft Store

在开始 Microsoft Store 认证过程之前，必须从项目中排除 `<project-name>.qps-ploc.xlf` 文件。 伪语言用于检测潜在的本地化问题或错误，但它不是有效的 Microsoft Store 语言。 如果未将其删除，则你的应用将在 Microsoft Store 认证过程中失败。

## <a name="related-topics"></a>相关主题

* [对 UI 和应用包清单中的字符串实施本地化](../../app-resources/localize-strings-ui-manifest.md)
* [全球化和本地化](globalizing-portal.md)
* [全球化指南](guidelines-and-checklist-for-globalizing-your-app.md)
* [对应用进行可本地化处理](prepare-your-app-for-localization.md)
* [BCP-47 语言标记](http://go.microsoft.com/fwlink/p/?linkid=227302)

## <a name="downloads"></a>下载

* [多语言应用工具包 4.0 .vsix 安装程序](https://marketplace.visualstudio.com/items?itemName=MultilingualAppToolkit.MultilingualAppToolkit-18308)
* [多语言应用工具包 4.0 .msi 安装程序](https://developer.microsoft.com/en-us/windows/develop/multilingual-app-toolkit)

## <a name="translation-services"></a>翻译服务

* [Microsoft 语言门户](http://go.microsoft.com/fwlink/p/?LinkId=330295)
* [Microsoft Translator](http://go.microsoft.com/fwlink/p/?LinkId=258220)
