---
author: Karl-Bridge-Microsoft
Description: "在使用键盘和类事件处理程序的应用中响应来自硬件键盘或软键盘的按键操作。"
title: "键盘交互"
ms.assetid: FF819BAC-67C0-4EC9-8921-F087BE188138
label: Keyboard interactions
template: detail.hbs
keywords: "键盘, 辅助功能, 导航, 焦点, 文本, 输入, 用户交互"
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 53ee08b33bcbbd895d0c6ea6cd621eeec2af40f5
ms.lasthandoff: 02/07/2017

---

# <a name="keyboard-interactions"></a>键盘交互
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

键盘输入是应用的整体用户交互体验的一个重要部分。 对于残疾人士，或者只是认为键盘是与应用交互的最有效方法的用户而言，键盘非常重要。 例如，用户应可以使用 Tab 和箭头键导航应用，使用空格键和 Enter 键激活 UI 元素，并使用键盘快捷方式访问命令。  

![键盘 Hero 图像](images/input-patterns/input-keyboard-small.jpg)

<div class="important-apis" >
<b>重要的 API</b><br/>
<ul>
<li>[**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941)</li>
<li>[**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942)</li>
<li>[**KeyRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh943072)</li>
</ul>
</div>
 


具有良好设计的键盘 UI 是软件辅助功能的一个重要方面。 它使具有视力缺陷或行动有障碍的用户能够在应用中导航并与应用的功能交互。 这些用户可能无法操作鼠标，而是依靠各种辅助技术，如键盘增强工具、屏幕键盘、屏幕放大器、屏幕阅读器、语音输入实用工具。

用户可以通过硬件键盘，以及屏幕键盘 (OSK) 和触摸键盘这两个软件键盘与通用应用交互。

屏幕键盘  
屏幕键盘是一种可视软件键盘，你可以借助触摸、鼠标、笔/触笔或其他指针设备（不需要触摸屏）来使用屏幕键盘代替物理键盘键入和输入数据。 屏幕键盘是针对没有物理键盘的系统提供的，或者是为行动有障碍而无法使用传统物理输入设备的用户提供使用。 屏幕键盘模拟硬件键盘的大部分功能（如果不是全部功能）。

可通过“设置”&gt;“轻松使用”从键盘页面打开屏幕键盘。

**注意** 屏幕键盘优先级高于触摸键盘，如果提供了屏幕键盘，将不显示触摸键盘。

 

![屏幕键盘](images/input-patterns/osk.png)

<sup>屏幕键盘</sup>

触摸键盘  
触摸键盘是一种借助触摸输入来输入文本的可视软件键盘。 触摸键盘不能取代屏幕键盘，因为前者仅用于文本输入（它不模拟硬件键盘）。

触摸键盘将在文本字段或其他可编辑的文本控件获得焦点，或者用户通过**“通知中心”**手动启用它时显示，具体取决于所用设备：

![通知中心中的触摸键盘图标](images/input-patterns/touch-keyboard-notificationcenter.png)

**注意** 用户可能需要通过“设置”&gt;“系统”转到“平板电脑模式**屏幕并打开**将设备用作平板电脑时，这会使 Windows 更兼容触摸功能”，才能自动启用触摸键盘的外观。

 

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

下面是适用于触摸键盘的其他模式的示例。 第一个图像是默认布局，第二个图像是缩略图布局（可能不适用于所有语言）。
<table>
<tr>
    <td>**默认布局模式中的触摸键盘：  **</td>
    <td>![默认布局模式中的触摸键盘](images/touchkeyboard-standard.png)</td>
</tr>
<tr>
    <td>**扩展布局模式中的触摸键盘：  **</td>
    <td>![扩展布局模式中的触摸键盘](images/touchkeyboard-expanded.png)</td>
</tr>
<tr>
    <td>**默认缩略图布局模式中的触摸键盘：  **</td>
    <td>![缩略图布局模式中的触摸键盘](images/touchkeyboard-thumb.png)</td>
</tr>
<tr>
    <td>**数字缩略图布局模式中的触摸键盘：  **</td>
    <td>![数字缩略图布局模式中的触摸键盘](images/touchkeyboard-numeric-thumb.png)</td>
</tr>
</table>


成功的键盘交互使用户能够仅使用键盘来完成基本应用方案；即用户可以访问所有交互元素并激活默认功能。 很多因素都可能会对键盘交互的成功产生或多或少的影响，其中包括键盘导航、用于辅助功能的访问键，以及面向高级用户的加速键（或快捷方式）。

**注意** 触摸键盘不支持切换操作以及大多数系统命令（请参阅[模式](#keyboard_command_patterns)）。

## <a name="navigation"></a>导航


若要将控件（包括导航元素）与键盘结合使用，该控件必须具有焦点。 让控件可用于接收键盘焦点的一种方式是，让其可通过 Tab 键导航进行访问。 设计良好的键盘导航模型会提供可预测的 Tab 键逻辑顺序，以便用户可以快速高效地浏览和使用你的应用。

所有交互式控件都应该有 Tab 键停止点（除非它们是在某一组中），而非交互式控件（例如标签）则不必具备此类停止点。

相关控件集可组成一个控件组，并分配单个 Tab 键停止点。 控件组适用于行为类似于单个控件的控件集，例如单选按钮。 当有过多控件仅使用 Tab 键进行高效导航时，也可使用控制组。 借助箭头键、Home、End、Page Up 和 Page Down，可在某一组内的控件之间移动输入焦点（无法使用这些键退出控件组）。

当应用首次启动时，应将初始键盘焦点设置在用户将直观（或最有可能）进行交互的元素上。 通常，这是应用的主内容视图，以便用户可以立即开始使用箭头键滚动应用内容。

不要在可能会产生负面甚至是灾难性结果的元素上设置初始键盘焦点。 这样便可以防止数据或系统访问权限丢失。

先尝试以 Tab 键顺序和显示顺序（或可视化层次结构）分级和显示最重要的命令、控件和内容。 然而，实际的显示位置可能取决于父布局容器，以及会影响该布局的子元素的某些属性。 特别地是，使用网格标记或表格标记的布局可具有一个与 Tab 键顺序完全不同的阅读顺序。 这不会一直是个难题，但前提是你应基于可触摸 UI 和键盘辅助功能 UI 测试你的应用的功能。

Tab 键顺序应尽可能遵循阅读顺序。 这可以降低发生混淆的几率，不过具体取决于区域设置和语言。

可将键盘按钮与你的应用中的相应 UI（向后和向前按钮）相关联。

尝试尽可能简单直观地导航回应用的“开始”屏幕，并在键内容之间导航。

将箭头键用作键盘快捷方式，以便在复合元素的子元素间能够进行正确的内部导航。 如果树状视图节点有单独的子元素，用于处理展开/折叠和节点激活，则应使用向左和向右键提供键盘展开/折叠功能。 这与平台控件一致。

因为触摸键盘会遮挡住大部分屏幕，因此通用 Windows 平台 (UWP) 将确保当用户在表单上的不同控件（包括当前不在视图中的控件）之间导航时，具有焦点的输入字段将滚动到视图中。 自定义控件应模拟此行为。

![显示和没有显示触摸键盘的表单](images/input-patterns/touch-keyboard-pan1.png)

在某些情况下，会有一些 UI 元素应始终保留在屏幕上。 对 UI 进行设计，以便表单控件包含在平移区域中并且重要的 UI 元素时静态的。 例如：

![包含应始终位于视图中的区域的表单](images/input-patterns/touch-keyboard-pan2.png)
## <a name="activation"></a>激活


无论控件当前是否有焦点，均可使用多种不同的方式进行激活。

空格键、Enter 和 Esc  
空格键应当用于激活具有输入焦点的控件。 Enter 键应当用于激活默认控件或具有输入焦点的控件。 默认控件是具有初始焦点的控件，或者是以独占方式响应 Enter 键的控件（通常会随输入焦点发生变化）。 另外，Esc 键应关闭或退出暂时性 UI，例如菜单和对话框。

此处显示的计算器应用使用空格键来激活具有焦点的按钮、将 Enter 键锁定为“=”按钮，并将 Esc 键锁定为“C”按钮。

![计算器应用](images/input-patterns/calculator.png)

键盘修饰符  
键盘修饰符分为以下类别：


| 类别 | 说明 |
|----------|-------------|
| 快捷键 | 不通过 UI 执行常见的操作，例如执行“Ctrl-S”对应于**保存**。 为键盘应用功能实现键盘快捷方式。 并非每个命令都有或需要一个快捷方式。 |   
| 访问键/热键 | 分配给每个可见的顶层控件，如“Alt-F”对应于**“文件”**菜单。 访问键不调用或激活命令。 |
| 加速键 | 执行默认系统或应用定义的命令，例如“Alt-PrtScrn”对应于屏幕捕获、“Alt-Tab”对应于切换应用，或者“F1”对应于帮助。 与加速键相关联的命令不需要是一个菜单项。 |
| 应用程序键/菜单键 | 显示上下文菜单。 |
| 窗口键/命令键 | 激活系统命令，例如**“系统菜单”**、**“锁屏界面”**或**“显示桌面”**。 |

访问键和加速键支持与控件直接交互，而不是使用 Tab 键导航到这些控件。
> 一些控件具有内部标签（例如命令按钮、复选框和单选按钮），而另一些控件则具有外部标签（例如列表视图）。 对于具有外部标签的控件，访问键会分配给此类标签，从而使得控件调用后，将焦点设置到关联控件内的某一元素或值中。


此处的示例显示了适用于 **Word** 中**“页面布局”**选项卡的访问键。

![适用于 Word 中页面布局选项卡的访问键](images/input-patterns/accesskeys-show.png)

在此处，在输入关联标签中标识的访问键后，将突出显示左缩进文本字段值。

![在输入关联标签中标识的访问键后，将突出显示左缩进文本字段值](images/input-patterns/accesskeys-entered.png)

## <a name="usability-and-accessibility"></a>实用功能和辅助功能


设计良好的键盘交互体验是软件辅助功能的一个重要方面。 它使具有视力缺陷或行动有障碍的用户能够在应用中导航并与应用的功能交互。 这些用户可能无法操作鼠标，而必须依靠各种辅助技术，其中包括键盘增强工具和屏幕键盘（以及屏幕放大器、屏幕阅读器和语音输入实用工具）。 对于这些用户，综合性体验比一致性体验更为重要。

有经验的用户通常强烈倾向于使用键盘，因为键盘可让基于键盘的命令更为快速地输入，而无需将双手从键盘上挪开。 对于这些用户，效率性和一致性体验至关重要；综合性体验仅对常用命令十分重要。

当针对实用功能和辅助功能进行设计时，两者区别甚微，这便是同时支持两种不同的键盘访问机制的原因。

访问键具有以下特征：

-   访问键是应用中 UI 元素的快捷方式。
-   它们使用 Alt 键 + 字母数字键。
-   它们主要用于辅助功能。
-   它们被分配给所有菜单以及大多数对话框控件。
-   其设计目的不在于被用户记住，它们通过在相应的控件标签字符下方显示下划线来直接在 UI 中提供相关说明。
-   它们仅在当前窗口中有效，用于导航到对应的菜单项或控件。
-   它们因自身的变动而无法完全一致地进行分配。 但是，对于一些常用命令（特别是提交按钮），访问键应完全一致地进行分配。
-   应对其执行本地化。

因为访问键的设计目的不在于被用户记住，所以为了便于查找而将其分配给标签中早期显示的字符，即便是标签中存在之后显示的关键字也是如此。

相比之下，快捷键具有以下特征：

-   快捷键是应用命令的快捷方式。
-   它们主要是将 Ctrl 与功能按键顺序组合使用（Windows 系统快捷键还将 Alt + 非字母数字键与 Windows 徽标键组合使用）。
-   它们主要用于提高高级用户的工作效率。
-   它们仅分配给最常用的命令。
-   它们的设计目的在于被用户记住，仅在菜单、工具提示和帮助中提供相关说明。
-   它们会影响整个程序，但在它们未应用时将不产生任何影响。
-   它们必须完全一致地进行分配，因为它们要求被用户记住且不直接提供说明。
-   无需对其执行本地化。

由于快捷键的设计目的在于被用户记住，因此最常用的快捷键最好是用字母（从首字母开始）或命令关键字内最易记住的字符来表示，例如用 Ctrl+C 表示复制，用 Ctrl+Q 表示请求。

用户应当能够仅使用硬件键盘或屏幕键盘完成你的应用支持的所有任务。

你必须为依靠屏幕阅读器和其他辅助技术的用户提供一种发现应用快捷键的便捷方式。 可使用工具提示、辅助名称、辅助说明或某种其他形式的屏幕通信，提供快捷键的相关说明。 至少需要在应用的帮助内容中提供有关访问键和快捷键的详细信息。

不要将已知的或标准快捷键分配给其他功能。 例如，Ctrl+F 通常用于查找或搜索。

不要试图将访问键分配给密集型 UI 中的所有交互式控件。 只需确保最重要和最常用的控件具有相关访问键，或使用控件组并将访问键分配给该控件组标签。

不要使用键盘修改器更改命令。 这样做是不可发现的，并且可能会引起混淆。

不要在控件具有输入焦点时禁用它。 这可能会干扰键盘输入。

若要确保键盘交互体验成功实现，使用键盘以独占方式全面测试应用至关重要。

## <a name="text-input"></a>文本输入


当依赖键盘输入时，需始终查询设备功能。 在某些设备（例如手机）上，触摸键盘只能用于文本输入，因为它无法提供硬件键盘上的快捷键或命令键（例如 Alt、功能键或 Windows 徽标键）。

不要让用户使用触摸键盘导航应用。 触摸键盘可能会解除，具体取决于控件获取的焦点。

尝试在与表单的整个交互过程中显示键盘。 这将消除 UI 改动，从而使得在表单或文本输入流的中期时不会对用户造成迷惑。

确保用户在键入时始终可以看到输入字段。 触摸键盘会遮挡住一半的屏幕，因此在用户遍历表单时带焦点的输入字段应滚动到视图中。

标准硬件键盘或 OSK 由七种键构成，每一种键都支持唯一功能：

-   字符键：用于将文本字符发送到具有输入焦点的窗口。
-   修改器键：用于在同时按下主键（例如 Ctrl、Alt、Shift 和 Windows 徽标键）时更改其功能。
-   导航键：用于移动输入焦点或文本输入位置，例如 Tab、Home、End、Page Up、Page Down 和方向型箭头键。
-   编辑键：用于操作文本，例如 Shift、Tab、Enter、Insert、Backspace 和 Delete 键。
-   功能键：用于执行某项特殊功能，例如 F1 至 F12 键。
-   切换键：用于将系统置于某种模式，例如 Caps Lock、ScrLk 和 Num Lock 键。
-   命令键：用于执行某项系统任务或命令激活，例如空格键、Enter、Esc、Pause/Break 和 Print Screen 键。

除上述类别外，还存在二级键和键组合，可用作应用功能的快捷方式：

-   访问键：通过按 Alt 键和字符键来显示控件或菜单项，而通过在菜单中分配的访问键字符的下方显示下划线，或在覆盖区域中显示访问键字符进行指示。
-   快捷键：通过按功能键或 Ctrl 键和字符键来显示应用命令。 你的应用可能拥有与命令相对应的 UI，也可能没有。

应用不能拦截称为安全注意序列 (SAS) 的其他键组合类。 这是一种安全功能，用于在登录过程中保护用户的系统，并包括 Ctrl-Alt-Del 和 Win-L。

此处显示了具有展开式文件菜单的记事本应用，其中同时包含访问键和加速键。

![具有展开式文件菜单的记事本应用，其中同时包含访问键和加速键。](images/input-patterns/notepad.png)

## <a name="keyboard-commands"></a>键盘命令


下面是在各种支持键盘输入的设备间提供的键盘交互的完整列表。 某些设备和平台需要本机按键操作和交互，已在表中加以注明。

在设计自定义控件和交互时，始终使用此键盘语言，可使你的应用令用户感到熟悉、可靠且简单易学。

不要重新定义默认的键盘快捷方式。

下面的表格列出了常用的键盘命令。 有关键盘命令的完整列表，请参阅 [Windows 键盘快捷键](http://go.microsoft.com/fwlink/p/?linkid=325424)。

**导航命令**

| 操作                               | 键命令                                      |
|--------------------------------------|--------------------------------------------------|
| 后退                                 | Alt+向左键或特殊键盘上的后退按钮 |
| 前进                              | Alt+向右键                                        |
| 向上                                   | Alt+向上键                                           |
| 取消或从当前模式中退出   | Esc                                              |
| 遍历列表中的项目         | 箭头键（向左、向右、向上、向下）                |
| 跳转到下一个项目列表           | Ctrl+向左键                                        |
| 语义式缩放                        | Ctrl++ 或 Ctrl+-                                 |
| 跳转到在集合中的命名项目 | 开始键入项目名称                           |
| 下一页                            | Page Up、Page Down 或窗格键                   |
| 下一个选项卡                             | Ctrl+Tab                                         |
| 上一个选项卡                         | Ctrl+Shift+Tab                                   |
| 打开应用栏                         | Windows+Z                                        |
| 激活或导航至一个项目    | 输入                                            |
| 选择                               | 空格键                                         |
| 连续选择                  | Shift+箭头键                                  |
| 全选                           | Ctrl+A                                           |

 

**常用命令**

| 操作                                                 | 键命令     |
|--------------------------------------------------------|-----------------|
| 固定项目                                            | Ctrl+Shift+1    |
| 保存                                                   | Ctrl+S          |
| 查找                                                   | Ctrl+F          |
| 打印                                                  | Ctrl+P          |
| 复制                                                   | Ctrl+C          |
| 剪切                                                    | Ctrl+X          |
| 新建项目                                               | Ctrl+N          |
| 粘贴                                                  | Ctrl+V          |
| 打开                                                   | Ctrl+O          |
| 打开地址（例如，Internet Explorer 中的一个 URL） | Ctrl+L 或 Alt+D |

 

**媒体导航命令**

| 操作       | 键命令 |
|--------------|-------------|
| 播放/暂停   | Ctrl+P      |
| 下一个项目    | Ctrl+F      |
| 上一个项目 | Ctrl+B      |

 

注意：“播放/暂停”和“下一个项目”的媒体导航键命令分别与“打印”和“查找”的键命令相同。 常用命令应优先于媒体导航命令。 例如，如果一个应用同时支持播放媒体和打印，则键命令 Ctrl+P 应为打印。
## <a name="visual-feedback"></a>视觉反馈


仅在进行键盘交互时使用焦点矩形。 如果用户开始使用触摸交互，则使键盘 UI 逐渐淡出。 这会使 UI 干净整洁。

如果元素不支持交互（如静态文本），请勿显示视觉反馈。 同样，这会使 UI 干净整洁。

对于所有代表相同输入目标的元素，尝试同时显示视觉反馈。

尝试提供屏幕按钮（如 + 和 -）作为模拟基于触摸的操作 （如平移、旋转、缩放等）的提示。

有关视觉反馈的更多常规指南，请参阅[视觉反馈指南](guidelines-for-visualfeedback.md)。


## <a name="keyboard-events-and-focus"></a>键盘事件和焦点


以下键盘事件仅针对硬件和触摸键盘发生。

| 事件                                      | 说明                    |
|--------------------------------------------|--------------------------------|
| [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) | 按下某个键时发生。  |
| [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942)     | 释放某个键时发生。 |


**重要提示**  
某些 Windows 运行时控件在内部处理输入事件。 在这些情况下，输入事件好像没有发生，因为你的事件侦听器没有调用相关联的处理程序。 通常，类处理程序将处理这些键的子集以提供对基本键盘辅助功能的内置支持。 例如，[**Button**](https://msdn.microsoft.com/library/windows/apps/br209265) 类将替代空格键和 Enter 键的 [**OnKeyDown**](https://msdn.microsoft.com/library/windows/apps/hh967982) 事件（[**OnPointerPressed**](https://msdn.microsoft.com/library/windows/apps/hh967989) 也是如此），并将其路由到控件的 [**Click**](https://msdn.microsoft.com/library/windows/apps/br227737) 事件。 当控件类处理按键时，不会引发 [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) 和 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) 事件。

这提供了与调用按钮等效的内置键盘，效果与使用手指点击或使用鼠标单击相似。 空格键或 Enter 键之外的键仍会引发 [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) 和 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) 事件。 有关基于类处理事件的工作原理的详细信息（特别是“控件中的输入事件处理程序”部分），请参阅[事件和路由事件概述](https://msdn.microsoft.com/library/windows/apps/mt185584)。


UI 中的控件仅在具有输入焦点时才会生成键盘事件。 当用户直接单击或点击布局中的一个控件时该控件获得焦点，或者使用 Tab 键进入内容区域内的 Tab 序列。

也可以调用控件的 [**Focus**](https://msdn.microsoft.com/library/windows/apps/hh702161) 方法来强制使用焦点。 当实现快捷键时需要此操作，因为默认情况下在 UI 加载时不设置键盘焦点。 有关详细信息，请参阅本主题后面部分的[快捷键示例](#shortcut_keys_example)。

若要使某个控件接收输入焦点，则必须启用该控件，该控件必须可见并且 [**IsTabStop**](https://msdn.microsoft.com/library/windows/apps/br209422) 和 [**HitTestVisible**](https://msdn.microsoft.com/library/windows/apps/br208933) 的属性值必须为 **true**。 这是大多数控件的默认状态。 当某个控件具有输入焦点时，该控件可能引发并响应键盘输入事件，如本主题后面部分所述。 你也可以通过处理 [**GotFocus**](https://msdn.microsoft.com/library/windows/apps/br208927) 和 [**LostFocus**](https://msdn.microsoft.com/library/windows/apps/br208943) 事件来响应接收或丢失焦点的控件。

默认情况下，控件的 Tab 键顺序即为它们在 Extensible Application Markup Language (XAML) 中的显示顺序。 但是，可以使用 [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/br209461) 属性修改此顺序。 有关详细信息，请参阅[实现键盘辅助功能](https://msdn.microsoft.com/library/windows/apps/hh868161)。

## <a name="keyboard-event-handlers"></a>键盘事件处理程序


输入事件处理程序实现提供以下信息的委派：

-   事件的发送者。 发送者报告附加事件处理程序的对象。
-   事件数据。 对于键盘事件，该数据将是 [**KeyRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh943072) 的一个实例。 处理程序的委托为 [**KeyEventHandler**](https://msdn.microsoft.com/library/windows/apps/br227904)。 对于大多数处理程序方案而言，**KeyRoutedEventArgs** 的最为相关的属性是 [**Key**](https://msdn.microsoft.com/library/windows/apps/hh943074)，并且可能为 [**KeyStatus**](https://msdn.microsoft.com/library/windows/apps/hh943075)。
-   [**OriginalSource**](https://msdn.microsoft.com/library/windows/apps/br208810)。 由于键盘事件是路由事件，因此事件数据提供 **OriginalSource**。 如果有意允许事件通过对象树向上浮生，则 **OriginalSource** 有时是所涉及的对象而不是发送者。 但是，这取决于你的设计。 有关如何使用 **OriginalSource** 而不是发送者的详细信息，请参阅本主题中的“键盘路由事件”部分或[事件和路由事件概述](https://msdn.microsoft.com/library/windows/apps/mt185584)。

### <a name="attaching-a-keyboard-event-handler"></a>附加键盘事件处理程序

可以为将事件作为成员包含的任何对象附加键盘事件处理程序功能。 这包括任何 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) 派生类。 以下 XAML 示例显示了如何为 [**Grid**](https://msdn.microsoft.com/library/windows/apps/br208942) 的 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br242704) 事件附加处理程序。

```xaml
<Grid KeyUp="Grid_KeyUp">
  ...
</Grid>
```

也可以在代码中附加事件处理程序。 有关详细信息，请参阅[事件和路由事件概述](https://msdn.microsoft.com/library/windows/apps/mt185584)。

### <a name="defining-a-keyboard-event-handler"></a>定义键盘事件处理程序

以下示例显示了在上一示例中附加的 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) 事件处理程序的不完整事件处理程序定义。

```csharp
void Grid_KeyUp(object sender, KeyRoutedEventArgs e)
{
    //handling code here
}
```

```vb
Private Sub Grid_KeyUp(ByVal sender As Object, ByVal e As KeyRoutedEventArgs)
    ' handling code here
End Sub
```

```c++
void MyProject::MainPage::Grid_KeyUp(
  Platform::Object^ sender,
  Windows::UI::Xaml::Input::KeyRoutedEventArgs^ e)
  {
      //handling code here
  }
```

### <a name="using-keyroutedeventargs"></a>使用 KeyRoutedEventArgs

所有键盘事件对事件数据均使用 [**KeyRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh943072)，而且 **KeyRoutedEventArgs** 包含以下属性：

-   [**Key**](https://msdn.microsoft.com/library/windows/apps/hh943074)
-   [**KeyStatus**](https://msdn.microsoft.com/library/windows/apps/hh943075)
-   [**Handled**](https://msdn.microsoft.com/library/windows/apps/hh943073)
-   [**OriginalSource**](https://msdn.microsoft.com/library/windows/apps/br208810)（继承自 [**RoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br208809)）

### <a name="key"></a>键

如果按下某个键，则引发 [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) 事件。 同样，如果释放某个键，则引发 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942)。 通常会侦听这些事件以处理特定键值。 若要确定按下或释放了哪个键，请检查事件数据中的 [**Key**](https://msdn.microsoft.com/library/windows/apps/hh943074) 值。 **Key** 返回 [**VirtualKey**](https://msdn.microsoft.com/library/windows/apps/br241812) 值。 
            **VirtualKey** 枚举包括所有受支持的键。

### <a name="modifier-keys"></a>修改键

修改键是用户通常与其他键组合而按下的键，如 Ctrl 或 Shift。 你的应用可以使用这些组合作为键盘快捷方式来调用应用命令。

使用 [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) 和 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) 事件处理程序中的代码检测快捷组合键。 然后，你可以跟踪感兴趣的修改键的按下状态。 当非修改键发生键盘事件时，可以同时检查修改键是否处于按下状态。

> [!NOTE]
> Alt 键由 **VirtualKey.Menu** 值表示。

 

### <a name="shortcut-keys-example"></a>快捷键示例


以下示例演示如何实现快捷键。 在此示例中，用户可以使用 Play、Pause 和 Stop 按钮或 Ctrl+P、Ctrl+A 和 Ctrl+S 键盘快捷键来控制媒体播放。 按钮 XAML 通过使用工具提示和按钮标签中的 [**AutomationProperties**](https://msdn.microsoft.com/library/windows/apps/br209081) 属性来显示快捷键。 此自述文档对于增加应用的实用功能和辅助功能非常重要。 有关详细信息，请参阅[键盘辅助功能](https://msdn.microsoft.com/library/windows/apps/mt244347)。

另请注意，加载页面时页面将输入焦点设置为其自身。 如果没有此步骤，则不会有控件获得初始输入焦点，并且在用户手动设置输入焦点之前，应用不会引发输入事件（例如，通过 Tab 浏览或单击某个控件）。

```xaml
<Grid KeyDown="Grid_KeyDown">

  <Grid.RowDefinitions>
    <RowDefinition Height="Auto" />
    <RowDefinition Height="Auto" />
  </Grid.RowDefinitions>

  <MediaElement x:Name="DemoMovie" Source="xbox.wmv"
    Width="500" Height="500" Margin="20" HorizontalAlignment="Center" />

  <StackPanel Grid.Row="1" Margin="10"
    Orientation="Horizontal" HorizontalAlignment="Center">

    <Button x:Name="PlayButton" Click="MediaButton_Click"
      ToolTipService.ToolTip="Shortcut key: Ctrl+P"
      AutomationProperties.AcceleratorKey="Control P">
      <TextBlock>Play</TextBlock>
    </Button>

    <Button x:Name="PauseButton" Click="MediaButton_Click"
      ToolTipService.ToolTip="Shortcut key: Ctrl+A"
      AutomationProperties.AcceleratorKey="Control A">
      <TextBlock>Pause</TextBlock>
    </Button>

    <Button x:Name="StopButton" Click="MediaButton_Click"
      ToolTipService.ToolTip="Shortcut key: Ctrl+S"
      AutomationProperties.AcceleratorKey="Control S">
      <TextBlock>Stop</TextBlock>
    </Button>

  </StackPanel>

</Grid>
```

```c++
//showing implementations but not header definitions
void MainPage::OnNavigatedTo(NavigationEventArgs^ e)
{
    (void) e;    // Unused parameter
    this->Loaded+=ref new RoutedEventHandler(this,&amp;MainPage::ProgrammaticFocus);
}
void MainPage::ProgrammaticFocus(Object^ sender, RoutedEventArgs^ e) {
    this->Focus(Windows::UI::Xaml::FocusState::Programmatic);
}

void KeyboardSupport::MainPage::MediaButton_Click(Platform::Object^ sender, Windows::UI::Xaml::RoutedEventArgs^ e)
{
    FrameworkElement^ fe = safe_cast<FrameworkElement^>(sender);
    if (fe->Name == "PlayButton") {DemoMovie->Play();}
    if (fe->Name == "PauseButton") {DemoMovie->Pause();}
    if (fe->Name == "StopButton") {DemoMovie->Stop();}
}


void KeyboardSupport::MainPage::Grid_KeyDown(Platform::Object^ sender, Windows::UI::Xaml::Input::KeyRoutedEventArgs^ e)
{
    if (e->Key == VirtualKey::Control) isCtrlKeyPressed = true;
}


void KeyboardSupport::MainPage::Grid_KeyUp(Platform::Object^ sender, Windows::UI::Xaml::Input::KeyRoutedEventArgs^ e)
{
    if (e->Key == VirtualKey::Control) isCtrlKeyPressed = false;
    else if (isCtrlKeyPressed) {
        if (e->Key==VirtualKey::P) {
            DemoMovie->Play();
        }
        if (e->Key==VirtualKey::A) {DemoMovie->Pause();}
        if (e->Key==VirtualKey::S) {DemoMovie->Stop();}
    }
}
```

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    // Set the input focus to ensure that keyboard events are raised.
    this.Loaded += delegate { this.Focus(FocusState.Programmatic); };
}

private void MediaButton_Click(object sender, RoutedEventArgs e)
{
    switch ((sender as Button).Name)
    {
        case "PlayButton": DemoMovie.Play(); break;
        case "PauseButton": DemoMovie.Pause(); break;
        case "StopButton": DemoMovie.Stop(); break;
    }
}

private void Grid_KeyUp(object sender, KeyRoutedEventArgs e)
{
    if (e.Key == VirtualKey.Control) isCtrlKeyPressed = false;
}

private void Grid_KeyDown(object sender, KeyRoutedEventArgs e)
{
    if (e.Key == VirtualKey.Control) isCtrlKeyPressed = true;
    else if (isCtrlKeyPressed)
    {
        switch (e.Key)
        {
            case VirtualKey.P: DemoMovie.Play(); break;
            case VirtualKey.A: DemoMovie.Pause(); break;
            case VirtualKey.S: DemoMovie.Stop(); break;
        }
    }
}
```

```VisualBasic
Private isCtrlKeyPressed As Boolean
Protected Overrides Sub OnNavigatedTo(e As Navigation.NavigationEventArgs)

End Sub

Private Sub Grid_KeyUp(sender As Object, e As KeyRoutedEventArgs)
    If e.Key = Windows.System.VirtualKey.Control Then
        isCtrlKeyPressed = False
    End If
End Sub

Private Sub Grid_KeyDown(sender As Object, e As KeyRoutedEventArgs)
    If e.Key = Windows.System.VirtualKey.Control Then isCtrlKeyPressed = True
    If isCtrlKeyPressed Then
        Select Case e.Key
            Case Windows.System.VirtualKey.P
                DemoMovie.Play()
            Case Windows.System.VirtualKey.A
                DemoMovie.Pause()
            Case Windows.System.VirtualKey.S
                DemoMovie.Stop()
        End Select
    End If
End Sub

Private Sub MediaButton_Click(sender As Object, e As RoutedEventArgs)
    Dim fe As FrameworkElement = CType(sender, FrameworkElement)
    Select Case fe.Name
        Case "PlayButton"
            DemoMovie.Play()
        Case "PauseButton"
            DemoMovie.Pause()
        Case "StopButton"
            DemoMovie.Stop()
    End Select
End Sub
```

> [!NOTE]
> 在 XAML 中设置 [**AutomationProperties.AcceleratorKey**](https://msdn.microsoft.com/library/windows/apps/hh759762) 或 [**AutomationProperties.AccessKey**](https://msdn.microsoft.com/library/windows/apps/hh759763) 会提供字符串信息，这可记录用于调用该特定操作的快捷键。 该信息由 Microsoft UI 自动化客户端（如讲述人）捕获，并且通常直接提供给用户。
>
> 设置 **AutomationProperties.AcceleratorKey** 或 **AutomationProperties.AccessKey** 不会自行执行任何操作。 你仍需要附加 [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) 或 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) 事件的处理程序，才能在你的应用中真正实现键盘快捷方式行为。 此外，不会自动为访问键提供带下划线的文本效果。 如果你希望在 UI 中显示带下划线的文本，则必须明确对助记键中特定键的文本标注下划线，作为嵌入式 [**Underline**](https://msdn.microsoft.com/library/windows/apps/br209982) 格式。

 

## <a name="keyboard-routed-events"></a>键盘路由事件


有些事件为路由事件，这些事件包括 [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) 和 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942)。 路由事件使用浮升路由策略。 浮升路由策略意味着某个事件从子对象开始，然后向上路由到对象树中的连续父对象。 这样便提供了处理相同事件以及与相同数据数据交互的另一个机会。

请考虑以下 XAML 示例，该示例处理一个 [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br208942) 和两个 [**Button**](https://msdn.microsoft.com/library/windows/apps/br209267) 对象的 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br209265) 事件。 在这种情况下，如果在任一 **Button** 对象具有焦点的同时释放键，则会引发 **KeyUp** 事件。 然后，该事件向上浮升到父 **Canvas**。

```xaml
<StackPanel KeyUp="StackPanel_KeyUp">
  <Button Name="ButtonA" Content="Button A"/>
  <Button Name="ButtonB" Content="Button B"/>
  <TextBlock Name="statusTextBlock"/>
</StackPanel>
```

以下示例显示了如何为前面示例中对应的 XAML 内容实现 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) 事件处理程序。

```csharp
void StackPanel_KeyUp(object sender, KeyRoutedEventArgs e)
{
    statusTextBlock.Text = String.Format(
        "The key {0} was pressed while focus was on {1}",
        e.Key.ToString(), (e.OriginalSource as FrameworkElement).Name);
}
```

请注意前面的处理程序中的 [**OriginalSource**](https://msdn.microsoft.com/library/windows/apps/br208810) 属性的用法。 在此处，**OriginalSource** 报告引发此事件的对象。 该对象不可能是 [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/br209635)，因为 **StackPanel** 不是控件，无法具有焦点。 只有 **StackPanel** 中的两个按钮之一可能引发此事件，但是是哪一个按钮呢？ 如果在父对象上处理此事件，可使用 **OriginalSource** 来辨别实际的事件源对象。

### <a name="the-handled-property-in-event-data"></a>事件数据中的 Handled 属性

根据你的事件处理策略，你可能希望只有一个事件处理程序对浮升事件进行响应。 例如，如果已将特定的 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) 处理程序附加到其中一个 [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265) 控件，则这是处理该事件的第一次机会。 在这种情况下，你可能不希望父面板也处理该事件。 对于此方案，可以使用事件数据中的 [**Handled**](https://msdn.microsoft.com/library/windows/apps/hh943073) 属性。

路由事件数据类中的 [**Handled**](https://msdn.microsoft.com/library/windows/apps/hh943073) 属性用于报告之前在事件路由上注册的另一个处理程序已进行操作。 这会影响路由事件系统的行为。 在事件处理程序中将 **Handled** 设置为 **true** 时，该事件停止路由，但不会发送到连续的父元素。

### <a name="addhandler-and-already-handled-keyboard-events"></a>AddHandler 和 already-handled 键盘事件

可以使用特殊技术来附加处理程序，该技术对已标记为已处理的事件进行操作。 此技术使用 [**AddHandler**](https://msdn.microsoft.com/library/windows/apps/hh702399) 方法注册处理程序，而不是使用 XAML 属性或特定于语言的用于添加处理程序的语法，如 C# 中的“+=”。 

此技术的一般限制是 **AddHandler** API 带有一个类型为 [**RoutedEvent**](https://msdn.microsoft.com/library/windows/apps/br208808) 的参数，该参数标识相关的路由事件。 并非所有路由事件都提供 **RoutedEvent** 标识符，因此此注意事项会影响在 [**Handled**](https://msdn.microsoft.com/library/windows/apps/hh943073) 情况下仍然可以处理的路由事件。 
            [
              **KeyDown**
            ](https://msdn.microsoft.com/library/windows/apps/br208941) 和 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) 事件在 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/hh702416) 上具有路由事件标识符（[**KeyDownEvent**](https://msdn.microsoft.com/library/windows/apps/hh702418) 和 [**KeyUpEvent**](https://msdn.microsoft.com/library/windows/apps/br208911)）。 但是，其他事件（如 [**TextBox.TextChanged**](https://msdn.microsoft.com/library/windows/apps/br209706)）没有路由事件标识符，因此不能使用 **AddHandler** 技术。

### <a name="overriding-keyboard-events-and-behavior"></a>替代键盘事件和行为

你可以替代特定控件（如 [**GridView**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.GridView)）的键事件，以针对各种输入设备（包括键盘和游戏板）提供一致的焦点导航。

在以下示例中，我们将该控件划入子类并替代 KeyDown 行为，以在按下任意箭头键时将焦点移动到 GridView 内容。

```csharp
public class CustomGridView : GridView
  {
    protected override void OnKeyDown(KeyRoutedEventArgs e)
    {
      // Override arrow key behaviors.
      if (e.Key != Windows.System.VirtualKey.Left && e.Key !=
        Windows.System.VirtualKey.Right && e.Key != 
          Windows.System.VirtualKey.Down && e.Key != 
            Windows.System.VirtualKey.Up)
              base.OnKeyDown(e);
      else
        FocusManager.TryMoveFocus(FocusNavigationDirection.Down);
    }
  }
```

> [!NOTE]
> 如果仅将 GridView 用于布局，请考虑使用其他控件（如 [**ItemsControl**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.ItemsControl) 与 [**ItemsWrapGrid**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.ItemsWrapGrid)）。

## <a name="commanding"></a>命令

少量 UI 元素提供对命令的内置支持。 命令在其基础实现中使用与输入相关的路由事件。 它能够通过调用一个命令处理程序来处理相关的 UI 输入，如某个指针操作或特定加速键。

如果命令处理可用于 UI 元素，可以考虑使用它的命令处理 API 代替任何具体的输入事件。 有关详细信息，请参阅 [**ButtonBase.Command**](https://msdn.microsoft.com/library/windows/apps/br227740)。

还可以实现 [**ICommand**](https://msdn.microsoft.com/library/windows/apps/br227885) 来封装从普通事件处理程序中调用的命令功能。 这使你甚至能够在没有任何可用 **Command** 属性的情况下使用命令。

## <a name="text-input-and-controls"></a>文本输入和控件

某些控件通过自身的处理来对键盘事件作出响应。 例如，[**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) 是一个控件，设计用于捕获然后在视觉上显示使用键盘输入的文本。 它以自己的逻辑使用 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) 和 [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) 来捕获击键，然后，即使在文本实际上已更改的情况下，也还是会引发自己的 [**TextChanged**](https://msdn.microsoft.com/library/windows/apps/br209706) 事件。

通常，仍然可以将 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) 和 [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) 的处理程序添加到 [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) 或计划处理文本输入的任何相关控件中。 但是，作为其预期设计，一个控件可能并不会对通过键事件指向它的所有键值作出响应。 行为特定于每个控件。

例如，[**ButtonBase**](https://msdn.microsoft.com/library/windows/apps/br227736)（[**Button**](https://msdn.microsoft.com/library/windows/apps/br209265) 的基类）处理 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942)，以便可以检查空格键或 Enter 键。 **ButtonBase** 认为 **KeyUp** 等同于按下鼠标左键以引发 [**Click**](https://msdn.microsoft.com/library/windows/apps/br227737) 事件。 在 **ButtonBase** 覆盖虚拟方法 [**OnKeyUp**](https://msdn.microsoft.com/library/windows/apps/hh967983) 时完成此事件处理操作。 在其实现过程中，会将 [**Handled**](https://msdn.microsoft.com/library/windows/apps/hh943073) 设置为 **true**。 如果空格键未接收到自己处理程序的 already-handled 事件，则结果为某个按钮的任意父按钮侦听键事件。

另一个示例是 [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683)。 
            **TextBox** 不会将某些键（如箭头键）视为文本，而是视为特定于控件 UI 的行为。 
            **TextBox** 将这些事件案例标记为已处理。

自定义控件可以通过重写 [**OnKeyDown**](https://msdn.microsoft.com/library/windows/apps/hh967982) / [**OnKeyUp**](https://msdn.microsoft.com/library/windows/apps/hh967983) 来为键事件实现自己的类似重写行为。 如果你的自定义控件处理特定加速键或者具有类似于为 [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) 描述的方案的控件或焦点行为，则应该将该逻辑放置在自己的 **OnKeyDown** / **OnKeyUp** 重写中。

## <a name="the-touch-keyboard"></a>触摸键盘

文本输入控件提供对触摸键盘的自动支持。 当用户将输入焦点设置为使用触摸输入的文本控件时，触摸键盘会自动出现。 当输入焦点不在文本控件上时，隐藏触摸键盘。

当出现触摸键盘时，会自动重新定位你的 UI 以确保可以看见具有焦点的元素。 这可能会导致 UI 的其他重要区域移出屏幕。 但是，你可以禁用默认行为并在出现触摸键盘时对自己的 UI 进行调整。 有关详细信息，请参阅[响应出现屏幕键盘的示例](http://go.microsoft.com/fwlink/p/?linkid=231633)。

如果创建一个需文本输入但并非从标准文本输入控件派生的自定义控件，则可以通过实现正确的 UI 自动化控件模式来添加触摸键盘支持。 有关详细信息，请参阅[响应触摸键盘的存在](respond-to-the-presence-of-the-touch-keyboard.md)和[触摸键盘示例](http://go.microsoft.com/fwlink/p/?linkid=246019)。

按下触摸键盘上的键引发 [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) 和 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) 事件，就像在硬件键盘上按下键一样。 但是，触摸键盘不会引发 Ctrl+A、Ctrl+Z、Ctrl+X、Ctrl+C 和 Ctrl+V 的输入事件，这些是输入控件中保留的文本操作。

通过将文本控件的输入范围设置为匹配你期望用户输入的数据类型，可以让用户在应用中更快捷地输入数据。 输入范围会针对控件所预期的文本输入类型提供提示，以便系统可以为该输入类型提供专用的触摸键盘布局。 例如，如果文本框中仅用于输入一个 4 位数的 PIN，请将 [**InputScope**](https://msdn.microsoft.com/library/windows/apps/hh702632) 属性设置为 [**Number**](https://msdn.microsoft.com/library/windows/apps/hh702028)。 这将通知系统显示数字键盘布局，以便于用户输入 PIN。 有关详细信息，请参阅[使用输入范围更改触摸键盘](https://msdn.microsoft.com/library/windows/apps/mt280229)。


## <a name="additional-articles-in-this-section"></a>本部分中的其他文章

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">主题</th>
<th align="left">说明</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>[响应触摸键盘的存在](respond-to-the-presence-of-the-touch-keyboard.md)</p></td>
<td align="left"><p>了解如何在显示或隐藏触摸键盘时定制你的应用 UI。</p></td>
</tr>
</tbody>
</table>

## <a name="related-articles"></a>相关文章

**开发人员**
* [标识输入设备](identify-input-devices.md)
* [响应触摸键盘的存在](respond-to-the-presence-of-the-touch-keyboard.md)

**设计人员**
* [键盘设计指南](https://msdn.microsoft.com/library/windows/apps/hh972345)

**示例**
* [基本输入示例](http://go.microsoft.com/fwlink/p/?LinkID=620302)
* [低延迟输入示例](http://go.microsoft.com/fwlink/p/?LinkID=620304)
* [焦点视觉示例](http://go.microsoft.com/fwlink/p/?LinkID=619895)

**存档示例**
* [输入示例](http://go.microsoft.com/fwlink/p/?linkid=226855)
* [输入：设备功能示例](http://go.microsoft.com/fwlink/p/?linkid=231530)
* [输入：触摸键盘示例](http://go.microsoft.com/fwlink/p/?linkid=246019)
* [响应屏幕键盘外观示例](http://go.microsoft.com/fwlink/p/?linkid=231633)
* [XAML 文本编辑示例](http://go.microsoft.com/fwlink/p/?LinkID=251417)
 

 

