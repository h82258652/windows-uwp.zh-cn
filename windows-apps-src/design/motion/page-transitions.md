---
author: serenaz
Description: Learn how to use page transitions in your UWP apps.
title: UWP 应用中的页面过渡
template: detail.hbs
ms.author: sezhen
ms.date: 04/08/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: stmoy
ms.localizationpriority: medium
ms.openlocfilehash: dc42199eba00071f5dbabd83a4ae524298a619ee
ms.sourcegitcommit: 91511d2d1dc8ab74b566aaeab3ef2139e7ed4945
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/30/2018
ms.locfileid: "1818241"
---
# <a name="page-transitions"></a>页面过渡

页面过渡是在用户在应用中的各个页面之间导航时播放的动画，提供了作为页面之间的关系的反馈。 页面过渡可帮助用户了解他们是否处于导航层次结构的顶部、在同级页面之间移动或导航到页面层次结构的更深层。

为应用、*页面刷新* 和*钻取* 内的页面之间的导航提供了两个不同的动画，由 [NavigationTransitionInfo](/uwp/api/windows.ui.xaml.media.animation.navigationtransitioninfo) 的子类表示。

## <a name="page-refresh"></a>页面刷新

页面刷新是幻灯片动画与传入内容的动画中的淡化效果的组合。 所需的感觉是用户已重新开始。

请在用户转到导航堆栈的顶部时使用页面刷新，例如[选项卡](../controls-and-patterns/tabs-pivot.md)或[左侧导航栏](../controls-and-patterns/navigationview.md)项目之间的导航。 默认情况下，[Frame.Navigate()](/uwp/api/windows.ui.xaml.controls.frame.navigate) 使用页面刷新。

![页面刷新动画](images/page-refresh.gif)

页面刷新动画由 [EntranceNavigationTransitionInfoClass](/uwp/api/windows.ui.xaml.media.animation.entrancenavigationtransitioninfo) 表示。

```csharp
// Explicitly play the page refresh animation
myFrame.Navigate(typeof(Page2), null, new EntranceNavigationTransitionInfo());
```

## <a name="drill"></a>钻取

请在用户导航到应用的更深层时使用钻取，例如在选择一个项目后显示更多信息。

所需的感觉是用户已进入应用的更深层。

![钻取动画](images/drill.gif)

钻取动画由 [DrillInNavigationTransitionInfo](/uwp/api/windows.ui.xaml.media.animation.drillinnavigationtransitioninfo) 类表示。

```csharp
// Play the drill in animation
myFrame.Navigate(typeof(Page2), null, new DrillInNavigationTransitionInfo());
```

## <a name="suppress"></a>禁止显示

如果你要使用[连接的动画](connected-animation.md)构建自己的过渡或显示/隐藏动画，那么禁止显示动画很有用。

要避免在导航期间播放任何动画，请使用位于其他 [NavigationTransitionInfo](/uwp/api/windows.ui.xaml.media.animation.navigationtransitioninfo) 子类型的位置的 [SuppressNavigationTransitionInfo](/uwp/api/windows.ui.xaml.media.animation.suppressnavigationtransitioninfo)。

```csharp
// Suppress the default animation
myFrame.Navigate(typeof(Page2), null, new SuppressNavigationTransitionInfo());
```

## <a name="backwards-navigation"></a>向后导航

默认情况下，[Frame.GoBack()](/uwp/api/windows.ui.xaml.controls.frame.goback) 基于为导航到页面播放的动画来播放对应的“回退”动画。 例如，使用钻入来导航到页面中的应用将在用户向后导航时看到钻出。

要在向后导航时播放特定过渡，请使用 `Frame.GoBack(NavigationTransitionInfo)`. 当你基于屏幕大小动态地修改导航行为时（例如，在响应式主/细节场景中），这可能很有用。

## <a name="related-topics"></a>相关主题

- [在两个页面之间导航](../basics/navigate-between-two-pages.md)
- [UWP 应用中运动](index.md)
