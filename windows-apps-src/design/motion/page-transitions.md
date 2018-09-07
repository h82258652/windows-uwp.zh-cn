---
author: QuinnRadich
Description: Learn how to use page transitions in your UWP apps.
title: UWP 应用中的页面过渡
template: detail.hbs
ms.author: quradic
ms.date: 04/08/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: stmoy
ms.localizationpriority: medium
ms.openlocfilehash: 0afc2c55ab0d0bdd2bee0206f986b2724d331eaf
ms.sourcegitcommit: 53ba430930ecec8ea10c95b390fe6e654fe363e1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/06/2018
ms.locfileid: "3413325"
---
# <a name="page-transitions"></a>页面过渡

页面过渡可将用户导航到应用中的各个页面，并提供反馈作为页面之间的关系。 页面过渡可帮助用户了解他们是否处于导航层次结构的顶部、在同级页面之间移动或导航到页面层次结构的更深层。

为应用、*页面刷新* 和*钻取* 内的页面之间的导航提供了两个不同的动画，由 [**NavigationTransitionInfo**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.navigationtransitioninfo) 的子类表示。

## <a name="page-refresh"></a>页面刷新

页面刷新是传入内容的上滑动画与淡入动画的组合。 请在用户转到导航堆栈的顶部时使用页面刷新，例如选项卡或左侧导航栏项目之间的导航。

所需的感觉是用户已重新开始。

![页面刷新动画](images/page-refresh.gif)

页面刷新动画由 [**EntranceNavigationTransitionInfoClass**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.entrancenavigationtransitioninfo) 表示。

```csharp
// Explicitly play the page refresh animation
myFrame.Navigate(typeof(Page2), null, new EntranceNavigationTransitionInfo());

```

**注意**：[**帧**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame)自动使用 [**NavigationThemeTransition**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.navigationthemetransition) 对两个页面之间的导航进行动画处理。 默认情况下，动画是页面刷新。

## <a name="drill"></a>钻取

请在用户导航到应用的更深层时使用钻取，例如在选择一个项目后显示更多信息。

所需的感觉是用户已进入应用的更深层。

![钻取动画](images/drill.gif)

钻取动画由 [**DrillInNavigationTransitionInfo**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.drillinnavigationtransitioninfo) 类表示。

```csharp
// Play the drill in animation
myFrame.Navigate(typeof(Page2), null, new DrillInNavigationTransitionInfo());
```

## <a name="suppress"></a>禁止显示

要避免在导航期间播放任何动画，请使用位于其他 **NavigationTransitionInfo** 子类型的位置的 [**SuppressNavigationTransitionInfo**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.suppressnavigationtransitioninfo)。

```csharp
// Suppress the default animation
myFrame.Navigate(typeof(Page2), null, new SuppressNavigationTransitionInfo());
```

如果要使用[连接的动画](connected-animation.md)构建自己的过渡或显示/隐藏动画，那么禁止显示动画很有用。

## <a name="backwards-navigation"></a>向后导航

要在向后导航时播放特定过渡，可以使用 `Frame.GoBack(NavigationTransitionInfo)`.

基于屏幕大小动态修改导航行为时（例如，在响应式主/细节场景中），这会很有用。

## <a name="related-topics"></a>相关主题

- [在两个页面之间导航](../basics/navigate-between-two-pages.md)
- [UWP 应用中运动](index.md)
