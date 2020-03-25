---
description: 地图样式表的条目和属性
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: 地图样式表参考
ms.date: 03/19/2017
ms.topic: article
keywords: windows 10, uwp, 地图, 地图样式表
ms.localizationpriority: medium
ms.openlocfilehash: b2e6e57721a5667a9ca38b21eee2a618353cd30b
ms.sourcegitcommit: c660def841abc742600fbcf6ed98e1f4f7beb8cc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/24/2020
ms.locfileid: "80218557"
---
# <a name="map-style-sheet-reference"></a>地图样式表参考

Microsoft 映射技术使用_地图样式表_来定义地图的外观。  使用 JavaScript 对象表示法（JSON）定义地图样式表，可以通过多种方式使用该样式表，包括 Windows 应用商店应用程序[MapControl](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol)中的[MapStyleSheet. ParseFromJson](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapstylesheet.parsefromjson#Windows_UI_Xaml_Controls_Maps_MapStyleSheet_ParseFromJson_System_String_)方法。

可以使用 "[地图样式表编辑器](https://www.microsoft.com/p/map-style-sheet-editor/9nbhtcjt72ft)" 应用程序以交互方式创建样式表。

以下 JSON 可用于使水源显示为红色，水标签显示为绿色，而土地区显示为蓝色：

```json
    {"version":"1.*",
        "settings":{"landColor":"#0000FF"},
        "elements":{"water":{"fillColor":"#FF0000","labelColor":"#00FF00"}}
    }
```

此 JSON 可用于从映射中删除所有标签和点。

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

本主题介绍可以用于自定义地图外观的 JSON 条目和[属性](#properties)。  还可以通过[MapStyleSheetEntry](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapelement.mapstylesheetentry)属性将这些属性应用于用户地图元素。

<a id="entries" />

## <a name="entries"></a>条目
使用此表“>”字符表示条目层次结构中的级别。  它还显示了哪些版本的 Windows 支持每个条目，哪些版本将忽略它。

| 版本 | Windows 版本名称 |
|---------|----------------------|
|  1703   | Creators 更新      |
|  1709   | Fall Creators Update |
|  1803   | 2018 年 4 月更新    |
|  1809   | 2018年10月更新  |
|  1903   | 2019月更新      |

| 名称                               | 属性组            | 1703 | 1709 | 1803 | 1809 | 1903 | 说明    |
|------------------------------------|---------------------------|------|------|------|------|------|----------------|
| version                            | [版本](#version)       |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 要使用的样式表版本。 |
| 设置                           | [设置](#settings)     |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 应用于整个样式表的设置。 |
| mapElement                         | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 所有地图条目的父条目。 |
| > baseMapElement                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 所有非用户条目的父条目。 |
| >> area                            | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 描述土地使用的区域。  它们不应与结构条目下的物理建筑物混淆。 |
| >>> airport                        | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 包含机场的区域。 |
| >>> areaOfInterest                 | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   |  ✔   | 业务或兴趣点高度集中的区域。 |
| >>> cemetery                       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 包含 cemeteries 的区域。 |
| >>> continent                      | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 大陆区域标签。 |
| >>> education                      | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 包含学校和其他教育设施的领域。 |
| >>> indigenousPeoplesReserve       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 包含 indigenous 的领域。 |
| >>> industrial                     | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   |  ✔   | 用于工业用途的区域。 |
| >>> island                         | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 岛区标签。 |
| >>> medical                        | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 用于医疗目的（例如医院校园）的区域。 |
| >>> military                       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 包含军事基准或具有军事使用的领域。 |
| >>> nautical                       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 用于与海里相关的区域。 |
| >>> neighborhood                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 邻近区域标签。 |
| >>> runway                         | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 用作飞机途径的区域。 |
| >>> sand                           | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 沙地区域（如海滩）。 |
| >>> shoppingCenter                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 为商场或其他购物中心分配的土地区域。 |
| >>> stadium                        | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 包含体育场的区域。 |
| >>> underground                    | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   |  ✔   | 地下区域（例如：地铁站）。 |
| >>> vegetation                     | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 森林、草地区域等。 |
| >>>> forest                        | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 森林陆地区域。 |
| >>>> golfCourse                    | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 包含高尔夫课程的区域。 |
| >>>> park                          | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 包含公园的领域。 |
| >>>> playingField                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 提取的球场，如棒球场或网球场。 |
| >>>> reserve                       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 包含特性性保留的区域。 |
| > > frozenWater                     | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 冻结水，如 glacier。 |
| >> point                           | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 使用某种类型的图标绘制的所有点特征。 |
| >>> address                        | [PointStyle](#pointstyle) |      |      |  ✔   |  ✔   |  ✔   | 地址编号标签。 |
| >>> naturalPoint                   | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 表示自然功能的图标。 |
| >>>> peak                          | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 表示山峰的图标。 |
| >>>>> volcanicPeak                 | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 表示火山山峰的图标。 |
| >>>> waterPoint                    | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 表示水景位置（如瀑布）的图标。 |
| >>> pointOfInterest                | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 表示任何关注位置的图标。 |
| >>>> business                      | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 表示任何业务位置的图标。 |
| > > > > > attractionPoint              | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   |  ✔   | 表示旅游 attractions 的图标，如博物馆、zoos 等。 |
| > > > > > > amusementPlacePoint         | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 娱乐放置图标。 |
| > > > > > > aquariumPoint               | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 水族馆图标。 |
| > > > > > > artGalleryPoint             | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 图片库图标。 |
| > > > > > > campPoint                   | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Camp 图标。 |
| > > > > > > fishingPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 钓鱼图标。 |
| > > > > > > gardenPoint                 | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 花园图标。 |
| > > > > > > hikingPoint                 | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 徒步旅行图标。 |
| > > > > > > libraryPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 库图标。 |
| > > > > > > museumPoint                 | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 博物馆图标。 |
| > > > > > > naturalPlacePoint           | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 自然位置图标。 |
| > > > > > > parkPoint                   | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 寄存图标。 |
| > > > > > > restAreaPoint               | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Rest 区域图标。 |
| > > > > > > touristAttractionPoint      | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 旅游引力图标。 |
| > > > > > > zooPoint                    | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Zoo 图标。 |
| > > > > > communityPoint               | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   |  ✔   | 代表社区一般用途的位置的图标。 |
| > > > > > > conventionCenterPoint       | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 约定中心图标。 |
| > > > > > > financialPoint              | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 财务图标。 |
| > > > > > > governmentPoint             | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 政府图标。 |
| > > > > > > informationTechnologyPoint  | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 信息技术图标。 |
| > > > > > > palacePoint                 | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Palace 图标。 |
| > > > > > > pollingStationPoint         | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | "轮询工作站" 图标。 |
| > > > > > > researchPoint               | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 研究图标。 |
| > > > > > educationPoint               | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   |  ✔   | 表示学校和其他教育相关位置的图标。 |
| > > > > > entertainmentPoint           | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   |  ✔   | 表示娱乐会场的图标，如电影院、cinemas 等。 |
| > > > > > > artsPoint                   | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 美术图标。 |
| > > > > > > bowlingPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 保龄球图标。 |
| > > > > > > casinoPoint                 | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Casino 图标。 |
| > > > > > > golfCoursePoint             | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 高尔夫球场图标。 |
| > > > > > > gymPoint                    | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 健身房图标。 |
| > > > > > > marinaPoint                 | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Marina 图标。 |
| > > > > > > movieTheaterPoint           | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 电影院图标。 |
| > > > > > > nightClubPoint              | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 晚班俱乐部图标。 |
| > > > > > > recreationPoint             | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 重新娱乐图标。 |
| > > > > > > skatingPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Skating 图标。 |
| > > > > > > skiAreaPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 滑雪区域图标。 |
| > > > > > > stadiumPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 游泳池图标。 |
| > > > > > > swimmingPoolPoint           | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 游泳池图标。 |
| > > > > > > theaterPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 剧院图标。 |
| > > > > > > wineryPoint                 | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Winery 图标。 |
| > > > > > essentialServicePoint        | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   |  ✔   | 代表停车场、银行、气体等重要服务的图标 |
| > > > > > > aTMPoint                    | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | ATM 图标。 |
| > > > > > > automobileRentalPoint       | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 汽车租赁图标。 |
| > > > > > > automobileRepairPoint       | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 汽车修理图标。 |
| > > > > > > bankPoint                   | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Bank 图标。 |
| > > > > > > clinicPoint                 | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 诊所图标。 |
| > > > > > > electricChargingStationPoint| [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 电充电工作站图标。 |
| > > > > > > fireStationPoint            | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | FireStation 图标。 |
| > > > > > > gasStationPoint             | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | GasStation 图标。 |
| > > > > > > groceryPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 杂货图标。 |
| > > > > > > hospitalPoint               | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 医院图标。 |
| > > > > > > laundryPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | "总" 图标。 |
| > > > > > > liquorAndWineStorePoint     | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Liquor 和葡萄酒店图标。 |
| > > > > > > mailPoint                   | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 邮件图标。 |
| > > > > > > marketPoint                 | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 市场图标。 |
| > > > > > > parkingPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 停车场图标。 |
| > > > > > > petsPoint                   | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 宠物图标。 |
| > > > > > > pharmacyPoint               | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 药房图标。 |
| > > > > > > policePoint                 | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 警方图标。 |
| > > > > > > postalServicePoint          | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | "邮政服务" 图标。 |
| > > > > > > professionalPoint           | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | "专业服务" 图标。 |
| > > > > > > toiletPoint                 | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 抽水马桶图标。 |
| > > > > > > veterinarianPoint           | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Veterinarian 图标。 |
| >>>>> foodPoint                    | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 表示餐馆、cafés 等的图标。 |
| > > > > > > barAndGrillPoint            | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Bar 和 grill 图标。 |
| > > > > > > barPoint                    | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 栏图标。 |
| > > > > > > breweryPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Brewery 图标。 |
| > > > > > > cafePoint                   | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 咖啡馆图标。 |
| > > > > > > iceCreamShopPoint           | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 冰淇淋店图标。 |
| > > > > > > restaurantPoint             | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 餐馆图标。 |
| > > > > > > teaShopPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | TeaShop 图标。 |
| > > > > > lodgingPoint                 | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   |  ✔   | 代表酒店和其他住宿企业的图标。 |
| > > > > > > gotelPoint                  | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 宾馆图标。 |
| > > > > > realEstatePoint              | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   |  ✔   | 表示房地产企业的图标。 |
| > > > > > shoppingPoint                | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   |  ✔   | 代表酒店和其他住宿企业的图标。 |
| > > > > > > automobileDealerPoint       | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 汽车经销商图标。 |
| > > > > > > beautyAndSpaPoint           | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 美和 spa 图标。 |
| > > > > > > bookstorePoint              | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 书店图标。 |
| > > > > > > departmentStorePoint        | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 部门存储图标。 |
| > > > > > > electronicsShopPoint        | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 电子店图标。 |
| > > > > > > golfShopPoint               | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 高尔夫店图标。 |
| > > > > > > homeApplianceShopPoint      | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 家庭设备商店图标。 |
| > > > > > > mallPoint                   | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 购物中心图标。 |
| > > > > > > phoneShopPoint              | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 电话商店图标。 |
| >>> populatedPlace                 | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 表示人口稠密位置（例如：城市或城镇）的大小的图标。 |
| >>>> capital                       | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 表示人口稠密地区的中心的图标。 |
| >>>>> adminDistrictCapital         | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 表示州或省的省会的图标。 |
| > > > > > > majorAdminDistrictCapital   | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 表示省/市/自治区的主要资本的图标。 |
| > > > > > > minorAdminDistrictCapital   | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 表示省/市/自治区的次要资本的图标。 |
| >>>>> countryRegionCapital         | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 表示国家或地区的首都/主要城市的图标。 |
| > > > > majorPopulatedPlace           | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 表示主要填充位置大小的图标。 |
| > > > > minorPopulatedPlace           | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | 表示次要填充位置大小的图标。 |
| >>> roadShield                     | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 表示道路的缩写名称的符号。 （例如：I-5）。 如果将设置条目的 **ImageFamily** 属性设置为 *Palette* 值，则仅使用面板值 |
| >>> roadExit                       | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 表示出口（通常属于出入管制高速公路）的图标。 |
| >>> transit                        | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 表示公共汽车站、火车站、机场等的图标。 |
| >> political                       | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 政治区域（如国家、地区和州）。 |
| >>> countryRegion                  | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 国家/地区的边框和标签。 |
| >>> adminDistrict                  | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Admin1、状态、省等、边框和标签。 |
| >>> district                       | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 管理员2、县等、边框和标签。 |
| >> structure                       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 建筑物和其他类似于建筑物的结构。 |
| >>> building                       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 建筑. |
| >>>> educationBuilding             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 用于教育的大楼。 |
| >>>> medicalBuilding               | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 用于医疗目的的大楼，如医院。 |
| >>>> transitBuilding               | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 用于运输的大楼，如机场。 |
| >> transportation                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 属于运输网络一部分的线条（例如：公路、火车和轮渡）。 |
| >>> road                           | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 表示所有公路的线条。 |
| >>>> controlledAccessHighway       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 表示大型受控访问高速公路的行。 |
| >>>>> highSpeedRamp                | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 表示通常连接到受控访问高速公路的高速斜坡的线条。 |
| >>>> highway                       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 表示高速公路的行。 |
| >>>> majorRoad                     | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 表示主要道路的线条。 |
| >>>> arterialRoad                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 表示 arterial 道路的线条。 |
| >>>> street                        | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 表示街道的行。 |
| >>>>> ramp                         | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 表示通常连接到高速公路的斜坡的线条。 |
| >>>>> unpavedStreet                | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 表示 unpaved 大街的线条。 |
| >>>> tollRoad                      | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 表示需要使用的公路的线条。 |
| >>> railway                        | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 铁路线。 |
| >>> trail                          | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 穿过公园的步行道或登山步道。 |
| > > > walkway                        | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   |  ✔   | 提升的 walkway。 |
| >>> waterRoute                     | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 轮渡路线。 |
| >> water                           | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 看起来像水的任何对象。 这包括海洋和溪流。 |
| >>> river                          | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 河流、溪流或其他水路。  轻注意，这可能是线条或多边形，并且可能会连接到非河流水体。 |
| > routeMapElement                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 所有与路由相关的条目。 |
| >> routeLine                       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 与路线行相关的条目。 |
| >>> drivingRoute                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 表示驱动路由的行。 |
| >>> scenicRoute                    | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   |  ✔   | 表示 scenic 驱动路线的线条。 |
| >>> walkingRoute                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 表示行走路由的行。 |
| > userMapElement                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 所有用户条目。 |
| >> userBillboard                   | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   |  ✔   | 默认 [MapBillboard](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard) 实例的样式。 |
| >> userLine                        | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 默认 [MapPolyline](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mappolyline) 实例的样式。 |
| >> userModel3D                     | [MapElement3D](#mapelement3d) |      |  ✔   |  ✔   |  ✔   |  ✔   | 默认 [MapModel3D](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapmodel3d) 实例的样式。  这主要用于设置 renderAsSurface。 |
| >> userPoint                       | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 默认 [MapIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapicon) 实例的样式。 |

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

| 属性                     | 类型    | 1703 | 1709 | 1803 | 1809 | 1903 | 说明 |
|------------------------------|---------|------|------|------|------|------|-------------|
| atmosphereVisible            | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 一个标志，指示大气是否在 3D 控件中出现。 |
| buildingTexturesVisible      | Bool    |      |      |  ✔   |  ✔   |  ✔   | 指示是否在具有纹理的符号 3D 建筑物上显示纹理的标志。 |
| fogColor                     | 颜色   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 在 3D 控件中出现的远距离雾的 ARGB 颜色。 |
| glowColor                    | 颜色   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 可以应用于标签发光和图标发光的 ARGB 颜色值。 |
| imageFamily                  | String  |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 用于此样式的图像集的名称。 对于使用基于实际符号的固定颜色的符号，将此值设置为 *Default*。 对于使用可配置调色板颜色的符号，将此值设置为 *Palette*。 |
| landColor                    | 颜色   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 在该陆地上绘制任何内容之前，陆地的 ARGB 颜色值。 |
| logosVisible                 | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 一个标志，指示具有 **Organization** 属性的项目是应绘制相应徽标还是使用通用图标。 |
| officialColorVisible         | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 一个标志，指示具有正式颜色属性的项目（如中国的交通线）是否应绘制该颜色。 例如，为黑白地图关闭此值。 |
| rasterRegionsVisible         | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 一个标志，该标志指示是否绘制与矢量（日本和韩国）的表示形式比向量更好的光栅区。 |
| shadedReliefVisible          | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 一个标志，指示是否要在地图上绘制海拔底纹。 |
| shadowColor                  | 颜色   |      |      |      |  ✔   |  ✔   | 使用阴影的图标后面的阴影颜色。 |
| spaceColor                   | 颜色   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 地图周围的区域 ARGB 颜色值。 |
| terrainFlat                  | Bool    |      |      |      |      |      | 一个标志，用于指示地形是否应为地图上的平面（禁用）。 |
| useDefaultImageColors        | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 一个标志，用于指示是否应使用 SVG 中的原始颜色，而不是在调色板项中查找图像中的颜色。 |

<a id="mapelement" />

### <a name="mapelement-properties"></a>MapElement 属性

| 属性                     | 类型    | 1703 | 1709 | 1803 | 1809 | 1903 | 说明 |
|------------------------------|---------|------|------|------|------|------|-------------|
| backgroundScale              | 浮点   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 图标的背景元素应缩放的量。  例如，使用 *1* 表示默认值，使用 *2* 表示两倍大。 |
| fillColor                    | 颜色   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 用于填充多边形、点图标的背景和线条中心（如果已拆分）的颜色。 |
| fontFamily                   | String  |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   |  |
| fontWeight                   | String  |      |      |      |      |  ✔   | 字样的密度，以笔划的亮度或笔画为依据。 可以设置 "**Light**"、"**Normal**"、"**SemiBold**" 和 "**Bold**" |
| iconColor                    | 颜色   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 在点图标中心显示的字形的颜色。 |
| iconScale                    | 浮点   |      |  ✔   |  ✔   |  ✔   |  ✔   | 图标的字形应缩放的量。  例如，使用 *1* 表示默认值，使用 *2* 表示两倍大。 |
| labelColor                   | 颜色   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   |  |
| labelOutlineColor            | 颜色   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   |  |
| labelScale                   | 浮点   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 默认标签大小的缩放量。 例如，使用 *1* 表示默认值，使用 *2* 表示两倍大。 |
| labelVisible                 | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   |  |
| overwriteColor               | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 使 **FillColor** 的 alpha 值覆盖 **StrokeColor** 而不是与它混合。 |
| 小数位数                        | 浮点   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 整个点的大小的缩放量。 例如，使用 *1* 表示默认值，使用 *2* 表示两倍大。 |
| strokeColor                  | 颜色   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 要用于多边形周围的轮廓、点图标周围的轮廓和线条颜色的颜色。 |
| strokeWidthScale             | 浮点   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 线条笔划的缩放量。 例如，使用 *1* 表示默认值，使用 *2* 表示两倍大。 |
| visible                      | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   |  |

<a id="borderedmap" />

### <a name="borderedmapelement"></a>BorderedMapElement

此属性组继承自 [MapElement](#mapelement) 属性组。

| 属性                     | 类型    | 1703 | 1709 | 1803 | 1809 | 1903 | 说明 |
|------------------------------|---------|------|------|------|------|------|-------------|
| borderOutlineColor           | 颜色   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 填色多边形边框的辅助或包围线条颜色。 |
| borderStrokeColor            | 颜色   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 填色多边形边框的主线条颜色。 |
| borderVisible                | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   |  |
| borderWidthScale             | 浮点   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 边框描边的缩放量。 例如，使用 *1* 表示默认值，使用 *2* 表示两倍大。 |

<a id="pointstyle" />

### <a name="pointstyle-properties"></a>PointStyle 属性

此属性组继承自 [MapElement](#mapelement) 属性组。

| 属性                     | 类型    | 1703 | 1709 | 1803 | 1809 | 1903 | 说明 |
|------------------------------|---------|------|------|------|------|------|-------------|
| shadowVisible                | Bool    |      |      |      |      |  ✔   | 指示图标阴影是否可见的标志 |
| 形状-背景             | String  |      |      |      |      |  ✔   | 用作图标背景的形状-替换该处存在的任何形状。 |
| 形状-图标                   | String  |      |      |      |      |  ✔   | 用作图标前景标志符号的形状-替换该处存在的任何形状。 |
| stemAnchorRadiusScale        | 浮点   |      |      |  ✔   |  ✔   |  ✔   | 图标主干的定位点应缩放的量。  例如，使用 *1* 表示默认值，使用 *2* 表示两倍大。 |
| stemColor                    | 颜色   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 出自 3D 模式中的图标底部的主干的颜色。 |
| stemHeightScale              | 浮点   |      |      |  ✔   |  ✔   |  ✔   | 图标主干的长度应缩放的量。  例如，使用 *1* 表示默认值，使用 *2* 表示两倍长。 |
| stemOutlineColor             | 颜色   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 出自 3D 模式中的图标底部的主干周围的轮廓颜色。 |
| stemWidthScale               | 浮点   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | 图标主干的宽度应缩放的量。  例如，使用 *1* 表示默认值，使用 *2* 表示两倍长。 |

<a id="mapelement3d" />

### <a name="mapelement3d"></a>MapElement3D

此属性组继承自 [MapElement](#mapelement) 属性组。

| 属性                     | 类型    | 1703 | 1709 | 1803 | 1809 | 1903 | 说明 |
|------------------------------|---------|------|------|------|------|------|------------|
| renderAsSurface              | Bool    |      |      |  ✔   |  ✔   |  ✔   | 指示 3D 模型应象建筑一样呈现的标志 - 无对地面的深度淡入。 |
