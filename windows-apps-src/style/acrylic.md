---
author: mijacobs
description: 
title: "亚克力材料"
template: detail.hbs
ms.author: mijacobs
ms.date: 08/9/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: rybick
dev-contact: jevansa
doc-status: Published
ms.openlocfilehash: 01c8d1bd961a5246a052d1dc7a746257687104e4
ms.sourcegitcommit: de6bc8acec2cd5ebc36bb21b2ce1a9980c3e78b2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/17/2017
---
# <a name="acrylic-material"></a>亚克力材料
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

> [!IMPORTANT]
> 本文介绍的功能尚未发布，在商业发行之前可能发生重大修改。 Microsoft 不对此处提供的信息作任何明示或默示的担保。

亚克力是一种[画笔](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Media.Brush)，用于创建半透明纹理。 你可以将亚克力应用到应用图面中，并帮助构建视觉层次结构。  <!-- By allowing user-selected wallpaper or colors to shine through, Acrylic keeps users in touch with the OS personalization they've chosen. -->

> **重要的 API**：[AcrylicBrush 类](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.media.acrylicbrush)、[背景属性](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_Background)


![浅色主题中的亚克力](images/Acrylic_DarkTheme_Base.png)

![深色主题中的亚克力](images/Acrylic_LightTheme_Base.png)

## <a name="acrylic-and-the-fluent-design-system"></a>亚克力和 Fluent 设计系统

 Fluent 设计系统可帮助你创建包含光线、深度、动画、材料和比例的现代、粗体 UI。 亚克力是一个 Fluent 设计系统组件，在你的应用中添加物理纹理（材料）和深度。 

## <a name="when-to-use-acrylic"></a>何时使用亚克力

我们建议将支持 UI（例如应用内导航或命令元素）放在亚克力图面上。 此材料对于对话框和浮出控件等瞬态 UI 元素也十分有用，因为它可帮助维持与触发瞬态 UI 的内容之间的视觉关联。 亚克力适合用作背景材料，在视觉上独立的窗格中显示，因此不要将亚克力应用于细节丰富的前景元素。

主要应用内容后的图面应使用纯色不透明背景。

考虑将亚克力扩展到一个或多个应用边缘（包括窗口标题栏）以改进视觉流。 避免通过堆叠不同混合类型的亚克力而导致产生条纹效果。 亚克力这种工具可让设计在视觉上更加协调，但使用不当可能会导致视觉干扰。

考虑以下使用模式，确定将亚克力融入应用的最佳方式。

### <a name="vertical-acrylic-pane"></a>垂直亚克力窗格

如应用带有垂直导航，我们建议将亚克力应用于包含导航元素的次级窗格。

![使用单个垂直亚克力窗格的应用模式](images/acrylic_app-pattern_vertical.png)

[NavigationView](../controls-and-patterns/navigationview.md) 是一种新的常用控件，用于在应用中添加导航，其视觉设计采用了亚克力。 NavigationView 的窗格在窗格并排打开并显示主要内容时显示背景亚克力，在窗格以覆盖形式打开时自动转换为应用内亚克力。

如果你的应用无法使用 NavigationView，而你计划自行添加亚克力，我们建议使用相对透明的亚克力（色调不透明度 60%）。
 - 窗格以覆盖形式在其他应用内容上打开时，应设置为 [60% 应用内亚克力](#acrylic-theme-resources)
 - 窗格并排打开并显示主要应用内容时，应设置为 [60% 背景亚克力](#acrylic-theme-resources)

### <a name="multiple-acrylic-panes"></a>多个亚克力窗格

如应用带有三个不同的垂直窗格，我们建议将亚克力应用于非主要内容。
 - 对于最靠近主要内容的次级窗格，使用 [80% 背景亚克力](#acrylic-theme-resources)
 - 对于远离主要内容的三级窗格，使用 [60% 背景亚克力](#acrylic-theme-resources)

![使用两个垂直亚克力窗格的应用模式](images/acrylic_app-pattern_double-vertical.png)

### <a name="horizontal-acrylic-pane"></a>水平亚克力窗格

如应用顶部带有水平导航、命令或其他明显的水平元素，我们建议对此类视觉元素应用 [70% 亚克力](#acrylic-theme-resources)。

![使用水平亚克力窗格的应用模式](images/acrylic_app-pattern_horizontal.png)

以连续、可缩放内容为重点的画布应用应在顶部栏中使用应用内亚克力，以便于用户连接此类内容。 画布应用包括地图、绘画和绘图等。

如应用没有单个连续画布，我们建议使用背景亚克力，从而将用户连接到他们的整体桌面环境。

### <a name="acrylic-in-utility-apps"></a>实用程序应用中的亚克力

小组件或轻型应用可以将亚克力边缘到边缘拖放到它们的应用窗口中，从而增强它们作为实用程序应用的用途。 属于此类别的应用通常具有短暂的用户参与时间，且不可能占用用户的整个桌面的屏幕。 示例包括计算器和操作中心。

![计算器实用程序将亚克力用作其整个背景](images/acrylic_app-pattern_full.png)

> [!Note]
> 呈现亚克力图面会大量占用 GPU，从而导致设备的功耗增加并缩短电池使用时间。 设备进入节电模式时会自动禁用亚克力效果，并且用户可以选择禁用所有应用的亚克力效果。


## <a name="acrylic-blend-types"></a>亚克力混合类型
亚克力的最明显特征是透明度。 有两种亚克力混合类型可改变材料透明度：
 - **背景亚克力**显示桌面壁纸和当前处于活动状态的应用后的其他窗口，增加了应用程序窗口之间的层次感，同时允许用户进行个性化偏好设置。
 - **应用内亚克力**在应用框架内增加层次感，焦点清晰且层次分明。

 ![背景亚克力](images/BackgroundAcrylic_DarkTheme.png)

 ![应用内亚克力](images/AppAcrylic_DarkTheme.png)

 叠加多层亚克力图面时务必小心。 背景亚克力，顾名思义，不应是 z 顺序中最靠近用户的元素。 多层背景亚克力往往会导致意外的光学错觉，应尽量避免。 如果你选择叠加多层亚克力，请使用应用内亚克力并考虑采用较浅的亚克力色调，让亚克力层在视觉上更靠近观看者。


## <a name="usability-and-adaptability"></a>可用性和适应性
亚克力外观可自动适应各种设备和上下文。

在高对比度模式中，用户仍将看到自己选择的熟悉的背景颜色，而非亚克力。 此外，背景亚克力和应用内亚克力均显示为纯色
 - 用户关闭个性化设置中的透明度时
 - 启用节电模式时
 - 应用在低端硬件上运行时

此外，只有背景亚克力会将透明度和纹理替换为纯色
 - 桌面上的应用窗口停用时
 - UWP 应用在手机、Xbox、HoloLens 或平板电脑模式下运行时

### <a name="legibility-considerations"></a>可读性注意事项
请务必确保应用上显示的任何文本[满足对比率要求](../accessibility/accessible-text-requirements.md)。 我们已优化亚克力设置，因此，增强色的黑色、白色甚至中间色的灰色文本在亚克力上显示时都能满足对比率要求。 平台提供的主题资源的默认对比色调为 80% 不透明度。 在亚克力上放置增强色正文文本时，你可以在降低色调不透明度的同时保持可读性。 在深色模式下，色调不透明度可设为 70%，而在浅色模式下，亚克力可满足 50% 不透明度条件下的对比率要求。

我们不建议在亚克力图面上放置颜色鲜亮的文本，因为这种组合可能无法满足 15 像素字体大小时的最低对比率要求。 请尽量避免在亚克力元素上放置[超链接](../controls-and-patterns/hyperlinks.md)。 此外，如果你选择自定义设置亚克力色调颜色或不透明度，而不采用主题资源提供的平台默认设置，请时刻注意可读性。

## <a name="acrylic-theme-resources"></a>亚克力主题资源
借助全新的 XAML AcrylicBrush 或预定义的 AcrylicBrush 主题资源，你可以在应用图面中轻松应用亚克力。 首先，你需要确定是使用应用内亚克力还是背景亚克力。 请务必仔细阅读上文描述的通用应用模式，以获取实用建议。

我们创建了一套适用于背景亚克力和应用内亚克力类型的画笔主题资源，可根据应用主题进行调整并按需回退为纯色。 名为 *Acrylic\ * WindowBrush* 的资源表示背景亚克力，名为 *Acrylic\ * ElementBrush* 的资源表示应用内亚克力。

<table>
    <tr>
        <th align="center">资源键</th>
        <th align="center">色调不透明度</th>
        <th align="center">[回退颜色](color.md)</th>
    </tr>
    <tr>
        <td> SystemControlAcrylicWindowBrush<br/>SystemControlAcrylicElementBrush </td>
        <td align="center"> 80% </td>
        <td> ChromeMedium </td>
    </tr>
    </tr>
        <td> **建议用法：**下面列出了适用于各种使用情况的常规亚克力资源。 如果你的应用使用 AltMedium 颜色的次级文本且文本大小小于 18 像素，将 80% 亚克力资源放在文本后即可[满足对比率要求](../accessibility/accessible-text-requirements.md)。 </td>
    </tr>
    <tr>
        <td> SystemControlAcrylicMediumHighWindowBrush<br/>SystemControlAcrylicMediumHighElementBrush </td>
        <td align="center"> 70% </td>
        <td> ChromeMedium </td>
    </tr>
    <tr>
        <td> **建议用法：**如果你的应用使用 AltMedium 颜色的次级文本且文本大小不小于 18 像素，可将透明度更高的 70% 亚克力资源放在文本后。 我们建议在应用的顶部水平导航和命令区域使用这些资源。  </td>
    </tr>
    <tr>
        <td> SystemControlAcrylicMediumWindowBrush<br/>SystemControlAcrylicMediumElementBrush </td>
        <td align="center"> 60% </td>
        <td> ChromeMediumLow </td>
    </tr>
    <tr>
        <td> **建议用法：**仅在亚克力上放置 AltHigh 颜色的主要文本时，你的应用可以使用这些 60% 资源。 我们建议你使用 60% 亚克力绘制应用的[垂直导航窗格](../controls-and-patterns/navigationview.md)，即汉堡菜单。 </td>
    </tr>
</table>

除了中性色亚克力以外，我们还添加了使用用户指定主题色的调色亚克力资源。 我们建议谨慎使用经过调色的亚克力。 对于提供的 dark1 和 dark2 变体，在资源上放置与深色主题文本颜色一致的白色或浅色文本。
<table>
    <tr>
        <th align="center">资源键</th>
        <th align="center">色调不透明度</th>
        <th align="center">[色调和回退颜色](color.md)</th>
    </tr>
    <tr>
        <td> SystemControlAcrylicAccentMediumHighWindowBrush<br/>SystemControlAcrylicAccentMediumHighElementBrush </td>
        <td align="center"> 70% </td>
        <td> SystemAccentColor </td>
    </tr>
    <tr>
        <td> SystemControlAcrylicAccentDark1WindowBrush<br/>SystemControlAcrylicAccentDark1ElementBrush </td>
        <td align="center"> 80% </td>
        <td> SystemAccentColorDark1 </td>
    </tr>
    <tr>
        <td> SystemControlAcrylicAccentDark2MediumHighWindowBrush<br/>SystemControlAcrylicAccentDark2MediumHighElementBrush </td>
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
 - **TintOpacity**：色调层不透明度。 我们建议刚开始时采用 80% 不透明度，尽管不同的颜色在采用其他透明度时看起来可能更具吸引力。
 - **BackgroundSource**：指定使用背景亚克力还是应用内亚克力的标记。
 - **FallbackColor**：在电池电量不足模式中替换亚克力的纯色。 对于背景亚克力，当应用并非位于活动状态桌面窗口中或者应用正在手机和 Xbox 上运行时，回退颜色也会替换亚克力。


![浅色主题亚克力样本](images/CustomAcrylic_Swatches_LightTheme.png)

![深色主题亚克力样本](images/CustomAcrylic_Swatches_DarkTheme.png)

若要添加亚克力画笔，请定义用于深色、浅色和高对比度主题的三个资源。 请注意，在高对比度主题中，我们建议使用 x:Key 与深色/浅色 AcrylicBrush 相同的 SolidColorBrush。

```xaml
<ResourceDictionary.ThemeDictionaries>
    <ResourceDictionary x:Key="Default">
        <AcrylicBrush x:Key="MyAcrylicBrush"
            BackgroundSource="HostBackdrop"
            TintColor="#FFFF0000"
            TintOpacity="0.8"
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

## <a name="extending-acrylic-into-your-title-bar"></a>将亚克力扩展到标题栏

要使应用窗口外观完整流畅，我们建议将亚克力放置在应用的标题栏区域。 为此，请将以下代码添加到 App.xaml.cs 中。

```csharp
CoreApplication.GetCurrentView().TitleBar.ExtendViewIntoTitleBar = true;
ApplicationViewTitleBar titleBar = ApplicationView.GetForCurrentView().TitleBar;
titleBar.ButtonBackgroundColor = Colors.Transparent;
titleBar.ButtonInactiveBackgroundColor = Colors.Transparent;
```

此外，你需要使用 `CaptionTextBlockStyle` 为应用程序标题（通常自动显示于标题栏）绘制 TextBlock。

## <a name="dos-and-donts"></a>应做事项和禁止事项
* 请将亚克力用作非主要应用图面（例如导航窗格）的背景材料。
* 请将亚克力扩展到至少一个应用边缘，通过与应用周围环境巧妙融合营造无缝体验。
* 不要直接并列放置应用内亚克力和背景亚克力，以避免接缝处产生不协调的视觉效果。
* 不要并列放置具有相同色调和不透明度的多个亚克力窗格，因为这会导致出现不协调的明显接缝。
* 不要将主题色文本放在亚克力图面上。

## <a name="how-we-designed-acrylic"></a>如何设计亚克力

我们微调亚克力的关键组件以凸显其独特外观和属性。 我们从透明度、模糊和噪点设置开始，为平滑图面增添视觉深度和维度。 我们添加了排除混合模式层，以确保放置在亚克力背景上的 UI 的对比度和可读性。 最后，我们添加了各种颜色色调，以供用户进行个性化设置。 这些图层协同作用，形成了全新的实用材料。

![亚克力设置](images/AcrylicRecipe_Diagram.png)
<br/>亚克力设置：背景、模糊、排除混合、颜色/色调覆盖、噪点

<!--
<div class="microsoft-internal-note">
When designing your app, please utilize these [design resources](http://uni/DesignDepot.FrontEnd/#/Search?t=Resources%7CNeon%7CToolkit&f=Acrylic%20Material) to show acrylic in comps. The linked templates are the most accurate way to represent acrylic material in Photoshop and Illustrator. The ordering, as noted in the recipe diagram above, should start from the top: <br/>
 - Noise asset (tiled) at 4% opacity <br/>
 - Base color/tint/alpha layer <br/>
 - Exclusion blend (white @ 10% opacity) <br/>
 - Gaussian blur (30px radius) <br/>
</div>
-->


## <a name="related-articles"></a>相关文章
[**展示**](reveal.md)
