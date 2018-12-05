---
Description: Introduces accessibility concepts that relate to Universal Windows Platform (UWP) apps.
ms.assetid: C89D79C2-B830-493D-B020-F3FF8EB5FFDD
title: 辅助功能
label: Accessibility
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 2f9cdfb8a20e273d5d9e5819fc1e28aba97e4296
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2018
ms.locfileid: "8692981"
---
# <a name="accessibility"></a>辅助功能  



介绍与通用 Windows 平台 (UWP) 应用相关的辅助功能概念。

辅助功能主要用于生成以下体验：让你的应用程序适用于在各种环境中使用技术并且对用户界面提出一系列需求和体验要求的用户。 对于某些情况，辅助功能要求由法律强制实施。 但是，无论法律是否有规定，最好都解决辅助功能问题，以尽可能扩大应用的受众范围。 其中还包括关于应用辅助功能的 Microsoft Store 声明。

> [!NOTE]
> 将应用声明为辅助应用仅与 Microsoft Store 有关。

| 文章 | 描述 |
|---------|-------------|
| [辅助功能概述](accessibility-overview.md) | 本文概述了与 UWP 应用的辅助功能方案相关的概念和技术。 |
| [设计非独占软件](designing-inclusive-software.md) | 了解如何改进适用于 Windows 10 的 UWP 应用的非独占设计。  以辅助功能思维设计和生成非独占软件。 |
| [开发非独占 Windows 应用](developing-inclusive-windows-apps.md) | 本文是开发辅助 UWP 应用的路线图。 |
| [辅助功能测试](accessibility-testing.md) | 为确保 UWP 应用为辅助应用所要遵循的测试过程。 |
| [应用商店中的辅助功能](accessibility-in-the-store.md) | 介绍有关在 Microsoft Store 中将 UWP 应用声明为辅助应用的要求。 |
| [辅助功能清单](accessibility-checklist.md) | 提供了可帮助你确保 UWP 应用为辅助应用的清单。 |
| [公开基本的辅助功能信息](basic-accessibility-information.md) | 基本的辅助功能信息通常按照名称、角色和值进行分类。 本主题介绍可帮助应用公开辅助技术所需的基本信息的代码。 |
| [键盘辅助功能](keyboard-accessibility.md) | 如果应用未提供良好的键盘访问，则盲人用户或行动不便的用户在使用该应用时会存在困难，或者可能根本无法使用该应用。 |
| [标志和标题](landmarks-and-headings.md) | 标志和标题定义帮助辅助技术（如屏幕阅读器）用户高效导航的用户界面的各个部分。 |
| [高对比度主题](high-contrast-themes.md) | 介绍了为确保 UWP 应用在高对比度主题处于活动状态时可供使用所需的步骤。 |
| [辅助文本要求](accessible-text-requirements.md) | 本主题介绍了应用中的文本的最佳辅助功能做法：确保颜色和背景满足必需的对比率。 本主题还讨论了 UWP 应用中的文本元素可以具有的 Microsoft UI 自动化角色，以及图形中文本的最佳做法。 |
| [要避免的辅助功能做法](practices-to-avoid.md) | 列出创建辅助的 UWP 应用时应避免的做法。 |
| [自定义自动化对等](custom-automation-peers.md) | 介绍 UI 自动化的自动化对等概念以及如何为自己的自定义 UI 类提供自动化支持。 |
| [控件模式和接口](control-patterns-and-interfaces.md) | 列出 Microsoft UI 自动化控件模式、客户端用于访问这些模式的类以及提供程序用于实现这些模式的接口。 |

## <a name="related-topics"></a>相关主题  
* [**Windows.UI.Xaml.Automation**](https://msdn.microsoft.com/library/windows/apps/BR209179) 
* [讲述人入门](https://support.microsoft.com/en-us/help/22798/windows-10-narrator-get-started)