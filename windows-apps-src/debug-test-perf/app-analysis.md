---
author: jwmsft
title: 应用分析
description: 分析应用的性能问题。
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10，uwp
ms.localizationpriority: medium
ms.openlocfilehash: f14f241eb6a1a39b29216fe703c3804b34f8a3bc
ms.sourcegitcommit: 82c3fc0b06ad490c3456ad18180a6b23ecd9c1a7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2018
ms.locfileid: "5480057"
---
# <a name="app-analysis-overview"></a>应用分析概述

应用分析是一种工具，可为开发人员提供关于性能问题的提醒通知。 应用分析参照一组性能指南和最佳做法运行应用代码。

应用分析从应用遇到的常见性能问题的规则集中识别问题。 在适当情况下，应用分析将指向 Visual Studio 的时间线工具、源信息和文档以为你提供调查的手段。

应用分析中的规则是指检查你的应用时所参照的指南或最佳做法。

## <a name="decoded-image-size-larger-than-render-size"></a>解码图像大小大于呈现大小

图像以非常高的分辨率捕获，这可能导致应用在解码图像数据时使用更多 CPU，并在从磁盘加载它后使用更多内存。 但是在内存中解码和保存高分辨率图像没有意义，只是将其显示为比其本机大小更小。 改为使用 DecodePixelWidth 和 DecodePixelHeight 属性以将在屏幕上绘制图像的相同大小创建图像版本。

### <a name="impact"></a>影响

以非原生大小显示图像可能对 CPU 时间（由于解码为正确大小和下载时间）和内存都造成负面影响。

### <a name="causes-and-solutions"></a>原因和解决方案

#### <a name="image-is-not-being-set-asynchronously"></a>图像未异步设置

应用正在使用 SetSource()，而不是 SetSourceAsync()。 在将流设置为异步解码图像时，应始终避免使用 [**SetSource**](https://msdn.microsoft.com/library/windows/apps/BR243255)，而改用 [**SetSourceAsync**](https://msdn.microsoft.com/library/windows/apps/JJ191522)。 

#### <a name="image-is-being-called-when-the-imagesource-is-not-in-the-live-tree"></a>当 ImageSource 不在活动数中时调用图像

在使用 SetSourceAsync 或 UriSource 设置内容后，BitmapImage 连接到活动 XAML 树。 在设置源之前，你应始终将 [**BitmapImage**](https://msdn.microsoft.com/library/windows/apps/BR243235) 附加到活动树。 每当在标记中指定图像元素或画笔时，将自动成为这种情况。 示例如下所示。 

**活动树示例**

示例 1（良好）- 标记中指定的统一资源标识符 (URI)。

```xml
<Image x:Name="myImage" UriSource="Assets/cool-image.png"/>
```

示例 2 标记 - 代码隐藏中指定的 URI。

```xml
<Image x:Name="myImage"/>
```

示例 2 代码隐藏（良好）- 在设置 BitmapImage 的 UriSource 前将其连接到树。

```vb
var bitmapImage = new BitmapImage();
myImage.Source = bitmapImage;
bitmapImage.UriSource = new URI("ms-appx:///Assets/cool-image.png", UriKind.RelativeOrAbsolute);
```

示例 2 代码隐藏 （不良）-在连接到树前设置 BitmapImage 的 UriSource。

```vb
var bitmapImage = new BitmapImage();
bitmapImage.UriSource = new URI("ms-appx:///Assets/cool-image.png", UriKind.RelativeOrAbsolute);
myImage.Source = bitmapImage;
```

#### <a name="image-brush-is-non-rectangular"></a>图像画笔非矩形 

当图像用于非矩形画笔时，该图像将使用软件栅格化路径，这将完全不会缩放图像。 此外，它必须在软件和硬件内存中存储该图像的副本。 例如，如果一个图像用作椭圆形的画笔，将在内部存储两次潜在较大的完整图像。 那么，在使用非矩形画笔时，你的应用应将其图像预先缩放为呈现时所要使用的相似大小。

或者，可以设置一个显式解码大小来使用 [**DecodePixelWidth**](https://msdn.microsoft.com/library/windows/apps/BR243243) 和 [**DecodePixelHeight**](https://msdn.microsoft.com/library/windows/apps/BR243241) 属性以将在屏幕上绘制图像的相同大小创建图像版本。

```xml
<Image>
    <Image.Source>
    <BitmapImage UriSource="ms-appx:///Assets/highresCar.jpg" 
                 DecodePixelWidth="300" DecodePixelHeight="200"/>
    </Image.Source>
</Image>
```

默认情况下，[**DecodePixelWidth**](https://msdn.microsoft.com/library/windows/apps/BR243243) 和 [**DecodePixelHeight**](https://msdn.microsoft.com/library/windows/apps/BR243241) 的单位是物理像素。 [**DecodePixelType**](https://msdn.microsoft.com/library/windows/apps/Dn298545) 属性可用于更改此行为：将 **DecodePixelType** 设置为 **Logical** 会导致解码大小自动考虑到系统的当前比例系数，类似于其他 XAML 内容。 因此，举例来说，如果你希望 **DecodePixelWidth** 和 **DecodePixelHeight** 与显示图像所要使用的图像控件的 Height 和 Width 属性匹配，则通常适合将 **DecodePixelType** 设置为 **Logical**。 对于使用物理像素的默认行为，你必须自行考虑到系统的当前比例系数；并且你应该侦听缩放更改通知，以防用户更改其显示首选项。

在某些无法事先确定合适解码大小的情况下，你应遵循 XAML 的自动正确大小解码，如果未指定显式的 DecodePixelWidth/DecodePixelHeight，则这将尽量尝试以合适大小解码该图像。

如果你事先知道图像内容的大小，则应设置显式的解码大小。 如果所提供的解码大小相对于其他 XAML 元素大小，你还应一起将 [**DecodePixelType**](https://msdn.microsoft.com/library/windows/apps/Dn298545) 设置为 **Logical**。 例如，如果你使用 Image.Width 和 Image.Heigh 显式设置内容大小，则可以将 DecodePixelType 设置为 DecodePixelType.Logical 以使用与图像控件相同的逻辑像素维度，然后显式使用 BitmapImage.DecodePixelWidth 和/或 BitmapImage.DecodePixelHeight 控制图像的大小以实现潜在较大的内存节省。

请注意，在确定解码内容的大小时，应考虑 Image.Stretch。

#### <a name="images-used-inside-of-bitmapicons-fall-back-to-decoding-to-natural-size"></a>BitmapIcons 内使用的图像回退到解码为自然大小 

或者，可以设置一个显式解码大小来通过使用 [**DecodePixelWidth**](https://msdn.microsoft.com/library/windows/apps/BR243243) 和 [**DecodePixelHeight**](https://msdn.microsoft.com/library/windows/apps/BR243241) 属性以将在屏幕上绘制图像的相同大小创建图像版本。

#### <a name="images-that-appear-extremely-large-on-screen-fall-back-to-decoding-to-natural-size"></a>在屏幕上显得非常大的图像回退到解码为自然大小 

在屏幕上显得非常大的图像回退到解码为自然大小。 或者，可以设置一个显式解码大小来通过使用 [**DecodePixelWidth**](https://msdn.microsoft.com/library/windows/apps/BR243243) 和 [**DecodePixelHeight**](https://msdn.microsoft.com/library/windows/apps/BR243241) 属性以将在屏幕上绘制图像的相同大小创建图像版本。

#### <a name="image-is-hidden"></a>隐藏图像

通过在主机图像元素或画笔或者任何父元素上将 Opacity 设置为 0 或将 Visibility 设置为 Collapsed 来隐藏该图像。 由于剪裁或透明度而在屏幕上不可见的图像可能回退到解码为自然大小。 

#### <a name="image-is-using-ninegrid-property"></a>图像使用 NineGrid 属性

当图像用于 [**NineGrid**](https://msdn.microsoft.com/library/windows/apps/BR242756) 时，该图像将使用软件栅格化路径，这将完全不会缩放图像。 此外，它必须在软件和硬件内存中存储该图像的副本。 在使用 **NineGrid** 时，你的应用应将其图像预先缩放为呈现时所要使用的相似大小。

使用 NineGrid 属性的图像回退到解码为自然大小。 请考虑将 ninegrid 效果添加到原始图像。

#### <a name="decodepixelwidth-or-decodepixelheight-are-set-to-a-size-thats-larger-than-the-image-will-appear-on-screen"></a>DecodePixelWidth 或 DecodePixelHeight 设置为大于图像将在屏幕上显示的大小。 

如果将 DecodePixelWidth/Height 显式设置为大于将在屏幕上显示的图像，应用将不必使用额外内存（最高为每像素 4 个字节），这对于大图像来说将很快成为昂贵的负担。 还将使用双线性缩放缩小该图像，这可能导致它在较大的比例系数中显得模糊。

#### <a name="image-is-decoded-as-part-of-producing-a-drag-and-drop-image"></a>在生成拖放图像的过程中解码图像

或者，可以设置一个显式解码大小来通过使用 [**DecodePixelWidth**](https://msdn.microsoft.com/library/windows/apps/BR243243) 和 [**DecodePixelHeight**](https://msdn.microsoft.com/library/windows/apps/BR243241) 属性以将在屏幕上绘制图像的相同大小创建图像版本。

## <a name="collapsed-elements-at-load-time"></a>加载时折叠的元素

应用中的常见模式是最初在 UI 中隐藏元素，并在以后显示它们。 在大多数情况下，应使用 x:Load 或 x:DeferLoadStrategy 延迟这些元素，以避免支付在加载时创建元素的成本。

这包括使用布尔值到可见性转换器来隐藏项目直到某个稍后时间的情况。

### <a name="impact"></a>影响

折叠元素与其他元素一起加载，并导致加载时间的增加。

### <a name="cause"></a>原因

由于某个元素在加载时折叠，因此触发了此规则。 折叠某个元素或将其不透明度设置为 0 无法阻止该元素的创建。 使用默认为 false 的布尔值到可见性转换器的应用可能导致此规则。

### <a name="solution"></a>解决方案

可使用 [x:Load attribute](../xaml-platform/x-load-attribute.md) 或 [x:DeferLoadStrategy](https://msdn.microsoft.com/library/windows/apps/Mt204785) 来延迟部分 UI 的加载，并在需要时加载它。 延迟处理在第一帧中不可见的 UI 是一个好方法。 你可以选择在需要时或作为一组延迟逻辑的一部分加载元素。 若要触发加载，请在你希望加载的元素上调用 findName。 x:Load 扩展了 x:DeferLoadStrategy 的功能，从而使元素可以进行卸载，并使加载状态可以通过 x:Bind 进行控制。

在某些情况下，使用 findName 显示部分 UI 可能并非答案。 如果你希望使用非常低的延迟在按钮单击时实现大量 UI，则确实如此。 在此情况下，可能要以额外内存为代价来实现更快的 UI 延迟，此时应使用 x:DeferLoadStrategy 并对要实现的元素将 Visibility 设置为 Collapsed。 在页面已加载并且 UI 线程可用后，你可以在必要时调用 findName 来加载元素。 在将元素的 Visibility 设置为 Visible 前，元素对用户不可见。

## <a name="listview-is-not-virtualized"></a>ListView 未虚拟化。

UI 虚拟化是你可以对提升集合性能所做出的最重要的改进。 这意味着按需创建表示项目的 UI 元素。 对于绑定到 1000 个项目的集合的项目控件，同时为所有项目创建 UI 会造成资源浪费，因为它们不可能全部同时显示。 ListView 和 GridView（及其他标准 ItemsControl 派生的控件）可为你执行 UI 虚拟化。 当项目即将滚动到视图中时（只距离几页），框架将为这些项目生成 UI 并缓存它们。 当这些项目不太可能再次显示时，框架将回收内存。

UI 虚拟化只是提升集合性能的几个关键因素之一。 降低集合项目的复杂性和数据虚拟化是提升集合性能的另外两个重要方面。 有关在 ListViews 和 GridViews 内提升集合性能的详细信息，请参阅 [ListView 和 GridView UI 优化](https://msdn.microsoft.com/windows/uwp/debug-test-perf/optimize-gridview-and-listview)和 [ListView 和 Gridview 数据虚拟化](https://msdn.microsoft.com/windows/uwp/debug-test-perf/listview-and-gridview-data-optimization)上的文章。

### <a name="impact"></a>影响

非虚拟化 ItemsControl 将通过加载超出必要数量的子项增加加载时间和资源使用量。

### <a name="cause"></a>原因

视口的概念对于 UI 虚拟化至关重要，因为该框架必须创建可能显示的元素。 通常，ItemsControl 的视口是逻辑控件的范围。 例如 ListView 的视口是 ListView 元素的宽度和高度。 某些面板允许子元素使用无限的空间（例如 ScrollViewer 和 Grid），并自动调整行或列的大小。 将虚拟化的 ItemsControl 放置在这样的面板中后，它有足够的空间用于显示它的所有项目，此时虚拟化便失去意义。 

### <a name="solution"></a>解决方案

通过在要使用的 ItemsControl 上设置宽度和高度来恢复虚拟化。

## <a name="ui-thread-blocked-or-idle-during-load"></a>UI 线程在加载期间被阻止或处于空闲状态

UI 线程阻止是指同步调用在线程外执行并且阻止 UI 线程的函数。  

有关提升应用启动性能的最佳做法的完整列表，请参阅[应用的启动性能的最佳做法](https://msdn.microsoft.com/windows/uwp/debug-test-perf/best-practices-for-your-app-s-startup-performance)和[使 UI 线程保持响应状态](https://msdn.microsoft.com/windows/uwp/debug-test-perf/keep-the-ui-thread-responsive)。

### <a name="impact"></a>影响

在加载期间阻止或空闲的 UI 线程将阻止布局和其他 UI 操作，从而增加启动时间。

### <a name="cause"></a>原因

用于 UI 的平台代码以及用于 UI 的应用代码都在同一 UI 线程上执行。 该线程上一次只能执行一个指令，因此如果应用代码处理事件的时间过长，框架将无法运行布局或引发表示用户交互的新事件。 应用的响应能力与处理工作的 UI 线程的可用性相关。

### <a name="solution"></a>解决方案

即使应用的部分功能不全，应用也可交互。 例如，如果你的应用显示需要些时间检索的数据，你可通过异步检索数据来确保独立于应用的启动代码的代码执行。 数据可用时，用数据填充应用的用户界面。 为了帮助应用保持响应状态，平台提供乐许多 API 的异步版本。 异步 API 确保你的活动执行线程永远不会占用大量时间。 从 UI 线程调用 API 时，请使用异步版本（如果可用）。

## <a name="binding-is-being-used-instead-of-xbind"></a>正在使用 {Binding} 而不是 {x:Bind}

当应用使用 {Binding} 语句时，将引发此规则。 若要提升应用性能，应用应考虑使用 {x:Bind}。

### <a name="impact"></a>影响

{Binding} 运行时需要比 {x:Bind} 更多的时间和内存。

### <a name="cause"></a>原因

应用正在使用 {Binding}，而不是 {x:Bind}。 {Binding} 会带来非比寻常的工作集和 CPU 开销。 创建 {Binding} 会导致一系列分配，而更新绑定目标可能会引起反射和装箱。

### <a name="solution"></a>解决方案

使用 {x:Bind} 标记扩展，它在生成时编译绑定。 {x:Bind} 绑定（通常指已编译的绑定）具有出色的性能、提供对绑定表达式的编译时验证，并通过允许你在作为页面的部分类生成的代码文件中设置断点来支持调试。 

请注意，x:Bind 并不适用于所有情况，如后期绑定方案。 有关 {x:Bind} 未涵盖的情况的完整列表，请参阅 {x:Bind} 文档。

## <a name="xname-is-being-used-instead-of-xkey"></a>正在使用 x:Name 而不是 x:Key

ResourceDictionaries 通常用于在某种程度的全局级别上存储资源，即，应用希望在多个位置引用的资源；例如，样式。画笔、模板等等。 一般情况下，我们优化了 ResourceDictionary，以便不实例化资源（除非另有要求）。 但是有一些你需要注意的地方。

### <a name="impact"></a>影响

创建 ResourceDictionary 后，将立即实例化具有 x:Name 的任何资源。 出现这种情况是因为 x:Name 告知平台你的应用需要对此资源的字段访问权限，因此平台需要创建某些内容来创建相关引用。

### <a name="cause"></a>原因

应用正在资源上设置 x:Name。

### <a name="solution"></a>解决方案

当你不从代码隐藏引用资源时，请使用 x:Key 而不是 x:Name。

## <a name="collections-control-is-using-a-non-virtualizing-panel"></a>集合控件正在使用非虚拟化面板

如果你提供自定义项目面板模板（请参阅 ItemsPanel），请确保使用虚拟化面板，如 ItemsWrapGrid 或 ItemsStackPanel。 如果使用 VariableSizedWrapGrid、WrapGrid 或 StackPanel，将不会获得虚拟化。 此外，只有当使用 ItemsWrapGrid 或 ItemsStackPanel 时才会引发以下 ListView 事件：ChoosingGroupHeaderContainer、ChoosingItemContainer 和 ContainerContentChanging。

UI 虚拟化是你可以对提升集合性能所做出的最重要的改进。 这意味着按需创建表示项目的 UI 元素。 对于绑定到 1000 个项目的集合的项目控件，同时为所有项目创建 UI 会造成资源浪费，因为它们不可能全部同时显示。 ListView 和 GridView（及其他标准 ItemsControl 派生的控件）可为你执行 UI 虚拟化。 当项目即将滚动到视图中时（只距离几页），框架将为这些项目生成 UI 并缓存它们。 当这些项目不太可能再次显示时，框架将回收内存。

UI 虚拟化只是提升集合性能的几个关键因素之一。 降低集合项目的复杂性和数据虚拟化是提升集合性能的另外两个重要方面。 有关在 ListViews 和 GridViews 内提升集合性能的详细信息，请参阅 [ListView 和 GridView UI 优化](https://msdn.microsoft.com/windows/uwp/debug-test-perf/optimize-gridview-and-listview)和 [ListView 和 Gridview 数据虚拟化](https://msdn.microsoft.com/windows/uwp/debug-test-perf/listview-and-gridview-data-optimization)上的文章。

### <a name="impact"></a>影响

非虚拟化 ItemsControl 将通过加载超出必要数量的子项增加加载时间和资源使用量。

### <a name="cause"></a>原因

你正在使用不支持虚拟化的面板。

### <a name="solution"></a>解决方案

使用虚拟化面板，如 ItemsWrapGrid 或 ItemsStackPanel。

## <a name="accessibility-uia-elements-with-no-name"></a>辅助功能：没有名称的 UIA 元素

在 XAML 中，你可以通过设置 AutomationProperties.Name 提供名称。 如果未设置 AutomationProperties.Name，许多自动化对等提供默认名称 UIA。 

### <a name="impact"></a>影响

如果用户接触到没有名称的元素，他们通常无法知道元素与何有关。 

### <a name="cause"></a>原因

Element 的 UIA 名称为 null 或为空。 此规则检查 UIA 看到的内容，而不是 AutomationProperties.Name 的值。

### <a name="solution"></a>解决方案

将控件的 XAML 中的 AutomationProperties.Name 属性设置为相应的本地化字符串。

有时正确的应用程序修复不是提供名称，而是从原始树以外的所有位置删除 UIA 元素。 可以通过设置 setting AutomationProperties.AccessibilityView = “Raw” 在 XAML 中执行此操作。

## <a name="accessibility-uia-elements-with-the-same-controltype-should-not-have-the-same-name"></a>辅助功能：具有相同 Controltype 的 UIA 元素不应具有相同的名称。

具有相同 UIA 父元素的两个 UIA 元素不得具有相同的 Name 和 ControlType。 如果 ControlType 不同，则可以存在具有相同 Name 的两个控件。 

此规则不检查具有不同父元素的重复名称。 但是，在大多数情况下，即使父元素不同，你也不应该在整个窗口内重复 Name 和 ControlType。 可以接受一个窗口内具有重复名称的情况是具有相同项目的两个列表。 在此情况下，列表项应具有相同的 Name 和 ControlType。

### <a name="impact"></a>影响

如果用户接触到与具有相同 UIA 父元素的另一个元素具有相同 Name 和 ControlType 的元素，则用户可能无法区分这些元素。

### <a name="cause"></a>原因

具有相同 UIA 父元素的 UIA 元素具有相同的 Name 和 ControlType。

### <a name="solution"></a>解决方案

使用 AutomationProperties.Name 在 XAML 中设置名称。 在经常发生此情况的列表中，使用绑定将 AutomationProperties.Name 的值绑定到数据源。


