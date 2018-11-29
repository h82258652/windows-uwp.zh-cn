---
ms.assetid: 831123A7-1F40-4B74-AE9F-69AC9883B4AD
description: 本文介绍如何使用手动设备控件实现增强的照片和视频捕获方案，包括光学图像防抖动和平滑缩放。
title: 用于照片和视频捕获的手动相机控件
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 303cbd5e87db773324cd98447df6d99dc6de5a0c
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/28/2018
ms.locfileid: "7975698"
---
# <a name="manual-camera-controls-for-photo-and-video-capture"></a>用于照片和视频捕获的手动相机控件



本文介绍如何使用手动设备控件实现增强的照片和视频捕获方案，包括光学图像防抖动和平滑缩放。

本文中讨论的控件全部使用相同模式添加到你的应用中。 首先，检查运行你的应用的当前设备是否支持该控件。 如果控件受支持，则为控件设置所需模式。 通常，如果特定控件在当前设备上不受支持，你应禁用或隐藏允许用户启用该功能的 UI 元素。

本文中的代码改编自[相机手动控件 SDK 示例](https://go.microsoft.com/fwlink/?linkid=845228)。 你可以下载该示例以查看上下文中使用的代码，或将该示例用作你自己的应用的起始点。

> [!NOTE]
> 本文以[使用 MediaCapture 捕获基本的照片、视频和音频](basic-photo-video-and-audio-capture-with-MediaCapture.md)中讨论的概念和代码为基础，该文章介绍了实现基本照片和视频捕获的步骤。 我们建议你先熟悉该文中的基本媒体捕获模式，然后再转到更高级的捕获方案。 本文中的代码假设你的应用已有一个正确完成初始化的 MediaCapture 的实例。

本文中讨论的所有设备控件 API 都是 [**Windows.Media.Devices**](https://msdn.microsoft.com/library/windows/apps/br206902) 命名空间的成员。

[!code-cs[VideoControllersUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetVideoControllersUsing)]

## <a name="exposure"></a>Exposure

[**ExposureControl**](https://msdn.microsoft.com/library/windows/apps/dn278910) 允许你设置照片或视频捕获期间所使用的快门速度。

该示例使用 [**Slider**](https://msdn.microsoft.com/library/windows/apps/br209614) 控件来调整当前曝光值，并使用复选框来切换自动曝光调整。

[!code-xml[ExposureXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetExposureXAML)]

通过检查 [**Supported**](https://msdn.microsoft.com/library/windows/apps/dn297710) 属性来检查当前捕获设备是否支持 **ExposureControl**。 如果该控件受支持，可针对此功能显示和启用 UI。 将用于指示自动曝光调整当前是否处于活动状态的复选框的选中状态设置为 [**Auto**](https://msdn.microsoft.com/library/windows/apps/dn278911) 属性的值。

曝光值必须在设备支持的范围内，且必须是受支持步长的增值。 获取当前设备的支持值，方法是选中 [**Min**](https://msdn.microsoft.com/library/windows/apps/dn278919)、[**Max**](https://msdn.microsoft.com/library/windows/apps/dn278914) 和 [**Step**](https://msdn.microsoft.com/library/windows/apps/dn278930) 属性，它们用于设置滑块控件的对应属性。

在将滑块控件的值设置为 **ExposureControl** 的当前值之前，需先注销 [**ValueChanged**](https://msdn.microsoft.com/library/windows/apps/br209737) 事件处理程序，以便在设置该值时不触发该事件。

[!code-cs[ExposureControl](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetExposureControl)]

在 **ValueChanged** 事件处理程序中，获取该控件的当前值并通过调用 [**SetValueAsync**](https://msdn.microsoft.com/library/windows/apps/dn278927) 设置曝光值。

[!code-cs[ExposureSlider](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetExposureSlider)]

在自动曝光复选框的 **CheckedChanged** 事件处理程序中，通过调用 [**SetAutoAsync**](https://msdn.microsoft.com/library/windows/apps/dn278920) 并以布尔值的形式进行传递，打开或关闭自动曝光调整。

[!code-cs[ExposureCheckBox](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetExposureCheckBox)]

> [!IMPORTANT]
> 自动曝光模式仅在预览流运行时才受支持。 在打开自动曝光之前，检查以确保预览流正在运行。

## <a name="exposure-compensation"></a>曝光补偿

[**ExposureCompensationControl**](https://msdn.microsoft.com/library/windows/apps/dn278897) 允许你设置照片或视频捕获期间所使用的曝光补偿。

此示例使用 [**Slider**](https://msdn.microsoft.com/library/windows/apps/br209614) 控件调整当前曝光补偿值。

[!code-xml[EvXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetEvXAML)]

通过检查 [Supported](supported-codecs.md) 属性来检查当前捕获设备是否支持 **ExposureCompensationControl**。 如果该控件受支持，可针对此功能显示和启用 UI。

曝光补偿值必须在设备支持的范围内，并且必须是受支持步长的增量。 获取当前设备的支持值，方法是选中 [**Min**](https://msdn.microsoft.com/library/windows/apps/dn278901)、[**Max**](https://msdn.microsoft.com/library/windows/apps/dn278899) 和 [**Step**](https://msdn.microsoft.com/library/windows/apps/dn278904) 属性，它们用于设置滑块控件的对应属性。

在将滑块控件的值设置为 **ExposureCompensationControl** 的当前值之前，需先注销 [**ValueChanged**](https://msdn.microsoft.com/library/windows/apps/br209737) 事件处理程序，以便在设置该值时不触发该事件。

[!code-cs[EvControl](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetEvControl)]

在 **ValueChanged** 事件处理程序中，获取该控件的当前值并通过调用 [**SetValueAsync**](https://msdn.microsoft.com/library/windows/apps/dn278903) 设置曝光值。

[!code-cs[EvValueChanged](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetEvValueChanged)]

## <a name="flash"></a>Flash

[**FlashControl**](https://msdn.microsoft.com/library/windows/apps/dn297725) 允许你启用或禁用闪光灯或者启用自动闪光灯（系统会动态确定是否使用闪光灯）。 在支持该控件的设备上，使用它还可以启用自动消除红眼。 这些设置均适用于捕获照片。 [**TorchControl**](https://msdn.microsoft.com/library/windows/apps/dn279077) 是单独的控件，用于针对视频捕获打开或关闭聚光。

此示例使用一组单选按钮，以便用户可以在打开、关闭和自动闪光设置之间切换。 还提供了一个复选框，以便可以在消除红眼和视频聚光之间切换。

[!code-xml[FlashXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetFlashXAML)]

通过检查 [**Supported**](https://msdn.microsoft.com/library/windows/apps/dn297837) 属性来检查当前捕获设备是否支持 **FlashControl**。 如果该控件受支持，可针对此功能显示和启用 UI。 如果 **FlashControl** 受支持，自动消除红眼不一定受支持，因此请在启用 UI 前检查 [**RedEyeReductionSupported**](https://msdn.microsoft.com/library/windows/apps/dn297766) 属性。 由于 **TorchControl** 独立于闪光控件，因此在使用它之前，你也必须先检查它的 [**Supported**](https://msdn.microsoft.com/library/windows/apps/dn279081) 属性。

对于每个闪光单选按钮，可在 [**Checked**](https://msdn.microsoft.com/library/windows/apps/br209796) 事件处理程序中启用或禁用相应的闪光设置。 注意，如果要将闪光设置为始终使用，你必须将 [**Enabled**](https://msdn.microsoft.com/library/windows/apps/dn297733) 属性设置为 true，而将 [**Auto**](https://msdn.microsoft.com/library/windows/apps/dn297728) 属性设置为 false。

[!code-cs[FlashControl](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetFlashControl)]

[!code-cs[FlashRadioButtons](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetFlashRadioButtons)]

在红眼消除复选框的处理程序中，将 [**RedEyeReduction**](https://msdn.microsoft.com/library/windows/apps/dn297758) 属性设置为相应值。

[!code-cs[RedEye](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetRedEye)]

最后，在视频聚光复选框的处理程序中，将 [**Enabled**](https://msdn.microsoft.com/library/windows/apps/dn279078) 属性设置为相应值。

[!code-cs[Torch](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetTorch)]

> [!NOTE] 
>  在某些设备上，除非设备正在运行预览流并且正在主动捕获视频，否则即使 [**TorchControl.Enabled**](https://msdn.microsoft.com/library/windows/apps/dn279078) 设置为 true，手电筒也不会发光。 建议按如下顺序执行操作：先打开视频预览，然后通过将 **Enabled** 设置为 true 打开手电筒，最后启动视频捕获。 在某些设备上，手电筒将在预览启动后亮起。 在其他设备上，在视频捕获启动后，聚光才会亮起。

## <a name="focus"></a>对焦

受 [**FocusControl**](https://msdn.microsoft.com/library/windows/apps/dn297788) 对象支持的用于调整相机对焦的常用方法有三种：连续自动对焦、点按对焦和手动对焦。 相机应用支持以上三种方法，但为了便于阅读，本文将分开讨论每种技术。 本部分还将讨论如何启用对焦辅助灯。

### <a name="continuous-autofocus"></a>连续自动对焦

启用连续自动对焦将指示相机动态调整对焦，以尝试将焦点集中在照片或视频的捕获对象上。 该示例使用单选按钮打开或关闭连续自动对焦。

[!code-xml[CAFXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetCAFXAML)]

通过检查 [**Supported**](https://msdn.microsoft.com/library/windows/apps/dn297785) 属性来检查当前捕获设备是否支持 **FocusControl**。 接下来，确定连续自动对焦是否受支持，方法是检查 [**SupportedFocusModes**](https://msdn.microsoft.com/library/windows/apps/dn608079) 列表中是否包含值 [**FocusMode.Continuous**](https://msdn.microsoft.com/library/windows/apps/dn608084)；如果包含，将显示连续自动对焦单选按钮。

[!code-cs[CAF](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetCAF)]

在连续自动对焦单选按钮的 [**Checked**](https://msdn.microsoft.com/library/windows/apps/br209796) 事件处理程序中，使用 [**VideoDeviceController.FocusControl**](https://msdn.microsoft.com/library/windows/apps/dn279091) 属性可获取该控件的一个实例。 如果你的应用先前调用了 [**LockAsync**](https://msdn.microsoft.com/library/windows/apps/dn608075)，则调用 [**UnlockAsync**](https://msdn.microsoft.com/library/windows/apps/dn608081) 以解锁该控件，以便启用两种对焦模式中的另外一种。

创建新的 [**FocusSettings**](https://msdn.microsoft.com/library/windows/apps/dn608085) 对象并将 [**Mode**](https://msdn.microsoft.com/library/windows/apps/dn608090) 属性设置为 **Continuous**。 将 [**AutoFocusRange**](https://msdn.microsoft.com/library/windows/apps/dn608086) 属性设置为适合你的应用方案的值，或设置为由用户从你的 UI 中选择的值。 将 **FocusSettings** 对象传入 [**Configure**](https://msdn.microsoft.com/library/windows/apps/dn608067) 方法，然后调用 [**FocusAsync**](https://msdn.microsoft.com/library/windows/apps/dn297794) 以启动连续自动对焦。

[!code-cs[CafFocusRadioButton](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetCafFocusRadioButton)]

> [!IMPORTANT]
> 自动对焦模式仅在预览流运行时才受支持。 在打开连续自动对焦之前，检查以确保预览流正在运行。

### <a name="tap-to-focus"></a>点按对焦

点按对焦技术使用 [**FocusControl**](https://msdn.microsoft.com/library/windows/apps/dn297788) 和 [**RegionsOfInterestControl**](https://msdn.microsoft.com/library/windows/apps/dn279064) 来指定捕获设备应对焦的捕获帧的子区域。 对焦区域由用户通过点按显示预览流的屏幕来确定。

此示例使用单选按钮启用和禁用点按对焦模式。

[!code-xml[TapFocusXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetTapFocusXAML)]

通过选中 [**Supported**](https://msdn.microsoft.com/library/windows/apps/dn297785) 属性，查看当前捕获设备是否支持 **FocusControl**。 **RegionsOfInterestControl** 必须受支持，且必须至少支持一个区域才能使用该技术。 选中 [**AutoFocusSupported**](https://msdn.microsoft.com/library/windows/apps/dn279066) 和 [**MaxRegions**](https://msdn.microsoft.com/library/windows/apps/dn279069) 属性，以确定是显示还是隐藏用于点按对焦的单选按钮。

[!code-cs[TapFocus](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetTapFocus)]

在点按对焦单选按钮的 [**Checked**](https://msdn.microsoft.com/library/windows/apps/br209796) 事件处理程序中，使用 [**VideoDeviceController.FocusControl**](https://msdn.microsoft.com/library/windows/apps/dn279091) 属性获取该控件的一个实例。 如果你的应用之前调用了 [**UnlockAsync**](https://msdn.microsoft.com/library/windows/apps/dn608081) 来启用连续对焦，则调用 [**LockAsync**](https://msdn.microsoft.com/library/windows/apps/dn608075) 以解锁该控件，然后等待用户点击屏幕更改焦点。

[!code-cs[TapFocusRadioButton](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetTapFocusRadioButton)]

该示例会在用户点击屏幕时对焦于某一区域，然后在用户再次点击时删除该对焦，就像切换一样。 使用布尔变量跟踪当前切换的状态。

[!code-cs[IsFocused](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetIsFocused)]

下一步是，在用户点击屏幕时通过处理当前正在显示捕获预览流的 [**CaptureElement**](https://msdn.microsoft.com/library/windows/apps/br209278) 的 [**Tapped**](https://msdn.microsoft.com/library/windows/apps/br208985) 事件来侦听相关事件。 如果相机当前未进行预览，或者如果点按对焦模式处于禁用状态，则从该处理程序返回而不执行任何操作。

如果跟踪变量 *\_isFocused* 已切换为 false，并且如果相机当前不在对焦进程中（由 **FocusControl** 的 [**FocusState**](https://msdn.microsoft.com/library/windows/apps/dn608074) 属性确定），则开始点击对焦进程。 从传入处理程序的事件参数获取用户点按的位置。 该示例还利用此机会选取将对焦的区域大小。 在本例中，大小为捕获元素最小尺寸的 1/4。 将点击位置和区域大小传入 **TapToFocus** 帮助程序方法，该方法将在下一部分中进行定义。

如果 *\_isFocused* 切换设置为 true，用户点击应该会从之前的区域中清除焦点。 这将在下面显示的 **TapUnfocus** 帮助程序方法中执行。

[!code-cs[TapFocusPreviewControl](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetTapFocusPreviewControl)]

在 **TapToFocus** 帮助程序方法中，先将 *\_isFocused* 切换设置为 true，以便下一屏幕点击可以从点击区域释放焦点。

此帮助程序方法中的下一个任务是确定矩形，其中包含将分配给对焦控件的预览流。 这需要两个步骤。 第一步是确定 [**CaptureElement**](https://msdn.microsoft.com/library/windows/apps/br209278) 控件内预览流占用的矩形。 这取决于预览流的大小和设备方向。 帮助程序方法 **GetPreviewStreamRectInControl**（将在本部分末尾处显示）执行此任务，并返回包含预览流的矩形。

**TapToFocus** 的下一个任务是，在捕获流中将 **CaptureElement.Tapped** 事件处理程序内确定的点击位置和所需对焦矩形尺寸转换为坐标。 **ConvertUiTapToPreviewRect** 帮助程序方法（将在本部分的后面部分显示）执行此转换，并以捕获流坐标的形式返回矩形，其中将请求对焦。

现在已获取目标矩形，将创建新的 [**RegionOfInterest**](https://msdn.microsoft.com/library/windows/apps/dn279058) 对象，从而将 [**Bounds**](https://msdn.microsoft.com/library/windows/apps/dn279062) 属性设置为之前步骤中所获得的目标矩形。

获取捕获设备的 [**FocusControl**](https://msdn.microsoft.com/library/windows/apps/dn297788)。 创建新的 [**FocusSettings**](https://msdn.microsoft.com/library/windows/apps/dn608085) 对象，并在进行检查以确保 [**Mode**](https://msdn.microsoft.com/library/windows/apps/dn608076) 和 [**AutoFocusRange**](https://msdn.microsoft.com/library/windows/apps/dn608086) 受 **FocusControl** 支持之后，将它们设置为所需值。 在 **FocusControl** 上调用 [**Configure**](https://msdn.microsoft.com/library/windows/apps/dn608067) 以使你的设置处于活动状态，并发信号通知设备开始对焦指定区域。

接下来，获取捕获设备的 [**RegionsOfInterestControl**](https://msdn.microsoft.com/library/windows/apps/dn279064) 并调用 [**SetRegionsAsync**](https://msdn.microsoft.com/library/windows/apps/dn279070) 以设置活动区域。 可以在支持该控件的设备上设置多个关注区域，不过该示例将仅设置一个区域。

最后，在 **FocusControl** 上调用 [**FocusAsync**](https://msdn.microsoft.com/library/windows/apps/dn297794) 以启动对焦。

> [!IMPORTANT]
> 当实现点按对焦时，操作顺序很重要。 我们按照以下顺序调用这些 API：
>
> 1. [**FocusControl.Configure**](https://msdn.microsoft.com/library/windows/apps/dn608067)
> 2. [**RegionsOfInterestControl.SetRegionsAsync**](https://msdn.microsoft.com/library/windows/apps/dn279070)
> 3. [**FocusControl.FocusAsync**](https://msdn.microsoft.com/library/windows/apps/dn297794)

[!code-cs[TapToFocus](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetTapToFocus)]

在 **TapUnfocus** 帮助程序方法中，获取 **RegionsOfInterestControl**，然后调用 [**ClearRegionsAsync**](https://msdn.microsoft.com/library/windows/apps/dn279068) 以清除 **TapToFocus** 帮助程序方法内已注册该控件的区域。 接下来，获取 **FocusControl** 并调用 [**FocusAsync**](https://msdn.microsoft.com/library/windows/apps/dn297794)，以使设备在没有关注区域的情况下重新对焦。

[!code-cs[TapUnfocus](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetTapUnfocus)]

**GetPreviewStreamRectInControl** 帮助程序方法使用预览流的分辨率和设备方向来确定包含预览流的预览元素内的矩形，并修去控件可能提供的任何上下黑边形式的填充，以保持该流的纵横比。 此方法使用[使用 MediaCapture 的照片、视频和音频捕获](basic-photo-video-and-audio-capture-with-MediaCapture.md)中提供的基本媒体捕获示例代码中定义的类成员变量。

[!code-cs[GetPreviewStreamRectInControl](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetGetPreviewStreamRectInControl)]

**ConvertUiTapToPreviewRect** 帮助程序方法用参数来表示点击事件的位置、所需的对焦区域大小，以及包含从 **GetPreviewStreamRectInControl** 帮助程序方法获取的预览流的矩形。 该方法使用上述值和设备的当前方向，来计算包含所需区域的预览流内的矩形。 同样地，此方法使用基本媒体捕获示例代码中定义的类成员变量，该代码可从[使用 MediaCapture 捕获照片和视频](capture-photos-and-video-with-mediacapture.md)中获取。

[!code-cs[ConvertUiTapToPreviewRect](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetConvertUiTapToPreviewRect)]

### <a name="manual-focus"></a>手动对焦

手动对焦技术使用 **Slider** 控件来设置捕获设备的当前对焦深度。 使用单选按钮来打开或关闭手动对焦。

[!code-xml[ManualFocusXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetManualFocusXAML)]

通过检查 [**Supported**](https://msdn.microsoft.com/library/windows/apps/dn297837) 属性来检查当前捕获设备是否支持 **FocusControl**。 如果该控件受支持，可针对此功能显示和启用 UI。

对焦值必须在设备支持的范围内，并且必须是受支持步长的增量。 获取当前设备的支持值，方法是选中 [**Min**](https://msdn.microsoft.com/library/windows/apps/dn297808)、[**Max**](https://msdn.microsoft.com/library/windows/apps/dn297802) 和 [**Step**](https://msdn.microsoft.com/library/windows/apps/dn297833) 属性，它们用于设置滑块控件的对应属性。

在将滑块控件的值设置为 **FocusControl** 的当前值之前，需先注销 [**ValueChanged**](https://msdn.microsoft.com/library/windows/apps/br209737) 事件处理程序，以便在设置该值时不触发该事件。

[!code-cs[Focus](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetFocus)]

在手动对焦单选按钮的 **Checked** 事件处理程序中，如果你的应用之前通过调用 [**UnlockAsync**](https://msdn.microsoft.com/library/windows/apps/dn608081) 解锁了对焦，则获取 **FocusControl** 对象并调用 [**LockAsync**](https://msdn.microsoft.com/library/windows/apps/dn608075)。

[!code-cs[ManualFocusChecked](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetManualFocusChecked)]

在手动对焦滑块的 **ValueChanged** 事件处理程序中，获取该控件的当前值并通过调用 [**SetValueAsync**](https://msdn.microsoft.com/library/windows/apps/dn297828) 设置对焦值。

[!code-cs[FocusSlider](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetFocusSlider)]

### <a name="enable-the-focus-light"></a>启用对焦灯

你可以在支持对焦辅助灯的设备上启用该灯，以帮助设备对焦。 此示例使用复选框来启用或禁用对焦辅助灯。

[!code-xml[FocusLightXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetFocusLightXAML)]

通过选中 [**Supported**](https://msdn.microsoft.com/library/windows/apps/dn297785) 属性，查看当前捕获设备是否支持 **FlashControl**。 还应检查 [**AssistantLightSupported**](https://msdn.microsoft.com/library/windows/apps/dn608066)以确保辅助灯也受支持。 如果两者均受支持，可针对此功能显示和启用 UI。

[!code-cs[FocusLight](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetFocusLight)]

在 **CheckedChanged** 事件处理程序中，获取捕获设备 [**FlashControl**](https://msdn.microsoft.com/library/windows/apps/dn297725) 对象。 设置 [**AssistantLightEnabled**](https://msdn.microsoft.com/library/windows/apps/dn608065) 属性以启用或禁用对焦灯。

[!code-cs[FocusLightCheckBox](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetFocusLightCheckBox)]

## <a name="iso-speed"></a>ISO 感光度

[**IsoSpeedControl**](https://msdn.microsoft.com/library/windows/apps/dn297850) 允许你设置照片或视频捕获期间所使用的 ISO 感光度。

该示例使用 [**Slider**](https://msdn.microsoft.com/library/windows/apps/br209614) 控件来调整当前曝光补偿，而使用复选框来切换自动 ISO 感光度调整。

[!code-xml[IsoXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetIsoXAML)]

通过检查 [**Supported**](https://msdn.microsoft.com/library/windows/apps/dn297869) 属性来检查当前捕获设备是否支持 **IsoSpeedControl**。 如果该控件受支持，可针对此功能显示和启用 UI。 将用于指示自动 ISO 感光度调整当前是否处于活动状态的复选框的选中状态设置为 [**Auto**](https://msdn.microsoft.com/library/windows/apps/dn608093) 属性的值。

ISO 感光度值必须在设备支持的范围内，且必须是受支持步长的增值。 获取当前设备的支持值，方法是选中 [**Min**](https://msdn.microsoft.com/library/windows/apps/dn608095)、[**Max**](https://msdn.microsoft.com/library/windows/apps/dn608094) 和 [**Step**](https://msdn.microsoft.com/library/windows/apps/dn608129) 属性，它们用于设置滑块控件的对应属性。

在将滑块控件的值设置为 **IsoSpeedControl** 的当前值之前，需先注销 [**ValueChanged**](https://msdn.microsoft.com/library/windows/apps/br209737) 事件处理程序，以便在设置该值时不触发该事件。

[!code-cs[IsoControl](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetIsoControl)]

在 **ValueChanged** 事件处理程序中，获取该控件的当前值并通过调用 [**SetValueAsync**](https://msdn.microsoft.com/library/windows/apps/dn608128) 设置 ISO 感光度值。

[!code-cs[IsoSlider](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetIsoSlider)]

在自动 ISO 感光度复选框的 **CheckedChanged** 事件处理程序中，通过调用 [**SetAutoAsync**](https://msdn.microsoft.com/library/windows/apps/dn608127) 可打开自动 ISO 感光度调整。 通过调用 [**SetValueAsync**](https://msdn.microsoft.com/library/windows/apps/dn608128) 并传入滑块控件的当前值，可关闭自动 ISO 感光度调整。

[!code-cs[IsoCheckBox](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetIsoCheckBox)]

## <a name="optical-image-stabilization"></a>光学图像防抖动

光学图像防抖动 (OIS) 通过机械地操作硬件捕获设备来稳定捕获的视频流，它可以提供比数字防抖动更优越的结果。 在不支持 OIS 的设备上，你可以使用 VideoStabilizationEffect，以便在捕获的视频上执行数字防抖动。 有关详细信息，请参阅[视频捕获的效果](effects-for-video-capture.md)。

通过检查 [**OpticalImageStabilizationControl.Supported**](https://msdn.microsoft.com/library/windows/apps/dn926689) 属性，确定 OIS 在当前设备上是否受支持。

OIS 控件支持以下三种模式：开、关和自动。这意味着设备可以动态方式确定 OIS 是否会改进媒体捕获；如果可以改进，则启用 OIS。 若要确定特定模式在设备上是否受支持，请检查以查看 [**OpticalImageStabilizationControl.SupportedModes**](https://msdn.microsoft.com/library/windows/apps/dn926690) 集合是否包含所需模式。

通过将 [**OpticalImageStabilizationControl.Mode**](https://msdn.microsoft.com/library/windows/apps/dn926691) 设置为所需模式，以便启用或禁用 OIS。

[!code-cs[SetOpticalImageStabilizationMode](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSetOpticalImageStabilizationMode)]

## <a name="powerline-frequency"></a>Powerline frequency
某些相机设备支持防闪烁处理，该功能依赖于获知当前环境中的电力线 AC 频率。 某些设备支持自动确定电力线频率，而另一些设备需要手动设置该频率。 以下代码示例显示如何确定设备上的电力线频率支持以及如何手动设置该频率（如果需要）。 

首先，调用 **VideoDeviceController** 方法 [**TryGetPowerlineFrequency**](https://msdn.microsoft.com/library/windows/apps/br206898)，从而传入 [**PowerlineFrequency**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.PowerlineFrequency) 类型的输出参数；如果此调用失败，则电力线频率控制在当前设备上不受支持。 如果该功能受支持，则你可以通过尝试设置自动模式来确定自动模式在设备上是否可用。 通过调用[**TrySetPowerlineFrequency**](https://msdn.microsoft.com/library/windows/apps/br206899)并传入值**自动**执行此操作。如果调用成功，这意味着你的自动电力线频率受支持。 如果设备上支持电力线频率控制器，但不支持自动频率检测，你仍然可以使用 **TrySetPowerlineFrequency** 手动设置频率。 在此示例中，**MyCustomFrequencyLookup** 是你实现的自定义方法，用于为设备的当前位置确定正确的频率。 

[!code-cs[PowerlineFrequency](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetPowerlineFrequency)]

## <a name="white-balance"></a>白平衡

[**WhiteBalanceControl**](https://msdn.microsoft.com/library/windows/apps/dn279104) 允许你设置照片或视频捕获期间所使用的白平衡。

该示例使用 [**ComboBox**](https://msdn.microsoft.com/library/windows/apps/br209348) 控件以从内置色温预设中进行选择，而使用 [**Slider**](https://msdn.microsoft.com/library/windows/apps/br209614) 控件进行白平衡调整。

[!code-xml[WhiteBalanceXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetWhiteBalanceXAML)]

通过检查 [**Supported**](https://msdn.microsoft.com/library/windows/apps/dn279120) 属性来检查当前捕获设备是否支持 **WhiteBalanceControl**。 如果该控件受支持，可针对此功能显示和启用 UI。 将组合框的项目设置为 [**ColorTemperaturePreset**](https://msdn.microsoft.com/library/windows/apps/dn278894) 枚举的值。 并且将选定项设置为 [**Preset**](https://msdn.microsoft.com/library/windows/apps/dn279110) 属性的当前值。

对于手动控件，白平衡值必须在设备支持的范围内，且必须是受支持步长的增值。 获取当前设备的支持值，方法是选中 [**Min**](https://msdn.microsoft.com/library/windows/apps/dn279109)、[**Max**](https://msdn.microsoft.com/library/windows/apps/dn279107) 和 [**Step**](https://msdn.microsoft.com/library/windows/apps/dn279119) 属性，它们用于设置滑块控件的对应属性。 在启用手动控件前，检查以确保最小受支持值和最大支持值之差大于该步长。 如果不是这样，则表明当前设备不支持手动控件。

在将滑块控件的值设置为 **WhiteBalanceControl** 的当前值之前，需先注销 [**ValueChanged**](https://msdn.microsoft.com/library/windows/apps/br209737) 事件处理程序，以便在设置该值时不触发该事件。

[!code-cs[WhiteBalance](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetWhiteBalance)]

在色温预设组合框的 [**SelectionChanged**](https://msdn.microsoft.com/library/windows/apps/br209776) 事件处理程序中，获取当前选定的预设并通过调用 [**SetPresetAsync**](https://msdn.microsoft.com/library/windows/apps/dn279113) 来设置控件值。 如果选定的预设值不为 **Manual**，将禁用手动白平衡滑块。

[!code-cs[WhiteBalanceComboBox](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetWhiteBalanceComboBox)]

在 **ValueChanged** 事件处理程序中，获取该控件的当前值并通过调用 [**SetValueAsync**](https://msdn.microsoft.com/library/windows/apps/dn278927) 设置白平衡值。

[!code-cs[WhiteBalanceSlider](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetWhiteBalanceSlider)]

> [!IMPORTANT]
> 调整白平衡仅在预览流运行时才受支持。 在设置白平衡值或预设前，检查以确保预览流正在运行。

> [!IMPORTANT]
> **ColorTemperaturePreset.Auto** 预设值指示系统自动调整白平衡级别。 在某些情况（如捕获照片序列，其中每个帧的平衡级别均相同）下，你需要将控件锁定为当前自动值。 为此，请调用 [**SetPresetAsync**](https://msdn.microsoft.com/library/windows/apps/dn279113) 并指定 **Manual** 预设，但不要在控件上使用 [**SetValueAsync**](https://msdn.microsoft.com/library/windows/apps/dn279114) 设置值。 这样做会导致设备锁定当前值。 请不要尝试读取当前控件值，并将该值传递给 **SetValueAsync**，因为不能保证该值是正确的。

## <a name="zoom"></a>缩放

[**ZoomControl**](https://msdn.microsoft.com/library/windows/apps/dn608149) 允许你设置照片或视频捕获期间所使用的缩放级别。

此示例使用 [**Slider**](https://msdn.microsoft.com/library/windows/apps/br209614) 控件调整当前缩放级别。 以下部分将介绍如何基于屏幕上的收缩手势调整缩放。

[!code-xml[ZoomXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetZoomXAML)]

通过检查 [**Supported**](https://msdn.microsoft.com/library/windows/apps/dn633819) 属性来检查当前捕获设备是否支持 **ZoomControl**。 如果该控件受支持，可针对此功能显示和启用 UI。

缩放级别值必须在设备支持的范围内，并且必须是受支持步长的增量。 获取当前设备的支持值，方法是选中 [**Min**](https://msdn.microsoft.com/library/windows/apps/dn633817)、[**Max**](https://msdn.microsoft.com/library/windows/apps/dn608150) 和 [**Step**](https://msdn.microsoft.com/library/windows/apps/dn633818) 属性，它们用于设置滑块控件的对应属性。

在将滑块控件的值设置为 **ZoomControl** 的当前值之前，需先注销 [**ValueChanged**](https://msdn.microsoft.com/library/windows/apps/br209737) 事件处理程序，以便在设置该值时不触发该事件。

[!code-cs[ZoomControl](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetZoomControl)]

在 **ValueChanged** 事件处理程序中，为 [**ZoomSettings**](https://msdn.microsoft.com/library/windows/apps/dn926722) 类创建一个新实例，并将 [**Value**](https://msdn.microsoft.com/library/windows/apps/dn926724) 属性设置为缩放滑块控件的当前值。 如果 **ZoomControl** 的 [**SupportedModes**](https://msdn.microsoft.com/library/windows/apps/dn926721) 属性包含 [**ZoomTransitionMode.Smooth**](https://msdn.microsoft.com/library/windows/apps/dn926726)，这表示设备支持在不同的缩放级别之间进行平滑过渡。 由于此模式能提供更好的用户体验，因此通常你会希望针对 **ZoomSettings** 对象的 [**Mode**](https://msdn.microsoft.com/library/windows/apps/dn926723) 属性使用该值。

最后，通过将 **ZoomSettings** 对象传递给 **ZoomControl** 对象的 [**Configure**](https://msdn.microsoft.com/library/windows/apps/dn926719) 方法来更改当前缩放设置。

[!code-cs[ZoomSlider](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetZoomSlider)]

### <a name="smooth-zoom-using-pinch-gesture"></a>借助收缩手势的平滑缩放

正如上一部分中所讨论的那样，在支持它的设备上，平滑缩放模式允许捕获设备在不同的数字缩放级别之间平滑过渡，以便用户可以在捕获操作期间动态调整缩放级别，而不会导致离散和不和谐过渡。 本部分将介绍如何调整缩放级别以响应收缩手势。

首先，通过检查 [**ZoomControl.Supported**](https://msdn.microsoft.com/library/windows/apps/dn633819) 属性，确定数字缩放控件在当前设备上是否受支持。 接下来，通过检查 [**ZoomControl.SupportedModes**](https://msdn.microsoft.com/library/windows/apps/dn926721) 以查看它是否包含值 [**ZoomTransitionMode.Smooth**](https://msdn.microsoft.com/library/windows/apps/dn926726)，确定是否可以使用平滑缩放模式。

[!code-cs[IsSmoothZoomSupported](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetIsSmoothZoomSupported)]

在启用多点触控的设备上，典型方案是基于两指收缩手势来调整缩放系数。 将 [**CaptureElement**](https://msdn.microsoft.com/library/windows/apps/br209278) 控件的 [**ManipulationMode**](https://msdn.microsoft.com/library/windows/apps/br208948) 属性设置为 [**ManipulationModes.Scale**](https://msdn.microsoft.com/library/windows/apps/br227934) 以启用收缩手势。 然后，注册收缩手势更改大小时引发的 [**ManipulationDelta**](https://msdn.microsoft.com/library/windows/apps/br208946) 事件。

[!code-cs[RegisterPinchGestureHandler](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetRegisterPinchGestureHandler)]

在针对 **ManipulationDelta** 事件的处理程序中，将根据用户的收缩手势的变化更新缩放系数。 [**ManipulationDelta.Scale**](https://msdn.microsoft.com/library/windows/apps/br242016) 值表示收缩手势的比例变化，如此收缩大小的小幅上升是略大于 1.0 的数字，并且收缩大小的小幅下降是略小于 1.0 的数字。 在此示例中，缩放控制的当前值乘以比例增量。

在设置缩放系数之前，你必须确保该值不小于由 [**ZoomControl.Min**](https://msdn.microsoft.com/library/windows/apps/dn633817) 属性表示的受设备支持的最小值。 此外，还要确保该值小于或等于 [**ZoomControl.Max**](https://msdn.microsoft.com/library/windows/apps/dn608150) 值。 最后，你必须确保该缩放系数是由[**步骤**](https://msdn.microsoft.com/library/windows/apps/dn633818)属性在设备支持该缩放步长的倍数。 如果你的缩放系数不符合这些要求，当你试图在捕获设备上设置缩放级别时将引发异常。

通过创建新的 [**ZoomSettings**](https://msdn.microsoft.com/library/windows/apps/dn926722) 对象，在捕获设备上设置缩放级别。 将 [**Mode**](https://msdn.microsoft.com/library/windows/apps/dn926723) 属性设置为 [**ZoomTransitionMode.Smooth**](https://msdn.microsoft.com/library/windows/apps/dn926726)，然后将 [**Value**](https://msdn.microsoft.com/library/windows/apps/dn926724) 属性设置为你想要的缩放系数。 最后，调用 [**ZoomControl.Configure**](https://msdn.microsoft.com/library/windows/apps/dn926719) 在设备上设置新的缩放值。 该设备将平滑过渡到新的缩放值。

[!code-cs[ManipulationDelta](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetManipulationDelta)]

## <a name="related-topics"></a>相关主题

* [相机](camera.md)
* [使用 MediaCapture 捕获基本的照片、视频和音频](basic-photo-video-and-audio-capture-with-MediaCapture.md)
