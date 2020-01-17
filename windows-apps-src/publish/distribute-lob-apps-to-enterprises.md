---
Description: 可以通过适用于企业的 Microsoft 应用商店或适用于教育的 Microsoft 应用商店向企业直接发布业务线 (LOB) 应用以获取批量购置，无需在应用商店中使应用广泛可用。
title: 将 LOB 应用分配到企业
ms.assetid: 2050126E-CE49-4DE3-AC2B-A572AC895158
ms.date: 01/16/2020
ms.topic: article
keywords: windows 10, uwp, lob, 业务线, 企业应用, 适用于企业的 store, 适用于教育的 store, 企业
ms.localizationpriority: medium
ms.openlocfilehash: faf750ece274776a147dff9e825f32534eb13014
ms.sourcegitcommit: 7a8aea567b26283c71420e0d305d78f675e1fba7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/16/2020
ms.locfileid: "76125669"
---
# <a name="distribute-lob-apps-to-enterprises"></a>将 LOB 应用分配到企业

可以使用多个选项将业务线（LOB）应用分发到组织的用户使用[.msix 包](https://docs.microsoft.com/windows/msix/)，而无需将应用广泛地提供给公众使用。 你可以使用设备管理工具、配置基于应用程序安装程序的部署、直接旁加载应用，或者将应用发布到 Microsoft Store for Business 或 Microsoft Store 教育。

## <a name="microsoft-endpoint-configuration-manager-and-microsoft-intune"></a>Microsoft 端点 Configuration Manager 和 Microsoft Intune

如果你的组织使用 Microsoft 端点 Configuration Manager 或 Microsoft Intune 来管理设备，则可以使用这些工具部署 LOB 应用。 有关详细信息，请参阅以下文章：

* [Configuration Manager 中的应用程序管理简介](https://docs.microsoft.com/configmgr/apps/understand/introduction-to-application-management)
* [Microsoft Intune 中的应用生命周期概述](https://docs.microsoft.com/intune/apps/app-lifecycle)

## <a name="app-installer"></a>应用安装程序

应用安装程序通过双击 .MSIX 应用包直接或通过双击从 web 服务器安装应用程序包的 appinstaller 文件来安装 Windows 10 应用。 这意味着用户无需使用 PowerShell 或其他开发人员工具来安装 LOB 应用。 应用安装程序还可以安装包含可选包和相关集的应用包。

可以从适用于企业的 Microsoft Store [Web 门户](https://businessstore.microsoft.com/store/details/app-installer/9NBLGGH4NNS1)下载应用安装程序，以便在企业中离线使用。 有关应用程序安装程序的详细信息，请参阅[通过应用安装程序安装 Windows 10 应用](https://docs.microsoft.com/windows/msix/app-installer/app-installer-root)。

## <a name="sideloading"></a>旁加载

将 LOB 应用直接分发给组织中的用户的另一种方法是旁加载。 此选项类似于基于应用安装的部署，因为它使用户能够直接安装 .MSIX 应用程序包。 从 Windows 10 版本2004开始，默认情况下会启用旁加载，用户可以通过双击 "签名 .MSIX 应用包" 来安装应用。 在 Windows 10 版本1909及更早版本中，旁加载需要一些额外的配置，并使用 PowerShell 脚本。 有关详细信息，请参阅[在 Windows 10 中旁加载 LOB 应用](https://docs.microsoft.com/windows/application-management/sideload-apps-in-windows-10)。

## <a name="microsoft-store-for-business-or-microsoft-store-for-education"></a>适用于企业的 Microsoft Store 或 Microsoft Store 教育

可以通过适用于企业的 Microsoft 应用商店或适用于教育的 Microsoft 应用商店向企业直接发布业务线 (LOB) 应用以获取批量购置，无需在应用商店中使应用广泛可用。 使用此选项时，应用由存储区签名，并且必须符合标准存储策略。

> [!NOTE]
> 目前，仅免费应用可以通过适用于企业的 Microsoft 应用商店或适用于教育的 Microsoft 应用商店专门分配到企业。 如果你提交了作为 LOB 的某个付费应用，它不会向企业提供。 

> [!IMPORTANT]
> 你无法使用 [Microsoft Store 提交 API](../monetize/create-and-manage-submissions-using-windows-store-services.md) 直接向企业发布 LOB 应用。 LOB 应用的所有提交都必须通过合作伙伴中心发布。

### <a name="set-up-the-enterprise-association"></a>设置企业关联

将 LOB 应用专门发布到企业的第一步是建立你的帐户和企业的专用应用商店之间的关联。

> [!IMPORTANT]
> 此关联过程必须由企业启动，并且必须使用与用于创建开发人员帐户的 Microsoft 帐户关联的电子邮件地址。 有关详细信息，请参阅[使用业务线应用](https://docs.microsoft.com/microsoft-store/working-with-line-of-business-apps)。

在企业选择邀请你发布供其专门使用的应用时，你将收到包含确认该关联的链接的电子邮件。 你还可以通过转到**帐户设置**的**企业关联**部分来确认这些关联（只要你已使用用于打开开发人员帐户的 Microsoft 帐户登录）。

若要确认关联，请单击**接受**。 随后你的帐户将能够发布供该企业专门使用的应用。

### <a name="submit-lob-apps"></a>提交 LOB 应用

在准备好发布供企业专门使用的应用后，该过程类似于应用提交过程。 应用完成相同的[认证过程](the-app-certification-process.md)，并且必须遵循所有 [Microsoft Store 策略](store-policies.md)。 仅有数个部分过程不同。

#### <a name="visibility"></a>可见性

在设置企业关联后，每次提交应用时，都会在提交的**定价和可用性**页面的**可见性**部分中看到一个下拉框。 默认情况下，此下拉框设置为“零售分配”。 若要使应用供某个企业专门使用，你需要选择“业务线 (LOB) 分配”。

选择**业务线 (LOB) 分配**后，常规**可用性**选项将替换为发布专用应用的企业列表。 所选企业之外的任何其他人将无法查看或下载应用。

必须至少选择一家企业才能将应用作为业务线应用发布。

<span id="organizational" />

#### <a name="organizational-licensing"></a>组织授权

默认情况下，在提交应用时，**应用商店托管（联机）批量许可**框已选中。 在发布 LOB 应用时，此框必须保持选中状态，以便企业可以批量购置你的应用。 此操作将可保证在**分配和可见性**部分中选择的企业之外的任何人均无法使用应用。

如果你想要通过断开连接（离线）许可使企业能够使用应用，也可以选中“断开连接(离线)许可”框。

有关详细信息，请参阅[组织授权选项](organizational-licensing.md)。

#### <a name="age-ratings"></a>年龄分级

对于 LOB 应用，提交过程的[年龄分级](age-ratings.md)步骤的工作方式与零售应用相同，但还具有其他选项供你手动指示你的应用的应用商店年龄分级，而不是完成问卷或导入现有的 IARC 分级 ID。 此手动分级仅适用于 LOB 分配，因此如果将应用的**可见性**设置更改为**零售分配**，则在发布该提交之前，将需要完成年龄分级问卷。

### <a name="enterprise-deployment-of-lob-apps"></a>LOB 应用的企业部署

单击“提交到应用商店”后，应用将完成认证过程。 准备就绪后，企业管理员必须在适用于企业的 Microsoft 应用商店或适用于教育的 Microsoft 应用商店门户中将应用添加到其专用应用商店。 企业稍后可以将应用部署到其用户。

> [!NOTE]
> 为了获取 LOB 应用，组织必须位于[受支持的市场](https://docs.microsoft.com/windows/whats-new/windows-store-for-business-overview#supported-markets)中，并且不得在提交应用时[排除该市场](define-pricing-and-market-selection.md)。 

有关详细信息，请参阅[使用业务线应用](https://docs.microsoft.com/microsoft-store/working-with-line-of-business-apps)和[使用专用应用商店分配应用](https://docs.microsoft.com/microsoft-store/distribute-apps-from-your-private-store)。

### <a name="update-lob-apps"></a>更新 LOB 应用

若要发布对已经发布为 LOB 的应用的更新，只需创建新的提交即可。 可以上载新程序包或进行任何其他更改，然后单击**提交到应用商店**使更新的版本可用。 请确保使**可见性**中的企业选择保持不变，除非希望更改这些选择，例如选择可购置应用的其他企业，或者删除之前分配了应用的企业。

如果想要停止提供之前作为业务线应用发布的应用，并阻止任何新的购置，你需要创建新的提交。 首先，需要将**可见性**选择从**业务线 (LOB) 分配**更改为**零售分配**。 然后，在[可发现性](choose-visibility-options.md#discoverability)部分，选择**使此产品在 Microsoft Store 中可用，但不可被发现**和**停止购置**选项。

提交完成认证过程后，应用将不再用于新的购置（虽然任何已拥有该应用的用户可以继续使用它）。

> [!NOTE]
> 将应用更改为**零售分配**时，如果尚未完成[年龄分级问卷](age-ratings.md)，将需要完成该问卷，即使该应用不适用于新的购置也是如此。