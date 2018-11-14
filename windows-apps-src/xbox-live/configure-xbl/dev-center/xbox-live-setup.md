---
title: Xbox Live 设置配置
author: shrutimundra
description: 介绍如何在合作伙伴中心中配置 Xbox Live 设置。
ms.assetid: ''
ms.author: kevinasg
ms.date: 10/30/2017
ms.topic: article
ms.localizationpriority: medium
keywords: Xbox live，Xbox，游戏，uwp，windows 10，Xbox one，合作伙伴中心，Xbox Live 设置
ms.openlocfilehash: 8d3aa7c7407dc3d3fd3f2bcdf9640b605c734447
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2018
ms.locfileid: "6261171"
---
# <a name="configure-xbox-live-setup-in-partner-center"></a>在合作伙伴中心中配置 Xbox Live 设置

可以使用[合作伙伴中心](https://developer.microsoft.com/dashboard)配置 Xbox Live 的属性与你的游戏相关联的初始集。 通过执行以下操作添加配置：

1. 导航到作品的 **Xbox Live 设置**部分（位于**服务** > **Xbox Live** > **Xbox Live 设置**下面）。
2. 在此页面上，你可以设置作品名称、默认区域设置、产品类型、设备系列和禁运日期。 设置完配置后，单击**保存**按钮以提交更改。

## <a name="title-names"></a>作品名称
通过单击**添加本地化作品**，你可以输入产品的名称并选择其本地化语言。 此处请注意，作品名称应与你在提交的属性页上定义的本地化产品名称相对应。 默认语言为英语 (en-US)。

![在合作伙伴中心添加本地化的作品对话框的图像](../../images/dev-center/xbox-live-setup/xbox-live-setup-1.png)

## <a name="default-locale"></a>默认区域设置
通过此选项，你可以设置用于在 Xbox Live 服务配置中配置所有字符串的默认语言。 例如，如果你将默认区域设置设为了西班牙语 (es-ES)，并且想要配置一项成就，则至少成就名称和描述必须使用西班牙语。 换言之，你不能将此选项设置为西班牙语，但只是用英语提供成就信息。 你提供的所有 Xbox Live 服务配置必须与默认区域设置的版本相同。 默认情况下，默认区域设置设为英语 (en-US)。
> [!NOTE]
> 此外，可以在“本地化字符串”页面上本地化所有字符串。  

![选择要在合作伙伴中心中选择默认区域设置的下拉列表的图像](../../images/dev-center/xbox-live-setup/xbox-live-setup-2.png)

## <a name="product-type"></a>产品类型
此下拉菜单允许你更改产品的类型。 它默认为类型**游戏**。 你所做的选择将影响 XboxLivefeatures 提供给你。 有三个可供选择的选项：
1. 应用 
2. 游戏 
3. 游戏演示 

![选择要在合作伙伴中心中选择产品类型的下拉列表的图像](../../images/dev-center/xbox-live-setup/xbox-live-setup-3.png)

## <a name="device-families"></a>设备系列
此配置允许你选择作品可以在哪些设备类型上访问 Xbox Live。 默认情况下，启用所有设备系列。 你可以选中要启用的设备。

![若要在合作伙伴中心中选择设备系列的选择复选框的图像](../../images/dev-center/xbox-live-setup/xbox-live-setup-4.png)

## <a name="embargo-date"></a>禁运日期
你选择的日期将确定你的 Xbox Live 配置向公众发布的日期。 需要注意的是，在到达禁运日期之前，即使你已经将更改发布到 RETAIL，这些更改也不会生效。 进一步说明：
1. 如果你选择的是未来的日期，则更改将于该日期向公众发布。
2. 如果你选择的是过去的日期，则一旦你将更改发布到 RETAIL，即会向公众发布这些更改。

单击日期时间选取器后，它将会展开，以便让你选择准确的日期和时间。 单击**确定**后，将会设置禁运日期。

![在合作伙伴中心中设置禁运日期的图像](../../images/dev-center/xbox-live-setup/xbox-live-setup-5.png)

## <a name="advanced-settings"></a>高级设置

可以单击**显示选项**以设置**多点入网**。 多点入网允许同一个用户同时从多个设备登录 Xbox Live。 Xbox Live 功能（例如成就和多人游戏）的访问权限将会受限。 因此，不建议对游戏使用此选项。 可通过选中复选框来启用此选项。
