---
author: TylerMSFT
title: ms-tonepicker 方案
description: 本主题介绍了 ms-tonepicker URI 方案，以及如何使用它显示音调选取器，以便选择音调、保存音调和获取音调的友好名称。
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 0c17e4fb-7241-4da9-b457-d6d3a7aefccb
ms.localizationpriority: medium
ms.openlocfilehash: 61967f0aa81c49cb4e81b11bedac84c318be1840
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2018
ms.locfileid: "5696077"
---
# <a name="choose-and-save-tones-using-the-ms-tonepicker-uri-scheme"></a>使用 ms-tonepicker URI 方案选择并保存音调

本主题介绍了如何使用 **ms-tonepicker:** URI 方案。 此 URI 方案可用于：
- 确定音调选取器在设备上是否可用。
- 显示音调选取器列出可用铃声、系统声音、短信铃声和闹钟声音；获取可表示用户所选声音的音调标记。
- 显示音调保存器，可将声音文件标记用作输入并将其保存到设备。 保存的音调随后可通过音调选取器提供。 用户还可以为音调提供友好名称。
- 将音调标记转换为其友好名称。

## <a name="ms-tonepicker-uri-scheme-reference"></a>ms-tonepicker: URI 方案引用

此 URI 方案不会通过 URI 方案字符串传递参数，而是通过 [ValueSet](https://msdn.microsoft.com/library/windows/apps/windows.foundation.collections.valueset.aspx) 传递参数。 所有字符串均区分大小写。

以下部分指示应传递哪些参数，以便完成指定的任务。

## <a name="task-determine-if-the-tone-picker-is-available-on-the-device"></a>任务：确定音调选取器在设备上是否可用
```cs
var status = await Launcher.QueryUriSupportAsync(new Uri("ms-tonepicker:"),     
                                     LaunchQuerySupportType.UriForResults,
                                     "Microsoft.Tonepicker_8wekyb3d8bbwe");

if (status != LaunchQuerySupportStatus.Available)
{
    // the tone picker is not available
}
```

## <a name="task-display-the-tone-picker"></a>任务：显示音调选取器

可传递用于显示音调选取器的参数如下所示：

| 参数 | 类型 | 必需 | 可能值 | 描述 |
|-----------|------|----------|-------|-------------|
| Action | 字符串 | 是 | “PickRingtone” | 打开音调选取器。 |
| CurrentToneFilePath | 字符串 | 否 | 现有音调标记。 | 在音调选取器中显示为当前音调的音调。 如果未设置此值，默认情况下将选择列表上的第一个音调。<br>严格来讲，这不是文件路径。 你可以在从音调选取器中返回的 `ToneToken` 值中，为 `CurrenttoneFilePath` 获取一个适合的值。  |
| TypeFilter | 字符串 | 否 | “Ringtones”、“Notifications”、“Alarms”、“None” | 选择要添加到选取器的音调。 如果未指定筛选器，将显示所有音调。 |

在 [LaunchUriResults.Result](https://msdn.microsoft.com/library/windows/apps/windows.system.launchuriresult.result.aspx) 中返回的值：

| 返回值 | 类型 | 可能值 | 描述 |
|--------------|------|-------|-------------|
| 结果 | Int32 | 0 - 成功。 <br>1 - 已取消。 <br>7 - 参数无效。 <br>8 - 没有匹配筛选条件的音调。 <br>255 - 未实现指定的操作。 | 选取器操作的结果。 |
| ToneToken | 字符串 | 所选音调的标记。 <br>如果用户在选取器中选择“默认值”****，该字符串为空。 | 此标记可用于 Toast 通知负载，或者可以指定为联系人的铃声或短信铃声。 如果 **Result** 为 0，仅在 ValueSet 中返回参数。 |
| DisplayName | 字符串 | 指定的音调的友好名称。 | 可显示给用户并用于表示所选音调的字符串。 如果 **Result** 为 0，仅在 ValueSet 中返回参数。 |


**示例：打开音调选取器，以便用户可选择音调**

``` cs
LauncherOptions options = new LauncherOptions();
options.TargetApplicationPackageFamilyName = "Microsoft.Tonepicker_8wekyb3d8bbwe";

ValueSet inputData = new ValueSet() {
    { "Action", "PickRingtone" },
    { "TypeFilter", "Ringtones" } // Show only ringtones
};

LaunchUriResult result = await Launcher.LaunchUriForResultsAsync(new Uri("ms-tonepicker:"), options, inputData);

if (result.Status == LaunchUriStatus.Success)
{
     Int32 resultCode =  (Int32)result.Result["Result"];
     if (resultCode == 0)
     {
         string token = result.Result["ToneToken"] as string;
         string name = result.Result["DisplayName"] as string;
     }
     else
     {
           // handle failure
     }
}
```

## <a name="task-display-the-tone-saver"></a>任务：显示音调保存器

可传递用于显示音调保存器的参数如下所示：

| 参数 | 类型 | 必需 | 可能值 | 描述 |
|-----------|------|----------|-------|-------------|
| Action | 字符串 | 是 | “SaveRingtone” | 打开选取器以保存铃声。 |
| ToneFileSharingToken | 字符串 | 是 | 用于要保存的铃声文件的 [SharedStorageAccessManager](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.sharedstorageaccessmanager.aspx) 文件共享标记 | 将特定的声音文件另存为铃声。 支持的文件内容类型为 mpeg 音频和 x-ms-wma 音频。 |
| DisplayName | 字符串 | 否 | 指定的音调的友好名称。 | 设置要在保存指定铃声时使用的友好名称。 |

在 [LaunchUriResults.Result](https://msdn.microsoft.com/library/windows/apps/windows.system.launchuriresult.result.aspx) 中返回的值：

| 返回值 | 类型 | 可能值 | 描述 |
|--------------|------|-------|-------------|
| 结果 | Int32 | 0 - 成功。<br>1 - 已由用户取消。<br>2 - 文件无效。<br>3 - 文件内容类型无效。<br>4 - 文件超过最大铃声大小（在 Windows 10 中为 1 MB）。<br>5 - 文件超过 40 秒的长度限制。<br>6 - 文件不受数字版权管理保护。<br>7 - 参数无效。 | 选取器操作的结果。 |

**示例：将本地音乐文件另存为铃声**

``` cs
LauncherOptions options = new LauncherOptions();
options.TargetApplicationPackageFamilyName = "Microsoft.Tonepicker_8wekyb3d8bbwe";

ValueSet inputData = new ValueSet() {
    { "Action", "SaveRingtone" },
    { "ToneFileSharingToken", SharedStorageAccessManager.AddFile(myLocalFile) }
};

LaunchUriResult result = await Launcher.LaunchUriForResultsAsync(new Uri("ms-tonepicker:"), options, inputData);

if (result.Status == LaunchUriStatus.Success)
{
     Int32 resultCode = (Int32)result.Result["Result"];

     if (resultCode == 0)
     {
         // no issues
     }
     else
     {
          switch (resultCode)
          {
             case 2:
              // The specified file was invalid
              break;
              case 3:
              // The specified file's content type is invalid
              break;
              case 4:
              // The specified file was too big
              break;
              case 5:
              // The specified file was too long
              break;
              case 6:
              // The file was protected by DRM
              break;
              case 7:
              // The specified parameter was incorrect
              break;
          }
      }
 }
```

## <a name="task-convert-a-tone-token-to-its-friendly-name"></a>任务：将音调标记转换为其友好名称

可传递用于获取音调的友好名称的参数如下所示：

| 参数 | 类型 | 必需 | 可能值 | 描述 |
|-----------|------|----------|-------|-------------|
| Action | 字符串 | 是 | “GetToneName” | 指示你想要获取音调的友好名称。 |
| ToneToken | 字符串 | 是 | 音调标记 | 从中获取显示名称的音调标记。 |

在 [LaunchUriResults.Result](https://msdn.microsoft.com/library/windows/apps/windows.system.launchuriresult.result.aspx) 中返回的值：

| 返回值 | 类型 | 可能值 | 描述 |
|--------------|------|-------|-------------|
| 结果 | Int32 | 0 - 选取器操作成功。<br>7 - 参数不正确（例如，未提供 ToneToken）。<br>9 - 从指定的标记读取名称时出错。<br>10 - 无法找到指定的音调标记。 | 选取器操作的结果。
| DisplayName | 字符串 | 音调的友好名称。 | 返回所选音调的显示名称。 如果 **Result** 为 0，仅在 ValueSet 中返回此参数。 |

**示例：从 Contact.RingToneToken 检索音调标记并在联系人卡片中显示其友好名称。**

```cs
using (var connection = new AppServiceConnection())
{
    connection.AppServiceName = "ms-tonepicker-nameprovider";
    connection.PackageFamilyName = "Microsoft.Tonepicker_8wekyb3d8bbwe";
    AppServiceConnectionStatus connectionStatus = await connection.OpenAsync();
    if (connectionStatus == AppServiceConnectionStatus.Success)
    {
        var message = new ValueSet() {
            { "Action", "GetToneName" },
            { "ToneToken", token)
        };
        AppServiceResponse response = await connection.SendMessageAsync(message);
        if (response.Status == AppServiceResponseStatus.Success)
        {
            Int32 resultCode = (Int32)response.Message["Result"];
            if (resultCode == 0)
            {
                string name = response.Message["DisplayName"] as string;
            }
            else
            {
                // handle failure
            }
        }
        else
        {
            // handle failure
        }
    }
}
```
