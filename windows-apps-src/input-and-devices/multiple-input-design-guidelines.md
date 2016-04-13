---
正如人们在彼此交流时会结合使用语音和手势，在与应用交互时也会用到多种类型和模式的输入。
多个输入设计指南
ms.assetid: 03EB5388-080F-467C-B272-C92BE00F2C69
多个输入
template: detail.hbs
---

# 多个输入


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


正如人们在彼此交流时会结合使用语音和手势，在与应用交互时也会用到多种类型和模式的输入。


若要尽可能容纳更多的用户和设备，我们建议你将应用设计为可与尽可能多的输入类型（手势、语音、触摸、触摸板、鼠标和键盘）结合使用。 这样做将使灵活性、可用性和辅助功能最大化。

若要开始操作，请考虑应用会在其中处理输入的各种应用场景。 尝试在你的应用中保持一致，并记住这些平台控件提供了对多个输入类型的内置支持。

-   用户是否能通过多个输入设备与应用程序交互？
-   所有输入方式都一直受支持吗？ 需使用某些控件？ 需在特定时间或环境下使用？
-   是否有输入法优先？

## <span id="Single__or_exclusive_-mode_interactions_"> </span> <span id="single__or_exclusive_-mode_interactions_"> </span> <span id="SINGLE__OR_EXCLUSIVE_-MODE_INTERACTIONS_"> </span>单一（或独占）模式交互


借助单一模式交互，可支持多种输入类型，但每个操作只能使用一种。 例如，将语音识别用于命令，而将手势用于导航；或者将触摸或手势用于文本输入，具体取决于邻近感应。

## <span id="Multimodal_interactions"> </span> <span id="multimodal_interactions"> </span> <span id="MULTIMODAL_INTERACTIONS"> </span>多模式交互


借助多模式交互，多个输入法将依次用于完成某一操作。

<span id="Speech___gesture"> </span> <span id="speech___gesture"> </span> <span id="SPEECH___GESTURE"> </span>语音 + 手势  
用户指向某一产品，然后说出“添加到购物车”。

<span id="Speech___touch"> </span> <span id="speech___touch"> </span> <span id="SPEECH___TOUCH"> </span>语音 + 触摸  
用户通过长按选择照片，然后说出“发送照片”。





<!--HONumber=Mar16_HO1-->


