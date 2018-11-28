---
title: Device Portal 网络凭据 API 参考
description: 了解如何添加、 删除或以编程方式更新网络凭据。
ms.localizationpriority: medium
ms.openlocfilehash: 2da8dae554a0dcbb84d3d3fc3873e2fb035175dc
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/27/2018
ms.locfileid: "7836232"
---
# <a name="network-credentials-api-reference"></a>网络凭据的 API 参考
你可以添加、 删除或更新在使用此 REST API 在开发工具包上的存储的网络凭据。

## <a name="get-existing-credentials"></a>获取现有凭据

**请求**

你可以获取具有该网络共享凭据的用户的用户名以及存储的共享的列表。

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

- 采用以下格式的 JSON 数组：
* 凭据
  * NetworkPath-网络共享文件夹的路径。
  * 用户名-这具有存储凭据的用户名。

**状态代码**

此 API 具有以下预期状态代码。

HTTP 状态代码      | 说明
:------     | :-----
200 | 成功
4XX | 错误代码
5XX | 错误代码

## <a name="add-or-update-stored-credentials-for-a-user"></a>添加或更新为用户存储的凭据

**请求**

方法      | 请求 URI
:------     | :-----
POST | /ext/networkcredential
<br />
**URI 参数**

可以在请求 URI 上指定以下附加参数：

| URI 参数      | 描述     | 
| ------------------ |-----------------|
| NetworkPath        | 共享的网络路径要添加凭据来访问。 |
<br>

**请求标头**

- 无

**请求正文**

- 以下 JSON 元素：
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

## <a name="remove-stored-credentials-for-a-share"></a>删除共享存储的凭据。

**请求**

方法      | 请求 URI
:------     | :-----
DELETE | /ext/networkcredential
<br />
**URI 参数**

可以在请求 URI 上指定以下附加参数：

| URI 参数      | 描述     | 
| ------------------ |-----------------|
| NetworkPath        | 将从中删除存储的凭据的共享网络路径。 |
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
204 | 凭据的请求已成功。
4XX | 错误代码
5XX | 错误代码

<br />
**可用设备系列**

* Windows Xbox


