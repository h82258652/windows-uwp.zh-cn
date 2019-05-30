---
description: 使用此 REST URI 以获取要在给定的日期范围和其他可选的筛选器的桌面应用程序聚合安装数据。
title: 获取桌面应用程序安装
ms.date: 03/01/2018
ms.topic: article
keywords: windows 10，桌面应用程序安装，Windows 桌面应用程序
localizationpriority: medium
ms.openlocfilehash: 27ee2f0b6977c551d50fce9dec26c864e3858413
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372137"
---
# <a name="get-desktop-application-installs"></a>获取桌面应用程序安装


使用此 REST URI 来获取聚合安装数据以 JSON 格式的桌面应用程序已添加到[Windows 桌面应用程序](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program)。 此 URI，可在给定的日期范围和其他可选的筛选器中获取安装数据。 此信息也位于[安装报表](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program)合作伙伴中心中的桌面应用程序。

## <a name="prerequisites"></a>先决条件


若要使用此方法，首先需要执行以下操作：

* 完成 Microsoft Store 分析 API 的所有[先决条件](access-analytics-data-using-windows-store-services.md#prerequisites)（如果尚未这样做）。
* [获取 Azure AD 访问令牌](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)，以供在此方法的请求标头中使用。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以获取新的令牌。

## <a name="request"></a>请求


### <a name="request-syntax"></a>请求语法

| 方法 | 请求 URI       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/installbasedaily```|


### <a name="request-header"></a>请求头

| Header        | 在任务栏的搜索框中键入   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授权 | string | 必需。 Azure AD 访问令牌的格式为 **Bearer** *token*&lt;&gt;。 |


### <a name="request-parameters"></a>请求参数

| 参数        | 在任务栏的搜索框中键入   |  描述      |  必需  
|---------------|--------|---------------|------|
| applicationId | string | 想要检索的桌面应用程序的产品 ID 安装数据。 若要获取的桌面应用程序的产品 ID，请打开任意[分析报告，以便您在合作伙伴中心中的桌面应用程序](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program)(如**安装报表**)，并从 URL 检索产品 ID。 |  是  |
| startDate | date | 要检索的安装数据日期范围中的开始日期。 默认值为当前日期之前的 90 天。 |  否  |
| endDate | date | 要检索的安装数据日期范围中的结束日期。 默认值为当前日期。 |  否  |
| top | int | 要在请求中返回的数据行数。 如果未指定，最大值和默认值为 10000。 当查询中存在多行数据时，响应正文中包含的下一个链接可用于请求下一页数据。 |  否  |
| skip | int | 要在查询中跳过的行数。 使用此参数可以浏览较大的数据集。 例如，top=10000 和 skip=0，将检索前 10000 行数据；top=10000 和 skip=10000，将检索之后的 10000 行数据，依此类推。 |  否  |
| filter | string  | 在响应中筛选行的一条或多条语句。 每条语句包含的响应正文中的字段名称和值使用 **eq** 或 **ne** 运算符进行关联，并且语句可以使用 **and** 或 **or** 进行组合。 *filter* 参数中的字符串值必须使用单引号括起来。 你可以指定响应正文中的以下字段：<p/><ul><li><strong>applicationVersion</strong></li><li><strong>deviceType</strong></li><li><strong>market</strong></li><li><strong>osVersion</strong></li><li><strong>osRelease</strong></li></ul> | 否   |
| orderby | string | 对每个安装的结果数据值进行排序的语句。 语法是 <em>orderby=field [order],field [order],...</em>。<em>field</em> 参数可以是响应正文中的以下字段之一：<p/><ul><li><strong>productName</strong></li><li><strong>date</strong><li><strong>applicationVersion</strong></li><li><strong>deviceType</strong></li><li><strong>market</strong></li><li><strong>osVersion</strong></li><li><strong>osRelease</strong></li><li><strong>installBase</strong></li></ul><p><em>order</em> 参数是可选的，可以是 <strong>asc</strong> 或 <strong>desc</strong>，用于指定每个字段的升序或降序排列。 默认值为 <strong>asc</strong>。</p><p>下面是一个 <em>orderby</em> 字符串的示例：<em>orderby=date,market</em></p> |  否  |
| groupby | string | 仅将数据聚合应用于指定字段的语句。 你可以指定响应正文中的以下字段：<p/><ul><li><strong>applicationVersion</strong></li><li><strong>deviceType</strong></li><li><strong>market</strong></li><li><strong>osVersion</strong></li><li><strong>osRelease</strong></li></ul><p>返回的数据行会包含 <em>groupby</em> 参数中指定的字段，以及以下字段：</p><ul><li><strong>applicationId</strong></li><li><strong>date</strong></li><li><strong>productName</strong></li><li><strong>installBase</strong></li></ul></p> |  否  |


### <a name="request-example"></a>请求示例

下面的示例演示多个请求的桌面应用程序安装数据。 替换*applicationId*的桌面应用程序的产品 id 的值。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/installbasedaily?applicationId=1234567890&startDate=2018-01-01&endDate=2018-02-01&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/installbasedaily?applicationId=1234567890&startDate=2018-01-01&endDate=2018-02-01&filter=market eq 'US' and deviceType eq 'PC' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>响应


### <a name="response-body"></a>响应正文

| ReplTest1      | 在任务栏的搜索框中键入   | 描述                  |
|------------|--------|-------------------------------------------------------|
| ReplTest1      | 数组  | 包含聚合安装数据的对象数组。 有关每个对象中的数据的详细信息，请参阅下表。 |
| @nextLink  | string | 如果存在数据的其他页，此字符串中包含的 URI 可用于请求下一页数据。 例如，当请求的 **top** 参数设置为 10000，但查询的安装数据超过 10000 行时，就会返回此值。 |
| TotalCount | int    | 查询的数据结果中的行总数。 |


*Value* 数组中的元素包含以下值。

| 值               | 在任务栏的搜索框中键入   | 描述                           |
|---------------------|--------|-------------------------------------------|
| date                | string | 安装基础值与相关的日期。 |
| applicationId       | string | 您检索其在桌面应用程序的产品 ID 安装数据。 |
| productName         | string | 从关联可执行文件的元数据派生的桌面应用程序的显示名称。 |
| applicationVersion  | string | 已安装的应用程序可执行文件的版本。 |
| deviceType          | string | 指定在其安装桌面应用程序的设备类型的以下字符串之一：<p/><ul><li><strong>PC</strong></li><li><strong>Server</strong></li><li><strong>平板电脑</strong></li><li><strong>Unknown</strong></li></ul> |
| market              | string | 在其中安装桌面应用程序市场的 ISO 3166 国家/地区代码。 |
| osVersion           | string | 用于指定在其上安装桌面应用程序的操作系统版本的以下字符串之一：<p/><ul><li><strong>Windows 7</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows 10</strong></li><li><strong>Windows Server 2016</strong></li><li><strong>Windows Server 1709</strong></li><li><strong>Unknown</strong></li></ul>   |
| osRelease           | string | 用于指定安装了桌面应用程序的操作系统版本或外部测试 Ring（作为操作系统版本内的亚组）的以下字符串之一。<p/><p>对于 Windows 10：</p><ul><li><strong>版本 1507</strong></li><li><strong>版本 1511</strong></li><li><strong>版本 1607</strong></li><li><strong>版本 1703</strong></li><li><strong>版本 1709</strong></li><li><strong>发布预览</strong></li><li><strong>深入了解快速</strong></li><li><strong>深入了解速度缓慢</strong></li></ul><p/><p>对于 Windows Server 1709：</p><ul><li><strong>RTM</strong></li></ul><p>对于 Windows Server 2016：</p><ul><li><strong>版本 1607</strong></li></ul><p>对于 Windows 8.1：</p><ul><li><strong>更新 1</strong></li></ul><p>对于 Windows 7：</p><ul><li><strong>Service Pack 1</strong></li></ul><p>如果操作系统版本或外部测试 Ring 未知，则此字段的值为 <strong>Unknown</strong>。</p> |
| installBase         | 数字 | 必须安装在指定的聚合级别的产品的不同设备数。 |


### <a name="response-example"></a>响应示例

以下示例举例说明此请求的 JSON 响应正文。

```json
{
  "Value": [
    {
      "date": "2018-01-24",
      "applicationId": "123456789",
      "productName": "Contoso Demo",
      "applicationVersion": "1.0.0.0",
      "deviceType": "PC",
      "market": "All",
      "osVersion": "Windows 10",
      "osRelease": "Version 1709",
      "installBase": 348218.0
    }
  ],
  "@nextLink": "desktop/installbasedaily?applicationId=123456789&startDate=2018-01-01&endDate=2018-02-01&top=10000&skip=10000&groupby=applicationVersion,deviceType,osVersion,osRelease",
  "TotalCount": 23012
}
```

## <a name="related-topics"></a>相关主题

* [Windows 桌面应用程序](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program)
