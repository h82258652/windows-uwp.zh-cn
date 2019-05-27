---
description: 在 Microsoft Store 分析 API 中使用此方法来获取聚合的外接程序获取数据以 JSON 格式 UWP 应用程序和 Xbox One 已引入通过 Xbox 开发人员门户 (XDP) 和 XDP 分析合作伙伴中心仪表板中提供的游戏。
title: 获取游戏和应用程序的外接程序获取数据
ms.date: 03/06/2019
ms.topic: article
keywords: Windows 10, uwp, 广告网络, 应用元数据
ms.localizationpriority: medium
ms.openlocfilehash: 518648d52c613a3dd5f1bca0d34a7f533b59733f
ms.sourcegitcommit: df8e4143e81a1c5fe1aa5f14407b8dd5f155a12e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/14/2019
ms.locfileid: "57829836"
---
# <a name="get-add-on-acquisitions-data-for-your-games-and-apps"></a>获取游戏和应用程序的外接程序获取数据 
在 Microsoft Store 分析 API 中使用此方法来获取聚合的外接程序获取数据以 JSON 格式 UWP 应用程序和 Xbox One 已引入通过 Xbox 开发人员门户 (XDP) 和 XDP 分析合作伙伴中心仪表板中提供的游戏。 

## <a name="prerequisites"></a>先决条件
若要使用此方法，首先需要执行以下操作： 

* 完成 Microsoft Store 分析 API 的所有[先决条件](access-analytics-data-using-windows-store-services.md)（如果尚未这样做）。 
* [获取 Azure AD 访问令牌](access-analytics-data-using-windows-store-services.md)，以供在此方法的请求标头中使用。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以获取新的令牌。 

> [!NOTE]
> 此 API 不在 2016 年 10 月 1 日之前提供每日聚合数据。 

## <a name="request"></a>请求

### <a name="request-syntax"></a>请求语法
| 方法 | 请求 URI |
| --- | --- | 
| GET | `https://manage.devcenter.microsoft.com/v1.0/my/analytics/addonacquisitions` |

### <a name="request-header"></a>请求头 
| Header | 在任务栏的搜索框中键入 | 描述 | 
| --- | --- | --- |
| 授权 | string | 必需。 在窗体中的 Azure AD 访问令牌**持有者** `<token>`。 |

### <a name="request-parameters"></a>请求参数
*ApplicationId*或*addonProductId*参数是必需的。 若要检索注册到该应用的所有加载项的购置数据，请指定 *applicationId* 参数。 若要检索单个外接程序的采集数据，请指定*addonProductId*参数。 如果同时指定这两个参数，会忽略 *inAppProductId* 参数。 

| 参数 | 在任务栏的搜索框中键入 | 描述 | 必需 | 
| --- | --- | --- | --- |
| applicationId | string | *ProductId*的 Xbox One 的游戏为其检索采集数据。 若要获取*productId*的您的游戏，导航到您的游戏中 XDP 分析程序，并检索*productId*从该 URL。 或者，如果从合作伙伴中心分析报告中，下载获取数据*productId*包含在.tsv 文件。 | 是 |
| addonProductId | string | *ProductId*外接程序想要检索采集数据。 | 是 |
| startDate | 日期 | 要检索的加载项购置数据日期范围中的开始日期。 默认值为当前日期。 | 否 |
| endDate | 日期 | 要检索的加载项购置数据日期范围中的结束日期。 默认值为当前日期。 | 否 |
| top | int | 要在请求中返回的数据行数。 如果未指定，最大值和默认值为 10000。 当查询中存在多行数据时，响应正文中包含的下一个链接可用于请求下一页数据。 | 否 |
| skip | int | 要在查询中跳过的行数。 使用此参数可以浏览较大的数据集。 例如，top=10000 和 skip=0，将检索前 10000 行数据；top=10000 和 skip=10000，将检索之后的 10000 行数据，依此类推。 | 否 |
| filter | string | 在响应中筛选行的一条或多条语句。 每个语句包含的响应正文和与 eq 或 ne 运算符相关联的值中的字段名称，可以使用组合语句和或。 字符串值必须用筛选器参数中的单个引号引起来。 例如，筛选 = 市场 eq 美国和性别 eq 是。 <br/> 你可以指定响应正文中的以下字段： <ul><li>**acquisitionType**</li><li>**age**</li><li>**storeClient**</li><li>**gender**</li><li>**market**</li><li>**osVersion**</li><li>**deviceType**</li><li>**sandboxId**</li></ul> | 否 |
| aggregationLevel | string | 指定用于检索聚合数据的时间范围。 可以是以下字符串之一：**day**、**week** 或 **month**。 如果未指定，默认值为 **day**。 | 否 |
| orderby | string | 对每个加载项购置的结果数据值进行排序的语句。 语法是 *orderby = 字段 [顺序] 字段 [顺序]...* *field* 参数可以是以下字符串之一。 <ul><li>**date**</li><li>**acquisitionType**</li><li>**age**</li><li>**storeClient**</li><li>**gender**</li><li>**market**</li><li>**osVersion**</li><li>**deviceType**</li><li>**orderName**</li></ul> 顺序参数是可选的并且可以为**asc**或**desc**指定按升序或降序为每个字段。 默认值为 **asc**。 <br/> 下面是一个 *orderby* 字符串的示例：*orderby=date,market* | 否 |
| groupby | string | 仅将数据聚合应用于指定字段的语句。 可以指定的字段如下所示： <ul><li>**date**</li><li>**applicationName**</li><li>**addonProductName**</li> <li>**acquisitionType**</li><li>**age**</li> <li>**storeClient**</li><li>**gender**</li> <li>**market**</li> <li>**osVersion**</li><li>**deviceType**</li><li>**paymentInstrumentType**</li><li>**sandboxId**</li><li>**xboxTitleIdHex**</li></ul> 返回的数据行会包含 *groupby* 参数中指定的字段，以及以下字段： <ul><li>**date**</li><li>**applicationId**</li><li>**addonProductId**</li><li>**acquisitionQuantity**</li></ul> Groupby 参数可用于*aggregationLevel*参数。 例如： *& groupby = 年龄、 市场和 aggregationLevel = 周* | 否 |

### <a name="request-example"></a>请求示例
以下示例演示了用于获取加载项购置数据的多个请求。 替换*addonProductId*并*applicationId*为外接程序或应用适当的 Store ID 的值。 

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/addonacquisitions?applicationId=9WZDNCRFJ314&startDate=1/1/2015&endDate=2/1/2015&skip=0 HTTP/1.1 

Authorization: Bearer <your access token> 

 

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/addonacquisitions?applicationId=9WZDNCRFJ314&startDate=1/1/2015&endDate=2/1/2015&skip=0&filter=market eq 'GB' and gender eq 'm' HTTP/1.1 

Authorization: Bearer <your access token>
```

## <a name="response"></a>响应

### <a name="response-body"></a>响应正文

| ReplTest1 | 在任务栏的搜索框中键入 | 描述 |
| --- | --- | --- |
| ReplTest1 | 数组 | 包含聚合加载项购置数据的对象数组。 有关每个对象中的数据的详细信息，请参阅下面的[加载项购置值](#add-on-acquisition-values)部分。 |
| @nextLink | string | 如果存在数据的其他页，此字符串中包含的 URI 可用于请求下一页数据。 例如，当请求的 **top** 参数设置为 10000，但查询的加载项购置数据超过 10000 行时，就会返回此值。 |
| TotalCount | int | 查询的数据结果中的行总数。 |

### <a name="add-on-acquisition-values"></a>加载项购置值
值数组中的元素包含以下值。

| 值 | 在任务栏的搜索框中键入 | 描述 | 
| --- | --- | --- |
| 日期 | string | 购置数据的日期范围内的第一个日期。 如果请求指定了某一天，此值就是该日期。 如果请求指定了一周、月或其他日期范围，此值是该日期范围内的第一个日期。 |
| addonProductId | string | *ProductId*外接程序为其检索采集数据。 |
| addonProductName | string | 加载项的显示名称。 此值才会显示在响应数据*aggregationLevel*参数设置为**天**，除非您指定**addonProductName**字段中*groupby*参数。 |
| applicationId | string | *ProductId*想检索外接程序获取数据的应用。 |
| applicationName | string | 游戏的显示名称。 |
| deviceType | string | 用于指定完成购置的设备类型的以下字符串之一： <ul><li>"PC"</li><li>"电话"</li><li>"控制台"</li><li>"IoT"</li><li>"服务器"</li><li>"Tablet"</li><li>"Holographic"</li><li>"未知"</li></ul> |
| storeClient | string | 用于指示发生购置的 Microsoft Store 版本的以下字符串之一： <ul><li>"Windows Phone 应用商店 （客户端）"</li><li>"Microsoft Store （客户端）"(或"Windows 应用商店 （客户端）"如果在 2018 年 3 月 23 日之前查询数据)</li><li>"Microsoft Store (web)"(或"Windows 应用商店 (web)"如果在 2018 年 3 月 23 日之前查询数据)</li><li>"批量购买的组织"</li><li>"其他"</li></ul> |
| osVersion | string | 发生购置行为的操作系统版本。 对于此方法，此值始终为"Windows 10"。 |
| market | string | 发生购置行为的市场的 ISO 3166 国家/地区代码。 |
| gender | string | 用于指定进行购置的用户的性别的以下字符串之一： <ul><li>"m"</li><li>"f"</li><li>"未知"</li></ul> |
| age | string | 用于指示进行购置的用户的年龄段的以下字符串之一： <ul><li>"小于 13"</li><li>"13-17"</li><li>"18-24"</li><li>"25-34"</li><li>"35-44"</li><li>"44-55"</li><li>"大于 55"</li><li>"未知"</li></ul> |
| acquisitionType | string | 下列字符串之一，用于指示购置类型： <ul><li>"免费" </li><li>"试用" </li><li>"Paid"</li><li>"促销代码" </li><li>"、 Iap"</li><li>"订阅、 Iap"</li><li>"专用受众"</li><li>"Pre Order"</li><li>"Xbox Game Pass"（或者"游戏通过"2018 年 3 月 23 日之前查询数据）</li><li>"磁盘"</li><li>"预付的代码"</li><li>"收费 Pre 顺序"</li><li>"取消预顺序"</li><li>"失败前顺序"</li></ul> |
| acquisitionQuantity | 整数 | 发生的购置数。 |
| inAppProductId | string | 使用此加载项的位置的产品的产品 ID。  |
| inAppProductName | string | 使用此加载项的位置的产品的产品名称。  |
| paymentInstrumentType | string | 用于获取付款方式类型。  |
| sandboxId | string | 创建游戏的沙盒 ID。 这可为值**零售**或专用沙盒 id。  |
| xboxTitleId | string | Xbox 标题从 XDP，如果适用的产品的 ID。  |
| localCurrencyCode | string | 根据合作伙伴中心帐户的国家/地区的本地货币代码。  |
| xboxProductId | string | 从 XDP，如果适用的产品的 Xbox 产品 ID。  |
| availabilityId | string | 从 XDP，如果适用的产品可用性 ID。  |
| skuId | string | 从 XDP，如果适用的产品 SKU ID。  |
| skuDisplayName | string | 从 XDP，如果适用的产品 SKU 显示名称。  |
| xboxParentProductId | string | 从 XDP，如果适用的产品 Xbox 父产品 ID。  |
| parentProductName | string | 如果适用，则父 XDP，从产品的产品名称。  |
| productTypeName | string | 从 XDP，如果适用的产品的产品类型名称。  |
| purchaseTaxType | string | 如果适用，请从 XDP，购买的产品的税务类型。  |
| purchasePriceUSDAmount | 数字 | 外接程序，客户支付的金额转换为美元。  |
| purchasePriceLocalAmount | 数字 | 应用于外接程序的税额。  |
| purchaseTaxUSDAmount | 数字 | 应用于外接程序，税额转换为美元。  |
| purchaseTaxLocalAmount | 数字 | 如果适用，请从 XDP，购买本地税额的产品。  |

### <a name="response-example"></a>响应示例
以下示例举例说明此请求的 JSON 响应正文。 

```JSON
{ 
  "Value": [ 
    { 
            "inAppProductId": "9NBLGGH1864K", 
            "inAppProductName": "866879", 
            "addonProductId": "9NBLGGH1864K", 
            "addonProductName": "866879", 
            "date": "2017-11-05", 
            "applicationId": "9WZDNCRFJ314", 
            "applicationName": "Tetris Blitz", 
            "acquisitionType": "Iap", 
            "age": "35-49", 
            "deviceType": "Phone", 
            "gender": "m", 
            "market": "US", 
            "osVersion": "Windows Phone 8.1", 
            "paymentInstrumentType": "Credit Card", 
            "sandboxId": "RETAIL", 
            "storeClient": "Windows Phone Store (client)", 
            "xboxTitleId": "", 
            "localCurrencyCode": "USD", 
            "xboxProductId": "00000000-0000-0000-0000-000000000000", 
            "availabilityId": "", 
            "skuId": "", 
            "skuDisplayName": "Full", 
            "xboxParentProductId": "", 
            "parentProductName": "Tetris Blitz", 
            "productTypeName": "Add-On", 
            "purchaseTaxType": "", 
            "acquisitionQuantity": 1, 
            "purchasePriceUSDAmount": 1.08, 
            "purchasePriceLocalAmount": 0.09, 
            "purchaseTaxUSDAmount": 1.08, 
            "purchaseTaxLocalAmount": 0.09 
        } 
    ], 

    "@nextLink": null, 
    
    "TotalCount": 7601 
} 
```