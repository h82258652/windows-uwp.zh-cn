---
author: mcleanbyron
description: 本文介绍常见的应用程序和加载项，包括应用程序内购买、 授权和自行安装应用程序更新的存储操作错误代码。
title: Microsoft Store 操作错误代码
ms.author: mcleans
ms.date: 08/24/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10 uwp、 应用程序内购买，IAPs，加载项，错误代码
ms.localizationpriority: medium
ms.openlocfilehash: 0931397e24eaba44cdf04092f5f367c43a91dd82
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/13/2018
ms.locfileid: "959373"
---
# <a name="error-codes-for-store-operations"></a>Microsoft Store 操作错误代码

<!-- confirm whether symbolic names are defined for app developers, or do they just handle direct error code values -->

本文介绍开发或测试应用程序中的存储有关操作时可能会遇到的常见错误代码。

## <a name="in-app-purchase-error-codes"></a>应用程序内购买错误代码

与应用程序内购买操作相关的以下错误代码。

|  错误代码  |  描述  |
|--------------|---------------|
| 0x803F6100   | 无法完成应用程序内购买，因为孩子的角处于活动状态。 若要完成购买，登录到您的 Microsoft 帐户的设备，并再次运行应用程序。               |
| 0x803F6101   | 找不到指定的应用。 应用程序可能不能使用在存储中，或者您可能的应用程序提供错误的存储 ID。     |
| 0x803F6102   | 找不到指定的加载项。 加载项可能不再可用的存储或您可能中的加载项提供了错误的存储 ID。                                               |
| 0x803F6103   | 找不到的指定的产品。 产品可能不再可用在存储中，或者您可能的产品提供错误的存储 ID。                                          |
| 0x803F6104   | 无法完成应用程序内购买，因为您运行的试用版的应用程序。 若要完成应用程序内购买，安装完整版的应用程序。               |
| 0x803F6105   | 无法完成应用程序内购买，因为您不使用 Microsoft 帐户登录。                                              |
| 0x803F6107   | 处理当前操作时，意外出现了问题。                                             |
| 0x803F6108   | 无法完成应用程序内购买，因为应用程序许可证缺少信息。 当您的应用程序负载端时，会发生此错误。 若要解决此问题，卸载应用程序，然后将其从要刷新的应用程序许可证的存储重新安装。                                          |
| 0x803F6109   | 无法完成消耗品的加载项履行，因为指定的数量大于余额。        |
| 0x803F610A   | 不支持指定的提供程序类型的存储用户帐户。                                            |
| 0x803F610B   | 不支持指定的存储操作。                                             |
| 0x803F610C   | 应用程序不支持指定的背景任务合同。                                             |
| 0x80040001   | 所提供的加载项产品列表 Id 无效。                        |
| 0x80040002   | 所提供的关键字列表无效。                   |
| 0x80040003   | 履行目标无效。                       |

## <a name="licensing-error-codes"></a>许可错误代码

下面的错误代码与许可应用程序或加载项的操作。

|  错误代码  |  描述  |
|--------------|---------------|
| 0x803F700C   | 设备当前处于脱机状态。 若要在设备处于脱机状态时，请使用此应用程序，打开您设置的存储和切换**脱机权限**设置。            |
| 0x803F8001   | 您没有该产品的权利。 您可能使用不同的 Microsoft 帐户晚于用于购买的产品。           |
| 0x803F8002   | 您的产品的权利已过期。           |
| 0x803F8003   | 您的产品的权利处于无效状态，防止创建许可证。   |
| 0x803F8009<br/>0x803F800A   | 应用程序的试用期已过期。   |
| 0x803F8190   |  许可证不允许使用中的当前国家或地区设备的产品。  |
| 0x803F81F5<br/>0x803F81F6<br/>0x803F81F7<br/>0x803F81F8<br/>0x803F81F9   |  已到达可用于游戏和存储区中的应用程序的设备的最大的数量。 要在当前的设备上使用该游戏或应用程序，请先从您的帐户中删除另一台设备。  |
| 0x803F9000<br/>0x803F9001    |  许可证已过期或已损坏。 为了解决此错误，请尝试运行[Windows 应用程序的疑难解答](https://support.microsoft.com/help/4027498/windows-run-the-troubleshooter-for-windows-apps)重置存储缓存。     |
| 0x803F9006    |  无法完成操作，因为有权此产品的用户未登录到其 Microsoft 帐户的设备。            |
| 0x803F9008<br/>0x803F9009    |  您的设备处于脱机状态。 您的设备需要处于联机状态才能使用此产品。            |
| 0x803F900A    |  订阅已过期。            |


## <a name="self-install-update-error-codes"></a>自行安装更新错误代码

下面的错误代码与[自行安装包更新](../packaging/self-install-package-updates.md)。

|  错误代码  |  描述  |
|--------------|---------------|
| 0x803F6200   | 用户同意需要下载包更新。               |
| 0x803F6201   | 用户同意需要下载并安装包更新。                                                  |
| 0x803F6203   | 用户同意需要安装包更新。                                         |
| 0x803F6204   | 用户同意需要下载程序包更新，因为在按流量计费的网络连接，则会发生下载。                                             |
| 0x803F6206   | 用户同意需要下载并安装程序包更新，因为在按流量计费的网络连接，则会发生下载。     |


## <a name="related-topics"></a>相关主题

* [应用内购买和试用](in-app-purchases-and-trials.md)
* [获取应用和加载项的产品信息](get-product-info-for-apps-and-add-ons.md)
* [获取应用和加载项的许可证信息](get-license-info-for-apps-and-add-ons.md)
* [支持应用内购买应用和加载项](enable-in-app-purchases-of-apps-and-add-ons.md)
* [启用易耗型加载项购买](enable-consumable-add-on-purchases.md)
* [为应用启用订阅加载项](enable-subscription-add-ons-for-your-app.md)
* [实现应用的试用版](implement-a-trial-version-of-your-app.md)
