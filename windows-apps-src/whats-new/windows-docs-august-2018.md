---
title: 什么是 Windows 文档中 2018 年 8 月中的新增功能-开发 UWP 应用
description: 新功能、 视频、 示例和开发人员指南具有已添加到 2018 年 8 月的 Windows 10 开发人员文档。
keywords: 最新内容、 更新、 功能、 开发人员指南，Windows 10 年 8 月
ms.date: 08/14/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9922aa1ad2442153dcc2c13d05520c05c3b56d31
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57616482"
---
# <a name="whats-new-in-the-windows-developer-docs-in-august-2018"></a>什么是 Windows 开发人员文档中在 2018 年 8 月的新增功能

Windows 开发人员文档持续更新对整个 Windows 平台的开发人员提供的新功能的信息。 以下功能概述、 开发人员指南和视频进行了年 8 月的月份中可用。

只需在 Windows 10 上[安装工具和 SDK](https://go.microsoft.com/fwlink/?LinkId=821431)，你便可以随时[创建新的通用 Windows 应用](../get-started/create-uwp-apps.md)，或了解如何使用 [Windows 上的现有应用代码](../porting/index.md)。

## <a name="features"></a>功能

### <a name="design"></a>设计

以下功能已添加到 Windows Insider Preview 版本中，可通过[Windows Insider](https://insider.windows.com/)程序。

* [Windows 用户界面库](https://aka.ms/winui-docs)是一组控件和其他用户 interfact 元素为 UWP 应用提供的 NuGet 包。 这些包也是与早期版本的 Windows 10 兼容，因此即使用户不具有最新的操作系统版本，适用于您的应用程序。

* [DropDownButton](../design/controls-and-patterns/buttons.md#create-a-drop-down-button)， [SplitButton](../design/controls-and-patterns/buttons.md#create-a-split-button)，和[ToggleSplitButton](../design/controls-and-patterns/buttons.md#create-a-toggle-split-button)按钮控件提供专用功能，以增强您的应用程序的用户界面。

![用于选择前景色拆分按钮](../design/controls-and-patterns/images/split-button-rtb.png)

* NavigationView 现在支持[排名靠前导航](../design/controls-and-patterns/navigationview.md)，在其中您的应用程序具有较小数量的导航选项，且需要更多空间来存储应用程序的内容的用例。

* 树视图已经过增强，支持[数据绑定，项模板，并拖放。](../design/controls-and-patterns/tree-view.md)

### <a name="package-support-framework"></a>包支持框架

包支持框架是一个开放源代码工具包，可帮助你应用修补程序到 win32 应用程序时不能访问源代码，以便它可以在 MSIX 容器中运行。

若要了解详细信息，请参阅[应用运行时修补程序通过使用包支持框架 MSIX 包](../porting/package-support-framework.md)。

## <a name="developer-guidance"></a>开发人员指南

### <a name="web-api-extensions"></a>Web API 扩展

一系列[旧 Microsoft API 扩展](https://developer.mozilla.org/docs/Web/API/Microsoft_API_extensions)已添加到跨浏览器 web 开发的 Mozilla 开发人员网络文档。 这些 API 扩展 Internet Explorer 或 Microsoft Edge 是唯一的并补充了 MDN web 文档中的兼容性和浏览器支持的现有信息。旧版 Microsoft [CSS 扩展](https://developer.mozilla.org/docs/Web/CSS/Microsoft_Extensions)并[JavaScript 扩展](https://developer.mozilla.org/docs/Web/JavaScript/Microsoft_JavaScript_extensions)也可用，并且您可以找到丰富的 web API 信息从 MDN 中直接显示[Visual Studio Code。](https://code.visualstudio.com/updates/v1_25#_new-css-pseudo-selectors-and-pseudo-elements-from-mdn)

### <a name="cwinrt-code-examples"></a>C + + WinRT 代码示例

我们添加了 250 [C + + WinRT](../cpp-and-winrt-apis/index.md)代码列表主题中我们的文档，随附的现有 C + + /cli CX 代码示例。

### <a name="project-rome"></a>Project Rome

[项目罗马 docs](https://docs.microsoft.com/windows/project-rome/)已重新站点组织为功能为先的方法。 这应使开发人员查找所需的以及跨多个平台实现他们选择的功能更轻松。

## <a name="videos"></a>视频

### <a name="xbox-live-unity-plugin"></a>Xbox Live Unity 插件

适用于 Unity Xbox Live 插件包含支持将 Xbox Live 签名、 统计信息、 好友列表、 云存储和排行榜添加到你的标题。 [观看视频](https://youtu.be/fVQZ-YgwNpY)若要了解详细信息，然后[下载 GitHub 包](https://aka.ms/UnityPlugin)若要开始。

### <a name="one-dev-question"></a>一个适用于开发人员问题

在开发人员的一个问题视频系列中，资深 Microsoft 开发者介绍一系列的有关 Windows 开发、 团队区域性和历史记录的问题。 下面是我们回答的最新问题 ！

Raymond Chen:

* [内核如何知道何时重新启动视频驱动程序？](https://youtu.be/3SNAdyO1l5c)

Larry Osterman:

* [什么是 Windows 中的 Burgermaster 对象背后的故事？](https://youtu.be/0TDSbyAIvX0)