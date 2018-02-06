---
author: jnHs
Description: When submitting an add-on, the options on the Properties page help determine the behavior of your add-on when offered to customers.
title: "输入加载项属性"
ms.assetid: 26D2139F-66FD-479E-940B-7491238ADCAE
ms.author: wdg-dev-content
ms.date: 01/12/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, 加载项, 属性, 订阅期, 产品生命周期, 内容类型, iap, 应用内购买, 应用内产品"
ms.localizationpriority: high
ms.openlocfilehash: 63fc414c230e5a988013b1509280bfdb083a93c0
ms.sourcegitcommit: 446fe2861651f51a129baa80791f565f81b4f317
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/12/2018
---
# <a name="enter-add-on-properties"></a>输入加载项属性


在提交加载项时，“属性”****页面上的选项有助于确定在提供给客户时加载项的行为。

## <a name="product-type"></a>产品类型

首次[创建加载项](set-your-add-on-product-id.md)时，将选择你的产品类型。 你选择的产品类型将显示在此处，你无法更改它。

> [!TIP]
> 如果没有发布该加载项，并且需要选择不同的产品类型，则可以删除该提交并重新开始。

在此页面上看到的字段会有所不同，具体取决于加载项的产品类型。


## <a name="product-lifetime"></a>产品生命周期

如果选择的产品类型为**耐用品**，此处将显示**产品生命周期**。 耐用型加载项的默认**产品生命周期**为**永久**，这意味着加载项永不过期。 如果你愿意，可以设置“产品生命周期”****，以使加载项在设置的持续时间后（可选择 1 至 365 天）过期。


## <a name="quantity"></a>数量

如果选择的产品类型为**应用商店管理的易耗品**，此处将显示**数量**。 将需要输入一个 1 到 1000000 之间的数字。 此数量将在客户获取你的加载项时授予他们，并且应用商店将在应用报告客户使用该加载项时跟踪平衡。


## <a name="subscription-period"></a>订阅期限

如果选择的产品类型为**订阅**，此处将显示**订阅期限**。 选择一个选项以指定向客户收取订阅费用的频率。 默认选项为**每月，不过你也可以选择 **3 个月**、**6 个月**、**每年**或 **24 个月** 

> [!IMPORTANT]
> 加载项发布之后，将无法更改**订阅期限**选择。


## <a name="free-trial"></a>免费试用

如果选择的产品类型为**订阅**，此处还将显示**免费试用**。 默认选项为**没有免费试用**。 如果你愿意，可以让客户在设定的时间段（**1 周**或 **1 个月**）内免费使用加载项。 

> [!IMPORTANT]
> 加载项发布之后，将无法更改**免费试用**选择。


## <a name="content-type"></a>内容类型

无论加载项的产品类型是什么，将需要指示所提供内容的类型。 对于大部分加载项，内容类型应为**电子软件下载**。 如果列表中的另一个选项可以更好地描述加载项（例如，如果要提供音乐下载或电子书），请改为选择该选项。

以下是加载项内容类型的可能选项：

-   电子软件下载
-   电子书
-   电子杂志单期
-   电子报纸单期
-   音乐下载
-   音乐流
-   联机数据存储/服务
-   软件即服务
-   视频下载
-   视频流


## <a name="additional-properties"></a>其他属性

这些字段是所有类型的加载项的可选字段。

<span id="keywords" />
### <a name="keywords"></a>关键字

可以选择为每个提交的加载项提供最多十个关键字，每个关键字最多 30 个字符。 然后，你的应用便可查询匹配这些字词的加载项。 你可以使用此功能在应用中生成可加载加载项的屏幕，而无需直接在应用的代码中指定产品 ID。 然后，可以随时更改加载项关键字，而无需在应用中更改代码或重新提交应用。

若要查询此字段，请使用 [Windows.Services.Store 命名空间](https://msdn.microsoft.com/en-us/library/windows/apps/windows.services.store.aspx)中的 [StoreProduct.Keywords](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct#Windows_Services_Store_StoreProduct_Keywords) 属性。 （或者，如果使用的是 [Windows.ApplicationModel.Store 命名空间](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.store.aspx)，则使用 [ProductListing.Keywords](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.productlisting#Windows_ApplicationModel_Store_ProductListing_Keywords) 属性。）

> [!NOTE]
> 关键字不适用于面向 Windows 8 和 Windows 8.1 的程序包。

<span id="custom-developer-data" />
### <a name="custom-developer-data"></a>自定义开发人员数据

你可以在**自定义开发人员数据**字段（之前被称为**标记**）中输入最多 3000 个字符来为应用内产品提供额外的上下文。 在大多数情况下，它采用的是 XML 字符串的形式，但是可以在此字段中输入需要的任何内容。 然后应用可以查询此字段以读取其内容（尽管应用无法编辑数据并将更改传回。）

例如，假设你有一款游戏，并且你正在出售一个可帮助客户访问其他级别的加载项。 通过使用**自定义开发人员数据**字段，当客户拥有该加载项时，应用可以进行查询以查看有哪些级别可用。 可更新加载项**自定义开发人员数据**字段中的信息并发布该加载项的已更新提交来随时调整值（在本例中，即为包含的级别），而无需在应用中更改代码或重新提交应用。

若要查询此字段，请使用 [Windows.Services.Store 命名空间](https://msdn.microsoft.com/en-us/library/windows/apps/windows.services.store.aspx)中的 [StoreSku.CustomDeveloperData](https://msdn.microsoft.com/en-us/library/windows/apps/windows.services.store.storesku.customdeveloperdata.aspx) 属性。 （或者，如果使用的是 [Windows.ApplicationModel.Store 命名空间](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.store.aspx)，则使用 [ProductListing.Tag](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.store.productlisting.tag.aspx) 属性。）

> [!NOTE]
> **自定义开发人员数据**字段不适用于面向 Windows 8 和 Windows 8.1 的程序包。

 

 

 
