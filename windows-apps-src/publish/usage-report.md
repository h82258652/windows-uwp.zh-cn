---
author: jnHs
Description: "Windows 开发人员中心仪表板中的“使用情况报告”可使你查看客户如何使用你的应用。"
title: "使用情况报告"
ms.assetid: 5F0E7F94-D121-4AD3-A6E5-9C0DEC437BD3
translationtype: Human Translation
ms.sourcegitcommit: 056642044953bab02f78912c7611ddcf5d6d48e6
ms.openlocfilehash: 476e7ee0c9c7ea7dce7f5e3a0389091ede9132c4

---

# 使用情况报告


Windows 开发人员中心仪表板中的“使用情况”****报告可使你查看客户如何使用你的应用，并获取有关已定义的自定义事件的消息。 你可以在仪表板中查看此数据，或[下载该报告](download-analytic-reports.md)以供脱机查看。

> **注意** 之前，如果你已在应用中激活了 Visual Studio Application Insights SDK，则**使用情况**报告仅提供数据。 更新了**使用情况**报告后，这就不再需要了。

## 应用筛选器


在页面顶部附近，可以展开“应用筛选器”****按日期范围和/或按产品组（相关操作系统版本）筛选此页面上的所有数据。

-   **日期**：默认筛选器为“最近 30 天”****，但是你可以将此条件扩大到“最近 12 个月”****。
-   **程序包版本**：默认设置为“全部”****。 如果应用包含多个程序包，可以在此处选择一个特定程序包。
-   **设备类型**：默认设置为“全部”****，但可以选择仅为一种特定设备类型显示数据。

以下列出的所有图中的信息都将反映“应用筛选器”****中所选的时段。 除非已使用“应用筛选器”****部分进行筛选以仅显示一个程序包版本，否则在默认情况下，这将包含所有程序包版本和受支持的设备类型的数据。

## 用户会话总数

“用户会话总数”****图显示选定时间段内的应用的日常用户会话数。

每个用户会话表示客户与应用交互的不同时间段。 每个用户会话均视为非活动状态时间的结束，因此，单个客户在相同的一天内可以进行多个用户会话。 请注意，此图表不会跟踪应用的唯一用户。

## 活动用户

“活动客户”****图显示在选定时间段内的某一天使用你的应用的客户数。

每个活动用户表示一个当天使用过你的应用的客户。 此图表不跟踪唯一用户会话（即，无论客户当天只使用你的应用一次还是多次，都将在此图表中表示一个客户）。

## 自定义事件

“自定义事件”****图显示你为应用定义的任何自定义事件的总发生次数。 这可以包括同一个客户的多次发生。

自定义事件通过在 [Microsoft 官方商城协定和盈利 SDK](../monetize/monetize-your-app-with-the-microsoft-store-engagement-and-monetization-sdk.md) 中使用 [Log](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomevents.log.aspx) 方法实现。



 







<!--HONumber=Jun16_HO4-->


