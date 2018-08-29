---
author: jnHs
Description: You can publish line-of-business (LOB) apps directly to enterprises for volume acquisition via the Microsoft Store for Business or Microsoft Store for Education, without making the apps broadly available in the Store.
title: 将 LOB 应用分配到企业
ms.assetid: 2050126E-CE49-4DE3-AC2B-A572AC895158
ms.author: wdg-dev-content
ms.date: 03/28/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, lob, 业务线, 企业应用, 适用于企业的 store, 适用于教育的 store, 企业
ms.localizationpriority: medium
ms.openlocfilehash: 9149533a12263e105356a1683257c4d9172eefb5
ms.sourcegitcommit: 3727445c1d6374401b867c78e4ff8b07d92b7adc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/29/2018
ms.locfileid: "2918073"
---
# <a name="distribute-lob-apps-to-enterprises"></a>将 LOB 应用分配到企业


可以通过适用于企业的 Microsoft 应用商店或适用于教育的 Microsoft 应用商店向企业直接发布业务线 (LOB) 应用以获取批量购置，无需在应用商店中使应用广泛可用。

> [!NOTE]
> 目前，仅免费应用可以通过适用于企业的 Microsoft 应用商店或适用于教育的 Microsoft 应用商店专门分配到企业。 如果你提交了作为 LOB 的某个付费应用，它不会向企业提供。 

> [!IMPORTANT]
> 你无法使用 [Microsoft Store 提交 API](../monetize/create-and-manage-submissions-using-windows-store-services.md) 直接向企业发布 LOB 应用。 LOB 应用的所有提交都必须使用 Windows 开发人员中心仪表板进行。


## <a name="set-up-the-enterprise-association"></a>设置企业关联

将 LOB 应用专门发布到企业的第一步是建立你的帐户和企业的专用应用商店之间的关联。

> [!IMPORTANT]
> 此关联过程必须由企业启动，并且必须使用与用于创建开发人员帐户的 Microsoft 帐户关联的电子邮件地址。 有关详细信息，请参阅[使用业务线应用](http://go.microsoft.com/fwlink/p/?LinkId=698846)。

在企业选择邀请你发布供其专门使用的应用时，你将收到包含确认该关联的链接的电子邮件。 你还可以通过转到**帐户设置**的**企业关联**部分来确认这些关联（只要你已使用用于打开开发人员帐户的 Microsoft 帐户登录）。

若要确认关联，请单击**接受**。 随后你的帐户将能够发布供该企业专门使用的应用。


## <a name="submit-lob-apps"></a>提交 LOB 应用

在准备好发布供企业专门使用的应用后，该过程类似于应用提交过程。 应用完成相同的[认证过程](the-app-certification-process.md)，并且必须遵循所有 [Microsoft Store 策略](https://docs.microsoft.com/legal/windows/agreements/store-policies)。 仅有数个部分过程不同。


### <a name="visibility"></a>可见性

在设置企业关联后，每次提交应用时，都会在提交的**定价和可用性**页面的**可见性**部分中看到一个下拉框。 默认情况下，此下拉框设置为“零售分配”****。 若要使应用供某个企业专门使用，你需要选择“业务线 (LOB) 分配”****。

选择**业务线 (LOB) 分配**后，常规**可用性**选项将替换为发布专用应用的企业列表。 所选企业之外的任何其他人将无法查看或下载应用。

必须至少选择一家企业才能将应用作为业务线应用发布。

<span id="organizational" />

### <a name="organizational-licensing"></a>组织授权

默认情况下，在提交应用时，**应用商店托管（联机）批量许可**框已选中。 在发布 LOB 应用时，此框必须保持选中状态，以便企业可以批量购置你的应用。 此操作将可保证在**分配和可见性**部分中选择的企业之外的任何人均无法使用应用。

如果你想要通过断开连接（离线）许可使企业能够使用应用，也可以选中“断开连接(离线)许可”**** 框。

有关详细信息，请参阅[组织授权选项](organizational-licensing.md)。


### <a name="age-ratings"></a>年龄分级

对于 LOB 应用，提交过程的[年龄分级](age-ratings.md)步骤的工作方式与零售应用相同，但还具有其他选项供你手动指示你的应用的应用商店年龄分级，而不是完成问卷或导入现有的 IARC 分级 ID。 此手动分级仅适用于 LOB 分配，因此如果将应用的**可见性**设置更改为**零售分配**，则在发布该提交之前，将需要完成年龄分级问卷。


## <a name="enterprise-deployment-of-lob-apps"></a>LOB 应用的企业部署

单击“提交到应用商店”**** 后，应用将完成认证过程。 准备就绪后，企业管理员必须在适用于企业的 Microsoft 应用商店或适用于教育的 Microsoft 应用商店门户中将应用添加到其专用应用商店。 企业稍后可以将应用部署到其用户。

> [!NOTE]
> 为了获取 LOB 应用，组织必须位于[受支持的市场](https://technet.microsoft.com/itpro/windows/whats-new/windows-store-for-business-overview#supported-markets)中，并且不得在提交应用时[排除该市场](define-pricing-and-market-selection.md)。 

有关详细信息，请参阅[使用业务线应用](http://go.microsoft.com/fwlink/p/?LinkId=698846)和[使用专用应用商店分配应用](http://go.microsoft.com/fwlink/p/?LinkId=698847)。


## <a name="update-lob-apps"></a>更新 LOB 应用

若要发布对已经发布为 LOB 的应用的更新，只需创建新的提交即可。 可以上载新程序包或进行任何其他更改，然后单击**提交到应用商店**使更新的版本可用。 请确保使**可见性**中的企业选择保持不变，除非希望更改这些选择，例如选择可购置应用的其他企业，或者删除之前分配了应用的企业。

如果想要停止提供之前作为业务线应用发布的应用，并阻止任何新的购置，你需要创建新的提交。 首先，需要将**可见性**选择从**业务线 (LOB) 分配**更改为**零售分配**。 然后，在[可发现性](choose-visibility-options.md#discoverability)部分，选择**使此产品在 Microsoft Store 中可用，但不可被发现**和**停止购置**选项。

提交完成认证过程后，应用将不再用于新的购置（虽然任何已拥有该应用的用户可以继续使用它）。

> [!NOTE]
> 将应用更改为**零售分配**时，如果尚未完成[年龄分级问卷](age-ratings.md)，将需要完成该问卷，即使该应用不适用于新的购置也是如此。


## <a name="distribute-lob-apps-through-sideloading"></a>通过旁加载分配 LOB 应用

通过适用于企业的 Microsoft 应用商店或适用于教育的 Microsoft 应用商店将应用分配到企业，这可以确保应用由应用商店进行签名，并遵守标准的应用商店策略。

在某些情况下，公司可能不希望其 LOB 应用通过 Windows 开发人员中心提交（例如出于合规性原因或应用需要其他功能）。 在这种情况下，企业可以通过旁加载直接将应用部署到计算机，无需使用适用于企业的 Microsoft 应用商店或适用于教育的 Microsoft 应用商店。

有关详细信息，请参阅[在 Windows 10 中旁加载 LOB 应用](http://go.microsoft.com/fwlink/p/?LinkId=623433)。

 

 




