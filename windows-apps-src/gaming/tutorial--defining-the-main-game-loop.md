---
title: 定义主游戏对象
description: 现在，我们将查看示例游戏的主对象的详细信息，以及它实现的规则如何转换为与游戏世界的交互。
ms.assetid: 6afeef84-39d0-cb78-aa2e-2e42aef936c9
ms.date: 06/24/2020
ms.topic: article
keywords: windows 10, uwp, 游戏, 主对象
ms.localizationpriority: medium
ms.openlocfilehash: 9a6d087be6df93ee6798c29147f7fd1c820bd225
ms.sourcegitcommit: 20969781aca50738792631f4b68326f9171a3980
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/26/2020
ms.locfileid: "85409556"
---
# <a name="define-the-main-game-object"></a>定义主游戏对象

> [!NOTE]
> 本主题是[使用 DirectX 教程系列创建简单通用 Windows 平台（UWP）游戏](tutorial--create-your-first-uwp-directx-game.md)的一部分。 该链接上的主题设置了序列的上下文。

完成示例游戏的基本框架并实现了用于处理高级用户和系统行为的状态机后，您将需要检查将示例游戏变成游戏的规则和机制。 让我们看看示例游戏主对象的详细信息，以及如何将游戏规则转换为与游戏世界的交互。

## <a name="objectives"></a>目标

- 了解如何应用基本开发技术来实现 UWP DirectX 游戏的游戏规则和机制。

## <a name="main-game-object"></a>主要游戏对象

在**Simple3DGameDX**示例游戏中， **Simple3DGame**是主要游戏对象类。 **Simple3DGame**的实例通过**应用：： Load**方法间接构造。

下面是**Simple3DGame**类的一些功能。

- 包含游戏逻辑的实现。
- 包含用于传达这些详细信息的方法。
  - 游戏状态更改为应用框架中定义的状态机。
  - 从应用到游戏对象本身的游戏状态更改。
  - 用于更新游戏 UI （叠加和打印头显示）、动画和物理学（dynamics）的详细信息。
  > [!NOTE]
  > 更新图形由**GameRenderer**类处理，该类包含用于获取和使用该游戏所使用的图形设备资源的方法。 有关详细信息，请参阅[呈现框架 I：要呈现的简介](tutorial--assembling-the-rendering-pipeline.md)。
- 用作定义游戏会话、级别或生存期的数据的容器，具体取决于你在高级别定义游戏的方式。 在这种情况下，游戏状态数据适用于游戏的生存期，在用户启动游戏时初始化一次。

若要查看由此类定义的方法和数据，请参阅下面[的 Simple3DGame 类](#the-simple3dgame-class)。

## <a name="initialize-and-start-the-game"></a>初始化并启动游戏

当玩家启动游戏时，游戏对象必须初始化其状态，创建和添加覆盖层，设置用于跟踪玩家成绩的变量，并实例化将用于构建级别的对象。 在此示例中，此操作在**应用：： Load**中创建**GameMain**实例时完成。

类型为**Simple3DGame**的游戏对象在**GameMain：： GameMain**构造函数中创建。 然后，在**GameMain：： ConstructInBackground 发出的：：** （从**GameMain：： GameMain**调用）期间使用**Simple3DGame：： Initialize**方法对其进行初始化。

### <a name="the-simple3dgameinitialize-method"></a>Simple3DGame：： Initialize 方法

示例游戏在游戏对象中设置这些组件。

- 创建新的音频播放对象。
- 创建游戏的图形基元的数组，包括级别基元、弹药和障碍的数组。
- 创建用于保存游戏状态数据的位置，名为*游戏*，并放入由 [**ApplicationData::Current**](/uwp/api/windows.storage.applicationdata.current) 指定的应用数据设置存储位置。
- 创建游戏计时器和初始游戏内重叠位图。
- 使用一组特定的视图和投影参数创建新的相机。
- 输入设备（控制器）设置为与相机相同的俯仰和偏航，以使玩家在起始控制位置和相机位置之间具有 1 对 1 的对应性。
- 创建玩家对象并设置为活动。 我们使用球体对象检测玩家与墙壁和障碍的距离，并使相机远离可能中断浸入式的位置。
- 创建游戏世界基元。
- 创建圆柱障碍。
- 创建目标（**Face** 对象）并编号。
- 创建弹药球体。
- 创建级别。
- 加载高分。
- 加载任何之前保存的游戏状态。

游戏现在提供 &mdash; 了世界、玩家、障碍、目标和 ammo 球的所有关键组件的实例。 还有各级别的实例，表示上述所有组件的配置以及每个特定级别的行为。 现在让我们看看游戏如何生成级别。

## <a name="build-and-load-game-levels"></a>构建和加载游戏级别

在 `Level[N].h/.cpp` 示例解决方案的**GameLevels**文件夹中找到的文件中，级别构造的大部分繁重的提升都是如此。 因为它侧重于一个非常具体的实现，所以我们不会在此涵盖它们。 重要的是，每个级别的代码都作为一个单独的**级别 [N]** 对象运行。 如果您想要扩展游戏，则可以创建一个级别为 **[N]** 的对象，该对象使用分配的数字作为参数，并随机放置障碍和目标。 或者，你可以让它从资源文件甚至 internet 加载级别的配置数据。

## <a name="define-the-gameplay"></a>定义游戏

此时，我们已经有了开发该游戏所需的所有组件。 已在内存中使用基元构造了级别，并已准备好让播放机开始与进行交互。

最佳游戏会立即做出反应，并提供即时反馈。 这适用于任何类型的游戏，从 twitch、实时的第一人称 shooters 到精心的、基于车的战略游戏。

### <a name="the-simple3dgamerungame-method"></a>Simple3DGame：： RunGame 方法

当游戏级别正在进行时，游戏处于**Dynamics**状态。 

**GameMain：： Update**是每帧更新一次应用程序状态的主要更新循环，如下所示。 如果游戏处于**Dynamics**状态，则 update 循环将调用**Simple3DGame：： RunGame**方法来处理工作。

```cppwinrt
// Updates the application state once per frame.
void GameMain::Update()
{
    // The controller object has its own update loop.
    m_controller->Update();

    switch (m_updateState)
    {
    ...
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
                ...
```
          
**Simple3DGame：： RunGame**用于处理定义游戏循环当前迭代的游戏播放当前状态的数据集。

下面是**Simple3DGame：： RunGame**中的游戏流逻辑。

- 此方法将更新在数秒内倒计时的计时器，直到级别完成，并进行测试以查看级别的时间是否已到期。 这是 &mdash; 当时间用完时游戏的规则之一，如果不是所有目标都已被拍摄，则会对其进行游戏。
- 如果已用完时间，则方法将设置**TimeExpired**游戏状态，并返回到上一代码中的**Update**方法。
- 如果持续时间仍然存在，则会轮询移动外观控制器以便更新到相机位置;具体而言，是指从照相机平面（播放机正在寻找的位置）到视图的角度的更新，以及从控制器最后一次轮询后角度已移动的距离。
- 将基于来自移动观看控制器的新数据更新相机。
- 将更新游戏世界中独立于玩家控制的对象的动态或动画和行为。 在此示例游戏中，将调用**Simple3DGame：： UpdateDynamics**方法来更新已激发的 ammo 球体的运动、支柱障碍的动画和目标的移动。 有关详细信息，请参阅[更新游戏世界](#update-the-game-world)。
- 方法将检查是否已满足某个级别的成功完成条件。 如果是，它将为级别完成分数，并检查这是否为最后一个级别（共6个）。 如果它是最后一个级别，则方法返回**GameState：： GameComplete**游戏状态;否则，它将返回**GameState：： LevelComplete**游戏状态。
- 如果该级别未完成，则方法将游戏状态设置为**GameState：： Active**，并返回。

## <a name="update-the-game-world"></a>更新游戏世界

在此示例中，当游戏运行时， **Simple3DGame：： UpdateDynamics**方法从**Simple3DGame：： RunGame**方法（从**GameMain：： Update**调用）中调用，以更新游戏场景中呈现的对象。

诸如**UpdateDynamics**之类的循环会调用用于设置游戏世界的任何方法，与播放机输入无关，以创建沉浸式游戏体验并使其处于活动状态。 这包括需要呈现的图形，并且即使在没有播放机输入的情况下，也可以运行动画循环来提供有关动态世界的信息。 在游戏中，这可能包括风的 swaying、波形 cresting 沿支撑线路、机械吸烟，以及外部庞然大物拉伸和移动。 还包括对象之间的交互，包括玩家范围和游戏世界之间的冲突或者弹药与障碍物和目标之间的冲突。

除非游戏特别暂停，否则游戏循环应继续更新游戏世界;无论是基于游戏逻辑、物理算法还是只是普通随机。

在此示例游戏中，这一原则称为*dynamics*，它包括支柱障碍的上升和下降，并且 ammo 球的运动和物理行为在激发和运动时出现。

### <a name="the-simple3dgameupdatedynamics-method"></a>Simple3DGame：： UpdateDynamics 方法 

此方法处理这四组计算。

- 游戏世界中触发的弹药范围的位置。
- 立柱障碍物的动画。
- 玩家与游戏世界界限的相交处。
- 弹药球体与障碍物、目标、其他弹药球体和游戏世界的碰撞。

障碍的动画发生在**动画 .h 和 .cpp**源代码文件中定义的循环中。 Ammo 和任何冲突的行为由代码中提供的简化物理学算法定义，并由游戏世界的一组全局常量进行参数化，包括重力和材料属性。 这都在游戏世界坐标空间中计算。

### <a name="review-the-flow"></a>查看流

现在我们已更新场景中的所有对象，并计算出冲突，我们需要使用此信息来绘制相应的视觉对象更改。 

在**GameMain：： Update**完成游戏循环的当前迭代后，示例立即调用**GameRenderer：： Render**以获取更新的对象数据，并生成新的场景以呈现给播放机，如下所示。

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
                ...
                // Otherwise, fall through and do normal processing to perform rendering.
            default:
                CoreWindow::GetForCurrentThread().Dispatcher().ProcessEvents(
                    CoreProcessEventsOption::ProcessAllIfPresent);
                // GameMain::Update calls Simple3DGame::RunGame. If game is in Dynamics
                // state, uses Simple3DGame::UpdateDynamics to update game world.
                Update();
                // Render is called immediately after the Update loop.
                m_renderer->Render();
                m_deviceResources->Present();
                m_renderNeeded = false;
            }
        }
        else
        {
            CoreWindow::GetForCurrentThread().Dispatcher().ProcessEvents(
                CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
    m_game->OnSuspending();  // Exiting due to window close, so save state.
}
```

## <a name="render-the-game-worlds-graphics"></a>渲染游戏世界的图形

建议经常更新游戏中的图形，理想情况下，主要游戏循环会循环访问。 循环循环访问时，游戏世界的状态将更新，有或没有播放机输入。 这样，计算出的动画和行为就能顺利地显示。 假设我们有一个只在玩家按下按钮时移动的简单水印场景。 这并不现实;一个不错的游戏看起来平滑且流畅。

再次调用**GameMain：： Run**中的示例游戏循环，如上文所示。 如果游戏的主窗口可见，且未进行快照或停用，则游戏将继续更新并呈现该更新的结果。 我们下一次检查的**GameRenderer：： Render**方法呈现该状态的表示形式。 这是在调用**GameMain：： update**后立即完成的，其中包括**Simple3DGame：： RunGame**以更新状态，如前一部分中所述。

**GameRenderer：： Render**绘制三维世界的投影，然后在其顶部绘制 Direct2D 叠加。 在完成之后，它将使用合并的缓冲区提供最后的交换链进行显示。

> [!NOTE]
> 对于示例游戏的 Direct2D 叠加有两个状态 &mdash; ，游戏显示了包含 "暂停" 菜单的位图的游戏信息覆盖，另一个游戏显示了十字线和触摸屏移动外观控制器的矩形。 得分文本在两个状态中绘制。 有关详细信息，请参阅[呈现框架 I：呈现简介](tutorial--assembling-the-rendering-pipeline.md)。

### <a name="the-gamerendererrender-method"></a>GameRenderer：： Render 方法

```cppwinrt
void GameRenderer::Render()
{
    bool stereoEnabled{ m_deviceResources->GetStereoState() };

    auto d3dContext{ m_deviceResources->GetD3DDeviceContext() };
    auto d2dContext{ m_deviceResources->GetD2DDeviceContext() };

    ...
        if (m_game != nullptr && m_gameResourcesLoaded && m_levelResourcesLoaded)
        {
            // This section is only used after the game state has been initialized and all device
            // resources needed for the game have been created and associated with the game objects.
            ...
            for (auto&& object : m_game->RenderObjects())
            {
                object->Render(d3dContext, m_constantBufferChangesEveryPrim.get());
            }
        }

        d3dContext->BeginEventInt(L"D2D BeginDraw", 1);
        d2dContext->BeginDraw();

        // To handle the swapchain being pre-rotated, set the D2D transformation to include it.
        d2dContext->SetTransform(m_deviceResources->GetOrientationTransform2D());

        if (m_game != nullptr && m_gameResourcesLoaded)
        {
            // This is only used after the game state has been initialized.
            m_gameHud.Render(m_game);
        }

        if (m_gameInfoOverlay.Visible())
        {
            d2dContext->DrawBitmap(
                m_gameInfoOverlay.Bitmap(),
                m_gameInfoOverlayRect
                );
        }
        ...
    }
}
```

## <a name="the-simple3dgame-class"></a>Simple3DGame 类

这些是由**Simple3DGame**类定义的方法和数据成员。

### <a name="member-functions"></a>成员函数

**Simple3DGame**定义的公共成员函数包括下面的函数。

- **初始化**。 设置全局变量的起始值并初始化游戏对象。 这将在 "[初始化和启动游戏"](#initialize-and-start-the-game)部分中介绍。
- **LoadGame**。 初始化新级别并开始加载它。
- **LoadLevelAsync**。 用于初始化级别的协同程序，然后调用呈现器上的另一个协同程序以加载特定于设备的级别资源。 该方法在单独的线程中运行；因此，只可从该线程调用 [**ID3D11Device**](/windows/desktop/api/d3d11/nn-d3d11-id3d11device) 方法（相对于 [**ID3D11DeviceContext**](/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext) 方法）。 任何设备上下文方法都在 **FinalizeLoadLevel** 方法中调用。 如果你不熟悉异步编程，请参阅[c + +/WinRT 的并发和异步操作](/windows/uwp/cpp-and-winrt-apis/concurrency)。
- **FinalizeLoadLevel**。 完成需要在主线程上进行的关卡加载所需的任何工作。 这包括对 Direct3D 11 设备上下文 ([**ID3D11DeviceContext**](/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext)) 方法的任何调用。
- **StartLevel**。 启动新级别的游戏。
- **PauseGame**。 暂停游戏。
- **RunGame**。 运行游戏循环的迭代。 如果游戏状态为 **Active**，则游戏循环的每次迭代都将从 **App::Update** 中调用它一次。
- **OnSuspending** 和 **OnResuming**。 分别暂停/继续游戏音频。

下面是私有成员函数。

- **LoadSavedState** 和 **SaveState**。 分别加载/保存游戏的当前状态。
- **LoadHighScore**和**SaveHighScore**。 分别在游戏中加载/保存高分。
- **InitializeAmmo**。 在每一轮开始时，将用作弹药的各范围对象的状态重置回其初始状态。
- **UpdateDynamics**。 这是一个重要的方法，因为它会根据固定动画例程、物理学和控件输入更新所有游戏对象。 这是定义游戏的交互性的核心。 这会在 "[更新游戏"](#update-the-game-world)一节中介绍。

其他公共方法是属性访问器，可将游戏和特定于覆盖的信息返回到应用框架以便显示。

### <a name="data-members"></a>数据成员

游戏循环运行时，将更新这些对象。

- **MoveLookController**对象。 表示播放机输入。 有关详细信息，请参阅[添加控件](tutorial--adding-controls.md)。
- **GameRenderer**对象。 表示一个 Direct3D 11 呈现器，该呈现器处理所有设备特定的对象及其呈现。 有关详细信息，请参阅[渲染框架 I](tutorial--assembling-the-rendering-pipeline.md)。
- **音频**对象。 控制游戏的音频播放。 有关详细信息，请参阅[添加声音](tutorial--adding-sound.md)。

游戏变量的其余部分包含基元的列表及其各自的游戏数量，游戏播放特定的数据和约束。

## <a name="next-steps"></a>后续步骤

我们尚未讨论实际的渲染引擎 &mdash; 如何在屏幕上转换对更新的基元的**渲染**方法的调用。 以下两部分介绍了这两个方面 &mdash; [：呈现框架 I：渲染](tutorial--assembling-the-rendering-pipeline.md)和[渲染框架 II：游戏渲染](tutorial-game-rendering.md)。 如果对播放机控件更新游戏状态的方式更感兴趣，请参阅[添加控件](tutorial--adding-controls.md)。
