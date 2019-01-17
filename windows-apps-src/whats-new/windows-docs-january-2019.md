---
title: 什么是 2019 年 1 月 Windows 文档新增功能-开发 UWP 应用
description: 新功能、 视频和开发人员指南已添加到 Windows 10 开发人员文档中针对 2019 年 1 月
keywords: 新增功能，更新，功能，开发人员指南，Windows 10 年 1 月
ms.date: 1/17/2019
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 4f224663506cbb60f6c1476caccb5ecffefeaf7b
ms.sourcegitcommit: cfdc854fede8e586202523cdb59d3d0a2f5b4b36
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/17/2019
ms.locfileid: "9014388"
---
# <a name="whats-new-in-the-windows-developer-docs-in-january-2019"></a>什么是 Windows 开发人员文档中 2019 年 1 月的新增功能

Windows 开发人员文档持续更新对整个 Windows 平台的开发人员提供的新功能的信息。 以下功能概述、 开发人员指南和视频进行了在 1 月 1 个月中可用。

只需在 Windows10 上[安装工具和 SDK](http://go.microsoft.com/fwlink/?LinkId=821431)，你便可以随时[创建新的通用 Windows 应用](../get-started/create-uwp-apps.md)，或了解如何使用 [Windows 上的现有应用代码](../porting/index.md)。

## <a name="features"></a>功能

### <a name="windows-development-on-microsoft-learn"></a>在 Microsoft 了解上的 Windows 开发

Microsoft 了解 Microsoft 开发人员提供新动手学习和培训机会。 如果你感兴趣学习如何开发 Windows 应用，请查看有关该平台，工具，以及如何编写第一个几个应用的全面介绍[我们新的学习路径](https://docs.microsoft.com/learn/paths/develop-windows10-apps/)。

![学习路径在 Windows 开发的图像](images/windows-learn.png)

### <a name="direct-3d-12"></a>直接 3D 12

[Direct3D 12 呈现器通道，](/windows/desktop/direct3d12/direct3d-12-render-passes)可以提高性能的呈现器，如果它基于基于磁贴的推迟呈现 (TBDR)，以及其他技术。 技术可帮助你的呈现器，以启用更好地标识资源呈现排序要求和数据的依赖项，你的应用程序，从而减少对关闭芯片内存的内存流量通过提高 GPU 效率。

### <a name="msix-modification-packages"></a>MSIX 修改包

[MSIX 修改包](https://docs.microsoft.com/windows/msix/modification-package-1809-update)的 Windows 10 版本 1809年改进的支持。 修改包可包括基于注册表的插件和相关联的自定义，并将启用通过 MSIX 使用虚拟注册表并按预期运行部署的应用程序。

![MSIX 修改创建程序包](images/msix-modification-package.png)

### <a name="open-source-of-wpf-windows-forms-and-winui"></a>打开 WPF、 Windows 窗体和 WinUI 源

WPF、 Windows 窗体和 WinUI UX 框架现在均可用于开放源代码在 GitHub 上所做的贡献。 有关详细信息和链接，请参阅[生成 Windows 应用博客](https://blogs.windows.com/buildingapps/2018/12/04/announcing-open-source-of-wpf-windows-forms-and-winui-at-microsoft-connect-2018/#OKZjJs1VVTrMMtkL.97)。

### <a name="progressive-web-apps-for-xbox"></a>适用于 Xbox 的渐进式 Web 应用

与[适用于 Xbox One 的渐进式 Web 应用](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/xbox-considerations)，你可以扩展 web 应用程序，并使其可作为 Xbox One 的应用通过 Microsoft 应用商店同时仍继续使用现有框架、 CDN 和服务器的后端。 大多数情况下，你可以打包你 PWA 适用于 Xbox One 与 windows 将相同的方式，但是，有几个本指南将指导你完成的主要区别。

### <a name="windows-machine-learning"></a>Windows 机器学习

我们已重构[WinML Api 的登录页](https://docs.microsoft.com/windows/ai/api-reference)，并添加新文档中的针对 WinML 自定义运营商和本机 Api。

[使用 PyTorch 训练](https://docs.microsoft.com/windows/ai/train-model-pytorch)提供关于如何训练模型使用 PyTorch 框架，本地或云中的指南。 然后可以下载的 ONNX 文件作为此模型，并使用它在 WinML 应用程序。

![WinML 图形](images/winml-graphic.png)

## <a name="developer-guidance"></a>开发人员指南

### <a name="choose-your-platform"></a>选择你的平台

在创建新的桌面应用程序感兴趣？ 查看我们修改了[选择平台](https://docs.microsoft.com/windows/desktop/choose-your-technology)页面以获取详细的描述和比较的 UWP、 WPF 和 Windows 窗体平台和 Win32 API 的详细信息。

### <a name="faqs-on-win32-webview"></a>在 Win32 web 视图的常见问题解答

使用 Microsoft Edge WebView 在桌面应用程序，以及指向示例和其他资源时，[常见问题](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webview#frequently-asked-questions-faqs)我们提供的常见问题的解答。

### <a name="japanese-era-change"></a>日语 era 更改

[准备你的应用程序日语 era 更改](../design/globalizing/japanese-era-change.md)向你显示如何确保的 Windows 应用程序是准备就绪的日语 era 更改集才能将在 2019 年 5 月 1 日。 [此页面是日语中也提供](https://docs.microsoft.com/ja-jp/windows/uwp/design/globalizing/japanese-era-change)。

## <a name="videos"></a>视频

### <a name="progressive-web-apps"></a>渐进式 Web 应用

渐进式 Web 应用是像本机应用函数跨不同浏览器和各种 Windows 10 设备的网站。 [观看视频](https://youtu.be/ugAewC3308Y)以了解更多信息，然后[查看文档](http://aka.ms/Windows-PWA)开始。

### <a name="vs-code-series"></a>VS 代码系列

请查看我们的[Visual Studio Code 的新视频系列](https://www.youtube.com/playlist?list=PLlrxD0HtieHjQX77y-0sWH9IZBTmv1tTx)有关 VSCode 是什么、 如何使用它，以及它的创建方式的信息。

### <a name="one-dev-question"></a>一个开发人员的问题

在一个开发人员的问题视频系列中，longtime Microsoft 开发人员介绍一系列有关 Windows 开发、 团队区域性和历史记录的问题。 下面是我们回答的最新问题 ！

Raymond Chen:

* [为什么具有 Program Files 和 Program Files (x86)？](https://youtu.be/N7o9eJpFYco)

Larry Osterman:

* [为何 COM 因此复杂？](https://youtu.be/-gkXAV-StVA )
* [Microsoft 像你第一访谈是什么？](https://youtu.be/qRb6otsHG5c)
