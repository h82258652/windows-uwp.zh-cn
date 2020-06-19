---
Description: 通过保留名称创建应用后，你可以开始着手其发布工作。 第一步是创建提交。
title: 应用提交
ms.assetid: 363BB9E4-4437-4238-A80F-ABDFC70D96E4
keywords: 清单, Windows, uwp, 提交, 游戏, 应用
ms.date: 10/31/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 39a71a54fcc2792b799f630a94d1382c01643536
ms.sourcegitcommit: 96b7be654a0922eeb421b5fa51ebfc586abe74fe
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/17/2020
ms.locfileid: "84945959"
---
# <a name="app-submissions"></a>应用提交


[通过保留名称创建应用](create-your-app-by-reserving-a-name.md)后，就可以开始使用它。 第一步是创建**提交**。

你可以在应用完成创建并准备发布时开始提交，甚至也可以在编写任一代码行之前开始输入信息。 你对提交的更新已保存，因此你可以在准备好时返回并处理。

> [!NOTE]
> 若要将应用提交到 Microsoft Store，必须在[合作伙伴中心](https://partner.microsoft.com/dashboard)具有有效的[开发人员帐户](https://developer.microsoft.com/store/register)。

发布应用后，可以通过在合作伙伴中心创建另一个提交来发布更新版本。 通过创建新提交，你可以做出并发布任何所需更改，无论你是要上载新程序包还是仅更改价格或类别等详细信息都是如此。 若要为发布的应用创建新的提交，请单击其 "**概述**" 页上显示的最新提交旁边的 "**更新**"。 如果需要，还可以[从应用商店中删除应用](guidance-for-app-package-management.md#removing-an-app-from-the-store)（如果需要，稍后再使其可用）。

> [!NOTE]
> 文档的本节介绍如何在合作伙伴中心创建应用提交。 此外，你也可以使用 [Microsoft Store 提交 API](../monetize/create-and-manage-submissions-using-windows-store-services.md) 自动执行应用提交。

> [!IMPORTANT]
> 你不能再上传使用 Windows Phone 3.x SDK 生成的新 XAP 包。 已存储在 XAP 包中的应用将在 Windows 10 移动版设备上继续运行。 有关详细信息，请参阅此[博客文章](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store)。

## <a name="app-submission-checklist"></a>应用提交清单

以下是你在创建应用提交时可以提供的详细信息，其中包含指向更多信息的链接。

下面标出了你需要提供或指定的项目。 某些区域是可选的，或已提供你可以按需更改的默认值。 不必按此处列出的顺序处理这些部分。

### <a name="pricing-and-availability-page"></a>定价和可用性页面
| 字段名称                    | 说明                                       | 有关详细信息                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **市场**                   | 默认：所有可能的市场  | [定义价格和市场选择](define-pricing-and-market-selection.md)         |
| **受众**                | 默认：公共受众 | [受众](choose-visibility-options.md#audience) |
| **可发现性**                | 默认：在 Microsoft Store 中提供此应用，并使其可被发现 | [可发现性](choose-visibility-options.md#discoverability) |
| **计划**                  | 默认：尽快发布        | [配置精确的发布计划](configure-precise-release-scheduling.md) |
| **基价**                | 必需                                    | [设置和计划应用定价](set-and-schedule-app-pricing.md)              |
| **免费试用**                | 默认：没有免费试用                      | [免费试用](set-app-pricing-and-availability.md#free-trial)              |
| **销售定价**              | 可选                                    | [打折出售应用和加载项](put-apps-and-add-ons-on-sale.md)           |
| **组织授权**    | 默认：允许组织获取卷 | [组织许可选项](organizational-licensing.md)        |
      |


### <a name="properties-page"></a>“属性”页

| 字段名称                    | 说明                                       | 有关详细信息                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **类别和子类别**  | 必需                                    | [类别和子类别表](category-and-subcategory-table.md)       |
| **隐私策略 URL**            | 在许多应用中是必需的。 请参阅[应用开发人员协议](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement)和 [Microsoft Store 策略](store-policies.md#105-personal-information) | [隐私策略 URL](enter-app-properties.md#privacy-policy-url)        |
| **网站**                   | 可选                                    | [网站](enter-app-properties.md#website)                   |
| **支持联系人信息**      | 如果你的产品适用于 Xbox，则这些为必填信息；否则为选填（但建议填写）信息                                   | [支持联系人信息](enter-app-properties.md#support-contact-info)              |
| **游戏设置**             | 可选（仅适用于游戏）         | [游戏设置](enter-app-properties.md#game-settings) |
| **显示模式**             | 可选                   | [显示模式](enter-app-properties.md#display-mode) |
| **产品声明**          | 默认：客户可以将此应用安装到备用驱动器或可移动存储；Windows 可以将此应用的数据包含在 OneDrive 的自动备份中 | [产品声明](app-declarations.md) |
| **系统要求**      | 可选                                    | [系统要求](enter-app-properties.md#system-requirements)      |

<span/>

### <a name="age-ratings-page"></a>年龄分级页面

| 字段名称                    | 说明                                       | 有关详细信息                          |
|-------------------------------|---------------------------------------------|----------------------------------------|
| **年龄分级**               | 必需                                    | [年龄分级](age-ratings.md)          |

<span/>

### <a name="packages-page"></a>程序包页面

| 字段名称                    | 说明                                  | 有关详细信息                          |
|-------------------------------|----------------------------------------|----------------------------------------|
| **程序包上传控制**    | 必需（至少一个程序包）        | [上传应用包](upload-app-packages.md) |
| **设备系列可用性** | 默认：基于你的程序包       | [设备系列可用性](device-family-availability.md) |
| **逐步软件包推出**   | 可选（仅适用于更新）            | [逐步软件包推出](gradual-package-rollout.md) |
| **强制更新**          | 可选（仅适用于更新）            | [强制更新](upload-app-packages.md#mandatory-update)


### <a name="store-listings"></a>应用商店一览

你需要你的应用支持的至少一种语言的所有必需信息。 我们建议提供应用支持的所有语言的 [Microsoft Store 一览](create-app-store-listings.md)，你还可以[提供其他语言的 Microsoft Store 一览](create-app-store-listings.md#store-listing-languages)。 若要轻松管理相同产品的多个一览，你可以[导入和导出 Microsoft Store 一览](import-and-export-store-listings.md)。

| 字段名称                    | 说明                                       | 有关详细信息                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **说明**               | 必需                                    | [编写出色的应用提要](write-a-great-app-description.md) |
| **此版本中的新增功能**   | 可选                                 | [发行说明](create-app-store-listings.md#whats-new-in-this-version)       |
| **应用功能**              | 可选                                    | [产品功能](create-app-store-listings.md#product-features)         |
| **屏幕截图**               | 必需（至少一张屏幕截图；建议提供四张或更多）          | [屏幕截图](app-screenshots-and-images.md#screenshots)          |
| **应用商店徽标**               | 建议；对于某些操作系统版本为必需 | [应用商店徽标](app-screenshots-and-images.md#store-logos)             |
| **预告片**                  | 可选                                    | [预告片](app-screenshots-and-images.md#trailers)                | 
| **Windows 10 和 Xbox 图像（16:9 超级人物图片）**     | 建议        | [Windows 10 和 Xbox 图像（16:9 超级人物图片）](app-screenshots-and-images.md#windows-10-and-xbox-image-169-super-hero-art) |
| **Xbox 图像**     | 如果发布到 Xbox，则需要正确的显示        | [Xbox 图像](app-screenshots-and-images.md#xbox-images) |
| **补充字段**  | 可选                                    | [补充字段](create-app-store-listings.md#supplemental-fields) 
| **搜索词**              | 可选                                    | [搜索词](create-app-store-listings.md#search-terms)         |
| **版权和商标信息** | 可选                                 | [版权和商标信息](create-app-store-listings.md#copyright-and-trademark-info) |
| **其他许可条款**  | 可选                                    | [其他许可条款](create-app-store-listings.md#additional-license-terms) |
| **开发者**              | 可选                                    | [开发者](create-app-store-listings.md#developed-by)                   |


<span/>

### <a name="submission-options-page"></a>提交选项页面

| 字段名称                    | 说明                                       | 有关详细信息                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **发布暂停选项**     | 默认：通过认证后立即发布此提交（或按照计划部分中选择的日期发布）      | [发布暂停选项](manage-submission-options.md#publishing-hold-options)    
| **认证说明**     | 建议          | [认证说明](notes-for-certification.md)             |
| **受限功能**     | 如果产品声明了任何[受限功能](../packaging/app-capability-declarations.md#restricted-capabilities)，则为必需    | [受限功能](manage-submission-options.md#publishing-hold-options)       

<span/>

> [!NOTE]
> 有关直接向企业发布业务线 (LOB) 应用的信息，请参阅[向企业分配 LOB 应用](distribute-lob-apps-to-enterprises.md)。
