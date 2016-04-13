---
地图控件可以显示道路地图和鸟瞰图、路线、搜索结果和交通。
地图指南
ms.assetid: 7B5B6BC9-D1EC-4978-8876-20B78EF44797
---

# 地图指南


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


地图控件可以显示道路地图、鸟瞰图、3D、视图、路线、搜索结果和交通。 在地图上，你可以显示用户的位置、路线和目标点。 地图还可以显示 3D 鸟瞰图、街景视图、交通、公交和本地企业。

![地图示例，基本视图](./images/win10fa/controls-maps-basic.jpg)

## 这是正确的控件吗？


当你希望应用内具有一个允许用户查看特定于应用或通用地理信息的地图时，请使用地图控件。 在应用中有一个地图控件意味着用户无需为了获取该信息而离开应用。

**注意** 如果你不介意用户离开应用，请考虑使用 Windows 地图应用提供该信息。 你的应用可以启动 Windows 地图应用来显示特定的地图、路线和搜索结果。 有关详细信息，请参阅[启动 Windows 地图应用](https://msdn.microsoft.com/library/windows/apps/mt228341)。

## 示例


此示例显示具有街景视图的地图：

![地图控件街景视图示例](./images/win10fa/controls-maps-streetside.jpg)

 

此示例显示具有鸟瞰 3D 视图的地图：

![地图控件 3D 视图示例](./images/win10fa/controls-maps-3dview.jpg)

 

此示例显示同时具有鸟瞰 3D 视图和街景视图的应用：

![具有街景视图的 3D 地图视图示例](./images/win10fa/controls-maps-3dstreetview.png)


## 建议


-   使用充足的屏幕空间（或整个屏幕）显示地图，以便用户无需过度平移和缩放来查看地理信息。

-   如果地图仅用于呈现静态的、信息性的视图，则使用较小的地图可能更合适。 如果你选择较小的静态地图，请根据可用性设置其维度：小到足够节省屏幕空间，大到足够保持清晰。

-   使用 [**map elements**](https://msdn.microsoft.com/library/windows/apps/dn637034) 在地图场景中嵌入目标点；任何其他信息都可作为覆盖地图场景的瞬态 UI 显示。

## 相关主题


* [使用 2D、3D 和街景视图方式显示地图](https://msdn.microsoft.com/library/windows/apps/mt219695)
* [在地图上显示目标点 (POI)](https://msdn.microsoft.com/library/windows/apps/mt219696)
* [必应地图开发人员中心](https://www.bingmapsportal.com/)
* [UWP 地图示例](http://go.microsoft.com/fwlink/p/?LinkId=619977)
* [//Build 2015 视频：在 Windows 应用中跨手机、平板电脑和 PC 利用地图和位置](https://channel9.msdn.com/Events/Build/2015/2-757)
* [启动 Windows 地图应用](https://msdn.microsoft.com/library/windows/apps/mt228341)
 

 






<!--HONumber=Mar16_HO1-->


