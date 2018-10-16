---
title: Xbox Live 标题存储
author: KevinAsgari
description: 了解如何使用 Xbox Live 标题存储来存储云中标题的游戏信息。
ms.assetid: a4182bc8-d232-4e77-93ae-97fe17ac71b1
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 49f0f88a7e64ce57462b3ee7b07676280d91fb41
ms.sourcegitcommit: 106aec1e59ba41aae2ac00f909b81bf7121a6ef1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/15/2018
ms.locfileid: "4623760"
---
# <a name="xbox-live-title-storage"></a>Xbox Live 标题存储

Xbox Live 标题存储服务提供了一种在云中存储标题游戏信息的方法。 所有平台上运行的游戏均可使用此服务。

<a name="ID4EW"></a>

## <a name="features-of-xbox-live-title-storage"></a>Xbox Live 标题存储功能

Xbox Live 标题存储的一些高级功能包括（但不限于）：

-   可以跨用户、游戏和各种平台共享
-   支持 JSON、二进制，以及配置文件

下列部分将对 Xbox Live 标题存储的主要功能进行详细说明：

-   [存储类型](#ID4ETB)
-   [数据类型](#ID4ECF)
-   [标题存储 URI](#ID4EBEAC)
-   [调节限制](#ID4ETEAC)

<a name="ID4ETB"></a>

适用于托管的合作伙伴和 ID@Xbox 成员：

| 存储类型       | 配额（托管的 Partners/ID@Xbox） | 配额（Xbox Live 创意者计划） |  用途                                                                                                                                                      | 平台                                                                                           | 用户                                       |
|--------------------|--------------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|---------------------------------------------|
| 受信任平台   | 每个用户 256 MB | 每个用户 64 MB    | 已保存游戏或游戏状态（播放/暂停/继续）等基于每个用户的数据。 更安全，但具有平台限制。 | 任何平台均可读取，但只有 Xbox One、Xbox 360 或 Windows Phone 可以写入。  | 可公开配置或仅所有者可配置。       |
| 通用平台 | 每个用户 64 MB | 每个用户 64 MB    | 已保存游戏或游戏状态（播放/暂停/继续）等基于每个用户的数据。 | 任何平台均可写入，但只有 Xbox One、Xbox 360 或 Windows Phone 以外的平台才可读取。 | 可公开配置或仅所有者可配置。       |
| 全局             | 256 MB | 256 MB            | 每个人均可读取的数据，如名单、地图、挑战，或艺术资源。 | 仅可通过 Windows 开发人员中心的 Xbox 开发人员门户写入，任何平台均可读取。                                | 所有用户均可读取。

### <a name="deprecated-storage-types"></a>弃用的存储类型

以下存储类型已弃用。 这些类型仅受当前正在使用它们的游戏支持。 它们不适用于新游戏。

| 存储类型       | 配额  |   用途                                                                                                                                                      | 平台                                                                                           | 用户                                       |
|--------------------|--------------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|---------------------------------------------|
| JSON               | 每个用户 64 MB     | 已保存游戏或游戏状态（播放/暂停/继续）等基于每个用户的数据。 更安全，无平台限制，但具有数据格式限制（仅 JSON）。 | 任何平台均可读取或写入。                                                                     | 可公开配置或仅所有者可配置。       |
| 设备             | 每个设备 64 MB   | 设置或设备首选项等特定于设备的数据。                                                                                            | 仅 Xbox One、Xbox 360，或 Windows Phone 可写入。 仅写入数据的设备可读取。  | 所有用户均可读取。                         |
| 会话存储    | 每个会话 256 MB | 任何加入到特定多人游戏会话的玩家数据。                                                                                             | 任何可加入会话的平台。                                                             | 会话中的所有用户均可读取或写入。 |


<a name="ID4ECF"></a>

## <a name="types-of-data"></a>数据类型

游戏指定要在 GET 或 PUT 方法的 **{type}** 参数中使用的数据类型。 以下部分描述了三种支持的类型：

-   [二进制信息](#ID4ENF)
-   [JSON 信息](#ID4EUF)
-   [配置信息](#ID4ECAAC)

<a name="ID4ENF"></a>

#### <a name="binary-information"></a>二进制信息

图像、声音和自定义数据均使用二进制类型。 由于必须通过 HTTP 传输数据，因此必须将二进制数据编码为 HTTP 支持的字符。 例如，可将数据转换成十六进制字符串或使用 base64 编码。 标题存储系统不会处理编码数据，因此当读取和写入标题存储时，你的游戏必须为编码和解码数据使用相同的方案。

<a name="ID4EUF"></a>

#### <a name="json-information"></a>JSON 信息

结构数据可使用 JSON 类型。 可在支持 JSON 对象的语言（如 JavaScript）中直接使用它们。 从 JSON 文件检索数据时，该游戏可提供 *select* 参数，以在结构内返回特定项目。 例如，使用包含以下信息的 JSON 格式的文件：

    {
    "difficulty" : 1,
    "level" :
        [
            { "number" : "1", "quest" : "swords" },
            { "number" : "2", "quest" : "iron" },
            { "number" : "3", "quest" : "gold" },
            { "number" : "4", "quest" : "queen" }
         ],
    "weapon" :
        {
             "name" : "poison",
             "timeleft" : "2mins"
        }
    }


| 注意                                                                                                                                              |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 出于安全考虑，JSON 数据的第一个元素不得为数组。 提交根目录具有数组的 JSON 将会被该服务拒绝。 |

可使用如下查询选择此结构的部分内容：

             GET https://titlestorage.xboxlive.com/users/xuid(1234)/storage/titlestorage/titlegroups/
             faa29d21-2b49-4908-96bf-b953157ac4fe/data/save1.dat,json?select=weapon.name
             Content-Type: application/octet-stream
             x-xbl-contract-version: 1
             Authorization: XBL3.0 x=<userHash>;<STSTokenString>
             Connection: Keep-Alive

此查询的响应正文为：

    {
        "name" : "poison"
    }

可使用如下查询访问数组：

      GET https://titlestorage.xboxlive.com//users/xuid(1234)/storage/titlestorage/titlegroups/
      faa29d21-2b49-4908-96bf-b953157ac4fe/data/save1.dat,json?select=levels[3].quest
      Content-Type: application/octet-stream
      x-xbl-contract-version: 1
      Authorization: XBL3.0 x=<userHash>;<STSTokenString>
      Connection: Keep-Alive

此查询的响应正文为：

    {
        "quest" : "queen"
    }

为 JSON 数据强制执行以下长度限制：

-   数值，最大长度 = 32
-   字符串值，最大长度 = 1024
-   属性名称，最大长度 = 64
-   层次结构，最大深度 = 16
-   数组，最大大小 = 1024
-   子属性，在对象中的最大值 = 1024

<a name="ID4ECAAC"></a>

#### <a name="configuration-information"></a>配置信息

可**配置** **{type}** 以指示此数据为配置 blob。 配置 blob 是存储在全局标题存储中的数据结构。 blob 格式类似于 JSON 对象。

配置 blob 可包含虚拟节点，此节点从可能性列表返回设置。 虚拟节点对为特定情况（如用于游戏或区域设置）提供设置非常有用。 虚拟节点包含若干可能的设置和值以及用于从值中选择的条件。 在以下示例中，**defaultCardDesign** 设置可具有虚拟节点中的一个值。

    {
      "defaultCardDesign":
      {
        "_virtualNode":
       {
          "_selectBy":"titleId",
          "_sourceNodes":
          [
            {"_selector":"123456799", "_data":"RobotUnicornCard.png,binary"},
            {"_selector":"default", "_data":"StandardCard.png,binary"}
          ]
        }
      },
    }

当游戏读取本文件时，系统将从 **\_sourceNodes** 数组中选择一个值。 在这种情况下，基于游戏的游戏 ID 选择此项目。 玩 **12456799** 游戏的用户会看到：

    {
      "defaultCardDesign":"RobotUnicornCard.png,binary",
      "_sourceNodes":["defaultCardDesign:titleID:1234567899"]
    }

其余用户会看到：

    {
      "defaultCardDesign":"StandardCard.png,binary",
      "_sourceNodes":["defaultCardDesign:titleID:default"]
    }

游戏可定义与请求中参数相匹配的自定义选择器。 例如，在此配置 blob 中：

    {
        "defaultCardDesign":
        {
            "_virtualNode":
            {
                "_selectBy":"custom:gameMode",
                "_sourceNodes":
                [
                    {"_selector":"silly", "_data":"RobotUnicornCard.png,binary"},
                    {"_selector":"serious", "_data":"SeriousCard.png,binary"},
                    {"_selector":"default", "_data":"StandardCard.png,binary"}
                 ]
            }
        },
        "backgroundColor":"green",
        "dealerHitsOnSoft17":true
    }

游戏将 **customSelector** 参数传递给字符串以选择要返回的项目。 例如，为了获取第二个选项，游戏会请求：

      GET https://titlestorage.xboxlive.com/media/titlegroups/faa29d21-2b49-4908-96bf-b953157ac4fe
      /storage/data/config.json,config?customSelector=gameMode.serious
      Content-Type: application/octet-stream
      x-xbl-contract-version: 1
      Authorization: XBL3.0 x=<userHash>;<STSTokenString>
      Connection: Keep-Alive

**\_selectBy** 值表示要执行的选择类型，**\_selector** 值表示要在该选择中使用的数据。 可能的值为：

<table>
<thead>
<tr>
<th>_selectBy</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td >titleId</td>
<td ><p><strong>_selector</strong> 匹配提供的声明中的游戏 ID。</p></td>
</tr>
<tr>
<td >区域设置</td>
<td ><p><strong>_selector</strong> 匹配“接受语言”标题中的区域设置字符串。</p></td>
</tr>
<tr>
<td >自定义</td>
<td ><p><strong>_selector</strong> 匹配在 <strong>customSelector</strong> 查询参数中传递的自定义字符串。 <strong>customSelector</strong> 包含一个或多个用逗号分隔的查询。 每个查询都是名称来自 <strong>selectBy</strong> 元素而值来自 <strong>_selector</strong> 元素。</p></td>
</tr>
</tbody>
</table>

<a name="ID4EBEAC"></a>

## <a name="title-storage-uris"></a>标题存储 URI

标题存储 URI 按如下所示进行格式化：

    https://titlestorage.xboxlive.com/{path}

此 URI 的 **{path}** 部分是要进行的请求类型，不得超过 245 个字符。

<a name="ID4ETEAC"></a>

## <a name="throttle-limit"></a>调节限制

游戏在每分钟内所能读取或写入的次数并没有固定限制，但平均情况下，在一个小时的会话中，通常每分钟不超过一次。 例如，在会话开始时，可对一个游戏进行 60 次读取或写入，但在不到一个小时的剩余时间内，将无法再进行读取或写入。 为了应对以后更多的调用，要对游戏进行强化，以防 Xbox LIVE 服务需要限制请求。

若游戏具有特殊的分区需求，例如额外的读取或写入，请联系 Microsoft。

<a name="ID4E5EAC"></a>

## <a name="using-title-storage"></a>使用标题存储

要开始使用标题存储，首先确定你想要存储的数据类型。 一些示例如下：保存的游戏、游戏状态、每日挑战、游戏地图，及艺术资源。

接下来，确定访问数据所需的游戏和平台。 标题存储支持来自单个平台上的单个标题和来自多个平台上的多个标题的云数据访问。

最后，使用本部分中的主题来配置存储、上传数据，并根据你的选择设置正确的访问权限。

<a name="ID4EJFAC"></a>

## <a name="in-this-section"></a>本部分内容

[在 Xbox Live 标题存储中读取配置 Blob](reading-configuration-blobs.md)  
演示从 Xbox Live 标题存储中读取配置 blob。

[在 Xbox Live 标题存储中存储二进制文件 blob](storing-binary-blobs.md)  
演示在 Xbox Live 标题存储中存储二进制文件 blob。

[在 Xbox Live 标题存储中读取二进制文件 blob](reading-binary-blobs.md)  
演示从 Xbox Live 标题存储中读取二进制 blob。

[在 Xbox Live 标题存储中存储 JSON Blob](storing-jsonblobs.md)  
演示在 Xbox Live 标题存储中存储 JSON blob。

[在 Xbox Live 标题存储中读取 JSON Blob](reading-jsonblobs.md)  
演示从 Xbox Live 标题存储中读取 JSON blob。

<a name="ID4E4FAC"></a>
