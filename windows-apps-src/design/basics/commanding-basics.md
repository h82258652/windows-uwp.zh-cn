---
author: mijacobs
Description: In a Universal Windows Platform (UWP) app, command elements are the interactive UI elements that enable the user to perform actions, such as sending an email, deleting an item, or submitting a form.
title: 通用 Windows 平台 (UWP) 应用的命令设计基础知识
ms.assetid: 1DB48285-07B7-4952-80EF-02B57D4469F2
label: Command design basics
template: detail.hbs
op-migration-status: ready
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 07b9ce7b5a57f6dc1ba202ed57e8b2d4d93e583f
ms.sourcegitcommit: 0ab8f6fac53a6811f977ddc24de039c46c9db0ad
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2018
ms.locfileid: "1654386"
---
#  <a name="command-design-basics-for-uwp-apps"></a>UWP 应用的命令设计基础知识

在通用 Windows 平台 (UWP) 应用中，*命令元素*是使用户能够执行诸如发送电子邮件、删除项或提交表单等操作的交互式 UI 元素。 

本文介绍了常见命令元素、这些元素支持的交互以及用于托管这些元素的常见界面。

![地图应用中的命令元素](images/maps.png)

请参阅地图应用中的命令元素的示例。

## <a name="provide-the-right-type-of-interactions"></a>提供正确类型的交互

在设计命令界面时，最重要的决定是选择用户应该能够执行哪些操作。 要计划正确类型的交互，关注你的应用 - 考虑要启用的用户体验以及用户将执行的步骤。 在你决定希望用户完成的操作后，你可以向用户提供工具来帮助他们完成这些操作。

你可能需要向应用提供的一些交互包括：

- 发送或提交信息 
- 选择设置和选项
- 搜索和筛选内容
- 打开、保存和删除文件
- 编辑或创建内容

## <a name="use-the-right-command-element-for-the-interaction"></a>使用正确的命令元素进行交互

使用正确的元素启用命令交互可区分直观的易用应用和费解、令人困惑的应用。 通用 Windows 平台 (UWP) 提供了大量可在应用中使用的命令元素。 下面是一些最常见控件的列表以及它们可支持的交互的摘要。

<div class="mx-responsive-img">
<table>
<thead>
<tr class="header">
<th align="left">类别</th>
<th align="left">元素</th>
<th align="left">交互</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><b>按钮</b><br/><br/>
    <img src="../controls-and-patterns/images/controls/button.png" alt="button" /></td>
<td align="left"><a href="../controls-and-patterns/buttons.md">按钮</a></td>
<td align="left">触发即时操作。 示例包括发送电子邮件、提交表单数据或在对话框中确认某个操作。</td>
</tr>
<tr class="even">
<td align="left">列表<br/><br/>
    <img src="../controls-and-patterns/images/controls/combo-box-open.png" alt="drop down list" /></td>
<td align="left"><a href="../controls-and-patterns/lists.md">下拉列表、列表框、列表视图和网格视图</a></td>
<td align="left">显示交互式列表或网格中的项。 通常用于许多选项或显示项目。</td>
</tr>
<tr class="odd">
<td align="left">选择控件<br/><br/>
    <img src="../controls-and-patterns/images/controls/radio-button.png" alt="radio button" /></td>
<td align="left"><a href="../controls-and-patterns/checkbox.md">复选框</a>、<a href="../controls-and-patterns/radio-button.md">单选按钮</a>、<a href="../controls-and-patterns/toggles.md">切换开关</a></td>
<td align="left">使用户能够从几个选项中选择，例如，当完成一项调查或配置应用设置时。</td>
</tr>
<tr class="even">
<td align="left">日期和时间选取器<br/><br/>
    <img src="../controls-and-patterns/images/controls/calendar-date-picker-open.png" alt="date picker" /></td>
<td align="left"><a href="../controls-and-patterns/date-and-time.md">日历日期选取器, 日历视图, 日期选取器, 时间选取器</a></td>
<td align="left">使用户可以查看和修改日期和时间信息，例如当创建事件或设置闹钟时。</td>
</tr>
<tr class="odd">
<td align="left">预测文本输入<br/><br/>
    <img src="../controls-and-patterns/images/controls/auto-suggest-box.png" alt="autosuggest box" /></td>
<td align="left"><a href="../controls-and-patterns/auto-suggest-box.md">自动建议框</a></td>
<td align="left">在用户键入时提供建议，例如，在输入数据或执行查询时。</td>
</tr>
</tbody>
</table>
</div>

有关完整列表，请参阅[控件和 UI 元素](https://dev.windows.com/design/controls-patterns)

##  <a name="place-commands-on-the-right-surface"></a>将命令放置在合适的图面上
可以将命令元素放置在应用中的各种图面上，包括应用画布或特殊命令容器（例如命令栏、菜单、对话框和浮出控件）。

请注意，如果可能，您应允许用户直接操纵内容，而不是使用作用于内容的命令。 例如，允许用户通过拖放列表项而不是使用上移和下移命令按钮来重新排列列表。
  
否则，如果用户无法直接操作内容，则将命令元素放置在应用中的命令图面上：

<div class="mx-responsive-img">
<table class="uwpd-top-aligned-table">

<tr class="header">
<th align="left">图面</th>
<th align="left">说明</th>
<th align="left">示例</th>
</tr>

<tr class="odd">
<td align="left" style="vertical-align: top">应用画布（内容区域）
<p><img src="images/content-area.png" alt="The content area of an app" /></p></td>

<td align="left" style="vertical-align: top;">如果用户始终需要一个命令来完成核心方案，则可将该命令置于画布上。 由于你可以将命令放在其影响的对象附近（或放在这些对象上），将命令放置在画布上将使它们显而易见，易于使用。
<p>但是，请谨慎选择放置在画布上的命令。 应用画布上具有过多命令将占用大量屏幕空间，可能会使用户不知所措。 如果命令不经常使用，请考虑将它放在另一个命令图面中。</p> 
</td><td>
地图应用画布上的自动建议框。
<br></br>
  <img src="images/maps-canvas.png" alt="autosuggest box on Maps app canvas"/>
</td>
</tr>

<tr class="even">
<td align="left" style="vertical-align: top;"><a href="../controls-and-patterns/app-bars.md">命令栏</a>
<p><img src="../controls-and-patterns/images/controls_appbar_icons.png" alt="Example of a command bar with icons" /></p></td>
<td align="left" style="vertical-align: top;"> 命令栏可帮助整理命令，并使它们易于访问。 命令栏可以放置在屏幕顶部、屏幕底部，也可以同时放置在屏幕的顶部和底部。 
</td>
<td>
地图应用顶部的命令栏。
<br></br>
<img src="images/maps-commandbar.png" alt="command bar in Maps app"/>
</td>
</tr>

<tr class="odd">
<td align="left" style="vertical-align: top;"><a href="../controls-and-patterns/menus.md">菜单和上下文菜单</a>
<p><img src="images/controls-contextmenu-singlepane.png" alt="Example of a single-pane context menu" /></p></td>
<td align="left" style="vertical-align: top;">有时，将多个命令归组到一个命令菜单中来节省空间会更有效。 菜单和上下文菜单会在用户发出请求时显示命令或选项列表。
<p>可以使用上下文菜单快速访问常用操作并访问辅助命令（仅在某些上下文中是相关的），例如剪贴板或自定义命令。 上下文菜单通常是由用户右键单击触发的。</p>
</td><td>
当用户在地图应用中右键单击时，上下文菜单将出现。
<br></br>
  <img src="images/maps-contextmenu.png" alt="context menu in Maps app"/>
</td>
</tr>
</table>
</div>

## <a name="provide-feedback-for-interactions"></a>提供交互的反馈

反馈传达命令的结果，并允许用户了解他们做了什么，以及他们下一步可以做什么。 理想的情况下，应该在 UI 中自然地集成反馈，这样用户就不必被打断，或者采取额外行动（除非绝对有必要）。 

下面是几种在应用中提供反馈的方法。

<div class="mx-responsive-img">
<table class="uwpd-top-aligned-table">

<tr class="header">
<th align="left">图面</th>
<th align="left">描述</th>
</tr>

<tr class="odd">
<td align="left" style="vertical-align: top;"> <a href="../controls-and-patterns/app-bars.md">命令栏</a>
<p><img src="../controls-and-patterns/images/controls_appbar_icons.png" alt="Example of a command bar with icons" /></p>
</td>
<td align="left" style="vertical-align: top;"> 命令栏的内容区域是一个直观位置，可在用户希望看到反馈时将状态传达给用户。
<p>
  <img src="images/commandbar_anatomy.png" alt="Command bar content area for feedback"/>
  </p>
</td>
</tr>

<tr class="even">
<td align="left" style="vertical-align: top;"><a href="../controls-and-patterns/dialogs.md">浮出控件</a>
<p><img src="images/controls-flyout-default-200.png" alt="Image of default flyout" /></p></td>
<td align="left" style="vertical-align: top;">
可以通过点击或单击浮出控件之外的某个位置来消除轻型上下文弹出窗口。
<p>
  <img src="../controls-and-patterns/images/controls/flyout.png" alt="Flyout above button"/>
  </p>
</td>
</tr>

<tr class="odd">
<td align="left" style="vertical-align: top;"><a href="../controls-and-patterns/dialogs.md">对话框控件</a>
<p><img src="images/controls-dialog-twobutton-200.png" alt="Example of a simple two-button dialog" /></p></td>
<td align="left" style="vertical-align: top;">对话框是用于提供上下文应用信息的模式 UI 覆盖。 在大部分情况下，除非明确取消对话框，否则它会阻止与应用窗口的交互，并且通常会请求用户执行某类操作。
<p>对话框可以中断，并且只应在某些情况下使用。 有关详细信息，请参阅[何时确认或撤消操作](#when-to-confirm-or-undo-actions)部分。</p>
<p>
  <img src="../controls-and-patterns/images/dialogs/dialog_RS2_delete_file.png" alt="dialog delete file"/></p>
</td>
</tr>

</table>
</div>

> [!TIP]
> 请务必注意你的应用使用确认对话框的频率；当用户犯错时，这些对话框十分有用，但每当用户有意尝试执行某个操作时，这些对话框反而是障碍。

### <a name="when-to-confirm-or-undo-actions"></a>何时确认或撤消操作

无论用户界面设计如何精良，无论用户如何小心，在某种情况下，所有用户都会后悔他们执行了某个操作。 在这些情况下，你的应用可以通过以下方式提供帮助：要求用户确认某个操作或提供一种用于撤消最近操作的方法。

-   对于不能撤消且会产生严重后果的操作，我们建议使用确认对话框。 此类操作的示例包括：
    -   覆盖文件
    -   未在关闭文件之前保存该文件
    -   确认永久删除文件或数据
    -   购买（除非用户选择不需要确认）
    -   提交表单，如进行注册时
-   对于可以撤消的操作，通常只需提供一个简单的撤消命令。 此类操作的示例包括：
    -   删除文件
    -   删除电子邮件（非永久）
    -   修改内容或编辑文本
    -   重命名文件

##  <a name="optimize-for-specific-input-types"></a>针对特定输入类型进行优化

有关优化特定输入类型或设备的用户体验的详细信息，请参阅[交互入门](../input/index.md)。