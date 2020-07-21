---
title: 撰写定制
description: 使用组合 Api 可定制 UI、优化性能并适应用户设置和设备特征。
ms.date: 07/16/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 95a7355241f9ba4cc7b4bb743b78ac09169d65d9
ms.sourcegitcommit: 2747d9266e1678fca96d3822ce47499ca91a2c70
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/14/2020
ms.locfileid: "77213674"
---
# <a name="tailoring-effects--experiences-using-windows-ui"></a>使用 Windows UI & 体验定制效果

Windows UI 提供了许多漂亮的效果、动画和含义。 但是，若要创建成功的应用程序，仍需要满足用户对性能和可定制性的期望。 通用 Windows 平台支持多种不同的设备系列，它们具有不同的特性和功能。 若要为所有用户提供非独占体验，需确保应用程序可跨设备缩放，并尊重用户首选项。 UI 定制可提供一种有效的方法来利用设备的功能，并确保获得愉快的用户体验。

UI 定制是一种广泛的类别，其中包含与以下领域相关的高性能、精美的 UI：

- 遵从并适应用户的效果设置
- 容纳动画的用户设置
- 为给定的硬件功能优化 UI

在此，我们将介绍如何使用上述区域中的可视层定制您的效果和动画，但有许多其他方法可定制您的应用程序以确保最终用户体验出色。 有关如何为各种设备[定制 ui](/windows/uwp/design/layout/screen-sizes-and-breakpoints-for-responsive-design)并[创建响应式 ui](/windows/uwp/design/layout/responsive-design)的指南文档可用。

## <a name="user-effects-settings"></a>用户效果设置

由于各种原因，用户可以自定义其 Windows 体验，因此应用程序应遵守并适应。 最终用户可以控制的一个领域是，更改他们在整个系统中使用的效果类型。

### <a name="transparency-effects-settings"></a>透明度效果设置

用户可以自定义的一种效果设置是打开/关闭透明效果。 可以在 "设置" 应用中的 "个性化 > 颜色" 或 "设置" 应用 > 轻松访问 > 显示中找到此项。

![设置中的透明性选项](images/tailoring-transparency-setting.png)

启用后，任何使用透明度的效果都将按预期方式显示。 这适用于 Acrylic、HostBackdropBrush 或任何不是完全不透明的自定义效果图。

关闭后，acrylic 材料将自动回退为纯色，因为 XAML 的 acrylic 画笔在默认情况下已侦听此事件。 在这里，我们看到计算器应用程序在未启用透明效果时适当地恢复为纯色：

![计算器与 Acrylic](images/tailoring-acrylic.png)
![计算器，Acrylic 响应透明度设置](images/tailoring-acrylic-fallback.png)

但是，对于任何自定义效果，应用程序需要响应[UISettings. AdvancedEffectsEnabled](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.advancedeffectsenabled)属性或[AdvancedEffectsEnabledChanged](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.advancedeffectsenabledchanged)事件，并切换效果/效果图以使用没有透明度的效果。 下面是一个示例：

```cs
public MainPage()
{
   var uisettings = new UISettings();
   bool advancedEffects = uisettings.AdvancedEffectsEnabled;
   uisettings.AdvancedEffectsEnabledChanged += Uisettings_AdvancedEffectsEnabledChanged;
}

private void Uisettings_AdvancedEffectsEnabledChanged(UISettings sender, object args)
{
    // TODO respond to sender.AdvancedEffectsEnabled
}
```

## <a name="animations-settings"></a>动画设置

同样，应用程序应侦听[UISettings](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.animationsenabled)属性，并对其做出响应，以确保根据 "设置" 中的用户设置 > 轻松访问 > 显示打开或关闭动画。

![设置中的动画选项](images/tailoring-animations-setting.png)

```cs
public MainPage()
{
   var uisettings = new UISettings();
   bool animationsEnabled = uisettings.AnimationsEnabled;
   // TODO respond to animations settings
}

```

## <a name="leveraging-the-capabilities-api"></a>利用功能 API

利用[CompositionCapabilities](/uwp/api/windows.ui.composition.compositioncapabilities) api，你可以在给定的硬件上检测哪些组合功能可用且性能良好，并定制设计以确保最终用户能够在任何设备上获得高性能和漂亮的体验。 Api 提供了一种方法来检查硬件系统功能，以实现各种外形规格的平稳效果缩放。 这样就可以轻松地对应用程序进行适当调整，以创建精美且无缝的最终用户体验。

此 API 提供了方法和事件侦听器，可用于对应用程序 UI 做出有效缩放决策。 此功能检测到系统可处理复杂的组合和渲染操作的程度，然后在易于使用的模型中返回信息供开发人员使用。

### <a name="using-composition-capabilities"></a>使用组合功能

CompositionCapabilities 功能已用于 Acrylic 材料等功能，在这种情况下，材料会根据方案和硬件降低到更高的性能效果。

可以通过几个简单的步骤将 API 添加到现有代码中。

1. 在应用程序的构造函数中获取功能对象。

    ```cs
    _capabilities = CompositionCapabilities.GetForCurrentView();
    ```

1. 为你的应用程序注册功能更改事件侦听器。

    ```cs
    _capabilities.Changed += HandleCapabilitiesChanged;
    ```

1. 向事件回调方法添加内容以处理不同的功能级别。 这可能与下一步类似，也可能不会。
1. 使用效果时，首先检查功能对象。 请考虑使用条件检查或 switch 控制语句，具体取决于您希望如何调整效果。

    ```cs
    if (_capabilities.AreEffectsSupported())
    {
        // Add incremental effects updates here

        if (_capabilities.AreEffectsFast())
        {
            // Add more advanced effects here where applicable
        }
    }
    ```

可在[WINDOWS UI Github](https://github.com/microsoft/WindowsCompositionSamples/tree/master/SampleGallery/Samples/SDK 15063/CompCapabilities)存储库中找到完整示例代码。

## <a name="fast-vs-slow-effects"></a>快速和缓慢效果

根据 CompositionCapabilities API 中所提供的[AreEffectsSupported](/uwp/api/windows.ui.composition.compositioncapabilities.areeffectssupported)和[AreEffectsFast](/uwp/api/windows.ui.composition.compositioncapabilities.areeffectsfast)方法的反馈，应用程序可以决定为设备优化所选的其他效果，从而实现昂贵或不受影响的影响。 众所周知，某些效果的资源比其他效果要多得多，并且应谨慎使用，其他效果可以更自由地使用。 但是，对于所有效果，当在某些情况下进行链接和动画处理时应谨慎使用，否则组合可能会更改效果图的性能特征。 下面是一些针对单个效果的性能特征的规则：

- 已知的影响对性能的影响包括：高斯模糊、阴影掩码、BackDropBrush、HostBackDropBrush 和分层视觉对象。 不建议对低端设备使用这些[功能（功能级别 9.1-9.3）](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-devices-downlevel-intro)，并且应谨慎地在高端设备上使用。
- 具有中等性能影响的影响包括颜色矩阵、特定混合效果 BlendModes （亮度、颜色、饱和度和色调）、聚焦、SceneLightingEffect 和（具体取决于方案） BorderEffect。 这些效果可能适用于低端设备上的某些方案，但在链接和动画处理时应谨慎使用。 建议限制使用为两个或更少，并仅对转换进行动画处理。
- 所有其他效果的性能不会受到影响，并且在动画和链接时适用于所有合理方案。

## <a name="related-articles"></a>相关文章

- [UWP 响应式设计技术](https://docs.microsoft.com/windows/uwp/design/layout/responsive-design)
- [UWP 设备定制](https://docs.microsoft.com/windows/uwp/design/layout/screen-sizes-and-breakpoints-for-responsive-design)
