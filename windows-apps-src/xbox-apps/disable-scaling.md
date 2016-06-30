---
author: payzer
title: "如何关闭缩放"
description: 
area: Xbox
ms.sourcegitcommit: 2fcccb9a045aad268afde615d31f8faa002b8a87
ms.openlocfilehash: 65416dd2b6c8656078b63c316f3972cda9c792fc

---

# 如何关闭缩放   
默认情况下，应用程序会缩放到 200%（适用于 XAML）和 150%（适用于 HTML 应用）。 可以关闭默认比例系数。 这将导致你的应用程序使用设备的实际像素尺寸（1910 x 1080 像素）。   
   
## HTML   
你可以通过使用以下代码片段选择退出比例系数： 
   
`bool result = Windows.UI.ViewManagement.ApplicationViewScaling.TrySetDisableLayoutScaling(true);` 

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
   
`bool result = Windows.UI.ViewManagement.ApplicationViewScaling.TrySetDisableLayoutScaling(true);`   
   
## DirectX/C++   
不可缩放 DirectX/C++ 应用程序。 自动缩放仅适用于 HTML 和 XAML 应用程序。   



<!--HONumber=Jun16_HO4-->


