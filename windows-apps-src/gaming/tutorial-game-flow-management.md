---
title: 游戏流管理
description: 了解如何初始化游戏状态、处理事件和设置游戏更新循环。
ms.assetid: 6c33bf09-b46a-4bb5-8a59-ca83ce257eb3
ms.date: 10/24/2017
ms.topic: article
keywords: windows 10, uwp, 游戏, directx
ms.localizationpriority: medium
ms.openlocfilehash: c6d13b848a9e5d2dfc145431f732187c35c46ab6
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2018
ms.locfileid: "8712153"
---
# <a name="game-flow-management"></a>游戏流管理

游戏现在具有一个窗口，已注册一些事件处理程序并异步加载资产。 本部分介绍游戏状态的使用、如何管理特定的关键游戏状态以及如何创建游戏引擎的更新循环。 然后，我们将了解用户界面流，最后了解 UWP 游戏所需的事件处理程序和事件的详细信息。

>[!Note]
>如果你尚未下载适用于此示例的最新游戏代码，请转到 [Direct3D 游戏示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX)。 此示例是大型 UWP 功能示例集合的一部分。 有关如何下载示例的说明，请参阅[从 GitHub 获取 UWP 示例](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples)。

## <a name="game-states-used-to-manage-game-flow"></a>用于管理游戏流的游戏状态

我们使用游戏状态管理游戏流。 你的游戏可以有任意数量的可能状态，因为用户可以随时从暂停状态恢复 UWP 游戏应用。

对于此游戏示例，它在启动时可能处于以下三种状态之一：
* 游戏循环正在运行并处于某个级别的中间。
* 由于游戏刚刚完成，游戏循环未运行。 （设置了高分）
* 没有启动游戏或者游戏在两个级别之间。 （高分为 0）

你可以根据自己的游戏需要定义所需的状态数量。 再次提醒，UWP 游戏可能随时终止，在恢复时，玩家希望游戏像从来没有停止过一样运行。

## <a name="game-states-initialization"></a>游戏状态初始化

在游戏初始化中，你不只关注游戏的冷启动，还要在游戏暂停或终止后重启。 示例游戏始终保存游戏状态，这样可以使它显示为始终在运行。 

在暂停状态中，游戏运行暂停，但游戏的资源仍然在内存中。 

同样，恢复事件是为了确保该示例游戏拾取其上次暂停或终止的位置。 当示例游戏在终止之后重新启动时，它将正常启动，然后确定上次的已知状态，以便玩家可以立即继续玩游戏。

根据游戏的状态，为玩家提供不同的选项。 

* 如果游戏在级别中间恢复，游戏将显示为暂停，覆盖层将提供一个继续选项。
* 如果游戏恢复到游戏已经完成的状态，它将显示高分和一个玩新游戏的选项。
* 最后，如果游戏在某个级别开始之前恢复，则覆盖层为用户提供一个开始选项。

该游戏示例不区分游戏是在冷启动（即首次启动且无暂停事件）或是从暂停状态恢复。 这是任何 UWP 应用的正确设计。

在此示例中，游戏状态的初始化发生在 [__GameMain::InitializeGameState__](#gamemaininitializegamestate-method) 中。

这是一个帮助你将该流程可视化的流程图，它包含初始化和更新循环。

* 初始化从你检查当前游戏状态的__开始__节点开始。 有关游戏代码，请转到 [__GameMain::InitializeGameState__](#gamemaininitializegamestate-method)。
* 有关更新循环的详细信息，请转到[更新游戏引擎](#update-game-engine)。 有关游戏代码，请转到 [__App::Update__](#appupdate-method)。

![游戏的主状态机](images/simple-dx-game-flow-statemachine.png)

### <a name="gamemaininitializegamestate-method"></a>GameMain::InitializeGameState 方法

__InitializeGameState__ 方法从 [__GameMain__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameMain.cpp#L32-L131) 构造函数类调用，调用时间为在 [__App::Load__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/App.cpp#L115-L123) 方法中创建 __GameMain__ 类对象时。

```cpp

GameMain::GameMain(...)
{
    m_deviceResources->RegisterDeviceNotify(this);
    ...

    create_task([this]()
    {
        ...

    }).then([this]()
    {
        // The finalize code needs to run in the same thread context
        // as the m_renderer object was created because the D3D device context
        // can ONLY be accessed on a single thread.
        m_renderer->FinalizeCreateGameDeviceResources();

        InitializeGameState(); //Initialization of game states occurs here.
        
        ...
    
    }, task_continuation_context::use_current()).then([this]()
    {
        ...
        
    }, task_continuation_context::use_current());
}

```

```cpp

void GameMain::InitializeGameState()
{
    // Set up the initial state machine for handling Game playing state.
    if (m_game->GameActive() && m_game->LevelActive())
    {
        // The last time the game terminated it was in the middle of a level.
        // We are waiting for the user to continue the game.
        //...
    }
    else if (!m_game->GameActive() && (m_game->HighScore().totalHits > 0))
    {
        // The last time the game terminated the game had been completed.
        // Show the high score.
        // We are waiting for the user to acknowledge the high score and start a new game.
        // The level resources for the first level will be loaded later.
        //...
    }
    else
    {
        // This is either the first time the game has run or
        // the last time the game terminated the level was completed.
        // We are waiting for the user to begin the next level.
        m_updateState = UpdateEngineState::WaitingForResources;
        m_pressResult = PressResultState::PlayLevel;
        SetGameInfoOverlay(GameInfoOverlayState::LevelStart);
        m_uiControl->SetAction(GameInfoOverlayCommand::PleaseWait);
    }
    m_uiControl->ShowGameInfoOverlay();
}

```

## <a name="update-game-engine"></a>更新游戏引擎

它在 [__App::Run__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/App.cpp#L127-L130) 方法中调用 [__GameMain::Run__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameMain.cpp#L143-L202)。 在此方法中，示例实现了一个基本状态机，用于处理玩家可以采取的所有主要操作。 此状态机的最高级别处理加载游戏、玩特定关卡，或者在游戏暂停之后继续某个关卡（通过系统或玩家）。

在该游戏示例中，游戏可以处于以下 3 个主要状态 (__UpdateEngineState__)：

1. __Waiting for resources__：游戏循环正在执行，在资源（具体为图形资源）可用之前无法转换。 用于加载资源的异步任务完成后，它将状态更新为 __ResourcesLoaded__。 当关卡正在从磁盘、游戏服务器或云后端加载新资源时，通常会在关卡之间发生此状态。 在该游戏示例中，将模拟此行为，因为该示例此时不需要任何附加的每关卡资源。
2. __Waiting for press__：游戏循环正在执行，正在等待特定的用户输入。 此输入是玩家加载游戏、开始某个级别或继续某个级别的操作。 此示例代码引用这些子状态作为 __PressResultState__ 枚举值。
3. 处于 __Dynamics__ 状态：在用户玩游戏过程中游戏循环正在运行。 在用户游戏过程中，游戏检查其可以转换的 3 个条件： 
    * __TimeExpired__：某个级别的设定时间到期
    * __LevelComplete__：玩家完成某个级别 
    * __GameComplete__：玩家完成所有级别

你的游戏只是一个含有多个较小状态机的状态机。 对于每个特定的状态，必须使用一个非常具体的条件进行定义。 从一种状态到另一状态的转换必须基于离散用户的输入或系统操作（如图形资源加载）。 规划游戏时，考虑绘制整个游戏流，以确保涵盖用户或系统可能采取的所有操作。 游戏可能非常复杂，因此状态机是一个强大的工具，它有助于直观显示这一复杂性，并使之非常易于管理。

让我们来看一看下方的更新循环代码。

### <a name="appupdate-method"></a>App::Update 方法

用于更新游戏引擎的状态机的结构

```cpp
void GameMain::Update()
{
    m_controller->Update(); //the controller instance has its own update loop.

    switch (m_updateState)
    {
    case UpdateEngineState::WaitingForResources:
        //...
        break;

    case UpdateEngineState::ResourcesLoaded:
        //...
        break;

    case UpdateEngineState::WaitingForPress:
        if (m_controller->IsPressComplete())
        {
            //...
        }
        break;

    case UpdateEngineState::Dynamics:
        if (m_controller->IsPauseRequested())
        {
            //...
        }
        else
        {
            GameState runState = m_game->RunGame(); //when the player is playing, the work is handled by this Simple3DGame::RunGame method.
            switch (runState)
            {
            case GameState::TimeExpired:
                //...
                break;

            case GameState::LevelComplete:
                //...
                break;

            case GameState::GameComplete:
                //...
                break;
            }
        }

        if (m_updateState == UpdateEngineState::WaitingForPress)
        {
            // Transitioning state, so enable waiting for the press event
            m_controller->WaitForPress(m_renderer->GameInfoOverlayUpperLeft(), m_renderer->GameInfoOverlayLowerRight());
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

## <a name="update-user-interface"></a>更新用户界面

我们需要通知玩家系统的状态，并允许根据玩家的操作和定义游戏的规则更改游戏状态。 许多游戏（包括此游戏示例）通常使用用户界面 (UI) 元素来向玩家显示此信息。 UI 包含游戏状态的表示形式，以及其他特定于游戏的信息，如得分、弹药或剩余的机会数。 UI 也称为覆盖层，因为它独立于主图形管道呈现，并放在 3D 投影的顶部。 一些 UI 信息还作为提醒显示 (HUD) 呈现，使用户的视线无需离开主游戏区也可获取这些信息。 在该示例游戏中，我们使用 Direct2D API 创建此覆盖层。 我们还可以使用 XAML 创建此覆盖层，这在[扩展游戏示例](tutorial-resources.md)中讨论。

用户界面有两个组件：

-   HUD，包含得分和有关游戏执行的当前状态信息。
-   暂停位图，这是一个在游戏的暂停状态期间由文本覆盖的黑色矩形。 这是游戏覆盖层。 我们将在[添加用户界面](tutorial--adding-a-user-interface.md)中进一步讨论。

毫不奇怪，覆盖层也有一个状态机。 覆盖层可以显示一个级别的开始或游戏结束消息。 它实际上是一块画布，在游戏暂停时，将我们显示的有关游戏状态的任何信息输出给玩家。

覆盖呈现可以是六个屏幕中的其中一个，具体取决于游戏的状态： 
1. 游戏开始时的资源加载屏幕
2. 游戏统计播放屏幕
3. 级别开始消息屏幕
4. 超时前完成所有级别时的游戏结束屏幕
5. 超时后的游戏结束屏幕
6. 暂停菜单屏幕

将用户界面与游戏的图形管道分开可以独立于游戏的图形呈现引擎来使用用户界面，并显著降低游戏代码的复杂性。

下面是游戏示例构造覆盖层状态机的方式。

```cpp
void GameMain::SetGameInfoOverlay(GameInfoOverlayState state)
{
    m_gameInfoOverlayState = state;
    switch (state)
    {
    case GameInfoOverlayState::Loading:
        m_uiControl->SetGameLoading(m_loadingCount);
        break;

    case GameInfoOverlayState::GameStats:
        //...
        break;

    case GameInfoOverlayState::LevelStart:
        //...
        break;

    case GameInfoOverlayState::GameOverCompleted:
        //...
        break;

    case GameInfoOverlayState::GameOverExpired:
        //...
        break;

    case GameInfoOverlayState::Pause:
        //...
        break;
    }
}
```

## <a name="events-handling"></a>事件处理

我们的示例代码在 App.cpp 中为 **Initialize**、**SetWindow** 和 **Load** 中的特定事件注册了大量处理程序。 在我们可以添加游戏构成或开始图形开发前，这些是需要运行的重要事件。 这些事件是获得正确的 UWP 应用体验的基础。 由于 UWP 应用可随时激活、停用、调整大小、贴靠、取消贴靠、暂停或恢复，因此游戏必须尽快注册这些事件，并且处理这些事件的方式应保证玩家获得流畅和可预期的体验。

下面是此示例中的事件处理程序，以及它们处理的事件。

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
<td align="left">处理 <a href="https://msdn.microsoft.com/library/windows/apps/br225018"><strong>CoreApplicationView::Activated</strong></a>。 游戏应用已经进入前台，因此将激活主窗口。</td>
</tr>
<tr class="even">
<td align="left">OnDpiChanged</td>
<td align="left">处理 <a href="https://docs.microsoft.com/uwp/api/windows.graphics.display.displayinformation#Windows_Graphics_Display_DisplayInformation_DpiChanged"><strong>Graphics::Display::DisplayInformation::DpiChanged</strong></a>。 显示器的 DPI 已更改，且游戏相应地调整其资源。
<div class="alert">
<strong>注意</strong>[<strong>CoreWindow</strong>] (https://msdn.microsoft.com/library/windows/desktop/hh404559)坐标位于[direct2d](https://msdn.microsoft.com/library/windows/desktop/dd370987)的 Dip （与设备无关像素）。 因此，要正确显示任何 2D 资产或基元，必须通知 Direct2D 关于 DPI 的更改。
</div>
<div>
</div></td>
</tr>
<tr class="odd">
<td align="left">OnOrientationChanged</td>
<td align="left">处理 <a href="https://docs.microsoft.com/uwp/api/windows.graphics.display.displayinformation#Windows_Graphics_Display_DisplayInformation_OrientationChanged"><strong>Graphics::Display::DisplayInformation::OrientationChanged</strong></a>。 显示方向更改，并且需要更新呈现。</td>
</tr>
<tr class="even">
<td align="left">OnDisplayContentsInvalidated</td>
<td align="left">处理 <a href="https://docs.microsoft.com/uwp/api/windows.graphics.display.displayinformation#Windows_Graphics_Display_DisplayInformation_DisplayContentsInvalidated"><strong>Graphics::Display::DisplayInformation::DisplayContentsInvalidated</strong></a>。 需要重新绘制显示，且需要再次呈现你的游戏。</td>
</tr>
<tr class="odd">
<td align="left">OnResuming</td>
<td align="left">处理 <a href="https://msdn.microsoft.com/library/windows/apps/br205859"><strong>CoreApplication::Resuming</strong></a>。 游戏应用从暂停状态恢复游戏。</td>
</tr>
<tr class="even">
<td align="left">OnSuspending</td>
<td align="left">处理 <a href="https://msdn.microsoft.com/library/windows/apps/br205860"><strong>CoreApplication::Suspending</strong></a>。 游戏应用将其状态保存到磁盘。 它有 5 秒时间将状态保存到存储。</td>
</tr>
<tr class="odd">
<td align="left">OnVisibilityChanged</td>
<td align="left">处理 <a href="https://msdn.microsoft.com/library/windows/apps/hh701591"><strong>CoreWindow::VisibilityChanged</strong></a>。 游戏应用的可见性已更改，变为可见或因另一个应用变为可见而致使其不可见。</td>
</tr>
<tr class="even">
<td align="left">OnWindowActivationChanged</td>
<td align="left">处理 <a href="https://msdn.microsoft.com/library/windows/apps/br208255"><strong>CoreWindow::Activated</strong></a>。 游戏应用的主窗口已停用或激活，因此它必须删除焦点并暂停游戏，或者重新获得焦点。 在这两种情况下，覆盖层都指示游戏已暂停。</td>
</tr>
<tr class="odd">
<td align="left">OnWindowClosed</td>
<td align="left">处理 <a href="https://msdn.microsoft.com/library/windows/apps/br208261"><strong>CoreWindow::Closed</strong></a>。 游戏应用关闭主窗口并暂停游戏。</td>
</tr>
<tr class="even">
<td align="left">OnWindowSizeChanged</td>
<td align="left">处理 <a href="https://msdn.microsoft.com/library/windows/apps/br208283"><strong>CoreWindow::SizeChanged</strong></a>。 游戏应用重新分配图形资源和覆盖层以适应大小更改，然后更新呈现器目标。</td>
</tr>
</tbody>
</table>

## <a name="next-steps"></a>后续步骤

在本主题中，我们介绍了如何使用游戏状态管理整个游戏流以及游戏由多个不同的状态机构成。 我们还了解到了如何更新 UI 和管理关键应用事件处理程序。 现在，我们准备深入研究呈现循环、游戏及其构成。
 
你可以按任何顺序浏览构成此游戏的其他组件：
* [定义主游戏对象](tutorial--defining-the-main-game-loop.md)
* [呈现框架 I：呈现简介](tutorial--assembling-the-rendering-pipeline.md)
* [呈现框架 II：游戏呈现](tutorial-game-rendering.md)
* [添加用户界面](tutorial--adding-a-user-interface.md)
* [添加控件](tutorial--adding-controls.md)
* [添加声音](tutorial--adding-sound.md)