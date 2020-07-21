---
title: 定义游戏的 UWP 应用框架
description: 编写通用 Windows 平台（UWP）游戏的第一步是生成框架，该框架使应用对象能够与 Windows 交互。
ms.assetid: 7beac1eb-ba3d-e15c-44a1-da2f5a79bb3b
ms.date: 06/24/2020
ms.topic: article
keywords: windows 10, uwp, 游戏, directx
ms.localizationpriority: medium
ms.openlocfilehash: 5052b4e71196950b6a8a91aaa271b5550fb448b5
ms.sourcegitcommit: 20969781aca50738792631f4b68326f9171a3980
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/26/2020
ms.locfileid: "85409616"
---
#  <a name="define-the-games-uwp-app-framework"></a>定义游戏的 UWP 应用框架

> [!NOTE]
> 本主题是[使用 DirectX 教程系列创建简单通用 Windows 平台（UWP）游戏](tutorial--create-your-first-uwp-directx-game.md)的一部分。 该链接上的主题设置了序列的上下文。

编写通用 Windows 平台（UWP）游戏的第一步是生成框架，该框架使应用对象能够与窗口进行交互，包括挂起-resume 事件处理、窗口焦点变化和对齐等 Windows 运行时功能。

## <a name="objectives"></a>目标

* 为通用 Windows 平台（UWP） DirectX 游戏设置框架，并实现定义整个游戏流的状态机。

> [!NOTE]
> 若要遵循本主题，请查看下载的[Simple3DGameDX](/samples/microsoft/windows-universal-samples/simple3dgamedx/)示例游戏的源代码。

## <a name="introduction"></a>简介

在[设置游戏项目](tutorial--setting-up-the-games-infrastructure.md)主题中，我们引入了**WWinMain**函数以及[**IFrameworkViewSource**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource)和[**IFrameworkView**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource)接口。 我们了解到**App**类（可以在 `App.cpp` **Simple3DGameDX**项目中的源代码文件中定义）同时用作*视图提供程序工厂*和*视图提供程序*。

本主题从这里开始，并详细了解游戏中的**应用**类应如何实现**IFrameworkView**方法。

## <a name="the-appinitialize-method"></a>App：： Initialize 方法

在应用程序启动时，Windows 调用的第一种方法是实现[**IFrameworkView：： Initialize**](/uwp/api/windows.applicationmodel.core.iframeworkview.initialize)。

你的实现应处理 UWP 游戏的最基本的行为，例如，确保游戏可以通过订阅这些事件来处理挂起（和可能的更高版本）事件。 我们还可以在此处访问显示适配器设备，因此，我们可以创建依赖于该设备的图形资源。

```cppwinrt
void Initialize(CoreApplicationView const& applicationView)
{
    applicationView.Activated({ this, &App::OnActivated });

    CoreApplication::Suspending({ this, &App::OnSuspending });

    CoreApplication::Resuming({ this, &App::OnResuming });

    // At this point we have access to the device. 
    // We can create the device-dependent resources.
    m_deviceResources = std::make_shared<DX::DeviceResources>();
}
```

尽可能避免原始指针（几乎总是可能的）。

- 对于 Windows 运行时类型，几乎可以完全避免指针，而只是在堆栈上构造值。 如果确实需要一个指针，请使用[**winrt：： com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr) （我们很快就会看到一个示例）。
- 对于唯一指针，请使用[**std：： unique_ptr**](/cpp/cpp/how-to-create-and-use-unique-ptr-instances)和**std：： make_unique**。
- 对于共享指针，请使用[**std：： shared_ptr**](/cpp/cpp/how-to-create-and-use-shared-ptr-instances)和**std：： make_shared**。

## <a name="the-appsetwindow-method"></a>App：： SetWindow 方法

**初始化**之后，Windows 将调用[**IFrameworkView：： SetWindow**](/uwp/api/windows.applicationmodel.core.iframeworkview.setwindow)的实现，并传递一个[**CoreWindow**](/uwp/api/windows.ui.core.corewindow)对象，该对象表示游戏的主窗口。

在**App：： SetWindow**中，我们订阅与窗口相关的事件，并配置某些窗口和显示行为。 例如，我们构造了鼠标指针（通过[**CoreCursor**](/uwp/api/windows.ui.core.corecursor)类），鼠标和触控控件都可使用该指针。 我们还将窗口对象传递到与设备相关的资源对象。

我们将详细讨论如何处理[游戏流管理](tutorial-game-flow-management.md#event-handling)主题中的事件。

```cppwinrt
void SetWindow(CoreWindow const& window)
{
    //CoreWindow window = CoreWindow::GetForCurrentThread();
    window.Activate();

    window.PointerCursor(CoreCursor(CoreCursorType::Arrow, 0));

    PointerVisualizationSettings visualizationSettings{ PointerVisualizationSettings::GetForCurrentView() };
    visualizationSettings.IsContactFeedbackEnabled(false);
    visualizationSettings.IsBarrelButtonFeedbackEnabled(false);

    m_deviceResources->SetWindow(window);

    window.Activated({ this, &App::OnWindowActivationChanged });

    window.SizeChanged({ this, &App::OnWindowSizeChanged });

    window.Closed({ this, &App::OnWindowClosed });

    window.VisibilityChanged({ this, &App::OnVisibilityChanged });

    DisplayInformation currentDisplayInformation{ DisplayInformation::GetForCurrentView() };

    currentDisplayInformation.DpiChanged({ this, &App::OnDpiChanged });

    currentDisplayInformation.OrientationChanged({ this, &App::OnOrientationChanged });

    currentDisplayInformation.StereoEnabledChanged({ this, &App::OnStereoEnabledChanged });

    DisplayInformation::DisplayContentsInvalidated({ this, &App::OnDisplayContentsInvalidated });
}
```

## <a name="the-appload-method"></a>App：： Load 方法

设置主窗口后，将调用[**IFrameworkView：： Load**](/uwp/api/windows.applicationmodel.core.iframeworkview.load)的实现。 **负载**是预提取游戏数据或资产的更好位置，而不是**初始化**和**SetWindow**。

```cppwinrt
void Load(winrt::hstring const& /* entryPoint */)
{
    if (!m_main)
    {
        m_main = winrt::make_self<GameMain>(m_deviceResources);
    }
}
```

正如您所看到的，实际工作会被委托给我们在此处进行的**GameMain**对象的构造函数。 **GameMain**类在和中定义 `GameMain.h` `GameMain.cpp` 。

### <a name="the-gamemaingamemain-constructor"></a>GameMain：： GameMain 构造函数

**GameMain**构造函数（以及它所调用的其他成员函数）将开始一组异步加载操作，以创建游戏对象、加载图形资源并初始化游戏的状态机。 在游戏开始之前，我们还执行任何必要的准备工作，如设置任何起始状态或全局值。

Windows 在开始处理输入之前会对游戏的时间施加限制。 因此使用异步，正如我们在这里所做的那样，这意味着**负载**可以快速返回，而它已经开始在后台继续进行。 如果加载时间较长，或者有大量资源，则为用户提供经常更新的进度条是一个好主意。 

如果你不熟悉异步编程，请参阅[c + +/WinRT 的并发和异步操作](/windows/uwp/cpp-and-winrt-apis/concurrency)。

```cppwinrt
GameMain::GameMain(std::shared_ptr<DX::DeviceResources> const& deviceResources) :
    m_deviceResources(deviceResources),
    m_windowClosed(false),
    m_haveFocus(false),
    m_gameInfoOverlayCommand(GameInfoOverlayCommand::None),
    m_visible(true),
    m_loadingCount(0),
    m_updateState(UpdateEngineState::WaitingForResources)
{
    m_deviceResources->RegisterDeviceNotify(this);

    m_renderer = std::make_shared<GameRenderer>(m_deviceResources);
    m_game = std::make_shared<Simple3DGame>();

    m_uiControl = m_renderer->GameUIControl();

    m_controller = std::make_shared<MoveLookController>(CoreWindow::GetForCurrentThread());

    auto bounds = m_deviceResources->GetLogicalSize();

    m_controller->SetMoveRect(
        XMFLOAT2(0.0f, bounds.Height - GameUIConstants::TouchRectangleSize),
        XMFLOAT2(GameUIConstants::TouchRectangleSize, bounds.Height)
        );
    m_controller->SetFireRect(
        XMFLOAT2(bounds.Width - GameUIConstants::TouchRectangleSize, bounds.Height - GameUIConstants::TouchRectangleSize),
        XMFLOAT2(bounds.Width, bounds.Height)
        );

    SetGameInfoOverlay(GameInfoOverlayState::Loading);
    m_uiControl->SetAction(GameInfoOverlayCommand::None);
    m_uiControl->ShowGameInfoOverlay();

    // Asynchronously initialize the game class and load the renderer device resources.
    // By doing all this asynchronously, the game gets to its main loop more quickly
    // and in parallel all the necessary resources are loaded on other threads.
    ConstructInBackground();
}

winrt::fire_and_forget GameMain::ConstructInBackground()
{
    auto lifetime = get_strong();

    m_game->Initialize(m_controller, m_renderer);

    co_await m_renderer->CreateGameDeviceResourcesAsync(m_game);

    // The finalize code needs to run in the same thread context
    // as the m_renderer object was created because the D3D device context
    // can ONLY be accessed on a single thread.
    // co_await of an IAsyncAction resumes in the same thread context.
    m_renderer->FinalizeCreateGameDeviceResources();

    InitializeGameState();

    if (m_updateState == UpdateEngineState::WaitingForResources)
    {
        // In the middle of a game so spin up the async task to load the level.
        co_await m_game->LoadLevelAsync();

        // The m_game object may need to deal with D3D device context work so
        // again the finalize code needs to run in the same thread
        // context as the m_renderer object was created because the D3D
        // device context can ONLY be accessed on a single thread.
        m_game->FinalizeLoadLevel();
        m_game->SetCurrentLevelToSavedState();
        m_updateState = UpdateEngineState::ResourcesLoaded;
    }
    else
    {
        // The game is not in the middle of a level so there aren't any level
        // resources to load.
    }

    // Since Game loading is an async task, the app visual state
    // may be too small or not have focus. Put the state machine
    // into the correct state to reflect these cases.

    if (m_deviceResources->GetLogicalSize().Width < GameUIConstants::MinPlayableWidth)
    {
        m_updateStateNext = m_updateState;
        m_updateState = UpdateEngineState::TooSmall;
        m_controller->Active(false);
        m_uiControl->HideGameInfoOverlay();
        m_uiControl->ShowTooSmall();
        m_renderNeeded = true;
    }
    else if (!m_haveFocus)
    {
        m_updateStateNext = m_updateState;
        m_updateState = UpdateEngineState::Deactivated;
        m_controller->Active(false);
        m_uiControl->SetAction(GameInfoOverlayCommand::None);
        m_renderNeeded = true;
    }
}

void GameMain::InitializeGameState()
{
    // Set up the initial state machine for handling Game playing state.
    ...
}
```

下面概述了构造函数启动的工作序列。

- 创建和初始化**GameRenderer**类型的对象。 有关详细信息，请参阅[呈现框架 I：呈现简介](tutorial--assembling-the-rendering-pipeline.md)。
- 创建和初始化**Simple3DGame**类型的对象。 有关详细信息，请参阅[定义主游戏对象](tutorial--defining-the-main-game-loop.md)。
- 创建游戏 UI 控件对象，并显示游戏信息覆盖，以便在资源文件加载时显示进度栏。 有关详细信息，请参阅[添加用户界面](tutorial--adding-a-user-interface.md)。
- 创建控制器对象，以从控制器（触摸、鼠标或 Xbox 无线控制器）读取输入。 有关详细信息，请参阅[添加控件](tutorial--adding-controls.md)。
- 分别在屏幕的左下角和右下角定义两个矩形区域，分别用于移动和照相机触摸控件。 播放机使用左下角的矩形（在对**SetMoveRect**的调用中定义）作为虚拟控制板，以便向前和向后移动照相机，并向后端移动。 右下方矩形（由**SetFireRect**方法定义）用作触发 ammo 的虚拟按钮。
- 使用协同程序将资源加载分解为单独的阶段。 访问 Direct3D 设备上下文仅限于创建设备上下文的线程;尽管对用于对象创建的 Direct3D 设备的访问是自由线程的。 因此， **GameRenderer：： CreateGameDeviceResourcesAsync**协同程序可以在独立于完成任务（**GameRenderer：： FinalizeCreateGameDeviceResources**）的线程上运行，该任务在原始线程上运行。
- 使用**Simple3DGame：： LoadLevelAsync**和**Simple3DGame：： FinalizeLoadLevel**的负载级别资源时，使用类似的模式。

下一主题（[游戏流管理](tutorial-game-flow-management.md)）将详细介绍**GameMain：： InitializeGameState** 。

## <a name="the-apponactivated-method"></a>App：： OnActivated 方法

接下来，引发[**CoreApplicationView：：已激活**](/uwp/api/windows.applicationmodel.core.coreapplicationview.activated)事件。 因此，将调用你拥有的任何**OnActivated**事件处理程序（例如，**应用：： OnActivated**方法）。

```cppwinrt
void OnActivated(CoreApplicationView const& /* applicationView */, IActivatedEventArgs const& /* args */)
{
    CoreWindow window = CoreWindow::GetForCurrentThread();
    window.Activate();
}
```

此处的唯一工作就是激活主要[**CoreWindow**](/uwp/api/windows.ui.core.corewindow)。 或者，你可以选择在**App：： SetWindow**中执行此操作。

## <a name="the-apprun-method"></a>App：： Run 方法

**Initialize**、 **SetWindow**和**Load**已设置该阶段。 既然游戏已启动并正在运行，就会调用[**IFrameworkView：： Run**](/uwp/api/windows.applicationmodel.core.iframeworkview.run)的实现。

```cppwinrt
void Run()
{
    m_main->Run();
}
```

同样，将工作委托给**GameMain**。

### <a name="the-gamemainrun-method"></a>GameMain：： Run 方法

**GameMain：： Run**是游戏的主要循环;可以在中找到该文件 `GameMain.cpp` 。 基本逻辑是：当游戏的窗口保持打开状态，调度所有事件，更新计时器，然后呈现并显示图形管道的结果。 此外，还会调度并处理用于在游戏状态间转换的事件。

此处的代码还涉及游戏引擎状态机中的两个状态。

- **UpdateEngineState：:D eactivated**。 这会指定禁用游戏窗口（失去焦点）或将其对齐。 
- **UpdateEngineState：： TooSmall**。 这将指定客户端区域太小，无法在中呈现游戏。

在这两种状态中，游戏会挂起事件处理，并等待窗口聚焦、unsnap 或调整大小。

在你的游戏获得焦点时，必须处理消息队列中到达的每个事件，因此必须使用 **ProcessAllIfPresent** 选项调用 [**CoreWindowDispatch.ProcessEvents**](/uwp/api/windows.ui.core.coredispatcher.processevents)。 其他选项可能会导致在处理消息事件时出现延迟，这会使您的游戏感觉不到响应，或者导致触摸行为缓慢。

如果游戏未显示、挂起和捕捉，则不希望它使用任何资源循环来调度永远不会到达的消息。 在这种情况下，游戏必须使用**ProcessOneAndAllPending**选项。 该选项将一直阻止到获取事件，然后处理该事件（以及在处理第一个过程中到达进程队列的任何其他事件）。 CoreWindowDispatch 在队列处理后立即返回**ProcessEvents。**

```cppwinrt
void GameMain::Run()
{
    while (!m_windowClosed)
    {
        if (m_visible)
        {
            switch (m_updateState)
            {
            case UpdateEngineState::Deactivated:
            case UpdateEngineState::TooSmall:
                if (m_updateStateNext == UpdateEngineState::WaitingForResources)
                {
                    WaitingForResourceLoading();
                    m_renderNeeded = true;
                }
                else if (m_updateStateNext == UpdateEngineState::ResourcesLoaded)
                {
                    // In the device lost case, we transition to the final waiting state
                    // and make sure the display is updated.
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
                    m_updateStateNext = UpdateEngineState::WaitingForPress;
                    m_uiControl->ShowGameInfoOverlay();
                    m_renderNeeded = true;
                }

                if (!m_renderNeeded)
                {
                    // The App is not currently the active window and not in a transient state so just wait for events.
                    CoreWindow::GetForCurrentThread().Dispatcher().ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
                    break;
                }
                // otherwise fall through and do normal processing to get the rendering handled.
            default:
                CoreWindow::GetForCurrentThread().Dispatcher().ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);
                Update();
                m_renderer->Render();
                m_deviceResources->Present();
                m_renderNeeded = false;
            }
        }
        else
        {
            CoreWindow::GetForCurrentThread().Dispatcher().ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
    m_game->OnSuspending();  // Exiting due to window close, so save state.
}
```

## <a name="the-appuninitialize-method"></a>App：：取消初始化方法

游戏结束时，将调用[**IFrameworkView：：**](/uwp/api/windows.applicationmodel.core.iframeworkview.uninitialize) 这是我们执行清理的机会。 关闭应用程序窗口不会终止应用程序的进程;但会将应用程序的状态编写为内存。 如果在系统回收此内存（包括对资源进行任何特殊清理）时必须进行特殊的操作，请将该清理的代码放入 "取消**初始化**"。

在我们的示例中，**应用：：取消初始化**是一个无操作。

```cpp
void Uninitialize()
{
}
```

## <a name="tips"></a>提示

开发您自己的游戏时，请根据本主题中所述的方法来设计您的启动代码。 下面是每种方法的基本建议的简单列表。

- 使用**Initialize**分配主类，并连接基本的事件处理程序。
- 使用**SetWindow**订阅任何特定于窗口的事件，并将主窗口传递到设备相关的资源对象，以便在创建交换链时可以使用该窗口。
- 使用**Load**处理任何其他设置，并启动异步创建对象和加载资源。 如果需要创建任何临时文件或数据（如过程生成的资产），也可以在此处执行此操作。

## <a name="next-steps"></a>后续步骤

本主题介绍了使用 DirectX 的 UWP 游戏的一些基本结构。 请记住这些方法，因为我们将在后面的主题中回顾其中一些方法。

在下一主题 " &mdash; [游戏流管理](tutorial-game-flow-management.md)" 中， &mdash; 我们将深入了解如何管理游戏状态和事件处理，以使游戏流动。
