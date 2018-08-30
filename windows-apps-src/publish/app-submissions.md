---
author: jnHs
Description: Once you've created your app by reserving a name, you can start working on getting it published. The first step is to create a submission.
title: 应用提交
ms.assetid: 363BB9E4-4437-4238-A80F-ABDFC70D96E4
keywords: 清单, Windows, uwp, 提交, 游戏, 应用
ms.author: wdg-dev-content
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9802577f9252b590657406bcb59b0c28adeb4781
ms.sourcegitcommit: 7efffcc715a4be26f0cf7f7e249653d8c356319b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/30/2018
ms.locfileid: "3115928"
---
# <a name="app-submissions"></a>应用提交


[通过保留名称创建应用](create-your-app-by-reserving-a-name.md)后，你可以开始着手其发布工作。 第一步是创建**提交**。

你可以在应用完成创建并准备发布时开始提交，甚至也可以在编写任一代码行之前开始输入信息。 保存你对你的提交的更新，以便你可以返回，并在其上工作，只要你准备就绪。

> [!NOTE]
> 你必须拥有[开发人员帐户](http://go.microsoft.com/fwlink/p/?LinkId=615100)才能访问[Windows 开发人员中心](https://partner.microsoft.com/dashboard)和提交到 Microsoft 应用商店的应用。

发布完应用后，你可以通过在仪表板中创建另一个提交来发布更新版本。 通过创建新提交，你可以做出并发布任何所需更改，无论你是要上载新程序包还是仅更改价格或类别等详细信息都是如此。 若要为已发布的应用创建新的提交，请单击“应用概述”页面上显示的最近提交旁边的**更新**。 你可以[删除从应用商店应用](guidance-for-app-package-management.md#removing-an-app-from-the-store)，如果你需要执行此操作 （并让其可再次更高版本，如果你想要）。

> [!NOTE]
> 此部分文档介绍如何在开发人员中心仪表板上创建应用提交。 此外，你也可以使用 [Microsoft Store 提交 API](../monetize/create-and-manage-submissions-using-windows-store-services.md) 自动执行应用提交。

## <a name="app-submission-checklist"></a>应用提交清单

以下是你在创建应用提交时可以提供的详细信息，其中包含指向更多信息的链接。

下面标出了你需要提供或指定的项目。 某些区域是可选的，或已提供你可以按需更改的默认值。 你无需在此处列出的顺序这些部分上工作。

### <a name="pricing-and-availability-page"></a>定价和可用性页面
| 字段名称                    | 注意                                       | 有关详细信息                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **市场**                   | 默认：所有可能的市场  | [定义价格和市场选择](define-pricing-and-market-selection.md)         |
| **受众**                | 默认：公共受众 | [受众](choose-visibility-options.md#audience) |
| **可发现性**                | 默认：在 Microsoft Store 中提供此应用，并使其可被发现 | [可发现性](choose-visibility-options.md#discoverability) |
| **计划**                  | 默认：尽快发布        | [配置精确的发布计划](configure-precise-release-scheduling.md) |
| **基价**                | 必填                                    | [设置和计划应用定价](set-and-schedule-app-pricing.md)              |
| **免费试用**                | 默认：没有免费试用                      | [免费试用](set-app-pricing-and-availability.md#free-trial)              |
| **销售定价**              | 可选                                    | [打折出售应用和加载项](put-apps-and-add-ons-on-sale.md)           |
| **组织授权**    | 默认：允许组织获取卷 | [组织许可选项](organizational-licensing.md)        |
      |


### <a name="properties-page"></a>属性页面

| 字段名称                    | 注意                                       | 有关详细信息                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **类别和子类别**  | 必需                                    | [类别和子类别表](category-and-subcategory-table.md)       |
| **隐私策略 URL**            | 在许多应用中是必需的。 请参阅[应用开发人员协议](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement)和 [Microsoft Store 策略](https://docs.microsoft.com/en-us/legal/windows/agreements/store-policies#105-personal-information) | [隐私策略 URL](enter-app-properties.md#privacy-policy-url)        |
| **网站**                   | 可选                                    | [网站](enter-app-properties.md#website)                   |
| **支持联系人信息**      | 如果你的产品适用于 Xbox，则这些为必填信息；否则为选填（但建议填写）信息                                   | [支持联系人信息](enter-app-properties.md#support-contact-info)              |
| **游戏设置**             | 可选（仅适用于游戏）         | [游戏设置](enter-app-properties.md#game-settings) |
| **显示模式**             | 可选                   | [显示模式](enter-app-properties.md#display-mode) |
| **产品声明**          | 默认：客户可以将此应用安装到备用驱动器或可移动存储；Windows 可以将此应用的数据包含在 OneDrive 的自动备份中 | [产品声明](app-declarations.md) |
| **系统要求**      | 可选                                    | [系统要求](enter-app-properties.md#system-requirements)      |

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
| **设备系列可用性** | 默认：基于你的程序包       | [设备系列可用性](device-family-availability.md) |
| **逐步部署程序包**   | 可选（仅适用于更新）            | [逐步部署程序包](gradual-package-rollout.md) |
| **强制更新**          | 可选（仅适用于更新）            | [强制更新](upload-app-packages.md#mandatory-update)


### <a name="store-listings"></a>应用商店一览

你需要你的应用支持的至少一种语言的所有必需信息。 我们建议提供应用支持的所有语言的 [Microsoft Store 一览](create-app-store-listings.md)，你还可以[提供其他语言的 Microsoft Store 一览](create-app-store-listings.md#store-listing-languages)。 若要轻松管理相同产品的多个一览，你可以[导入和导出 Microsoft Store 一览](import-and-export-store-listings.md)。

| 字段名称                    | 注意                                       | 有关详细信息                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **说明**               | 必需                                    | [编写出色的应用提要](write-a-great-app-description.md) |
| **此版本的最近更新**   | 可选                                 | [发行说明](create-app-store-listings.md#whats-new-in-this-version)       |
| **应用功能**              | 可选                                    | [应用功能](create-app-store-listings.md#app-features)         |
| **屏幕截图**               | 必需（至少一张屏幕截图；建议提供四张或更多）          | [屏幕截图](app-screenshots-and-images.md#screenshots)          |
| **Microsoft Store 徽标**               | 建议；对于某些操作系统版本为必需 | [Microsoft Store 徽标](app-screenshots-and-images.md#store-logos)             |
| **其他插图资源**     | 建议（尤其对于某些操作系统版本）         | [其他插图资源](app-screenshots-and-images.md#additional-art-assets) |
| **预告片**                  | 可选                                    | [预告片](app-screenshots-and-images.md#trailers)                | 
| **补充字段**  | 可选                                    | [补充信息](create-app-store-listings.md#supplemental-fields) 
| **搜索词**              | 可选                                    | [搜索词](create-app-store-listings.md#search-terms)         |
| **版权和商标信息** | 可选                                 | [版权和商标信息](create-app-store-listings.md#copyright-and-trademark-info) |
| **附加许可条款**  | 可选                                    | [附加许可证条款](create-app-store-listings.md#additional-license-terms) |
| **开发团队**              | 可选                                    | [开发团队](create-app-store-listings.md#developed-by)                   |
| **特定于平台的 Microsoft Store 一览** | 可选                               | [创建特定于平台的 Microsoft Store 一览](create-platform-specific-store-listings.md)  |

<span/>

### <a name="submission-options-page"></a>提交选项页面

| 字段名称                    | 注意                                       | 有关详细信息                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **发布暂停选项**     | 默认：通过认证后立即发布此提交（或按照计划部分中选择的日期发布）      | [发布暂停选项](manage-submission-options.md#publishing-hold-options)    
| **认证说明**     | 推荐          | [认证说明](notes-for-certification.md)             |
| **受限功能**     | 如果你的产品声明了任何[受限的功能](../packaging/app-capability-declarations.md#restricted-capabilities)所需    | [受限功能](manage-submission-options.md#publishing-hold-options)       

<span/>

> [!NOTE]
> 有关直接向企业发布业务线 (LOB) 应用的信息，请参阅[向企业分配 LOB 应用](distribute-lob-apps-to-enterprises.md)。
