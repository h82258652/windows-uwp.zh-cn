---
title: 优化 UWP DirectX 游戏的输入延迟
description: 输入延迟可能会大大影响游戏体验，将其优化可使游戏更美观。
ms.assetid: e18cd1a8-860f-95fb-098d-29bf424de0c0
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 游戏, directx, 输入延迟
ms.localizationpriority: medium
ms.openlocfilehash: 537dd6e9d3f300666a0692b66f422ce00dd68460
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "8346404"
---
#  <a name="optimize-input-latency-for-universal-windows-platform-uwp-directx-games"></a>优化通用 Windows 平台 (UWP) DirectX 游戏的输入延迟



输入延迟会大大影响游戏体验，将其优化可使游戏感觉更完美。 此外，适当的输入事件优化可延长电池使用时间。 了解如何选择正确的 CoreDispatcher 输入事件处理选项，以确保你的游戏尽可能流畅地处理输入。

## <a name="input-latency"></a>输入延迟


输入延迟是指系统响应用户输入所用的时间。 该响应通常是屏幕上显示的内容的更改，或通过音频反馈听到的内容。

每一个输入事件（不管它来自触摸屏指针、鼠标指针还是键盘）都会生成由事件处理程序处理的消息。 现代的触摸数字化器和游戏外设 都可以最低每指针 100Hz 的频率报告输入事件，这意味着对于每个指针（或击键），应用每秒可以接收到不少于 100 个事件。 如果多个指针同时操作， 或者使用精度更高的输入设备（例如，游戏鼠标），该更新比率将增加。 事件消息队列将很快排满。

了解游戏的输入延迟需求十分重要，这使你可采用最适合方案的方式处理事件。 没有适用于所有游戏的解决方案。

## <a name="power-efficiency"></a>电源效率


在输入延迟的上下文中，“电源效率”是指游戏对 GPU 的使用程度。 使用较少 GPU 资源的游戏具有较高的电源效率，可延长电池使用时间。 该情况也适用于 CPU。

如果一个游戏可采用低于 60 帧每秒（这是当前大部分显示器上的最大渲染速度）绘制整个屏幕而不损害用户体验，则可通过降低其绘制频率来提高电源效率。 一些游戏仅在用户输入时更新屏幕，因此这些游戏不应以 60 帧每秒的速度重复绘制相同的内容。

## <a name="choosing-what-to-optimize-for"></a>选择优化的目标


设计 DirectX 应用时，你需要作出一些选择。 该应用是否需要以 60 帧每秒的速度渲染以显示流畅的动画？或者它是否仅需在输入时进行渲染？ 它是否需要具有尽可能短的输入延迟？或者它是否能容忍一点延迟？ 用户是否期待我的应用谨慎地使用电池？

这些问题的回答可能将你的应用归入以下方案之一：

1.  按需渲染。 此类别中的游戏在响应特定输入类型时仅需更新屏幕。 电源效率十分出色，因为应用不重复渲染完全相同的框架，而且输入延迟很低，因为应用大部分时间都在等待输入。 桌面游戏和新闻阅读器是可能归于此类别的应用的示例。
2.  按需渲染，并带有过渡动画。 此方案与第一个方案相似，除了特定输入类型将启动一个动画，此动画不依赖来自用户的后续输入。 电源效率良好，因为游戏不重复渲染完全相同的框架，而且输入延迟很低，同时游戏不以动画显示。 对每次移动进行动画处理的交互式儿童游戏和桌面游戏可能归入此类别。
3.  以 60 帧每秒的速度渲染。 在此方案中，游戏持续更新屏幕。 电源效率低，因为它渲染屏幕可以显示的最大框架数。 输入延迟高，因为在显示内容时 DirectX 会阻止线程。 这样做使线程无法向屏幕发送多于它向用户显示的框架。 第一人称射击游戏、即时战略游戏和基于物理学的游戏是可能归入此类别的应用的示例。
4.  以 60 帧每秒的速度渲染，并实现尽可能短的输入延迟。 与方案 3 类似，应用持续更新屏幕，因此电源效率将会很低。 区别是，该游戏在单独的线程上响应输入，因此输入处理 不会因为向屏幕显示图形而被阻止。 联机多人游戏、格斗游戏或节奏/计时游戏可能归入此类别，因为它们支持 在极端紧凑的事件窗口中的移动输入。

## <a name="implementation"></a>实现


大部分 DirectX 游戏都由所谓游戏循环驱动。 基本算法是执行以下步骤，直到用户退出游戏或应用为止：

1.  处理输入
2.  更新游戏状态
3.  绘制游戏内容

当 DirectX 游戏的内容经过渲染并准备好呈现到屏幕上时，游戏循环将等待 GPU 为接收新帧做好准备，然后启动以再次处理输入。

我们将通过在一个简单的七巧板游戏上进行迭代，来展示上述各种方案的游戏循环的实现。 针对每种实现讨论的决策点、收益和权衡可作为指南，从而帮助你优化应用以实现较短的输入延迟和较高的电源效率。

## <a name="scenario-1-render-on-demand"></a>方案 1：按需渲染


七巧板游戏的首次迭代仅在用户移动一片七巧板时更新屏幕。 用户可以将一片七巧板拖到某个位置，或者通过选中它并触摸正确的目标位置来贴靠该七巧板。 在第二种情况中，该七巧板将跳到目标位置，此处没有任何动画或效果。

该代码在 [**IFrameworkView::Run**](https://msdn.microsoft.com/library/windows/apps/hh700505) 方法中具有使用 **CoreProcessEventsOption::ProcessOneAndAllPending** 的单线程游戏循环。 使用该选项将调度队列中所有当前可用的事件。 如果没有等待的事件，游戏循环将等到出现一个事件为止。

``` syntax
void App::Run()
{
    
    while (!m_windowClosed)
    {
        // Wait for system events or input from the user.
        // ProcessOneAndAllPending will block the thread until events appear and are processed.
        CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);

        // If any of the events processed resulted in a need to redraw the window contents, then we will re-render the
        // scene and present it to the display.
        if (m_updateWindow || m_state->StateChanged())
        {
            m_main->Render();
            m_deviceResources->Present();

            m_updateWindow = false;
            m_state->Validate();
        }
    }
}
```

## <a name="scenario-2-render-on-demand-with-transient-animations"></a>方案 2：按需渲染，并带有过渡动画


在第二次迭代中，游戏经过修改，当用户选中一块七巧板并触摸它的正确目标位置时，将出现它越过屏幕到达目标位置的动画。

与之前一样，该代码具有使用 **ProcessOneAndAllPending** 的单线程游戏循环，以调度队列中的输入事件。 现在的区别在于：在动画效果期间，该循环更改为使用 **CoreProcessEventsOption::ProcessAllIfPresent**，以使其不再等待新的输入事件。 如果没有等待的事件，[**ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215) 将立即返回并允许应用呈现动画中的下一个帧。 动画完成后，该循环将切换回 **ProcessOneAndAllPending** 以限制屏幕更新。

``` syntax
void App::Run()
{

    while (!m_windowClosed)
    {
        // 2. Switch to a continuous rendering loop during the animation.
        if (m_state->Animating())
        {
            // Process any system events or input from the user that is currently queued.
            // ProcessAllIfPresent will not block the thread to wait for events. This is the desired behavior when
            // you are trying to present a smooth animation to the user.
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

            m_state->Update();
            m_main->Render();
            m_deviceResources->Present();
        }
        else
        {
            // Wait for system events or input from the user.
            // ProcessOneAndAllPending will block the thread until events appear and are processed.
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);

            // If any of the events processed resulted in a need to redraw the window contents, then we will re-render the
            // scene and present it to the display.
            if (m_updateWindow || m_state->StateChanged())
            {
                m_main->Render();
                m_deviceResources->Present();

                m_updateWindow = false;
                m_state->Validate();
            }
        }
    }
}
```

为了支持 **ProcessOneAndAllPending** 和 **ProcessAllIfPresent** 之间的过渡，应用必须跟踪状态以了解是否正在创建动画。 在七巧板应用中，通过添加可在游戏循环期间在 GameState 类上调用的新方法来实现此目的。 游戏循环的动画分支通过调用 GameState 的新 Update 方法来促使动画状态更新。

## <a name="scenario-3-render-60-frames-per-second"></a>方案 3：以 60 帧每秒的速度渲染


在第三次迭代中，该应用显示一个计时器，它将向用户显示他们已在七巧板游戏上使用的时间。 因为它显示运行时间的单位精确到毫秒，所以它必须以 60 帧每秒的速度渲染才能保持不断更新显示内容。

与方案 1 和 2 一样，该应用具有单线程游戏循环。 该方案的不同之处在于：因为它不断地呈现，所以它不再需要像前两个方案一样跟踪游戏状态的更改。 因此，它可默认使用 **ProcessAllIfPresent** 处理事件。 如果没有等待的事件，**ProcessEvents** 将立即返回并继续呈现下一个帧。

``` syntax
void App::Run()
{

    while (!m_windowClosed)
    {
        if (m_windowVisible)
        {
            // 3. Continuously render frames and process system events and input as they appear in the queue.
            // ProcessAllIfPresent will not block the thread to wait for events. This is the desired behavior when
            // trying to present smooth animations to the user.
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

            m_state->Update();
            m_main->Render();
            m_deviceResources->Present();
        }
        else
        {
            // 3. If the window isn't visible, there is no need to continuously render.
            // Process events as they appear until the window becomes visible again.
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
}
```

该方法是编写游戏的最简单方式，因为你无需跟踪其他状态即可确定呈现时间。 它可实现可能达到的最快渲染，并在计时器间隔中提供合理的输入响应。

然而，这种开发的便利需要付出代价。 以 60 帧每秒的速度呈现需要比按需呈现使用更多的电源。 在游戏更改每帧显示的内容时，最好是使用 **ProcessAllIfPresent**。 它还会使输入延迟增加多达 16.7 毫秒，因为应用现在会在屏幕的同步间隔（而不是在 **ProcessEvents** 时）阻止游戏循环。 因为每帧仅处理一次队列 (60 Hz)，所以一些输入事件可能会失败。

## <a name="scenario-4-render-60-frames-per-second-and-achieve-the-lowest-possible-input-latency"></a>方案 4：以 60 帧每秒的速度渲染，并实现尽可能短的输入延迟


一些游戏可以忽略或补偿增加的输入延迟（如方案 3 中所见）。 然而，如果较短的输入延迟对于游戏体验和玩家感受反馈十分重要，以 60 帧每秒的速度渲染的游戏需要在单独线程上处理输入。

七巧板游戏的第四次迭代在方案 3 上构建，方法是将游戏循环中的输入处理和图形渲染拆分为单独的线程。 各自具有单独的线程可确保输入不被图形输出延迟；但是，代码因此变得更加复杂。 在方案 4 中，输入线程使用 [**CoreProcessEventsOption::ProcessUntilQuit**](https://msdn.microsoft.com/library/windows/apps/br208217) 调用 [**ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215)，它将等待新事件并调度所有可用事件。 它将继续此行为，直到窗口关闭或游戏调用 [**CoreWindow::Close**](https://msdn.microsoft.com/library/windows/apps/br208260) 为止。

``` syntax
void App::Run()
{
    // 4. Start a thread dedicated to rendering and dedicate the UI thread to input processing.
    m_main->StartRenderThread();

    // ProcessUntilQuit will block the thread and process events as they appear until the App terminates.
    CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessUntilQuit);
}

void JigsawPuzzleMain::StartRenderThread()
{
    // If the render thread is already running, then do not start another one.
    if (IsRendering())
    {
        return;
    }

    // Create a task that will be run on a background thread.
    auto workItemHandler = ref new WorkItemHandler([this](IAsyncAction^ action)
    {
        // Notify the swap chain that this app intends to render each frame faster
        // than the display's vertical refresh rate (typically 60 Hz). Apps that cannot
        // deliver frames this quickly should set this to 2.
        m_deviceResources->SetMaximumFrameLatency(1);

        // Calculate the updated frame and render once per vertical blanking interval.
        while (action->Status == AsyncStatus::Started)
        {
            // Execute any work items that have been queued by the input thread.
            ProcessPendingWork();

            // Take a snapshot of the current game state. This allows the renderers to work with a
            // set of values that won't be changed while the input thread continues to process events.
            m_state->SnapState();

            m_sceneRenderer->Render();
            m_deviceResources->Present();
        }

        // Ensure that all pending work items have been processed before terminating the thread.
        ProcessPendingWork();
    });

    // Run the task on a dedicated high priority background thread.
    m_renderLoopWorker = ThreadPool::RunAsync(workItemHandler, WorkItemPriority::High, WorkItemOptions::TimeSliced);
}
```

在 Microsoft Visual Studio2015 的**DirectX 11 和 XAML 应用 (通用 Windows)** 模板将游戏循环拆分为多个采用相似的线程。 它使用 [**Windows::UI::Core::CoreIndependentInputSource**](https://msdn.microsoft.com/library/windows/apps/dn298460) 对象启动专用于处理输入的线程，还创建独立于 XAML UI 线程的呈现线程。 有关这些模板的更多详细信息，请阅读[从模板创建通用 Windows 平台和 DirectX 游戏项目](user-interface.md)。

## <a name="additional-ways-to-reduce-input-latency"></a>缩短输入延迟的其他方法


### <a name="use-waitable-swap-chains"></a>使用可等待的交换链

DirectX 游戏通过更新用户在屏幕上看到的内容来响应用户输入。 在 60 Hz 屏幕上，屏幕每隔 16.7 毫秒（1 秒/60 帧）刷新一次。 图 1 显示了在刷新信号为 16.7 毫秒 (VBlank) 时，以 60 帧每秒的速度渲染的应用的输入事件的大致生命周期和响应情况：

图 1

![图 1 DirectX 中的输入延迟 ](images/input-latency1.png)

在 Windows8.1，DXGI 引入了用于交换链，它使应用即可轻松地缩短该延迟，无需实现启发以保持 Present 队列为空**DXGI\_SWAP\_CHAIN\_FLAG\_FRAME\_LATENCY\_WAITABLE\_OBJECT**标志。 使用该标志创建的交换链将作为可等待的交换链进行引用。 图 2 显示了使用可等待的交换链时输入事件的大致生命周期和响应情况：

图 2

![图 2 DirectX 中的输入延迟可等待](images/input-latency2.png)

我们可从以上图表中看出，如果这些游戏可以在 16.7 毫秒的预算时间（由屏幕的刷新频率定义）内渲染和呈现每帧画面，就有可能缩短整整 2 帧的输入延迟。 七巧板示例使用可等待的交换链并控制 Present 队列限制，方法是调用：` m_deviceResources->SetMaximumFrameLatency(1);`

 

 




