---
author: Karl-Bridge-Microsoft
Description: "了解如何在显示或隐藏触摸键盘时定制你的应用 UI。"
title: "响应触摸键盘的存在"
ms.assetid: 70C6130E-23A2-4F9D-88E7-7060062DA988
label: Respond to the presence of the touch keyboard
template: detail.hbs
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: 5f4f9553a54dc902c7c6a50d6a1b4cf6251fd72c

---

# 响应触摸键盘的存在

了解如何在显示或隐藏触摸键盘时定制你的应用 UI。


**重要的 API**

-   [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/br209185)
-   [**InputPane**](https://msdn.microsoft.com/library/windows/apps/br242255)



![默认布局模式中的触摸键盘](images/touchkeyboard-standard.png)

<sup>\\ 默认\\ 布局\\ 模式中\\ 的触摸\\ 键盘\\</sup>

对于支持触摸的设备，触摸键盘支持文本输入。 当用户点击可编辑的输入字段时，通用 Windows 平台 (UWP) 文本输入控件会默认调用触摸键盘。 当用户在表单的控件之间导航时，触摸键盘通常保持可见，不过此行为可能因表单内其他控件类型的不同而有所不同。

若要针对并非派生自标准文本输入控件的自定义文本输入控件支持相应的触摸键盘行为，必须使用 [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/br209185) 类向 Microsoft UI 自动化公开控件，并实现正确的 UI 自动化控件模式。 请参阅[键盘辅助功能](https://msdn.microsoft.com/library/windows/apps/mt244347)和[自定义的自动化对等](https://msdn.microsoft.com/library/windows/apps/mt297667)。

在此支持添加到你的自定义控件后，你便可以对触摸键盘的存在做出适当的响应。

**先决条件：  **

本主题基于[键盘交互](keyboard-interactions.md)生成。

你应该对标准键盘交互、处理键盘输入和事件以及 UI 自动化有一个基本了解。

如果你还不熟悉通用 Windows 平台 (UWP) 应用开发，请仔细阅读这些主题来熟悉此处讨论的技术。

-   [创建你的第一个应用](https://msdn.microsoft.com/library/windows/apps/bg124288)
-   借助[事件和路由事件概述](https://msdn.microsoft.com/library/windows/apps/mt185584)了解事件

**用户体验指南：  **

有关于设计出既实用又有吸引力且已针对键盘输入进行优化的应用的有用提示，请参阅[键盘设计指南](https://msdn.microsoft.com/library/windows/apps/hh972345)。

## <span id="Touch_keyboard_and_a_custom_UI"></span><span id="touch_keyboard_and_a_custom_ui"></span><span id="TOUCH_KEYBOARD_AND_A_CUSTOM_UI"></span>触摸键盘和自定义 UI


下面是关于自定义的文本输入控件的一些基本建议。

-   在与表单的整个交互中显示触摸键盘。

-   确保你的自定义控件具有正确的 UI 自动化[**AutomationControlType**](https://msdn.microsoft.com/library/windows/apps/br209182)，以确保在焦点从文本输入字段移出时（但仍然位于文本输入的上下文中）保持键盘始终显示。 例如，如果在文本输入时打开了一个菜单，但你希望该键盘一直显示，则该菜单必须具有 **AutomationControlType** 菜单。

-   不要操作 UI 自动化属性来控制触摸键盘。 其他辅助工具依赖于 UI 自动化属性的精度。

-   确保用户在交互时始终可以看到输入字段。

    触摸键盘会遮挡住大部分屏幕，不过 UWP 可确保当用户在表单上的不同控件（包括当前不在视图中的控件）之间导航时，具有焦点的输入字段滚动到视图中。

    自定义 UI 时，可通过处理 [**InputPane**](https://msdn.microsoft.com/library/windows/apps/br242255) 对象所公开的 [**Showing**](https://msdn.microsoft.com/library/windows/apps/br242262) 和 [**Hiding**](https://msdn.microsoft.com/library/windows/apps/br242260) 事件，针对触摸键盘的外观提供相似的行为。

    ![显示和没有显示触摸键盘的表单](images/touch-keyboard-pan1.png)

    在某些情况下，会有一些 UI 元素应始终保留在屏幕上。 对 UI 进行设计，以便表单控件包含在平移区域中并且重要的 UI 元素时静态的。 例如：

    ![包含应始终位于视图中的区域的表单](images/touch-keyboard-pan2.png)

## <span id="handling_events"></span><span id="HANDLING_EVENTS"></span>处理 Showing 和 Hiding 事件


下面是关于附加触摸键盘的 [**showing**](https://msdn.microsoft.com/library/windows/apps/br242262) 和 [**hiding**](https://msdn.microsoft.com/library/windows/apps/br242260) 事件的事件处理程序的示例。

```CSharp
public class MyApplication
{
    public MyApplication()
    {
        // Grab the input pane for the main application window and attach
        // touch keyboard event handlers.
        Windows.Foundation.Application.InputPane.GetForCurrentView().Showing  
            += new EventHandler(_OnInputPaneShowing);
        Windows.Foundation.Application.InputPane.GetForCurrentView().Hiding 
            += new EventHandler(_OnInputPaneHiding);
    }

    private void _OnInputPaneShowing(object sender, IInputPaneVisibilityEventArgs eventArgs)
    {
        // If the size of this window is going to be too small, the app uses 
        // the Showing event to begin some element removal animations.
        if (eventArgs.OccludedRect.Top < 400)
        {
            _StartElementRemovalAnimations();

            // Don&#39;t use framework scroll- or visibility-related 
            // animations that might conflict with the app&#39;s logic.
            eventArgs.EnsuredFocusedElementInView = true; 
        }
    }

    private void _OnInputPaneHiding(object sender, IInputPaneVisibilityEventArgs eventArgs)
    {
        if (_ResetToDefaultElements())
        {
            eventArgs.EnsuredFocusedElementInView = true; 
        }
    }

    private void _StartElementRemovalAnimations()
    {
        // This function starts the process of removing elements 
        // and starting the animation.
    }

    private void _ResetToDefaultElements()
    {
        // This function resets the window&#39;s elements to their default state.
    }
}
```

## <span id="related_topics"></span>相关文章

* [键盘交互](keyboard-interactions.md)
* [键盘辅助功能](https://msdn.microsoft.com/library/windows/apps/mt244347)
* [自定义自动化对等](https://msdn.microsoft.com/library/windows/apps/mt297667)


**存档示例**
* [输入：触摸键盘示例](http://go.microsoft.com/fwlink/p/?linkid=246019)
* [响应屏幕键盘外观示例](http://go.microsoft.com/fwlink/p/?linkid=231633)
* [XAML 文本编辑示例](http://go.microsoft.com/fwlink/p/?LinkID=251417)
* [XAML 辅助功能示例](http://go.microsoft.com/fwlink/p/?linkid=238570)
 

 







<!--HONumber=Jun16_HO3-->


