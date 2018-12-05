---
description: 在 Microsoft Store 分析 API 中使用此方法，可获取加载项的聚合购置数据。
title: 获取 Xbox One 的加载项购置
ms.date: 10/18/2018
ms.topic: article
keywords: windows 10，uwp，应用商店服务，Microsoft Store 分析 API，Xbox One 的加载项购置
ms.localizationpriority: medium
ms.openlocfilehash: f102d2d692a2307c25dcb95e66d612fc561dec70
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2018
ms.locfileid: "8691008"
---
# <a name="get-xbox-one-add-on-acquisitions"></a>获取 Xbox One 的加载项购置

使用 Microsoft Store 分析 API 中获取聚合加载项购置数据采用 JSON 格式的 Xbox One 游戏通过 Xbox 开发人员门户 (XDP) 引入并提供在 XDP 分析合作伙伴中心仪表板中的此方法。

## <a name="prerequisites"></a>先决条件

若要使用此方法，首先需要执行以下操作：

* 如果尚未开始操作，请先完成 Microsoft Store 分析 API 的所有[先决条件](access-analytics-data-using-windows-store-services.md#prerequisites)。
* [获取 Azure AD 访问令牌](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)，以供在此方法的请求标头中使用。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以获取新的令牌。

## <a name="request"></a>请求


### <a name="request-syntax"></a>请求语法

| 方法 | 请求 URI                                                                |
|--------|----------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/addonacquisitions``` |


### <a name="request-header"></a>请求头

| 标头        | 类型   | 描述          |
|---------------|--------|--------------|
| 授权 | 字符串 | 必需。 Azure AD 访问令牌的格式为 **Bearer** &lt;*token*&gt;。 |


### <a name="request-parameters"></a>请求参数

*ApplicationId*或*addonProductId*参数是必需的。 若要检索注册到该应用的所有加载项的购置数据，请指定 *applicationId* 参数。 若要检索单个加载项的购置数据，同时指定*addonProductId*参数。 如果同时指定这两个参数，会忽略 *inAppProductId* 参数。

| 参数        | 类型   |  说明      |  必需  |
|---------------|--------|---------------|------|
| applicationId | 字符串 | 要为其检索购置数据的 Xbox One 游戏*的 productId* 。 若要获取你的游戏的*productId* ，导航到你的游戏在 XDP 分析程序中，并从 URL 检索*产品 Id* 。 或者，如果你从合作伙伴中心分析报告下载购置数据，该.tsv 文件中包含*productId* 。 |  是  |
| addonProductId | 字符串 | 你要检索购置数据的加载项*产品 Id* 。  | 是  |
| startDate | 日期 | 要检索的加载项购置数据日期范围中的开始日期。 默认值为当前日期。 |  否  |
| endDate | 日期 | 要检索的加载项购置数据日期范围中的结束日期。 默认值为当前日期。 |  否  |
| top | int | 要在请求中返回的数据行数。 如果未指定，最大值和默认值为 10000。 当查询中存在多行数据时，响应正文中包含的下一个链接可用于请求下一页数据。 |  否  |
| skip | int | 要在查询中跳过的行数。 使用此参数可以浏览较大的数据集。 例如，top=10000 和 skip=0，将检索前 10000 行数据；top=10000 和 skip=10000，将检索之后的 10000 行数据，依此类推。 |  否  |
| filter |字符串  | <p>在响应中筛选行的一条或多条语句。 每条语句包含的响应正文中的字段名称和值使用 eq 或 ne 运算符进行关联，并且语句可以使用 and 或 or 进行组合。 filter 参数中的字符串值必须使用单引号括起来。 例如，filter=market eq 'US' and gender eq 'm'。</p> <p>你可以指定响应正文中的以下字段：</p> <ul><li><strong>acquisitionType</strong></li><li><strong>age</strong></li><li><strong>storeClient</strong></li><li><strong>gender</strong></li><li><strong>market</strong></li><li><strong>osVersion</strong></li><li><strong>deviceType</strong></li><li><strong>sandboxId</strong></li></ul>| 否   |
| aggregationLevel | 字符串 | 指定用于检索聚合数据的时间范围。 可以是以下字符串之一：<strong>day</strong>、<strong>week</strong> 或 <strong>month</strong>。 如果未指定，默认值为 <strong>day</strong>。 | 否 |
| orderby | 字符串 | 对每个加载项购置的结果数据值进行排序的语句。 语法是<em>orderby = field [order]，[order]，字段...</em><em>域</em>参数可以是以下字符串之一：<ul><li><strong>date</strong></li><li><strong>acquisitionType</strong></li><li><strong>age</strong></li><li><strong>storeClient</strong></li><li><strong>gender</strong></li><li><strong>market</strong></li><li><strong>osVersion</strong></li><li><strong>deviceType</strong></li><li><strong>orderName</strong></li></ul><p><em>order</em> 参数是可选的，可以是 <strong>asc</strong> 或 <strong>desc</strong>，用于指定每个字段的升序或降序排列。 默认值为 <strong>asc</strong>。</p><p>下面是一个 <em>orderby</em> 字符串：<em>orderby=date,market</em></p> |  否  |
| groupby | 字符串 | 仅将数据聚合应用于指定字段的语句。 可以指定的字段如下所示：<ul><li><strong>日期型</strong></li><li><strong>applicationName</strong></li><li><strong>addonProductName</strong></li><li><strong>acquisitionType</strong></li><li><strong>age</strong></li><li><strong>storeClient</strong></li><li><strong>gender</strong></li><li><strong>market</strong></li><li><strong>osVersion</strong></li><li><strong>deviceType</strong></li><li><strong>paymentInstrumentType</strong></li><li><strong>sandboxId</strong></li><li><strong>xboxTitleIdHex</strong></li></ul><p>返回的数据行会包含 <em>groupby</em> 参数中指定的字段，以及以下字段：</p><ul><li><strong>date</strong></li><li><strong>applicationId</strong></li><li><strong>addonProductId</strong></li><li><strong>acquisitionQuantity</strong></li></ul><p><em>groupby</em> 参数可以与 <em>aggregationLevel</em> 参数结合使用。 例如： <em> &amp;groupby = 期限，market&amp;aggregationLevel = week</em></p> |  否  |


### <a name="request-example"></a>请求示例

以下示例演示了用于获取加载项购置数据的多个请求。 *AddonProductId*和*applicationId*值替换为你的加载项或应用的相应应用商店 ID。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/addonacquisitions?addonProductId=BRRT4NJ9B3D2&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/addonacquisitions?applicationId=BRRT4NJ9B3D1&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/addonacquisitions?addonProductId=BRRT4NJ9B3D2&startDate=1/1/2015&endDate=7/3/2015&top=100&skip=0&filter=market ne 'US' and gender ne 'Unknown' and gender ne 'm' and market ne 'NO' and age ne '>55' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>响应


### <a name="response-body"></a>响应正文

| 值      | 类型   | 描述         |
|------------|--------|------------------|
| 值      | 数组  | 包含聚合加载项购置数据的对象数组。 有关每个对象中的数据的详细信息，请参阅下面的[加载项购置值](#add-on-acquisition-values)部分。                                                                                                              |
| @nextLink  | 字符串 | 如果存在数据的其他页，此字符串中包含的 URI 可用于请求下一页数据。 例如，当请求的 **top** 参数设置为 10000，但查询的加载项购置数据超过 10000 行时，就会返回此值。 |
| TotalCount | int    | 查询的数据结果中的行总数。    |


<span id="add-on-acquisition-values" />

### <a name="add-on-acquisition-values"></a>加载项购置值

*Value* 数组中的元素包含以下值。

| 值               | 类型    | 说明        |
|---------------------|---------|---------------------|
| date                | 字符串  | 购置数据的日期范围内的第一个日期。 如果请求指定了某一天，此值就是该日期。 如果请求指定了一周、月或其他日期范围，此值是该日期范围内的第一个日期。 |
| addonProductId      | 字符串  | 要为其检索购置数据的加载项*产品 Id* 。                                                                                                                                                                 |
| addonProductName    | 字符串  | 加载项的显示名称。 此值仅显示的响应数据中如果*aggregationLevel*参数设置为**一天**，除非*groupby*参数中指定的**addonProductName**字段。                                                                                                                                                                                                            |
| applicationId       | 字符串  | 你想要检索加载项购置数据的应用*的 productId* 。                                                                                                                                                           |
| applicationName     | 字符串  | 游戏的显示名称。                                                                                                                                                                                                             |
| deviceType          | 字符串  | <p>用于指定完成购置的设备类型的以下字符串之一：</p> <ul><li>"PC"</li><li>"手机"</li><li>"控制台"</li><li>"IoT"</li><li>"服务器"</li><li>"平板"</li><li>"全息"</li><li>"未知"</li></ul>                                                                                                  |
| storeClient         | 字符串  | <p>用于指示发生购置的 Microsoft Store 版本的以下字符串之一：</p> <ul><li>"Windows Phone 应用商店 （客户端）"</li><li>"Microsoft Store (client)"(或"Windows 应用商店 （客户端）"如果查询 2018 年 3 月 23 日之前的数据)</li><li>"Microsoft Store (web)"(或"Windows 应用商店 (web)"如果查询 2018 年 3 月 23 日之前的数据)</li><li>"组织批量购买"</li><li>"其他"</li></ul>                                                                                            |
| osVersion           | 字符串  | 发生购置行为的操作系统版本。 对于此方法，此值始终是"Windows 10"。                                                                                                   |
| market              | 字符串  | 发生购置行为的市场的 ISO 3166 国家/地区代码。                                                                                                                                                                  |
| gender              | 字符串  | <p>用于指定进行购置的用户的性别的以下字符串之一：</p> <ul><li>"m"</li><li>"f"</li><li>"未知"</li></ul>                                                                                                    |
| age            | 字符串  | <p>用于指示进行购置的用户的年龄段的以下字符串之一：</p> <ul><li>"小于 13"</li><li>"13 17"</li><li>"18 至 24"</li><li>"25 34"</li><li>"35 44"</li><li>"44-55"</li><li>"大于 55"</li><li>"未知"</li></ul>                                                                                                 |
| acquisitionType     | 字符串  | <p>下列字符串之一，用于指示购置类型：</p> <ul><li>"免费"</li><li>"试用"</li><li>"付款"</li><li>"促销充值码"</li><li>"Iap"</li><li>"订阅 Iap"</li><li>"私人受众"</li><li>"预顺序"</li><li>"Xbox Game Pass"（或者"Game Pass"查询 2018 年 3 月 23 日之前的数据）</li><li>"磁盘"</li><li>"预付的码"</li><li>"收费预顺序"</li><li>"取消预顺序"</li><li>"失败预顺序"</li></ul>                                                                                                    |
| acquisitionQuantity | 整数 | 发生的购置数。                        |


### <a name="response-example"></a>响应示例

以下示例举例说明此请求的 JSON 响应正文。

```json
{
  "Value": [
    {
      "date": "2018-10-18",
      "addonProductId": " BRRT4NJ9B3D2",
      "addonProductName": "Contoso add-on 7",
      "applicationId": "BRRT4NJ9B3D1",
      "applicationName": "Contoso Demo",
      "deviceType": "Console",
      "storeClient": "Windows Phone Store (client)",
      "osVersion": "Windows 10",
      "market": "GB",
      "gender": "m",
      "age": "50orover",
      "acquisitionType": "iap",
      "acquisitionQuantity": 1
    }
  ],
  "@nextLink": "addonacquisitions?applicationId=BRRT4NJ9B3D1&addonProductId=&aggregationLevel=day&startDate=2015/01/01&endDate=2016/02/01&top=1&skip=1",
  "TotalCount": 33677
}
```
