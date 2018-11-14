---
title: 存储二进制文件 blob
author: KevinAsgari
description: 了解如何在 Xbox Live 标题存储中存储二进制文件 blob。
ms.assetid: a0da36ef-5a5a-466d-80a8-e055ba7e4cdc
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, 标题存储
ms.localizationpriority: medium
ms.openlocfilehash: 3aa0598e724ffc0d919c9ad8b2ffdc3bb29d7d09
ms.sourcegitcommit: bdc40b08cbcd46fc379feeda3c63204290e055af
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/08/2018
ms.locfileid: "6161229"
---
# <a name="storing-a-binary-blob-in-xbox-live-title-storage"></a>在 Xbox Live 标题存储中存储二进制文件 blob

1.  使用以下方法发送请求，以向标题存储发送数据。

        PUT https://titlestorage.xboxlive.com/trustedplatform/users/xuid(1245111)/scids/{scid}/data/lastturn.bin,binary              
        Content-Type: application/octet-stream
        x-xbl-contract-version: 1
        Authorization: XBL3.0 x=<userHash>;<STSTokenString>
        Content-Length: 40
        Connection: Keep-Alive


-   用户必须在会话中才能更新。

-   STSTokenString 是为简便起见使用的占位符，应该将其替换为由身份验证请求返回的令牌。

2.  发送二进制数据。 由于数据将通过 HTTP 传输，因此，必须将数据限制为可接受的字符集。 图像或音频数据等信息必须进行编码。 你可以选择任何可生成 HTTP 兼容字符的编码方法。
d
```
  01EAEFBAD05903A4
  1EA2311656677DFF
  CF00
```

#### <a name="reference"></a>引用

**/trustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type}**
