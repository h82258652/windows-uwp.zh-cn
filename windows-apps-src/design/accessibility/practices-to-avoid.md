---
Description: 列出创建辅助通用 Windows 平台 (UWP) 应用时要避免的做法。
ms.assetid: 024A9B70-9821-45BB-93F1-61C0B2ECF53E
title: 要避免的辅助功能做法
label: Accessibility practices to avoid
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 289c5f92d8f67e1b34ea399d782192135a108496
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2019
ms.locfileid: "66359505"
---
# <a name="accessibility-practices-to-avoid"></a>要避免的辅助功能做法

如果你要创建辅助通用 Windows 平台 (UWP) 应用，请参阅此应避免的做法列表： 

* 如果你可以使用默认 Windows 控件或已实现 Microsoft UI 自动化支持的控件，**请避免生成自定义 UI 元素**。 标准 Windows 控件默认具有辅助性，并且通常只需添加几个特定于应用的辅助功能属性即可。 相反，为真正的自定义控件实现 [**AutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer) 支持会更为复杂（请参阅[自定义自动化对等](custom-automation-peers.md)）。
* **不要在 Tab 键顺序中放置静态文本或其他非交互元素**（例如，通过为非交互元素设置 [**TabIndex**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.tabindex) 属性）。 如果非交互元素在 Tab 键顺序中，这将违背键盘辅助功能指南，因为它会降低用户进行键盘导航的效率。 许多辅助技术使用 Tab 键顺序和元素聚焦功能，以此作为它们向辅助技术用户呈现应用界面的一部分逻辑。 Tab 键顺序中的纯文本元素会对预期 Tab 键顺序中只有交互元素（例如按钮、复选框、文本输入字段、组合框、列表等）的用户造成困惑。
* **避免使用 UI 元素的绝对定位**（例如在 [**Canvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Canvas) 元素中），因为表示顺序通常与子元素的声明顺序（即实际逻辑顺序）不同。 尽可能以文档或逻辑顺序排列 UI 元素，以确保屏幕阅读器能够以正确顺序阅读这些元素。 如果 UI 元素的可见顺序可以不同于文档顺序或逻辑顺序，请使用明确的 Tab 键索引值（设置 [**TabIndex**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.tabindex)）来定义正确的阅读顺序。
* **不要使用颜色作为传达信息的唯一方法。** 色盲用户无法接收仅通过颜色传递的信息，例如颜色状态指示器。 包括其他视觉提示（最好是文本），以确保信息是辅助信息。
* 除非应用功能的确需要，否则**不要自动刷新整个应用画布**。 如果需要自动刷新页面内容，请仅更新页面的某些区域。 辅助技术通常必须假设已刷新的应用画布是全新的结构，即使有效的更改非常小也是如此。 对于辅助技术用户来说，这样做的代价是已刷新的应用的任何文档视图或说明现在必须重新创建并再次呈现给用户。
  
  用户有意启动页面导航是刷新应用结构的合法情况。 但是，请确保启动导航的 UI 项已正确标识或命名，从而指示调用它将导致上下文更改和页面重新加载。

  > [!NOTE]
  > 如果你确实刷新某个区域中的内容，请考虑将该元素上的 [**AccessibilityProperties.LiveSetting**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.automationproperties.livesettingproperty) 辅助功能属性设置为非默认设置 **Polite** 或 **Assertive** 之一。 某些辅助技术可以将此设置映射到活动区域的可访问富 Internet 应用程序 (ARIA) 概念，并因此通知用户某个内容区域已经发生更改。

* **不要使用 UI 元素的闪存三倍以上每秒。** 元素闪烁会致使一些人癫痫发作。 最好是避免使用闪烁的 UI 元素。
* **不会更改用户上下文或自动激活功能。** 仅当用户对具有焦点的 UI 元素直接操作时才会出现上下文或激活更改。 用户上下文更改包括更改焦点，显示新内容以及导航至其他页面。 在无需用户干预的情况下进行上下文更改会使残疾用户不知所措。 此要求的例外情况包括：显示子菜单、验证表单、显示其他控件中的帮助文本以及更改上下文以响应异步事件。

<span id="related_topics"/>

## <a name="related-topics"></a>相关主题  
* [辅助功能](accessibility.md)
* [应用商店中的辅助功能](accessibility-in-the-store.md)
* [可访问性清单](accessibility-checklist.md)
