---
Description: 选择模式允许用户选择单个或多个项并对其执行操作。
title: 选择模式
label: Selection mode
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: anawish
design-contact: kimsea
dev-contact: ranjeshj
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: d7b781e6074468fbe73446e4057e36ff31266d05
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "72165094"
---
# <a name="selection-mode-overview"></a>“选择模式”概述

选择模式允许用户选择单个或多个项并对其执行操作。 可通过上下文菜单调用此模式，方法是在项上使用 CTRL+单击或 SHIFT+单击组合键，或者在库视图中的某一项上滚动选择目标。 当选择模式处于活动状态时，每个列表项旁都将显示复选框，而操作将显示在屏幕的顶部或底部。

## <a name="different-selection-modes"></a>不同的选择模式
有以下三种选择模式：

- 单选：用户一次只能选择一个项目。
- 多选：用户无需使用修改器即可选择多个项目。
- 展开：用户可以使用修改器选择多个项目，例如长按 SHIFT 键。

## <a name="selection-mode-examples"></a>选择模式示例
### <a name="here-is-a-listview-with-single-selection-mode-enabled"></a>下面是启用了单选模式的 ListView。
![单选模式的列表视图](images/listview-selection-single.png)

“Art Venere”项处于选中状态，光标当前正悬停在“Mitsue Tollner”项上。

### <a name="here-is-a-listview-with-multiple-selection-mode-enabled"></a>下面是启用了多选模式的 ListView：
![多选模式的列表视图](images/listview-selection-multiple.png)

有三个项处于选中状态，光标正悬停在“Donette Foller”项上。

### <a name="here-is-a-gridview-with-single-selection-mode-enabled"></a>下面是启用了单选模式的 GridView：
![单选模式的网格视图](images/gridview-selection-single.png)

第 7 项处于选中状态，光标当前正悬停在第 3 项上。

### <a name="here-is-a-gridview-with-multiple-selection-mode-enabled"></a>下面是启用了多选模式的 GridView：
![多选模式的网格视图](images/gridview-selection-multiple.png)

第 2、4、5 项处于选中状态，光标当前正悬停在第 7 项上。

## <a name="behavior-and-interaction"></a>行为和交互
在项目上点击任意位置即可将其选中。 点击命令栏操作会影响所有选定项。 如果没有选择任何项目，命令栏操作应处于非活动状态，除了“全选”。

选择模式没有弱解除模型；在选择模式处于活动状态的框架之外点击不会取消该模式。 这是为了防止意外停用该模式。 单击后退按钮可关闭多选模式。

当选中某项操作时，会显示可视化确认。 请考虑为某些操作显示确认对话框，特别是诸如删除等破坏性操作。

选择模式仅限于它处于活动状态的页面，并不会影响该页面之外的任何项。

选择模式的入口点应对应于它所影响的内容。

有关命令栏建议，请参阅[命令栏指南](app-bars.md)。

若要详细了解特定控件的选择模式及其选择事件的处理方式，请访问该控件的指南页面。
