---
任何应用的主要用途都是提供对内容的访问权限。 在照片编辑应用中，照片即是内容；在旅行应用中，地图和旅行目的地信息即是内容；等等。
通用 Windows 平台 (UWP) 应用的内容设计基础知识
ms.assetid: 3102530A-E0D1-4C55-AEFF-99443D39D567
内容设计基础知识
template: detail.hbs
---

#  UWP 应用的内容设计基础知识


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


任何应用的主要用途都是提供对内容的访问权限：在照片编辑应用中，照片即是内容；在旅行应用中，地图和旅行目的地信息即是内容；等等。 导航元素提供对内容的访问权限；命令元素使用户能够与内容进行交互；内容元素显示实际内容。

本文提供了适用于三种内容方案的内容设计建议。

## <span id="Design_for_the_right_content_scenario"> </span> <span id="design_for_the_right_content_scenario"> </span> <span id="DESIGN_FOR_THE_RIGHT_CONTENT_SCENARIO"> </span>针对正确的内容方案进行设计


存在三种主要的内容方案：

-   **使用**：使用内容的主要单向体验。 它包括阅读、听音乐、观看视频以及照片和图像查看之类的任务。
-   **创建**：侧重于创建新内容的主要单向体验。 它可细分为从无到有创建内容，如拍摄照片或视频、使用绘图应用创建新图像或打开一个全新文档。
-   **交互式**：包括使用、创建和修改内容的双向内容体验。

## <span id="Consumption-focused_apps"> </span> <span id="consumption-focused_apps"> </span> <span id="CONSUMPTION-FOCUSED_APPS"> </span>侧重于使用的应用


在侧重于使用的应用中，内容元素获得最高优先级，后跟所需的[导航元素](navigation-basics.md)，从而帮助用户查找他们所需的内容。 侧重于使用的应用示例包括电影播放器、阅读应用、音乐应用和照片查看器。

![新闻阅读器应用](images/news-reader/v2/newsreader-v2-tablet-phone.png)

针对侧重于使用的应用的一般建议：

-   请考虑创建专用[导航](navigation-basics.md)页面和内容查看页面，以便当用户找到他们要查找的内容时，可以在专用页面上不受干扰地查看内容。
-   请考虑创建一个全屏视图选项，用于扩展内容以填充整个屏幕并隐藏其他所有 UI 元素。

## <span id="Creation-focused_apps"> </span> <span id="creation-focused_apps"> </span> <span id="CREATION-FOCUSED_APPS"> </span>侧重于创建的应用


在侧重于创建的应用中，内容和[命令](commanding-basics.md)元素都是最重要的 UI 元素：命令元素使用户能够创建新的内容。 示例包括绘图应用、照片编辑应用、视频编辑应用和文字处理应用。

例如，下面是一个照片应用的设计，该应用使用命令栏来访问工具和照片操作选项。 因为所有命令都位于命令栏中，所以该应用可将其大部分屏幕空间都分配给其内容以及要编辑的照片。

![使用活动画布的照片编辑应用的设计示例](images/photo-editor/uap-photo-tabletphone-sbs.png)

针对侧重于创建的应用的一般建议：

-   尽量减少[导航](navigation-basics.md)元素的使用。
-   [命令](commanding-basics.md)元素在侧重于创建的应用中尤为重要。 由于用户将会执行大量的命令，我们建议提供命令历史记录/撤消功能。

## <span id="Apps_with_interactive_content"> </span> <span id="apps_with_interactive_content"> </span> <span id="APPS_WITH_INTERACTIVE_CONTENT"> </span>具有交互式内容的应用


在具有交互式内容的应用中，用户将创建、查看和编辑内容；许多应用都属于此类别。 这些类型的应用的示例包括：业务线应用、库存管理应用，以及使用户能够创建或修改食谱的烹饪应用。

![协作工具设计，具有交互式内容的应用](images/collaboration-tool/uap-collaboration-tabphone-700.png)

这些种类的应用需要使所有三种 UI 元素保持平衡：

-   [导航](navigation-basics.md)元素帮助用户查找和查看内容。 如果查看和查找内容都是最重要的应用场景，则优先考虑导航元素、筛选和排序以及搜索。
-   [命令](commanding-basics.md)元素帮助用户创建、编辑和处理内容。

针对具有交互式内容的应用的一般建议：

-   当导航、内容和命令元素都非常重要时，可能很难使这三种元素保持平衡。 如果可能，请考虑创建单独的屏幕，以用于浏览、创建和编辑内容，或提供模式切换。

## <span id="Commonly_used_content_elements"> </span> <span id="commonly_used_content_elements"> </span> <span id="COMMONLY_USED_CONTENT_ELEMENTS"> </span>常用的内容元素


以下是常用于显示内容的一些 UI 元素。 （有关 UI 元素的完整列表，请参阅[控件和 UI 元素](https://msdn.microsoft.com/library/windows/apps/dn611856)。）

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">类别</th>
<th align="left">元素</th>
<th align="left">说明</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">音频和视频</td>
<td align="left">[Media playback and transport controls](../controls-and-patterns/media-playback.md)</td>
<td align="left">播放音频和视频。</td>
</tr>
<tr class="even">
<td align="left">图像查看器</td>
<td align="left">[
            Flip view](../controls-and-patterns/flipview.md), [image](../controls-and-patterns/images-imagebrushes.md)</td>
<td align="left">显示图像。 翻转视图用于显示集锦中的图像（例如相册中的照片或产品详细信息页中的项目），一次显示一张图像。</td>
</tr>
<tr class="odd">
<td align="left">列表</td>
<td align="left">[drop-down list, list box, list view and grid view](../controls-and-patterns/lists.md)</td>
<td align="left">显示交互式列表或网格中的项。 使用这些元素以使用户可以从新发行电影列表中选择一部电影或管理清单。</td>
</tr>
<tr class="even">
<td align="left">文本和文本输入</td>
<td align="left"><p>[
            Text block](../controls-and-patterns/text-block.md), [text box](../controls-and-patterns/text-box.md), [rich edit box](../controls-and-patterns/rich-edit-box.md)</p>
</td>
<td align="left">显示文本。 某些元素使用户能够编辑文本。 有关详细信息，请参阅：[Text controls](../controls-and-patterns/text-controls.md)</td>
</tr>
</tbody>
</table>

 

\[此文章包含特定于通用 Windows 平台 (UWP) 应用和 Windows 10 的信息。 有关 Windows 8.1 指南，请下载 [Windows 8.1 指南 PDF](https://go.microsoft.com/fwlink/p/?linkid=258743)。\]

 

 






<!--HONumber=Mar16_HO1-->


