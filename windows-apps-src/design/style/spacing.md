---
title: 间距和大小
description: 新的 Fluent 标准和 Compact 控件样式确保无论何种设备和输入方法熟练的用户体验。
keywords: UWP、 Windows 10、 控件、 大小、 密度、 标准版、 compact
ms.date: 4/4/2019
ms.topic: article
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 7b74e3dc2ad047d9e52509b71ef00b829ad63a0d
ms.sourcegitcommit: 7a1d5198345d114c58287d8a047eadc4fe10f012
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/08/2019
ms.locfileid: "59249449"
---
# <a name="control-size-and-density"></a>控件的大小和密度

使用控件的大小和密度的组合来优化你的通用 Windows 平台 (UWP) 应用程序和提供最适合你的应用的功能和交互要求的用户体验。

默认情况下，UWP 应用程序都呈现具有低密度 (或`Standard`) 布局。 但是，从开始 WinUI 2.1，高密度 (或`Compact`) 布局选项的信息丰富的 UI 和类似的专用的方案，也支持。 这可以指定通过基本样式资源 （请参阅下面的示例）。

未更改时的功能和行为，并且这两个中保持不一致的大小和密度选项，默认的正文字体大小已更新为 14px 的所有控件，以支持这些两个密度选项。 此字体大小跨区域和设备，并确保应用程序保持均衡且熟练的用户。

## <a name="fluent-standard-sizing"></a>Fluent 标准调整大小

*Fluent 标准大小*旨在提供信息密度和用户舒适之间的平衡。 实际上，在屏幕上的所有项都对齐到 40 x 40 个有效像素 (epx) 目标 UI 元素与网格都对齐并进行相应调整，可以基于系统级别缩放。

**标准大小的设计可以适应触摸和输入的指针。**

> [!NOTE]
>有效像素和缩放的详细信息，请参阅[UWP 应用程序设计简介](../basics/design-and-ui-intro.md#effective-pixels-and-scaling)
>
> 系统级别缩放的详细信息，请参阅[对齐、 边距、 填充](../layout/alignment-margin-padding.md)。

适用于 Windows 10 年 10 月 2018 Update （版本 1809年）、 标准版、 所有 UWP 控件的默认大小值已小于为了提高可用性，跨所有使用方案。

下图显示了一些控件布局更改引入 windows 10 2018 年 10 月更新。 具体而言，一个标头和控件的顶部之间的边距已小于 8epx 的 4epx，和 44epx 网格已更改为 40epx 网格。

![标准控件的布局示例](images/standarddensity.png)

*标准控件的布局示例*

此下一步图显示了所做更改才能控制大小适用于 Windows 10 2018 年 10 月更新。 具体而言，对齐到 40epx 网格。

![标准命令示例](images/standarddensitycommanding.png)

## <a name="fluent-compact-sizing"></a>Fluent Compact 大小调整

Compact 调整大小启用广泛、 丰富信息的控件组，并可帮助了解以下信息：

- 浏览大量内容。
- 最大化页面上的可见内容。
- 导航和交互的控件和内容

**Compact 大小调整主要用于容纳指针输入。**

### <a name="examples"></a>示例

可通过可以在页级别或上一个特定的布局应用程序中指定的特殊资源字典实现，compact 大小调整。 资源字典现已推出[WinUI](https://docs.microsoft.com/en-us/uwp/toolkits/winui/) Nuget 包。

下面的示例演示如何将`Compact`样式可应用于页面和单个的网格控件。

#### <a name="page-level"></a>页级别

```xaml
<Page.Resources>
    <ResourceDictionary Source="ms-appx:///Microsoft.UI.Xaml/DensityStyles/Compact.xaml" />
</Page.Resources>
```

#### <a name="grid-level"></a>网格级别

```xaml
<Grid>
    <Grid.Resources>
        <ResourceDictionary Source="ms-appx:///Microsoft.UI.Xaml/DensityStyles/Compact.xaml" />
    </Grid.Resources>
</Grid>
```

## <a name="related-articles"></a>相关文章

- [触摸目标的准则](../input/guidelines-for-targeting.md)
- [ResourceDictionary 和 XAML 资源引用](https://docs.microsoft.com/en-us/windows/uwp/design/controls-and-patterns/resourcedictionary-and-xaml-resource-references)
- [资源字典](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.resourcedictionary)
- [XAML 样式](https://docs.microsoft.com/en-us/windows/uwp/design/controls-and-patterns/xaml-styles) 
