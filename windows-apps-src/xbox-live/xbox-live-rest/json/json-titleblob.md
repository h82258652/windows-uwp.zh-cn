---
title: TitleBlob (JSON)
assetID: fd1c904d-e8d0-f61f-e403-40b25bd4ac14
permalink: en-us/docs/xboxlive/rest/json-titleblob.html
description: " TitleBlob (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 51a0b17a46d1c71ffdf9098d4637ca59d840c90a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57612582"
---
# <a name="titleblob-json"></a>TitleBlob (JSON)
包含有关从存储的标题信息。 
<a id="ID4EP"></a>

 
## <a name="titleblob"></a>TitleBlob
 
TitleBlob 对象具有以下规范。
 
| 成员| 在任务栏的搜索框中键入| 描述| 
| --- | --- | --- | 
| clientFileTime| DateTime| [可选]日期和时间的最后一个上传文件。| 
| displayName| 字符串| [可选]向用户显示文件的名称。| 
| Etag| 字符串| 标记中使用的文件下载和上传请求。| 
| fileName| 字符串| 文件的名称。| 
| 大小| 64 位有符号的整数| 以字节为单位的文件的大小。| 
| smartBlobType| 字符串| [可选]数据类型。 可能的值为： 的配置来说，json，二进制。| 
  
<a id="ID4E6C"></a>

 
## <a name="sample-json-syntax"></a>示例 JSON 语法
 

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

[JavaScript 对象表示法 (JSON) 对象引用](atoc-xboxlivews-reference-json.md)

   