---
title: 获取 UWP 应用示例
description: 了解如何从 GitHub 下载 UWP 代码示例
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 示例代码, 代码示例
ms.assetid: 393c5a81-ee14-45e7-acd7-495e5d916909
ms.localizationpriority: medium
ms.openlocfilehash: 64eb0d13db1fbcf49d9da28e57eb85ff84823bf1
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2018
ms.locfileid: "8750582"
---
# <a name="get-uwp-app-samples"></a>获取 UWP 应用示例

通用 Windows 平台 (UWP) 应用示例通过 GitHub 上的存储库提供。 请参阅[示例](https://developer.microsoft.com/windows/samples "开发人员中心示例")获取可搜索的分类列表，或浏览 [Microsoft/Windows-universal-samples](https://github.com/Microsoft/Windows-universal-samples "通用 Windows 平台应用示例 GitHub 存储库")存储库，其中包含了演示所有 UWP 功能及其 API 使用模式的示例。  
![GitHub UWP 示例存储库](images/GitHubUWPSamplesPage.png)

## <a name="download-the-code"></a>下载代码

若要下载示例，请转到[存储库](https://github.com/Microsoft/Windows-universal-samples "通用 Windows 平台应用示例 GitHub 存储库")并选择**克隆或下载**，然后选择**下载 ZIP**。 或者，只需单击[此处](https://github.com/Microsoft/Windows-universal-samples/archive/master.zip "通用 Windows 平台应用示例 zip 文件下载")。

zip 文件始终具有最新示例。 无需 GitHub 帐户即可下载它。 当发布 SDK 更新时或者如果你想要选取任何最新更改/附加内容，只需重新查看最新的 zip 文件。

![示例下载](images/SamplesDownloadButton.png)


> [!NOTE]
> UWP 示例需要使用 Visual Studio 2015 或更高版本和 Windows SDK 才能打开、生成并运行。 你可以在[此处](http://go.microsoft.com/fwlink/p/?LinkID=280676 "Windows 开发工具下载")获得支持生成 UWP 应用的 Visual Studio 社区的免费副本。  
>
> 此外，请确保解压缩整个存档，而不只是个别示例。 所有示例都依赖于该存档中的 SharedContent 文件夹。 UWP 功能示例使用 Visual Studio 中的“已链接”文件减少重复的常见文件，包括示例模板文件和图像资源。 这些常见文件存储在存储库根的 SharedContent 文件夹中，并且指代使用链接的项目文件。

下载 zip 文件之后，在 Visual Studio 中打开示例：

1.  在解压缩存档之前，右键单击它、依次选择**属性** > **取消阻止** > **应用**。 然后，将该存档解压缩到你的计算机上的本地文件夹。

    ![解压缩的存档](images/SamplesUnzip1.png)
2.  在示例文件夹中，你将看到大量文件夹，每个文件夹都包含 UWP 功能示例。

    ![示例文件夹](images/SamplesUnzip2.png)

3.  选择示例（如高度计），你将看到指示受支持语言的多个文件夹。

    ![语言文件夹](images/SamplesUnzip3.png)

4.  选择要使用的语言（如用于 C\# 的 CS），你将看到可在 Visual Studio 中打开的 Visual Studio 解决方案文件。

    ![VS 解决方案](images/SamplesUnzip4.png)

## <a name="give-feedback-ask-questions-and-report-issues"></a>提供反馈、提出问题和报告问题

如果你有难题或问题，只需使用存储库上的“问题”选项卡创建新问题，我们将尽可能提供帮助。

![反馈图像](images/GitHubUWPSamplesFeedback.png)
