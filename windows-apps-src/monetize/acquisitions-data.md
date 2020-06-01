---
description: 此方法用于在 Microsoft Store 分析 API 中获取 JSON 格式的聚合采集数据适用于 UWP 应用和 Xbox One 已引入通过 Xbox 开发人员门户 (XDP) 和 XDP 分析仪表板中提供的游戏。
title: 获取游戏和应用程序获取数据
ms.date: 03/06/2019
ms.topic: article
keywords: Windows 10, uwp, 广告网络, 应用元数据
ms.localizationpriority: medium
ms.openlocfilehash: beca5620f25713e8a07e5dbaf64e955d920702a7
ms.sourcegitcommit: df8e4143e81a1c5fe1aa5f14407b8dd5f155a12e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/14/2019
ms.locfileid: "57829837"
---
# <a name="get-acquisitions-data-for-your-games-and-apps"></a>获取游戏和应用程序获取数据 
此方法用于在 Microsoft Store 分析 API 中获取 JSON 格式的聚合采集数据适用于 UWP 应用和 Xbox One 已引入通过 Xbox 开发人员门户 (XDP) 和 XDP 分析仪表板中提供的游戏。 

> [!NOTE]
> 此 API 不在 2016 年 10 月 1 日之前提供每日聚合数据。 

## <a name="prerequisites"></a>先决条件
若要使用此方法，首先需要执行以下操作： 

* 完成 Microsoft Store 分析 API 的所有[先决条件](access-analytics-data-using-windows-store-services.md)（如果尚未这样做）。 
* [获取 Azure AD 访问令牌](access-analytics-data-using-windows-store-services.md)，以供在此方法的请求标头中使用。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以获取新的令牌。 

## <a name="request"></a>请求
### <a name="request-syntax"></a>请求语法

| 方法 | 请求 URI |
| --- | --- |
| GET | `https://manage.devcenter.microsoft.com/v1.0/my/analytics/acquisitions` |

### <a name="request-header"></a>请求头

| Header | 在任务栏的搜索框中键入 | 描述 |
| --- | --- | --- |
| 授权 | string | 必需。 在窗体中的 Azure AD 访问令牌**持有者** `<token>`。 |

### <a name="request-parameters"></a>请求参数

| 参数 | 在任务栏的搜索框中键入 | 描述 | 必需 |
| --- | --- | --- | --- |
| applicationId | string | 要检索其购置数据的 Xbox One 游戏的产品 ID。 若要获取您的游戏的产品 ID，请导航到您的游戏中 XDP 分析程序，并从 URL 检索产品 ID。 或者，如果从合作伙伴中心的分析报告下载收购数据，请在.tsv 文件中包括的产品 ID。  | 是 |
| startDate | date | 要检索的购置数据日期范围中的开始日期。 默认值为当前日期。  | 否 |
| endDate | date | 要检索的购置数据日期范围中的结束日期。 默认值为当前日期。  | 否 |
| top | 整数 | 要返回的数据的行数。 如果未指定，最大值和默认值为 10000。 当查询中存在多行数据时，响应正文中包含的下一个链接可用于请求下一页数据。  | 否 |
| skip | 整数 | 要在查询中跳过的行数。 使用此参数可以浏览较大的数据集。 例如，*顶部 = 10000 和跳过 = 0*检索的数据，首先 10000 行*顶部 = 10000 和跳过 = 10000*检索数据和等等的下一步 10000 行。  | 否 |
| filter | string | 在响应中筛选行的一条或多条语句。 每条语句包含的响应正文中的字段名称和值使用 **eq** 或 **ne** 运算符进行关联，并且语句可以使用 **and** 或 **or** 进行组合。 字符串值必须用筛选器参数中的单个引号引起来。 例如，*filter=market eq 'US' and gender eq 'm'* 。  <br/> 你可以指定响应正文中的以下字段： <ul><li>**acquisitionType**</li><li>**age**</li><li>**storeClient**</li><li>**gender**</li><li>**market**</li><li>**osVersion**</li><li>**deviceType**</li><li>**sandboxId**</li></ul> | 否 |
| aggregationLevel | string | 指定用于检索聚合数据的时间范围。 可以是以下字符串之一：**day**、**week** 或 **month**。 如果未指定，默认值为 **day**。  | 否 |
| orderby | string | 对每个购置的结果数据值进行排序的语句。 语法是 *orderby = 字段 [顺序] 字段 [顺序]...* *field* 参数可以是以下字符串之一。 <ul><li>**date**</li><li>**acquisitionType**</li><li>**age**</li><li>**storeClient**</li><li>**gender**</li><li>**market**</li><li>**osVersion**</li><li>**deviceType**</li><li>**paymentInstrumentType**</li><li>**sandboxId**</li><li>**xboxTitleIdHex**</li></ul> *order* 参数是可选的，可以是 **asc** 或 **desc**，用于指定每个字段的升序或降序排列。 默认值为 **asc**。 下面是一个 *orderby* 字符串的示例：*orderby=date,market*  | 否 |
| groupby | string | 仅将数据聚合应用于指定字段的语句。 可以指定的字段如下所示： <ul><li>**date**</li><li>**applicationName**</li><li>**acquisitionType**</li><li>**ageGroup**</li><li>**storeClient**</li><li>**gender**</li><li>**market**</li><li>**osVersion**</li><li>**deviceType**</li><li>**paymentInstrumentType**</li><li>**sandboxId**</li><li>**xboxTitleIdHex**</li></ul> 返回的数据行会包含 *groupby* 参数中指定的字段，以及以下字段： <ul><li>**date**</li><li>**applicationId**</li><li>**acquisitionQuantity**</li></ul> *Groupby*参数可以与 aggregationLevel 参数一起使用。 例如： *& groupby = ageGroup、 市场和 aggregationLevel = 周*  | 否 |

### <a name="request-example"></a>请求示例
以下示例演示用于获取 Xbox One 游戏购置数据的多个请求。 替换*applicationId*与您的游戏的产品 ID 的值。  

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/acquisitions?applicationId=9WZDNCRFHXHT&startDate=1/1/2017&endDate=2/1/2019&top=10&skip=0 HTTP/1.1 
Authorization: Bearer <your access token> 

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/acquisitions?applicationId=9WZDNCRFHXHT&startDate=1/1/2017&endDate=2/1/2019&skip=0&filter=market eq 'US' and gender eq 'm' HTTP/1.1 
Authorization: Bearer <your access token> 
```

## <a name="response"></a>响应

### <a name="response-body"></a>响应正文
| ReplTest1 | 在任务栏的搜索框中键入 | 描述 |
| --- | --- | --- |
| ReplTest1 | 数组 | 包含游戏聚合购置数据的对象数组。 有关每个对象中的数据的详细信息，请参阅以下[购置值](#acquisition-values)部分。 |
| @nextLink | string | 如果存在数据的其他页，此字符串中包含的 URI 可用于请求下一页数据。 例如，当请求的 **top** 参数设置为 10000，但查询的购置数据超过 10000 行时，就会返回此值。 |
| TotalCount | 整数 | 查询的数据结果中的行总数。 |

### <a name="acquisition-values"></a>购置值 
*Value* 数组中的元素包含以下值。 

| ReplTest1 | 在任务栏的搜索框中键入 | 描述 |
| --- | --- | --- |
| date | string | 购置数据的日期范围内的第一个日期。 如果请求指定了某一天，此值就是该日期。 如果请求指定了一周、月或其他日期范围，此值是该日期范围内的第一个日期。 |
| applicationId | string | 要检索其购置数据的 Xbox One 游戏的产品 ID。 |
| applicationName | string | 游戏的显示名称。 |
| acquisitionType | string | 下列字符串之一，用于指示购置类型：  <ul><li>**免费**</li><li>**试用版**</li><li>**付费**</li><li>**促销代码**</li><li>**Iap**</li><li>**订阅、 Iap**</li><li>**私有访问群体**</li><li>**Pre 顺序**</li><li>**Xbox Game Pass**（或者，如果在 2018 年 3 月 23 之前查询数据，则是 **Game Pass**）</li><li>**Disk**</li><li>**预付的代码**</li><li>**计费的 Pre 顺序**</li><li>**已取消的预顺序**</li><li>**失败的 Pre 订单**</li></ul> |
| age | string | 用于指示进行购置的用户的年龄段的以下字符串之一： <ul><li>**小于 13**</li><li>**13-17**</li><li>**18-24**</li><li>**25-34**</li><li>**35-44**</li><li>**44-55**</li><li>**大于 55**</li><li>**Unknown**</li></ul> |
| deviceType | string | 用于指定完成购置的设备类型的以下字符串之一： <ul><li>**PC**</li><li>**Phone**</li><li>**Console**</li><li>**IoT**</li><li>**Server**</li><li>**平板电脑**</li><li>**Holographic**</li><li>**Unknown**</li></ul> |
| gender | string | 用于指定进行购置的用户的性别的以下字符串之一： <ul><li>**m**</li><li>**f**</li><li>**Unknown**</li></ul> |
| market | string | 发生购置行为的市场的 ISO 3166 国家/地区代码。 |
| osVersion | string | 发生购置行为的操作系统版本。 对于此方法，此值始终是 **Windows 10**。 |
| paymentInstrumentType | string | 用于指示用于购置的付款说明的以下字符串之一： <ul><li>**信用卡**</li><li>**直接借记卡**</li><li>**推断的购买**</li><li>**MS 余额**</li><li>**移动运营商**</li><li>**联机银行转帐**</li><li>**PayPal**</li><li>**拆分事务**</li><li>**令牌兑换**</li><li>**零金额**</li><li>**eWallet**</li><li>**Unknown**</li></ul> |
| sandboxId | string | 为游戏创建的沙盒的 ID。 这可为值**零售**或专用沙盒 id。 |
| storeClient | string | 用于指示发生购置的 Microsoft Store 版本的以下字符串之一： <ul><li>**Windows Phone 应用商店 （客户端）**</li><li>**Microsoft Store (client)** （或 **Windows Store (client)** ，如果查询 2018 年 3 月 23 日之前的数据） </li><li>**Microsoft Store (web)** （或 **Windows Store (web)** ，如果查询 2018 年 3 月 23 日之前的数据） </li><li>**由组织的批量购买**</li><li>**其他**</li></ul> |
| xboxTitleId | string | Xbox 开发人员门户 (XDP) 为支持 Xbox Live 的游戏指定的 Xbox Live 标题 ID（以十六进制值表示）。 |
| acquisitionQuantity | 数字 | 在指定的聚合级别期间发生的购置数。 |
| purchasePriceUSDAmount | 数字 | 客户为购置所付金额，已使用每月汇率转换为美元。 |
| purchaseTaxUSDAmount | 数字 | 购置税额，已转换为美元。 |
| localCurrencyCode | string | 根据合作伙伴中心帐户的国家/地区的本地货币代码。  |
| xboxProductId | string | 从 XDP，如果适用的产品的 Xbox 产品 ID。  |
| availabilityId | string | 从 XDP，如果适用的产品可用性 ID。  |
| skuId | string | 从 XDP，如果适用的产品 SKU ID。  |
| skuDisplayName  | string | 如果适用，SKU 将显示 XDP，从产品的名称。  |
| xboxParentProductId | string | 从 XDP，如果适用的产品 Xbox 父产品 ID。  |
| parentProductName | string | 如果适用，则父 XDP，从产品的产品名称。  |
| productTypeName | string | 从 XDP，如果适用的产品的产品类型名称。  |
| purchaseTaxType | string | 如果适用，请从 XDP，购买的产品的税务类型。  |
| purchasePriceLocalAmount | 数字 | 如果适用，请从 XDP，购买的产品价格本地量。  |
| purchaseTaxLocalAmount | 数字 | 如果适用，请从 XDP，购买本地税额的产品。  |

### <a name="response-example"></a>响应示例
以下示例举例说明此请求的 JSON 响应正文。 

```JSON
{ 
    "Value": [ 
        { 
            "date": "2019-01-15T01:00:00.0000000Z", 
            "applicationId": "9WZDNCRFHXHT", 
            "applicationName": null, 
            "acquisitionType": "Paid", 
            "age": null, 
            "deviceType": "Phone", 
            "gender": null, 
            "market": "US", 
            "osVersion": "Windows 10", 
            "paymentInstrumentType": null, 
            "sandboxId": "RETAIL", 
            "storeClient": "Microsoft Store (client)", 
            "xboxTitleId": null, 
            "localCurrencyCode": "USD", 
            "xboxProductId": null, 
            "availabilityId": "B42LRTSZ2MCJ", 
            "skuId": "0010", 
            "skuDisplayName": null, 
            "xboxParentProductId": null, 
            "parentProductName": null, 
            "productTypeName": "Game", 
            "purchaseTaxType": "TaxesNotIncluded", 
            "acquisitionQuantity": 1, 
            "purchasePriceUSDAmount": 3.08, 
            "purchasePriceLocalAmount": 3.08, 
            "purchaseTaxUSDAmount": 0.09, 
            "purchaseTaxLocalAmount": 0.09 
        } 
    ], 

    "@nextLink": "acquisitions?applicationId=9WZDNCRFHXHT&aggregationLevel=day&startDate=2017-01-01T08:00:00.0000000Z&endDate=2019-01-16T08:44:15.6045249Z&top=1&skip=1", 
    
    "TotalCount": 12221 
} 
```

 
