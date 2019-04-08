---
description: 一种创建半透明纹理的画笔。
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
ms.openlocfilehash: 0600e66c672a28683befdb7b0090f5455a28c948
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57624192"
---
# <a name="acrylic-material"></a>亚克力材料

![主图](images/header-acrylic.svg)

染料是一种[画笔](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Media.Brush)创建半透明纹理。 你可以将亚克力应用到应用图面中，并帮助构建视觉层次结构。  <!-- By allowing user-selected wallpaper or colors to shine through, acrylic keeps users in touch with the OS personalization they've chosen. -->

> **重要的 API**：[AcrylicBrush 类](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.media.acrylicbrush)，[后台属性](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.control.Background)

:::row:::
    :::column:::
        Acrylic in light theme
        ![Acrylic in light theme](images/Acrylic_LightTheme_Base.png)
    :::column-end:::
    :::column:::
        Acrylic in dark theme
        ![Acrylic in dark theme](images/Acrylic_DarkTheme_Base.png)
    :::column-end:::
:::row-end:::

## <a name="acrylic-and-the-fluent-design-system"></a>亚克力和 Fluent 设计系统

 Fluent 设计系统可帮助你创建包含光线、深度、动画、材料和比例的现代、粗体 UI。 亚克力是一种 Fluent 设计系统组件，用于在你的应用中添加物理纹理（材料）和深度。 要了解详细信息，请参阅 [UWP 的 Fluent Design 概述](../fluent-design-system/index.md)。

 ## <a name="video-summary"></a>视频摘要

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev002/player]

## <a name="examples"></a>示例

:::row:::
    :::column span:::
        ![Some image](images/XAML-controls-gallery-app-icon.png)
    :::column-end:::
    :::column span="2":::
        **XAML Controls Gallery**<br>
        If you have the XAML Controls Gallery app installed, click <a href="xamlcontrolsgallery:/item/Acrylic">here</a> to open the app and see acrylic in action.

        <a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Get the XAML Controls Gallery app (Microsoft Store)</a><br>
        <a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Get the source code (GitHub)</a>
    :::column-end:::
:::row-end:::

## <a name="acrylic-blend-types"></a>亚克力混合类型
亚克力的最明显特征是透明度。 有两种亚克力混合类型可改变材料透明度：
 - **背景亚克力**显示桌面壁纸和当前处于活动状态的应用后的其他窗口，增加了应用程序窗口之间的层次感，同时允许用户进行个性化偏好设置。
 - **应用内亚克力**在应用框架内增加层次感，焦点清晰且层次分明。

 ![背景亚克力](images/BackgroundAcrylic_DarkTheme.png)

 ![应用内亚克力](images/AppAcrylic_DarkTheme.png)

 层时要注意的多个丙烯画还： 背景染料的多个层可以创建让人分散注意力光学戏法。

## <a name="when-to-use-acrylic"></a>何时使用亚克力

* 使用应用程序内染料以支持 UI，例如可能会重叠时可以滚动，又与之交互的内容的图面上。
* 使用背景染料的瞬时 UI 元素，如上下文菜单中，浮出控件和 light dimsissable UI。<br />在临时方案中使用染料有助于保持与触发瞬时 UI 的内容的可视化关系。

如果将应用程序内染料导航图面上，可以考虑扩展来提高您的应用程序上的流丙烯画窗格下方的内容。 使用 NavigationView 将自动执行此操作为您。 但是，若要避免创建条带化效果，不尝试将多个小的丙烯画边到边-这可以创建两个经过模糊处理的图面之间不需要的接缝。 染料是一个工具，可向设计，使 visual 协调工作，但如果使用不正确，可能会导致视觉干扰。

请考虑以下的使用模式，来决定如何最好地将染料合并到您的应用程序：

### <a name="horizontal-navigation-or-commanding"></a>水平导航栏或命令

如果您的应用程序能够利用 NavigationView 并不打算添加染料，我们建议使用相对半透明染料 60%色彩透明度。
 - 窗格以覆盖形式在其他应用内容上打开时，应设置为 [60% 应用内亚克力](#acrylic-theme-resources)

![使用应用程序内水平命令的地图应用](images/Maps_In_App_Acrylic_1.png)

此外，你的内容扩展或下染料的滚动顶部会为但您的应用程序更令人着迷、 无缝体验。

### <a name="vertical-panes"></a>垂直窗格

对于垂直窗格或帮助您的应用程序关闭内容部分的图面，我们建议你使用而不是染料的不透明背景。 如果垂直窗格内容之上打开，例如在 NavigationView 的**Compact**或**最小**模式下，我们建议使用应用程序内染料来帮助维护页面的上下文，当用户打开此窗格。

### <a name="transient-surfaces"></a>暂时性的图面

使用菜单浮出控件，非模式弹出窗口中，为应用或轻量解除窗格中，我们建议使用背景染料。

![使用信息性的浮出控件的邮件应用程序模式](images/Mail_TransientContextMenu.png)

默认情况下，许多我们控件将使用染料。 [MenuFlyouts](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/menus)， [AutoSuggestBox](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/auto-suggest-box)，[组合框](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.combobox)和 light dimiss 弹出类似控件都将使用暂时性染料时调用它们。

> [!Note]
> 呈现丙烯画的图面是 GPU 密集型的这会增加设备的功率消耗并缩短电池使用寿命。 Acrylic 效果被自动禁用设备进入节电模式和用户可以禁用的所有应用的丙烯画效果时，如果用户选择。

## <a name="usability-and-adaptability"></a>可用性和适应性
亚克力外观可自动适应各种设备和上下文。

在高对比度模式中，用户仍将看到自己选择的熟悉的背景颜色，而非亚克力。 此外，背景染料和应用程序内染料显示纯色为：
 - 当用户关闭中设置的透明度 > 个性化设置 > 颜色
 - 当激活节电模式
 - 应用在低端硬件上运行时

此外，仅背景染料会将其半透明度和纹理纯色为：
 - 桌面上的应用窗口停用时
 - UWP 应用在手机、Xbox、HoloLens 或平板电脑模式下运行时

### <a name="legibility-considerations"></a>可读性注意事项
请务必确保应用上显示的任何文本[满足对比率要求](../accessibility/accessible-text-requirements.md)。 我们已优化亚克力设置，因此，增强色的黑色、白色甚至中间色的灰色文本在亚克力上显示时都能满足对比率要求。 平台提供的主题资源的默认对比色调为 80% 不透明度。 在亚克力上放置增强色正文文本时，你可以在降低色调不透明度的同时保持可读性。 在深色模式下，色调不透明度可设为 70%，而在浅色模式下，亚克力可满足 50% 不透明度条件下的对比率要求。

我们不建议在亚克力图面上放置颜色鲜亮的文本，因为这种组合可能无法满足 15 像素字体大小时的最低对比率要求。 请尽量避免在亚克力元素上放置[超链接](../controls-and-patterns/hyperlinks.md)。 此外，如果你选择自定义设置亚克力色调颜色或不透明度，而不采用主题资源提供的平台默认设置，请时刻注意可读性。

## <a name="acrylic-theme-resources"></a>亚克力主题资源
借助全新的 XAML AcrylicBrush 或预定义的 AcrylicBrush 主题资源，你可以在应用图面中轻松应用亚克力。 首先，你需要确定是使用应用内亚克力还是背景亚克力。 请务必仔细阅读上文描述的通用应用模式，以获取实用建议。

我们创建了一套适用于背景亚克力和应用内亚克力类型的画笔主题资源，可根据应用主题进行调整并按需回退为纯色。 名为 *AcrylicWindow* 的资源表示背景亚克力，名为 *AcrylicElement* 的资源表示应用内亚克力。

<table>
    <tr>
        <th align="center">资源键</th>
        <th align="center">色调不透明度</th>
        <th align="center"><a href="color.md">回退颜色</a> </th>
    </tr>
    <tr>
        <td> SystemControlAcrylicWindowBrush、SystemControlAcrylicElementBrush <br/> SystemControlChromeLowAcrylicWindowBrush、SystemControlChromeLowAcrylicElementBrush <br/> SystemControlBaseHighAcrylicWindowBrush、SystemControlBaseHighAcrylicElementBrush <br/> SystemControlBaseLowAcrylicWindowBrush、SystemControlBaseLowAcrylicElementBrush <br/> SystemControlAltHighAcrylicWindowBrush、SystemControlAltHighAcrylicElementBrush <br/> SystemControlAltLowAcrylicWindowBrush、SystemControlAltLowAcrylicElementBrush </td>
        <td align="center"> 80% </td>
        <td> ChromeMedium <br/> ChromeLow <br/><br/> BaseHigh <br/><br/> BaseLow <br/><br/> AltHigh <br/><br/> AltLow </td>
    </tr>
    </tr>
        <td> <b>建议的用法：</b>这些是通用的丙烯画资源的各种用法良好。 如果你的应用使用 AltMedium 颜色的次级文本且文本大小小于 18 像素，将 80% 亚克力资源放在文本后即可<a href="../accessibility/accessible-text-requirements.md">满足对比率要求</a>。 </td>
    </tr>
    <tr>
        <td> SystemControlAcrylicWindowMediumHighBrush、SystemControlAcrylicElementMediumHighBrush <br/> SystemControlBaseHighAcrylicWindowMediumHighBrush、SystemControlBaseHighAcrylicElementMediumHighBrush </td>
        <td align="center"> 70% </td>
        <td> ChromeMedium <br/><br/> BaseHigh </td>
    </tr>
    <tr>
        <td> <b>建议的用法：</b>如果您的应用程序使用辅助文本 AltMedium 颜色的文本大小或更大的 18px，放置这些更半透明 70%丙烯画资源文本的背景。 我们建议在应用的顶部水平导航和命令区域使用这些资源。  </td>
    </tr>
    <tr>
        <td> SystemControlChromeHighAcrylicWindowMediumBrush、SystemControlChromeHighAcrylicElementMediumBrush <br/> SystemControlChromeMediumAcrylicWindowMediumBrush、SystemControlChromeMediumAcrylicElementMediumBrush <br/> SystemControlChromeMediumLowAcrylicWindowMediumBrush、SystemControlChromeMediumLowAcrylicElementMediumBrush <br/> SystemControlBaseHighAcrylicWindowMediumBrush、SystemControlBaseHighAcrylicElementMediumBrush <br/> SystemControlBaseMediumLowAcrylicWindowMediumBrush、SystemControlBaseMediumLowAcrylicElementMediumBrush <br/> SystemControlAltMediumLowAcrylicWindowMediumBrush、SystemControlAltMediumLowAcrylicElementMediumBrush  </td>
        <td align="center"> 60% </td>
        <td> ChromeHigh <br/><br/> ChromeMedium <br/><br/> ChromeMediumLow <br/><br/> BaseHigh <br/><br/> BaseLow <br/><br/> AltMediumLow </td>
    </tr>
    <tr>
        <td> <b>建议的用法：</b>通过染料放置仅主要文本 AltHigh 颜色的您的应用程序可以利用这些 60%的资源。 我们建议你使用 60% 亚克力绘制应用的<a href="../controls-and-patterns/navigationview.md">垂直导航窗格</a>，即汉堡菜单。 </td>
    </tr>
</table>

除了中性色亚克力以外，我们还添加了使用用户指定主题色的调色亚克力资源。 我们建议谨慎使用经过调色的亚克力。 对于提供的 dark1 和 dark2 变体，在资源上放置与深色主题文本颜色一致的白色或浅色文本。
<table>
    <tr>
        <th align="center">资源键</th>
        <th align="center">色调不透明度</th>
        <th align="center"><a href="color.md">浅色和回退颜色</a> </th>
    </tr>
    <tr>
        <td> SystemControlAccentAcrylicWindowAccentMediumHighBrush, SystemControlAccentAcrylicElementAccentMediumHighBrush  </td>
        <td align="center"> 70% </td>
        <td> SystemAccentColor </td>
    </tr>
    <tr>
        <td> SystemControlAccentDark1AcrylicWindowAccentDark1Brush, SystemControlAccentDark1AcrylicElementAccentDark1Brush  </td>
        <td align="center"> 80% </td>
        <td> SystemAccentColorDark1 </td>
    </tr>
    <tr>
        <td> SystemControlAccentDark2AcrylicWindowAccentDark2MediumHighBrush, SystemControlAccentDark2AcrylicElementAccentDark2MediumHighBrush  </td>
        <td align="center"> 70% </td>
        <td> SystemAccentColorDark2 </td>
    </tr>
</table>


若要绘制特定图面，请将上述主题资源之一应用到元素背景中，如同应用任何其他画笔资源一样。

```xaml
<Grid Background="{ThemeResource SystemControlAcrylicElementBrush}">
```

## <a name="custom-acrylic-brush"></a>自定义亚克力画笔
你可以选择在应用的亚力克中添加颜色色调，以展示个性化设计或实现与页面其他元素之间的视觉平衡。 若要显示颜色而非灰度，你需要使用以下属性定义属于你自己的亚克力画笔。
 - **TintColor**：颜色/色调覆盖层。 考虑指定 RGB 颜色值和 alpha 通道不透明度。
 - **TintOpacity**：色调层不透明度。 虽然不同的颜色可能看起来在其他 translucencies 更具说服力，我们会作为起点，建议 80%的不透明度。
 - **TintLuminosityOpacity**： 控制的允许通过在后台从丙烯画的外围应用的饱和的量。
 - **BackgroundSource**：指定使用背景亚克力还是应用内亚克力的标记。
 - **FallbackColor**： 替换在 Battery Saver 染料的纯色。 对于背景亚克力，当应用并非位于活动状态桌面窗口中或者应用正在手机和 Xbox 上运行时，回退颜色也会替换亚克力。

![浅色主题亚克力样本](images/CustomAcrylic_Swatches_LightTheme.png)

![深色主题亚克力样本](images/CustomAcrylic_Swatches_DarkTheme.png)

![亮度 opactity 相比色彩不透明度](images/LuminosityVersusTint.png)

若要添加亚克力画笔，请定义用于深色、浅色和高对比度主题的三个资源。 请注意，在高对比度主题中，我们建议使用 x:Key 与深色/浅色 AcrylicBrush 相同的 SolidColorBrush。

> [!Note] 
> 如果未指定 TintLuminosityOpacity 值，系统将自动调整根据 TintColor 和 TintOpacity 其值。

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
if (Windows.Foundation.Metadata.ApiInformation.IsTypePresent("Windows.UI.Xaml.Media.XamlCompositionBrushBase"))
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

## <a name="extend-acrylic-into-the-title-bar"></a>将亚克力扩展到标题栏

若要创建无缝的应用窗口外观，可以将亚克力应用到标题栏区域。 此示例中将亚克力扩展到标题栏的方式是将 [ApplicationViewTitleBar](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewTitleBar) 对象的 [ButtonBackgroundColor](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewTitleBar.ButtonBackgroundColor) 和 [ButtonInactiveBackgroundColor](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewTitleBar.ButtonInactiveBackgroundColor) 属性设置为 [Colors.Transparent](https://docs.microsoft.com/uwp/api/Windows.UI.Colors.Transparent)。

```csharp
private void ExtendAcrylicIntoTitleBar()
{
    CoreApplication.GetCurrentView().TitleBar.ExtendViewIntoTitleBar = true;
    ApplicationViewTitleBar titleBar = ApplicationView.GetForCurrentView().TitleBar;
    titleBar.ButtonBackgroundColor = Colors.Transparent;
    titleBar.ButtonInactiveBackgroundColor = Colors.Transparent;
}
```

此代码可放在应用的 [OnLaunched](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application#Windows_UI_Xaml_Application_OnLaunched_Windows_ApplicationModel_Activation_LaunchActivatedEventArgs_) 方法 (_App.xaml.cs_) 中、对 [Window.Activate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window.Activate) 的调用后（如此处所示），或应用的第一页中。

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
* 请将亚克力用作非主要应用图面（例如导航窗格）的背景材料。
* 请将亚克力扩展到至少一个应用边缘，通过与应用周围环境巧妙融合营造无缝体验。
* 不要将桌面 arylic 放在您的应用程序的较大的背景图面上-这会破坏染料主要用于临时图面心理模型。
* 不要直接并列放置应用内亚克力和背景亚克力，以避免接缝处产生不协调的视觉效果。
* 不要并列放置具有相同色调和不透明度的多个亚克力窗格，因为这会导致出现不协调的明显接缝。
* 不要将主题色文本放在亚克力图面上。

## <a name="how-we-designed-acrylic"></a>如何设计亚克力

我们微调亚克力的关键组件以凸显其独特外观和属性。 我们开始使用半透明度、 模糊和干扰向平面图面添加视觉深度和维度。 我们添加了排除混合模式层，以确保放置在亚克力背景上的 UI 的对比度和可读性。 最后，我们添加了各种颜色色调，以供用户进行个性化设置。 这些图层协同作用，形成了全新的实用材料。

![Acrylic 方案](images/AcrylicRecipe_Diagram.jpg)
<br/>亚克力设置：背景、模糊、排除混合、颜色/色调覆盖、噪点


## <a name="get-the-sample-code"></a>获取示例代码

- [XAML 控件库示例](https://github.com/Microsoft/Xaml-Controls-Gallery) - 以交互式格式查看所有 XAML 控件。

## <a name="related-articles"></a>相关文章

[**显示突出显示**](reveal.md)
