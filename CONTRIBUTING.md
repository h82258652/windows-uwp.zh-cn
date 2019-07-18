---
ms.openlocfilehash: a91c080805bca5d536aad3755ca7edf052d1fe0e
ms.sourcegitcommit: b8087f8b6cf8367f8adb7d6db4581d9aa47b4861
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/27/2019
ms.locfileid: "67414061"
---
# <a name="contributing-to-uwp-conceptual-documentation"></a>向 UWP 概念性文档投稿

感谢你对通用 Windows 平台 (UWP) 文档的关注！ 感谢你向我们的文档提供反馈以及编辑和添加内容。

## <a name="writing-content"></a>编写内容

我们的文档采用 Markdown（一种轻型文本样式语法）进行编写。 如果不熟悉 Markdown，则可以[在 GitHub 上了解基础知识](https://guides.github.com/features/mastering-markdown/)。 不确定时，始终可以从我们文档中的其他页面复制格式设置样式。

## <a name="public-contributions"></a>公开投稿

如果你不是  Microsoft 员工，则可以通过[公共内容存储库](https://github.com/MicrosoftDocs/windows-uwp)进行投稿。 公开投稿适用于对现有页面的更改和说明。

### <a name="editing-a-file"></a>编辑文件

如果已处于公共内容存储库中，请首先导航到要更改的文件。 在此处，选择显示的内容上方的铅笔图标以开始编辑。

或者，如果在 docs.microsoft.com 中查看页面，则可以选择页面右上角的“编辑”  按钮。 这会使你重定向到存储库中的关联源文件。

开始编辑时，GitHub 会自动将官方存储库分叉到你的个人 GitHub 帐户中，在其中可以进行更改。 完成后，将拉取请求提交回“文档”  分支。

### <a name="pull-requests"></a>拉取请求

提交拉取请求之后，会按照内容质量清单对它进行评估，以确保它满足我们的基本标准。 如果通过评估，则会将它分配给 UWP 文档团队的成员以进行进一步审阅。 如果失败，则会告诉你要进行哪些更改。

分配的审阅者可以批准或拒绝 PR，或与你合作进行进一步更改。

## <a name="internal-contributions"></a>内部投稿

如果你是 Microsoft 员工，则可以通过[内容存储库](https://github.com/microsoftdocs/windows-uwp-pr)进行投稿。 可以在 [Windows 创作指南](https://review.docs.microsoft.com/windows-authoring-guide/uwp/?branch=master)中找到有关使用此存储库的指导。 只能通过私有存储库对有关即将推出的功能的文档进行投稿。

### <a name="editing-a-file"></a>编辑文件

与在公共存储库中一样，可以在浏览器中对私有存储库进行小更改，而无需创建本地克隆。 必须  确保在正确分支上进行投稿。 有关创建个人分支的详细信息，请参阅 [Windows 创作指南中的说明](https://review.docs.microsoft.com/windows-authoring-guide/uwp/conceptual/branches?branch=master)。

### <a name="making-substantial-changes"></a>进行重大更改

若要对现有文章进行大量更改、添加或更改图像或是投稿新文章，请创建私有内容存储库的本地克隆。 有关详细信息，请遵循 [Windows 创作指南中的说明](https://review.docs.microsoft.com/windows-authoring-guide/uwp/conceptual/)。

### <a name="pull-requests"></a>拉取请求

在内部存储库中创建拉取请求时，请确保将个人分支合并到从中创建它的分支中。

在你提交拉取请求之后，我们会使用 [PR 合并器](https://review.docs.microsoft.com/help/contribute/prmerger-overview?branch=master)对它进行评估，确保它满足我们的基本标准。 如果它通过评估，则可加注 `#sign-off`，将它传给 UWP 文档团队的成员进行进一步评审。 如果无法加注，系统会告知你需要进行哪些更改才能进行签署。

分配的审阅者可以批准或拒绝 PR，或与你合作进行进一步更改。 除非已自行批准，否则审阅者不会合并 PR。

## <a name="using-issues-to-provide-feedback-on-uwp-conceptual-documentation"></a>使用问题提供有关 UWP 概念性文档的反馈

如果要提供有关文档的反馈，而不是自己进行编辑，则可以[在公共存储库中创建问题](https://github.com/MicrosoftDocs/windows-uwp/issues)。 选择“问题”  选项卡并选择“新问题”  按钮。 请务必包含主题标题和页面的 URL。 问题会被分配给 UWP 文档团队成员进行审阅。

* 对于内部问题，使用 [WDG 内容请求工具](https://aka.ms/pubrequest)。
