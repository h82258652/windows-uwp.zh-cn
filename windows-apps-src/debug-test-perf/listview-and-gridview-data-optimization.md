---
ms.assetid: 3A477380-EAC5-44E7-8E0F-18346CC0C92F
title: ListView 和 GridView 数据虚拟化
description: 通过数据虚拟化改进 ListView 和 GridView 性能和启动时间。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7d00a41c5a58935a4ecfe623c71a1264a2dc1132
ms.sourcegitcommit: a20457776064c95a74804f519993f36b87df911e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71339614"
---
# <a name="listview-and-gridview-data-virtualization"></a>ListView 和 GridView 数据虚拟化


**请注意**，  有关更多详细信息，请参阅 build/会话在[用户与 GridView 和 ListView 中的大量数据交互时大大提高性能](https://channel9.msdn.com/Events/Build/2013/3-158)。

通过数据虚拟化改进 [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) 和 [**GridView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView) 性能和启动时间。 有关 UI 虚拟化、元素缩减和项目的进度更新，请参阅 [ListView 和 GridView UI 优化](optimize-gridview-and-listview.md)。

数据虚拟化的方法 为此类数据集所需：容量过大以至于无法或不应一次全部存储在内存中。 将初始部分加载到内存中（从本地磁盘、网络或云），然后为此部分数据集应用 UI 虚拟化。 你可以稍后根据需要以增量方式或从主数据集中的任意点（随机访问）加载数据。 数据虚拟化是否适合你取决于许多因素。

-   数据集的大小
-   每个项目的大小
-   数据集的来源（本地磁盘、网络或云）
-   应用的总内存消耗。

**请注意  请**注意，默认情况下，为 ListView 和 GridView 启用了一项功能，当用户快速平移/滚动时，会显示临时占位符视觉对象。 加载数据时，这些占位符视觉效果将替换为你的项模板。 你可以通过将 [**ListViewBase.ShowsScrollingPlaceholders**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.showsscrollingplaceholders) 设置为 false 来关闭此功能，但是如果你这样做，我们建议你使用 x:Phase 属性在项模板中逐步呈现元素。 请参阅[逐步更新 ListView 和 GridView 项](optimize-gridview-and-listview.md#update-items-incrementally)。

下面是有关增量和随机访问数据虚拟化技术的更多详细信息。

## <a name="incremental-data-virtualization"></a>增量数据虚拟化

增量数据虚拟化会按顺序加载数据。 使用增量数据虚拟化的 [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) 可以用于查看包含一百万个项目的集合，但最初只加载 50 个项目。 当用户平移/滚动时，便加载接下来的 50 个项目。 当项目已加载后，滚动栏的滚动块大小将会减小。 对于此类型的数据虚拟化，需要编写实现这些接口的数据源类。

-   [**IList**](https://docs.microsoft.com/dotnet/api/system.collections.ilist)
-   [**INotifyCollectionChanged**](https://docs.microsoft.com/dotnet/api/system.collections.specialized.inotifycollectionchanged) （C#/VB）或[**IObservableVector&lt;t&gt;** ](https://docs.microsoft.com/uwp/api/Windows.Foundation.Collections.IObservableVector_T_) （C++/cx）
-   [**为 isupportincrementalloading**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.ISupportIncrementalLoading)

诸如此类的数据源是可以不断扩展的内存中列表。 项目控件将使用标准 [**IList**](https://docs.microsoft.com/dotnet/api/system.collections.ilist) 索引器和计数属性请求项目。 计数应表示本地项目的数目，而不是数据集的真实大小。

当项目控件接近现有数据的末尾时，它将调用 [**ISupportIncrementalLoading.HasMoreItems**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.isupportincrementalloading.hasmoreitems)。 如果返回 **true**，它将调用 [**ISupportIncrementalLoading.LoadMoreItemsAsync**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.isupportincrementalloading.loadmoreitemsasync)，用于传递要加载项目的建议数目。 根据加载数据的来源（本地磁盘、网络或云），你可以选择加载与建议数目不同的项目。 例如，如果你的服务支持 50 个项目的批处理，但项目控件仅请求 10 个项目，你仍可以加载 50 个项目。 从后端加载数据、将其添加到列表，然后通过 [**INotifyCollectionChanged**](https://docs.microsoft.com/dotnet/api/system.collections.specialized.inotifycollectionchanged) 或 [**IObservableVector&lt;T&gt;** ](https://docs.microsoft.com/uwp/api/Windows.Foundation.Collections.IObservableVector_T_) 引发变更通知，以使项目控件了解新项目。 还需返回实际记载的项目计数。 如果你加载的项目少于建议数目，或者项目控件甚至在此期间已进一步平移/滚动，则需要为更多项目再次调用数据源，然后继续循环。 若要了解详细信息，可以下载 Windows 8.1 的[XAML 数据绑定示例](https://code.msdn.microsoft.com/windowsapps/Data-Binding-7b1d67b5)，然后在 Windows 10 应用中重复使用其源代码。

## <a name="random-access-data-virtualization"></a>随机访问数据虚拟化

随机访问数据虚拟化允许通过数据集中的任意点加载。 使用随机访问数据虚拟化的 [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView)（用于查看包含一百万个项目的集合）可以加载前 100000 到 100050 个项目。 如果用户随后移动到列表开头，控件将加载前 1 到 50 个项目。 滚动栏的滚动块将始终指示 **ListView** 包含一百万个项目。 滚动栏的滚动块位置相对于可视项目在集合的整个数据集中的位置。 这种类型的数据虚拟化可以显著减少集合的内存需求和加载时间。 若要启用它，需要编写数据源类，用于按需获取数据、管理本地缓存以及实现这些接口。

-   [**IList**](https://docs.microsoft.com/dotnet/api/system.collections.ilist)
-   [**INotifyCollectionChanged**](https://docs.microsoft.com/dotnet/api/system.collections.specialized.inotifycollectionchanged) （C#/VB）或[**IObservableVector&lt;t&gt;** ](https://docs.microsoft.com/uwp/api/Windows.Foundation.Collections.IObservableVector_T_) （C++/cx）
-   （可选）[**IItemsRangeInfo**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.IItemsRangeInfo)
-   （可选）[**ISelectionInfo**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.ISelectionInfo)

[**IItemsRangeInfo**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.IItemsRangeInfo)提供有关控件主动使用哪些项的信息。 只要项目控件的视图发生变化，它便调用此方法，并将包含以下两组范围。

-   视口中的项目集。
-   可能不在视口中的由控件使用的非虚拟化项目集。
    -   项目控件为实现平滑的触摸平移而保留的视口周围项目的缓冲区。
    -   聚焦项目。
    -   第一个项目。

通过实现 [**IItemsRangeInfo**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.IItemsRangeInfo)，你的数据源将了解哪些项目需要获取和缓存，以及何时删除不再需要的缓存数据。 **IItemsRangeInfo** 使用 [**ItemIndexRange**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.ItemIndexRange) 对象根据项目在集合中的索引对项目集进行描述。 这样便可以不必使用项目指针，指针可能会不正确或不稳定。 **IItemsRangeInfo** 专门仅供项目控件的单个实例使用，因为它依赖于该项目控件的状态信息。 如果多个项目控件需要访问相同数据，则需要使用每个控件的数据源的单独实例。 它们可以共享通用缓存，但从缓存中执行删除的逻辑会更加复杂。

下面是随机访问数据虚拟化数据源的基本策略。

-   当系统请求某个项目时
    -   如果内存中提供该项目，则将其返回。
    -   如果没有该项目，则返回 null 或占位符项。
    -   使用对项目的请求（或者来自 [**IItemsRangeInfo**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.IItemsRangeInfo) 的范围信息）了解所需项目，并以异步方式从后端获取项目的数据。 检索数据后，通过 [**INotifyCollectionChanged**](https://docs.microsoft.com/dotnet/api/system.collections.specialized.inotifycollectionchanged) 或 [**IObservableVector&lt;T&gt;** ](https://docs.microsoft.com/uwp/api/Windows.Foundation.Collections.IObservableVector_T_) 引发更改通知，以使项目控件了解新项目。
-   （可选）当项目控件的视区更改时，通过实现 [**IItemsRangeInfo**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.IItemsRangeInfo) 来标识需要使用数据源中的哪些项目。

除此之外，对于加载数据项目的时间、加载数目、在内存中所保留项目的策略，取决于你的应用程序。 需要记住的一些常规注意事项：

-   采用异步方式请求数据；请勿阻止 UI 线程。
-   找到获取项目时采用的批处理的最合适大小。 使用 chunky 而不是 chatty。 不要过小以免进行太多小的请求；不要过大以免检索时间过长。
-   请考虑你希望在同一时间内挂起多少请求。 一次执行一个请求较为容易，但是如果周转时间较长，速度将过于缓慢。
-   是否可以取消对数据的请求？
-   如果使用托管服务，每次执行事务是否收费？
-   当查询结果发生变化时，服务将提供哪种类型的通知？ 当项目在索引 33 处插入时你是否要知晓？ 如果你的服务支持基于“键加偏移”的查询，这可能会优于只使用索引。
-   在预先获取项目方面需要多高的智能程度？ 是否要试图跟踪滚动的方向和速度以预测所需项目？
-   在清除缓存方面需要多高的积极程度？ 这是内存与体验的权衡。




