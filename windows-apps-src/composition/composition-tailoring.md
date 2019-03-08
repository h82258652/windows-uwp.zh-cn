---
title: 组合量身定制
description: 使用组合 Api 来定制你的 UI、 优化性能，和满足用户设置和设备特征。
ms.date: 07/16/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: bcc9a6d89a143d8fd03d73dbd83b832ed9513ee2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57644412"
---
# <a name="tailoring-effects--experiences-using-windows-ui"></a>调整效果和使用 Windows UI 体验

Windows UI 提供差异化许多美观的效果、 动画和方法。 但是，符合用户预期的性能和可自定义性仍是创建成功的应用程序的必要部分。 通用 Windows 平台支持大规模、 多样化的具有不同的特性和功能的设备系列。 若要为所有用户提供的非独占的体验，您需要确保在设备上的应用程序规模并尊重用户首选项。 调整 UI 可以提供有效地利用设备的功能和确保愉快和非独占的用户体验。

用户界面定制很广，其中包含工作的高性能、 美观的 UI 相对于以下几个方面：

- 尊重并适应效果的用户设置
- 用于动画如何适应用户设置
- 为给定的硬件功能优化的 UI

在这里，我们将介绍如何量身定制效果和动画与可视化层中的区域更高版本，但有许多其他方式来定制你的应用程序以确保出色的最终用户体验。 指南文档位于如何[定制您的 UI](/windows/uwp/design/layout/screen-sizes-and-breakpoints-for-responsive-design)为各种设备和[创建响应式 UI](/windows/uwp/design/layout/responsive-design)。

## <a name="user-effects-settings"></a>用户效果设置

用户可以自定义各种原因，该应用程序应遵循的喜好并符合其 Windows 体验。 最终用户可以控制的一个方面更改他们将看到在其系统时使用的效果的类型。

### <a name="transparency-effects-settings"></a>透明度效果设置

一个用户可以自定义此类效果设置为打开/关闭透明效果。 可以在待个性化设置的设置应用程序中找到此 > 颜色，或通过设置应用程序 > 轻松访问 > 显示。

![在设置透明度选项](images/tailoring-transparency-setting.png)

打开时，任何使用透明度的效果将按预期方式显示。 这适用于染料、 HostBackdropBrush 或任何自定义效果图，它不完全不透明。

当关闭，丙烯画材料将自动回退到纯色因为 XAML 的 acrylic 画笔已经得到默认情况下此事件。 这里，我们可以看到相应地转而使用纯色时未启用透明效果计算器应用程序：

![计算器，染料](images/tailoring-acrylic.png)
![染料响应的透明度设置与计算器](images/tailoring-acrylic-fallback.png)

但是，对于任何自定义影响应用程序需要响应[UISettings.AdvancedEffectsEnabled](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.advancedeffectsenabledchanged)属性或[AdvancedEffectsEnabledChanged](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.advancedeffectsenabledchanged)事件和切换出影响/影响若要使用具有不透明效果的图表。 下面是此示例：

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

同样，应用程序应侦听和响应[UISettings.AnimationsEnabled](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.animationsenabled)属性以确保动画打开或关闭基于中设置的用户设置 > 轻松访问 > 显示。

![设置中的动画选项](images/tailoring-animations-setting.png)

```cs
public MainPage()
{
   var uisettings = new UISettings();
   bool animationsEnabled = uisettings.AnimationsEnabled;
   // TODO respond to animations settings
}

```

## <a name="leveraging-the-capabilities-api"></a>利用 API 的功能

通过利用[CompositionCapabilities](/uwp/api/windows.ui.composition.compositioncapabilities) Api，可以检测哪些组合功能是否可用和高性能的给定的硬件和定制用于确保最终用户在任何上获得高性能且美观的体验的设计设备。 Api 提供了一种方法来检查硬件系统功能，才能实现正常缩放跨多个窗体因素的影响。 这样，可以轻松进行适当地定制的应用程序创建美观和无缝的最终用户体验。

此 API 提供方法和事件侦听器，可用于做出缩放决策的应用程序 UI 的效果。 此功能检测到系统可以处理复杂组合和呈现操作的程度，然后在易于使用的开发人员可利用模型中返回的信息。

### <a name="using-composition-capabilities"></a>使用组合功能

CompositionCapabilities 功能已用于丙烯画材料，材料回退到具体取决于方案和硬件的更多高的性能效果等功能。

将 API 可以添加到简单的几步中的现有代码。

1. 获取应用程序的构造函数中的功能对象。

    ```cs
    _capabilities = CompositionCapabilities.GetForCurrentView();
    ```

1. 注册您的应用程序的功能已更改的事件侦听器。

    ```cs
    _capabilities.Changed += HandleCapabilitiesChanged;
    ```

1. 将内容添加到事件回调方法，以处理不同功能级别。 这可能会也可能不是类似于下面的下一步。
1. 在使用时效果，请先检查的功能对象。 请考虑使用条件检查或切换控件语句，具体取决于你想要调整的效果。

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

可以上找到完整的示例代码[Windows UI Github 存储库](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SampleGallery/Samples/SDK%2015063/CompCapabilities)。

## <a name="fast-vs-slow-effects"></a>快速和慢速的效果

根据从所提供的反馈[AreEffectsSupported](/uwp/api/windows.ui.composition.compositioncapabilities.areeffectssupported)并[AreEffectsFast](/uwp/api/windows.ui.composition.compositioncapabilities.areeffectsfast) CompositionCapabilities API 中的方法，该应用程序可以决定交换成本高或不受支持的效果针对设备优化他们选择的其他效果。 一些效果已知一直为比其他的多个占用大量资源，应谨慎使用，并可以更自由地使用其他效果。 对于所有效果，但是，注意应使用链接和对作为某些方案或组合进行动画处理可能更改影响关系图的性能特征时。 以下是一些经验法则性能特性的单个效果：

- 已知会影响高性能的影响如下所示 – 高斯模糊、 卷影的掩码、 BackDropBrush、 HostBackDropBrush，和可视化层。 不建议使用这些低端设备[（功能级别 9.1-9.3）](https://msdn.microsoft.com/library/windows/desktop/ff476876(v=vs.85).aspx)，并应十分谨慎高端设备上。
- 效果与中等性能影响包括颜色矩阵，（亮度、 颜色、 饱和度和 Hue），某些 Blend 效果 BlendModes 聚焦、 SceneLightingEffect，和 （取决于方案） BorderEffect。 这些效果可能适用于某些情况下，在低端设备上，但链接和动画处理时，应使用护理。 建议用途限制为两个或更少，并在仅转换进行动画处理。
- 所有其他效果低性能产生影响和工作时进行动画处理和链接的所有合理方案中。

## <a name="related-articles"></a>相关文章

- [UWP 响应式设计技术](https://docs.microsoft.com/windows/uwp/design/layout/responsive-design)
- [UWP 设备量身定制](https://docs.microsoft.com/windows/uwp/design/layout/screen-sizes-and-breakpoints-for-responsive-design)
