---
title: Xbox Live 测试用户管理 API 参考
description: 了解如何以编程方式访问用户管理 API。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 70876ab6-8222-4940-b4fb-65b581a77d6a
ms.openlocfilehash: c934a88dd1825fb0111083d71eb25e477956d79c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627362"
---
#<a name="xbox-live-user-management"></a>Xbox Live 用户管理 #

**请求**

你可以获取主机中的用户列表，也可以更新该列表，如添加、删除、登录、注销或修改现有用户。

| 方法        | 请求 URI     | 
| ------------- |-----------------|
| GET           | /ext/user |
| PUT           | /ext/user |
<br>

**URI 参数**

* 无

**请求标头**

* 无

**请求正文**

调用 PUT 应包含具有以下结构的 JSON 数组：

* 用户
  * AutoSignIn（可选）：用于禁用或启用自动登录 EmailAddress 或 UserId 指定帐户的布尔值。
  * 电子邮件地址 (可选-除非赞助用户中的签名，如果未提供用户 Id，必须提供):指定修改/添加/删除用户的电子邮件地址。
  * 密码 (可选-必须提供用户当前不在控制台上):用于在控制台中添加新用户的密码。
  * SignedIn（可选）：用于指定是否应登录或注销所提供帐户的布尔值。
  * 用户 Id (可选-除非赞助用户中的签名，如果未提供电子邮件地址，必须提供):指定修改/添加/删除用户的 UserId。
  * SponsoredUser（可选）：用于指定是否添加赞助用户的布尔值。
  * （可选） 删除： 布尔值，指定要从控制台中删除此用户

###<a name="response"></a>响应 # # #

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
<br>


