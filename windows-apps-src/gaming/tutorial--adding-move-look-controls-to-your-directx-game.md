---
title: 游戏的移动观看控件
description: 了解如何向你的 DirectX 游戏添加传统的鼠标和键盘移动观看控件（也称为鼠标观看控件）。
ms.assetid: 4b4d967c-3de9-8a97-ae68-0327f00cc933
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 游戏, 移动观看, 控件
ms.localizationpriority: medium
ms.openlocfilehash: 222f46bbda165442003aecea0bbd138bcb844a3b
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/03/2018
ms.locfileid: "8464592"
---
# <a name="span-iddevgamingtutorialaddingmove-lookcontrolstoyourdirectxgamespanmove-look-controls-for-games"></a><span id="dev_gaming.tutorial__adding_move-look_controls_to_your_directx_game"></span>游戏的移动观看控件



了解如何向你的 DirectX 游戏添加传统的鼠标和键盘移动观看控件（也称为鼠标观看控件）。

我们还将讨论对触摸设备的移动观看支持，该设备将移动控制器定义为屏幕的左下部分，其行为类似于方向输入， 并为屏幕的其余区域定义观看控制器，还将相机定位到玩家最后在该区域触摸的中心位置。

如果你对此控件概念不熟悉，请这样考虑：键盘（或基于触摸的方向输入框）在此 3D 空间中控制你的双腿， 其行为类似于你的双腿只能前后移动，或者左右直走。 鼠标（或触摸指针）控制你的头部。 你使用头部观看控制器方向 - 向左或向右、向上或向下，或者该平面上的某个位置。 如果在你的视区有一个目标，则需要使用鼠标将相机视区定位到该目标的中心，然后按前进键向前移动，或者后退远离该目标。 要圈住目标，则需要将相机视区保持在目标的中心，同时向左或向右移动。 你可以看到，对于导航 3D 环境，这是一个非常有效的控制方法！

这些控件在游戏中通常称为 WASD 控件，其中 W、A、、S 和 D 键用于 x-z 平面固定的相机移动，鼠标用于控制相机围绕 x 和 y 轴的旋转。

## <a name="objectives"></a>目标


-   在 DirectX 游戏上为鼠标和键盘以及触摸屏添加基本移动观看控件。
-   实现用于导航 3D 环境的第一人称相机。

## <a name="a-note-on-touch-control-implementations"></a>关于触摸控件实现的说明


对于触摸控件，我们实现两个控制器：移动控制器（处理 x-z 平面上相对于相机视点的移动）和观看控制器（瞄准相机的观看点）。 我们的移动控制器映射到键盘 WASD 按钮，观看控制器映射到鼠标。 但是对于触摸控件，我们需要定义屏幕的一个区域作为方向输入或者虚拟 WASD 按钮，屏幕其余区域作为观看控件的输入空间。

我们的屏幕如下所示：

![移动观看控制器布局](images/movelook-touch.png)

当你在屏幕左下角移动触摸指针（而非鼠标！）时，任何向上移动都会使相机向前移动。 任何向下移动都会使相机向后移动。 在移动控制器的指针空间左右移动与此相同。 在该空间外部，相机将成为一个观看控制器 - 只需触摸相机或将其拖到其面对的位置。

## <a name="set-up-the-basic-input-event-infrastructure"></a>设置基本输入事件基础结构


首先，我们必须创建用于处理来自鼠标和键盘的输入事件的控制类，并基于该输入更新相机视图。 由于我们要实现移动观看控件，因此我们将其称为 **MoveLookController**。

```cpp
using namespace Windows::UI::Core;
using namespace Windows::System;
using namespace Windows::Foundation;
using namespace Windows::Devices::Input;
#include <DirectXMath.h>

// Methods to get input from the UI pointers
ref class MoveLookController
{
};  // class MoveLookController
```

现在，我们创建一个标头来定义移动观看控制器的状态及其第一人称相机，以及实现控件和更新相机状态的基本方法和事件处理程序。

```cpp
#define ROTATION_GAIN 0.004f    // Sensitivity adjustment for the look controller
#define MOVEMENT_GAIN 0.1f      // Sensitivity adjustment for the move controller

ref class MoveLookController
{
private:
    // Properties of the controller object
    DirectX::XMFLOAT3 m_position;               // The position of the controller
    float m_pitch, m_yaw;           // Orientation euler angles in radians

    // Properties of the Move control
    bool m_moveInUse;               // Specifies whether the move control is in use
    uint32 m_movePointerID;         // Id of the pointer in this control
    DirectX::XMFLOAT2 m_moveFirstDown;          // Point where initial contact occurred
    DirectX::XMFLOAT2 m_movePointerPosition;   // Point where the move pointer is currently located
    DirectX::XMFLOAT3 m_moveCommand;            // The net command from the move control

    // Properties of the Look control
    bool m_lookInUse;               // Specifies whether the look control is in use
    uint32 m_lookPointerID;         // Id of the pointer in this control
    DirectX::XMFLOAT2 m_lookLastPoint;          // Last point (from last frame)
    DirectX::XMFLOAT2 m_lookLastDelta;          // For smoothing

    bool m_forward, m_back;         // States for movement
    bool m_left, m_right;
    bool m_up, m_down;


public:

    // Methods to get input from the UI pointers
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

    void OnKeyDown(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::KeyEventArgs^ args
        );

    void OnKeyUp(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::KeyEventArgs^ args
        );

    // Set up the Controls that this controller supports
    void Initialize( _In_ Windows::UI::Core::CoreWindow^ window );

    void Update( Windows::UI::Core::CoreWindow ^window );
    
internal:
    // Accessor to set position of controller
    void SetPosition( _In_ DirectX::XMFLOAT3 pos );

    // Accessor to set position of controller
    void SetOrientation( _In_ float pitch, _In_ float yaw );

    // Returns the position of the controller object
    DirectX::XMFLOAT3 get_Position();

    // Returns the point  which the controller is facing
    DirectX::XMFLOAT3 get_LookPoint();


};  // class MoveLookController
```

我们的代码包含 4 组私有字段。 下面介绍每个字段的目的。

首先，我们定义一些有用的字段，用于保存有关相机视图的更新信息。

-   **m\_position** 是 3D 场景（使用场景坐标）中相机的位置（因此是一个视觉平面）。
-   **m\_pitch** 是相机的俯仰度或其绕视觉平面的 x 轴上下旋转弧度。
-   **m\_yaw** 是相机的偏航度或其绕视觉平面的 y 轴左右旋转弧度。

现在，我们定义用于存储有关控制器状态和位置信息的字段。 首先，我们将定义基于触摸的移动控制器所需的字段。 （对于移动控制器的键盘实现没有特殊要求。 我们只需使用特定处理程序读取键盘事件。）

-   **m\_moveInUse** 指示是否正在使用移动控制器。
-   **m\_movePointerID** 是当前移动指针的唯一 ID。 在检查指针 ID 值时，我们用它来区分观看指针和移动指针。
-   **m\_moveFirstDown** 是玩家在屏幕上首次触摸移动控制器指针区域的点。 我们稍后将使用此值设置死区，以防止视图的抖动。
-   **m\_movePointerPosition** 是玩家当前在屏幕上将指针移动到的点。 我们通过检查它相对于 **m\_moveFirstDown** 的移动，确定玩家要移动的方向。
-   **m\_moveCommand** 是移动控制器的最终计算命令：向上（前进）、向下（后退）、向左或向右。

现在，我们定义用于观看控制器的字段，同时适用于鼠标和触摸实现。

-   **m\_lookInUse** 指示是否正在使用观看控件。
-   **m\_lookPointerID** 是当前观看控制器指针的唯一 ID。 在检查指针 ID 值时，我们用它来区分观看指针和移动指针。
-   **m\_lookLastPoint** 是场景坐标中从上一帧中捕获的最后一个点。
-   **m\_lookLastDelta** 是在当前 **m\_position** 与 **m\_lookLastPoint** 之间计算出的差异。

最后，我们为运动的 6 个维度定义 6 个布尔值，用于指示每个方向移动操作的当前状态（开或关）：

-   **m\_forward**、**m\_back**、**m\_left**、**m\_right**、**m\_up**和 **m\_down**。

我们使用 6 个事件处理程序捕获输入数据来更新控制器的状态：

-   **OnPointerPressed**。 玩家使用游戏屏幕中的指针按下了鼠标左键，或者触摸了屏幕。
-   **OnPointerMoved**。 玩家使用游戏屏幕中的指针移动了鼠标，或者在屏幕上拖动了触摸指针。
-   **OnPointerReleased**。 玩家使用游戏屏幕中的指针释放了鼠标左键，或者停止了触摸屏幕。
-   **OnKeyDown**。 玩家按下了一个键。
-   **OnKeyUp**。 玩家释放了一个键。

最后，我们使用这些方法和属性来初始化、访问和更新控制器的状态信息。

-   **Initialize**。 应用调用此事件处理程序来初始化控件，并将其附加到描述显示窗口的 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 对象。
-   **SetPosition**。 应用调用此方法以在场景空间中设置控件的（x、y 和 z）坐标。
-   **SetOrientation**。 应用调用此方法以设置相机的俯仰和偏航。
-   **get\_Position**。 应用访问此属性来获取相机在场景空间中的当前位置。 使用此属性作为当前相机位置与应用进行通信的方法。
-   **get\_LookPoint**。 应用访问此属性来获取控制器相机面向的当前点。
-   **Update**。 读取移动和观看控制器的状态并更新相机位置。 从应用的主循环中不断调用此方法可刷新相机控制器数据和相机在场景空间中的位置。

现在，你在此处拥有了实现移动观看控件所需的全部组件。 因此，我们将这些片段连接在一起。

## <a name="create-the-basic-input-events"></a>创建基本输入事件


Windows 运行时事件调度程序提供我们需要 **MoveLookController** 类的实例处理的 5 个事件：

-   [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208278)
-   [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208276)
-   [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208279)
-   [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208271)
-   [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208270)

这些事件在 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 类型上实现。 我们假设你有一个 **CoreWindow** 对象要处理。 如果不知道如何获取该对象，请参阅[如何设置通用 Windows 平台 (UWP) C++ 应用以显示 DirectX 视图](https://msdn.microsoft.com/library/windows/apps/hh465077)。

由于在应用运行时将触发这些事件，处理程序将更新控制器在私有字段中定义的状态信息。

首先，让我们填充鼠标和触摸指针事件处理程序。 在第一个事件处理程序 **OnPointerPressed()** 中，我们从 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 获取指针的 x-y 坐标，该类型管理用户单击鼠标或触摸屏幕的观看控制器区域时的显示。

**OnPointerPressed**

```cpp
void MoveLookController::OnPointerPressed(
_In_ CoreWindow^ sender,
_In_ PointerEventArgs^ args)
{
    // Get the current pointer position.
    uint32 pointerID = args->CurrentPoint->PointerId;
    DirectX::XMFLOAT2 position = DirectX::XMFLOAT2( args->CurrentPoint->Position.X, args->CurrentPoint->Position.Y );

    auto device = args->CurrentPoint->PointerDevice;
    auto deviceType = device->PointerDeviceType;
    if ( deviceType == PointerDeviceType::Mouse )
    {
        // Action, Jump, or Fire
    }

    // Check  if this pointer is in the move control.
    // Change the values  to percentages of the preferred screen resolution.
    // You can set the x value to <preferred resolution> * <percentage of width>
    // for example, ( position.x < (screenResolution.x * 0.15) ).

    if (( position.x < 300 && position.y > 380 ) && ( deviceType != PointerDeviceType::Mouse ))
    {
        if ( !m_moveInUse ) // if no pointer is in this control yet
        {
            // Process a DPad touch down event.
            m_moveFirstDown = position;                 // Save the location of the initial contact.
            m_movePointerPosition = position;
            m_movePointerID = pointerID;                // Store the id of the pointer using this control.
            m_moveInUse = TRUE;
        }
    }
    else // This pointer must be in the look control.
    {
        if ( !m_lookInUse ) // If no pointer is in this control yet...
        {
            m_lookLastPoint = position;                         // save the point for later move
            m_lookPointerID = args->CurrentPoint->PointerId;  // store the id of pointer using this control
            m_lookLastDelta.x = m_lookLastDelta.y = 0;          // these are for smoothing
            m_lookInUse = TRUE;
        }
    }
}
```

此事件处理程序检查指针是否为鼠标（对于本示例，同时支持鼠标和触摸）以及是否位于移动控制器区域中。 如果满足这两个条件，它将检查刚才是否按下了指针，具体地说，通过测试 **m\_moveInUse** 是否为 false 检查此单击是否与上次移动或观看输入无关。 如果无关，处理程序将捕获发生点按操作的移动控制器区域中的点，并将 **m\_moveInUse** 设置为 true，以便当再次调用此处理程序时，它不会覆盖移动控制器输入交互的起始位置。 它还将移动控制器指针 ID 更新为当前指针的 ID。

如果指针是鼠标或者触摸指针不在移动控制器区域中，它一定位于观看控制器区域中。 该指针将 **m\_lookLastPoint** 设置为用户按下鼠标按钮或触摸并点按的当前位置、重置增量，并将观看控制器的指针 ID 更新为当前指针 ID。 它还会将观看控制器的状态设置为活动。

**OnPointerMoved**

```cpp
void MoveLookController::OnPointerMoved(
    _In_ CoreWindow ^sender,
    _In_ PointerEventArgs ^args)
{
    uint32 pointerID = args->CurrentPoint->PointerId;
    DirectX::XMFLOAT2 position = DirectX::XMFLOAT2(args->CurrentPoint->Position.X, args->CurrentPoint->Position.Y);

    // Decide which control this pointer is operating.
    if (pointerID == m_movePointerID)           // This is the move pointer.
    {
        // Move control
        m_movePointerPosition = position;       // Save the current position.

    }
    else if (pointerID == m_lookPointerID)      // This is the look pointer.
    {
        // Look control

        DirectX::XMFLOAT2 pointerDelta;
        pointerDelta.x = position.x - m_lookLastPoint.x;        // How far did pointer move
        pointerDelta.y = position.y - m_lookLastPoint.y;

        DirectX::XMFLOAT2 rotationDelta;
        rotationDelta.x = pointerDelta.x * ROTATION_GAIN;   // Scale for control sensitivity.
        rotationDelta.y = pointerDelta.y * ROTATION_GAIN;

        m_lookLastPoint = position;                     // Save for the next time through.

                                                        // Update our orientation based on the command.
        m_pitch -= rotationDelta.y;                     // Mouse y increases down, but pitch increases up.
        m_yaw -= rotationDelta.x;                       // Yaw is defined as CCW around the y-axis.

                                                        // Limit the pitch to straight up or straight down.
        m_pitch = (float)__max(-DirectX::XM_PI / 2.0f, m_pitch);
        m_pitch = (float)__min(+DirectX::XM_PI / 2.0f, m_pitch);
    }
}
```

每当指针移动（在此情况下为拖动触摸屏指针或者在按下鼠标左键的同时移动鼠标指针）时都会触发 **OnPointerMoved** 事件处理程序。 如果指针 ID 与移动控制器指针的 ID 相同，则它是移动指针；否则，我们需要检查它是否是作为活动指针的观看控制器。

如果它是移动控制器，我们只需更新指针位置。 只要不断触发 [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208276) 事件，我们就不断更新指针位置，因为我们希望将最终位置与 **OnPointerPressed** 事件处理程序捕获的第一个位置进行比较。

如果是观看控制器，则情况稍复杂一些。 我们需要计算新观看点并将相机定位到它的中心，因而需要计算上一个观看点与当前屏幕位置之间的增量，然后乘以我们的缩放比例，我们可以进行调整，使观看点的移动相对于屏幕移动的距离变小或变大。 我们使用该值来计算俯仰和偏航。

最后，当玩家停止移动鼠标或触摸屏幕时，我们需要停用移动或观看控制器行为。 我们使用在触发 [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208279) 时调用的 **OnPointerReleased** 将 **m\_moveInUse** 或 **m\_lookInUse** 设置为 FALSE，并关闭相机平移，将指针 ID 设置为零。

**OnPointerReleased**

```cpp
void MoveLookController::OnPointerReleased(
_In_ CoreWindow ^sender,
_In_ PointerEventArgs ^args)
{
    uint32 pointerID = args->CurrentPoint->PointerId;
    DirectX::XMFLOAT2 position = DirectX::XMFLOAT2( args->CurrentPoint->Position.X, args->CurrentPoint->Position.Y );


    if ( pointerID == m_movePointerID )    // This was the move pointer.
    {
        m_moveInUse = FALSE;
        m_movePointerID = 0;
    }
    else if (pointerID == m_lookPointerID ) // This was the look pointer.
    {
        m_lookInUse = FALSE;
        m_lookPointerID = 0;
    }
}
```

到目前为止，我们已经处理了所有触摸屏事件。 现在，让我们处理基于键盘的移动控制器的键输入事件。

**OnKeyDown**

```cpp
void MoveLookController::OnKeyDown(
                                   __in CoreWindow^ sender,
                                   __in KeyEventArgs^ args )
{
    Windows::System::VirtualKey Key;
    Key = args->VirtualKey;

    // Figure out the command from the keyboard.
    if ( Key == VirtualKey::W )     // Forward
        m_forward = true;
    if ( Key == VirtualKey::S )     // Back
        m_back = true;
    if ( Key == VirtualKey::A )     // Left
        m_left = true;
    if ( Key == VirtualKey::D )     // Right
        m_right = true;
}
```

只要按下其中一个键，此事件处理器就会将相应的方向移动状态设置为 true。

**OnKeyUp**

```cpp
void MoveLookController::OnKeyUp(
                                 __in CoreWindow^ sender,
                                 __in KeyEventArgs^ args)
{
    Windows::System::VirtualKey Key;
    Key = args->VirtualKey;

    // Figure out the command from the keyboard.
    if ( Key == VirtualKey::W )     // forward
        m_forward = false;
    if ( Key == VirtualKey::S )     // back
        m_back = false;
    if ( Key == VirtualKey::A )     // left
        m_left = false;
    if ( Key == VirtualKey::D )     // right
        m_right = false;
}
```

而且，当释放按键时，此事件处理程序会将其设置回 false。 当我们调用 **Update** 时，它将检查这些方向移动状态，并相应地移动相机。 这比触摸实现相对简单一些！

## <a name="initialize-the-touch-controls-and-the-controller-state"></a>初始化触摸控件和控制器状态


现在让我们将事件连接在一起，并初始化所有控制器状态字段。

**Initialize**

```cpp
void MoveLookController::Initialize( _In_ CoreWindow^ window )
{

    // Opt in to receive touch/mouse events.
    window->PointerPressed += 
    ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerPressed);

    window->PointerMoved += 
    ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerMoved);

    window->PointerReleased += 
    ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerReleased);

    window->CharacterReceived +=
    ref new TypedEventHandler<CoreWindow^, CharacterReceivedEventArgs^>(this, &MoveLookController::OnCharacterReceived);

    window->KeyDown += 
    ref new TypedEventHandler<CoreWindow^, KeyEventArgs^>(this, &MoveLookController::OnKeyDown);

    window->KeyUp += 
    ref new TypedEventHandler<CoreWindow^, KeyEventArgs^>(this, &MoveLookController::OnKeyUp);

    // Initialize the state of the controller.
    m_moveInUse = FALSE;                // No pointer is in the Move control.
    m_movePointerID = 0;

    m_lookInUse = FALSE;                // No pointer is in the Look control.
    m_lookPointerID = 0;

    //  Need to init this as it is reset every frame.
    m_moveCommand = DirectX::XMFLOAT3( 0.0f, 0.0f, 0.0f );

    SetOrientation( 0, 0 );             // Look straight ahead when the app starts.

}
```

**Initialize** 将指向该应用的 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 实例的引用作为参数提供，并在该 **CoreWindow** 上注册我们已开发的相应事件处理程序。 它初始化移动和观看指针的 ID、将触摸屏移动控制器实现的命令矢量设置为零，并将相机设置为在应用启动时观看正前方。

## <a name="getting-and-setting-the-position-and-orientation-of-the-camera"></a>获取和设置相机的位置和方向


让我们定义一些方法，以便相对于视区获取和设置相机位置。

```cpp
void MoveLookController::SetPosition( _In_ DirectX::XMFLOAT3 pos )
{
    m_position = pos;
}

// Accessor to set the position of the controller.
void MoveLookController::SetOrientation( _In_ float pitch, _In_ float yaw )
{
    m_pitch = pitch;
    m_yaw = yaw;
}

// Returns the position of the controller object.
DirectX::XMFLOAT3 MoveLookController::get_Position()
{
    return m_position;
}

// Returns the point at which the camera controller is facing.
DirectX::XMFLOAT3 MoveLookController::get_LookPoint()
{
    float y = sinf(m_pitch);        // Vertical
    float r = cosf(m_pitch);        // In the plane
    float z = r*cosf(m_yaw);        // Fwd-back
    float x = r*sinf(m_yaw);        // Left-right
    DirectX::XMFLOAT3 result(x,y,z);
    result.x += m_position.x;
    result.y += m_position.y;
    result.z += m_position.z;

    // Return m_position + DirectX::XMFLOAT3(x, y, z);
    return result;
}
```

## <a name="updating-the-controller-state-info"></a>更新控制器状态信息


现在，我们将执行计算，可将 **m\_movePointerPosition** 中跟踪的指针坐标信息转换为世界坐标系中的新坐标信息。 每当刷新主应用循环时，应用都会调用此方法。 因此，我们需要在这里计算要传递给应用的新视点位置信息，以便在投影到视口之前更新 视图矩阵。

```cpp
void MoveLookController::Update(CoreWindow ^window)
{
    // Check for input from the Move control.
    if (m_moveInUse)
    {
        DirectX::XMFLOAT2 pointerDelta(m_movePointerPosition);
        pointerDelta.x -= m_moveFirstDown.x;
        pointerDelta.y -= m_moveFirstDown.y;

        // Figure out the command from the touch-based virtual joystick.
        if (pointerDelta.x > 16.0f)      // Leave 32 pixel-wide dead spot for being still.
            m_moveCommand.x = 1.0f;
        else
            if (pointerDelta.x < -16.0f)
            m_moveCommand.x = -1.0f;

        if (pointerDelta.y > 16.0f)      // Joystick y is up, so change sign.
            m_moveCommand.y = -1.0f;
        else
            if (pointerDelta.y < -16.0f)
            m_moveCommand.y = 1.0f;
    }

    // Poll our state bits that are set by the keyboard input events.
    if (m_forward)
        m_moveCommand.y += 1.0f;
    if (m_back)
        m_moveCommand.y -= 1.0f;

    if (m_left)
        m_moveCommand.x -= 1.0f;
    if (m_right)
        m_moveCommand.x += 1.0f;

    if (m_up)
        m_moveCommand.z += 1.0f;
    if (m_down)
        m_moveCommand.z -= 1.0f;

    // Make sure that 45 degree cases are not faster.
    DirectX::XMFLOAT3 command = m_moveCommand;
    DirectX::XMVECTOR vector;
    vector = DirectX::XMLoadFloat3(&command);

    if (fabsf(command.x) > 0.1f || fabsf(command.y) > 0.1f || fabsf(command.z) > 0.1f)
    {
        vector = DirectX::XMVector3Normalize(vector);
        DirectX::XMStoreFloat3(&command, vector);
    }
    

    // Rotate command to align with our direction (world coordinates).
    DirectX::XMFLOAT3 wCommand;
    wCommand.x = command.x*cosf(m_yaw) - command.y*sinf(m_yaw);
    wCommand.y = command.x*sinf(m_yaw) + command.y*cosf(m_yaw);
    wCommand.z = command.z;

    // Scale for sensitivity adjustment.
    wCommand.x = wCommand.x * MOVEMENT_GAIN;
    wCommand.y = wCommand.y * MOVEMENT_GAIN;
    wCommand.z = wCommand.z * MOVEMENT_GAIN;

    // Our velocity is based on the command.
    // Also note that y is the up-down axis. 
    DirectX::XMFLOAT3 Velocity;
    Velocity.x = -wCommand.x;
    Velocity.z = wCommand.y;
    Velocity.y = wCommand.z;

    // Integrate
    m_position.x += Velocity.x;
    m_position.y += Velocity.y;
    m_position.z += Velocity.z;

    // Clear movement input accumulator for use during the next frame.
    m_moveCommand = DirectX::XMFLOAT3(0.0f, 0.0f, 0.0f);

}
```

由于我们不希望在玩家使用基于触摸的移动控制器时移动发生抖动，我们将围绕指针设置一个直径为 32 像素的视觉死区。 我们还添加了速度，该速度为命令值加上移动增益率。 （你可以根据自己的喜好调整此行为，以便根据指针在移动控制器区域的移动距离 来减慢或加快移动速率。）

在计算速度时，我们还会将从移动和观看控制器接收的坐标转换为发送到我们用于 计算场景视图矩阵的方法的实际视点的移动。 首先，我们反转 x 坐标，因为如果我们单击移动或左右拖动观看控制器，视点将在场景中向 相反方向旋转，相机可能会沿着其中心轴旋转。 然后，我们交换 y 和 z 轴，因为在移动控制器上按向上/向下键或触摸拖动运动（读取为 y 轴行为）应转换为将视点移入或移出屏幕（z 轴）的相机操作。

玩家视点的最后位置是最后位置加计算的速度，呈现器在调用 **get\_Position** 方法（通常在每个帧的设置期间）时将读取此数据。 然后，我们将移动命令重置为零。

## <a name="updating-the-view-matrix-with-the-new-camera-position"></a>使用相机的新位置更新视图矩阵


我们可以获取相机聚焦的场景空间坐标，每当指示你的应用更新坐标时，将执行此操作（例如在主屏循环中每 60 秒）。 此伪代码建议你可以实现的调用行为：

```cpp
myMoveLookController->Update( m_window );   

// Update the view matrix based on the camera position.
myFirstPersonCamera->SetViewParameters(
                 myMoveLookController->get_Position(),       // Point we are at
                 myMoveLookController->get_LookPoint(),      // Point to look towards
                 DirectX::XMFLOAT3( 0, 1, 0 )                   // Up-vector
                 ); 
```

恭喜你！ 你已经在游戏中为触摸屏和键盘/鼠标输入触摸控件实现了基本的移动观看控件！



 

 




