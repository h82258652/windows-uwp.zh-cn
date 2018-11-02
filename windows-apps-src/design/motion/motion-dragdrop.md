---
author: mijacobs
Description: Use drag-and-drop animations when users move objects, such as moving an item within a list, or dropping an item on top of another.
title: 在 UWP 应用中拖动动画
ms.assetid: 6064755F-6E24-4901-A4FF-263F05F0DFD6
label: Motion--Drag and drop
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10，uwp
ms.localizationpriority: medium
ms.openlocfilehash: d271d0b9c8e7ce73835457789aca3fa2cb5eda97
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/02/2018
ms.locfileid: "5974317"
---
# <a name="drag-animations"></a>拖动动画




当用户移动对象时，使用拖放动画，例如在列表中移动项目，或将项目放在其他列表顶部。

> **重要 API**：[**DragItemThemeAnimation 类**](https://msdn.microsoft.com/library/windows/apps/br243174)


## <a name="dos-and-donts"></a>应做事项和禁止事项


**拖动开始动画**

-   在用户开始移动对象时使用拖动开始动画。
-   当且仅当有其他对象可能受到拖放操作影响时，才在动画中包括这些受影响的对象。
-   使用拖动结束动画可完成以拖动开始动画开始的任何动画序列。 它将恢复由拖动开始动画引起的拖动对象的大小更改。

**拖动结束动画**

-   在用户放下拖动的对象时使用拖动结束动画。
-   将拖动结束动画与列表的添加和删除动画结合使用。
-   当且仅当拖动开始动画中包括了受影响的对象时，才应在拖动结束动画中包括这些受影响的对象。
-   如果没有首先使用拖动开始动画，则不要使用拖动结束动画。 你需要使用这两个动画，才能在完成拖动序列之后，让对象还原为其原始大小。

**在进入动画间拖动**

-   当用户将拖动源拖入可以放在两个其他对象之间的放置区域时，使用在进入间拖动动画。
-   选择一个合理的目标拖放区域。 此区域不应该太小，否则用户难以放置拖放源。
-   移动受影响的对象以显示放置区域的建议方向是直接相互分开。 垂直移动还是水平移动取决于受影响的对象彼此的方向。
-   如果拖放源无法放入某个区域，请不要使用在进入间拖动动画。 在进入间拖动动画告诉用户可以将拖动源放入受影响的对象之间。

**在离开动画间拖动**

-   当用户将对象从放置它的两个其他对象之间的区域拖开时，使用在离开间拖动动画。
-   如果没有首先使用在进入间拖动动画，则不要使用在离开间拖动动画。


## <a name="related-articles"></a>相关文章

**对于开发人员**
* [动画概述](https://msdn.microsoft.com/library/windows/apps/mt187350)
* [创建拖放顺序动画](https://msdn.microsoft.com/library/windows/apps/xaml/jj649427)
* [快速入门：使用库动画创建 UI 动画](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)
* [**DragItemThemeAnimation 类**](https://msdn.microsoft.com/library/windows/apps/br243174)
* [**DropTargetItemThemeAnimation 类**](https://msdn.microsoft.com/library/windows/apps/br243186)
* [**DragOverThemeAnimation 类**](https://msdn.microsoft.com/library/windows/apps/br243180)


 




