---
description: 地图样式表的条目和属性
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: 地图样式表参考
ms.date: 03/19/2017
ms.topic: article
keywords: windows 10, uwp, 地图, 地图样式表
ms.localizationpriority: medium
ms.openlocfilehash: ec30fd5d63dfa522ef721d2d8a2e4950e6e8e854
ms.sourcegitcommit: c9edb164356c0843d8a9b19ab43707d486e392e1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/25/2020
ms.locfileid: "85377928"
---
# <a name="map-style-sheet-reference"></a>地图样式表参考

Microsoft 映射技术使用[地图样式表](https://docs.microsoft.com/BingMaps/styling/map-style-sheets)来定义地图的外观，包括在 Windows 应用商店应用程序的[MapControl](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol)中显示的。  地图样式表是通过[MapStyleSheet. ParseFromJson](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapstylesheet.parsefromjson#Windows_UI_Xaml_Controls_Maps_MapStyleSheet_ParseFromJson_System_String_)方法使用 JAVASCRIPT 对象表示法（JSON）定义的。

样式表包含其属性被重写以更改地图最终外观的[条目](https://docs.microsoft.com/BingMaps/styling/map-style-sheet-entries)。

例如，可以使用以下 JSON 使水源显示为红色，水标签显示为绿色，而土地区显示为蓝色：

```json
    {"version":"1.*",
        "settings":{"landColor":"#0000FF"},
        "elements":{"water":{"fillColor":"#FF0000","labelColor":"#00FF00"}}
    }
```

可以使用 "[地图样式表编辑器](https://www.microsoft.com/p/map-style-sheet-editor/9nbhtcjt72ft)" 应用程序以交互方式创建样式表。
