---
author: payzer
title: "Xbox 最佳做法"
description: "如何针对 Xbox 优化你的应用程序。"
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 0cfa8e22-7345-47b7-b132-880bbc050d44
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: a66e94ca26a089ffd08b0ba7a4ffb42aa5d8685c
ms.lasthandoff: 02/08/2017

---

# <a name="xbox-best-practices"></a>Xbox 最佳做法
默认情况下，所有 UWP 应用都将在 Xbox One 上运行，你无需执行任何额外操作。 但是，如果想要你的应用大放异彩、吸引客户并在 Xbox 最佳应用体验方面具有竞争力，你应该遵循以下做法。
  > [!NOTE]
  > 在开始之前，查看[设计 Xbox 和电视](../input-and-devices/designing-for-tv.md)中提供的设计指南。   

## <a name="to-build-the-best-experiences-for-xbox-one"></a>生成 Xbox One 的最佳做法

### <a name="do-turn-off-mouse-mode"></a>*应做事项：*关闭鼠标模式
Xbox 用户喜欢其控制器。 若要优化控制器输入，[禁用鼠标模式](how-to-disable-mouse-mode.md)，并启用方向导航（也称为 [X-Y 焦点](../input-and-devices/designing-for-tv.md#xy-focus-navigation-and-interaction)）。 密切注意焦点陷阱和不可访问的 UI。

### <a name="do-draw-a-focus-rectangle-that-is-appropriate-for-a-10-foot-experience"></a>*应做事项：*绘制适用于 10 英尺体验的焦点矩形
大多数 Xbox 用户面向电视坐在客厅中，因此请记住，标准焦点矩形难以在十英尺远的距离看到屏幕。 若要确保用户始终可以清楚地看到具有输入焦点的 UI 元素，请遵循[焦点视觉对象](../input-and-devices/designing-for-tv.md#focus-visual)指南。 在 XAML 中，当你的应用在 Xbox 上运行时，将免费获取此行为，但 HTML 应用需要使用自定义 CSS 样式。

###    <a name="do-integrate-with-the-systemmediatransportcontrols-class"></a>*应做事项：*与 SystemMediaTransportControls 类集成 
Xbox 用户想要使用 Xbox 媒体遥控器、Cortana（尤其是“播放”和“暂停”语音命令）和 Xbox SmartGlass 控制媒体应用。 若要免费获取这些功能，你的应用应使用 [SystemMediaTransportControls](https://msdn.microsoft.com/en-us/library/windows/apps/windows.media.systemmediatransportcontrols.aspx) 类，该类将自动包含在 Xbox 媒体控件中。 如果你的应用具有自定义媒体控件，请确保与 **SystemMediaTransportControls** 类集成，以向用户提供这些功能。 如果你要创建背景音乐应用，与 **SystemMediaTransportControls** 类集成，确保背景音乐控件在 Xbox 多任务选项卡中正常工作。

### <a name="do-use-adaptive-ui-to-account-for-snapped-apps"></a>*应做事项：*使用自适应 UI 解释贴靠的应用
Xbox One 的一个独特功能是用户可以将应用（如 Cortana）贴靠到任何其他应用旁边，因此你的应用应在*填充模式*下运行时流畅响应。 实现[自适应 UI](../get-started/universal-application-platform-guide.md#design-adaptive-ui-with-adaptive-panels)，并确保在开发期间通过在你的应用旁边贴靠某个应用测试你的应用。

### <a name="consider-draw-to-the-edge-of-the-screen"></a>*注意事项：*绘制到屏幕的边缘
许多电视会截断屏幕的边缘，因此你的应用的所有重要内容都应显示在[电视安全区域](../input-and-devices/designing-for-tv.md#tv-safe-area)内。 UWP 使用*过度扫描*使该内容保持在电视安全区域内，但此默认行为可能会围绕你的应用绘制一个明显边框。 若要提供最佳做法，关闭默认行为，然后按照[如何将 UI 绘制到屏幕的边缘](turn-off-overscan.md)中的说明操作。
> [!IMPORTANT]
  > 如果禁用过度扫描，你有责任确保交互式元素和文本保留在电视安全区域内。 

###    <a name="consider-use-tv-safe-colors"></a>*注意事项：*使用电视安全颜色 
电视不会处理严重的颜色浓度，但计算机监视器会处理。 避免在应用中使用高浓度颜色，以免用户看到奇怪的带状效果或褪色的图像。 另外，请注意，电视间的差异是指在*你的*电视上显示良好的颜色可能不适用于用户。 若要了解如何使你的应用良好地适应每个人，请阅读[电视颜色](../input-and-devices/designing-for-tv.md#colors)！

### <a name="remember-you-can-disable-scaling"></a>*记住：*可以禁用缩放
UWP 应用将自动缩放，以确保 UI 元素（如控件和字体）在所有设备上都符合条件。 使用 XAML 的应用按 200% 缩放，使用 HTML 的应用按 150% 缩放。 如果你希望更好地控制你的应用在 Xbox 上的外观，禁用默认的比例系数以使用 HDTV (1920x1080) 的实际像素尺寸。 查看[如何关闭缩放](disable-scaling.md)和[有效像素和缩放](../layout/design-and-ui-intro.md#effective-pixels-and-scaling)，获取有关定制你的应用以在 Xbox 上良好显示的信息。

## <a name="channel-9"></a>第 9 频道
有关在 Xbox 上生成出色应用的信息，请参阅[第 9 频道](https://channel9.msdn.com/)上的以下讨论：

- [生成适用于 Xbox 的出色通用 Windows 平台 (UWP) 应用](https://channel9.msdn.com/Events/Build/2016/B883)
- [调整你的适用于 Xbox One 和电视的应用](https://channel9.msdn.com/Events/Build/2016/T651-R1)
- [UWP 开发 1：生成自适应 UI](https://channel9.msdn.com/Events/Build/2016/L724-R1)
- [浏览器外的 Web 应用：跨平台与跨设备](https://channel9.msdn.com/Events/Build/2016/B888)


## <a name="see-also"></a>另请参阅
- [Xbox One 上的 UWP](index.md)


