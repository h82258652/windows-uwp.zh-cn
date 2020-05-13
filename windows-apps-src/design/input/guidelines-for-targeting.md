---
Description: 本主题介绍使用接触几何图形来确定触摸目标并提供在 Windows 运行时应用中确定目标的最佳实践。
title: 目标设定
ms.assetid: 93ad2232-97f3-42f5-9e45-3fc2143ac4d2
label: Targeting
template: detail.hbs
ms.date: 03/18/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 13737e9aeb906e0430b2a18dd75ae1fce3a75956
ms.sourcegitcommit: 87fd0ec1e706a460832b67f936a3014f0877a88c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/12/2020
ms.locfileid: "83234588"
---
# <a name="guidelines-for-touch-targets"></a>触摸目标准则

无论设备类型或输入方法是什么，Windows 应用程序中的所有交互式 UI 元素都必须足够大，以便用户能够准确地访问和使用。

支持触摸输入（以及触摸联系人区域相对不精确的性质）需要进一步优化目标大小和控件布局，因为触控数字化器报告的较大、更复杂的输入数据集用于确定用户的预期（或最可能的）目标。

所有 UWP 控件都是使用默认的触摸目标大小和布局设计的，使你能够构建视觉上的、易于使用的、易于使用和信心百倍的应用。

在本主题中，我们将介绍这些默认行为，以便可以使用平台控件和自定义控件（应用程序是否需要）来设计应用程序的最大可用性。

> **重要 API**：[**Windows.UI.Core**](https://docs.microsoft.com/uwp/api/Windows.UI.Core)、[**Windows.UI.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Input)、[**Windows.UI.Xaml.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input)

## <a name="fluent-standard-sizing"></a>Fluent Standard 大小

创建 Fluent Standard 大小  是为了在信息密度与用户舒适之间提供平衡。 实际上，屏幕上的所有项都对齐到 40x40 有效像素 (epx) 目标，这使 UI 元素可与网格对齐并基于系统级别缩放进行相应缩放。

> [!NOTE]
> 有关有效像素和缩放的更多信息，请参阅 [Windows 应用设计简介](../basics/design-and-ui-intro.md#effective-pixels-and-scaling)
>
> 有关系统级别缩放的更多信息，请参阅[对齐、边距和填充](../layout/alignment-margin-padding.md)。

## <a name="fluent-compact-sizing"></a>Fluent Compact 大小

应用程序可以通过*简洁的精简大小*显示更高级别的信息密度。 精简调整大小将 UI 元素对齐到 32x32 window.epx.codesnippet 目标，这允许 UI 元素与更紧密的网格对齐，并根据系统级缩放适当地缩放。

### <a name="examples"></a>示例

可以在页面或网格级别应用精简大小调整。

### <a name="page-level"></a>页面级别

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

通常情况下，将 "触摸目标大小" 设置为 7.5 mm 正方形范围（135 PPI 显示屏上的40x40 像素，以 1.0 x 缩放达到平稳）。 通常，UWP 控件与 7.5 mm touch 目标保持一致（这可能因特定控件和任何常见使用模式而异）。 有关更多详细信息，请参阅[控件大小和密度](../style/spacing.md)。

可以根据你的特定方案的需要调整这些目标大小的建议。 下面是需要注意的事项：

- 接触频率-考虑将重复或频繁按下的目标设为大于最小大小。
- 错误结果-如果错误发生的情况下出现严重后果，则目标应具有更大的填充量，并在内容区域的边缘进一步放置。 尤其是经常触摸的目标更是如此。
- 在内容区域中的位置。
- 外形规格和屏幕大小。
- Finger 状态。
- 触摸可视化效果。

## <a name="related-articles"></a>相关文章

- [Windows 应用设计简介](../basics/design-and-ui-intro.md)
- [控件大小和密度](../style/spacing.md)
- [对齐、边距和填充](../layout/alignment-margin-padding.md)

### <a name="samples"></a>示例

- [基本输入示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
- [低延迟输入示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
- [用户交互模式示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
- [焦点视觉对象示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)

### <a name="archive-samples"></a>存档示例

- [输入：XAML 用户输入事件示例](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20XAML%20user%20input%20events%20sample)
- [输入：设备功能示例](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Input%20Device%20capabilities%20sample%20(Windows%208))
- [输入：触摸点击测试示例](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20desktop%20samples/%5BC%2B%2B%5D-Windows%208%20desktop%20samples/C%2B%2B/Windows%208%20desktop%20samples/Input%20Touch%20hit%20testing%20sample)
- [XAML 滚动、平移和缩放示例](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Universal%20Windows%20app%20samples/111487-Universal%20Windows%20app%20samples/XAML%20scrolling%2C%20panning%2C%20and%20zooming%20sample)
- [输入：简化的墨迹示例](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Simplified%20ink%20sample)
- [输入：Windows 8 手势示例](https://docs.microsoft.com/samples/browse/?redirectedfrom=MSDN-samples)
* [输入：操作和笔势示例](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Gestures%20and%20manipulations%20with%20GestureRecognizer)
- [DirectX 触控输入示例](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%2B%2B%5D-Windows%208%20app%20samples/C%2B%2B/Windows%208%20app%20samples/DirectX%20touch%20input%20sample%20(Windows%208))
