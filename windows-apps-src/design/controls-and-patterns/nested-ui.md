---
Description: 使用嵌套的 UI 来支持列表项上的多个操作
title: 列表项中嵌套的 UI
label: Nested UI in list items
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 60a29717-56f2-4388-a9ff-0098e34d5896
pm-contact: chigy
design-contact: kimsea
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: aa672c99dc83e7955c4d4f91b5bc34620c48ed01
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2019
ms.locfileid: "66364551"
---
# <a name="nested-ui-in-list-items"></a>列表项中嵌套的 UI

 

嵌套的 UI 是用户界面 (UI)，用于公开包含在容器内嵌套的可操作控件，也可捕获独立的焦点。

你可以使用嵌套的 UI 来向用户显示附加选项，从而有助于促使用户采取重要的操作。 但是，公开的操作越多，UI 就变得越复杂。 在选择使用此 UI 模式时需要格外谨慎。 本文提供了指南来帮助你针对特定 UI 确定最佳做法。

> **重要的 API**：[ListView 类](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview)， [GridView 类](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.gridview)

在本文中，我们将讨论如何在 [ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview) 和 [GridView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.gridview) 项目中创建嵌套的 UI。 尽管本部分不讨论其他嵌套的 UI 情形，但是这些概念是可转移的。 在开始之前，你应当熟悉在 UI 中使用 ListView 或 GridView 控件的常规指南，可在[列表](lists.md)以及[列表视图和网格视图](listview-and-gridview.md)文章中找到它。

在本文中，我们使用术语*列表*、*列表项目*和*嵌套的 UI*，其定义如下：
- *列表*是指包含在列表视图或网格视图中的项目集合。
- *列表项目*是指列表中用户可对其采取操作的各个项目。
- *嵌套的 UI* 是指用户可对其采取操作的列表项目内的 UI 元素，此类操作不同于对列表项目本身采取的操作。

![嵌套的 UI 部分](images/nested-ui-example-1.png)

> 注意&nbsp;&nbsp; ListView 和 GridView 都派生自 [ListViewBase](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase) 类，因此它们的功能相同，但数据显示方法不同。 在本文中，当涉及到列表时，信息会同时应用到 ListView 和 GridView 控件。

## <a name="primary-and-secondary-actions"></a>主要操作和辅助操作

在使用列表创建 UI 时，请考虑用户可以从这些列表项目执行哪些操作。  

- 用户是否可以通过单击项目来执行某项操作？
    - 通常情况下，单击列表项目会启动某项操作，但也并非一定奏效。
- 用户是否可以执行多项操作？
    - 例如，点击列表中的电子邮件将打开该电子邮件。 但是，可能有其他操作，例如删除电子邮件，即用户可能希望执行该操作，而不必先打开它。 这将有益于用户直接在列表中访问该操作。
- 操作是否应该向用户公开？
    - 考虑所有输入类型。 一些嵌套的 UI 的形式非常适合某种输入方法，但可能不适用于其他方法。  

*主要操作*是指用户在按下列表项目时预期发生的操作。

*辅助操作*通常是与列表项目关联的快捷键。 这些快捷键可用于列表管理或与列表项目相关的操作。

## <a name="options-for-secondary-actions"></a>辅助操作的选项

当创建列表 UI 时，首先需要确保考虑到 UWP 支持的所有输入方法。 有关不同类型的输入的详细信息，请参阅[输入基础版](../input/index.md)。

在确认应用支持适用于 UWP 的所有输入后，应确定应用的辅助操作的重要程度是否足以在主列表中公开为快捷键。 请记住，公开的操作越多，UI 就变得越复杂。 是否确实需要在主列表 UI 中公开辅助操作，或者是否可以将它们放置在其他位置？

当附加操作需要随时可供所有输入访问时，可考虑在主列表 UI 中公开它们。

如果确认将辅助操作放在主列表 UI 中并非必需项，则可以使用其他方法向用户公开它们。 以下是放置辅助操作时可以考虑的一些选项。

### <a name="put-secondary-actions-on-the-detail-page"></a>将辅助操作放置在详细信息页面上

将辅助操作放置在该页面上，当按下列表项目时可以导航到该页面。 当使用大纲/细节模式时，详细信息页面通常是适合放置辅助操作的位置。

有关详细信息，请参阅[大纲/细节模式](master-details.md)。

### <a name="put-secondary-actions-in-a-context-menu"></a>将辅助操作放置在上下文菜单中

将辅助操作放置在上下文菜单中，用户可通过右键单击或长按进行访问。 这有利于让用户执行操作（例如删除电子邮件），而不必加载详细信息页面。 这是一个不错的做法，同时使这些选项在详细信息页面上可用，因为上下文菜单专用于快捷键而不是主 UI。

若要在从游戏板或遥控器进行输入时公开辅助操作，建议使用上下文菜单。

有关详细信息，请参阅[上下文菜单和浮出控件](menus.md)。

### <a name="put-secondary-actions-in-hover-ui-to-optimize-for-pointer-input"></a>将辅助操作放置在悬停 UI 中以针对指针输入进行优化

如果预期应用经常会与指针输入（如鼠标和笔）结合使用，并希望辅助操作仅供这些输入随时访问，则可以仅在悬停时显示辅助操作。 此快捷键仅在使用指针输入时才可见，因此请确保还使用其他选项来支持其他输入类型。

![悬停时显示的嵌套 UI](images/nested-ui-hover.png)


有关详细信息，请参阅[鼠标交互](../input/mouse-interactions.md)。

## <a name="ui-placement-for-primary-and-secondary-actions"></a>主要操作和辅助操作的 UI 放置

如果你确定辅助操作应在主列表 UI 中公开，建议使用以下指南。

当使用主要和辅助操作创建列表项目时，将主要操作放在左侧，而将辅助操作放在右侧。 在从左到右阅读文化中，用户会关联列表项目左侧的操作作为主要操作。

在这些示例中，我们将讨论列表 UI，其中项目倾向于以水平方式排列（其宽度大于高度）。 但是，可能会有列表项目在形状上更接近方形，或者其高度大于宽度。 通常，这些项目都是在网格中使用的项目。 对于这些项目，如果无法垂直滚动列表，可以将辅助操作放置在列表项目底部，而不是放置在右侧。

## <a name="consider-all-inputs"></a>考虑所有输入

当确定使用嵌套的 UI 时，还需根据所有输入类型评估用户体验。 如前面所述，嵌套的 UI 非常适用于某些输入类型。 但是，它并非始终适用于其他一些输入类型。 特别是，键盘、控制器和远程输入可能难以访问嵌套的 UI 元素。 请务必遵循下面的指南以确保你的 UWP 适用于所有输入类型。

## <a name="nested-ui-handling"></a>嵌套的 UI 处理

如果你有多个操作嵌套在列表项目中，建议使用本指南来处理使用键盘、游戏板、遥控器或其他非指针输入进行的导航。

### <a name="nested-ui-where-list-items-perform-an-action"></a>列表项目执行某项操作的嵌套的 UI

如果带嵌套元素的列表 UI 支持调用、选择（单选或多选）或拖放操作之类的操作，建议使用这些箭头技术在嵌套的 UI 元素中导航。

![嵌套的 UI 部分](images/nested-ui-navigation.png)

**Gamepad**

当输入来自游戏板时，提供此用户体验：

- 从 **A** 开始，借助右方向键可将焦点放在 **B** 上。
- 从 **B** 开始，借助右方向键可将焦点放在 **C** 上。
- 从 **C** 开始，右方向键为空操作，或将焦点放在此处（如果 列表右侧有可聚焦的 UI 元素）。
- 从 **C** 开始，借助左方向键可将焦点放在 **B** 上。
- 从 **B** 开始，借助左方向键可将焦点放在 **A** 上。
- 从 **A** 开始，左方向键为空操作，或将焦点放在此处（如果列表右侧有可聚焦的 UI 元素）。
- 从 **A**、**B** 或 **C** 开始，借助下方向键可将焦点放在 **D** 上。
- 在位于列表项左侧的 UI 元素中，借助右方向键可将焦点放在 **A** 上。
- 在位于列表项右侧的 UI 元素中，借助左方向键可将焦点放在 **A** 上。

**键盘**

当输入来自键盘时，以下是用户将获得的体验：

- 从 **A** 开始，借助 Tab 键可将焦点放在 **B** 上。
- 从 **B** 开始，借助 Tab 键可将焦点放在 **C** 上。
- 从 **C** 开始，借助 Tab 键可按 Tab 键顺序将焦点放在下一个可聚焦的 UI 元素上。
- 从 **C** 开始，借助 Shift+Tab 键可将焦点放在 **B** 上。
- 从 **B** 开始，借助 Shift+Tab 或向左键可将焦点放在 **A** 上。
- 从 **A** 开始，借助 Shift+Tab 键可按逆 Tab 键顺序将焦点放在下一个可聚焦的 UI 元素上。
- 从 **A**、**B** 或 **C** 开始，借助下箭头键可将焦点放在 **D** 上。
- 在位于列表项左侧的 UI 元素中，借助 Tab 键可将焦点放在 **A** 上。
- 在位于列表项右侧的 UI 元素中，借助 Shift+Tab 键可将焦点放在 **C** 上。

若要实现此 UI，请在列表上将 [IsItemClickEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.isitemclickenabled) 设置为 **true**。 [SelectionMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.selectionmode) 可以是任意值。

有关用于实现此目的的代码，请参阅本文的[示例](#example)部分。

### <a name="nested-ui-where-list-items-do-not-perform-an-action"></a>列表项不执行某项操作的嵌套的 UI

你可能会使用列表视图，因为它提供虚拟化和优化的滚动行为，但不具有与列表项关联的操作。 通常，这些 UI 仅使用列表项对元素进行分组，并确保它们作为一组进行滚动。

此类型的 UI 往往比前面的示例更加复杂，其中具有大量用户可以对其执行操作的嵌套元素。

![嵌套的 UI 部分](images/nested-ui-grouping.png)


若要实现此 UI，请在你的列表上设置以下属性：
- 将 [SelectionMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.selectionmode) 设置为 **None**。
- 将 [IsItemClickEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.isitemclickenabled) 设置为 **false**。
- 将 [IsFocusEngagementEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.isfocusengagementenabled) 设置为 **true**。

```xaml
<ListView SelectionMode="None" IsItemClickEnabled="False" >
    <ListView.ItemContainerStyle>
         <Style TargetType="ListViewItem">
             <Setter Property="IsFocusEngagementEnabled" Value="True"/>
         </Style>
    </ListView.ItemContainerStyle>
</ListView>
```

当列表项不执行操作时，建议使用本指南处理使用游戏板或键盘进行的导航。

**Gamepad**

当输入来自游戏板时，提供此用户体验：

- 在列表项中，借助下方向键可将焦点放在下一个列表项上。
- 在列表项中，向左/右键为空操作，或将焦点放在此处（如果列表右侧有可聚焦的 UI 元素）。
- 在列表项中，借助“A”按钮可按上/下或左/右优先级将焦点放在嵌套的 UI 中。
- 当位于嵌套的 UI 内时，请遵循 XY 焦点导航模型。  在用户按下“B”按钮前，焦点只能在包含在当前列表项内的嵌套 UI 中进行导航，这会将焦点放回该列表项上。

**键盘**

当输入来自键盘时，以下是用户将获得的体验：

- 在列表项中，借助下箭头键可将焦点放在下一个列表项上。
- 在列表项中，按向左/右箭是空操作。
- 在列表项中，按 Tab 键可将焦点放在嵌套 UI 项目中的下一制表位上。
- 在某一嵌套的 UI 项目中，按 Tab 键可按 Tab 键顺序遍历嵌套的 UI 项目。  在遍历所有嵌套的 UI 项目后，它会按 Tab 键顺序将焦点放在 ListView 后面的下一控件上。
- 如果按 Shift+Tab 键，将按照与 Tab 键相反的方向运行。

## <a name="example"></a>示例

此示例展示了如何实现[列表项执行某项操作的嵌套的 UI](#nested-ui-where-list-items-perform-an-action)。

```xaml
<ListView SelectionMode="None" IsItemClickEnabled="True"
          ChoosingItemContainer="listview1_ChoosingItemContainer"/>
```

```csharp
private void OnListViewItemKeyDown(object sender, KeyRoutedEventArgs e)
{
    // Code to handle going in/out of nested UI with gamepad and remote only.
    if (e.Handled == true)
    {
        return;
    }

    var focusedElementAsListViewItem = FocusManager.GetFocusedElement() as ListViewItem;
    if (focusedElementAsListViewItem != null)
    {
        // Focus is on the ListViewItem.
        // Go in with Right arrow.
        Control candidate = null;

        switch (e.OriginalKey)
        {
            case Windows.System.VirtualKey.GamepadDPadRight:
            case Windows.System.VirtualKey.GamepadLeftThumbstickRight:
                var rawPixelsPerViewPixel = DisplayInformation.GetForCurrentView().RawPixelsPerViewPixel;
                GeneralTransform generalTransform = focusedElementAsListViewItem.TransformToVisual(null);
                Point startPoint = generalTransform.TransformPoint(new Point(0, 0));
                Rect hintRect = new Rect(startPoint.X * rawPixelsPerViewPixel, startPoint.Y * rawPixelsPerViewPixel, 1, focusedElementAsListViewItem.ActualHeight * rawPixelsPerViewPixel);
                candidate = FocusManager.FindNextFocusableElement(FocusNavigationDirection.Right, hintRect) as Control;
                break;
        }

        if (candidate != null)
        {
            candidate.Focus(FocusState.Keyboard);
            e.Handled = true;
        }
    }
    else
    {
        // Focus is inside the ListViewItem.
        FocusNavigationDirection direction = FocusNavigationDirection.None;
        switch (e.OriginalKey)
        {
            case Windows.System.VirtualKey.GamepadDPadUp:
            case Windows.System.VirtualKey.GamepadLeftThumbstickUp:
                direction = FocusNavigationDirection.Up;
                break;
            case Windows.System.VirtualKey.GamepadDPadDown:
            case Windows.System.VirtualKey.GamepadLeftThumbstickDown:
                direction = FocusNavigationDirection.Down;
                break;
            case Windows.System.VirtualKey.GamepadDPadLeft:
            case Windows.System.VirtualKey.GamepadLeftThumbstickLeft:
                direction = FocusNavigationDirection.Left;
                break;
            case Windows.System.VirtualKey.GamepadDPadRight:
            case Windows.System.VirtualKey.GamepadLeftThumbstickRight:
                direction = FocusNavigationDirection.Right;
                break;
            default:
                break;
        }

        if (direction != FocusNavigationDirection.None)
        {
            Control candidate = FocusManager.FindNextFocusableElement(direction) as Control;
            if (candidate != null)
            {
                ListViewItem listViewItem = sender as ListViewItem;

                // If the next focusable candidate to the left is outside of ListViewItem,
                // put the focus on ListViewItem.
                if (direction == FocusNavigationDirection.Left &&
                    !listViewItem.IsAncestorOf(candidate))
                {
                    listViewItem.Focus(FocusState.Keyboard);
                }
                else
                {
                    candidate.Focus(FocusState.Keyboard);
                }
            }

            e.Handled = true;
        }
    }
}

private void listview1_ChoosingItemContainer(ListViewBase sender, ChoosingItemContainerEventArgs args)
{
    if (args.ItemContainer == null)
    {
        args.ItemContainer = new ListViewItem();
        args.ItemContainer.KeyDown += OnListViewItemKeyDown;
    }
}
```

```csharp
// DependencyObjectExtensions.cs definition.
public static class DependencyObjectExtensions
{
    public static bool IsAncestorOf(this DependencyObject parent, DependencyObject child)
    {
        DependencyObject current = child;
        bool isAncestor = false;

        while (current != null && !isAncestor)
        {
            if (current == parent)
            {
                isAncestor = true;
            }

            current = VisualTreeHelper.GetParent(current);
        }

        return isAncestor;
    }
}
```
