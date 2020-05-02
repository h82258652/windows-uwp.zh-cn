---
description: 用于创建半透明纹理的一种画笔。
title: 亚克力材料
template: detail.hbs
ms.date: 08/09/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: rybick
dev-contact: jevansa
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 9739933f9fd23c6f169c24c4f789e53ba894708d
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "80696633"
---
# <a name="acrylic-material"></a>亚克力材料

![主图](images/header-acrylic.svg)

Acrylic 是一种[画笔](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Brush)，用于创建半透明纹理。 你可以将 Acrylic 应用到应用图面中，并帮助构建视觉层次结构。  <!-- By allowing user-selected wallpaper or colors to shine through, acrylic keeps users in touch with the OS personalization they've chosen. -->

> **重要的 API**：[AcrylicBrush 类](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.acrylicbrush)、[背景属性](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.Background)

:::row:::
    :::column:::
Acrylic 在浅色主题中的应用 ![Acrylic 在浅色主题中的应用](images/Acrylic_LightTheme_Base.png)
    :::column-end:::
    :::column:::
Acrylic 在深色主题中的应用 ![Acrylic 在深色主题中的应用](images/Acrylic_DarkTheme_Base.png)
    :::column-end:::
:::row-end:::

## <a name="acrylic-and-the-fluent-design-system"></a>Acrylic 和 Fluent Design 系统

 Fluent Design System 可帮助你创建包含光线、深度、动画、材料和比例的现代粗体 UI。 Acrylic 是一种 Fluent Design 系统组件，用于在你的应用中添加物理纹理（材料）和深度。 要了解详细信息，请参阅 [UWP 的 Fluent Design 概述](/windows/apps/fluent-design-system)。

 ## <a name="video-summary"></a>视频摘要

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev002/player]

## <a name="examples"></a>示例

:::row:::
    :::column span:::
![部分图像](images/XAML-controls-gallery-app-icon.png)
    :::column-end:::
    :::column span="2":::
**XAML 控件库**<br>
如果已安装 XAML 控件库应用，请单击<a href="xamlcontrolsgallery:/item/Acrylic">此处</a>打开此应用，了解 Acrylic 的实际应用。

<a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用 (Microsoft Store)</a><br>
<a href="https://github.com/Microsoft/Xaml-Controls-Gallery">获取源代码 (GitHub)</a>
    :::column-end:::
:::row-end:::

## <a name="acrylic-blend-types"></a>Acrylic 混合类型
Acrylic 最明显的特征是其透明度。 有两种 Acrylic 混合类型可改变材料透明度：
 - 背景 Acrylic，显示桌面壁纸和当前处于活动状态的应用后的其他窗口，增加了应用程序窗口之间的层次感，同时允许用户进行个性化偏好设置  。
 - 应用内 Acrylic，在应用框架内增加层次感，焦点清晰且层次分明  。

 ![背景 Acrylic](images/BackgroundAcrylic_DarkTheme.png)

 ![应用内 Acrylic](images/AppAcrylic_DarkTheme.png)

 对多层 Acrylic 图面谨慎分层：多层背景 Acrylic 会造成令人分心的视觉错觉。

## <a name="when-to-use-acrylic"></a>何时使用 Acrylic

* 将应用内 Acrylic 用于支持 UI，例如在滚动或交互时可能重叠内容的图面上。
* 将背景 Acrylic 用于瞬态 UI 元素，例如上下文菜单、浮出控件和可轻型消除的 UI。<br />在瞬态场景中使用 Acrylic 有助于维护与触发瞬态 UI 的内容之间的视觉关系。

如果在导航图面上使用的是应用内 Acrylic，则考虑扩展 Acrylic 窗格下的内容，以改善应用上的流。 使用 NavigationView 将自动为你执行此操作。 但为避免产生条纹效果，尽量不要边对边放置多个 Acrylic - 这可能会在两个模糊图面之间产生多余接缝。 Acrylic 这种工具可让设计在视觉上更加协调，但使用不当可能会导致视觉干扰。

考虑以下使用模式，确定将 Acrylic 融入应用的最佳方式：

### <a name="vertical-panes"></a>垂直窗格

对于有助于将应用内容分段的垂直窗格或图面，我们建议使用不透明背景，而不是 Acrylic。 如果垂直窗格在内容上方展开（比如在 NavigationView 的“精简”或“最小”模式下），建议使用应用内 acrylic 来帮助在用户展开此窗格时维护页面的上下文   。

### <a name="transient-surfaces"></a>瞬态图面

对于具有菜单浮出控件、非模式弹出窗口或轻型消除窗格的应用，我们建议使用背景 Acrylic。

![使用信息浮出控件的邮件应用模式](images/Mail_TransientContextMenu.png)

多数控件将默认使用 Acrylic。 调用 [MenuFlyouts](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/menus)、[AutoSuggestBox](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/auto-suggest-box)、[ComboBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.combobox) 和具有轻型消除弹出窗口的类似控件时，它们都将使用瞬态亚克力。

> [!Note]
> 呈现 Acrylic 图面会大量占用 GPU，从而导致设备的功耗增加并缩短电池使用时间。 设备进入节电模式时会自动禁用 Acrylic 效果，并且用户可以选择禁用所有应用的 Acrylic 效果。

## <a name="usability-and-adaptability"></a>可用性和适应性
Acrylic 外观可自动适应各种设备和上下文。

在高对比度模式中，用户仍将看到自己选择的熟悉的背景颜色，而非 Acrylic。 此外，背景 Acrylic 和应用内 Acrylic 均显示为纯色：
 - 用户关闭“设置”>“个性化设置”>“颜色”中的透明度时
 - 启用节电模式时
 - 应用在低端硬件上运行时

此外，只有背景 Acrylic 会将半透明度和纹理替换为纯色：
 - 桌面上的应用窗口停用时
 - UWP 应用在手机、Xbox、HoloLens 或平板电脑模式下运行时

### <a name="legibility-considerations"></a>可读性注意事项
请务必确保应用上显示的任何文本[满足对比率要求](../accessibility/accessible-text-requirements.md)。 我们已优化 Acrylic 设置，因此，增强色的黑色、白色甚至中间色的灰色文本在 Acrylic 上显示时都能满足对比率要求。 平台提供的主题资源的默认对比色调为 80% 不透明度。 在 Acrylic 上放置增强色正文文本时，你可以在降低色调不透明度的同时保持可读性。 在深色模式下，色调不透明度可设为 70%，而在浅色模式下，Acrylic 可满足 50% 不透明度条件下的对比率要求。

不建议在 Acrylic 图面上放置颜色鲜亮的文本，因为这种组合可能无法满足 15 像素字体大小时的最低对比率要求。 请尽量避免在 Acrylic 元素上放置[超链接](../controls-and-patterns/hyperlinks.md)。 此外，如果选择自定义设置 Acrylic 色调颜色或不透明度，而不采用主题资源提供的平台默认设置，请时刻注意可读性。

## <a name="acrylic-theme-resources"></a>Acrylic 主题资源
借助全新的 XAML AcrylicBrush 或预定义的 AcrylicBrush 主题资源，可以在应用图面中轻松应用 Acrylic。 首先，需要确定是使用应用内 Acrylic 还是背景 Acrylic。 请务必仔细阅读上文描述的通用应用模式，以获取实用建议。

我们创建了一套适用于背景 Acrylic 和应用内 Acrylic 类型的画笔主题资源，可根据应用主题进行调整并按需回退为纯色。 名为 AcrylicWindow 的资源表示背景 Acrylic，名为 AcrylicElement 的资源表示应用内 Acrylic   。

<table>
    <tr>
        <th align="center">资源键</th>
        <th align="center">色调不透明度</th>
        <th align="center"><a href="color.md">回滚颜色</a> </th>
    </tr>
    <tr>
        <td> SystemControlAcrylicWindowBrush、SystemControlAcrylicElementBrush <br/> SystemControlChromeLowAcrylicWindowBrush、SystemControlChromeLowAcrylicElementBrush <br/> SystemControlBaseHighAcrylicWindowBrush、SystemControlBaseHighAcrylicElementBrush <br/> SystemControlBaseLowAcrylicWindowBrush、SystemControlBaseLowAcrylicElementBrush <br/> SystemControlAltHighAcrylicWindowBrush、SystemControlAltHighAcrylicElementBrush <br/> SystemControlAltLowAcrylicWindowBrush、SystemControlAltLowAcrylicElementBrush </td>
        <td align="center"> 80% </td>
        <td> ChromeMedium <br/> ChromeLow <br/><br/> BaseHigh <br/><br/> BaseLow <br/><br/> AltHigh <br/><br/> AltLow </td>
    </tr>
    </tr>
        <td> <b>建议用法：</b>下面列出了适用于各种使用情况的常规 Acrylic 资源。 如果应用使用 AltMedium 颜色的次级文本且文本大小小于 18 像素，将 80% Acrylic 资源放在文本后即可<a href="../accessibility/accessible-text-requirements.md">满足对比率要求</a>。 </td>
    </tr>
    <tr>
        <td> SystemControlAcrylicWindowMediumHighBrush、SystemControlAcrylicElementMediumHighBrush <br/> SystemControlBaseHighAcrylicWindowMediumHighBrush、SystemControlBaseHighAcrylicElementMediumHighBrush </td>
        <td align="center"> 70% </td>
        <td> ChromeMedium <br/><br/> BaseHigh </td>
    </tr>
    <tr>
        <td> <b>建议用法：</b>如果应用使用 AltMedium 颜色的次级文本且文本大小不小于 18 像素，可将半透明度更高的 70% Acrylic 资源放在文本后。 建议在应用的顶部水平导航和命令区域使用这些资源。  </td>
    </tr>
    <tr>
        <td> SystemControlChromeHighAcrylicWindowMediumBrush、SystemControlChromeHighAcrylicElementMediumBrush <br/> SystemControlChromeMediumAcrylicWindowMediumBrush、SystemControlChromeMediumAcrylicElementMediumBrush <br/> SystemControlChromeMediumLowAcrylicWindowMediumBrush、SystemControlChromeMediumLowAcrylicElementMediumBrush <br/> SystemControlBaseHighAcrylicWindowMediumBrush、SystemControlBaseHighAcrylicElementMediumBrush <br/> SystemControlBaseMediumLowAcrylicWindowMediumBrush、SystemControlBaseMediumLowAcrylicElementMediumBrush <br/> SystemControlAltMediumLowAcrylicWindowMediumBrush、SystemControlAltMediumLowAcrylicElementMediumBrush  </td>
        <td align="center"> 60% </td>
        <td> ChromeHigh <br/><br/> ChromeMedium <br/><br/> ChromeMediumLow <br/><br/> BaseHigh <br/><br/> BaseLow <br/><br/> AltMediumLow </td>
    </tr>
    <tr>
        <td> <b>建议用法：</b>仅在 Acrylic 上放置 AltHigh 颜色的主要文本时，你的应用可以使用这些 60% 资源。 建议使用 60% Acrylic 绘制应用的<a href="../controls-and-patterns/navigationview.md">垂直导航窗格</a>，即汉堡菜单。 </td>
    </tr>
</table>

除了中性色 Acrylic，我们还添加了使用用户指定主题色的调色 Acrylic 资源。 我们建议谨慎使用经过调色的 Acrylic。 对于提供的 dark1 和 dark2 变体，在资源上放置与深色主题文本颜色一致的白色或浅色文本。
<table>
    <tr>
        <th align="center">资源键</th>
        <th align="center">色调不透明度</th>
        <th align="center"><a href="color.md">色调和回退颜色</a> </th>
    </tr>
    <tr>
        <td> SystemControlAccentAcrylicWindowAccentMediumHighBrush, SystemControlAccentAcrylicElementAccentMediumHighBrush  </td>
        <td align="center"> 70% </td>
        <td> SystemAccentColor </td>
    </tr>
    <tr>
        <td> SystemControlAccentDark1AcrylicWindowAccentDark1Brush、SystemControlAccentDark1AcrylicElementAccentDark1Brush  </td>
        <td align="center"> 80% </td>
        <td> SystemAccentColorDark1 </td>
    </tr>
    <tr>
        <td> SystemControlAccentDark2AcrylicWindowAccentDark2MediumHighBrush、SystemControlAccentDark2AcrylicElementAccentDark2MediumHighBrush  </td>
        <td align="center"> 70% </td>
        <td> SystemAccentColorDark2 </td>
    </tr>
</table>


若要绘制特定图面，请将上述主题资源之一应用到元素背景中，如同应用任何其他画笔资源一样。

```xaml
<Grid Background="{ThemeResource SystemControlAcrylicElementBrush}">
```

## <a name="custom-acrylic-brush"></a>自定义 Acrylic 画笔
你可以选择在应用的 Acrylic 中添加颜色色调，以展示个性化设计或实现与页面其他元素之间的视觉平衡。 若要显示颜色而非灰度，你需要使用以下属性定义属于你自己的 Acrylic 画笔。
 - **TintColor**：颜色/色调覆盖层。 考虑指定 RGB 颜色值和 alpha 通道不透明度。
 - **TintOpacity**：色调层不透明度。 我们建议刚开始时采用 80% 不透明度，尽管不同的颜色在采用其他半透明度时看起来可能更具吸引力。
 - **TintLuminosityOpacity**：控制可从背景穿过 Acrylic 图面的饱和度的量。
 - **BackgroundSource**：指定使用背景 Acrylic 还是应用内 Acrylic 的标记。
 - **FallbackColor**：在节电模式中替换 Acrylic 的纯色。 对于背景 Acrylic，当应用并非位于活动状态桌面窗口中或者应用正在手机和 Xbox 上运行时，回滚颜色也会替换 Acrylic。

![浅色主题 Acrylic 样本](images/CustomAcrylic_Swatches_LightTheme.png)

![深色主题 Acrylic 样本](images/CustomAcrylic_Swatches_DarkTheme.png)

![亮度不透明度与色调不透明度的比较](images/LuminosityVersusTint.png)

若要添加 Acrylic 画笔，请定义用于深色、浅色和高对比度主题的三个资源。 请注意，在高对比度主题中，我们建议使用 x:Key 与深色/浅色 AcrylicBrush 相同的 SolidColorBrush。

> [!Note]
> 如果未指定 TintLuminosityOpacity 值，系统将根据 TintColor 和 TintOpacity 自动调整其值。

```xaml
<ResourceDictionary.ThemeDictionaries>
    <ResourceDictionary x:Key="Default">
        <AcrylicBrush x:Key="MyAcrylicBrush"
            BackgroundSource="HostBackdrop"
            TintColor="#FFFF0000"
            TintOpacity="0.8"
            TintLuminosityOpacity="0.5"
            FallbackColor="#FF7F0000"/>
    </ResourceDictionary>

    <ResourceDictionary x:Key="HighContrast">
        <SolidColorBrush x:Key="MyAcrylicBrush"
            Color="{ThemeResource SystemColorWindowColor}"/>
    </ResourceDictionary>

    <ResourceDictionary x:Key="Light">
        <AcrylicBrush x:Key="MyAcrylicBrush"
            BackgroundSource="HostBackdrop"
            TintColor="#FFFF0000"
            TintOpacity="0.8"
            TintLuminosityOpacity="0.5"
            FallbackColor="#FFFF7F7F"/>
    </ResourceDictionary>
</ResourceDictionary.ThemeDictionaries>
```

下面的示例显示了如何在代码中声明 AcrylicBrush。 如果你的应用支持多个操作系统目标，请务必检查以确认此 API 在用户计算机上可用。

```csharp
if (Windows.Foundation.Metadata.ApiInformation.IsTypePresent("Windows.UI.Xaml.Media.AcrylicBrush"))
{
    Windows.UI.Xaml.Media.AcrylicBrush myBrush = new Windows.UI.Xaml.Media.AcrylicBrush();
    myBrush.BackgroundSource = Windows.UI.Xaml.Media.AcrylicBackgroundSource.HostBackdrop;
    myBrush.TintColor = Color.FromArgb(255, 202, 24, 37);
    myBrush.FallbackColor = Color.FromArgb(255, 202, 24, 37);
    myBrush.TintOpacity = 0.6;

    grid.Fill = myBrush;
}
else
{
    SolidColorBrush myBrush = new SolidColorBrush(Color.FromArgb(255, 202, 24, 37));

    grid.Fill = myBrush;
}
```

## <a name="extend-acrylic-into-the-title-bar"></a>将 Acrylic 扩展到标题栏

若要创建无缝的应用窗口外观，可以将 Acrylic 应用到标题栏区域。 此示例中将 Acrylic 扩展到标题栏的方式是将 [ApplicationViewTitleBar](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewTitleBar) 对象的 [ButtonBackgroundColor](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewTitleBar.ButtonBackgroundColor) 和 [ButtonInactiveBackgroundColor](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewTitleBar.ButtonInactiveBackgroundColor) 属性设置为 [Colors.Transparent](https://docs.microsoft.com/uwp/api/Windows.UI.Colors.Transparent)。

```csharp
private void ExtendAcrylicIntoTitleBar()
{
    CoreApplication.GetCurrentView().TitleBar.ExtendViewIntoTitleBar = true;
    ApplicationViewTitleBar titleBar = ApplicationView.GetForCurrentView().TitleBar;
    titleBar.ButtonBackgroundColor = Colors.Transparent;
    titleBar.ButtonInactiveBackgroundColor = Colors.Transparent;
}
```

此代码可放在应用的 [OnLaunched](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application#Windows_UI_Xaml_Application_OnLaunched_Windows_ApplicationModel_Activation_LaunchActivatedEventArgs_) 方法 (App.xaml.cs) 中、对 [Window.Activate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window.Activate) 的调用后（如此处所示），或应用的第一页中  。

```csharp
// Call your extend acrylic code in the OnLaunched event, after
// calling Window.Current.Activate.
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
    Frame rootFrame = Window.Current.Content as Frame;

    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == null)
    {
        // Create a Frame to act as the navigation context and navigate to the first page
        rootFrame = new Frame();

        rootFrame.NavigationFailed += OnNavigationFailed;

        if (e.PreviousExecutionState == ApplicationExecutionState.Terminated)
        {
            //TODO: Load state from previously suspended application
        }

        // Place the frame in the current Window
        Window.Current.Content = rootFrame;
    }

    if (e.PrelaunchActivated == false)
    {
        if (rootFrame.Content == null)
        {
            // When the navigation stack isn't restored navigate to the first page,
            // configuring the new page by passing required information as a navigation
            // parameter
            rootFrame.Navigate(typeof(MainPage), e.Arguments);
        }
        // Ensure the current window is active
        Window.Current.Activate();

        // Extend acrylic
        ExtendAcrylicIntoTitleBar();
    }
}
```

此外，你需要使用 `CaptionTextBlockStyle` 为应用程序标题（通常自动显示于标题栏）绘制 TextBlock。 有关详细信息，请参阅 [标题栏自定义](../shell/title-bar.md)。

## <a name="dos-and-donts"></a>应做事项和禁止事项
* 请将 Acrylic 用作非主要应用图面（例如导航窗格）的背景材料。
* 请将 Acrylic 扩展到至少一个应用边缘，通过与应用周围环境巧妙融合营造无缝体验。
* 不要将桌面 Acrylic 放在应用的大背景图面上 - 这打破了 Acrylic 主要用于瞬态图面的心智模式。
* 不要直接并列放置应用内 Acrylic 和背景 Acrylic，以避免接缝处产生不协调的视觉效果。
* 不要并列放置具有相同色调和不透明度的多个 Acrylic 窗格，因为这会导致出现不协调的明显接缝。
* 不要将主题色文本放在 Acrylic 图面上。

## <a name="how-we-designed-acrylic"></a>如何设计 Acrylic

我们微调 Acrylic 的关键组件以凸显其独特外观和属性。 我们从半透明度、模糊和噪点设置开始，为平滑图面增添视觉深度和维度。 我们添加了排除混合模式层，以确保放置在 Acrylic 背景上的 UI 的对比度和可读性。 最后，我们添加了各种颜色色调，以供用户进行个性化设置。 这些图层协同作用，形成了全新的实用材料。

![设置](images/AcrylicRecipe_Diagram.jpg)
<br/>Acrylic 设置：背景、模糊、排除混合、颜色/色调覆盖、噪点


## <a name="get-the-sample-code"></a>获取示例代码

- [XAML 控件库示例](https://github.com/Microsoft/Xaml-Controls-Gallery) - 以交互式格式查看所有 XAML 控件。

## <a name="related-articles"></a>相关文章

[突出显示](reveal.md) 
