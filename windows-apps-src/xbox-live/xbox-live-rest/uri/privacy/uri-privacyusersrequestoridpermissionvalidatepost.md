---
title: POST (/users/{requestorId}/permission/validate)
assetID: 7a5ea583-ffca-5da7-a02a-535c52535928
permalink: en-us/docs/xboxlive/rest/uri-privacyusersrequestoridpermissionvalidatepost.html
description: " POST (/users/{requestorId}/permission/validate)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: edd91560ffb5d81b30da4b1453612cc5853a456f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623422"
---
# <a name="post-usersrequestoridpermissionvalidate"></a>POST (/users/{requestorId}/permission/validate)
获取有关是否允许用户执行指定的操作的目标用户集使用的是或否答案的一组。

  * [备注](#ID4EQ)
  * [URI 参数](#ID4ECB)
  * [Authorization](#ID4ENB)
  * [所需的请求标头](#ID4ESC)
  * [请求正文](#ID4E4D)
  * [HTTP 状态代码](#ID4ETE)
  * [所需的响应标头](#ID4EIG)
  * [响应正文](#ID4E5H)

<a id="ID4EQ"></a>


## <a name="remarks"></a>备注

请求正文接受用户的列表和设置的列表，并且结果为每个用户/设置对允许/阻止的结果。

在网络间的多玩家方案 （其中隐私通信检查必须执行之间有 Xbox 用户 ID (XUID) 的用户和关闭网络用户的不这样做），请参阅[PermissionCheckBatchRequest (JSON)](../../json/json-permissioncheckbatchrequest.md)对于用户类型。

<a id="ID4ECB"></a>


## <a name="uri-parameters"></a>URI 参数

| 参数| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- |
| requestorId| 字符串| 必需。 在执行操作的用户的标识符。 可能的值为<code>xuid({xuid})</code>和<code>me</code>。 这必须是已登录的用户。 示例值： <code>xuid(0987654321)</code>。|

<a id="ID4ENB"></a>


## <a name="authorization"></a>授权

使用授权声明 | 声明| 在任务栏的搜索框中键入| 是否为必需？| 示例值|
| --- | --- | --- | --- | --- | --- | --- |
| xuid| 64 位有符号的整数| 是| 1234567890|

<a id="ID4ESC"></a>


## <a name="required-request-headers"></a>所需的请求标头

| 标头| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 授权| 字符串| HTTP 身份验证的身份验证凭据。 示例值： <code>XBL3.0 x=&lt;userhash>;&lt;token></code>|
| X-RequestedServiceVersion| 字符串| 生成此请求应定向到 Xbox LIVE 的服务的名称/编号。 验证标头中的身份验证令牌等的声明的有效性后仅为将请求路由到该服务。示例值：1.|

<a id="ID4E4D"></a>


## <a name="request-body"></a>请求正文

<a id="ID4EDE"></a>


### <a name="required-members"></a>所需的成员

请参阅[PermissionCheckBatchRequest (JSON)](../../json/json-permissioncheckbatchrequest.md)。


```cpp
{
    "users":
    [
        {"xuid":"12345"},
        {"xuid":"54321"}
    ],
    "permissions":
    [
        "ViewTargetGameHistory",
        "ViewTargetProfile"
    ]
}

```


<a id="ID4ETE"></a>


## <a name="http-status-codes"></a>HTTP 状态代码

服务将返回其中一个状态代码在本部分中使用此方法在此资源上发出的请求的响应中。 有关与 Xbox Live 服务一起使用的标准 HTTP 状态代码的完整列表，请参阅[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)。

| 代码| 原因短语| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| 确定| 已成功检索该会话。|
| 400| 请求无效。| 示例： 设置不正确 Id、 不正确的 Uri，等等。|
| 404| 在 URI 中指定的用户不存在。| 找不到指定的资源。|

<a id="ID4EIG"></a>


## <a name="required-response-headers"></a>所需的响应标头

| 标头| 在任务栏的搜索框中键入| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 内容类型| 字符串| 请求正文的 MIME 类型。 示例值： <code>application/json</code>|
| 内容长度| 字符串| 在响应中发送的字节数。 示例值：34|
| Cache-Control| 字符串| 正常请求从服务器以指定缓存行为。 示例： <code>no-cache, no-store</code>|

<a id="ID4E5H"></a>


## <a name="response-body"></a>响应正文

请参阅[PermissionCheckBatchResponse (JSON)](../../json/json-permissioncheckbatchresponse.md)。

<a id="ID4ELAAC"></a>


### <a name="sample-response"></a>示例响应


```cpp
{
    "responses":
    [
        {
            "user": {"xuid":"12345"},
            "permissions":
            [
                {
                    "isAllowed":true
                },
                {
                    "isAllowed":true
                }
            ]
        },
        {
            "user": {"xuid":"54321"},
            "permissions":
            [
                {
                    "isAllowed":false,
                    "reasons":
                    [
                        {"reason":"NotAllowed"}
                    ]
                },
                {
                    "isAllowed":false,
                    "reasons":
                    [
                        {"reason":"PrivilegeRest", "restrictedSetting":"AllowProfileViewing"}
                    ]
                }
            ]
        }
    ]
}

```


<a id="ID4EVAAC"></a>


## <a name="see-also"></a>另请参阅

<a id="ID4EXAAC"></a>


##### <a name="parent"></a>Parent 的子磁盘）

[/users/{requestorId}/permission/validate](uri-privacyusersrequestoridpermissionvalidate.md)

 [PermissionId 枚举](../../enums/privacy-enum-permissionid.md)
