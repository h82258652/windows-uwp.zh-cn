---
author: TylerMSFT
title: 启动 Windows 地图应用
description: 了解如何从你的应用启动 Windows 地图应用。
ms.assetid: E363490A-C886-4D92-9A64-52E3C24F1D98
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6fd7377294e0d460720f6a16e71981ab0924ac9a
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2018
ms.locfileid: "5709520"
---
# <a name="launch-the-windows-maps-app"></a>启动 Windows 地图应用




了解如何从你的应用启动 Windows 地图应用。 本主题介绍了 **bingmaps:、*ms-drive-to:、ms-walk-to:** 和 **ms-settings:** 统一资源标识符 (URI) 方案。 使用这些 URI 方案来将 Windows 地图应用启动为特定的地图、路线和搜索结果或从“设置”应用下载 Windows 地图离线地图。

**提示** 若要了解有关从你的应用启动 Windows 地图应用的详细信息，请从 GitHub 上的 [Windows 通用示例存储库](http://go.microsoft.com/fwlink/p/?LinkId=619979)中下载[通用 Windows 平台 (UWP) 地图示例](http://go.microsoft.com/fwlink/p/?LinkId=619977)。

## <a name="introducing-uris"></a>URI 简介

URI 方案允许你通过单击超链接（或在你的应用中以编程方式）打开应用。 正如可以使用 **mailto:** 打开新的电子邮件或使用 **http:** 打开 Web 浏览器，你还可以使用 **bingmaps:**、**ms-drive-to:** 和 **ms-walk-to:** 打开 Windows 地图应用。

-   **bingmaps:** URI 提供附带位置、搜索结果、路线和路况的地图。
-   **ms-drive-to:** URI 提供从你的当前位置开始的行车路线规划。
-   **ms-walk-to:** URI 提供从你的当前位置开始的步行路线规划。

例如，以下 URI 将打开 Windows 地图应用，并显示以纽约市为中心的地图。

```xml
<bingmaps:?cp=40.726966~-74.006076>
```

![以纽约市为中心的地图。](images/mapnyc.png)

下面是 URI 方案的说明：

**bingmaps:?query**

在此 URI 方案中，*query* 是一系列参数名称/值对：

**&amp;param1=value1&amp;param2=value2 …**

有关可用参数的完整列表，请参阅 [bingmaps:](#bingmaps-param-reference)、[ms-drive-to:](#ms-drive-to-param-reference) 和 [ms-walk-to:](#ms-walk-to-param-reference) 参数引用。 本主题的后面还会提供示例。

## <a name="launch-a-uri-from-your-app"></a>从你的应用启动 URI


若要从你的应用启动 Windows 地图应用，请使用 **bingmaps:**、**ms-drive-to:** 或 **ms-walk-to:** URI 调用 [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476) 方法。 以下示例启动前一个示例中相同的 URI。 有关通过 URI 启动应用的详细信息，请参阅[启动 URI 的默认应用](launch-default-app.md)。

```cs
// Center on New York City
var uriNewYork = new Uri(@"bingmaps:?cp=40.726966~-74.006076");

// Launch the Windows Maps app
var launcherOptions = new Windows.System.LauncherOptions();
launcherOptions.TargetApplicationPackageFamilyName = "Microsoft.WindowsMaps_8wekyb3d8bbwe";
var success = await Windows.System.Launcher.LaunchUriAsync(uriNewYork, launcherOptions);
```

在此示例中，[**LauncherOptions**](https://msdn.microsoft.com/library/windows/apps/hh701435) 类用于帮助确保启动 Windows 地图应用。

## <a name="display-known-locations"></a>显示已知的位置

可以使用许多选项来控制要显示的地图部分。 你可以将 *cp*（中心点）参数与 *rad*（半径）或 *lvl*（缩放级别）参数配合使用，以显示位置，并选择接近度以对其进行放大。 当你使用 *cp* 参数时，你还可以指定 *hdg*（标题）和 *pit*（倾斜）以控制要查看的方向。 另一种方法是使用 *bb*（边界框）参数来提供你想要显示的区域的最大东、南、西、北坐标。

若要控制视图类型，请使用 *sty*（样式）和 *ss*（Streetside）参数。 *sty* 参数允许你在路线图和鸟瞰图之间切换。 *ss* 参数将地图置于 Streetside 视图。 有关这些参数和其他参数的详细信息，请参阅 [bingmaps: 参数引用](#bingmaps-param-reference)。


| 示例 URI                                                                 | 结果                                                                                                                                                                                        |
|----------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| bingmaps:?                                                                 | 打开地图应用。                                                                                                                                                                            |
| bingmaps:?cp=40.726966~-74.006076                                          | 显示以纽约市为中心的地图。                                                                                                                                                    |
| bingmaps:?cp=40.726966~-74.006076&amp;lvl=10                                   | 使用缩放级别 10 显示以纽约市为中心的地图。                                                                                                                            |
| bingmaps:?bb=39.719\_-74.52~41.71\_-73.5                                   | 显示纽约市（**bb** 参数中指定的区域）的地图。                                                                                                           |
| bingmaps:?bb=39.719\_-74.52~41.71\_-73.5&cp=47~-122                        | 显示纽约市（使用边界框参数指定的区域）的地图。 将忽略使用 **cp** 参数为西雅图指定的中心点，因为指定了 *bb*。 |
| bingmaps:?collection=point.36.116584\_-115.176753\_Caesars%20Palace&lvl=16 | 显示名为恺撒王宫酒店的某个点（位于拉斯维加斯）的地图并将缩放级别设置为 16。                                                                                                 |
| bingmaps:?collection=point.40.726966\_-74.006076\_Some%255FBusiness        | 显示带有名为 Some\_Business（位于拉斯维加斯）的点的地图。                                                                                                                               |
| bingmaps:?cp=40.726966~-74.006076&trfc=1&sty=a                             | 显示纽约市的地图并已启用路况和鸟瞰图样式。                                                                                                                          |
| bingmaps:?cp=47.6204~-122.3491&sty=3d                                      | 显示 Space Needle 的 3D 视图。                                                                                                                                                        |
| bingmaps:?cp=47.6204~-122.3491&amp;sty=3d&amp;rad=200&amp;pit=75&amp;hdg=165               | 显示 Space Needle 的 3D 视图（半径 200 米、俯仰 75 度和方位 165 度）。                                                                             |
| bingmaps:?cp=47.6204~-122.3491&ss=1                                        | 显示 Space Needle 的街景视图。                                                                                                                                                |


## <a name="display-search-results"></a>显示搜索结果

使用 *q* 参数搜索位置时，我们建议尽可能使用特定搜索词，并使用 *cp*、*bb* 或 *where* 参数指定搜索位置。 如果你未指定搜索位置，并且用户的当前位置不可用，则搜索可能不会返回有意义的结果。 搜索结果将显示在最合适的地图视图中。 有关这些参数和其他参数的详细信息，请参阅 [bingmaps: 参数引用](#bingmaps-param-reference)。


| 示例 URI                                                    | 结果                                                                            |
|---------------------------------------------------------------|------------------------------------------------------------------------------------|
| bingmaps:?q=1600%20Pennsylvania%20Ave,%20Washington,%20DC     | 显示地图并搜索位于华盛顿哥伦比亚特区的白宫的地址。 |
| bingmaps:?q=coffee&where=Seattle                              | 搜索西雅图的咖啡馆。                                                    |
| bingmaps:?cp=40.726966~-74.006076&where=New%20York            | 在指定的中心点附近搜索纽约。                             |
| bingmaps:?bb=39.719\_-74.52~41.71\_-73.5&q=pizza              | 在指定的边框（即在纽约市）搜索比萨。      |

 
## <a name="display-multiple-points"></a>显示多个点


使用 *collection* 参数在地图上显示一组自定义的点。 如果存在多个点，将显示点的列表。 一个集合中最多可以包含 25 个点，它们按所提供的顺序列出。 集合优先于搜索和路线请求。 有关此参数和其他参数的详细信息，请参阅 [bingmaps: 参数引用](#bingmaps-param-reference)。

| 示例 URI | 结果                                                                                                                   |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------|
| bingmaps:?collection=point.36.116584\_-115.176753\_Caesars%20Palace                                                                                                | 搜索拉斯维加斯的恺撒王宫酒店，并在地图上以最佳的地图视图显示结果。                         |
| bingmaps:?collection=point.36.116584\_-115.176753\_Caesars%20Palace&amp;lvl=16                                                                                         | 显示名为恺撒王宫酒店（位于拉斯维加斯）的图钉，缩放级别为 16。                                               |
| bingmaps:?collection=point.36.116584\_-115.176753\_Caesars%20Palace~point.36.113126\_-115.175188\_The%20Bellagio&amp;lvl=16&amp;cp=36.114902~-115.176669                   | 显示名为恺撒王宫酒店和百乐宫酒店（均位于拉斯维加斯）的两枚图钉，缩放级别为 16。              |
| bingmaps:?collection=point.40.726966\_-74.006076\_Fake%255FBusiness%255Fwith%255FUnderscore                                                                        | 显示带有名为 Fake\_Business\_with\_Underscore 的图钉的纽约市。                                                  |
| bingmaps:?collection=name.Hotel%20List~point.36.116584\_-115.176753\_Caesars%20Palace~point.36.113126\_-115.175188\_The%20Bellagio&amp;lvl=16&amp;cp=36.114902~-115.176669 | 显示名为酒店列表的列表，以及拉斯维加斯的恺撒王宫酒店和百乐宫酒店的两枚图钉，缩放级别为 16。 |

 

## <a name="display-directions-and-traffic"></a>显示路线和路况


你可以使用 *rtp* 参数显示两个点之间的路线；这些点可以是地址或纬度和经度坐标。 使用 *trfc* 参数显示路况信息。 若要指定路线的类型：驾车、步行或公交，请使用 *mode* 参数。 如果未指定 *mode*，将使用用户的首选交通模式提供路线。 有关这些参数和其他参数的详细信息，请参阅 [bingmaps: 参数引用](#bingmaps-param-reference)。

![路线的示例](images/windowsmapgcdirections.png)

| 示例 URI                                                                                                              | 结果                                                                                                                                                         |
|-------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| bingmaps:?rtp=pos.44.9160\_-110.4158~pos.45.0475\_-109.4187                                                             | 显示带有点对点路线的地图。 因为未指定 *mode*，所以将使用交通首选项的用户的模式提供路线。 |
| bingmaps:?cp=43.0332~-87.9167&amp;trfc=1                                                                                    | 显示以威斯康辛州的密尔沃基市为中心的地图以及路况。                                                                                                        |
| bingmaps:?rtp=adr.One Microsoft Way, Redmond, WA 98052~pos.39.0731\_-108.7238                                           | 显示带有从指定的地址到指定位置的路线的地图。                                                                            |
| bingmaps:?rtp=adr.1%20Microsoft%20Way,%20Redmond,%20WA,%2098052~pos.36.1223\_-111.9495\_Grand%20Canyon%20northern%20rim | 显示从 1 Microsoft Way, Redmond, WA, 98052 到 Grand Canyon's northern rim 的路线。                                                                |
| bingmaps:?rtp=adr.Davenport, CA~adr.Yosemite Village                                                                    | 显示地图以及从指定的位置到指定的地标的驾车路线。                                                                   |
| bingmaps:?rtp=adr.Mountain%20View,%20CA~adr.San%20Francisco%20International%20Airport,%20CA&amp;mode=d                      | 显示从芒廷维尤到旧金山国际机场（这两者均位于加利福尼亚州）的驾车路线。                                                                  |
| bingmaps:?rtp=adr.Mountain%20View,%20CA~adr.San%20Francisco%20International%20Airport,%20CA&amp;mode=w                      | 显示从芒廷维尤到旧金山国际机场（这两者均位于加利福尼亚州）的步行路线。                                                                  |
| bingmaps:?rtp=adr.Mountain%20View,%20CA~adr.San%20Francisco%20International%20Airport,%20CA&amp;mode=t                      | 显示从芒廷维尤到旧金山国际机场（这两者均位于加利福尼亚州）的公交路线。                                                                  |

## <a name="display-turn-by-turn-directions"></a>显示路线规划


**ms-drive-to:** 和 **ms-walk-to:** URI 方案允许你直接启动到路线的规划视图。 这些 URI 方案只能提供自用户的当前位置开始的路线。 如果必须提供未包含用户的当前位置的点之间的路线，请使用先前部分中所述的 **bingmaps:** URI 方案。 有关这些 URI 方案的详细信息，请参阅 [ms-drive-to:](#ms-drive-to-param-reference) 和 [ms-walk-to:](#ms-walk-to-param-reference) 参数引用。

> **重要提示** 当启动 **ms-drive-to:** 或 **ms-walk-to:** URI 方案时，“地图”应用将检查设备是否曾有过 GPS 位置定位。 如果有，则“地图”应用将继续进行路线规划。 如果没有，应用将显示路线概述，如[显示路线和路况](#display-directions-and-traffic)中所述。

![路线规划的示例](images/windowsmapsappdirections.png)

| 示例 URI                                                                                                | 结果                                                                                       |
|-----------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------|
| ms-drive-to:?destination.latitude=47.680504&amp;destination.longitude=-122.328262&amp;destination.name=Green Lake | 显示地图以及从你的当前位置到 Green Lake 的驾车路线规划。 |
| ms-walk-to:?destination.latitude=47.680504&amp;destination.longitude=-122.328262&amp;destination.name=Green Lake  | 显示地图以及从你的当前位置到 Green Lake 的行走路线规划。 |


## <a name="download-offline-maps"></a>下载离线地图

**ms-settings:** URI 方案可使你直接启动到“设置”应用中的特定页面。 尽管 **ms-settings:** URI 方案不会启动到“地图”应用中，但它允许你直接启动到“设置”应用中的“离线地图”页面，并显示下载“地图”应用所使用的离线地图的确认对话框。 URI 方案接受由纬度和经度指定的点，并自动确定是否存在可用于包含该点的离线地图。  如果经过的纬度和经度恰好落在多个下载区域内，确认对话框将让用户选取要下载其中哪一个区域。 如果离线地图不适用于包含该点的区域，则“设置”应用中的“离线地图”页面将显示一个错误对话框。

| 示例 URI  | 结果 |
|-------------|---------|
| ms-settings:maps-downloadmaps?latlong=47.6,-122.3 | 将“设置”应用打开到显示确认对话框的“离线地图”页面，以便下载包含指定经纬度点的区域的地图。 |

<span id="bingmaps-param-reference"/>

## <a name="bingmaps-parameter-reference"></a>bingmaps: 参数引用

此表中每个参数的语法都是通过使用扩展的巴科斯范式 (ABNF) 显示的。

<table>
<colgroup>
<col width="25%" />
<col width="25%" />
<col width="25%" />
<col width="25%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">参数</th>
<th align="left">定义</th>
<th align="left">ABNF 定义和示例</th>
<th align="left">详细信息</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><b>cp</b></p></td>
<td align="left"><p>中心点</p></td>
<td align="left"><p>cp = "cp=" cpval</p>
<p>cpval = degreeslat "~" degreeslon</p>
<p>degreeslat = \["-"\] 1*3DIGIT \["." 1*7DIGIT\]</p>
<p>degreeslon = \["-"\] 1*2DIGIT \["." 1*7DIGIT]</p>
<p>示例：</p>
<p>cp=40.726966~-74.006076</p></td>
<td align="left"><p>这两个值都必须采用十进制度数表示，并由波形符 (<b>~</b>) 分隔。</p>
<p>有效的经度值范围为 -180 到 +180（包括这两者）。</p>
<p>有效的纬度值范围为 -90 到 +90（包括这两者）。</p></td>
</tr>
<tr class="even">
<td align="left"><p><b>bb</b></p></td>
<td align="left"><p>边界框</p></td>
<td align="left"><p>bb = "bb=" southlatitude "_" westlongitude "~" northlatitude "_" eastlongitude</p>
<p>southlatitude = degreeslat</p>
<p>northlatitude = degreeslat</p>
<p>westlongitude = degreeslon</p>
<p>eastlongitude = degreeslon</p>
<p>degreeslat = \["-"\] 13DIGIT \["." 17DIGIT\]</p>
<p>degreeslon = ["-"] 12DIGIT ["." 17DIGIT]</p>
<p>示例：</p>
<p>bb=39.719_-74.52~41.71_-73.5</p></td>
<td align="left"><p>用于指定边界框的矩形区域，采用十进制度数表示，使用波形符 (<b>~</b>) 来区分左下角和右上角。 各个矩形区域的经纬度由下划线 (<b>_</b>) 分隔。</p>
<p>有效的经度值范围为 -180 到 +180（包括这两者）。</p>
<p>有效的纬度值范围为 -90 到 +90（包括这两者）。</p><p>当提供边界框时，将忽略 cp 和 lvl 参数。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><b>where</b></p></td>
<td align="left"><p>位置</p></td>
<td align="left"><p>where = "where=" whereval</p>
<p>whereval = 1 *( ALPHA / DIGIT / "-" / "." / "_" / pct-encoded / "!" / "$" / "'" / "(" / ")" / "*" / "+" / "," / ";" / ":" / "@" / "/" / "?")</p>
<p>示例：</p>
<p>where=1600%20Pennsylvania%20Ave,%20Washington,%20DC</p></td>
<td align="left"><p>特定位置、路标或地点的搜索词。</p></td>
</tr>
<tr class="even">
<td align="left"><p><b>q</b></p></td>
<td align="left"><p>查询词</p></td>
<td align="left"><p>q = "q="</p>
<p>whereval</p>
<p>示例：</p>
<p>q=mexican%20restaurants</p></td>
<td align="left"><p>本地商家或商家类别的搜索词。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><b>lvl</b></p></td>
<td align="left"><p>缩放级别</p></td>
<td align="left"><p>lvl = "lvl=" 1<i>2DIGIT \["." 1</i>2DIGIT\]</p>
<p>示例：</p>
<p>lvl=10.50</p></td>
<td align="left"><p>定义地图视图的缩放级别。 有效值介于 1 至 20 之间，其中 1 表示缩到最小。</p></td>
</tr>
<tr class="even">
<td align="left"><p><b>sty</b></p></td>
<td align="left"><p>样式</p></td>
<td align="left"><p>sty = "sty=" ("a" / "r"/"3d")</p>
<p>示例：</p>
<p>sty=a</p></td>
<td align="left"><p>定义地图样式 此参数的有效值包括：</p>
<ul>
<li>**a**：借助地图显示鸟瞰图。</li>
<li>**r**：借助地图显示街景图。</li>
<li>**3d**：借助地图显示 3D 视图。 可与 **cp** 参数结合使用，也可以与 **rad** 参数结合使用。</li>
</ul>
<p>在 Windows 10 中，鸟瞰图和 3D 视图样式相同。</p>
<div class="alert">
**注意**省略**sty**参数将产生相同的结果与 sty = r。
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p><b>rad</b></p></td>
<td align="left"><p>半径</p></td>
<td align="left"><p>rad = "rad=" 1*8DIGIT</p>
<p>示例：</p>
<p>rad=1000</p></td>
<td align="left"><p>一个圆形区域，可指定所需的地图视图。 半径值以米为单位进行测量。</p></td>
</tr>
<tr class="even">
<td align="left"><p><b>pit</b></p></td>
<td align="left"><p>俯仰</p></td>
<td align="left"><p>pit = "pit=" pitch</p>
<p>示例：</p>
<p>pit=60</p></td>
<td align="left"><p>指示查看地图的角度，其中 90 是水平查看（最大），0 是俯视查看（最小）。</p><p>有效的俯仰值范围为 0 到 90（包括这两者）。</td>
</tr>
<tr class="odd">
<td align="left"><p><b>hdg</b></p></td>
<td align="left"><p>方位</p></td>
<td align="left"><p>hdg = "hdg=" heading</p>
<p>示例：</p>
<p>hdg=180</p></td>
<td align="left"><p>指示以角度表示的地图前进方向，其中 0 或 360 = 北、90 = 东、180 = 南和 270 = 西。</p></td>
</tr>
<tr class="even">
<td align="left"><p><b>ss</b></p></td>
<td align="left"><p>街景</p></td>
<td align="left"><p>ss = "ss=" BIT</p>
<p>示例：</p>
<p>ss=1</p></td>
<td align="left"><p>指示在 <code>ss=1</code> 时所显示的街景图像。 省略 <b>ss</b> 参数将产生与 <code>ss=0</code> 相同的结果。 通过与 <b>cp</b> 参数结合使用，指定街道级视图的位置。</p>
<div class="alert">
**注意**街道级图像在所有地区不可用。
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p><b>trfc</b></p></td>
<td align="left"><p>路况</p></td>
<td align="left"><p>trfc = "trfc=" BIT</p>
<p>示例：</p>
<p>trfc=1</p></td>
<td align="left"><p>指定地图上是否包含路况信息。 省略 trfc 参数将产生与 <code>trfc=0</code> 时相同的结果。</p>
<div class="alert">
**注意**路况数据在所有地区不可用。
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left"><p><b>rtp</b></p></td>
<td align="left"><p>路线</p></td>
<td align="left"><p>rtp = "rtp=" (waypoint "~" [waypoint]) / ("~" waypoint)</p>
<p>waypoint = ("pos." point ) / ("adr." whereval)</p>
<p>point = "point." pointval ["_" title]</p>
<p>pointval = degreeslat "" degreeslon</p>
<p>degreeslat = ["-"] 13DIGIT ["." 17DIGIT]</p>
<p>degreeslon = ["-"] 12DIGIT ["." 17DIGIT]</p>
<p>title = whereval</p>
<p>whereval = 1( ALPHA / DIGIT / "-" / "." / "_" / pct-encoded / "!" / "$" / "'" / "(" / ")" / "" / "+" / "," / ";" / ":" / "@" / "/" / "?")</p>


<p>示例：</p>
<p>rtp=adr.Mountain%20View,%20CA~adr.SFO</p>
<p>rtp=adr.One%20Microsoft%20Way,%20Redmond,%20WA~pos.45.23423_-122.1232 _My%20Picnic%20Spot</p></td>
<td align="left"><p>定义要在地图上绘制的路线起点和终点，由波形符 (<b>~</b>) 分隔。 每个经过点可能是通过使用纬度和经度的位置定义的，也可能是通过可选标题或地址标识符定义的。</p>
<p>一条完整的路线正好包含两个经过点。 例如，包含两个经过点的路线由 <code>rtp="A"~"B"</code> 定义。</p>
<p>还可以指定不完整的路线。 例如，你可以仅定义包含 <code>rtp="A"~</code> 的路线起点。 在此情况下，显示路线输入时将在**出发地**字段中带有所提供的经过点，并且**目的地**字段具有焦点。</p>
<p>如果指定路线终点，与 <code>rtp=~"B"</code> 相同，路线面板上的**目的地**字段中将显示所提供的经过点。 如果提供精确的当前位置，当前位置将预先填写在具有焦点的**出发地**字段中。</p>
<p>如果给出一条不完整的路线，则不会绘制任何路线。</p>
<p>通过与 **mode** 参数结合使用，指定交通的模式（驾车、公交或步行）。 如果未指定 **mode**，将使用交通首选项的用户的模式提供路线。</p>
<div class="alert">
**注意**如果由**pos**参数值指定该位置可以位置使用标题。 将显示标题，而不是显示纬度和经度。
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p><b>mode</b></p></td>
<td align="left"><p>交通模式</p></td>
<td align="left"><p>mode = "mode=" ("d" / "t" / "w")</p>
<p>示例：</p>
<p>mode=d</p></td>
<td align="left"><p>定义交通模式。 此参数的有效值包括：</p>
<ul>
<li>**d**：显示驾车路线的路线概述</li>
<li>**t**：显示公交路线的路线概述</li>
<li>**w**：显示步行路线的路线概述</li>
</ul>
<p>针对交通路线与 **rtp** 参数结合使用。 如果未指定 **mode**，将使用交通首选项的用户的模式提供路线。 在提供 **mode** 时，可以不提供任何路线参数用于为该模式输入从当前位置的路线输入。</p></td>
</tr>

<tr class="even">
<td align="left"><p><b>collection</b></p></td>
<td align="left"><p>集合</p></td>
<td align="left"><p>collection = "collection="(name"~"/)point["~"point]</p>
<p>name = "name." whereval </p>
<p>whereval = 1( ALPHA / DIGIT / "-" / "." / "_" / pct-encoded / "!" / "$" / "'" / "(" / ")" / "" / "+" / "," / ";" / ":" / "@" / "/" / "?") </p>
<p>point = "point." pointval ["_" title] </p>
<p>pointval = degreeslat "" degreeslon </p>
<p>degreeslat = ["-"] 13DIGIT ["." 17DIGIT] </p>
<p>degreeslon = ["-"] 12DIGIT ["." 17DIGIT] </p>
<p>title = whereval</p>


<p>示例：</p>
<p>collection=name.My%20Trip%20Stops~point.36.116584_-115.176753_Las%20Vegas~point.37.8268_-122.4798_Golden%20Gate%20Bridge</p></td>
<td align="left"><p>要添加到地图和列表的点的集合。 可以使用 name 参数命名点集合。 使用纬度、经度和可选标题指定点。</p>
<p>用波形符 (**~**) 分隔名称和多个点。</p>
<p>如果你指定的项目中包含波形符，请确保已将该波形符编码为 <code>%7E</code>。 如果不带有“中心点”和“缩放级别”参数，则集合将提供最佳地图视图。</p>

<p>**重要提示** 如果你指定的项目中包含下划线，请确保将该下划线双编码为 %255F。</p></td>
</tr>
</tbody>
</table>

  
<span id="ms-drive-to-param-reference"/>

## <a name="ms-drive-to-parameter-reference"></a>ms-drive-to: 参数引用


用于启动逐向导航驾驶路线请求的 URI 无需编码，并且具有以下格式。

> **注意** 不在此 URI 方案中指定起点。 起点将始终假定为当前位置。 如果你需要指定不同于当前位置的起点，请参阅[显示路线和路况](#display-directions-and-traffic)。

 

| 参数 | 定义 | 示例 | 详细信息 |
|------------|-----------|---------|---------|
| **destination.latitude** | 目的地纬度 | 示例：destination.latitude=47.6451413797194 | 目的地的纬度。 有效的纬度值范围为 -90 到 +90（包括这两者）。 |
| **destination.longitude** | 目的地经度 | 示例：destination.longitude=-122.141964733601 | 目的地的经度。 有效的经度值范围为 -180 到 +180（包括这两者）。 |
| **destination.name** | 目的地的名称 | 示例：destination.name=Redmond, WA | 目的地的名称。 你无需编码 **destination.name** 值。 |

 
<span id="ms-walk-to-param-reference"/>

## <a name="ms-walk-to-parameter-reference"></a>ms-walk-to: 参数引用


用于启动逐向导航步行路线请求的 URI 无需编码，并且具有以下格式。

> **注意** 不在此 URI 方案中指定起点。 起点将始终假定为当前位置。 如果你需要指定不同于当前位置的起点，请参阅[显示路线和路况](#display-directions-and-traffic)。
 

| 参数 | 定义 | 示例 | 详细信息 |
|-----------|------------|---------|----------|
| **destination.latitude** | 目的地纬度 | 示例：destination.latitude=47.6451413797194 | 目的地的纬度。 有效的纬度值范围为 -90 到 +90（包括这两者）。 |
| **destination.longitude** | 目的地经度 | 示例：destination.longitude=-122.141964733601 | 目的地的经度。 有效的经度值范围为 -180 到 +180（包括这两者）。 |
| **destination.name** | 目的地的名称 | 示例：destination.name=Redmond, WA | 目的地的名称。 你无需编码 **destination.name** 值。 |

## <a name="ms-settings-parameter-reference"></a>ms-settings: 参数引用

**ms-settings:** URI 方案的地图应用特定参数的语法定义如下。 **maps-downloadmaps** 采用 **ms-settings:maps-downloadmaps?** 的形式与 **ms-settings:** URI 一起指定以指示脱机地图设置页。 

| 参数 | 定义 | 示例 | 详细信息 |
|-----------|------------|---------|----------|
| **latlong** | 定义离线地图区域的点。 | 示例：latlong=47.6,-122.3 | geopoint 由逗号分隔的纬度和经度指定。 有效的纬度值范围为 -90 到 +90（包括这两者）。 有效的经度值范围为 -180 到 +180（包括这两者）。 |
