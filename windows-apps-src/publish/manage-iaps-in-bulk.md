---
author: jnHs
Description: "批量管理 IAP 允许你同时更改多个 IAP，而非逐个提交每个更新。"
title: "批量管理 IAP"
ms.sourcegitcommit: 475371dd55aa111f3743c03dc1600e8cfdbeb5b0
ms.openlocfilehash: ae4d4ed33b9bd10a2b01b336c942ad3212de6533


---

# 批量管理 IAP

> **重要信息** 此功能当前仅向已加入[开发人员中心预览体验计划](dev-center-insider-program.md)的开发者帐户提供。 在向所有开发人员提供此功能前，此功能的实现可能会有所变化。 此初步文档提供一些有关此功能工作原理的基本信息。

批量管理 IAP 允许你同时更改多个 IAP，而非逐个提交每个更新。 你也可以通过单击“批量管理 IAP”****，在应用的概述页访问此功能。

## 导出当前 IAP 信息

若要开始，首先需要下载 .csv 模板文件。 如果你已创建了 IAP，此文件将包括与其相关的信息。 如果没有创建，它将是空白文件，可用来输入新 IAP 的信息。 

若要生成并下载此模板文件，请单击“导出 IAP”****，并将 .csv 文件保存到计算机。

.csv 文件包含以下列。 

| 列名称               | 描述                            | 必需？      |
|---------------------------|----------------------------------|----------------------|
| 产品 ID    |  IAP 的唯一[产品 ID](set-your-iap-product-id.md#product-id)。  | 是。 在 IAP 发布后无法更改。 |
| 操作 |在导入模板时需要应用的操作。 受支持的值为 **Submit**（提交新 IAP，或更新之前发布的 IAP）和 **CreateDraft**（保存更改，无需将其提交到应用商店）。 |    是 |
| 产品类型  | IAP 的[产品类型](set-your-iap-product-id.md#product-type)。 受支持的值为 **Consumable** 或 **Durable**。 | 是。 在 IAP 发布后无法更改。 |
| 产品生命周期  | 对于耐用型 IAP，此值为 **Forever**（用于不会过期的产品）或一段固定时间。 可接受的时长值为：**1day、3days、5days、7days、14days、30days、60days、90days、180days、365days**   | 是（如果产品类型为耐用型） |
| 内容类型  | IAP 的[内容类型](enter-iap-properties.md#content-type)。 对于大多数 IAP，这应该是 **ElectronicSoftwareDownload**。 其他可接受的值为：**ElectronicBooks、ElectronicMagazineSingleIssue、ElectronicNewspaperSingleIssue、MusicDownload、MusicStreaming、OnlineDataStorageServices、VideoDownload、VideoStreaming、SoftwareAsAService** | 是 |
| 标记   | 在应用实现中使用的可选[标记](enter-iap-properties.md#tag)信息。 | 否 |
| 基价    | 想要提供 IAP 的[价格区间](set-iap-pricing-and-availability.md#base-price)。 必须是 **Free** 或格式为 **0.99USD** 的有效价格区间。 |   是 |
| 发布日期  | 希望发布 IAP 的日期。 可接受的值为 **Immediate**、**Manual** 或符合 [ISO 8601 标准](http://go.microsoft.com/fwlink/p/?LinkId=817237)的日期字符串。 | 是 |
| 标题    | 客户将看到的 IAP 名称，前面是语言代码和分号。 例如，若要使用美式英语的标题“Example Title”，请*输入“en-us;Example Title”*。 其他语言的其他标题可以使用分号隔开。 每个标题必须是 100 个字符或更少。     | 是 |
|描述   | 向客户显示的可选其他信息，前面是语言区域设置代码和分号。 例如，若要使用美式英语的描述“This is an example”，请输入*“en-us;This is an example”*。 其他语言的其他标题可以使用分号隔开。 每段描述必须是 200 个字符或更少。    | 否 |
| 市场 | 希望提供 IAP 的一个或多个[市场](define-pricing-and-market-selection.md#windows-store-consumer-markets)。 使用分号分隔每个市场。 | 是 |
|关键字 | 在应用实现中使用的可选[关键字](enter-iap-properties.md#keywords)。 | 否 |

## 导入 IAP

在可以导入更改前，需要使用所需更改来更新下载的 .csv 文件。

若要更改已发布的 IAP，请在电子表格副本中更新希望更改的值。 可以删除不想更新的任意行数的 IAP，或者将其原样保留。 请注意，如果该 IAP 中已经在处理某个提交，将无法使用 .csv 文件进行更改。

> **重要提示** 当向已经发布的 IAP 提交更新时，无法更改**产品 ID** 和**产品类型**字段。

若要提交新 IAP，请添加一个新行并输入新 IAP 的信息。 请确保输入所有所需信息。 

完成所有更改后，保存 .csv 文件（文件名相同），然后通过将其拖动到指定字段（或单击“浏览文件”****）上传该文件。 将显示更改总结，还会显示在提交前必须修复的任何错误。 在验证信息正确后，单击“提交到应用商店”****。 每个 IAP 都将使用你提供的信息完成提交过程。




<!--HONumber=Jun16_HO4-->


