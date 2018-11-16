---
author: mijacobs
Description: An overview of common page patterns and UI elements for displaying content in your UWP app.
title: 通用 Windows 平台 (UWP) 应用的内容设计基础知识
ms.assetid: 3102530A-E0D1-4C55-AEFF-99443D39D567
label: Content design basics
template: detail.hbs
op-migration-status: ready
ms.author: mijacobs
ms.date: 12/1/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1f894cbc4c5c024d1b51660c48eb749479457b81
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2018
ms.locfileid: "6986732"
---
# <a name="content-design-basics-for-uwp-apps"></a>UWP 应用的内容设计基础知识

任何应用的主要用途都是提供对内容的访问权限。 由于应用出于许多不同的用途而存在，因此内容具有多种形式：在照片编辑应用中，照片即是内容；在旅行应用中，地图和旅行目的地信息即是内容；等等。 

本文概述了可以在应用中提供内容的方式。 我们描述了可用于显示内容的常见页面模式和 UI 元素，无论内容的形式如何。

## <a name="common-page-patterns"></a>常见页面模式

许多应用使用部分或所有此类常见页面模式来显示不同类型的内容。 同样地，可以随意混合和匹配这些模式以针对应用的内容进行优化。

### <a name="landing"></a>登录

![登录页面](images/content-basics/hero-screen.png)

登录页面（也称为 hero 屏幕）通常显示在应用体验的最高层。 大型图面区域充当一个平台以供应用突出显示用户可能要浏览和使用的内容。

### <a name="collections"></a>集合

![库](images/content-basics/gridview.png)

集合可让用户浏览一组内容或数据。 [网格视图](../controls-and-patterns/item-templates-gridview.md)是适用于照片或媒体中心式内容的选项，[列表视图](../controls-and-patterns/item-templates-listview.md)是适用于以文字为主内容或数据的选项。

### <a name="hub"></a>中心

![中心](images/content-basics/hub.png)

[中心](../controls-and-patterns/hub.md)专为窗口购物而设计。 用户可以抢先了解提供的内容；这一切都是为了展示内容的多样性，并确保内容简短。 例如，中心部分 1 可包含 hero 屏幕，中心部分 2 可包含一个集合，中心部分 3 可包含另一个集合，等等。

### <a name="masterdetail"></a>大纲/细节

![大纲细节](images/content-basics/master-detail.png)

[大纲/细节](../controls-and-patterns/master-details.md)模型包含一个列表视图（大纲）和一个内容视图（细节）。 这两个窗格都是固定的，并且具有垂直滚动。 列表项和内容视图之间有明确的关系：在大纲视图中选择项目后，会相应地更新明细视图。 除了提供明细视图导航之外，还可以添加和删除大纲视图中的项目。

### <a name="details"></a>详细信息

![多个视图](images/multi-view.png)

当用户找到他们要查找的内容时，考虑创建专用内容查看页面，以便用户能够不受干扰地查看页面。 如果可能，[创建一个全屏视图选项](../layout/show-multiple-views.md)，用于扩展内容以填充整个屏幕并隐藏其他所有 UI 元素。 

要针对屏幕大小变化进行调整，也请考虑创建一个[响应式设计](design-and-ui-intro.md)，用于相应地隐藏/显示 UI 元素。

### <a name="forms"></a>表单
![表单](images/content-basics/forms.png)

[表单](../controls-and-patterns/forms.md)是一组控件，用于收集和提交来自用户的数据。 大多数（而非全部）应用将某种形式的表单用于设置页面、登录门户、反馈中心、账户创建或其他目的。 

## <a name="common-content-elements"></a>常见内容元素

要创建这些页面模式，你将需要使用单个内容元素的组合。 以下是常用于显示内容的一些 UI 元素。 有关 UI 元素的完整列表，请参阅[控件和模式](../controls-and-patterns/index.md)。

<div class="mx-responsive-img">
<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">类别</th>
<th align="left">元素</th>
<th align="left">说明</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">音频和视频<br/><br/>
    <img src="images/content-basics/media-transport.png" alt="media transport control" /></td>
<td align="left"><a href="../controls-and-patterns/media-playback.md">媒体播放和传输控件</a></td>
<td align="left">播放音频和视频。</td>
</tr>
<tr class="even">
<td align="left">图像查看器<br/><br/>
    <img src="images/content-basics/flipview.jpg" alt="flip view" /></td>
<td align="left"><a href="../controls-and-patterns/flipview.md">翻转视图</a>、<a href="../controls-and-patterns/images-imagebrushes.md">图像</a></td>
<td align="left">显示图像。 翻转视图用于显示集锦中的图像（例如相册中的照片或产品详细信息页中的项目），一次显示一张图像。</td>
</tr>
<tr class="odd">
<td align="left">集合 <br/><br/>
    <img src="images/content-basics/listview.png" alt="list view" /></td>
<td align="left"><a href="../controls-and-patterns/lists.md">列表视图和网格视图</a></td>
<td align="left">显示交互式列表或网格中的项。 使用这些元素以使用户可以从新发行电影列表中选择一部电影或管理清单。</td>
</tr>
<tr class="even">
<td align="left">文本和文本输入 <br/><br/>
    <img src="images/content-basics/textbox.png" alt="text box" /></td>
<td align="left"><p><a href="../controls-and-patterns/text-block.md">文本块</a>、<a href="../controls-and-patterns/text-box.md">文本框</a>、<a href="../controls-and-patterns/rich-edit-box.md">可编辑对话框</a></p>
</td>
<td align="left">显示文本。 某些元素使用户能够编辑文本。 有关详细信息，请参阅<a href="../controls-and-patterns/text-controls.md">文本控件</a>。
<p>有关如何显示文本的指南，请参阅[版式](../style/typography.md)。</p>
</td>
</tr>
<tr class="odd">
<td align="left">地图<br/><br/>
    <img src="images/content-basics/mapcontrol.png" alt="map control" /></td>
<td align="left"><a href="../../maps-and-location/display-maps.md">MapControl</a></td>
<td align="left">显示地球的符号或逼真映射。</td>
</tr>
<tr class="even">
<td align="left">WebView</td>
<td align="left"><a href="../controls-and-patterns/web-view.md">WebView</a></td>
<td align="left">呈现 HTML 内容。</td>
</tr>
</tbody>
</table>
</div>
