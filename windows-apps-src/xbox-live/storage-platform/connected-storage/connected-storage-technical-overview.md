---
title: 连接存储技术概述
author: aablackm
description: 连接存储内部工作原理的深入探讨。
ms.assetid: a0bacf59-120a-4ffc-85e1-fbeec5db1308
ms.author: aablackm
ms.date: 02/27/2018
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, 连接存储
ms.localizationpriority: medium
ms.openlocfilehash: 8740d9287ff63ba113266d6c7cf39f2a21823d4b
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/09/2018
ms.locfileid: "6182149"
---
# <a name="connected-storage"></a>连接存储

> [!NOTE]
> 此文档最初是为托管的合作伙伴 Xbox One 开发人员编写的。  Windows 上的 UWP 可以忽略某些特定于 Xbox One 的内容（例如本地和游戏存储）。  此文档中的概念内容和 API 仍然相关。  如有任何疑问，请与 Microsoft 代表联系（如果适用）。

Xbox One 和 Xbox 360 上的存储和保存模型有很大的不同；Xbox One 的应用程序模型更加灵活，支持快速应用程序切换、多个同时应用程序和应用的快速暂停和恢复。 采用连接存储 API 存储的数据会自动漫游，可供用户跨多个 Xbox One 主机使用，并且还能脱机使用。

本主题讲解：

-   使用连接存储 API 存储 Xbox One 上保存的游戏和应用数据。
-   应用设计的最佳做法会保存系统，以便它们与由 Xbox One 应用程序模型提供的用户体验优势良好集成。
-   连接存储系统如何将应用保存数据的速度最大化
-   系统如何处理多个控制台中的数据同步和数据冲突，而无需应用 UI。
-   连接存储复原模型的目的是为了让应用始终拥有存储在单独容器中的数据的本身一致视图，即使在中断网络连接或断电的情况下也是如此。

> [!NOTE]
> 正如此处使用的那样，术语*应用* 指在主机上运行的任意应用程序，包括游戏。

## <a name="overview"></a>概述

Xbox One 应用程序模型允许用户同时使用多个应用，这就表示，在关闭控制台或移动到其他应用之前，你的应用无法要求用户等待系统保存数据。 Xbox One 用户还会喜欢在主机中自动漫游数据，就好像每台 Xbox One 主机都是为其量身定做的一样。 Xbox One 平台提供连接存储 API 来帮助你的应用满足这些要求。

连接存储系统允许应用将数据存储为*容器*中的一个或多个 *blob*。 当应用保存数据时，它会从独占分区被快速复制到共享分区，以便能够在应用的生存期外处理存储磁盘上的数据并将其上传到 Xbox Live 标题存储的任务。

当应用从连接存储系统请求特定的用户数据时，系统会自动对云进行已更新数据检查，并在用户需要等待数据下载时通知用户。 在某些情况下，系统还会让用户在冲突数据之间选择，例如，当用户在多个控制台上脱机运行，或其他控制台正在为此用户上传保存的数据时。

应用还有针对每个用户的专用（但有限的）云存储数量，因此，用户无需做出与永久删除某个应用中的数据，为其他应用的保存提供空间有关的艰难选择。 但是，Xbox One 硬盘上用于本地保存的缓存的存储空间有限，因此，系统会提供释放本地缓存空间的用户体验。 用户可以控制本地缓存的内容；脱机运行时，也绝不会失去对数据的访问权限。 连接存储系统还允许应用将少量与用户无关的数据存储在本地。 这一按照计算机的数据不会漫游，也不会上传到云。

Xbox One 连接存储系统负责系统电源管理，因此会自动处理待写入硬盘和上传到云的内容—游戏无需显示内容为“正在保存，请不要关闭你的控制台”的 UI。

### <a name="the-xbox-one-app-model-and-app-navigation"></a>Xbox One 应用模型和应用导航

Xbox One 允许用户在多个应用程序之间快速切换。 正确编写的应用程序让用户可以使用快速加载的相关上下文，选择他们在上次活动期间的离开位置，即使应用自用户上次访问后已关闭。

Xbox One 主机一次只能在独占分区中运行一个应用。 展示快速切换用户体验需要在用户想运行其他应用时，快速关闭当前运行的独占应用。 当用户尝试切换到其他应用时，系统会向活动应用发送暂停通知。 在此期间，应用应该保存相关状态，并从其暂停功能中返回。 系统会为此操作强制执行 1 秒钟的最大时间限制。 如果应用在 1 秒钟内尚未返回，系统会强制终止应用程序。 与现代智能手机和平板电脑上的导航模型一样，应用无法阻止用户导航离开。

还存在其他暂停应用的情况，例如，当用户按下控制台上的“电源”按钮时，系统会进入空闲或低功耗状态。 应用暂停后，无需系统卸载即可恢复。 这样可以启用快速恢复功能。 请务必了解，暂停的应用还有可能会终止或恢复。 应用必须始终保存状态，以防其终止。

要正确使用 Xbox One 应用程序模型，应用应准备好将状态快速序列化到内存缓冲区中，以便可以在 1 秒钟的暂停期限内保存相关状态。

请注意，关于用户游戏的已保存数据和关于应用状态的数据（例如菜单中的位置）之间存在差异。 除了在应用暂停时保存游戏外，如果用户正在配置设置或自定义主游戏引擎之外的特征，则你应该考虑保持菜单状态。

用户可以长时间将游戏保持为暂停状态。 当游戏在长时间暂停之后继续时，请考虑提供不同的体验。 如果用户玩游戏的时间不超过两周，那么，将用户从活动重新释放到交火中可能是一种不和谐且意外的体验，而一小时的休息可能更常见，而且可以保证快速返回游戏。

有关 Xbox One 应用模型的更多信息，请参阅以下资源：

-   [客厅的现代应用程序切换](https://developer.xboxlive.com/en-us/platform/documentlibrary/events/Documents/Xfest%202012/Xfest%202012%20-%20Modern%20Application%20Switching%20for%20the%20Living%20Room.pptx)，Xfest 2012 上的一个演示
-   [Shell 体验](https://developer.xboxlive.com/_layouts/xna/download.ashx?file=PROD-D_Experience.pptx&folder=platform/xfest2013)，Xfest 2013 上的一个演示
-   [Xbox One 的进程周期管理](https://developer.xboxlive.com/en-us/platform/development/education/Documents/PLM%20for%20Xbox%20One.aspx)，GDN 上的一份白皮书

> [!NOTE]
> 由于 Xbox One 支持脱机运行的公告，[Xfest 2012](https://developer.xboxlive.com/en-us/platform/documentlibrary/events/Pages/Xfest2012.aspx) 和 [Xfest 2013](https://developer.xboxlive.com/en-us/platform/documentlibrary/events/Pages/Xfest2013.aspx) 上的部分演示包含过期的信息。


### <a name="storage-options-on-xbox-one"></a>Xbox One 上的存储选项

Xbox One 提供多个存储选项，每个选项都有各自的优势和限制。 应用可能需要将选项组合使用，具体取决于应用的要求。


<a name="connected-storage"></a>连接存储
-----------------

连接存储旨在帮助应用保存 Xbox One 游戏数据，以及应该在控制台之间漫游的其他相关的应用状态数据。 连接存储 API（特定于 Xbox One）可协助你保存和上传此数据。 API 可与 Xbox One 应用程序模型结合使用。

连接存储 API 提供以下功能：

-   应用一次可以将多达 16 MB 的数据快速保存到系统分区的内存缓冲区中，这些数据稍后会由系统在 HDD 上本地缓存，然后上传到云中。
- 对于托管的合作伙伴和 ID@Xbox 开发人员：
  - 每个云存储的用户/应用 256 MB。
- 对于 Xbox Live 创意者计划开发人员：
  - 每个云存储的用户/应用 64 MB。
-   电源失败的可靠响应—应用无需处理正在保存的部分数据。
-   数据会自动上传到云，即使应用没有运行。
-   数据在连接到 Xbox Live 的 Xbox One 主机中可用。
-   Xbox Live 可以处理跨设备的同步和冲突管理，无需应用参与。

有关连接存储 API 的更多信息，请参阅 Xbox Live SDK 的 xblesdk.chm（其中记录了 Xbox Live Extension SDK API）中的相应部分。


<a name="xbox-live-title-storage"></a>Xbox Live 标题存储
-----------------------

标题存储服务使用以下功能为数据存储提供跨平台 REST API：

-   可跨用户、应用和各平台共享数据
-   支持二进制文件、JSON 和配置文件
-   对于托管的合作伙伴和 ID@Xbox 开发人员：
    - 每个云存储的用户/应用 256 MB
    - 每个游戏全局存储 256 MB
- 对于 Xbox Live 创意者计划开发人员：
  -   每个云存储的用户/应用 64 MB
  -   每个游戏全局存储 256 MB

使用此服务的要求：

-   Xbox One 主机必须处于联机状态才能访问服务
-   所有服务交互必须在应用运行时完成；数据传输不会在后台自动完成。

有关更多信息，请参阅 XDK 文档中的 *Xbox Live 标题存储*。


<a name="local-temporary-storage"></a>本地临时存储
-----------------------

在控制台上，应用有权访问具有以下特征的本地临时存储：

-   2 GB 的专用硬盘存储，可通过路径 T:\\ 访问。
-   应用未运行时，此存储的内容可能会退出。

有关本地存储的更多信息，请参阅 XDK 文档中的“本地存储”。


<a name="configuring-your-app-for-connected-storage"></a>配置应用的连接存储
------------------------------------------

使用连接存储 API 时，所有读写操作都与在应用的清单文件 (AppXManifest.xml) 中定义的 Xbox Live 主要服务配置 ID (SCID) 关联：

```xml
      <Extensions>
        <mx:Extension Category="xbox.live">
        <mx:XboxLive TitleId="<your title ID>" PrimaryServiceConfigId="<your SCID>"
        RequireXboxLive="<boolean indicating Live requirement>" />
        </mx:Extension>
      </Extensions>
```

有关为应用获取游戏 ID 和 SCID 的更多信息，请参阅 XDK 文档中的*为 Xbox Live 开发设置沙盒*。



## <a name="connected-storage-system-concepts"></a>连接存储：系统概念

本部分介绍连接存储系统的组件、它们的关系以及它们的正确使用。

### <a name="connected-storage-space"></a>连接存储空间

在高级别上，连接存储系统中的所有数据都与用户或计算机（例如，一台单独的 Xbox One 主机）关联。 由特定用户或计算机的应用保存的所有数据都存储在连接存储空间中。

应用的每个用户都能获取总存储限制为 256 MB 的连接存储空间。 请注意，此存储仅专用于应用—不与其他应用共享。

应用在计算机的本地连接存储空间中还有 64 MB 的空间。 此存储空间与用户无关，即使没有用户登录也可以访问。

要获取连接存储空间，应用可以调用 *ConnectedStorageSpace.GetForUserAsync 方法*或 *ConnectedStorageSpace.GetForMachineAsync 方法*。 这可能是一个长时间运行的操作，尤其是，如果用户已经在一台设备上保存数据，并第一次在其他设备上继续游戏。 有关此过程的详细信息，以及应用在等待获取连接存储空间时可能发生的错误情况，请参阅本文后面的*同步连接存储空间*。

应用获取 **ConnectedStorageSpace** 对象后，请调用使用此对象（或派生自此对象的其他对象）的 *Windows.Storage Namespace* 下的所有方法，无需依赖 Web 服务中的响应即可完成。 但是，由于对 Xbox One HDD 的访问并非活动的应用独有，因此，无法对这些方法的性能的严格上限作出保证。


### <a name="connected-storage-containers-and-blobs"></a>连接存储：容器和 blob

*连接存储容器*（或简称*容器*）是基本的存储单位。 每个连接存储空间都可以包含大量容器，如下图中所示。

**图 1.连接存储空间（每个游戏/计算机或每个游戏/用户）**

![](../../images/connected_storage/connected_storage_space_containers.png)
数据会作为一个或多个称为 *blob* 的缓冲区存储在容器中。 下图说明了磁盘上容器的内部系统表示形式。 每个容器中都有一个容器文件，此文件包含对容器中的每个 blob 的数据文件的引用。

**图 2.容器图示**

![](../../images/connected_storage/container_storage_blobs.png)

若要存储容器中的数据，请调用 *ConnectedStorageContainer.SubmitUpdatesAsync Method*，提供名称和 blob（缓冲区对象）的映射。 **SubmitUpdatesAsync** 调用中描述的所有更改会自动应用，也就是说，所有 blob 都会根据要求更新，或者，整个操作都会终止，且容器会保持其在调用之前的状态。

使用 **SubmitUpdatesAsync** 的单独保存操作限制为一次 16 MB 的数据。


### <a name="submitupdatesasync-behavior"></a>SubmitUpdatesAsync 行为

调用 **SubmitUpdatesAsync** 时，为调用提供的缓冲区会从应用分区快速被复制到系统分区中的专用内存空间中。 将内存成功复制到系统分区中后，会在应用内调用 **SubmitUpdatesAsync** 调用中提供的完成处理程序，指示可安全地将其在本地为数据分配的内存释放到应用。

然后，系统会将 blob 保存到控制台的硬盘中，并通过在此容器上执行整个操作的最终容器更新完成操作。

共享分区中的内存上有 16 MB 的上限可用于接收 **SubmitUpdatesAsync** 数据。 如果由于专用的 16 MB 缓冲区中没有足够的可用内存，导致系统无法立即为对 **SubmitUpdatesAsync** 的调用提供服务，则调用将排队等待服务。 系统会持续将数据从 16 MB 缓冲区传输到硬盘，而排队的更新会按照当 16 MB 缓冲中的空间可用时要求的顺序获得服务。

**图 3.SubmitUpdatesAsync 行为**

![](../../images/connected_storage/submitupdatesasync_behavior.png)
上传到云的方式与之相似：各个 blob 都会上传到服务中，并由引用所有其他上传的 blob 的容器文件的最终更新执行更新操作。 上传到云的过程中，合并到单一的最终更新可确保在 **SubmitUpdatesAsync** 中引用的所有数据完整地提交，或保持容器不变。 这样一来，即使系统在上传操作期间脱机或断电，用户还可以转到其他 Xbox One 主机，从云中下载数据，并通过所有容器的一致视图继续玩游戏。

> [!IMPORTANT]
> 跨容器的数据依赖关系是不安全的。  单个 *SubmitUpdatesAsync* 调用的结果可以保证完全应用，或完全不应用。

**SubmitUpdatesAsync** 调用不得假定将来的 **SubmitUpdatesAsync** 调用会成功完成，以便将容器保持为有效状态。 换言之，应用无法依赖多个 **SubmitUpdatesAsync** 调用来将所有需要的数据保存到容器中。 每个 **SubmitUpdatesAsync** 调用必须将指定容器的内容保持为有效状态，以便应用稍后读取。

要说明此问题，请考虑以下场景：容器跟踪由名为 Bob 的人物持有的黄金和食物数量。 游戏可以存储两个 blob，分别名为*食物*和*黄金*。 开始时，Bob 的库存中有 100 块黄金，没有食物。

**图 4.示例场景：开始时，Bob 有 100 块黄金。**

![](../../images/connected_storage/submitupdatesasync_example_scenario1.png)

现在，Bob 花掉了 50 块黄金。 游戏准备进行 **SubmitUpdatesAsync** 调用，他会将黄金 blob 的值更新为 50。

系统会将更新的 blob 和有关容器更新的信息捕获到更新缓冲区中。 然后，系统会将新的 blob 值复制到硬盘中。

**图 5.系统会捕获更新的信息，并将值复制到硬盘中。**

![](../../images/connected_storage/submitupdatesasync_example_scenario2.png)

最后，系统会更新 HDD 上的容器文件，以引用新的 blob。 最终，系统会在垃圾回收操作中删除未引用的 blob。

**图 6.系统会更新 HDD 上的容器文件，并删除未引用的 blob。**

![](../../images/connected_storage/submitupdatesasync_example_scenario3.png)

请注意，在每次 **SubmitUpdatesAsync** 调用使用的 blob 越多，完成文件系统操作的必要原子操作（以便可靠地存储数据）所需时间就越长。 上述示例中数据存储的粒度过小，但可以用于清楚地演示一个容器中多个 blob 的原子更新行为。


### <a name="updating-multiple-blobs--the-wrong-way"></a>更新多个 blob — 错误方法

考虑 Bob 想买一些食物的场景。 为简单起见，我们假设 1 个单位的黄金可以购买 1 个单位的食物，而 Bob 想买 25 个单位的食物。 应用可以发出一个 **SubmitUpdatesAsync** 调用来添加 25 个单位的食物，然后再发出另一个调用，来从 Bob\_Inventory 容器中减去 25 个单位的黄金。 但是，即使调用了两个 **SubmitUpdatesAsync** 调用的已完成处理程序，仍有可能会因断电等事件而产生错误结果，这可能会阻止数据写入硬盘，或导致与云的同步不完整。 下图说明了由系统采取的步骤，以及在任一步骤中断电时会发生的结果。

假定两个 **SubmitUpdatesAsync** 调用中的数据都已经位于系统的更新缓冲区中，并且已经调用了两个调用的游戏的完成处理程序。

首先，系统将新的食物 blob 值的数据写入磁盘。

**图 7.系统将食物 blob 值写入磁盘。**

![](../../images/connected_storage/update_method_wrong_way_1.png)
然后，系统会更新容器，以引用新写入的值。 如下图所示，如果在此步骤之后，下个步骤之前断电，Bob 将会以一笔划算的交易结束，他会获得 25 份食物，而无需从库存中减去相应的黄金。

**图 8.系统会更新容器，以引用新写入的值。**

![](../../images/connected_storage/update_method_wrong_way_2.png)

接下来，系统将新的黄金 blob 值的数据写入磁盘。 由 Bob\_Inventory 容器引用的黄金的值仍未更新，且 Bob 比应得的情况多出了 25 块黄金—但是我们离所需的结果又近了一步。

**图 9.系统将新的黄金 blob 值的数据写入磁盘。**

![](../../images/connected_storage/update_method_wrong_way_3.png)

最后，系统会更新容器文件，以引用新写入的黄金 blob — 这是预期的结果。

**图 10.系统会更新容器文件，以引用新写入的黄金 blob。**

![](../../images/connected_storage/update_method_wrong_way_4.png)

### <a name="updating-multiple-blobs--the-right-way"></a>更新多个 blob — 正确方法

确保 Bob 库存中的黄金和食物以原子方式更新（不可能由于断电而出现错误的中间状态）的正确方法是，在单个 **SubmitUpdatesAsync** 调用中更新两个 blob。 然后，系统会执行以下步骤。

首先，系统将新的食物 blob 值的数据写入磁盘。

**图 11.系统写入新的食物 blob 值的数据。**

![](../../images/connected_storage/update_method_right_way_1.png)
然后，系统将新的黄金 blob 值的数据写入磁盘。

**图 12.系统写入新的黄金 blob 值的数据。**

![](../../images/connected_storage/update_method_right_way_2.png)
最后，系统会更新容器文件，以同时引用两个新的 blob。

**图 13.系统会更新容器文件，以同时引用两个新的 blob。**

![](../../images/connected_storage/update_method_right_way_3.png)
虽然此示例非常简单，但它说明了通过发出拥有所有需要的更新的 **SubmitUpdatesAsync** 调用，在必须以原子方式应用的容器中进行所有数据更改的重要性。 在使用黄金购买食物的情况中执行此操作后，应用会避免可能出现的争用情况，这种情况会错误地仅更新一个值，并使人物留有过多黄金。

### <a name="performance-characteristics-and-considerations"></a>性能特征和注意事项

共享分区中的 16 MB 更新缓冲区允许非常快速地执行有限数量的更新操作。 系统可以将数据保留到磁盘中所用的速度取决于缓冲区中的数据量以及 blob 数。 由于每个 blob 都通过复原写入磁盘，因此，缓冲区中的 blob 数越多，将 blob 保留到磁盘中所花的时间就越长。

图 13 显示了，当系统中没有其他硬盘活动时，在使用两个 512k blob 更新和一个 1024k blob 更新的情况下，每 2 秒 **SubmitUpdatesAsync** 的操作处理时间示例。 系统可以保持稳定状态运行，处理每个更新的时间在 14–18 毫秒以内。

**图 14.在使用两个 512k blob 更新和一个 1024k blob 更新（且没有其他硬盘活动）的情况下，每 2 秒 SubmitUpdatesAsync 的操作处理时间。**

![](../../images/connected_storage/submitupdatesasync_proc_time_mixed_size_fixed_interval.png)
图 14 显示了在不同的时间间隔下，三个 1024k blob 的处理时间。

在 3 秒的间隔下，系统可以保持 87 毫秒的稳定状态处理这些更新。 将频率增加为每 2 秒一次，系统仍然可以在 87 毫秒的稳定状态内处理更新。

将更新之间的间隔减少为 1 秒会改变稳定状态行为。 系统可以保持 87 毫秒/更新的状态处理 60 个更新，但这以外的每个更新花费的时间明显变长，每个更新的稳定状态处理时间可达到 500 毫秒，伴随着明显波动。 这是因为填写 16 MB 内存缓冲区的速度比它将数据刷新到磁盘的速度快；更新被迫等待之前的更新写入。

当把间隔更新为每 0.5 秒一个更新时，效果会显著增加。 在达到处理每个更新花费的时间超过 1 秒的稳定状态之前，系统只能以此间隔处理 7 个更新，然后又会变为 87 毫秒/更新，变化很大。

**图 15.在不同的时间间隔下，对三个 1024k blob 的处理时间。**

![](../../images/connected_storage/submitupdatesasync_proc_time_fixed_size_various_intervals.png)
以下仅是说明示例。 应用通常不应该如此频繁地保存数据，但它通常也不会在没有磁盘 I/O 的环境中运行。

请务必基于这些示例了解系统的特征，以在不同的操作条件下衡量此应用，从而确保在应用的暂停处理程序期间，可以在 1 秒钟以内完成保存操作。


## <a name="synchronizing-a-connected-storage-space"></a>同步连接存储空间

-   连接检查
-   锁定获取
-   容器列出、比较和合并逻辑
-   容器下载

当应用请求访问连接存储空间时，系统会执行同步过程，以将 Xbox One 主机中用户的保存数据保持为统一状态，并使用户数据可在脱机运行时使用。 由于同步花费的时间可能各有不同，且可能需要用户做决策，系统可能会此过程的不同阶段向用户显示 UI。

用户可以通过随时按下 Xbox 按钮导航离开应用，即使同步 UI 处于活动状态。 系统会隐藏 UI，且同步会在没有用户交互的情况下尽可能继续。 当用户导航回应用时，会再次显示 UI，除非同步已经完成。 UI 隐藏时，系统绝不会做出有关用户选择的假设。

由于当用户位于“主页”屏幕时，系统不显示同步 UI，且应用的呈现在大应用磁贴中仍可见，因此，当 **GetForUserAsync** 调用完成时，应用会呈现上下文适当的视觉对象，这一点很重要。 持续的呈现会向用户指示，应用仍然是交互式的，并且正在等待数据加载。

下图概述了当应用请求连接存储空间时，系统遵循的高级序列。 如果整个序列花费的时间超过数秒，则会显示系统绘制的同步 UI。

**图 16.当应用请求连接存储空间时，系统遵循的序列。**

![](../../images/connected_storage/app_requests_connected_storage_space.png)
为 **GetForUserAsync** 请求提供服务时，系统会通过以下阶段：

-   连接检查
-   锁定获取
-   容器列出、比较和合并逻辑
-   容器下载

### <a name="connectivity-check"></a>连接检查

要开始为 **GetForUserAsync** 请求提供服务，系统会检查连接。 如果控制台处于脱机状态，则会跳过整个同步过程，并会为当前会话将指定用户的连接存储空间标记为脱机。 在应用下次访问同一个用户的连接存储空间，且系统能够访问标题存储服务时，会通过云存储协调所有修改的数据。 这种情况下从未显示过 UI。

有关连接存储上下文之外的脱机处理的更多信息，请参阅 *Xbox One 游戏的服务中断复原*。

### <a name="lock-acquisition"></a>锁定获取

验证连接后，系统会尝试获取对与你的应用和当前用户关联的云存储空间的独占访问权。 通过在标题存储的连接存储区域中放置锁定文件可完成此操作。 如果控制台处于可访问服务的联机状态，并能够在很短的时间内获取锁定，则不会显示 UI，且同步过程会继续。

当系统已获取用于特定连接存储空间的锁定，并将连接存储空间的实例返回应用后，在此连接存储空间内应用对数据的 API 调用操作不会在成功的 Web 请求上受阻。 锁定会提供足够的保护，因此，即使用户要在应用获取连接存储空间后从系统中拔出网络电缆，API 调用也会基于本地可用的数据进行。

锁定获取步骤期间有几个可能的错误情况：

 **同步 UI** 如果控制台处于联机状态，但未能在短时间内从服务中获取锁定，则会显示“同步”UI。

 **中断锁定** 如果用户自上次在当前控制台上运行应用起，一直在另一个控制台上运行应用，则另一个控制台可能会获得对存储空间的独占访问权限，且正在上传数据。 或者，可能另一个控制台已经开始上传数据，但是在完成之前断开了连接或发生了断电。

这两种情况都称为*锁定争用*，并且，在任一情况下，系统都会显示 UI，以说明另一个控制台正在上传数据。 用户可以等待此过程完成，或使用云中当前可用的数据。 如果用户选择使用云数据，系统会自身采用锁定（中断锁定），为用户和应用获取云存储的独占访问权。 其他控制台上的上传会取消，而同步过程会继续。

### <a name="container-listing-comparison-and-merger-logic"></a>容器列出、比较和合并逻辑

获取锁定后，系统会为提供的应用和用户请求云中所有容器的列表。 然后，它会将本地硬盘的内容与云中的内容进行比较，并根据比较的结果继续：

 **本地数据匹配云** 如果其他控制台上没有发生更改，且云中和本地硬盘中的数据相同，则同步完成，此时会调用 **GetForUserAsync** 调用的完成处理程序，且应用会继续加载和保存。

 **没有本地数据** 如果云有数据，但本地控制台没有，则会本地下载云中的数据。 例如，当用户第一次在朋友家玩游戏时可能会发生这种情况。

 **相同容器，在本地和云中修改** 如果用户已经通过在其他控制台上运行在云中修改了容器，并在脱机使用当前控制台时修改了同一个容器，则无法自动合并数据。 系统将要求用户选择要保留的数据。 如果发生冲突，用户可以选择替换策略：始终保留本地数据或云数据，或者，用户可以选择**取消**并延迟选择。 如果用户选择将云或本地数据作为替换策略，则会相应地解决名称相同但内容不同的容器。

如果用户选择**取消**，游戏将拥有对处于未解决状态的保存系统的访问权限，就像用户在脱机运行。 在这种情况下，如果控制台处于联机状态，当应用下次请求访问连接存储空间时，冲突解决 UI 会再次出现。

### <a name="container-download"></a>容器下载

解决所有冲突后，系统会拥有识别需要从云中下载的容器必需的所有信息。 将下载所有需要的容器，此时将调用 *ConnectedStorageSpace.GetForUserAsync 方法*的完成处理程序，且应用可以继续下载和保存。

执行此步骤期间的部分可能的错误：

**本地存储不足**  
如果对于所需容器来说本地硬盘空间不足，则系统会向用户显示 UI，要求他们通过删除本地保存的数据释放磁盘空间。 为帮助他们避免永久删除未在云中备份的重要数据，UI 会清楚地指示仅仅是本地缓存的数据和特定于当前控制台的唯一数据。

向用户显示 UI 时：

-   如果用户释放了足够的空间，同步会继续并完成。
-   如果用户取消 UI，且没有释放足够的空间，则 **GetForUserAsync** 调用的完成处理程序会返回 **OutOfLocalStorage**，请参阅 *ConnectedStorageErrorStatus 枚举*。 应用应确认用户是否想要在无法保存数据的情况下运行。 如果用户同意，应用应继续，但不保存用户数据。 如果用户指示其想要在运行时保存数据，则应用应重复 **GetForUserAsync** 调用，此调用会显示 UI 以释放空间。

**用户取消同步**  
如果用户不想等待同步完成并选择了取消，则系统会通知用户，并非所有保存的数据都可用。 此时将调用 **GetForUserAsync** 调用的完成处理程序，且应用可以继续下载和保存。

**网络超时**  
如果由于网络连接和服务可用性的问题发生数据下载超时，则系统会向用户提供重试同步的选项。 如果用户选择不重试，则系统会通知用户，并非所有保存的数据都可用。 此时将调用 **GetForUserAsync** 调用的完成处理程序，且应用可以继续下载和保存。

## <a name="development-tools"></a>开发工具

有两种工具能帮助你开发应用对连接存储的使用：XbStorage 和 Fiddler。

### <a name="managing-connected-storage-with-xbstorage"></a>使用 XbStorage 管理连接存储

XbStorage 是一种开发工具，可在开发电脑的 Xbox One 开发工具包上管理本地连接存储数据。

此工具允许从硬盘上清除本地连接存储空间，以及通过使用 XML 文件将个人用户或计算机连接存储空间导入和导出。

在本地连接存储空间执行操作时，系统的行为就像由应用本身执行操作一样。 将数据从连接存储空间复制到本地文件会导致在复制之前与云进行同步。

同样，将开发电脑上 XML 文件中的数据复制到 Xbox One 开发工具包上的连接存储容器会导致控制台开始将此数据上传到云。 有一个例外：如果开发工具包无法获取锁定，或者，如果控制台中的容器和云中的容器之间存在冲突。 在这种情况下，控制台的行为就像用户已经决定不解决冲突一样（例如，通过选取一个要保留的容器版本），且控制台的行为就像它在脱机运行，直到下次启动游戏一样。

XbStorage 的重置命令会清除所有 SCID 和用户的已保存数据的本地存储，但不会改变在云中存储的数据。 如果用户漫游到控制台，并在运行一个游戏时从云中下载数据，那么，这对将控制台设置为它应该所处的状态非常有用。

有关 XbStorage 的详细信息，请参阅 XDK 文档中的*管理连接存储 (xbstorage.exe)*。

### <a name="monitoring-connected-storage-network-activity-using-fiddler"></a>使用 Fiddler 监视连接存储网络活动

它可能有助于确定，当执行云存储操作时，控制台是否与服务交互。 使用 Fiddler 有助于确定你的控制台是否成功调用服务，或者它是否遇到授权错误。 有关在 Xbox One 上设置 Fiddler 的信息，请参阅 XDK 文档中的*如何将 Fiddler 用于 Xbox One*。

## <a name="resources"></a>资源

除上文建议的资源外，以下内容可能有助于开发应用或游戏：

-   XDK 文档中的连接存储概述
-   [进程周期管理](https://developer.xboxlive.com/_layouts/xna/download.ashx?file=ProcessLifetimeManagement_08_2013_qfe5.zip&folder=platform/aug2013xdk_qfe5/samples)，游戏开发人员网络 (GDN) 示例中提供的示例
-   [“Xbox One 的进程周期管理 (PLM)”](https://developer.xboxlive.com/en-us/platform/development/education/Documents/PLM%20for%20Xbox%20One.aspx)，GDN 白皮书中的可用白皮书
