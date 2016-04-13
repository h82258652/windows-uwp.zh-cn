---
Description: 使用横向滑动来支持使用轻扫手势进行选择以及使用滑动手势拖动（移动）交互。
title: 横向滑动指南
ms.assetid: 897555e2-c567-4bbe-b600-553daeb223d5
label: 横向滑动
template: detail.hbs
---

# 十字滑块指南


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**重要的 API**

-   [**CrossSliding**](https://msdn.microsoft.com/library/windows/apps/br241942)
-   [**CrossSlideThresholds**](https://msdn.microsoft.com/library/windows/apps/br241941)
-   [**Windows.UI.Input**](https://msdn.microsoft.com/library/windows/apps/br242084)

使用横向滑动来支持使用轻扫手势进行选择以及使用滑动手势拖动（移动）交互。

## <span id="Dos_and_don_ts"> </span> <span id="dos_and_don_ts"> </span> <span id="DOS_AND_DON_TS"> </span>注意事项


-   对在单个方向上滚动的列表或集合使用横向滑动。
-   当点击交互用于其他目的时，可为项目选择使用横向滑动。
-   请勿使用横向滑动将项目添加到队列。

## <span id="Additional_usage_guidance"> </span> <span id="additional_usage_guidance"> </span> <span id="ADDITIONAL_USAGE_GUIDANCE"> </span>其他用法指南


仅在可以沿一个方向（垂直或水平）平移的内容区域内进行选择和拖动。 要使任何交互发挥效果，必须锁定一个平移方向，并且必须在垂直于该平移方向的方向上执行手势。

下面我们将展示使用横向滑动选择和拖动对象。 左侧的图像显示了如果在抬起接触点并释放对象之前轻扫手势未超过距离阈值，如何选择对象。 右侧的图像显示滑动手势超过了距离阈值并导致对象被拖动。

![显示选择和拖放过程的图表。](images/crossslide-mechanism.png)

下图中显示了横向滑动交互使用的阈值距离。

![显示选择和拖放过程的屏幕截图。](images/crossslide-threshold.png)

若要保留平移功能，必须超过 2.7mm 的小阈值（目标分辨率约为 10 个像素），然后才能激活选择或拖动交互。 这个小阈值可帮助系统区分横向滑动和平移，而且还帮助确保将点击手势与横向滑动和平移区分开来。

此图像显示了用户如何触摸 UI 中的元素，但在接触时稍微向下移动他们的手指。 如果没有阈值，则会由于开始的垂直移动，此交互将被视为横向滑动。 有了阈值，系统才会将此次移动正确视为水平平移。

![显示选择或拖放消除歧义阈值的屏幕截图。](images/crossslide-threshold2.png)

如果应用中包括横向滑动功能，请考虑以下指南。

对在单个方向上滚动的列表或集合使用横向滑动。 有关详细信息，请参阅[添加 ListView 控件](https://msdn.microsoft.com/library/windows/apps/hh465382)。

**注意** 如果内容区域可以在两个方向上平移（如 Web 浏览器或电子阅读器），则长按计时交互应该用于调用诸如图像和超链接之类的对象的上下文菜单。

 

|                                                                                         |                                                                                         |
|-----------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------|
| ![水平平移，二维列表](images/groupedlistview1.png)                | ![垂直平移，一维列表](images/listviewlistlayout.png)                |
| 水平平移二维列表。 垂直拖动以选择或移动项。 | 垂直平移一维列表。 水平拖动以选择或移动项。 |

 

### <span id="selection"></span><span id="SELECTION"></span>

**选择**

选择就是标记一个或多个对象，而不是启动或激活。 该操作类似于鼠标单击一个或多个对象，或按住 Shift 键的同时鼠标单击这些对象。

通过触摸元素并在短时间拖动交互之后释放该元素可实现横向滑动选择。 这种选择方法无需专用的选择模式以及其他触摸界面所需的长按计时交互，并且与用于激活的点击交互不存在冲突。

除了距离阈值之外，横向滑动选择还受 90° 阈值区域所限，如下图所示。 如果对象被拖动到此区域之外，则不会选择该对象。

![显示选择阈值区域的图表。](images/crossslide-selection.png)

横向滑动交互由长按计时交互进行了补充，也称为“自我显示”交互。 此补充的交互会激活一个动画，显示可在对象上执行的操作。 有关消除歧义 UI 的详细信息，请参阅[视觉反馈指南](guidelines-for-visualfeedback.md)。

以下屏幕截图演示了自我显示动画的工作方式。

1.  长按以启动用于自我显示交互的动画。 该项目的选中状态会影响动画显示的内容：如果未选中，则显示复选标记；如果已选中，则不显示复选标记。

    ![显示已取消选择状态的屏幕截图。](images/crossslide-selfreveal1.png)

2.  使用轻扫手势（向上或向下）选中项目。

    ![显示可供选择的动画的屏幕截图。](images/crossslide-selfreveal2.png)

3.  现在已选中项目。 使用滑动手势替代选择行为以移动该项目。

    ![显示可供拖放的动画的屏幕截图。](images/crossslide-selfreveal3.png)

在将单击用作唯一主动作的应用中，使用单击进行选择。 显示横向滑动自我显示动画，以将该功能与用于激活和导航的标准点击交互区分开来。

**选择篮**

选择篮是已从应用程序的主列表或集合中选择的项的明确动态展示。 此功能对于跟踪所选项非常有用，应由下列应用程序使用：

-   可从多个位置选择项的应用。
-   可选择多个项的应用。
-   操作或命令取决于选择列表的应用。

跨操作和命令时，选择篮中的内容保持不变。 例如，如果你从库中选择一系列照片，对每张照片进行颜色校正，并以某种方式共享这些照片，则这些照片仍然处于选中状态。

如果在应用程序中未使用选择篮，则某个操作或命令之后应该清除当前选择。 例如，如果你从播放列表中选择了一首歌曲并进行评分，则应该清除该选择。

当未使用选择篮并且列表或集合中有其他项已被激活时，也应该清除当前选择。 例如，如果你选择某封收件箱邮件，则预览窗格会更新。 如果你选择另一封收件箱邮件，则会取消之前邮件的选择并且更新预览窗格。

**队列**

队列不等于选择篮列表，也不应作为此类选择篮列表对待。 它们的主要区别包括：

-   选择篮中的项列表只是视觉上的展示，而队列中的项则是通过特定的操作组合在一起。
-   在选择篮中，项只能展示一次，而在队列中可以展示多次。
-   选择篮中的项顺序代表选择的顺序。 队列中的项顺序则与功能直接相关。

因此，不应该使用横向滑动选择交互向队列中添加项， 而是应该通过拖动操作向队列中添加项。

### <span id="draganddrop"></span><span id="DRAGANDDROP"></span>

**拖动**

使用拖动操作将一个或多个对象从一个位置移动到另一个位置。

如果需要移动多个对象，则让用户选择多个项，然后同时拖动所有选中项。

## <span id="related_topics"> </span>相关文章


**示例**
* [基本输入示例](http://go.microsoft.com/fwlink/p/?LinkID=620302)
* [低延迟输入示例](http://go.microsoft.com/fwlink/p/?LinkID=620304)
* [用户交互模式示例](http://go.microsoft.com/fwlink/p/?LinkID=619894)
* [焦点视觉示例](http://go.microsoft.com/fwlink/p/?LinkID=619895)
**存档示例**
* [输入：XAML 用户输入事件示例](http://go.microsoft.com/fwlink/p/?linkid=226855)
* [输入：设备功能示例](http://go.microsoft.com/fwlink/p/?linkid=231530)
* [输入：触摸点击测试示例](http://go.microsoft.com/fwlink/p/?linkid=231590)
* [XAML 滚动、平移以及缩放示例](http://go.microsoft.com/fwlink/p/?linkid=251717)
* [输入：简化的墨迹示例](http://go.microsoft.com/fwlink/p/?linkid=246570)
* [输入：Windows 8 手势示例](http://go.microsoft.com/fwlink/p/?LinkId=264995)
* [输入：操作和手势 (C++) 示例](http://go.microsoft.com/fwlink/p/?linkid=231605)
* [DirectX 触控输入示例](http://go.microsoft.com/fwlink/p/?LinkID=231627)
 

 






<!--HONumber=Mar16_HO1-->


