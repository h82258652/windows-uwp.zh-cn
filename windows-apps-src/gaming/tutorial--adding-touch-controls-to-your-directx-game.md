---
author: mtoepke
title: 游戏的触摸控件
description: 了解如何将基本触摸控件添加到使用 DirectX 的通用 Windows 平台 (UWP) C++ 游戏。
ms.assetid: 9d40e6e4-46a9-97e9-b848-522d61e8e109
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 游戏, 触摸, 控件, directx, 输入
ms.localizationpriority: medium
ms.openlocfilehash: 53c4a91f3ef20c11783796c3ca362f74b3f39adb
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/09/2018
ms.locfileid: "6194937"
---
# <a name="touch-controls-for-games"></a>游戏的触摸控件



了解如何将基本触摸控件添加到使用 DirectX 的通用 Windows 平台 (UWP) C++ 游戏。 我们将介绍如何添加基于触摸的控件以便在 Direct3D 环境中（使用手指或触笔在其中拖动来改变相机视角） 移动固定平面相机。

你可以将这些控件合并到游戏中，让玩家能够在 3D 环境中通过拖动滚动或平移地图或游戏区。 例如，在策略游戏或拼图游戏中，你可以使用这些控件让玩家通过左右平移来观看大于屏幕的游戏环境。

> **请注意**我们的代码也适用于基于鼠标的平移控件。 与指针相关的事件通过 Windows 运行时 API 进行抽象，因此它们可以处理基于触摸或基于鼠标的指针事件。

 

## <a name="objectives"></a>目标


-   创建简单触摸拖动控件，用于在 DirectX 游戏中平移固定平面相机。

## <a name="set-up-the-basic-touch-event-infrastructure"></a>设置基本触摸事件基础结构


首先，我们在本示例中定义基本控制器类型 **CameraPanController**。 在此处，我们将控制器定义为抽象概念，即用户可以执行的行为集。

**CameraPanController** 类是一个定期刷新的有关相机控制器状态的信息集合，并为应用提供一种从其更新循环中获取该信息的方法。

```cpp
using namespace Windows::UI::Core;
using namespace Windows::System;
using namespace Windows::Foundation;
using namespace Windows::Devices::Input;
#include <directxmath.h>

// Methods to get input from the UI pointers
ref class CameraPanController
{
}
```

现在，让我们创建一个定义相机控制器状态的标头，以及实现相机控制器交互的基本方法和事件处理程序。

```cpp
ref class CameraPanController
{
private:
    // Properties of the controller object
    DirectX::XMFLOAT3 m_position;               // the position of the camera

    // Properties of the camera pan control
    bool m_panInUse;                
    uint32 m_panPointerID;          
    DirectX::XMFLOAT2 m_panFirstDown;           
    DirectX::XMFLOAT2 m_panPointerPosition;   
    DirectX::XMFLOAT3 m_panCommand;         
    
internal:
    // Accessor to set the position of the controller
    void SetPosition( _In_ DirectX::XMFLOAT3 pos );

       // Accessor to set the fixed "look point" of the controller
       DirectX::XMFLOAT3 get_FixedLookPoint();

    // Returns the position of the controller object
    DirectX::XMFLOAT3 get_Position();

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

    // Set up the Controls supported by this controller
    void Initialize( _In_ Windows::UI::Core::CoreWindow^ window );

    void Update( Windows::UI::Core::CoreWindow ^window );

};  // Class CameraPanController
```

私有字段包含相机控制器的当前状态。 下面我们了解这些内容。

-   **m\_position** 是相机在场景空间中的位置。 在本示例中，z 坐标值固定为 0。 我们可以使用 DirectX::XMFLOAT2 表示此值，但是对于本示例和为了未来可扩展性，我们使用 DirectX::XMFLOAT3。 我们通过 **get\_Position** 属性将此值传递到应用本身，以便它可以相应地更新视口。
-   **m\_panInUse** 是一个布尔值，用于指示是否激活平移操作；更具体地说，就是玩家是否在触摸屏幕和移动相机。
-   **m\_panPointerID** 是指针的唯一 ID。 我们在示例中不使用此项，但使用它可以很好地将控制器状态类与特定指针关联。
-   **m\_panFirstDown** 是屏幕上的点，指示在相机平移操作期间玩家首次触摸屏幕或单击鼠标的位置。 稍后我们将使用此值设置死区，以防止触摸屏幕时或鼠标轻度颤动时发生抖动。
-   **m\_panPointerPosition** 是玩家当前在屏幕上将指针移动到的点。 我们通过检查它相对于 **m\_panFirstDown** 的移动来确定玩家要移动的方向。
-   **m\_panCommand** 是相机控制器最后计算的命令：向上、向下、向左或向右。 由于我们使用固定到 x-y 平面上的相机，因此它可以是一个 DirectX::XMFLOAT2 值。

我们使用这 3 个事件处理程序更新相机控制器状态信息。

-   **OnPointerPressed** 是一个事件处理程序，当玩家在触摸表面上按下手指并且指针移动到所按位置的坐标时应用将调用此处理程序。
-   **OnPointerMoved** 是一个事件处理程序，玩家在触摸表面上轻扫手指时应用将调用此处理程序。 它将使用拖动路径的新坐标进行更新。
-   **OnPointerReleased** 是一个事件处理程序，玩家从触摸表面移开按着的手指时应用将调用此处理程序。

最后，我们使用这些方法和属性来初始化、访问和更新相机控制器状态信息。

-   **Initialize** 是一个事件处理程序，应用调用此处理程序来初始化控件，并将它们附加到描述显示窗口的 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 对象。
-   **SetPosition** 是一个方法，应用调用此方法在场景空间中设置控件的（x、y 和 z）坐标。 注意，z 坐标在整个教程中始终为 0。
-   **get_Position** 是一个属性，应用访问此属性来获取相机在场景空间中的当前位置。 使用此属性作为当前相机位置与应用进行通信的方法。
-   **get\_FixedLookPoint** 是一个属性，应用访问此属性来获取控制器相机面对的当前点。 在本示例中，它通常锁定到 x-y 平面。
-   **Update** 是一个方法，用于读取控制器状态和更新相机位置。 从应用的主循环中不断调用此 &lt;方法&gt; 可刷新相机控制器数据和相机在场景空间中的位置。

现在，你拥有了实现触摸控件所需的全部组件。 你可以检测触摸或鼠标指针事件发生的时间和位置，以及操作是什么。 你可以相对于场景空间设置相机的位置和方向，并跟踪更改。 最后，你可以将新相机位置传达给执行调用的应用。

现在，我们将各个片段连接在一起。

## <a name="create-the-basic-touch-events"></a>创建基本触摸事件


Windows 运行时事件调度程序提供我们希望自己的应用处理的 3 个事件：

-   [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208278)
-   [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208276)
-   [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208279)

这些事件在 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 类型上实现。 我们假设你有一个 **CoreWindow** 对象要处理。 有关详细信息，请参阅[如何设置 UWP C++ 应用以显示 DirectX 视图](https://msdn.microsoft.com/library/windows/apps/hh465077)。

由于在应用运行时将触发这些事件，因此处理程序将更新相机控制器在我们的私有字段中定义的状态信息。

首先，让我们填充触摸指针事件处理程序。 在第一个事件处理程序 **OnPointerPressed** 中，我们在用户触摸屏幕或单击鼠标时从用于管理显示的 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 中获取指针的 x-y 坐标。

**OnPointerPressed**

```cpp
void CameraPanController::OnPointerPressed(
                                           _In_ CoreWindow^ sender,
                                           _In_ PointerEventArgs^ args)
{
    // Get the current pointer position.
    uint32 pointerID = args->CurrentPoint->PointerId;
    DirectX::XMFLOAT2 position = DirectX::XMFLOAT2( args->CurrentPoint->Position.X, args->CurrentPoint->Position.Y );

    auto device = args->CurrentPoint->PointerDevice;
    auto deviceType = device->PointerDeviceType;
    

       if ( !m_panInUse )   // If no pointer is in this control yet.
    {
       m_panFirstDown = position;                   // Save the location of the initial contact.
       m_panPointerPosition = position;
       m_panPointerID = pointerID;              // Store the id of the pointer using this control.
       m_panInUse = TRUE;
    }
    
}
```

我们使用此处理程序让当前 **CameraPanController** 实例知道应将相机控制器视为处于活动状态（通过将 **m\_panInUse** 设置为 TRUE）。 这样，当应用调用 **Update** 时，它将使用当前位置数据更新视口。

现在，我们已经建立了当用户在显示窗口中触摸屏幕或点按时相机移动的基本值，我们必须确定当用户拖动鼠标点按或按住按钮移动鼠标时的操作。

每当指针移动、在玩家每次将其拖动到屏幕时将触发 **OnPointerMoved** 事件处理程序。 我们需要让应用知道指针的当前位置，通过上述方式可以做到这一点。

**OnPointerMoved**

```cpp
void CameraPanController::OnPointerMoved(
                                        _In_ CoreWindow ^sender,
                                        _In_ PointerEventArgs ^args)
{
    uint32 pointerID = args->CurrentPoint->PointerId;
    DirectX::XMFLOAT2 position = DirectX::XMFLOAT2( args->CurrentPoint->Position.X, args->CurrentPoint->Position.Y );

    m_panPointerPosition = position;
}
```

最后，当玩家停止触摸屏幕时，我们需要停用相机平移行为。 我们使用在触发 [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208279) 时调用的 **OnPointerReleased** 将 **m\_panInUse** 设置为 FALSE，并关闭相机平移，将指针 ID 设置为 0。

**OnPointerReleased**

```cpp
void CameraPanController::OnPointerReleased(
                                             _In_ CoreWindow ^sender,
                                             _In_ PointerEventArgs ^args)
{
    uint32 pointerID = args->CurrentPoint->PointerId;
    DirectX::XMFLOAT2 position = DirectX::XMFLOAT2( args->CurrentPoint->Position.X, args->CurrentPoint->Position.Y );

    m_panInUse = FALSE;
    m_panPointerID = 0;
}
```

## <a name="initialize-the-touch-controls-and-the-controller-state"></a>初始化触摸控件和控制器状态


让我们挂起这些事件并初始化相机控制器的所有基本状态字段。

**Initialize**

```cpp
void CameraPanController::Initialize( _In_ CoreWindow^ window )
{

    // Start recieving touch/mouse events.
    window->PointerPressed += 
    ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &CameraPanController::OnPointerPressed);

    window->PointerMoved += 
    ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &CameraPanController::OnPointerMoved);

    window->PointerReleased += 
    ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &CameraPanController::OnPointerReleased);


    // Initialize the state of the controller.
    m_panInUse = FALSE;             
    m_panPointerID = 0;

    //  Initialize this as it is reset on every frame.
    m_panCommand = DirectX::XMFLOAT3( 0.0f, 0.0f, 0.0f );

}
```

**Initialize** 将指向该应用的 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 实例的引用作为参数提供，并在该 **CoreWindow** 上注册我们已开发的相应事件的事件处理程序。

## <a name="getting-and-setting-the-position-of-the-camera-controller"></a>获取并设置相机控制器的位置


让我们定义一些方法，以便在场景空间中获取和设置相机控制器的位置。

```cpp
void CameraPanController::SetPosition( _In_ DirectX::XMFLOAT3 pos )
{
    m_position = pos;
}

// Returns the position of the controller object
DirectX::XMFLOAT3 CameraPanController::get_Position()
{
    return m_position;
}

DirectX::XMFLOAT3 CameraPanController::get_FixedLookPoint()
{
    // For this sample, we don't need to use the trig functions because our
    // look point is fixed. 
    DirectX::XMFLOAT3 result= m_position;
    result.z += 1.0f;
    return result;    

}
```

**SetPosition** 是一个公共方法，如果需要将相机控制器位置设置到特定的点，我们可以从应用中调用此方法。

**get\_Position** 是我们最重要的公共属性：它是应用在场景空间中获取相机控制器当前位置的方法，以便可以相应地更新视口。

**get_FixedLookPoint** 是一个公共属性，在本示例中用于获取垂直于 x-y 平面的视点。 在计算 x、y 和 z 轴坐标值时，如果要为固定相机创建更多斜角，可以更改此方法以使用三角函数正弦和余弦。

## <a name="updating-the-camera-controller-state-information"></a>更新相机控制器的状态信息


现在，我们将执行计算，将 **m\_panPointerPosition** 中跟踪的指针坐标信息转换为 3D 场景空间中相应的新坐标信息。 每当刷新主应用循环时，应用都会调用此方法。 我们需要在其中计算要传递给应用的新位置 信息，以便在投影到视口之前更新视图矩阵。

```cpp

void CameraPanController::Update( CoreWindow ^window )
{
    if ( m_panInUse )
    {
        pointerDelta.x = m_panPointerPosition.x - m_panFirstDown.x;
        pointerDelta.y = m_panPointerPosition.y - m_panFirstDown.y;

        if ( pointerDelta.x > 16.0f )        // Leave 32 pixel-wide dead spot for being still.
            m_panCommand.x += 1.0f;
        else
            if ( pointerDelta.x < -16.0f )
                m_panCommand.x += -1.0f;

        if ( pointerDelta.y > 16.0f )        
            m_panCommand.y += 1.0f;
        else
            if (pointerDelta.y < -16.0f )
                m_panCommand.y += -1.0f;
    }

       DirectX::XMFLOAT3 command = m_panCommand;
   
    // Our velocity is based on the command.
    DirectX::XMFLOAT3 Velocity;
    Velocity.x =  command.x;
    Velocity.y =  command.y;
    Velocity.z =  0.0f;

    // Integrate
    m_position.x = m_position.x + Velocity.x;
    m_position.y = m_position.y + Velocity.y;
    m_position.z = m_position.z + Velocity.z;

    // Clear the movement input accumulator for use during the next frame.
    m_panCommand = DirectX::XMFLOAT3( 0.0f, 0.0f, 0.0f );

}
```

由于我们不希望触摸或鼠标的抖动造成相机平移不稳定，因此围绕指针设置一个直径为 32 像素的死区。 我们还有一个速度值，在本例中为 1:1，该值是指针经过死区时跨过的像素与移动速度的比值。 你可以调整此行为来减慢或加快移动速度。

## <a name="updating-the-view-matrix-with-the-new-camera-position"></a>使用相机的新位置更新视图矩阵


现在，我们可以获取相机聚焦的场景空间坐标，每当指示你的应用执行更新坐标时，将执行此操作（例如在主屏循环中每 60 秒）。 此伪代码建议你可以实现的调用行为：

```cpp
 myCameraPanController->Update( m_window ); 

 // Update the view matrix based on the camera position.
 myCamera->MyMethodToComputeViewMatrix(
        myController->get_Position(),        // The position in the 3D scene space.
        myController->get_FixedLookPoint(),      // The point in the space we are looking at.
        DirectX::XMFLOAT3( 0, 1, 0 )                    // The axis that is "up" in our space.
        );  
```

恭喜你！ 你已在游戏中实现了一组简单的相机平移触摸控件。


 

 

 




