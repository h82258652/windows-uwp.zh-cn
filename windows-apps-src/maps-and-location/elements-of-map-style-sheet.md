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
ms.openlocfilehash: 8fb80bc28900ee695ecf3b9e62b5dafc8f1a8cb3
ms.sourcegitcommit: f91aa1e402f1bc093b48a03fbae583318fc7e05d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2018
ms.locfileid: "1917770"
---
# <a name="map-style-sheet-reference"></a>地图样式表参考

可以使用 JavaScript 对象表示法 (JSON) 创建地图样式表。

例如，应使用以下 JSON 使水区域显示为红色、水标签显示为绿色并且陆地区域显示为蓝色：

```json
    {"version":"1.*",
        "settings":{"landColor":"#0000FF"},
        "elements":{"water":{"fillColor":"#FF0000", "labelColor":"#00FF00"}}
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
使用此表“>”字符表示条目层次结构中的级别。   

| 名称                         | 属性组            | 说明    |
|------------------------------|---------------------------|----------------|
| version                      | [Version](#version)       | 要使用的样式表版本。 |
| settings                     | [Settings](#settings)     | 应用于整个样式表的设置。 |
| mapElement                   | [MapElement](#mapelement) | 所有地图条目的父条目。 |
| > baseMapElement             | [MapElement](#mapelement) | 所有非用户条目的父条目。 |
| >> area                      | [MapElement](#mapelement) | 陆地用途的区域（不要与结构条目混淆）。 |
| >>> airport                  | [MapElement](#mapelement) | 包含机场的区域。 |
| >>> areaOfInterest           | [MapElement](#mapelement) | 业务或兴趣点高度集中的区域。 |
| >>> cemetery                 | [MapElement](#mapelement) | 墓地区域。 |
| >>> continent                | [MapElement](#mapelement) | 整个大陆的区域。 |
| >>> education                | [MapElement](#mapelement) |  |
| >>> indigenousPeoplesReserve | [MapElement](#mapelement) |  |
| >>> industrial               | [MapElement](#mapelement) | 用于工业用途的陆地的区域。 |
| >>> island                   | [MapElement](#mapelement) | 岛区域中的标签。 |
| >>> medical                  | [MapElement](#mapelement) | 用于医疗用途的陆地的区域（例如：医院园区）。 |
| >>> military                 | [MapElement](#mapelement) | 军事基地区域。 |
| >>> nautical                 | [MapElement](#mapelement) | 用于航海用途的陆地的区域。 |
| >>> neighborhood             | [MapElement](#mapelement) | 定义为邻居的区域中的标签。 |
| >>> runway                   | [MapElement](#mapelement) | 跑道覆盖的陆地区域。 |
| >>> sand                     | [MapElement](#mapelement) | 沙地区域（如海滩）。 |
| >>> shoppingCenter           | [MapElement](#mapelement) | 为商场或其他购物中心分配的土地区域。 |
| >>> stadium                  | [MapElement](#mapelement) | 体育场区域。 |
| >>> underground              | [MapElement](#mapelement) | 地下区域（例如：地铁站）。 |
| >>> vegetation               | [MapElement](#mapelement) | 森林、草地区域等。 |
| >>>> forest                  | [MapElement](#mapelement) | 森林陆地区域。 |
| >>>> golfCourse              | [MapElement](#mapelement) |  |
| >>>> park                    | [MapElement](#mapelement) | 公园区域。 |
| >>>> playingField            | [MapElement](#mapelement) | 提取的球场，如棒球场或网球场。 |
| >>>> reserve                 | [MapElement](#mapelement) | 自然保护区区域。 |
| >> point                     | [PointStyle](#pointstyle) | 使用某种图标呈现的所有点功能。 |
| >>> address                  | [PointStyle](#pointstyle) | 地址数字。 |
| >>> naturalPoint             | [PointStyle](#pointstyle) |  |
| >>>> peak                    | [PointStyle](#pointstyle) | 表示山峰的图标。 |
| >>>>> volcanicPeak           | [PointStyle](#pointstyle) | 表示火山山峰的图标。 |
| >>>> waterPoint              | [PointStyle](#pointstyle) | 表示水景位置（如瀑布）的图标。 |
| >>> pointOfInterest          | [PointStyle](#pointstyle) | 餐馆、医院、学校、码头、滑雪场等。 |
| >>>> business                | [PointStyle](#pointstyle) | 餐馆、医院、学校等。 |
| >>>>> foodPoint              | [PointStyle](#pointstyle) | 餐馆、咖啡馆等。 |
| >>> populatedPlace           | [PointStyle](#pointstyle) | 表示人口稠密位置（例如：城市或城镇）的大小的图标。 |
| >>>> capital                 | [PointStyle](#pointstyle) | 表示人口稠密地区的中心的图标。 |
| >>>>> adminDistrictCapital   | [PointStyle](#pointstyle) | 表示州或省的省会的图标。 |
| >>>>> countryRegionCapital   | [PointStyle](#pointstyle) | 表示国家或地区的首都/主要城市的图标。 |
| >>> roadShield               | [PointStyle](#pointstyle) | 表示道路的缩写名称的符号。 （例如：I-5）。 如果将设置条目的 **ImageFamily** 属性设置为 *Palette* 值，则仅使用面板值 |
| >>> roadExit                 | [PointStyle](#pointstyle) | 表示出口（通常属于出入管制高速公路）的图标。 |
| >>> transit                  | [PointStyle](#pointstyle) | 表示公共汽车站、火车站、机场等的图标。 |
| >> political                 | [BorderedMapElement](#borderedmapelement) | 政治区域（如国家、地区和州）。 |
| >>> countryRegion            | [BorderedMapElement](#borderedmapelement) |  |
| >>> adminDistrict            | [BorderedMapElement](#borderedmapelement) | Admin1，州、省等。 |
| >>> district                 | [BorderedMapElement](#borderedmapelement) | Admin2，国家/地区等。 |
| >> structure                 | [MapElement](#mapelement) | 建筑物和其他类似于建筑物的结构。 |
| >>> building                 | [MapElement](#mapelement) |  |
| >>>> educationBuilding       | [MapElement](#mapelement) |  |
| >>>> medicalBuilding         | [MapElement](#mapelement) |  |
| >>>> transitBuilding         | [MapElement](#mapelement) |  |
| >> transportation            | [MapElement](#mapelement) | 属于运输网络一部分的线条（例如：公路、火车和轮渡）。 |
| >>> road                     | [MapElement](#mapelement) | 表示所有公路的线条。 |
| >>>> controlledAccessHighway | [MapElement](#mapelement) |  |
| >>>>> highSpeedRamp          | [MapElement](#mapelement) | 表示坡道的线条。 这些坡道通常在出入管制高速公路旁出现。 |
| >>>> highway                 | [MapElement](#mapelement) |  |
| >>>> majorRoad               | [MapElement](#mapelement) |  |
| >>>> arterialRoad            | [MapElement](#mapelement) |  |
| >>>> street                  | [MapElement](#mapelement) |  |
| >>>>> ramp                   | [MapElement](#mapelement) | 表示高速公路的入口和出口的线条。 |
| >>>>> unpavedStreet          | [MapElement](#mapelement) |  |
| >>>> tollRoad                | [MapElement](#mapelement) | 付费使用的公路。 |
| >>> railway                  | [MapElement](#mapelement) | 铁路线。 |
| >>> trail                    | [MapElement](#mapelement) | 穿过公园的步行道或登山步道。 |
| >>> waterRoute               | [MapElement](#mapelement) | 轮渡路线。 |
| >> water                     | [MapElement](#mapelement) | 看起来像水的任何对象。 这包括海洋和溪流。 |
| >>> river                    | [MapElement](#mapelement) | 河流、溪流或其他水路。  轻注意，这可能是线条或多边形，并且可能会连接到非河流水体。 |
| > routeMapElement            | [MapElement](#mapelement) | 所有路线条目都处于此条目下。 |
| >> routeLine                 | [MapElement](#mapelement) | 所有路线的样式。 |
| >>> drivingRoute             | [MapElement](#mapelement) |  |
| >>> scenicRoute              | [MapElement](#mapelement) |  |
| >>> walkingRoute             | [MapElement](#mapelement) |  |
| > userMapElement             | [MapElement](#mapelement) | 所有用户条目都处于此条目下。 |
| >> userBillboard             | [MapElement](#mapelement) | 默认 [MapBillboard](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard) 实例的样式。 |
| >> userLine                  | [MapElement](#mapelement) | 默认 [MapPolyline](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mappolyline) 实例的样式。 |
| >> userModel3D               | [MapElement3D](#mapelement3d) | 默认 [MapModel3D](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapmodel3d) 实例的样式。  这主要用于设置 renderAsSurface。 |
| >> userPoint                 | [PointStyle](#pointstyle) | 默认 [MapIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapicon) 实例的样式。 |


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

| 属性                     | 类型    | 说明                                                                                                                                                                                                                 |
|------------------------------|---------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| atmosphereVisible            | Bool    | 一个标志，指示大气是否在 3D 控件中出现。                                                                                                                                                     |
| buildingTexturesVisible      | Bool    | 指示是否在具有纹理的符号 3D 建筑物上显示纹理的标志。                                                                                                                          |
| fogColor                     | Color   | 在 3D 控件中出现的远距离雾的 ARGB 颜色。                                                                                                                                                    |
| glowColor                    | Color   | 可以应用于标签发光和图标发光的 ARGB 颜色值。                                                                                                                                                     |
| imageFamily                  | String  | 用于此样式的图像集的名称。 对于使用基于实际符号的固定颜色的符号，将此值设置为 *Default*。 对于使用可配置调色板颜色的符号，将此值设置为 *Palette*。 |
| landColor                    | Color   | 在该陆地上绘制任何内容之前，陆地的 ARGB 颜色值。                                                                                                                                                     |
| logosVisible                 | Bool    | 一个标志，指示具有 **Organization** 属性的项目是应绘制相应徽标还是使用通用图标。                                                                                         |
| officialColorVisible         | Bool    | 一个标志，指示具有正式颜色属性的项目（如中国的交通线）是否应绘制该颜色。 例如，为黑白地图关闭此值。                               |
| rasterRegionsVisible         | Bool    | 一个标志，指示是否要绘制我们不按矢量呈现的光栅区域（例如：日本和韩国）。                                                                                                |
| shadedReliefVisible          | Bool    | 一个标志，指示是否要在地图上绘制海拔底纹。                                                                                                                                                  |
| shadedReliefDarkColor        | Color   | 晕渲地貌阴暗面的颜色。  Alpha 通道表示最大 alpha 值。                                                                                                                            |
| shadedReliefLightColor       | Color   | 晕渲地貌光明面的颜色。  Alpha 通道表示最大 alpha 值。                                                                                                                           |
| spaceColor                   | Color   | 地图周围的区域 ARGB 颜色值。                                                                                                                                                                               |
| useDefaultImageColors        | Bool    | 一个标志，指示是否应使用 SVG 中的原始颜色而不是在图像中查找颜色的调色板条目。                                                                                |

<a id="mapelement" />

### <a name="mapelement-properties"></a>MapElement 属性

| 属性                     | 类型    | 描述                                                                                                                       |
|------------------------------|---------|-----------------------------------------------------------------------------------------------------------------------------------|
| backgroundScale              | Float   | 图标的背景元素应缩放的量。  例如，使用 *1* 表示默认值，使用 *2* 表示两倍大。 |
| fillColor                    | Color   | 用于填充多边形、点图标的背景和线条中心（如果已拆分）的颜色。       |
| fontFamily                   | String  |                                                                                                                                   |
| iconColor                    | Color   | 在点图标中心显示的字形的颜色。                                                                       |
| iconScale                    | Float   | 图标的字形应缩放的量。  例如，使用 *1* 表示默认值，使用 *2* 表示两倍大。              |
| labelColor                   | Color   |                                                                                                                                   |
| labelOutlineColor            | Color   |                                                                                                                                   |
| labelScale                   | Float   | 默认标签大小的缩放量。 例如，使用 *1* 表示默认值，使用 *2* 表示两倍大。                  |
| labelVisible                 | Bool    |                                                                                                                                   |
| overwriteColor               | Bool    | 使 **FillColor** 的 alpha 值覆盖 **StrokeColor** 而不是与它混合。                               |
| scale                        | Float   | 整个点的大小的缩放量。 例如，使用 *1* 表示默认值，使用 *2* 表示两倍大。                |
| strokeColor                  | Color   | 要用于多边形周围的轮廓、点图标周围的轮廓和线条颜色的颜色。                         |
| strokeWidthScale             | Float   | 线条笔划的缩放量。 例如，使用 *1* 表示默认值，使用 *2* 表示两倍大。                  |
| visible                      | Bool    |                                                                                                                                   |

<a id="borderedmap" />

### <a name="borderedmapelement"></a>BorderedMapElement

此属性组继承自 [MapElement](#mapelement) 属性组。

| 属性                     | 类型    | 说明                                                           |
|------------------------------|---------|-----------------------------------------------------------------------|
| borderOutlineColor           | Color   | 填色多边形边框的辅助或包围线条颜色。 |
| borderStrokeColor            | Color   | 填色多边形边框的主线条颜色。             |
| borderVisible                | Bool    |                                                                       |
| borderWidthScale             | Float   |                                                                       |

<a id="pointstyle" />

### <a name="pointstyle-properties"></a>PointStyle 属性

此属性组继承自 [MapElement](#mapelement) 属性组。

| 属性                     | 类型    | 描述                                                                                                                      |
|------------------------------|---------|----------------------------------------------------------------------------------------------------------------------------------|
| stemAnchorRadiusScale        | Float   | 图标主干的定位点应缩放的量。  例如，使用 *1* 表示默认值，使用 *2* 表示两倍大。 |
| stemColor                    | Color   | 出自 3D 模式中的图标底部的主干的颜色。                                                           |
| stemHeightScale              | Float   | 图标主干的长度应缩放的量。  例如，使用 *1* 表示默认值，使用 *2* 表示两倍长。 |
| stemWidthScale               | Float   | 图标主干的宽度应缩放的量。  例如，使用 *1* 表示默认值，使用 *2* 表示两倍长。  |
| stemOutlineColor             | Color   | 出自 3D 模式中的图标底部的主干周围的轮廓颜色。                                        |

<a id="mapelement3d" />

### <a name="mapelement3d"></a>MapElement3D

此属性组继承自 [MapElement](#mapelement) 属性组。

| 属性                     | 类型    | 描述                                                                                                                      |
|------------------------------|---------|----------------------------------------------------------------------------------------------------------------------------------|
| renderAsSurface              | Bool    | 指示 3D 模型应象建筑一样呈现的标志 - 无对地面的深度淡入。               |
