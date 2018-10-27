---
author: laurenhughes
ms.assetid: 1901c4c2-5161-435d-bc7b-f40c69cdb138
title: 文件、文件夹和库
description: 了解有关应用设置的读取和写入、文件和文件夹选取器，以及诸如视频/音乐库的特殊“沙盒式”位置的信息。
ms.author: lahugh
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 80c6292c12568d7f1017ca67707689ce9539c804
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2018
ms.locfileid: "5709125"
---
 # <a name="files-folders-and-libraries"></a>文件、文件夹和库


使用 [Windows.Storage](https://msdn.microsoft.com/library/windows/apps/br227346)、[Windows.Storage.Streams](https://msdn.microsoft.com/library/windows/apps/br241791) 和 [Windows.Storage.Pickers](https://msdn.microsoft.com/library/windows/apps/br207928) 命名空间中的 API，以在文件中读取和写入文本和其他数据格式并管理文件和文件夹。 在本部分中，你还将了解有关读取和写入应用设置、文件和文件夹选取器、特殊沙盒式位置（如视频/音乐库）的信息。

| 主题 | 说明  |
|-------|--------------|
| [枚举和查询文件和文件夹](quickstart-listing-files-and-folders.md) | 访问位于文件夹、库、设备或网络位置的文件和文件夹。 还可以通过构造文件和文件夹查询来查询某个位置的文件和文件夹。 |
| [创建、写入和读取文件](quickstart-reading-and-writing-files.md) | 使用 [StorageFile](https://msdn.microsoft.com/library/windows/apps/br227171) 对象读取和写入文件。 |
| [获取文件属性](quickstart-getting-file-properties.md) | 获取由 [StorageFile](https://msdn.microsoft.com/library/windows/apps/br227171) 对象表示的文件属性：顶级、基本和扩展。 |
| [使用选取器打开文件和文件夹](quickstart-using-file-and-folder-pickers.md) | 通过让用户选取器交互来访问文件和文件夹。 你可以使用 [FolderPicker](https://msdn.microsoft.com/library/windows/apps/br207881) 来获取对文件夹的访问权限。 |
| [使用选取器保存文件](quickstart-save-a-file-with-a-picker.md) | 使用 [FileSavePicker](https://msdn.microsoft.com/library/windows/apps/br207871) 让用户指定名称和他们要让应用保存文件的位置。 |
| [访问家庭组内容](quickstart-accessing-homegroup-content.md) | 访问存储在用户的“家庭组”文件夹中的内容，包括图片、音乐和视频。 |
| [确定 Microsoft OneDrive 文件的可用性](quickstart-determining-availability-of-microsoft-onedrive-files.md) | 使用 [StorageFile.IsAvailable](https://msdn.microsoft.com/library/windows/apps/windows.storage.storagefile.isavailable.aspx) 属性确定 Microsoft OneDrive 文件是否可用。 |
| [音乐、图片和视频库中的文件和文件夹](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md) | 将现有的音乐、图片和视频文件夹添加到相应的库。 你还可以从库中删除文件夹、获取库中的文件夹列表，并发现存储的照片、音乐和视频。 |
| [跟踪最近使用的文件和文件夹](how-to-track-recently-used-files-and-folders.md) | 通过将用户经常访问的文件添加到你的应用的最近使用列表 (MRU) 中来跟踪这些文件。 该平台会为你管理 MRU，它会根据各个项的上次访问时间对它们进行排序，并在列表中的项超过 25 个的限制时删除最旧的项。 所有应用都有其自己的 MRU。 |
| [访问 SD 卡](access-the-sd-card.md) | 你可以在可选 MicroSD 卡上存储和访问不重要的数据，尤其是内部存储具有限制的低成本移动设备。 |
| [文件访问权限](file-access-permissions.md) | 默认情况下，应用可以访问特定文件系统位置。 应用也可以通过文件选取器或声明功能访问其他位置。 |
| [快速访问 UWP 中的文件属性](fast-file-properties.md) | 高效地从库中收集文件及其属性的列表以用于 UWP 应用。 |

## <a name="related-samples"></a>相关示例
[文件夹枚举示例](http://go.microsoft.com/fwlink/p/?linkid=619993)

[文件访问示例](http://go.microsoft.com/fwlink/p/?linkid=619995)

[文件选取器示例](http://go.microsoft.com/fwlink/p/?linkid=619994)
 

 
