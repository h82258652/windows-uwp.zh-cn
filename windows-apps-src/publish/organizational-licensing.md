---
Description: 可以在应用提交的“组织许可”部分中指示是否提供应用以及如何提供应用，以便通过适用于企业的 Microsoft Store 和适用于教育的 Microsoft Store 批量购买应用。
title: 组织许可选项
ms.assetid: 1EB139B0-67E7-4F66-AAEF-491B1E52E96F
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, 适用于企业的 store, 适用于教育的 store, 组织, 批量许可, 企业, 教育 store, 企业 store, 批量购买, 批量
localizationpriority: high
ms.openlocfilehash: 880e472b9a6ed19bae85c00b4014b431e3185f61
ms.sourcegitcommit: a7effa01ca1c810e792b60f89ba38ce3bf0b310e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "81545017"
---
# <a name="organizational-licensing-options"></a>组织许可选项


可以在应用提交的[定价和可用性](set-app-pricing-and-availability.md#organizational-licensing)页的“组织许可”部分中指示是否提供应用以及如何提供应用，以便通过适用于企业的 Microsoft Store 和适用于教育的 Microsoft Store 批量购买应用  。

借助这些设置，可以选择允许将应用提供给要为其用户获取和部署多个许可证的组织（企业和教育），从而为跨 Windows 10 设备类型（包括电脑、平板电脑和手机）扩大组织的覆盖面提供了机遇。

还需要允许直接发布给企业的任何[业务线 (LOB) 应用](distribute-lob-apps-to-enterprises.md)的组织许可。

> [!NOTE]
> 每个应用的选项配置是相互独立的。 可通过创建新的提交来随时更改应用的首选项，而你的更改将在提交完成[认证过程](the-app-certification-process.md)后生效。

> [!IMPORTANT]
> 使用 [Microsoft Store 提交 API](../monetize/create-and-manage-submissions-using-windows-store-services.md) 的提交不会提供给适用于企业的 Microsoft Store 和适用于教育的 Microsoft Store。 若要让组织可以批量购买你的应用，则必须在合作伙伴中心创建并提交你的提交。


## <a name="allowing-your-app-to-be-offered-to-organizations"></a>允许将你的应用提供给组织

默认情况下，标记为“使我的应用可用于带有应用商店托管(联机)许可和分发的组织”  的框处于选中状态。 这意味着，你希望你的应用能够包含在我们将提供给组织以供批量购置的应用目录中，并随附通过应用商店联机许可系统托管的应用许可。

> [!NOTE]
> 这并不保证你的应用可用于所有组织。

如果你首选不允许我们为组织提供应用以供批量购置，请取消选中此框。 请注意，此更改仅在应用完成认证过程后才进行。 如果任意组织以前购置过你的应用的许可证，这些许可证仍将有效，并且已拥有该应用的用户可继续使用它。

> [!TIP]
> 若要专门将业务线 (LOB) 应用发布到特定组织，你可以设置企业关联，并允许组织直接向其专用应用商店添加应用。 有关详细信息，请参阅[向企业分配 LOB 应用](distribute-lob-apps-to-enterprises.md)。


## <a name="allowing-disconnected-offline-licensing"></a>允许已断开连接的（脱机）许可

许多组织需要能够使用脱机许可启用应用。 例如，某些组织需要将应用部署到很少或从不连接到 Internet 的设备。 若要允许将你的应用提供给这些客户，请选中标记为“允许适用于组织的组织托管(脱机)许可和分发”  的框。

请注意，默认情况下“未勾选”此复选框  。 必须选中该框，以允许我们将应用提供给已经过认证的组织，这些组织将使用组织托管（脱机）许可安装应用。 组织必须经过额外验证才能通过这种方式将付费应用安装到其最终用户。

脱机许可允许组织批量购置你的应用，然后安装该应用，而无需让每台设备都访问应用商店的许可系统。 组织能够下载你的应用包以及许可证，这使他们可以（通过其所拥有的管理工具或在操作系统映像上预加载应用）将该程序包安装到设备，而无需在使用了特定许可时通知应用商店。 支持此应用场景不仅能大大地增加部署的灵活性，还能提升你的应用对这些客户的吸引力。

> [!IMPORTANT]
> .xap 程序包不支持脱机许可。

 
## <a name="paid-app-support"></a>付费应用支持

目前，位于某些市场中的开发者帐户可以提供通过适用于企业的 Microsoft 应用商店批量购置的付费应用。 

> [!NOTE]
> 在某些市场中，同一价格段内的应用在适用于企业的 Microsoft Store 或适用于教育的 Microsoft Store 中显示的价格可能不同于 Microsoft Store 中向零售客户显示的价格。 组织购买应用的收益付款计算方式与消费者购买应用的收益付款计算方式相同。 有关详细信息，请参阅[获得收入](getting-paid-apps.md)和[应用开发人员协议](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement)。 有关支持适用于企业的 Microsoft Store 和适用于教育的 Microsoft Store 的市场列表，请参阅[适用于企业的 Microsoft Store 和适用于教育的 Microsoft Store 概述](https://docs.microsoft.com/windows/manage/windows-store-for-business-overview#supported-markets)。

如果下面未列出你所在的国家或地区，你的付费应用当前将不能在适用于企业的 Microsoft Store 和适用于教育的 Microsoft Store 中提供。 如果是这样，你为付费应用选择的组织许可可能在以后应用，因为我们可能会在将来添加对来自其他开发者帐户市场的提交的支持。

目前，位于以下国家和地区的开发人员可以通过适用于企业的 Microsoft Store 和适用于教育的 Microsoft Store 向组织客户分配付费应用：

- 奥地利
- 比利时
- 保加利亚
- 加拿大
- 克罗地亚
- 塞浦路斯
- 捷克
- 丹麦
- 爱沙尼亚
- 芬兰
- 法国
- 德国
- 希腊
- 匈牙利
- 爱尔兰
- 曼岛
- 意大利
- 拉脱维亚
- 列支敦士登
- 立陶宛
- 卢森堡
- 马耳他
- 摩纳哥
- 荷兰
- 挪威
- 波兰
- 葡萄牙
- 罗马尼亚
- 斯洛伐克
- 斯洛文尼亚
- 西班牙
- 瑞典
- 瑞士
- 英国
- 美国
