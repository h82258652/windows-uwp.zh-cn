---
Description: 了解如何设计和优化你的 UWP 应用，使其为键盘高级用户以及残障人士和具有其他辅助功能需求的用户提供可能的最佳体验。
title: 键盘交互
ms.assetid: FF819BAC-67C0-4EC9-8921-F087BE188138
label: Keyboard interactions
template: detail.hbs
keywords: 键盘, 辅助功能, 导航, 焦点, 文本, 输入, 用户交互, 游戏板, 远程
ms.date: 03/29/2017
ms.topic: article
pm-contact: chigy
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.openlocfilehash: 449f0c81bdd54d99ef0977ca1c9b6ba10ba5eae7
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258358"
---
# <a name="keyboard-interactions"></a>键盘交互

![键盘 Hero 图像](images/keyboard/keyboard-hero.jpg)

了解如何设计和优化你的 UWP 应用，使其为键盘高级用户以及残障人士和具有其他辅助功能需求的用户提供可能的最佳体验。

跨设备、键盘输入是整个通用 Windows 平台 (UWP) 交互体验的重要部分。 设计出色的键盘体验可以让用户高效地导航应用的 UI 并访问其所有功能，而无需将其双手抬离键盘。

![键盘和游戏板图像](images/keyboard/keyboard-gamepad.jpg)

***Common interaction patterns are shared between keyboard and gamepad***

在本主题中，我们将重点介绍适用于电脑上的键盘输入的 UWP 应用设计。 However, a well-designed keyboard experience is important for supporting accessibility tools such as Windows Narrator, using [software keyboards](#software-keyboard) such as the touch keyboard and the On-Screen Keyboard (OSK), and for handling other input device types, such as the Xbox gamepad and remote control.

此处讨论的许多指南和建议（包括[焦点视觉对象](#focus-visuals)、[访问键](#access-keys)和 [UI 导航](#navigation)）也适用于其他情况。

**注意**  硬件和软件键盘均可用于文本输入，但是本主题的重点在于导航和交互。

## <a name="built-in-support"></a>内置支持

键盘与鼠标是电脑上广泛使用的外设，它们是电脑体验的基础部分。 电脑用户希望在应用和单个应用响应键盘输入时获得全面、一致的体验。

虽然平台本身可为创建最适合于你的自定义控件和应用的键盘体验提供广泛基础，但是，所有 UWP 控件均包括内置支持，以实现丰富的键盘体验和用户交互。

![具有手机图像的键盘](images/keyboard/keyboard-phone.jpg)

***UWP supports keyboard with any device***

## <a name="basic-experiences"></a>基本体验
![基于焦点的设备](images/keyboard/focus-based-devices.jpg)

如前面所述，输入设备（如 Xbox 游戏板和遥控器）和辅助功能工具（如讲述人）具有许多相同的导航和命令键盘输入体验。 输入设备和工具中的共同体验可以减少你的额外工作，有助于实现通用 Windows 平台“构建一次即可在任何地方运行”的目标。

Where necessary, we'll identify key differences you should be aware of and describe any mitigations you should consider.

以下是本主题中将会讨论的设备和工具：

| 设备/工具                       | 描述     |
|-----------------------------------|-----------------|
|键盘（硬件和软件）   |In addition to the standard hardware keyboard, UWP applications support two software keyboards: the [touch (or software) keyboard](#software-keyboard) and the [On-Screen Keyboard](#on-screen-keyboard).|
|手柄和遥控器         |Xbox 游戏板和遥控器是 [10 英尺体验](../devices/designing-for-tv.md)中的基础输入设备。 有关游戏板和遥控器的 UWP 支持的特定详细信息，请参阅[游戏板和遥控器交互](gamepad-and-remote-interactions.md)。|
|屏幕读取器（讲述人）          |讲述人是一款面向 Windows 的内置屏幕读取器，可提供独特的交互体验和功能，但仍依赖于基本的键盘导航和输入。 有关讲述人的详细信息，请参阅[讲述人入门](https://support.microsoft.com/help/22798/windows-10-complete-guide-to-narrator)。|

## <a name="custom-experiences-and-efficient-keyboarding"></a>自定义体验和高效键盘操作
如前所述，为了确保应用程序非常适合具有不同技能、能力和期望的用户，键盘支持是不可或缺的。 建议优先考虑以下各项。
- 支持键盘导航和交互
    - 确保将可操作项识别为制表位（非可操作项不是制表位），并且导航顺序符合逻辑且可预测（请参阅[制表位](#tab-stops)）
    - 将初始焦点设置在最符合逻辑的元素上（请参阅[初始焦点](#initial-focus)）
    - 为“内部导航”提供箭头键导航（请参阅[导航](#navigation)）
- 支持键盘快捷方式
    - 为快速操作提供加速键（请参阅[加速器](#accelerators)）
    - Provide access keys to navigate your application's UI (see [Access keys](access-keys.md))

### <a name="focus-visuals"></a>Focus visuals

UWP 支持适合于所有输入类型和体验的单个焦点视觉对象设计。
![Focus visual](images/keyboard/focus-visual.png)

焦点视觉对象：

- 在 UI 元素接收到来自键盘和/或游戏板/遥控器的焦点时显示
- 呈现为 UI 元素周围的突出显示边框，以指示可以采取操作
- 帮助用户在不迷失的情况下导航应用 UI
- 可以为你的应用自定义（请参阅[高可见性焦点视觉对象](guidelines-for-visualfeedback.md#high-visibility-focus-visuals)）

**注意**UWP 焦点视觉对象与讲述人焦点矩形不相同。

### <a name="tab-stops"></a>制表位

若要将控件（包括导航元素）与键盘结合使用，该控件必须具有焦点。 One way for a control to receive keyboard focus is to make it accessible through tab navigation by identifying it as a tab stop in your application's tab order.

For a control to be included in the tab order, the [IsEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_IsEnabled) property must be set to **true** and the [IsTabStop](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_IsTabStop) property must be set to **true**.

To specifically exclude a control from the tab order, set the [IsTabStop](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_IsTabStop) property to **false**.

默认情况下，Tab 键顺序反映了 UI 元素的创建顺序。 例如，如果 `StackPanel` 包含 `Button`、`Checkbox` 和 `TextBox`，则 Tab 键顺序为 `Button`、`Checkbox` 和 `TextBox`。

You can override the default tab order by setting the [TabIndex](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_TabIndex) property.

#### <a name="tab-order-should-be-logical-and-predictable"></a>Tab 键顺序应符合逻辑且可预测

使用符合逻辑且可预测的 Tab 键顺序的出色键盘导航模型可以让你的应用更加直观，并且有助于用户更高效且有效地浏览、发现和访问功能。

All interactive controls should have tab stops (unless they are in a [group](#control-group)), while non-interactive controls, such as labels, should not.

避免使用会让焦点在应用程序中跳来跳去的自定义 Tab 键顺序。 例如，表单中的控件列表的 Tab 键顺序应当是从顶部到底部，从左边到右边（取决于区域设置）。

See [Keyboard accessibility](../accessibility/keyboard-accessibility.md) for more details about customizing tab stops.

#### <a name="try-to-coordinate-tab-order-and-visual-order"></a>尝试协调 Tab 键顺序和视觉对象顺序

Coordinating tab order and visual order (also referred to as reading order or display order) helps reduce confusion for users as they navigate through your application's UI.

先尝试以 Tab 键顺序和视觉对象顺序分级和显示最重要的命令、控件和内容。 然而，实际的显示位置可能取决于父布局容器，以及会影响该布局的子元素的某些属性。 特别地是，使用网格标记或表格标记的布局可具有一个与 Tab 键顺序完全不同的视觉对象顺序。

**注意**视觉对象顺序也依赖于区域和语言。

### <a name="initial-focus"></a>初始焦点

初始焦点用于指定在首次启动或激活应用程序或页面时接收焦点的 UI 元素。 When using a keyboard, it is from this element that a user starts interacting with your application's UI.

For UWP apps, initial focus is set to the element with the highest [TabIndex](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_TabIndex) that can receive focus. 容器控件的子元素将被忽略。 因此，可视化树中的第一个元素接收焦点。

#### <a name="set-initial-focus-on-the-most-logical-element"></a>将初始焦点设置为最符合逻辑的元素

将初始焦点设置为用户在启动应用或导航页面时最有可能采取的第一个（或主要）操作的 UI 元素上。 一些示例如下：
-   在照片应用中，焦点被设为库中的第一个项目
-   在音乐应用中，焦点被设为播放按钮

#### <a name="dont-set-initial-focus-on-an-element-that-exposes-a-potentially-negative-or-even-disastrous-outcome"></a>Don't set initial focus on an element that exposes a potentially negative, or even disastrous, outcome

This level of functionality should be a user's choice. 将初始焦点设为具有重要结果的元素可能会导致出现意外的数据丢失或系统访问。 For example, don't set focus to the delete button when navigating to an e-mail.

See [Focus navigation](focus-navigation.md) for more details about overriding tab order.

### <a name="navigation"></a>导航

通常情况下是通过 Tab 键和箭头键支持键盘导航。

![Tab 和箭头键](images/keyboard/tab-and-arrow.png)

默认情况下，UWP 控件遵循以下基本的键盘行为：
-   **Tab 键**用于以 Tab 键顺序在可操作/活动控件之间导航。
-   **Shift + Tab** 用于以与 Tab 键顺序相反的顺序导航控件。 如果用户已使用箭头键在控件内导航，则焦点将设为控件内的最后一个已知值。
-   **箭头键**显示控件特定的“内部导航”。当用户进入“内部导航”时，箭头键不会在控件外导航。 一些示例如下：
    -   Up/Down arrow key moves focus inside `ListView` and `MenuFlyout`
    -   Modify currently selected values for `Slider` and `RatingsControl`
    -   Move caret inside `TextBox`
    -   Expand/collapse items inside `TreeView`

Use these default behaviors to optimize your application's keyboard navigation.

#### <a name="use-inner-navigation-with-sets-of-related-controls"></a>通过多组相关控件使用“内部导航”

Providing arrow key navigation into a set of related controls reinforces their relationship within the overall organization of your application's UI.

例如，此处所示的 `ContentDialog` 控件在默认情况下将会为水平行的按钮提供内部导航（有关自定义控件，请参阅[控件组](#control-group)部分）。

![对话示例](images/keyboard/dialog.png)

***Interaction with a collection of related buttons is made easier with arrow key navigation***

如果显示的是单列中的项目，向上/向下箭头键将会导航项目。 如果显示的是单行中的项目，则向右/向左箭头键将会导航项目。 如果项目位于多个列中，则所有 4 个箭头键均将用于导航。

#### <a name="define-a-single-tab-stop-for-a-collection-of-related-controls"></a>Define a single tab stop for a collection of related controls

By defining a single tab stop for a collection of related, or complementary, controls, you can minimize the number of overall tab stops in your app.

例如，下图所示为两个堆叠的 `ListView` 控件。 左图所示为使用制表位在 `ListView` 控件之间进行导航的箭头键导航，右图所示为如何更轻松、高效地在子元素之间导航而无需通过 Tab 键遍历父控件。


<table>
  <td><img src="images/keyboard/arrow-and-tab.png" alt="arrow and tab" /></td>
  <td><img src="images/keyboard/arrow-only.png" alt="arrow only" /></td>
</table>

***Interaction with two stacked ListView controls can be made easier and more efficient by eliminating the tab stop and navigating with just arrow keys.***

请访问[控件组](#control-group)部分了解如何为你的应用程序 UI 应用优化示例。

### <a name="interaction-and-commanding"></a>交互和命令

控件具有焦点之后，用户可以使用键盘输入与其进行交互并调用任何相关功能。

#### <a name="text-entry"></a>文本输入

对于专为 `TextBox` 和 `RichEditBox` 之类的文本输入设计的控件，所有键盘输入均用于输入或导航文本，其优先级高于其他键盘命令。 例如，`AutoSuggestBox` 控件的下拉菜单无法将**空格**键识别为选择命令。

![文本输入](images/keyboard/text-entry.png)

#### <a name="space-key"></a>空格键

如果不是处于文本输入模式，则**空格**键将调用与聚焦的控件关联的操作或命令（类似于点按触摸屏或用鼠标单击）。

![空格键](images/keyboard/space-key.png)

#### <a name="enter-key"></a>Enter 键

**Enter** 键可执行多种常见的用户交互，具体取决于具有焦点的控件：
-   激活命令控件，如 `Button` 或 `Hyperlink`。 为了避免最终用户困惑，**Enter** 键还可以激活类似于 `ToggleButton` 或 `AppBarToggleButton` 之类的命令控件的控件。
-   显示控件（如 `ComboBox` 和 `DatePicker`）的选取器 UI。 **Enter** 键还可用于提交和关闭选取器 UI。
-   激活列表控件，如 `ListView`、`GridView` 和 `ComboBox`。
    -   **Enter** 键将像**空格**键一样对列表和网格项执行选择操作，除非这些项还关联了其他操作（打开新的窗口）。
    -   如果此控件还关联了其他操作，则 **Enter** 键将执行其他操作，**Space** 键执行选择操作。

**注意**虽然 **Enter** 键和 **Space** 键并不总是执行相同的操作，但经常是执行相同操作。

![Enter 键](images/keyboard/enter-key.png)

Esc 键让用户可以取消瞬态 UI（以及该 UI 中的任何正在进行的操作）。

此体验的示例包括：
-   用户打开一个具有选定值的 `ComboBox` 并使用箭头键将焦点选择移至新值。 按下 Esc 键将关闭 `ComboBox` 并将选定值重置为原始值。
-   用户为电子邮件调用永久删除操作并且系统弹出一个用户确认操作的 `ContentDialog`。 用户确定这不是目标操作并按 **Esc** 键关闭对话框。 当 **Esc** 键与**取消**按钮关联时，对话将关闭且操作将被取消。 **Esc** 键只会影响瞬态 UI，它不会关闭或导航回应用 UI。

![Esc 键](images/keyboard/esc-key.png)

#### <a name="home-and-end-keys"></a>Home 和 End 键

**Home** 和 **End** 键让用户可以滚动至 UI 区域的开头或结尾。

此体验的示例包括：
-   对于 `ListView` 和 `GridView` 控件，**Home** 键用于将焦点移至第一个元素并将其滚动至视图，而 **End** 键用于将焦点移至最后一个元素并将其滚动至视图。
-   对于 `ScrollView` 控件，**Home** 键用于滚动至区域的顶部，而 **End** 键用于滚动至区域的底部（焦点未改变）。

![home 和 end 键](images/keyboard/home-and-end.png)

#### <a name="page-up-and-page-down-keys"></a>向上和向下翻页键

**翻页**键让用户可以离散增量滚动 UI 区域。

例如，对于 `ListView` 和 `GridView` 控件，**向上翻页**键用于按“页”（通常为视区高度）向上滚动区域并将焦点移至区域顶部。 或者，**向下翻页**键用于按页向下滚动区域并将焦点移至区域底部。

![向上和向下翻页键](images/keyboard/page-up-and-down.png)

### <a name="keyboard-shortcuts"></a>键盘快捷方式

Keyboard shortcuts can make your app easier to use by providing both enhanced support for accessibility and improved efficiency for keyboard users.

In addition to supporting keyboard navigation and activation in your app, it is also good practice to provide shortcuts for your application's functionality. Tab navigation provides a good, basic level of keyboard support, but with more complex UI you might want to add support for shortcut keys as well. 

快捷方式是一套键盘组合，通过为用户提供一种可访问应用程序功能的高效方法来提高效率。 有两种类型的快捷方式：
-   [Accelerators](#accelerators) are shortcuts that invoke an app command. Your app may or may not provide specific UI that corresponds to the command. Accelerators typically consist of the Ctrl key plus a letter key.
-   [Access keys](#access-keys) are shortcuts that set focus to specific UI in your application. Access keys typicaly consist of the Alt key plus a letter key.

Providing consistent keyboard shortcuts that support similar tasks across applications makes them much more useful and powerful and helps users remember them.

#### <a name="accelerators"></a>加速器

Accelerators help users perform common actions in an application much more quickly and efficiently. 

加速器示例：
-   Pressing Ctrl + N key anywhere in the **Mail** app launches a new mail item.
-   Pressing Ctrl + E key anywhere in Microsoft Edge (and many Microsoft Store applications) launches search.

加速器具有以下特征：
-   They primarily use Ctrl and Function key sequences (Windows system shortcut keys also use Alt + non-alphanumeric keys and the Windows logo key).
-   它们仅分配给最常用的命令。
-   它们的设计目的在于被用户记住，仅在菜单、工具提示和帮助中提供相关说明。
-   They have effect throughout the entire application, when supported.
-   They should be assigned consistently as they are memorized and not directly documented.

#### <a name="access-keys"></a>访问键

有关 UWP 中的支持访问键的更多深入信息，请参阅[访问键](access-keys.md)页面。

访问键可以帮助一次只能按一个键的残障人士用户操作 UI 中的特定项目。 此外，访问键可用于传播其他快捷方式，以帮助高级用户更快地执行操作。

访问键具有以下特征：
-   它们使用 Alt 键 + 字母数字键。
-   它们主要用于辅助功能。
-   They are documented directly in the UI, adjacent to the control, through [Key Tips](access-keys.md).
-   它们仅在当前窗口中有效，用于导航到对应的菜单项或控件。
-   Access keys should be assigned consistently to commonly used commands (especially commit buttons), whenever possible.
-   应对其执行本地化。

#### <a name="common-keyboard-shortcuts"></a>常见的键盘快捷方式

The following table is a small sample of frequently used keyboard shortcuts. 

| “操作”                               | 键命令                                      |
|--------------------------------------|--------------------------------------------------|
| 全选                           | Ctrl+A                                           |
| 连续选择                  | Shift+箭头键                                  |
| “保存”                                 | Ctrl+S                                           |
| 找到                                 | Ctrl+F                                           |
| 打印                                | Ctrl+P                                           |
| “复制”                                 | Ctrl+C                                           |
| Cut                                  | Ctrl+X                                           |
| 粘帖                                | Ctrl+V                                           |
| 撤销                                 | Ctrl+Z                                           |
| 下一个选项卡                             | Ctrl+Tab                                         |
| 关闭选项卡                            | Ctrl+F4 或 Ctrl+W                                |
| 语义式缩放                        | Ctrl++ 或 Ctrl+-                                 |

For a comprehensive list of Windows system shortcuts, see [keyboard shortcuts for Windows](https://support.microsoft.com/help/12445/windows-keyboard-shortcuts). For common application shortcuts, see [keyboard shortcuts for Microsoft applications](https://support.microsoft.com/help/13805/windows-keyboard-shortcuts-in-apps).

## <a name="advanced-experiences"></a>高级体验

在本部分，我们将讨论 UWP 应用支持的一些更为复杂的键盘交互体验，以及在不同的设备上通过不同的工具使用应用时需要注意的一些行为。

### <a name="control-group"></a>Control group

你可以在已使用箭头键启用“内部导航”的“控件组”（或方向区域）中对一组相关或补充性的控件进行分组。 控件组可以是单个制表位，或者你可以在控制组中指定多个制表位。

#### <a name="arrow-key-navigation"></a>箭头键导航

用户期望当以下位置的 UI 区域中存在一组类似的相关控件时也支持箭头键导航：
-   `AppBarButtons` in a `CommandBar`
-   `ListItems` or `GridItems` inside `ListView` or `GridView`
-   `Buttons` inside `ContentDialog`

UWP 控件默认情况下之后箭头键导航。 有关自定义布局和控制组，请使用 `XYFocusKeyboardNavigation="Enabled"` 以提供类似的行为。

Consider adding support for arrow key navigation when using the following controls:

<table>
  <tr>
    <td>
      <p><img src="images/keyboard/dialog.png" alt="Dialog buttons"/></p>
      <p><sup>Dialog buttons</sup></p>
      <p><img src="images/keyboard/radiobutton.png" alt="Radio buttons"/></p>
      <p><sup>RadioButtons</sup></p>     
    </td>
    <td>
      <p><img src="images/keyboard/appbar.png" alt="AppBar buttons"/></p>
      <p><sup>AppBarButtons</sup></p>
      <p><img src="images/keyboard/list-and-grid-items.png" alt="List and Grid items"/></p>
      <p><sup>ListItems and GridItems</sup></p>
    </td>    
  </tr>
</table>

#### <a name="tab-stops"></a>制表位

Depending on your application's functionality and layout, the best navigation option for a control group might be a single tab stop with arrow navigation to child elements, multiple tab stops, or some combination.

##### <a name="use-multiple-tab-stops-and-arrow-keys-for-buttons"></a>为按钮使用多个制表位和箭头键

辅助功能用户依赖于完善的键盘导航规则，它们通常不使用箭头键导航按钮集合。 但是，没有视觉障碍的用户可能会觉得行为非常自然。

本案例中默认的 UWP 行为示例为 `ContentDialog`。 箭头键可以在按钮之间导航，而且每个按钮也是一个制表位。

##### <a name="assign-single-tab-stop-to-familiar-ui-patterns"></a>为熟悉的 UI 模式分配单个制表位

如果你的布局遵循的是著名的控件组 UI 模式，则为组分配单个制表位可以提高用户的导航效率。

示例包括：
-   `RadioButtons`
-   Multiple `ListViews` that look like and behave like a single `ListView`
-   外观和行为类似于磁贴（例如“开始”菜单磁贴）网格的任何 UI

#### <a name="specifying-control-group-behavior"></a>指定控件组行为

使用以下 API 支持自定义控件组行为（所有这些均会在本主题的后面详细讨论）：

-   [XYFocusKeyboardNavigation](focus-navigation.md#2d-directional-navigation-for-keyboard) 支持空间之间的箭头键导航
-   [TabFocusNavigation](focus-navigation.md#tab-navigation) 指示是存在多个制表位还是单个制表位
-   [FindFirstFocusableElement and FindLastFocusableElement](focus-navigation-programmatic.md#find-the-first-and-last-focusable-element) 将通过 **Home** 键将焦点设置在第一个项目上并通过 **End** 键将焦点设置在最后一个项目上

下图所示为关联单选按钮控件组的直观键盘导航行为。 在本案例中，我们建议为控件组设置单个制表位、使用箭头键在单选按钮之间进行内部导航、将 **Home** 键绑定至第一个单选按钮并将 **End** 键绑定至最后一个单选按钮。

![组织到一起](images/keyboard/putting-it-all-together.png)

### <a name="keyboard-and-narrator"></a>键盘和讲述人

讲述人是一款面向键盘用户的 UI 辅助功能工具（也支持其他输入类型）。 但是，讲述人功能超出了 UWP 应用支持的键盘交互，因此，为讲述人设计 UWP 应用时需要注意。 （[讲述人基础知识页面](https://support.microsoft.com/help/22808/windows-10-narrator-basics)将引导你完成讲述人用户体验。）

UWP 键盘行为与讲述人支持的键盘行为之间的部分差异如下：
-   未通过标准键盘导航公开的其他 UI 元素导航键组合（如 Caps lock + 箭头键用于读取控件标签）。
-   至已禁用项的导航。 默认情况下，已禁用项未通过标准键盘导航公开。
-   用于根据 UI 粒度进行更快导航的控件“视图”。 用户可以导航至项目、字符、字、行、段落、链接、标题、表格、地标和建议位置。 标准键盘导航将这些对象公布为简单列表，如果不提供快捷键则会使导航比较麻烦。

#### <a name="case-study--autosuggestbox-control"></a>案例研究 – AutoSuggestBox control

使用 Tab 键和方向箭的标准键盘导航无法访问 `AutoSuggestBox`“搜索”按钮，因为用户可以按 **Enter** 键提交搜索查询。 但是，当用户按 Caps Lock + 箭头键时，可以通过讲述人对其进行访问。

![自动建议键盘焦点](images/keyboard/auto-suggest-keyboard.png)

*With keyboard, users press the* ***Enter*** *key to submit search query*

<table>
  <tr>
    <td>
      <p><img src="images/keyboard/auto-suggest-narrator-1.png" alt="autosuggest narrator focus"/></p>
      <p><em>With Narrator, users press the <strong>Enter</strong> key to submit search query</em></p>
    </td>
    <td>
      <p><img src="images/keyboard/auto-suggest-narrator-2.png" alt="autosuggest narrator focus on search"/></p>
      <p><em>With Narrator, users are also able to access the search button using the <strong>Caps Lock + Right arrow key</strong>, then pressing <strong>Space</strong> key</em></p>
    </td>
  </tr>
</table>

### <a name="keyboard-and-the-xbox-gamepad-and-remote-control"></a>键盘和 Xbox 游戏板和遥控器

Xbox 游戏板和遥控器支持许多 UWP 键盘行为和体验。 但是，由于键盘上缺乏各种键选项，游戏板和遥控器缺少许多键盘优化（遥控器比游戏板更为受限）。

See [Gamepad and remote control interactions](gamepad-and-remote-interactions.md) for more detail on UWP support for gamepad and remote control input.

以下所示为键盘、游戏板和遥控器之间的部分键映射。

| **键盘**  | **手柄**                         | **Remote control**  |
|---------------|-------------------------------------|---------------------|
| 空格         | A 按钮                            | 选择按钮       |
| Enter         | A 按钮                            | 选择按钮       |
| 退出        | B 按钮                            | 后退按钮         |
| Home/End      | N/A                                 | N/A                 |
| 向上/向下翻页  | 扳机键按钮用于垂直滚动，缓冲键按钮用于水平滚动   | N/A                 |

设计 UWP 应用以供与游戏板和遥控器搭配使用时应注意的一些主要差异包括：
-   文本输入需要用户按 A 激活文本控件。
-   焦点导航不限于控件组，用户可以自由地导航至应用中的任何可聚焦 UI 元素。

    **注意**焦点可移至按键方向中的任何可聚焦 UI 元素，除非它是覆盖 UI 或者已指定[焦点占用](gamepad-and-remote-interactions.md#focus-engagement)，这将使焦点在通过 A 按钮启用/未启用时无法进入/退出区域。 有关更多信息，请参阅[方向导航](#directional-navigation)部分。
-   方向键和左摇杆按钮用于将焦点在控件之间移动，适合于内部导航。

    **注意**游戏板和遥控器只能导航至与按下的方向键具有相同视觉顺序的项目。 当没有可接收节点的后续元素时，该方向的导航将禁用。 键盘用户并不总是具有此约束，取决于具体情况。 有关更多信息，请参阅[内置键盘优化](#built-in-keyboard-optimization)部分。

#### <a name="directional-navigation"></a>Directional navigation

方向导航由 UWP [Focus Manager](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.FocusManager) 帮助程序类管理，它将按下方向键（箭头键、方向键）并尝试移动对应的视觉方向的焦点。

Unlike the keyboard, when an app opts out of [Mouse Mode](gamepad-and-remote-interactions.md#mouse-mode), directional navigation is applied across the entire application for gamepad and remote control. See [Gamepad and remote control interactions](gamepad-and-remote-interactions.md) for more detail on directional navigation optimization.

**注意**使用键盘 Tab 键的导航不被视为方向导航。 有关更多信息，请参阅[制表位](#tab-stops)部分。

<table>
  <tr>
    <td>
      <p><img src="images/keyboard/directional-navigation.png" alt="directional navigation"/></p>
      <p><em><strong>Directional navigation supported</strong></br>Using directional keys (keyboard arrows, gamepad and remote control D-pad), user can navigate between different controls.</em></p>
    </td>
    <td>
      <p><img src="images/keyboard/no-directional-navigation.png" alt="no directional navigation"/></p>
      <p><em><strong>Directional navigation not supported</strong> </br>用户无法使用方向键在不同控件之间导航。 Other methods of navigating between controls (tab key) are not impacted.</em></p>
    </td>
  </tr>
</table>

### <a name="built-in-keyboard-optimization"></a>Built in keyboard optimization

可以专门针对键盘输入进行 UWP 应用，具体取决于使用的布局和控件。

以下示例显示了已分配至单个制表位的一组列表项、网格项和菜单项（请参阅[制表位](#tab-stops)部分）。 当组具有焦点时，将通过方向键以对应的视觉顺序执行内部导航（请参阅[导航](#navigation)部分）。

![单列箭头键导航](images/keyboard/single-column-arrow.png)

***Single Column Arrow Key Navigation***

![单行箭头键导航](images/keyboard/single-row-arrow.png)

***Single Row Arrow Key Navigation***

![多列/行箭头键导航](images/keyboard/multiple-column-and-row-navigation.png)

***Multiple Column/Row Arrow Key Navigation***

#### <a name="wrapping-homogeneous-list-and-grid-view-items"></a>导航同类列表和网格视图项

方向导航并非总是导航多行和多列的列表和 GridView 项的最有效方法。

**注意**菜单项通常为单列列表，但在某些情况下，特殊的焦点规则可能也适用（请参阅[弹出窗口 UI](#popup-ui)）。

可以创建具有多行和多列的列表和网格对象。 这些通常为行主序（项目线填充整行，然后再填充下一行）或列主序（项目先填充整列，然后再填充下一列）。 行或列主序取决于滚动方向，你应确保项目顺序不会与此方向冲突。

在行主序（项目从左至右、从上到下填充）中，当焦点位于某行的最后一个项目并按下向右箭头键时，焦点将移至下一行的第一个项目。 相同的行为将以相反的顺序发生：当焦点设为某行的第一个项目且按下向左箭头键时，焦点将移至上一行的最后一个项目。

在列主序（项目从上到下、从左至右填充）中，当焦点位于某列的最后一个项目且按下向下箭头键时，焦点将移至下一列的第一个项目。 相同的行为以相反的顺序发生：当焦点设为某列的第一个项目且按下向上箭头键时，焦点将移至上一列的最后一个项目。

<table>
  <tr>
    <td>
      <p><img src="images/keyboard/row-major-keyboard.png" alt="row major keyboard navigation"/></p>
      <p><em>Row major keyboard navigation</em></p>
    </td>
    <td>
      <p><img src="images/keyboard/column-major-keyboard.png" alt="column major keyboard navigation"/></p>
      <p><em>Column major keyboard navigation</em></p>
    </td>
  </tr>
</table>

#### <a name="popup-ui"></a>Popup UI

As mentioned, you should try to ensure directional navigation corresponds to the visual order of the controls in your application's UI.

Some controls (such as the context menu, CommandBar overflow menu, and AutoSuggest menu) display a menu popup in a location and direction (downwards by default) relative to the primary control and available screen space. Note that the opening direction can be affected by a variety of factors at run time.

<table>
  <td><img src="images/keyboard/command-bar-open-down.png" alt="command bar opens down with down arrow key" /></td>
  <td><img src="images/keyboard/command-bar-open-up.png" alt="command bar opens up with down arrow key" /></td>
</table>

For these controls, when the menu is first opened (and no item has been selected by the user), the Down arrow key always sets focus to the first item while the Up arrow key always sets focus to the last item on the menu. 

If the last item has focus and the Down arrow key is pressed, focus moves to the first item on the menu. Similarly, if the first item has focus and the Up arrow key is pressed, focus moves to the last item on the menu. This behavior is referred to as *cycling* and is useful for navigating popup menus that can open in unpredictable directions.

> [!NOTE]
> Cycling should be avoided in non-popup UIs where users might come to feel trapped in an endless loop. 

We recommend that you emulate these same behaviors in your custom controls. Code sample on how to implement this behavior can be found in [Programmatic focus navigation](focus-navigation-programmatic.md#find-the-first-and-last-focusable-element) documentation.

## <a name="test-your-app"></a>测试你的应用

使用所有受支持的输入设备测试你的应用，确保可以一致、直观的方式导航至 UI 元素并且没有意外的元素会干扰所需的 Tab 键顺序。

## <a name="related-articles"></a>相关文章
* [Keyboard events](keyboard-events.md)
* [标识输入设备](identify-input-devices.md)
* [Respond to the presence of the touch keyboard](respond-to-the-presence-of-the-touch-keyboard.md)
* [焦点视觉对象示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)

## <a name="appendix"></a>附录

### <a name="software-keyboard"></a>Software keyboard

软件键盘是一种显示在屏幕上的键盘，用户可以借助触摸、鼠标、笔/触笔或其他指针设备（不需要触摸屏）来使用屏幕键盘代替物理键盘键入和输入数据。 在触摸屏上，可以通过直接触摸这些键盘来输入文本。 在 Xbox One 设备上，需要通过移动焦点视觉对象或使用游戏板或遥控器的快捷键选择单个键。

![Windows 10 触摸键盘](images/keyboard/default.png)

***Windows 10 Touch Keyboard***

![Xbox one 屏幕键盘](images/keyboard/xbox-onscreen-keyboard.png)

***Xbox One Onscreen Keyboard***

触摸键盘将在文本字段或其他可编辑的文本控件获得焦点，或者用户通过**通知中心**手动启用它时显示，具体取决于所用设备：

![通知中心中的触摸键盘图标](images/keyboard/touch-keyboard-notificationcenter.png)

如果你的应用以编程方式将焦点设置到文本输入控件，将不调用触摸键盘。 这可以消除并非由用户直接策划的非预期性行为。 不过，该键盘会在焦点以编程方式移至非文本输入控件时自动隐藏。

当用户在表单的控件之间导航时，触摸键盘通常保持可见。 此行为可能根据表单中的其他控件类型而有所不同。

下面是非编辑控件列表，这些控件可在文本输入会话期间使用触摸键盘接收焦点，而无需解除该键盘。 触摸键盘仍然位于视图中（而不会对 UI 进行不必要的改动，也不会对用户造成迷惑），因为用户可能需要使用触摸键盘在这些控件和文本输入之间来回切换。

-   复选框
-   组合框
-   单选按钮
-   滚动条
-   树
-   树项目
-   Menu
-   菜单栏
-   菜单项目
-   工具栏
-   列表
-   列表项

下面是适用于触摸键盘的其他模式的示例。 第一个图像是默认布局，第二个图像是缩略图布局（可能不适用于所有语言）。

![默认布局模式中的触摸键盘](images/keyboard/default.png)

***The touch keyboard in default layout mode***

![扩展布局模式中的触摸键盘](images/keyboard/extendedview.png)

***The touch keyboard in expanded layout mode***

成功的键盘交互使用户能够仅使用键盘来完成基本应用方案；即用户可以访问所有交互元素并激活默认功能。 很多因素都可能会对键盘交互的成功产生或多或少的影响，其中包括键盘导航、用于辅助功能的访问键，以及面向高级用户的加速键（或快捷方式）。

**NOTE**  The touch keyboard does not support toggle and most system commands.

#### <a name="on-screen-keyboard"></a>屏幕键盘
和软件键盘一样，屏幕键盘也是一种可视软件键盘，你可以借助触摸、鼠标、笔/触笔或其他指针设备（不需要触摸屏）来使用屏幕键盘代替物理键盘键入和输入数据。 屏幕键盘是针对没有物理键盘的系统提供的，或者是为行动有障碍而无法使用传统物理输入设备的用户提供使用。 屏幕键盘模拟硬件键盘的大部分功能（如果不是全部功能）。

可通过“设置”&gt;“轻松使用”从键盘页面打开屏幕键盘。

**注意** 屏幕键盘优先级高于触摸键盘，如果提供了屏幕键盘，将不显示触摸键盘。

![屏幕键盘](images/keyboard/osk.png)

***On-Screen Keyboard***

有关屏幕键盘的更多详细信息，请访问[屏幕键盘页面](https://support.microsoft.com/help/10762/windows-use-on-screen-keyboard)。

## <a name="related-articles"></a>相关文章

- [键盘辅助功能](../accessibility/keyboard-accessibility.md)
