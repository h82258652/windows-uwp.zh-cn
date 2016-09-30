---
author: mijacobs
Description: "在通用 Windows 平台 (UWP) 应用中，命令元素是使用户能够执行诸如发送电子邮件、删除项或提交表单等操作的交互式 UI 元素。"
title: "通用 Windows 平台 (UWP) 应用的命令设计基础知识"
ms.assetid: 1DB48285-07B7-4952-80EF-02B57D4469F2
label: Command design basics
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: be7ff187df9800a8c2c47c4315f3f9b021e265f8

---

#  UWP 应用的命令设计基础知识

在通用 Windows 平台 (UWP) 应用中，*命令元素*是使用户能够执行诸如发送电子邮件、删除项或提交表单等操作的交互式 UI 元素。 本文将介绍命令元素（如按钮和复选框）、它们支持的交互以及承载它们的命令图面（如命令栏和上下文菜单）。

## <span id="Provide_the_right_type_of_interactions"></span><span id="provide_the_right_type_of_interactions"></span><span id="PROVIDE_THE_RIGHT_TYPE_OF_INTERACTIONS"></span>提供正确类型的交互


在设计命令界面时，最重要的决定是选择用户应该能够执行哪些操作。 例如，如果你要创建一个照片应用，则用户将需要照片编辑工具。 但是，如果你要创建一个偶尔显示照片的社交媒体应用，则可能无需优先考虑图像编辑，因此可以省略编辑工具以节省空间。 确定想要用户完成的操作并提供工具帮助他们完成操作。

有关如何为应用规划正确交互的建议，请参阅[规划你的应用](https://msdn.microsoft.com/library/windows/apps/hh465427.aspx)。

## <span id="Use_the_right_command_element_for_the_interaction"></span><span id="use_the_right_command_element_for_the_interaction"></span><span id="USE_THE_RIGHT_COMMAND_ELEMENT_FOR_THE_INTERACTION"></span>使用正确的命令元素进行交互


使用正确的元素进行正确交互的意思可能是，可凭直觉使用的应用和看似费解或令人困惑的应用之间的差别。 Universal Windows Platform (UWP) 提供了大量可在应用中使用的、采用控件形式的命令元素。 下面是一些最常见控件的列表以及它们支持的交互的摘要。

| 类别              | 元素                                                                                                                                                                                                            | 交互                                                                                                                                        |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------|
| 按钮               | [按钮](https://msdn.microsoft.com/library/windows/apps/hh465470)                                                                                                                                                     | 触发一个即时操作，例如发送电子邮件、在对话框中确认某个操作、提交表单数据。                                    |
| 日期和时间选取器 | [日历日期选取器, 日历视图, 日期选取器, 时间选取器](https://msdn.microsoft.com/library/windows/apps/hh465466)                                                                                                                 | 使用户可以查看和修改日期和时间信息，例如当输入信用卡过期日期或设置闹钟时。                   |
| 列表                 | [下拉列表、列表框、列表视图和网格视图](https://msdn.microsoft.com/library/windows/apps/mt186889)                                                                                                                                              | 显示交互式列表或网格中的项。 使用这些元素以使用户可以从新发行电影列表中选择一部电影或管理清单。 |
| 预测文本输入 | [自动建议框](https://msdn.microsoft.com/library/windows/apps/dn997762)                                                                                                                                                                    | 在用户输入数据或执行查询时，在用户键入的同时提供建议，从而节省用户的时间。                                                   |
| 选择控件    | [复选框](https://msdn.microsoft.com/library/windows/apps/hh700393)、[单选按钮](https://msdn.microsoft.com/library/windows/apps/hh700395)、[切换开关](https://msdn.microsoft.com/library/windows/apps/hh465475) | 使用户可以选择不同的选项，例如，当完成一项调查或配置应用设置时。                                      |

 

有关完整列表，请参阅[控件和 UI 元素](https://dev.windows.com/design/controls-patterns)

## <span id="_________Place_commands_on_the_right_surface_______"></span><span id="_________place_commands_on_the_right_surface_______"></span><span id="_________PLACE_COMMANDS_ON_THE_RIGHT_SURFACE_______"></span> 将命令放置在合适的图面上


可以将命令元素放置在应用中的各种图面上，包括应用画布（应用的内容区域）或可以充当命令容器的特殊命令元素（例如命令栏、菜单、对话框和浮出控件）。 下面是放置命令的一些常规建议：

-   尽可能让用户在应用的画布上直接操纵内容，而不是添加作用于内容的命令。 例如，在旅行应用中，让用户通过在画布的列表中拖放活动来重新安排行程，而不是通过选择活动并使用“上移”或“下移”命令按钮。
-   否则，如果用户无法直接操作内容，则将命令放置在以下某个 UI 图面上：

    -   在[命令栏](https://msdn.microsoft.com/library/windows/apps/hh465302)中：应当将大部分命令放置在命令栏上，这有助于组织命令并使其易于访问。
    -   在应用画布上：如果用户位于仅具有单一用途的页面或视图上，则可以直接在画布上为此用途提供相应的命令。 这些命令应非常少。
    -   在[上下文菜单](https://msdn.microsoft.com/library/windows/apps/hh465308)中：可以使用上下文菜单执行剪贴板操作（如剪切、复制和粘贴）或适用于无法选择的内容的命令（如将图钉添加到地图上的某个位置）。

下面是 Windows 提供的命令图面和何时使用它们的建议的列表。

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">图面</th>
<th align="left">说明</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">应用画布（内容区域）
<p><img src="images/content-area.png" alt="The content area of an app" /></p></td>
<td align="left"><p>如果一个命令非常重要，并且用户在完成核心方案时需要不断地使用，可以将其放在画布上（应用的内容区域）。 由于你可以将命令放在其影响的对象附近（或放在这些对象上），将命令放置在画布上将使它们显而易见，易于使用。</p>
<p>但是，请谨慎选择放置在画布上的命令。 应用画布上具有过多命令将占用大量屏幕空间，可能会使用户不知所措。 如果命令不经常使用，请考虑将它放在另一个命令图面（例如菜单或命令栏的&quot;“更多”&quot;区域）中。</p></td>
</tr>
<tr class="even">
<td align="left">[命令栏](https://msdn.microsoft.com/library/windows/apps/hh465302)
<p><img src="images/controls-appbar-icons-200.png" alt="Example of a command bar with icons" /></p></td>
<td align="left"><p>命令栏使用户能够轻松地访问操作。 可以使用命令栏来显示特定于用户上下文的命令或选项，例如照片选择或绘图模式。</p>
<p>命令栏可以放置在屏幕顶部、屏幕底部，也可以同时放置在屏幕的顶部和底部。 此照片编辑应用的设计将显示内容区域和命令栏：</p>
<p><img src="images/commands-appcanvas-example.png" alt="A photo app" /></p>
<p>有关命令栏的详细信息，请参阅[命令栏指南](https://msdn.microsoft.com/library/windows/apps/hh465302)文章。</p></td>
</tr>
<tr class="odd">
<td align="left">[菜单和上下文菜单](../controls-and-patterns/dialogs-popups-menus.md)
<p><img src="images/controls-contextmenu-singlepane.png" alt="Example of a single-pane context menu" /></p></td>
<td align="left"><p>有时，将多个命令归组到一个命令菜单中效率会更高一些。 菜单可让你使用较少的空间显示更多选项。 菜单可以包括交互式控件。</p>
<p>可以使用上下文菜单快速访问常用操作并访问辅助命令（仅在某些上下文中是相关的）。</p>
<p>上下文菜单适用于以下类型的命令和命令应用场景：</p>
<ul>
<li>对文本选择执行如复制、剪切、粘贴、拼写检查等上下文操作。</li>
<li>需要对其执行操作但是无法选择或以其他方式指示的对象的命令。</li>
<li>显示剪贴板命令。</li>
<li>自定义命令。</li>
</ul>
<p>此示例显示了一个地铁应用的设计，该应用使用上下文菜单修改路线、将路线标记为书签或选择另一趟地铁。</p>
<p><img src="images/subway/uap-subway-ak-8in-dashboard-200.png" alt="A context menu in an subway app" /></p>
<p>有关上下文菜单的详细信息，请参阅[上下文菜单指南](https://msdn.microsoft.com/library/windows/apps/hh465308)文章。</p></td>
</tr>
<tr class="even">
<td align="left">[对话框控件](../controls-and-patterns/dialogs-popups-menus.md)
<p><img src="images/controls-dialog-twobutton-200.png" alt="Example of a simple two-button dialog" /></p></td>
<td align="left"><p>对话框是用于提供上下文应用信息的模式 UI 覆盖。 在大部分情况下，除非明确取消对话框，否则它会阻止与应用窗口的交互，并且通常会请求用户执行某类操作。</p>
<p>对话框可以中断，并且只应在某些情况下使用。 有关详细信息，请参阅[何时确认或撤消操作](#whentoconfirm)部分。</p></td>
</tr>
<tr class="odd">
<td align="left">[浮出控件](../controls-and-patterns/dialogs-popups-menus.md)
<p><img src="images/controls-flyout-default-200.png" alt="Image of default flyout" /></p></td>
<td align="left"><p>轻型上下文弹出窗口，用于显示与用户操作相关的 UI。 使用浮出控件执行以下操作：</p>
<p></p>
<ul>
<li>显示菜单。</li>
<li>显示有关某个项目的更多详细信息。</li>
<li>要求用户确认某个操作，而不阻止与应用的交互。</li>
</ul>
<p>可以通过点击或单击浮出控件之外的某个位置来取消浮出控件。 有关浮出控件的详细信息，请参阅[对话框、菜单和弹出窗口](../controls-and-patterns/dialogs-popups-menus.md)文章。</p></td>
</tr>
</tbody>
</table>

 

## <span id="whentoconfirm"></span><span id="WHENTOCONFIRM"></span>何时确认或撤消操作


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

**提示** 请务必注意你的应用使用确认对话框的频率；当用户犯错时，它们十分有用，但每当用户有意尝试执行某个操作时，它们反而是障碍。

 

## <span id="_________Optimize_for_specific_input_types_______"></span><span id="_________optimize_for_specific_input_types_______"></span><span id="_________OPTIMIZE_FOR_SPECIFIC_INPUT_TYPES_______"></span> 针对特定输入类型进行优化


有关优化特定输入类型或设备的用户体验的详细信息，请参阅[交互入门](../input-and-devices/input-primer.md)。




 

 







<!--HONumber=Jun16_HO4-->


