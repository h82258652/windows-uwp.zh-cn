---
title: 2018 年 8 月 Windows 文档中的新增功能 - 开发 UWP 应用
description: 新增功能、视频、示例和开发人员指南已添加到 2018 年 8 月 Windows 10 开发人员文档。
keywords: 新增功能, 更新, 功能, 开发人员指南, Windows 10, 八月
ms.date: 08/14/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 723cae783ba16fe5be9bb2076f96d4a5f823d733
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258837"
---
# <a name="whats-new-in-the-windows-developer-docs-in-august-2018"></a>2018 年 8 月 Windows 开发人员文档中的新增功能

Windows 开发人员文档持续更新对整个 Windows 平台的开发人员提供的新功能的信息。 八月当月提供了以下功能概述、开发人员指南和视频。

只需在 Windows 10 上[安装工具和 SDK](https://developer.microsoft.com/windows/downloads#_blank)，你便可以随时[创建新的通用 Windows 应用](../get-started/create-uwp-apps.md)，或了解如何使用 [Windows 上的现有应用代码](../porting/index.md)。

## <a name="features"></a>功能

### <a name="design"></a>设计

Windows 预览体验成员预览版本增加了以下功能，可通过 [Windows 预览体验成员](https://insider.windows.com/)计划获取。

* [Windows UI 库](https://docs.microsoft.com/uwp/toolkits/winui/)是一组 NuGet 程序包，提供用于 UWP 应用的控件和其他用户界面元素。 这些程序包还与 Windows 10 的早期版本兼容，所以即使用户未使用最新操作系统版本，应用也可以正常工作。

* [DropDownButton](../design/controls-and-patterns/buttons.md#create-a-drop-down-button)、[SplitButton](../design/controls-and-patterns/buttons.md#create-a-split-button) 和 [ToggleSplitButton](../design/controls-and-patterns/buttons.md#create-a-toggle-split-button) 提供具有专门功能的按钮控件，可增强应用的用户界面。

![用于选择前景色的拆分按钮](../design/controls-and-patterns/images/split-button-rtb.png)

* NavigationView 现在支持[顶部导航栏](../design/controls-and-patterns/navigationview.md)，其适用情况为应用的导航选项较少以及应用的内容部分需要更多空间。

* TreeView 已经过增强，可支持[数据绑定、项模板和拖放。](../design/controls-and-patterns/tree-view.md)

### <a name="package-support-framework"></a>包支持框架

包支持框架是一个开放源代码工具包，有助于在无权访问源代码时将修补程序应用于 win32 应用程序，以便其在 MSIX 容器中运行。

若要了解详细信息，请参阅[使用包支持框架将运行时修补程序应用到 MSIX 包](../porting/package-support-framework.md)。

## <a name="developer-guidance"></a>开发人员指南

### <a name="web-api-extensions"></a>Web API 扩展

[旧版 Microsoft API 扩展](https://developer.mozilla.org/docs/Web/API/Microsoft_API_extensions)列表已添加到 Mozilla 开发人员网络文档中，用于进行跨浏览器 Web 开发。 这些 API 扩展是 Internet Explorer 或 Microsoft Edge 所独有的，补充了 MDN Web 文档中有关兼容性和浏览器支持的现有信息。也可使用旧版 Microsoft [CSS 扩展](https://developer.mozilla.org/docs/Web/CSS/Microsoft_Extensions)和 [JavaScript 扩展](https://developer.mozilla.org/docs/Web/JavaScript/Microsoft_JavaScript_extensions)，并且可在 [Visual Studio Code](https://code.visualstudio.com/updates/v1_25#_new-css-pseudo-selectors-and-pseudo-elements-from-mdn) 中直接显示的 MDN 中找到大量 Web API 信息。

### <a name="cwinrt-code-examples"></a>C++/WinRT 代码示例

我们在文档中向主题添加了 250 个 [C++/WinRT](../cpp-and-winrt-apis/index.md) 代码列表，并随之附带现有的 C++/CX 代码示例。

### <a name="project-rome"></a>Project Rome

[Project Rome 文档](https://docs.microsoft.com/windows/project-rome/)站点已重组为功能优先的方法。 这将使开发人员可以更轻松地查找到所需内容，并跨多个平台实现所选功能。

## <a name="videos"></a>视频

### <a name="xbox-live-unity-plugin"></a>Xbox Live Unity 插件

适用于 Unity 的 Xbox Live 插件支持将 Xbox Live 签名、统计信息、好友列表、云存储和排行榜添加到标题。 [观看视频](https://youtu.be/fVQZ-YgwNpY)了解详细信息，然后[下载 GitHub 包](https://aka.ms/UnityPlugin)以开始使用。

### <a name="one-dev-question"></a>一个开发问题

在“一个开发问题”视频系列中，资深的 Microsoft 开发人员介绍了关于 Windows 开发、团队文化和发展历程的一系列问题。 以下是我们已回答的最新问题！

Raymond Chen：

* [内核如何知道何时重启视频驱动程序？](https://youtu.be/3SNAdyO1l5c)

Larry Osterman：

* [Windows 中的 Burgermaster 对象有什么故事？](https://youtu.be/0TDSbyAIvX0)