---
ms.assetid: C1E42E8B-B97D-4B09-9326-25E968680A0F
description: 在 Microsoft Store 分析 API 中使用此方法，获取给定日期范围和其他可选筛选器内某一 Xbox One 游戏的聚合购置数据。
title: 获取 Xbox One 游戏购置
ms.date: 10/18/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 服务, Microsoft Store 分析 API, Xbox One 游戏购置
ms.localizationpriority: medium
ms.openlocfilehash: 348430f7ceee66a9c4e82f258a70e57d8f344943
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2018
ms.locfileid: "8881654"
---
# <a name="get-xbox-one-game-acquisitions"></a>获取 Xbox One 游戏购置

使用 Microsoft Store 分析 API 中获取的聚合购置数据采用 JSON 格式的 Xbox One 游戏通过 Xbox 开发人员门户 (XDP) 引入并提供在 XDP 分析仪表板中的此方法。

## <a name="prerequisites"></a>先决条件

若要使用此方法，首先需要执行以下操作：

* 如果尚未开始操作，请先完成 Microsoft Store 分析 API 的所有[先决条件](access-analytics-data-using-windows-store-services.md#prerequisites)。
* [获取 Azure AD 访问令牌](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)，以供在此方法的请求标头中使用。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以获取新的令牌。

## <a name="request"></a>请求


### <a name="request-syntax"></a>请求语法

| 方法 | 请求 URI       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/acquisitions``` |


### <a name="request-header"></a>请求头

| 标头        | 类型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授权 | 字符串 | 必需。 Azure AD 访问令牌的格式为 **Bearer** &lt;*token*&gt;。 |


### <a name="request-parameters"></a>请求参数

| 参数        | 类型   |  说明      |  必需  
|---------------|--------|---------------|------|
| applicationId | 字符串 | 要检索其购置数据的 Xbox One 游戏的产品 ID。 若要获取你的游戏的产品 ID，请导航到你的游戏在 XDP 分析程序中，并从 URL 检索产品 ID。 或者，如果你从合作伙伴中心分析报告下载购置数据，该.tsv 文件中包含的产品 ID。  |  是  |
| startDate | date | 要检索的购置数据日期范围中的开始日期。 默认值为当前日期。 |  否  |
| endDate | date | 要检索的购置数据日期范围中的结束日期。 默认值为当前日期。 |  否  |
| top | 整数 | 要返回的数据的行数。 如果未指定，最大值和默认值为 10000。 当查询中存在多行数据时，响应正文中包含的下一个链接可用于请求下一页数据。 |  否  |
| skip | int | 要在查询中跳过的行数。 使用此参数可以浏览较大的数据集。 例如，top=10000 和 skip=0，将检索前 10000 行数据；top=10000 和 skip=10000，将检索之后的 10000 行数据，依此类推。 |  否  |
| filter | 字符串  | 在响应中筛选行的一条或多条语句。 每条语句包含的响应正文中的字段名称和值使用 **eq** 或 **ne** 运算符进行关联，并且语句可以使用 **and** 或 **or** 进行组合。 *filter* 参数中的字符串值必须使用单引号引起来。 例如，*filter=market eq 'US' and gender eq 'm'*。 <p/><p/>你可以指定响应正文中的以下字段：<p/><ul><li><strong>acquisitionType</strong></li><li><strong>age</strong></li><li><strong>storeClient</strong></li><li><strong>gender</strong></li><li><strong>market</strong></li><li><strong>osVersion</strong></li><li><strong>deviceType</strong></li><li><strong>sandboxId</strong></li></ul> | 否   |
| aggregationLevel | 字符串 | 指定用于检索聚合数据的时间范围。 可以是以下字符串之一：<strong>day</strong>、<strong>week</strong> 或 <strong>month</strong>。 如果未指定，默认值为 <strong>day</strong>。 | 否 |
| orderby | 字符串 | 对每个购置的结果数据值进行排序的语句。 语法为 <em>orderby=field [order],field [order],...</em>，其中 <em>field</em> 参数可以是以下字符串之一：<ul><li><strong>date</strong></li><li><strong>acquisitionType</strong></li><li><strong>age</strong></li><li><strong>storeClient</strong></li><li><strong>gender</strong></li><li><strong>market</strong></li><li><strong>osVersion</strong></li><li><strong>deviceType</strong></li><li><strong>paymentInstrumentType</strong></li><li><strong>sandboxId</strong></li><li><strong>xboxTitleIdHex</strong></li></ul><p><em>order</em> 参数是可选的，可以是 <strong>asc</strong> 或 <strong>desc</strong>，用于指定每个字段的升序或降序排列。 默认值为 <strong>asc</strong>。</p><p>下面是一个 <em>orderby</em> 字符串：<em>orderby=date,market</em></p> |  否  |
| groupby | 字符串 | 仅将数据聚合应用于指定字段的语句。 可以指定的字段如下所示：<ul><li><strong>日期型</strong></li><li><strong>applicationName</strong></li><li><strong>acquisitionType</strong></li><li><strong>ageGroup</strong></li><li><strong>storeClient</strong></li><li><strong>gender</strong></li><li><strong>market</strong></li><li><strong>osVersion</strong></li><li><strong>deviceType</strong></li><li><strong>paymentInstrumentType</strong></li><li><strong>sandboxId</strong></li><li><strong>xboxTitleIdHex</strong></li></ul><p>返回的数据行会包含 <em>groupby</em> 参数中指定的字段，以及以下字段：</p><ul><li><strong>date</strong></li><li><strong>applicationId</strong></li><li><strong>acquisitionQuantity</strong></li></ul><p><em>groupby</em> 参数可以与 <em>aggregationLevel</em> 参数结合使用。 例如：<em>&amp;groupby=ageGroup,market&amp;aggregationLevel=week</em></p> |  否  |


### <a name="request-example"></a>请求示例

以下示例演示用于获取 Xbox One 游戏购置数据的多个请求。 *ApplicationId*值替换为你的游戏的产品 ID。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/acquisitions?applicationId=BRRT4NJ9B3D1&startDate=1/1/2017&endDate=2/1/2017&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/acquisitions?applicationId=BRRT4NJ9B3D1&startDate=8/1/2017&endDate=8/31/2017&skip=0&filter=market eq 'US' and gender eq 'm' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>响应


### <a name="response-body"></a>响应正文

| 值      | 类型   | 描述                  |
|------------|--------|-------------------------------------------------------|
| 值      | 数组  | 包含游戏聚合购置数据的对象数组。 有关每个对象中的数据的详细信息，请参阅以下[购置值](#acquisition-values)部分。                                                                                                                      |
| @nextLink  | 字符串 | 如果存在数据的其他页，此字符串中包含的 URI 可用于请求数据的下一页。 例如，当请求的 **top** 参数设置为 10000，但查询的购置数据超过 10000 行时，就会返回此值。 |
| TotalCount | int    | 查询的数据结果中的行总数。              |


### <a name="acquisition-values"></a>购置值

*Value* 数组中的元素包含以下值。

| 值               | 类型   | 说明                           |
|---------------------|--------|-------------------------------------------|
| date                | 字符串 | 购置数据的日期范围内的第一个日期。 如果请求指定了某一天，此值就是该日期。 如果请求指定了一周、月或其他日期范围，此值是该日期范围内的第一个日期。 |
| applicationId       | 字符串 | 要检索其购置数据的 Xbox One 游戏的产品 ID。 |
| applicationName     | 字符串 | 游戏的显示名称。       |
| acquisitionType     | 字符串 | 下列字符串之一，用于指示购置类型：<ul><li><strong>Free</strong></li><li><strong>试用</strong></li><li><strong>Paid</strong></li><li><strong>Promotional code</strong></li><li><strong>Iap</strong></li><li><strong>订阅 Iap</strong></li><li><strong>私人受众</strong></li><li><strong>预顺序</strong></li><li><strong>Xbox Game Pass</strong>（或者，如果在 2018 年 3 月 23 之前查询数据，则是 <strong>Game Pass</strong>）</li><li><strong>磁盘</strong></li><li><strong>预付码</strong></li><li><strong>计费的前顺序</strong></li><li><strong>取消的预顺序</strong></li><li><strong>失败的前顺序</strong></li></ul>    |
| age                 | 字符串 | 用于指示进行购置的用户的年龄段的以下字符串之一：<ul><li><strong>less than 13</strong></li><li><strong>13-17</strong></li><li><strong>18-24</strong></li><li><strong>25-34</strong></li><li><strong>35-44</strong></li><li><strong>44-55</strong></li><li><strong>greater than 55</strong></li><li><strong>Unknown</strong></li></ul>     |
| deviceType          | 字符串 | 用于指定完成购置的设备类型的以下字符串之一：<ul><li><strong>电脑</strong></li><li><strong>电话</strong></li><li><strong>控制台</strong></li><li><strong>IoT</strong></li><li><strong>服务器</strong></li><li><strong>平板电脑</strong></li><li><strong>全息</strong></li><li><strong>未知</strong></li></ul>  |
| gender              | 字符串 | 用于指定进行购置的用户的性别的以下字符串之一：<ul><li><strong>m</strong></li><li><strong>f</strong></li><li><strong>Unknown</strong></li></ul>     |
| market              | 字符串 | 发生购置行为的市场的 ISO 3166 国家/地区代码。  |
| osVersion           | 字符串 | 发生购置行为的操作系统版本。 对于此方法，此值始终是 **Windows 10**。</li></ul>    |
| paymentInstrumentType           | 字符串 | 用于指示用于购置的付款说明的以下字符串之一：<ul><li><strong>信用卡</strong></li><li><strong>直接借记卡</strong></li><li><strong>推断的购买</strong></li><li><strong>MS 余额</strong></li><li><strong>移动运营商</strong></li><li><strong>在线银行转帐</strong></li><li><strong>PayPal</strong></li><li><strong>分割交易</strong></li><li><strong>预付码兑换</strong></li><li><strong>已支付金额为零</strong></li><li><strong>eWallet</strong></li><li><strong>未知</strong></li></ul>    |
| sandboxId              | 字符串 | 为游戏创建的沙盒的 ID。 这可以为值 **RETAIL** ，或者是私有沙盒的 ID。  |
| storeClient         | 字符串 | 用于指示发生购置的 Microsoft Store 版本的以下字符串之一：<ul><li>**Windows Phone Store (client)**</li><li>**Microsoft Store (client)**（或 **Windows Store (client)**，如果查询 2018 年 3 月 23 日之前的数据）</li><li>**Microsoft Store (web)**（或 **Windows Store (web)**，如果查询 2018 年 3 月 23 日之前的数据）</li><li>**Volume purchase by organizations**</li><li>**其他**</li></ul>                             |
| xboxTitleIdHex              | 字符串 | Xbox 开发人员门户 (XDP) 为支持 Xbox Live 的游戏指定的 Xbox Live 标题 ID（以十六进制值表示）。  |
| acquisitionQuantity | 数字 | 在指定的聚合级别期间发生的购置数。     |
| purchasePriceUSDAmount | 数字 | 客户为购置所付金额，已使用每月汇率转换为美元。    |
| taxUSDAmount     | 数字 | 购置税额，已转换为美元。 |


### <a name="response-example"></a>响应示例

以下示例举例说明此请求的 JSON 响应正文。

```json
{
  "Value": [
    {
      "date": "2017-02-01",
      "applicationId": "BRRT4NJ9B3D1 ",
      "applicationName": "Contoso Game",
      "acquisitionType": "Paid",
      "age": "35-49",
      "deviceType": "Console",
      "gender": "m",
      "market": "US",
      "osVersion": "Windows 10",
      "PaymentInstrumentType": "Credit Card ",
      "sandboxId": "RETAIL",
      "storeClient": "Windows Store (web)",
      "xboxTitleIdHex": "",
      "acquisitionQuantity": 1,
      "purchasePriceUSDAmount": 29.99,
      "taxUSDAmount": 2.99
    }
  ],
  "@nextLink": "xbox/acquisitions?applicationId=BRRT4NJ9B3D1&aggregationLevel=day&startDate=2017/02/01&endDate=2017/03/01&top=1&skip=1&orderby=date desc",
  "TotalCount": 39812
}
```

## <a name="related-topics"></a>相关主题

* [使用 Microsoft Store 服务访问分析数据](access-analytics-data-using-windows-store-services.md)
