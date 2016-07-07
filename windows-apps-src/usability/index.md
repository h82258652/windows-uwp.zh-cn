---
description: "本部分中的设计和编码说明可帮助你针对特定类型的输入和设备自定义 UWP 应用。"
title: "UWP 应用中的可用性 - Windows 应用开发"
author: mijacobs
translationtype: Human Translation
ms.sourcegitcommit: 9f75c39d26bd0c8858f404ab4fcd3d23562ea033
ms.openlocfilehash: f02713dfee278866af53c6dd529d2faa3e9f625c

---

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

# UWP 应用中的可用性

UWP 应用可自动处理各种各样的输入并在各种设备上运行，例如，无需执行任何额外操作即可支持触摸输入或让你的应用在手机上运行。 

但是，有时你可能希望为特定类型的输入或设备优化应用。 例如，如果你要创建绘画应用，可能需要自定义处理笔输入的方式。 

本部分中的设计和编码说明可帮助你针对特定类型的输入和设备自定义 UWP 应用。 

## 辅助功能

辅助功能是指应用可供那些对使用常规用户界面存在限制（阻碍或阻止他们使用）的用户使用。 对于某些情况，辅助功能要求是由法律强制实施的。 但是，无论法律是否有规定，最好都解决辅助功能问题，以尽可能扩大应用的受众范围。

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[辅助功能概述](../accessibility/accessibility-overview.md)</b> <br/> 本文概述了与 UWP 应用的辅助功能方案相关的概念和技术。</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[设计非独占软件](../accessibility/designing-inclusive-software.md)</b><br/>了解如何改进适用于 Windows 10 的通用 Windows 平台 (UWP) 应用的非独占设计。  以辅助功能为中心来设计和生成非独占软件。</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[开发非独占 Windows 应用](../accessibility/developing-inclusive-windows-apps.md)</b><br/> 本文是开发辅助 UWP 应用的路线图。</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[辅助功能测试](../accessibility/accessibility-testing.md) </b><br/>为确保 UWP 应用为辅助应用所要遵循的测试过程。</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[应用商店中的辅助功能](../accessibility/accessibility-in-the-store.md)</b><br/>介绍有关在 Windows 应用商店中将 UWP 应用声明为辅助应用的要求。</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[辅助功能清单](../accessibility/accessibility-checklist.md)</b><br/>提供了可帮助你确保 UWP 应用为辅助应用的清单。</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[公开基本的辅助功能信息](../accessibility/basic-accessibility-information.md)</b><br/>基本的辅助功能信息通常按照名称、角色和值进行分类。 本主题介绍可帮助应用公开辅助技术所需的基本信息的代码。</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[键盘辅助功能](../accessibility/keyboard-accessibility.md)</b><br/>如果应用未提供良好的键盘访问，则盲人用户或行动不便的用户在使用该应用时会存在困难，或者可能根本无法使用该应用。</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[高对比度主题](../accessibility/high-contrast-themes.md)</b><br/>介绍了为确保 UWP 应用在高对比度主题处于活动状态时可供使用所需的步骤。 </p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[辅助文本要求](../accessibility/accessible-text-requirements.md)</b><br/>本主题介绍了应用中的文本的最佳辅助功能做法：确保颜色和背景满足必需的对比率。 本主题还讨论了 UWP 应用中的文本元素可以具有的 Microsoft UI 自动化角色，以及图形中文本的最佳做法。</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[要避免的辅助功能做法](../accessibility/practices-to-avoid.md)</b><br/>列出创建辅助的 UWP 应用时应避免的做法。</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[自定义自动化对等](../accessibility/custom-automation-peers.md)</b><br/>介绍 UI 自动化的自动化对等概念以及如何为自己的自定义 UI 类提供自动化支持。</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[控件模式和接口](../accessibility/control-patterns-and-interfaces.md)</b><br/>列出 Microsoft UI 自动化控件模式、客户端用于访问这些模式的类以及提供程序用于实现这些模式的接口。</p>
  </div>
  <div class="side-by-side-content-right">
<p><b></b>   
</p>
  </div>
</div>
</div>



## 全球化和本地化

Windows 的使用遍及世界各地，用户的文化背景、区域及语言也各不相同。 用户可以说任意一种语言，甚至是多种语言。 用户可能位于世界的任何地方，可能在任何地方说任何语言。 通过使用全球化和本地化对应用进行设计使其更具有适应性，可以拓展其潜在市场。 

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[应做事项和禁止事项](../globalizing/guidelines-and-checklist-for-globalizing-your-app.md)</b><br/>在将你的应用全球化使其适用于更广泛的用户以及将你的应用本地化使其适用于特定市场时，请遵循这些最佳做法。</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[使用全球通用的格式](../globalizing/use-global-ready-formats.md)</b><br/>通过适当设置日期、时间、数字和货币的格式，开发全球通用的应用。</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[管理语言和区域](../globalizing/manage-language-and-region.md)</b><br/>通过使用 Windows 提供的各种语言和区域设置，控制 Windows 如何选择 UI 资源和设置应用的 UI 元素的格式。</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[使用模式设置日期和时间的格式](../globalizing/use-patterns-to-format-dates-and-times.md)</b><br/>使用 [<strong>DateTimeFormatting</strong>](https://msdn.microsoft.com/library/windows/apps/br206859) API 和自定义模式严格按照所需模式显示日期和时间。</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[调整布局和字体并支持 RTL](../globalizing/adjust-layout-and-fonts--and-support-rtl.md)</b><br/>开发你的应用来支持多种语言的布局和字体，包括 RTL（从右到左）排列方向。</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[准备你的应用以供进行本地化](../globalizing/prepare-your-app-for-localization.md)</b><br/>准备将应用本地化到其他市场、语言或地区。</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[将 UI 字符串放入资源中](../globalizing/put-ui-strings-into-resources.md)</b><br/>将你的 UI 的字符串资源放入资源文件中。 随后你可从代码或标记中引用这些字符串。</p>
  </div>
  <div class="side-by-side-content-right">
<b></b>   
<p></p>
  </div>
</div>
</div>


## 应用设置

借助应用设置，用户可以自定义你的应用，从而可以针对其个人需求和偏好来优化它。 通过提供适当的设置并妥善存储它们，可使用户体验更加出色。 

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[指南](../app-settings/guidelines-for-app-settings.md)</b><br/>有关创建和显示应用设置的最佳做法。</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[存储和检索应用数据](../app-settings/store-and-retrieve-app-data.md)</b><br/>如何存储和检索本地、漫游和临时应用数据。</p>
  </div>
</div>
</div>

## 应用内帮助
无论你设计的应用有多好，某些用户仍将需要一些额外帮助。 

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[应用帮助指南](../app-help-guidelines/guidelines-for-app-help.md)</b><br/>应用程序可能会很复杂，而为用户提供有效的帮助可大幅改善他们的体验。 
</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[说明性 UI](../app-help-guidelines/instructional-ui.md)</b><br/>有时，向用户解释应用中他们不熟悉的功能（例如特定的触摸交互）会很有用。 在这些情况下，你需要通过 UI 向用户显示说明，以便他们可以发现并使用可能错过的功能。</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[应用内帮助](../app-help-guidelines/in-app-help.md)</b><br/>在大多数情况下，最好在用户选择查看时在应用程序内显示帮助。 在创建应用内帮助时，请考虑以下指南。</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[外部帮助](../app-help-guidelines/external-help.md)</b><br/>在大多数情况下，最好在用户选择查看时在应用程序内显示帮助。 在创建应用内帮助时，请考虑以下指南。</p>
  </div>
</div>
</div>






<!--HONumber=Jun16_HO4-->


