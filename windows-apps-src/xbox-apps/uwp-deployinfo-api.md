---
author: WilliamsJason
title: Device Portal 部署信息 API 参考
description: 了解如何以编程方式访问部署信息 API。
ms.localizationpriority: medium
ms.openlocfilehash: c0e8c6ea8fb42c6e11de8002da4b6c78d35e675b
ms.sourcegitcommit: c104b653601d9b81cfc8bb6032ca434cff8fe9b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/25/2018
ms.locfileid: "1921165"
---
# <a name="requests-deployment-information-for-one-or-more-installed-packages"></a>请求一个或多个已安装程序包的部署信息。

**请求**

方法      | 请求 URI
:------     | :------
POST | /ext/app/deployinfo
<br />
**URI 参数**

 - 无

**请求标头**

- 无

**请求正文**

以下格式的 JSON 数组：

* DeployInfo
  * PackageFullName - 我们正请求其信息的程序包的名称。
  * OverlayFolder - 重叠文件夹路径的可选路径（如果使用此功能）。

###<a name="response"></a>响应

**响应正文**

以下格式的 JSON 数组（部分字段可选）：

* DeployInfo
  * PackageFullName - 我们正接收相关信息的程序包的名称。
  * DeployType - 部署类型。
  * DeployPathOrSpecifiers - 松散部署的部署路径或者打包部署的安装说明符。
  * DeployDrive - 为适用部署类型将程序包部署到的驱动器。
  * DeploySizeInBytes - 以字节表示的适用部署类型程序包的大小。
  * OverlayFolder - 支持此功能的部署的重叠文件夹。

**状态代码**

此 API 具有以下预期状态代码。

HTTP 状态代码      | 说明
:------     | :-----
200 | 成功
4XX | 错误代码
5XX | 错误代码
<br />

**可用设备系列**

* Windows Xbox