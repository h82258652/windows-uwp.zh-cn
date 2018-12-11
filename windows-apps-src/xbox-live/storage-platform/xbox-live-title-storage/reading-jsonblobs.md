---
title: 读取 JSON blob
description: 了解如何在 Xbox Live 标题存储中读取 JSON blob。
ms.assetid: 3697af16-d054-4835-af7f-7fee8c628345
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 30d2b9f6539e73df1c5ea5ae18b3d1a6ca061d60
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2018
ms.locfileid: "8881885"
---
# <a name="reading-a-json-blob-in-xbox-live-title-storage"></a>在 Xbox Live 标题存储中读取 JSON blob

1.  使用 *GET* 方法发送请求，以从标题存储中读取数据。 此示例使用全局标题存储。

        GET https://titlestorage.xboxlive.com/global/scids/{scid}/data/surprise.json,json
        Content-Type: application/octet-stream
        x-xbl-contract-version: 1
        Authorization: XBL3.0 x=<userHash>;<STSTokenString>
        Connection: Keep-Alive

-   用户必须在会话中才能更新。

-   STSTokenString 是为简便起见使用的占位符，应该将其替换为由身份验证请求返回的令牌。

#### <a name="reference"></a>引用

**/global/scids/{scid}/data/{pathAndFileName},{type}**
