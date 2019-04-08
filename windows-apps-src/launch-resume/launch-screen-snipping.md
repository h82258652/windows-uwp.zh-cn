---
title: 启动屏幕截取
description: 本主题介绍 ms screenclip 和 ms screensketch URI 方案。 若要启动切片和草图应用或打开新的代码段，您的应用程序可以使用这些 URI 方案。
ms.date: 08/09/2017
ms.topic: article
keywords: windows 10、 uwp、 uri、 代码段、 草图
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 06e988387f574b74d511b14a2ebca24b0a149158
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57595382"
---
# <a name="launch-screen-snipping"></a>启动屏幕截取

**Ms screenclip:** 和**ms screensketch:** URI 方案，可启动代码段或编辑屏幕快照。

## <a name="open-a-new-snip-from-your-app"></a>从您的应用程序中打开新的代码段

**Ms screenclip:** URI，应用可以自动打开并开始新的代码段。 生成的代码段复制到用户的剪贴板，但不是会自动传递回打开应用。

**ms screenclip:** 采用以下参数：

| 参数 | 在任务栏的搜索框中键入 | 必需 | 描述 |
| --- | --- | --- | --- |
| 源 | 字符串 | 否 | 要指示启动 URI 的源的自由格式字符串。 |
| delayInSeconds | int | 否 | 介于 1 到 30 的整数值。 在完整秒为单位的 URI 调用代码段的开始时指定延迟。 |
| callbackformat | 字符串 | 否 | 此参数将不可用。 |

## <a name="launching-the-snip--sketch-app"></a>启动代码段和草图应用

**Ms screensketch:** URI 可以以编程方式启动代码段和草图应用中，并批注适用于该应用中打开特定的映像。

**ms screensketch:** 采用以下参数：

| 参数 | 在任务栏的搜索框中键入 | 必需 | 描述 |
| --- | --- | --- | --- |
| sharedAccessToken | 字符串 | 否 | 标识要在代码段和草图应用中打开的文件标记。 从中检索[SharedStorageAccessManager.AddFile](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.sharedstorageaccessmanager.addfile)。 如果省略此参数，则将没有打开的文件的情况下启动应用。 |
| secondarySharedAccessToken | 字符串 | 否 | 有关代码段的元数据与标识的 JSON 文件的字符串。 元数据可能包括**clipPoints**字段的 x，y 坐标、 数组和/或一个[userActivity](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity)。 |
| 源 | 字符串 | 否 | 要指示启动 URI 的源的自由格式字符串。 |
| IsTemporary | 布尔 | 否 | 如果设置为 True，屏幕草图将尝试打开它后删除该文件。 |

下面的示例调用[LaunchUriAsync](https://docs.microsoft.com/uwp/api/Windows.System.Launcher#Windows_System_Launcher_LaunchUriAsync_Windows_Foundation_Uri_)方法从用户的应用将映像发送到代码段和草图。

```csharp

bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-screensketch:edit?source=MyApp&isTemporary=false&sharedAccessToken=2C37ADDA-B054-40B5-8B38-11CED1E1A2D"));

```

下面的示例说明了哪些文件指定**secondarySharedAccessToken**的参数**ms screensketch**可能包含：

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
  "userActivity": "{\"$schema\":\"http://activity.windows.com/user-activity.json\",\"UserActivity\":\"type\",\"1.0\":\"version\",\"cross-platform-identifiers\":[{\"platform\":\"windows_universal\",\"application\":\"Microsoft.MicrosoftEdge_8wekyb3d8bbwe!MicrosoftEdge\"},{\"platform\":\"host\",\"application\":\"edge.activity.windows.com\"}],\"activationUrl\":\"microsoft-edge:https://support.microsoft.com/en-us/help/13776/windows-use-snipping-tool-to-capture-screenshots\",\"contentUrl\":\"https://support.microsoft.com/en-us/help/13776/windows-use-snipping-tool-to-capture-screenshots\",\"visualElements\":{\"attribution\":{\"iconUrl\":\"https://www.microsoft.com/favicon.ico?v2\",\"alternateText\":\"microsoft.com\"},\"description\":\"https://support.microsoft.com/en-us/help/13776/windows-use-snipping-tool-to-capture-screenshots\",\"backgroundColor\":\"#FF0078D7\",\"displayText\":\"Use snipping tool to capture screenshots - Windows Help\",\"content\":{\"$schema\":\"http://adaptivecards.io/schemas/adaptive-card.json\",\"type\":\"AdaptiveCard\",\"version\":\"1.0\",\"body\":[{\"type\":\"Container\",\"items\":[{\"type\":\"TextBlock\",\"text\":\"Use snipping tool to capture screenshots - Windows Help\",\"weight\":\"bolder\",\"size\":\"large\",\"wrap\":true,\"maxLines\":3},{\"type\":\"TextBlock\",\"text\":\"https://support.microsoft.com/en-us/help/13776/windows-use-snipping-tool-to-capture-screenshots\",\"size\":\"normal\",\"wrap\":true,\"maxLines\":3}]}]}},\"isRoamable\":true,\"appActivityId\":\"https://support.microsoft.com/en-us/help/13776/windows-use-snipping-tool-to-capture-screenshots\"}"
}

```
