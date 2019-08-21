---
title: 获取 UWP 应用示例
description: 了解如何从 GitHub 下载 UWP 代码示例。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 示例代码, 代码示例
ms.assetid: 393c5a81-ee14-45e7-acd7-495e5d916909
ms.localizationpriority: medium
ms.openlocfilehash: 6b0e30804eabb7e50c5a7319bba9a6b2c83e1d7e
ms.sourcegitcommit: 99595e4938213aafdb49635d684d8ba8eb3f697a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/15/2019
ms.locfileid: "69487820"
---
# <a name="get-uwp-app-samples"></a>获取 UWP 应用示例

通用 Windows 平台 (UWP) 应用示例在 GitHub 上的存储库中提供。 请参阅[示例](https://developer.microsoft.com/windows/samples)以获取可搜索的分类列表，也可以浏览 [Microsoft/Windows-universal-samples](https://github.com/Microsoft/Windows-universal-samples "通用 Windows 平台应用示例 GitHub 存储库") 存储库。 Windows-universal-samples 存储库包含演示所有 UWP 功能及其 API 使用模式的示例。

![GitHub UWP 示例存储库](images/GitHubUWPSamplesPage.png)

## <a name="download-the-code"></a>下载代码

若要下载示例，请转到[存储库](https://github.com/Microsoft/Windows-universal-samples "通用 Windows 平台应用示例 GitHub 存储库")。 选择“克隆或下载”，然后选择“下载 ZIP”。   

![示例下载](images/SamplesDownloadButton.png)

也可从本文[下载示例](https://github.com/Microsoft/Windows-universal-samples/archive/master.zip "通用 Windows 平台应用示例 zip 文件下载")。

示例下载 .zip 文件始终具有最新示例。 无需 GitHub 帐户即可下载此文件。 如果 SDK 更新已发布，或者你想要选取任何最新的更改和附加内容，则请下载最新的 zip 文件。

> [!NOTE]
> 若要打开、生成和运行 UWP 示例，则必须已安装 Visual Studio 2015 或更高版本和 Windows SDK。 可以获取 [Visual Studio Community 的免费副本](https://go.microsoft.com/fwlink/p/?LinkID=280676 "Windows 开发工具下载")。 Visual Studio Community 支持生成 UWP 应用。  
>
> 请确保解压缩整个存档，而不是个别示例，否则示例无法正常运行。 所有示例都依赖于该存档中的 SharedContent 文件夹。 UWP 功能示例使用 Visual Studio 中的“已链接”文件减少重复的常见文件，包括示例模板文件和图像资源。 常见文件存储在存储库根的 SharedContent 文件夹中。 项目文件中会使用链接来引用常见文件。
> 

## <a name="open-the-samples"></a>打开示例

下载 .zip 文件之后，在 Visual Studio 中打开示例。

1.  在解压缩存档之前，右键单击该文件，然后选择“属性” > “取消阻止” > “应用”    。 然后，将该存档解压缩到计算机上的本地文件夹中。

    ![解压缩的存档](images/SamplesUnzip1.png)
2.  Samples 文件夹中的每个文件夹都包含一个 UWP 功能示例。

    ![示例文件夹](images/SamplesUnzip2.png)
3.  选择一个示例，例如 Altimeter。 子文件夹表示受支持的语言。

    ![语言文件夹](images/SamplesUnzip3.png)
4.  选择要使用的语言对应的文件夹。 在文件夹内容中，你将看到可在 Visual Studio 中打开的 Visual Studio 解决方案 (.sln) 文件。 例如，*Altimeter.sln*。

    ![VS 解决方案](images/SamplesUnzip4.png)

## <a name="give-feedback-ask-questions-and-report-issues"></a>提供反馈、提出问题和报告问题

如果你有问题，请使用存储库中的“问题”选项卡创建新问题。  我们将尽可能提供帮助。

![反馈图像](images/GitHubUWPSamplesFeedback.png)
