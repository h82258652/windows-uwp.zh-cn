---
author: mijacobs
Description: In a Universal Windows Platform (UWP) app, command elements are the interactive UI elements that enable the user to perform actions, such as sending an email, deleting an item, or submitting a form.
title: 通用 Windows 平台 (UWP) 应用的命令设计基础知识
ms.assetid: 1DB48285-07B7-4952-80EF-02B57D4469F2
label: Command design basics
template: detail.hbs
op-migration-status: ready
ms.author: mijacobs
ms.date: 10/01/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 104788b98377b55564fcc204ecc161521d071c6b
ms.sourcegitcommit: 933e71a31989f8063b020746fdd16e9da94a44c4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/11/2018
ms.locfileid: "4536902"
---
#  <a name="command-design-basics-for-uwp-apps"></a>UWP 应用的命令设计基础知识

在通用 Windows 平台 (UWP) 应用中，*命令元素*是使用户能够执行诸如发送电子邮件、删除项或提交表单等操作的交互式 UI 元素。 本文介绍常见命令元素、它们支持的交互以及用于承载它们的命令图面。

## <a name="provide-the-right-type-of-interactions"></a>提供正确类型的交互

在设计命令界面时，最重要的决定是选择用户应该能够执行哪些操作。 要计划正确类型的交互，关注你的应用 - 考虑要启用的用户体验以及用户将执行的步骤。 在你决定希望用户完成的操作后，你可以向用户提供工具来帮助他们完成这些操作。

你可能需要向应用提供的一些交互包括：

- 发送或提交信息 
- 选择设置和选项
- 搜索和筛选内容
- 打开、保存和删除文件
- 编辑或创建内容

## <a name="use-the-right-command-element-for-the-interaction"></a>使用正确的命令元素进行交互

使用正确的元素启用命令交互可区分直观的易用应用和费解、令人困惑的应用。 通用 Windows 平台 (UWP) 提供了大量可在应用中使用的命令元素。 下面列出了一些最常见的控件并总结了它们可支持的交互。

:::row:::
    :::column:::
        ![button image](images/commanding/thumbnail-button.svg)
    :::column-end:::
    :::column span="2":::
        <b>Buttons</b>

        <a href="../controls-and-patterns/buttons.md" style="text-decoration:none">Buttons</a> trigger an immediate action. Examples include sending an email, submitting form data, or confirming an action in a dialog.
:::row-end:::

:::row:::
    :::column:::
        ![list image](images/commanding/thumbnail-list.svg)
    :::column-end:::
    :::column span="2":::
        <b>Lists</b>

        <a href="../controls-and-patterns/lists.md" style="text-decoration:none">Lists</a> present items in a interactive list or a grid. Usually used for many options or display items. Examples include drop-down list, list box, list view and grid view.
:::row-end:::

:::row:::
    :::column:::
        ![selection control image](images/commanding/thumbnail-selection.svg)
    :::column-end:::
    :::column span="2":::
        <b>Selection controls</b>

        Lets users choose from a few options, such as when completing a survey or configuring app settings. Examples include <a href="../controls-and-patterns/checkbox.md">check box</a>, <a href="../controls-and-patterns/radio-button.md">radio button</a>, and <a href="../controls-and-patterns/toggles.md">toggle switch</a>.
:::row-end:::

:::row:::
    :::column:::
        ![Calendar  image](images/commanding/thumbnail-calendar.svg)
    :::column-end:::
    :::column span="2":::
        <b>Calendar, date and time pickers</b>

        <a href="../controls-and-patterns/date-and-time.md">Calendar, date and time pickers</a> enable users to view and modify date and time info, such as when creating an event or setting an alarm. Examples include calendar date picker, calendar view, date picker, time picker.
:::row-end:::

:::row:::
    :::column:::
        ![Predictive text entry image](images/commanding/thumbnail-autosuggest.svg)
    :::column-end:::
    :::column span="2":::
        <b>Predictive text entry</b>

        Provides suggestions as users type, such as when entering data or performing queries. Examples include <a href="../controls-and-patterns/auto-suggest-box.md">auto-suggest box</a>.<br>
:::row-end:::

有关完整列表，请参阅[控件和 UI 元素](../controls-and-patterns/index.md)

## <a name="place-commands-on-the-right-surface"></a>将命令放置在合适的图面上

在你的应用，包括应用画布或特殊命令容器，例如命令栏、 命令栏浮出控件、 菜单栏和对话框中，可以将命令元素放置在多个图面。

请注意，如果可能，你应让用户操作直接内容而不是使用作用于内容的命令。 例如，允许用户通过拖放列表项而不是使用上移和下移命令按钮来重新排列列表。

如果用户无法直接操作内容，则将命令元素放置在应用中的命令图面上。 下面列出了一些最常见的命令图面。

:::row:::
    :::column:::
        ![app canvas image](images/commanding/thumbnail-canvas.svg)
    :::column-end:::
    :::column span="2":::
        <b>App canvas (content area)</b>

        If a command is constantly needed for users to complete core scenarios, put it on the canvas. Because you can put commands near (or on) the objects they affect, putting commands on the canvas makes them easy and obvious to use. However, choose the commands you put on the canvas carefully. Too many commands on the app canvas take up valuable screen space and can overwhelm the user. If the command won't be frequently used, consider putting it in another command surface.
:::row-end:::

:::row:::
    :::column:::
        ![commandbar image](images/commanding/thumbnail-commandbar.svg)
    :::column-end:::
    :::column span="2":::
        <b>Command bars and menu bars</b>

        <a href="../controls-and-patterns/app-bars.md">Command bars</a> help organize commands and make them easy to access. Command bars can be placed at the top of the screen, at the bottom of the screen, or at both the top and bottom of the screen (a <a href="../controls-and-patterns/menus.md#create-a-menu-bar">MenuBar</a> can also be used when the functionality in your app is too complex for a command bar).
:::row-end:::

:::row:::
    :::column:::
        ![context menu image](images/commanding/thumbnail-contextmenu.svg)
    :::column-end:::
    :::column span="2":::
        <b>Menus and context menus</b>

        <p>Menus and context menus save space by organizing commands and hiding them until the user needs them. Users typically access a menu or context menu by clicking a button or right-clicking a control.</p> 

        <p>The <a href="../controls-and-patterns/command-bar-flyout.md">command bar flyout </a> is a type of contextual menu that combines the benefits of a command bar and a context menu into a single control. It can provide shortcuts to commonly-used actions and provide access to secondary commands that are only relevant in certain contexts, such as clipboard or custom commands.</p>

        <p>UWP also provides a set of traditional menus and context menus; for more info, see the <a href="../controls-and-patterns/menus.md">menus and context menus overview</a>.</p>
:::row-end:::

## <a name="provide-feedback-for-interactions"></a>提供交互的反馈

反馈传达命令的结果，并允许用户了解他们做了什么，以及他们下一步可以做什么。 理想的情况下，应该在 UI 中自然地集成反馈，这样用户就不必被打断或采取额外行动（除非绝对有必要）。 

下面是几种在应用中提供反馈的方法。

:::row:::
    :::column:::
        ![commandbar content area image](images/commanding/thumbnail-commandbar2.svg)
    :::column-end:::
    :::column span="2":::
        <b>Command bar</b>

        The content area of the <a href="../controls-and-patterns/app-bars.md">command bar</a> is an intuitive place to communicate status to users if they'd like to see feedback.
:::row-end:::

:::row:::
    :::column:::
        ![Flyout image](images/commanding/thumbnail-flyout.svg)
    :::column-end:::
    :::column span="2":::
        <b>Flyouts</b>

       <a href="../controls-and-patterns/dialogs-and-flyouts/index.md">浮出控件</a>是可通过点击或单击浮出控件之外的某个位置来消除轻型上下文弹出窗口。
:::row-end:::

:::row:::
    :::column:::
        ![Dialog image](images/commanding/thumbnail-dialog.svg)
    :::column-end:::
    :::column span="2":::
        <b>Dialog controls</b>

        <a href="../controls-and-patterns/dialogs-and-flyouts/index.md">Dialog controls</a> are modal UI overlays that provide contextual app information. In most cases, dialogs block interactions with the app window until being explicitly dismissed, and often request some kind of action from the user. Dialogs can be disruptive and should only be used in certain situations. For more info, see the [When to confirm or undo actions](#when-to-confirm-or-undo-actions) section.
    :::column-end:::
:::row-end:::

> [!TIP]
> 请务必注意你的应用使用确认对话框的频率；当用户犯错时，这些对话框十分有用，但每当用户有意尝试执行某个操作时，这些对话框反而是障碍。

### <a name="when-to-confirm-or-undo-actions"></a>何时确认或撤消操作

无论用户界面设计如何精良，无论用户如何小心，在某种情况下，所有用户都会后悔他们执行了某个操作。 在这些情况下，你的应用可以通过以下方式提供帮助：要求用户确认某个操作或提供一种用于撤消最近操作的方法。

:::row:::
    :::column:::
        ![do image](images/do.svg)

        For actions that can't be undone and have major consequences, we recommend using a confirmation dialog. Examples of such actions include:
        -   Overwriting a file
        -   Not saving a file before closing
        -   Confirming permanent deletion of a file or data
        -   Making a purchase (unless the user opts out of requiring a confirmation)
        -   Submitting a form, such as signing up for something
    :::column-end:::
    :::column:::
        ![do image](images/do.svg)

        For actions that can be undone, offering a simple undo command is usually enough. Examples of such actions include:
        -   Deleting a file
        -   Deleting an email (not permanently)
        -   Modifying content or editing text
        -   Renaming a file
:::row-end:::

##  <a name="optimize-for-specific-input-types"></a>针对特定输入类型进行优化

有关优化特定输入类型或设备的用户体验的详细信息，请参阅[交互入门](../input/index.md)。

