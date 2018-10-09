---
author: QuinnRadich
title: 在 2018 年 8 月 Windows 文档中新增功能-开发 UWP 应用
description: 新功能、 视频、 示例和开发人员指南已添加到 2018 年 8 月 Windows 10 开发人员文档。
keywords: 新增功能，更新，功能，开发人员指南，Windows 10，8 月
ms.author: quradic
ms.date: 08/14/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: c294dedc8e19605bc2cee0308022bed8624df57e
ms.sourcegitcommit: 49aab071aa2bd88f1c165438ee7e5c854b3e4f61
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2018
ms.locfileid: "4466907"
---
# <a name="whats-new-in-the-windows-developer-docs-in-august-2018"></a>什么是 Windows 开发人员文档中 2018 年 8 月的新增功能

Windows 开发人员文档持续更新对整个 Windows 平台的开发人员提供的新功能的信息。 以下功能概述、 开发人员指南和视频进行了在 8 月的月份中可用。

只需在 Windows10 上[安装工具和 SDK](http://go.microsoft.com/fwlink/?LinkId=821431)，你便可以随时[创建新的通用 Windows 应用](../get-started/create-uwp-apps.md)，或了解如何使用 [Windows 上的现有应用代码](../porting/index.md)。

## <a name="features"></a>功能

### <a name="design"></a>设计

以下功能已添加到 Windows Insider Preview 版本，可通过[Windows 预览体验](https://insider.windows.com/)计划。

* [Windows UI 库](https://aka.ms/winui-docs)是一组提供适用于 UWP 应用的控件和其他用户 interfact 元素的 NuGet 程序包。 这些程序包也是使用早期版本的 Windows 10 兼容，因此即使你的用户没有设置的最新的操作系统版本的工作原理你的应用。

* [DropDownButton](../design/controls-and-patterns/buttons.md#create-a-drop-down-button)、[拆分按钮](../design/controls-and-patterns/buttons.md#create-a-split-button)，以及[ToggleSplitButton](../design/controls-and-patterns/buttons.md#create-a-toggle-split-button)提供专用的功能来增强你的应用的用户界面的按钮控件。

![用于选择前景色拆分按钮](../design/controls-and-patterns/images/split-button-rtb.png)

* NavigationView 现在支持[顶部导航](../design/controls-and-patterns/navigationview.md)，对于在其中你的应用具有较少的导航选项，并且需要更多的空间用于应用的内容的情况。

* 树视图已增强，以支持[数据绑定，项模板，并将拖放。](../design/controls-and-patterns/tree-view.md)

### <a name="package-support-framework"></a>包支持框架

包支持框架是可帮助你修复时应用到 win32 应用程序不能访问的源代码，以便它可以在 MSIX 容器中运行的开源工具包。

若要了解详细信息，请参阅[应用运行时修复到使用程序包支持框架 MSIX 包](../porting/package-support-framework.md)。

## <a name="developer-guidance"></a>开发人员指南

### <a name="web-api-extensions"></a>Web API 扩展

已添加到 Mozilla Developer Network 文档中针对跨浏览器 web 开发的[旧 Microsoft API 扩展](https://developer.mozilla.org/docs/Web/API/Microsoft_API_extensions)列表。 这些 API 扩展特定于 Internet Explorer 或 Microsoft Edge，并补充有关 MDN web 文档中的兼容性和浏览器支持的现有信息。传统的 Microsoft [CSS 扩展](https://developer.mozilla.org/docs/Web/CSS/Microsoft_Extensions)和[JavaScript 扩展](https://developer.mozilla.org/docs/Web/JavaScript/Microsoft_JavaScript_extensions)也是可用，并且可以找到丰富的 web API 信息从 MDN 直接在呈现[Visual Studio Code。](https://code.visualstudio.com/updates/v1_25#_new-css-pseudo-selectors-and-pseudo-elements-from-mdn)

### <a name="cwinrt-code-examples"></a>C + + /winrt 代码示例

我们添加了 250 [C + + WinRT](../cpp-and-winrt-apis/index.md)代码一览主题中我们的文档，附带的现有 C + + CX 代码示例。

### <a name="project-rome"></a>Project Rome

[项目 rome 文档](https://docs.microsoft.com/windows/project-rome/)站点具有已重新整理到功能第一种方法。 这应该便于开发人员可以找到他们要查找的并跨多个平台实现他们所选的功能。

## <a name="videos"></a>视频

### <a name="xbox-live-unity-plugin"></a>Xbox Live Unity 插件

Xbox Live 的 Unity 插件包含向你的游戏添加 Xbox Live 签名、 统计数据、 好友列表、 云存储和排行榜的支持。 [观看视频](https://youtu.be/fVQZ-YgwNpY)以了解更多信息，然后[下载 GitHub 程序包](https://aka.ms/UnityPlugin)若要开始使用。

### <a name="one-dev-question"></a>一个开发人员的问题

在开发人员的一个问题视频系列中，longtime Microsoft 开发人员介绍一系列有关 Windows 开发、 团队区域性和历史记录的问题。 下面是我们回答的最新问题 ！

Raymond Chen:

* [内核如何知道何时重新启动视频驱动程序？](https://youtu.be/3SNAdyO1l5c)

Larry Osterman:

* [什么是 Windows 中的 Burgermaster 对象背后的故事？](https://youtu.be/0TDSbyAIvX0)