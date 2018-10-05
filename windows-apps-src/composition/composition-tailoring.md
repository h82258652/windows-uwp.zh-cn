---
author: daneuber
title: 合成定制
description: 使用合成 Api 来定制你的 UI，优化性能，并使其适应用户设置和设备特性。
ms.author: jimwalk
ms.date: 07/16/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 66384c4df3195ae0fff35ae5dd7e1b1983204068
ms.sourcegitcommit: 63cef0a7805f1594984da4d4ff2f76894f12d942
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/05/2018
ms.locfileid: "4392214"
---
# <a name="tailoring-effects--experiences-using-windows-ui"></a>定制效果和使用 Windows UI 体验

Windows UI 提供差异化有关的许多美观效果、 动画和方式。 但是，达到用户期望的性能和可自定义性仍是创建成功应用程序的必要组成部分。 通用 Windows 平台支持大规模、 多样化设备系列的它具有不同的特性和功能。 为了为所有用户提供的非独占的体验，你需要确保你的应用程序比例跨设备和尊重用户首选项。 定制 UI 可以提供一种能够利用设备的功能，同时确保让人愉快和非独占的用户体验的有效方法。

UI 定制很广，包括高性能，相对于以下方面的美观 UI 工作：

- 尊重并适应效果的用户设置
- 以适应动画的用户设置
- 针对给定的硬件功能进行优化的 UI

在这里，我们将介绍如何定制你的效果和动画可视化层中的区域上方，但有许多其他方式来定制你的应用程序，以确保出色的最终用户体验。 提供有关如何针对各种设备和[创建响应式 UI](/design/layout/responsive-design.md)[定制你的 UI](/design/layout/screen-sizes-and-breakpoints-for-responsive-design.md)指南文档。

## <a name="user-effects-settings"></a>用户效果设置

用户可以自定义各种原因，该应用程序应尊重和调整到其 Windows 体验。 最终用户可以控制的一个区域更改效果他们可看到用于整个其系统的类型。

### <a name="transparency-effects-settings"></a>透明效果设置

此类用户可以自定义的一个效果设置为打开开/关透明效果。 这可以在设置应用个性化下找到 > 颜色，或通过设置应用 > 轻松访问 > 显示。

![设置中的透明度选项](images/tailoring-transparency-setting.png)

打开时，任何使用透明度的效果将显示按预期运行。 这适用于亚克力、 HostBackdropBrush 或不是完全不透明任何自定义效果图。

关闭后，亚克力材料将自动回退为纯色因为默认情况下，XAML 的亚克力画笔具有侦听对此事件。 在这里，我们看到相应地回退为纯色时未启用透明效果的计算器应用：

![使用亚克力的计算器](images/tailoring-acrylic.png)
![使用亚克力响应透明度设置的计算器](images/tailoring-acrylic-fallback.png)

但是，任何自定义效果的应用程序需要响应[UISettings.AdvancedEffectsEnabled](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.advancedeffectsenabledchanged)属性或[AdvancedEffectsEnabledChanged](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.advancedeffectsenabledchanged)事件和切换出去效果/效果图使用具有不透明的效果。 下面是此过程的示例：

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

同样，应用程序应该侦听和响应[UISettings.AnimationsEnabled](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.animationsenabled)属性，以确保动画是打开还是关闭具体取决于设置中的用户设置 > 轻松访问 > 显示。

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

通过利用[CompositionCapabilities](/uwp/api/windows.ui.composition.compositioncapabilities) Api，你可以检测哪些合成功能均可和性能在给定的硬件和定制设计，以确保最终用户中获取任何设备上的性能和美观的体验。 Api 提供一种方法来检查硬件系统功能才能实现跨多个外形规格缩放的正常效果。 这可以轻松相应地定制的应用程序创建美观和无缝的最终用户体验。

此 API 提供了方法和事件侦听器，用于使缩放应用程序 UI 的决策的效果。 该功能检测到程度，系统可以处理复杂合成和呈现操作，然后在易于使用模型中针对开发人员能够利用返回信息。

### <a name="using-composition-capabilities"></a>使用合成功能

CompositionCapabilities 功能已用于功能，如亚克力材料，材料回退到更多的性能效果，具体取决于方案和硬件。

该 API 可添加到几个简单步骤中的现有代码。

1. 获取你的应用程序的构造函数中的功能对象。

    ```cs
    _capabilities = CompositionCapabilities.GetForCurrentView();
    ```

1. 注册你的应用的功能已更改的事件侦听器。

    ```cs
    _capabilities.Changed += HandleCapabilitiesChanged;
    ```

1. 将内容添加到事件回调方法来处理各种功能级别。 这可能或不可能类似于下面的下一步。
1. 在使用效果，请首先检查的功能对象。 请考虑使用条件检查或切换控制语句，具体取决于你想要定制效果。

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

[Windows UI Github 存储库](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SampleGallery/Samples/SDK%2015063/CompCapabilities)上找不到完整示例代码。

## <a name="fast-vs-slow-effects"></a>快速与慢效果

根据反馈 CompositionCapabilties API 中提供的[AreEffectsSupported](/uwp/api/windows.ui.composition.compositioncapabilities.areeffectssupported)和[AreEffectsFast](/uwp/api/windows.ui.composition.compositioncapabilities.areeffectsfast)方法，应用程序可以决定以用于进行了优化他们所选的其他效果交换耗费资源或不受支持的效果为设备。 某些效果已知的一致地比他人做得更多需要大量资源和应谨慎，使用，并且可以更自由地用于其他效果。 对于所有效果，但是，小心应使用链接和为某些方案或组合创建动画可能会更改效果图的性能特征时。 下面是单个效果一些经验法则性能特征：

- 已知具有高性能影响的效果如下所示 – 高斯模糊、 阴影掩码、 BackDropBrush、 HostBackDropBrush，和可视化层。 这些不推荐用于低端设备[（功能级别 9.1 9.3）](https://msdn.microsoft.com/library/windows/desktop/ff476876(v=vs.85).aspx)，并且应谨慎使用高端设备上。
- 中等性能影响的效果包括颜色矩阵，某些混合效果 BlendModes （亮度、 颜色、 饱和度和色调） 聚焦、 SceneLightingEffect，和 （取决于方案） BorderEffect。 这些效果可适用于某些情况下，低端在设备上，但链接并进行动画处理时，应使用小心。 建议为两个或更少限制使用和创建仅过渡动画。
- 所有其他效果很低性能影响，并且在所有合理情况下，当创建动画和链接起作用。

## <a name="related-articles"></a>相关文章

- [UWP 响应式设计技术](https://docs.microsoft.com/windows/uwp/design/layout/responsive-design)
- [UWP 设备定制](https://docs.microsoft.com/windows/uwp/design/layout/screen-sizes-and-breakpoints-for-responsive-design)
