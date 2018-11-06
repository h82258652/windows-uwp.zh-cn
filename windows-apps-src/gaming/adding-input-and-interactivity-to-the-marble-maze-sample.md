---
author: eliotcowley
title: 向 Marble Maze 示例添加输入和交互性
description: 了解在处理输入设备时应记住的一些重要实践。
ms.assetid: b946bf62-c0ca-f9ec-1a87-8195b89a5ab4
ms.author: elcowle
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, 游戏, 输入, 示例
ms.localizationpriority: medium
ms.openlocfilehash: 0b7e9a3f655b8be1b93334ed8decf9fe6fa8bbf2
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/05/2018
ms.locfileid: "6041279"
---
# <a name="adding-input-and-interactivity-to-the-marble-maze-sample"></a>向 Marble Maze 添加输入和交互性示例




通用 Windows 平台 (UWP) 游戏可在广泛的设备上运行，如台式计算机、笔记本电脑和平板电脑。 一台设备可能拥有过多的输入和控制机制。 本文档介绍了在使用输入设备时要记住的重要实践，并展示了 Marble Maze 如何应用这些实践。

> [!NOTE]
> 与本文档对应的示例代码位于 [DirectX Marble Maze 游戏示例](http://go.microsoft.com/fwlink/?LinkId=624011)中。

 
本文档讨论了在游戏中使用输入时的一些重要事项：

-   如果可能，支持多种输入设备，使你的游戏能够适应更广泛的客户偏好和能力。 尽管游戏控制器和传感器的使用是可选的，但我们强烈建议使用它来增强玩家体验。 我们设计了游戏控制器和传感器 API 来帮助你更轻松地集成这些输入设备。

-   要初始化触摸，必须注册窗口事件，例如在指针被激活、释放和移动时。 要初始化加速计，可在初始化应用程序时创建一个 [Windows::Devices::Sensors::Accelerometer](https://msdn.microsoft.com/library/windows/apps/br225687) 对象。 Xbox 控制器不需要初始化。

-   对于单人游戏，考虑是否组合来自所有可能 Xbox 控制器的输入。 这样，你无需跟踪何种输入来自哪个控制器。 或者，只需跟踪仅来自最近添加的控制器的输入，如我们在此示例中所做的。

-   在处理输入设备之前处理 Windows 事件。

-   Xbox 控制器和加速计支持轮询。 也就是说，你可在需要数据时轮询它。 对于触摸，在可供你的输入处理代码访问的数据结构中记录各种触摸事件。

-   考虑是否将输入值规范化为一种通用格式。 这么做可简化游戏的其他组件如何解释输入内容，例如力学模拟，还可以使编写适用于不同屏幕分辨率的游戏更容易。

## <a name="input-devices-supported-by-marble-maze"></a>Marble Maze 支持的输入设备


Marble Maze 支持通过 Xbox 控制器、鼠标和触摸来选择菜单项，支持通过 Xbox 控制器、鼠标、触摸和加速计来控制游戏。 Marble Maze 使用 [Windows::Gaming::Input](https://docs.microsoft.com/uwp/api/windows.gaming.input) API 在控制器中轮询输入。 触摸让应用程序能够跟踪和响应指尖输入。 加速计是一种度量力量的传感器，它沿 X、Y 和 Z 轴应用。 通过使用 Windows 运行时，可轮询加速计设备的当前状态，以及通过 Windows 运行时事件处理机制接收触摸事件。

> [!NOTE]
> 本文档使用触摸表示触摸和鼠标输入，使用指针表示任何使用指针事件的设备。 因为触摸和鼠标使用标准指针事件，所以你可使用任何一种设备来选择菜单项和控制游戏。

 

> [!NOTE]
> 程序包清单将 **Landscape** 设置为仅支持的旋转方向，以免在旋转设备来滚动弹珠时更改方向。 若要查看程序包清单，在 Visual Studio 中打开**解决方案资源管理器**中的 **Package.appxmanifest**。

 

## <a name="initializing-input-devices"></a>初始化输入设备


Xbox 控制器不需要初始化。 要初始化触摸，必须注册窗口事件，例如指针被激活（例如，玩家按下鼠标按钮或触摸屏幕）、释放和移动时。 若要初始化加速计，必须在初始化应用程序时创建一个 [Windows::Devices::Sensors::Accelerometer](https://msdn.microsoft.com/library/windows/apps/br225687) 对象。

下面的示例展示了 **App::SetWindow** 方法如何注册 [Windows::UI::Core::CoreWindow::PointerPressed](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.PointerPressed)、[Windows::UI::Core::CoreWindow::PointerReleased](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.PointerReleased) 和 [Windows::UI::Core::CoreWindow::PointerMoved](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.PointerMoved) 指针事件。 这些事件在应用程序初始化期间和游戏循环之前注册。

这些事件在调用事件处理程序的单独线程中处理。

有关应用程序如何初始化的详细信息，请参阅 [Marble Maze 应用程序结构](marble-maze-application-structure.md)。

```cpp
window->PointerPressed += ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(
    this, 
    &App::OnPointerPressed);

window->PointerReleased += ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(
    this, 
    &App::OnPointerReleased);

window->PointerMoved += ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(
    this, 
    &App::OnPointerMoved);
```

**MarbleMazeMain** 类还创建了一个 **std::map** 对象来持有触摸事件。 这个 map 对象的键是一个唯一表示输入指针的值。 每个键映射到每个触摸点与屏幕中心之间的距离。 Marble Maze 随后使用这些值计算迷宫倾斜的程度。

```cpp
typedef std::map<int, XMFLOAT2> TouchMap;
TouchMap        m_touches;
```

**MarbleMazeMain** 类还持有一个 [Accelerometer](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Accelerometer) 对象。

```cpp
Windows::Devices::Sensors::Accelerometer^           m_accelerometer;
```

**Accelerometer** 对象在 **MarbleMazeMain** 构造函数中初始化，如下面的示例中所示。 [Windows::Devices::Sensors::Accelerometer::GetDefault](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Accelerometer.GetDefault) 方法返回默认加速计的一个实例。 如果没有默认加速计，**Accelerometer::GetDefault** 将返回 **nullptr**。

```cpp
// Returns accelerometer ref if there is one; nullptr otherwise.
m_accelerometer = Windows::Devices::Sensors::Accelerometer::GetDefault();
```

##  <a name="navigating-the-menus"></a>导航菜单

可使用鼠标、触摸或 Xbox 控制器导航菜单，如下所示：

-   使用方向板更改活动菜单项。
-   使用触摸、A 按钮或“菜单”按钮挑选一个菜单项或关闭当前菜单，例如高分表。
-   使用“菜单”按钮暂停或恢复游戏。
-   使用鼠标单击一个菜单项可选择该操作。

###  <a name="tracking-xbox-controller-input"></a>跟踪 Xbox 控制器输入

为了跟踪当前连接到设备的游戏板，**MarbleMazeMain** 定义成员变量 **m_myGamepads**，这是 [Windows::Gaming::Input::Gamepad](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepad) 对象的集合。 这在构造函数中初始化，如下所示：

```cpp
m_myGamepads = ref new Vector<Gamepad^>();

for (auto gamepad : Gamepad::Gamepads)
{
    m_myGamepads->Append(gamepad);
}
```

而且，**MarbleMazeMain** 构造函数在添加或删除游戏板时注册事件：

```cpp
Gamepad::GamepadAdded += 
    ref new EventHandler<Gamepad^>([=](Platform::Object^, Gamepad^ args)
{
    m_myGamepads->Append(args);
    m_currentGamepadNeedsRefresh = true;
});

Gamepad::GamepadRemoved += 
    ref new EventHandler<Gamepad ^>([=](Platform::Object^, Gamepad^ args)
{
    unsigned int indexRemoved;

    if (m_myGamepads->IndexOf(args, &indexRemoved))
    {
        m_myGamepads->RemoveAt(indexRemoved);
        m_currentGamepadNeedsRefresh = true;
    }
});
```

添加游戏板时，它将被添加到 **m_myGamepads**；删除游戏板时，我们会检查游戏板是否位于 **m_myGamepads** 中，如果是，则会将其删除。 在这两种情况下，我们都会将 **m_currentGamepadNeedsRefresh** 设置为 **true**，指示我们需要重新分配 **m_gamepad**。

最后，我们将游戏板分配到 **m_gamepad** 并将 **m_currentGamepadNeedsRefresh** 设置为 **false**：

```cpp
m_gamepad = GetLastGamepad();
m_currentGamepadNeedsRefresh = false;
```

在**更新**方法中，我们会检查是否需要重新分配 **m_gamepad**：

```cpp
if (m_currentGamepadNeedsRefresh)
{
    auto mostRecentGamepad = GetLastGamepad();

    if (m_gamepad != mostRecentGamepad)
    {
        m_gamepad = mostRecentGamepad;
    }

    m_currentGamepadNeedsRefresh = false;
}
```

如果 **m_gamepad** 确实需要重新分配，我们会使用 **GetLastGamepad** 将它分配到最近添加的游戏板，并将其如下定义：

```cpp
Gamepad^ MarbleMaze::MarbleMazeMain::GetLastGamepad()
{
    Gamepad^ gamepad = nullptr;

    if (m_myGamepads->Size > 0)
    {
        gamepad = m_myGamepads->GetAt(m_myGamepads->Size - 1);
    }

    return gamepad;
}
```

此方法仅在 **m_myGamepads** 中返回最后一个游戏板。

最多可将四个 Xbox 控制器连接到一台 Windows 10 设备。 若要避免确定哪个控制器是活动的，我们只跟踪最近添加的游戏板。 如果游戏支持多个玩家，你必须分别跟踪每个玩家的输入。

**MarbleMazeMain::Update** 方法轮询要进行输入的游戏板：

```cpp
if (m_gamepad != nullptr)
{
    m_oldReading = m_newReading;
    m_newReading = m_gamepad->GetCurrentReading();
}
```

我们使用 **m_oldReading** 跟踪在最后一帧中获取的输入读数，并使用 **m_newReading** 跟踪最新输入读数，我们通过调用 [Gamepad::GetCurrentReading](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepad.GetCurrentReading) 获取这些数据。 这将返回 [GamepadReading](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepadreading) 对象，其中包含游戏板当前状态的信息。

为了检查按钮是按下还是释放，我们定义 **MarbleMazeMain::ButtonJustPressed** 和 **MarbleMazeMain::ButtonJustReleased**，这将比较此帧和最后一帧的按钮读数。 这样，我们可以仅在最初按下或释放按钮时执行操作，而不是在按住按钮时执行操作：

```cpp
bool MarbleMaze::MarbleMazeMain::ButtonJustPressed(GamepadButtons selection)
{
    bool newSelectionPressed = (selection == (m_newReading.Buttons & selection));
    bool oldSelectionPressed = (selection == (m_oldReading.Buttons & selection));
    return newSelectionPressed && !oldSelectionPressed;
}

bool MarbleMaze::MarbleMazeMain::ButtonJustReleased(GamepadButtons selection)
{
    bool newSelectionReleased = 
        (GamepadButtons::None == (m_newReading.Buttons & selection));

    bool oldSelectionReleased = 
        (GamepadButtons::None == (m_oldReading.Buttons & selection));

    return newSelectionReleased && !oldSelectionReleased;
}
```

[GamepadButtons](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepadbuttons) 读数使用按位运算进行比较&mdash;我们检查是否使用*按位 and* (&) 按下按钮。 我们将确定是否通过比较旧读数和新读数按下或释放按钮。

我们使用上述方法检查是否按下了某些按钮，然后执行任何必须发生的相应操作。 例如，按下“菜单”按钮 (**GamepadButtons::Menu**) 时，游戏状态会从活动更改为暂停，或者从暂停更改为活动。

```cpp
if (ButtonJustPressed(GamepadButtons::Menu) || m_pauseKeyPressed)
{
    m_pauseKeyPressed = false;

    if (m_gameState == GameState::InGameActive)
    {
        SetGameState(GameState::InGamePaused);
    }  
    else if (m_gameState == GameState::InGamePaused)
    {
        SetGameState(GameState::InGameActive);
    }
}
```

我们还会检查玩家是否按下“视图”按钮，在这种情况下，我们将重启该游戏或清除高分表：

```cpp
if (ButtonJustPressed(GamepadButtons::View) || m_homeKeyPressed)
{
    m_homeKeyPressed = false;

    if (m_gameState == GameState::InGameActive ||
        m_gameState == GameState::InGamePaused ||
        m_gameState == GameState::PreGameCountdown)
    {
        SetGameState(GameState::MainMenu);
        m_inGameStopwatchTimer.SetVisible(false);
        m_preGameCountdownTimer.SetVisible(false);
    }
    else if (m_gameState == GameState::HighScoreDisplay)
    {
        m_highScoreTable.Reset();
    }
}
```

如果主菜单是活动的，活动的菜单项会在向上或向下按方向板时发生改变。 如果用户选择当前的选项，相应的 UI 元素将标记为已选择。

```cpp
// Handle menu navigation.
bool chooseSelection = 
    (ButtonJustPressed(GamepadButtons::A) 
    || ButtonJustPressed(GamepadButtons::Menu));

bool moveUp = ButtonJustPressed(GamepadButtons::DPadUp);
bool moveDown = ButtonJustPressed(GamepadButtons::DPadDown);

switch (m_gameState)
{
case GameState::MainMenu:
    if (chooseSelection)
    {
        m_audio.PlaySoundEffect(MenuSelectedEvent);
        if (m_startGameButton.GetSelected())
        {
            m_startGameButton.SetPressed(true);
        }
        if (m_highScoreButton.GetSelected())
        {
            m_highScoreButton.SetPressed(true);
        }
    }
    if (moveUp || moveDown)
    {
        m_startGameButton.SetSelected(!m_startGameButton.GetSelected());
        m_highScoreButton.SetSelected(!m_startGameButton.GetSelected());
        m_audio.PlaySoundEffect(MenuChangeEvent);
    }
    break;

case GameState::HighScoreDisplay:
    if (chooseSelection || anyPoints)
    {
        SetGameState(GameState::MainMenu);
    }
    break;

case GameState::PostGameResults:
    if (chooseSelection || anyPoints)
    {
        SetGameState(GameState::HighScoreDisplay);
    }
    break;

case GameState::InGamePaused:
    if (m_pausedText.IsPressed())
    {
        m_pausedText.SetPressed(false);
        SetGameState(GameState::InGameActive);
    }
    break;
}
```

### <a name="tracking-touch-and-mouse-input"></a>跟踪触摸和鼠标输入

对于触摸和鼠标输入，用户可通过触摸或单击一个菜单项来选择它。 下面的示例展示了 **MarbleMazeMain::Update** 方法如何处理指针输入来选择菜单项。 **m\_pointQueue** 成员变量跟踪用户在屏幕上触摸或单击的位置。 Marble Maze 收集指针输入的方式将在本文档后面[处理指针输入](#processing-pointer-input)一节中详细介绍。

```cpp
// Check whether the user chose a button from the UI. 
bool anyPoints = !m_pointQueue.empty();
while (!m_pointQueue.empty())
{
    UserInterface::GetInstance().HitTest(m_pointQueue.front());
    m_pointQueue.pop();
}
```

**UserInterface::HitTest** 方法确定所提供的点是否位于任何 UI 元素边界内。 任何通过此测试的 UI 元素都会标记为已触摸。 此方法使用 **PointInRect** 帮助程序函数确定所提供的点是否位于每个 UI 元素的边界内。

```cpp
void UserInterface::HitTest(D2D1_POINT_2F point)
{
    for (auto iter = m_elements.begin(); iter != m_elements.end(); ++iter)
    {
        if (!(*iter)->IsVisible())
            continue;

        TextButton* textButton = dynamic_cast<TextButton*>(*iter);
        if (textButton != nullptr)
        {
            D2D1_RECT_F bounds = (*iter)->GetBounds();
            textButton->SetPressed(PointInRect(point, bounds));
        }
    }
}
```

### <a name="updating-the-game-state"></a>更新游戏状态

在 **MarbleMazeMain::Update** 方法处理控制器和触摸输入后，如果按下任何按钮，它会更新游戏状态。

```cpp
// Update the game state if the user chose a menu option. 
if (m_startGameButton.IsPressed())
{
    SetGameState(GameState::PreGameCountdown);
    m_startGameButton.SetPressed(false);
}
if (m_highScoreButton.IsPressed())
{
    SetGameState(GameState::HighScoreDisplay);
    m_highScoreButton.SetPressed(false);
}
```

##  <a name="controlling-game-play"></a>控制游戏


游戏循环和 **MarbleMazeMain::Update** 方法一起更新游戏对象的状态。 如果游戏接受来自多台设备的输入，你可将来自所有设备的输入累积到一个变量集中，以便能编写更易于维护的代码。 **MarbleMazeMain::Update** 方法定义一个可累积来自所有设备的运动的变量集。

```cpp
float combinedTiltX = 0.0f;
float combinedTiltY = 0.0f;
```

不同输入设备的输入机制可能不同。 例如，指针输入使用 Windows 运行时事件处理模型来处理。 相反，在需要时轮询来自 Xbox 控制器的输入数据。 我们建议你始终遵循为给定设备规定的输入机制。 本节介绍 Marble Maze 如何从每个设备读取输入，它如何更新组合的输入值，以及它如何使用组合的输入值更新游戏的状态。

###  <a name="processing-pointer-input"></a>处理指针输入

处理指针输入时，可调用 [Windows::UI::Core::CoreDispatcher::ProcessEvents](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.processevents) 方法来处理窗口事件。 在更新或呈现场景之前，在游戏循环中调用此方法。 Marble Maze 在 **App::Run** 方法中执行此调用： 

```cpp
while (!m_windowClosed)
{
    if (m_windowVisible)
    {
        CoreWindow::GetForCurrentThread()->
            Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

        m_main->Update();

        if (m_main->Render())
        {
            m_deviceResources->Present();
        }
    }
    else
    {
        CoreWindow::GetForCurrentThread()->
            Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
    }
}
```

如果窗口可见，我们将 **CoreProcessEventsOption::ProcessAllIfPresent** 传递到 **ProcessEvents** 来处理所有排队的事件并立即返回；否则，我们传递 **CoreProcessEventsOption::ProcessOneAndAllPending** 来处理所有排队的事件，并等待下一个新事件。 在处理事件后，Marble Maze 呈现并显示下一帧。

Windows 运行时调用为每个发生的事件所注册的处理程序。 **App::SetWindow** 方法注册事件并将指针信息转发到 **MarbleMazeMain** 类。

```cpp
void App::OnPointerPressed(
    Windows::UI::Core::CoreWindow^ sender, 
    Windows::UI::Core::PointerEventArgs^ args)
{
    m_main->AddTouch(args->CurrentPoint->PointerId, args->CurrentPoint->Position);
}

void App::OnPointerReleased(
    Windows::UI::Core::CoreWindow^ sender, 
    Windows::UI::Core::PointerEventArgs^ args)
{
    m_main->RemoveTouch(args->CurrentPoint->PointerId);
}

void App::OnPointerMoved(
    Windows::UI::Core::CoreWindow^ sender, 
    Windows::UI::Core::PointerEventArgs^ args)
{
    m_main->UpdateTouch(args->CurrentPoint->PointerId, args->CurrentPoint->Position);
}
```

**MarbleMazeMain** 类通过对保留触摸事件的 map 对象进行更新来对指针事件做出反应。 **MarbleMazeMain::AddTouch** 方法在首次按下指针时调用，例如当用户最初在支持触摸的设备上触摸屏幕时。 **MarbleMazeMain::UpdateTouch** 方法在指针位置移动时调用。 **MarbleMazeMain::RemoveTouch** 方法在指针释放时调用，例如当用户停止触摸屏幕时。

```cpp
void MarbleMazeMain::AddTouch(int id, Windows::Foundation::Point point)
{
    m_touches[id] = PointToTouch(point, m_deviceResources->GetLogicalSize());

    m_pointQueue.push(D2D1::Point2F(point.X, point.Y));
}

void MarbleMazeMain::UpdateTouch(int id, Windows::Foundation::Point point)
{
    if (m_touches.find(id) != m_touches.end())
        m_touches[id] = PointToTouch(point, m_deviceResources->GetLogicalSize());
}

void MarbleMazeMain::RemoveTouch(int id)
{
    m_touches.erase(id);
}
```

**PointToTouch** 函数转换当前的指针位置，以便原点位于屏幕中心，然后按比例缩放坐标，以便它们大约在 -1.0 与 +1.0 之间。 这使跨不同输入方法以一致方式计算迷宫倾斜程度变得更容易。

```cpp
inline XMFLOAT2 PointToTouch(Windows::Foundation::Point point, Windows::Foundation::Size bounds)
{
    float touchRadius = min(bounds.Width, bounds.Height);
    float dx = (point.X - (bounds.Width / 2.0f)) / touchRadius;
    float dy = ((bounds.Height / 2.0f) - point.Y) / touchRadius;

    return XMFLOAT2(dx, dy);
}
```

**MarbleMazeMain::Update** 方法通过在倾斜系数上加上一个固定刻度值来更新组合的输入值。 这个刻度值通过试验多个不同的值来确定。

```cpp
// Account for touch input.
for (TouchMap::const_iterator iter = m_touches.cbegin(); 
    iter != m_touches.cend(); 
    ++iter)
{
    combinedTiltX += iter->second.x * m_touchScaleFactor;
    combinedTiltY += iter->second.y * m_touchScaleFactor;
}
```

### <a name="processing-accelerometer-input"></a>处理加速计输入

若要处理加速计输入，**MarbleMazeMain::Update** 方法会调用 [Windows::Devices::Sensors::Accelerometer::GetCurrentReading](https://msdn.microsoft.com/library/windows/apps/br225699) 方法。 此方法返回一个 [Windows::Devices::Sensors::AccelerometerReading](https://msdn.microsoft.com/library/windows/apps/br225688) 对象，它表示一个加速计读数。 **Windows::Devices::Sensors::AccelerometerReading::AccelerationX** 和 **Windows::Devices::Sensors::AccelerometerReading::AccelerationY** 属性具有分别沿 X 和 Y 轴的 G-Force 加速。

下面的示例展示了 **MarbleMazeMain::Update** 方法如何轮询加速计并更新组合的输入值。 倾斜设备时，重力会导致弹珠移动速度更快。

```cpp
// Account for sensors.
if (m_accelerometer != nullptr)
{
    Windows::Devices::Sensors::AccelerometerReading^ reading =
        m_accelerometer->GetCurrentReading();

    if (reading != nullptr)
    {
        combinedTiltX += 
            static_cast<float>(reading->AccelerationX) * m_accelerometerScaleFactor;

        combinedTiltY += 
            static_cast<float>(reading->AccelerationY) * m_accelerometerScaleFactor;
    }
}
```

因为无法确保用户计算机上是否有加速计，所以始终要在轮询加速计之前确保你有一个有效的 [Accelerometer](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Accelerometer) 对象。

### <a name="processing-xbox-controller-input"></a>处理 Xbox 控制器输入

在 **MarbleMazeMain::Update** 方法中，我们使用 **m_newReading** 处理左摇杆的输入：

```cpp
float leftStickX = static_cast<float>(m_newReading.LeftThumbstickX);
float leftStickY = static_cast<float>(m_newReading.LeftThumbstickY);

auto oppositeSquared = leftStickY * leftStickY;
auto adjacentSquared = leftStickX * leftStickX;

if ((oppositeSquared + adjacentSquared) > m_deadzoneSquared)
{
    combinedTiltX += leftStickX * m_controllerScaleFactor;
    combinedTiltY += leftStickY * m_controllerScaleFactor;
}
```

我们检查左摇杆的输入是否在死区之外，如果是，我们会将它添加到 **combinedTiltX** 和 **combinedTiltY**（乘以比例系数）使舞台倾斜。

> [!IMPORTANT]
> 使用 Xbox 控制器时，请始终考虑死区。 死区指各个游戏板在对初始移动的敏感性上的差异。 在一些控制器中，较小的运动可能不会生成读数，但在其他控制器中，它可能生成明显的读数。 若要在游戏中考虑此情形，可为初始操纵杆运动创建一个不运动区域。 有关死区的详细信息，请参阅[读取操纵杆](gamepad-and-vibration.md#reading-the-thumbsticks)。

 

###  <a name="applying-input-to-the-game-state"></a>将输入应用到游戏状态

设备以不同方式报告输入值。 例如，指针输入可能采用屏幕坐标格式，而控制器输入可能采用完全不同的格式。 将来自多台设备的输入组合到一组输入值中的一项挑战是标准化，或者将值转换为通用格式。 Marble Maze 通过在 \[-1.0, 1.0\] 区间内缩放这些值来规范化它们。 **PointToTouch** 函数（本节前面已介绍）将屏幕坐标转换为大体在 -1.0 与 +1.0 之间的规范化值。

> [!TIP]
> 即使你的应用程序仅使用一种输入方法，我们也建议你始终规范输入值。 执行此操作可简化游戏的其他组件如何解释输入（例如力学模拟），以及使编写适用于不同屏幕分辨率的游戏更容易。

 

在 **MarbleMazeMain::Update** 方法处理输入后，将会创建一个矢量来表示在倾斜迷宫时对弹珠的影响。 下面的示例展示了 Marble Maze 如何使用 [XMVector3Normalize](https://msdn.microsoft.com/library/windows/desktop/microsoft.directx_sdk.geometric.xmvector3normalize) 函数创建一个规范化的重力矢量。 **maxTilt** 变量约束迷宫倾斜的程度并阻止迷宫朝一侧倾斜。

```cpp
const float maxTilt = 1.0f / 8.0f;

XMVECTOR gravity = XMVectorSet(
    combinedTiltX * maxTilt, 
    combinedTiltY * maxTilt, 
    1.0f, 
    0.0f);

gravity = XMVector3Normalize(gravity);
```

若要完成场景对象的更新，Marble Maze 将更新的重力矢量传递给力学模拟，在力学模拟中更新自前一帧以来经历的时间，并更新弹珠的位置和方向。 如果弹珠从迷宫落下，**MarbleMazeMain::Update** 方法将弹珠放回它最后接触的检查点，并重置力学模拟的状态。

```cpp
XMFLOAT3A g;
XMStoreFloat3(&g, gravity);
m_physics.SetGravity(g);

if (m_gameState == GameState::InGameActive)
{
    // Only update physics when gameplay is active.
    m_physics.UpdatePhysicsSimulation(static_cast<float>(m_timer.GetElapsedSeconds()));

    // ...Code omitted for simplicity...

}

// ...Code omitted for simplicity...

// Check whether the marble fell off of the maze. 
const float fadeOutDepth = 0.0f;
const float resetDepth = 80.0f;
if (marblePosition.z >= fadeOutDepth)
{
    m_targetLightStrength = 0.0f;
}
if (marblePosition.z >= resetDepth)
{
    // Reset marble.
    memcpy(&marblePosition, &m_checkpoints[m_currentCheckpoint], sizeof(XMFLOAT3));
    oldMarblePosition = marblePosition;
    m_physics.SetPosition((const XMFLOAT3&)marblePosition);
    m_physics.SetVelocity(XMFLOAT3(0, 0, 0));
    m_lightStrength = 0.0f;
    m_targetLightStrength = 1.0f;

    m_resetCamera = true;
    m_resetMarbleRotation = true;
    m_audio.PlaySoundEffect(FallingEvent);
}
```

本节没有介绍力学模拟的工作原理。 有关这方面的细节，请参阅 Marble Maze 源代码中的 **Physics.h** 和 **Physics.cpp**。

## <a name="next-steps"></a>后续步骤


查阅[向 Marble Maze 添加音频示例](adding-audio-to-the-marble-maze-sample.md)，了解在使用音频时要记住的一些重要实践。 本文档讨论了 Marble Maze 如何使用 Microsoft 媒体基础和 XAudio2 来加载、混合和播放音频资源。

## <a name="related-topics"></a>相关主题


* [向 Marble Maze 添加音频示例](adding-audio-to-the-marble-maze-sample.md)
* [向 Marble Maze 添加可视内容示例](adding-visual-content-to-the-marble-maze-sample.md)
* [开发 Marble Maze，一款使用 C++ 和 DirectX 的 UWP 游戏](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)

 

 




