---
author: Jwmsft
Description: "带有内容区域和命令区域的单视图应用或模式体验模式，例如照片查看器/编辑器、文档查看器、地图、绘画或使用自由滚动视图的其他应用。"
title: "活动画布布局模式"
ms.assetid: 4D768472-64D6-406C-9E87-F750F6B981A0
label: TBD
template: detail.hbs
op-migration-status: ready
translationtype: Human Translation
ms.sourcegitcommit: b258771c887d4422433522344b11130b7e9ed1e6
ms.openlocfilehash: b38d7664a8a874743c5307e44e81104ce512454e

---
# <a name="active-canvas-layout-pattern"></a>活动画布布局模式

活动画布是带有内容区域和命令区域的模式。 它适用于单视图应用或模式体验，例如照片查看器/编辑器、文档查看器、地图、绘画或使用自由滚动视图的其他应用。 在操作执行方面，活动画布可以与命令栏或仅与按钮搭配使用，具体取决于用户所需的操作数和操作类型。

## <a name="examples"></a>示例

此照片编辑应用的设计主要包括一个活动画布模式，其左侧具有一个移动示例，其右侧具有一个桌面示例。 图像编辑图面是一块画布，底部的命令栏包含应用所需的所有上下文操作。

![使用活动画布模式的照片编辑器示例](images/uap-photo-pc-phone-700.png)

此地铁地图应用的设计使用了顶部具有简单 UI 带的活动画布，该 UI 带中只有两个操作和一个搜索框。 上下文操作显示在上下文菜单中，如右图所示。

![使用活动画布模式的地图应用示例](images/uap-subway-pc-phone-700.png)


## <a name="implementing-this-pattern"></a>实现此模式

活动画布模式由一个内容区域和一个命令区域组成。

**内容区域。**  内容区域通常是一块自由滚动的画布。 在一个应用中可以存在多个内容区域。

**命令区域。**  如果你要放置大量命令，可以使用可根据屏幕大小做出响应的命令栏。 如果不放置那么多命令，并且不涉及响应式 UI，则节省空间按钮也行之有效。



## <a name="related-articles"></a>相关文章

-   [**应用栏和命令栏**](../controls-and-patterns/app-bars.md)



<!--HONumber=Dec16_HO2-->


