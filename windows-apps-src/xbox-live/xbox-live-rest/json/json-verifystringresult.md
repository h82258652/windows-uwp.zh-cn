---
title: VerifyStringResult (JSON)
assetID: 272c688e-179e-c7e9-086b-e76d0d4bcb57
permalink: en-us/docs/xboxlive/rest/json-verifystringresult.html
description: " VerifyStringResult (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b01793222be80efccdca1f24f5226a2e9ff78064
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57599482"
---
# <a name="verifystringresult-json"></a>VerifyStringResult (JSON)
导致代码与提交到每个字符串对应[/system/strings/validate](../uri/stringserver/uri-systemstringsvalidate.md)。
<a id="ID4ER"></a>


## <a name="verifystringresult"></a>VerifyStringResult

VerifyStringResult 对象具有以下规范。

| 成员| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- |
| resultCode| 32 位无符号的整数| 必需。 HResult 代码对应于提交字符串。|
| offendingString| 字符串| 必需。 导致要拒绝的字符串的字符串值。|

<a id="ID4EXB"></a>


## <a name="sample-json-syntax"></a>示例 JSON 语法


```json
{
    "verifyStringResult":
    [
        {"resultCode": "1", "offendingString": "badword"},
        {"resultCode": "0", "offendingString": ""},
        {"resultCode": "0", "offendingString": ""},
        {"resultCode": "0", "offendingString": ""},
    ]
}

```


#### <a name="common-hresult-values"></a>常见的 HResult 值

| 值| 错误名称|
| --- | --- | --- | --- | --- |
| 0| 成功|
| 1| 冒犯性的字符串|
| 2| 字符串太长|
| 3| 未知的错误|

<a id="ID4ELD"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4END"></a>


##### <a name="parent"></a>Parent 的子磁盘）

[JavaScript 对象表示法 (JSON) 对象引用](atoc-xboxlivews-reference-json.md)


<a id="ID4EXD"></a>


##### <a name="reference"></a>参考

[POST （/系统/字符串/验证）](../uri/stringserver/uri-systemstringsvalidatepost.md)
