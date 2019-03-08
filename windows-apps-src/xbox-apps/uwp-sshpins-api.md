---
title: 设备门户 SSH Pin 码 API 参考
description: 了解如何以编程方式删除所有受信任的 SSH Pin 码。
ms.localizationpriority: medium
ms.topic: article
ms.date: 02/08/2017
ms.openlocfilehash: 2c7dc6fab021c11c98276ee53af161bea25601a9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57663352"
---
# <a name="ssh-pins-api-reference"></a>SSH Pin 码 API 参考
可以使用此 REST API 删除开发工具包上的所有受信任 SSH Pin 码。

## <a name="remove-trusted-ssh-pins"></a>删除受信任的 SSH Pin 码

**请求**

方法      | 请求 URI
:------     | :-----
DELETE | /ext/app/sshpins
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

HTTP 状态代码      | 描述
:------     | :-----
204 | 清除 Pin 码的请求已成功。
4XX | 错误代码
5XX | 错误代码

<br />
**可用的设备系列**

* Windows Xbox

