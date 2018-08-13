---
author: WilliamsJason
title: 设备门户网络凭据 API 参考
description: 了解如何添加、 删除或以编程方式更新网络凭据。
ms.localizationpriority: medium
ms.openlocfilehash: 7e00169f92ee6f0aa48df64ec4a1186f9682b358
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/13/2018
ms.locfileid: "410114"
---
# <a name="network-credentials-api-reference"></a>网络凭据 API 参考 （英文）
您可以添加、 删除或更新使用此 REST API 您 devkit 存储的网络凭据。

## <a name="get-existing-credentials"></a>获取现有凭据

**请求**

您可以获取用户拥有的网络共享的凭据的用户名以及存储的共享的列表。

方法      | 请求 URI
:------     | :-----
GET | /ext/networkcredential
<br />
**URI 参数**

- 无

**请求标头**

- 无

**请求正文**   

- 无

**响应**   

- JSON 数组按以下格式：
* 凭据
  * NetworkPath-网络共享文件夹的路径。
  * 用户名-已存储凭据的用户名。

**状态代码**

此 API 具有以下预期状态代码。

HTTP 状态代码      | 说明
:------     | :-----
200 | 成功
4XX | 错误代码
5XX | 错误代码

## <a name="add-or-update-stored-credentials-for-a-user"></a>添加或更新的用户存储的凭据

**请求**

方法      | 请求 URI
:------     | :-----
POST | /ext/networkcredential
<br />
**URI 参数**

可以在请求 URI 上指定以下附加参数：

| URI 参数      | 说明     | 
| ------------------ |-----------------|
| NetworkPath        | 共享文件夹的网络路径要添加的凭据以访问。 |
<br>

**请求标头**

- 无

**请求正文**

- 下面的 JSON 元素：
* NetworkPath-网络共享文件夹的路径。
* 用户名-用于存储凭据的用户名。
* 密码-此用户的新的或更新密码。

**响应**   

- 无  

**状态代码**

此 API 具有以下预期状态代码。

HTTP 状态代码      | 说明
:------     | :-----
204 | 成功
4XX | 错误代码
5XX | 错误代码

## <a name="remove-stored-credentials-for-a-share"></a>删除针对共享存储的凭据。

**请求**

方法      | 请求 URI
:------     | :-----
DELETE | /ext/networkcredential
<br />
**URI 参数**

可以在请求 URI 上指定以下附加参数：

| URI 参数      | 说明     | 
| ------------------ |-----------------|
| NetworkPath        | 指向要从中删除存储的凭据的共享的网络路径。 |
<br>

**请求标头**

- 无

**请求正文**   

- 无

**响应**   

- 无 

**状态代码**

此 API 具有以下预期状态代码。

HTTP 状态代码      | 说明
:------     | :-----
204 | 为凭据请求是成功的。
4XX | 错误代码
5XX | 错误代码

<br />
**可用设备系列**

* Windows Xbox


