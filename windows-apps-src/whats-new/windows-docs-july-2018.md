---
title: 什么是 Windows 文档中 2018 年 7 月中的新增功能-开发 UWP 应用
description: 新功能、 视频、 示例和开发人员指南具有已添加到 2018 年 7 月的 Windows 10 开发人员文档。
keywords: 最新内容、 更新、 功能、 开发人员指南，Windows 10 年 7 月
ms.date: 07/11/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 22a3a9614a4488791a36f81a3d4dedac572111b4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57596832"
---
# <a name="whats-new-in-the-windows-developer-docs-in-july-2018"></a>什么是 Windows 开发人员文档中在 2018 年 7 月的新增功能

Windows 开发人员文档持续更新对整个 Windows 平台的开发人员提供的新功能的信息。 以下功能概述、 开发人员指南、 视频和示例进行了 7 月中可用。

只需在 Windows 10 上[安装工具和 SDK](https://go.microsoft.com/fwlink/?LinkId=821431)，你便可以随时[创建新的通用 Windows 应用](../get-started/create-uwp-apps.md)，或了解如何使用 [Windows 上的现有应用代码](../porting/index.md)。

## <a name="features"></a>功能

### <a name="progressive-web-apps-on-windows"></a>在 Windows 上的渐进式 Web 应用

[渐进式 Web 应用 (Pwa)](https://developer.microsoft.com/windows/pwa)是只需将 web 应用[渐进式增强](https://wikipedia.org/wiki/Progressive_enhancement)上支持的平台和浏览器引擎，如启动从 homescreen 安装脱机的本机应用类似的功能支持，并将推送通知。 在 Windows 10 中与 Microsoft Edge (EdgeHTML) 引擎，Pwa 享受正在运行的附加的优势[独立于浏览器窗口中为 UWP 应用。](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/windows-features)

![Pwa 的操作中的图像](images/progressive-web-apps.jpg)

请查看我们 PWA 指南：

* [为 PWA 中生成一个简单的 web 应用](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/get-started)
* [增强与 Windows 运行时在 PWA](https://docs.microsoft.com/en-us/microsoft-edge/progressive-web-apps/windows-features)
* [将在 PWA 发布到 Microsoft Store](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/microsoft-store)

### <a name="notepad"></a>记事本

在 Windows 10 Insider Preview 构建 17713，可用[已具有许多新功能更新记事本](https://aka.ms/ant-man)。 缩放、 自动换行围绕查找/替换和对 Unix/Linux (LF) 和 Mac (CR) 行尾的支持目前可供[Windows 预览体验成员](https://insider.windows.com/)。 

## <a name="developer-guidance"></a>开发人员指南

### <a name="design-landing-page"></a>设计登陆页面

请查看[更新登录页设计](https://developer.microsoft.com/windows/apps/design)一眼，大致了解 UWP 设计区域和 Fluent 设计的最新功能的信息。

### <a name="design-toolkits"></a>设计工具包

Adobe XD 和 Adobe Illustrator 工具包已更新的新功能。 这些设计工具包为设计 UWP 应用提供控件和布局模板。 [请其查看此处。](../design/downloads/index.md)

### <a name="webvr"></a>WebVR

我们已添加到几个新主题[WebVR 文档](https://docs.microsoft.com/microsoft-edge/webvr/
):

* [什么是 WebVR？](https://docs.microsoft.com/microsoft-edge/webvr/what-is-webvr
) 介绍 WebVR 是什么、 为什么你应该使用它，以及如何为其开发快速入门。

* [渐进式 Web 应用中的 WebVR](https://docs.microsoft.com/microsoft-edge/webvr/webvr-in-pwas):了解如何将 WebVR 添加到渐进式 Web App (PWA)。

* [WebView 中的 WebVR](https://docs.microsoft.com/microsoft-edge/webvr/webvr-in-webview):了解如何将 WebVR 添加到 Windows 10 应用程序中的 WebView 控件。

* [WebVR 演示](https://docs.microsoft.com/microsoft-edge/webvr/demos):请查看使用 Microsoft Edge 和 Windows Mixed Reality 沉浸式头戴式一些 WebVR 演示。

此外，我们对某些更新现有页面：

* 目录现在更好地划分为四个不同的顶级存储桶：**基础知识**，**开发**，**资源**，并且**演示**。

* [WebVR 开发人员指南 （登录页）](https://docs.microsoft.com/microsoft-edge/webvr/):全新的外观，其中包含较大的图像和图标和演示。

* [使用 Microsoft Edge 使用 WebVR](https://docs.microsoft.com/microsoft-edge/webvr/webvr-with-edge):经过更新以包含有关 Windows 10 2018 年 4 月更新。

## <a name="videos"></a>视频

### <a name="get-started-for-devs-create-and-customize-a-form-on-windows-10"></a>开始进行开发人员：创建和自定义 Windows 10 上的窗体

我们[开始 docs](../get-started/index.md)为 Windows 开发人员现在向亲身体验提供基本的应用开发任务。 此视频将引导你完成这些主题之一，并介绍了在您的应用程序中创建窗体用户界面的基础知识。 [观看视频](https://www.youtube.com/watch?v=AgngKzq4hKI&feature=youtu.be)若要查看在操作中，然后代码[自己查看主题。](https://aka.ms/CreateForms)

### <a name="enhance-your-bot-with-project-personality-chat"></a>增强项目的个性聊天的机器人

项目的个性聊天，可以将可自定义角色添加到聊天机器人。 通过与 Microsoft Bot Framework SDK 集成，可以添加以更对话的方式与客户进行交互的小型对话功能。 [观看视频](https://www.youtube.com/watch?v=5C_uD8g2QKg&feature=youtu.be)若要了解如何实现它，然后[试用互动演示](https://aka.ms/PersonalityChat)动手体验。

### <a name="one-dev-question"></a>一个适用于开发人员问题

在开发人员的一个问题视频系列中，资深 Microsoft 开发者介绍一系列的有关 Windows 开发、 团队区域性和历史记录的问题。 下面是我们回答的最新问题 ！

Raymond Chen:

* [为什么未将应用到 Microsoft？](https://www.youtube.com/watch?v=oL8ymamkEMU&feature=youtu.be)

Larry Osterman:

* [我们为什么不让开发人员更改默认音频设备？](https://www.youtube.com/watch?v=6aNUoVfbnmg&feature=youtu.be)
* [为什么会这么多 UWP 函数异步？](https://www.youtube.com/watch?v=5M724QIy1Mk&feature=youtu.be)

## <a name="samples"></a>示例

### <a name="photo-editor-cwinrt"></a>照片编辑器 C + + WinRT

照片编辑器示例应用演示了如何使用开发[C + + WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md)语言投影。 应用程序允许你检索从照片**图片**库，然后编辑所选图像具有关联的照片效果。 [克隆或下载此处的示例。](https://github.com/Microsoft/Windows-appsample-photo-editor)

![下面举例说明正在操作的示例](images/photo-editor-banner.png)
