---
description: 使用此 REST URI 以获取要在给定的日期范围和其他可选的筛选器的桌面应用程序块的详细信息数据。
title: 获取桌面应用程序的升级块详情
ms.date: 07/11/2018
ms.topic: article
keywords: windows 10 中，桌面应用程序块，Windows 桌面应用程序
localizationpriority: medium
ms.openlocfilehash: 66516c2bee58b3628542b66afcb0de24d0d9702b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57600222"
---
# <a name="get-upgrade-block-details-for-your-desktop-application"></a>获取桌面应用程序的升级块详情

此 REST URI 用于获取在桌面应用程序中的特定可执行文件在其阻止从运行在 Windows 10 升级的 Windows 10 设备的详细信息。 可以仅针对已添加到的桌面应用程序使用此 URI [Windows 桌面应用程序](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program)。 此信息也位于[应用程序会阻止报表](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#application-blocks-report)合作伙伴中心中的桌面应用程序。

此 URI 将类似于[桌面应用程序块，Get 升级](get-desktop-block-data.md)，但它在桌面应用程序中返回设备特定的可执行文件的块信息。

## <a name="prerequisites"></a>必备条件


若要使用此方法，首先需要执行以下操作：

* 完成 Microsoft Store 分析 API 的所有[先决条件](access-analytics-data-using-windows-store-services.md#prerequisites)（如果尚未这样做）。
* [获取 Azure AD 访问令牌](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)，以供在此方法的请求标头中使用。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以获取新的令牌。

## <a name="request"></a>请求


### <a name="request-syntax"></a>请求语法

| 方法 | 请求 URI       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/blockdetails```|


### <a name="request-header"></a>请求头

| 标头        | 在任务栏的搜索框中键入   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授权 | 字符串 | 必需。 Azure AD 访问令牌的格式为 **Bearer** *token*&lt;&gt;。 |


### <a name="request-parameters"></a>请求参数

| 参数        | 在任务栏的搜索框中键入   |  描述      |  必需  
|---------------|--------|---------------|------|
| applicationId | 字符串 | 你想要检索的数据块的桌面应用程序的产品 ID。 若要获取的桌面应用程序的产品 ID，请打开任意[分析报告，以便您在合作伙伴中心中的桌面应用程序](https://msdn.microsoft.com/library/windows/desktop/mt826504)(如**阻止报表**)，并从 URL 检索产品 ID。 |  是  |
| fileName | 字符串 | 已阻止可执行文件的名称 |
| startDate | 日期 | 开始块数据的日期范围内要检索的日期。 默认值为当前日期之前的 90 天。 |  否  |
| endDate | 日期 | 结束块数据的日期范围内要检索的日期。 默认值为当前日期。 |  否  |
| top | int | 要在请求中返回的数据行数。 如果未指定，最大值和默认值为 10000。 当查询中存在多行数据时，响应正文中包含的下一个链接可用于请求下一页数据。 |  否  |
| skip | int | 要在查询中跳过的行数。 使用此参数可以浏览较大的数据集。 例如，top=10000 和 skip=0，将检索前 10000 行数据；top=10000 和 skip=10000，将检索之后的 10000 行数据，依此类推。 |  否  |
| filter | 字符串  | 在响应中筛选行的一条或多条语句。 每条语句包含的响应正文中的字段名称和值使用 **eq** 或 **ne** 运算符进行关联，并且语句可以使用 **and** 或 **or** 进行组合。 *filter* 参数中的字符串值必须使用单引号括起来。 你可以指定响应正文中的以下字段：<p/><ul><li><strong>applicationVersion</strong></li><li><strong>architecture</strong></li><li><strong>blockType</strong></li><li><strong>deviceType</strong></li><li><strong>market</strong></li><li><strong>osRelease</strong></li><li><strong>osVersion</strong></li><li><strong>productName</strong></li><li><strong>targetOs</strong></li></ul> | 否   |
| orderby | 字符串 | 一个语句，为每个块的数据值对结果进行排序。 语法是 <em>orderby=field [order],field [order],...</em>。<em>field</em> 参数可以是响应正文中的以下字段之一：<p/><ul><li><strong>applicationVersion</strong></li><li><strong>architecture</strong></li><li><strong>blockType</strong></li><li><strong>date</strong><li><strong>deviceType</strong></li><li><strong>market</strong></li><li><strong>osRelease</strong></li><li><strong>osVersion</strong></li><li><strong>productName</strong></li><li><strong>targetOs</strong></li><li><strong>deviceCount</strong></li></ul><p><em>order</em> 参数是可选的，可以是 <strong>asc</strong> 或 <strong>desc</strong>，用于指定每个字段的升序或降序排列。 默认值为 <strong>asc</strong>。</p><p>下面是一个 <em>orderby</em> 字符串的示例：<em>orderby=date,market</em></p> |  否  |
| groupby | 字符串 | 仅将数据聚合应用于指定字段的语句。 你可以指定响应正文中的以下字段：<p/><ul><li><strong>applicationVersion</strong></li><li><strong>architecture</strong></li><li><strong>blockType</strong></li><li><strong>deviceType</strong></li><li><strong>market</strong></li><li><strong>osRelease</strong></li><li><strong>osVersion</strong></li><li><strong>targetOs</strong></li></ul><p>返回的数据行会包含 <em>groupby</em> 参数中指定的字段，以及以下字段：</p><ul><li><strong>applicationId</strong></li><li><strong>date</strong></li><li><strong>productName</strong></li><li><strong>deviceCount</strong></li></ul></p> |  否  |


### <a name="request-example"></a>请求示例

下面的示例演示用于获取桌面应用程序块数据的多个请求。 替换*applicationId*的桌面应用程序的产品 id 的值。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/blockdetails?applicationId=10238467886765136388&fileName=contoso.exe&startDate=2018-05-01&endDate=2018-06-07&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/blockdetails?applicationId=10238467886765136388&fileName=contoso.exe&startDate=2018-05-01&endDate=2018-06-07&filter=market eq 'US' and deviceType eq 'PC' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>响应


### <a name="response-body"></a>响应正文

| 值      | 在任务栏的搜索框中键入   | 描述                  |
|------------|--------|-------------------------------------------------------|
| 值      | 数组  | 包含聚合块数据的对象的数组。 有关每个对象中的数据的详细信息，请参阅下表。 |
| @nextLink  | 字符串 | 如果存在数据的其他页，此字符串中包含的 URI 可用于请求下一页数据。 例如，如果返回此值**顶部**请求的参数设置为 10000，但有超过 10000 行的查询的块数据。 |
| TotalCount | int    | 查询的数据结果中的行总数。 |


*Value* 数组中的元素包含以下值。

| 值               | 在任务栏的搜索框中键入   | 描述                           |
|---------------------|--------|-------------------------------------------|
| applicationId       | 字符串 | 为其检索块数据的桌面应用程序的产品 ID。 |
| 日期                | 字符串 | 使用块命中值相关联的日期。 |
| productName         | 字符串 | 从关联可执行文件的元数据派生的桌面应用程序的显示名称。 |
| fileName            | 字符串 | 已阻止可执行文件。 |
| applicationVersion  | 字符串 | 已被阻止的应用程序可执行文件的版本。 |
| osVersion           | 字符串 | 指定在其的桌面应用程序当前正在运行的 OS 版本的以下字符串之一：<p/><ul><li><strong>Windows 7</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows 10</strong></li><li><strong>Windows Server 2016</strong></li><li><strong>Windows Server 1709</strong></li><li><strong>Unknown</strong></li></ul>   |
| osRelease           | 字符串 | 在当前运行的桌面应用程序 （作为 OS 版本中 subpopulation) 指定 OS 版本或试验环的以下字符串之一。<p/><p>对于 Windows 10：</p><ul><li><strong>版本 1507</strong></li><li><strong>版本 1511</strong></li><li><strong>版本 1607</strong></li><li><strong>版本 1703</strong></li><li><strong>版本 1709</strong></li><li><strong>发布预览</strong></li><li><strong>深入了解快速</strong></li><li><strong>深入了解速度缓慢</strong></li></ul><p/><p>对于 Windows Server 1709：</p><ul><li><strong>RTM</strong></li></ul><p>对于 Windows Server 2016：</p><ul><li><strong>版本 1607</strong></li></ul><p>对于 Windows 8.1：</p><ul><li><strong>更新 1</strong></li></ul><p>对于 Windows 7：</p><ul><li><strong>Service Pack 1</strong></li></ul><p>如果操作系统版本或外部测试 Ring 未知，则此字段的值为 <strong>Unknown</strong>。</p> |
| market              | 字符串 | 在桌面应用程序则会阻止对市场的 ISO 3166 国家/地区代码。 |
| deviceType          | 字符串 | 指定在其的桌面应用程序被阻止的设备类型的以下字符串之一：<p/><ul><li><strong>PC</strong></li><li><strong>Server</strong></li><li><strong>平板电脑</strong></li><li><strong>Unknown</strong></li></ul> |
| blockType            | 字符串 | 指定在设备上找到的块的类型的以下字符串之一：<p/><ul><li><strong>潜在 Sediment</strong></li><li><strong>临时 Sediment</strong></li><li><strong>运行时通知</strong></li></ul><p/><p/>有关这些块类型及其含义的开发人员和用户的详细信息，请参阅的说明[应用程序会阻止报表](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#application-blocks-report)。 |
| architecture        | 字符串 | 存在块设备的体系结构： <p/><ul><li><strong>ARM64</strong></li><li><strong>X86</strong></li></ul> |
| targetOs            | 字符串 | 指定在桌面应用程序在其阻止运行的 Windows 10 OS 版本的以下字符串之一： <p/><ul><li><strong>版本 1709</strong></li><li><strong>1803 的版本</strong></li></ul> |
| deviceCount         | 数字 | 在指定的聚合级别具有块的不同设备数。 |


### <a name="response-example"></a>响应示例

以下示例举例说明此请求的 JSON 响应正文。

```json
{
  "Value": [
    {
     "applicationId": "10238467886765136388",
     "date": "2018-06-03",
     "productName": "Contoso Demo",
     "fileName": "contosodemo.exe",
     "applicationVersion": "2.2.2.0",
     "osVersion": "Windows 8.1",
     "osRelease": "Update 1",
     "market": "ZA",
     "deviceType": "All",
     "blockType": "Runtime Notification",
     "architecture": "X86",
     "targetOs": "RS4",
     "deviceCount": 120
    }
  ],
  "@nextLink": "desktop/blockdetails?applicationId=123456789&startDate=2018-01-01&endDate=2018-02-01&top=10000&skip=10000&groupby=applicationVersion,deviceType,osVersion,osRelease",
  "TotalCount": 23012
}
```

## <a name="related-topics"></a>相关主题

* [Windows 桌面应用程序](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program)
* [获取升级块的桌面应用程序](get-desktop-block-data.md)
