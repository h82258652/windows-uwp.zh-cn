---
author: Xansky
description: 本文介绍了常见的应用和加载项，其中包括应用内购买、 许可和自行安装应用更新的应用商店操作错误代码。
title: Microsoft Store 操作错误代码
ms.author: mhopkins
ms.date: 08/24/2017
ms.topic: article
keywords: windows 10，uwp，应用内购买，Iap，加载项，错误代码
ms.localizationpriority: medium
ms.openlocfilehash: 1a4eff890da48bd60405cadee2d7ecb92bb1b2fa
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/31/2018
ms.locfileid: "5876222"
---
# <a name="error-codes-for-store-operations"></a>Microsoft Store 操作错误代码

<!-- confirm whether symbolic names are defined for app developers, or do they just handle direct error code values -->

本文介绍常见开发或测试你的应用中的应用商店相关的操作时可能遇到的错误代码。

## <a name="in-app-purchase-error-codes"></a>应用内购买错误代码

与应用内购买的操作相关的以下错误代码。

|  错误代码  |  描述  |
|--------------|---------------|
| 0x803F6100   | 无法完成应用内购买，因为儿童园地处于活动状态。 若要完成购买，登录到你的 Microsoft 帐户的设备，并再次运行应用程序。               |
| 0x803F6101   | 找不到指定的应用。 应用可能不再提供可在应用商店，或者你可能会为应用提供错误的应用商店 ID。     |
| 0x803F6102   | 找不到指定的加载项。 加载项可能不再提供可在应用商店，或者你可能拥有提供错误的应用商店 ID 的加载项。                                               |
| 0x803F6103   | 找不到指定的产品。 产品可能不再提供可在应用商店，或者你可能会为产品提供错误的应用商店 ID。                                          |
| 0x803F6104   | 无法完成应用内购买，因为你正在运行的应用的试用版。 若要完成应用内购买，安装应用的完整版。               |
| 0x803F6105   | 无法完成应用内购买，因为你未登录 Microsoft 帐户。                                              |
| 0x803F6107   | 意外出现处理当前操作时。                                             |
| 0x803F6108   | 无法完成应用内购买，因为应用许可缺少的信息。 当你旁加载应用时，会发生此错误。 若要解决此问题，卸载应用，然后再从应用商店以刷新应用许可证重新安装。                                          |
| 0x803F6109   | 无法完成易耗型加载项的实施情况，因为指定的数量大于的剩余余额。        |
| 0x803F610A   | 不支持的应用商店用户帐户的指定的提供程序类型。                                            |
| 0x803F610B   | 指定的应用商店操作不受支持。                                             |
| 0x803F610C   | 该应用不支持指定的后台任务合约。                                             |
| 0x80040001   | 所提供的加载项的产品列表 Id 无效。                        |
| 0x80040002   | 所提供的关键字列表无效。                   |
| 0x80040003   | 实施目标无效。                       |

## <a name="licensing-error-codes"></a>授权错误代码

以下错误代码相关许可的应用或加载项的操作。

|  错误代码  |  描述  |
|--------------|---------------|
| 0x803F700C   | 设备是当前处于脱机状态。 若要在设备处于脱机状态时，请使用此应用，打开你的应用商店设置和切换**脱机权限**设置。            |
| 0x803F8001   | 你没有产品的权利。 你可能正在使用不同的 Microsoft 帐户而非在用于购买产品。           |
| 0x803F8002   | 你的产品的授权已过期。           |
| 0x803F8003   | 你的产品的授权处于无效状态，阻止创建许可证。   |
| 0x803F8009<br/>0x803F800A   | 应用在试用期内已过期。   |
| 0x803F8190   |  许可证不允许在当前的国家或地区你的设备中使用的产品。  |
| 0x803F81F5<br/>0x803F81F6<br/>0x803F81F7<br/>0x803F81F8<br/>0x803F81F9   |  你已经可以用于游戏和应用商店中的设备的最大数量。 若要在当前设备上使用此游戏或应用，首先从帐户中删除另一台设备。  |
| 0x803F9000<br/>0x803F9001    |  许可证已过期或损坏。 为了帮助解决此错误，请尝试运行[适用于 Windows 应用的疑难解答](https://support.microsoft.com/help/4027498/windows-run-the-troubleshooter-for-windows-apps)来重置的应用商店缓存。     |
| 0x803F9006    |  不会完成该操作，因为有权获得此产品的用户未登录到他们的 Microsoft 帐户的设备。            |
| 0x803F9008<br/>0x803F9009    |  你的设备处于脱机状态。 你的设备需要处于在线状态，以使用此产品。            |
| 0x803F900A    |  订阅已过期。            |


## <a name="self-install-update-error-codes"></a>自行安装更新错误代码

以下错误代码相关[自行安装的程序包更新](../packaging/self-install-package-updates.md)。

|  错误代码  |  描述  |
|--------------|---------------|
| 0x803F6200   | 若要下载的程序包更新，需要用户同意。               |
| 0x803F6201   | 用户同意需要下载并安装包更新。                                                  |
| 0x803F6203   | 安装程序包更新需要用户同意。                                         |
| 0x803F6204   | 若要下载的程序包更新，因为下载将会发生按流量计费的网络连接，需要用户同意。                                             |
| 0x803F6206   | 用户同意需要下载并安装程序包更新，因为下载将会发生按流量计费的网络连接。     |


## <a name="related-topics"></a>相关主题

* [应用内购买和试用](in-app-purchases-and-trials.md)
* [获取应用和加载项的产品信息](get-product-info-for-apps-and-add-ons.md)
* [获取应用和加载项的许可证信息](get-license-info-for-apps-and-add-ons.md)
* [支持应用内购买应用和加载项](enable-in-app-purchases-of-apps-and-add-ons.md)
* [启用易耗型加载项购买](enable-consumable-add-on-purchases.md)
* [为应用启用订阅加载项](enable-subscription-add-ons-for-your-app.md)
* [实现应用的试用版](implement-a-trial-version-of-your-app.md)
