---
author: payzer
title: "Device Portal SMB API 参考"
description: "了解如何以编程方式访问 SMB API。"
translationtype: Human Translation
ms.sourcegitcommit: 3d76bf181baa9dfd973467d43241230fddf2daf7
ms.openlocfilehash: d6a097c84e6b967c41507d9e60c266f0bacb93fe

---

# 开发人员文件夹 API 参考   
你可以使用标准文件资源管理器来访问 Xbox One 中与开发相关的文件。 这使你可以轻松查看文件，并将文件从电脑重新放置到主机。

**请求**

你可以利用以下请求来访问开发人员文件夹。 请求将返回：    
* 文件共享的位置。 此位置可以输入到文件资源管理器的地址栏中。
* 用于访问文件共享的用户名。
* 用于访问文件共享的密码。

方法      | 请求 URI
:------     | :-----
GET | /ext/smb/developerfolder
<br />
**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应**   
路径 - 指向文件开发人员文件共享的路径。   
用户名 - 访问开发人员文件共享所需的用户名。   
密码 - 访问开发人员文件共享所需的密码。   

**状态代码**

此 API 具有以下预期状态代码。

HTTP 状态代码      | 说明
:------     | :-----
200 | 已授予访问文件共享凭据的请求。
4XX | 错误代码
5XX | 错误代码
<br />
**可用设备系列**

* Windows Xbox



<!--HONumber=Aug16_HO3-->


