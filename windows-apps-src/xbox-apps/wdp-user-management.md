---
title: Xbox Live 测试用户管理 API 参考
description: 了解如何以编程方式访问用户管理 API。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 70876ab6-8222-4940-b4fb-65b581a77d6a
ms.openlocfilehash: 52f333af73084ed14982b9d09b6770c8294980f7
ms.sourcegitcommit: 6169660ea437915265165c4631d9702587e4793d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/06/2019
ms.locfileid: "74902530"
---
# <a name="xbox-live-user-management"></a>Xbox Live 用户管理

## <a name="request"></a>请求

你可以获取主机中的用户列表，也可以更新该列表，如添加、删除、登录、注销或修改现有用户。

| 方法        | 请求 URI     | 
| ------------- |-----------------|
| 获取           | /ext/user |
| PUT           | /ext/user |


**URI 参数**

* 无

**请求标头**

* 无

**请求正文**

调用 PUT 应包含具有以下结构的 JSON 数组：

* 用户
  * AutoSignIn（可选）：用于禁用或启用自动登录 EmailAddress 或 UserId 指定帐户的布尔值。
  * EmailAddress（可选，但如果未提供 UserId，则必须提供，除非登录的是赞助用户）：用于指定要修改/添加/删除的用户的电子邮件。
  * 密码（可选，但如果用户当前不在主机上，则必须提供）：用于向主机添加新用户的密码。
  * SignedIn（可选）：用于指定是否应登录或注销所提供帐户的布尔值。
  * UserId（可选，但如果未提供 EmailAddress，则必须提供，除非登录的是赞助用户）：用于指定要修改/添加/删除的用户的 UserId。
  * SponsoredUser（可选）：用于指定是否添加赞助用户的布尔值。
  * Delete （可选）：指定从控制台中删除此用户的布尔值

## <a name="response"></a>响应

**响应正文**

调用 GET 将返回具有以下属性的 JSON 数组：

* 用户
  * AutoSignIn（可选）
  * EmailAddress（可选）
  * Gamertag
  * SignedIn
  * UserId
  * XboxUserId
  * SponsoredUser（可选）
  
**状态代码**

此 API 具有以下预期状态代码。

| HTTP 状态代码   | 描述     | 
| ------------------ |-----------------|
| 200                | 调用 GET 成功，并且在响应正文中已返回用户的 JSON 数组 |
| 204                | 调用 PUT 成功，并且主机中的用户已更新 |
| 4XX                | 无效请求数据或格式的各种错误 |
| 5XX                | 意外失败的错误代码 |
