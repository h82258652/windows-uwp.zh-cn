---
author: jnHs
Description: When you create a new add-on in the Windows Dev Center dashboard, you need to specify a product type and assign it a product ID.
title: 设置加载项产品类型和产品 ID
ms.assetid: 59497B0F-82F0-4CEE-B628-040EF9ED8D3D
ms.author: wdg-dev-content
ms.date: 01/12/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 加载项, iap, 持久型, 消耗品, 订阅, 产品类型, 产品 ID, 应用内购买, 应用内产品
ms.localizationpriority: medium
ms.openlocfilehash: 0673048fc9a1ed8fb7c439607ebc4197039699e9
ms.sourcegitcommit: 4f6dc806229a8226894c55ceb6d6eab391ec8ab6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/20/2018
ms.locfileid: "4082918"
---
# <a name="set-your-add-on-product-type-and-product-id"></a>设置加载项产品类型和产品 ID

加载项必须与已在仪表板中创建的应用（即使并未提交）关联。 可在应用的**概述**页面或**加载项**页面找到**创建新加载项**的按钮。

选中**创建新加载项**后，系统将提示你指定产品类型并为加载项分配产品 ID。

## <a name="product-type"></a>产品类型

首先，你将需要指示所提供的加载项类型。 该选择是指客户使用加载项的方式。

> [!NOTE]
> 在保存此创建加载项的页面后，将无法更改该产品类型。 如果选择了错误的产品类型，可以随时删除正在提交的加载项，然后从头开始创建新的加载项。

<span id="durable" />

### <a name="durable"></a>耐用品

如果加载项通常只需要购买一次，则选中**耐用品**作为产品类型。 此类加载项通常用于解锁应用中的额外功能。

耐用型加载项的默认**产品生命周期**为**永久**，这意味着加载项永不过期。 可以在加载项提交过程的[属性](enter-add-on-properties.md)步骤中，选择将**产品生命周期**更改为不同的持续时间。 若执行此操作，则加载项的过期时间将晚于你指定的持续时间（可选择 1 至 365 天），这种情况下，客户可在加载项过期后再次购买。

<span id="consumable" />

### <a name="consumable"></a>易耗型产品

如果加载项可以购买、使用（消耗），并且可以重新购买，你将希望选择一种**易耗型**产品类型。 易耗型加载项通常用于客户可按固定数量购买然后会使用完毕的产品，例如游戏货币（金币、硬币等）。 有关详细信息，请参阅[支持购买易耗型加载项](../monetize/enable-consumable-add-on-purchases.md)。

易耗型加载项的类型有两种：
- **开发人员管理的易耗品**：必须在应用中管理平衡和实施情况。 所有的操作系统版本都支持。
- **应用商店管理的易耗品：** Microsoft 会跟踪运行 Windows 10 版本 1607 或更高版本的所有客户设备的平衡；所有较早的操作系统版本不支持此功能。 若要使用此选项，父产品必须使用 Windows 10 SDK 版本 14393 或更高版本编译。 另请注意，在发布父产品前，无法将应用商店管理的易耗型加载项添加到应用商店（但可以在仪表板中创建提交，并可随时处理）。 需要在提交的**属性**步骤中输入应用商店管理的易耗型加载项数量。

<span id="subscription" />

### <a name="subscription"></a>订阅

若希望定期向客户收取加载项费用，则选择**订阅**。

客户最初获取订阅加载项后，将定期支付费用以继续使用此加载项。 客户可以随时取消订阅，以免日后继续产生费用。 你需要指定订阅期，以及是否在提交中的**属性**步骤提供免费试用版。

订阅加载项仅适用于运行 Windows 10、版本 1607 或更高版本的客户。 必须使用 Windows 10 SDK 版本 14393 或更高版本编译父应用，并且必须使用 **Windows.Services.Store** 命名空间（而不是 **Windows.ApplicationModel.Store** 命名空间）中的应用内购买 API。 有关详细信息，请参阅[为应用启用订阅加载项](../monetize/enable-subscription-add-ons-for-your-app.md)。

在将订阅加载项发布到 Microsoft Store 之前，必须提交父产品（但你可以在仪表板中创建提交，并且可随时处理）。

## <a name="product-id"></a>产品 ID

无论选择何种产品类型，将需要为加载项输入唯一的产品 ID。 该名称可用于识别仪表板中的加载项，并且该标识符可用于[指向代码中的加载项](../monetize/in-app-purchases-and-trials.md#how-to-use-product-ids-for-add-ons-in-your-code)。

以下是在选择产品 ID 时应记住的一些事项：

-   客户不会看到此产品 ID。 （稍后，你可以输入一个用于向客户显示的[标题和描述](create-add-on-descriptions.md)。）
-   你无法在加载项发布后更改或删除其产品 ID。
-   产品 ID 的长度不可超过 100 个字符。
-   产品 ID 不可包含以下任何字符：**&lt; &gt; \* % & : \\ ? + ,**
-   若要在所有操作系统版本中提供加载项，必须只使用字母数字字符、句点和/或下划线。 如果你使用任何其他类型的字符，该加载项将不可供运行 Windows Phone 8.1 或更早版本的客户购买。
-   产品 ID 在 Microsoft Store 中无需是唯一的，但对于你的开发人员帐户必须是唯一的。
 
