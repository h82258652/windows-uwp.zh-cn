---
Description: 在通用 Windows 平台 (UWP) 应用中，命令元素是使用户能够执行诸如发送电子邮件、删除项或提交表单等操作的交互式 UI 元素。
title: 通用 Windows 平台 (UWP) 应用的命令设计基础知识
ms.assetid: 1DB48285-07B7-4952-80EF-02B57D4469F2
label: Command design basics
template: detail.hbs
op-migration-status: ready
ms.date: 11/01/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 6be51c274078d3b8db5ae50033bbf714ec4aa12a
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "80081395"
---
# <a name="command-design-basics-for-uwp-apps"></a>UWP 应用的命令设计基础知识

在通用 Windows 平台 (UWP) 应用中，*命令元素*是使用户能够执行诸如发送电子邮件、删除项或提交表单等操作的交互式 UI 元素。 命令界面  包含常见命令元素、托管它们的命令图面、它们支持的交互，以及它们提供的体验。

## <a name="provide-the-best-command-experience"></a>提供最佳命令体验

命令界面最重要的方面是你尝试让用户完成的东西。 在规划应用的功能时，请考虑好完成这些任务的必需步骤，以及需要启用的用户体验。 完成这些体验的初稿后，即可确定实现这些体验所需的工具和交互。

下面是一些常见的命令体验：

- 发送或提交信息
- 选择设置和选项
- 搜索和筛选内容
- 打开、保存和删除文件
- 编辑或创建内容

在设计命令体验时要有创意。 选择应用支持的输入设备，以及应用响应每个设备的方式。 通过支持最广泛范围的功能和首选项，尽可能提高应用的可用性、可移植性和可访问性（如需更多详细信息，请参阅[通用 Windows 平台 (UWP) 应用的命令控制设计](../controls-and-patterns/commanding.md)）。



<!--
When designing a command interface, the most important decision is choosing what a user can do. To plan the right type of interactions, focus on your app - consider the user experiences you want to enable, and what steps users will need to take. Once you decide what you want users to accomplish, then you can provide them the tools to do so.
-->

## <a name="choose-the-right-command-elements"></a>选择适当的命令元素

若要区分直观的易用应用与令人费解和困惑的应用，一条标准是能否在命令界面中使用适当的元素。 通用 Windows 平台 (UWP) 中提供范围广泛的命令元素。 下面列出了一些最常见的 UWP 命令元素。

:::row:::
    :::column:::
![按钮图像](images/commanding/thumbnail-button.svg)
    :::column-end:::
    :::column span="2":::
<b>按钮</b>

<a href="../controls-and-patterns/buttons.md" style="text-decoration:none">按钮</a>触发即时操作。 示例包括发送电子邮件、提交表单数据或在对话框中确认某个操作。
:::row-end:::

:::row:::
    :::column:::
![列表图像](images/commanding/thumbnail-list.svg)
    :::column-end:::
    :::column span="2":::
<b>列表</b>

<a href="../controls-and-patterns/lists.md" style="text-decoration:none">列表</a>显示交互式列表或网格中的项。 通常用于许多选项或显示项。 示例包括下拉列表、列表框、列表视图和网格视图。
:::row-end:::

:::row:::
    :::column:::
![选择控件图像](images/commanding/thumbnail-selection.svg)
    :::column-end:::
    :::column span="2":::
<b>选择控件</b>

使用户在特定情况下（例如，当完成一项调查或配置应用设置时）能够从几个选项中进行选择。 示例包括<a href="../controls-and-patterns/checkbox.md">复选框</a>、<a href="../controls-and-patterns/radio-button.md">单选按钮</a>、<a href="../controls-and-patterns/toggles.md">切换开关</a>。
:::row-end:::

:::row:::
    :::column:::
![日历图像](images/commanding/thumbnail-calendar.svg)
    :::column-end:::
    :::column span="2":::
<b>日历、日期和时间选取器</b>

<a href="../controls-and-patterns/date-and-time.md">日历、日期和时间选取器</a>使用户可以在特定情况下（例如在创建事件或设置闹钟时）查看和修改日期和时间信息。 示例包括日历日期选取器、日历视图、日期选取器、时间选取器。
:::row-end:::

:::row:::
    :::column:::
![预测文本输入图像](images/commanding/thumbnail-autosuggest.svg)
    :::column-end:::
    :::column span="2":::
<b>预测文本输入</b>

在用户键入时（例如，在输入数据或执行查询时）提供建议。 示例包括<a href="../controls-and-patterns/auto-suggest-box.md">自动建议框</a>。<br>
:::row-end:::

有关完整列表，请参阅[控件和 UI 元素](../controls-and-patterns/index.md)

## <a name="place-commands-on-the-right-surface"></a>将命令放置在合适的图面上

可以将命令元素放置在应用中的各种图面上，包括应用画布或特殊命令容器（例如命令栏、命令栏浮出控件、菜单栏或对话框）。

始终尝试让用户直接操作内容而不是通过命令来操作内容，例如，让用户通过拖拉来重新排列列表项，而不是通过向上和向下命令按钮这样做。 

不过，在某些输入设备中可能无法这样做；在需要适应特定的用户功能和首选项时，可能也无法这样做。 在这些情况下，请尽可能多地提供命令控制，并将这些命令元素置于应用的命令图面上。

下面列出了一些最常见的命令图面。

:::row:::
    :::column:::
![应用画布图像](images/commanding/thumbnail-canvas.svg)
    :::column-end:::
    :::column span="2":::
<b>应用画布（内容区域）</b>

如果用户始终需要一个命令来完成核心方案，则可将该命令置于画布上。 由于你可以将命令放在其影响的对象附近（或放在这些对象上），将命令放置在画布上将使它们显而易见，易于使用。 但是，请谨慎选择放置在画布上的命令。 应用画布上的命令过多将占用大量宝贵的屏幕空间，可能会使用户不知所措。 如果命令不经常使用，请考虑将它放在另一个命令图面中。
:::row-end:::

:::row:::
    :::column:::
![commandbar 图像](images/commanding/thumbnail-commandbar.svg)
    :::column-end:::
    :::column span="2":::
<b>命令栏和菜单栏</b>

<a href="../controls-and-patterns/app-bars.md">命令栏</a>可用于组织命令，并使它们易于访问。 命令栏可以放置在屏幕顶部和/或屏幕底部（当应用中的功能对于命令栏来说太复杂时，也可使用 <a href="../controls-and-patterns/menus.md#create-a-menu-bar">MenuBar</a>）。
:::row-end:::

:::row:::
    :::column:::
![上下文菜单图像](images/commanding/thumbnail-contextmenu.svg)
    :::column-end:::
    :::column span="2":::
<b>菜单和上下文菜单</b>

<p>菜单和上下文菜单通过在用户不需要使用时对命令进行组织和隐藏，从而节省空间。 用户通常通过单击按钮或右键单击控件的方式来访问菜单或上下文菜单。</p> 

<p><a href="../controls-and-patterns/command-bar-flyout.md">命令栏浮出控件</a>是一种上下文菜单，它将命令栏和上下文菜单的优点结合到了单个控件之中。 可以使用它快速访问常用操作并访问辅助命令（仅在某些上下文中是相关的），例如剪贴板或自定义命令。</p>

<p>UWP 还提供一组传统菜单和上下文菜单；有关详细信息，请参阅<a href="../controls-and-patterns/menus.md">菜单和上下文菜单概述</a>。</p>
:::row-end:::

## <a name="provide-command-feedback"></a>提供命令反馈 

命令反馈告知用户：检测到了交互或命令、命令是如何解释和处理的，以及命令是否成功。 这可以让用户了解他们做了什么，以及他们下一步可以做什么。 理想的情况下，应该在 UI 中自然地集成反馈，这样用户就不必被打断或采取额外行动（除非绝对有必要）。

> [!NOTE]
> 只有在必要且其他地方不提供反馈的情况下才提供反馈。 使应用程序 UI 保持整洁，除非要添加值。

下面是几种在应用中提供反馈的方法。

:::row:::
    :::column:::
![命令栏内容区域图像](images/commanding/thumbnail-commandbar2.svg)
    :::column-end:::
    :::column span="2":::
<b>命令栏</b>

<a href="../controls-and-patterns/app-bars.md">命令栏</a>的内容区域是一个直观位置，可在用户希望看到反馈时将状态传达给用户。
:::row-end:::

:::row:::
    :::column:::
![浮出控件图像](images/commanding/thumbnail-flyout.svg)
    :::column-end:::
    :::column span="2":::
<b>浮出控件</b>

       <a href="../controls-and-patterns/dialogs-and-flyouts/index.md">浮出控件</a>是轻型上下文弹出窗口，可以通过点击或单击浮出控件之外的某个位置来消除。
:::row-end:::

:::row:::
    :::column:::
![对话框图像](images/commanding/thumbnail-dialog.svg)
    :::column-end:::
    :::column span="2":::
<b>对话框控件</b>

<a href="../controls-and-patterns/dialogs-and-flyouts/index.md">对话框控件</a>是用于提供上下文应用信息的模式 UI 覆盖。 在大部分情况下，除非明确取消对话框，否则它会阻止与应用窗口的交互，并且通常会请求用户执行某类操作。 对话框可以中断，并且只应在某些情况下使用。 有关详细信息，请参阅[何时确认或撤消操作](#when-to-confirm-or-undo-actions)部分。
    :::column-end:::
:::row-end:::

> [!TIP]
> 请务必注意你的应用使用确认对话框的频率；当用户犯错时，这些对话框十分有用，但每当用户有意尝试执行某个操作时，这些对话框反而是障碍。

### <a name="when-to-confirm-or-undo-actions"></a>何时确认或撤消操作

无论应用程序 UI 的设计如何精良，所有用户都会在执行操作时犯错。 在这些情况下，应用可以通过以下方式提供帮助：要求确认某个操作，或提供一种用于撤消最近操作的方法。

:::row:::
    :::column:::
![应做事项图像](images/do.svg)

对于不能撤消且会产生严重后果的操作，我们建议使用确认对话框。 此类操作的示例包括：
-   覆盖文件
-   未在关闭文件之前保存该文件
-   确认永久删除文件或数据
-   购买（除非用户选择不需要确认）
-   提交表单，如进行注册时
    :::column-end:::
    :::column:::
![应做事项图像](images/do.svg)

对于可以撤消的操作，通常只需提供一个简单的撤消命令。 此类操作的示例包括：
-   删除文件
-   删除电子邮件（非永久）
-   修改内容或编辑文本
-   重命名文件
:::row-end:::

##  <a name="optimize-for-specific-input-types"></a>针对特定输入类型进行优化

有关优化特定输入类型或设备的用户体验的详细信息，请参阅[交互入门](../input/index.md)。

