---
title: 启动屏幕截取
description: 本主题介绍 screenclip 和 screensketch URI 方案。 应用可以使用这些 URI 方案来启动 & 草拟应用的截图，或打开新的代码段。
ms.date: 08/09/2017
ms.topic: article
keywords: windows 10、uwp、uri、截图、草图
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: d9469dd6efd3598ab7abd9791a976385f4dfce49
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/06/2020
ms.locfileid: "75684662"
---
# <a name="launch-screen-snipping"></a>启动屏幕截取

使用**screenclip：** 和**screensketch：** URI 方案，可以启动截图或编辑屏幕截图。

## <a name="open-a-new-snip-from-your-app"></a>从应用打开新的截图

使用**screenclip：** URI，你的应用程序可以自动打开并启动新的截图。 生成的截图会复制到用户的剪贴板，但不会自动传递回打开的应用。

**screenclip：** 采用以下参数：

| 参数 | 在任务栏的搜索框中键入 | 必需 | 描述 |
| --- | --- | --- | --- |
| 源 | 字符串 | 否 | 指示启动 URI 的源的任意形式的字符串。 |
| delayInSeconds | int | 否 | 介于1到30之间的整数值。 指定在 URI 调用和截图开始之间的延迟（以秒为单位）。 |
| callbackformat | 字符串 | 否 | 此参数不可用。 |

## <a name="launching-the-snip--sketch-app"></a>& 草绘应用启动截图

使用**screensketch：** URI 可以通过编程方式启动截图 & 草拟应用，并打开该应用中的特定图像以进行批注。

**screensketch：** 采用以下参数：

| 参数 | 在任务栏的搜索框中键入 | 必需 | 描述 |
| --- | --- | --- | --- |
| sharedAccessToken | 字符串 | 否 | 标识要在 & 草拟应用的截图中打开的文件的标记。 从[SharedStorageAccessManager](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.sharedstorageaccessmanager.addfile)中检索。 如果省略此参数，则在不打开文件的情况下启动应用程序。 |
| secondarySharedAccessToken | 字符串 | 否 | 一个字符串，该字符串标识包含有关截图的元数据的 JSON 文件。 元数据可以包含**clipPoints**字段，该字段具有 x、y 坐标和/或[userActivity](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity)的数组。 |
| 源 | 字符串 | 否 | 指示启动 URI 的源的任意形式的字符串。 |
| isTemporary | Bool | 否 | 如果设置为 True，则在打开文件后，屏幕草图将尝试删除该文件。 |

下面的示例调用[LaunchUriAsync](https://docs.microsoft.com/uwp/api/Windows.System.Launcher#Windows_System_Launcher_LaunchUriAsync_Windows_Foundation_Uri_)方法，以从用户的应用程序中将图像发送到剪裁 &。

```csharp

bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-screensketch:edit?source=MyApp&isTemporary=false&sharedAccessToken=2C37ADDA-B054-40B5-8B38-11CED1E1A2D"));

```

下面的示例演示了**screensketch**的**secondarySharedAccessToken**参数指定的文件可能包含的内容：

```json
{
  "clipPoints": [
    {
      "x": 0,
      "y": 0
    },
    {
      "x": 2080,
      "y": 0
    },
    {
      "x": 2080,
      "y": 780
    },
    {
      "x": 0,
      "y": 780
    }
  ],
  "userActivity": "{\"$schema\":\"http://activity.windows.com/user-activity.json\",\"UserActivity\":\"type\",\"1.0\":\"version\",\"cross-platform-identifiers\":[{\"platform\":\"windows_universal\",\"application\":\"Microsoft.MicrosoftEdge_8wekyb3d8bbwe!MicrosoftEdge\"},{\"platform\":\"host\",\"application\":\"edge.activity.windows.com\"}],\"activationUrl\":\"microsoft-edge:https://support.microsoft.com/help/13776/windows-use-snipping-tool-to-capture-screenshots\",\"contentUrl\":\"https://support.microsoft.com/help/13776/windows-use-snipping-tool-to-capture-screenshots\",\"visualElements\":{\"attribution\":{\"iconUrl\":\"https://www.microsoft.com/favicon.ico?v2\",\"alternateText\":\"microsoft.com\"},\"description\":\"https://support.microsoft.com/help/13776/windows-use-snipping-tool-to-capture-screenshots\",\"backgroundColor\":\"#FF0078D7\",\"displayText\":\"Use snipping tool to capture screenshots - Windows Help\",\"content\":{\"$schema\":\"http://adaptivecards.io/schemas/adaptive-card.json\",\"type\":\"AdaptiveCard\",\"version\":\"1.0\",\"body\":[{\"type\":\"Container\",\"items\":[{\"type\":\"TextBlock\",\"text\":\"Use snipping tool to capture screenshots - Windows Help\",\"weight\":\"bolder\",\"size\":\"large\",\"wrap\":true,\"maxLines\":3},{\"type\":\"TextBlock\",\"text\":\"https://support.microsoft.com/help/13776/windows-use-snipping-tool-to-capture-screenshots\",\"size\":\"normal\",\"wrap\":true,\"maxLines\":3}]}]}},\"isRoamable\":true,\"appActivityId\":\"https://support.microsoft.com/help/13776/windows-use-snipping-tool-to-capture-screenshots\"}"
}

```
