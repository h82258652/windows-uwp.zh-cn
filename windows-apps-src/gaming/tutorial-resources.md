---
author: mtoepke
title: "扩展游戏示例"
description: "恭喜你！ 此时，你已了解基本的通用 Windows 平台 (UWP) DirectX 3D 游戏的关键组件。"
ms.assetid: a1432c45-569e-7ecd-4098-f5ad6da9327e
keywords: DirectX, XAML
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 2d36c8f8f4e3f51928f1c7707e0cb6f69386645d
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="extend-the-game-sample"></a>扩展游戏示例


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

恭喜你！ 此时，你已了解了基本 Windows 通用 (UWP) DirectX 3D 游戏的关键组件。 你可以设置游戏的框架，包括视图提供程序和呈现管道，并实现基本的游戏循环。 还可以创建基本的用户界面覆盖层以及合并声音和控件。 你正在创建一个专属游戏， 这里提供了一些资源可使你进一步了解 DirectX 游戏开发。

-   [DirectX 图形和游戏](https://msdn.microsoft.com/library/windows/desktop/ee663274)
-   [Direct3D 11 概述](https://msdn.microsoft.com/library/windows/desktop/ff476345)
-   [Direct3D 11 参考](https://msdn.microsoft.com/library/windows/desktop/ff476147)

## <a name="extending-the-game-sample-using-xaml-for-the-overlay"></a>扩展游戏示例：将 XAML 用于覆盖层


还有一种方法我们未深入讨论，即使用 XAML 代替 Direct2D 作为覆盖层。 与 Direct2D 相比，XAML 在绘制用户界面元素方面有许多优势， 最重要的一点是它可以更方便地将 Windows 10 外观和感觉融入到 DirectX 游戏中。 许多用于定义 UWP 应用的常用元素、 样式和行为都紧密集成到 XAML 模型中，大幅减少了游戏开发人员的实现工作。 如果你自己设计的游戏有一个复杂的用户界面，请考虑使用 XAML 代替 Direct2D。

那么，使用 Direct2D 实现用户界面与使用 XAML 实现相同的用户界面有何不同？

-   可在 XAML 文件 \*.xaml 中定义覆盖层，而不是将其作为手动放置并写入 Direct2D 目标缓冲区 Direct2D 基元和 DirectWrite 字符串的集合。 如果你了解 XAML，你会发现创建和配置较复杂的覆盖层要容易得多，尤其是使用 Visual Studio 的 XAML 编辑工具。
-   用户界面元素来自属于 Windows 运行时 XAML API 一部分的标准化元素，其中包括 [**Windows::UI::Xaml**](https://msdn.microsoft.com/library/windows/apps/br209045) 和 [**Windows::UI::Xaml::Controls**](https://msdn.microsoft.com/library/windows/apps/br227716)。 处理 XAML 用户界面元素行为的代码在代码隐藏文件 Main.xaml.cpp 中定义。
-   XAML 作为一个紧密集成的 Windows 运行时组件，通常处理大小调整和视图状态更改事件，相应地转换覆盖层，因此不需要手动指定如何重新绘制覆盖层的组件。
-   交换链不直接附加到 [**Windows::UI::Core::CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 对象；或者至少你不必附加它。 而在构建新的 [**SwapChainBackgroundPanel**](https://msdn.microsoft.com/library/windows/apps/hh702626) 对象时，合并 XAML 的 DirectX 应用将关联一个交换链。 **SwapChainBackgroundPanel** 对象设置为在启动时由应用单一实例创建的当前窗口对象的 [**Content**](https://msdn.microsoft.com/library/windows/apps/br209051) 属性，该窗口作为 **CoreWindow** 对象传递到 **Simple3DGame::Initialize**。

可在 **Main.app.xaml** 文件中为 **SwapChainBackgroundPanel** 声明 XAML，如下所示：

```xml
<Page
    x:Name="DXMainPage"
    x:Class="Simple3DGameXaml.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    d:DesignWidth="1366"
    d:DesignHeight="768">

    <SwapChainBackgroundPanel x:Name="DXSwapChainPanel">

    <!-- ... XAML user controls and elements -->

    </SwapChainBackgroundPanel>
</Page>
```

```cpp
void App::OnLaunched(LaunchActivatedEventArgs^ /* args */)
{
    Suspending += ref new SuspendingEventHandler(this, &App::OnSuspending);
    Resuming += ref new EventHandler<Object^>(this, &App::OnResuming);

    m_mainPage = ref new MainPage();
    m_mainPage->SetApp(this);

    Window::Current->Content = m_mainPage;
    Window::Current->Activated += ref new WindowActivatedEventHandler(this, &App::OnWindowActivationChanged);
    Window::Current->Activate();

    m_controller = ref new MoveLookController();
    m_renderer = ref new GameRenderer();
    m_game = ref new Simple3DGame();

    auto window = Window::Current->CoreWindow;

    window->SizeChanged +=
        ref new TypedEventHandler<CoreWindow^, WindowSizeChangedEventArgs^>(this, &App::OnWindowSizeChanged);

    window->VisibilityChanged +=
        ref new TypedEventHandler<CoreWindow^, VisibilityChangedEventArgs^>(this, &App::OnVisibilityChanged);

    DisplayProperties::LogicalDpiChanged +=
        ref new DisplayPropertiesEventHandler(this, &App::OnLogicalDpiChanged);

    m_controller->Initialize(window);
    m_controller->AutoFire(false);

    m_controller->SetMoveRect(
        XMFLOAT2(0.0f, window->Bounds.Height - GameConstants::TouchRectangleSize),
        XMFLOAT2(GameConstants::TouchRectangleSize, window->Bounds.Height)
        );
    m_controller->SetFireRect(
        XMFLOAT2(window->Bounds.Width - GameConstants::TouchRectangleSize, window->Bounds.Height - GameConstants::TouchRectangleSize),
        XMFLOAT2(window->Bounds.Width, window->Bounds.Height)
        );

    m_renderer->Initialize(window, m_mainPage->GetSwapChainBackgroundPanel(), DisplayProperties::LogicalDpi);
    SetGameInfoOverlay(GameInfoOverlayState::Loading);
    SetAction(GameInfoOverlayCommand::None);
    ShowGameInfoOverlay();

    m_onRenderingEventToken = CompositionTarget::Rendering::add(ref new EventHandler<Object^>(this, &App::OnRendering));
    m_renderNeeded = true;

    create_task([this]()
    {
        // Asynchronously initialize the game class and load the renderer device resources.
        // This way, the game gets to its main loop faster 
        // and in parallel all the necessary resources are loaded on other threads.
        m_game->Initialize(m_controller, m_renderer);

        return m_renderer->CreateGameDeviceResourcesAsync(m_game);

    }).then([this]()
    {
        // The finalize code needs to run in the same thread context
        // as the m_renderer object was created because the D3D device context
        // can be accessed only on a single thread.
        m_renderer->FinalizeCreateGameDeviceResources();

        InitializeLicense();
        InitializeGameState();

        if (m_updateState == UpdateEngineState::WaitingForResources)
        {
            // In the middle of a game, so spin up the async task to load the level.
            create_task([this]()
            {
                return m_game->LoadLevelAsync();

            }).then([this]()
            {
                // The m_game object may need to deal with D3D device context work so
                // again the finalize code needs to run in the same thread
                // context as the m_renderer object was created because the D3D 
                // device context can  be accessed only on a single thread.
                m_game->FinalizeLoadLevel();
                m_updateState = UpdateEngineState::ResourcesLoaded;

            }, task_continuation_context::use_current());
        }
    }, task_continuation_context::use_current());
}
```

若要将配置的交换链连接到 XAML 定义的 [**SwapChainBackgroundPanel**](https://msdn.microsoft.com/library/windows/apps/hh702626) 面板实例，必须获取底层本机 [**ISwapChainBackgroundPanel**](https://msdn.microsoft.com/library/windows/desktop/hh848326) 接口实现的指针并对其调用 [**SetSwapChain**](https://msdn.microsoft.com/library/windows/desktop/hh848327)，从而向其传递配置的交换链。 从派生自专用于 DirectX/XAML 互操作的 **DirectXBase::CreateWindowSizeDependentResources** 的方法：

```cpp
        ComPtr<IDXGIDevice1> dxgiDevice;
        DX::ThrowIfFailed(
            m_d3dDevice.As(&dxgiDevice)
            );

        // Next, get the associated adapter from the DXGI Device.
        ComPtr<IDXGIAdapter> dxgiAdapter;
        DX::ThrowIfFailed(
            dxgiDevice->GetAdapter(&dxgiAdapter)
            );

        // Next, get the parent factory from the DXGI adapter.
        ComPtr<IDXGIFactory2> dxgiFactory;
        DX::ThrowIfFailed(
            dxgiAdapter->GetParent(IID_PPV_ARGS(&dxgiFactory))
            );

        // Create the swap chain and then associate it with the SwapChainBackgroundPanel.
        DX::ThrowIfFailed(
            dxgiFactory->CreateSwapChainForComposition(
                m_d3dDevice.Get(),
                &swapChainDesc,
                nullptr,
                &m_swapChain
                )
            );

        ComPtr<ISwapChainBackgroundPanelNative> dxRootPanelAsNative;

        // Set the swap chain on the SwapChainBackgroundPanel.
        reinterpret_cast<IUnknown*>(m_swapChainPanel)->QueryInterface(__uuidof(ISwapChainBackgroundPanelNative), (void**)&dxRootPanelAsNative);

        DX::ThrowIfFailed(
            dxRootPanelAsNative->SetSwapChain(m_swapChain.Get())
            );

        DX::ThrowIfFailed(
            dxgiDevice->SetMaximumFrameLatency(1)
            );
```

有关此过程的详细信息，请参阅 [DirectX 和 XAML 互操作](https://msdn.microsoft.com/library/windows/apps/hh825871)。

## <a name="complete-code-for-the-xaml-game-sample-xaml-codebehinds"></a>XAML 游戏示例 XAML 代码隐藏的完整代码


下面是 XAML 版本的 Direct3D 11.1 射击游戏示例中的代码隐藏的完整代码。

（与其余主题中讨论的游戏示例版本不同，XAML 版本分别在 **App.xaml.cpp** 和 **MainPage.xaml.cpp** 文件中定义其框架，而不是在 **DirectXApp.cpp** 和 **GameInfoOverlay.cpp** 中。）

App.xaml.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

#include "MainPage.xaml.h"
#include "Simple3DGame.h"
#include "App.g.h"

namespace Simple3DGameXaml
{
    private enum class UpdateEngineState
    {
        Uninitialized,
        WaitingForResources,
        WaitingForPress,
        Dynamics,
        Snapped,
        Suspended,
        Deactivated,
    };

    private enum class PressResultState
    {
        LoadGame,
        PlayLevel,
        ContinueLevel,
    };

    private enum class GameInfoOverlayState
    {
        GameStats,
        GameOverExpired,
        GameOverCompleted,
        LevelStart,
        Pause,
        Snapped,
    };

    public ref class App sealed
    {
    public:
        App();

        virtual void OnLaunched(Windows::ApplicationModel::Activation::LaunchActivatedEventArgs^ pArgs);

        void PauseRequested() { if (m_updateState == UpdateEngineState::Dynamics) m_pauseRequested = true; };
        void PressComplete()  { if (m_updateState == UpdateEngineState::WaitingForPress) m_pressComplete = true; };
        void ResetGame();

    private:
        ~App();

        void OnSuspending(
            _In_ Platform::Object^ sender, 
            _In_ Windows::ApplicationModel::SuspendingEventArgs^ args
            );
        void OnResuming(
            _In_ Platform::Object^ sender,
            _In_ Platform::Object^ args
            );

        void OnViewStateChanged(
            _In_ Windows::UI::ViewManagement::ApplicationView^ view,
            _In_ Windows::UI::ViewManagement::ApplicationViewStateChangedEventArgs^ args
            );

        void OnWindowActivationChanged(
            _In_ Platform::Object^ sender,
            _In_ Windows::UI::Core::WindowActivatedEventArgs^ args
            );

        void OnWindowSizeChanged(
            _In_ Windows::UI::Core::CoreWindow^ sender,
            _In_ Windows::UI::Core::WindowSizeChangedEventArgs^ args
            );

        void OnLogicalDpiChanged(
            _In_ Platform::Object^ sender
            );

        void OnRendering(
            _In_ Object^ sender, 
            _In_ Object^ args
            );

        void InitializeGameState();
        void Update();
        void SetGameInfoOverlay(GameInfoOverlayState state);
        void SetAction (GameInfoOverlayCommand command);
        void ShowGameInfoOverlay();
        void HideGameInfoOverlay();
        void SetSnapped();
        void HideSnapped();

        Windows::Foundation::EventRegistrationToken         m_eventToken;
        bool                                                m_pauseRequested;
        bool                                                m_pressComplete;
        bool                                                m_renderNeeded;
        bool                                                m_haveFocus;

        MainPage^                                           m_mainPage;
        Simple3DGame^                                       m_game;
        MoveLookController^                                 m_controller;           // Controller to handle user input

        UpdateEngineState                                   m_updateState;
        UpdateEngineState                                   m_updateStateNext;
        PressResultState                                    m_pressResult;
        GameInfoOverlayState                                m_gameInfoOverlayState;
    };
}
```

App.xaml.cpp

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "App.xaml.h"

using namespace Simple3DGameXaml;

using namespace Platform;
using namespace Windows::ApplicationModel::Activation;
using namespace Windows::ApplicationModel;
using namespace Windows::ApplicationModel::Core;
using namespace Windows::ApplicationModel::Activation;
using namespace Windows::UI::Core;
using namespace Windows::UI::ViewManagement;
using namespace Windows::Foundation;
using namespace Windows::Foundation::Collections;
using namespace Windows::UI::Xaml;
using namespace Windows::UI::Xaml::Controls;
using namespace Windows::UI::Xaml::Controls::Primitives;
using namespace Windows::UI::Xaml::Data;
using namespace Windows::UI::Xaml::Input;
using namespace Windows::UI::Xaml::Media;
using namespace Windows::UI::Xaml::Navigation;
using namespace Windows::UI::Xaml::Media::Animation;
using namespace Windows::Graphics::Display;

//----------------------------------------------------------------------
App::App():
    m_pauseRequested(false),
    m_pressComplete(false),
    m_renderNeeded(false),
    m_haveFocus(false)
{
    InitializeComponent();
}
//----------------------------------------------------------------------
App::~App()
{
    CompositionTarget::Rendering::remove(m_eventToken);
}
//----------------------------------------------------------------------
void App::OnLaunched(Windows::ApplicationModel::Activation::LaunchActivatedEventArgs^ args)
{
    m_mainPage = ref new MainPage(this);

    Window::Current->Content = m_mainPage;
    Window::Current->Activated += ref new WindowActivatedEventHandler(this, &App::OnWindowActivationChanged);
    Window::Current->Activate();

    // Create the game and pass to window and root panel for swap chain setup.
    m_controller = ref new MoveLookController();
    m_controller->Initialize(Window::Current->CoreWindow);
    
    m_game = ref new Simple3DGame();
    m_game->Initialize(Window::Current->CoreWindow, m_mainPage, DisplayProperties::LogicalDpi, m_controller);

    m_eventToken = CompositionTarget::Rendering::add(ref new EventHandler<Object^>(this, &App::OnRendering));

    ApplicationView::GetForCurrentView()->ViewStateChanged +=
        ref new TypedEventHandler<ApplicationView^, ApplicationViewStateChangedEventArgs^>(
            this,
            &App::OnViewStateChanged
            );

    CoreApplication::Suspending += ref new EventHandler<SuspendingEventArgs^>(this, &App::OnSuspending);
    CoreApplication::Resuming += ref new EventHandler<Object^>(this, &App::OnResuming);

    Window::Current->CoreWindow->SizeChanged += 
        ref new TypedEventHandler<CoreWindow^, WindowSizeChangedEventArgs^>(this, &App::OnWindowSizeChanged);

    DisplayProperties::LogicalDpiChanged +=
        ref new DisplayPropertiesEventHandler(this, &App::OnLogicalDpiChanged);

    InitializeGameState();
}
//----------------------------------------------------------------------
void App::OnRendering(
    _In_ Object^ sender, 
    _In_ Object^ args
    )
{
    Update();
    if (m_updateState == UpdateEngineState::Dynamics  || m_renderNeeded)
    {
        m_game->Render();
        m_renderNeeded = false;
    }
}
//--------------------------------------------------------------------------------------
void App::OnWindowSizeChanged(
    _In_ CoreWindow^ sender,
    _In_ WindowSizeChangedEventArgs^ args
    )
{
    m_renderNeeded = true;
    m_game->UpdateForWindowSizeChange();
}
//--------------------------------------------------------------------------------------
void App::OnLogicalDpiChanged(
    _In_ Object^ sender
    )
{
    m_game->SetDpi(DisplayProperties::LogicalDpi);
}
//--------------------------------------------------------------------------------------
void App::InitializeGameState()
{
    //
    // Set up the initial state machine for handling the Game playing state.
    //
    if (m_game->GameActive() && m_game->LevelActive())
    {
        // The last time the game terminated it was in the middle 
        // of a level. 
        // We are waiting for the user to continue the game.
        m_updateState = UpdateEngineState::WaitingForResources;
        m_pressResult = PressResultState::ContinueLevel;
        SetGameInfoOverlay(GameInfoOverlayState::Pause);
    }
    else if (!m_game->GameActive() && (m_game->HighScore().totalHits > 0))
    {
        // The last time the game terminated the game had been completed.
        // Show the high score.
        // We are waiting for the user to start a new game.
        m_updateState = UpdateEngineState::WaitingForResources;
        m_pressResult = PressResultState::LoadGame;
        SetGameInfoOverlay(GameInfoOverlayState::GameStats);
    }
    else
    {
        // This is either the first time the game has run or
        // the last time the game terminated the level was completed.
        // We are waiting for the user to begin the next level.
        m_updateState = UpdateEngineState::WaitingForResources;
        m_pressResult = PressResultState::PlayLevel;
        SetGameInfoOverlay(GameInfoOverlayState::LevelStart);
    }
    SetAction(GameInfoOverlayCommand::PleaseWait);
    ShowGameInfoOverlay();
}
//--------------------------------------------------------------------------------------
void App::Update()
{
    m_controller->Update();

    switch (m_updateState)
    {
    case UpdateEngineState::WaitingForResources:
        if (m_game->IsResourceLoadingComplete())
        {
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
            m_controller->WaitForPress();
            ShowGameInfoOverlay();
            m_renderNeeded = true;
        }
        break;

    case UpdateEngineState::WaitingForPress:
        if (m_controller->IsPressComplete() || m_pressComplete)
        {
            m_pressComplete = false;

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
        if (m_controller->IsPauseRequested() || m_pauseRequested)
        {
            m_pauseRequested = false;

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
            // Transitioning state, so enable waiting for the press event
            m_controller->WaitForPress();
        }
        break;
    }
}
//--------------------------------------------------------------------------------------
void App::OnWindowActivationChanged(
    _In_ Platform::Object^ sender,
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
                m_controller->WaitForPress();
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
    _In_ Platform::Object^ sender, 
    _In_ SuspendingEventArgs^ args
    )
{
    // Save application state.
    // If your application needs time to complete a lengthy operation, it can request a deferral.
    // The SuspendingOperation has a deadline time. Make sure all your operations are complete by that time!
    // If the app doesn't return from this handler within five seconds, it will be terminated.
    SuspendingOperation^ op = args->SuspendingOperation;
    SuspendingDeferral^ deferral = op->GetDeferral();

    switch (m_updateState)
    {
    case UpdateEngineState::Dynamics:
       // Game is in the active game play state, Stop Game Timer and Pause play and save state.
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
        // Any other state don't save as next state as they are transient states and have already set m_updateStateNext
        break;
    }
    m_updateState = UpdateEngineState::Suspended;

    m_controller->Active(false);
    m_game->OnSuspending();

    deferral->Complete();
}
//--------------------------------------------------------------------------------------
void App::OnResuming(
    _In_ Platform::Object^ sender,
    _In_ Platform::Object^ args
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
        m_controller->WaitForPress();
    }
    m_game->OnResuming();
    ShowGameInfoOverlay();
    m_renderNeeded = true;
}
//--------------------------------------------------------------------------------------
void App::OnViewStateChanged(
     _In_ ApplicationView^ view, 
     _In_ ApplicationViewStateChangedEventArgs^ args
     )
{
    m_renderNeeded = true;

    if (args->ViewState == ApplicationViewState::Snapped)
    {
        switch (m_updateState)
        {
        case UpdateEngineState::Dynamics:
            // From Dynamic mode, when coming out of SNAPPED layout rather than going directly back into game play
            // go to the paused state waiting for user input to continue.
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
    else if (args->ViewState == ApplicationViewState::Filled ||
        args->ViewState == ApplicationViewState::FullScreenLandscape ||
        args->ViewState == ApplicationViewState::FullScreenPortrait)
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
                    m_controller->WaitForPress();
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
    case GameInfoOverlayState::GameStats:
        m_mainPage->SetGameStats(
            m_game->HighScore().levelCompleted + 1,
            m_game->HighScore().totalHits,
            m_game->HighScore().totalShots
            );
        break;

    case GameInfoOverlayState::LevelStart:
        m_mainPage->SetLevelStart(
            m_game->LevelCompleted() + 1,
            m_game->CurrentLevel()->Objective(),
            m_game->CurrentLevel()->TimeLimit(),
            m_game->BonusTime()
            );
        break;

    case GameInfoOverlayState::GameOverCompleted:
        m_mainPage->SetGameOver(
            true, 
            m_game->LevelCompleted() + 1,
            m_game->TotalHits(),
            m_game->TotalShots(),
            m_game->HighScore().totalHits
            );
        break;

    case GameInfoOverlayState::GameOverExpired:
        m_mainPage->SetGameOver(
            false, 
            m_game->LevelCompleted(),
            m_game->TotalHits(),
            m_game->TotalShots(),
            m_game->HighScore().totalHits
            );
        break;

    case GameInfoOverlayState::Pause:
        m_mainPage->SetPause(
            m_game->LevelCompleted() + 1,
            m_game->TotalHits(),
            m_game->TotalShots(),
            m_game->TimeRemaining()
            );
        break;
    }
}
//--------------------------------------------------------------------------------------
void App::SetAction(GameInfoOverlayCommand command)
{
    m_mainPage->SetAction(command);
}
//--------------------------------------------------------------------------------------
void App::ShowGameInfoOverlay()
{
    m_mainPage->ShowGameInfoOverlay();
}
//--------------------------------------------------------------------------------------
void App::HideGameInfoOverlay()
{
    m_mainPage->HideGameInfoOverlay();
}
//--------------------------------------------------------------------------------------
void App::SetSnapped()
{
    m_mainPage->SetSnapped();
}
//--------------------------------------------------------------------------------------
void App::HideSnapped()
{
    m_mainPage->HideSnapped();
}
//--------------------------------------------------------------------------------------
void App::ResetGame()
{
    m_updateState = UpdateEngineState::WaitingForResources;
    m_pressResult = PressResultState::PlayLevel;
    m_controller->Active(false);
    m_game->LoadGame();
    SetAction(GameInfoOverlayCommand::PleaseWait);
    SetGameInfoOverlay(GameInfoOverlayState::LevelStart);
    ShowGameInfoOverlay();
    m_renderNeeded = true;
}
```

MainPage.xaml

```xml
<SwapChainBackgroundPanel
    x:Name="DXSwapChainPanel"
    x:Class="Simple3DGameXaml.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    d:DesignWidth="1366"
    d:DesignHeight="768">
  <UserControl x:Name="LayoutControl" Background="Transparent">
    <Grid x:Name="LayoutRoot">
      <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="GameInfoOverlayStates">
          <VisualState x:Name="NormalState">
            <Storyboard>
              <DoubleAnimation Storyboard.TargetName="GameInfoOverlay" Storyboard.TargetProperty="(UIElement.Opacity)" Duration="00:00:00.25" To="0">
                <DoubleAnimation.EasingFunction>
                  <CubicEase EasingMode="EaseIn" />
                </DoubleAnimation.EasingFunction>
              </DoubleAnimation>

              <ObjectAnimationUsingKeyFrames Storyboard.TargetProperty="Visibility" Storyboard.TargetName="GameInfoOverlay">
                <DiscreteObjectKeyFrame KeyTime="0:0:1" Value="Collapsed" />
              </ObjectAnimationUsingKeyFrames>
            </Storyboard>
          </VisualState>

          <VisualState x:Name="GameInfoOverlayState">
            <Storyboard>
              <DoubleAnimation Storyboard.TargetName="GameInfoOverlay" Storyboard.TargetProperty="(UIElement.Opacity)" Duration="00:00:00.25" To="1">
                <DoubleAnimation.EasingFunction>
                  <CubicEase EasingMode="EaseIn" />
                </DoubleAnimation.EasingFunction>
              </DoubleAnimation>

              <ObjectAnimationUsingKeyFrames Storyboard.TargetProperty="Visibility" Storyboard.TargetName="GameInfoOverlay">
                <DiscreteObjectKeyFrame KeyTime="0:0:0" Value="Visible" />
              </ObjectAnimationUsingKeyFrames>
            </Storyboard>
          </VisualState>

          <VisualState x:Name="SnappedState">
            <Storyboard>
              <DoubleAnimation Storyboard.TargetName="SDKHeader" Storyboard.TargetProperty="(UIElement.Opacity)" Duration="00:00:00.25" To="0">
                <DoubleAnimation.EasingFunction>
                  <CubicEase EasingMode="EaseIn" />
                </DoubleAnimation.EasingFunction>
              </DoubleAnimation>
              <DoubleAnimation Storyboard.TargetName="FullscreenView" Storyboard.TargetProperty="(UIElement.Opacity)" Duration="00:00:00.25" To="0">
                <DoubleAnimation.EasingFunction>
                  <CubicEase EasingMode="EaseIn" />
                </DoubleAnimation.EasingFunction>
              </DoubleAnimation>
              <DoubleAnimation Storyboard.TargetName="SnappedView" Storyboard.TargetProperty="(UIElement.Opacity)" Duration="00:00:00.25" To="1">
                <DoubleAnimation.EasingFunction>
                  <CubicEase EasingMode="EaseIn" />
                </DoubleAnimation.EasingFunction>
              </DoubleAnimation>

              <ObjectAnimationUsingKeyFrames Storyboard.TargetProperty="Visibility" Storyboard.TargetName="SDKHeader">
                <DiscreteObjectKeyFrame KeyTime="0:0:1" Value="Collapsed" />
              </ObjectAnimationUsingKeyFrames>
              <ObjectAnimationUsingKeyFrames Storyboard.TargetProperty="Visibility" Storyboard.TargetName="FullscreenView">
                <DiscreteObjectKeyFrame KeyTime="0:0:1" Value="Collapsed" />
              </ObjectAnimationUsingKeyFrames>
              <ObjectAnimationUsingKeyFrames Storyboard.TargetProperty="Visibility" Storyboard.TargetName="SnappedView">
                <DiscreteObjectKeyFrame KeyTime="0:0:0" Value="Visible" />
              </ObjectAnimationUsingKeyFrames>
            </Storyboard>
          </VisualState>

          <VisualState x:Name="UnsnappedState">
            <Storyboard>
              <DoubleAnimation Storyboard.TargetName="SDKHeader" Storyboard.TargetProperty="(UIElement.Opacity)" Duration="00:00:00.25" To="1">
                <DoubleAnimation.EasingFunction>
                  <CubicEase EasingMode="EaseIn" />
                </DoubleAnimation.EasingFunction>
              </DoubleAnimation>
              <DoubleAnimation Storyboard.TargetName="FullscreenView" Storyboard.TargetProperty="(UIElement.Opacity)" Duration="00:00:00.25" To="1">
                <DoubleAnimation.EasingFunction>
                  <CubicEase EasingMode="EaseIn" />
                </DoubleAnimation.EasingFunction>
              </DoubleAnimation>
              <DoubleAnimation Storyboard.TargetName="SnappedView" Storyboard.TargetProperty="(UIElement.Opacity)" Duration="00:00:00.25" To="0">
                <DoubleAnimation.EasingFunction>
                  <CubicEase EasingMode="EaseIn" />
                </DoubleAnimation.EasingFunction>
              </DoubleAnimation>

              <ObjectAnimationUsingKeyFrames Storyboard.TargetProperty="Visibility" Storyboard.TargetName="SDKHeader">
                <DiscreteObjectKeyFrame KeyTime="0:0:0" Value="Visible" />
              </ObjectAnimationUsingKeyFrames>
              <ObjectAnimationUsingKeyFrames Storyboard.TargetProperty="Visibility" Storyboard.TargetName="FullscreenView">
                <DiscreteObjectKeyFrame KeyTime="0:0:0" Value="Visible" />
              </ObjectAnimationUsingKeyFrames>
              <ObjectAnimationUsingKeyFrames Storyboard.TargetProperty="Visibility" Storyboard.TargetName="SnappedView">
                <DiscreteObjectKeyFrame KeyTime="0:0:1" Value="Collapsed" />
              </ObjectAnimationUsingKeyFrames>
            </Storyboard>
          </VisualState>
        </VisualStateGroup>
      </VisualStateManager.VisualStateGroups>
      <Grid x:Name="ContentRoot">
        <Grid.RowDefinitions>
          <RowDefinition Height="1*"/>
          <RowDefinition Height="2*"/>
          <RowDefinition Height="1*"/>
        </Grid.RowDefinitions>

        <!-- Sample Overlay Title -->
        <StackPanel x:Name="SDKHeader" Grid.Row="0">
          <StackPanel Orientation="Horizontal">
            <Image Source="windows-sdk.png"/>
            <TextBlock Text="Windows 8 SDK Samples" VerticalAlignment="Bottom" Style="{StaticResource OverlayTitleStyle}" TextWrapping="Wrap"/>
          </StackPanel>
          <TextBlock x:Name="FeatureName" Text="UWP DirectX/XAML first-person game sample" Style="{StaticResource OverlayH1Style}" TextWrapping="Wrap"/>
        </StackPanel>
        <!-- End Sample Overlay Title -->

        <Grid Grid.Row="1" x:Name="SnappedView" Background="{StaticResource PageBackgroundBrush}" Visibility="Collapsed">
          <Grid.RowDefinitions>
            <RowDefinition Height="2*"/>
            <RowDefinition Height="5*"/>
            <RowDefinition Height="1*"/>
          </Grid.RowDefinitions>

          <!-- Title of the Game Info Overlay -->
          <StackPanel Grid.Row="1" Orientation="Horizontal" HorizontalAlignment="Center">
            <TextBlock
              Text="Game Paused"
              Style="{StaticResource TitleStyle}"/>
          </StackPanel>
        </Grid>

        <Grid Grid.Row="1" x:Name="FullscreenView">
          <Grid.ColumnDefinitions>
            <ColumnDefinition Width="1*"/>
            <ColumnDefinition Width="2*"/>
            <ColumnDefinition Width="1*"/>
          </Grid.ColumnDefinitions>

          <!-- Center of the outer Grid.  This is the Center 50% of the screen -->
          <Grid x:Name="GameInfoOverlay" Grid.Column="1" Background="{StaticResource PageBackgroundBrush}" Visibility="Collapsed"
                Tapped="OnGameInfoOverlayTapped">
            <Grid.RowDefinitions>
              <RowDefinition Height="2*"/>
              <RowDefinition Height="5*"/>
              <RowDefinition Height="1*"/>
            </Grid.RowDefinitions>

            <!-- Title of the Game Info Overlay -->
            <StackPanel Grid.Row="0" Orientation="Horizontal" HorizontalAlignment="Center">
              <TextBlock x:Name="GameInfoOverlayTitle"
                Text="Title"
                Style="{StaticResource TitleStyle}"/>
            </StackPanel>

            <!-- Body1: Game Statistics -->
            <Grid x:Name="Stats" Grid.Row="1" Visibility="Collapsed">
              <Grid.ColumnDefinitions>
                <ColumnDefinition Width="Auto"/>
                <ColumnDefinition Width="*"/>
              </Grid.ColumnDefinitions>
              <Grid.RowDefinitions>
                <RowDefinition Height="Auto"/>
                <RowDefinition Height="Auto"/>
                <RowDefinition Height="Auto"/>
                <RowDefinition Height="*"/>
              </Grid.RowDefinitions>

              <StackPanel Grid.Column="0" Grid.Row="0" Orientation="Horizontal">
                <TextBlock
                  Text="Levels Completed"
                  Style="{StaticResource H1Style}"/>
              </StackPanel>
              <StackPanel Grid.Column="0" Grid.Row="1" Orientation="Horizontal">
                <TextBlock
                  Text="Total Points"
                  Style="{StaticResource H1Style}"/>
              </StackPanel>
              <StackPanel Grid.Column="0" Grid.Row="2" Orientation="Horizontal">
                <TextBlock
                  Text="Total Shots"
                  Style="{StaticResource H1Style}"/>
              </StackPanel>
              <StackPanel x:Name="HighScoreTitle" Grid.Column="0" Grid.Row="3" Orientation="Horizontal" Visibility="Visible">
                <TextBlock
                  Text="High Score"
                  Style="{StaticResource H1StyleSpace}"/>
              </StackPanel>
              <StackPanel Grid.Column="1" Grid.Row="0" Orientation="Horizontal">
                <TextBlock x:Name="LevelsCompleted"
                  Text="1"
                  Style="{StaticResource H1Style}"/>
              </StackPanel>
              <StackPanel Grid.Column="1" Grid.Row="1" Orientation="Horizontal">
                <TextBlock x:Name="TotalPoints"
                  Text="9"
                  Style="{StaticResource H1Style}"/>
              </StackPanel>
              <StackPanel Grid.Column="1" Grid.Row="2" Orientation="Horizontal">
                <TextBlock x:Name="TotalShots"
                  Text="25"
                  Style="{StaticResource H1Style}"/>
              </StackPanel>
              <StackPanel x:Name="HighScoreData" Grid.Column="1" Grid.Row="3" Orientation="Horizontal">
                <TextBlock x:Name="HighScore"
                  Text="120"
                  Style="{StaticResource H1StyleSpace}"/>
              </StackPanel>
            </Grid>

            <!-- Body2: Level Start -->
            <Grid x:Name="LevelStart" Grid.Row="1" Visibility="Visible">
              <Grid.ColumnDefinitions>
                <ColumnDefinition Width="Auto"/>
                <ColumnDefinition Width="*"/>
              </Grid.ColumnDefinitions>
              <Grid.RowDefinitions>
                <RowDefinition Height="Auto"/>
                <RowDefinition Height="Auto"/>
                <RowDefinition Height="*"/>
              </Grid.RowDefinitions>

              <StackPanel Grid.Column="0" Grid.Row="0" Orientation="Horizontal">
                <TextBlock
                  Text="Objective"
                  Style="{StaticResource H1Style}"/>
              </StackPanel>
              <StackPanel Grid.Column="0" Grid.Row="1" Orientation="Horizontal">
                <TextBlock
                  Text="Time Limit"
                  Style="{StaticResource H1Style}"/>
              </StackPanel>
              <StackPanel x:Name="BonusTimeTitle" Grid.Column="0" Grid.Row="2" Orientation="Horizontal">
                <TextBlock
                  Text="Bonus Time"
                  Style="{StaticResource H1Style}"/>
              </StackPanel>
              <Grid Grid.Column="1" Grid.Row="0">
                <TextBlock x:Name="Objective"
                  Text="Objective Text - replaced before it is displayed"
                  TextWrapping="Wrap"
                  Style="{StaticResource H1Style}"/>
              </Grid>
              <StackPanel Grid.Column="1" Grid.Row="1" Orientation="Horizontal">
                <TextBlock x:Name="TimeLimit"
                  Text="30 sec"
                  Style="{StaticResource H1Style}"/>
              </StackPanel>
              <StackPanel x:Name="BonusTimeData" Grid.Column="1" Grid.Row="2" Orientation="Horizontal">
                <TextBlock x:Name="BonusTime"
                  Text="20 sec"
                  Style="{StaticResource H1Style}"/>
              </StackPanel>
            </Grid>

            <!-- Body3: Pause -->
            <Grid x:Name="PauseData" Grid.Row="1" Visibility="Visible">
              <Grid.ColumnDefinitions>
                <ColumnDefinition Width="Auto"/>
                <ColumnDefinition Width="*"/>
              </Grid.ColumnDefinitions>
              <Grid.RowDefinitions>
                <RowDefinition Height="Auto"/>
                <RowDefinition Height="Auto"/>
                <RowDefinition Height="Auto"/>
                <RowDefinition Height="*"/>
              </Grid.RowDefinitions>

              <StackPanel Grid.Column="0" Grid.Row="0" Orientation="Horizontal">
                <TextBlock
                  Text="Level"
                  Style="{StaticResource H1Style}"/>
              </StackPanel>
              <StackPanel Grid.Column="0" Grid.Row="1" Orientation="Horizontal">
                <TextBlock
                  Text="Hits"
                  Style="{StaticResource H1Style}"/>
              </StackPanel>
              <StackPanel Grid.Column="0" Grid.Row="2" Orientation="Horizontal">
                <TextBlock
                  Text="Shots"
                  Style="{StaticResource H1Style}"/>
              </StackPanel>
              <StackPanel Grid.Column="0" Grid.Row="3" Orientation="Horizontal">
                <TextBlock
                  Text="Time"
                  Style="{StaticResource H1Style}"/>
              </StackPanel>
              <StackPanel Grid.Column="1" Grid.Row="0" Orientation="Horizontal">
                <TextBlock x:Name="PauseLevel"
                  Text="1"
                  TextWrapping="Wrap"
                  Style="{StaticResource H1Style}"/>
              </StackPanel>
              <StackPanel Grid.Column="1" Grid.Row="1" Orientation="Horizontal">
                <TextBlock x:Name="PauseHits"
                  Text="0"
                  Style="{StaticResource H1Style}"/>
              </StackPanel>
              <StackPanel Grid.Column="1" Grid.Row="2" Orientation="Horizontal">
                <TextBlock x:Name="PauseShots"
                  Text="0"
                  Style="{StaticResource H1Style}"/>
              </StackPanel>
              <StackPanel Grid.Column="1" Grid.Row="3" Orientation="Horizontal">
                <TextBlock x:Name="PauseTimeRemaining"
                  Text="20.0 sec"
                  Style="{StaticResource H1Style}"/>
              </StackPanel>
            </Grid>

            <!-- Footer of Game Info Overlay.  There are several options -->
            <StackPanel x:Name="TapToContinue" Grid.Row="2" Orientation="Horizontal" Visibility="Collapsed">
              <TextBlock
                Text="Tap to continue ..."
                Style="{StaticResource H3Style}"
                TextWrapping="Wrap"/>
            </StackPanel>
            <StackPanel x:Name="PleaseWait" Grid.Row="2" Orientation="Horizontal" Visibility="Collapsed">
              <TextBlock
                Text="Level Loading Please Wait ..."
                Style="{StaticResource H3Style}"
                TextWrapping="Wrap"/>
            </StackPanel>
            <StackPanel x:Name="PlayAgain" Grid.Row="2" Orientation="Horizontal" Visibility="Collapsed">
              <TextBlock
                Text="Tap to play again ..."
                Style="{StaticResource H3Style}"
                TextWrapping="Wrap"/>
            </StackPanel>
          </Grid>
        </Grid>
      </Grid>
      <AppBar x:Name="GameAppBar"  Height="88" VerticalAlignment="Bottom">
        <Grid>
          <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*"/>
            <ColumnDefinition Width="Auto"/>
          </Grid.ColumnDefinitions>
          <StackPanel Grid.Column="0" Orientation="Horizontal" HorizontalAlignment="Left" VerticalAlignment="Top">
            <Button x:Name="Reset" Tag="Reset" Style="{StaticResource ResetButtonStyle}" Click="OnResetButtonClicked"/>
          </StackPanel>
          <StackPanel Grid.Column="1" Orientation="Horizontal" HorizontalAlignment="Right" VerticalAlignment="Top">
            <Button x:Name="Pause" Tag="Pause" Style="{StaticResource PauseButtonStyle}" Click="OnPauseButtonClicked"/>
            <Button x:Name="Play"  Tag="Play"  Style="{StaticResource PlayButtonStyle}"  Click="OnPlayButtonClicked"/>
          </StackPanel>
        </Grid>
      </AppBar>
    </Grid>
  </UserControl>
</SwapChainBackgroundPanel>
```

MainPage.xaml.h

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

#include "MainPage.g.h"

namespace Simple3DGameXaml
{
    ref class App;

    public enum class GameInfoOverlayCommand
    {
        None,
        TapToContinue,
        PleaseWait,
        PlayAgain,
    };

    public ref class MainPage sealed
    {
    public:
        MainPage(App^ app);

        void SetGameStats(int maxLevel, int hitCount, int shotCount);
        void SetGameOver(bool win, int maxLevel, int hitCount, int shotCount, int highScore);
        void SetLevelStart(int level, Platform::String^ objective, float timeLimit, float bonusTime);
        void SetPause(int level, int hitCount, int shotCount, float timeRemaining);
        void SetSnapped();
        void HideSnapped();
        void SetAction(GameInfoOverlayCommand action);
        void HideGameInfoOverlay();
        void ShowGameInfoOverlay();

    protected:
        void OnPauseButtonClicked(Object^ sender, Windows::UI::Xaml::RoutedEventArgs^ e);
        void OnPlayButtonClicked(Object^ sender, Windows::UI::Xaml::RoutedEventArgs^ e);
        void OnResetButtonClicked(Object^ sender, Windows::UI::Xaml::RoutedEventArgs^ e);
        void OnGameInfoOverlayTapped(Object^ sender, Windows::UI::Xaml::Input::TappedRoutedEventArgs^ args);

    private:
        App^ m_app;
    };
}
```

Main.xaml.cpp 代码隐藏

```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "App.xaml.h"
#include "MainPage.xaml.h"

using namespace Simple3DGameXaml;

using namespace Platform;
using namespace Windows::Foundation;
using namespace Windows::Foundation::Collections;
using namespace Windows::Graphics::Display;
using namespace Windows::UI::ViewManagement;
using namespace Windows::UI::Xaml;
using namespace Windows::UI::Xaml::Controls;
using namespace Windows::UI::Xaml::Controls::Primitives;
using namespace Windows::UI::Xaml::Data;
using namespace Windows::UI::Xaml::Input;
using namespace Windows::UI::Xaml::Media;
using namespace Windows::UI::Xaml::Navigation;

//----------------------------------------------------------------------
MainPage::MainPage(App^ app)
{
    InitializeComponent();

    m_app = app;
}
//----------------------------------------------------------------------
void MainPage::HideGameInfoOverlay()
{
    VisualStateManager::GoToState(this->LayoutControl, ref new String(L"NormalState"), true);
}
//----------------------------------------------------------------------
void MainPage::ShowGameInfoOverlay()
{
    VisualStateManager::GoToState(this->LayoutControl, ref new String(L"GameInfoOverlayState"), true);
}
//----------------------------------------------------------------------
void MainPage::SetAction(GameInfoOverlayCommand action)
{
    // Enable only one of the four possible commands at the bottom of the
    // Game Info Overlay.

    PlayAgain->Visibility = ::Visibility::Collapsed;
    PleaseWait->Visibility = ::Visibility::Collapsed;
    TapToContinue->Visibility = ::Visibility::Collapsed;

    switch (action)
    {
    case GameInfoOverlayCommand::PlayAgain:
        PlayAgain->Visibility = ::Visibility::Visible;
        break;
    case GameInfoOverlayCommand::PleaseWait:
        PleaseWait->Visibility = ::Visibility::Visible;
        break;
    case GameInfoOverlayCommand::TapToContinue:
        TapToContinue->Visibility = ::Visibility::Visible;
        break;
    case GameInfoOverlayCommand::None:
        break;
    }
}
//----------------------------------------------------------------------
void MainPage::SetGameStats(
    int maxLevel, 
    int hitCount, 
    int shotCount
    )
{
    GameInfoOverlayTitle->Text = "Game Statistics";
    Stats->Visibility = ::Visibility::Visible;
    LevelStart->Visibility = ::Visibility::Collapsed;
    PauseData->Visibility = ::Visibility::Collapsed;

    static const int bufferLength = 20;
    static char16 wsbuffer[bufferLength];

    int length = swprintf_s(wsbuffer, bufferLength, L"%d", maxLevel);
    LevelsCompleted->Text = ref new Platform::String(wsbuffer, length);

    length = swprintf_s(wsbuffer, bufferLength, L"%d", hitCount);
    TotalPoints->Text = ref new Platform::String(wsbuffer, length);

    length = swprintf_s(wsbuffer, bufferLength, L"%d", shotCount);
    TotalShots->Text = ref new Platform::String(wsbuffer, length);

    // High Score is not used for showing Game Statistics
    HighScoreTitle->Visibility = ::Visibility::Collapsed;
    HighScoreData->Visibility  = ::Visibility::Collapsed;
}
//----------------------------------------------------------------------
void MainPage::SetGameOver(
    bool win, 
    int maxLevel, 
    int hitCount, 
    int shotCount, 
    int highScore
    )
{
    if (win)
    {
        GameInfoOverlayTitle->Text = "You Won!";
    }
    else
    {
        GameInfoOverlayTitle->Text = "Game Over";
    }
    Stats->Visibility = ::Visibility::Visible;
    LevelStart->Visibility = ::Visibility::Collapsed;
    PauseData->Visibility = ::Visibility::Collapsed;

    static const int bufferLength = 20;
    static char16 wsbuffer[bufferLength];

    int length = swprintf_s(wsbuffer, bufferLength, L"%d", maxLevel);
    LevelsCompleted->Text = ref new Platform::String(wsbuffer, length);

    length = swprintf_s(wsbuffer, bufferLength, L"%d", hitCount);
    TotalPoints->Text = ref new Platform::String(wsbuffer, length);

    length = swprintf_s(wsbuffer, bufferLength, L"%d", shotCount);
    TotalShots->Text = ref new Platform::String(wsbuffer, length);

    // Show High Score
    HighScoreTitle->Visibility = ::Visibility::Visible;
    HighScoreData->Visibility  = ::Visibility::Visible;
    length = swprintf_s(wsbuffer, bufferLength, L"%d", highScore);
    HighScore->Text = ref new Platform::String(wsbuffer, length);
}
//----------------------------------------------------------------------
void MainPage::SetLevelStart(
    int level, 
    Platform::String^ objective, 
    float timeLimit, 
    float bonusTime
    )
{
    static const int bufferLength = 20;
    static char16 wsbuffer[bufferLength];

    int length = swprintf_s(wsbuffer, bufferLength, L"Level %d", level);
    GameInfoOverlayTitle->Text = ref new Platform::String(wsbuffer, length);

    Stats->Visibility = ::Visibility::Collapsed;
    LevelStart->Visibility = ::Visibility::Visible;
    PauseData->Visibility = ::Visibility::Collapsed;

    Objective->Text = objective;

    length = swprintf_s(wsbuffer, bufferLength, L"%6.1f sec", timeLimit);
    TimeLimit->Text = ref new Platform::String(wsbuffer, length);

    if (bonusTime > 0.0)
    {
        BonusTimeTitle->Visibility = ::Visibility::Visible;
        BonusTimeData->Visibility  = ::Visibility::Visible;
        length = swprintf_s(wsbuffer, bufferLength, L"%6.1f sec", bonusTime);
        BonusTime->Text = ref new Platform::String(wsbuffer, length);
    }
    else
    {
        BonusTimeTitle->Visibility = ::Visibility::Collapsed;
        BonusTimeData->Visibility  = ::Visibility::Collapsed;  
    }
}
//----------------------------------------------------------------------
void MainPage::SetPause(int level, int hitCount, int shotCount, float timeRemaining)
{
    GameInfoOverlayTitle->Text = "Paused";
    Stats->Visibility = ::Visibility::Collapsed;
    LevelStart->Visibility = ::Visibility::Collapsed;
    PauseData->Visibility = ::Visibility::Visible;

    static const int bufferLength = 20;
    static char16 wsbuffer[bufferLength];

    int length = swprintf_s(wsbuffer, bufferLength, L"%d", level);
    PauseLevel->Text = ref new Platform::String(wsbuffer, length);

    length = swprintf_s(wsbuffer, bufferLength, L"%d", hitCount);
    PauseHits->Text = ref new Platform::String(wsbuffer, length);

    length = swprintf_s(wsbuffer, bufferLength, L"%d", shotCount);
    PauseShots->Text = ref new Platform::String(wsbuffer, length);

    length = swprintf_s(wsbuffer, bufferLength, L"%6.1f sec", timeRemaining);
    PauseTimeRemaining->Text = ref new Platform::String(wsbuffer, length);
}
//----------------------------------------------------------------------
void MainPage::SetSnapped()
{
    VisualStateManager::GoToState(this->LayoutControl, ref new String(L"SnappedState"), true);
}
//----------------------------------------------------------------------
void MainPage::HideSnapped()
{
    VisualStateManager::GoToState(this->LayoutControl, ref new String(L"UnsnappedState"), true);
}
//----------------------------------------------------------------------
void MainPage::OnGameInfoOverlayTapped(Object^ sender, TappedRoutedEventArgs^ args)
{
    m_app->PressComplete();
}
//----------------------------------------------------------------------
void MainPage::OnPauseButtonClicked(Object^ sender, RoutedEventArgs^ args)
{
    m_app->PauseRequested();
}
//----------------------------------------------------------------------
void MainPage::OnPlayButtonClicked(Object^ sender, RoutedEventArgs^ args)
{
    m_app->PressComplete();
}
//----------------------------------------------------------------------
void MainPage::OnResetButtonClicked(Object^ sender, RoutedEventArgs^ args)
{
    m_app->ResetGame();
}
//----------------------------------------------------------------------
```

要下载为覆盖层使用 XAML 的示例游戏的版本，请转到 [Direct3D 射击游戏示例 (XAML)](http://go.microsoft.com/fwlink/p/?linkid=241418)。

 

 




