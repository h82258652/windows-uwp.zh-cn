---
title: 获取 Windows 应用示例
description: 了解如何从 GitHub 下载 Windows 代码示例。
ms.date: 06/30/2020
ms.topic: article
keywords: windows 10, 示例代码, 代码示例
ms.assetid: 393c5a81-ee14-45e7-acd7-495e5d916909
ms.localizationpriority: medium
ms.openlocfilehash: 285388c4c1b4791e1ca271a853476416b6d13c13
ms.sourcegitcommit: 179f8098d10e338ad34fa84934f1654ec58161cd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85757611"
---
# <a name="get-windows-app-samples"></a>获取 Windows 应用示例

多个 GitHub 存储库中提供了诸多官方 Windows 代码示例，包括[通用 Windows 平台 (UWP) 应用示例](https://github.com/microsoft/Windows-universal-samples)、[Windows 经典示例](https://github.com/microsoft/Windows-classic-samples)以及 [Windows 开发人员文档示例](#windows-developer-documentation-samples)集合。 这些示例演示了大多数 Windows 功能及其 API 使用模式。

:::image type="content" source="images/github-windows-samples-page.png" alt-text="GitHub Windows 通用示例存储库":::

要更轻松地查找特定示例，可以通过[示例浏览器](https://docs.microsoft.com/samples/browse/)浏览和搜索各种 Microsoft 开发人员工具和技术的代码示例的分类集合。

:::image type="content" source="images/samples-browser-windows.png" alt-text="Microsoft 示例浏览器":::

### <a name="windows-developer-documentation-samples"></a>Windows 开发人员文档示例

下面列出了专为支持 Windows 开发人员文档而创建的迷你应用示例列表。 除非特别说明，下面的示例是已更新为使用最新 [WinUI 2.4](/windows/apps/winui/winui2/release-notes/winui-2.4) 控件的所有通用 Windows 平台 (UWP) 应用。

- [Rss 阅读器](https://github.com/Microsoft/Windows-appsample-rssreader) - 检索 RSS 源和查看文章
- [Family Notes](https://github.com/Microsoft/Windows-appsample-familynotes) - 探索不同的输入形式和用户感知方案
- [Customer Orders](https://github.com/Microsoft/Windows-appsample-customers-orders-database) - 适用于企业开发人员的功能，如 Azure Active Directory (AAD) 身份验证，UI 控件（包括数据网格）、Sqlite 和 SQL Azure 数据库集成、实体框架，以及云 API 服务
- [Lunch Scheduler](https://github.com/Microsoft/Windows-appsample-lunch-scheduler) - 安排你与朋友和同事的午餐
- [Coloring Book](https://github.com/Microsoft/Windows-appsample-coloringbook) - Windows Ink（包括 Windows Ink 工具栏）和射线控制器（适用于 Surface Dial 等滚轮设备）功能
- [网络帮助程序（测验游戏）](https://github.com/Microsoft/Windows-appsample-networkhelper)- 网络发现和通信
- [HUE Lights Controller](https://github.com/Microsoft/Windows-appsample-huelightcontroller) - 带有 Cortana 和蓝牙低功耗（蓝牙 LE）的智能家庭自动化
- [Marble Maze](https://github.com/Microsoft/Windows-appsample-marble-maze) - 使用 DirectX 的基本 3D 游戏
- [PhotoLab](https://github.com/Microsoft/Windows-appsample-photo-lab) - 查看和编辑图像文件

## <a name="download-the-code"></a>下载代码

要下载示例，请转到某个 Microsoft 存储库，例如[通用 Windows 平台 (UWP) 应用示例](https://github.com/microsoft/Windows-universal-samples)。 选择“克隆或下载”，然后选择“下载 ZIP”。 

![示例下载](images/SamplesDownloadButton.png)

示例下载 .zip 文件始终具有最新示例。 无需 GitHub 帐户即可下载此文件。 如果 SDK 更新已发布，或者你想要选取任何最新的更改和附加内容，则请下载最新的 zip 文件。

> [!NOTE]
> 要打开、生成和运行 Windows 示例，则必须已安装 Visual Studio 和 Windows SDK。 可以获取 [Visual Studio Community](https://www.microsoft.com/?ref=go) 的免费副本。  
>
> 请确保解压缩整个存档，而不是个别示例，否则示例无法正常运行。 许多示例依赖于 SharedContent 文件夹中的常见文件，并使用链接文件（包括示例模板文件和图像资产）减少重复。

## <a name="open-the-samples"></a>打开示例

下载 .zip 文件之后，在 Visual Studio 中打开示例。

1. 在解压缩存档之前，右键单击该文件，选择“属性” > “取消阻止” > “应用”  。 然后，将该存档解压缩到计算机上的本地文件夹中。

    ![解压缩的存档](images/SamplesUnzip1.png)

2. Samples 文件夹中的每个文件夹都包含一个 Windows 功能示例。

    ![示例文件夹](images/SamplesUnzip2.png)

3. 选择一个示例。 支持的语言由特定于语言的子文件夹指示。

    ![语言文件夹](images/SamplesUnzip3.png)

4. 选择要使用的语言对应的文件夹。 在文件夹内容中，你将看到可在 Visual Studio 中打开的 Visual Studio 解决方案 (.sln) 文件。

    ![VS 解决方案](images/SamplesUnzip4.png)

## <a name="give-feedback-ask-questions-and-report-issues"></a>提供反馈、提出问题和报告问题

如果你有问题，请使用存储库中的“问题”选项卡创建新问题。

![反馈图像](images/GitHubUWPSamplesFeedback.png)
