---
author: QuinnRadich
title: 在 2018 年 7 月 Windows 文档中新增功能-开发 UWP 应用
description: 新功能、 视频、 示例和开发人员指南已被添加到 2018 年 7 月 Windows 10 开发人员文档。
keywords: 新增功能，更新，功能，开发人员指南，Windows 10 年 7 月
ms.author: quradic
ms.date: 7/11/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: b9c2ff7e809c635eb97e818c91e5d6647a963560
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2018
ms.locfileid: "5762063"
---
# <a name="whats-new-in-the-windows-developer-docs-in-july-2018"></a>什么是 Windows 开发人员文档中 2018 年 7 月中的新增功能

Windows 开发人员文档持续更新对整个 Windows 平台的开发人员提供的新功能的信息。 以下功能概述、 开发人员指南、 视频和示例已在 7 月的月份中可用。

只需在 Windows10 上[安装工具和 SDK](http://go.microsoft.com/fwlink/?LinkId=821431)，你便可以随时[创建新的通用 Windows 应用](../get-started/create-uwp-apps.md)，或了解如何使用 [Windows 上的现有应用代码](../porting/index.md)。

## <a name="features"></a>功能

### <a name="progressive-web-apps-on-windows"></a>在 Windows 上的渐进式 Web 应用

[渐进式 Web 应用 (Pwa)](https://developer.microsoft.com/windows/pwa)都只需通过本机类似于应用的功能，支持平台和浏览器引擎，例如启动从 homescreen 安装、 离线支持和推送[逐步增强](https://wikipedia.org/wiki/Progressive_enhancement)的 web 应用通知。 使用 Microsoft Edge (EdgeHTML) 引擎的 Windows 10，Pwa 享受运行优点[独立于浏览器窗口与 UWP 应用。](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/windows-features)

![Pwa 的操作中的图像](images/progressive-web-apps.jpg)

请查看我们 PWA 指南：

* [为 PWA 生成一个简单的 web 应用](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/get-started)
* [增强 Windows 运行时与你 PWA](https://docs.microsoft.com/en-us/microsoft-edge/progressive-web-apps/windows-features)
* [将你 PWA 发布到 Microsoft Store](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/microsoft-store)

### <a name="notepad"></a>记事本

在 Windows 10 Insider Preview 生成 17713，[许多新功能已更新记事本](http://aka.ms/ant-man)中可用。 缩放、 环绕查找/替换，以及对 Unix/Linux （换行符） 和 Mac （回车） 行尾支持现可向[Windows 预览体验成员](https://insider.windows.com/)。 

## <a name="developer-guidance"></a>开发人员指南

### <a name="design-landing-page"></a>设计登录页面

请查看[更新登录页的设计](https://developer.microsoft.com/windows/apps/design)为一个一览概述 UWP 设计区域，并添加到 Fluent 设计的最新功能的信息。

### <a name="design-toolkits"></a>设计工具包

Adobe XD 和 Adobe Illustrator 工具包已更新新功能。 这些设计工具包提供用于设计 UWP 应用的控件和布局模板。 [它们签出此处。](../design/downloads/index.md)

### <a name="webvr"></a>WebVR

我们已添加到[WebVR 文档](https://docs.microsoft.com/microsoft-edge/webvr/
)的几个新主题：

* [WebVR 是什么？](https://docs.microsoft.com/microsoft-edge/webvr/what-is-webvr
) 介绍了 WebVR 是什么，为什么应使用它，以及如何为其开发入门。

* [渐进式 Web 应用中的 WebVR](https://docs.microsoft.com/microsoft-edge/webvr/webvr-in-pwas)： 了解如何将 WebVR 添加到渐进式 Web 应用 (PWA)。

* [WebVR 在 web 视图中](https://docs.microsoft.com/microsoft-edge/webvr/webvr-in-webview)： 了解如何将 WebVR 添加到 Windows 10 应用中的 web 视图控件。

* [WebVR 演示](https://docs.microsoft.com/microsoft-edge/webvr/demos)： 签出一些 WebVR 演示使用 Microsoft Edge 和 Windows Mixed Reality 沉浸式头戴显示设备。

此外，我们已对现有页面进行一些更新：

* 目录中现在更好地分为四个不同的顶级存储桶：**基础知识**、**开发**、**资源**和**演示**。

* [WebVR 开发人员指南 （登录页）](https://docs.microsoft.com/microsoft-edge/webvr/)： 刷新的外观和感觉，具有较大的图像和图标和新演示。

* [使用 Microsoft Edge 使用 WebVR](https://docs.microsoft.com/microsoft-edge/webvr/webvr-with-edge)： 更新以包含有关 Windows 10 2018 年 4 月更新。

## <a name="videos"></a>视频

### <a name="get-started-for-devs-create-and-customize-a-form-on-windows-10"></a>要开始使用适用于开发人员： 创建和自定义 Windows 10 上的表单

我们的[入门文档](../get-started/index.md)适用于 Windows 开发人员现在提供基本应用开发任务动手的体验。 此视频将指导你通过一个这些主题的链接，并介绍有关在应用中创建窗体 UI 基础知识。 [观看视频](https://www.youtube.com/watch?v=AgngKzq4hKI&feature=youtu.be)以查看操作，然后中的代码[自行查看本主题。](http://aka.ms/CreateForms)

### <a name="enhance-your-bot-with-project-personality-chat"></a>增强你的项目个性聊天机器人

项目个性聊天，可以将自定义角色添加到你聊天机器人。 通过与 Microsoft 机器人框架 SDK 集成，你可以添加更多谈话地与客户交互的小访谈功能。 [观看视频](https://www.youtube.com/watch?v=5C_uD8g2QKg&feature=youtu.be)以了解如何实现它，然后动手来获取[试用交互式演示](http://aka.ms/PersonalityChat)。

### <a name="one-dev-question"></a>一个开发人员的问题

在开发人员的一个问题视频系列中，longtime Microsoft 开发人员介绍一系列有关 Windows 开发、 团队区域性和历史记录的问题。 下面是我们回答的最新问题 ！

Raymond Chen:

* [为什么未将应用到 Microsoft？](https://www.youtube.com/watch?v=oL8ymamkEMU&feature=youtu.be)

Larry Osterman:

* [我们为何不要让开发人员更改默认音频设备？](https://www.youtube.com/watch?v=6aNUoVfbnmg&feature=youtu.be)
* [为什么要有许多 UWP 功能异步？](https://www.youtube.com/watch?v=5M724QIy1Mk&feature=youtu.be)

## <a name="samples"></a>示例

### <a name="photo-editor-cwinrt"></a>照片编辑器 C + + WinRT

照片编辑器示例应用展示了使用开发[C + + WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md)语言投影。 该应用使你从**图片**库检索照片，然后编辑具有相关联的照片效果的选择的图像。 [克隆或下载下面的示例。](https://github.com/Microsoft/Windows-appsample-photo-editor)

![一种正在操作的示例](images/photo-editor-banner.png)
