---
author: jnHs
Description: "在提交加载项时，“属性”页面上的选项有助于确定在提供给客户时加载项的行为。"
title: "输入加载项属性"
ms.assetid: 26D2139F-66FD-479E-940B-7491238ADCAE
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 186088f249c2e6fe116c970bd1969fcb59863ba6
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="enter-add-on-properties"></a>输入加载项属性


在提交加载项时，“属性”****页面上的选项有助于确定在提供给客户时加载项的行为。

## <a name="product-type"></a>产品类型

首次[创建加载项](set-your-add-on-product-id.md)时，将选择你的产品类型。 你选择的产品类型将显示在此处，你无法更改它。

> **注意**  如果你没有发布该加载项， 并且需要选择不同的产品类型，则可以删除该提交并重新开始。 

你可能会看到以下字段，具体取决于所选的产品类型：

### <a name="product-lifetime"></a>产品生命周期
如果你选择的产品类型为“耐用品”****，“产品生命周期”****将显示在此处。 持久型加载项的默认“产品生命周期”****为“永久”****，这意味着加载项永不过期。 如果你愿意，可以设置“产品生命周期”****，以使加载项在设置的持续时间后（可选择 1 至 365 天）过期。 

### <a name="quantity"></a>数量
如果为产品类型选择了“应用商店管理的易耗品”****，“数量”****将显示在此处。 你将需要输入 1 到 1000000 之间的数字。 此数量将在客户获取你的加载项时授予他们，并且应用商店将在应用报告客户使用该加载项时跟踪平衡。

## <a name="content-type"></a>内容类型

无论加载项产品类型是什么，你还需要指示所提供内容的类型。 对于大部分加载项，内容类型应为“电子软件下载”****。 如果列表中的另一个选项可以更好地描述你的加载项（例如，如果要提供音乐下载或电子书），请改为选择该选项。 

以下是加载项内容类型的可能选项：

-   电子软件下载
-   电子书
-   电子杂志单期
-   电子报纸单期
-   音乐下载
-   音乐流
-   联机数据存储/服务
-   视频下载
-   视频流
-   软件即服务

## <a name="keywords"></a>关键字

可以选择为每个提交的加载项提供最多十个关键字，每个关键字最多 30 个字符。 然后，你的应用便可查询匹配这些字词的加载项。 你可以使用此功能在应用中生成可加载加载项的屏幕，而无需直接在应用的代码中指定产品 ID。 然后，可以随时更改加载项关键字，而无需在应用中更改代码或重新提交应用。

> **注意**  关键字不适用于面向 Windows 8 和 Windows 8.1 的程序包。

## <a name="custom-developer-data"></a>自定义开发人员数据

你可以在**自定义开发人员数据**字段（之前被称为**标记**）中输入最多 3000 个字符来为应用内产品提供额外的上下文。 在大多数情况下，它采用的是 XML 字符串的形式，但是你可以在此字段中输入你需要的任何内容。

若要查询此字段，请使用 [Windows.Services.Store 命名空间](https://msdn.microsoft.com/en-us/library/windows/apps/windows.services.store.aspx)中的 [StoreSku.CustomDeveloperData](https://msdn.microsoft.com/en-us/library/windows/apps/windows.services.store.storesku.customdeveloperdata.aspx) 属性。 （或者，如果你使用的是 [Windows.ApplicationModel.Store 命名空间](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.store.aspx)，则使用 [ProductListing.Tag](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.store.productlisting.tag.aspx) 属性。)

例如，假设你有一款游戏，并且你正将一袋金币作为加载项来出售。 通过使用“自定义开发人员数据”****字段，应用可以查询这袋金币。 可通过更新加载项“自定义开发人员数据”****字段中的信息来随时调整值（在本例中，为袋中的金币数），而无需在应用中更改代码或重新提交应用。

> **注意**  **自定义开发人员数据**字段不适用于面向 Windows 8 和 Windows 8.1 的程序包。

 

 

 




