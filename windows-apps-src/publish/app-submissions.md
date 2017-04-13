---
author: jnHs
Description: "通过保留名称创建应用后，你可以开始着手其发布工作。 第一步是创建提交。"
title: "应用提交"
ms.assetid: 363BB9E4-4437-4238-A80F-ABDFC70D96E4
keywords: "清单"
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 8d6764051a6f3d9d5c6d851f0ba15353da5f9d5e
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="app-submissions"></a>应用提交


[通过保留名称创建应用](create-your-app-by-reserving-a-name.md)后，你可以开始着手其发布工作。 第一步是创建**提交**。

你可以在应用完成创建并准备发布时开始提交，甚至也可以在编写任一代码行之前开始输入信息。 提交将保存在你的仪表板中，以便只要你准备就绪，就可以开始提交。

发布完应用后，你可以通过在仪表板中创建另一个提交来发布更新版本。 通过创建新提交，你可以做出并发布任何所需更改，无论你是要上载新程序包还是仅更改价格或类别等详细信息都是如此。 若要创建应用的新提交，单击在“应用概述”页面上显示的最新提交旁边的“更新”****。

> **注意**&nbsp;&nbsp;文档的此部分介绍了如何在开发人员中心仪表板上创建应用提交。 此外，你也可以使用 [Windows Store submission API](../monetize/create-and-manage-submissions-using-windows-store-services.md) 自动创建应用提交。

## <a name="app-submission-checklist"></a>应用提交清单


以下是你在创建应用提交时可以提供的详细信息，其中包含指向更多信息的链接。

下面标出了你需要提供或指定的项目。 某些区域是可选的，或已提供你可以按需更改的默认值。

### <a name="pricing-and-availability-page"></a>定价和可用性页面
| 字段名称                    | 注意                                       | 有关详细信息                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **基价**                | 必需                                    | [基价](set-app-pricing-and-availability.md#base-price)              |
| **免费试用**                | 默认：没有免费试用                      | [添加试用版和应用内购买](https://msdn.microsoft.com/library/windows/apps/jj193599)  |
| **市场和自定义价格** | 默认：所有潜在市场，无自定义价格 | [定义价格和市场选择](define-pricing-and-market-selection.md)              |
| **销售定价**              | 可选                                    | [打折出售应用和加载项](put-apps-and-add-ons-on-sale.md)                                       |
| **分发和可见性** | 默认：在应用商店中提供此应用 | [分发和可见性](set-app-pricing-and-availability.md#distribution-and-visibility) |
| **组织授权**    | 默认：允许组织获取卷 | [组织许可选项](organizational-licensing.md)                        |
| **发布日期**                | 默认：尽快发布      | [发布日期](set-app-pricing-and-availability.md#publish-date)          |

<span/>

### <a name="app-properties-page"></a>应用属性页面

| 字段名称                    | 注意                                       | 有关详细信息                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **类别和子类别**  | 必需                                    | [类别和子类别表](category-and-subcategory-table.md)       |
| **系统要求**      | 可选                                    | [系统要求](enter-app-properties.md#system-requirements)      |
| **应用声明**          | 默认：客户可以将此应用安装到备用驱动器或可移动媒体；Windows 可以将此应用的数据包含在 OneDrive 的自动备份中 | [应用声明](app-declarations.md) |

<span/>

### <a name="age-ratings-page"></a>年龄分级页面

| 字段名称                    | 注意                                       | 有关详细信息                          |
|-------------------------------|---------------------------------------------|----------------------------------------|
| **年龄分级**               | 必需                                    | [年龄分级](age-ratings.md)          |

<span/>

### <a name="packages-page"></a>程序包页面

| 字段名称                    | 注意                                  | 有关详细信息                          |
|-------------------------------|----------------------------------------|----------------------------------------|
| **程序包上传控制**    | 必需（至少一个程序包）        | [上传应用包](upload-app-packages.md) |
| **设备系列可用性** | 默认：基于你的程序包       | [设备系列可用性](upload-app-packages.md#device-family-availability) |
| **逐步部署程序包**   | 可选（仅适用于更新）            | [逐步部署程序包](gradual-package-rollout.md) |
| **强制更新**          | 可选（仅适用于更新）            | [强制更新](upload-app-packages.md#mandatory-update)

<span/>

### <a name="store-listings"></a>应用商店一览

你需要你的应用支持的至少一种语言的所有必需信息。 我们建议你提供应用支持的所有语言的[应用商店一览](create-app-store-listings.md)，你还可以[提供其他语言的应用商店一览](create-app-store-listings.md#store-listing-languages)。

| 字段名称                    | 注意                                       | 有关详细信息                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **说明**               | 必需                                    | [编写出色的应用提要](write-a-great-app-description.md) |
| **发行说明**             | 可选                                    | [发行说明](create-app-store-listings.md#release-notes)         |
| **屏幕截图**               | 必需（至少一张屏幕截图）          | [应用屏幕截图和图像](app-screenshots-and-images.md)       |
| **应用磁贴图标**             | 可选，但强烈建议用于 Windows Phone 8.1 及更早版本 | [应用磁贴图标](create-app-store-listings.md#app-tile-icon) |
| **促销插图**       | 可选                                    | [应用屏幕截图和图像](app-screenshots-and-images.md)       |
| **应用功能**              | 可选                                    | [功能](create-app-store-listings.md#app-features)               |
| **其他系统要求**      | 可选                                    | [其他系统要求](create-app-store-listings.md#additional-system-requirements) |
| **关键字**                  | 可选                                    | [关键字](create-app-store-listings.md#keywords)                   |
| **版权和商标信息** | 可选                                 | [版权和商标信息](create-app-store-listings.md#copyright-and-trademark-info) |
| **附加许可条款**  | 可选                                    | [附加许可证条款](create-app-store-listings.md#additional-license-terms) |
| **网站**                   | 可选                                    | [网站](create-app-store-listings.md#website)                     |
| **支持人员联系信息**      | 可选                                    | [支持人员联系信息](create-app-store-listings.md)                |
| **隐私策略**            | 在某些应用中是必需的。 请参阅[应用开发人员协议](https://msdn.microsoft.com/library/windows/apps/hh694058)和 [Windows 应用商店策略](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx#pol_10_5_1) | [隐私策略](create-app-store-listings.md#privacy-policy) |
| **特定于平台的应用商店一览** | 可选                               | [创建特定于平台的应用商店一览](create-platform-specific-store-listings.md) |

<span/>

### <a name="notes-for-certification-page"></a>认证说明页面

| 字段名称                    | 注意                                       | 有关详细信息                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **注意**                     | 可选                                    | [认证说明](notes-for-certification.md)             |

<span/>

**注意**&nbsp;&nbsp;有关直接向企业发布业务线 (LOB) 应用的信息，请参阅[向企业分配 LOB 应用](distribute-lob-apps-to-enterprises.md)。
