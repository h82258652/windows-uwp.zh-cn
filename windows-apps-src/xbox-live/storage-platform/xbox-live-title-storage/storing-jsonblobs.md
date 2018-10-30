---
title: 存储 JSON blob
author: KevinAsgari
description: 了解如何在 Xbox Live 标题存储中存储 JSON blob。
ms.assetid: f424aca1-e671-4e31-84c6-098fda4a9558
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, 标题存储
ms.localizationpriority: medium
ms.openlocfilehash: 629bc38e8d5e5afa4c9d78349587585d0701b3de
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2018
ms.locfileid: "5752135"
---
# <a name="storing-a-json-blob-in-xbox-live-title-storage"></a>在 Xbox Live 标题存储中存储 JSON blob

1.  使用 *PUT* 方法发送请求，以向标题存储发送数据。

        PUT https://titlestorage.xboxlive.com/json/users/xuid(1245111)/scids/{scid}/data/{pathAndFileName},json
        Content-Type: application/octet-stream
        x-xbl-contract-version: 1
        Authorization: XBL3.0 x=<userHash>;<STSTokenString>
        Content-Length: 240
        Connection: Keep-Alive



-   用户必须在会话中才能更新。

-   STSTokenString 是为简便起见使用的占位符，应该将其替换为由身份验证请求返回的令牌。

2.  发送 JSON 对象。

        {
            "startlevel":"1",
            "expression":"smile"
        }

#### <a name="reference"></a>引用

**/json/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},json)**
