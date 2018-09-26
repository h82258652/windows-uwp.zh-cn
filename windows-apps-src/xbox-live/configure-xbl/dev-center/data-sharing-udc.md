---
title: 在开发人员中心配置数据共享
author: KevinAsgari
description: 介绍如何在开发人员中心配置数据共享，以允许其他应用、游戏和服务访问 Xbox Live 设置。
ms.assetid: ''
ms.author: kevinasg
ms.date: 02/21/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, udc, 通用开发人员中心
ms.openlocfilehash: 2b56a4d3fa857222e2b93cd0f9e39133c16c2ebc
ms.sourcegitcommit: e4f3e1b2d08a02b9920e78e802234e5b674e7223
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/26/2018
ms.locfileid: "4212226"
---
# <a name="configure-data-sharing-on-dev-center"></a>在开发人员中心配置数据共享

可以使用 [Windows 开发人员中心](https://developer.microsoft.com/dashboard/windows/overview)以允许其他服务、游戏和应用访问你的作品的 Xbox Live 设置和数据。 例如，你可能需要 Web 服务在网站上显示排行榜，也可能需要一个伴侣应用来访问游戏作品存储，以查看或修改保存的游戏数据。

默认情况下，只有主题作品本身可以访问存储在 Xbox Live 服务上的设置和数据。 可以通过在开发人员中心配置数据共享进行更改。

> [!NOTE]
> 本主题不适用于 Xbox Live 创意者计划中的主题作品。

通过执行以下操作添加配置：

1. 在[开发人员中心](https://developer.microsoft.com/dashboard/windows/overview)选择主题作品后，导航到**服务** > **Xbox Live**。

2. 单击**数据共享**链接。

3. 单击想要授予访问权限的设置，然后单击“添加应用/服务”按钮。 这将向配置为可访问此设置的应用/服务列表的底部添加一个新行。

4. 在下拉框中，选择应用或服务的类型，然后填写详细信息框，以指示将访问该数据的应用、应用或服务的主题作品 ID 或服务 ID。

5. 选择应用或服务是否只能读取数据，还是具有数据的完全访问权限。

6. 对每个设置以及需要访问这些设置的每个应用或服务重复上述步骤。 可以单击**删除**以删除某个条目。

7. 结束后，单击**保存**按钮保存更改。

![数据共享添加应用或服务屏幕](../../images/dev-center/data-sharing-2.png)
