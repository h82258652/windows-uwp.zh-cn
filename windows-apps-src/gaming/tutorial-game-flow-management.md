---
title: 游戏流管理
description: 了解如何初始化游戏状态、处理事件和设置游戏更新循环。
ms.assetid: 6c33bf09-b46a-4bb5-8a59-ca83ce257eb3
ms.date: 06/24/2020
ms.topic: article
keywords: windows 10, uwp, 游戏, directx
ms.localizationpriority: medium
ms.openlocfilehash: 181eca743a9ccdc76ebfc1302e8bb04d85a32269
ms.sourcegitcommit: 20969781aca50738792631f4b68326f9171a3980
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/26/2020
ms.locfileid: "85409626"
---
# <a name="game-flow-management"></a>游戏流管理

> [!NOTE]
> 本主题是[使用 DirectX 教程系列创建简单通用 Windows 平台（UWP）游戏](tutorial--create-your-first-uwp-directx-game.md)的一部分。 该链接上的主题设置了序列的上下文。

游戏现在包含一个窗口，已经注册了一些事件处理程序，并以异步方式加载了资源。 本主题说明如何使用游戏状态，如何管理特定的关键游戏状态，以及如何为游戏引擎创建更新循环。 接下来，我们将了解用户界面流，最后了解有关 UWP 游戏所需的事件处理程序的详细信息。

## <a name="game-states-used-to-manage-game-flow"></a>用于管理游戏流的游戏状态

我们使用游戏状态管理游戏流。

当**Simple3DGameDX**示例游戏在计算机上首次运行时，它处于 "未启动任何游戏" 状态。 游戏后续运行时，可以处于这些状态中的任何一种。

- 未启动任何游戏，或游戏在级别之间（高分数为零）。
- 游戏循环正在运行，位于级别中间。
- 游戏循环未运行，因为游戏已完成（高分数的值为非零值）。

你的游戏可以拥有任意数量的所需状态。 但请记住，可以随时终止。 恢复后，用户希望它以终止时的状态恢复。

## <a name="game-state-management"></a>游戏状态管理

因此，在游戏初始化期间，你需要支持冷启动游戏，以及在停止游戏后继续游戏。 **Simple3DGameDX**示例始终保存其游戏状态，以使其不会停止。

为响应挂起事件，游戏已挂起，但该游戏的资源仍在内存中。 同样，将对 resume 事件进行处理，以确保示例游戏在挂起或终止时所处的状态中选取。 根据游戏的状态，为玩家提供不同的选项。 

- 如果游戏恢复了中间级别，则它会显示暂停，覆盖区会提供继续操作的选项。
- 如果游戏在游戏完成状态中恢复，则会显示 "高分" 和一个用于玩新游戏的选项。
- 最后，如果在启动某个级别之前恢复游戏，则覆盖将向用户显示一个启动选项。

该示例游戏并不区分游戏是否正在冷启动、第一次启动而没有挂起事件，或从挂起状态恢复。 这是任何 UWP 应用的正确设计。

在此示例中，将在[**GameMain：： InitializeGameState**](#the-gamemaininitializegamestate-method)中初始化游戏状态，下一部分将显示该方法的轮廓。

下面是一个流程图，用于帮助您将流可视化。 它包含初始化和更新循环。

- 初始化从你检查当前游戏状态的**开始**节点开始。 有关游戏代码，请参阅下一节中的[**GameMain：： InitializeGameState**](#the-gamemaininitializegamestate-method) 。
* 有关更新循环的详细信息，请转到[更新游戏引擎](#update-game-engine)。 对于游戏代码，请参阅[**GameMain：： Update**](#the-gamemainupdate-method)。

![游戏的主状态机](images/simple-dx-game-flow-statemachine.png)

### <a name="the-gamemaininitializegamestate-method"></a>GameMain：： InitializeGameState 方法

**GameMain：： InitializeGameState**方法是通过**GameMain**类的构造函数间接调用的，这是在**应用：： Load**内生成**GameMain**实例的结果。

```cppwinrt
GameMain::GameMain(std::shared_ptr<DX::DeviceResources> const& deviceResources) : ...
{
    m_deviceResources->RegisterDeviceNotify(this);
    ...
    ConstructInBackground();
}

winrt::fire_and_forget GameMain::ConstructInBackground()
{
    ...
    m_renderer->FinalizeCreateGameDeviceResources();

    InitializeGameState();
    ...
}

void GameMain::InitializeGameState()
{
    // Set up the initial state machine for handling Game playing state.
    if (m_game->GameActive() && m_game->LevelActive())
    {
        // The last time the game terminated it was in the middle
        // of a level.
        // We are waiting for the user to continue the game.
        ...
    }
    else if (!m_game->GameActive() && (m_game->HighScore().totalHits > 0))
    {
        // The last time the game terminated the game had been completed.
        // Show the high score.
        // We are waiting for the user to acknowledge the high score and start a new game.
        // The level resources for the first level will be loaded later.
        ...
    }
    else
    {
        // This is either the first time the game has run or
        // the last time the game terminated the level was completed.
        // We are waiting for the user to begin the next level.
        ...
    }
    m_uiControl->ShowGameInfoOverlay();
}
```

## <a name="update-game-engine"></a>更新游戏引擎

**App：： run**方法会调用**GameMain：： run**。 在**GameMain：： Run**中是用于处理用户可以采取的所有主要操作的基本状态机。 此状态机的最高级别涉及在游戏暂停（由系统或用户执行）后加载游戏、播放特定级别或继续执行某个级别。

在示例游戏中，游戏可以处于3个主要状态（由**UpdateEngineState**枚举表示）。

1. **UpdateEngineState：： WaitingForResources**。 游戏循环正在执行，在资源（特别是图形资源）可用之前无法转换。 异步资源加载任务完成后，我们会将状态更新为**UpdateEngineState：： ResourcesLoaded**。 当级别从磁盘、游戏服务器或云后端加载新资源时，通常会发生这种情况。 在示例游戏中，我们将模拟此行为，因为该示例在当时不需要任何其他的每个级别的资源。
2. **UpdateEngineState：： WaitingForPress**。 游戏循环正在执行，正在等待特定的用户输入。 此输入是一个播放机操作，用于加载游戏、启动级别或继续执行级别。 示例代码通过**PressResultState**枚举引用这些子状态。
3. **UpdateEngineState：:D ynamics**。 在用户玩游戏过程中游戏循环正在运行。 在用户游戏过程中，游戏检查其可以转换的 3 个条件： 
 - **GameState：： TimeExpired**。 某个级别的时间限制的过期时间。
 - **GameState：： LevelComplete**。 按播放机完成级别。
 - **GameState：： GameComplete**。 由播放机完成所有级别。

游戏只是包含多个较小状态机的状态机。 必须按非常具体的条件定义每个特定状态。 从一个状态到另一个状态的转换必须基于离散用户输入或系统操作（如图形资源加载）。

在计划游戏时，请考虑绘制整个游戏流，以确保你已解决用户或系统可以执行的所有可能操作。 游戏可能非常复杂，因此，状态机是一个功能强大的工具，可帮助您直观显示这一复杂性，并使其更易于管理。

让我们看一下更新循环的代码。

### <a name="the-gamemainupdate-method"></a>GameMain：： Update 方法

这是用于更新游戏引擎的状态机的结构。

```cppwinrt
void GameMain::Update()
{
    // The controller object has its own update loop.
    m_controller->Update(); 

    switch (m_updateState)
    {
    case UpdateEngineState::WaitingForResources:
        ...
        break;

    case UpdateEngineState::ResourcesLoaded:
        ...
        break;

    case UpdateEngineState::WaitingForPress:
        if (m_controller->IsPressComplete())
        {
            ...
        }
        break;

    case UpdateEngineState::Dynamics:
        if (m_controller->IsPauseRequested())
        {
            ...
        }
        else
        {
            // When the player is playing, work is done by Simple3DGame::RunGame.
            GameState runState = m_game->RunGame();
            switch (runState)
            {
            case GameState::TimeExpired:
                ...
                break;

            case GameState::LevelComplete:
                ...
                break;

            case GameState::GameComplete:
                ...
                break;
            }
        }

        if (m_updateState == UpdateEngineState::WaitingForPress)
        {
            // Transitioning state, so enable waiting for the press event.
            m_controller->WaitForPress(
                m_renderer->GameInfoOverlayUpperLeft(),
                m_renderer->GameInfoOverlayLowerRight());
        }
        if (m_updateState == UpdateEngineState::WaitingForResources)
        {
            // Transitioning state, so shut down the input controller
            // until resources are loaded.
            m_controller->Active(false);
        }
        break;
    }
}
```

## <a name="update-the-user-interface"></a>更新用户界面

我们需要通知玩家系统的状态，并允许根据玩家的操作和定义游戏的规则更改游戏状态。 许多游戏（包括此示例游戏）通常使用用户界面（UI）元素向播放机提供此信息。 UI 包含游戏状态的表示形式，以及其他特定于播放的信息，例如评分、ammo 或剩余几率。 UI 也称为覆盖区，因为它与主图形管道分开呈现，并放置在三维投影的顶部。

某些 UI 信息也显示为打印头显示（HUD），允许用户查看该信息，而无需完全离开主要游戏区域。 在示例游戏中，我们使用 Direct2D Api 创建此覆盖区。 此外，我们还可以使用 XAML 创建此覆盖，我们将在[扩展示例游戏](tutorial-resources.md)中介绍。

用户界面有两个组件。

- 包含有关游戏的当前状态的分数和信息的 HUD。
- 暂停位图，这是一个在游戏的暂停状态期间由文本覆盖的黑色矩形。 这是游戏覆盖层。 我们将在[添加用户界面](tutorial--adding-a-user-interface.md)中进一步讨论。

毫不奇怪，覆盖层也有一个状态机。 覆盖层可以显示级别启动或游戏结束消息。 它实际上是一个画布，可以在游戏暂停或挂起时，输出有关游戏状态的任何信息。

呈现的覆盖可以是这六个屏幕中的一种，具体取决于游戏的状态。

1. 游戏开始时的资源加载进度屏幕。
2. 游戏统计信息屏幕。
3. 级别启动消息屏幕。
4. 当所有级别都完成时，如果没有运行的时间，则为屏幕上的游戏。
5. 当时间用完时，游戏结束。
6. 暂停菜单屏幕。

将用户界面与游戏的图形管道分离，使您可以独立于游戏的图形渲染引擎进行处理，并大大降低游戏代码的复杂性。

下面是示例游戏如何构造覆盖的状态机。

```cppwinrt
void GameMain::SetGameInfoOverlay(GameInfoOverlayState state)
{
    m_gameInfoOverlayState = state;
    switch (state)
    {
    case GameInfoOverlayState::Loading:
        m_uiControl->SetGameLoading(m_loadingCount);
        break;

    case GameInfoOverlayState::GameStats:
        ...
        break;

    case GameInfoOverlayState::LevelStart:
        ...
        break;

    case GameInfoOverlayState::GameOverCompleted:
        ...
        break;

    case GameInfoOverlayState::GameOverExpired:
        ...
        break;

    case GameInfoOverlayState::Pause:
        ...
        break;
    }
}
```

## <a name="event-handling"></a>事件处理

如我们在[定义游戏的 UWP 应用框架](tutorial--building-the-games-uwp-app-framework.md)主题中所述，**应用**类的许多视图提供程序方法都注册事件处理程序。 在添加游戏机制或开始图形开发之前，这些方法需要正确处理这些重要事件。

适当处理相关事件是 UWP 应用体验的基础。 因为 UWP 应用可随时激活、停用、调整大小、进行快照、backstack、挂起或恢复，所以游戏必须尽快注册这些事件，并以保持体验平滑且可预测播放机的方式进行处理。

这是此示例中使用的事件处理程序及其处理的事件。

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">事件处理程序</th>
<th align="left">说明</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">OnActivated</td>
<td align="left">处理 <a href="/uwp/api/windows.applicationmodel.core.coreapplicationview.activated"><strong>CoreApplicationView::Activated</strong></a>。 游戏应用已经进入前台，因此将激活主窗口。</td>
</tr>
<tr class="even">
<td align="left">OnDpiChanged</td>
<td align="left">处理 <a href="/uwp/api/windows.graphics.display.displayinformation#Windows_Graphics_Display_DisplayInformation_DpiChanged"><strong>Graphics::Display::DisplayInformation::DpiChanged</strong></a>。 显示器的 DPI 已更改，且游戏相应地调整其资源。
<div class="alert">
<strong>请注意</strong>， <a href="/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgifactory2-createswapchainforcorewindow"><strong>CoreWindow</strong></a>坐标以与设备无关的像素（dip）为<a href="/windows/desktop/Direct2D/direct2d-overview">Direct2D</a>。 因此，要正确显示任何 2D 资产或基元，必须通知 Direct2D 关于 DPI 的更改。
</div>
<div>
</div></td>
</tr>
<tr class="odd">
<td align="left">OnOrientationChanged</td>
<td align="left">处理 <a href="/uwp/api/windows.graphics.display.displayinformation#Windows_Graphics_Display_DisplayInformation_OrientationChanged"><strong>Graphics::Display::DisplayInformation::OrientationChanged</strong></a>。 显示方向更改，并且需要更新呈现。</td>
</tr>
<tr class="even">
<td align="left">OnDisplayContentsInvalidated</td>
<td align="left">处理 <a href="/uwp/api/windows.graphics.display.displayinformation#Windows_Graphics_Display_DisplayInformation_DisplayContentsInvalidated"><strong>Graphics::Display::DisplayInformation::DisplayContentsInvalidated</strong></a>。 需要重新绘制显示，且需要再次呈现你的游戏。</td>
</tr>
<tr class="odd">
<td align="left">OnResuming</td>
<td align="left">处理 <a href="/uwp/api/windows.applicationmodel.core.coreapplication.resuming"><strong>CoreApplication::Resuming</strong></a>。 游戏应用从暂停状态恢复游戏。</td>
</tr>
<tr class="even">
<td align="left">OnSuspending</td>
<td align="left">处理 <a href="/uwp/api/windows.applicationmodel.core.coreapplication.suspending"><strong>CoreApplication::Suspending</strong></a>。 游戏应用将其状态保存到磁盘。 它有 5 秒时间将状态保存到存储。</td>
</tr>
<tr class="odd">
<td align="left">OnVisibilityChanged</td>
<td align="left">处理 <a href="/uwp/api/windows.ui.core.corewindow.visibilitychanged"><strong>CoreWindow::VisibilityChanged</strong></a>。 游戏应用的可见性已更改，变为可见或因另一个应用变为可见而致使其不可见。</td>
</tr>
<tr class="even">
<td align="left">OnWindowActivationChanged</td>
<td align="left">处理 <a href="/uwp/api/windows.ui.core.corewindow.activated"><strong>CoreWindow::Activated</strong></a>。 游戏应用的主窗口已停用或激活，因此它必须删除焦点并暂停游戏，或者重新获得焦点。 在这两种情况下，覆盖层都指示游戏已暂停。</td>
</tr>
<tr class="odd">
<td align="left">OnWindowClosed</td>
<td align="left">处理 <a href="/uwp/api/windows.ui.core.corewindow.closed"><strong>CoreWindow::Closed</strong></a>。 游戏应用关闭主窗口并暂停游戏。</td>
</tr>
<tr class="even">
<td align="left">OnWindowSizeChanged</td>
<td align="left">处理 <a href="/uwp/api/windows.ui.core.corewindow.sizechanged"><strong>CoreWindow::SizeChanged</strong></a>。 游戏应用重新分配图形资源和覆盖层以适应大小更改，然后更新呈现器目标。</td>
</tr>
</tbody>
</table>

## <a name="next-steps"></a>后续步骤

在本主题中，我们已了解如何使用游戏状态管理整体游戏流，以及由多个不同状态机组成的游戏。 我们还了解了如何更新 UI，以及如何管理密钥应用程序事件处理程序。 现在，我们已准备好深入了解呈现循环、游戏及其机制。
 
可以按任何顺序浏览本游戏的其余主题。

- [定义主游戏对象](tutorial--defining-the-main-game-loop.md)
- [呈现框架 I：呈现简介](tutorial--assembling-the-rendering-pipeline.md)
- [呈现框架 II：游戏呈现](tutorial-game-rendering.md)
- [添加用户界面](tutorial--adding-a-user-interface.md)
- [添加控件](tutorial--adding-controls.md)
- [添加声音](tutorial--adding-sound.md)
