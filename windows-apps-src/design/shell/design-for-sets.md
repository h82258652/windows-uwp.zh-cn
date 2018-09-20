---
author: jwmsft
description: 了解如何优化你的应用以提供最佳的集 UI 体验。
title: 集设计
template: detail.hbs
ms.author: jimwalk
ms.date: 05/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 标题栏
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: 7c3e0e6ec7331e860c9153e2a2e29a51fb5848bd
ms.sourcegitcommit: 4f6dc806229a8226894c55ceb6d6eab391ec8ab6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/20/2018
ms.locfileid: "4087236"
---
# <a name="designing-for-sets"></a>集设计

> [!IMPORTANT]
> 本文介绍的功能尚未发布，在商业发行之前可能发生实质性修改。 Microsoft 对于此处提供的信息不作任何明示或默示的担保。

开始使用 Windows Insider Preview，集功能可供你的应用的用户使用。 使用集，应用在可以与其他应用共享的窗口内绘制，每个应用在标题栏中获取自己的选项卡。

在本文中，我们将详细介绍你可能想要优化应用以提供最佳集 UI 体验的区域。

> [!TIP]
> 有关集的详细信息，请参阅博客文章[集简介](https://insider.windows.com/en-us/articles/introducing-sets/)和 Microsoft Build 2018 上的演讲[集开发](https://developer.microsoft.com/events/build/content/developing-for-sets-on-windows-10)。

## <a name="customizing-tab-visuals"></a>自定义选项卡的视觉效果

默认情况下，系统会尝试在应用的选项卡处于活动状态时为其选择相应的文本和图标颜色。 这可以确保你只需付出最少的努力，或者即使你不针对集进行任何优化，你的应用选项卡也可看起来很美观。

不过，有些情况下，可能最好自定义应用的选项卡颜色。 在此部分中，我们将介绍默认的选项卡行为，并讨论你何时应该或者不应该为你的应用修改这些行为。

### <a name="tab-states"></a>选项卡状态

当你的应用位于集中时，其选项卡可能处于以下三种状态之一：已选择并活动、已选择但不活动，或未选择且不活动。

- **已选择并活动**：从一组分组的窗口内选择的选项卡，位于活动的前台窗口中。

    （在本文中，任何讨论修改_活动_选项卡的情况均指选项卡是“已选择并活动”。）
- **已选择但不活动**：从一组分组的窗口内选择的选项卡，但不位于活动的前台窗口中。
- **未选择且不活动**：不是从一组分组的窗口内选择的选项卡。

任何不活动选项卡的颜色均由系统基于系统主题更新和维护。 你的应用对它没有影响。

默认情况下，已选择并活动的选项卡遵循用户在 Windows 设置中指定的系统主题颜色。 你可以仅在选项卡处于活动状态时为你的应用自定义选项卡颜色。

![集中的选项卡状态](images/sets-tab-states.jpg)

### <a name="coloring-of-active-tabs"></a>活动选项卡的着色

活动选项卡的颜色由你在应用或系统设置中设置的值确定。 在应用处于活动状态时使用的选项卡颜色按照如下方式确定：

- 如果你在应用中指定选项卡颜色，它具有最高优先级。 当你的应用处于活动状态时，无论系统设置如何，都将使用你在应用中指定的选项卡颜色。
- 否则，如果用户在 Windows 设置中选择了在标题栏中显示主题色的选项，将使用系统主题色。
  - 此设置可以在“个性化”>“颜色”>“在下面的设计面显示主题色: 标题栏”下的“Windows 设置”应用中找到。
- 最后，如果未应用应用或用户设置，选项卡将使用当前系统的主题色。

### <a name="considerations-when-you-modify-tab-colors"></a>修改选项卡颜色的注意事项

在这里，我们讨论你可能希望修改应用的选项卡颜色的情况，以及在这些情况下应该考虑的事项。 另外还将讨论一些不建议你修改选项卡颜色，而让系统对其进行管理的情况。

#### <a name="match-your-brand-colors"></a>匹配你的品牌颜色

通常，确定你是否修改选项卡颜色的最主要因素是希望匹配品牌颜色。 当修改应用选项卡以匹配品牌颜色时，应该根据此部分中列出的其他注意事项（如应用布局或不同的系统主题颜色）测试颜色效果。

#### <a name="horizontal-layout"></a>水平布局

如果你的应用布局包括跨顶部水平运行的纯色（非亚克力）品牌颜色，通过使用匹配的颜色将应用与其选项卡相连通常效果较好。 请为选项卡选择感觉与你的应用相融的纯色，最好是通过向应用顶部使用的颜色提供某种程度的连续性。

#### <a name="horizontal-layout-with-acrylic"></a>使用亚克力的水平布局

如果你的应用使用跨顶部水平运行的一片亚克力材料，建议你让系统来确定选项卡颜色。

我们还建议在此处使用应用内亚克力，而不是使用背景亚克力，以让你的应用程序背景流入此区域，而不是创建在整个应用程序或此区域后面的桌面显示的带状效果。

请注意，用户可以设置这些选项卡以在此情况下使用主题色，以使其可以显示浅色/深色主题或主题色。

#### <a name="vertical-layout"></a>垂直布局

如果应用布局包含垂直运行的纯色垂直窗格，我们建议你不要自定义选项卡颜色。 应用上方的选项卡位置可以由用户随时更改，因此你不能期待应用和选项卡的顶部之间出现颜色连续性。系统将使用其他视觉提示（如阴影）来将选项卡与应用连接起来。

### <a name="how-to-modify-tab-colors"></a>如何修改选项卡颜色

活动选项卡颜色使用现有的标题栏自定义 API。 如果你已为应用自定义了标题栏颜色，此更改在应用位于集中时也会应用于应用选项卡。

若要修改选项卡颜色，请设置 [ApplicationViewTitleBar](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationviewtitlebar) 属性以指定：

- 选项卡的纯色背景色。
- 选项卡文本的纯色前景色。

此示例显示了如何获取 ApplicationViewTitleBar 的实例以及如何设置其颜色属性。

```csharp
// using Windows.UI.ViewManagement;
var titleBar = ApplicationView.GetForCurrentView().TitleBar;

// Set active window colors
titleBar.ForegroundColor = Windows.UI.Colors.White;
titleBar.BackgroundColor = Windows.UI.Colors.Green;
```

有关详细信息，请参阅[标题栏自定义](title-bar.md#simple-color-customization)一文的_简单的颜色自定义_部分和[标题栏自定义示例](http://go.microsoft.com/fwlink/p/?LinkId=620613)。

### <a name="considerations-for-full-title-bar-customization"></a>标题栏完全自定义的注意事项

你也可以完全自定义应用的标题栏，如[标题栏自定义](title-bar.md#full-customization)一文的_完全自定义_部分所述。 这样做通常是为了[将亚克力扩展到标题栏](../style/acrylic.md#extend-acrylic-into-the-title-bar)，或者在标题栏中放置自定义内容。 如果这样做，请务必遵循全屏和平板模式指南，并且仅在 [CoreApplicationViewTitleBar.IsVisible](/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.isvisible) 为 **true** 时显示自定义的标题栏内容。

当你的应用在集中运行时，CoreApplicationViewTitleBar.IsVisible 为 **false**，标题栏内容不会显示。 但是，如果你未按照本指南隐藏自定义的标题栏内容，它会显示在应用的选项卡下面，而不是显示在标题栏区域。

如果你在自定义的标题栏 UI 中放置了内容或功能，则应考虑在应用的其他 UI 设计面中提供它。

### <a name="how-to-modify-the-tab-icon"></a>如何修改选项卡图标

若要确保你的应用图标在集中看起来效果最好，则应为应用提供备用的未着色图标。 （在应用的选项卡中使用的应用图标与在任务栏中使用的是同一图标。）备用图标的用途是让它在任何背景颜色下都看起来很美观。 如果提供备用图标，则会使用该图标。

在你的应用清单中，除常规图标外，还应指定备用形式的未着色图标。 有关详细信息，请参阅[应用图标和徽标](/windows/uwp/design/style/app-icons-and-logos)。 要指定的图标在文章的[有关应用图标资源的详细信息](/windows/uwp/design/style/app-icons-and-logos#more-about-app-icon-assets)部分中记录为"未着色的目标大小列表资源"。

如果你未在应用清单中指定备用图标，系统将使用选项卡颜色为磁贴图标重新着色，并使用该颜色。

![集中使用的图标](images/sets-icons.png)

> 任务栏中和应用选项卡中使用相同的图标。

## <a name="restore-previous-sets-with-user-activities"></a>使用用户活动还原以前的集

集的优势是可以让你的用户在启动应用或打开文档时还原以前打开的应用的选项卡和 Web 内容。 （请观看博客文章[集简介](https://insider.windows.com/en-us/articles/introducing-sets/)中的视频了解详细信息。）可通过_用户活动_启用。

默认情况下，系统会代表你的应用创建用户活动，这在用户启动应用或打开文档时让应用可以还原到一个选项卡。 但是，系统创建的默认用户活动只能在应用的默认状态下启动应用。 无法将应用还原到之前作为集的一部分所处的状态。

你可以通过提供自定义用户活动针对集优化你的应用。 你提供的用户活动深层链接到你的应用，以将其还原到上次作为正在还原的集的一部分所处的状态。

若要提供自定义用户活动：

- 使用适当的 [UserActivity](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity) 响应操作系统启动的 [UserActivityRequest](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivityrequest)。
- UserActivity 包含系统用来启动具有特定上下文的应用的激活深层链接 URI。

有关详细信息，请参阅 [UserActivityRequestManager.UserActivityRequested 事件](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivityrequestmanager.useractivityrequested)、[处理 URI 激活](https://docs.microsoft.com/windows/uwp/launch-resume/handle-uri-activation)和[跨设备继续用户活动](https://docs.microsoft.com/windows/uwp/launch-resume/useractivities)。

## <a name="enable-multi-instance-for-uwp-apps"></a>为 UWP 应用启用多实例

从 Windows 10 版本 1803 开始，UWP 应用支持多实例。 UWP 默认情况下仍是单实例，你必须明确选择加入你希望实现多实例的每个应用。

如果你让应用实现多实例，它会提供允许用户一次在多个集中运行应用的好处。 单实例应用一次只能在一个集中运行。

有关如何为 UWP 应用启用多实例的详细信息，请参阅[创建多实例通用 Windows 应用](https://docs.microsoft.com/windows/uwp/launch-resume/multi-instance-uwp)。

## <a name="use-an-in-app-back-button"></a>使用应用内“后退”按钮

若要在应用中实现向后导航，建议你按照[“后退”按钮指南](../basics/navigation-history-and-backwards-navigation.md)在应用的 UI 中放置“后退”按钮。 如果你的应用使用了 NavigationView 控件，你应该使用 NavigationView 的内置“后退”按钮。

如果你的应用使用系统“后退”按钮，你应该替换为应用内“后退”按钮。 这将确保用户具有一致的“后退”按钮体验，不论应用是否在集中运行，同时还可以确保“后退”按钮视觉效果在应用之间保持一致。

有关集成应用内“后退”按钮的详细指南，请参阅[导航历史记录和向后导航](../basics/navigation-history-and-backwards-navigation.md)。

### <a name="support-for-the-system-back-button-in-sets"></a>集中系统“后退”按钮的支持

如果你的应用使用系统“后退”按钮，而不是应用内按钮，系统 UI 仍将呈现系统“后退”按钮以确保后向兼容性

- 如果你的应用不在集中，那么“后退”按钮将在标题栏内呈现。 “后退”按钮的视觉体验和用户交互保持不变。
- 如果你的应用在集中，那么“后退”按钮将在系统后退栏内呈现。

系统后退栏是在选项卡区带和应用的内容区域之间插入的“区带”。 此区带横跨整个应用，“后退”按钮位于左边缘。 其具有足够大的垂直高度以确保“后退”按钮有足够的触摸目标大小。

![集中的系统后退栏](images/sets-system-back-bar.png)

> 系统后退栏显示在应用中。

系统后退栏基于“后退”按钮的可见性动态显示。 当后退按钮可见时，系统后退栏插入，将应用内容下移到选项卡区带以下。 当“后退”按钮隐藏时，系统后退栏动态删除，将应用内容上移到选项卡区带。

若要避免应用的 UI 上移或下移，我们建议使用应用内“后退”按钮，而不是系统“后退”按钮。

标题栏自定义会延续到应用选项卡和系统后退栏。 如果你使用 ApplicationViewTitleBar API 来指定背景色和前景色属性，这些颜色将应用到选项卡和系统后退栏。

## <a name="related-articles"></a>相关文章

- [标题栏自定义](title-bar.md)
- [导航历史记录和向后导航](../basics/navigation-history-and-backwards-navigation.md)
- [颜色](../style/color.md)
