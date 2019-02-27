---
Description: Design your app so that it looks good and functions well in Mixed Reality.
title: 针对混合现实而开发
ms.assetid: ''
label: Designing for Mixed Reality
template: detail.hbs
isNew: true
keywords: 混合现实, Hololens, 增强现实, 注视, 语音, 控制器
ms.date: 02/05/2018
ms.topic: article
pm-contact: chigy
design-contact: jeffarn
dev-contact: ''
doc-status: ''
ms.localizationpriority: medium
ms.openlocfilehash: b988859422a80b31d94a133e36631b078ac7c14e
ms.sourcegitcommit: 079801609165bc7eb69670d771a05bffe236d483
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/27/2019
ms.locfileid: "9116209"
---
# <a name="designing-for-mixed-reality"></a>针对混合现实而开发

设计应用以在混合现实中看起来很美观，并且利用新的输入法。

## <a name="overview"></a>概述

[混合现实](https://developer.microsoft.com/windows/mixed-reality/mixed_reality)是将物理世界与数字世界相混合的结果。 混合现实体验的范围包括在一种极限设备，如 HoloLens（一种混合计算机生成的内容与真实世界的设备）以及其他设备，虚拟现实的完全沉浸式视图（正如使用 Windows Mixed Reality 头戴显示设备看到的场景）。 有关体验各不相同的方式的示例，请参阅[混合现实应用的类型](https://developer.microsoft.com/en-us/windows/mixed-reality/types_of_mixed_reality_apps)。

几乎所有现有 UWP 应用都将在混合现实环境中作为 2D 应用运行，没有任何更改，但用户的体验可通过遵循本主题中指南的相关内容得到改善。

![混合现实视图](images/MR-01.png)

HoloLens 和 Windows Mixed Reality 头戴显示设备支持在 UWP 平台上运行的应用程序，并且支持两种不同的体验类型。 

### <a name="2d-vs-immersive-experience"></a>2D 与沉浸式体验

沉浸式应用接管对用户显示的整个屏幕，同时将用户置于由应用创建的视图的中心。 例如，沉浸式游戏可将用户置于异形星球的表面上，或者导游应用可将用户置于南美洲的某个村庄。 创建沉浸式应用需要 3D 图形或捕获的立体视频。 沉浸式应用通常是通过使用第三方游戏引擎（如 Unity）或利用 DirectX 来开发的。

如果要创建沉浸式应用，你应访问 [Windows Mixed Reality 开发人员中心](https://developer.microsoft.com/windows/mixed-reality)以了解更多信息。

2D 应用作为传统的平面窗口在用户的视图内运行。 在 HoloLens 上，这表示固定到墙的某个视图或用户自己的真实客厅或办公室空间中的某点。 在 Windows Mixed Reality 头戴显示设备中，该应用已固定到[混合现实家庭版](https://docs.microsoft.com/windows/mixed-reality/enthusiast-guide/your-mixed-reality-home)（有时称为*悬崖小屋*）中的墙上。

![在混合现实中运行的多个应用](images/MR-multiple.png)


这些 2D 应用不会接管整个视图：它们将放置在视图内。 多个 2D 应用可同时存在于该环境中。

本主题的余下部分将讨论有关 2D 体验的设计注意事项。

## <a name="launching-2d-apps"></a>启动 2D 应用

![混合现实开始菜单](images/MR-start-options.png)

所有应用均从“开始”菜单启动，但也可创建 3D 对象来用作应用启动器。 有关详细信息，请观看[此视频](https://www.youtube.com/watch?v=TxIslHsEXno)。

## <a name="the-2d-app-input-overview"></a>2D 应用输入概述

HoloLens 和混合现实平台上均支持键盘和鼠标。 你可以通过蓝牙将键盘和鼠标与 HoloLens 直接配对。 混合现实应用支持连接到主计算机的鼠标和键盘。 两者在需要进行精细控制的情况下都可能很有用。

其他更自然的输入法也受支持，而且这些输入法可能在用户未坐在办公桌前使用真实键盘或需要精细控制时尤其有用。

无需任何额外的硬件或编码，应用将在使用 2D 应用时使用注视 - 用户沿着查看的矢量 - 作为鼠标指针。 其实现如同鼠标指针悬停在虚拟场景中的某个事物上。

在典型交互中，用户将查看应用中的某控件，从而导致该控件突出显示。 用户将使用手势（在 HoloLens 上）或控制器或通过提供语音命令来触发操作。 如果用户选择文本输入字段，软件键盘将显示。 


![混合现实中的弹出式键盘](images/MR-keyboard.png)

请务必注意，所有这些交互都将自动发生，不会在部件上进行额外的编码，从而在 UWP 平台上运行。 来自 HoloLens 和混合现实头戴显示设备的输入将显示为到 2D 应用的触控输入。 这意味着默认情况下许多 UWP 应用将运行且在混合现实中可用。 

也就是说，借助一些额外的工作，这种体验可得到大大改善。 例如，[语音控制](https://developer.microsoft.com/windows/mixed-reality/voice_design)可能尤其有效。 HoloLens 和混合现实环境支持用于启动应用和与应用进行交互的语音命令，并且包括的语音支持将显示为此方法的自然扩展。 若要详细了解如何向 UWP 应用添加语音支持，请参阅[语音交互]( https://docs.microsoft.com/windows/uwp/design/input/speech-interactions)。 


### <a name="selecting-the-right-controller"></a>选择合适的控制器

![混合现实运动控制器](images/MR-controllers.png)

多个新颖的输入法已进行专门设计以用于混合现实，具体为：

* [手势](https://developer.microsoft.com/windows/mixed-reality/gestures)（仅限 HoloLens，但仅用于启动 2D 应用）
* [游戏板支持](https://developer.microsoft.com/windows/mixed-reality/hardware_accessories)（两个环境均适用）
* [遥控器设备](https://developer.microsoft.com/windows/mixed-reality/hardware_accessories)（仅限 HoloLens）
* [运动控制器](https://developer.microsoft.com/windows/mixed-reality/motion_controllers)（仅限混合现实设备，如上所示。）

这些控制器使得与虚拟对象的交互似乎变得自然和精确。 其中一些交互可免费获取。 例如，HoloLens 选择手势或单击运动控制器的 Windows 键或扳机键将生成你所预期，同样，在部件上进行任何编码的输入的响应。

其他情况下，你可能需要添加代码以利用可用的额外信息和输入。 例如，运动控制器可用于通过精细控制操纵对象，前提是你编写用于将其位置和按钮按下置于帐户中的代码。

> [!NOTE]
> 总结：指导主体应始终为用户尽可能提供自然且流畅的输入法。


## <a name="2d-app-design-considerations-functionality"></a>2D 应用设计注意事项：功能

当创建一个将可能用在混合现实平台上的 UWP 应用时，需要记住以下几个事项。

* 当用于运动控制器、游戏板或手势时拖放可能不适用。 如果应用程序很大程度上依赖于拖放操作，你将面要提供支持此操作的替代方法，例如呈现一个用于确认对象是否迁移到新位置的对话。

* 注意声音更改的方式。 如果应用生成声音效果，则声音源将显示在应用在虚拟世界中的固定位置。 随着用户远离应用，声音将减小。 有关更多信息，请参阅[空间音效](https://developer.microsoft.com/windows/mixed-reality/spatial_sound)。

* 考虑视野并提供提示。 并非每个设备都提供与计算机监视器一样大的视野。 有关完整的详细信息，请参阅[全息照相帧](https://developer.microsoft.com/windows/mixed-reality/holographic_frame)。 此外，用户可能距正在运行的应用有一些距离。 也就是说，应用可能显示固定到（真实或虚拟）世界中不同位置处的墙上。 应用可能需要获取用户关注，或始终考虑整个视图不可见。 Toast 通知可用，但获取用户关注的另一种方式可能是发出声音或[语音](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/SpeechRecognitionAndSynthesis/cs/Scenario_SynthesizeText.xaml.cs)警报。

* 系统自动向 2D 应用提供一个[应用栏](https://developer.microsoft.com/windows/mixed-reality/app_bar_and_bounding_box)以允许用户在虚拟环境中移动和缩放它们。 这些视图可以垂直调整大小，或者在保持同一纵横比的情况下调整大小。


## <a name="2d-app-design-considerations-uiux"></a>2D 应用设计注意事项：UI/UX

* 实现 [Fluent Design 系统](https://docs.microsoft.com/windows/uwp/design/fluent-design-system/)的 XAML 控件（如[导航视图](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/navigationview)）和效果（如[亚克力](https://docs.microsoft.com/windows/uwp/design/style/acrylic)）都尤其适用于 2D 混合现实应用。

* 在混合现实设备中或至少在混合现实模拟器中测试文本和窗口大小。 应用将有一个为 853x480 有效像素的默认窗口大小。 使用较大的字号（建议使用约为 32 的磅值）并阅读[针对 Hololens 更新现有通用应用](https://developer.microsoft.com/windows/mixed-reality/updating_your_existing_universal_app_for_hololens)。 [版式](https://developer.microsoft.com/windows/mixed-reality/typography)一文中详细涵盖了本主题。 在 Visual Studio 中工作时，有一个适用于 57" HoloLens 2D 应用的 XAML 设计编辑器设置，可提供具有正确的比例和维度的视图。 

![混合现实应用中显示的文本应为大。](images/MR-text.png)

* [注视是鼠标](https://developer.microsoft.com/windows/mixed-reality/gaze_targeting)。 当用户看着某内容时，它将充当**触控悬停**事件，所以，仅看着某个对象可能会触发无意的弹出窗口或其他不需要的交互。 你可能需要检查应用目前是否正在混合现实中运行并更改此行为。 请参阅下面的**运行时支持**。 

* 当用户使用运动控制器注视某些内容或点时，**触控悬停**事件将发生。 这包括 **PointerPoint**，其中 **PointerType** 为**触控**，但 **IsInContact** 为 **false**。 当某些形式的提交发生（例如，按钮下游戏板 A 按钮、按下遥控器设备、按下运动控制器扳机键或“选择”语音识别人头）时，**触控按下**事件将发生，同时具有 **IsInContact** 的 **PointerPoint** 变为 **true**。 有关这些输入事件的更多信息，请参阅[触控交互](https://docs.microsoft.com/windows/uwp/design/input/touch-interactions)。

* 请记住，注视并不与鼠标指向一样地准确。 较小的鼠标目标或按钮可能会为用户带来沮丧，因此请相应地调整控件大小。 如果它们针对触控进行设计，则将适用于混合现实，但你可能决定在运行时放大一些按钮。 请参阅[针对 Hololens 更新现有通用应用](https://developer.microsoft.com/windows/mixed-reality/updating_your_existing_universal_app_for_hololens)。

* HoloLens 将黑色定义为光线不足。 它只是不会呈现，从而允许“现实世界”隐约显示。 如果这会带来困惑，则应用程序不应使用黑色。 在混合现实头戴显示设备中，黑色就是黑色。

* HoloLens 不支持应用中的颜色主题，而且默认为蓝色，以确保用户的最佳体验。 有关选择颜色的更多建议，你应参阅[本主题](https://developer.microsoft.com/windows/mixed-reality/color,_light_and_materials)，其中讨论了在混合现实设计中对颜色和材料的使用。


## <a name="other-points-to-consider"></a>要考虑的其他要点

* 尽管[桌面桥](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-root)可帮助将现有 (Win32) 桌面应用带入 Windows 10 和 Microsoft Store，但它无法创建同时在 HoloLens 上和混合现实中运行的应用。




## <a name="runtime-support"></a>运行时支持

应用可确定其是否在运行时在混合现实设备上运行，并且以此为契机来调整控件大小或以其他方式优化应用以便在头戴显示设备上使用。

以下是一小段代码，用于仅当应用将在混合现实设备上使用时调整 XAML TextBlock 控件内文本的大小。

```csharp

bool isViewingInMR = Windows.ApplicationModel.Preview.Holographic.HolographicApplicationPreview.IsCurrentViewPresentedOnHolographicDisplay();

            if (isViewingInMR)
            {
                // Running on headset, resize the XAML text
                textBlock.Text = "I'm running in Mixed Reality!";
                textBlock.FontSize = 32;
            }
            else
            {
                // Running on desktop
                textBlock.Text = "I'm running on the desktop.";
                textBlock.FontSize = 16;
            }

```





## <a name="related-articles"></a>相关文章


* [从 shell 中使用 API 的应用的当前限制](https://developer.microsoft.com/windows/mixed-reality/current_limitations_for_apps_using_apis_from_the_shell)
* [构建 2D 应用](https://developer.microsoft.com/windows/mixed-reality/building_2d_apps)
* [HoloLens：针对 Microsoft HoloLens 构建 UWP 2D 应用](https://channel9.msdn.com/Events/Build/2016/B854)
* [条件 XAML](https://docs.microsoft.com/en-us/windows/uwp/debug-test-perf/conditional-xaml)


