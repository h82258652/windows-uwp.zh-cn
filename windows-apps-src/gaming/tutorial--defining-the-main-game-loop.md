---
author: joannaleecy
title: 定义主游戏对象
description: 现在，我们将了解游戏示例主对象的详细信息，以及如何将其实现的规则转换与游戏世界的交互。
ms.assetid: 6afeef84-39d0-cb78-aa2e-2e42aef936c9
ms.author: joanlee
ms.date: 10/24/2017
ms.topic: article
keywords: windows 10, uwp, 游戏, 主对象
ms.localizationpriority: medium
ms.openlocfilehash: b94d7139f35b3a18edd66af9959a0958d0bdcbc1
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/05/2018
ms.locfileid: "6038468"
---
# <a name="define-the-main-game-object"></a>定义主游戏对象

一旦你已介绍示例游戏的基本框架，并实现了处理高级用户和系统行为的状态机，你会想要检查的规则和转变为游戏的游戏示例的机制。 让我们看一下游戏示例主对象的详细信息以及如何转换为游戏世界的交互的游戏的规则。

>[!Note]
>如果你尚未下载适用于此示例的最新游戏代码，请转到 [Direct3D 游戏示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX)。 此示例是大型 UWP 功能示例集合的一部分。 有关如何下载示例的说明，请参阅[从 GitHub 获取 UWP 示例](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples)。

## <a name="objective"></a>目标

了解如何将应用基本开发技巧实现游戏的规则和为 UWP DirectX 游戏的技术。

## <a name="main-game-object"></a>主要游戏对象

在此示例游戏中， __Simple3DGame__是主游戏对象类。 __App:: load__方法中构建__Simple3DGame__对象的实例。

__Simple3DGame__类对象：
* 指定游戏逻辑的实现
* 包含通信的方法：
    * 应用框架中定义的状态机游戏状态的更改。
    * 从应用到游戏对象本身游戏状态的更改。
    * 更新游戏的 UI （覆盖层和提醒显示）、 动画和物理特性 （动态） 的详细信息。

    >[!Note]
    >更新的图形__GameRenderer__类，其中包含要获取并使用游戏使用的图形设备资源的方法来处理。 有关详细信息，请参阅[呈现框架 i： 呈现简介](tutorial--assembling-the-rendering-pipeline.md)。

* 可用作定义游戏会话、 级别的数据的生命周期，具体取决于你的游戏在高级别上的定义方式的容器。 在此情况下，游戏状态并将数据的生命周期的游戏中，初始化一次在用户启动游戏时。

若要查看方法和在此类对象中定义的数据，请转到[Simple3DGame 对象](#simple3dgame-object)。

## <a name="initialize-and-start-the-game"></a>初始化和启动游戏

当玩家启动游戏时，游戏对象必须初始化其状态，创建和添加覆盖层，设置用于跟踪玩家成绩的变量，并实例化将用于构建级别的对象。 在此示例中，完成此操作时[__app:: load__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/App.cpp#L115-L123)中创建新的__GameMain__实例。 

__GameMain__构造函数中创建游戏对象， __Simple3DGame__。 然后，初始化期间的[异步创建__GameMain__构造函数中的任务](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameMain.cpp#L65-L74)使用[__simple3dgame:: initialize__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L54-L250)方法。

### <a name="simple3dgameinitialize-method"></a>Simple3dgame:: initialize 方法

该游戏示例中设置游戏对象中的以下组件：

* 创建新的音频播放对象。
* 创建游戏的图形基元的数组，包括级别基元、弹药和障碍的数组。
* 创建用于保存游戏状态数据的位置，名为*游戏*，并放入由 [**ApplicationData::Current**](https://msdn.microsoft.com/library/windows/apps/br241619) 指定的应用数据设置存储位置。
* 创建游戏计时器和初始游戏内重叠位图。
* 使用一组特定的视图和投影参数创建新的相机。
* 输入设备（控制器）设置为与相机相同的俯仰和偏航，以使玩家在起始控制位置和相机位置之间具有 1 对 1 的对应性。
* 创建玩家对象并设置为活动。 我们使用球体对象以检测玩家对墙和障碍的邻近感应，防止相机获取放入可能打破沉浸的位置。
* 创建游戏世界基元。
* 创建圆柱障碍。
* 创建目标（**Face** 对象）并编号。
* 创建弹药球体。
* 创建级别。
* 加载高分。
* 加载任何之前保存的游戏状态。

游戏现在已有全部关键组件的实例：游戏世界、玩家、障碍物、目标和弹药范围。 还有各级别的实例，表示上述所有组件的配置以及每个特定级别的行为。 现在让我们了解游戏如何构建级别。

## <a name="build-and-load-game-levels"></a>生成并加载游戏关卡

大多数繁重的工作的关卡的示例解决方案__GameLevels__文件夹中找到的__Level.h/.cpp__文件中完成。 因为它侧重于非常具体的实现，我们将不会涵盖它们在此处。 重要的是每个关卡的代码都作为单独的 __LevelN__ 对象运行。 如果你想要扩展游戏，你可以创建一个**级别**对象，用于获取分配的数字作为参数并随机放置障碍物和目标。 或者，你可以让其从源文件甚至 Internet 加载关卡配置数据。

## <a name="define-the-game-play"></a>定义游戏玩法

这时，我们已经有了装配游戏所需的全部组件。 级别已经在内存中从基元构建并准备好供玩家开始与交互。

该最好的游戏立即响应玩家输入，并提供即时反馈。 这适用于任何类型的游戏时，从抽搐动作、 实时思考、 基于轮次的策略游戏的第一人称射击游戏。

### <a name="simple3dgamerungame-method"></a>Simple3DGame::RunGame 方法

当某个关卡时，游戏处于__Dynamics__状态。 

[__Gamemain:: Update__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameMain.cpp#L261-L329)是主要更新循环，可更新一次每帧的应用程序状态，如下所示。 在更新循环中，它将调用[__Simple3DGame::RunGame__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L337-L418)方法来处理工作，如果游戏处于__Dynamics__状态。

```cpp
// Updates the application state once per frame.
void GameMain::Update()
{
    m_controller->Update(); //the controller instance has its own update loop.

    switch (m_updateState)
    {
        //...

    case UpdateEngineState::Dynamics:
        if (m_controller->IsPauseRequested())
        {
            //...
        }
        else
        {
            GameState runState = m_game->RunGame(); //when playing a level, the game is in the Dynamics state. Work is handled by Simple3DGame::RunGame method.
            switch (runState)
            {
                
      //...
```
          
[__Simple3DGame::RunGame__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L337-L418)处理定义游戏循环的当前迭代的玩游戏的当前状态的数据的集。

游戏流逻辑__RunGame__中：
*  此方法更新倒记秒数的计时器直到完成该级别，并进行测试以查看该级别的时间是否已过期。 这是游戏的规则之一： 当超时并不拍摄所有目标时，它是游戏结束。
*  如果时间用尽，此方法将设置 **TimeExpired** 游戏状态，并返回到之前的代码中的 **Update** 方法。
*  如果时间剩余，将轮询移动观看控制器以获取相机位置的更新；尤其是来自相机平面（玩家正在看的位置）视图正常投影的角度更新，以及该角度从上一次轮询控制器开始移动的距离。
*  将基于来自移动观看控制器的新数据更新相机。
*  将更新游戏世界中独立于玩家控制的对象的动态或动画和行为。 在此游戏示例中，调用[__UpdateDynamics()__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L436-L856)方法来更新已触发的弹药球体动作、 立柱障碍物的动画以及目标的移动。 有关详细信息，请参阅[更新游戏世界](#update-the-game-world)。
*  此方法将进行检查，以查看是否满足成功完成级别的条件。 如果是这样，它将落实该关卡的分数，并查看这是否为最后一个关卡（共 6 个）。 如果它是最终关卡，此方法将返回 **GameComplete** 游戏状态；否则，它将返回 __LevelComplete__ 游戏状态。
*  如果未完成该关卡，此方法将游戏状态设置为 __Active__ 并返回。

## <a name="update-the-game-world"></a>更新游戏世界

在此示例中，当运行游戏时， [__Simple3DGame::UpdateDynamics()__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L436-L856)方法是从[__Simple3DGame::RunGame__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L337-L418)方法调用 （这称为从[__gamemain:: Update__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameMain.cpp#L261-L329)） 来更新游戏场景中呈现的对象。

在__UpdateDynamics__循环中，用来设置的运动，独立于玩家的游戏世界的调用方法输入创建沉浸式游戏体验，使级别*栩栩如生*。 这包括需要呈现的图形和正在运行的动画循环以显示有关生活，即使没有玩家输入呼吸世界。 例如，树中 wind、 形成波浪 cresting 沿摇摆行、 机械吸烟和外部怪物拉伸和四处移动。 还包括对象之间的交互，包括玩家范围和游戏世界之间的冲突或者弹药与障碍物和目标之间的冲突。

游戏循环应始终保持更新游戏世界，基于游戏逻辑，物理算法，还是是否只是纯随机的除非游戏被明确暂停。 

在该游戏示例中，此原则称为*动态*原则，这包括立柱障碍物的上升和下降，以及被触发的弹药范围的运动和物理行为。 

### <a name="simple3dgameupdatedynamics-method"></a>Simple3DGame::UpdateDynamics 方法 

此方法处理四组计算：

* 游戏世界中触发的弹药范围的位置。
* 立柱障碍物的动画。
* 玩家与游戏世界界限的相交处。
* 弹药球体与障碍物、目标、其他弹药球体和游戏世界的碰撞。

障碍物的动画是 **Animate.h/.cpp** 中定义的一个循环。 弹药和任何碰撞的行为由简化的物理算法定义，在代码中提供并由一组全局常量游戏世界中，其中包括重力和材料属性的参数化。 这都在游戏世界坐标空间中计算。

### <a name="review-the-flow"></a>查看流程

既然我们已更新场景中的所有对象并计算了所有碰撞，我们需要使用此信息绘制对应的视觉更改。 

__GameMain::Update()__ 完成游戏循环的当前迭代后，该示例将立即调用__Render()__ 以获取更新的对象数据并生成新的场景以向玩家，显示，如此处所示。 接下来，让我们看一看呈现。

```cpp
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
                // ...
                // otherwise fall through and do normal processing to get the rendering handled.
            default:
                CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);
                Update(); // GameMain::Update calls Simple3DGame::RunGame() if game is in Dynamics state, uses Simple3DGame::UpdateDynamics() to update game world.
                m_renderer->Render(); //Render() is called immediately after the Update() loop
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

## <a name="render-the-game-worlds-graphics"></a>呈现游戏世界的图形

我们建议游戏中的图形最多在主游戏循环每次迭代时更新一次。 无论玩家是否输入，在循环迭代时都更新游戏。 这可以流畅地显示计算的动画和行为。 可以想像一个简单的水流场景，而水流仅在玩家按某个按钮时才移动会是什么情况。 这会造成一个非常乏味的视觉效果。 优秀的游戏看上去应该平滑流畅。

回忆示例游戏的循环[__gamemain:: Run__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameMain.cpp#L143-L202)如上所示。 如果游戏的主窗口可见，而且未贴靠或停用，游戏将继续更新并呈现该更新的结果。 现在，我们要检查的[__呈现__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameRenderer.cpp#L474-L624)的方法呈现的表示形式的状态。 这是之后立即调用**更新**，其中包括**RunGame**更新状态，这在上一节中进行了讨论。

此方法绘制 3D 世界的投影，然后在其上绘制 Direct2D 覆盖层。 在完成之后，它将使用合并的缓冲区提供最后的交换链进行显示。

>[!Note]
>有两个示例游戏的 Direct2D 覆盖层状态： 一个游戏显示包含暂停菜单中，以及一个游戏显示触摸屏移动观看矩形十字准线的位图的游戏信息覆盖层控制器。 得分文本在两个状态中绘制。 有关详细信息，请参阅[呈现框架 I：呈现简介](tutorial--assembling-the-rendering-pipeline.md)。

### <a name="gamerendererrender-method"></a>GameRenderer::Render 方法

```cpp
void GameRenderer::Render()
{
    bool stereoEnabled = m_deviceResources->GetStereoState();

    auto d3dContext = m_deviceResources->GetD3DDeviceContext();
    auto d2dContext = m_deviceResources->GetD2DDeviceContext();
   
        // ...
        if (m_game != nullptr && m_gameResourcesLoaded && m_levelResourcesLoaded)
        {
            // This section is only used after the game state has been initialized and all device
            // resources needed for the game have been created and associated with the game objects.
            //...
            auto objects = m_game->RenderObjects();
            for (auto object = objects.begin(); object != objects.end(); object++)
            {
                (*object)->Render(d3dContext, m_constantBufferChangesEveryPrim.Get()); // Renders the 3D objects
            }

        //...
        d3dContext->BeginEventInt(L"D2D BeginDraw", 1);
        d2dContext->BeginDraw(); //Start drawing the overlays
        
        // To handle the swapchain being pre-rotated, set the D2D transformation to include it.
        d2dContext->SetTransform(m_deviceResources->GetOrientationTransform2D());

        if (m_game != nullptr && m_gameResourcesLoaded)
        {
            // This is only used after the game state has been initialized.
            m_gameHud->Render(m_game); // Renders number of hits, shots, and time
        }

        if (m_gameInfoOverlay->Visible())
        {
            d2dContext->DrawBitmap(     // Renders the game overlay
                m_gameInfoOverlay->Bitmap(),
                m_gameInfoOverlayRect
                );
        }
        //...
    }
}
```

## <a name="simple3dgame-object"></a>Simple3DGame 对象

这些是方法和__Simple3DGame__对象类中定义的数据。

### <a name="methods"></a>方法

**Simple3DGame**上定义的内部方法包括：

-   **初始化**： 设置起始值的全局变量，并初始化游戏对象。 这将在[初始化和启动游戏](#initialize-and-start-the-game)部分中进行介绍。
-   **LoadGame**： 初始化新关卡并开始加载它。
-   **LoadLevelAsync**： 启动异步任务 （如果你不熟悉异步任务，请参阅[并行模式库](https://docs.microsoft.com/cpp/parallel/concrt/parallel-patterns-library-ppl)） 以初始化关卡，然后调用异步任务以加载特定于设备的关卡资源对呈现器。 该方法在单独的线程中运行；因此，只可从该线程调用 [**ID3D11Device**](https://msdn.microsoft.com/library/windows/desktop/ff476379) 方法（相对于 [**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385) 方法）。 任何设备上下文方法都在 **FinalizeLoadLevel** 方法中调用。
-   **FinalizeLoadLevel**： 完成的关卡加载所需要在主线程上执行任何工作。 这包括对 Direct3D 11 设备上下文 ([**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385)) 方法的任何调用。
-   **StartLevel**： 开始玩游戏的新关卡。
-   **PauseGame**： 暂停游戏。
-   **RunGame**： 运行游戏循环的迭代。 如果游戏状态为 **Active**，则游戏循环的每次迭代都将从 **App::Update** 中调用它一次。
-   **OnSuspending**和**OnResuming**： 分别暂停和恢复游戏的音频。

私有方法：

-   **LoadSavedState**和**SaveState**： 加载和分别保存的游戏的当前状态。
-   **SaveHighScore**和**LoadHighScore**： 分别保存和加载的最高分游戏中。
-   **InitializeAmmo**： 重置的每一轮开始将用作弹药回其初始状态对象的状态。
-   **UpdateDynamics**： 这是一个重要方法，因为它会更新根据封装的动画例程、 物理特性和控件输入的所有游戏对象。 这是定义游戏的交互性的核心。 这是在[更新游戏世界](#update-the-game-world)部分中进行介绍。

其他公共方法是属性 getter，用于将游戏执行和特定于覆盖层的信息返回到应用框架进行显示。

### <a name="data"></a>数据

在代码示例顶部，共有四个对象，其实例随游戏循环的运行而更新。

-   **MoveLookController**对象： 表示玩家输入。 有关详细信息，请参阅[添加控件](tutorial--adding-controls.md)。
-   **GameRenderer**对象： 表示从处理所有特定于设备的对象及其呈现**DirectXBase**类派生的 Direct3D 11 呈现器。 有关详细信息，请参阅[呈现框架 I](tutorial--assembling-the-rendering-pipeline.md)。
-   **音频**对象： 控制游戏的音频播放。 有关详细信息，请参阅[添加声音](tutorial--adding-sound.md)。

其余游戏变量包含基元列表及其相应的游戏内数量，以及特定于游戏执行的数据和约束。

## <a name="next-steps"></a>后续步骤

现在，你可能希望了解实际的呈现引擎： 如何在更新基元上的__呈现__方法调用获取转换为像素在屏幕上。 这将在两个部分进行介绍&mdash;[呈现框架 i： 呈现简介](tutorial--assembling-the-rendering-pipeline.md)并[呈现框架 II： 游戏呈现](tutorial-game-rendering.md)。 如果你对玩家控件如何更新游戏状态更感兴趣，请参阅[添加控件](tutorial--adding-controls.md)。
