---
author: scottmill
title: 相对鼠标移动
description: 使用相对鼠标控件（这些控件不使用系统光标且不返回绝对屏幕坐标）以跟踪游戏中鼠标移动之间的像素增量。
ms.author: scotmi
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 游戏, 鼠标, 输入
ms.assetid: 08c35e05-2822-4a01-85b8-44edb9b6898f
ms.openlocfilehash: dff08052af7f005366f9cb5154b307c13a316953
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.locfileid: "204137"
---
# <a name="relative-mouse-movement-and-corewindow"></a>相对鼠标移动和 CoreWindow

在游戏中，鼠标是许多玩家都熟悉的一种常见控制选项，也是许多游戏类型的基本工具，包括第一人称和第三人称射击游戏和实时战略游戏。 下面我们将讨论相对鼠标控件的实现，这些控件不使用系统光标且不返回绝对屏幕坐标，而是会跟踪鼠标移动之间的像素增量。

某些应用（例如游戏）使用鼠标作为更为一般的输入设备。 例如，三维模型师可能会通过模拟虚拟轨迹球，使用鼠标输入来定向三维对象，或者游戏可能会使用鼠标来更改通过鼠标状控件查看相机的方向。 

在这些情况下，应用需要相对鼠标数据。 相对鼠标值表示鼠标自上一帧以后移动了多远距离，而不是窗口或屏幕中的绝对 x-y 坐标值。 另外，在操作三维对象或场景时，由于鼠标光标相对于屏幕坐标的位置并不具有相关性，因此应用经常会隐藏光标。 

当用户采取操作将应用移入相对三维对象/场景操作模式时，应用必须： 
- 忽略默认鼠标处理。
- 启用相对鼠标处理。
- 通过将鼠标光标设置为空指针 (nullptr) 来隐藏鼠标光标。 

当用户采取操作将应用移出相对三维对象/场景操作模式时，应用必须： 
- 启用默认/绝对鼠标处理。
- 禁用相对鼠标处理。 
- 将鼠标光标设置为非空值（使其可见）。

> **注意**  
使用这种模式，将在进入无光标的相对模式时保留绝对鼠标光标的位置。 光标将重新显示在与以前启用相对鼠标移动模式时相同的屏幕坐标位置。

 

## <a name="handling-relative-mouse-movement"></a>处理相对鼠标移动


要访问相对鼠标增量值，请注册 [MouseDevice::MouseMoved](https://msdn.microsoft.com/library/windows/apps/xaml/windows.devices.input.mousedevice.mousemoved.aspx) 事件，如下所示。


```cpp


// register handler for relative mouse movement events
Windows::Devices::Input::MouseDevice::GetForCurrentView()->MouseMoved +=
        ref new TypedEventHandler<MouseDevice^, MouseEventArgs^>(this, &MoveLookController::OnMouseMoved);


```

```cpp


void MoveLookController::OnMouseMoved(
    _In_ Windows::Devices::Input::MouseDevice^ mouseDevice,
    _In_ Windows::Devices::Input::MouseEventArgs^ args
    )
{
    float2 pointerDelta;
    pointerDelta.x = static_cast<float>(args->MouseDelta.X);
    pointerDelta.y = static_cast<float>(args->MouseDelta.Y);

    float2 rotationDelta;
    rotationDelta = pointerDelta * ROTATION_GAIN;    // scale for control sensitivity

    // update our orientation based on the command
    m_pitch -= rotationDelta.y;                        // mouse y increases down, but pitch increases up
    m_yaw   -= rotationDelta.x;                        // yaw defined as CCW around y-axis

    // limit pitch to straight up or straight down
    float limit = (float)(M_PI/2) - 0.01f;
    m_pitch = (float) __max( -limit, m_pitch );
    m_pitch = (float) __min( +limit, m_pitch );

    // keep longitude in useful range by wrapping
    if ( m_yaw >  M_PI )
        m_yaw -= (float)M_PI*2;
    else if ( m_yaw < -M_PI )
        m_yaw += (float)M_PI*2;
}

```

此代码示例中的事件处理程序 **OnMouseMoved** 根据鼠标的移动来呈现视图。 鼠标指针的位置作为 [MouseEventArgs](https://msdn.microsoft.com/library/windows/apps/xaml/windows.devices.input.mouseeventargs.aspx) 对象传递到该处理程序。 

当你的应用更改为处理相对鼠标移动值时，从 [CoreWindow::PointerMoved](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.core.corewindow.pointermoved.aspx) 事件跳过处理绝对鼠标数据。 但是，如果由于鼠标输入（相对于触控输入）而发生 **CoreWindow::PointerMoved** 事件时，将仅跳过此输入。 通过将 [CoreWindow::PointerCursor](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.core.corewindow.pointercursor.aspx) 设置为 **nullptr** 隐藏光标。 

## <a name="returning-to-absolute-mouse-movement"></a>返回到绝对鼠标移动

当应用退出三维对象或场景操作模式且不再使用相对鼠标移动时（例如当它返回到菜单屏幕时），返回到正常的绝对鼠标移动处理。 此时，停止读取相对鼠标数据，重新启动标准鼠标（和指针）事件处理，并将 [CoreWindow::PointerCursor](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.core.corewindow.pointercursor.aspx) 设置为非空值。 

> **注意**  
当你的应用处于三维对象/场景操作模式（处理相对鼠标移动，关闭光标）时，鼠标将无法调用边缘 UI，例如超级按钮、Back 堆栈或应用栏。 因此，请务必提供一种机制退出此特殊模式，例如通过常用的 **Esc** 键退出。

## <a name="related-topics"></a>相关主题

* [游戏的移动观看控件](tutorial--adding-move-look-controls-to-your-directx-game.md) 
* [游戏的触摸控件](tutorial--adding-touch-controls-to-your-directx-game.md)