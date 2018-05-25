---
title: 开发人员中心上的特权配置
author: aablackm
description: 了解如何在 Windows 开发人员中心上配置特权
ms.author: aablackm
ms.date: 04/09/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: low
keywords: Xbox Live, Xbox, 游戏, uwp, windows 10, Xbox one, 特权, Windows 开发人员中心
ms.openlocfilehash: 77b779bfd4ffcbff31267e93c9475948825a2b00
ms.sourcegitcommit: 91511d2d1dc8ab74b566aaeab3ef2139e7ed4945
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/30/2018
---
# <a name="configure-privileges-on-windows-development-center"></a>在 Windows 开发人员中心上配置特权

特权配置页面用于指示是否限制玩家流式传输你的作品和 [Mixer](https://mixer.com/) 等服务。 默认情况下，你的游戏将不限制在任何流式传输平台上广播，只有当你要限制广播时才需要更改此页面。 可以通过以下两种方式限制广播。 你可以通过选中**默认**部分中的复选框来禁止到处广播，也可以通过在**沙盒替代**部分中添加沙盒来限制通过沙盒广播。

选中**默认**部分中的复选框可限制在所有服务和沙盒中广播此作品。

![默认广播受限](../../images/dev-center/privileges/default-privileges-check.JPG)

若要限制在特定沙盒中广播，请单击**沙盒替代**部分中的**添加**按钮。 从下拉列表中选择目标沙盒并选中下方的复选框，以限制在选定沙盒上广播此作品。 可以取消选中或删除沙盒替代，以删除对广播的限制。

![沙盒广播受限](../../images/dev-center/privileges/sandbox-privileges-check.JPG)

单击**保存**按钮以保留对这些设置所做的所有配置更改。

> [!NOTE]
> 通过选中此复选框禁用广播仅可禁止在电脑上通过 Xbox 控制台或 Windows 游戏栏进行流式传输。 选中此页面上的复选框不会阻止使用捕获卡或其他外部捕获或流式传输服务。