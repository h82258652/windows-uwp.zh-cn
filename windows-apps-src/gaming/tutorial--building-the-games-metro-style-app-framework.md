---
定义游戏的通用 Windows 平台 (UWP) 应用框架
为通用 Windows 平台 (UWP) DirectX 游戏进行编码的第一部分是生成使游戏对象与 Windows 交互的框架。
ms.assetid: 7beac1eb-ba3d-e15c-44a1-da2f5a79bb3b
---

#  定义游戏的通用 Windows 平台 (UWP) 应用框架


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 的文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

为通用 Windows 平台 (UWP) DirectX 游戏进行编码的第一部分是生成使游戏对象与 Windows 交互的框架。 这包括 Windows 运行时属性，如暂停/恢复事件处理、窗口焦点和贴靠，以及用户界面的事件、交互和过渡。 我们将了解该示例游戏是如何构建的，以及它如何为玩家与系统交互定义高级状态机。

## 目标


-   为 UWP DirectX 游戏设置框架，以及实现定义整个游戏流的状态机。

## 初始化和启动视图提供程序


在任何 UWP DirectX 游戏中，必须获取应用单一实例（即定义运行应用的实例的 Windows 运行时对象） 能够访问其所需图形资源的视图提供程序。 通过 Windows 运行时，你的应用可以直接与图形界面连接，但需要指定所需的资源和处理资源的方式。

如我们在[设置游戏项目](tutorial--setting-up-the-games-infrastructure.md)中所述，Microsoft Visual Studio 2015 在 **Sample3DSceneRenderer.cpp** 文件中提供了 DirectX 的基本呈现器的实现，该文件在你选取**“DirectX 11 应用(通用 Windows)”**模板时提供。

有关理解和创建视图提供程序和呈现器的更多详细信息，请参阅[如何设置使用 C++ 和 DirectX 的 UWP 以显示 DirectX 视图](https://msdn.microsoft.com/library/windows/apps/hh465077)。

也就是说，你必须提供应用单一实例调用的 5 个方法的实现：

-   [**Initialize**](https://msdn.microsoft.com/library/windows/apps/hh700495)
-   [**SetWindow**](https://msdn.microsoft.com/library/windows/apps/hh700509)
-   [**Load**](https://msdn.microsoft.com/library/windows/apps/hh700501)
-   [**Run**](https://msdn.microsoft.com/library/windows/apps/hh700505)
-   [**Uninitialize**](https://msdn.microsoft.com/library/windows/apps/hh700523)

在 DirectX11 应用（通用 Windows）模板中，这 5 种方法在 [App.h](#code_sample) 中的 **App** 对象中定义。 让我们看一下它们在此游戏中的实现方式。

视图提供程序的初始化方法

```cpp
void App::Initialize(
    _In_ CoreApplicationView^ applicationView
    )
{
    applicationView->Activated +=
        ref new TypedEventHandler<CoreApplicationView^, IActivatedEventArgs^>(this, &App::OnActivated);

    CoreApplication::Suspending +=
        ref new EventHandler<SuspendingEventArgs^>(this, &App::OnSuspending);

    CoreApplication::Resuming +=
        ref new EventHandler<Platform::Object^>(this, &App::OnResuming);

    m_controller = ref new MoveLookController();
    m_renderer = ref new GameRenderer();
    m_game = ref new Simple3DGame();
}
```

应用单一实例首先调用 **Initialize**。 因此，使用此方法处理 UWP 游戏的大多数基本行为非常重要，如处理主窗口的激活以及确保游戏可以处理突然暂停（可能随后恢复）事件。

在初始化游戏应用时，该方法为控制器分配特定内存以允许玩家开始提供输入。 它还创建游戏的呈现程序和状态机的未初始化的新实例。 我们将在[定义主游戏对象](tutorial--defining-the-main-game-loop.md)中进行详细讨论。

目前，游戏应用能够处理暂停（或恢复）消息，并为控制器、呈现程序和游戏本身分配了内存。 但还没有要使用的窗口，并且游戏未初始化。 还需要提供更多内容！

视图提供程序的 SetWindow 方法

```cpp
void App::SetWindow(
    _In_ CoreWindow^ window
    )
{
    window->PointerCursor = ref new CoreCursor(CoreCursorType::Arrow, 0);

    window->SizeChanged +=
        ref new TypedEventHandler<CoreWindow^, WindowSizeChangedEventArgs^>(this, &App::OnWindowSizeChanged);

    window->Closed +=
        ref new TypedEventHandler<CoreWindow^, CoreWindowEventArgs^>(this, &App::OnWindowClosed);

    window->VisibilityChanged +=
        ref new TypedEventHandler<CoreWindow^, VisibilityChangedEventArgs^>(this, &App::OnVisibilityChanged);

    DisplayProperties::LogicalDpiChanged +=
        ref new DisplayPropertiesEventHandler(this, &App::OnLogicalDpiChanged);

    m_controller->Initialize(window);

    m_controller->SetMoveRect(
        XMFLOAT2(0.0f, window->Bounds.Height - GameConstants::TouchRectangleSize),
        XMFLOAT2(GameConstants::TouchRectangleSize, window->Bounds.Height)
        );
    m_controller->SetFireRect(
        XMFLOAT2(window->Bounds.Width - GameConstants::TouchRectangleSize, window->Bounds.Height - GameConstants::TouchRectangleSize),
        XMFLOAT2(window->Bounds.Width, window->Bounds.Height)
        );

    m_renderer->Initialize(window, DisplayProperties::LogicalDpi);
    SetGameInfoOverlay(GameInfoOverlayState::Loading);
    ShowGameInfoOverlay();
}
```

现在，通过调用 [**SetWindow**](https://msdn.microsoft.com/library/windows/apps/hh700509) 的实现，应用 singleton 提供一个表示游戏主窗口的 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 对象，并将其资源和事件提供给游戏。 由于提供了可以使用的窗口，游戏现在可以开始添加基本的用户界面组件和事件：一个指针（由鼠标和触摸控件使用）、一个用于窗口调整、关闭和 DPI 更改的基本事件（如果显示设备更改）。

游戏应用还初始化控制器（因为有一个要交互的窗口），并初始化游戏对象本身。 它可以从控制器（触摸、鼠标或 XBox 360 控制器）读取输入。

控制器初始化后，应用在屏幕的左下角和右下角定义两个矩形区域，分别用于移动和相机触摸控件。 左下角的矩形通过调用 **SetMoveRect** 定义，玩家将该矩形用作虚拟控制板来前后左右移动相机。 右下角的矩形由 **SetFireRect** 方法定义，可用作设计弹药的虚拟按钮。

这些组件开始汇集起来。

视图提供程序的 Load 方法

```cpp
void App::Load(
    Platform::String^ entryPoint
    )
{
    task<void>([this]()
    {
        m_game->Initialize(m_controller, m_renderer);

        return m_renderer->CreateGameDeviceResourcesAsync(m_game);

    }).then([this]()
    {
        // The finalize code needs to run in the same thread context
        // in which the m_renderer object was created because the D3D device context
        // can be accessed only on a single thread.
        m_renderer->FinalizeCreateGameDeviceResources();

        InitializeGameState();

        if (m_updateState == UpdateEngineState::WaitingForResources)
        {
            // In the middle of a game, so spin up the async task to load the level.
            create_task([this]()
            {
                return m_game->LoadLevelAsync();

            }).then([this]()
            {
                // The m_game object may need to deal with D3D device context work, so
                // the finalizer code needs to run in the same thread
                // context as the m_renderer object was created because the D3D 
                // device context can  be accessed only on a single thread.
                m_game->FinalizeLoadLevel();
                m_updateState = UpdateEngineState::ResourcesLoaded;

            }, task_continuation_context::use_current());
        }
    }, task_continuation_context::use_current());
}
```

在设置主窗口之后，应用单一实例将调用 **Load**。 在本示例中，此方法使用一组异步任务（其语法在[并行模式库](https://msdn.microsoft.com/library/windows/apps/dd492418.aspx)中定义）来创建游戏对象、加载图形资源以及初始化游戏的状态机。 通过使用异步任务模式，Load 方法可快速完成并允许应用开始处理输入。 在此方法中，应用还在资源文件加载时显示进度栏。

我们将资源加载分成两个独立阶段，因为对 Direct3D 11 设备上下文的访问限于原先创建设备上下文的线程，而对用于设备创建的 Direct3D 11 设备的访问无线程限制。 **CreateGameDeviceResourcesAsync** 任务在不同于完成任务 (*FinalizeCreateGameDeviceResources*) 的单独线程上运行，而完成任务在原始线程上运行。 我们通过 **LoadLevelAsync** 和 **FinalizeLoadLevel** 使用类似的模式来加载关卡资源。

创建游戏的对象并加载图形资源后，我们将游戏的状态机初始化为开始条件（例如：设置初始弹药计数、关卡编号和对象位置）。 如果游戏状态指示玩家正在恢复游戏，则我们可加载当前关卡（游戏先前暂停时玩家所处的关卡）。

在 **Load** 方法中，我们会在游戏开始之前做一些必要的准备，如设置任意开始状态或全局值。 如果你希望预取游戏数据或资源，此位置比 **SetWindow** 或 **Initialize** 更好。 由于 Windows 会对你的游戏在必须开始处理输入前所能花费的时间施加限制，因此请在游戏中使用异步任务进行任何加载。 如果加载需要一段时间（如果有很多资源），则向用户提供一个定期更新的进度栏。

在开发自己的游戏时，请围绕这些方法设计启动代码。 此处是对各方法的基本建议的简单列表：

-   使用 **Initialize** 分配主类和连接基本事件处理程序。
-   使用 **SetWindow** 创建主窗口和连接任何特定于窗口的事件。
-   使用 **Load** 来处理任何其余设置、启动对象的异步创建和加载资源。 如果需要创建任何临时文件或数据，如按顺序生成的资源，也在这里完成。

这样，示例游戏创建游戏状态机的一个实例，并将其设置为开始配置。 它处理所有系统事件和输入事件。 还提供一个用于显示内容的窗口。 游戏执行代码现在已准备好运行。

视图提供程序的 Run 方法

```cpp
void App::Run()
{
    while (!m_windowClosed)
    {
        if (m_visible)
        {
            switch (m_updateState)
            {
            case UpdateEngineState::Deactivated:
            case UpdateEngineState::Snapped:
                if (!m_renderNeeded)
                {
                    // The app is not currently the active window, so just wait for events.
                    CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
                    break;
                }
                // Otherwise, fall through and do normal processing to get the rendering handled.
            default:
                CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);
                Update();
                m_renderer->Render();
                m_renderNeeded = false;
            }
        }
        else
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
    m_game->OnSuspending();  // Exiting due to window close.  Make sure to save the state.
}
```

下面开始介绍游戏应用的执行部分。 运行了 3 个方法并设置阶段之后，游戏应用将运行 **Run** 方法，这时将启动有趣的游戏！

在该游戏示例中，我们启动一个 while 循环，当玩家关闭游戏窗口时该循环将终止。 示例代码在游戏引擎状态机中转换为以下两个状态之一：

-   游戏窗口停用（失去焦点）或贴靠。 在发生此事件时，游戏将暂停事件处理并等待窗口获得焦点或取消贴靠。
-   否则，游戏将更新自己的状态并呈现要显示的图形。

在你的游戏获得焦点时，必须处理消息队列中到达的每个事件，因此必须使用 **ProcessAllIfPresent** 选项调用 [**CoreWindowDispatch.ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215)。 其他选项可能导致延迟处理消息事件，这会让人感到游戏停止响应，或者导致触摸行为反应慢和没有“粘合力”。

当然，当应用不可见、暂停或贴靠时，我们不希望应用使用任何资源循环去调度永不会到达的消息。 因此你的游戏必须使用 **ProcessOneAndAllPending**，它在获得事件前将进行阻止，随即在获得事件后处理该事件以及处理队列中在处理第一个事件期间到达的任何其他事件。 然后，[**ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215) 将在处理队列之后立即返回。

游戏正在运行！ 用于在游戏状态之间转换的事件将被发送和处理。 在游戏循环周期，图形得到更新。 我们希望玩家感到开心。 但最后不得不停止...

...我们需要清除位置。 这时要使用 **Uninitialize**。

视图提供程序的 Uninitialize 方法

```cpp
void App::Uninitialize()
{
}
```

在该游戏示例中，我们让游戏的应用单一实例在游戏终止时清除所有内容。 在 Windows 10 中， 关闭游戏窗口不会终止应用的进程，而是将应用 singleton 的状态写入内存。 如果在系统回收此内存时必须执行一些特殊的资源清除，则将执行该清除的代码放入此方法。

我们在本教程中会再次提到这 5 种方法，因此请记住它们。 现在，我们了解一下游戏引擎的总体结构以及定义它的状态机。

## 初始化游戏引擎状态


由于用户可以随时从暂停状态恢复 UWP 游戏应用，因此应用可以有任意数量的可能状态。

游戏示例在启动时可能处于以下三种状态之一：

-   游戏循环正在运行并处于某个级别的中间。
-   由于游戏刚刚完成，游戏循环未运行。 （设置了高分。）
-   没有启动游戏或者游戏在两个级别之间。 （高分为 0。）

当然，在你自己的游戏中，你可以有更多或更少的状态。 此外，请始终记住 UWP 游戏可能随时终止， 在恢复时，玩家预期游戏像从来没有停止过一样运作。

在该游戏示例中，代码流如下所示。

```cpp
void App::InitializeGameState()
{
    //
    // Set up the initial state machine for handling game playing state.
    //
    if (m_game->GameActive() && m_game->LevelActive())
    {
        m_updateState = UpdateEngineState::WaitingForResources;
        // ...

    }
    else if (!m_game->GameActive() && (m_game->HighScore().totalHits > 0))
    {
        m_updateState = UpdateEngineState::WaitingForPress;
        // ...
    }
    else
    {
        m_updateState = UpdateEngineState::WaitingForResources;
        // ...
    }
    SetAction(GameInfoOverlayCommand::PleaseWait);
    ShowGameInfoOverlay();
}
```

初始化不像是冷启动应用，更像是在应用中止后重新启动应用。 示例游戏始终保存状态，这样可以使应用显示为始终在运行。 暂停状态就是：游戏运行暂停，但游戏的资源仍然在内存中。 同样，恢复事件指示该示例游戏正在拾取其上次暂停或终止的位置。 当示例游戏在终止之后重新启动时，它将正常启动，然后确定上次的已知状态，以便玩家可以立即继续玩游戏。

流程图展示了游戏示例的初始化过程的初始状态和转换。

![在主循环开始之前初始化和准备游戏的过程](images/simple3dgame-appstartup.png)

根据游戏的状态，为玩家提供不同的选项。 如果游戏在级别中间恢复，游戏将显示为暂停，覆盖层将提供一个继续选项。 如果游戏恢复到游戏已经完成的状态，它将显示高得分和一个玩新游戏的选项。 最后，如果游戏在某个级别开始之前恢复，则覆盖层为用户提供一个开始选项。

该游戏示例不区分本身冷启动的游戏（即首次启动的无暂停事件的游戏）和从暂停状态恢复的游戏。 这是任何 UWP 应用的正确设计。

## 处理事件


我们的示例代码为 **Initialize**、**SetWindow** 和 **Load** 中的特定事件注册了大量处理程序。 你可能猜到这些是重要的事件，因为代码示例在转到任何游戏机或图形开发之前运行状况良好。 你猜对了！ 这些事件是获得正确的 UWP 应用体验的基础，由于 UWP 应用可随时激活、停用、调整大小、贴靠、取消贴靠、暂停 或恢复，因此游戏必须尽快注册这些事件，并且处理这些事件的方式应保证玩家获得流畅和可预期的体验。

下面是示例中的事件处理程序，以及它们处理的事件。 你可以在[此部分的完整代码](#code_sample)中找到这些事件处理程序的完整代码

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">事件处理程序</th>
<th align="left">描述</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">OnActivated</td>
<td align="left">处理 [<strong>CoreApplicationView::Activated</strong>](https://msdn.microsoft.com/library/windows/apps/br225018)。 游戏应用已经进入前台，因此将激活主窗口。</td>
</tr>
<tr class="even">
<td align="left">OnLogicalDpiChanged</td>
<td align="left">处理 [<strong>DisplayProperties::LogicalDpiChanged</strong>](https://msdn.microsoft.com/library/windows/apps/br226150)。 主游戏窗口的 DPI 已更改，游戏应用相应地调整其资源。
<div class="alert">
<strong>注意</strong> [<strong>CoreWindow</strong>](https://msdn.microsoft.com/library/windows/desktop/hh404559) 坐标以 DIP 为单位（与设备无关的像素），和在 [Direct2D](https://msdn.microsoft.com/library/windows/desktop/dd370987) 中一样。 因此，要正确显示任何 2D 资源或基元，必须通知 Direct2D 关于 DPI 的更改。
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left">OnResuming</td>
<td align="left">处理 [<strong>CoreApplication::Resuming</strong>](https://msdn.microsoft.com/library/windows/apps/br205859)。 游戏应用从暂停状态恢复游戏。</td>
</tr>
<tr class="even">
<td align="left">OnSuspending</td>
<td align="left">处理 [<strong>CoreApplication::Suspending</strong>](https://msdn.microsoft.com/library/windows/apps/br205860)。 游戏应用将其状态保存到磁盘。 它有 5 秒时间将状态保存到存储。</td>
</tr>
<tr class="odd">
<td align="left">OnVisibilityChanged</td>
<td align="left">处理 [<strong>CoreWindow::VisibilityChanged</strong>](https://msdn.microsoft.com/library/windows/apps/hh701591)。 游戏应用的可见性已更改，变为可见或因另一个应用变为可见而致使其不可见。</td>
</tr>
<tr class="even">
<td align="left">OnWindowActivationChanged</td>
<td align="left">处理 [<strong>CoreWindow::Activated</strong>](https://msdn.microsoft.com/library/windows/apps/br208255)。 游戏应用的主窗口已停用或激活，因此它必须删除焦点并暂停游戏，或者重新获得焦点。 在这两种情况下，覆盖层都指示游戏已暂停。</td>
</tr>
<tr class="odd">
<td align="left">OnWindowClosed</td>
<td align="left">处理 [<strong>CoreWindow::Closed</strong>](https://msdn.microsoft.com/library/windows/apps/br208261)。 游戏应用关闭主窗口并暂停游戏。</td>
</tr>
<tr class="even">
<td align="left">OnWindowSizeChanged</td>
<td align="left">处理 [<strong>CoreWindow::SizeChanged</strong>](https://msdn.microsoft.com/library/windows/apps/br208283)。 游戏应用重新分配图形资源和覆盖层以适应大小更改，然后更新呈现器目标。</td>
</tr>
</tbody>
</table>

 

你自己的游戏必须处理这些事件，因为它们是 UWP 应用设计的一部分。

## 更新游戏引擎


在 **Run** 的游戏循环中，示例已实现了一个基本状态机，用于处理玩家可以采取的所有主要操作。 此状态机的最高级别处理加载游戏、玩特定关卡，或者在游戏暂停之后继续某个关卡（通过系统或玩家）。

在该游戏示例中，游戏可以处于以下 3 个主要状态 (UpdateEngineState)：

-   **Waiting for resources**。 游戏循环正在执行，在资源（特别是图形资源）可用之前无法转换。 用于加载资源的异步任务完成后，它将状态更新为 **ResourcesLoaded**。 当关卡可能需要从磁盘加载新资源时发生，通常在关卡之间发生此状态。 在该游戏示例中，将模拟此行为，因为该示例此时不需要任何附加的每关卡资源。
-   **Waiting for press**。 游戏循环正在执行，正在等待特定的用户输入。 此输入是玩家加载游戏、开始某个级别或继续某个级别的操作。 示例代码引用这些子状态作为 PressResultState 枚举值。
-   **Dynamics**。 在用户玩游戏过程中游戏循环正在运行。 在用户玩游戏过程中，游戏检查其可以转换的 3 个条件：某个级别的设定时间到期、玩家完成某个级别或者玩家完成所有级别。

下面是代码结构。 完整代码在[此部分的完整代码](#code_sample)中提供。

用于更新游戏引擎的状态机的结构

```cpp
void App::Update()
{
    m_controller->Update();

    switch (m_updateState)
    {
    case UpdateEngineState::WaitingForResources:
        // Waiting for initial load.  Display an update once per 60 updates.
        loadCount++;
        if ((loadCount % 60) == 0)
        {
            m_loadingCount++;
            SetGameInfoOverlay(m_gameInfoOverlayState);
        }
        break;

    case UpdateEngineState::ResourcesLoaded:
        switch (m_pressResult)
        {
        case PressResultState::LoadGame:
            // ...
            break;

        case PressResultState::PlayLevel:
            // ...
            break;

        case PressResultState::ContinueLevel:
            // ...
            break;
        }
        // ...
        break;

    case UpdateEngineState::WaitingForPress:
        if (m_controller->IsPressComplete() || m_pressComplete)
        {
            m_pressComplete = false;

            switch (m_pressResult)
            {
            case PressResultState::LoadGame:
                // ...
                break;

            case PressResultState::PlayLevel:
                // ...
                break;

            case PressResultState::ContinueLevel:
                // ...
                break;
            }
        }
        break;

    case UpdateEngineState::Dynamics:
        if (m_controller->IsPauseRequested() || m_pauseRequested)
        {
            // ...
        }
        else 
        {
            GameState runState = m_game->RunGame();
            switch (runState)
            {
            case GameState::TimeExpired:
                // ...
                break;

            case GameState::LevelComplete:
                // ...
                break;

            case GameState::GameComplete:
                // ...
                break;
            }
        }

        if (m_updateState == UpdateEngineState::WaitingForPress)
        {
            // transitioning state, so enable waiting for the press event
            m_controller->WaitForPress(m_game->GameInfoOverlayUpperLeft(), m_game->GameInfoOverlayLowerRight());
        }
        if (m_updateState == UpdateEngineState::WaitingForResources)
        {
            // Transitioning state, so shut down the input controller until resources are loaded
            m_controller->Active(false);
        }

        break;
    }
}
```

从视觉上讲，主游戏状态机如下所示：

![游戏的主状态机](images/simple3dgame-mainstatemachine.png)

我们将在[定义主游戏对象](tutorial--defining-the-main-game-loop.md)中更详细地讨论游戏逻辑本身。 现在，重要的是你的游戏是状态机。 每个特定的状态机必须有非常特定的标准来定义它，并且从一种状态到另一状态的转换必须基于离散用户的输入或系统操作（如图形资源加载）。 在规划游戏时，应绘制图示，如我们使用的图示，确保在较高层次上解决用户或系统可能采取的所有可能操作。 游戏可能非常复杂，而状态机是一个强大的工具，可以直观显示这一复杂性，并使之非常易于管理。

当然，我们已经说过，有的状态机存在于状态机内部。 控制器有一个状态机，用于处理玩家可能生成的所有可接受的输入。 在图中，点按是用户输入的一种形式。 此状态机在较高级别工作，不关心是哪种输入；它假定控制器的状态机将处理影响运动和射击行为的任何转换，以及相关的呈现更新。 我们将在[添加控件](tutorial--adding-controls.md)中讨论管理输入状态。

## 更新用户界面


我们需要通知玩家系统的状态，并允许玩家根据游戏规则更改高级状态。 对于大多数游戏，包括此游戏示例，这通过提醒显示实现，其中包含游戏状态的显示以及其他特定于游戏的信息，如得分、弹药或剩余的机会数。 我们将此显示称为覆盖层，因为它独立于主图形管道呈现，并放在 3D 投影的顶部。 在该示例游戏中，我们使用 Direct2D API 创建此覆盖层。 我们还可以使用 XAML 创建此覆盖层，这在[扩展游戏示例](tutorial-resources.md)中讨论。

用户界面有两个组件：

-   提醒显示，包含得分和有关游戏执行的当前状态信息。
-   暂停位图，这是一个在游戏的暂停状态期间由文本覆盖的黑色矩形。 这是游戏覆盖层。 我们将在[添加用户界面](tutorial--adding-a-user-interface.md)中进一步讨论。

毫不奇怪，覆盖层也有一个状态机。 覆盖层可以显示一个级别的开始或游戏结束消息。 它实际上是一块画布，在游戏暂停时，将我们显示的有关游戏状态的任何信息输出给玩家。

下面是游戏示例构造覆盖层状态机的方式。

```cpp
void App::SetGameInfoOverlay(GameInfoOverlayState state)
{
    m_gameInfoOverlayState = state;
    switch (state)
    {

    case GameInfoOverlayState::Loading:
        m_renderer->InfoOverlay()->SetGameLoading(m_loadingCount);
        break;

    case GameInfoOverlayState::GameStats:
        // ...
        break;

    case GameInfoOverlayState::LevelStart:
        // ...
        break;

    case GameInfoOverlayState::GameOverCompleted:
        // ...
        break;

    case GameInfoOverlayState::GameOverExpired:
        // ...
        break;

    case GameInfoOverlayState::Pause:
        // ...
        break;
    }
}
```

覆盖层共显示 6 个状态屏幕，具体取决于游戏本身的状态：游戏开始时的资源加载屏幕、游戏执行屏幕、级别开始消息屏幕、当所有级别完成而没用完时间时的游戏结束屏幕、时间用完时的游戏结束屏幕，以及暂停菜单屏幕。

将用户界面与游戏的图形管道分开可以独立于游戏的图形呈现引擎来使用用户界面，并显著降低游戏代码的复杂性。

## 后续步骤


综上所述，我们介绍了游戏示例的基本结构，并为使用 DirectX 进行 UWP 游戏应用开发提供了一个很好的模型。 当然，它的内容实际上远不止这些。 我们只是对游戏的 skeleton 大致过了一遍而已。 现在，我们将深入讨论游戏及其技术，以及这些技术如何实现为核心游戏对象。 我们将在[定义主游戏对象](tutorial--defining-the-main-game-loop.md)中介绍该部分。

还需要更详细地了解示例游戏的图形引擎。 该部分将在[装配呈现管道](tutorial--assembling-the-rendering-pipeline.md)中介绍。

## 这部分的完整示例代码


App.h

```cpp
///// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

#include "Simple3DGame.h"

enum class UpdateEngineState
{
    WaitingForResources,
    ResourcesLoaded,
    WaitingForPress,
    Dynamics,
    Snapped,
    Suspended,
    Deactivated,
};

enum class PressResultState
{
    LoadGame,
    PlayLevel,
    ContinueLevel,
};

enum class GameInfoOverlayState
{
    Loading,
    GameStats,
    GameOverExpired,
    GameOverCompleted,
    LevelStart,
    Pause,
};

ref class App : public Windows::ApplicationModel::Core::IFrameworkView
{
internal:
    App();

public:
    // IFrameworkView Methods
    virtual void Initialize(_In_ Windows::ApplicationModel::Core::CoreApplicationView^ applicationView);
    virtual void SetWindow(_In_ Windows::UI::Core::CoreWindow^ window);
    virtual void Load(_In_ Platform::String^ entryPoint);
    virtual void Run();
    virtual void Uninitialize();

private:
    void InitializeGameState();

    // Event Handlers
    void OnSuspending(
        _In_ Platform::Object^ sender,
        _In_ Windows::ApplicationModel::SuspendingEventArgs^ args
        );

    void OnResuming(
        _In_ Platform::Object^ sender,
        _In_ Platform::Object^ args
        );

    void UpdateViewState();

    void OnWindowActivationChanged(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::WindowActivatedEventArgs^ args
        );

    void OnWindowSizeChanged(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::WindowSizeChangedEventArgs^ args
        );

    void OnWindowClosed(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::CoreWindowEventArgs^ args
        );

    void OnLogicalDpiChanged(
        _In_ Platform::Object^ sender
        );

    void OnActivated(
        _In_ Windows::ApplicationModel::Core::CoreApplicationView^ applicationView,
        _In_ Windows::ApplicationModel::Activation::IActivatedEventArgs^ args
        );

    void OnVisibilityChanged(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::VisibilityChangedEventArgs^ args
        );

    void Update();
    void SetGameInfoOverlay(GameInfoOverlayState state);
    void SetAction (GameInfoOverlayCommand command);
    void ShowGameInfoOverlay();
    void HideGameInfoOverlay();
    void SetSnapped();
    void HideSnapped();

    bool                                                m_windowClosed;
    bool                                                m_renderNeeded;
    bool                                                m_haveFocus;
    bool                                                m_visible;

    MoveLookController^                                 m_controller;
    GameRenderer^                                       m_renderer;
    Simple3DGame^                                       m_game;

    UpdateEngineState                                   m_updateState;
    UpdateEngineState                                   m_updateStateNext;
    PressResultState                                    m_pressResult;
    GameInfoOverlayState                                m_gameInfoOverlayState;
    GameInfoOverlayCommand                              m_gameInfoOverlayCommand;
    uint32                                              m_loadingCount;
};

ref class Direct3DApplicationSource : Windows::ApplicationModel::Core::IFrameworkViewSource
{
public:
    virtual Windows::ApplicationModel::Core::IFrameworkView^ CreateView();
};
```

App.cpp

```cpp
//--------------------------------------------------------------------------------------
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "App.h"

using namespace concurrency;
using namespace DirectX;
using namespace Windows::ApplicationModel;
using namespace Windows::ApplicationModel::Activation;
using namespace Windows::ApplicationModel::Core;
using namespace Windows::Foundation;
using namespace Windows::Graphics::Display;
using namespace Windows::UI::Core;
using namespace Windows::UI::Input;
using namespace Windows::UI::ViewManagement;


App::App() :
    m_windowClosed(false),
    m_haveFocus(false),
    m_gameInfoOverlayCommand(GameInfoOverlayCommand::None),
    m_visible(true),
    m_loadingCount(0),
    m_updateState(UpdateEngineState::WaitingForResources)
{
}

//--------------------------------------------------------------------------------------

void App::Initialize(
    _In_ CoreApplicationView^ applicationView
    )
{
    applicationView->Activated +=
        ref new TypedEventHandler<CoreApplicationView^, IActivatedEventArgs^>(this, &App::OnActivated);

    CoreApplication::Suspending +=
        ref new EventHandler<SuspendingEventArgs^>(this, &App::OnSuspending);

    CoreApplication::Resuming +=
        ref new EventHandler<Platform::Object^>(this, &App::OnResuming);

    m_controller = ref new MoveLookController();
    m_renderer = ref new GameRenderer();
    m_game = ref new Simple3DGame();
}

//--------------------------------------------------------------------------------------

void App::SetWindow(
    _In_ CoreWindow^ window
    )
{
    window->PointerCursor = ref new CoreCursor(CoreCursorType::Arrow, 0);

    PointerVisualizationSettings^ visualizationSettings = PointerVisualizationSettings::GetForCurrentView();
    visualizationSettings->IsContactFeedbackEnabled = false;
    visualizationSettings->IsBarrelButtonFeedbackEnabled = false;

    window->SizeChanged +=
        ref new TypedEventHandler<CoreWindow^, WindowSizeChangedEventArgs^>(this, &App::OnWindowSizeChanged);

    window->Closed +=
        ref new TypedEventHandler<CoreWindow^, CoreWindowEventArgs^>(this, &App::OnWindowClosed);

    window->VisibilityChanged +=
        ref new TypedEventHandler<CoreWindow^, VisibilityChangedEventArgs^>(this, &App::OnVisibilityChanged);

    DisplayProperties::LogicalDpiChanged +=
        ref new DisplayPropertiesEventHandler(this, &App::OnLogicalDpiChanged);

    m_controller->Initialize(window);

    m_controller->SetMoveRect(
        XMFLOAT2(0.0f, window->Bounds.Height - GameConstants::TouchRectangleSize),
        XMFLOAT2(GameConstants::TouchRectangleSize, window->Bounds.Height)
        );
    m_controller->SetFireRect(
        XMFLOAT2(window->Bounds.Width - GameConstants::TouchRectangleSize, window->Bounds.Height - GameConstants::TouchRectangleSize),
        XMFLOAT2(window->Bounds.Width, window->Bounds.Height)
        );

    m_renderer->Initialize(window, DisplayProperties::LogicalDpi);
    SetGameInfoOverlay(GameInfoOverlayState::Loading);
    ShowGameInfoOverlay();
}

//--------------------------------------------------------------------------------------

void App::Load(
    _In_ Platform::String^ /* entryPoint */
    )
{
    create_task([this]()
    {
        // Asynchronously initialize the game class and load the renderer device resources.
        // By doing all this asynchronously, the game gets to its main loop more quickly
        // and loads all the necessary resources in parallel on other threads.
        m_game->Initialize(m_controller, m_renderer);

        return m_renderer->CreateGameDeviceResourcesAsync(m_game);

    }).then([this]()
    {
        // The finalize code needs to run in the same thread context
        // as the m_renderer object was created because the D3D device context
        // can ONLY be accessed on a single thread.
        m_renderer->FinalizeCreateGameDeviceResources();

        InitializeGameState();

        if (m_updateState == UpdateEngineState::WaitingForResources)
        {
            // In the middle of a game, so spin up the async task to load the level.
            create_task([this]()
            {
                return m_game->LoadLevelAsync();

            }).then([this]()
            {
                // The m_game object may need to deal with D3D device context work, so
                // again the finalize code needs to run in the same thread
                // context as the m_renderer object was created because the D3D 
                // device context can ONLY be accessed on a single thread.
                m_game->FinalizeLoadLevel();
                m_updateState = UpdateEngineState::ResourcesLoaded;

            }, task_continuation_context::use_current());
        }
    }, task_continuation_context::use_current());
}

//--------------------------------------------------------------------------------------

void App::Run()
{
    while (!m_windowClosed)
    {
        if (m_visible)
        {
            switch (m_updateState)
            {
            case UpdateEngineState::Deactivated:
            case UpdateEngineState::Snapped:
                if (!m_renderNeeded)
                {
                    // The App is not currently the active window, so just wait for events.
                    CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
                    break;
                }
                // Otherwise, fall through and do normal processing to get the rendering handled.
            default:
                CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);
                Update();
                m_renderer->Render();
                m_renderNeeded = false;
            }
        }
        else
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
    m_game->OnSuspending();  // Exiting due to window close.  Make sure to save state.
}

//--------------------------------------------------------------------------------------

void App::Uninitialize()
{
}

//--------------------------------------------------------------------------------------

void App::OnWindowSizeChanged(
    _In_ CoreWindow^ window,
    _In_ WindowSizeChangedEventArgs^ /* args */
    )
{
    UpdateViewState();
    m_renderer->UpdateForWindowSizeChange();

    // The location of the GameInfoOverlay may have changed with the size change, so update the controller.
    m_controller->SetMoveRect(
        XMFLOAT2(0.0f, window->Bounds.Height - GameConstants::TouchRectangleSize),
        XMFLOAT2(GameConstants::TouchRectangleSize, window->Bounds.Height)
        );
    m_controller->SetFireRect(
        XMFLOAT2(window->Bounds.Width - GameConstants::TouchRectangleSize, window->Bounds.Height - GameConstants::TouchRectangleSize),
        XMFLOAT2(window->Bounds.Width, window->Bounds.Height)
        );

    if (m_updateState == UpdateEngineState::WaitingForPress)
    {
        m_controller->WaitForPress(m_renderer->GameInfoOverlayUpperLeft(), m_renderer->GameInfoOverlayLowerRight());
    }
}

//--------------------------------------------------------------------------------------

void App::OnWindowClosed(
    _In_ CoreWindow^ /* sender */,
    _In_ CoreWindowEventArgs^ /* args */
    )
{
    m_windowClosed = true;
}

//--------------------------------------------------------------------------------------

void App::OnLogicalDpiChanged(
    _In_ Platform::Object^ /* sender */
    )
{
    m_renderer->SetDpi(DisplayProperties::LogicalDpi);

    // The GameInfoOverlay may have been recreated as a result of DPI changes, so
    // regenerate the data.
    SetGameInfoOverlay(m_gameInfoOverlayState);
    SetAction(m_gameInfoOverlayCommand);
}

//--------------------------------------------------------------------------------------

void App::OnActivated(
    _In_ CoreApplicationView^ /* applicationView */,
    _In_ IActivatedEventArgs^ /* args */
    )
{
    CoreWindow::GetForCurrentThread()->Activated +=
        ref new TypedEventHandler<CoreWindow^, WindowActivatedEventArgs^>(this, &App::OnWindowActivationChanged);
    CoreWindow::GetForCurrentThread()->Activate();
}

//--------------------------------------------------------------------------------------

void App::OnVisibilityChanged(
    _In_ CoreWindow^ /* sender */,
    _In_ VisibilityChangedEventArgs^ args
    )
{
    m_visible = args->Visible;
}

//--------------------------------------------------------------------------------------

void App::InitializeGameState()
{
    // Set up the initial state machine for handling the Game playing state.
    if (m_game->GameActive() && m_game->LevelActive())
    {
        // The last time the game terminated it was in the middle
        // of a level.
        // We are waiting for the user to continue the game.
        m_updateState = UpdateEngineState::WaitingForResources;
        m_pressResult = PressResultState::ContinueLevel;
        SetGameInfoOverlay(GameInfoOverlayState::Pause);
        SetAction(GameInfoOverlayCommand::PleaseWait);
    }
    else if (!m_game->GameActive() && (m_game->HighScore().totalHits > 0))
    {
        // The last time the game terminated the game had been completed.
        // Show the high score.
        // We are waiting for the user to acknowledge the high score and start a new game.
        // The level resources for the first level will be loaded later.
        m_updateState = UpdateEngineState::WaitingForPress;
        m_pressResult = PressResultState::LoadGame;
        SetGameInfoOverlay(GameInfoOverlayState::GameStats);
        m_controller->WaitForPress(m_renderer->GameInfoOverlayUpperLeft(), m_renderer->GameInfoOverlayLowerRight());
        SetAction(GameInfoOverlayCommand::TapToContinue);
    }
    else
    {
        // This is either the first time the game has run or
        // the last time the game terminated the level was completed.
        // We are waiting for the user to begin the next level.
        m_updateState = UpdateEngineState::WaitingForResources;
        m_pressResult = PressResultState::PlayLevel;
        SetGameInfoOverlay(GameInfoOverlayState::LevelStart);
        SetAction(GameInfoOverlayCommand::PleaseWait);
    }
    ShowGameInfoOverlay();
}

//--------------------------------------------------------------------------------------

void App::Update()
{
    static uint32 loadCount = 0;

    m_controller->Update();

    switch (m_updateState)
    {
    case UpdateEngineState::WaitingForResources:
        // Waiting for the initial load.  Display an update once per 60 updates.
        loadCount++;
        if ((loadCount % 60) == 0)
        {
            m_loadingCount++;
            SetGameInfoOverlay(m_gameInfoOverlayState);
        }
        break;

    case UpdateEngineState::ResourcesLoaded:
        switch (m_pressResult)
        {
        case PressResultState::LoadGame:
            SetGameInfoOverlay(GameInfoOverlayState::GameStats);
            break;

        case PressResultState::PlayLevel:
            SetGameInfoOverlay(GameInfoOverlayState::LevelStart);
            break;

        case PressResultState::ContinueLevel:
            SetGameInfoOverlay(GameInfoOverlayState::Pause);
            break;
        }
        m_updateState = UpdateEngineState::WaitingForPress;
        SetAction(GameInfoOverlayCommand::TapToContinue);
        m_controller->WaitForPress(m_renderer->GameInfoOverlayUpperLeft(), m_renderer->GameInfoOverlayLowerRight());
        ShowGameInfoOverlay();
        m_renderNeeded = true;
        break;

    case UpdateEngineState::WaitingForPress:
        if (m_controller->IsPressComplete())
        {
            switch (m_pressResult)
            {
            case PressResultState::LoadGame:
                m_updateState = UpdateEngineState::WaitingForResources;
                m_pressResult = PressResultState::PlayLevel;
                m_controller->Active(false);
                m_game->LoadGame();
                SetAction(GameInfoOverlayCommand::PleaseWait);
                SetGameInfoOverlay(GameInfoOverlayState::LevelStart);
                ShowGameInfoOverlay();

                m_game->LoadLevelAsync().then([this]()
                {
                    m_game->FinalizeLoadLevel();
                    m_updateState = UpdateEngineState::ResourcesLoaded;

                }, task_continuation_context::use_current());
                break;

            case PressResultState::PlayLevel:
                m_updateState = UpdateEngineState::Dynamics;
                HideGameInfoOverlay();
                m_controller->Active(true);
                m_game->StartLevel();
                break;

            case PressResultState::ContinueLevel:
                m_updateState = UpdateEngineState::Dynamics;
                HideGameInfoOverlay();
                m_controller->Active(true);
                m_game->ContinueGame();
                break;
            }
        }
        break;

    case UpdateEngineState::Dynamics:
        if (m_controller->IsPauseRequested())
        {
            m_game->PauseGame();
            SetGameInfoOverlay(GameInfoOverlayState::Pause);
            SetAction(GameInfoOverlayCommand::TapToContinue);
            m_updateState = UpdateEngineState::WaitingForPress;
            m_pressResult = PressResultState::ContinueLevel;
            ShowGameInfoOverlay();
        }
        else
        {
            GameState runState = m_game->RunGame();
            switch (runState)
            {
            case GameState::TimeExpired:
                SetAction(GameInfoOverlayCommand::TapToContinue);
                SetGameInfoOverlay(GameInfoOverlayState::GameOverExpired);
                ShowGameInfoOverlay();
                m_updateState = UpdateEngineState::WaitingForPress;
                m_pressResult = PressResultState::LoadGame;
                break;

            case GameState::LevelComplete:
                SetAction(GameInfoOverlayCommand::PleaseWait);
                SetGameInfoOverlay(GameInfoOverlayState::LevelStart);
                ShowGameInfoOverlay();
                m_updateState = UpdateEngineState::WaitingForResources;
                m_pressResult = PressResultState::PlayLevel;

                m_game->LoadLevelAsync().then([this]()
                {
                    m_game->FinalizeLoadLevel();
                    m_updateState = UpdateEngineState::ResourcesLoaded;

                }, task_continuation_context::use_current());
                break;

            case GameState::GameComplete:
                SetAction(GameInfoOverlayCommand::TapToContinue);
                SetGameInfoOverlay(GameInfoOverlayState::GameOverCompleted);
                ShowGameInfoOverlay();
                m_updateState  = UpdateEngineState::WaitingForPress;
                m_pressResult = PressResultState::LoadGame;
                break;
            }
        }

        if (m_updateState == UpdateEngineState::WaitingForPress)
        {
            // Transitioning state, so enable waiting for the press event.
            m_controller->WaitForPress(m_renderer->GameInfoOverlayUpperLeft(), m_renderer->GameInfoOverlayLowerRight());
        }
        if (m_updateState == UpdateEngineState::WaitingForResources)
        {
            // Transitioning state, so shut down the input controller until resources are loaded.
            m_controller->Active(false);
        }
        break;
    }
}

//--------------------------------------------------------------------------------------

void App::OnWindowActivationChanged(
    _In_ Windows::UI::Core::CoreWindow^ /* sender */,
    _In_ Windows::UI::Core::WindowActivatedEventArgs^ args
    )
{
    if (args->WindowActivationState == CoreWindowActivationState::Deactivated)
    {
        m_haveFocus = false;

        switch (m_updateState)
        {
        case UpdateEngineState::Dynamics:
            // From Dynamic mode, when coming out of Deactivated rather than going directly back into game play
            // go to the paused state waiting for user input to continue.
            m_updateStateNext = UpdateEngineState::WaitingForPress;
            m_pressResult = PressResultState::ContinueLevel;
            SetGameInfoOverlay(GameInfoOverlayState::Pause);
            ShowGameInfoOverlay();
            m_game->PauseGame();
            m_updateState = UpdateEngineState::Deactivated;
            SetAction(GameInfoOverlayCommand::None);
            m_renderNeeded = true;
            break;

        case UpdateEngineState::WaitingForResources:
        case UpdateEngineState::WaitingForPress:
            m_updateStateNext = m_updateState;
            m_updateState = UpdateEngineState::Deactivated;
            SetAction(GameInfoOverlayCommand::None);
            ShowGameInfoOverlay();
            m_renderNeeded = true;
            break;
        }
        // Don't have focus, so shutdown input processing.
        m_controller->Active(false);
    }
    else if (args->WindowActivationState == CoreWindowActivationState::CodeActivated
        || args->WindowActivationState == CoreWindowActivationState::PointerActivated)
    {
        m_haveFocus = true;

        if (m_updateState == UpdateEngineState::Deactivated)
        {
            m_updateState = m_updateStateNext;

            if (m_updateState == UpdateEngineState::WaitingForPress)
            {
                SetAction(GameInfoOverlayCommand::TapToContinue);
                m_controller->WaitForPress(m_renderer->GameInfoOverlayUpperLeft(), m_renderer->GameInfoOverlayLowerRight());
            }
            else if (m_updateStateNext == UpdateEngineState::WaitingForResources)
            {
                SetAction(GameInfoOverlayCommand::PleaseWait);
            }
        }
    }
}

//--------------------------------------------------------------------------------------

void App::OnSuspending(
    _In_ Platform::Object^ /* sender */,
    _In_ SuspendingEventArgs^ args
    )
{
    // Save application state.
    // If your application needs time to complete a lengthy operation, it can request a deferral.
    // The SuspendingOperation has a deadline time. Make sure all your operations are complete by that time!
    // If the app doesn't return from this handler within five seconds, it will be terminated.
    SuspendingOperation^ op = args->SuspendingOperation;
    SuspendingDeferral^ deferral = op->GetDeferral();

    create_task([=]()
    {
        switch (m_updateState)
        {
        case UpdateEngineState::Dynamics:
            // Game is in the active game play state, Stop Game Timer and Pause play and save the state.
            SetAction(GameInfoOverlayCommand::None);
            SetGameInfoOverlay(GameInfoOverlayState::Pause);
            m_updateStateNext = UpdateEngineState::WaitingForPress;
            m_pressResult = PressResultState::ContinueLevel;
            m_game->PauseGame();
            break;

        case UpdateEngineState::WaitingForResources:
        case UpdateEngineState::WaitingForPress:
            m_updateStateNext = m_updateState;
            break;

        default:
            // If it is any other state, don't save as the next state as they are transient states and have already set m_updateStateNext
            break;
        }
        m_updateState = UpdateEngineState::Suspended;

        m_controller->Active(false);
        m_game->OnSuspending();

        deferral->Complete();
    });
}

//--------------------------------------------------------------------------------------

void App::OnResuming(
    _In_ Platform::Object^ /* sender */,
    _In_ Platform::Object^ /* args */
    )
{
    if (m_haveFocus)
    {
        m_updateState = m_updateStateNext;
    }
    else
    {
        m_updateState = UpdateEngineState::Deactivated;
    }

    if (m_updateState == UpdateEngineState::WaitingForPress)
    {
        SetAction(GameInfoOverlayCommand::TapToContinue);
        m_controller->WaitForPress(m_renderer->GameInfoOverlayUpperLeft(), m_renderer->GameInfoOverlayLowerRight());
    }
    m_game->OnResuming();
    ShowGameInfoOverlay();
    m_renderNeeded = true;
}

//--------------------------------------------------------------------------------------

void App::UpdateViewState()
{
    m_renderNeeded = true;

    if (ApplicationView::Value == ApplicationViewState::Snapped)
    {
        switch (m_updateState)
        {
        case UpdateEngineState::Dynamics:
            // From Dynamic mode, when coming out of SNAPPED layout rather than going directly back into game play,
            // go to the paused state and wait for user input to continue.
            m_updateStateNext = UpdateEngineState::WaitingForPress;
            m_pressResult = PressResultState::ContinueLevel;
            SetGameInfoOverlay(GameInfoOverlayState::Pause);
            SetAction(GameInfoOverlayCommand::TapToContinue);
            m_game->PauseGame();
            break;

        case UpdateEngineState::WaitingForResources:
        case UpdateEngineState::WaitingForPress:
            // Avoid corrupting the m_updateStateNext on a transition from Snapped -> Snapped.
            // Otherwise, just cache the current state and return to it when leaving SNAPPED layout.

            m_updateStateNext = m_updateState;
            break;

        default:
            break;
        }

        m_updateState = UpdateEngineState::Snapped;
        m_controller->Active(false);
        HideGameInfoOverlay();
        SetSnapped();
    }
    else if (ApplicationView::Value == ApplicationViewState::Filled ||
        ApplicationView::Value == ApplicationViewState::FullScreenLandscape ||
        ApplicationView::Value == ApplicationViewState::FullScreenPortrait)
    {
        if (m_updateState == UpdateEngineState::Snapped)
        {

            HideSnapped();
            ShowGameInfoOverlay();
            m_renderNeeded = true;

            if (m_haveFocus)
            {
                if (m_updateStateNext == UpdateEngineState::WaitingForPress)
                {
                    SetAction(GameInfoOverlayCommand::TapToContinue);
                    m_controller->WaitForPress(m_renderer->GameInfoOverlayUpperLeft(), m_renderer->GameInfoOverlayLowerRight());
                }
                else if (m_updateStateNext == UpdateEngineState::WaitingForResources)
                {
                    SetAction(GameInfoOverlayCommand::PleaseWait);
                }

                m_updateState = m_updateStateNext;
            }
            else
            {
                m_updateState = UpdateEngineState::Deactivated;
                SetAction(GameInfoOverlayCommand::None);
            }
        }
    }
}

//--------------------------------------------------------------------------------------

void App::SetGameInfoOverlay(GameInfoOverlayState state)
{
    m_gameInfoOverlayState = state;
    switch (state)
    {
    case GameInfoOverlayState::Loading:
        m_renderer->InfoOverlay()->SetGameLoading(m_loadingCount);
        break;

    case GameInfoOverlayState::GameStats:
        m_renderer->InfoOverlay()->SetGameStats(
            m_game->HighScore().levelCompleted + 1,
            m_game->HighScore().totalHits,
            m_game->HighScore().totalShots
            );
        break;

    case GameInfoOverlayState::LevelStart:
        m_renderer->InfoOverlay()->SetLevelStart(
            m_game->LevelCompleted() + 1,
            m_game->CurrentLevel()->Objective(),
            m_game->CurrentLevel()->TimeLimit(),
            m_game->BonusTime()
            );
        break;

    case GameInfoOverlayState::GameOverCompleted:
        m_renderer->InfoOverlay()->SetGameOver(
            true,
            m_game->LevelCompleted() + 1,
            m_game->TotalHits(),
            m_game->TotalShots(),
            m_game->HighScore().totalHits
            );
        break;

    case GameInfoOverlayState::GameOverExpired:
        m_renderer->InfoOverlay()->SetGameOver(
            false,
            m_game->LevelCompleted(),
            m_game->TotalHits(),
            m_game->TotalShots(),
            m_game->HighScore().totalHits
            );
        break;

    case GameInfoOverlayState::Pause:
        m_renderer->InfoOverlay()->SetPause();
        break;
    }
}

//--------------------------------------------------------------------------------------

void App::SetAction (GameInfoOverlayCommand command)
{
    m_gameInfoOverlayCommand = command;
    m_renderer->InfoOverlay()->SetAction(command);
}

//--------------------------------------------------------------------------------------

void App::ShowGameInfoOverlay()
{
    m_renderer->InfoOverlay()->ShowGameInfoOverlay();
}

//--------------------------------------------------------------------------------------

void App::HideGameInfoOverlay()
{
    m_renderer->InfoOverlay()->HideGameInfoOverlay();
}

//--------------------------------------------------------------------------------------

void App::SetSnapped()
{
    m_renderer->InfoOverlay()->SetPause();
    m_renderer->InfoOverlay()->ShowGameInfoOverlay();
}

//--------------------------------------------------------------------------------------

void App::HideSnapped()
{
    m_renderer->InfoOverlay()->HideGameInfoOverlay();
    SetGameInfoOverlay(m_gameInfoOverlayState);
}

//--------------------------------------------------------------------------------------

IFrameworkView^ DirectXAppSource::CreateView()
{
    return ref new App();
}

//--------------------------------------------------------------------------------------

[Platform::MTAThread]
int main(Platform::Array<Platform::String^>^)
{
    auto direct3DApplicationSource = ref new Direct3DApplicationSource();
    CoreApplication::Run(direct3DApplicationSource);
    return 0;
}

//--------------------------------------------------------------------------------------
```

 

 






<!--HONumber=Mar16_HO1-->


