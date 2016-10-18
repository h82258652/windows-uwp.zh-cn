---
description: "针对特定类型的输入和设备自定义 UWP 应用。 充分利用触摸和语音命令。 在 Xbox、手机甚至是电视上运行你的应用。"
title: "UWP 应用输入和设备设计 – Windows 应用开发"
author: mijacobs
keywords: "设备基础版, 应用输入, 自定义 UWP 应用程序"
translationtype: Human Translation
ms.sourcegitcommit: 5a6666d4e706d4d49d646b5bb2e43b82394eb215
ms.openlocfilehash: 85bcd15d4b9262188f0821642faf0d3d0cb7dbad

---
# 输入和设备

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

UWP 应用可自动处理各种各样的输入并在各种设备上运行，例如，无需执行任何额外操作即可支持触摸输入或让你的应用在手机上运行。 

但是，有时你可能希望为特定类型的输入或设备优化应用。 例如，如果你要创建绘画应用，可能需要自定义处理笔输入的方式。 

本部分中的设计和编码说明可帮助你针对特定类型的输入和设备自定义 UWP 应用。 

## 输入和交互

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[输入基础版](input-primer.md)</b><br/> 当与特定的外形规格配对时，自行熟悉每个输入设备类型及其行为、功能和限制。   
</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Cortana](cortana-interactions.md) </b><br/> 通过语音命令扩展 Cortana 的基本功能，这些命令用于在外部应用程序中启动并执行一个单独操作。   
</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<b>[游戏板和遥控器](gamepad-and-remote-interactions.md)</b><br/>UWP 应用现在支持游戏板和遥控器输入。 游戏板和遥控器是 Xbox 和电视体验的主要输入设备。  
  </div>
  <div class="side-by-side-content-right">
<b>[键盘](keyboard-interactions.md)</b><br/>键盘输入是应用的整体用户交互体验的一个重要部分。 对于残疾人士，或者只是认为键盘是与应用交互的最有效方法的用户而言，键盘非常重要。  
  </div>
</div>
</div>
<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[鼠标](mouse-interactions.md)</b><br/>鼠标输入最适合那些需要精确指向和单击目标的用户交互。 由于 Windows 的 UI 针对触摸的不精确特性进行了优化，因此它自然支持这种固有的精确度。
</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[笔](pen-and-stylus-interactions.md)</b><br/>针对笔输入优化 UWP 应用，以便为用户同时提供标准的指针设备功能和最佳的 Windows 墨迹体验。   
</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[语音](speech-interactions.md)</b><br/>将语音识别和文本到语音转换（又称 TTS 或语音合成）直接集成到你的应用的用户体验中。
</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[触摸](touch-interactions.md)</b><br/>UWP 具有多种处理触摸输入的不同机制，使你能够创建用户能够放心浏览的沉浸式体验。
</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[触摸板](touchpad-interactions.md)  </b><br/>设计你的应用，以便用户可以通过触摸板与其交互。 触摸板将间接式多点触控输入和定位设备（如鼠标）的精确输入结合起来。 这种结合使触摸板既适用于触摸优化的 UI，也适用于效率应用的较小目标。
</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[多个输入](multiple-input-design-guidelines.md)  </b><br/>若要尽可能容纳更多的用户和设备，我们建议你将应用设计为可与尽可能多的输入类型（手势、语音、触摸、触摸板、鼠标和键盘）结合使用。 这样做将最大程度实现灵活性、可用性和辅助功能。
</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[视觉缩放和调整大小](guidelines-for-optical-zoom.md)</b><br/>本文介绍 Windows 缩放和调整大小元素，并提供在你的应用中使用这些交互机制时的用户体验指南。
</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[平移](guidelines-for-panning.md)</b><br/>平移或滚动允许用户在单个视图中导航，以显示在视口内放不下的视图内容。  
</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[旋转](guidelines-for-rotation.md)</b><br/> 本文介绍用于旋转的新 Windows UI，并提供在 UWP 应用中使用这个新交互机制时应该考虑的用户体验指南。
</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[选择文本和图像](guidelines-for-textselection.md)</b><br/>本文介绍了选择和操作文本、图像和控件，并提供了将这些机制用于应用中时应考虑的用户体验指南。
</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[定向](guidelines-for-targeting.md)</b><br/>在 Windows 中确定触摸目标需使用触控数字化器检测到的每个手指的全部接触区域。 确定用户的预期（或最可能）目标时，数字化器报告的输入数据集越大、越复杂，精度越高。
</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[视觉反馈](guidelines-for-visualfeedback.md)</b><br/>当检测、解释和处理用户的交互时，可使用视觉反馈显示给用户。 视觉反馈可通过鼓励交互来帮助用户。 它将指示交互是否成功，以加强用户的控制感觉。 它还可以传送系统状态并减少错误。  
</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[标识输入设备](identify-input-devices.md)</b><br/>标识连接到通用 Windows 平台 (UWP) 设备的输入设备，并标识其功能和属性。 
</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[自定义文本输入](custom-text-input.md)</b><br/>Windows.UI.Text.Core 命名空间中的核心文本 API 支持 UWP 应用通过 Windows 设备上受支持的任何文本服务接收文本输入。 这使应用接收的文本可以是任何语言以及来自任何输入类型，例如键盘、语音或笔。
</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[处理指针输入](handle-pointer-input.md)</b><br/>在通用 Windows 平台 (UWP) 应用中接收、处理和管理来自指针设备的输入数据，例如触摸、鼠标、笔/触笔和触摸板。
</p>
  </div>
  <div class="side-by-side-content-right">
<p><b></b><br/>   
</p>
  </div>
</div>
</div>


## 设备

了解支持 UWP 应用的设备将帮助你提供每个外形规格的最佳用户体验。 针对特定设备进行设计时，主要注意事项包括应用将如何显示在该设备上，在该设备上使用应用的位置、时间和方式，以及用户将如何与该设备交互。

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[设备基础版](device-primer.md)</b><br/>了解支持 UWP 应用的设备将帮助你提供每个外形规格的最佳用户体验。 
</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[针对 Xbox 和电视进行设计](designing-for-tv.md)</b><br/>设计你的通用 Windows 平台 (UWP) 应用，以便它在 Xbox One 和电视屏幕上外观良好且运行正常。
</p>
  </div>
</div>
</div>




<!--HONumber=Aug16_HO5-->


