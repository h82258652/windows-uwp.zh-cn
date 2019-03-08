---
title: 定义主游戏对象
description: 现在，我们将了解游戏示例主对象的详细信息，以及如何将其实现的规则转换为与游戏世界的交互。
ms.assetid: 6afeef84-39d0-cb78-aa2e-2e42aef936c9
ms.date: 10/24/2017
ms.topic: article
keywords: windows 10, uwp, 游戏, 主对象
ms.localizationpriority: medium
ms.openlocfilehash: 96aefc8b053dd7490f47910ca5bb79989855e1a3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651492"
---
# <a name="define-the-main-game-object"></a>定义主游戏对象

一旦布局示例游戏的基本框架并实现可处理的高级用户和系统行为的状态机，你将想要检查的规则和游戏的示例将转换为一个游戏的机制。 让我们看一下游戏示例的主要对象的详细信息以及如何将游戏的规则转换为与游戏世界的交互。

>[!Note]
>如果你尚未下载适用于此示例的最新游戏代码，请转到 [Direct3D 游戏示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX)。 此示例是大型 UWP 功能示例集合的一部分。 有关如何下载示例的说明，请参阅[从 GitHub 获取 UWP 示例](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples)。

## <a name="objective"></a>目标

了解如何将应用基本开发技术来实现游戏的规则和 UWP DirectX 游戏的机制。

## <a name="main-game-object"></a>主要游戏对象

在此示例游戏中， __Simple3DGame__是主要的游戏对象类。 实例__Simple3DGame__中构造对象__App::Load__方法。

__Simple3DGame__类对象：
* 指定在玩游戏逻辑的实现
* 包含进行通信的方法：
    * 在应用程序框架中定义的状态机的游戏状态方面的更改。
    * 从应用到游戏对象本身的游戏状态方面的更改。
    * 更新游戏的 UI （覆盖区上并平视显示）、 动画和物理特性 (dynamics) 的详细信息。

    >[!Note]
    >更新的图形处理__GameRenderer__类，该类包含用于获取和使用该游戏所使用的图形设备资源的方法。 有关详细信息，请参阅[呈现框架实现：简介呈现](tutorial--assembling-the-rendering-pipeline.md)。

* 用作定义的级别，游戏会话数据或生存时间，具体取决于如何定义您的游戏在高级别的容器。 在这种情况下，游戏状态数据的生存期内的游戏，并初始化一次当用户启动游戏。

若要查看的方法和此类对象中定义的数据，请转到[Simple3DGame 对象](#simple3dgame-object)。

## <a name="initialize-and-start-the-game"></a>初始化并启动游戏

当玩家启动游戏时，游戏对象必须初始化其状态，创建和添加覆盖层，设置用于跟踪玩家成绩的变量，并实例化将用于构建级别的对象。 在此示例中，这完成时的新__GameMain__中创建实例[ __App::Load__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/App.cpp#L115-L123)。 

在游戏对象__Simple3DGame__，在中创建__GameMain__构造函数。 然后初始化使用[ __Simple3DGame::Initialize__ ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L54-L250)方法期间[异步创建中的任务__GameMain__构造函数](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameMain.cpp#L65-L74).

### <a name="simple3dgameinitialize-method"></a>Simple3DGame::Initialize 方法

游戏示例设置了游戏对象中的以下组件：

* 创建新的音频播放对象。
* 创建游戏的图形基元的数组，包括级别基元、弹药和障碍的数组。
* 创建用于保存游戏状态数据的位置，名为*游戏*，并放入由 [**ApplicationData::Current**](https://msdn.microsoft.com/library/windows/apps/br241619) 指定的应用数据设置存储位置。
* 创建游戏计时器和初始游戏内重叠位图。
* 使用一组特定的视图和投影参数创建新的相机。
* 输入设备（控制器）设置为与相机相同的俯仰和偏航，以使玩家在起始控制位置和相机位置之间具有 1 对 1 的对应性。
* 创建玩家对象并设置为活动。 我们使用球体对象来检测玩家的临近墙壁和障碍并使照相机保持从获取放置在可能会破坏浸入式的位置。
* 创建游戏世界基元。
* 创建圆柱障碍。
* 创建目标（**Face** 对象）并编号。
* 创建弹药球体。
* 创建级别。
* 加载高分。
* 加载任何之前保存的游戏状态。

游戏现在已有全部关键组件的实例：游戏世界、玩家、障碍物、目标和弹药范围。 还有各级别的实例，表示上述所有组件的配置以及每个特定级别的行为。 现在让我们了解游戏如何构建级别。

## <a name="build-and-load-game-levels"></a>生成和负载的游戏级别

大部分繁重的任务中完成级别构造的__Level.h/.cpp__文件中找到__GameLevels__示例解决方案的文件夹。 因为它主要关注的非常具体的实现，我们将不会介绍这些此处。 重要的是每个关卡的代码都作为单独的 __LevelN__ 对象运行。 如果你想要玩游戏，则可以创建**级别**采用作为参数和随机分配的数字的对象放置的障碍和目标。 或者，您可以将该级别配置数据加载从资源文件或甚至是 Internet。

## <a name="define-the-game-play"></a>定义玩游戏

这时，我们已经有了装配游戏所需的全部组件。 级别已构造内存中基元，并已准备好让玩家开始与进行交互。

参阅最出色的游戏玩家输入，对的立即做出响应，并提供即时反馈。 这适用于任何类型的游戏时，从 twitch 操作，实时第一人称射击游戏到周密、 基于轮次的战略游戏。

### <a name="simple3dgamerungame-method"></a>Simple3DGame::RunGame 方法

游戏时播放一个级别，处于__Dynamics__状态。 

[__GameMain::Update__ ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameMain.cpp#L261-L329)是更新一次每个框架的应用程序状态，如下所示的主要更新循环。 在更新循环中，它将调用[ __Simple3DGame::RunGame__ ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L337-L418)方法来处理工作，如果游戏中__Dynamics__状态。

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
          
[__Simple3DGame::RunGame__ ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L337-L418)处理定义玩游戏的游戏循环的当前迭代的当前状态的数据集。

游戏中的流逻辑__RunGame__:
*  此方法更新倒记秒数的计时器直到完成该级别，并进行测试以查看该级别的时间是否已过期。 这是一个游戏的规则： 时间用完，不在拍摄时所有目标时，游戏就结束了。
*  如果时间用尽，此方法将设置 **TimeExpired** 游戏状态，并返回到之前的代码中的 **Update** 方法。
*  如果时间剩余，将轮询移动观看控制器以获取相机位置的更新；尤其是来自相机平面（玩家正在看的位置）视图正常投影的角度更新，以及该角度从上一次轮询控制器开始移动的距离。
*  将基于来自移动观看控制器的新数据更新相机。
*  将更新游戏世界中独立于玩家控制的对象的动态或动画和行为。 在此游戏的示例中， [ __UpdateDynamics()__ ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L436-L856)调用方法来更新具有已激发，支柱障碍物的动画和目标的移动 ammo 球体的动作。 有关详细信息，请参阅[更新游戏世界](#update-the-game-world)。
*  此方法将进行检查，以查看是否满足成功完成级别的条件。 如果是这样，它将落实该关卡的分数，并查看这是否为最后一个关卡（共 6 个）。 如果它是最终关卡，此方法将返回 **GameComplete** 游戏状态；否则，它将返回 __LevelComplete__ 游戏状态。
*  如果未完成该关卡，此方法将游戏状态设置为 __Active__ 并返回。

## <a name="update-the-game-world"></a>更新游戏的世界

在此示例中，当运行游戏时， [ __Simple3DGame::UpdateDynamics()__ ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L436-L856)方法从调用[ __Simple3DGame::RunGame__ ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L337-L418)方法 (这从调用[ __GameMain::Update__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameMain.cpp#L261-L329)) 更新游戏场景中呈现的对象。

在中__UpdateDynamics__循环中，调用用于动态、 独立于玩家输入，设置游戏世界的方法创建的沉浸式游戏体验，并使级别附带*处于活动状态*。 这包括需要呈现的图形和正在运行的动画循环代码以使生活，即使没有播放机输入兴师世界。 例如，在顾不了这么多，形成 cresting 沿陆线条、 机制吸烟和外星人庞然大物拉伸和四处移动的波次的树。 还包括对象之间的交互，包括玩家范围和游戏世界之间的冲突或者弹药与障碍物和目标之间的冲突。

游戏循环应始终保持更新游戏世界是否基于游戏逻辑，物理算法，或是否只是纯随机的除非专门游戏是否暂停。 

在该游戏示例中，此原则称为*动态*原则，这包括立柱障碍物的上升和下降，以及被触发的弹药范围的运动和物理行为。 

### <a name="simple3dgameupdatedynamics-method"></a>Simple3DGame::UpdateDynamics 方法 

此方法处理四组计算：

* 游戏世界中触发的弹药范围的位置。
* 立柱障碍物的动画。
* 玩家与游戏世界界限的相交处。
* 弹药球体与障碍物、目标、其他弹药球体和游戏世界的碰撞。

障碍物的动画是 **Animate.h/.cpp** 中定义的一个循环。 Ammo 和任何冲突的行为是由简化的物理算法定义，提供在代码中，通过一组的全局常量的游戏的世界里，包括重力和材料属性进行参数化。 这都在游戏世界坐标空间中计算。

### <a name="review-the-flow"></a>查看流

现在，我们已更新的场景中的所有对象并计算任何冲突，我们需要使用此信息来绘制相应的可视更改。 

之后__GameMain::Update()__ 完成当前迭代游戏循环的示例会立即调用__render （)__ 以获取更新的对象数据，并生成新的场景，以便向播放机中，将为如下所示。 接下来，让我们看看呈现。

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

回想一下示例游戏循环中所示[ __GameMain::Run__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameMain.cpp#L143-L202)。 如果游戏的主窗口可见，而且未贴靠或停用，游戏将继续更新并呈现该更新的结果。 [__呈现__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameRenderer.cpp#L474-L624)我们要检查的方法现在将呈现该状态的表示形式。 这是对的调用后立即**更新**，其中包括**RunGame**对上一节中介绍过的更新状态。

此方法绘制 3D 世界的投影，然后在其上绘制 Direct2D 覆盖层。 在完成之后，它将使用合并的缓冲区提供最后的交换链进行显示。

>[!Note]
>有两个示例游戏的 Direct2D 叠加的状态： 一个游戏游戏信息覆盖区，其中包含暂停菜单上，另一个游戏显示十字线以及触摸屏移动外观的矩形在那里的位图的显示位置控制器。 得分文本在两个状态中绘制。 有关详细信息，请参阅[呈现框架实现：简介呈现](tutorial--assembling-the-rendering-pipeline.md)。

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

这些是方法和中定义的数据__Simple3DGame__对象类。

### <a name="methods"></a>方法

上定义的内部方法**Simple3DGame**包括：

-   **初始化**:设置全局变量的起始值并初始化游戏对象。 中包含的此[初始化并启动游戏](#initialize-and-start-the-game)部分。
-   **LoadGame**:初始化新关卡并开始加载。
-   **LoadLevelAsync**:开始一个异步任务 (如果您不熟悉异步任务，请参阅[并行模式库](https://docs.microsoft.com/cpp/parallel/concrt/parallel-patterns-library-ppl)) 来初始化级别，然后调用一个异步任务上的呈现器加载设备的特定级别资源。 该方法在单独的线程中运行；因此，只可从该线程调用 [**ID3D11Device**](https://msdn.microsoft.com/library/windows/desktop/ff476379) 方法（相对于 [**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385) 方法）。 任何设备上下文方法都在 **FinalizeLoadLevel** 方法中调用。
-   **FinalizeLoadLevel**:完成需要在主线程上进行的关卡加载所需的任何工作。 这包括对 Direct3D 11 设备上下文 ([**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385)) 方法的任何调用。
-   **StartLevel**:开始执行游戏的新关卡。
-   **PauseGame**:暂停游戏。
-   **RunGame**:运行游戏循环的迭代。 如果游戏状态为 **Active**，则游戏循环的每次迭代都将从 **App::Update** 中调用它一次。
-   **OnSuspending**并**OnResuming**:分别暂停和恢复游戏的音频。

私有方法：

-   **LoadSavedState**并**SaveState**:分别加载和保存游戏的当前状态。
-   **SaveHighScore**并**LoadHighScore**:分别保存和加载游戏中的高分。
-   **InitializeAmmo**:在每一轮开始时，将用作弹药的各范围对象的状态重置回其初始状态。
-   **UpdateDynamics**:这是一个重要方法，因为它将根据封装的动画例程、物理特性和控件输入更新所有游戏对象。 这是定义游戏的交互性的核心。 中包含的此[更新游戏世界](#update-the-game-world)部分。

其他公共方法是属性 getter，用于将游戏执行和特定于覆盖层的信息返回到应用框架进行显示。

### <a name="data"></a>数据

在代码示例顶部，共有四个对象，其实例随游戏循环的运行而更新。

-   **MoveLookController**对象：表示玩家输入。 有关详细信息，请参阅[添加控件](tutorial--adding-controls.md)。
-   **GameRenderer**对象：表示派生自的 Direct3D 11 呈现器**DirectXBase**处理特定于设备的所有对象和其呈现的类。 有关详细信息，请参阅[呈现框架我](tutorial--assembling-the-rendering-pipeline.md)。
-   **音频**对象：控制游戏播放音频。 有关详细信息，请参阅[添加声音](tutorial--adding-sound.md)。

其余游戏变量包含基元列表及其相应的游戏内数量，以及特定于游戏执行的数据和约束。

## <a name="next-steps"></a>后续步骤

到目前为止，大家可能非常好奇有关实际呈现引擎： 如何调用__呈现__上的更新后的基元的方法获取屏幕上转换为像素。 两个部分中介绍了此&mdash;[呈现框架实现：对呈现简介](tutorial--assembling-the-rendering-pipeline.md)和[呈现框架 II:游戏渲染](tutorial-game-rendering.md)。 如果你对玩家控件如何更新游戏状态更感兴趣，请参阅[添加控件](tutorial--adding-controls.md)。
