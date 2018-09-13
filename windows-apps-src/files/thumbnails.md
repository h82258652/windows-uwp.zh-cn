---
author: QuinnRadich
Description: How to use thumbnail images to help users preview files in UWP apps.
title: UWP 应用中的缩略图图像指南
label: Thumbnail images
template: detail.hbs
ms.author: quradic
ms.date: 01/08/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: df1eec58d936ba4f03e1eadae534abf0620b1a39
ms.sourcegitcommit: c8f6866100a4b38fdda8394ea185b02d7af66411
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/13/2018
ms.locfileid: "3958001"
---
# <a name="thumbnail-images"></a>缩略图图像

这些指南介绍如何使用缩略图图像帮助用户在 UWP 应用中浏览时预览文件。 

> **重要 API**：[ThumbnailMode 枚举](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.thumbnailmode)

## <a name="should-my-app-include-thumbnails"></a>我的应用应该包括缩略图吗？

如果应用允许用户浏览文件，则可以显示缩略图图像以帮助用户快速预览这些文件。 

使用缩略图的情况： 
- 显示库集合（例如文件和文件夹）中多个项目的预览。 例如，照片库应使用缩略图，以在用户浏览照片文件时为用户提供每张照片的小视图。

    ![视频库](images/thumbnail-gallery.png)

- 显示列表中某单个项目（如某特定文件）的预览。 例如，用户在决定是否打开某个文件之前，可能希望查看有关该文件的详细信息（包括更大的缩略图以提供更好的预览效果）。 

    ![视频预览](images/thumbnail-preview.png)

## <a name="dos-and-donts"></a>应做事项和禁止事项
- 在检索缩略图时，指定[缩略图模式](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.thumbnailmode)（PicturesView、VideosView、DocumentsView、MusicView、ListView 或 SingleItem）。 这可确保缩略图图像经过优化，显示用户想要查看的文件类型。 
    - 使用 SingleItem 模式检索单个项目的缩略图（不论文件类型）。 其他缩略图模式旨在显示多个文件的预览。 

- 在加载缩略图时显示通用占位符图像以代替缩略图。 使用占位符有助于让应用看起来更具响应能力，因为用户可以在加载缩略图之前与预览进行交互。 

    占位符图像应：
    * 特定于它代表的项目类型。 例如，文件夹、图片和视频都应有其自己的专用占位符。 
    * 与其代表的缩略图图像具有相同的大小和纵横比。 
    * 在缩略图图像加载完毕前一直显示。 

- 使用带有文本标签的占位符图像来表示文件夹和文件组，以区别于单个文件。

- 如果无法检索缩略图，则显示占位符图像。 

- 提供文档和音乐文件的预览时显示其他文件信息。 然后，用户可以识别可能无法仅从缩略图图像中轻松获得的文件关键信息。 例如，对于音乐文件，可以显示艺术家的姓名和专辑封面的缩略图。 

- 不显示图片和视频文件的其他文件信息。 在大多数情况下，缩略图图像足以让用户浏览图片和视频。 

## <a name="additional-usage-guidelines"></a>其他使用指南
推荐的[缩略图模式](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.thumbnailmode)和功能：

<table>
<tr>
<th> 显示预览</th>
<th> 缩略图模式 </th>
<th> 所检索到的缩略图图像的功能 </th>
</tr>
<tr>
<td> 图片<br /> 视频 </td>
<td> PicturesView <br />VideosView </td>
<td> <b>大小</b>：中等，最好至少为 190（如果图像大小为 190x130） <br />
<b>纵横比</b>：一致，宽纵横比约为 0.7（如果大小为 190，则为 190x130） <br />
裁剪以供预览。 <br /> 
由于纵横比一致，因此非常适合在网格中对齐图像。  </td>
</tr>
<tr>
<td> 文档<br />音乐 </td>
<td> DocumentsView <br />MusicView <br /> ListView</td>
<td> <b>大小</b>：小，最好至少为 40 x 40 像素 <br />
<b>纵横比</b>：一致，平方纵横比  <br />
由于平方纵横比一致，因此非常适合预览专辑封面。 <br /> 
文档与在文件选取器窗口中看起来相同（它使用相同的图标）。 </td>
</tr>
</tr>
<tr>
<td> 任何单个项目（不论文件类型） </td>
<td> SingleItem </td>
<td> <b>大小</b>：小，最好至少为 40 x 40 像素 <br />
<b>纵横比</b>：一致，平方纵横比  <br />
由于平方纵横比一致，因此非常适合预览专辑封面。 <br /> 
文档与在文件选取器窗口中看起来相同（它使用相同的图标）。 </td>
</tr>
</table>

以下是显示检索的缩略图图像（因文件类型和缩略图模式而异）的不同之处的示例：
<div class="mx-responsive-img">
<table>
<tr>
<th>项目类型</th>
<th>使用以下方法检索时： <ul><li>PicturesView <li>VideosView</ul></th>
<th>使用以下方法检索时： <ul><li>DocumentsView <li>MusicView <li>ListView</ul></th>
<th>使用以下方法检索时： <ul><li>SingleItem</ul></th>
<tr>
<tr>
<td>图片</td>
<td>缩略图使用文件的原始纵横比。 <br />
<img src="images/thumbnail-pic-picvidmode.png" alt="Picture thumbnail in picture or video mode"/></td>
<td>缩略图裁剪为平方纵横比。 <br />
<img src="images/thumbnail-pic-doclistmusic-modes.png" alt="Picture thumbnail in documents, music, or list modes"/></td>
<td>缩略图使用文件的原始纵横比。<br />
<img src="images/thumbnail-pic-single-mode.png" alt="Picture thumbnail in single mode"/> </td>
</tr>
<tr>
<td>视频</td>
<td>缩略图具有一个区分其与图片的图标。 <br />
<img src="images/thumbnail-vid-picvid-modes.png" alt="Video thumbnail in picture or video mode"/></td>
<td>缩略图裁剪为平方纵横比。 <br />
<img src="images/thumbnail-vid-doclistmusic-modes.png" alt="Video thumbnail in documents, music, or list mode"/> </td>
<td>缩略图使用文件的原始纵横比。 <br />
<img src="images/thumbnail-vid-single-mode.png" alt="Video thumbnail in single mode"/></td>
</tr>
<tr>
<td>音乐</td>
<td>缩略图为适当大小的背景上的图标。 背景颜色由应用的磁贴背景颜色决定。 <br />
<img src="images/thumbnail-music-picvid-modes.png" alt="Music thumbnail in picture or video mode"/></td>
<td>如果文件包含专辑封面，则缩略图为该专辑封面。  <br />
<img src="images/thumbnail-music-doclistmusic-modes.png" alt="Music thumbnail in documents, music, or list mode"/> <br />
否则，缩略图为适当大小的背景上的图标。</td>
<td>如果文件包含专辑封面，则缩略图为具有该文件原始纵横比的专辑封面。  <br />
<img src="images/thumbnail-music-single-mode.png" alt="Music thumbnail in single mode"/> <br />
否则，缩略图为图标。 </td>
</tr>
<tr>
<td>文档</td>
<td>缩略图为适当大小的背景上的图标。 背景颜色由应用的磁贴背景颜色决定。 <br />
<img src="images/thumbnail-docs-picvid-modes.png" alt="Document thumbnail in picture or video mode"/></td>
<td>缩略图为适当大小的背景上的图标。 背景颜色由应用的磁贴背景颜色决定。 <br />
<img src="images/thumbnail-doc-doclistmusic-modes.png" alt="Document thumbnail in documents, music, or list mode"/></td>
<td>文档缩略图（如果存在）。 <br />
<img src="images/thumbnail-doc1-single-mode.png" alt="Document thumbnail in single mode"/><br />
否则，缩略图为图标。 <br />
<img src="images/thumbnail-doc2-single-mode.png" alt="Document thumbnail icon in single mode"/></td>
</tr>
<tr>
<td>文件夹</td>
<td>如果文件夹中包含图片文件，则使用图片缩略图。  <br />
<img src="images/thumbnail-dir-picvid-modes.png" alt="Folder thumbnail in picture or video mode"/> <br />
否则不会检索任何缩略图。</td>
<td>未检索到任何缩略图图像。</td>
<td>缩略图为文件夹图标。<br />
<img src="images/thumbnail-dir-single-mode.png" alt="Folder icon thumbnail in single mode"/></td>
</tr>
<tr>
<td>文件组</td>
<td>如果文件夹中包含图片文件，则使用图片缩略图。<br />
<img src="images/thumbnail-grp-picvid-modes.png" alt="File group thumbnail in picture or video mode"/> <br /> 否则不会检索任何缩略图。 </td>
<td>如果组的文件中存在具有专辑封面的文件，则缩略图为该专辑封面。 <br />
<img src="images/thumbnail-grp-doclistmusic-modes.png" alt="File group thumbnail in documents, music or list mode"/> <br />否则不会检索任何缩略图。 </td>
<td>如果组中的一个文件有唱片集画面，则缩略图是唱片集画面，而且将使用文件的原始纵横比。 <br />
<img src="images/thumbnail-grp1-single-mode.png" alt="File group thumbnail in picture or video mode"/> <br />否则，缩略图为表示一组文件的图标。 <br />
<img src="images/thumbnail-grp2-single-mode.png" alt="File group icon in single mode"/> 
</td>
</tr>
</table>
</div>

## <a name="related-topics"></a>相关主题
- [ThumbnailMode 枚举](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.thumbnailmode)
- [StorageItemThumbnail 类](https://docs.microsoft.com/uwp/api/Windows.Storage.FileProperties.StorageItemThumbnail)
- [StorageFile 类](https://docs.microsoft.com/uwp/api/windows.storage.storagefile)
- [文件和文件夹缩略图示例 (GitHub)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FileThumbnails)
- [列表和网格视图](../design/controls-and-patterns/lists.md)