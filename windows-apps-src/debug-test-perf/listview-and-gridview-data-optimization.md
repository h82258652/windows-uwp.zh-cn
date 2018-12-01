---
ms.assetid: 3A477380-EAC5-44E7-8E0F-18346CC0C92F
title: ListView 和 GridView 数据虚拟化
description: 通过数据虚拟化改进 ListView 和 GridView 性能和启动时间。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 411d19ba26dca1edff91fb7e5b432aa4da3bd120
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "8340764"
---
# <a name="listview-and-gridview-data-virtualization"></a>ListView 和 GridView 数据虚拟化


**注意**更多详细信息，请参阅 //build/ 会议：[可以显著提高性能与大量数据 GridView 和 ListView 中的用户交互时](https://channel9.msdn.com/Events/Build/2013/3-158)。

通过数据虚拟化改进 [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) 和 [**GridView**](https://msdn.microsoft.com/library/windows/apps/BR242705) 性能和启动时间。 有关 UI 虚拟化、元素缩减和项目的进度更新，请参阅 [ListView 和 GridView UI 优化](optimize-gridview-and-listview.md)。

数据虚拟化的方法 为此类数据集所需：容量过大以至于无法或不应一次全部存储在内存中。 将初始部分加载到内存中（从本地磁盘、网络或云），然后为此部分数据集应用 UI 虚拟化。 你可以稍后根据需要以增量方式或从主数据集中的任意点（随机访问）加载数据。 数据虚拟化是否适合你取决于许多因素。

-   数据集的大小
-   每个项目的大小
-   数据集的来源（本地磁盘、网络或云）
-   应用的总内存消耗。

**注意**是一项功能默认情况下启用的 ListView 和 GridView 的显示临时占位符视觉效果，当用户快速平移/滚动时。 加载数据时，这些占位符视觉效果将替换为你的项模板。 你可以通过将 [**ListViewBase.ShowsScrollingPlaceholders**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.showsscrollingplaceholders) 设置为 false 来关闭此功能，但是如果你这样做，我们建议你使用 x:Phase 属性在项模板中逐步呈现元素。 请参阅[逐步更新 ListView 和 GridView 项](optimize-gridview-and-listview.md#update-items-incrementally)。

下面是有关增量和随机访问数据虚拟化技术的更多详细信息。

## <a name="incremental-data-virtualization"></a>增量数据虚拟化

增量数据虚拟化会按顺序加载数据。 使用增量数据虚拟化的 [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) 可以用于查看包含一百万个项目的集合，但最初只加载 50 个项目。 当用户平移/滚动时，便加载接下来的 50 个项目。 当项目已加载后，滚动栏的滚动块大小将会减小。 对于此类型的数据虚拟化，需要编写实现这些接口的数据源类。

-   [**IList**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.ilist.aspx)
-   [**INotifyCollectionChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.specialized.inotifycollectionchanged.aspx) (C#/VB) 或 [**IObservableVector&lt;T&gt;**](https://msdn.microsoft.com/library/windows/apps/BR226052) (C++/CX)
-   [**ISupportIncrementalLoading**](https://msdn.microsoft.com/library/windows/apps/Hh701916)

诸如此类的数据源是可以不断扩展的内存中列表。 项目控件将使用标准 [**IList**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.ilist.aspx) 索引器和计数属性请求项目。 计数应表示本地项目的数目，而不是数据集的真实大小。

当项目控件接近现有数据的末尾时，它将调用 [**ISupportIncrementalLoading.HasMoreItems**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.isupportincrementalloading.hasmoreitems)。 如果返回 **true**，它将调用 [**ISupportIncrementalLoading.LoadMoreItemsAsync**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.isupportincrementalloading.loadmoreitemsasync)，用于传递要加载项目的建议数目。 根据加载数据的来源（本地磁盘、网络或云），你可以选择加载与建议数目不同的项目。 例如，如果你的服务支持 50 个项目的批处理，但项目控件仅请求 10 个项目，你仍可以加载 50 个项目。 从后端加载数据、将其添加到列表，然后通过 [**INotifyCollectionChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.specialized.inotifycollectionchanged.aspx) 或 [**IObservableVector&lt;T&gt;**](https://msdn.microsoft.com/library/windows/apps/BR226052) 引发变更通知，以使项目控件了解新项目。 还需返回实际记载的项目计数。 如果你加载的项目少于建议数目，或者项目控件甚至在此期间已进一步平移/滚动，则需要为更多项目再次调用数据源，然后继续循环。 你可以了解有关 Windows8.1 下载[XAML 数据绑定示例](https://code.msdn.microsoft.com/windowsapps/Data-Binding-7b1d67b5)和 windows 10 应用中重复使用其源代码。

## <a name="random-access-data-virtualization"></a>随机访问数据虚拟化

随机访问数据虚拟化允许通过数据集中的任意点加载。 使用随机访问数据虚拟化的 [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878)（用于查看包含一百万个项目的集合）可以加载前 100000 到 100050 个项目。 如果用户随后移动到列表开头，控件将加载前 1 到 50 个项目。 滚动栏的滚动块将始终指示 **ListView** 包含一百万个项目。 滚动栏的滚动块位置相对于可视项目在集合的整个数据集中的位置。 这种类型的数据虚拟化可以显著减少集合的内存需求和加载时间。 若要启用它，需要编写数据源类，用于按需获取数据、管理本地缓存以及实现这些接口。

-   [**IList**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.ilist.aspx)
-   [**INotifyCollectionChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.specialized.inotifycollectionchanged.aspx) (C#/VB) 或 [**IObservableVector&lt;T&gt;**](https://msdn.microsoft.com/library/windows/apps/BR226052) (C++/CX)
-   （可选）[**IItemsRangeInfo**](https://msdn.microsoft.com/library/windows/apps/Dn877070)
-   （可选）[**ISelectionInfo**](https://msdn.microsoft.com/library/windows/apps/Dn877074)

[**IItemsRangeInfo**](https://msdn.microsoft.com/library/windows/apps/Dn877070) 提供关于控件正在使用哪些项目的信息。 只要项目控件的视图发生变化，它便调用此方法，并将包含以下两组范围。

-   视口中的项目集。
-   可能不在视口中的由控件使用的非虚拟化项目集。
    -   项目控件为实现平滑的触摸平移而保留的视口周围项目的缓冲区。
    -   聚焦项目。
    -   第一个项目。

通过实现 [**IItemsRangeInfo**](https://msdn.microsoft.com/library/windows/apps/Dn877070)，你的数据源将了解哪些项目需要获取和缓存，以及何时删除不再需要的缓存数据。 **IItemsRangeInfo** 使用 [**ItemIndexRange**](https://msdn.microsoft.com/library/windows/apps/Dn877081) 对象根据项目在集合中的索引对项目集进行描述。 这样便可以不必使用项目指针，指针可能会不正确或不稳定。 **IItemsRangeInfo** 专门仅供项目控件的单个实例使用，因为它依赖于该项目控件的状态信息。 如果多个项目控件需要访问相同数据，则需要使用每个控件的数据源的单独实例。 它们可以共享通用缓存，但从缓存中执行删除的逻辑会更加复杂。

下面是随机访问数据虚拟化数据源的基本策略。

-   当系统请求某个项目时
    -   如果内存中提供该项目，则将其返回。
    -   如果没有该项目，则返回 null 或占位符项。
    -   使用对项目的请求（或者来自 [**IItemsRangeInfo**](https://msdn.microsoft.com/library/windows/apps/Dn877070) 的范围信息）了解所需项目，并以异步方式从后端获取项目的数据。 检索数据后，通过 [**INotifyCollectionChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.specialized.inotifycollectionchanged.aspx) 或 [**IObservableVector&lt;T&gt;**](https://msdn.microsoft.com/library/windows/apps/BR226052) 引发更改通知，以使项目控件了解新项目。
-   （可选）当项目控件的视区更改时，通过实现 [**IItemsRangeInfo**](https://msdn.microsoft.com/library/windows/apps/Dn877070) 来标识需要使用数据源中的哪些项目。

除此之外，对于加载数据项目的时间、加载数目、在内存中所保留项目的策略，取决于你的应用程序。 需要记住的一些常规注意事项：

-   采用异步方式请求数据；请勿阻止 UI 线程。
-   找到获取项目时采用的批处理的最合适大小。 使用 chunky 而不是 chatty。 不要过小以免进行太多小的请求；不要过大以免检索时间过长。
-   请考虑你希望在同一时间内挂起多少请求。 一次执行一个请求较为容易，但是如果周转时间较长，速度将过于缓慢。
-   是否可以取消对数据的请求？
-   如果使用托管服务，每次执行事务是否收费？
-   当查询结果发生变化时，服务将提供哪种类型的通知？ 当项目在索引 33 处插入时你是否要知晓？ 如果你的服务支持基于“键加偏移”的查询，这可能会优于只使用索引。
-   在预先获取项目方面需要多高的智能程度？ 是否要试图跟踪滚动的方向和速度以预测所需项目？
-   在清除缓存方面需要多高的积极程度？ 这是内存与体验的权衡。




