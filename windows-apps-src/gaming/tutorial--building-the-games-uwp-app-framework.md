---
author: joannaleecy
title: 定义游戏的 UWP 应用框架
description: 为通用 Windows 平台 (UWP) DirectX 游戏进行编码的第一部分是生成使游戏对象与 Windows 交互的框架。
ms.assetid: 7beac1eb-ba3d-e15c-44a1-da2f5a79bb3b
ms.author: joanlee
ms.date: 10/24/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 游戏, directx
ms.localizationpriority: medium
ms.openlocfilehash: 406960820edaf3e8b14e93a6d9dfe9d723a216d6
ms.sourcegitcommit: 842ddba19fa3c028ea43e7922011515dbeb34e9c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/05/2018
ms.locfileid: "1488948"
---
#  <a name="define-the-uwp-app-framework"></a>定义 UWP 应用框架

构建框架以让游戏对象与 Windows 交互，包括 Windows 运行时属性（如暂停-恢复事件处理、窗口焦点的改变和贴靠）。

若要设置此框架，首先获取视图提供程序，以使应用单一实例（即定义运行应用的实例的 Windows 运行时对象）能够访问其所需的图形资源。 通过 Windows 运行时，你的游戏还可以直接与图形界面连接，这让你可以指定所需的资源和处理资源的方式。

视图提供程序对象实现 __IFrameworkView__ 界面，其包括一系列创建此游戏示例需要配置的方法。

你需要实现应用单一实例调用的以下五个方法：
* [__Initialize__](#initialize-the-view-provider)
* [__SetWindow__](#configure-the-window-and-display-behavior)
* [__Load__](#load-method-of-the-view-provider)
* [__Run__](#run-method-of-the-view-provider)
* [__Uninitialize__](#uninitialize-method-of-the-view-provider)

__Initialize__ 方法在应用程序启动时调用。 __SetWindow__ 方法在 __Initialize__ 之后调用。 然后调用 __Load__ 方法。 __Run__ 方法在游戏运行时调用。 当游戏结束时，调用 __Uninitialize__ 方法。 有关详细信息，请参阅 [__IFrameworkView__ API 参考](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview)。 

>[!Note]
>如果你尚未下载适用于此示例的最新游戏代码，请转到 [Direct3D 游戏示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX)。 此示例是大型 UWP 功能示例集合的一部分。 有关如何下载示例的说明，请参阅[从 GitHub 获取 UWP 示例](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples)。

## <a name="objective"></a>目标

为通用 Windows 平台 (UWP) DirectX 游戏设置框架，以及实现定义整个游戏流的状态机。

## <a name="define-the-view-provider-factory-and-view-provider-object"></a>定义视图提供程序工厂和视图提供程序对象

我们来检查一下 __App.cpp__ 中的__主__循环。 

在此步骤中，我们将为视图创建工厂（实现 __IFrameworkViewSource__），这反过来创建定义视图的视图提供程序对象的实例（实现 __IFrameworkView__）。

### <a name="main-method"></a>主要方法

如果你已加载 GitHub 示例代码，则创建新的 __DirectXApplicationSource__。 （如果你使用原始 DirectX 模板，则使用 __Direct3DApplicationSource__）这是实现 __IFrameworkViewSource__ 的视图提供程序工厂。 视图提供程序工厂的 __IFrameworkViewSource__ 界面定义了一个方法 __CreateView__。

在 __CoreApplication::Run__ 中，__CreateView__ 方法在传递 __Direct3DApplicationSource__ 或 __DirectXApplicationSource__ 时由应用单一实例调用。

__CreateView__ 返回对实现 __IFrameworkView__ 的应用对象的新实例的引用，即此示例中的 __App__ 类对象。 __App__ 类对象是视图提供程序对象。

```cpp
// The main function is only used to initialize our IFrameworkView class.
[Platform::MTAThread]
int main(Platform::Array<Platform::String^>^)
{
    auto directXApplicationSource = ref new DirectXApplicationSource();
    CoreApplication::Run(directXApplicationSource);
    return 0;
}

//--------------------------------------------------------------------------------------

IFrameworkView^ DirectXApplicationSource::CreateView()
{
    return ref new App();
}
```

## <a name="initialize-the-view-provider"></a>初始化视图提供程序

创建视图提供程序对象后，应用单一实例在应用程序启动时调用 [**Initialize**](https://msdn.microsoft.com/library/windows/apps/hh700495) 方法。 因此，使用此方法处理 UWP 游戏的大多数基本行为非常重要，如处理主窗口的激活以及确保游戏可以处理突然暂停（可能随后恢复）事件。

此时，游戏应用可以处理暂停（或继续）消息。 但仍没有要使用的窗口，并且游戏未初始化。 还需要提供更多内容！

### <a name="appinitialize-method"></a>App::Initialize 方法

在此方法中，为激活、暂停和恢复游戏创建各种事件处理程序。

获取对设备资源的访问权限。 __make_shared__ 函数用于在首次创建内存资源时创建 __shared_ptr__。 使用 __make_shared__ 的好处是它是防异常的。 它还使用同一调用为控制块和资源分配内存，从而减少构造开销。

```cpp
void App::Initialize(
    CoreApplicationView^ applicationView
    )
{
    // Register event handlers for app lifecycle. This example includes Activated, so that we
    // can make the CoreWindow active and start rendering on the window.
    applicationView->Activated +=
        ref new TypedEventHandler<CoreApplicationView^, IActivatedEventArgs^>(this, &App::OnActivated);

    CoreApplication::Suspending +=
        ref new EventHandler<SuspendingEventArgs^>(this, &App::OnSuspending);

    CoreApplication::Resuming +=
        ref new EventHandler<Platform::Object^>(this, &App::OnResuming);

    // At this point we have access to the device. 
    // We can create the device-dependent resources.
    m_deviceResources = std::make_shared<DX::DeviceResources>();
}
```

## <a name="configure-the-window-and-display-behaviors"></a>配置窗口和显示行为

现在，我们来看看 [__SetWindow__](https://msdn.microsoft.com/library/windows/apps/hh700509) 的实现。 在 __SetWindow__ 方法中，你配置窗口和显示行为。

### <a name="appsetwindow-method"></a>App::SetWindow 方法

应用单一实例提供一个表示游戏主窗口的 [__CoreWindow__](https://msdn.microsoft.com/library/windows/apps/br208225) 对象，并将其资源和事件提供给游戏。 现在，已经有一个要处理的窗口了，游戏现在可以开始在基本 UI 组件和事件中进行添加了。

然后，使用 __CoreCursor__ 方法创建指针，此方法可由鼠标和触摸控件使用。

最后，为调整窗口大小、关闭和 DPI 更改（如果显示设备更改）创建基本事件。 有关事件的详细信息，请转到[事件处理](tutorial-game-flow-management.md#events-handling)。

```cpp
void App::SetWindow(
    CoreWindow^ window
    )
{
    // Codes added to modify the original DirectX template project
    window->PointerCursor = ref new CoreCursor(CoreCursorType::Arrow, 0);

    PointerVisualizationSettings^ visualizationSettings = PointerVisualizationSettings::GetForCurrentView();
    visualizationSettings->IsContactFeedbackEnabled = false;
    visualizationSettings->IsBarrelButtonFeedbackEnabled = false;
    // --end of codes added
    
    m_deviceResources->SetWindow(window);

    window->Activated +=
        ref new TypedEventHandler<CoreWindow^, WindowActivatedEventArgs^>(this, &App::OnWindowActivationChanged);

    window->SizeChanged +=
        ref new TypedEventHandler<CoreWindow^, WindowSizeChangedEventArgs^>(this, &App::OnWindowSizeChanged);

    window->VisibilityChanged +=
        ref new TypedEventHandler<CoreWindow^, VisibilityChangedEventArgs^>(this, &App::OnVisibilityChanged);
        
    window->Closed +=
        ref new TypedEventHandler<CoreWindow^, CoreWindowEventArgs^>(this, &App::OnWindowClosed);

    DisplayInformation^ currentDisplayInformation = DisplayInformation::GetForCurrentView();

    currentDisplayInformation->DpiChanged +=
        ref new TypedEventHandler<DisplayInformation^, Platform::Object^>(this, &App::OnDpiChanged);

    currentDisplayInformation->OrientationChanged +=
        ref new TypedEventHandler<DisplayInformation^, Object^>(this, &App::OnOrientationChanged);
    
    // Codes added to modify the original DirectX template project
    currentDisplayInformation->StereoEnabledChanged +=
        ref new TypedEventHandler<DisplayInformation^, Platform::Object^>(this, &App::OnStereoEnabledChanged);
    // --end of codes added
    
    DisplayInformation::DisplayContentsInvalidated +=
        ref new TypedEventHandler<DisplayInformation^, Platform::Object^>(this, &App::OnDisplayContentsInvalidated);
}
```

## <a name="load-method-of-the-view-provider"></a>视图提供程序的 Load 方法

在设置主窗口之后，应用单一实例将调用 [__Load__](https://msdn.microsoft.com/library/windows/apps/hh700501)。 此方法使用一组异步任务来创建游戏对象、加载图形资源以及初始化游戏的状态机。 如果你希望预取游戏数据或资源，此位置比 **SetWindow** 或 **Initialize** 更好。 

由于 Windows 会对你的游戏在必须开始处理输入前所能花费的时间施加限制，因此你需要为快速完成 __Load__ 方法进行设计，以使其可以开始处理输入。 如果加载需要一段时间或者如果有很多资源，则向用户提供一个定期更新的进度栏。 此方法还用于在游戏开始之前做一些必要的准备，如设置任意开始状态或全局值。

如果你不熟悉异步编程和任务并行度概念，请转到[使用 C++ 进行异步编程](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps)。

### <a name="appload-method"></a>App::Load 方法

创建包含加载任务的 __GameMain__ 类。

```cpp
void App::Load(
    Platform::String^ entryPoint
    )
{
        if (!m_main)
    {
        m_main = std::unique_ptr<GameMain>(new GameMain(m_deviceResources));
    }
}
````

### <a name="gamemain-constructor"></a>GameMain 构造函数

* 创建和初始化游戏呈现器。 有关详细信息，请参阅[呈现框架 I：呈现简介](tutorial--assembling-the-rendering-pipeline.md)。
* 创建和初始化 Simple3Dgame 对象。 有关详细信息，请参阅[定义主游戏对象](tutorial--defining-the-main-game-loop.md)。    
* 创建游戏 UI 控件对象和显示游戏信息覆盖层以在加载资源文件时显示进度栏。 有关详细信息，请参阅[添加用户界面](tutorial--adding-a-user-interface.md)。
* 创建控制器，使它可以从控制器（触摸、鼠标或 XBox 无线控制器）读取输入。 有关详细信息，请参阅[添加控件](tutorial--adding-controls.md)。
* 控制器初始化后，我们在屏幕的左下角和右下角定义了两个矩形区域，分别用于移动和相机触摸控件。 左下角的矩形通过调用 **SetMoveRect** 定义，玩家将该矩形用作虚拟控制板来前后左右移动相机。 右下角的矩形由 **SetFireRect** 方法定义，可用作设计弹药的虚拟按钮。
* 使用 __create_task__ 和 __create_task::then__ 将资源加载划分为两个独立的阶段。 由于对 Direct3D 11 设备上下文的访问被限制到创建设备上下文的线程，而为对象创建对 Direct3D 11 设备的访问无线程限制，这意味着 **CreateGameDeviceResourcesAsync** 任务可以从完成任务 (*FinalizeCreateGameDeviceResources*) 在单独的线程上运行，其在初始线程上运行。 我们通过 **LoadLevelAsync** 和 **FinalizeLoadLevel** 使用类似的模式来加载关卡资源。

```cpp
GameMain::GameMain(const std::shared_ptr<DX::DeviceResources>& deviceResources) :
    m_deviceResources(deviceResources),
    m_windowClosed(false),
    m_haveFocus(false),
    m_gameInfoOverlayCommand(GameInfoOverlayCommand::None),
    m_visible(true),
    m_loadingCount(0),
    m_updateState(UpdateEngineState::WaitingForResources)
{
    m_deviceResources->RegisterDeviceNotify(this);

    m_renderer = ref new GameRenderer(m_deviceResources);
    m_game = ref new Simple3DGame();

    m_uiControl = m_renderer->GameUIControl();

    m_controller = ref new MoveLookController(CoreWindow::GetForCurrentThread());

    auto bounds = m_deviceResources->GetLogicalSize();

    m_controller->SetMoveRect(
        XMFLOAT2(0.0f, bounds.Height - GameConstants::TouchRectangleSize),
        XMFLOAT2(GameConstants::TouchRectangleSize, bounds.Height)
        );
    m_controller->SetFireRect(
        XMFLOAT2(bounds.Width - GameConstants::TouchRectangleSize, bounds.Height - GameConstants::TouchRectangleSize),
        XMFLOAT2(bounds.Width, bounds.Height)
        );

    SetGameInfoOverlay(GameInfoOverlayState::Loading);
    m_uiControl->SetAction(GameInfoOverlayCommand::None);
    m_uiControl->ShowGameInfoOverlay();

    create_task([this]()
    {
        // Asynchronously initialize the game class and load the renderer device resources.
        // By doing all this asynchronously, the game gets to its main loop more quickly
        // and in parallel all the necessary resources are loaded on other threads.
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
            // In the middle of a game so spin up the async task to load the level.
            return m_game->LoadLevelAsync().then([this]()
            {
                // The m_game object may need to deal with D3D device context work so
                // again the finalize code needs to run in the same thread
                // context as the m_renderer object was created because the D3D
                // device context can ONLY be accessed on a single thread.
                m_game->FinalizeLoadLevel();
                m_game->SetCurrentLevelToSavedState();
                m_updateState = UpdateEngineState::ResourcesLoaded;

            }, task_continuation_context::use_current());
        }
        else
        {
            // The game is not in the middle of a level so there aren't any level
            // resources to load.  Creating a no-op task so that in both cases
            // the same continuation logic is used.
            return create_task([]()
            {
            });
        }
    }, task_continuation_context::use_current()).then([this]()
    {
        // Since Game loading is an async task, the app visual state
        // may be too small or not have focus.  Put the state machine
        // into the correct state to reflect these cases.

        if (m_deviceResources->GetLogicalSize().Width < GameConstants::MinPlayableWidth)
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
    }, task_continuation_context::use_current());
}
```

## <a name="run-method-of-the-view-provider"></a>视图提供程序的 Run 方法

之前的三种方法：__Initialize__、__SetWindow__ 和 __Load__ 已设置了此阶段。 现在游戏可进入 **Run** 方法了，启动有趣的游戏吧！ 用于在游戏状态之间转换的事件将被发送和处理。 在游戏循环周期，图形被更新。

### <a name="apprun"></a>App::Run

启动一个 __while__ 循环，当玩家关闭游戏窗口时该循环将终止。

示例代码在游戏引擎状态机中转换为以下两个状态之一：
    * __Deactivated__：游戏窗口停用（失去焦点）或贴靠。 在发生此事件时，游戏将暂停事件处理并等待窗口获得焦点或取消贴靠。
    * __TooSmall__：游戏将更新自己的状态并呈现要显示的图形。

在你的游戏获得焦点时，必须处理消息队列中到达的每个事件，因此必须使用 **ProcessAllIfPresent** 选项调用 [**CoreWindowDispatch.ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215)。 其他选项可能导致延迟处理消息事件，这会让人感到游戏停止响应，或者导致触摸行为反应慢和没有“粘合力”。

当游戏不可见、暂停或贴靠时，你不希望应用使用任何资源循环去调度永不会到达的消息。 这种情况下，你的游戏必须使用 **ProcessOneAndAllPending**，它在获得事件前将进行阻止，随即在获得事件后处理该事件以及处理队列中在处理第一个事件期间到达的任何其他事件。 然后，[**ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215) 将在处理队列之后立即返回。

```cpp
void App::Run()
{
    m_main->Run();
}

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
                    CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
                    break;
                }
                // otherwise fall through and do normal processing to get the rendering handled.
            default:
                CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);
                Update();
                m_renderer->Render();
                m_deviceResources->Present();
                m_renderNeeded = false;
            }
        }
        else
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
    m_game->OnSuspending();  // exiting due to window close.  Make sure to save state.
}
```

## <a name="uninitialize-method-of-the-view-provider"></a>视图提供程序的 Uninitialize 方法

当用户最终结束游戏会话时，我们需要清理。 这时要使用 **Uninitialize**。

在 Windows 10 中，关闭游戏窗口不会终止应用的进程，而是将应用 singleton 的状态写入内存。 如果在系统回收此内存时必须执行一些特殊操作（包括任何特殊的资源清除），则将执行该清除的代码放入此方法。

### <a name="app-uninitialize"></a>App:: Uninitialize

```cpp
void App::Uninitialize()
{
}
```

## <a name="tips"></a>提示

在开发自己的游戏时，请围绕这些方法设计启动代码。 此处是对各方法的基本建议的简单列表：

-   使用 **Initialize** 分配主类和连接基本事件处理程序。
-   使用 **SetWindow** 创建主窗口和连接任何特定于窗口的事件。
-   使用 **Load** 来处理任何其余设置、启动对象的异步创建和加载资源。 如果需要创建任何临时文件或数据，如按顺序生成的资源，也在这里完成。


## <a name="next-steps"></a>后续步骤

这包括采用 DirectX 的 UWP 游戏的基本结构。 请记住这五种方法，我们将在此演练的其他部分加以参考。 接下来，我们将深入了解如何管理游戏状态和事件处理以按照[游戏流管理](tutorial-game-flow-management.md)让游戏顺畅进行。



