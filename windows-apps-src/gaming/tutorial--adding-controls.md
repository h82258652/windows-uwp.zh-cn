---
title: 添加控件
description: 现在，我们了解该游戏示例如何在 3D 游戏中实现移动观看控件，以及如何开发基本的触摸、鼠标和游戏控制器控件。
ms.assetid: f9666abb-151a-74b4-ae0b-ef88f1f252f8
ms.date: 10/24/2017
ms.topic: article
keywords: windows 10, uwp, 游戏, 控件, 输入
ms.localizationpriority: medium
ms.openlocfilehash: edc790ba949010fb1975317c5113ca02744889a0
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/06/2020
ms.locfileid: "75684565"
---
# <a name="add-controls"></a>添加控件


适用于 Windows 10 上的 UWP 应用的 \[ 更新。 有关 Windows 2.x 的文章，请参阅[存档](https://docs.microsoft.com/previous-versions/windows/apps/mt244353(v=win.10)?redirectedfrom=MSDN)\]

优秀的通用 Windows 平台 (UWP) 游戏支持多种界面。 可能的玩家可能在平板电脑上安装了 Windows 10，无需物理按钮、连接了 Xbox 控制器的 PC 或具有高性能鼠标和游戏键盘的最新桌面游戏装置。 在我们的游戏中，控制在 [**MoveLookController**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp) 类中实现。 此类将全部三个输入类型（鼠标和键盘、触控和游戏板）聚合到一个控制器内。 最终结果是一个第一人称射击游戏，其使用通过多台设备使用的流派标准移动观看控件。

> [!NOTE]
> 有关控件的详细信息，请参阅[游戏的移动观看控件](tutorial--adding-move-look-controls-to-your-directx-game.md)和[游戏的触摸控件](tutorial--adding-touch-controls-to-your-directx-game.md)。


## <a name="objective"></a>目标

此时，我们有呈现的游戏，但我们不能四处移动玩家或射击目标。 我们来看看我们的游戏如何在 UWP DirectX 游戏中为以下类型的输入实现第一人称射击游戏移动观看控件。
- 鼠标和键盘
- 触控
- 游戏板

>[!Note]
>如果你尚未下载适用于此示例的最新游戏代码，请转到 [Direct3D 游戏示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX)。 此示例是大型 UWP 功能示例集合的一部分。 有关如何下载示例的说明，请参阅[从 GitHub 获取 UWP 示例](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples)。

## <a name="common-control-behaviors"></a>常用控件行为


触摸控件和鼠标/键盘控件具有非常类似的核心实现。 在 UWP 应用中，指针只是屏幕上的点。 你可以滑动鼠标或在触摸屏上滑动手指来移动指针。 因此，你可以注册单个事件集，而不用担心玩家使用鼠标或触摸屏移动或点按指针。

在初始化该游戏示例中的 **MoveLookController** 类时，将注册四个特定于指针的事件和一个特定于鼠标的事件：

事件 | 描述
:------ | :-------
[**CoreWindow：:P ointerPressed**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.pointerpressed) | 点按（并按住）左右鼠标按钮，或触摸触摸屏表面。
[**CoreWindow：:P ointerMoved**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.pointermoved) |移动鼠标，或在触摸表面执行拖动操作。
[**CoreWindow：:P ointerReleased**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.pointerreleased) |释放鼠标左按钮，或者移开与触摸表面接触的物体。
[**CoreWindow：:P ointerExited**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.pointerexited) |指针移出主窗口。
[**Windows：:D evices：： Input：： MouseMoved**](https://docs.microsoft.com/uwp/api/windows.devices.input.mousedevice.mousemoved) | 鼠标移动了一定距离。 注意，我们只关注鼠标移动增量值，不关注当前 X-Y 位置。


这些事件处理程序被设置为当 **MoveLookController** 在应用程序窗口中初始化后即为用户输入启动侦听。
```cpp
void MoveLookController::InitWindow(_In_ CoreWindow^ window)
{
    ResetState();

    window->PointerPressed +=
        ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerPressed);

    window->PointerMoved +=
        ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerMoved);

    window->PointerReleased +=
        ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerReleased);

    window->PointerExited +=
        ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerExited);

    // There is a separate handler for mouse only relative mouse movement events.
    MouseDevice::GetForCurrentView()->MouseMoved +=
        ref new TypedEventHandler<MouseDevice^, MouseEventArgs^>(this, &MoveLookController::OnMouseMoved);
    //
    // ...
    //
}
```

[  **InitWindow**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L68-L92) 的完整代码可以在 GitHub 上看到。


若要确定游戏应在何时侦听某个输入，**MoveLookController** 类有三个特定于控制器的状态（与控制器类型无关）：

State | 描述
:----- | :-------
**无** | 这是控制器的初始化状态。 忽略所有输入，因为游戏不期待任何控制器输入。
**WaitForInput** | 控制器等待玩家通过使用鼠标左键、触摸事件或游戏板上的菜单按钮确认来自游戏的消息。
**Active** | 控制器处于活动的游戏模式。



### <a name="waitforinput-state-and-pausing-the-game"></a>WaitForInput 状态和暂停游戏

游戏在暂停后进入 **WaitForInput** 状态。 当玩家将指针移出游戏主窗口或者按下暂停按钮（P 键或游戏板**开始**按钮）时将发生此情况。 **MoveLookController** 注册点按操作，并在它调用 [**IsPauseRequested**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L107-L127) 方法时通知游戏循环。 此时，如果 **IsPauseRequested** 返回 **true**，游戏循环将对 **MoveLookController** 调用 **WaitForPress** 以将控制器转入 **WaitForInput** 状态。 


进入 **WaitForInput** 状态后，游戏将停止处理几乎所有游戏输入事件，直到它返回到 **Active** 状态。 例外情况是暂停按钮，按此按钮会使游戏回退到活动状态。 除 "暂停" 按钮外，要使游戏返回到播放机的**活动**状态，播放机需要选择菜单项。 



### <a name="the-active-state"></a>Active 状态

在 **Active** 状态期间，**MoveLookController** 实例处理来自所有启用的输入设备的事件，并解释玩家的意图。 因此，在从游戏循环中调用 **Update** 之后，它会更新玩家视野的速度和观看方向，并与游戏共享更新数据。


所有指针输入都在 **Active** 状态中跟踪，不同的指针 ID 对应于不同的指针操作。
当收到 [**PointerPressed**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.pointerpressed) 事件时，**MoveLookController** 将获取窗口创建的指针 ID 值。 指针 ID 表示特定的输入类型。 例如，在多点触控设备上，可能同时有若干不同的主动输入。 ID 用于跟踪玩家在使用哪个输入。 如果触摸屏的移动矩形中有一个事件，将指派一个指针 ID 跟踪移动矩形中的任何指针事件。 射击矩形中的其他指针事件则使用单独的指针 ID 进行单独跟踪。


> [!NOTE]
> 鼠标和游戏板右操纵杆的输入也有单独处理的 ID。

在将指针事件映射到特定的游戏操作之后，可以更新 **MoveLookController** 对象与主游戏循环共享的数据。

调用时，游戏示例中的[**Update**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L1005-L1096)方法将处理输入并更新速度和外观方向变量（**m\_速度**和**m\_lookdirection**），然后通过调用公共[**速度**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L906-L909)和[**lookdirection**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L913-L923)方法来检索游戏循环。

> [!NOTE]
> 有关 [**Update**](#the-update-method) 方法的更多详细信息，可以在此页的后面部分看到。




游戏循环可以通过在 **MoveLookController** 实例上调用 **IsFiring** 方法测试玩家是否正在射击。 **MoveLookController** 通过检查了解玩家是否已在三种输入类型之一上按下射击按钮。

```cpp
bool MoveLookController::IsFiring()
{
    if (m_state == MoveLookControllerState::Active)
    {
        if (m_autoFire)
        {
            return (m_fireInUse || (m_mouseInUse && m_mouseLeftInUse) || PollingFireInUse());
        }
        else
        {
            if (m_firePressed)
            {
                m_firePressed = false;
                return true;
            }
        }
    }
    return false;
}
```









现在，我们将较为详细地讨论这三种控件类型中每个类型的实现。

## <a name="adding-relative-mouse-controls"></a>添加相对鼠标控件


如果检测到鼠标移动，我们希望利用该移动确定相机的新的俯仰和偏航。 我们通过实现相对鼠标控件实现此目的，在该控件中我们处理鼠标所移动的相对距离（移动开始到停止之间的增量），与记录运动的绝对 x-y 像素坐标相反。

为此，我们通过检查由 [**MouseMoved**](https://docs.microsoft.com/uwp/api/windows.devices.input.mousedevice.mousemoved) 事件返回的 [**Windows::Device::Input::MouseEventArgs::MouseDelta**](https://docs.microsoft.com/uwp/api/windows.devices.input.mouseeventargs.mousedelta) 参数对象上的 [**MouseDelta::X**](https://docs.microsoft.com/uwp/api/Windows.Devices.Input.MouseDelta) 和 **MouseDelta::Y** 字段获取 X（水平运动）和 Y（垂直运动）坐标的变化。

```cpp
void MoveLookController::OnMouseMoved(
    _In_ MouseDevice^ /* mouseDevice */,
    _In_ MouseEventArgs^ args
    )
{
    // Handle Mouse Input via dedicated relative movement handler.

    switch (m_state)
    {
    case MoveLookControllerState::Active:
        XMFLOAT2 mouseDelta;
        mouseDelta.x = static_cast<float>(args->MouseDelta.X);
        mouseDelta.y = static_cast<float>(args->MouseDelta.Y);

        XMFLOAT2 rotationDelta;
        rotationDelta.x  = mouseDelta.x * MoveLookConstants::RotationGain;   // scale for control sensitivity
        rotationDelta.y  = mouseDelta.y * MoveLookConstants::RotationGain;

        // Update our orientation based on the command.
        m_pitch -= rotationDelta.y;
        m_yaw   += rotationDelta.x;

        // Limit pitch to straight up or straight down.
        float limit = XM_PI / 2.0f - 0.01f;
        m_pitch = __max(-limit, m_pitch);
        m_pitch = __min(+limit, m_pitch);

        // Keep longitude in same range by wrapping.
        if (m_yaw >  XM_PI)
        {
            m_yaw -= XM_PI * 2.0f;
        }
        else if (m_yaw < -XM_PI)
        {
            m_yaw += XM_PI * 2.0f;
        }
        break;
    }
}
```

## <a name="adding-touch-support"></a>添加触控支持

触摸控件非常适合用于支持使用平板电脑的用户。 此游戏通过划分屏幕的某些区域，利用每次针对游戏内特定操作的调整来收集触控输入。
此游戏的触控输入使用三个区域。

![移动观看触控布局](images/simple-dx-game-controls-touchzones.png)

以下命令汇总了我们的触摸控件行为。
用户输入 | “操作”
:------- | :--------
移动矩形 | 触控输入转换为虚拟游戏杆，这时，垂直运动将转换为向前/向后位置移动，水平运动将转换为向左/向右位置移动。
射击矩形 | 射击视区。
触摸移动和射击矩形的外部 | 更改相机视图的旋转（俯仰和偏航）。

**MoveLookController** 检查指针 ID 以确定发生事件的位置，并采取以下操作之一：

-   如果在移动或射击矩形中发生了 [**PointerMoved**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.pointermoved) 事件，则更新控制器的指针位置。
-   如果在屏幕的其他位置（定义为观看控制区）发生了 [**PointerMoved**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.pointermoved) 事件，则计算观看方向矢量的俯仰和偏航变化。


在实现触摸控件之后，我们之前使用 Direct2D 绘制的矩形将指示玩家移动、射击和观看区域在哪里。


![触摸控件](images/simple-dx-game-controls-showzones.png)


现在我们来看看我们如何实现每个控件。


### <a name="move-and-fire-controller"></a>移动和射击控制器
屏幕左下区的移动控制器矩形用作方向板。 在此空间内左右滑动拇指可左右移动玩家，而上下滑动可前后移动相机。
完成此设置后，点击屏幕右下区的射击控制器可射击视区。

[  **SetMoveRect**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L843-L853) 和 [**SetFireRect**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L857-L867) 方法创建输入矩形，使用两个 2D 矢量在屏幕上指定每个矩形的左上角和右下角位置。 


然后，将参数分配给**m\_fireUpperLeft**和**m\_fireLowerRight** ，这将帮助我们确定用户是否在矩形内触摸。 
```cpp
m_fireUpperLeft  = upperLeft;
m_fireLowerRight = lowerRight;
```

如果重新调整屏幕大小，这些矩形将重新绘制为相应的大小。


现在，我们已经对控件进行了分区，应该确定用户何时可以实际使用它们了。
为此，我们在 **MoveLookController::InitWindow** 方法中为用户何时按下、移动或释放指针设置了一些事件处理程序。

```cpp
window->PointerPressed +=
    ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerPressed);

window->PointerMoved +=
    ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerMoved);

window->PointerReleased +=
    ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerReleased);
```


我们将首先确定，用户在使用 [**OnPointerPressed**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L179-L313) 方法第一次按移动或射击矩形内部时将发生什么。
在这里，我们检查他们触摸控件的位置以及指针是否已在该控制器中。 如果这是触摸特定控件的第一个手指，我们执行以下操作。
- 将 system.windows.uielement.touchdown> 的位置以**m\_moveFirstDown**或**m\_FireFirstDown**存储为2d 向量。
- 将指针 ID 分配给**m\_movePointerID**或**m\_firePointerID**。
- 将正确的**正在使用**标志（**m\_moveInUse**或**m\_fireInUse**）设置为 `true`，因为现在已经有了该控件的活动指针。


```cpp
    PointerPoint^ point = args->CurrentPoint;
    uint32 pointerID = point->PointerId;
    Point pointerPosition = point->Position;
    PointerPointProperties^ pointProperties = point->Properties;
    auto pointerDevice = point->PointerDevice;
    auto pointerDeviceType = pointerDevice->PointerDeviceType;

    XMFLOAT2 position = XMFLOAT2(pointerPosition.X, pointerPosition.Y);

    case MoveLookControllerState::Active:
        switch (pointerDeviceType)
        {
        case Windows::Devices::Input::PointerDeviceType::Touch:
            // Check to see if this pointer is in the move control.
            if (position.x > m_moveUpperLeft.x &&
                position.x < m_moveLowerRight.x &&
                position.y > m_moveUpperLeft.y &&
                position.y < m_moveLowerRight.y)
            {
                if (!m_moveInUse)         // If no pointer is in this control yet:
                {
                    // Process a DPad touch down event.
                    m_moveFirstDown = position;                 // Save the location of the initial contact
                    m_movePointerID = pointerID;                // Store the pointer using this control
                    m_moveInUse = true;                         // Set InUse flag to signal there is an active move pointer
                }
            }
            // Check to see if this pointer is in the fire control.
            else if (position.x > m_fireUpperLeft.x &&
                position.x < m_fireLowerRight.x &&
                position.y > m_fireUpperLeft.y &&
                position.y < m_fireLowerRight.y)
            {
                if (!m_fireInUse)
                {
                    m_fireLastPoint = position;     // Save the location of the initial contact
                    m_firePointerID = pointerID;    // Store the pointer using this control
                    m_fireInUse = true;             // Set InUse flag to signal there is an active fire pointer
                }
            }
            ...
```


现在，我们已经确定了用户是在触摸移动控件还是射击控件，我们会看到玩家是否使用他们按下的手指进行了任何移动。
使用 [**MoveLookController::OnPointerMoved**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L317-L395) 方法，我们检查哪个指针已移动，然后将它的新位置存储为 2D 矢量。  


```cpp
    PointerPoint^ point = args->CurrentPoint;
    uint32 pointerID = point->PointerId;
    Point pointerPosition = point->Position;
    PointerPointProperties^ pointProperties = point->Properties;
    auto pointerDevice = point->PointerDevice;

    XMFLOAT2 position = XMFLOAT2(pointerPosition.X, pointerPosition.Y);     // convert to allow math
    
    switch (m_state)
    {
    case MoveLookControllerState::Active:
        // Decide which control this pointer is operating.
        
        // Move control
        if (pointerID == m_movePointerID)
        {
            m_movePointerPosition = position;       // Save the current position.
        }
        // Look control
        else if (pointerID == m_lookPointerID)
        {
            // ...
        }
        // Fire control
        else if (pointerID == m_firePointerID)
        {
            m_fireLastPoint = position;
        }
```


当用户在控件内创建了手势后，他们将释放指针。 使用 [**MoveLookController::OnPointerReleased**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L441-L500) 方法，我们确定释放了哪个指针并执行一系列重置。


如果移动控件已释放，我们执行以下操作。
- 在所有方向将玩家的速度设置为 `0` 以阻止他们在游戏中移动。
- **\_moveInUse**切换到 `false`，因为用户不再触及移动控制器。
- 将移动指针 ID 设置为 `0`，因为移动控制器中不再有指针。

```cpp
       if (pointerID == m_movePointerID)
        {
            m_velocity = XMFLOAT3(0, 0, 0);      // Stop on release.
            m_moveInUse = false;
            m_movePointerID = 0;
        }
```


对于射击控件，如果它已被释放，我们要做的只是将 **m_fireInUse** 标志切换为 `false`，并将射击指针 ID 切换为 `0`，因为射击控件内不会再有指针。
```cpp
        else if (pointerID == m_firePointerID)
        {
            m_fireInUse = false;
            m_firePointerID = 0;
        }
```

### <a name="look-controller"></a>观看控制器
我们将屏幕未使用区域的触摸设备指针事件视为观看控制器。 在区域四处滑动手指可更改玩家相机的俯仰和偏航（旋转）。


如果在此区域的触摸设备上引发了 **MoveLookController::OnPointerPressed** 事件，并且游戏状态设置为 **Active**，则为其分配一个指针 ID。

```cpp
    if (!m_lookInUse)   // If no pointer is in this control yet:
    {
        m_lookLastPoint = position;                   // Save the pointer for a later move.
        m_lookPointerID = pointerID;                  // Store the pointer using this control.
        m_lookLastDelta.x = m_lookLastDelta.y = 0;    // These are for smoothing.
        m_lookInUse = true;
    }
```
你可以在 [GitHub](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L252-L259) 上查看 **MoveLookController::OnPointerPressed** 方法的完整代码。




在这里，**MoveLookController** 将触发该事件的指针的指针 ID 分配到对应于观看区域的特定变量。 如果在外观区域发生触摸， **m\_lookPointerID**变量设置为触发事件的指针 ID。 布尔变量**m\_lookInUse**）也被设置为指示该控件尚未发布。

现在，我们讨论游戏示例如何处理 [**PointerMoved**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.pointermoved) 触摸屏事件。


在 **MoveLookController::OnPointerMoved** 方法内，我们查看哪类指针 ID 被分配到该事件。 如果是 **m_lookPointerID**，我们将计算指针位置的变化。
然后，我们使用此增量计算应更改多少旋转。 最后，我们可以更新要在游戏中使用的**m\_螺距**和**m\_偏航**来更改播放机旋转。 

```cpp
    else if (pointerID == m_lookPointerID)     // This is the look pointer.
    {
        // Look control
        XMFLOAT2 pointerDelta;
        pointerDelta.x = position.x - m_lookLastPoint.x;        // How far did the pointer move.
        pointerDelta.y = position.y - m_lookLastPoint.y;

        XMFLOAT2 rotationDelta;
        rotationDelta.x = pointerDelta.x * MoveLookConstants::RotationGain;       // Scale for control sensitivity.
        rotationDelta.y = pointerDelta.y * MoveLookConstants::RotationGain;
        m_lookLastPoint = position;                             // Save for the next time through.

        // Update our orientation based on the command.
        m_pitch -= rotationDelta.y;
        m_yaw   += rotationDelta.x;

        // Limit pitch to straight up or straight down.
        float limit = XM_PI / 2.0f - 0.01f;
        m_pitch = __max(-limit, m_pitch);
        m_pitch = __min(+limit, m_pitch);M_PI / 2.0f, m_pitch);
        }
```





我们要讨论的最后一个部分是游戏示例如何处理 [**PointerReleased**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.pointerreleased) 触摸屏事件。
当用户完成了触摸手势并在屏幕上删除了手指后，[**MoveLookController::OnPointerReleased**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L441-L500) 将启动。
如果触发 [**PointerReleased**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.pointerreleased) 事件的指针的指针 ID 是以前记录的移动指针的 ID，则 **MoveLookController** 将速度设置为 `0`，这是因为玩家已停止触摸观看区域。

```cpp
    else if (pointerID == m_lookPointerID)
    {
        m_lookInUse = false;
        m_lookPointerID = 0;
    }
```





## <a name="adding-mouse-and-keyboard-support"></a>添加鼠标和键盘支持


此游戏具有以下键盘和鼠标控件布局。

用户输入 | “操作”
:------- | :--------
W | 向前移动玩家
一个 | 向左移动玩家
S | 向后移动玩家
D | 向右移动玩家
X | 向上移动视图
空格键 | 向上移动视图
P | 暂停游戏
鼠标移动 | 更改相机视图的旋转（俯仰和偏航）
鼠标左键 | 射击视区


若要使用键盘，游戏示例在 [**MoveLookController::InitWindow**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L84-L88) 方法内注册两个新事件 [**CoreWindow::KeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.keyup) 和 [**CoreWindow::KeyDown**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.keydown)。 这些事件处理键的按下和释放。

```cpp
window->KeyDown +=
        ref new TypedEventHandler<CoreWindow^, KeyEventArgs^>(this, &MoveLookController::OnKeyDown);

window->KeyUp +=
        ref new TypedEventHandler<CoreWindow^, KeyEventArgs^>(this, &MoveLookController::OnKeyUp);
```

尽管鼠标使用的也是指针，但它的处理方式与触摸控件稍有不同。 若要与我们的控件布局保持一致，无论何时移动鼠标，**MoveLookController** 都会旋转，并在按下鼠标左键时射击。


这在 **MoveLookController** 的 [**OnPointerPressed**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L179-L313) 方法中处理。

在此方法中，我们查看哪种类型的指针设备与 [`Windows::Devices::Input::PointerDeviceType`](https://docs.microsoft.com/uwp/api/Windows.Devices.Input.PointerDeviceType) 枚举一起使用。 如果游戏处于 **Active** 状态且 **PointerDeviceType** 不是 **Touch**，我们假设为鼠标输入。

```cpp
    case MoveLookControllerState::Active:
        switch (pointerDeviceType)
        {
        case Windows::Devices::Input::PointerDeviceType::Touch:
            // Behavior for touch controls
        
        default:
        // Behavior for mouse controls
            bool rightButton = pointProperties->IsRightButtonPressed;
            bool leftButton = pointProperties->IsLeftButtonPressed;

            if (!m_autoFire && (!m_mouseLeftInUse && leftButton))
            {
                m_firePressed = true;
            }

            if (!m_mouseInUse)
            {
                m_mouseInUse = true;
                m_mouseLastPoint = position;
                m_mousePointerID = pointerID;
                m_mouseLeftInUse = leftButton;
                m_mouseRightInUse = rightButton;
                m_lookLastDelta.x = m_lookLastDelta.y = 0;          // These are for smoothing.
            }
            break;
        }
```

在玩家停止按其中一个鼠标按钮时，将引发 [CoreWindow::PointerReleased](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow.PointerReleased) 鼠标事件，调用 [MoveLookController::OnPointerReleased](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L441-L500) 方法，输入完成。 此时，如果按下了鼠标左键并在现在释放，球体将停止射击。 由于始终启用观看，所以游戏将继续使用同一鼠标指针跟踪正在进行的观看事件。

```cpp
    case MoveLookControllerState::Active:
        // Touch points
        if (pointerID == m_movePointerID)
        {
            // Stop movement
        }
        else if (pointerID == m_lookPointerID)
        {
            // Stop look rotation
        }
        // Fire button has been released
        else if (pointerID == m_firePointerID)
        {
            // Stop firing
        }
        // Mouse point
        else if (pointerID == m_mousePointerID)
        {
            bool rightButton = pointProperties->IsRightButtonPressed;
            bool leftButton = pointProperties->IsLeftButtonPressed;

            // Mouse no longer in use so stop firing
            m_mouseInUse = false;

            // Don't clear the mouse pointer ID so that Move events still result in Look changes.
            // m_mousePointerID = 0;
            m_mouseLeftInUse = leftButton;
            m_mouseRightInUse = rightButton;
        }
        break;
    }
}
```



现在，我们来看看要支持的最后一种控件类型：游戏板。 游戏板独立于触摸控件和鼠标控件进行处理，因为它们不使用指针对象。 出于此原因，需要添加几个新的事件处理程序和方法。

## <a name="adding-gamepad-support"></a>添加游戏板支持


对于此游戏，游戏板支持通过调用 [Windows.Gaming.Input](https://docs.microsoft.com/uwp/api/windows.gaming.input) API 添加。 这组 API 提供对游戏控制器输入（如赛车方向盘和飞行杆）的访问。 


下面将是我们的游戏板控件。

用户输入 | “操作”
:------- | :--------
左摇杆 | 移动玩家
右摇杆 | 更改相机视图的旋转（俯仰和偏航）
右扳机键 | 射击视区
“开始”/“菜单”按钮 | 暂停或继续游戏




在 [**InitWindow**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L68-L103) 方法中，我们添加了两个新事件来确定游戏板是否已[添加](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L1100-L1105)或[删除](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L1109-L1114)。 这些事件更新 **m_gamepadsChanged** 属性。 在**UpdatePollingDevices**方法中使用此方法来检查已知 gamepads 的列表是否已更改。 

```cpp
    // Detect gamepad connection and disconnection events.
    Gamepad::GamepadAdded +=
        ref new EventHandler<Gamepad^>(this, &MoveLookController::OnGamepadAdded);

    Gamepad::GamepadRemoved +=
        ref new EventHandler<Gamepad^>(this, &MoveLookController::OnGamepadRemoved);
```

> [!NOTE]
> 当应用未处于焦点时，UWP 应用无法从 Xbox One 控制器接收输入。

### <a name="the-updatepollingdevices-method"></a>UpdatePollingDevices 方法

**MoveLookController** 实例的 [**UpdatePollingDevices**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L654-L782) 方法立即检查游戏板是否已连接。 如果已连接，我们将开始通过 [**Gamepad.GetCurrentReading**](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepad.GetCurrentReading) 读取它的状态。 这将返回 [**GamepadReading**](https://docs.microsoft.com/uwp/api/Windows.Gaming.Input.GamepadReading) 结构，让我们可以检查点击了哪些按钮或移动了什么操纵杆。


如果游戏的状态是 **WaitForInput**，我们将仅侦听控制器的“开始”/“菜单”按钮，以便可以继续游戏。


如果是 **Active** 状态，我们将检查用户的输入并确定需要发生哪些游戏内操作。
例如，如果用户向特定方向移动了左摇杆，这让游戏知道我们需要朝摇杆移动的方法移动玩家。 向特定方向移动摇杆必须表现为大于**死区**的半径；否则，不会发生任何操作。 此死区半径需要阻止“漂移”，当控制器从玩家放在摇杆的拇指上拾取较小移动时，将呈现“漂移”。 如果没有死区，控制对用户可能会表现得过于敏感。


x 轴和 y 轴的操纵杆输入都在 -1 和 1 之间。 以下常量指定操纵杆死区的半径。

```cpp
#define THUMBSTICK_DEADZONE 0.25f
```

使用此变量，我们随后将开始处理可操作的操纵杆输入。 移动将在任一轴上按照 [-1, -.26] 或 [.26, 1] 值进行。

![操纵杆的死区](images/simple-dx-game-controls-deadzone.png)

这个 **UpdatePollingDevices** 方法处理左右操纵杆。
检查每个杆的 X 和 Y 值以查看它们是否在死区之外。 如果有一个或两个值都在死区之外，我们将更新相应的组件。
例如，如果左操纵杆沿 X 轴向左移动，我们会向 **m_moveCommand** 矢量的 **x** 组件添加 -1。 此矢量将用于聚合跨所有设备的所有移动，随后还将用于计算玩家应移动的位置。 


```cpp
        // Use the left thumbstick to control the eye point position (position of the player)

        // Check if left thumbstick is outside of dead zone on x axis
        if (reading.LeftThumbstickX > THUMBSTICK_DEADZONE ||
            reading.LeftThumbstickX < -THUMBSTICK_DEADZONE)
        {
            // Get value of left thumbstick's position on x axis
            float x = static_cast<float>(reading.LeftThumbstickX);
            // Set the x of the move vector to 1 if the stick is being moved right.
            // Set to -1 if moved left. 
            m_moveCommand.x -= (x > 0) ? 1 : -1;
        }

        // Check if left thumbstick is outside of dead zone on y axis
        if (reading.LeftThumbstickY > THUMBSTICK_DEADZONE ||
            reading.LeftThumbstickY < -THUMBSTICK_DEADZONE)
        {
            // Get value of left thumbstick's position on y axis
            float y = static_cast<float>(reading.LeftThumbstickY);
            // Set the y of the move vector to 1 if the stick is being moved forward.
            // Set to -1 if moved backwards. 
            m_moveCommand.y += (y > 0) ? 1 : -1;
        }

```

与左操纵杆控制移动的方式类似，右操纵杆控制相机的旋转。


右操纵杆的行为与鼠标和键盘控件设置中的鼠标移动行为一致。
如果杆在死区之外，我们将计算当前指针位置与用户正在尝试查看的位置之间的差值。
这一指针位置的更改 (**pointerDelta**) 随后用于更新相机旋转的俯仰和偏航（随后应用于 **Update** 方法）。
**pointerDelta** 矢量可能看起来很熟悉，因为它也会在 [MoveLookController::OnPointerMoved](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L318-L395) 方法中用来跟踪鼠标和触摸输入的指针位置的变化。


```cpp
        // Use the right thumbstick to control the look at position

        XMFLOAT2 pointerDelta;

        // Check if right thumbstick is outside of deadzone on x axis
        if (reading.RightThumbstickX > THUMBSTICK_DEADZONE ||
            reading.RightThumbstickX < -THUMBSTICK_DEADZONE)
        {
            float x = static_cast<float>(reading.RightThumbstickX);
            // Register the change in the pointer along the x axis
            pointerDelta.x = x * x * x; 
        }
        // No actionable thumbstick movement. Register no change in pointer.
        else
        {
            pointerDelta.x = 0.0f;
        }
        // Check if right thumbstick is outside of deadzone on y axis
        if (reading.RightThumbstickY > THUMBSTICK_DEADZONE ||
            reading.RightThumbstickY < -THUMBSTICK_DEADZONE)
        {
            float y = static_cast<float>(reading.RightThumbstickY);
            // Register the change in the pointer along the y axis
            pointerDelta.y = y * y * y;
        }
        else
        {
            pointerDelta.y = 0.0f;
        }

        XMFLOAT2 rotationDelta;
        rotationDelta.x = pointerDelta.x *  0.08f;       // Scale for control sensitivity.
        rotationDelta.y = pointerDelta.y *  0.08f;

        // Update our orientation based on the command.
        m_pitch += rotationDelta.y;
        m_yaw += rotationDelta.x;

        // Limit pitch to straight up or straight down.
        m_pitch = __max(-XM_PI / 2.0f, m_pitch);
        m_pitch = __min(+XM_PI / 2.0f, m_pitch);
```

如果没有射击视区功能，游戏的控件是不完整的！


此 **UpdatePollingDevices** 方法还会检查是否按下了正确的触发器。 如果正确，我们的 **m_firePressed** 属性将翻转为 true，通知游戏应开始射击视区。
```cpp
    if (reading.RightTrigger > TRIGGER_DEADZONE)
    {
        if (!m_autoFire && !m_gamepadTriggerInUse)
        {
            m_firePressed = true;
        }

        m_gamepadTriggerInUse = true;
    }
    else
    {
        m_gamepadTriggerInUse = false;
    }
```


## <a name="the-update-method"></a>Update 方法

最后作个收尾，我们来深入了解一下 [**Update**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L1005-L1096) 方法。
此方法合并玩家使用任何受支持的输入执行的任何移动或旋转，以生成速度矢量并更新用于访问游戏循环的俯仰和偏航值。


**Update** 方法通过调用 [**UpdatePollingDevices**](#the-updatepollingdevices-method) 启动，以更新控制器的状态。 此方法还收集游戏板的所有输入，并将其移动添加到 **m_moveCommand** 矢量。 

在我们的 **Update** 方法中，我们随后执行了以下输入检查。
- 如果玩家使用移动控制器矩形，我们随后将确定指针位置的变化，并使用此信息计算用户是否将指针移出了控制器的死区。 如果移出，**m_moveCommand** 矢量属性随后将更新为虚拟游戏杆值。
- 如果按下任何移动键盘输入，则会将 `1.0f` 或 `-1.0f` 的值添加到**m_moveCommand**向量的相应分量中 &mdash; "转发"，将 "`1.0f`" 添加到 "反向"。


考虑了所有移动输入后，我们通过一些计算来运行 **m_moveCommand** 矢量以生成新矢量，用于表示与游戏世界有关的玩家方向。
然后，我们执行与该世界有关的移动，并随着这个方向的速度对玩家应用这些移动。
最后，我们将 **m_moveCommand** 矢量重置为 `(0.0f, 0.0f, 0.0f)` 以为下一个游戏帧作好一切准备。


```cpp
void MoveLookController::Update()
{
    // Get any gamepad input and update state
    UpdatePollingDevices();

    // Get change in 
    if (m_moveInUse)
    {
        XMFLOAT2 pointerDelta;

        pointerDelta.x = m_movePointerPosition.x - m_moveFirstDown.x;
        pointerDelta.y = m_movePointerPosition.y - m_moveFirstDown.y;

        // Figure out the command from the virtual joystick.
        XMFLOAT3 commandDirection = XMFLOAT3(0.0f, 0.0f, 0.0f);
        if (fabsf(pointerDelta.x) > 16.0f)         // Leave 32 pixel-wide dead spot for being still.
            m_moveCommand.x -= pointerDelta.x/fabsf(pointerDelta.x);

        if (fabsf(pointerDelta.y) > 16.0f)
            m_moveCommand.y -= pointerDelta.y/fabsf(pointerDelta.y);
    }

    // Poll our state bits set by the keyboard input events.
    if (m_forward)
    {
        m_moveCommand.y += 1.0f;
    }
    if (m_back)
    {
        m_moveCommand.y -= 1.0f;
    }
    if (m_left)
    {
        m_moveCommand.x += 1.0f;
    }
    if (m_right)
    {
        m_moveCommand.x -= 1.0f;
    }
    if (m_up)
    {
        m_moveCommand.z += 1.0f;
    }
    if (m_down)
    {
        m_moveCommand.z -= 1.0f;
    }

    // Make sure that 45deg cases are not faster.
    if (fabsf(m_moveCommand.x) > 0.1f ||
        fabsf(m_moveCommand.y) > 0.1f ||
        fabsf(m_moveCommand.z) > 0.1f)
    {
        XMStoreFloat3(&m_moveCommand, XMVector3Normalize(XMLoadFloat3(&m_moveCommand)));
    }

    // Rotate command to align with our direction (world coordinates).
    XMFLOAT3 wCommand;
    wCommand.x =  m_moveCommand.x * cosf(m_yaw) - m_moveCommand.y * sinf(m_yaw);
    wCommand.y =  m_moveCommand.x * sinf(m_yaw) + m_moveCommand.y * cosf(m_yaw);
    wCommand.z =  m_moveCommand.z;

    // Scale for sensitivity adjustment.
    // Our velocity is based on the command. Y is up.
    m_velocity.x = -wCommand.x * MoveLookConstants::MovementGain;
    m_velocity.z =  wCommand.y * MoveLookConstants::MovementGain;
    m_velocity.y =  wCommand.z * MoveLookConstants::MovementGain;

    // Clear movement input accumulator for use during next frame.
    m_moveCommand = XMFLOAT3(0.0f, 0.0f, 0.0f);
}
```


## <a name="next-steps"></a>后续步骤

现在，我们已经添加了控件，我们还需要添加另一项功能来创建沉浸式游戏：声音！
对于任何游戏而言，音乐和声音效果都非常重要，下面我们讨论[添加声音](tutorial--adding-sound.md)。
 

 

 




