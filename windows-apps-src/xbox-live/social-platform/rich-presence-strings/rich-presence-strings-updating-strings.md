---
title: “完整状态”更新字符串
author: KevinAsgari
description: 了解如何更新 Xbox Live“完整状态”字符串。
ms.assetid: eb2bb82e-8730-4d74-9b33-95d133360e44
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, 完整状态
ms.localizationpriority: medium
ms.openlocfilehash: 4dd235edd7f47f0a2d9b42be28bd974042de5739
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/06/2018
ms.locfileid: "6050355"
---
# <a name="rich-presence-updating-strings"></a>“完整状态”更新字符串

若要更新标题中的“完整状态”字符串，你可以在 JSON 对象中使用合适的参数调用编写标题 URI。 此 restful 调用也可以由 Xbox 服务 API 包装。 有关相关 API 的信息，请参阅 **Microsoft.Xbox.Services.Presence 命名空间**。

URI 如下所示：

          POST /users/xuid({xuid})/devices/current/titles/current

下面是只用于设置“完整状态”字符串的字段。 还有其他一些与标题的编写状态相关的可选字段未在此处列出。

## <a name="titlerequest-object"></a>TitleRequest 对象

属性 | 类型 | 必填 | 描述
---|---|---|---
活动|ActivityRequest|N|描述标题内信息的记录（完整状态和媒体信息，如果有）

## <a name="activityrequest-object"></a>ActivityRequest 对象

属性 | 类型 | 必填 | 描述
---|---|---|---
richPresence|RichPresenceRequest|N|应使用的“完整状态”字符串 friendlyName。

## <a name="richpresencerequest-object"></a>RichPresenceRequest 对象

属性 | 类型 | 必填 | 描述
---|---|---|---
ID|字符串|Y|应使用的“完整状态”字符串 friendlyName
Scid|字符串|Y|告诉我们定义“完整状态”字符串的位置的 Scid。

例如，如果我想要为 xuid 是 12345 的用户更新完整状态，调用将如下所示：

          POST /users/xuid(12345)/devices/current/titles/current


包含以下 JSON 正文：

```json
          {
            activity:
            {
              richPresence:
              {
                id:"playingMap",
                scid:"0000-0000-0000-0000-01010101"
              }
            }
          }
```

使用包装器 API，这会是对 **PresenceService.SetPresenceAsync 方法**的调用

如果你需要数据平台保持最新，那么你无需在每次数据填充空白更改时重置“完整状态”字符串。 在上方的示例中，我们知道你想要使用当前地图。 当用户尝试读取字符串以填充当前值时，“完整状态”将查找数据平台中的数据。 所以，即使玩家不停切换地图，你也无需重置游戏中的“完整状态”字符串，只要你将相应事件发送到数据平台。 请记住，数据查找前往数据平台的通道可能需要几秒钟时间。

然后，当某人尝试读取用户 12345 的完整状态时，服务将查看请求所在的区域，并在返回之前相应设置字符串的格式。

在这里，我们假设用户想要读取 en-US 字符串。 读取完整状态的工作原理如下所示（有关此调用的详细信息，请参见 **GET (/users/xuid({xuid}))**）

          GET /users/xuid(12345)?level=all

所使用的包装器 API 是 **PresenceService.GetPresenceAsync 方法**

这里正在发生的是：你要求用户的 PresenceRecord，他的 xuid 是 12345。 你请求的详细级别为“全部”。 如果未指定“全部”，将不返回“完整状态”。

将在 JSON 响应中返回以下信息：

```json
          {
            xuid:"12345",
            state:"online",
            devices:
            [
              {
                type:"D",
                titles:
                [
                  {
                    id:"12345",
                    name:"Buckets are Awesome",
                    lastModified:"2012-09-17T07:15:23.4930000",
                    placement: "full",
                    state:"active",
                    activity:
                    {
                      richPresence:"Playing on map:Mountains"
                    }
                  }
                ]
              }
            ]
          }
```
