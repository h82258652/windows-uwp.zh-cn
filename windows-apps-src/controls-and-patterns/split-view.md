---
title: 拆分视图
ms.assetid: E9E4537F-1160-4183-9A83-26602FCFDC9A
description: 拆分视图控件具有一个可展开/可折叠的窗格和一个内容区域。
label: 拆分视图
template: detail.hbs
---

# 拆分视图控件的指南


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**重要的 API**

-   [**SplitView 类 (XAML)**](https://msdn.microsoft.com/library/windows/apps/dn864360)
-   [**SplitView 对象 (HTML)**](https://msdn.microsoft.com/library/windows/apps/dn919970)

拆分视图控件具有一个可展开/可折叠的窗格和一个内容区域。 内容区域始终可见。 窗格可以展开和折叠或停留在打开状态，而且可以从应用窗口的左侧或右侧显示其自身。 窗格中有三种模式：

-   **覆盖**

    在打开之前隐藏窗格。 在打开时，窗格覆盖内容区域。

-   **内联**

    窗格始终可见，并且不会覆盖内容区域。 窗格和内容区域划分可用的屏幕实际使用面积。

-   **精简**

    在此模式下窗格始终可见，它仅足够宽以显示图标（通常 48 epx 宽）。 窗格和内容区域划分可用的屏幕实际使用面积。 尽管标准精简模式不覆盖内容区域，但它可以转化为更宽的窗格来显示更多内容，这将覆盖该内容区域。

## <span id="Is_this_the_right_control_"> </span> <span id="is_this_the_right_control_"> </span> <span id="IS_THIS_THE_RIGHT_CONTROL_"> </span>这是正确的控件吗？


拆分视图可用于创建[导航窗格模式](nav-pane.md)。 若要构建此模式，需要将一个展开/折叠按钮（“汉堡包”按钮）和一个列表视图添加到拆分视图控件中。

## <span id="Examples"> </span> <span id="examples"> </span> <span id="EXAMPLES"> </span>示例


在其默认形式下的拆分视图控件是一个基本的容器。 借助添加的按钮和列表视图，拆分视图控件可以作为导航菜单。 下面是扩展和精简模式下的拆分视图作为导航菜单的示例。

![覆盖模式和精简模式下的拆分视图菜单示例](images/controls-splitview-menu01.png)
## <span id="Recommendations"> </span> <span id="recommendations"> </span> <span id="RECOMMENDATIONS"> </span>建议


-   在为导航菜单使用拆分视图时，我们建议在窗格中放置相应导航控件，以允许访问应用的其他区域。 使用窗格进行导航可提供一致的用户体验。 此外，此菜单实现可帮助用户熟悉应用的所有部分、提供对应用主页的快速访问，并且可以鼓励用户探索应用的更多部分。

\[本文包含特定于通用 Windows 平台 (UWP) 应用和 Windows 10 的信息。 有关 Windows 8.1 指南，请下载 [Windows 8.1 指南 PDF](https://go.microsoft.com/fwlink/p/?linkid=258743)。\]

## <span id="related_topics"> </span>相关主题


* [导航窗格模式](nav-pane.md)
* [列表视图](lists.md)
 

 






<!--HONumber=Mar16_HO1-->


