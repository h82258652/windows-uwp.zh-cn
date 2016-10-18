---
author: payzer
title: "如何关闭缩放"
description: "关闭默认比例系数说明"
translationtype: Human Translation
ms.sourcegitcommit: 582f5677c15f7cd62c398103b48743ba4bea6c5b
ms.openlocfilehash: 8079be9685558277565766fa8d0ebbfd4a555904

---

# 如何关闭缩放   
默认情况下，应用程序会缩放到 200%（适用于 XAML）和 150%（适用于 HTML 应用）。 可以关闭默认比例系数。 这将导致应用程序使用设备的实际像素尺寸（1910 x 1080 像素）。   
   
## HTML   
你可以通过使用以下代码片段选择退出比例系数： 
   
```
var result = Windows.UI.ViewManagement.ApplicationViewScaling.trySetDisableLayoutScaling(true);
```

或者，你可以使用 Web 友好方法：   

```   
@media (max-height: 1080px) {   
    @-ms-viewport {   
        height: 1080px;   
    }   
}   
```

## XAML
你可以通过使用以下代码片段选择退出比例系数：   
   
```
bool result = Windows.UI.ViewManagement.ApplicationViewScaling.TrySetDisableLayoutScaling(true);
```
   
## DirectX/C++   
不可缩放 DirectX/C++ 应用程序。 自动缩放仅适用于 HTML 和 XAML 应用程序。  

## 另请参阅
- [适用于 Xbox 的最佳做法](tailoring-for-xbox.md)
- [Xbox One 上的 UWP](index.md)



<!--HONumber=Aug16_HO3-->


