---
title: 读取配置 blob
author: KevinAsgari
description: 了解如何在 Xbox Live 标题存储中读取配置 blob。
ms.assetid: ee62d221-69b9-4f52-9b5d-5a44d04de548
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: be118ff3bc12cd3dd541d6889daa70c7e5a8eab3
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2018
ms.locfileid: "5692533"
---
# <a name="reading-a-configuration-blob-in-xbox-live-title-storage"></a>在 Xbox Live 标题存储中读取配置 blob

*配置 blob* 是 Xbox Live 标题存储中保留游戏数据的文件。 这些数据是包括能够在传递到游戏之前进行筛选的虚拟节点的 JSON 对象。 有关配置 blob 的详细信息，请参阅**标题存储 URI**。

### <a name="to-read-a-configuration-blob"></a>要读取配置 blob

1.  使用以下方法发送请求，以从标题存储中读取数据。

        GET https://titlestorage.xboxlive.com/global/scids/{scid}/data/config.json,config              
        Content-Type: application/octet-stream
        x-xbl-contract-version: 1
        Authorization: XBL3.0 x=<userHash>;<STSTokenString>
        Connection: Keep-Alive


-   用户必须在会话中才能更新。
-   STSTokenString 是为简便起见使用的占位符，应该将其替换为由身份验证请求返回的令牌。

#### <a name="reference"></a>引用

**/global/scids/{scid}/data/{pathAndFileName},{type}**
**GET (/global/scids/{scid}/data/{pathAndFileName},{type})**
