---
title: 间距和大小
description: 新的 Fluent Standard 和 Compact 控件样式可确保获得舒适的用户体验（无论是何种设备和输入方法）。
keywords: UWP, Windows 10, 控件, 大小, 密度, standard, compact
ms.date: 04/19/2019
ms.topic: article
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 1c535e4ea4ad3c93acb048de2050d5ae7a9c2c2b
ms.sourcegitcommit: bf95c8b29145a224957a940512394e6aa97cb90f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/17/2019
ms.locfileid: "71061936"
---
# <a name="control-size-and-density"></a>控件大小和密度

使用控件大小和密度的组合可优化通用 Windows 平台 (UWP) 应用程序，并提供最适合于应用功能和交互要求的用户体验。

默认情况下，UWP 应用使用低密度（或`Standard`）布局进行呈现。 但是从 WinUI 2.1 开始，也支持适用于信息丰富的 UI 和类似专用方案的高密度（或 `Compact`）布局选项。 这可以通过基本样式资源进行指定（请参阅以下示例）。

虽然功能和行为没有变化，在两个大小和密度选项间保持一致，但是默认正文字号已对所有控件更新为 14px，以支持这两个密度选项。 此字号可跨区域和设备正常工作，确保应用程序对用户保持平衡和舒适。

## <a name="examples"></a>示例

<table>
<th align="left">XAML 控件库<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果已安装 <strong style="font-weight: semi-bold">XAML 控件库</strong>应用，请单击此处<a href="xamlcontrolsgallery:/item/Compact Sizing">打开此应用，了解 Compact 大小的实际应用</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="fluent-standard-sizing"></a>Fluent Standard 大小

创建 Fluent Standard 大小  是为了在信息密度与用户舒适之间提供平衡。 实际上，屏幕上的所有项都对齐到 40x40 有效像素 (epx) 目标，这使 UI 元素可与网格对齐并基于系统级别缩放进行相应缩放。

Standard 大小旨在适应触控和指针输入。 

> [!NOTE]
>有关有效像素和缩放的更多信息，请参阅 [UWP 应用设计简介](../basics/design-and-ui-intro.md#effective-pixels-and-scaling)
>
> 有关系统级别缩放的更多信息，请参阅[对齐、边距和填充](../layout/alignment-margin-padding.md)。

对于 Windows 10 2018 年 10 月更新（版本 1809），所有 UWP 控件的标准默认大小都已减小，以提高在所有使用方案间的可用性。

下图显示随 Windows 10 2018 年 10 月更新引入的一些控件布局更改。 具体而言，标题与控件顶部之间的边距从 8epx 减小到 4epx，44epx 网格更改为 40epx 网格。

![标准控件布局示例](images/standarddensity.png)

标准控件布局示例 

这下一个图显示 Windows 10 2018 年 10 月更新的控件大小更改。 具体而言，对齐到 40epx 网格。

![标准命令示例](images/standarddensitycommanding.png)

## <a name="fluent-compact-sizing"></a>Fluent Compact 大小

Compact 大小可实现信息丰富的密集控件组，并且可与帮助提供以下功能：

- 浏览大量内容。
- 最大化页面上的可见内容。
- 对控件和内容进行导航和交互

Compact 大小主要用于适应指针输入。 

### <a name="examples"></a>示例

Compact 大小通过可以在页面级别或特定布局上在应用程序中指定的特殊资源字典来实现。 [WinUI](https://docs.microsoft.com/en-us/uwp/toolkits/winui/) Nuget 包中提供了资源字典。

以下示例演示如何将 `Compact` 样式应用于页面和单个网格控件。

#### <a name="page-level"></a>页面级别

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

## <a name="get-the-sample-code"></a>获取示例代码

- [XAML 控件库示例](https://github.com/Microsoft/Xaml-Controls-Gallery) - 以交互式格式查看所有 XAML 控件。

## <a name="related-articles"></a>相关文章

- [触摸目标准则](../input/guidelines-for-targeting.md)
- [ResourceDictionary 和 XAML 资源引用](https://docs.microsoft.com/en-us/windows/uwp/design/controls-and-patterns/resourcedictionary-and-xaml-resource-references)
- [资源字典](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.resourcedictionary)
- [XAML 样式](https://docs.microsoft.com/en-us/windows/uwp/design/controls-and-patterns/xaml-styles) 
