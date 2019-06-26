---
title: 2018 年 7 月 Windows 文档中的新增功能 - 开发 UWP 应用
description: 新增功能、视频、示例和开发人员指南已添加到 2018 年 7 月 Windows 10 开发人员文档。
keywords: 新增功能, 更新, 功能, 开发人员指南, Windows 10, 七月
ms.date: 07/11/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 22a3a9614a4488791a36f81a3d4dedac572111b4
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/13/2019
ms.locfileid: "63780252"
---
# <a name="whats-new-in-the-windows-developer-docs-in-july-2018"></a>2018 年 7 月 Windows 开发人员文档中的新增功能

Windows 开发人员文档持续更新对整个 Windows 平台的开发人员提供的新功能的信息。 七月当月提供了以下功能概述、开发人员指南、视频和示例。

只需在 Windows 10 上[安装工具和 SDK](https://go.microsoft.com/fwlink/?LinkId=821431)，你便可以随时[创建新的通用 Windows 应用](../get-started/create-uwp-apps.md)，或了解如何使用 [Windows 上的现有应用代码](../porting/index.md)。

## <a name="features"></a>功能

### <a name="progressive-web-apps-on-windows"></a>Windows 上的渐进式 Web 应用

[渐进式 Web 应用 (PWA)](https://developer.microsoft.com/windows/pwa) 不过是[渐进增强](https://wikipedia.org/wiki/Progressive_enhancement)了支持平台和浏览器引擎上的本机类应用功能（例如从主屏幕启动的安装、脱机支持和推送通知）的 Web 应用。 在具有 Microsoft Edge (EdgeHTML) 引擎的 Windows 10 上，PWA 还享有[作为 UWP 应用独立于浏览器窗口](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/windows-features)运行的额外优势。

![运行中的 PWA 的图像](images/progressive-web-apps.jpg)

请参阅我们的 PWA 指南，完成以下操作：

* [将简单的 Web 应用生成为 PWA](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/get-started)
* [使用 Windows 运行时增强 PWA](https://docs.microsoft.com/en-us/microsoft-edge/progressive-web-apps/windows-features)
* [将你的 PWA 发布到 Microsoft Store](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/microsoft-store)

### <a name="notepad"></a>记事本

Windows 10 Insider 预览版 17713 中可用，[记事本已更新许多新功能](https://aka.ms/ant-man)。 [Windows 预览体验成员](https://insider.windows.com/)现可使用缩放、全部查找/替换，以及对 Unix/Linux (LF) 和 Mac (CR) 行尾的支持。 

## <a name="developer-guidance"></a>开发人员指南

### <a name="design-landing-page"></a>设计登陆页面

查看[已更新的设计登陆页面](https://developer.microsoft.com/windows/apps/design)大致了解 UWP 设计领域，以及有关 Fluent Design 最新新增功能的信息。

### <a name="design-toolkits"></a>设计工具包

Adobe XD 和 Adobe Illustrator 工具包已更新，添加了新功能。 这些设计工具包提供用于设计 UWP 应用的控件和布局模板。 [在此处查看相关内容。](../design/downloads/index.md)

### <a name="webvr"></a>WebVR

我们已将多个新主题添加到 [WebVR 文档](https://docs.microsoft.com/microsoft-edge/webvr/
)：

* [什么是 WebVR？](https://docs.microsoft.com/microsoft-edge/webvr/what-is-webvr
) 介绍 WebVR 是什么、使用原因，以及如何开始开发它。

* [渐进式 Web 应用中的 WebVR](https://docs.microsoft.com/microsoft-edge/webvr/webvr-in-pwas)：了解如何将 WebVR 添加到渐进式 Web 应用 (PWA)。

* [WebView 中的 WebVR](https://docs.microsoft.com/microsoft-edge/webvr/webvr-in-webview)：了解如何将 WebVR 添加到 Windows 10 应用程序中的 WebView 控件。

* [WebVR 演示](https://docs.microsoft.com/microsoft-edge/webvr/demos)：请使用 Microsoft Edge 和 Windows Mixed Reality 沉浸式头戴显示设备来查看 WebVR 演示。

此外，我们对现有页面进行了一些更新：

* 目录现改进为四个不同的顶级桶：基础知识、开发、资源和演示     。

* [WebVR 开发人员指南（登陆页）](https://docs.microsoft.com/microsoft-edge/webvr/)：新外观和感觉，具有更大的图像和图标以及新的演示。

* [将 WebVR 与 Microsoft Edge 结合使用](https://docs.microsoft.com/microsoft-edge/webvr/webvr-with-edge)：更新以包括关于 Windows 10 2018 年 4 月更新的信息。

## <a name="videos"></a>视频

### <a name="get-started-for-devs-create-and-customize-a-form-on-windows-10"></a>开发人员入门：在 Windows 10 上创建和自定义窗体

面向 Windows 开发人员的[入门文档](../get-started/index.md)现在提供基本应用开发任务的实践经验。 此视频将引导你了解其中一个主题，并介绍在应用中创建窗体 UI 的基本知识。 [观看视频](https://www.youtube.com/watch?v=AgngKzq4hKI&feature=youtu.be)，查看实际操作中使用的代码，然后[自行查看主题。](https://aka.ms/CreateForms)

### <a name="enhance-your-bot-with-project-personality-chat"></a>使用“项目个性化聊天”增强机器人

使用“项目个性化聊天”可将可自定义的角色添加到聊天机器人。 通过与 Microsoft Bot Framework SDK 集成，可以添加闲聊功能，从而以更随意的交谈方式与客户进行交互。 [观看视频](https://www.youtube.com/watch?v=5C_uD8g2QKg&feature=youtu.be)了解相关实现方法，然后[尝试互动演示](https://aka.ms/PersonalityChat)进行实践体验。

### <a name="one-dev-question"></a>一个开发问题

在“一个开发问题”视频系列中，资深的 Microsoft 开发人员介绍了关于 Windows 开发、团队文化和发展历程的一系列问题。 以下是我们已回答的最新问题！

Raymond Chen：

* [为什么向 Microsoft 申请？](https://www.youtube.com/watch?v=oL8ymamkEMU&feature=youtu.be)

Larry Osterman：

* [为什么我们不让开发人员更改默认的音频设备？](https://www.youtube.com/watch?v=6aNUoVfbnmg&feature=youtu.be)
* [为什么有大量 UWP 函数异步？](https://www.youtube.com/watch?v=5M724QIy1Mk&feature=youtu.be)

## <a name="samples"></a>示例

### <a name="photo-editor-cwinrt"></a>照片编辑器 C++/WinRT

照片编辑器示例应用通过 [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) 语言投影展示开发。 使用此应用，可以从“图片”库中检索照片，然后使用关联的照片效果来编辑所选图像  。 [在此处克隆或下载示例。](https://github.com/Microsoft/Windows-appsample-photo-editor)

![操作中的示例的示例](images/photo-editor-banner.png)
