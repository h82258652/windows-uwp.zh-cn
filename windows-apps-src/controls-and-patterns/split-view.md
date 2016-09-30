---
author: Jwmsft
title: "拆分视图"
ms.assetid: E9E4537F-1160-4183-9A83-26602FCFDC9A
description: "拆分视图控件具有一个可展开/可折叠的窗格和一个内容区域。"
label: Split view
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: 391bfdbbf09474ad707dbbf306d4997825fa8386

---

# 拆分视图控件指南



**重要的 API**

-   [**SplitView 类 (XAML)**](https://msdn.microsoft.com/library/windows/apps/dn864360)
-   [**SplitView 对象 (HTML)**](https://msdn.microsoft.com/library/windows/apps/dn919970)

拆分视图控件具有一个可展开/可折叠的窗格和一个内容区域。 内容区域始终可见。 窗格可以展开和折叠或停留在打开状态，而且可以从应用窗口的左侧或右侧显示其自身。 窗格中有四种模式：

-   **Overlay**

    在打开之前隐藏窗格。 在打开时，窗格覆盖内容区域。

-   **内联**

    窗格始终可见，并且不会覆盖内容区域。 窗格和内容区域划分可用的屏幕空间。

-   **CompactOverlay**

    在此模式下始终可以看见狭窄的部分窗格，宽度恰好足以显示图标。 默认关闭窗格宽度为 48px，可以使用 `CompactPaneLength` 进行修改。 如果打开窗格，将覆盖内容区域。

-   **CompactInline**

    在此模式下始终可以看见狭窄的部分窗格，宽度恰好足以显示图标。 默认关闭窗格宽度为 48px，可以使用 `CompactPaneLength` 进行修改。 如果打开窗格，将减少用于内容的空间，从而会将内容挤出去。

## <span id="Is_this_the_right_control_"></span><span id="is_this_the_right_control_"></span><span id="IS_THIS_THE_RIGHT_CONTROL_"></span>这是正确的控件吗？

拆分视图控件可用于创建[导航窗格](nav-pane.md)。 若要生成此模式，需要添加一个展开/折叠按钮（“汉堡包”按钮）和一个表示导航项目的列表视图。

拆分视图控件还可以用于创建任何“抽屉”体验，其中用户可以打开和关闭补充窗格。

## <span id="Examples"></span><span id="examples"></span><span id="EXAMPLES"></span>示例

处于其默认形式下的拆分视图控件是一个基本容器。 下面是使用 SplitView 显示其中心的 Microsoft Edge 应用的示例。

![Microsoft Edge 拆分视图示例](images/split_view_Edge.png)



## <span id="related_topics"></span>相关主题


* [导航窗格模式](nav-pane.md)
* [列表视图](lists.md)
 

 



<!--HONumber=Jun16_HO4-->


