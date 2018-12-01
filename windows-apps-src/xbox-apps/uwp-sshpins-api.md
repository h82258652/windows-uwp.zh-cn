---
title: 设备门户 SSH Pin 码 API 参考
description: 了解如何以编程方式删除所有受信任的 SSH Pin 码。
ms.localizationpriority: medium
ms.openlocfilehash: 1ddf15d3cdb4089a8ef010a4ae46d247a06a10d7
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "8335518"
---
# <a name="ssh-pins-api-reference"></a>SSH Pin 码 API 参考
可以使用此 REST API 删除开发工具包上的所有受信任 SSH Pin 码。

## <a name="remove-trusted-ssh-pins"></a>删除受信任的 SSH Pin 码

**请求**

方法      | 请求 URI
:------     | :-----
删除 | /ext/app/sshpins
<br />
**URI 参数**

- 无

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
204 | 清除 Pin 码的请求已成功。
4XX | 错误代码
5XX | 错误代码

<br />
**可用设备系列**

* Windows Xbox

