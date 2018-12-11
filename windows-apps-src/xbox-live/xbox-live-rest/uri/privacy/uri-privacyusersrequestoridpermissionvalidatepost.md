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
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2018
ms.locfileid: "8899501"
---
# <a name="post-usersrequestoridpermissionvalidate"></a>POST (/users/{requestorId}/permission/validate)
获取有关是否允许用户执行一组的目标用户的指定的操作或否答案的一组。

  * [备注](#ID4EQ)
  * [URI 参数](#ID4ECB)
  * [授权](#ID4ENB)
  * [需的请求标头](#ID4ESC)
  * [请求正文](#ID4E4D)
  * [HTTP 状态代码](#ID4ETE)
  * [所需的响应标头](#ID4EIG)
  * [响应正文](#ID4E5H)

<a id="ID4EQ"></a>


## <a name="remarks"></a>备注

请求正文中获取用户列表和列表的设置，并结果是为每个用户/设置对允许/阻止结果。

在多人游戏 （其中隐私通信检查必须执行用户具有 Xbox 用户 ID (XUID) 和关闭网络用户并不之间） 方案中跨网络，请参阅针对用户类型[PermissionCheckBatchRequest (JSON)](../../json/json-permissioncheckbatchrequest.md) 。

<a id="ID4ECB"></a>


## <a name="uri-parameters"></a>URI 参数

| 参数| 类型| 描述|
| --- | --- | --- |
| requestorId| 字符串| 必需。 执行该操作的用户的标识符。 可能的值为<code>xuid({xuid})</code>和<code>me</code>。 这必须是已登录的用户。 示例值： <code>xuid(0987654321)</code>。|

<a id="ID4ENB"></a>


## <a name="authorization"></a>授权

使用授权声明 | 声明| 类型| 是否必需？| 示例值|
| --- | --- | --- | --- | --- | --- | --- |
| Xuid| 64 位有符号整数| 是| 1234567890|

<a id="ID4ESC"></a>


## <a name="required-request-headers"></a>需的请求标头

| 标头| 类型| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 授权| 字符串| HTTP 身份验证的身份验证凭据。 示例值： <code>XBL3.0 x=&lt;userhash>;&lt;token></code>|
| X RequestedServiceVersion| 字符串| 名称/的内部版本号应指向此请求的 Xbox LIVE 的服务。 验证在标头、 身份验证令牌等中的声明的有效性后仅为请求路由到该服务。示例值： 1。|

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

此部分中使用此方法对此资源所做的请求的响应，该服务返回的状态代码之一。 有关使用 Xbox Live 服务的标准 HTTP 状态代码的完整列表，请参阅[标准 HTTP 状态代码](../../additional/httpstatuscodes.md)。

| 代码| 原因短语| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| “确定”| 已成功检索会话。|
| 400| 请求无效。| 示例： 设置不正确 Id、 不正确的 Uri 等。|
| 404| URI 中指定的用户不存在。| 找不到指定的资源。|

<a id="ID4EIG"></a>


## <a name="required-response-headers"></a>所需的响应标头

| 标头| 类型| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Content-Type| 字符串| 请求正文中的 MIME 类型。 示例值： <code>application/json</code>|
| Content-Length| 字符串| 正在发送响应中的字节数。 示例值： 34|
| 缓存控制| 字符串| 礼貌请求从服务器指定缓存行为。 示例： <code>no-cache, no-store</code>|

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
