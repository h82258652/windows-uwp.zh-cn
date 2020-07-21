---
description: 在 Microsoft Store analytics API 中使用此方法，以 JSON 格式获取已通过 Xbox 开发人员门户（XDP）引入并可在 XDP Analytics 合作伙伴中心仪表板中使用的的聚合外接程序获取数据。
title: 获取游戏和应用的附加设备购置数据
ms.date: 03/06/2019
ms.topic: article
keywords: Windows 10, uwp, 广告网络, 应用元数据
ms.localizationpriority: medium
ms.openlocfilehash: 3e1349582515ce66232ea8266efc588610faae98
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493562"
---
# <a name="get-add-on-acquisitions-data-for-your-games-and-apps"></a>获取游戏和应用的附加设备购置数据 
在 Microsoft Store analytics API 中使用此方法，以 JSON 格式获取已通过 Xbox 开发人员门户（XDP）引入并可在 XDP Analytics 合作伙伴中心仪表板中使用的的聚合外接程序获取数据。 

## <a name="prerequisites"></a>必备条件
若要使用此方法，首先需要执行以下操作： 

* 完成 Microsoft Store 分析 API 的所有[先决条件](access-analytics-data-using-windows-store-services.md)（如果尚未这样做）。 
* [获取 Azure AD 访问令牌](access-analytics-data-using-windows-store-services.md)，以供在此方法的请求标头中使用。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以获取新的令牌。 

> [!NOTE]
> 此 API 在10月 2016 1 日之前未提供每日聚合数据。 

## <a name="request"></a>请求

### <a name="request-syntax"></a>请求语法
| 方法 | 请求 URI |
| --- | --- | 
| GET | `https://manage.devcenter.microsoft.com/v1.0/my/analytics/addonacquisitions` |

### <a name="request-header"></a>请求头 
| 标头 | 类型 | 描述 | 
| --- | --- | --- |
| 授权 | 字符串 | 必需。 Azure AD 访问令牌的格式为 Bearer `<token>`。 |

### <a name="request-parameters"></a>请求参数
*ApplicationId*或*addonProductId*参数是必需的。 若要检索注册到该应用的所有加载项的购置数据，请指定 *applicationId* 参数。 若要检索单个外接程序的获取数据，请指定*addonProductId*参数。 如果同时指定这两个参数，会忽略 *inAppProductId* 参数。 

| 参数 | 类型 | 说明 | 必需 | 
| --- | --- | --- | --- |
| applicationId | 字符串 | 要为其检索获取数据的 Xbox one 游戏的*产品 id* 。 若要获取你的游戏的*productId* ，请导航到 XDP Analytics 计划中的游戏，并从 URL 中检索*productId* 。 或者，如果您从 "合作伙伴中心分析" 报表下载您的购买数据，则*productId*将包含在 tsv 文件中。 | 是 |
| addonProductId | string | 要检索其获取数据的外接程序的*productId* 。 | 是 |
| startDate | date | 要检索的加载项购置数据日期范围中的开始日期。 默认值为当前日期。 | 否 |
| endDate | 日期 | 要检索的加载项购置数据日期范围中的结束日期。 默认值为当前日期。 | 否 |
| top | int | 要在请求中返回的数据行数。 如果未指定，最大值和默认值为 10000。 当查询中存在多行数据时，响应正文中包含的下一个链接可用于请求下一页数据。 | 否 |
| skip | int | 要在查询中跳过的行数。 使用此参数可以浏览较大的数据集。 例如，top=10000 和 skip=0，将检索前 10000 行数据；top=10000 和 skip=10000，将检索之后的 10000 行数据，依此类推。 | 否 |
| filter | string | 在响应中筛选行的一条或多条语句。 每条语句包含的响应正文中的字段名称和值使用 eq 或 ne 运算符进行关联，并且语句可以使用 and 或 or 进行组合。 filter 参数中的字符串值必须使用单引号引起来。 例如，filter=market eq 'US' and gender eq 'm'。 <br/> 可以指定响应正文中的以下字段： <ul><li>**acquisitionType**</li><li>**年**</li><li>**storeClient**</li><li>**性别**</li><li>**market**</li><li>**osVersion**</li><li>**deviceType**</li><li>**sandboxId**</li></ul> | 否 |
| aggregationLevel | string | 指定用于检索聚合数据的时间范围。 可以是以下字符串之一：**day**、**week** 或 **month**。 如果未指定，默认值为 **day**。 | 否 |
| orderby | string | 对每个加载项购置的结果数据值进行排序的语句。 语法为*orderby = field [order]，field [order],...**字段*参数可以为以下字符串之一： <ul><li>**date**</li><li>**acquisitionType**</li><li>**年**</li><li>**storeClient**</li><li>**性别**</li><li>**market**</li><li>**osVersion**</li><li>**deviceType**</li><li>**orderName**</li></ul> order 参数是可选的，可以是 **asc** 或 **desc**，用于指定每个字段的升序或降序排列。 默认值为**asc**。 <br/> 下面是一个 *orderby* 字符串的示例：*orderby=date,market* | 否 |
| groupby | string | 仅将数据聚合应用于指定字段的语句。 可以指定的字段如下所示： <ul><li>**date**</li><li>**applicationName**</li><li>**addonProductName**</li> <li>**acquisitionType**</li><li>**年**</li> <li>**storeClient**</li><li>**性别**</li> <li>**market**</li> <li>**osVersion**</li><li>**deviceType**</li><li>**paymentInstrumentType**</li><li>**sandboxId**</li><li>**xboxTitleIdHex**</li></ul> 返回的数据行会包含 *groupby* 参数中指定的字段，以及以下字段： <ul><li>**date**</li><li>**applicationId**</li><li>**addonProductId**</li><li>**acquisitionQuantity**</li></ul> groupby 参数可以与 *aggregationLevel* 参数结合使用。 例如： *&groupby = age，marketplace&aggregationLevel = week* | 否 |

### <a name="request-example"></a>请求示例
以下示例演示了用于获取加载项购置数据的多个请求。 将*addonProductId*和*applicationId*值替换为外接程序或应用程序的适当存储 ID。 

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/addonacquisitions?applicationId=9WZDNCRFJ314&startDate=1/1/2015&endDate=2/1/2015&skip=0 HTTP/1.1 

Authorization: Bearer <your access token> 

 

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/addonacquisitions?applicationId=9WZDNCRFJ314&startDate=1/1/2015&endDate=2/1/2015&skip=0&filter=market eq 'GB' and gender eq 'm' HTTP/1.1 

Authorization: Bearer <your access token>
```

## <a name="response"></a>响应

### <a name="response-body"></a>响应正文

| 值 | 类型 | 说明 |
| --- | --- | --- |
| Value | array | 包含聚合加载项购置数据的对象数组。 有关每个对象中的数据的详细信息，请参阅下面的[加载项购置值](#add-on-acquisition-values)部分。 |
| @nextLink | string | 如果存在其他数据页，则此字符串包含一个你可用来请求下一页数据的 URI。 例如，当请求的 **top** 参数设置为 10000，但查询的加载项购置数据超过 10000 行时，就会返回此值。 |
| TotalCount | int | 查询的数据结果中的行总数。 |

### <a name="add-on-acquisition-values"></a>加载项购置值
Value 数组中的元素包含以下值。

| 值 | 类型 | 说明 | 
| --- | --- | --- |
| date | string | 购置数据的日期范围内的第一个日期。 如果请求指定了某一天，此值就是该日期。 如果请求指定了一周、月或其他日期范围，此值是该日期范围内的第一个日期。 |
| addonProductId | string | 要为其检索获取数据的外接程序的*productId* 。 |
| addonProductName | string | 加载项的显示名称。 如果*aggregationLevel*参数设置为**day**，则此值仅出现在响应数据中，除非在*groupby*参数中指定**addonProductName**字段。 |
| applicationId | 字符串 | 要检索其外接程序获取数据的应用程序的*productId* 。 |
| applicationName | string | 游戏的显示名称。 |
| deviceType | string | 用于指定完成购置的设备类型的以下字符串之一： <ul><li>计算机</li><li>移动</li><li>"控制台-Xbox One"</li><li>"控制台-Xbox 系列 X"</li><li>IoT</li><li>服务</li><li>Tablet</li><li>全息</li><li>未知</li></ul> |
| storeClient | string | 用于指示发生购置的 Microsoft Store 版本的以下字符串之一： <ul><li>"Windows Phone 存储（客户端）"</li><li>"Microsoft Store （客户端）" （或 "Windows 应用商店（客户端）"）（如果在2018年3月23日之前查询数据）</li><li>"Microsoft Store （web）" （如果在2018年3月23日之前查询数据，则为 "Windows 应用商店（web）"）</li><li>"按组织批量购买"</li><li>以外</li></ul> |
| osVersion | string | 发生购置行为的操作系统版本。 对于此方法，此值始终为 "Windows 10"。 |
| market | string | 发生购置行为的市场的 ISO 3166 国家/地区代码。 |
| gender | string | 用于指定进行购置的用户的性别的以下字符串之一： <ul><li>“m”</li><li>“f”</li><li>未知</li></ul> |
| age | string | 用于指示进行购置的用户的年龄段的以下字符串之一： <ul><li>"小于 13"</li><li>"13-17"</li><li>"18-24"</li><li>"25-34"</li><li>"35-44"</li><li>"44-55"</li><li>"大于 55"</li><li>未知</li></ul> |
| acquisitionType | string | 下列字符串之一，用于指示购置类型： <ul><li>忙 </li><li>到期 </li><li>付费</li><li>"促销代码" </li><li>"Iap"</li><li>"订阅 Iap"</li><li>"专用群体"</li><li>"前置顺序"</li><li>"Xbox 游戏 Pass" （如果在2018年3月23日之前查询数据，则为 "游戏传递"）</li><li>磁盘</li><li>"预付代码"</li><li>"收费的提前订单"</li><li>"取消的前置顺序"</li><li>"失败的前置顺序"</li></ul> |
| acquisitionQuantity | integer | 发生的购置数。 |
| inAppProductId | string | 使用此外接程序的产品的产品 ID。  |
| inAppProductName | string | 使用此外接程序的产品的产品名称。  |
| paymentInstrumentType | string | 用于获取的付款方式类型。  |
| sandboxId | string | 为游戏创建的沙箱 ID。 此值可以是 "**零售**" 或 "私有沙箱 ID"。  |
| xboxTitleId | string | XDP 中产品的 Xbox 标题 ID （如果适用）。  |
| localCurrencyCode | string | 基于合作伙伴中心帐户的国家/地区的本地货币代码。  |
| xboxProductId | string | XDP 中产品的 Xbox 产品 ID （如果适用）。  |
| availabilityId | string | XDP 中产品的可用性 ID （如果适用）。  |
| skuId | string | XDP 中产品的 SKU ID （如果适用）。  |
| skuDisplayName | string | XDP 中产品的 SKU 显示名称（如果适用）。  |
| xboxParentProductId | string | XDP 中产品的 Xbox 父产品 ID （如果适用）。  |
| parentProductName | string | XDP 中产品的父产品名称（如果适用）。  |
| productTypeName | string | XDP 中产品的产品类型名称（如果适用）。  |
| purchaseTaxType | string | 从 XDP 购买产品的税务类型（如果适用）。  |
| purchasePriceUSDAmount | 数字 | 客户为外接程序支付的金额，转换为美元。  |
| purchasePriceLocalAmount | 数字 | 应用于外接程序的税额。  |
| purchaseTaxUSDAmount | 数字 | 应用于外接程序的税费，转换为美元。  |
| purchaseTaxLocalAmount | 数字 | 如果适用，请从 XDP 购买产品的增值税当地金额。  |

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