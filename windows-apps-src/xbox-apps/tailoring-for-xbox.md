---
title: Xbox 最佳做法
description: 如何针对 Xbox 优化你的应用程序。
ms.date: 10/12/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6e64feb8938be3e7338c87acdf8fd18fb13e525b
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2018
ms.locfileid: "8890026"
---
# <a name="xbox-best-practices"></a>Xbox 最佳做法

默认情况下，所有 UWP 应用都将在 Xbox One 上运行，你无需执行任何额外操作。 但是，如果想要你的应用大放异彩、吸引客户并在 Xbox 最佳应用体验方面具有竞争力，你应该遵循以下做法。
  > [!NOTE]
  > 在开始之前，查看[设计 Xbox 和电视](../design/devices/designing-for-tv.md)中提供的设计指南。   

## <a name="to-build-the-best-experiences-for-xbox-one"></a>生成 Xbox One 的最佳做法

### <a name="do-turn-off-mouse-mode"></a>*应做事项：* 关闭鼠标模式

Xbox 用户喜欢其控制器。 若要优化控制器输入，[禁用鼠标模式](how-to-disable-mouse-mode.md)，并启用方向导航（也称为 [X-Y 焦点](../design/devices/designing-for-tv.md#xy-focus-navigation-and-interaction)）。 请注意焦点陷阱和不可访问的 UI。

### <a name="do-draw-a-focus-rectangle-that-is-appropriate-for-a-10-foot-experience"></a>*应做事项：* 绘制适用于 10 英尺体验的焦点矩形

大多数 Xbox 用户面向电视坐在客厅中，因此请记住，标准焦点矩形难以在十英尺远的距离看到屏幕。 若要确保用户始终可以清楚地看到具有输入焦点的 UI 元素，请遵循[焦点视觉对象](../design/devices/designing-for-tv.md#focus-visual)指南。 在 XAML 中，当你的应用在 Xbox 上运行时，将免费获取此行为，但 HTML 应用需要使用自定义 CSS 样式。

### <a name="do-integrate-with-the-systemmediatransportcontrols-class"></a>*应做事项：* 与 SystemMediaTransportControls 类集成

Xbox 用户想要使用 Xbox 媒体遥控器、Cortana（尤其是“播放”和“暂停”语音命令）和 Xbox SmartGlass 控制媒体应用。 若要免费获取这些功能，你的应用应使用 [SystemMediaTransportControls](https://msdn.microsoft.com/library/windows/apps/windows.media.systemmediatransportcontrols.aspx) 类，该类将自动包含在 Xbox 媒体控件中。 如果你的应用具有自定义媒体控件，请确保与 **SystemMediaTransportControls** 类集成，以向用户提供这些功能。 如果你要创建背景音乐应用，与 **SystemMediaTransportControls** 类集成，确保背景音乐控件在 Xbox 多任务选项卡中正常工作。

<!-- ### *Do:* Use adaptive UI to account for snapped apps
One of the unique features of Xbox One is that users can snap apps such as Cortana next to any other app, so your app should respond gracefully when it runs in *fill mode*. Implement [adaptive UI](../get-started/universal-application-platform-guide.md#design-adaptive-ui-with-adaptive-panels) and make sure to test your app during development by snapping an app next to it. -->

### <a name="consider-draw-to-the-edge-of-the-screen"></a>*注意事项：* 绘制到屏幕的边缘

许多电视会截断屏幕的边缘，因此你的应用的所有重要内容都应显示在[电视安全区域](../design/devices/designing-for-tv.md#tv-safe-area)内。 UWP 使用*过度扫描*使该内容保持在电视安全区域内，但此默认行为可能会围绕你的应用绘制一个明显边框。 若要提供最佳做法，关闭默认行为，然后按照[如何将 UI 绘制到屏幕的边缘](turn-off-overscan.md)中的说明操作。
> [!IMPORTANT]
  > 如果禁用过度扫描，你有责任确保交互式元素和文本保留在电视安全区域内。 

### <a name="consider-use-tv-safe-colors"></a>*注意事项：* 使用电视安全颜色

电视不会处理严重的颜色浓度，但计算机监视器会处理。 避免在应用中使用高浓度颜色，以免用户看到奇怪的带状效果或褪色的图像。 另外，请注意，电视间的差异是指在*你的*电视上显示良好的颜色可能不适用于用户。 读取[颜色](../design/devices/designing-for-tv.md#colors)以了解如何使你良好地适应每个人的应用 ！

### <a name="remember-you-can-disable-scaling"></a>*记住：* 可以禁用缩放

UWP 应用将自动缩放，以确保 UI 元素（如控件和字体）在所有设备上都符合条件。 使用 XAML 的应用按 200% 缩放，使用 HTML 的应用按 150% 缩放。 如果你希望更好地控制你的应用在 Xbox 上的外观，禁用默认的比例系数以使用 HDTV (1920x1080) 的实际像素尺寸。 查看[如何关闭缩放](disable-scaling.md)和[有效像素和缩放](../design/basics/design-and-ui-intro.md#effective-pixels-and-scaling)，获取有关定制你的应用以在 Xbox 上良好显示的信息。

如果你想要了解应用于 UWP 应用的一些做法，请观看此视频！
</br>
</br>
<iframe src="https://channel9.msdn.com/Blogs/One-Dev-Minute/Tailoring-your-UWP-app-for-Xbox/player" width="960" height="540" allowFullScreen frameBorder="0"></iframe>

## <a name="channel-9"></a>第 9 频道

有关在 Xbox 上生成出色应用的信息，请参阅[第 9 频道](https://channel9.msdn.com/)上的以下讨论：

- [生成适用于 Xbox 的出色通用 Windows 平台 (UWP) 应用](https://channel9.msdn.com/Events/Build/2016/B883)
- [调整你的适用于 Xbox One 和电视的应用](https://channel9.msdn.com/Events/Build/2016/T651-R1)
- [UWP 开发 1：生成自适应 UI](https://channel9.msdn.com/Events/Build/2016/L724-R1)
- [浏览器外的 Web 应用：跨平台与跨设备](https://channel9.msdn.com/Events/Build/2016/B888)

## <a name="app-dev-on-xbox"></a>在 Xbox 上的应用开发人员

**在 Xbox 上的应用开发人员**事件是很好的起点，适用于开发人员熟悉 Xbox 上生成应用。

* [观看的会话记录](https://developer.microsoft.com/windows/projects/campaigns/app-dev-on-xbox-event#WatchNow)
* [阅读博客文章](https://developer.microsoft.com/windows/projects/campaigns/app-dev-on-xbox-event#BlogSeries)

## <a name="see-also"></a>另请参阅

- [Xbox One 上的 UWP](index.md)
- [针对 Xbox 和电视进行设计](../design/devices/designing-for-tv.md)
