---
author: msatranjr
Description: This topic describes performance guidelines for apps that require access to a user's location.
title: 位置感知应用指南
ms.assetid: 16294DD6-5D12-4062-850A-DB5837696B4D
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 位置, 地图, 地理位置
ms.localizationpriority: medium
ms.openlocfilehash: 903a7b308c78e4ab9826ea4c46c642cb3361b462
ms.sourcegitcommit: c4d3115348c8b54fcc92aae8e18fdabc3deb301d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/22/2018
ms.locfileid: "5402708"
---
# <a name="guidelines-for-location-aware-apps"></a>位置感知应用指南




**重要的 API**

-   [**地理位置**](https://msdn.microsoft.com/library/windows/apps/br225603)
-   [**Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534)

本主题描述需要访问用户位置信息的应用的性能指南。

## <a name="recommendations"></a>建议


-   仅在应用需要位置数据时开始使用位置对象。

    在访问用户的位置之前调用 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn859152)。 此时，你的应用必须在前台，并且 **RequestAccessAsync** 必须从 UI 线程中进行调用。 除非用户向你的应用授予访问其位置的权限，否则你的应用将无法访问位置数据。

-   如果位置信息对于你的应用来说并非必要，则在用户尝试完成需要该信息的任务之前不要访问它。 例如，如果社交网络应用有一个“使用位置信息登记”按钮，在用户单击该按钮之前，该应用不应该访问位置信息。 如果你的应用主要功能要求立即访问位置信息，你可以这样做。

-   [**Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534) 对象必须首先在前台应用的主 UI 线程上使用，以便向用户触发同意提示。 首次使用 **Geolocator** 可能会首次调用 [**getGeopositionAsync**](https://msdn.microsoft.com/library/windows/apps/hh973536)，或首次向处理程序注册 [**positionChanged**](https://msdn.microsoft.com/library/windows/apps/br225540) 事件。

-   告诉用户使用位置数据的方式。
-   提供 UI，使用户可以手动刷新他们的位置信息。
-   在等待获取位置数据时显示进度栏或进度环。 <!--For info on the available progress controls and how to use them, see [**Guidelines for progress controls**](guidelines-and-checklist-for-progress-controls.md).-->
-   定位服务被禁用或不可用时显示一条相应的错误消息或对话框。

    如果位置设置不允许你的应用访问用户位置，我们建议提供一个指向 **“设置”** 应用中的 **“位置隐私设置”** 的便捷链接。 例如，你可以使用超链接控件或调用 [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476) 方法，以使用 `ms-settings:privacy-location` URI 通过代码启动 **“设置”** 应用。 有关详细信息，请参阅[启动 Windows 设置应用](https://msdn.microsoft.com/library/windows/apps/mt228342)。

-   当用户禁用对位置数据的访问时，清除缓存的位置信息并释放 [**Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534)。

    如果用户通过“设置”关闭对位置信息的访问，则释放 [**Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534) 对象。 对于任何位置 API 调用，该应用都将收到 **ACCESS\_DENIED** 结果。 如果你的应用保存或缓存了位置数据，则在用户吊销对位置信息的访问时需清除缓存的所有数据。 提供另一种在位置数据无法通过定位服务提供时手动输入位置信息的方法。

-   提供用于重新启用定位服务的 UI。 例如，提供重新实例化[**Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534)对象，并尝试再次获取位置信息的刷新按钮。

    使应用提供用于重新启用定位服务的 UI：

    -   如果用户在禁用位置访问之后重新启用位置访问，则不会通知应用。 [**status**](https://msdn.microsoft.com/library/windows/apps/br225601) 属性不会更改，并且没有 [**statusChanged**](https://msdn.microsoft.com/library/windows/apps/br225542) 事件。 你的应用应该创建新的 [**Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534) 对象并调用 [**getGeopositionAsync**](https://msdn.microsoft.com/library/windows/apps/hh973536)，以尝试获取更新后的位置数据或重新订阅 [**positionChanged**](https://msdn.microsoft.com/library/windows/apps/br225540) 事件。 如果之后状态指示已重新启用位置，请清除之前应用显示的所有用于通知用户定位服务已被禁用的 UI，并对新状态执行相应的响应。
    -   当用户明确地尝试使用需要位置信息的功能时，或者在其他任何适当情况下，你的应用也应该在激活后重新尝试获取位置数据。

**性能**

-   如果你的应用不需要接收位置信息，使用一次性位置信息请求。 例如，向照片添加位置标记的应用不需要接收位置更新事件。 相反，它应该使用 [**getGeopositionAsync**](https://msdn.microsoft.com/library/windows/apps/hh973536) 请求位置，如[获取当前位置](https://msdn.microsoft.com/library/windows/apps/mt219698)中所述。

    当你进行一次性位置请求时，你应该设置以下值。

    -   通过设置 [**DesiredAccuracy**](https://msdn.microsoft.com/library/windows/apps/br225535) 或 [**DesiredAccuracyInMeters**](https://msdn.microsoft.com/library/windows/apps/jj635271) 指定由应用请求的精度。 有关使用这些参数的建议，请参阅下方。
    -   设置 [**GetGeopositionAsync**](https://msdn.microsoft.com/library/windows/apps/hh973536) 的最大存留期参数以指定多长时间之前获取的位置对应用有用。 如果你的应用可以使用几秒或几分钟前的位置，则你的应应用几乎可以立即收到位置并且有助于节省设备电源。
    -   设置 [**GetGeopositionAsync**](https://msdn.microsoft.com/library/windows/apps/hh973536) 的超时参数。 这是你的应用可以等待返回位置或错误的时长。 你将需要在对用户的响应和应用所需的精度之间做出权衡。
-   当需要频繁的位置更新时，使用连续的位置会话。 将 [**positionChanged**](https://msdn.microsoft.com/library/windows/apps/br225540) 事件和 [**statusChanged**](https://msdn.microsoft.com/library/windows/apps/br225542) 事件用于实时检测超过特定阈值的移动或连续的位置更新。

    当请求位置更新时，你可能要通过设置 [**DesiredAccuracy**](https://msdn.microsoft.com/library/windows/apps/br225535) 或 [**DesiredAccuracyInMeters**](https://msdn.microsoft.com/library/windows/apps/jj635271) 指定由应用请求的精度。 你还应该设置需要位置更新的频率，方法是使用 [**MovementThreshold**](https://msdn.microsoft.com/library/windows/apps/br225539) 或 [**ReportInterval**](https://msdn.microsoft.com/library/windows/apps/br225541)。

    -   指定移动阈值。 某些应用仅在用户移动较大的距离时才需要位置更新。 例如，提供当地新闻或天气更新的应用，除非用户的位置更改为其他城市，否则可能不需要位置更新。 这种情况下，你可以通过设置 [**MovementThreshold**](https://msdn.microsoft.com/library/windows/apps/br225539) 属性来调整位置更新事件所要求的最小移动量。 此操作具有筛选 [**PositionChanged**](https://msdn.microsoft.com/library/windows/apps/br225540) 事件的效果。 仅当位置更改超过移动阈值时才引发这些事件。

    -   使用与你的应用体验协调并最大程度减少系统资源使用量的 [**reportInterval**](https://msdn.microsoft.com/library/windows/apps/br225541)。 例如，天气应用可能只需要每隔 15 分钟进行一次数据更新。 除了实时导航应用之外，大多数应用都不需要精度很高的持续位置更新流。 如果你的应用不需要最精确的数据流或者不需要经常更新，则设置 **ReportInterval** 属性以指示应用所需的位置更新最低频率。 这样，位置源就可以仅在需要时计算位置，从而可以节省电耗。

        需要实时数据的应用应将 [**ReportInterval**](https://msdn.microsoft.com/library/windows/apps/br225541) 设置为 0，以指示最低间隔尚未指定。 默认报告间隔为 1 秒或硬件可以支持的最快频率 – 取两者中的较短值。

        提供位置数据的设备可跟踪不同应用所请求的报告间隔，并以请求的最小间隔提供数据报告。 这样，对准确性要求最高的应用就会接收到其所需的数据。 因此，如果其他应用请求的更新频率更高，则定位程序可能以高于你的应用所请求的频率生成更新。

        **注意**  不保证位置源会针对给定的报告间隔兑现请求。 并非所有定位程序设备都跟踪报告间隔，但你仍然应该为那些进行跟踪的设备提供报告间隔。

    -   为了帮助节省电耗，请设置 [**desiredAccuracy**](https://msdn.microsoft.com/library/windows/apps/br225535) 属性，以向位置平台指示你的应用是否需要高精度的数据。 如果应用都不需要高精度的数据，则系统可以不打开 GPS 提供程序以节省电耗。

        -   将 [**desiredAccuracy**](https://msdn.microsoft.com/library/windows/apps/br225535) 设置为 **HIGH** 以启用 GPS 来获取数据。
        -   如果你的应用仅将位置信息用于广告定位，请将 [**desiredAccuracy**](https://msdn.microsoft.com/library/windows/apps/br225535) 设置为 **Default** 并仅使用单发调用模式以尽可能减少电耗。

        如果应用对精度有特定要求，你可能要使用 [**DesiredAccuracyInMeters**](https://msdn.microsoft.com/library/windows/apps/jj635271) 属性，而不是 [**DesiredAccuracy**](https://msdn.microsoft.com/library/windows/apps/br225535)。 这在 Windows Phone 上特别有用，在其中通常基于移动电话信标、Wi-Fi 信标和人造卫星获取位置。 采用较具体的精度值将有助于系统识别要使用的正确技术，以在提供位置时消耗最低的电源。

        例如：

        -   如果应用要获取位置用于广告调整、天气、新闻，5000 米的精度一般足够。
        -   如果你的应用显示附近的街区中的交易，300 米的精度，最好通常提供结果。
        -   如果用户查找附件餐厅的推荐，我们可能要获取一个街区内的位置，因此 100 米的精度足够了。
        -   如果用户试图共享他的位置，应用应该请求大约 10 米的精度。
    -   如果应用有特定的精度要求，请使用 [**Geocoordinate.accuracy**](https://msdn.microsoft.com/library/windows/apps/br225526) 属性。 例如，导航应用应该使用 **Geocoordinate.accuracy** 属性来确定可用的位置数据是否符合应用的要求。

-   考虑启动延迟。 应用首次请求位置数据时，定位程序启动时可能会有短暂的延迟（1-2 秒）。 在设计应用的 UI 时，请注意以下事项： 例如，你需要避免阻止其他使 [**GetGeopositionAsync**](https://msdn.microsoft.com/library/windows/apps/hh973536) 调用挂起的任务。

-   考虑后台行为。 如果应用没有焦点，则该应用在后台暂停时将不会收到位置更新事件。 如果应用通过记录来跟踪位置更新，则需注意这一点。 应用再次获得焦点时，将只接收到新的事件。 处于非活动状态时将不会获取发生的任何更新。

-   有效使用原始和融合传感器。 存在两种传感器：*原始传感器*和*融合传感器*。

    -   原始传感器包括加速计、陀螺仪和磁力计。
    -   融合传感器包括方向、测斜仪和指南针。 融合传感器从原始传感器的组合中获取数据。

    Windows 运行时 API 可以访问除磁力计之外的所有传感器。 融合传感器比原始传感器更准确和稳定，但能耗也较多。 应针对相应目的使用正确的传感器。 有关详细信息，请参阅[传感器](https://msdn.microsoft.com/library/windows/apps/mt187358)。

**连接待机**
- 当电脑处于连接待机状态时，始终可以实例化 [**Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534) 对象。 但是，**Geolocator** 对象将找不到任何可聚合的传感器，因此对 [**GetGeopositionAsync**](https://msdn.microsoft.com/library/windows/apps/hh973536) 的调用将在 7 秒后超时，[**PositionChanged**](https://msdn.microsoft.com/library/windows/apps/br225540) 事件侦听器将永远不会被调用，而 [**StatusChanged**](https://msdn.microsoft.com/library/windows/apps/br225542) 事件侦听器将在 **NoData** 状态下被调用一次。

## <a name="additional-usage-guidance"></a>其他使用指南


### <a name="detecting-changes-in-location-settings"></a>检测定位设置中的更改

用户可以使用“设置”**** 应用中的“位置隐私设置”**** 来关闭位置功能。

-   若要检测用户何时禁用或重新启用定位服务：
    -   处理 [**StatusChanged**](https://msdn.microsoft.com/library/windows/apps/br225542) 事件。 如果用户关闭定位服务，则 **StatusChanged** 事件参数的 [**Status**](https://msdn.microsoft.com/library/windows/apps/br225601) 属性将拥有值 **Disabled**。
    -   检查从 [**GetGeopositionAsync**](https://msdn.microsoft.com/library/windows/apps/hh973536) 返回的错误代码。 如果用户禁用了定位服务，则调用 **GetGeopositionAsync** 将失败，且返回 **ACCESS\_DENIED** 错误，并且 [**LocationStatus**](https://msdn.microsoft.com/library/windows/apps/br225538) 属性的值为 **Disabled**。
-   如果位置数据对于你的应用至关重要（例如，地图应用），请务必执行以下操作：
    -   如果用户位置发生更改，则处理 [**PositionChanged**](https://msdn.microsoft.com/library/windows/apps/br225540) 事件以获取更新。
    -   按照上述步骤处理 [**StatusChanged**](https://msdn.microsoft.com/library/windows/apps/br225542) 事件，以检测定位设置中的更改。

请注意，在定位服务可用时，它将返回数据。 它可能先返回一个带有较大误差半径的位置，然后在更准确的信息可用时使用它更新该位置。 显示用户位置的应用通常要在更准确的信息可用时更新该位置。

### <a name="graphical-representations-of-location"></a>位置信息的图形表示形式

让你的应用使用 [**Geocoordinate.accuracy**](https://msdn.microsoft.com/library/windows/apps/br225526) 在地区上清晰地指示用户的当前位置。 精度有三个主要区段：大约 10 米的误差半径，大约 100 米的误差半径，以及大于 1 千米的误差半径。 通过使用精度信息，你可以确保自己的应用在可用数据的上下文中精确显示位置。 有关使用地图控件的常规信息，请参阅[以 2D、3D 和街景视图方式显示地图](https://msdn.microsoft.com/library/windows/apps/mt219695)。

-   对于约等于 10 米（GPS 分辨率）的精度，在地图上可以用一个点或图钉来表示位置。 在此精度下，还可以显示经纬度坐标和街道地址。

    ![以大约 10 米的 GPS 精度显示的地图示例。](images/10metererrorradius.png)

-   对于 10 到 500 米之间（大约 100 米）的精度，通常使用 WLAN 解决方案来接收位置。 从移动电话获取的位置有大约 300 米的精度。 在这种情况下，我们建议你的应用显示一个误差半径。 对于需要用一个中心点来显示方向的应用，可以在该中心点的周围显示一个误差半径。

    ![以大约 100 米的 WLAN 精度显示的地图示例。](images/100metererrorradius.png)

-   如果返回的精度大于 1 千米，则你可能是在以 IP 级别的分辨率接收位置信息。 此精度级别通常过低，无法在地图上精确定位某个特定的地点。 应用应放大到地图上的城市级别，或基于误差半径的相应区域（如区域级别）。

    ![以大约 1 千米的 WLAN 精度显示的地图示例。](images/1000metererrorradius.png)

当位置精度从一个精度区段切换到另一个时，会在不同的图形表示形式之间提供优美的过渡。 可通过以下方法完成此操作：

-   让过渡动画更平滑，使过渡保持快速流畅。
-   等待一些连续的报告确认精度更改，以防止不必要的和过于频繁的缩放。

### <a name="textual-representations-of-location"></a>位置信息的文本表示形式

某些类型的应用（例如，天气应用或本地信息应用），需要在不同的精度区段以文本方式表示位置的方法。 请确保清楚地显示位置，并且只详细到该数据中提供的精度级别。

-   对于约等于 10 米（GPS 分辨率）的精度，接收到的位置数据相当精确，因此可以细化到街区名的级别。 还可以使用城市名、州名或省名以及国家/地区名。
-   对于约等于 100 米（Wi-Fi 分辨率）的精度，接收到的位置数据的准确度适中，因此我们建议你显示的信息可以详细到城市名。 避免使用街区名。
-   对于大于 1 千米（IP 分辨率）的精度，仅显示州名、省名或国家/地区名。

### <a name="privacy-considerations"></a>隐私注意事项

用户的地理位置属于个人身份信息 (PII)。 下列网站提供保护用户隐私的指南。

-   [Microsoft 隐私]( http://go.microsoft.com/fwlink/p/?LinkId=259692)

<!--For more info, see [Guidelines for privacy-aware apps](guidelines-for-enabling-sensitive-devices.md).-->

## <a name="related-topics"></a>相关主题

* [设置地理围栏](https://msdn.microsoft.com/library/windows/apps/mt219702)
* [获取当前位置](https://msdn.microsoft.com/library/windows/apps/mt219698)
* [使用 2D、3D 和街景视图方式显示地图](https://msdn.microsoft.com/library/windows/apps/mt219695)
<!--* [Design guidelines for privacy-aware apps](guidelines-for-enabling-sensitive-devices.md)-->
* [UWP 位置示例（地理位置）](http://go.microsoft.com/fwlink/p/?linkid=533278)
 

 
