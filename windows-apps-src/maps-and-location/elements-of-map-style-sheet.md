---
author: normesta
description: 地图样式表的条目和属性
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: 地图样式表参考
ms.author: normesta
ms.date: 03/16/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 地图, 地图样式表
ms.localizationpriority: medium
ms.openlocfilehash: 984741de5be585f7d6d726ec4c736e6ebce78830
ms.sourcegitcommit: 9c79fdab9039ff592edf7984732d300a14e81d92
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/23/2018
ms.locfileid: "2814929"
---
# <a name="map-style-sheet-reference"></a>地图样式表参考

Microsoft 映射技术使用映射定义映射的外观的样式表。  映射样式表使用 JavaScript 对象表示法 (JSON) 定义的并可在各种方式包括在 Windows 应用商店应用程序的[MapControl](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol)通过[MapStyleSheet.ParseFromJson](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapstylesheet.parsefromjson#Windows_UI_Xaml_Controls_Maps_MapStyleSheet_ParseFromJson_System_String_)方法。

例如，应使用以下 JSON 使水区域显示为红色、水标签显示为绿色并且陆地区域显示为蓝色：

```json
    {"version":"1.*",
        "settings":{"landColor":"#0000FF"},
        "elements":{"water":{"fillColor":"#FF0000","labelColor":"#00FF00"}}
    }
```
还可以使用 JSON 从地图中删除所有标签和点。

```json

    {"version":"1.*", "elements":{"mapElement":{"labelVisible":false},"point":{"visible":false}}}
```

有时将转换属性的值以生成最终结果。  例如，植被填充色根据所显示实体的类型会有略微不同的阴影。  此行为可以关闭，因此可通过使用 ignoreTransform 而使用提供的精确值。

```json
    {"version":"1.*",
        "settings":{"shadedReliefVisible":false},
        "elements":{"vegetation":{"fillColor":{"value":"#999999","ignoreTransform":true}}}
    }
```

本主题介绍可以用于自定义地图外观的 JSON 条目和[属性](#properties)。

<a id="entries" />

## <a name="entries"></a>条目
使用此表“>”字符表示条目层次结构中的级别。  它还会显示哪些版本的 Windows 支持每个条目，其忽略它。

| 内部版本 | Windows 版本名称 |
|-------|----------------------|
| 1506  | 创意者更新      |
| 1629  | 秋季创意者更新 |
| 1713  | 2018 年 4 月更新    |

| 名称                         | 属性组            | 1506 | 1629 | 1713 | 下一个 | 说明    |
|------------------------------|---------------------------|------|------|------|------|----------------|
| version                      | [Version](#version)       |  ✔   |  ✔   |  ✔   |  ✔   | 要使用的样式表版本。 |
| settings                     | [Settings](#settings)     |  ✔   |  ✔   |  ✔   |  ✔   | 应用于整个样式表的设置。 |
| mapElement                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 所有地图条目的父条目。 |
| > baseMapElement             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 所有非用户条目的父条目。 |
| >> area                      | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 使用区域描述园地。  这些不应将与物理建筑物结构项下这些混淆。 |
| >>> airport                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 包含机场的区域。 |
| >>> areaOfInterest           | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | 业务或兴趣点高度集中的区域。 |
| >>> cemetery                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 包含 cemeteries 的区域。 |
| >>> continent                | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 大陆区域标签。 |
| >>> education                | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 包含学校和其他教育基础设施的区域。 |
| >>> indigenousPeoplesReserve | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 包含原住民人民保留的区域。 |
| >>> industrial               | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | 用于行业的目的的区域。 |
| >>> island                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 岛区域标签。 |
| >>> medical                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 用于医疗的目的的区域 (例如： 医院校园)。 |
| >>> military                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 包含军事进制数或军事使用的区域。 |
| >>> nautical                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 用于进行航海相关的区域。 |
| >>> neighborhood             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 邻居区域标签。 |
| >>> runway                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 用作飞机飞机跑道的区域。 |
| >>> sand                     | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 沙地区域（如海滩）。 |
| >>> shoppingCenter           | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 为商场或其他购物中心分配的土地区域。 |
| >>> stadium                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 包含体育场的区域。 |
| >>> underground              | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | 地下区域（例如：地铁站）。 |
| >>> vegetation               | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 森林、草地区域等。 |
| >>>> forest                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 森林陆地区域。 |
| >>>> golfCourse              | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 包含高尔夫课程的区域。 |
| >>>> park                    | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 包含公园的区域。 |
| >>>> playingField            | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 提取的球场，如棒球场或网球场。 |
| >>>> reserve                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 保留包含性质的区域。 |
| >> point                     | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 都指向有图标的某种绘制的功能。 |
| >>> address                  | [PointStyle](#pointstyle) |      |      |  ✔   |  ✔   | 地址数字标签。 |
| >>> naturalPoint             | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 表示自然功能的图标。 |
| >>>> peak                    | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 表示山峰的图标。 |
| >>>>> volcanicPeak           | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 表示火山山峰的图标。 |
| >>>> waterPoint              | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 表示水景位置（如瀑布）的图标。 |
| >>> pointOfInterest          | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 表示有趣的任何位置的图标。 |
| >>>> business                | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 代表任何业务其中的图标。 |
| >>>>> attractionPoint        | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | 表示如博物馆、 zoos 等风景焦点的图标。 |
| >>>>> communityPoint         | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | 表示社区的常规的使用位置的图标。 |
| >>>>> educationPoint         | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | 代表学校和其他培训图标相关的位置。 |
| >>>>> entertainmentPoint     | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | 表示娱乐地点，如剧院、 cinemas 等的图标。 |
| >>>>> essentialServicePoint  | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | 表示基本的服务，如驻留、 银行、 天然气等的图标。 |
| >>>>> foodPoint              | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 表示餐厅、 吧等的图标。 |
| >>>>> lodgingPoint           | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | 代表酒店或和其他住宿企业的图标。 |
| >>>>> realEstatePoint        | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | 表示不动产企业的图标。 |
| >>>>> shoppingPoint          | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | 代表酒店或和其他住宿企业的图标。 |
| >>> populatedPlace           | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 表示人口稠密位置（例如：城市或城镇）的大小的图标。 |
| >>>> capital                 | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 表示人口稠密地区的中心的图标。 |
| >>>>> adminDistrictCapital   | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 表示州或省的省会的图标。 |
| >>>>> countryRegionCapital   | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 表示国家或地区的首都/主要城市的图标。 |
| >>> roadShield               | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 表示道路的缩写名称的符号。 （例如：I-5）。 如果将设置条目的 **ImageFamily** 属性设置为 *Palette* 值，则仅使用面板值 |
| >>> roadExit                 | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 表示出口（通常属于出入管制高速公路）的图标。 |
| >>> transit                  | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 表示公共汽车站、火车站、机场等的图标。 |
| >> political                 | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 政治区域（如国家、地区和州）。 |
| >>> countryRegion            | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 国家/地区区域边框和标签。 |
| >>> adminDistrict            | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Admin1、 状态、 省、 等 borders 和标签。 |
| >>> district                 | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Admin2、 县等，borders 和标签。 |
| >> structure                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 建筑物和其他类似于建筑物的结构。 |
| >>> building                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 建筑物。 |
| >>>> educationBuilding       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 用于教育的建筑物。 |
| >>>> medicalBuilding         | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 建筑物用于如医院医疗目的。 |
| >>>> transitBuilding         | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 用于在传输过程机场例如建筑物。 |
| >> transportation            | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 属于运输网络一部分的线条（例如：公路、火车和轮渡）。 |
| >>> road                     | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 表示所有公路的线条。 |
| >>>> controlledAccessHighway | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 表示大型、 控制访问通道的行。 |
| >>>>> highSpeedRamp          | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 行表示通常是连接到的高速渐变控制访问通道。 |
| >>>> highway                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 表示通道的行。 |
| >>>> majorRoad               | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 表示主要道路的行。 |
| >>>> arterialRoad            | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 表示动脉道路的行。 |
| >>>> street                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 表示街道的行。 |
| >>>>> ramp                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 表示通常是连接到独立通道的渐变的行。 |
| >>>>> unpavedStreet          | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 表示未铺设的街道的行。 |
| >>>> tollRoad                | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 表示使用付费的道路的行。 |
| >>> railway                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 铁路线。 |
| >>> trail                    | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 穿过公园的步行道或登山步道。 |
| >>> 走道                  | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | 提升的走道。 |
| >>> waterRoute               | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 轮渡路线。 |
| >> water                     | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 看起来像水的任何对象。 这包括海洋和溪流。 |
| >>> river                    | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 河流、溪流或其他水路。  轻注意，这可能是线条或多边形，并且可能会连接到非河流水体。 |
| > routeMapElement            | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 所有路由的相关的条目。 |
| >> routeLine                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 路由行相关项。 |
| >>> drivingRoute             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 表示驱动路由的行。 |
| >>> scenicRoute              | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | 表示风景如画驱动路由的行。 |
| >>> walkingRoute             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 表示逐个路由行。 |
| > userMapElement             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 用户的所有条目。 |
| >> userBillboard             | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | 默认 [MapBillboard](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard) 实例的样式。 |
| >> userLine                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 默认 [MapPolyline](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mappolyline) 实例的样式。 |
| >> userModel3D               | [MapElement3D](#mapelement3d) |      |  ✔   |  ✔   |  ✔   | 默认 [MapModel3D](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapmodel3d) 实例的样式。  这主要用于设置 renderAsSurface。 |
| >> userPoint                 | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 默认 [MapIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapicon) 实例的样式。 |

<a id="properties" />

## <a name="properties"></a>属性

本部分介绍可以用于每个条目的属性。

<a id="version" />

### <a name="version-properties"></a>Version 属性

| 属性                     | 类型    | 说明                                                                                                           |
|------------------------------|---------|-----------------------------------------------------------------------------------------------------------------------|
| version                      | String  | 目标样式表版本。 用于适用性。 “1.0”用于默认值，“1.*”用于其他次要功能更新。 |

<a id="settings" />

### <a name="settings-properties"></a>Settings 属性

| 属性                     | 类型    | 1506 | 1629 | 1713 | 下一个 | 说明 |
|------------------------------|---------|------|------|------|------|-------------|
| atmosphereVisible            | Bool    |  ✔   |  ✔   |  ✔   |  ✔   | 一个标志，指示大气是否在 3D 控件中出现。 |
| buildingTexturesVisible      | Bool    |      |      |  ✔   |  ✔   | 指示是否在具有纹理的符号 3D 建筑物上显示纹理的标志。 |
| fogColor                     | Color   |  ✔   |  ✔   |  ✔   |  ✔   | 在 3D 控件中出现的远距离雾的 ARGB 颜色。 |
| glowColor                    | Color   |  ✔   |  ✔   |  ✔   |  ✔   | 可以应用于标签发光和图标发光的 ARGB 颜色值。 |
| imageFamily                  | String  |  ✔   |  ✔   |  ✔   |  ✔   | 用于此样式的图像集的名称。 对于使用基于实际符号的固定颜色的符号，将此值设置为 *Default*。 对于使用可配置调色板颜色的符号，将此值设置为 *Palette*。 |
| landColor                    | Color   |  ✔   |  ✔   |  ✔   |  ✔   | 在该陆地上绘制任何内容之前，陆地的 ARGB 颜色值。 |
| logosVisible                 | Bool    |  ✔   |  ✔   |  ✔   |  ✔   | 一个标志，指示具有 **Organization** 属性的项目是应绘制相应徽标还是使用通用图标。 |
| officialColorVisible         | Bool    |  ✔   |  ✔   |  ✔   |  ✔   | 一个标志，指示具有正式颜色属性的项目（如中国的交通线）是否应绘制该颜色。 例如，为黑白地图关闭此值。 |
| rasterRegionsVisible         | Bool    |  ✔   |  ✔   |  ✔   |  ✔   | 指示绘制在其具有比向量 （日本和韩国） 的更好地表示光栅区域的标志。 |
| shadedReliefVisible          | Bool    |  ✔   |  ✔   |  ✔   |  ✔   | 一个标志，指示是否要在地图上绘制海拔底纹。 |
| shadedReliefDarkColor        | Color   |  ✔   |  ✔   |  ✔   |  ✔   | 晕渲地貌阴暗面的颜色。  Alpha 通道代表 alpha 的最大值。 |
| shadedReliefLightColor       | Color   |  ✔   |  ✔   |  ✔   |  ✔   | 晕渲地貌光明面的颜色。  Alpha 通道代表 alpha 的最大值。 |
| shadowColor                  | Color   |      |      |      |  ✔️   | 后面使用阴影的图标阴影的颜色。 |
| spaceColor                   | Color   |  ✔   |  ✔   |  ✔   |  ✔   | 地图周围的区域 ARGB 颜色值。 |
| useDefaultImageColors        | Bool    |  ✔   |  ✔   |  ✔   |  ✔   | 指示是否 SVG 中的原始颜色应使用，而不是查找 up 图像中颜色的调色板条目的标志。 |

<a id="mapelement" />

### <a name="mapelement-properties"></a>MapElement 属性

| 属性                     | 类型    | 1506 | 1629 | 1713 | 下一个 | 描述 |
|------------------------------|---------|------|------|------|------|-------------|
| backgroundScale              | Float   |  ✔   |  ✔   |  ✔   |  ✔   | 图标的背景元素应缩放的量。  例如，使用 *1* 表示默认值，使用 *2* 表示两倍大。 |
| fillColor                    | Color   |  ✔   |  ✔   |  ✔   |  ✔   | 用于填充多边形、点图标的背景和线条中心（如果已拆分）的颜色。 |
| fontFamily                   | String  |  ✔   |  ✔   |  ✔   |  ✔   |  |
| iconColor                    | Color   |  ✔   |  ✔   |  ✔   |  ✔   | 在点图标中心显示的字形的颜色。 |
| iconScale                    | Float   |      |  ✔   |  ✔   |  ✔   | 图标的字形应缩放的量。  例如，使用 *1* 表示默认值，使用 *2* 表示两倍大。 |
| labelColor                   | Color   |  ✔   |  ✔   |  ✔   |  ✔   |  |
| labelOutlineColor            | Color   |  ✔   |  ✔   |  ✔   |  ✔   |  |
| labelScale                   | Float   |  ✔   |  ✔   |  ✔   |  ✔   | 默认标签大小的缩放量。 例如，使用 *1* 表示默认值，使用 *2* 表示两倍大。 |
| labelVisible                 | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  |
| overwriteColor               | Bool    |  ✔   |  ✔   |  ✔   |  ✔   | 使 **FillColor** 的 alpha 值覆盖 **StrokeColor** 而不是与它混合。 |
| scale                        | Float   |  ✔   |  ✔   |  ✔   |  ✔   | 整个点的大小的缩放量。 例如，使用 *1* 表示默认值，使用 *2* 表示两倍大。 |
| strokeColor                  | Color   |  ✔   |  ✔   |  ✔   |  ✔   | 要用于多边形周围的轮廓、点图标周围的轮廓和线条颜色的颜色。 |
| strokeWidthScale             | Float   |  ✔   |  ✔   |  ✔   |  ✔   | 线条笔划的缩放量。 例如，使用 *1* 表示默认值，使用 *2* 表示两倍大。 |
| visible                      | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  |

<a id="borderedmap" />

### <a name="borderedmapelement"></a>BorderedMapElement

此属性组继承自 [MapElement](#mapelement) 属性组。

| 属性                     | 类型    | 1506 | 1629 | 1713 | 下一个 | 说明 |
|------------------------------|---------|------|------|------|------|-------------|
| borderOutlineColor           | Color   |  ✔   |  ✔   |  ✔   |  ✔   | 填色多边形边框的辅助或包围线条颜色。 |
| borderStrokeColor            | Color   |  ✔   |  ✔   |  ✔   |  ✔   | 填色多边形边框的主线条颜色。 |
| borderVisible                | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  |
| borderWidthScale             | Float   |  ✔   |  ✔   |  ✔   |  ✔   | 按其进行缩放边框的笔划量。 例如，使用 *1* 表示默认值，使用 *2* 表示两倍大。 |

<a id="pointstyle" />

### <a name="pointstyle-properties"></a>PointStyle 属性

此属性组继承自 [MapElement](#mapelement) 属性组。

| 属性                     | 类型    | 1506 | 1629 | 1713 | 下一个 | 说明 |
|------------------------------|---------|------|------|------|------|-------------|
| 形状背景             | Float   |      |      |      |  ✔️   | 要用作背景图标-替换存在任何形状的形状。 |
| stemAnchorRadiusScale        | Float   |      |      |  ✔   |  ✔   | 图标主干的定位点应缩放的量。  例如，使用 *1* 表示默认值，使用 *2* 表示两倍大。 |
| stemColor                    | Color   |  ✔   |  ✔   |  ✔   |  ✔   | 出自 3D 模式中的图标底部的主干的颜色。 |
| stemHeightScale              | Float   |      |      |  ✔   |  ✔   | 图标主干的长度应缩放的量。  例如，使用 *1* 表示默认值，使用 *2* 表示两倍长。 |
| stemOutlineColor             | Color   |  ✔   |  ✔   |  ✔   |  ✔   | 出自 3D 模式中的图标底部的主干周围的轮廓颜色。 |
| stemWidthScale               | Float   |  ✔   |  ✔   |  ✔   |  ✔   | 图标主干的宽度应缩放的量。  例如，使用 *1* 表示默认值，使用 *2* 表示两倍长。 |

<a id="mapelement3d" />

### <a name="mapelement3d"></a>MapElement3D

此属性组继承自 [MapElement](#mapelement) 属性组。

| 属性                     | 类型    | 1506 | 1629 | 1713 | 下一个 | 描述 |
|------------------------------|---------|------|------|------|------|------------|
| renderAsSurface              | Bool    |      |      |  ✔   |  ✔   | 指示 3D 模型应象建筑一样呈现的标志 - 无对地面的深度淡入。 |
