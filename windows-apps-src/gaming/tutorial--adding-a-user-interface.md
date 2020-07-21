---
title: 添加用户界面
description: 了解如何向 DirectX UWP 游戏添加2D 用户界面覆盖区。
ms.assetid: fa40173e-6cde-b71b-e307-db90f0388485
ms.date: 10/24/2017
ms.topic: article
keywords: windows 10, uwp, 游戏, 用户界面, directx
ms.localizationpriority: medium
ms.openlocfilehash: b6b59bd4f42d31e1f29cc1af298199b42cce6781
ms.sourcegitcommit: 20969781aca50738792631f4b68326f9171a3980
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/26/2020
ms.locfileid: "85409536"
---
# <a name="add-a-user-interface"></a>添加用户界面

> [!NOTE]
> 本主题是[使用 DirectX 教程系列创建简单通用 Windows 平台（UWP）游戏](tutorial--create-your-first-uwp-directx-game.md)的一部分。 该链接上的主题设置了序列的上下文。

现在，我们的游戏已经有了三维视觉对象，可以将重点放在添加一些2D 元素上，使游戏可以向播放机提供游戏状态反馈。 这可以通过在三维图形管道输出的顶层添加简单菜单选项和打印头显示组件来实现。

>[!Note]
>如果尚未下载此示例的最新游戏代码，请参阅[Direct3D 示例游戏](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX)。 此示例是大型 UWP 功能示例集合的一部分。 有关如何下载示例的说明，请参阅[从 GitHub 获取 UWP 示例](/windows/uwp/get-started/get-uwp-app-samples)。

## <a name="objective"></a>目标

使用 Direct2D，将许多用户界面图形和行为添加到了 UWP DirectX 游戏中，其中包括：
- 打印头显示，包括[移动外观控制器](tutorial--adding-controls.md)boundry 矩形
- 游戏状态菜单


## <a name="the-user-interface-overlay"></a>用户界面覆盖层


尽管有很多方法可在 DirectX 游戏中显示文本和用户界面元素，但我们将重点介绍如何使用[Direct2D](/windows/desktop/Direct2D/direct2d-portal)。 我们还将对文本元素使用[DirectWrite](/windows/desktop/DirectWrite/direct-write-portal) 。


Direct2D 是一组2D 绘图 Api，用于绘制基于像素的基元和效果。 在从 Direct2D 开始时，最好保持简单。 复杂的布局和界面行为需要时间和规划。 如果游戏要求使用复杂的用户界面，如模拟和策略游戏中所述，请考虑改用 XAML。

> [!NOTE]
> 有关使用 UWP DirectX 游戏中的 XAML 开发用户界面的信息，请参阅[扩展示例游戏](tutorial-resources.md)。

Direct2D 并非专门针对 HTML 和 XAML 等用户界面或布局而设计。 它不提供用户界面组件，如列表、框或按钮。 它还不提供布局组件，例如 divs、表或网格。


对于本示例，我们有两个主要的 UI 组件。
1. 评分和游戏中控件的显示。
2. 用于显示游戏状态文本和选项（如暂停信息和级别启动选项）的覆盖区。

### <a name="using-direct2d-for-a-heads-up-display"></a>对抬头显示使用 Direct2D

下图显示了该示例的游戏内打印头。 这是一种简单而整洁的，使玩家能够专注于浏览三维世界并拍摄目标。 良好的界面或打印头的显示决不会使播放机处理和响应游戏中的事件的能力变得更加复杂。

![游戏覆盖层的屏幕截图](images/simple-dx-game-ui-overlay.png)

覆盖包含以下基本基元。
- 通知播放机的[**DirectWrite**](/windows/desktop/DirectWrite/direct-write-portal)右上角的文本 
    - 成功的命中数
    - 播放机已进行的快照数
    - 剩余时间
    - 当前级别数 
- 用于形成十字的两个相交线段
- "边界"[移动外观控制器](tutorial--adding-controls.md)底部的两个矩形。 


在[**GameHud**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameHud.h)类的[**GameHud：： Render**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameHud.cpp#L234-L358)方法中绘制了覆盖的游戏内打印头显示状态。 在此方法中，将更新表示用户界面的 Direct2D 覆盖，以反映命中次数、剩余时间和级别编号的变化。

如果游戏已初始化，则将 `TotalHits()` 、和添加 `TotalShots()` `TimeRemaining()` 到[**swprintf_s**](/cpp/c-runtime-library/reference/sprintf-s-sprintf-s-l-swprintf-s-swprintf-s-l)缓冲区，并指定打印格式。 然后，可以使用[**DrawText**](/windows/desktop/Direct2D/id2d1rendertarget-drawtext)方法来绘制它。 我们对当前级别指示器执行相同的操作，绘制空数字以显示未完成的级别（如➀），并填充数字（如➊），以显示已完成的特定级别。


以下代码段演示了的**GameHud：： Render**方法的过程 
- 使用[* * ID2D1RenderTarget 创建位图：:D rawbitmap * *](/windows/desktop/api/d2d1/nf-d2d1-id2d1rendertarget-drawbitmap(id2d1bitmap_constd2d1_rect_f__float_d2d1_bitmap_interpolation_mode_constd2d1_rect_f_))
- 使用[ **D2D1：： RECTF**将 UI 区域分割为个矩形](/windows/desktop/api/dcommon/ns-dcommon-d2d_rect_f)
- 使用**DrawText**生成文本元素

```cppwinrt
void GameHud::Render(_In_ std::shared_ptr<Simple3DGame> const& game)
{
    auto d2dContext = m_deviceResources->GetD2DDeviceContext();
    auto windowBounds = m_deviceResources->GetLogicalSize();

    if (m_showTitle)
    {
        d2dContext->DrawBitmap(
            m_logoBitmap.get(),
            D2D1::RectF(
                GameUIConstants::Margin,
                GameUIConstants::Margin,
                m_logoSize.width + GameUIConstants::Margin,
                m_logoSize.height + GameUIConstants::Margin
                )
            );
        d2dContext->DrawTextLayout(
            Point2F(m_logoSize.width + 2.0f * GameUIConstants::Margin, GameUIConstants::Margin),
            m_titleHeaderLayout.get(),
            m_textBrush.get()
            );
        d2dContext->DrawTextLayout(
            Point2F(GameUIConstants::Margin, m_titleBodyVerticalOffset),
            m_titleBodyLayout.get(),
            m_textBrush.get()
            );
    }

    // Draw text for number of hits, total shots, and time remaining
    if (game != nullptr)
    {
        // This section is only used after the game state has been initialized.
        static const int bufferLength = 256;
        static wchar_t wsbuffer[bufferLength];
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
            m_textFormatBody.get(),
            D2D1::RectF(
                windowBounds.Width - GameUIConstants::HudRightOffset,
                GameUIConstants::HudTopOffset,
                windowBounds.Width,
                GameUIConstants::HudTopOffset + (GameUIConstants::HudBodyPointSize + GameUIConstants::Margin) * 3
                ),
            m_textBrush.get()
            );

        // Using the unicode characters starting at 0x2780 ( ➀ ) for the consecutive levels of the game.
        // For completed levels start with 0x278A ( ➊ ) (This is 0x2780 + 10).
        uint32_t levelCharacter[6];
        for (uint32_t i = 0; i < 6; i++)
        {
            levelCharacter[i] = 0x2780 + i + ((static_cast<uint32_t>(game->LevelCompleted()) == i) ? 10 : 0);
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
            m_textFormatBodySymbol.get(),
            D2D1::RectF(
                windowBounds.Width - GameUIConstants::HudRightOffset,
                GameUIConstants::HudTopOffset + (GameUIConstants::HudBodyPointSize + GameUIConstants::Margin) * 3 + GameUIConstants::Margin,
                windowBounds.Width,
                GameUIConstants::HudTopOffset + (GameUIConstants::HudBodyPointSize + GameUIConstants::Margin) * 4
                ),
            m_textBrush.get()
            );

        if (game->IsActivePlay())
        {
            // Draw the move and fire rectangles
            ...
            // Draw the crosshairs
            ...
        }
    }
}
```

进一步打破方法，此部分[**GameHud：： Render**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameHud.cpp#L320-L358)方法使用[**ID2D1RenderTarget：:D rawrectangle**](/windows/desktop/api/d2d1/nf-d2d1-id2d1rendertarget-drawrectangle(constd2d1_rect_f__id2d1brush_float_id2d1strokestyle))，通过两次调用[**ID2D1RenderTarget：:D rawline**](/windows/desktop/api/d2d1/nf-d2d1-id2d1rendertarget-drawline)来绘制移动和激发矩形。

```cppwinrt
// Check if game is playing
if (game->IsActivePlay())
{
    // Draw a rectangle for the touch input for the move control.
    d2dContext->DrawRectangle(
        D2D1::RectF(
            0.0f,
            windowBounds.Height - GameUIConstants::TouchRectangleSize,
            GameUIConstants::TouchRectangleSize,
            windowBounds.Height
            ),
        m_textBrush.get()
        );
    // Draw a rectangle for the touch input for the fire control.
    d2dContext->DrawRectangle(
        D2D1::RectF(
            windowBounds.Width - GameUIConstants::TouchRectangleSize,
            windowBounds.Height - GameUIConstants::TouchRectangleSize,
            windowBounds.Width,
            windowBounds.Height
            ),
        m_textBrush.get()
        );

    // Draw the cross hairs
    d2dContext->DrawLine(
        D2D1::Point2F(windowBounds.Width / 2.0f - GameUIConstants::CrossHairHalfSize,
            windowBounds.Height / 2.0f),
        D2D1::Point2F(windowBounds.Width / 2.0f + GameUIConstants::CrossHairHalfSize,
            windowBounds.Height / 2.0f),
        m_textBrush.get(),
        3.0f
        );
    d2dContext->DrawLine(
        D2D1::Point2F(windowBounds.Width / 2.0f, windowBounds.Height / 2.0f -
            GameUIConstants::CrossHairHalfSize),
        D2D1::Point2F(windowBounds.Width / 2.0f, windowBounds.Height / 2.0f +
            GameUIConstants::CrossHairHalfSize),
        m_textBrush.get(),
        3.0f
        );
}
```

在**GameHud：： Render**方法中，我们将游戏窗口的逻辑大小存储在 `windowBounds` 变量中。 这将使用 [`GetLogicalSize`](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Common/DeviceResources.h#L41) **DeviceResources**类的方法。 

```cppwinrt
auto windowBounds = m_deviceResources->GetLogicalSize();
```

获取游戏窗口的大小对于 UI 编程至关重要。 窗口的大小以称为 Dip （与设备无关的像素）的度量值为单位提供，其中 DIP 定义为1/96 英寸。 绘图发生时，Direct2D 会将绘图单位缩放为实际像素，并使用 Windows 每英寸点数（DPI）设置。 同样，使用[**DirectWrite**](/windows/desktop/DirectWrite/direct-write-portal)绘制文本时，可以为字体大小指定 dip 而不是点。 DIP 表示为浮点数字。 

### <a name="displaying-game-state-info"></a>显示游戏状态信息

除打印头显示外，示例游戏还具有一个表示六个游戏状态的覆盖区。 所有状态都带有一个大的黑色矩形基元，其中包含供播放机阅读的文本。 不绘制移动外观控制器矩形和十字线，因为它们在这些状态中不处于活动状态。

覆盖是使用[**GameInfoOverlay**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.h)类创建的，这使我们能够确定显示哪种文本以与游戏状态保持一致。

![覆盖的状态和操作](images/simple-dx-game-ui-finaloverlay.png)

覆盖分为两部分：**状态**和**操作**。 **状态**secton 进一步细分为**标题**和**正文**矩形。 **操作**部分只有一个矩形。 每个矩形的用途不同。

-   `titleRectangle`包含标题文本。
-   `bodyRectangle`包含正文文本。
-   `actionRectangle`包含通知播放机执行特定操作的文本。

游戏具有六个可以设置的状态。 使用重叠**状态**部分传达的游戏状态。 使用与以下状态相对应的多种方法更新**状态**矩形。

- 加载
- 初始开始/高分数统计
- 级别开始
- 已暂停游戏
- 游戏结束
- 游戏获胜


使用[**GameInfoOverlay：： SetAction**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L522-L564)方法更新覆盖区的**操作**部分，允许将操作文本设置为以下其中一项。
- "再次点击播放 ..."
- "级别正在加载，请稍候 ..."
- "点击以继续 ..."
- 无

> [!NOTE]
> 这两种方法将在[表示游戏状态](#representing-game-state)部分进一步讨论。

根据游戏中的操作，"**状态**" 和 "**操作**" 部分文本字段会进行调整。
让我们看看如何初始化并绘制这六种状态的覆盖层。

### <a name="initializing-and-drawing-the-overlay"></a>初始化和绘制覆盖层

六种**状态**状态具有几个常见功能，使其所需的资源和方法非常相似。
    - 它们都使用屏幕中心的黑色矩形作为其背景。
    - 显示的文本为**标题**或**正文**文本。
    - 该文本使用 Segoe UI 字体并在后端矩形的顶部绘制。 


示例游戏在创建覆盖层时有四种方法。
 

#### <a name="gameinfooverlaygameinfooverlay"></a>GameInfoOverlay::GameInfoOverlay
[**GameInfoOverlay：： GameInfoOverlay**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L30-L78)构造函数初始化覆盖面，并维护将用于向播放机显示信息的位图图面。 构造函数从传递给它的[**ID2D1Device**](/windows/desktop/api/d2d1_1/nn-d2d1_1-id2d1device)对象获取工厂，它使用它来创建覆盖对象自身可以绘制到的[**ID2D1DeviceContext**](/windows/desktop/api/d2d1_1/nn-d2d1_1-id2d1devicecontext) 。 [IDWriteFactory::CreateTextFormat](/windows/desktop/api/dwrite/nf-dwrite-idwritefactory-createtextformat) 


#### <a name="gameinfooverlaycreatedevicedependentresources"></a>GameInfoOverlay::CreateDeviceDependentResources
[**GameInfoOverlay：： CreateDeviceDependentResources**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L82-L104)是用于创建画笔的方法，用于绘制文本。 为此，我们将获取一个[**ID2D1DeviceContext2**](/windows/desktop/api/d2d1_3/nn-d2d1_3-id2d1devicecontext2)对象，该对象可用于创建和绘制几何图形，以及墨迹和渐变网格呈现等功能。 然后，使用[**ID2D1SolidColorBrush**](/windows/desktop/api/d2d1/nn-d2d1-id2d1solidcolorbrush)创建一系列彩色画笔来绘制 folling UI 元素。
- 矩形背景的黑色画笔
- 白色画笔状态文本
- 操作文本的橙色画笔

#### <a name="deviceresourcessetdpi"></a>DeviceResources：： SetDpi

[**DeviceResources：： SetDpi**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Common/DeviceResources.cpp#L514-L527)方法设置窗口的每英寸点数。 此方法在 DPI 更改时调用，需要重新调整，当调整游戏窗口大小时，会发生这种情况。 更新 DPI 后，此方法还会调用[**DeviceResources：： CreateWindowSizeDependentResources**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Common/DeviceResources.cpp#L214-L487) ，以确保每次调整窗口大小时都重新创建必要的资源。

#### <a name="gameinfooverlaycreatewindowssizedependentresources"></a>GameInfoOverlay::CreateWindowsSizeDependentResources

[**GameInfoOverlay：： CreateWindowsSizeDependentResources**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L108-L225)方法是所有绘图发生的位置。 下面是该方法步骤的概述。
- 将为**标题**、**正文**和**操作**文本的 UI 文本部分创建三个矩形。
    ```cppwinrt 
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

- 创建名为的位图 `m_levelBitmap` ，并使用**CreateBitmap**将当前 DPI 命名为帐户。
- `m_levelBitmap`使用[**ID2D1DeviceContext：： SetTarget**](/windows/desktop/api/d2d1_1/nf-d2d1_1-id2d1devicecontext-settarget)设置为2d 呈现器目标。
- 使用[**ID2D1RenderTarget：： Clear**](/windows/desktop/api/d2d1/nf-d2d1-id2d1rendertarget-clear)使每个像素变为黑色时，将清除位图。
- 调用[**ID2D1RenderTarget：： BeginDraw**](/windows/desktop/api/d2d1/nf-d2d1-id2d1rendertarget-begindraw)以启动绘制。 
- 调用**DrawText** `m_titleString` ，以 `m_bodyString` `m_actionString` 使用相应的**ID2D1SolidColorBrush**绘制 approperiate 矩形中存储的文本。
- 调用[**ID2D1RenderTarget：： EndDraw**](ID2D1RenderTarget::EndDraw)以停止中的所有绘制操作 `m_levelBitmap` 。
- 使用名为的**CreateBitmap**创建另一个位图 `m_tooSmallBitmap` 作为回退，仅当显示配置对于游戏太小时才显示。
- 重复此过程以便在上 `m_levelBitmap` 进行绘制 `m_tooSmallBitmap` ，这一次只绘制 `Paused` 正文中的字符串。




现在，我们只需六种方法来填充六个覆盖状态的文本！

### <a name="representing-game-state"></a>表示游戏状态


游戏中六个覆盖状态的每个状态在**GameInfoOverlay**对象中都有相应的方法。 这些方法绘制覆盖层的变体，以向玩家传达游戏本身的显式信息。 此通信使用**标题**和**正文**字符串来表示。 由于示例已在初始化时为此信息配置了资源和布局，并且具有[**GameInfoOverlay：： CreateDeviceDependentResources**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L82-L104)方法，因此它只需提供覆盖状态特定字符串。

覆盖的**状态**部分是通过调用以下方法之一设置的。

游戏状态 | 状态集方法 | 状态字段
:----- | :------- | :---------
加载 | [GameInfoOverlay::SetGameLoading](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L254-L306) |**标题**</br>加载资源 </br>**正文**</br> 以增量方式打印 "." 以表示加载活动。
初始开始/高分数统计 | [GameInfoOverlay::SetGameStats](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L310-L354) |**标题**</br>高分</br> **正文**</br> 已完成级别# </br>总点数#</br>照片总数#
级别开始 | [GameInfoOverlay::SetLevelStart](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L413-L471) |**标题**</br>调配#</br>**正文**</br>级别目标说明。
已暂停游戏 | [GameInfoOverlay::SetPause](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L475-L502) |**标题**</br>已暂停游戏</br>**正文**</br>无
游戏结束 | [GameInfoOverlay::SetGameOver](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L358-L409) |**标题**</br>游戏结束</br> **正文**</br> 已完成级别# </br>总点数#</br>照片总数#</br>已完成级别#</br>高分#
游戏获胜 | [GameInfoOverlay::SetGameOver](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L358-L409) |**标题**</br>你赢了！</br> **正文**</br> 已完成级别# </br>总点数#</br>照片总数#</br>已完成级别#</br>高分#

对于[**GameInfoOverlay：： CreateWindowSizeDependentResources**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L117-L134)方法，示例声明了三个对应于重叠的特定区域的矩形区域。

记住这些覆盖层后，下面我们了解一个特定于状态的方法 **GameInfoOverlay::SetGameStats**，并了解如何绘制覆盖层。

```cppwinrt
void GameInfoOverlay::SetGameStats(int maxLevel, int hitCount, int shotCount)
{
    int length;

    auto d2dContext = m_deviceResources->GetD2DDeviceContext();

    d2dContext->SetTarget(m_levelBitmap.get());
    d2dContext->BeginDraw();
    d2dContext->SetTransform(D2D1::Matrix3x2F::Identity());
    d2dContext->FillRectangle(&m_titleRectangle, m_backgroundBrush.get());
    d2dContext->FillRectangle(&m_bodyRectangle, m_backgroundBrush.get());
    m_titleString = L"High Score";

    d2dContext->DrawText(
        m_titleString.c_str(),
        m_titleString.size(),
        m_textFormatTitle.get(),
        m_titleRectangle,
        m_textBrush.get()
        );
    length = swprintf_s(
        wsbuffer,
        bufferLength,
        L"Levels Completed %d\nTotal Points %d\nTotal Shots %d",
        maxLevel,
        hitCount,
        shotCount
        );
    m_bodyString = std::wstring(wsbuffer, length);
    d2dContext->DrawText(
        m_bodyString.c_str(),
        m_bodyString.size(),
        m_textFormatBody.get(),
        m_bodyRectangle,
        m_textBrush.get()
        );

    // We ignore D2DERR_RECREATE_TARGET here. This error indicates that the device
    // is lost. It will be handled during the next call to Present.
    HRESULT hr = d2dContext->EndDraw();
    if (hr != D2DERR_RECREATE_TARGET)
    {
        // The D2DERR_RECREATE_TARGET indicates there has been a problem with the underlying
        // D3D device. All subsequent rendering will be ignored until the device is recreated.
        // This error will be propagated and the appropriate D3D error will be returned from the
        // swapchain->Present(...) call. At that point, the sample will recreate the device
        // and all associated resources. As a result, the D2DERR_RECREATE_TARGET doesn't
        // need to be handled here.
        winrt::check_hresult(hr);
    }
}
```

使用**GameInfoOverlay**对象初始化的 Direct2D 设备上下文，此方法使用背景画笔以黑色填充标题和正文矩形。 它使用白色文本画笔将“High Score”字符串文本绘制到标题矩形，并将包含更新游戏状态信息的文本绘制到正文矩形。


操作矩形由对**GameMain**对象的方法中的[**GameInfoOverlay：： SetAction**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L522-L564)的后续调用进行更新，该方法提供**GameInfoOverlay：： SetAction**确定要向播放机发送正确消息所需的游戏状态信息，如 "点击以继续"。

在[**GameMain：： SetGameInfoOverlay**](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/GameMain.cpp#L606-L661)方法中选择了任何给定状态的覆盖，如下所示：

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

现在，游戏可以根据游戏状态向播放机传达文本信息，并且我们有一种方法可以切换在游戏中显示的内容。

### <a name="next-steps"></a>后续步骤

在下一主题中，[添加控件](tutorial--adding-controls.md)，我们将探讨播放机如何与示例游戏交互，以及输入如何改变游戏状态。
