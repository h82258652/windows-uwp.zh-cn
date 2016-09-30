---
author: mtoepke
title: "添加控件"
description: "现在，我们了解该游戏示例如何在 3D 游戏中实现移动观看控件，以及如何开发基本的触摸、鼠标和游戏控制器控件。"
ms.assetid: f9666abb-151a-74b4-ae0b-ef88f1f252f8
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: b3297ffd92d9a61d73c574def7e8101dc9196a69

---

# 添加控件


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 的文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

现在，我们了解该游戏示例如何在 3D 游戏中实现移动观看控件，以及如何开发基本的触摸、鼠标和游戏控制器控件。

## 目标


-   在使用 DirectX 的通用 Windows 平台 (UWP) 游戏中实现鼠标/键盘、触摸和 Xbox 控制器控件。

## UWP 游戏应用和控件


优秀的 UWP 游戏支持多种界面。 潜在玩家可能将 Windows 10 安装在没有物理按钮的平板电脑、 连接了 Xbox 控制器的媒体电脑，或者具有高性能鼠标和游戏键盘的最新桌面游戏设备上。 如果游戏设计允许，你的游戏应支持所有这些设备。

此示例支持所有这三种设备。 这是一个简单的第一人称射击游戏，适用于此类游戏的标准移动观看控件对于这三种形式的输入很容易实现。

有关控件 （尤其是移动观看控件）的详细信息， 请参阅[适用于游戏的移动观看控件](tutorial--adding-move-look-controls-to-your-directx-game.md) 和[适用于游戏的触摸控件](tutorial--adding-touch-controls-to-your-directx-game.md)。

## 常用控件行为


触摸控件和鼠标/键盘控件具有非常类似的核心实现。 在 UWP 应用中，指针只是屏幕上的点。 你可以滑动鼠标或在触摸屏上滑动手指来移动指针。 因此，你可以注册单个事件集，而不用担心玩家使用鼠标或触摸屏移动或点按指针。

在初始化该游戏示例中的 **MoveLookController** 类时，将注册四个特定于指针的事件和一个特定于鼠标的事件：

-   [**CoreWindow::PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208278)。 点按（并按住）左右鼠标按钮，或触摸触摸屏表面。
-   [**CoreWindow::PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208276)。 移动鼠标，或在触摸表面执行拖动操作。
-   [**CoreWindow::PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208279)。 释放鼠标左按钮，或者移开与触摸表面接触的物体。
-   [**CoreWindow::PointerExited**](https://msdn.microsoft.com/library/windows/apps/br208275)。 指针移出主窗口。
-   [**Windows::Devices::Input::MouseMoved**](https://msdn.microsoft.com/library/windows/apps/hh758356)。 鼠标移动了一定距离。 注意，我们只关注鼠标移动增量值，不关注当前 x-y 位置。

```cpp
void MoveLookController::Initialize(
    _In_ CoreWindow^ window
    )
{
    window->PointerPressed +=
        ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerPressed);

    window->PointerMoved +=
        ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerMoved);

    window->PointerReleased +=
        ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerReleased);

    window->PointerExited +=
        ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerExited);

    window->KeyDown +=
        ref new TypedEventHandler<CoreWindow^, KeyEventArgs^>(this, &MoveLookController::OnKeyDown);

    window->KeyUp +=
        ref new TypedEventHandler<CoreWindow^, KeyEventArgs^>(this, &MoveLookController::OnKeyUp);

    // A separate handler for mouse only relative mouse movement events.
    Windows::Devices::Input::MouseDevice::GetForCurrentView()->MouseMoved +=
        ref new TypedEventHandler<MouseDevice^, MouseEventArgs^>(this, &MoveLookController::OnMouseMoved);

    ResetState();
    m_state = MoveLookControllerState::None;

    m_pitch = 0.0f;
    m_yaw   = 0.0f;
}
```

使用 [XInput](https://msdn.microsoft.com/library/windows/desktop/hh405053) API 单独处理 Xbox 控制器。 稍后我们将讨论游戏控制器控件的实现。

在该游戏示例中，**MoveLookController** 类有三个特定于控制器的状态（与控件类型无关）：

-   **None**。 这是控制器的初始化状态。 游戏不期待任何控制器输入。
-   **WaitForInput**。 游戏暂停并等待玩家继续。
-   **Active**。 游戏正在运行，在处理玩家输入。

**Active** 状态是玩家主动玩游戏时的状态。 在此状态期间，**MoveLookController** 实例处理来自所有启用的输入设备的输入事件，并基于累积的事件数据解释玩家的意图。 因此，在从游戏循环中调用更新之后，它会更新玩家视野的速度和观看方向（视野平面法向），并与游戏共享更新数据。

注意，玩家可以同时执行多个操作。 例如，玩家可以在移动相机时射击球体。 所有这些输入都在 **Active** 状态中跟踪，不同的指针 ID 对应于不同的指针操作。 这非常有必要，因为从玩家角度看，射击矩形中的指针事件与移动矩形或屏幕其余位置的指针事件不同。

当收到 [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208278) 事件时，**MoveLookController** 将获取窗口创建的指针 ID 值。 指针 ID 表示特定的输入类型。 例如，在多点触控设备上，可能同时有若干不同的主动输入。 ID 用于跟踪玩家在使用哪个输入。 如果触摸屏的移动矩形中有一个事件，将指派一个指针 ID 跟踪移动矩形中的任何指针事件。 射击矩形中的其他指针事件则使用单独的指针 ID 进行单独跟踪。 （我们将在触摸控件部分中详细讨论此内容。）

来自鼠标的输入还有一个 ID，同样单独处理。

在将指针事件映射到特定的游戏操作之后，可以更新 **MoveLookController** 对象与主游戏循环共享的数据。

在调用游戏示例中的 **Update** 方法时，该方法将处理输入并更新速度和观看方向变量（**m\_velocity** 和 **m\_lookdirection**），然后游戏循环通过调用 **MoveLookController** 实例上的公用 **Velocity** 和 **LookDirection** 方法检索这些变量。

```cpp
void MoveLookController::Update()
{
    UpdateGameController();

    if (m_moveInUse)
    {
        // Move control.
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
    // Our velocity is based on the command, y is up.
    m_velocity.x = -wCommand.x * MOVEMENT_GAIN;
    m_velocity.z =  wCommand.y * MOVEMENT_GAIN;
    m_velocity.y =  wCommand.z * MOVEMENT_GAIN;

    // Clear movement input accumulator for use during next frame.
    m_moveCommand = XMFLOAT3(0.0f, 0.0f, 0.0f);
}
```

游戏循环可以通过在 **MoveLookController** 实例上调用 **IsFiring** 方法测试玩家是否正在射击。 **MoveLookController** 通过检查了解玩家是否已在三种输入类型之一上按下射击按钮。

```cpp
bool MoveLookController::IsFiring()
{
    if (m_state == MoveLookControllerState::Active)
    {
        if (m_autoFire)
        {
            return (m_fireInUse || (m_mouseInUse && m_mouseLeftInUse) || m_xinputTriggerInUse);
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

如果玩家将指针移出游戏主窗口或者按下暂停按钮（P 键或 Xbox 控制器开始按钮），则游戏必须暂停。 **MoveLookController** 已注册点按操作，并在它调用 **IsPauseRequested** 方法时通知游戏循环。 此时，如果 **IsPauseRequested** 返回 **true**，游戏循环将对 **MoveLookController** 调用 **WaitForPress** 以将控制器转入 **WaitForInput** 状态。 然后，**MoveLookController** 等待玩家选择菜单项之一来加载、继续或退出游戏，并停止处理游戏播放输入事件，直到它返回 **Active** 状态。

请参阅[本部分的完整代码示例](#code_sample)。

现在，我们将较为详细地讨论这三种控件类型中每个类型的实现。

## 实现相对鼠标控件


如果检测到鼠标移动，我们希望利用该移动确定相机的新的俯仰和偏航。 我们通过实现相对鼠标控件实现此目的，在该控件中我们处理鼠标所移动的相对距离（移动开始到停止之间的增量），与记录运动的绝对 x-y 像素坐标相反。

为此，我们通过检查由 [**MouseMoved**](https://msdn.microsoft.com/library/windows/apps/hh758356) 事件返回的 [**Windows::Device::Input::MouseEventArgs::MouseDelta**](https://msdn.microsoft.com/library/windows/apps/hh758358) 参数对象上的 [**MouseDelta::X**](https://msdn.microsoft.com/library/windows/apps/hh758353) 和 **MouseDelta::Y** 字段获取 X（水平运动）和 Y（垂直运动）坐标的变化。

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
        rotationDelta.x  = mouseDelta.x * ROTATION_GAIN;   // scale for control sensitivity
        rotationDelta.y  = mouseDelta.y * ROTATION_GAIN;

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

## 实现触摸控件


触摸控件的开发最需要技巧，因为它们最复杂，需要最精细的调整才能奏效。 在该游戏示例中，屏幕左下区的矩形用作方向板，在此空间左右滑动拇指可左右移动相机，上下滑动拇指可前后移动相机。 按屏幕右下区的矩形可以对视区射击。 瞄准（俯仰和偏航）通过在屏幕区域（为移动和射击保留的屏幕区域除外）上滑动手指控制；在移动手指时，相机（具有固定的十字准线）将执行类似的移动。

移动和射击矩形通过示例代码中的两个方法创建：

```cpp
void SetMoveRect(
        _In_ DirectX::XMFLOAT2 upperLeft,
        _In_ DirectX::XMFLOAT2 lowerRight
        );
 void SetFireRect(
        _In_ DirectX::XMFLOAT2 upperLeft,
        _In_ DirectX::XMFLOAT2 lowerRight
        );
```

我们将屏幕其他区域的触摸设备指针事件视为观看命令。 如果屏幕调整了大小，则必须重新计算（和重新绘制）这些矩形。

如果在其中一个区域中引发触摸设备指针事件并且游戏状态设置为 **Active**，则为其分配一个指针 ID，如我们之前所述。

```cpp
void MoveLookController::OnPointerPressed(
    _In_ CoreWindow^ sender,
    _In_ PointerEventArgs^ args
    )
{
    PointerPoint^ point = args->CurrentPoint;
    UINT32 pointerID = point->PointerId;
    Point pointerPosition = point->Position;
    PointerPointProperties^ pointProperties = point->Properties;
    auto pointerDevice = point->PointerDevice;
    auto pointerDeviceType = pointerDevice->PointerDeviceType;

    XMFLOAT2 position = XMFLOAT2(pointerPosition.X, pointerPosition.Y);     // convert to allow math

    switch (m_state)
    {
    case MoveLookControllerState::WaitForInput:
        // ...
        // Game is paused, wait for click inside the game window.
        // ...
        break;

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
                    m_moveFirstDown = position;                 // Save the location of the initial contact.
                    m_movePointerID = pointerID;                // Store the pointer using this control.
                    m_moveInUse = true;
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
                    m_fireLastPoint = position;
                    m_firePointerID = pointerID;
                    m_fireInUse = true;
                }
            }
            else
            {
                if (!m_lookInUse)   // If no pointer is in this control yet:
                {
                    m_lookLastPoint = position;                   // Save the pointer for a later move.
                    m_lookPointerID = pointerID;                  // Store the pointer using this control.
                    m_lookLastDelta.x = m_lookLastDelta.y = 0;    // These are for smoothing.
                    m_lookInUse = true;
                }
            }
            break;

        default:
            // ...
            // Handle mouse input here.
                                                // ...
            break;
        }
        break;
    }
    return;
}
```

如果这三个控制区域，即移动矩形、射击矩形或屏幕其余部分（观看控制区）之一发生 [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208278) 事件，则 **MoveLookController** 会将触发该事件的指针的指针 ID 分配给与触发该事件的屏幕区域对应的特定变量。 例如，如果该事件发生在移动矩形中，则将 **m\_movePointerID** 变量设置为触发该事件的指针 ID。 还将设置布尔值“in use”变量（在本示例中为 **m\_lookInUse**）以指示尚未释放控件。

现在，我们讨论游戏示例如何处理 [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208276) 触摸屏事件。

```cpp
void MoveLookController::OnPointerMoved(
    _In_ CoreWindow^ sender,
    _In_ PointerEventArgs^ args
    )
{
    PointerPoint^ point = args->CurrentPoint;
    UINT32 pointerID = point->PointerId;
    Point pointerPosition = point->Position;
    PointerPointProperties^ pointProperties = point->Properties;
    auto pointerDevice = point->PointerDevice;
    auto pointerDeviceType = pointerDevice->PointerDeviceType;

    XMFLOAT2 position = XMFLOAT2(pointerPosition.X, pointerPosition.Y);     // Convert to allow math.

    switch (m_state)
    {
    case MoveLookControllerState::Active:
        // Decide which control this pointer is operating.
        if (pointerID == m_movePointerID)     // This is the move pointer.
        {
            m_movePointerPosition = position;       // Save the current position.
        }
        else if (pointerID == m_lookPointerID)     // This is the look pointer.
        {
            // Look control
            XMFLOAT2 pointerDelta;
            pointerDelta.x = position.x - m_lookLastPoint.x;        // How far did the pointer move.
            pointerDelta.y = position.y - m_lookLastPoint.y;

            XMFLOAT2 rotationDelta;
            rotationDelta.x = pointerDelta.x * ROTATION_GAIN;       // Scale for control sensitivity.
            rotationDelta.y = pointerDelta.y * ROTATION_GAIN;
            m_lookLastPoint = position;                             // Save for the next time through.

            // Update our orientation based on the command.
            m_pitch -= rotationDelta.y;
            m_yaw   += rotationDelta.x;

            // Limit pitch to straight up or straight down.
            m_pitch = __max(-XM_PI / 2.0f, m_pitch);
            m_pitch = __min(+XM_PI / 2.0f, m_pitch);
        }
        else if (pointerID == m_firePointerID)
        {
            m_fireLastPoint = position;
        }
        else if (pointerID == m_mousePointerID)
        {
            // ...
        }
        break;
    }
}
```

**MoveLookController** 检查指针 ID 以确定发生事件的位置，并采取以下操作之一：

-   如果在移动或射击矩形中发生了 [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208276) 事件，则更新控制器的指针位置。
-   如果在屏幕的其他位置（定义为观看控制区）发生了 [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208276) 事件，则计算观看方向矢量的俯仰和偏航变化。

最后，我们讨论游戏示例如何处理 [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208279) 触摸屏事件。

```cpp
void MoveLookController::OnPointerReleased(
    _In_ CoreWindow^ sender,
    _In_ PointerEventArgs^ args
    )
{
    PointerPoint^ point = args->CurrentPoint;
    UINT32 pointerID = point->PointerId;
    Point pointerPosition = point->Position;
    PointerPointProperties^ pointProperties = point->Properties;

    XMFLOAT2 position = XMFLOAT2(pointerPosition.X, pointerPosition.Y);     // Convert to allow math.
    switch (m_state)
    {
    case MoveLookControllerState::WaitForInput:
        if (m_buttonInUse && (pointerID == m_buttonPointerID))
        {
            m_buttonInUse = false;
            m_buttonPressed = true;
        }
        break;

    case MoveLookControllerState::Active:
        if (pointerID == m_movePointerID)
        {
            m_velocity = XMFLOAT3(0, 0, 0);      // Stop on release.
            m_moveInUse = false;
            m_movePointerID = 0;
        }
        else if (pointerID == m_lookPointerID)
        {
            m_lookInUse = false;
            m_lookPointerID = 0;
        }
        else if (pointerID == m_firePointerID)
        {
            m_fireInUse = false;
            m_firePointerID = 0;
        }
        else if (pointerID == m_mousePointerID)
        {
            // ...
        }
        break;
    }
}
```

如果触发 [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208279) 事件的指针的指针 ID 是以前记录的移动指针的 ID，则 **MoveLookController** 将速度设置为 0，这是因为玩家已停止触摸移动矩形。 如果未将速度设置为 0，则玩家仍在移动！ 如果你希望实现一定形式的惯性，可以在此处添加随着将来从游戏循环调用 **Update** 开始将速度返回到 0 的方法。

否则，如果在射击矩形或观看区域触发 [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208279) 事件，**MoveLookController** 将重置特定的指针 ID。

这便是如何在游戏示例中实现触摸屏控件的基础知识。 现在我们继续了解鼠标和键盘控件。

## 实现鼠标和键盘控件


游戏示例实现如下鼠标和键盘控件：

-   W、S、A 和 D 键分别向前、后、左、右移动玩家视图。 按 X 和空格键可分别向上、向下移动视图。
-   按 P 键暂停游戏。
-   移动鼠标可让玩家控制相机视图的旋转（俯仰和偏航）。
-   单击左按钮可以射击视区。

若要使用键盘，游戏示例需注册两个额外事件：[**CoreWindow::KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208271) 和 [**CoreWindow::KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208270)，这两个事件分别处理按下和释放按键。

```cpp
window->KeyDown +=
        ref new TypedEventHandler<CoreWindow^, KeyEventArgs^>(this, &MoveLookController::OnKeyDown);

window->KeyUp +=
        ref new TypedEventHandler<CoreWindow^, KeyEventArgs^>(this, &MoveLookController::OnKeyUp);
```

尽管鼠标使用的也是指针，但它的处理方式与触摸控件稍有不同。 显然，它不使用移动和射击矩形，因为这对于玩家来说太不方便：如何能够同时按下移动和射击控件？ 刚才说过，**MoveLookController** 控制器在鼠标移动时将预定观看控制区，在按下鼠标左键时将预定射击控制区，如此处所示。

```cpp
void MoveLookController::OnPointerPressed(
    _In_ CoreWindow^ /* sender */,
    _In_ PointerEventArgs^ args
    )
{
    PointerPoint^ point = args->CurrentPoint;
    uint32 pointerID = point->PointerId;
    Point pointerPosition = point->Position;
    PointerPointProperties^ pointProperties = point->Properties;
    auto pointerDevice = point->PointerDevice;
    auto pointerDeviceType = pointerDevice->PointerDeviceType;

    XMFLOAT2 position = XMFLOAT2(pointerPosition.X, pointerPosition.Y);     // convert to allow math

    switch (m_state)
    {
    case MoveLookControllerState::WaitForInput:
        if (position.x > m_buttonUpperLeft.x &&
            position.x < m_buttonLowerRight.x &&
            position.y > m_buttonUpperLeft.y &&
            position.y < m_buttonLowerRight.y)
        {
            // Wait until the button is released before setting the variable.
            m_buttonPointerID = pointerID;
            m_buttonInUse = true;

        }
        break;

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
                    m_moveFirstDown = position;                 // Save the location of the initial contact.
                    m_movePointerID = pointerID;                // Store the pointer using this control.
                    m_moveInUse = true;
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
                    m_fireLastPoint = position;
                    m_firePointerID = pointerID;
                    m_fireInUse = true;
                    if (!m_autoFire)
                    {
                        m_firePressed = true;
                    }
                }
            }
            else
            {
                if (!m_lookInUse)   // If no pointer is in this control yet:
                {
                    m_lookLastPoint = position;                   // Save the pointer for a later move.
                    m_lookPointerID = pointerID;                  // Store the pointer using this control.
                    m_lookLastDelta.x = m_lookLastDelta.y = 0;    // These are for smoothing.
                    m_lookInUse = true;
                }
            }
            break;

        default:
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
            else
            {

            }
            break;
        }

        break;
    }

    return;
}
```

现在，我们讨论该游戏示例如何处理 [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208279) 鼠标事件。

```cpp
void MoveLookController::OnPointerReleased(
    _In_ CoreWindow^ /* sender */,
    _In_ PointerEventArgs^ args
    )
{
    PointerPoint^ point = args->CurrentPoint;
    uint32 pointerID = point->PointerId;
    Point pointerPosition = point->Position;
    PointerPointProperties^ pointProperties = point->Properties;

    XMFLOAT2 position = XMFLOAT2(pointerPosition.X, pointerPosition.Y);     // Convert to allow math.
    switch (m_state)
    {
    case MoveLookControllerState::WaitForInput:
        if (m_buttonInUse && (pointerID == m_buttonPointerID))
        {
            m_buttonInUse = false;
            m_buttonPressed = true;
        }
        break;

    case MoveLookControllerState::Active:
        if (pointerID == m_movePointerID)
        {
            m_velocity = XMFLOAT3(0, 0, 0);      // Stop on release.
            m_moveInUse = false;
            m_movePointerID = 0;
        }
        else if (pointerID == m_lookPointerID)
        {
            m_lookInUse = false;
            m_lookPointerID = 0;
        }
        else if (pointerID == m_firePointerID)
        {
            m_fireInUse = false;
            m_firePointerID = 0;
        }
        else if (pointerID == m_mousePointerID)
        {
            bool rightButton = pointProperties->IsRightButtonPressed;
            bool leftButton = pointProperties->IsLeftButtonPressed;

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

当玩家停止按其中一个鼠标按钮时，输入即完成：视区停止射击。 但是，由于始终启用观看， 所以游戏将继续使用同一鼠标指针跟踪正在进行的观看事件。

现在，我们讨论最后一个控制类型：Xbox 控制器。 它独立于触摸控件和鼠标控件进行处理，因为它不使用指针对象。

## 实现 Xbox 控制器控件


在游戏示例中，Xbox 控制器支持是通过调用 [XInput](https://msdn.microsoft.com/library/windows/desktop/hh405053) API 添加的， 这是一组旨在简化游戏控制器编程工作的 API。 在游戏示例中，我们将 Xbox 控制器的左摇杆用于玩家移动， 将右摇杆用于观看控件，将右触发器用于射击。 我们使用开始按钮暂停和恢复游戏。

**MoveLookController** 实例上的 **Update** 方法立即检查游戏控制器是否已连接，然后检查控制器状态。

```cpp
void MoveLookController::UpdateGameController()
{
    if (!m_isControllerConnected)
    {
        // Check for controller connection by trying to get the capabilties.
        DWORD capsResult = XInputGetCapabilities(0, XINPUT_FLAG_GAMEPAD, &m_xinputCaps);
        if (capsResult != ERROR_SUCCESS)
        {
            return;
        }
        // Device is connected.
        m_isControllerConnected = true;
        m_xinputStartButtonInUse = false;
        m_xinputTriggerInUse = false;
    }

    DWORD stateResult = XInputGetState(0, &m_xinputState);
    if (stateResult != ERROR_SUCCESS)
    {
        // Device is no longer connected.
        m_isControllerConnected = false;
    }

    switch (m_state)
    {
    case MoveLookControllerState::WaitForInput:
        if (m_xinputState.Gamepad.wButtons & XINPUT_GAMEPAD_START)
        {
            m_xinputStartButtonInUse = true;
        }
        else if (m_xinputStartButtonInUse)
        {
            // Trigger one time only on button release.
            m_xinputStartButtonInUse = false;
            m_buttonPressed = true;
        }
        break;

    case MoveLookControllerState::Active:
        if (m_xinputState.Gamepad.wButtons & XINPUT_GAMEPAD_START)
        {
            m_xinputStartButtonInUse = true;
        }
        else if (m_xinputStartButtonInUse)
        {
            // Trigger one time only on button release.
            m_xinputStartButtonInUse = false;
            m_pausePressed = true;
        }
        // Use the Right Thumb joystick on the XBox controller to control
        // the eye point position control.
        // The controller input goes from -32767 to 32767.   We will normalize
        // this from -1 to 1 and keep a dead spot in the middle to avoid drift.

        if (m_xinputState.Gamepad.sThumbLX > XINPUT_GAMEPAD_LEFT_THUMB_DEADZONE ||
            m_xinputState.Gamepad.sThumbLX < -XINPUT_GAMEPAD_LEFT_THUMB_DEADZONE)
        {
            float x = (float)m_xinputState.Gamepad.sThumbLX/32767.0f;
            m_moveCommand.x -= x / fabsf(x);
        }

        if (m_xinputState.Gamepad.sThumbLY > XINPUT_GAMEPAD_LEFT_THUMB_DEADZONE ||
            m_xinputState.Gamepad.sThumbLY < -XINPUT_GAMEPAD_LEFT_THUMB_DEADZONE)
        {
            float y = (float)m_xinputState.Gamepad.sThumbLY/32767.0f;
            m_moveCommand.y += y / fabsf(y);
        }

        // Use the Left Thumb Joystick on the XBox controller to control
        // the look at control.
        // The controller input goes from -32767 to 32767.   We will normalize
        // this from -1 to 1 and keep a dead spot in the middle to avoid drift.
        XMFLOAT2 pointerDelta;
        if (m_xinputState.Gamepad.sThumbRX > XINPUT_GAMEPAD_RIGHT_THUMB_DEADZONE ||
            m_xinputState.Gamepad.sThumbRX < -XINPUT_GAMEPAD_RIGHT_THUMB_DEADZONE)
        {
            pointerDelta.x = (float)m_xinputState.Gamepad.sThumbRX/32767.0f;
            pointerDelta.x = pointerDelta.x * pointerDelta.x * pointerDelta.x;
        }
        else
        {
            pointerDelta.x = 0.0f;
        }
        if (m_xinputState.Gamepad.sThumbRY > XINPUT_GAMEPAD_RIGHT_THUMB_DEADZONE ||
            m_xinputState.Gamepad.sThumbRY < -XINPUT_GAMEPAD_RIGHT_THUMB_DEADZONE)
        {
            pointerDelta.y = (float)m_xinputState.Gamepad.sThumbRY/32767.0f;
            pointerDelta.y = pointerDelta.y * pointerDelta.y * pointerDelta.y;
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
        m_yaw   += rotationDelta.x;

        // Limit pitch to straight up or straight down.
        m_pitch = __max(-XM_PI / 2.0f, m_pitch);
        m_pitch = __min(+XM_PI / 2.0f, m_pitch);

        // Check the state of the A button.  This is used to indicate fire control.

        if (m_xinputState.Gamepad.bRightTrigger > XINPUT_GAMEPAD_TRIGGER_THRESHOLD)
        {
            if (!m_autoFire && !m_xinputTriggerInUse)
            {
                m_firePressed = true;
            }
            m_xinputTriggerInUse = true;
        }
        else
        {
            m_xinputTriggerInUse = false;
        }
        break;
    }
}
```

如果游戏控制器处于 **Active** 状态，此方法将检查用户是否向特定方向移动了左摇杆。 但是，向特定方向移动摇杆必须表现为大于死区的半径；否则，不会发生任何操作。 此死区半径需要表示“漂移”，当控制器从玩家放在摇杆的拇指上拾取分钟移动时，将呈现“漂移”。 如果没有此死区，玩家可能会很快厌烦，因为控制的感觉非常不稳定。

然后，**Update** 方法对右摇杆执行相同的检查，了解玩家是否更改了相机的面对方向，只要摇杆上的移动大于另一死区半径即可。

**Update** 计算新俯仰和偏航，然后了解用户按下了右摇杆还是射击按钮。

这就是此示例实现一组完整的控制选项的方式。 此外，请记住，一个优秀的 UWP 应用将支持各种控制选项， 以便使用不同外观设置和设备的玩家都能够用他们喜欢的方式来玩！

## 后续步骤


我们已查看了 UWP DirectX 游戏的每个主要组件，但音频除外！ 对于任何游戏而言，音乐和声音效果都非常重要， 下面我们讨论[添加声音](tutorial--adding-sound.md)！

## 这部分的完整示例代码


MoveLookController.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

// Uncomment to print debug tracing.
// #define MOVELOOKCONTROLLER_TRACE 1

enum class MoveLookControllerState
{
    None,
    WaitForInput,
    Active,
};

ref class MoveLookController
{
internal:
    MoveLookController();

    void Initialize(
        _In_ Windows::UI::Core::CoreWindow^ window
        );
    void SetMoveRect(
        _In_ DirectX::XMFLOAT2 upperLeft,
        _In_ DirectX::XMFLOAT2 lowerRight
        );
    void SetFireRect(
        _In_ DirectX::XMFLOAT2 upperLeft,
        _In_ DirectX::XMFLOAT2 lowerRight
        );
    void WaitForPress(
        _In_ DirectX::XMFLOAT2 UpperLeft,
        _In_ DirectX::XMFLOAT2 LowerRight
        );
    void WaitForPress();

    void Update();
    bool IsFiring();
    bool IsPressComplete();
    bool IsPauseRequested();

    DirectX::XMFLOAT3 Velocity();
    DirectX::XMFLOAT3 LookDirection();
    float Pitch();
    void  Pitch(_In_ float pitch);
    float Yaw();
    void  Yaw(_In_ float yaw);
    bool  Active();
    void  Active(_In_ bool active);

    bool AutoFire();
    void AutoFire(_In_ bool AutoFire);

protected:
    void OnPointerPressed(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::PointerEventArgs^ args
        );
    void OnPointerMoved(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::PointerEventArgs^ args
        );
    void OnPointerReleased(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::PointerEventArgs^ args
        );
    void OnPointerExited(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::PointerEventArgs^ args
        );
    void OnKeyDown(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::KeyEventArgs^ args
        );
    void OnKeyUp(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::KeyEventArgs^ args
        );

    void OnMouseMoved(
        _In_ Windows::Devices::Input::MouseDevice^ mouseDevice,
        _In_ Windows::Devices::Input::MouseEventArgs^ args
        );

#ifdef MOVELOOKCONTROLLER_TRACE
    void DebugTrace(const wchar_t *format, ...);
#endif

private:
    // Properties of the controller object
    MoveLookControllerState     m_state;
    DirectX::XMFLOAT3           m_velocity;             // How far we move it this frame
    float                       m_pitch;
    float                       m_yaw;                  // Orientation euler angles in radians

    // Properties of the Move control
    DirectX::XMFLOAT2           m_moveUpperLeft;        // Bounding box where this control will activate
    DirectX::XMFLOAT2           m_moveLowerRight;
    bool                        m_moveInUse;            // The move control is in use.
    uint32                      m_movePointerID;        // The id of the pointer in this control.
    DirectX::XMFLOAT2           m_moveFirstDown;        // The point where the initial contact occurred.
    DirectX::XMFLOAT2           m_movePointerPosition;  // The point where the move pointer is currently located.
    DirectX::XMFLOAT3           m_moveCommand;          // The net command from the move control.

    // Properties of the Look control
    bool                        m_lookInUse;            // The look control is in use.
    uint32                      m_lookPointerID;        // The id of the pointer in this control.
    DirectX::XMFLOAT2           m_lookLastPoint;        // The last point (from last frame)
    DirectX::XMFLOAT2           m_lookLastDelta;        // for smoothing.

    // Properties of the Fire control
    bool                        m_autoFire;
    bool                        m_firePressed;
    DirectX::XMFLOAT2           m_fireUpperLeft;        // The bounding box where this control will activate.
    DirectX::XMFLOAT2           m_fireLowerRight;
    bool                        m_fireInUse;            // The fire control is in use.
    UINT32                      m_firePointerID;        // The id of the pointer in this control.
    DirectX::XMFLOAT2           m_fireLastPoint;        // The last fire position.

    // Properties of the Mouse control. This is a combination of Look and Fire.
    bool                        m_mouseInUse;
    uint32                      m_mousePointerID;
    DirectX::XMFLOAT2           m_mouseLastPoint;
    bool                        m_mouseLeftInUse;
    bool                        m_mouseRightInUse;

    bool                        m_buttonInUse;
    uint32                      m_buttonPointerID;
    DirectX::XMFLOAT2           m_buttonUpperLeft;
    DirectX::XMFLOAT2           m_buttonLowerRight;
    bool                        m_buttonPressed;
    bool                        m_pausePressed;

    // XBox Input related members
    bool                        m_isControllerConnected;  // Do we have a controller connected?
    XINPUT_CAPABILITIES         m_xinputCaps;             // The capabilites of the controller.
    XINPUT_STATE                m_xinputState;            // The current state of the controller.
    bool                        m_xinputStartButtonInUse;
    bool                        m_xinputTriggerInUse;

    // Input states for Keyboard
    bool                        m_forward;
    bool                        m_back;                    // States for movement
    bool                        m_left;
    bool                        m_right;
    bool                        m_up;
    bool                        m_down;
    bool                        m_pause;

private:
    void     ResetState();
    void     UpdateGameController();
};
```

MoveLookController.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "MoveLookController.h"
#include "DirectXSample.h"

using namespace Windows::UI::Core;
using namespace Windows::UI::Input;
using namespace Windows::UI;
using namespace Windows::Foundation;
using namespace Microsoft::WRL;
using namespace DirectX;
using namespace Windows::Devices::Input;
using namespace Windows::System;

#define ROTATION_GAIN 0.008f        // The sensitivity adjustment for the look controller.
#define MOVEMENT_GAIN 2.f           // The sensitivity adjustment for the move controller.

// A basic Move/Look Controller class such as in an FPS
// horizontal (x-z-plane) movement on left virtual joystick
// also supports WASD keyboard input
// steering and orientation via left mouse down or touch drag.

//----------------------------------------------------------------------

MoveLookController::MoveLookController():
    m_autoFire(true),
    m_isControllerConnected(false)
{
}

//----------------------------------------------------------------------
// Set up the controls supported by this controller.

void MoveLookController::Initialize(
    _In_ CoreWindow^ window
    )
{
    window->PointerPressed +=
        ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerPressed);

    window->PointerMoved +=
        ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerMoved);

    window->PointerReleased +=
        ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerReleased);

    window->PointerExited +=
        ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerExited);

    window->KeyDown +=
        ref new TypedEventHandler<CoreWindow^, KeyEventArgs^>(this, &MoveLookController::OnKeyDown);

    window->KeyUp +=
        ref new TypedEventHandler<CoreWindow^, KeyEventArgs^>(this, &MoveLookController::OnKeyUp);

    // A separate handler for mouse only relative mouse movement events.
    Windows::Devices::Input::MouseDevice::GetForCurrentView()->MouseMoved +=
        ref new TypedEventHandler<MouseDevice^, MouseEventArgs^>(this, &MoveLookController::OnMouseMoved);

    ResetState();
    m_state = MoveLookControllerState::None;

    m_pitch = 0.0f;
    m_yaw   = 0.0f;
}

//----------------------------------------------------------------------

bool MoveLookController::IsPauseRequested()
{
    switch (m_state)
    {
    case MoveLookControllerState::Active:
        UpdateGameController();
        if (m_pausePressed)
        {

            m_pausePressed = false;
            return true;
        }
        else
        {
            return false;
        }
    }
    return false;
}

//----------------------------------------------------------------------

bool MoveLookController::IsFiring()
{
    if (m_state == MoveLookControllerState::Active)
    {
        if (m_autoFire)
        {
            return (m_fireInUse || (m_mouseInUse && m_mouseLeftInUse) || m_xinputTriggerInUse);
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

//----------------------------------------------------------------------

bool MoveLookController::IsPressComplete()
{
    switch (m_state)
    {
    case MoveLookControllerState::WaitForInput:
        UpdateGameController();
        if (m_buttonPressed)
        {

            m_buttonPressed = false;
            return true;
        }
        else
        {
            return false;
        }
        break;
    }

    return false;
}

//----------------------------------------------------------------------

void MoveLookController::OnPointerPressed(
    _In_ CoreWindow^ /* sender */,
    _In_ PointerEventArgs^ args
    )
{
    PointerPoint^ point = args->CurrentPoint;
    uint32 pointerID = point->PointerId;
    Point pointerPosition = point->Position;
    PointerPointProperties^ pointProperties = point->Properties;
    auto pointerDevice = point->PointerDevice;
    auto pointerDeviceType = pointerDevice->PointerDeviceType;

    XMFLOAT2 position = XMFLOAT2(pointerPosition.X, pointerPosition.Y);     // Convert to allow math.

    switch (m_state)
    {
    case MoveLookControllerState::WaitForInput:
        if (position.x > m_buttonUpperLeft.x &&
            position.x < m_buttonLowerRight.x &&
            position.y > m_buttonUpperLeft.y &&
            position.y < m_buttonLowerRight.y)
        {
            // Wait until button released before setting variable.
            m_buttonPointerID = pointerID;
            m_buttonInUse = true;

        }
        break;

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
                    m_moveFirstDown = position;                 // Save the location of the initial contact.
                    m_movePointerID = pointerID;                // Store the pointer using this control.
                    m_moveInUse = true;
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
                    m_fireLastPoint = position;
                    m_firePointerID = pointerID;
                    m_fireInUse = true;
                    if (!m_autoFire)
                    {
                        m_firePressed = true;
                    }
                }
            }
            else
            {
                if (!m_lookInUse)   // If no pointer is in this control yet:
                {
                    m_lookLastPoint = position;                   // Save the point for a later move.
                    m_lookPointerID = pointerID;                  // Store the pointer using this control.
                    m_lookLastDelta.x = m_lookLastDelta.y = 0;    // These are for smoothing.
                    m_lookInUse = true;
                }
            }
            break;

        default:
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
            else
            {

            }
            break;
        }

        break;
    }

    return;
}

//----------------------------------------------------------------------

void MoveLookController::OnPointerMoved(
    _In_ CoreWindow^ /* sender */,
    _In_ PointerEventArgs^ args
    )
{
    PointerPoint^ point = args->CurrentPoint;
    uint32 pointerID = point->PointerId;
    Point pointerPosition = point->Position;
    PointerPointProperties^ pointProperties = point->Properties;
    auto pointerDevice = point->PointerDevice;

    XMFLOAT2 position = XMFLOAT2(pointerPosition.X, pointerPosition.Y);     // Convert to allow math.

    switch (m_state)
    {
    case MoveLookControllerState::Active:
        // Decide which control this pointer is operating.
        if (pointerID == m_movePointerID)     // This is the move pointer.
        {
            m_movePointerPosition = position;       // Save the current position.
        }
        else if (pointerID == m_lookPointerID)     // This is the look pointer.
        {
            // Look control
            XMFLOAT2 pointerDelta;
            pointerDelta.x = position.x - m_lookLastPoint.x;        // How far did the pointer move?
            pointerDelta.y = position.y - m_lookLastPoint.y;

            XMFLOAT2 rotationDelta;
            rotationDelta.x = pointerDelta.x * ROTATION_GAIN;       // Scale for control sensitivity.
            rotationDelta.y = pointerDelta.y * ROTATION_GAIN;
            m_lookLastPoint = position;                             // Save for the next time through.

            // Update our orientation based on the command.
            m_pitch -= rotationDelta.y;
            m_yaw   += rotationDelta.x;

            // Limit pitch to straight up or straight down.
            float limit = XM_PI / 2.0f - 0.01f;
            m_pitch = __max(-limit, m_pitch);
            m_pitch = __min(+limit, m_pitch);

            // Keep longitude in the same range by wrapping.
            if (m_yaw >  XM_PI)
            {
                m_yaw -= XM_PI * 2.0f;
            }
            else if (m_yaw < -XM_PI)
            {
                m_yaw += XM_PI * 2.0f;
            }
        }
        else if (pointerID == m_firePointerID)
        {
            m_fireLastPoint = position;
        }
        else if (pointerID == m_mousePointerID)
        {
            m_mouseLeftInUse  = pointProperties->IsLeftButtonPressed;
            m_mouseRightInUse = pointProperties->IsRightButtonPressed;;
            m_mouseLastPoint = position;                            // save for next time through

            // Handle mouse movement via a separate relative mouse movement handler (OnMouseMoved).
        }
        break;
    }
}

//----------------------------------------------------------------------

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
        rotationDelta.x  = mouseDelta.x * ROTATION_GAIN;   // Scale for control sensitivity.
        rotationDelta.y  = mouseDelta.y * ROTATION_GAIN;

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

//----------------------------------------------------------------------

void MoveLookController::OnPointerReleased(
    _In_ CoreWindow^ /* sender */,
    _In_ PointerEventArgs^ args
    )
{
    PointerPoint^ point = args->CurrentPoint;
    uint32 pointerID = point->PointerId;
    Point pointerPosition = point->Position;
    PointerPointProperties^ pointProperties = point->Properties;

    XMFLOAT2 position = XMFLOAT2(pointerPosition.X, pointerPosition.Y);     // Convert to allow math.
    switch (m_state)
    {
    case MoveLookControllerState::WaitForInput:
        if (m_buttonInUse && (pointerID == m_buttonPointerID))
        {
            m_buttonInUse = false;
            m_buttonPressed = true;
        }
        break;

    case MoveLookControllerState::Active:
        if (pointerID == m_movePointerID)
        {
            m_velocity = XMFLOAT3(0, 0, 0);      // Stop on release.
            m_moveInUse = false;
            m_movePointerID = 0;
        }
        else if (pointerID == m_lookPointerID)
        {
            m_lookInUse = false;
            m_lookPointerID = 0;
        }
        else if (pointerID == m_firePointerID)
        {
            m_fireInUse = false;
            m_firePointerID = 0;
        }
        else if (pointerID == m_mousePointerID)
        {
            bool rightButton = pointProperties->IsRightButtonPressed;
            bool leftButton = pointProperties->IsLeftButtonPressed;

            m_mouseInUse = false;

            // Don't clear the mouse pointer ID so that Move events still result in Look changes.
            // m_mousePointerID = 0;
            m_mouseLeftInUse = leftButton;
            m_mouseRightInUse = rightButton;
        }
        break;
    }
}

//----------------------------------------------------------------------

void MoveLookController::OnPointerExited(
    _In_ CoreWindow^ /* sender */,
    _In_ PointerEventArgs^ args
    )
{
    PointerPoint^ point = args->CurrentPoint;
    uint32 pointerID = point->PointerId;
    Point pointerPosition = point->Position;
    PointerPointProperties^ pointProperties = point->Properties;

    XMFLOAT2 position  = XMFLOAT2(pointerPosition.X, pointerPosition.Y);        // Convert to allow math.

    switch (m_state)
    {
    case MoveLookControllerState::WaitForInput:
        if (m_buttonInUse && (pointerID == m_buttonPointerID))
        {
            m_buttonInUse = false;
            m_buttonPressed = false;
        }
        break;

    case MoveLookControllerState::Active:
        if (pointerID == m_movePointerID)
        {
            m_velocity = XMFLOAT3(0, 0, 0);      // Stop on release.
            m_moveInUse = false;
            m_movePointerID = 0;
        }
        else if (pointerID == m_lookPointerID)
        {
            m_lookInUse = false;
            m_lookPointerID = 0;
        }
        else if (pointerID == m_firePointerID)
        {
            m_fireInUse = false;
            m_firePointerID = 0;
        }
        else if (pointerID == m_mousePointerID)
        {
            m_mouseInUse = false;
            m_mousePointerID = 0;
            m_mouseLeftInUse = false;
            m_mouseRightInUse = false;
        }
        break;
    }
}

//----------------------------------------------------------------------

void MoveLookController::OnKeyDown(
    _In_ CoreWindow^ /* sender */,
    _In_ KeyEventArgs^ args
    )
{
    Windows::System::VirtualKey Key;
    Key = args->VirtualKey;

    // Figure out the command from the keyboard.
    if (Key == VirtualKey::W)       // forward
        m_forward = true;
    if (Key == VirtualKey::S)       // back
        m_back = true;
    if (Key == VirtualKey::A)       // left
        m_left = true;
    if (Key == VirtualKey::D)       // right
        m_right = true;
    if (Key == VirtualKey::Space)   // up
        m_up = true;
    if (Key == VirtualKey::X)       // down
        m_down = true;
    if (Key == VirtualKey::P)       // Pause
        m_pause = true;
}

//----------------------------------------------------------------------

void MoveLookController::OnKeyUp(
    _In_ CoreWindow^ /* sender */,
    _In_ KeyEventArgs^ args
    )
{
    Windows::System::VirtualKey Key;
    Key = args->VirtualKey;

    // Figure out the command from the keyboard.
    if (Key == VirtualKey::W)       // Forward
        m_forward = false;
    if (Key == VirtualKey::S)       // Back
        m_back = false;
    if (Key == VirtualKey::A)       // Left
        m_left = false;
    if (Key == VirtualKey::D)       // Right
        m_right = false;
    if (Key == VirtualKey::Space)   // Up
        m_up = false;
    if (Key == VirtualKey::X)       // Down
        m_down = false;
    if (Key == VirtualKey::P)
    {
        if (m_pause)
        {
            // Trigger pause only one time on button release.
            m_pausePressed = true;
            m_pause = false;
        }
    }
}

//----------------------------------------------------------------------

void MoveLookController::ResetState()
{
    // Reset the state of the controller.
    // Disable any active pointer IDs to stop all interaction.
    m_buttonPressed = false;
    m_pausePressed = false;
    m_buttonInUse = false;
    m_moveInUse = false;
    m_lookInUse = false;
    m_fireInUse = false;
    m_mouseInUse = false;
    m_mouseLeftInUse = false;
    m_mouseRightInUse = false;
    m_movePointerID = 0;
    m_lookPointerID = 0;
    m_firePointerID = 0;
    m_mousePointerID = 0;
    m_velocity = XMFLOAT3(0.0f, 0.0f, 0.0f);

    m_xinputStartButtonInUse = false;
    m_xinputTriggerInUse = false;

    m_moveCommand = XMFLOAT3(0.0f, 0.0f, 0.0f);
    m_forward = false;
    m_back = false;
    m_left = false;
    m_right = false;
    m_up = false;
    m_down = false;
    m_pause = false;
}

//----------------------------------------------------------------------

void MoveLookController::SetMoveRect (
    _In_ XMFLOAT2 upperLeft,
    _In_ XMFLOAT2 lowerRight
    )
{
    m_moveUpperLeft  = upperLeft;
    m_moveLowerRight = lowerRight;
}

//----------------------------------------------------------------------

void MoveLookController::SetFireRect (
    _In_ XMFLOAT2 upperLeft,
    _In_ XMFLOAT2 lowerRight
    )
{
    m_fireUpperLeft  = upperLeft;
    m_fireLowerRight = lowerRight;
}

//----------------------------------------------------------------------

void MoveLookController::WaitForPress(
    _In_ XMFLOAT2 upperLeft,
    _In_ XMFLOAT2 lowerRight
    )
{

    ResetState();
    m_state = MoveLookControllerState::WaitForInput;
    m_buttonUpperLeft  = upperLeft;
    m_buttonLowerRight = lowerRight;

    // Turn on the mouse cursor.
    CoreWindow::GetForCurrentThread()->PointerCursor = ref new CoreCursor(CoreCursorType::Arrow, 0);
}

//----------------------------------------------------------------------

void MoveLookController::WaitForPress()
{
    ResetState();
    m_state = MoveLookControllerState::WaitForInput;
    m_buttonUpperLeft.x = 0.0f;
    m_buttonUpperLeft.y = 0.0f;
    m_buttonLowerRight.x = 0.0f;
    m_buttonLowerRight.y = 0.0f;

    // Turn on the mouse cursor.
    CoreWindow::GetForCurrentThread()->PointerCursor = ref new CoreCursor(CoreCursorType::Arrow, 0);
}

//----------------------------------------------------------------------

XMFLOAT3 MoveLookController::Velocity()
{
    return m_velocity;
}

//----------------------------------------------------------------------

XMFLOAT3 MoveLookController::LookDirection()
{
    XMFLOAT3 lookDirection;

    float r = cosf(m_pitch);              // In the plane
    lookDirection.y = sinf(m_pitch);      // Vertical
    lookDirection.z = r * cosf(m_yaw);    // Fwd-back
    lookDirection.x = r * sinf(m_yaw);    // Left-right

    return lookDirection;
}

//----------------------------------------------------------------------

float MoveLookController::Pitch()
{
    return m_pitch;
}

//----------------------------------------------------------------------

void MoveLookController::Pitch(_In_ float pitch)
{
    m_pitch = pitch;
}

//----------------------------------------------------------------------

float MoveLookController::Yaw()
{
    return m_yaw;
}

//----------------------------------------------------------------------

void MoveLookController::Yaw(_In_ float yaw)
{
    m_yaw = yaw;
}

//----------------------------------------------------------------------

void MoveLookController::Active(_In_ bool active)
{
    ResetState();

    if (active)
    {
        m_state = MoveLookControllerState::Active;
        // Turn the mouse cursor off (hidden).
        CoreWindow::GetForCurrentThread()->PointerCursor = nullptr;
    }
    else
    {
        m_state = MoveLookControllerState::None;
        // Turn the mouse cursor on.
        auto window = CoreWindow::GetForCurrentThread();
        if (window)
        {
            // Protect case where there isn't a window associated with the current thread.
            // This happens on initialization.
            window->PointerCursor = ref new CoreCursor(CoreCursorType::Arrow, 0);
        }
    }
}

//----------------------------------------------------------------------

bool MoveLookController::Active()
{
    if (m_state == MoveLookControllerState::Active)
    {
        return true;
    }
    else
    {
        return false;
    }
}

//----------------------------------------------------------------------

void MoveLookController::AutoFire(_In_ bool autoFire)
{
    m_autoFire = autoFire;
}

//----------------------------------------------------------------------

bool MoveLookController::AutoFire()
{
    return m_autoFire;
}

//----------------------------------------------------------------------

void MoveLookController::Update()
{
    UpdateGameController();

    if (m_moveInUse)
    {
        // Move control.
        XMFLOAT2 pointerDelta;

        pointerDelta.x = m_movePointerPosition.x - m_moveFirstDown.x;
        pointerDelta.y = m_movePointerPosition.y - m_moveFirstDown.y;

        // Figure out the command from the virtual joystick.
        XMFLOAT3 commandDirection = XMFLOAT3(0.0f, 0.0f, 0.0f);
        if (fabsf(pointerDelta.x) > 16.0f)         // leave 32 pixel-wide dead spot for being still
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

    // Make sure that 45 deg cases are not faster.
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
    // Our velocity is based on the command, y is up.
    m_velocity.x = -wCommand.x * MOVEMENT_GAIN;
    m_velocity.z =  wCommand.y * MOVEMENT_GAIN;
    m_velocity.y =  wCommand.z * MOVEMENT_GAIN;

    // Clear the movement input accumulator for use during next frame.
    m_moveCommand = XMFLOAT3(0.0f, 0.0f, 0.0f);
}

//----------------------------------------------------------------------

void MoveLookController::UpdateGameController()
{
    if (!m_isControllerConnected)
    {
        // Check for controller connection by trying to get the capabilties.
        DWORD capsResult = XInputGetCapabilities(0, XINPUT_FLAG_GAMEPAD, &m_xinputCaps);
        if (capsResult != ERROR_SUCCESS)
        {
            return;
        }
        // The device is connected.
        m_isControllerConnected = true;
        m_xinputStartButtonInUse = false;
        m_xinputTriggerInUse = false;
    }

    DWORD stateResult = XInputGetState(0, &m_xinputState);
    if (stateResult != ERROR_SUCCESS)
    {
        // The device is no longer connected.
        m_isControllerConnected = false;
    }

    switch (m_state)
    {
    case MoveLookControllerState::WaitForInput:
        if (m_xinputState.Gamepad.wButtons & XINPUT_GAMEPAD_START)
        {
            m_xinputStartButtonInUse = true;
        }
        else if (m_xinputStartButtonInUse)
        {
            // Trigger one time only on button release.
            m_xinputStartButtonInUse = false;
            m_buttonPressed = true;
        }
        break;

    case MoveLookControllerState::Active:
        if (m_xinputState.Gamepad.wButtons & XINPUT_GAMEPAD_START)
        {
            m_xinputStartButtonInUse = true;
        }
        else if (m_xinputStartButtonInUse)
        {
            // Trigger one time only on button release.
            m_xinputStartButtonInUse = false;
            m_pausePressed = true;
        }
        // Use the Right Thumb joystick on the XBox controller to control
        // the eye point position control.
        // The controller input goes from -32767 to 32767.   We will normalize
        // this from -1 to 1 and keep a dead spot in the middle to avoid drift.

        if (m_xinputState.Gamepad.sThumbLX > XINPUT_GAMEPAD_LEFT_THUMB_DEADZONE ||
            m_xinputState.Gamepad.sThumbLX < -XINPUT_GAMEPAD_LEFT_THUMB_DEADZONE)
        {
            float x = (float)m_xinputState.Gamepad.sThumbLX/32767.0f;
            m_moveCommand.x -= x / fabsf(x);
        }

        if (m_xinputState.Gamepad.sThumbLY > XINPUT_GAMEPAD_LEFT_THUMB_DEADZONE ||
            m_xinputState.Gamepad.sThumbLY < -XINPUT_GAMEPAD_LEFT_THUMB_DEADZONE)
        {
            float y = (float)m_xinputState.Gamepad.sThumbLY/32767.0f;
            m_moveCommand.y += y / fabsf(y);
        }

        // Use the Left Thumb Joystick on the XBox controller to control
        // the look at control.
        // The controller input goes from -32767 to 32767.   We will normalize
        // this from -1 to 1 and keep a dead spot in the middle to avoid drift.
        XMFLOAT2 pointerDelta;
        if (m_xinputState.Gamepad.sThumbRX > XINPUT_GAMEPAD_RIGHT_THUMB_DEADZONE ||
            m_xinputState.Gamepad.sThumbRX < -XINPUT_GAMEPAD_RIGHT_THUMB_DEADZONE)
        {
            pointerDelta.x = (float)m_xinputState.Gamepad.sThumbRX/32767.0f;
            pointerDelta.x = pointerDelta.x * pointerDelta.x * pointerDelta.x;
        }
        else
        {
            pointerDelta.x = 0.0f;
        }
        if (m_xinputState.Gamepad.sThumbRY > XINPUT_GAMEPAD_RIGHT_THUMB_DEADZONE ||
            m_xinputState.Gamepad.sThumbRY < -XINPUT_GAMEPAD_RIGHT_THUMB_DEADZONE)
        {
            pointerDelta.y = (float)m_xinputState.Gamepad.sThumbRY/32767.0f;
            pointerDelta.y = pointerDelta.y * pointerDelta.y * pointerDelta.y;
        }
        else
        {
            pointerDelta.y = 0.0f;
        }

        XMFLOAT2 rotationDelta;
        rotationDelta.x = pointerDelta.x *  0.08f;       // scale for control sensitivity
        rotationDelta.y = pointerDelta.y *  0.08f;

        // Update our orientation based on the command.
        m_pitch += rotationDelta.y;
        m_yaw   += rotationDelta.x;

        // Limit pitch to straight up or straight down.
        m_pitch = __max(-XM_PI / 2.0f, m_pitch);
        m_pitch = __min(+XM_PI / 2.0f, m_pitch);

        // Check the state of the A button.  This is used to indicate fire control.

        if (m_xinputState.Gamepad.bRightTrigger > XINPUT_GAMEPAD_TRIGGER_THRESHOLD)
        {
            if (!m_autoFire && !m_xinputTriggerInUse)
            {
                m_firePressed = true;
            }
            m_xinputTriggerInUse = true;
        }
        else
        {
            m_xinputTriggerInUse = false;
        }
        break;
    }
}
```

> **注意**  
本文适用于编写通用 Windows 平台 (UWP) 应用的 Windows 10 开发人员。 如果你要针对 Windows 8.x 或 Windows Phone 8.x 进行开发，请参阅[存档文档](http://go.microsoft.com/fwlink/p/?linkid=619132)。

 

## 相关主题


[使用 DirectX 创建一款简单的 UWP 游戏](tutorial--create-your-first-metro-style-directx-game.md)

 

 







<!--HONumber=Jun16_HO4-->


