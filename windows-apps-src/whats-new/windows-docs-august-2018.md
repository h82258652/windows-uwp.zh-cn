---
author: QuinnRadich
title: What's New in Windows 文档中年 8 月 2018年-开发 UWP 应用程序
description: 新功能、 视频、 示例和开发人员指南均已添加到年 8 月 2018年 Windows 10 开发人员文档。
keywords: what's new，更新、 功能、 Windows 10，八月的开发人员指南
ms.author: quradic
ms.date: 8/9/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 06eef0c115675ba9673a81459c91e0f08f6fab71
ms.sourcegitcommit: be5b71a8ec7b686d5f93d56d10cb9a50c3c5bb4a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/14/2018
ms.locfileid: "2748862"
---
# <a name="whats-new-in-the-windows-developer-docs-in-august-2018"></a>What's New in Windows 开发人员文档中年 8 月 2018

Windows 开发人员文档持续更新对整个 Windows 平台的开发人员提供的新功能的信息。 以下功能概述、 开发人员指南和视频进行了八月中可用。

只需在 Windows10 上[安装工具和 SDK](http://go.microsoft.com/fwlink/?LinkId=821431)，你便可以随时[创建新的通用 Windows 应用](../get-started/create-uwp-apps.md)，或了解如何使用 [Windows 上的现有应用代码](../porting/index.md)。

## <a name="features"></a>功能

### <a name="design"></a>设计

以下功能已添加到 Windows 内幕预览版本，可通过[Windows 内幕](https://insider.windows.com/)计划。

* [Windows 用户界面库](https://aka.ms/winui-docs)是一套 UWP 应用程序提供控件和其他用户 interfact 元素的 NuGet 程序包。 这些包也是与早期版本的 Windows 10 兼容，因此即使用户不具有最新的操作系统版本，适用于您的应用程序。

* [DropDownButton、 拆分按钮和 ToggleSplitButton](../design/controls-and-patterns/buttons.md)提供与专用功能来增强您的应用程序用户体验的按钮控件。

* NavigationView 现在用您的应用程序具有导航选项数量较少的情况下支持[顶部导航中，](../design/controls-and-patterns/navigationview.md)并要求您的应用程序的内容的更多空间。

* 树视图得到了增强，支持[数据绑定、 项目模板和龙放。](../design/controls-and-patterns/tree-view.md)

![用于选择前景色的拆分按钮](../design/controls-and-patterns/images/split-button-rtb.png)

### <a name="package-support-framework"></a>包支持框架

包支持框架是可帮助您应用修补程序到 win32 应用程序时不能访问的源代码，以便它可以运行 MSIX 容器中打开源工具包。  

若要了解详细信息，请参阅[向使用 Package 支持框架的 MSIX 包修复应用运行时](../porting/package-support-framework.md)。

## <a name="developer-guidance"></a>开发人员指南

### <a name="web-api-extensions"></a>Web API 扩展

跨浏览器的 web 开发的 Mozilla Developer Network 文档已添加的[旧的 Microsoft API 扩展名](https://developer.mozilla.org/docs/Web/API/Microsoft_API_extensions)列表。 这些 API 扩展所特有的 Internet Explorer 或 Microsoft 边缘和现有 MDN web 文档中的兼容性和浏览器支持信息进行了补充。旧的 Microsoft [CSS 扩展](https://developer.mozilla.org/docs/Web/CSS/Microsoft_Extensions)和[JavaScript 扩展](https://developer.mozilla.org/docs/Web/JavaScript/Microsoft_JavaScript_extensions)也是可用，并且可以找到丰富 web MDN API 信息直接中显示[Visual Studio 代码。](https://code.visualstudio.com/updates/v1_25#_new-css-pseudo-selectors-and-pseudo-elements-from-mdn)

### <a name="cwinrt-code-examples"></a>C + + / WinRT 代码示例

我们已添加 250 [C + + / WinRT](../cpp-and-winrt-apis/index.md)代码段主题在我们的文档，随附现有 C + + / CX 代码示例。

### <a name="project-rome"></a>Project Rome

到功能优先方法重新组织了[项目 Rome 文档](https://docs.microsoft.com/windows/project-rome/)网站。 这应该更便于开发人员可以找到这些查找，并跨多个平台实现其选择的功能。

## <a name="videos"></a>视频

### <a name="xbox-live-unity-plugin"></a>Xbox Live 集体插件

完全一致的 Xbox Live 插件包含支持将 Xbox Live 签名、 状态、 好友列表、 云存储和排名添加到您的标题。 [观看视频](https://youtu.be/fVQZ-YgwNpY)了解更多信息，然后[下载 GitHub 包](https://aka.ms/UnityPlugin)若要开始。

### <a name="one-dev-question"></a>一个开发问题

在一个开发问题视频系列，longtime Microsoft 开发人员介绍一系列问题有关 Windows 开发、 团队区域性和历史记录。 下面是我们回答的最新问题 ！

Raymond Chen:

* [内核如何知道何时重新启动视频驱动程序？](https://youtu.be/3SNAdyO1l5c)

Larry Osterman:

* [在 Windows 中 Burgermaster 对象后面部分是什么？](https://youtu.be/0TDSbyAIvX0)