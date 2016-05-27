---
author: Jwmsft
Description: 语义式缩放控件可使用户在同一数据集的两个不同语义式视图之间缩放。
title: 语义式缩放
ms.assetid: B5C21FE7-BA83-4940-9CC1-96F6A2DC28C7
label: Semantic zoom
template: detail.hbs
---

# 语义式缩放



语义式缩放控件使用户能够在相同内容的两个不同视图之间进行缩放，以便它们可以通过大型数据集进行快速导航。 放大视图是内容的主视图。 你可以在此视图中显示完整的数据集。 缩小视图是同一内容的更高级别的视图。 通常，你会在此视图中显示分组数据集的组标头。 例如，查看通讯簿时，用户可以放大一个字母，并查看与该字母关联的名称。 

**重要的 API**

-   [**SemanticZoom 类**](https://msdn.microsoft.com/library/windows/apps/hh702601)

**功能**：

-   缩小视图的大小受语义式缩放控件的边界限制。
-   点击组标题可切换视图。 可以启用收缩作为一种在视图之间切换的方式。
-   视图之间的活动标题切换。

## 示例

在“照片”应用中使用的语义式缩放。

![在“照片”应用中使用的语义式缩放](images/control-examples/semantic-zoom-photos.png)

通讯簿是使用语义式缩放控件可更方便地导航数据集的一个示例。 全体视图是通讯簿中人员的完整、按字母顺序排列的概述（左边的图像），而放大视图按顺序显示数据并带有更详细的信息（右边的图像）。

![在联系人列表中使用的语义式缩放示例](images/semanticzoom-win10.png)

## 建议

-   在你的应用中使用语义式缩放时，请确保项目布局和平移方向不会根据缩放级别而更改。 布局和平移交互应该在各个缩放级别上保持一致且可预测。
-   语义式缩放支持用户快速跳转到内容，因此请将缩小模式中的页面/屏幕数限制为三个。 过多的平移会减少语义式缩放的实用性。
-   避免使用语义式缩放来更改内容的范围。 例如，相册不应在文件资源管理器中切换为文件夹视图。
-   使用视图中必不可少的结构和语义。
-   对分组集合中的项使用组名称。
-   对未分组但已排好序的集合使用分类次序（如按日期时间先后排序或名称列表字母排序）。



## 相关文章

* [常见用户交互指南](https://dev.windows.com/design/inputs-devices)


**示例 (XAML)**
* [输入：XAML 用户输入事件示例](http://go.microsoft.com/fwlink/p/?linkid=226855)
* [XAML 滚动、平移以及缩放示例](http://go.microsoft.com/fwlink/p/?linkid=251717)

**示例 (DirectX)**
* [DirectX 触控输入示例](http://go.microsoft.com/fwlink/p/?LinkID=231627)
* [输入：操作和手势 (C++) 示例](http://go.microsoft.com/fwlink/p/?linkid=231605)
 

 






<!--HONumber=May16_HO2-->


