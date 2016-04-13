---
Description: 当检测、解释和处理用户与 Windows 应用商店应用的交互时，使用视觉反馈显示给用户。
title: 视觉反馈
ms.assetid: bf2f3672-95f0-4c8c-9a72-0934f2d3b767
label: Visual feedback
template: detail.hbs
---

# 视觉反馈指南


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


当检测、解释和处理用户的交互时，可使用视觉反馈显示给用户。 视觉反馈可通过鼓励交互来帮助用户。 它将指示交互是否成功，以加强用户的控制感觉。 它还可以传送系统状态并减少错误。

**重要的 API**

-   [**Windows.Devices.Input**](https://msdn.microsoft.com/library/windows/apps/br225648)
-   [**Windows.UI.Input**](https://msdn.microsoft.com/library/windows/apps/br242084)
-   [**Windows.UI.Core**](https://msdn.microsoft.com/library/windows/apps/br208383)


## <span id="Dos_and_don_ts"> </span> <span id="dos_and_don_ts"> </span> <span id="DOS_AND_DON_TS"> </span>注意事项


-   无论接触多么简单，都应该提供视觉反馈。 这可帮助用户：
    -   确认触摸屏幕正在工作。
    -   确定目标是否支持触摸或是否有响应。
    -   确定用户是否错过了其预期目标。
-   针对所有交互事件立即显示反馈。
-   提供由不会干扰用户的巧妙而又直观的线索组成的反馈。
-   确保触摸目标在所有的操作期间都附着在指尖上。
-   当平移被限制为一个方向时，启用通过轻扫手势来选择项。
-   请勿使用可能会干扰对应用的使用的触摸视觉化。 有关详细信息，请参阅 [**ShowGestureFeedback**](https://msdn.microsoft.com/library/windows/apps/br241969)。
-   除非绝对有必要，否则不要显示反馈。 除非你要添加的值在其他任何地方都不可用，否则，请通过不显示视觉反馈来使 UI 干净整洁。 一定不要显示与已经可见的文本重复的工具提示。 应当为特殊情况（例如，在选择项目时未显示截断文本（带有省略号的文本），或者为了了解或使用你的应用而需要其他信息）保留工具提示。
-   对于信息 UI 以外的所有内容，请勿使用长按手势。
    **重要提示** 在同时启用了水平和垂直平移的情况下，可以通过长按来进行选择。

     

-   请勿自定义内置 Windows 8 手势的视觉反馈行为，因为这样会产生不一致的情况，并且会带来混淆的用户体验。
-   在平移或拖动期间不显示视觉反馈；对象在屏幕上实际移动就足够了。 但是，如果内容区域未平移或滚动，则使用触摸可视化来指示边界条件。 有关详细信息，请参阅[平移指南](guidelines-for-panning.md)。
-   对于未标识为目标的控件不显示反馈。 当依赖触摸输入来获取要求准确基于位置的活动时，视觉反馈非常重要。 每次检测到触摸输入时都显示反馈将有助于用户了解应用及其控件定义的任何自定义目标启发。
-   请勿将适用于某一种输入类型的反馈行为用于其他输入类型。 例如，键盘焦点矩形仅应该用于键盘输入，而不能用于触摸。

## <span id="Additional_usage_guidance"> </span> <span id="additional_usage_guidance"> </span> <span id="ADDITIONAL_USAGE_GUIDANCE"> </span>其他用法指南


对于要求准确性和精度的触摸交互来说，触摸可视化非常重要。 例如，你的应用应当清楚地指示点击位置，以便让用户知道他们是否错过了其目标、他们对其目标错过了多长时间、他们必须进行哪些调整。

使用通过 Windows 应用商店应用语言框架（使用 JavaScript 的 Windows 应用商店应用和使用 C++、C\# 或 Visual Basic 的 Windows 应用商店应用）公开的平台控件免费获取 Windows 8 可视化。 如果你的应用具有需要自定义反馈的自定义交互，你应该确保反馈合适、支持各种输入设备并且不会干扰用户执行其任务。 在视觉反馈可能会与关键 UI 冲突或者甚至会遮盖关键 UI 的游戏或绘图应用中，这可能会是特殊问题。

**重要提示**  
我们不建议更改内置手势的交互行为。

 

**反馈 UI**

反馈 UI 通常依赖输入设备（触摸、触摸板、鼠标、笔/触笔、键盘等）。 例如，鼠标的内置反馈通常涉及到移动和更改光标，而触摸和笔需要接触可视化，键盘输入和导航使用焦点矩形和突出显示。

使用 [**ShowGestureFeedback**](https://msdn.microsoft.com/library/windows/apps/br241969) 设置平台手势的反馈行为。

如果要自定义反馈 UI，请确保你提供支持而且适合所有输入模式的反馈。

下面是 Windows 8 中内置的接触可视化的示例。

<table>
<colgroup>
<col width="25%" />
<col width="25%" />
<col width="25%" />
<col width="25%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">
<img src="images/feedback-touch-cursor.png" alt="Screenshot showing a touch visualization." />
<p>触摸可视化</p></td>
<td align="left"><img src="images/feedback-mouse-cursor2.png" alt="Screenshot showing a mouse visualization." />
<p>鼠标/触摸板可视化</p></td>
<td align="left"><img src="images/feedback-pen-cursor3.png" alt="Screenshot showing a pen visualization." />
<p>笔可视化</p></td>
<td align="left"><img src="images/feedback-keyboard-cursor.png" alt="Screenshot showing a keyboard visualization." />
<p>键盘可视化</p></td>
</tr>
</tbody>
</table>

 

### <span id="informationalui"></span><span id="INFORMATIONALUI"></span>

**信息 UI（弹出窗口）**

视觉反馈的一个主要形式是信息 UI（或消除歧义 UI）。 信息 UI 标识和显示有关对象的信息、描述功能及其访问方式，并在必要时提供指导。

以下是 Windows 应用商店应用支持的不同类型的信息 UI。

-   工具提示
-   丰富工具提示
-   菜单
-   消息对话框
-   弹出窗口

信息 UI 对于克服指尖封闭（障碍）和改进与应用的触摸交互尤其有用。 它甚至还具有专门执行此操作的内置手势：长按。

长按是一种定时交互，通常不建议在 Windows 8 中使用。 这种情况下，可以接受定时交互，因为它充当了解和探究的工具。 建议的时间长度取决于信息 UI 的类型。 下面是建议的时间阈值。

信息 UI 类型
定时
激活
用法
封闭工具提示（用于描述推移和小目标）
0 ms
是
用于快速解释操作。 通常用于命令。
封闭工具提示（用于操作）
200 ms
是
丰富工具提示
~2000 ms
否
用于速度较慢、准备更充分的探究和了解。 通常用于集合项。
自我显示交互
~2000 ms
否
上下文菜单
~2000 ms
否
显示与所选对象相关的有限命令集合。
弹出窗口
~2000 ms
否
显示与所选对象相关的有限命令集合。
 

关于提供信息 UI 的详细信息，请参阅[为你的 UI 布局](https://msdn.microsoft.com/library/windows/apps/hh465304)和[显示弹出窗口](https://msdn.microsoft.com/library/windows/apps/hh738362)。

**工具提示**

要求用户执行操作之前，使用工具提示显示有关控件的详细信息。

当用户在控件或对象上执行长按手势（或检测到悬停事件）时，将自动显示工具提示 ([**Tooltip**](https://msdn.microsoft.com/library/windows/apps/br229763))。 当接触或光标移动到控件或对象之外时，工具提示消失。 工具提示可以包括文本和图像，但不可交互。

**用于小目标的封闭工具提示**

封闭工具提示应该描述封闭的目标。 当确定目标并激活小于标准触摸目标大小的项（如网页上的超链接）时，这些工具提示非常有用。

超过某个特定的时间阈值之后，你可以将这些工具提示替换为信息弹出窗口。 例如，使用封闭工具提示来显示超链接的封闭文本，然后将该工具提示替换为包含此 URL 的弹出窗口。

**操作和命令的封闭工具提示**

这些工具提示描述在用户将其手指从元素抬起时所发生的操作或出现的命令。 当以某个按钮或相似的控件为目标并激活它时，这些工具提示很有用。

超过某个特定的时间阈值后，小目标工具提示会跟随在操作工具提示之后。 这种情况下，小目标工具提示应该展开以包含操作工具提示中的附加信息。

**丰富工具提示**

这些工具提示显示有关某个元素的辅助信息。 例如，丰富工具提示可以是图像的文本描述、被截断的标题的全部文本或与目标相关的其他信息。

丰富工具提示通常包含不需要立即可用的信息，在某些情况下，如果显示太快，可能会分散注意力。 较长的时间阈值允许用户为获取信息做好更充分的准备。

显示丰富工具提示之后，当用户抬起手指时该对象不再激活。 这就是从工具提示收集的信息可能会影响用户不激活项的原因。

我们建议视觉设计以及丰富工具提示中的信息更独特，并且比标准工具提示的信息更多。

**上下文菜单**

上下文菜单 ([**PopupMenu**](https://msdn.microsoft.com/library/windows/apps/br208693)) 是轻型菜单，可让用户立即访问 Windows 应用商店应用中文本或 UI 对象上的操作（如剪贴板命令）。

触摸优化的上下文菜单由两个部分组成。 视觉提示（即提示）作为长按交互的结果进行显示。 然后，上下文菜单本身在提示消失并在用户抬起手指之后显示。

下图演示了如何通过在所选内容内或控制手柄上点击（也可以使用长按）来调用上下文菜单。

![在所选内容内或在控制手柄上点击（或长按）可调用上下文菜单。](images/textselection-show-context.png)

请参阅[添加上下文菜单](https://msdn.microsoft.com/library/windows/apps/hh465300)。

**消息对话框**

在继续操作之前，基于用户操作或应用状态，使用消息对话框 ([**MessageDialog**](https://msdn.microsoft.com/library/windows/apps/br208674)) 来提示用户响应。 需要显式的用户交互，并且阻止向应用进行输入，直到用户做出响应为止。

![用于错误消息的消息对话框](images/messagedialog.png)

以下是一些显示消息对话框的典型原因。

-   显示紧急信息
-   在继续执行操作前提问
-   显示错误消息

请参阅[添加消息对话框](https://msdn.microsoft.com/library/windows/apps/hh738361)。

**弹出菜单**

弹出菜单 ([**Flyout**](https://msdn.microsoft.com/library/windows/apps/br211726)) 是在点击、单击或其他激活时显示的一种轻型 UI 面板，用于为用户提供信息、问题或与当前活动相关的选项菜单。 它可以进行轻型取消（当用户触摸或单击弹出面板外，或按 ESC 键时，它将消失）。 换而言之，可以忽略弹出窗口。

不同于工具提示，弹出窗口可以接受输入。 不同于消息对话框，应用仍可用并接受输入。

![包含确认的弹出窗口](images/flyout.png)

请参阅[添加弹出窗口和菜单](https://msdn.microsoft.com/library/windows/apps/hh465325)。

### <span id="selfreveal"></span><span id="SELFREVEAL"></span>

**自我显示 UI**

自我显示交互是一种信息视觉提示或动画，演示如何使用目标对象执行操作并提供该操作结果的预览。

下图显示了“开始”屏幕上横向滑动选择的自我显示交互。 当用户触摸应用磁贴（不拖动磁贴）时，该磁贴向下滑动（就像被拖动一样）以显示该选择选中标记，实际选中该应用时，该标记会显示。

![显示已取消选择状态的屏幕截图。](images/crossslide-selfreveal1.png)

*在某个项目上向下按手指可启动所选内容的自我显示交互。 自我显示交互演示了将针对该项目执行的操作。*

![显示可供选择的动画的屏幕截图。](images/crossslide-selfreveal2.png)

*如果不抬起手指，轻扫可实际选择该项目。*

![显示可供拖放的动画的屏幕截图。](images/crossslide-selfreveal3.png)

*如果用户继续滑动其手指，自我显示视觉化会更改为显示该对象现在可以移动。*

显示自我显示交互之后，当用户抬起手指时该对象不再激活。

## <span id="related_topics"> </span>相关文章


**对于设计人员**
* [平移指南](guidelines-for-panning.md)
**对于开发人员**
* [自定义用户交互](https://msdn.microsoft.com/library/windows/apps/mt185599)
**示例**
* [基本输入示例](http://go.microsoft.com/fwlink/p/?LinkID=620302)
* [低延迟输入示例](http://go.microsoft.com/fwlink/p/?LinkID=620304)
* [用户交互模式示例](http://go.microsoft.com/fwlink/p/?LinkID=619894)
* [焦点视觉示例](http://go.microsoft.com/fwlink/p/?LinkID=619895)
**存档示例**
* [输入：XAML 用户输入事件示例](http://go.microsoft.com/fwlink/p/?linkid=226855)
* [输入：设备功能示例](http://go.microsoft.com/fwlink/p/?linkid=231530)
* [输入：触摸点击测试示例](http://go.microsoft.com/fwlink/p/?linkid=231590)
* [XAML 滚动、平移以及缩放示例](http://go.microsoft.com/fwlink/p/?linkid=251717)
* [输入：简化的墨迹示例](http://go.microsoft.com/fwlink/p/?linkid=246570)
* [输入：Windows 8 手势示例](http://go.microsoft.com/fwlink/p/?LinkId=264995)
* [输入：操作和手势 (C++) 示例](http://go.microsoft.com/fwlink/p/?linkid=231605)
* [DirectX 触控输入示例](http://go.microsoft.com/fwlink/p/?LinkID=231627)
 

 






<!--HONumber=Mar16_HO4-->


