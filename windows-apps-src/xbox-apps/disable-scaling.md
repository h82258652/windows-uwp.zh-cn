---
author: payzer
title: 如何关闭缩放
description: 关闭默认比例系数说明
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 6e68c1fc-a407-4c0b-b0f4-e445ccb72ff3
ms.localizationpriority: medium
ms.openlocfilehash: 82b42b25d3894a82e92af9a520ee5f951a5ba344
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/26/2018
ms.locfileid: "5684752"
---
# <a name="how-to-turn-off-scaling"></a>如何关闭缩放   
默认情况下，应用程序会缩放到 200%（适用于 XAML）和 150%（适用于 HTML 应用）。 可以关闭默认比例系数。 这将导致应用程序使用设备的实际像素尺寸（1910 x 1080 像素）。   
   
## <a name="html"></a>HTML   
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

## <a name="xaml"></a>XAML
你可以通过使用以下代码片段选择退出比例系数：   
   
```
bool result = Windows.UI.ViewManagement.ApplicationViewScaling.TrySetDisableLayoutScaling(true);
```
   
## <a name="directxc"></a>DirectX/C++   
不可缩放 DirectX/C++ 应用程序。 自动缩放仅适用于 HTML 和 XAML 应用程序。  

## <a name="see-also"></a>另请参阅
- [适用于 Xbox 的最佳做法](tailoring-for-xbox.md)
- [Xbox One 上的 UWP](index.md)
