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
ms.openlocfilehash: e3fcf6b792990fad9cb0071aece878cac31f5420
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662912"
---
# <a name="keyboard-interactions"></a>键盘交互

![键盘 Hero 图像](images/keyboard/keyboard-hero.jpg)

了解如何设计和优化你的 UWP 应用，使其为键盘高级用户以及残障人士和具有其他辅助功能需求的用户提供可能的最佳体验。

跨设备、键盘输入是整个通用 Windows 平台 (UWP) 交互体验的重要部分。 设计出色的键盘体验可以让用户高效地导航应用的 UI 并访问其所有功能，而无需将其双手抬离键盘。

![键盘和游戏板图像](images/keyboard/keyboard-gamepad.jpg)

***键盘和游戏板共享常见的交互模式***

在本主题中，我们将重点介绍适用于电脑上的键盘输入的 UWP 应用设计。 但是，设计良好的键盘的体验非常重要的支持可访问性工具，如 Windows 讲述人，使用[软件键盘](#software-keyboard)如触摸键盘和屏幕键盘 (OSK)，并处理其他输入的设备类型，如 Xbox 游戏手柄和远程控制。

此处讨论的许多指南和建议（包括[焦点视觉对象](#focus-visuals)、[访问键](#access-keys)和 [UI 导航](#navigation)）也适用于其他情况。

**注意**  硬件和软件键盘均可用于文本输入，但是本主题的重点在于导航和交互。

## <a name="built-in-support"></a>内置支持

键盘与鼠标是电脑上广泛使用的外设，它们是电脑体验的基础部分。 电脑用户希望在应用和单个应用响应键盘输入时获得全面、一致的体验。

虽然平台本身可为创建最适合于你的自定义控件和应用的键盘体验提供广泛基础，但是，所有 UWP 控件均包括内置支持，以实现丰富的键盘体验和用户交互。

![具有手机图像的键盘](images/keyboard/keyboard-phone.jpg)

***UWP 支持键盘与任何设备***

## <a name="basic-experiences"></a>基本体验
![基于焦点的设备](images/keyboard/focus-based-devices.jpg)

如前面所述，输入设备（如 Xbox 游戏板和遥控器）和辅助功能工具（如讲述人）具有许多相同的导航和命令键盘输入体验。 输入设备和工具中的共同体验可以减少你的额外工作，有助于实现通用 Windows 平台“构建一次即可在任何地方运行”的目标。

必要时，我们将识别应能够识别并描述任何缓解措施，应考虑的主要区别。

以下是本主题中将会讨论的设备和工具：

| 设备/工具                       | 描述     |
|-----------------------------------|-----------------|
|键盘（硬件和软件）   |除了标准的硬件键盘，UWP 应用程序支持两个软件键盘：[触摸屏输入 （或软件） 键盘](#software-keyboard)并[屏幕键盘](#on-screen-keyboard)。|
|游戏板和遥控器         |Xbox 游戏板和遥控器是 [10 英尺体验](../devices/designing-for-tv.md)中的基础输入设备。 有关游戏板和遥控器的 UWP 支持的特定详细信息，请参阅[游戏板和遥控器交互](gamepad-and-remote-interactions.md)。|
|屏幕读取器（讲述人）          |讲述人是一款面向 Windows 的内置屏幕读取器，可提供独特的交互体验和功能，但仍依赖于基本的键盘导航和输入。 有关讲述人的详细信息，请参阅[讲述人入门](https://support.microsoft.com/help/22798/windows-10-narrator-get-started)。|

## <a name="custom-experiences-and-efficient-keyboarding"></a>自定义体验和高效键盘操作
如前所述，为了确保应用程序非常适合具有不同技能、能力和期望的用户，键盘支持是不可或缺的。 建议优先考虑以下各项。
- 支持键盘导航和交互
    - 确保将可操作项识别为制表位（非可操作项不是制表位），并且导航顺序符合逻辑且可预测（请参阅[制表位](#tab-stops)）
    - 将初始焦点设置在最符合逻辑的元素上（请参阅[初始焦点](#initial-focus)）
    - 为“内部导航”提供箭头键导航（请参阅[导航](#navigation)）
- 支持键盘快捷方式
    - 为快速操作提供加速键（请参阅[加速器](#accelerators)）
    - 提供访问键可浏览应用程序的 UI (请参阅[访问密钥](access-keys.md))

### <a name="focus-visuals"></a>焦点视觉对象

UWP 支持适合于所有输入类型和体验的单个焦点视觉对象设计。
![焦点视觉对象](images/keyboard/focus-visual.png)

焦点视觉对象：

- 在 UI 元素接收到来自键盘和/或游戏板/遥控器的焦点时显示
- 呈现为 UI 元素周围的突出显示边框，以指示可以采取操作
- 帮助用户在不迷失的情况下导航应用 UI
- 可以为你的应用自定义（请参阅[高可见性焦点视觉对象](guidelines-for-visualfeedback.md#high-visibility-focus-visuals)）

**注意**UWP 焦点视觉对象与讲述人焦点矩形不相同。

### <a name="tab-stops"></a>制表位

若要将控件（包括导航元素）与键盘结合使用，该控件必须具有焦点。 若要接收键盘焦点的控件的一种方法是使其可以通过 tab 键导航访问通过标识为一个选项卡停止应用程序的 tab 键顺序。

要包含在 tab 键顺序中的控件[IsEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_IsEnabled)属性必须设置为**true**并[IsTabStop](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_IsTabStop)属性必须设置为**true**.

若要专门从 tab 键顺序中排除一个控件，请设置[IsTabStop](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_IsTabStop)属性设置为**false**。

默认情况下，Tab 键顺序反映了 UI 元素的创建顺序。 例如，如果 `StackPanel` 包含 `Button`、`Checkbox` 和 `TextBox`，则 Tab 键顺序为 `Button`、`Checkbox` 和 `TextBox`。

可以通过设置重写默认 tab 键顺序[TabIndex](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_TabIndex)属性。

#### <a name="tab-order-should-be-logical-and-predictable"></a>Tab 键顺序应符合逻辑且可预测

使用符合逻辑且可预测的 Tab 键顺序的出色键盘导航模型可以让你的应用更加直观，并且有助于用户更高效且有效地浏览、发现和访问功能。

所有交互式控件应具有的所有制表位 (除非它们是在[组](#control-group))，而非交互控件，例如标签，不应。

避免使用会让焦点在应用程序中跳来跳去的自定义 Tab 键顺序。 例如，表单中的控件列表的 Tab 键顺序应当是从顶部到底部，从左边到右边（取决于区域设置）。

请参阅[键盘辅助功能](../accessibility/keyboard-accessibility.md)有关自定义选项卡停止点的详细信息。

#### <a name="try-to-coordinate-tab-order-and-visual-order"></a>尝试协调 Tab 键顺序和视觉对象顺序

协调 tab 键顺序和视觉顺序 （也称为读取顺序或显示顺序） 可帮助降低用户感到困惑，因为用户导航到应用程序的 UI。

先尝试以 Tab 键顺序和视觉对象顺序分级和显示最重要的命令、控件和内容。 然而，实际的显示位置可能取决于父布局容器，以及会影响该布局的子元素的某些属性。 特别地是，使用网格标记或表格标记的布局可具有一个与 Tab 键顺序完全不同的视觉对象顺序。

**注意**视觉对象顺序也依赖于区域和语言。

### <a name="initial-focus"></a>初始焦点

初始焦点用于指定在首次启动或激活应用程序或页面时接收焦点的 UI 元素。 在使用键盘，它是从用户启动交互应用程序的 UI 与此元素。

对于 UWP 应用，初始焦点设置为具有最高的元素[TabIndex](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_TabIndex) ，可以接收焦点。 容器控件的子元素将被忽略。 因此，可视化树中的第一个元素接收焦点。

#### <a name="set-initial-focus-on-the-most-logical-element"></a>将初始焦点设置为最符合逻辑的元素

将初始焦点设置为用户在启动应用或导航页面时最有可能采取的第一个（或主要）操作的 UI 元素上。 一些示例如下：
-   在照片应用中，焦点被设为库中的第一个项目
-   在音乐应用中，焦点被设为播放按钮

#### <a name="dont-set-initial-focus-on-an-element-that-exposes-a-potentially-negative-or-even-disastrous-outcome"></a>不公开潜在的负面，或甚至灾难性的结果元素上设置初始焦点

此级别应该是功能的用户的选择。 将初始焦点设为具有重要结果的元素可能会导致出现意外的数据丢失或系统访问。 例如，未将焦点设置为删除按钮，导航到一封电子邮件时。

请参阅[焦点导航](focus-navigation.md)有关重写 tab 键顺序的详细信息。

### <a name="navigation"></a>导航

通常情况下是通过 Tab 键和箭头键支持键盘导航。

![Tab 和箭头键](images/keyboard/tab-and-arrow.png)

默认情况下，UWP 控件遵循以下基本的键盘行为：
-   **Tab 键**用于以 Tab 键顺序在可操作/活动控件之间导航。
-   **Shift + Tab** 用于以与 Tab 键顺序相反的顺序导航控件。 如果用户已使用箭头键在控件内导航，则焦点将设为控件内的最后一个已知值。
-   **箭头键**显示控件特定的“内部导航”。当用户进入“内部导航”时，箭头键不会在控件外导航。 一些示例如下：
    -   向上/向下箭头键将焦点内的移`ListView`和 `MenuFlyout`
    -   修改当前所选的值`Slider`和 `RatingsControl`
    -   移动插入符号置于 `TextBox`
    -   展开/折叠中的项 `TreeView`

使用这些默认行为来优化应用程序的键盘导航。

#### <a name="use-inner-navigation-with-sets-of-related-controls"></a>通过多组相关控件使用“内部导航”

提供为一组相关控件的箭头键导航强调了应用程序的 UI 的整个组织内及其关系。

例如，此处所示的 `ContentDialog` 控件在默认情况下将会为水平行的按钮提供内部导航（有关自定义控件，请参阅[控件组](#control-group)部分）。

![对话示例](images/keyboard/dialog.png)

***与一组相关的按钮的交互更轻松地使用箭头键导航***

如果显示的是单列中的项目，向上/向下箭头键将会导航项目。 如果显示的是单行中的项目，则向右/向左箭头键将会导航项目。 如果项目位于多个列中，则所有 4 个箭头键均将用于导航。

#### <a name="define-a-single-tab-stop-for-a-collection-of-related-controls"></a>定义单个制表位的一系列相关的控件

通过定义单个制表位的相关，或互为补充的控件的集合，您可以尽量减少总体制表位的应用程序中。

例如，下图所示为两个堆叠的 `ListView` 控件。 左图所示为使用制表位在 `ListView` 控件之间进行导航的箭头键导航，右图所示为如何更轻松、高效地在子元素之间导航而无需通过 Tab 键遍历父控件。


<table>
  <td><img src="images/keyboard/arrow-and-tab.png" alt="arrow and tab" /></td>
  <td><img src="images/keyboard/arrow-only.png" alt="arrow only" /></td>
</table>

***与两个堆积 ListView 控件的交互可更轻松、 更高效： 消除制表位，与只是箭头键导航。***

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

键盘快捷方式可以更轻松地同时对辅助功能的增强支持和键盘用户效率得到增强，从而使用您的应用程序。

除了应用程序中支持的键盘导航和激活，它也是很好的做法的应用程序的功能提供快捷方式。 Tab 键导航提供了很好，基本级别的键盘支持，但你可能想要添加的快捷键也支持更复杂的 UI。 

快捷方式是一套键盘组合，通过为用户提供一种可访问应用程序功能的高效方法来提高效率。 有两种类型的快捷方式：
-   [加速器](#accelerators)是调用应用程序命令的快捷方式。 您的应用程序可能会或可能不会提供对应于命令的特定 UI。 加速器通常包含的 Ctrl 键和字母键。
-   [访问密钥](#access-keys)是将焦点设置到你的应用程序中的特定用户界面的快捷方式。 访问密钥通常包含的 Alt 键和字母键。

提供跨应用程序支持类似的任务的一致的键盘快捷方式会使它们更有用、 功能强大，可帮助用户记住它们。

#### <a name="accelerators"></a>加速器

加速器可以帮助用户得更快速高效地在应用程序中执行常见操作。 

加速器示例：
-   按 Ctrl + N 键任意位置在**邮件**应用将启动新的邮件项。
-   按 Ctrl + E Microsoft Edge 中的任意位置的密钥 （和许多 Microsoft Store 应用程序） 将启动搜索。

加速器具有以下特征：
-   它们主要使用 Ctrl 和函数键 （Windows 系统键盘快捷方式使用 Alt + 非字母数字键和 Windows 徽标键） 的序列。
-   它们仅分配给最常用的命令。
-   它们的设计目的在于被用户记住，仅在菜单、工具提示和帮助中提供相关说明。
-   支持时，它们会产生影响整个应用程序。
-   它们应被所存储并不直接记录一致地分配。

#### <a name="access-keys"></a>访问键

有关 UWP 中的支持访问键的更多深入信息，请参阅[访问键](access-keys.md)页面。

访问键可以帮助一次只能按一个键的残障人士用户操作 UI 中的特定项目。 此外，访问键可用于传播其他快捷方式，以帮助高级用户更快地执行操作。

访问键具有以下特征：
-   它们使用 Alt 键 + 字母数字键。
-   它们主要用于辅助功能。
-   直接在 UI 中，控件，旁边有文档记录通过[键提示](access-keys.md)。
-   它们仅在当前窗口中有效，用于导航到对应的菜单项或控件。
-   访问密钥应一致地分配到常用的命令 （尤其是提交按钮），只要有可能。
-   应对其执行本地化。

#### <a name="common-keyboard-shortcuts"></a>常见的键盘快捷方式

下表是经常使用的键盘快捷方式的小示例。 

| 操作                               | 键命令                                      |
|--------------------------------------|--------------------------------------------------|
| 全选                           | Ctrl+A                                           |
| 连续选择                  | Shift+箭头键                                  |
| 保存                                 | Ctrl+S                                           |
| 查找                                 | Ctrl+F                                           |
| Print                                | Ctrl+P                                           |
| 复制                                 | Ctrl+C                                           |
| 剪切                                  | Ctrl+X                                           |
| 粘贴                                | Ctrl+V                                           |
| 撤销                                 | Ctrl+Z                                           |
| 下一个选项卡                             | Ctrl+Tab                                         |
| 关闭选项卡                            | Ctrl+F4 或 Ctrl+W                                |
| 语义式缩放                        | Ctrl++ 或 Ctrl+-                                 |

Windows 系统快捷方式的完整列表，请参阅[Windows 键盘快捷键](https://support.microsoft.com/help/12445/windows-keyboard-shortcuts)。 常见的应用程序快捷方式，请参阅[键盘快捷方式的 Microsoft 应用程序进行](https://support.microsoft.com/help/13805/windows-keyboard-shortcuts-in-apps)。

## <a name="advanced-experiences"></a>高级体验

在本部分，我们将讨论 UWP 应用支持的一些更为复杂的键盘交互体验，以及在不同的设备上通过不同的工具使用应用时需要注意的一些行为。

### <a name="control-group"></a>控件组

你可以在已使用箭头键启用“内部导航”的“控件组”（或方向区域）中对一组相关或补充性的控件进行分组。 控件组可以是单个制表位，或者你可以在控制组中指定多个制表位。

#### <a name="arrow-key-navigation"></a>箭头键导航

用户期望当以下位置的 UI 区域中存在一组类似的相关控件时也支持箭头键导航：
-   `AppBarButtons` 在 `CommandBar`
-   `ListItems` 或`GridItems`内`ListView`或 `GridView`
-   `Buttons` 内部 `ContentDialog`

UWP 控件默认情况下之后箭头键导航。 有关自定义布局和控制组，请使用 `XYFocusKeyboardNavigation="Enabled"` 以提供类似的行为。

请考虑添加对箭头键导航的支持时使用以下控件：

<table>
  <tr>
    <td>
      <p><img src="images/keyboard/dialog.png" alt="Dialog buttons"/></p>
      <p><sup>对话框按钮</sup></p>
      <p><img src="images/keyboard/radiobutton.png" alt="Radio buttons"/></p>
      <p><sup>RadioButtons</sup></p>     
    </td>
    <td>
      <p><img src="images/keyboard/appbar.png" alt="AppBar buttons"/></p>
      <p><sup>AppBarButtons</sup></p>
      <p><img src="images/keyboard/list-and-grid-items.png" alt="List and Grid items"/></p>
      <p><sup>ListItems 和 GridItems</sup></p>
    </td>    
  </tr>
</table>

#### <a name="tab-stops"></a>制表位

根据应用程序的功能和布局，控制组最佳的导航选项可能是单个制表位箭头导航到子元素、 多个制表位或某种组合。

##### <a name="use-multiple-tab-stops-and-arrow-keys-for-buttons"></a>为按钮使用多个制表位和箭头键

辅助功能用户依赖于完善的键盘导航规则，它们通常不使用箭头键导航按钮集合。 但是，没有视觉障碍的用户可能会觉得行为非常自然。

本案例中默认的 UWP 行为示例为 `ContentDialog`。 箭头键可以在按钮之间导航，而且每个按钮也是一个制表位。

##### <a name="assign-single-tab-stop-to-familiar-ui-patterns"></a>为熟悉的 UI 模式分配单个制表位

如果你的布局遵循的是著名的控件组 UI 模式，则为组分配单个制表位可以提高用户的导航效率。

示例包括：
-   `RadioButtons`
-   多个`ListViews`，如下所示，其行为类似一个 `ListView`
-   外观和行为类似于磁贴（例如“开始”菜单磁贴）网格的任何 UI

#### <a name="specifying-control-group-behavior"></a>指定控件组行为

使用以下 API 支持自定义控件组行为（所有这些均会在本主题的后面详细讨论）：

-   [XYFocusKeyboardNavigation](focus-navigation.md#2d-directional-navigation-for-keyboard) 支持空间之间的箭头键导航
-   [TabFocusNavigation](focus-navigation.md#tab-navigation) 指示是存在多个制表位还是单个制表位
-   [FindFirstFocusableElement and FindLastFocusableElement](focus-navigation-programmatic.md#find-the-first-and-last-focusable-element) 将通过 **Home** 键将焦点设置在第一个项目上并通过 **End** 键将焦点设置在最后一个项目上

下图所示为关联单选按钮控件组的直观键盘导航行为。 在本案例中，我们建议为控件组设置单个制表位、使用箭头键在单选按钮之间进行内部导航、将 **Home** 键绑定至第一个单选按钮并将 **End** 键绑定至最后一个单选按钮。

![组织到一起](images/keyboard/putting-it-all-together.png)

### <a name="keyboard-and-narrator"></a>键盘和讲述人

讲述人是一款面向键盘用户的 UI 辅助功能工具（也支持其他输入类型）。 但是，讲述人功能超出了 UWP 应用支持的键盘交互，因此，为讲述人设计 UWP 应用时需要注意。 （[讲述人基础知识页面](https://support.microsoft.com/help/22808/windows-10-narrator-learning-basics)将引导你完成讲述人用户体验。）

UWP 键盘行为与讲述人支持的键盘行为之间的部分差异如下：
-   未通过标准键盘导航公开的其他 UI 元素导航键组合（如 Caps lock + 箭头键用于读取控件标签）。
-   至已禁用项的导航。 默认情况下，已禁用项未通过标准键盘导航公开。
    -   用于根据 UI 粒度进行更快导航的控件“视图”。 用户可以导航至项目、字符、字、行、段落、链接、标题、表格、地标和建议位置。 标准键盘导航将这些对象公布为简单列表，如果不提供快捷键则会使导航比较麻烦。

#### <a name="case-study--autosuggestbox-control"></a>案例研究 – AutoSuggestBox control

使用 Tab 键和方向箭的标准键盘导航无法访问 `AutoSuggestBox`“搜索”按钮，因为用户可以按 **Enter** 键提交搜索查询。 但是，当用户按 Caps Lock + 箭头键时，可以通过讲述人对其进行访问。

![自动建议键盘焦点](images/keyboard/auto-suggest-keyboard.png)

*使用键盘，按用户* ***Enter*** *键，提交搜索查询*

<table>
  <tr>
    <td>
      <p><img src="images/keyboard/auto-suggest-narrator-1.png" alt="autosuggest narrator focus"/></p>
      <p><em>使用讲述人按用户<strong>Enter</strong>键，提交搜索查询</em></p>
    </td>
    <td>
      <p><img src="images/keyboard/auto-suggest-narrator-2.png" alt="autosuggest narrator focus on search"/></p>
      <p><em>讲述人，用户也都可以访问搜索按钮使用<strong>Caps Lock + 右箭头键</strong>，然后按<strong>空间</strong>密钥</em></p>
    </td>
  </tr>
</table>

### <a name="keyboard-and-the-xbox-gamepad-and-remote-control"></a>键盘和 Xbox 游戏板和遥控器

Xbox 游戏板和遥控器支持许多 UWP 键盘行为和体验。 但是，由于键盘上缺乏各种键选项，游戏板和遥控器缺少许多键盘优化（遥控器比游戏板更为受限）。

请参阅[游戏手柄和远程控制交互](gamepad-and-remote-interactions.md)的游戏手柄和遥控器输入 UWP 支持的详细信息。

以下所示为键盘、游戏板和遥控器之间的部分键映射。

| **键盘**  | **Gamepad**                         | **远程控制**  |
|---------------|-------------------------------------|---------------------|
| 空格         | A 按钮                            | 选择按钮       |
| Enter         | A 按钮                            | 选择按钮       |
| 退出        | B 按钮                            | 后退按钮         |
| Home/End      | 不适用                                 | 不适用                 |
| 向上/向下翻页  | 扳机键按钮用于垂直滚动，缓冲键按钮用于水平滚动   | 不适用                 |

设计 UWP 应用以供与游戏板和遥控器搭配使用时应注意的一些主要差异包括：
-   文本输入需要用户按 A 激活文本控件。
-   焦点导航不限于控件组，用户可以自由地导航至应用中的任何可聚焦 UI 元素。

    **注意**焦点可移至按键方向中的任何可聚焦 UI 元素，除非它是覆盖 UI 或者已指定[焦点占用](gamepad-and-remote-interactions.md#focus-engagement)，这将使焦点在通过 A 按钮启用/未启用时无法进入/退出区域。 有关更多信息，请参阅[方向导航](#directional-navigation)部分。
-   方向键和左摇杆按钮用于将焦点在控件之间移动，适合于内部导航。

    **注意**游戏板和遥控器只能导航至与按下的方向键具有相同视觉顺序的项目。 当没有可接收节点的后续元素时，该方向的导航将禁用。 键盘用户并不总是具有此约束，取决于具体情况。 有关更多信息，请参阅[内置键盘优化](#built-in-keyboard-optimization)部分。

#### <a name="directional-navigation"></a>方向导航

方向导航由 UWP [Focus Manager](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.FocusManager) 帮助程序类管理，它将按下方向键（箭头键、方向键）并尝试移动对应的视觉方向的焦点。

与键盘时应用 opts 外出时的不同[鼠标模式](gamepad-and-remote-interactions.md#mouse-mode)，游戏手柄和远程控制在整个应用程序应用方向导航。 请参阅[游戏手柄和远程控制交互](gamepad-and-remote-interactions.md)的方向导航优化的详细信息。

**注意**使用键盘 Tab 键的导航不被视为方向导航。 有关更多信息，请参阅[制表位](#tab-stops)部分。

<table>
  <tr>
    <td>
      <p><img src="images/keyboard/directional-navigation.png" alt="directional navigation"/></p>
      <p><em><strong>支持的方向导航</strong></br>使用方向键 （键盘箭头、 游戏板和远程控制方向键），用户可以导航之间不同的控件。</em></p>
    </td>
    <td>
      <p><img src="images/keyboard/no-directional-navigation.png" alt="no directional navigation"/></p>
      <p><em><strong>不支持的方向导航</strong> </br>用户无法使用方向键在不同控件之间导航。 导航控件 （tab 键） 之间的其他方法不会受到影响。</em></p>
    </td>
  </tr>
</table>

### <a name="built-in-keyboard-optimization"></a>内置键盘优化

可以专门针对键盘输入进行 UWP 应用，具体取决于使用的布局和控件。

以下示例显示了已分配至单个制表位的一组列表项、网格项和菜单项（请参阅[制表位](#tab-stops)部分）。 当组具有焦点时，将通过方向键以对应的视觉顺序执行内部导航（请参阅[导航](#navigation)部分）。

![单列箭头键导航](images/keyboard/single-column-arrow.png)

***单个列箭头键导航***

![单行箭头键导航](images/keyboard/single-row-arrow.png)

***单个行箭头键导航***

![多列/行箭头键导航](images/keyboard/multiple-column-and-row-navigation.png)

***多个列/行箭头键导航***

#### <a name="wrapping-homogeneous-list-and-grid-view-items"></a>导航同类列表和网格视图项

方向导航并非总是导航多行和多列的列表和 GridView 项的最有效方法。

**注意**菜单项通常为单列列表，但在某些情况下，特殊的焦点规则可能也适用（请参阅[弹出窗口 UI](#popup-ui)）。

可以创建具有多行和多列的列表和网格对象。 这些通常为行主序（项目线填充整行，然后再填充下一行）或列主序（项目先填充整列，然后再填充下一列）。 行或列主序取决于滚动方向，你应确保项目顺序不会与此方向冲突。

在行主序（项目从左至右、从上到下填充）中，当焦点位于某行的最后一个项目并按下向右箭头键时，焦点将移至下一行的第一个项目。 按相反的顺序会发生此相同的行为：当焦点设置为行中的第一项并且按下左的箭头键时，焦点移动到最后一个项上一行中。

在列主序（项目从上到下、从左至右填充）中，当焦点位于某列的最后一个项目且按下向下箭头键时，焦点将移至下一列的第一个项目。 按相反的顺序会发生此相同的行为：当焦点设置为列中的第一项并且按下向上箭头键时，焦点移动到最后一个项上一列中。

<table>
  <tr>
    <td>
      <p><img src="images/keyboard/row-major-keyboard.png" alt="row major keyboard navigation"/></p>
      <p><em>行主要键盘导航</em></p>
    </td>
    <td>
      <p><img src="images/keyboard/column-major-keyboard.png" alt="column major keyboard navigation"/></p>
      <p><em>列主键盘导航</em></p>
    </td>
  </tr>
</table>

#### <a name="popup-ui"></a>弹出 UI

如前文所述，您应尝试确保方向导航对应于应用程序的 UI 中控件的可视化顺序。

某些控件，如 ContextMenu、 AppBarOverflowMenu 和自动建议，包括位置和方向相对于主控件 （根据可用的屏幕空间） 中显示的弹出菜单。 例如，如果向下打开（默认方向）菜单的空间不足，那么它会向上打开。 无法保证每次打开菜单的方向均相同。

<table>
  <td><img src="images/keyboard/command-bar-open-down.png" alt="command bar opens down with down arrow key" /></td>
  <td><img src="images/keyboard/command-bar-open-up.png" alt="command bar opens up with down arrow key" /></td>
</table>

对于这些控件，首次打开菜单（且用户未选择任何项目）时，向下箭头键始终会将焦点设置为第一个项目，向上箭头键始终会将焦点设置为菜单上的最后一个项目。 同样地，当选择最后一个项目且按下向下箭头键时，焦点将会移至菜单上的第一个项目，当选择第一个项目且按下向上箭头键时，焦点将移至菜单上的最后一个项目。

你应尝试在你的自定义控件中模拟这些行为。 有关如何实现此行为的代码示例可在[编程实现的焦点导航](focus-navigation-programmatic.md#find-the-first-and-last-focusable-element)文档。

## <a name="test-your-app"></a>测试应用

使用所有受支持的输入设备测试你的应用，确保可以一致、直观的方式导航至 UI 元素并且没有意外的元素会干扰所需的 Tab 键顺序。

## <a name="related-articles"></a>相关文章
* [键盘事件](keyboard-events.md)
* [标识输入设备](identify-input-devices.md)
* [响应触摸键盘存在](respond-to-the-presence-of-the-touch-keyboard.md)
* [焦点视觉对象示例](https://go.microsoft.com/fwlink/p/?LinkID=619895)

## <a name="appendix"></a>附录

### <a name="software-keyboard"></a>软件键盘

软件键盘是一种显示在屏幕上的键盘，用户可以借助触摸、鼠标、笔/触笔或其他指针设备（不需要触摸屏）来使用屏幕键盘代替物理键盘键入和输入数据。 在触摸屏上，可以通过直接触摸这些键盘来输入文本。 在 Xbox One 设备上，需要通过移动焦点视觉对象或使用游戏板或遥控器的快捷键选择单个键。

![Windows 10 触摸键盘](images/keyboard/default.png)

***Windows 10 的触摸键盘***

![Xbox one 屏幕键盘](images/keyboard/xbox-onscreen-keyboard.png)

***Xbox One 屏幕键盘***

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
-   菜单
-   菜单栏
-   菜单项目
-   工具栏
-   列表
-   列表项

下面是适用于触摸键盘的其他模式的示例。 第一个图像是默认布局，第二个图像是缩略图布局（可能不适用于所有语言）。

![默认布局模式中的触摸键盘](images/keyboard/default.png)

***触摸屏输入在默认键盘布局模式***

![扩展布局模式中的触摸键盘](images/keyboard/extendedview.png)

***在展开的布局模式下触摸键盘***

成功的键盘交互使用户能够仅使用键盘来完成基本应用方案；即用户可以访问所有交互元素并激活默认功能。 很多因素都可能会对键盘交互的成功产生或多或少的影响，其中包括键盘导航、用于辅助功能的访问键，以及面向高级用户的加速键（或快捷方式）。

**请注意**  触摸键盘不支持切换和系统的大部分命令。

#### <a name="on-screen-keyboard"></a>屏幕键盘
和软件键盘一样，屏幕键盘也是一种可视软件键盘，你可以借助触摸、鼠标、笔/触笔或其他指针设备（不需要触摸屏）来使用屏幕键盘代替物理键盘键入和输入数据。 屏幕键盘是针对没有物理键盘的系统提供的，或者是为行动有障碍而无法使用传统物理输入设备的用户提供使用。 屏幕键盘模拟硬件键盘的大部分功能（如果不是全部功能）。

可通过“设置”&gt;“轻松使用”从键盘页面打开屏幕键盘。

**注意** 屏幕键盘优先级高于触摸键盘，如果提供了屏幕键盘，将不显示触摸键盘。

![屏幕键盘](images/keyboard/osk.png)

***屏幕键盘***

有关屏幕键盘的更多详细信息，请访问[屏幕键盘页面](https://support.microsoft.com/help/10762/windows-use-on-screen-keyboard)。

## <a name="related-articles"></a>相关文章

- [键盘辅助功能](../accessibility/keyboard-accessibility.md)
