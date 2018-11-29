---
title: 添加用户界面
description: 了解如何添加到 DirectX UWP 游戏的 2D 用户界面覆盖层。
ms.assetid: fa40173e-6cde-b71b-e307-db90f0388485
ms.date: 10/24/2017
ms.topic: article
keywords: windows 10, uwp, 游戏, 用户界面, directx
ms.localizationpriority: medium
ms.openlocfilehash: 09005eb12997126a9cad68c388beb0473b19fda3
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/28/2018
ms.locfileid: "7980394"
---
# <a name="add-a-user-interface"></a>添加用户界面


现在，我们的游戏有现成其 3D 视觉效果，就可以专注于添加一些 2D 元素，以便游戏可以向玩家提供有关游戏状态的反馈。 这可以通过添加简单的菜单选项和抬头显示组件在三维图形管道输出。

>[!Note]
>如果你尚未下载适用于此示例的最新游戏代码，请转到 [Direct3D 游戏示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX)。 此示例是大型 UWP 功能示例集合的一部分。 有关如何下载示例的说明，请参阅[从 GitHub 获取 UWP 示例](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples)。

## <a name="objective"></a>目标

使用 Direct2D，将大量的用户界面图形和行为添加到我们 UWP DirectX 游戏包括：
- 提醒显示，包括[移动观看控制器](tutorial--adding-controls.md)边界矩形
- 游戏状态菜单


## <a name="the-user-interface-overlay"></a>用户界面覆盖层


尽管有多种方法可在 DirectX 游戏中显示文本和用户界面元素，我们将重点介绍使用[Direct2D](https://msdn.microsoft.com/library/windows/apps/dd370990.aspx)。 我们将还使用[DirectWrite](https://msdn.microsoft.com/library/windows/desktop/dd368038)文本元素。


Direct2D 是一组的 2D 图形 Api，用于绘制基于像素的基元和效果。 在开始使用 Direct2D，最好是为简单起见。 复杂的布局和界面行为需要时间和规划。 如果你的游戏需要复杂的用户界面，例如模拟和战略游戏中，请考虑改为使用 XAML。

> [!NOTE]
> 有关开发 UWP DirectX 游戏中的使用 XAML 用户界面的信息，请参阅[扩展游戏示例](tutorial-resources.md)。

Direct2D 不被专为用户界面或布局如 HTML 和 XAML。 它不会提供用户界面组件，如列表、 框中，或按钮。 它还不提供布局组件，如 div、 表或网格。


对于此游戏示例中，我们有两个主要 UI 组件。
1. 抬头显示分数和游戏内控件。
2. 覆盖层用于显示游戏状态文本和选项，例如暂停信息和级别开始选项。

### <a name="using-direct2d-for-a-heads-up-display"></a>对抬头显示使用 Direct2D

下图显示了该示例游戏内提醒显示。 它是简单整齐，可让玩家专注于在三维世界中导航、 射击目标。 良好的界面或提醒显示必须永远不会增加的复杂性玩家处理和响应游戏中事件的能力。

![游戏覆盖层的屏幕截图](images/simple-dx-game-ui-overlay.png)

覆盖层包含的以下基本基元。
- [**DirectWrite**](https://msdn.microsoft.com/en-us/library/windows/desktop/dd368038)文本向玩家的右上角中 
    - 成功命中数
    - 提示玩家的数量
    - 在此级别中的剩余时间
    - 当前关卡数 
- 两个相交线段用于形成十字
- [移动观看控制器](tutorial--adding-controls.md)边界的底部角处的两个矩形。 


[**GameHud**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameHud.h)类[**GameHud::Render**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameHud.cpp#L234-L358)方法绘制覆盖层的游戏内提醒显示状态。 在此方法中，更新的 Direct2D 覆盖层，代表我们的 UI 以反映中发生，时间剩余，和级别号数的更改。

如果已初始化游戏，我们将添加`TotalHits()`， `TotalShots()`，并`TimeRemaining()`到[**swprintf_s**](https://docs.microsoft.com/cpp/c-runtime-library/reference/sprintf-s-sprintf-s-l-swprintf-s-swprintf-s-l)缓冲区并指定打印的格式。 然后，我们可以绘制使用[**DrawText**](https://msdn.microsoft.com/en-us/library/windows/desktop/dd742848)方法。 我们执行相同操作的当前的级别指示器，绘制空的编号，以显示 ➀，如未完成的级别和填充的编号，如 ➊ 以显示已完成的特定级别。


以下代码段将指导完成**GameHud::Render**方法的过程 
- 创建位图 using [* * ID2D1RenderTarget::DrawBitmap * *](https://msdn.microsoft.com/en-us/library/windows/desktop/dd371880)
- 划分为使用[**D2D1::RectF**的矩形的 UI 区域](https://msdn.microsoft.com/en-us/library/windows/desktop/dd368184)
- 使用**DrawText**使文本元素

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

重大方法进一步，向[**GameHud::Render**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameHud.cpp#L320-L358)方法绘制这条我们移动和射击矩形填充[**ID2D1RenderTarget::DrawRectangle**](https://msdn.microsoft.com/library/windows/desktop/dd371902)，并使用[**ID2D1RenderTarget::DrawLine**](https://msdn.microsoft.com/library/windows/desktop/dd371895)的两个调用的十字。

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

在**GameHud::Render**方法中，我们将存储在游戏窗口的逻辑大小`windowBounds`变量。 这将使用[`GetLogicalSize`](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Common/DeviceResources.h#L41) **DeviceResources**类的方法。 
```cpp
auto windowBounds = m_deviceResources->GetLogicalSize();
```

 获取游戏窗口的大小是必需的 UI 编程。 名为 Dip （与设备无关像素），其中 DIP 定义为一英寸的 1/96 的度量给定的窗口大小。 Direct2D 将绘图单位缩放到实际像素绘制发生时，通过使用 Windows 每英寸点数 (DPI) 设置执行此操作。 同样，当你使用[**DirectWrite**](https://msdn.microsoft.com/en-us/library/windows/desktop/dd368038)绘制文本，你指定 Dip 而不是大小的字体。 DIP 表示为浮点数字。

 

### <a name="displaying-game-state-info"></a>显示游戏状态信息

除了提醒显示，该游戏示例有表示六个游戏状态的覆盖层。 所有状态都功能较大的黑色矩形基元，附带供玩家阅读的文本。 因为它们不活动在这些状态，未绘制移动观看控制器矩形和十字线。

创建覆盖层使用[**GameInfoOverlay**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.h)类，从而使我们可以切换出去显示文本的内容，以与游戏的状态保持一致。

![状态和覆盖层的操作](images/simple-dx-game-ui-finaloverlay.png)

覆盖层划分为两个部分：**状态**和**操作**。 **状态**secton 进一步细分为**标题**和**正文**矩形。 **操作**部分仅有一个矩形。 每个矩形都有不同的用途。

-   `titleRectangle` 包含的标题文本。
-   `bodyRectangle` 包含正文文本。
-   `actionRectangle` 包含通知玩家采取具体操作的文本。

该游戏有六个可设置的状态。 传达使用覆盖层的**状态**部分游戏的状态。 使用多种方法使用以下状态相应更新**状态**矩形。

- 正在加载
- 初始的开始菜单高分数统计数据
- 级别开始菜单
- 游戏暂停
- 游戏结束
- 游戏中胜出


使用[**GameInfoOverlay::SetAction**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L522-L564)方法，以使设置为以下任一操作文本，覆盖层的**操作**部分进行更新。
- "点击以再次播放..."
- "级别加载，请稍候..."
- "点击以继续..."
- 无

> [!NOTE]
> 这两种方法将讨论[表示游戏状态](#representing-game-state)部分中进一步。

根据怎么游戏、**状态**和**操作**部分中进行调整文本字段。
让我们看一下如何初始化和绘制这些六个状态的覆盖层。

### <a name="initializing-and-drawing-the-overlay"></a>初始化和绘制覆盖层

**六个状态**具有一些共同特征，使资源和方法所需非常相似。
    - 它们都使用自己的背景为屏幕的中央黑色矩形。
    - 显示的文本是**标题**或**正文**文本。
    - 文本使用 Segoe UI 字体和背景矩形顶部绘制。 


该游戏示例有四种方法创建覆盖时派上用场。
 

#### <a name="gameinfooverlaygameinfooverlay"></a>GameInfoOverlay::GameInfoOverlay
[**GameInfoOverlay::GameInfoOverlay**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L30-L78)构造函数初始化覆盖层，维护位图表面，我们将使用上显示为玩家的信息。 构造函数从传递给它，用来创建覆盖对象本身，可以绘制到[**ID2D1DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/hh404479) [**ID2D1Device**](https://msdn.microsoft.com/library/windows/desktop/hh404478)对象获得一个工厂。 [IDWriteFactory::CreateTextFormat](https://msdn.microsoft.com/en-us/library/windows/desktop/dd368203) 


#### <a name="gameinfooverlaycreatedevicedependentresources"></a>Gameinfooverlay:: Createdevicedependentresources
[**Gameinfooverlay:: Createdevicedependentresources**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L82-L104)是我们用于创建将用于绘制我们的文本的画笔的方法。 若要执行此操作，我们获取一个[**ID2D1DeviceContext2**](https://msdn.microsoft.com/en-us/library/windows/desktop/dn890789)对象，它可以创建并绘制的几何图形，以及功能，例如墨迹和渐变网格呈现。 然后，我们创建一系列的彩色使用[**ID2D1SolidColorBrush**](https://msdn.microsoft.com/en-us/library/windows/desktop/dd372207)绘制 folling UI 元素的画笔。
- 若要获得矩形背景的黑色画笔
- 白色画笔状态文本
- 橙色画笔来操作文本

#### <a name="deviceresourcessetdpi"></a>DeviceResources::SetDpi
[**DeviceResources::SetDpi**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Common/DeviceResources.cpp#L514-L527)方法设置每英寸点数的窗口。 获取调用此方法，当 DPI 更改，并且需要重新调整其时会在调整游戏的窗口大小。 更新后 DPI，此方法还会调用[**deviceresources:: Createwindowsizedependentresources**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Common/DeviceResources.cpp#L214-L487)以确保每次调整窗口大小时，将重新创建必要的资源。


#### <a name="gameinfooverlaycreatewindowssizedependentresources"></a>GameInfoOverlay::CreateWindowsSizeDependentResources
[**GameInfoOverlay::CreateWindowsSizeDependentResources**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L108-L225)方法是我们的所有绘制都发生。 下面是该方法的步骤概述。
- 关闭**标题**、**正文**和**操作**文本的 UI 文本部分创建三个矩形。
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

- 位图创建名为`m_levelBitmap`，当前的 DPI 考虑使用**CreateBitmap**的客户。
- `m_levelBitmap` 因为我们 2D 呈现器目标使用[**ID2D1DeviceContext::SetTarget**](https://msdn.microsoft.com/en-us/library/windows/desktop/hh404533)设置。
- 位图清除所做的每个像素的黑色使用[**ID2D1RenderTarget::Clear**](https://msdn.microsoft.com/en-us/library/windows/desktop/dd371772)。
- [**ID2D1RenderTarget::BeginDraw**](https://msdn.microsoft.com/en-us/library/windows/desktop/dd371768)称为启动绘图。 
- **DrawText**调用来绘制文本存储在`m_titleString`、 `m_bodyString`，并`m_actionString`中使用相应的**ID2D1SolidColorBrush**相应矩形。
- [**ID2D1RenderTarget::EndDraw**](ID2D1RenderTarget::EndDraw)调用以停止上的所有绘图操作`m_levelBitmap`。
- 另一个位图创建使用**CreateBitmap**名为`m_tooSmallBitmap`将用作回退，显示仅显示配置是否对游戏来说太小。
- 重复上绘制的过程`m_levelBitmap`的`m_tooSmallBitmap`，这一次仅绘制字符串`Paused`正文中。




现在，我们只是六个方法，以填充我们六个的覆盖层状态的文本 ！

### <a name="representing-game-state"></a>表示游戏状态


每个游戏中的六个覆盖层状态**GameInfoOverlay**对象中有一个相应的方法。 这些方法绘制覆盖层的变体，以向玩家传达游戏本身的显式信息。 这种通信表示与**标题**和**正文**字符串。 该示例已经配置了资源和布局为它初始化时此信息和[**gameinfooverlay:: Createdevicedependentresources**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L82-L104)方法，因为它只需提供特定于状态的字符串的覆盖层。

通过调用以下方法之一设置覆盖层的**状态**部分。

游戏状态 | 状态 set 方法 | 状态字段
:----- | :------- | :---------
正在加载 | [GameInfoOverlay::SetGameLoading](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L254-L306) |**Title**</br>加载资源 </br>**正文**</br> 以增量方式打印"。"暗示加载活动。
初始的开始菜单高分数统计数据 | [Gameinfooverlay:: Setgamestats](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L310-L354) |**Title**</br>较高分数</br> **正文**</br> 级别完成 # </br>总分数 #</br>总尽在掌握 #
级别开始菜单 | [GameInfoOverlay::SetLevelStart](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L413-L471) |**Title**</br>级别 #</br>**正文**</br>级别目标的说明。
游戏暂停 | [GameInfoOverlay::SetPause](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L475-L502) |**Title**</br>游戏暂停</br>**正文**</br>无
游戏结束 | [GameInfoOverlay::SetGameOver](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L358-L409) |**Title**</br>游戏结束</br> **正文**</br> 级别完成 # </br>总分数 #</br>总尽在掌握 #</br>级别完成 #</br>高分数 #
游戏中胜出 | [GameInfoOverlay::SetGameOver](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L358-L409) |**Title**</br>你赢了！</br> **正文**</br> 级别完成 # </br>总分数 #</br>总尽在掌握 #</br>级别完成 #</br>高分数 #




使用[**GameInfoOverlay::CreateWindowSizeDependentResources**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L117-L134)方法，该示例声明的覆盖层的特定区域对应的三个矩形区域。



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

使用**GameInfoOverlay**对象初始化的 Direct2D 设备上下文，则此方法将填充黑色使用背景画笔标题和正文矩形。 它使用白色文本画笔将“High Score”字符串文本绘制到标题矩形，并将包含更新游戏状态信息的文本绘制到正文矩形。


操作矩形通过从**GameMain**对象，可提供**GameInfoOverlay::SetAction**需确定到正确的消息的游戏状态信息的方法对[**GameInfoOverlay::SetAction**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L522-L564)的后续调用更新玩家，如"点击以继续"。

任何给定状态的覆盖层时选择[**GameMain::SetGameInfoOverlay**](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/GameMain.cpp#L606-L661)方法如下：

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

游戏现在具有一种方法，以向玩家根据游戏状态传达文本信息和我们已切换显示的内容向其整个游戏的一种方法。

### <a name="next-steps"></a>后续步骤

在下一主题[添加控件](tutorial--adding-controls.md)中，我们将介绍玩家如何与游戏示例交互以及输入如何更改游戏状态。



 




