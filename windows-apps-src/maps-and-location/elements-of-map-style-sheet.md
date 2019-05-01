---
description: 地图样式表的条目和属性
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: 地图样式表参考
ms.date: 03/19/2017
ms.topic: article
keywords: windows 10, uwp, 地图, 地图样式表
ms.localizationpriority: medium
ms.openlocfilehash: 5f59775de8d86b5a0bae77d8c84e08e0328896f4
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2019
ms.locfileid: "63816684"
---
# <a name="map-style-sheet-reference"></a>地图样式表参考

使用 Microsoft 映射技术_映射样式表_可以定义地图的外观。  映射样式表使用 JavaScript 对象表示法 (JSON) 定义，可以采用各种方式包括在 Windows 应用商店应用程序的[MapControl](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol)通过[MapStyleSheet.ParseFromJson](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapstylesheet.parsefromjson#Windows_UI_Xaml_Controls_Maps_MapStyleSheet_ParseFromJson_System_String_)方法。

可以使用以交互方式创建样式表[地图样式表编辑器](https://www.microsoft.com/p/map-style-sheet-editor/9nbhtcjt72ft)应用程序。

可以使用以下 JSON 将以红色显示的水区域、 水标签显示为绿色，和土地的区域显示为蓝色：

```json
    {"version":"1.*",
        "settings":{"landColor":"#0000FF"},
        "elements":{"water":{"fillColor":"#FF0000","labelColor":"#00FF00"}}
    }
```

此 JSON 可以用于从映射中删除所有标签和点。

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

本主题介绍可以用于自定义地图外观的 JSON 条目和[属性](#properties)。  这些属性也应用于用户地图元素通过[MapStyleSheetEntry](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapelement.mapstylesheetentry)属性。

<a id="entries" />

## <a name="entries"></a>条目
使用此表“>”字符表示条目层次结构中的级别。  它还显示支持的 Windows 版本的每个条目以及其忽略。

| Version | Windows 版本名称 |
|---------|----------------------|
|  1703   | Creators 更新      |
|  1709   | 秋季创意者更新 |
|  1803   | 2018 年 4 月更新    |
|  1809   | 2018 年 10 月更新  |

| 名称                         | 属性组            | 1703 | 1709 | 1803 | 1809 | 描述    |
|------------------------------|---------------------------|------|------|------|------|----------------|
| version                      | [版本](#version)       |  ✔   |  ✔   |  ✔   |  ✔   | 要使用的样式表版本。 |
| 设置                     | [设置](#settings)     |  ✔   |  ✔   |  ✔   |  ✔   | 应用于整个样式表的设置。 |
| mapElement                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 所有地图条目的父条目。 |
| > baseMapElement             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 所有非用户条目的父条目。 |
| >> area                      | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 使用描述土地的区域。  这些应不会与物理建筑物的结构项之下的混淆。 |
| >>> airport                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 包含机场的区域。 |
| >>> areaOfInterest           | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | 业务或兴趣点高度集中的区域。 |
| >>> cemetery                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 包含 cemeteries 的区域。 |
| >>> continent                | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 大洲区域标签。 |
| >>> education                | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 包含学校和其他教育机构的区域。 |
| >>> indigenousPeoplesReserve | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 包含原住民 peoples 预留的区域。 |
| >>> industrial               | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | 用于工业目的的区域。 |
| >>> island                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 岛区域标签。 |
| >>> medical                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 用于医疗目的的区域 (例如： 医院校园)。 |
| >>> military                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 包含军事基或具有军事用途的区域。 |
| >>> nautical                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 用于航海相关目的的区域。 |
| >>> neighborhood             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 邻域区域标签。 |
| >>> runway                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 用作飞机就要的区域。 |
| >>> sand                     | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 沙地区域（如海滩）。 |
| >>> shoppingCenter           | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 为商场或其他购物中心分配的土地区域。 |
| >>> stadium                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 包含体育场的区域。 |
| >>> underground              | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | 地下区域（例如：地铁站）。 |
| >>> vegetation               | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 森林、草地区域等。 |
| >>>> forest                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 森林陆地区域。 |
| >>>> golfCourse              | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 包含高尔夫课程的区域。 |
| >>>> park                    | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 包含公园的区域。 |
| >>>> playingField            | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 提取的球场，如棒球场或网球场。 |
| >>>> reserve                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 包含特性的区域保留。 |
| >> point                     | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 绘制有某种类型的图标的所有点功能。 |
| >>> address                  | [PointStyle](#pointstyle) |      |      |  ✔   |  ✔   | 解决数字标签。 |
| >>> naturalPoint             | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 表示自然功能的图标。 |
| >>>> peak                    | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 表示山峰的图标。 |
| >>>>> volcanicPeak           | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 表示火山山峰的图标。 |
| >>>> waterPoint              | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 表示水景位置（如瀑布）的图标。 |
| >>> pointOfInterest          | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 表示感兴趣的任何位置的图标。 |
| >>>> business                | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 表示业务的任何位置的图标。 |
| >>>>> attractionPoint        | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | 表示旅游忍耐博物馆、 zoos 等的图标。 |
| >>>>> communityPoint         | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | 对社区表示位置的常规使用的图标。 |
| >>>>> educationPoint         | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | 图标表示学校和其他教育版相关的位置。 |
| >>>>> entertainmentPoint     | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | 表示娱乐会场影院、 电影院等的图标。 |
| >>>>> essentialServicePoint  | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | 表示服务，如停车、 银行、 天然气等的图标。 |
| >>>>> foodPoint              | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 表示餐馆、 吧等的图标。 |
| >>>>> lodgingPoint           | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | 表示旅店和其他住宿企业的图标。 |
| >>>>> realEstatePoint        | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | 表示的房地产公司的图标。 |
| >>>>> shoppingPoint          | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | 表示旅店和其他住宿企业的图标。 |
| >>> populatedPlace           | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 表示人口稠密位置（例如：城市或城镇）的大小的图标。 |
| >>>> capital                 | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 表示人口稠密地区的中心的图标。 |
| >>>>> adminDistrictCapital   | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 表示州或省的省会的图标。 |
| >>>>> countryRegionCapital   | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 表示国家或地区的首都/主要城市的图标。 |
| >>> roadShield               | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 表示道路的缩写名称的符号。 (例如：I-5)。 如果将设置条目的 **ImageFamily** 属性设置为 *Palette* 值，则仅使用面板值 |
| >>> roadExit                 | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 表示出口（通常属于出入管制高速公路）的图标。 |
| >>> transit                  | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 表示公共汽车站、火车站、机场等的图标。 |
| >> political                 | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 政治区域（如国家、地区和州）。 |
| >>> countryRegion            | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 国家/地区区域边框和标签。 |
| >>> adminDistrict            | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Admin1、 状态、 省/自治区等边框和标签。 |
| >>> district                 | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 管理员 2、 县、 等，边框和标签。 |
| >> structure                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 建筑物和其他类似于建筑物的结构。 |
| >>> building                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 建筑物。 |
| >>>> educationBuilding       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 建筑物教育版使用。 |
| >>>> medicalBuilding         | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 建筑物用于医疗目的，例如在医院。 |
| >>>> transitBuilding         | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 用于传输如机场的建筑物。 |
| >> transportation            | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 属于运输网络一部分的线条（例如：公路、火车和轮渡）。 |
| >>> road                     | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 表示所有公路的线条。 |
| >>>> controlledAccessHighway | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 表示较大的控制访问高速公路的行。 |
| >>>>> highSpeedRamp          | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 表示通常连接到高速斜坡的线条控制访问高速公路。 |
| >>>> highway                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 表示高速公路的行。 |
| >>>> majorRoad               | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 表示主要的道路的线条。 |
| >>>> arterialRoad            | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 表示动脉道路的线条。 |
| >>>> street                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 表示街道的线。 |
| >>>>> ramp                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 表示通常连接到高速公路斜坡的行。 |
| >>>>> unpavedStreet          | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 表示未铺设的街道的线。 |
| >>>> tollRoad                | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 表示产生费用，若要使用的道路的线条。 |
| >>> railway                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 铁路线。 |
| >>> trail                    | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 穿过公园的步行道或登山步道。 |
| >>> 走道                  | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | 提升的走道。 |
| >>> waterRoute               | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 轮渡路线。 |
| >> water                     | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 看起来像水的任何对象。 这包括海洋和溪流。 |
| >>> river                    | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 河流、溪流或其他水路。  轻注意，这可能是线条或多边形，并且可能会连接到非河流水体。 |
| > routeMapElement            | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 所有路由的相关的条目。 |
| >> routeLine                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 将行路由相关条目。 |
| >>> drivingRoute             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 代表驾驶路线的线条。 |
| >>> scenicRoute              | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | 代表 scenic 驾驶路线的线条。 |
| >>> walkingRoute             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 表示路由的每个步骤的行。 |
| > userMapElement             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 所有的用户条目。 |
| >> userBillboard             | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | 默认 [MapBillboard](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard) 实例的样式。 |
| >> userLine                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | 默认 [MapPolyline](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mappolyline) 实例的样式。 |
| >> userModel3D               | [MapElement3D](#mapelement3d) |      |  ✔   |  ✔   |  ✔   | 默认 [MapModel3D](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapmodel3d) 实例的样式。  这主要用于设置 renderAsSurface。 |
| >> userPoint                 | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | 默认 [MapIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapicon) 实例的样式。 |

<a id="properties" />

## <a name="properties"></a>属性

本部分介绍可以用于每个条目的属性。

<a id="version" />

### <a name="version-properties"></a>Version 属性

| 属性                     | 在任务栏的搜索框中键入    | 描述                                                                                                           |
|------------------------------|---------|-----------------------------------------------------------------------------------------------------------------------|
| version                      | 字符串  | 目标样式表版本。 用于适用性。 “1.0”用于默认值，“1.*”用于其他次要功能更新。 |

<a id="settings" />

### <a name="settings-properties"></a>Settings 属性

| 属性                     | 在任务栏的搜索框中键入    | 1703 | 1709 | 1803 | 1809 | 描述 |
|------------------------------|---------|------|------|------|------|-------------|
| atmosphereVisible            | Bool    |  ✔   |  ✔   |  ✔   |  ✔   | 一个标志，指示大气是否在 3D 控件中出现。 |
| buildingTexturesVisible      | Bool    |      |      |  ✔   |  ✔   | 指示是否在具有纹理的符号 3D 建筑物上显示纹理的标志。 |
| fogColor                     | 颜色   |  ✔   |  ✔   |  ✔   |  ✔   | 在 3D 控件中出现的远距离雾的 ARGB 颜色。 |
| glowColor                    | 颜色   |  ✔   |  ✔   |  ✔   |  ✔   | 可以应用于标签发光和图标发光的 ARGB 颜色值。 |
| imageFamily                  | 字符串  |  ✔   |  ✔   |  ✔   |  ✔   | 用于此样式的图像集的名称。 对于使用基于实际符号的固定颜色的符号，将此值设置为 *Default*。 对于使用可配置调色板颜色的符号，将此值设置为 *Palette*。 |
| landColor                    | 颜色   |  ✔   |  ✔   |  ✔   |  ✔   | 在该陆地上绘制任何内容之前，陆地的 ARGB 颜色值。 |
| logosVisible                 | Bool    |  ✔   |  ✔   |  ✔   |  ✔   | 一个标志，指示具有 **Organization** 属性的项目是应绘制相应徽标还是使用通用图标。 |
| officialColorVisible         | Bool    |  ✔   |  ✔   |  ✔   |  ✔   | 一个标志，指示具有正式颜色属性的项目（如中国的交通线）是否应绘制该颜色。 例如，为黑白地图关闭此值。 |
| rasterRegionsVisible         | Bool    |  ✔   |  ✔   |  ✔   |  ✔   | 一个标志，指示绘制光栅区域必须比矢量 （日本和韩国） 的更好地表示形式。 |
| shadedReliefVisible          | Bool    |  ✔   |  ✔   |  ✔   |  ✔   | 一个标志，指示是否要在地图上绘制海拔底纹。 |
| shadedReliefDarkColor        | 颜色   |  ✔   |  ✔   |  ✔   |  ✔   | 晕渲地貌阴暗面的颜色。  Alpha 通道表示的最大的 alpha 值。 |
| shadedReliefLightColor       | 颜色   |  ✔   |  ✔   |  ✔   |  ✔   | 晕渲地貌光明面的颜色。  Alpha 通道表示的最大的 alpha 值。 |
| shadowColor                  | 颜色   |      |      |      |  ✔   | 后面使用阴影的图标的阴影颜色。 |
| spaceColor                   | 颜色   |  ✔   |  ✔   |  ✔   |  ✔   | 地图周围的区域 ARGB 颜色值。 |
| useDefaultImageColors        | Bool    |  ✔   |  ✔   |  ✔   |  ✔   | 一个标志，指示是否在 SVG 中的原始颜色应使用，而不是查询图像中颜色的调色板条目。 |

<a id="mapelement" />

### <a name="mapelement-properties"></a>MapElement 属性

| 属性                     | 在任务栏的搜索框中键入    | 1703 | 1709 | 1803 | 1809 | 描述 |
|------------------------------|---------|------|------|------|------|-------------|
| backgroundScale              | 浮点   |  ✔   |  ✔   |  ✔   |  ✔   | 图标的背景元素应缩放的量。  例如，使用 *1* 表示默认值，使用 *2* 表示两倍大。 |
| fillColor                    | 颜色   |  ✔   |  ✔   |  ✔   |  ✔   | 用于填充多边形、点图标的背景和线条中心（如果已拆分）的颜色。 |
| fontFamily                   | 字符串  |  ✔   |  ✔   |  ✔   |  ✔   |  |
| iconColor                    | 颜色   |  ✔   |  ✔   |  ✔   |  ✔   | 在点图标中心显示的字形的颜色。 |
| iconScale                    | 浮点   |      |  ✔   |  ✔   |  ✔   | 图标的字形应缩放的量。  例如，使用 *1* 表示默认值，使用 *2* 表示两倍大。 |
| labelColor                   | 颜色   |  ✔   |  ✔   |  ✔   |  ✔   |  |
| labelOutlineColor            | 颜色   |  ✔   |  ✔   |  ✔   |  ✔   |  |
| labelScale                   | 浮点   |  ✔   |  ✔   |  ✔   |  ✔   | 默认标签大小的缩放量。 例如，使用 *1* 表示默认值，使用 *2* 表示两倍大。 |
| labelVisible                 | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  |
| overwriteColor               | Bool    |  ✔   |  ✔   |  ✔   |  ✔   | 使 **FillColor** 的 alpha 值覆盖 **StrokeColor** 而不是与它混合。 |
| scale                        | 浮点   |  ✔   |  ✔   |  ✔   |  ✔   | 整个点的大小的缩放量。 例如，使用 *1* 表示默认值，使用 *2* 表示两倍大。 |
| strokeColor                  | 颜色   |  ✔   |  ✔   |  ✔   |  ✔   | 要用于多边形周围的轮廓、点图标周围的轮廓和线条颜色的颜色。 |
| strokeWidthScale             | 浮点   |  ✔   |  ✔   |  ✔   |  ✔   | 线条笔划的缩放量。 例如，使用 *1* 表示默认值，使用 *2* 表示两倍大。 |
| visible                      | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  |

<a id="borderedmap" />

### <a name="borderedmapelement"></a>BorderedMapElement

此属性组继承自 [MapElement](#mapelement) 属性组。

| 属性                     | 在任务栏的搜索框中键入    | 1703 | 1709 | 1803 | 1809 | 描述 |
|------------------------------|---------|------|------|------|------|-------------|
| borderOutlineColor           | 颜色   |  ✔   |  ✔   |  ✔   |  ✔   | 填色多边形边框的辅助或包围线条颜色。 |
| borderStrokeColor            | 颜色   |  ✔   |  ✔   |  ✔   |  ✔   | 填色多边形边框的主线条颜色。 |
| borderVisible                | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  |
| borderWidthScale             | 浮点   |  ✔   |  ✔   |  ✔   |  ✔   | 边框的笔画进行缩放的量。 例如，使用 *1* 表示默认值，使用 *2* 表示两倍大。 |

<a id="pointstyle" />

### <a name="pointstyle-properties"></a>PointStyle 属性

此属性组继承自 [MapElement](#mapelement) 属性组。

| 属性                     | 在任务栏的搜索框中键入    | 1703 | 1709 | 1803 | 1809 | 描述 |
|------------------------------|---------|------|------|------|------|-------------|
| shape-Background             | 浮点   |      |      |      |  ✔   | 要用作背景的图标--替换为不存在任何形状的形状。 |
| stemAnchorRadiusScale        | 浮点   |      |      |  ✔   |  ✔   | 图标主干的定位点应缩放的量。  例如，使用 *1* 表示默认值，使用 *2* 表示两倍大。 |
| stemColor                    | 颜色   |  ✔   |  ✔   |  ✔   |  ✔   | 出自 3D 模式中的图标底部的主干的颜色。 |
| stemHeightScale              | 浮点   |      |      |  ✔   |  ✔   | 图标主干的长度应缩放的量。  例如，使用 *1* 表示默认值，使用 *2* 表示两倍长。 |
| stemOutlineColor             | 颜色   |  ✔   |  ✔   |  ✔   |  ✔   | 出自 3D 模式中的图标底部的主干周围的轮廓颜色。 |
| stemWidthScale               | 浮点   |  ✔   |  ✔   |  ✔   |  ✔   | 图标主干的宽度应缩放的量。  例如，使用 *1* 表示默认值，使用 *2* 表示两倍长。 |

<a id="mapelement3d" />

### <a name="mapelement3d"></a>MapElement3D

此属性组继承自 [MapElement](#mapelement) 属性组。

| 属性                     | 在任务栏的搜索框中键入    | 1703 | 1709 | 1803 | 1809 | 描述 |
|------------------------------|---------|------|------|------|------|------------|
| renderAsSurface              | Bool    |      |      |  ✔   |  ✔   | 指示 3D 模型应象建筑一样呈现的标志 - 无对地面的深度淡入。 |
