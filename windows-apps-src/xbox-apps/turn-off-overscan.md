---
author: payzer
title: "如何关闭过度扫描"
description: 
area: Xbox
translationtype: Human Translation
ms.sourcegitcommit: 32a875348debac9aec9f5a26bc4e7e0af2a0a5b4
ms.openlocfilehash: abd06e78364ff32cc10d733e33b153b854dbc467

---

# 如何将 UI 绘制到屏幕的边缘   
默认情况下，应用程序会将边框置于视口的边缘。 这是要考虑使用电视安全区域。 有关详细信息，请参阅[针对 Xbox 和电视进行设计](http://go.microsoft.com/fwlink/?LinkID=760736#tv-safe-area)。  我们建议关闭此功能并绘制到屏幕的边缘。 通过添加以下代码，可以在你的应用程序启动时绘制到屏幕的边缘：
   
`Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().SetDesiredBoundsMode(Windows.UI.ViewManagement.ApplicationViewBoundsMode.UseCoreWindow);`
   
注意：C++/DirectX 应用程序无需担心这一问题。 系统始终会将你的应用程序呈现到屏幕的边缘。



<!--HONumber=Jun16_HO5-->


