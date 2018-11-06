---
title: TitleBlob (JSON)
assetID: fd1c904d-e8d0-f61f-e403-40b25bd4ac14
permalink: en-us/docs/xboxlive/rest/json-titleblob.html
author: KevinAsgari
description: " TitleBlob (JSON)"
ms.author: kevinasg
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a6ed09f078be5c7063d00d35d3bc3749f791d042
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/06/2018
ms.locfileid: "6030362"
---
# <a name="titleblob-json"></a>TitleBlob (JSON)
包含有关作品存储中的信息。 
<a id="ID4EP"></a>

 
## <a name="titleblob"></a>TitleBlob
 
TitleBlob 对象具有以下规范。
 
| 成员| 类型| 说明| 
| --- | --- | --- | 
| clientFileTime| DateTime| [可选]日期和时间的上次上传的文件。| 
| displayName| 字符串| [可选]向用户显示的文件的名称。| 
| etag| 字符串| 标记中使用的文件下载并上传请求。| 
| fileName| 字符串| 文件的名称。| 
| 大小| 64 位有符号整数| 文件以字节为单位的大小。| 
| smartBlobType| 字符串| [可选]数据类型。 可能的值为： 配置、 json、 二进制文件。| 
  
<a id="ID4E6C"></a>

 
## <a name="sample-json-syntax"></a>JSON 语法示例
 

```json
{
    "fileName":"foo\bar\blob.txt,binary",
    "clientFileTime":"2012-01-01T01:02:03.1234567Z",
    "displayName":"Friendly Name",
    "size":12,
    "etag":"0x8CEB3E4F8F3A5BF",
    "smartBlobType":"binary"
}
      
```

  
<a id="ID4EID"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EKD"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[JavaScript 对象表示法 (JSON) 对象参考](atoc-xboxlivews-reference-json.md)

   