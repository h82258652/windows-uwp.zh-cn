---
title: 2019 年 1 月 Windows 文档中的新增功能 - 开发 UWP 应用
description: 新增功能、视频和开发人员指南已添加到 2019 年 1 月 Windows 10 开发人员文档
keywords: 新增功能, 更新, 功能, 开发人员指南, Windows 10, 一月
ms.date: 01/17/2019
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 7947fb6e71a9f2ddbedcd8e3ee8bab7b720dc444
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "74902471"
---
# <a name="whats-new-in-the-windows-developer-docs-in-january-2019"></a>2019 年 1 月 Windows 开发人员文档中的新增功能

Windows 开发人员文档持续更新对整个 Windows 平台的开发人员提供的新功能的信息。 一月当月提供了以下功能概述、开发人员指南和视频。

只需在 Windows 10 上[安装工具和 SDK](https://developer.microsoft.com/windows/downloads#_blank)，你便可以随时[创建新的通用 Windows 应用](../get-started/create-uwp-apps.md)，或了解如何使用 [Windows 上的现有应用代码](../porting/index.md)。

## <a name="features"></a>功能

### <a name="windows-development-on-microsoft-learn"></a>Microsoft Learn 上的 Windows 开发

Microsoft Learn 为 Microsoft 开发人员提供新的动手学习和培训机会。 如果你对学习如何开发 Windows 应用感兴趣，请查看[我们的新学习路径](/learn/paths/develop-windows10-apps/)，其中详细介绍了平台、工具以及如何编写最初的几个应用。

![Windows 开发学习路径的图像](images/windows-learn.png)

### <a name="direct-3d-12"></a>Direct 3D 12

如果渲染器基于“基于磁贴的延迟渲染”(TBDR) 以及其他技术，则 [Direct3D 12 渲染器通道](/windows/desktop/direct3d12/direct3d-12-render-passes)可提高渲染器的性能。 该技术通过使应用程序更好地识别资源呈现排序要求和数据依赖项，并因此减少与芯片外内存之间的内存流量，来帮助呈现器提高 GPU 效率。

### <a name="msix-modification-packages"></a>MSIX 修改包

Windows 10 版本 1809 改进了对 [MSIX 修改包](/windows/msix/modification-package-1809-update)的支持。 修改包可包括基于注册表的插件以及相关的自定义，并支持通过 MSIX 部署应用程序以使用虚拟注册表并按预期运行。

![MSIX 修改包创建](images/msix-modification-package.png)

### <a name="open-source-of-wpf-windows-forms-and-winui"></a>WPF、Windows 窗体和 WinUI 的开源

WPF、Windows 窗体和 WinUI UX 框架现可用于 GitHub 上的开源内容。 有关详细信息和链接，请参阅[生成 Windows 应用博客](https://blogs.windows.com/buildingapps/2018/12/04/announcing-open-source-of-wpf-windows-forms-and-winui-at-microsoft-connect-2018/#OKZjJs1VVTrMMtkL.97)。

### <a name="progressive-web-apps-for-xbox"></a>适用于 Xbox 的渐进式 Web 应用

使用[适用于 Xbox One 的渐进式 Web 应用](/microsoft-edge/progressive-web-apps/xbox-considerations)，可以扩展 Web 应用，并通过 Microsoft Store 将其作为 Xbox One 应用提供，同时仍继续使用现有框架、CDN 和服务器后端。 大多数情况下，可以通过与 Windows 相同的方式打包适用于 Xbox One 的 PWA，但存在几个关键差异，本指南将介绍这些差异。

### <a name="windows-machine-learning"></a>Windows 机器学习

我们重构了 [WinML API 的登陆页面](/windows/ai/api-reference)，并为 WinML 自定义运算符和本机 API 添加了新的文档。

[使用 PyTorch 训练模型](/windows/ai/train-model-pytorch)提供关于如何在本地或云中使用 PyTorch 框架训练模型的指导。 然后，可以 ONNX 文件形式下载此模型，并在 WinML 应用程序中使用。

![WinML 图形](images/winml-graphic.png)

## <a name="developer-guidance"></a>开发人员指南

### <a name="choose-your-platform"></a>选择平台

希望新建桌面应用程序？ 查看我们改进的[选择平台](/windows/desktop/choose-your-technology)页面，获取关于 UWP、WPF 和 Windows 窗体平台的详细介绍和对比，以及关于 Win32 API 的详细信息。

### <a name="faqs-on-win32-webview"></a>Win32 WebView 上的常见问题解答

[常见问题解答](/windows/communitytoolkit/controls/wpf-winforms/webview#frequently-asked-questions-faqs)为在桌面应用程序中使用 Microsoft Edge WebView 时所提出的常见问题提供解答，并提供示例和其他资源的链接。

### <a name="japanese-era-change"></a>日本年号更改

[使应用程序做好应对日本年号更改的准备](../design/globalizing/japanese-era-change.md)展示如何确保 Windows 应用程序针对于 2019 年 5 月 1 日进行的日本纪元更改做好准备。 [本页也有日语版](/windows/uwp/design/globalizing/japanese-era-change)。

## <a name="videos"></a>视频

### <a name="progressive-web-apps"></a>渐进式 Web 应用

渐进式 Web 应用是网站，功能类似于各种浏览器和各种 Windows 10 设备上的本机应用。 [观看视频](https://youtu.be/ugAewC3308Y)了解详细信息，然后[查看文档](https://developer.microsoft.com/windows/pwa)以开始。

### <a name="vs-code-series"></a>VS Code 系列

查看 [Visual Studio Code 的新视频系列](https://www.youtube.com/playlist?list=PLlrxD0HtieHjQX77y-0sWH9IZBTmv1tTx)，了解 VSCode 是什么、如何使用它以及如何创建它。

### <a name="one-dev-question"></a>一个开发问题

在“一个开发问题”视频系列中，资深的 Microsoft 开发人员介绍了关于 Windows 开发、团队文化和发展历程的一系列问题。 以下是我们已回答的最新问题！

Raymond Chen：

* [为什么使用程序文件和程序文件 (x86)？](https://youtu.be/qRb6otsHG5c)
* [你在 Microsoft 的第一次面试怎么样？](https://youtu.be/MfzzbNp8kfw)

Larry Osterman：

* [为什么 COM 如此复杂？](https://youtu.be/-gkXAV-StVA)
* [你在 Microsoft 的第一次面试怎么样？](https://youtu.be/N7o9eJpFYco)