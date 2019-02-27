---
Description: You can generate promotional codes for an app or add-on that you have published in the Microsoft Store.
title: 生成促销代码
ms.assetid: 9B632266-64EC-4D62-A4C4-55B6643D8750
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, 促销代码, 促销代码, 预付码, 预付码
ms.localizationpriority: medium
ms.openlocfilehash: db4cde6f8c195101ec31de26c00ffa7325e08d71
ms.sourcegitcommit: 175d0fc32db60017705ab58136552aee31407412
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/27/2019
ms.locfileid: "9114483"
---
# <a name="generate-promotional-codes"></a>生成促销充值码


[合作伙伴中心](https://partner.microsoft.com/dashboard)允许你的应用或已在 Microsoft Store 中发布的加载项生成促销代码。 促销充值码是让有影响力的用户能够免费访问你的应用或加载项的一种简便方式。 你还可能会让用户免费访问你的应用或加载项，或使用 windows 10 [beta 测试](beta-testing-and-targeted-distribution.md)使用促销代码来处理客户服务方案。 

每个促销代码都具有对应的唯一可兑换 URL，客户可单击才能兑换充值码并从 Microsoft Store 安装应用或加载项。  请注意，你的应用必须先通过[应用认证过程](the-app-certification-process.md)的最终发布阶段，客户才能兑换促销代码以安装应用。

你可以生成一次性代码 （并分配一个与每个客户），或者你可以选择生成代码可用于多个时间由指定的客户数。

> [!TIP]
> 可使用[定向推送通知](send-push-notifications-to-your-apps-customers.md)向细分市场客户分发促销代码。 执行此操作时，请务必使用允许多个客户使用同一代码的促销代码。


## <a name="promotional-code-policies"></a>促销充值码策略

请注意以下促销代码策略：

-   你可以为已发布到 Microsoft Store 的任何应用或加载项（订阅加载项除外）生成促销代码。 客户可以在你的应用或加载项支持的任何版本的 Windows 上兑换这些代码。
-   在预订日期 6 个月后，促销代码过期（除非你选择较早的过期日期）。
-   对于每个应用或加载项，可生成每 6 个月允许最多 1600 次兑换的代码。 6 个月的时段从提交第一个促销代码订单开始计算，即使选择了更早的到期日期，也是如此。 每个产品共 1600 次兑换的规定适用于一次性代码和可多次使用的代码。
-   必须按照[应用开发人员协议](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement)中定义的要求进行操作，包括 **3k. 促销代码**部分。

> [!NOTE]
> 即使你的应用是对客户不可用，可以使用促销代码 (即，如果选择了**可用，但不可被发现在应用商店中使此产品**与**停止购置： 任何具有直接链接的客户可以看到产品的应用商店一览，但它们才能够下载该如果他们拥有之前，产品或拥有一个促销代码并在使用 Windows 10 设备**你的提交[可发现性](choose-visibility-options.md#discoverability)部分中的选项)。 使用此选项，客户必须在 Windows 10 （包括 Xbox） 上才能获取你的产品的促销代码。


## <a name="order-promotional-codes"></a>预订促销代码

应用或加载项预订促销代码：

1.  在[合作伙伴中心](https://partner.microsoft.com/dashboard)的左侧的导航菜单中，展开**吸引**，然后选择**促销代码**。

2.   在**促销代码**页面上，单击**预订代码**。

3.  在“新促销代码订单”**** 页面上，输入以下内容：
    -   选择要为其生成充值码的应用或加载项。 （请注意，你不能为订阅加载项生成促销代码。）
    -   为订单指定名称。 在查看促销代码使用情况数据时，你可以使用此名称区分不同的代码订单。
    -   选择订单类型。 你可以选择生成一组每个仅使用一次的促销代码，或者选择生成一个可以多次使用的促销代码。
    -   指定要订购的代码数量（如果要生成一组代码）或代码可兑换的次数（如果要生成一个将使用多次的代码）。
    -   指定促销代码应处于活动状态的时间。 若要选择特定开始日期和时间，请清除**立即激活代码**复选框。 否则，代码会立即活动 （尽管你的产品必须已完成发布过程中为客户使用的代码的顺序）。
    -   指定促销充值码应到期的时间。 若要选择早于 6 个月的特定到期日期和时间，请清除**代码 6 个月后到期**复选框。

4.  单击“预订充值码”****。 随即将返回**促销代码**页面，可在此页面中的应用促销代码订购摘要表中查看新订单。


## <a name="download-and-distribute-promotional-codes"></a>下载和分发促销充值码

若要下载已完成的促销代码订单并将代码分发给客户：

1.  在[合作伙伴中心](https://partner.microsoft.com/dashboard)的左侧的导航菜单中，展开**吸引**，然后选择**促销代码。**
2.  单击促销代码订单的**下载**链接，然后将生成的文件保存到计算机。 此文件包含采用制表符分隔值 (.tsv) 格式的促销代码订单相关信息。
3.  在选择的编辑器中打开 .tsv 文件。 为了实现最佳体验，请在可采用表格结构显示数据的应用程序（例如 Microsoft Excel）中打开 .tsv 文件。 但是，也可以在任何文本编辑器中打开此文件。

    该文件包含每个代码的以下数据列：

    -   **产品名称**：代码相关联的应用或加载项的名称。
    -   **订单名称**：生成此代码的订单的名称。
    -   **促销代码**：代码本身。 这是由连字符分隔、字母数字字符组成的 5x5 字符串。 例如：DM3GY-M2GYM-6YMW6-4QHHT-23W2Z
    -   **可兑换 URL**：客户可用于兑换代码并安装应用或加载项的 URL。 URL 具有以下格式： https://go.microsoft.com/fwlink/?LinkId=532540&mstoken=&lt; promotional_code>
    -   **开始日期**：此代码的激活日期。
    -   **到期日期**：此代码的到期日期。
    -   **代码 ID**：此代码的唯一 ID。
    -   **订单 ID**：生成此代码的订单的唯一 ID。
    -   **分发对象**：可使用此空字段记录向其分发此代码的客户。
    -   **可用次数**：（生成此文件时）此代码仍可兑换的次数。
    -   **已兑换次数**：（生成此文件时）代码已兑换次数。

4.  通过你偏好的任何通信格式（例如，针对性通知、电子邮件、短信或打印的卡片）将可兑换 URL 分发给客户。 我们建议你的通信包括以下内容：
    -   关于促销代码用于哪个应用或加载项的说明，以及关于客户为何收到该代码的说明（可选）。
    -   代码的可兑换 URL。
    -   指导客户访问可兑换 URL、使用其 Microsoft 帐户登录并按照说明下载和安装应用的说明。


## <a name="code-redemption-user-experience"></a>代码兑换用户体验

向客户分发促销代码 （或其可兑换 URL） 后，他们可以单击 URL 即可免费获取产品。 单击可兑换 URL 将打开经过身份验证的**兑换代码**页面（位于 <https://account.microsoft.com/billing/redeem>）。 此页面包含用户即将兑换的应用的描述。 如果客户没有使用其 Microsoft 帐户登录，系统将提示其登录。 你的客户也可以访问 <https://account.microsoft.com/billing/redeem>，直接输入代码。

> [!IMPORTANT]
> 建议不要在产品尚未完成发布过程时向客户分发促销代码（即使你已选择**在应用商店中提供此产品，同时使其不可被发现**，也是如此）。 当客户尝试将促销代码用于尚未发布的产品时，将看到错误消息。

客户单击**兑换**后，Microsoft Store 将打开到应用的概览页面（如果客户使用 Windows 10 或 Windows 8.1 设备），在此页面中，客户可单击**安装**以免费下载和安装应用。 如果客户使用未安装 Microsoft Store 的计算机或设备，则此链接将打开此应用的 Microsoft Store 网页。 此代码将应用于客户的 Microsoft 帐户，以便其稍后在 Windows 设备（与同一个 Microsoft 帐户相关联）上免费下载此应用。

> [!NOTE]
> 在某些情况下，客户可能会看到而不是**安装**，**购买**按钮，即使应用已通过促销充值码成功兑换。 客户可单击**购买**来免费安装此应用。


## <a name="review-your-promotional-codes"></a>查看促销代码

若要查看你的应用和加载项的促销充值码订单的详细的摘要，请导航到**促销充值码**页 （在合作伙伴中心的左侧的导航菜单中，展开**吸引**，然后选择**促销代码**）。 可以查看所有当前和非活动促销代码的以下详细信息：
-   订单名称
-   应用或加载项
-   开始日期
-   过期日期
-   可用
-   已兑换

你还可以从此表中[下载](#download-and-distribute-promotional-codes)订单。

 
