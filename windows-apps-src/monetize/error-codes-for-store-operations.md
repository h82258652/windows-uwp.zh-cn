---
description: 本文介绍了应用和加载项的常见 Microsoft Store 操作错误代码，包括应用内购买、许可和自行安装应用更新。
title: Microsoft Store 操作错误代码
ms.date: 08/24/2017
ms.topic: article
keywords: windows 10, uwp, 应用内购买, IAP, 加载项, 错误代码
ms.localizationpriority: medium
ms.openlocfilehash: ba505b30076c356a39ae195e1d187cbc49d8a66a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662872"
---
# <a name="error-codes-for-store-operations"></a>Microsoft Store 操作错误代码

<!-- confirm whether symbolic names are defined for app developers, or do they just handle direct error code values -->

本文介绍了你在开发和测试应用中与 Microsoft Store 相关的操作时可能会遇到的常见错误代码。

## <a name="in-app-purchase-error-codes"></a>应用内购买错误代码

以下错误代码与应用内购买操作相关。

|  错误代码  |  描述  |
|--------------|---------------|
| 0x803F6100   | “儿童园地”处于活动状态，因此应用内购买无法完成。 若要完成购买，请使用 Microsoft 帐户登录设备并再次运行应用程序。               |
| 0x803F6101   | 找不到指定的应用。 此应用在 Microsoft Store 中不再可用，或者你为此应用提供的 Microsoft Store ID 错误。     |
| 0x803F6102   | 找不到指定的加载项。 此加载项在 Microsoft Store 中不再可用，或者你为此加载项提供的 Microsoft Store ID 错误。                                               |
| 0x803F6103   | 找不到指定的产品。 此产品在 Microsoft Store 中不再可用，或者你为此产品提供的 Microsoft Store ID 错误。                                          |
| 0x803F6104   | 你运行的是试用版应用，因此无法完成应用内购买。 若要完成应用内购买，请安装完整版的应用。               |
| 0x803F6105   | 你未使用 Microsoft 帐户登录，因此应用内购买无法完成。                                              |
| 0x803F6107   | 处理当前操作时出现意外。                                             |
| 0x803F6108   | 应用许可证缺少信息，因此应用内购买无法完成。 旁加载应用时发生此错误。 若要解决此问题，请卸载应用，然后从 Microsoft Store 重新安装，以刷新应用许可证。                                          |
| 0x803F6109   | 指定的数量超过剩余余额，因此易耗型加载项的实现无法完成。        |
| 0x803F610A   | 为此 Microsoft Store 用户帐户指定的提供商类型不受支持。                                            |
| 0x803F610B   | 不支持指定的 Microsoft Store 操作。                                             |
| 0x803F610C   | 此应用不支持指定的后台任务合约。                                             |
| 0x80040001   | 提供的加载项产品 ID 列表无效。                        |
| 0x80040002   | 提供的关键字列表无效。                   |
| 0x80040003   | 实现目标无效。                       |

## <a name="licensing-error-codes"></a>许可错误代码

以下错误代码与应用或加载项的许可操作相关。

|  错误代码  |  描述  |
|--------------|---------------|
| 0x803F700C   | 设备当前处于离线状态。 若要在设备离线时使用此应用，请打开 Microsoft Store 设备并切换**离线权限**设置。            |
| 0x803F8001   | 你对此产品没有权限。 你使用的 Microsoft 帐户与购买此产品时使用的帐户不同。           |
| 0x803F8002   | 你对此产品的权限已过期。           |
| 0x803F8003   | 你对此产品的权限处于无效状态，因此无法创建许可证。   |
| 0x803F8009<br/>0x803F800A   | 此应用的试用期已到期。   |
| 0x803F8190   |  许可证不允许在当前国家/地区或设备的所在区域使用此产品。  |
| 0x803F81F5<br/>0x803F81F6<br/>0x803F81F7<br/>0x803F81F8<br/>0x803F81F9   |  你已达到可与 Microsoft Store 中的游戏和应用同时使用的最大设备数量。 若要在当前设备上使用此游戏或应用，请先删除帐户中的其他设备。  |
| 0x803F9000<br/>0x803F9001    |  许可证已过期或损坏。 若要帮助解决此错误，请尝试在运行[Windows 应用程序故障排除工具](https://support.microsoft.com/help/4027498/windows-run-the-troubleshooter-for-windows-apps)以重置存储缓存。     |
| 0x803F9006    |  对此产品拥有权限的用户未使用其 Microsoft 帐户登录，因此操作无法完成。            |
| 0x803F9008<br/>0x803F9009    |  你的设备处于离线状态。 你的设备需要处于在线状态才能使用此产品。            |
| 0x803F900A    |  订阅已过期。            |


## <a name="self-install-update-error-codes"></a>自行安装更新错误代码

以下错误代码与[自行安装包更新](../packaging/self-install-package-updates.md)相关。

|  错误代码  |  描述  |
|--------------|---------------|
| 0x803F6200   | 需要用户同意才能下载包更新。               |
| 0x803F6201   | 需要用户同意才能下载和安装包更新。                                                  |
| 0x803F6203   | 需要用户同意才能安装包更新。                                         |
| 0x803F6204   | 将在按流量计费的网络连接上进行下载，因此需要用户同意才能下载包更新。                                             |
| 0x803F6206   | 将在按流量计费的网络连接上进行下载，因此需要用户同意才能下载和安装包更新。     |


## <a name="related-topics"></a>相关主题

* [应用内购买和试用版](in-app-purchases-and-trials.md)
* [获取产品信息的应用程序和外接程序](get-product-info-for-apps-and-add-ons.md)
* [获取应用程序和外接程序的许可证信息](get-license-info-for-apps-and-add-ons.md)
* [启用应用内购买的应用程序和外接程序](enable-in-app-purchases-of-apps-and-add-ons.md)
* [启用可使用外接程序购买](enable-consumable-add-on-purchases.md)
* [启用订阅外接程序为你的应用](enable-subscription-add-ons-for-your-app.md)
* [实现您的应用程序的试用版](implement-a-trial-version-of-your-app.md)
