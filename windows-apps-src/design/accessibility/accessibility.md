---
Description: 介绍与 Windows 应用相关的辅助功能概念。
ms.assetid: C89D79C2-B830-493D-B020-F3FF8EB5FFDD
title: 辅助功能
label: Accessibility
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0e410094f738860e71dadb960fccbdbc59306050
ms.sourcegitcommit: 87fd0ec1e706a460832b67f936a3014f0877a88c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/12/2020
ms.locfileid: "83234144"
---
# <a name="accessibility"></a>辅助功能  

辅助功能与构建体验相关，使你的 Windows 应用程序可供使用各种环境中技术的人员使用，并为你的 UI 提供各种需求和体验。 对于某些情况，辅助功能要求由法律强制实施。 但是，无论法律是否有规定，最好都解决辅助功能问题，以尽可能扩大应用的受众范围。

> 还有一个与应用的辅助功能有关的 Microsoft Store 声明！

| 文章 | 说明 |
|---------|-------------|
| [辅助功能概述](accessibility-overview.md) | 本文概述了与 Windows 应用的辅助功能方案相关的概念和技术。 |
| [设计非独占软件](designing-inclusive-software.md) | 了解适用于 Windows 10 的 Windows 应用的不断发展的包含设计。  以辅助功能为中心来设计和生成非独占软件。 |
| [开发非独占 Windows 应用](developing-inclusive-windows-apps.md) | 本文是用于开发可访问的 Windows 应用的路线图。 |
| [辅助功能测试](accessibility-testing.md) | 要遵循的测试过程，以确保你的 Windows 应用可供访问。 |
| [应用商店中的辅助功能](accessibility-in-the-store.md) | 描述在 Microsoft Store 中声明 Windows 应用程序可访问的要求。 |
| [辅助功能清单](accessibility-checklist.md) | 提供了一个核对清单，可帮助你确保 Windows 应用程序可访问。 |
| [公开基本的辅助功能信息](basic-accessibility-information.md) | 基本的辅助功能信息通常按照名称、角色和值进行分类。 本主题介绍了可帮助应用公开辅助技术所需的基本信息的代码。 |
| [键盘辅助功能](keyboard-accessibility.md) | 如果应用未提供良好的键盘访问，则盲人用户或行动不便的用户在使用该应用时会存在困难，或者可能根本无法使用该应用。 |
| [屏幕阅读器和硬件系统按钮](system-button-narration.md) | 屏幕阅读器（如[讲述人](https://support.microsoft.com/en-us/help/22798/windows-10-complete-guide-to-narrator)）必须能够识别和处理硬件系统按钮事件并将其状态传达给用户。 在某些情况下，屏幕阅读器可能需要以独占方式处理按钮事件，而不会让它们向上冒泡到其他处理程序。 |
| [标志和标题](landmarks-and-headings.md) | 标志和标题定义帮助辅助技术（如屏幕阅读器）用户高效导航的用户界面的各个部分。 |
| [高对比度主题](high-contrast-themes.md) | 描述在高对比度主题处于活动状态时，确保您的 Windows 应用程序可用所需的步骤。 |
| [辅助文本要求](accessible-text-requirements.md) | 本主题介绍了应用中的文本的最佳辅助功能做法：确保颜色和背景满足必需的对比率。 本主题还讨论了 Windows 应用中的文本元素可以具有的 Microsoft UI 自动化角色，以及图形中文本的最佳方案。 |
| [要避免的辅助功能做法](practices-to-avoid.md) | 列出要在创建可访问的 Windows 应用时要避免的做法。 |
| [自定义的自动化对等](custom-automation-peers.md) | 介绍 UI 自动化的自动化对等概念以及如何为自己的自定义 UI 类提供自动化支持。 |
| [控件模式和接口](control-patterns-and-interfaces.md) | 列出 Microsoft UI 自动化控件模式、客户端用于访问这些模式的类以及提供程序用于实现这些模式的接口。 |

## <a name="related-topics"></a>相关主题  
* [**Windows。**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation) 
* [讲述人入门](https://support.microsoft.com/help/22798/windows-10-complete-guide-to-narrator)
