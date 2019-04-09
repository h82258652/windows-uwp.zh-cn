---
Description: 本主题介绍使用接触几何图形来确定触摸目标并提供在 Windows 运行时应用中确定目标的最佳实践。
title: 定位
ms.assetid: 93ad2232-97f3-42f5-9e45-3fc2143ac4d2
label: Targeting
template: detail.hbs
ms.date: 03/18/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5c05b6686d31606a9510b1433339dc8829a52893
ms.sourcegitcommit: 7a1d5198345d114c58287d8a047eadc4fe10f012
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/08/2019
ms.locfileid: "59247175"
---
# <a name="guidelines-for-touch-targets"></a>触摸目标的准则

在通用 Windows 平台 (UWP) 应用程序中的所有交互用户界面元素必须足够大，以便用户能够准确地访问和使用，无论何种设备类型或输入方法。

支持触摸输入 （和触控接触区域的相对不精确的性质） 需要目标大小和控制布局方面进一步优化，因为更大、 更复杂的报告的触控数字化器的输入数据集用于确定用户的预期 （或最可能的） 目标。

所有 UWP 控件在设计时默认触摸目标大小和布局，您可以构建视觉平衡且具有吸引力的应用程序的舒适、 易于使用，和置信度。

在本主题中，我们将介绍这些默认行为，以便您可以设计您的应用程序的使用平台控件和自定义控件 （如果您的应用程序需要） 的最大可用性。

> **重要的 API**：[**Windows.UI.Core**](https://msdn.microsoft.com/library/windows/apps/br208383)， [ **Windows.UI.Input**](https://msdn.microsoft.com/library/windows/apps/br242084)， [ **Windows.UI.Xaml.Input**](https://msdn.microsoft.com/library/windows/apps/br227994)

## <a name="fluent-standard-sizing"></a>Fluent 标准调整大小

*Fluent 标准大小*旨在提供信息密度和用户舒适之间的平衡。 实际上，在屏幕上的所有项都对齐到 40 x 40 个有效像素 (epx) 目标 UI 元素与网格都对齐并进行相应调整，可以基于系统级别缩放。

> [!NOTE]
>有效像素和缩放的详细信息，请参阅[UWP 应用程序设计简介](../basics/design-and-ui-intro.md#effective-pixels-and-scaling)
>
> 系统级别缩放的详细信息，请参阅[对齐、 边距、 填充](../layout/alignment-margin-padding.md)。

## <a name="fluent-compact-sizing"></a>Fluent Compact 大小调整

应用程序可显示的信息密度并更高级别的*Fluent Compact 大小调整*。 Compact 大小调整将对齐到 32 x 32 epx 目标，它允许进行更紧密的网格并相应地基于系统级别缩放进行缩放的 UI 元素的 UI 元素。

### <a name="examples"></a>示例

可以在页面或网格级别应用 compact 大小调整。

### <a name="page-level"></a>页级别

```xaml
<Page.Resources>
    <ResourceDictionary Source="ms-appx:///Microsoft.UI.Xaml/DensityStyles/Compact.xaml" />
</Page.Resources>
```

### <a name="grid-level"></a>网格级别

```xaml
<Grid>
    <Grid.Resources>
        <ResourceDictionary Source="ms-appx:///Microsoft.UI.Xaml/DensityStyles/Compact.xaml" />
    </Grid.Resources>
</Grid>
```

## <a name="target-size"></a>目标大小

一般情况下，将在触摸目标大小设置为 7.5 mm 方形范围 （135 PPI 显示在 x 缩放停滞 1.0 40 x 40 像素）。 通常情况下，UWP 控件符合 7.5 mm 触摸目标 （这可以因特定的控件和任何常见使用模式）。 请参阅[控制大小和密度](../style/spacing.md)以了解详细信息。

可以根据你的特定方案的需要调整这些目标大小的建议。 下面是一些要考虑的事项：

- 频率的收尾工作了-考虑进行重复或经常按下大于最小大小的目标。
- 错误结果-如果触摸在错误导致严重后果的目标应具有更高的填充和放置最荒谬不过的内容区域的边缘。 尤其是经常触摸的目标更是如此。
- 内容区域中的位置。
- 窗体身份和屏幕大小。
- 手指状况。
- 触摸可视化效果。

## <a name="related-articles"></a>相关文章

- [UWP 应用设计简介](../basics/design-and-ui-intro.md)
- [控件的大小和密度](../style/spacing.md)
- [对齐、边距和填充](../layout/alignment-margin-padding.md)

### <a name="samples"></a>示例

- [基本输入示例](https://go.microsoft.com/fwlink/p/?LinkID=620302)
- [低延迟输入示例](https://go.microsoft.com/fwlink/p/?LinkID=620304)
- [用户交互模式示例](https://go.microsoft.com/fwlink/p/?LinkID=619894)
- [焦点视觉示例](https://go.microsoft.com/fwlink/p/?LinkID=619895)

### <a name="archive-samples"></a>存档示例

- [输入：XAML 用户输入的事件示例](https://go.microsoft.com/fwlink/p/?linkid=226855)
- [输入：设备功能示例](https://go.microsoft.com/fwlink/p/?linkid=231530)
- [输入：触控命中测试示例](https://go.microsoft.com/fwlink/p/?linkid=231590)
- [XAML 滚动、平移以及缩放示例](https://go.microsoft.com/fwlink/p/?linkid=251717)
- [输入：简化的墨迹示例](https://go.microsoft.com/fwlink/p/?linkid=246570)
- [输入：Windows 8 手势示例](https://go.microsoft.com/fwlink/p/?LinkId=264995)
- [输入：操作和手势 (C++) 示例](https://go.microsoft.com/fwlink/p/?linkid=231605)
- [DirectX 触控输入示例](https://go.microsoft.com/fwlink/p/?LinkID=231627)
