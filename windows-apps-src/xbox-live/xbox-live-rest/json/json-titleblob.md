---
title: TitleBlob (JSON)
assetID: fd1c904d-e8d0-f61f-e403-40b25bd4ac14
permalink: en-us/docs/xboxlive/rest/json-titleblob.html
author: KevinAsgari
description: " TitleBlob (JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 91423df8367c275f40cd7f856a60070e1a46ad40
ms.sourcegitcommit: 49aab071aa2bd88f1c165438ee7e5c854b3e4f61
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2018
ms.locfileid: "4470824"
---
# <a name="titleblob-json"></a>TitleBlob (JSON)
包含有关从存储游戏的信息。 
<a id="ID4EP"></a>

 
## <a name="titleblob"></a>TitleBlob
 
TitleBlob 对象具有以下规范。
 
| 成员| 类型| 描述| 
| --- | --- | --- | 
| clientFileTime| DateTime| [可选]日期和时间的上次上传的文件。| 
| displayName| 字符串| [可选]向用户显示的文件的名称。| 
| etag| 字符串| 标记中使用的文件下载并上传请求。| 
| fileName| 字符串| 文件的名称。| 
| 大小| 64 位有符号整数| 以字节为单位的文件大小。| 
| smartBlobType| 字符串| [可选]数据类型。 可能的值为： 配置，json、 二进制文件。| 
  
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

   