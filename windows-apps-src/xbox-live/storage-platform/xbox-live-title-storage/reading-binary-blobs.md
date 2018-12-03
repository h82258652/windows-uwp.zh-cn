---
title: 读取二进制文件 blob
description: 了解在 Xbox Live 标题存储中读取二进制文件 blob。
ms.assetid: 9b8e0c35-0cea-4491-bf30-22fad224f11b
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, 标题存储
ms.localizationpriority: medium
ms.openlocfilehash: e2a0be72142a3e11c7c680cfc998287f396c2afc
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2018
ms.locfileid: "8340448"
---
# <a name="reading-a-binary-blob-in-xbox-live-title-storage"></a>在 Xbox Live 标题存储中读取二进制文件 blob

1.  使用 *GET* 方法发送请求，以从标题存储中读取数据。 此示例使用全局标题存储。

        GET https://titlestorage.xboxlive.com/global/scids/{scid}/data/userinfo.bin,binary
        Content-Type: application/octet-stream
        x-xbl-contract-version: 1
        Authorization: XBL3.0 x=<userHash>;<STSTokenString>
        Connection: Keep-Alive



-   用户必须在会话中才能更新。

-   STSTokenString 是为简便起见使用的占位符，应该将其替换为由身份验证请求返回的令牌。

#### <a name="reference"></a>引用

**/global/scids/{scid}/data/{pathAndFileName},{type}**
