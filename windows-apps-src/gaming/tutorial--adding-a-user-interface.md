---
title: 添加用户界面
description: 了解如何将二维用户界面覆盖添加到 DirectX UWP 游戏。
ms.assetid: fa40173e-6cde-b71b-e307-db90f0388485
ms.date: 10/24/2017
ms.topic: article
keywords: windows 10, uwp, 游戏, 用户界面, directx
ms.localizationpriority: medium
ms.openlocfilehash: 09005eb12997126a9cad68c388beb0473b19fda3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57609052"
---
# <a name="add-a-user-interface"></a>添加用户界面


现在，我们的游戏已经及其 3D 视觉对象之后，就可以重点是添加一些 2D 元素，以便游戏玩家提供游戏状态有关的反馈。 这可以通过添加简单的菜单选项来实现，并在 3d 图形之上的危险警告显示组件管道输出。

>[!Note]
>如果你尚未下载适用于此示例的最新游戏代码，请转到 [Direct3D 游戏示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX)。 此示例是大型 UWP 功能示例集合的一部分。 有关如何下载示例的说明，请参阅[从 GitHub 获取 UWP 示例](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples)。

## <a name="objective"></a>目标

使用 Direct2D，将大量图形用户界面和行为添加到我们 UWP DirectX 游戏包括：
- 平视显示，其中包括[移动外观控制器](tutorial--adding-controls.md)边界矩形
- 游戏状态菜单


## <a name="the-user-interface-overlay"></a>用户界面覆盖层


虽然有许多方式在 DirectX 游戏中显示文本和用户界面元素，我们将重点介绍在使用[Direct2D](https://msdn.microsoft.com/library/windows/apps/dd370990.aspx)。 我们还将使用[DirectWrite](https://msdn.microsoft.com/library/windows/desktop/dd368038)文本元素。


Direct2D 是一组的 2D 绘图 Api 用来绘制像素基于基元和效果。 与 Direct2D 开始时最好为简单起见。 复杂的布局和界面行为需要时间和规划。 如果您的游戏需要使用复杂的用户界面，如模拟和战略游戏中考虑改为使用 XAML。

> [!NOTE]
> 有关开发 UWP DirectX 游戏中的使用 XAML 用户界面的信息，请参阅[扩展游戏示例](tutorial-resources.md)。

Direct2D 不被专为用户界面或如 HTML 和 XAML 的布局。 它没有提供用户界面组件，如列表、 框或按钮。 它还没有提供布局的组件，如 div、 表格或网格。


对于此游戏的示例中，我们有两个主要 UI 组件。
1. 评分和游戏中控件的危险警告显示。
2. 覆盖用于显示游戏的状态文本和选项，例如暂停信息和级别启动选项。

### <a name="using-direct2d-for-a-heads-up-display"></a>对抬头显示使用 Direct2D

下图显示了此示例中的游戏危险警告显示。 它是简单整洁，从而让播放器能够专注于导航 3D 世界和射击目标。 很好的接口或危险警告显示必须永远不会增加复杂性播放机来处理和游戏中的事件做出反应的能力。

![游戏覆盖层的屏幕截图](images/simple-dx-game-ui-overlay.png)

在覆盖区上包含以下基本基元。
- [**DirectWrite** ](https://msdn.microsoft.com/en-us/library/windows/desktop/dd368038)通知的播放机在右上角中的文本 
    - 成功的命中数
    - 已在播放机的快照的数量
    - 在级别中的剩余时间
    - 当前级别数 
- 两个相交用于构成十字线的直线线段
- 在底部角落的两个矩形[移动外观控制器](tutorial--adding-controls.md)边界。 


在覆盖区上的游戏中平视显示状态中绘制[ **GameHud::Render** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameHud.cpp#L234-L358)方法[ **GameHud** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameHud.h)类。 在此方法中，表示我们的 UI 在 Direct2D 覆盖区上将更新以反映命中，时间剩余，和级别号数中的更改。

如果已初始化游戏，我们将添加`TotalHits()`， `TotalShots()`，并`TimeRemaining()`到[ **swprintf_s** ](https://docs.microsoft.com/cpp/c-runtime-library/reference/sprintf-s-sprintf-s-l-swprintf-s-swprintf-s-l)缓冲并指定打印格式。 我们然后可以使用它绘制[ **DrawText** ](https://msdn.microsoft.com/en-us/library/windows/desktop/dd742848)方法。 我们执行相同操作的当前级别指示符，绘制空编号，以显示等 ➀，未完成的级别和已填充的数字，如 ➊ 以显示特定级别已完成。


下面的代码段将指导完成**GameHud::Render**方法的过程 
- 创建位图使用[* * ID2D1RenderTarget::DrawBitmap * *](https://msdn.microsoft.com/en-us/library/windows/desktop/dd371880)
- 划分的 UI 区域，为使用矩形[ **D2D1::RectF**](https://msdn.microsoft.com/en-us/library/windows/desktop/dd368184)
- 使用**DrawText**以使文本元素

```cpp
void GameHud::Render(_In_ Simple3DGame^ game)
{
    auto d2dContext = m_deviceResources->GetD2DDeviceContext();
    auto windowBounds = m_deviceResources->GetLogicalSize();

    if (m_showTitle)
    {
        d2dContext->DrawBitmap(
            m_logoBitmap.Get(),
            D2D1::RectF(
                GameConstants::Margin,
                GameConstants::Margin,
                m_logoSize.width + GameConstants::Margin,
                m_logoSize.height + GameConstants::Margin
                )
            );
        d2dContext->DrawTextLayout(
            Point2F(m_logoSize.width + 2.0f * GameConstants::Margin, GameConstants::Margin),
            m_titleHeaderLayout.Get(),
            m_textBrush.Get()
            );
        d2dContext->DrawTextLayout(
            Point2F(GameConstants::Margin, m_titleBodyVerticalOffset),
            m_titleBodyLayout.Get(),
            m_textBrush.Get()
            );
    }

    // Draw text for number of hits, total shots, and time remaining
    if (game != nullptr)
    {
        // This section is only used after the game state has been initialized.
        static const int bufferLength = 256;
        static char16 wsbuffer[bufferLength];
        int length = swprintf_s(
            wsbuffer,
            bufferLength,
            L"Hits:\t%10d\nShots:\t%10d\nTime:\t%8.1f",
            game->TotalHits(),
            game->TotalShots(),
            game->TimeRemaining()
            );
        
        // Draw the upper right portion of the HUD displaying total hits, shots, and time remaining
        d2dContext->DrawText(
            wsbuffer,
            length,
            m_textFormatBody.Get(),
            D2D1::RectF(
                windowBounds.Width - GameConstants::HudRightOffset,
                GameConstants::HudTopOffset,
                windowBounds.Width,
                GameConstants::HudTopOffset + (GameConstants::HudBodyPointSize + GameConstants::Margin) * 3
                ),
            m_textBrush.Get()
            );

        // Using the unicode characters starting at 0x2780 ( ➀ ) for the consecutive levels of the game.
        // For completed levels start with 0x278A ( ➊ ) (This is 0x2780 + 10).
        uint32 levelCharacter[6];
        for (uint32 i = 0; i < 6; i++)
        {
            levelCharacter[i] = 0x2780 + i + ((static_cast<uint32>(game->LevelCompleted()) == i) ? 10 : 0);
        }
        length = swprintf_s(
            wsbuffer,
            bufferLength,
            L"%lc %lc %lc %lc %lc %lc",
            levelCharacter[0],
            levelCharacter[1],
            levelCharacter[2],
            levelCharacter[3],
            levelCharacter[4],
            levelCharacter[5]
            );
        // Create a new rectangle and draw the current level info text inside
        d2dContext->DrawText(
            wsbuffer,
            length,
            m_textFormatBodySymbol.Get(),
            D2D1::RectF(
                windowBounds.Width - GameConstants::HudRightOffset,
                GameConstants::HudTopOffset + (GameConstants::HudBodyPointSize + GameConstants::Margin) * 3 + GameConstants::Margin,
                windowBounds.Width,
                GameConstants::HudTopOffset + (GameConstants::HudBodyPointSize+ GameConstants::Margin) * 4
                ),
            m_textBrush.Get()
            );

        if (game->IsActivePlay())
        {
            // Draw the move and fire rectangles
            // Draw the crosshairs
        }
    }
}
```

向下进一步，此段的方法的重大[ **GameHud::Render** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameHud.cpp#L320-L358)方法绘制我们移动并触发与矩形[ **ID2D1RenderTarget::DrawRectangle**](https://msdn.microsoft.com/library/windows/desktop/dd371902)，并使用两个调用的十字准线[ **ID2D1RenderTarget::DrawLine**](https://msdn.microsoft.com/library/windows/desktop/dd371895)。

```cpp
        // Check if game is playing
        if (game->IsActivePlay())
        {
            // Draw a rectangle for the touch input for the move control.
            d2dContext->DrawRectangle(
                D2D1::RectF(
                    0.0f,
                    windowBounds.Height - GameConstants::TouchRectangleSize,
                    GameConstants::TouchRectangleSize,
                    windowBounds.Height
                    ),
                m_textBrush.Get()
                );
            // Draw a rectangle for the touch input of the fire control.
            d2dContext->DrawRectangle(
                D2D1::RectF(
                    windowBounds.Width - GameConstants::TouchRectangleSize,
                    windowBounds.Height - GameConstants::TouchRectangleSize,
                    windowBounds.Width,
                    windowBounds.Height
                    ),
                m_textBrush.Get()
                );

            // Draw two lines to form crosshairs
            d2dContext->DrawLine(
                D2D1::Point2F(windowBounds.Width / 2.0f - GameConstants::CrossHairHalfSize, windowBounds.Height / 2.0f),
                D2D1::Point2F(windowBounds.Width / 2.0f + GameConstants::CrossHairHalfSize, windowBounds.Height / 2.0f),
                m_textBrush.Get(),
                3.0f
                );
            d2dContext->DrawLine(
                D2D1::Point2F(windowBounds.Width / 2.0f, windowBounds.Height / 2.0f - GameConstants::CrossHairHalfSize),
                D2D1::Point2F(windowBounds.Width / 2.0f, windowBounds.Height / 2.0f + GameConstants::CrossHairHalfSize),
                m_textBrush.Get(),
                3.0f
                );
        }
```

在中**GameHud::Render**方法我们存储中的游戏窗口的逻辑大小`windowBounds`变量。 这将使用[ `GetLogicalSize` ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Common/DeviceResources.h#L41)方法**DeviceResources**类。 
```cpp
auto windowBounds = m_deviceResources->GetLogicalSize();
```

 获取游戏窗口的大小是必需的 UI 编程。 称为 Dip （设备独立像素为单位），其中某个 DIP 定义为 1/96 英寸提供窗口的大小。 Direct2D 缩放到实际的像素的绘图单位绘图时，执行此操作通过使用 Windows 每英寸点数 (DPI) 设置。 同样，使用文本绘制[ **DirectWrite**](https://msdn.microsoft.com/en-us/library/windows/desktop/dd368038)，你指定 Dip，而不是点的字体的大小。 DIP 表示为浮点数字。

 

### <a name="displaying-game-state-info"></a>显示游戏状态信息

除了平视显示，游戏示例有六种游戏状态表示一个覆盖区。 所有状态的都功能与文本要读取的播放器的大黑色的矩形基元。 因为它们不是活动在这些状态，不绘制移动外观控制器矩形和十字准线。

使用创建覆盖层[ **GameInfoOverlay** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.h)类，使我们能够切出显示的文本对齐的游戏的状态。

![状态和覆盖层的操作](images/simple-dx-game-ui-finaloverlay.png)

在覆盖区上将分解为两个部分：**状态**并**操作**。 **状态**secton 进一步细分成**标题**并**正文**矩形。 **操作**部分仅包含一个矩形。 每个矩形具有不同的用途。

-   `titleRectangle` 包含标题文本。
-   `bodyRectangle` 包含正文文本。
-   `actionRectangle` 包含通知播放机来执行特定操作的文本。

该游戏有六个可以设置的状态。 使用传达游戏的状态**状态**在覆盖区上的部分。 **状态**矩形会更新使用多种方法具有以下状态相对应。

- 正在加载
- 初始启动/高分数的统计信息
- 级别开始
- 暂停游戏
- 游戏结束
- 结束-赢得的游戏


**操作**使用更新在覆盖区上的部分[ **GameInfoOverlay::SetAction** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L522-L564)方法，允许操作文本设置为以下值之一。
- "点击以再次播放..."
- "级别加载，请稍候..."
- "点击以继续..."
- 无

> [!NOTE]
> 将讨论这两种方法中进一步[表示游戏状态](#representing-game-state)部分。

这怎么回事在游戏中，根据**状态**并**操作**调整部分文本字段。
让我们看看我们如何初始化和绘制这些六种状态的覆盖。

### <a name="initializing-and-drawing-the-overlay"></a>初始化和绘制覆盖层

六个**状态**状态具有一些共同点，使资源和方法所需非常相似。
    - 它们都在屏幕的中心作为其背景使用黑色的矩形。
    - 显示的文本可以是**标题**或**正文**文本。
    - 使用 Segoe UI 字体和后矩形上绘制的文本。 


游戏示例有创建覆盖层时派上用场的四个方法。
 

#### <a name="gameinfooverlaygameinfooverlay"></a>GameInfoOverlay::GameInfoOverlay
[ **GameInfoOverlay::GameInfoOverlay** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L30-L78)构造函数初始化在覆盖区上，我们将使用上显示的信息向播放机在位图面维护。 构造函数获取从工厂[ **ID2D1Device** ](https://msdn.microsoft.com/library/windows/desktop/hh404478)对象传递给它，它用于创建[ **ID2D1DeviceContext** ](https://msdn.microsoft.com/library/windows/desktop/hh404479)可以绘制覆盖对象本身。 [IDWriteFactory::CreateTextFormat](https://msdn.microsoft.com/en-us/library/windows/desktop/dd368203) 


#### <a name="gameinfooverlaycreatedevicedependentresources"></a>GameInfoOverlay::CreateDeviceDependentResources
[**GameInfoOverlay::CreateDeviceDependentResources** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L82-L104)是我们的方法，用于创建画笔用于绘制文本。 若要执行此操作，我们获得[ **ID2D1DeviceContext2** ](https://msdn.microsoft.com/en-us/library/windows/desktop/dn890789)对象可以创建和绘制的几何形状，以及功能，例如墨迹和渐变网格呈现该对象。 然后，创建一系列使用彩色画笔[ **ID2D1SolidColorBrush** ](https://msdn.microsoft.com/en-us/library/windows/desktop/dd372207)绘制 folling UI 元素。
- 若要获得矩形背景黑色画笔
- 白色画笔状态文本
- 橙色画笔操作文本

#### <a name="deviceresourcessetdpi"></a>DeviceResources::SetDpi
[ **DeviceResources::SetDpi** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Common/DeviceResources.cpp#L514-L527)方法可设置每英寸点数的窗口。 调用此方法时所使用的 DPI 发生更改，需要重新调整游戏窗口调整大小时，会发生该情况。 在更新后的 DPI，此方法还会调用[**DeviceResources::CreateWindowSizeDependentResources** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Common/DeviceResources.cpp#L214-L487)进行确保必要的资源，会重新创建每次调整窗口大小。


#### <a name="gameinfooverlaycreatewindowssizedependentresources"></a>GameInfoOverlay::CreateWindowsSizeDependentResources
[ **GameInfoOverlay::CreateWindowsSizeDependentResources** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L108-L225)方法是我们的所有绘图都发生的地方。 以下是该方法的步骤的概述。
- 关闭的 UI 文本的部分创建三个矩形**标题**，**正文**，并**操作**文本。
    ```cpp 
    m_titleRectangle = D2D1::RectF(
        GameInfoOverlayConstant::SideMargin,
        GameInfoOverlayConstant::TopMargin,
        overlaySize.width - GameInfoOverlayConstant::SideMargin,
        GameInfoOverlayConstant::TopMargin + GameInfoOverlayConstant::TitleHeight
        );
    m_actionRectangle = D2D1::RectF(
        GameInfoOverlayConstant::SideMargin,
        overlaySize.height - (GameInfoOverlayConstant::ActionHeight + GameInfoOverlayConstant::BottomMargin),
        overlaySize.width - GameInfoOverlayConstant::SideMargin,
        overlaySize.height - GameInfoOverlayConstant::BottomMargin
        );
    m_bodyRectangle = D2D1::RectF(
        GameInfoOverlayConstant::SideMargin,
        m_titleRectangle.bottom + GameInfoOverlayConstant::Separator,
        overlaySize.width - GameInfoOverlayConstant::SideMargin,
        m_actionRectangle.top - GameInfoOverlayConstant::Separator
        );
    ```

- 位图创建的名为`m_levelBitmap`，考虑到帐户使用的当前 DPI **CreateBitmap**。
- `m_levelBitmap` 设置为我们 2D 呈现器目标使用[ **ID2D1DeviceContext::SetTarget**](https://msdn.microsoft.com/en-us/library/windows/desktop/hh404533)。
- 位图清除与进行黑色使用的每个像素[ **ID2D1RenderTarget::Clear**](https://msdn.microsoft.com/en-us/library/windows/desktop/dd371772)。
- [**ID2D1RenderTarget::BeginDraw** ](https://msdn.microsoft.com/en-us/library/windows/desktop/dd371768)调用以启动绘图。 
- **DrawText**调用以绘制中存储的文本`m_titleString`， `m_bodyString`，和`m_actionString`中使用相应的 approperiate 矩形**ID2D1SolidColorBrush**。
- [**ID2D1RenderTarget::EndDraw** ](ID2D1RenderTarget::EndDraw)调用来停止上的所有绘制操作`m_levelBitmap`。
- 使用创建另一个位图**CreateBitmap**名为`m_tooSmallBitmap`要用作回退，只显示显示配置是否对游戏来说太小。
- 用于上绘制重复过程`m_levelBitmap`有关`m_tooSmallBitmap`，这次仅绘制字符串`Paused`在正文中。




现在我们只需将六种方法来填充我们六个覆盖状态的文本 ！

### <a name="representing-game-state"></a>表示游戏的状态


每个游戏中的六个覆盖状态都有相应的方法**GameInfoOverlay**对象。 这些方法绘制覆盖层的变体，以向玩家传达游戏本身的显式信息。 此通信可表示与**标题**并**正文**字符串。 因为示例已配置的资源和为该信息的布局初始化时且[ **GameInfoOverlay::CreateDeviceDependentResources** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L82-L104)方法，它只需提供覆盖特定于状态的字符串。

**状态**部分覆盖的设置通过调用以下方法之一。

游戏状态 | 状态设置方法 | 状态字段
:----- | :------- | :---------
正在加载 | [GameInfoOverlay::SetGameLoading](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L254-L306) |**Title**</br>加载资源 </br>**正文**</br> 以增量方式将打印"。"暗示加载活动。
初始启动/高分数的统计信息 | [GameInfoOverlay::SetGameStats](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L310-L354) |**Title**</br>高评分</br> **正文**</br> 级别已完成 # </br>总点 #</br>总快照 #
级别开始 | [GameInfoOverlay::SetLevelStart](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L413-L471) |**Title**</br>级别 #</br>**正文**</br>级别目标的描述。
暂停游戏 | [GameInfoOverlay::SetPause](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L475-L502) |**Title**</br>暂停游戏</br>**正文**</br>无
游戏结束 | [GameInfoOverlay::SetGameOver](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L358-L409) |**Title**</br>游戏结束</br> **正文**</br> 级别已完成 # </br>总点 #</br>总快照 #</br>级别已完成 #</br>高评分 #
结束-赢得的游戏 | [GameInfoOverlay::SetGameOver](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L358-L409) |**Title**</br>你赢了！</br> **正文**</br> 级别已完成 # </br>总点 #</br>总快照 #</br>级别已完成 #</br>高评分 #




与[ **GameInfoOverlay::CreateWindowSizeDependentResources** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L117-L134)方法，该示例声明三个对应于在覆盖区上的特定区域的矩形区域。



记住这些覆盖层后，下面我们了解一个特定于状态的方法 **GameInfoOverlay::SetGameStats**，并了解如何绘制覆盖层。

```cpp
void GameInfoOverlay::SetGameStats(int maxLevel, int hitCount, int shotCount)
{
    int length;

    auto d2dContext = m_deviceResources->GetD2DDeviceContext();

    d2dContext->SetTarget(m_levelBitmap.Get());
    d2dContext->BeginDraw();
    d2dContext->SetTransform(D2D1::Matrix3x2F::Identity());
    d2dContext->FillRectangle(&m_titleRectangle, m_backgroundBrush.Get());
    d2dContext->FillRectangle(&m_bodyRectangle, m_backgroundBrush.Get());
    m_titleString = "High Score";

    d2dContext->DrawText(
        m_titleString->Data(),
        m_titleString->Length(),
        m_textFormatTitle.Get(),
        m_titleRectangle,
        m_textBrush.Get()
        );
    length = swprintf_s(
        wsbuffer,
        bufferLength,
        L"Levels Completed %d\nTotal Points %d\nTotal Shots %d",
        maxLevel,
        hitCount,
        shotCount
        );
    m_bodyString = ref new Platform::String(wsbuffer, length);
    d2dContext->DrawText(
        m_bodyString->Data(),
        m_bodyString->Length(),
        m_textFormatBody.Get(),
        m_bodyRectangle,
        m_textBrush.Get()
        );

    // We ignore D2DERR_RECREATE_TARGET here. This error indicates that the device
    // is lost. It will be handled during the next call to Present.
    HRESULT hr = d2dContext->EndDraw();
    if (hr != D2DERR_RECREATE_TARGET)
    {
        // The D2DERR_RECREATE_TARGET indicates there has been a problem with the underlying
        // D3D device.  All subsequent rendering will be ignored until the device is recreated.
        // This error will be propagated and the appropriate D3D error will be returned from the
        // swapchain->Present(...) call.   At that point, the sample will recreate the device
        // and all associated resources.  As a result, the D2DERR_RECREATE_TARGET doesn't
        // need to be handled here.
        DX::ThrowIfFailed(hr);
    }
}
```

使用 Direct2D 设备上下文的**GameInfoOverlay**初始化的对象，此方法填充标题和正文矩形与改为使用背景画笔。 它使用白色文本画笔将“High Score”字符串文本绘制到标题矩形，并将包含更新游戏状态信息的文本绘制到正文矩形。


通过后续调用更新操作矩形[ **GameInfoOverlay::SetAction** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L522-L564)上的方法从**GameMain**对象，它提供所需的游戏状态信息通过**GameInfoOverlay::SetAction**来确定正确的消息到播放机中，如"点击以继续"。

中所选择的任何给定状态的覆盖[ **GameMain::SetGameInfoOverlay** ](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/GameMain.cpp#L606-L661)方法如下：

```cpp
void GameMain::SetGameInfoOverlay(GameInfoOverlayState state)
{
    m_gameInfoOverlayState = state;
    switch (state)
    {
    case GameInfoOverlayState::Loading:
        m_uiControl->SetGameLoading();
        break;

    case GameInfoOverlayState::GameStats:
        m_uiControl->SetGameStats(
            m_game->HighScore().levelCompleted + 1,
            m_game->HighScore().totalHits,
            m_game->HighScore().totalShots
            );
        break;

    case GameInfoOverlayState::LevelStart:
        m_uiControl->SetLevelStart(
            m_game->LevelCompleted() + 1,
            m_game->CurrentLevel()->Objective(),
            m_game->CurrentLevel()->TimeLimit(),
            m_game->BonusTime()
            );
        break;

    case GameInfoOverlayState::GameOverCompleted:
        m_uiControl->SetGameOver(
            true,
            m_game->LevelCompleted() + 1,
            m_game->TotalHits(),
            m_game->TotalShots(),
            m_game->HighScore().totalHits
            );
        break;

    case GameInfoOverlayState::GameOverExpired:
        m_uiControl->SetGameOver(
            false,
            m_game->LevelCompleted(),
            m_game->TotalHits(),
            m_game->TotalShots(),
            m_game->HighScore().totalHits
            );
        break;

    case GameInfoOverlayState::Pause:
        m_uiControl->SetPause(
            m_game->LevelCompleted() + 1,
            m_game->TotalHits(),
            m_game->TotalShots(),
            m_game->TimeRemaining()
            );
        break;
    }
}
```

游戏现在具有一种方法进行通信的文本信息到播放机根据游戏状态，并且我们有一种方法切换显示的内容与其在游戏。

### <a name="next-steps"></a>后续步骤

在下一主题[添加控件](tutorial--adding-controls.md)中，我们将介绍玩家如何与游戏示例交互以及输入如何更改游戏状态。



 




