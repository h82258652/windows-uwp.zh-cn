---
title: 什么是 Windows 文档中 2019 年 1 月中的新增功能-开发 UWP 应用
description: 已添加到 2019 年 1 月的 Windows 10 开发人员文档的新功能、 视频和开发人员指南
keywords: 最新内容、 更新、 功能、 开发人员指南，Windows 10 年 1 月
ms.date: 01/17/2019
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: beb80c28866b8f8207f203b70cb504dcd034098d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57636572"
---
# <a name="whats-new-in-the-windows-developer-docs-in-january-2019"></a>什么是 Windows 开发人员文档中在 2019 年 1 月的新增功能

Windows 开发人员文档持续更新对整个 Windows 平台的开发人员提供的新功能的信息。 以下功能概述、 开发人员指南和视频进行了年 1 月的月份中可用。

只需在 Windows 10 上[安装工具和 SDK](https://go.microsoft.com/fwlink/?LinkId=821431)，你便可以随时[创建新的通用 Windows 应用](../get-started/create-uwp-apps.md)，或了解如何使用 [Windows 上的现有应用代码](../porting/index.md)。

## <a name="features"></a>功能

### <a name="windows-development-on-microsoft-learn"></a>Microsoft 了解的 Windows 开发

Microsoft 了解到 Microsoft 开发人员提供新的动手学习和培训机会。 如果有兴趣学习如何开发 Windows 应用，请查看[我们新的学习路径](https://docs.microsoft.com/learn/paths/develop-windows10-apps/)平台、 工具，以及如何编写首个几个应用的全面介绍。

![学习路径的 Windows 开发的图像](images/windows-learn.png)

### <a name="direct-3d-12"></a>直接 3D 12

[Direct3D 12 中呈现传递](/windows/desktop/direct3d12/direct3d-12-render-passes)可以提高您的呈现器的性能如果它具有基于基于磁贴的延迟呈现 (TBDR) 以及其他技术。 方法可帮助您通过启用应用程序以更好地识别资源呈现排序需求和数据依赖项，并因此降低内存进出关闭芯片内存提高 GPU 效率的呈现器。

### <a name="msix-modification-packages"></a>MSIX 修改包

Windows 10 版本 1809年改进了对支持[MSIX 修改包](https://docs.microsoft.com/windows/msix/modification-package-1809-update)。 修改包可以包括基于注册表的插件和关联的自定义，并将启用通过 MSIX 使用虚拟注册表，并按预期方式运行部署的应用程序。

![MSIX 修改包创建](images/msix-modification-package.png)

### <a name="open-source-of-wpf-windows-forms-and-winui"></a>WPF、 Windows 窗体和 WinUI 的开放源代码

WPF、 Windows 窗体和 WinUI UX 框架现可在 GitHub 上的开放源代码发布内容。 有关详细信息和链接，请参阅[构建 Windows 应用博客](https://blogs.windows.com/buildingapps/2018/12/04/announcing-open-source-of-wpf-windows-forms-and-winui-at-microsoft-connect-2018/#OKZjJs1VVTrMMtkL.97)。

### <a name="progressive-web-apps-for-xbox"></a>适用于 Xbox 渐进式 Web 应用

与[渐进式 Web 应用，适用于 Xbox One](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/xbox-considerations)，可以扩展 web 应用程序并将其提供为 Xbox One 的应用程序通过 Microsoft Store 同时仍继续使用现有框架、 CDN 和服务器的后端。 大多数情况下，就像为 Windows 的方式相同，for Xbox One 包在 PWA，但是，有几个本指南将引导您完成的主要差异。

### <a name="windows-machine-learning"></a>Windows 机器学习

我们已重新构建[WinML Api 的登陆页面](https://docs.microsoft.com/windows/ai/api-reference)，并添加 WinML 自定义运算符和本机 Api 的新文档。

[PyTorch 与对模型进行定型](https://docs.microsoft.com/windows/ai/train-model-pytorch)指南提供有关如何使用 PyTorch framework 本地或在云中训练模型。 然后可以下载此模型作为 ONNX 文件，并在 WinML 应用程序中使用它。

![WinML 图](images/winml-graphic.png)

## <a name="developer-guidance"></a>开发人员指南

### <a name="choose-your-platform"></a>选择你的平台

在创建新的桌面应用程序感兴趣？ 请查看我们改进[选择你的平台](https://docs.microsoft.com/windows/desktop/choose-your-technology)页的详细的说明和 UWP、 WPF 和 Windows 窗体平台和 Win32 API 的详细信息的比较。

### <a name="faqs-on-win32-webview"></a>在 Win32 WebView 的常见问题

我们[方面的常见问题](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webview#frequently-asked-questions-faqs)桌面应用程序中使用 Microsoft Edge web 视图时提供常见问题的解答，以及链接到示例和其他资源。

### <a name="japanese-era-change"></a>日语纪元更改

[准备用于日语纪元更改应用程序](../design/globalizing/japanese-era-change.md)演示如何确保应用程序已准备的日语纪元更改集，才能在 Windows 上 2019 年 5 月 1 日，放置。 [此页也会出现在日语](https://docs.microsoft.com/ja-jp/windows/uwp/design/globalizing/japanese-era-change)。

## <a name="videos"></a>视频

### <a name="progressive-web-apps"></a>渐进式 Web 应用

渐进式 Web 应用是功能上相当于本机应用程序跨不同的浏览器和各种 Windows 10 设备的 web 站点。 [观看视频](https://youtu.be/ugAewC3308Y)若要了解详细信息，然后[签出文档](https://aka.ms/Windows-PWA)若要开始。

### <a name="vs-code-series"></a>VS 代码系列

请查看我们[Visual Studio Code 上的新视频系列](https://www.youtube.com/playlist?list=PLlrxD0HtieHjQX77y-0sWH9IZBTmv1tTx)VSCode 是什么、 如何使用它，以及它的创建方式有关的信息。

### <a name="one-dev-question"></a>一个适用于开发人员问题

在开发人员的一个问题视频系列中，资深 Microsoft 开发者介绍一系列的有关 Windows 开发、 团队区域性和历史记录的问题。 下面是我们回答的最新问题 ！

Raymond Chen:

* [为什么有 Program Files 和 Program Files (x86)？](https://youtu.be/N7o9eJpFYco)

Larry Osterman:

* [为什么是 COM 因此复杂？](https://youtu.be/-gkXAV-StVA )
* [Microsoft 在第一个访谈像是什么？](https://youtu.be/qRb6otsHG5c)
