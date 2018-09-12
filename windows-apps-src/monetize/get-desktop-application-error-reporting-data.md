---
author: mcleanbyron
description: 使用 Microsoft Store 分析 API 中的此方法，可获取给定日期范围和其他可选筛选器的桌面应用程序聚合错误报告数据。
title: 获取桌面应用程序的错误报告数据
ms.author: mcleans
ms.date: 09/04/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, Microsoft Store 服务, Microsoft Store 分析 API, 错误, 桌面应用程序
ms.localizationpriority: medium
ms.openlocfilehash: 66594fd6f59281011fccab21ad5618827db322dd
ms.sourcegitcommit: 2a63ee6770413bc35ace09b14f56b60007be7433
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/12/2018
ms.locfileid: "3934632"
---
# <a name="get-error-reporting-data-for-your-desktop-application"></a>获取桌面应用程序的错误报告数据

在 Microsoft Store 分析 API 使用此方法，可获取已添加到 [Windows 桌面应用程序计划](https://msdn.microsoft.com/library/windows/desktop/mt826504)的桌面应用程序的聚合错误报告数据。 此方法仅可以检索过去 30 天内发生的错误。 还可以在 Windows 开发人员中心仪表板的[运行状况报告](https://msdn.microsoft.com/library/windows/desktop/mt826504)中获取桌面应用程序的这些信息。

## <a name="prerequisites"></a>先决条件

若要使用此方法，首先需要执行以下操作：

* 如果尚未开始操作，请先完成 Microsoft Store 分析 API 的所有[先决条件](access-analytics-data-using-windows-store-services.md#prerequisites)。
* [获取 Azure AD 访问令牌](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)，以供在此方法的请求标头中使用。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以获取新的令牌。

## <a name="request"></a>请求


### <a name="request-syntax"></a>请求语法

| 方法 | 请求 URI                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/failurehits``` |


### <a name="request-header"></a>请求头

| 标头        | 类型   | 说明                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授权 | 字符串 | 必需。 Azure AD 访问令牌的格式为 **Bearer** &lt;*token*&gt;。 |


### <a name="request-parameters"></a>请求参数

| 参数        | 类型   |  说明      |  必需  
|---------------|--------|---------------|------|
| applicationId | string | 要为其检索错误报告数据的桌面应用程序的产品 ID。 要获取桌面应用程序的产品 ID，请打开任意[桌面应用程序的开发人员中心分析报告](https://msdn.microsoft.com/library/windows/desktop/mt826504)（如**运行状况报告**）并从 URL 检索产品 ID。 |  是  |
| startDate | date | 要检索的错误报告数据日期范围中的开始日期，格式为 ```mm/dd/yyyy```。 默认值为当前日期。<p/><p/>**注意：**&nbsp;&nbsp;此方法仅可以检索过去 30 天内发生的错误。  |  否  |
| endDate | date | 要检索的错误报告数据日期范围中的结束日期，格式为 ```mm/dd/yyyy```。 默认值为当前日期。   |  否  |
| top | int | 要在请求中返回的数据行数。 如果未指定，最大值和默认值为 10000。 当查询中存在多行数据时，响应正文中包含的下一个链接可用于请求下一页数据。 |  否  |
| skip | int | 要在查询中跳过的行数。 使用此参数可以浏览较大的数据集。 例如，top=10000 和 skip=0，将检索前 10000 行数据；top=10000 和 skip=10000，将检索之后的 10000 行数据，依此类推。 |  否  |
| filter |字符串  | 在响应中筛选行的一条或多条语句。 每条语句包含的响应正文中的字段名称和值使用 **eq** 或 **ne** 运算符进行关联，并且语句可以使用 **and** 或 **or** 进行组合。 *filter* 参数中的字符串值必须使用单引号括起来。 你可以指定响应正文中的以下字段：<p/><ul><li><strong>fileName</strong></li><li><strong>applicationVersion</strong></li><li><strong>failureName</strong></li><li><strong>failureHash</strong></li><li><strong>symbol</strong></li><li><strong>osVersion</strong></li><li><strong>osBuild</strong></li><li><strong>osRelease</strong></li><li><strong>eventType</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li><li><strong>productName</strong></li><li><strong>date</strong></li></ul> | 否   |
| aggregationLevel | 字符串 | 指定用于检索聚合数据的时间范围。 可以是以下字符串之一：**day**、**week** 或 **month**。 如果未指定，默认值为 **day**。 如果指定 **week** 或 **month**，则 *failureName* 和 *failureHash* 值限制为 1000 个存储桶。<p/>  | 否 |
| orderby | 字符串 | 对结果数据值进行排序的语句。 语法为 *orderby=field [order],field [order],...*，其中 *field* 参数可以是以下字符串之一：<ul><li><strong>fileName</strong></li><li><strong>applicationVersion</strong></li><li><strong>failureName</strong></li><li><strong>failureHash</strong></li><li><strong>symbol</strong></li><li><strong>osVersion</strong></li><li><strong>osBuild</strong></li><li><strong>osRelease</strong></li><li><strong>eventType</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li><li><strong>productName</strong></li><li><strong>date</strong></li></ul>*order* 参数是可选的，它可以是 **asc** 或 **desc**，用于指定对每个字段进行升序还是降序排列。 默认值为 **asc**。</p><p>下面是一个 *orderby* 字符串：*orderby=date,market*</p> |  否  |
| groupby | 字符串 | 仅将数据聚合应用于指定字段的语句。 可以指定的字段如下所示：<ul><li>**failureName**</li><li>**failureHash**</li><li>**symbol**</li><li>**osVersion**</li><li>**eventType**</li><li>**market**</li><li>**deviceType**</li></ul><p>返回的数据行会包含 *groupby* 参数中指定的字段，以及以下字段：</p><ul><li>**date**</li><li>**applicationId**</li><li>**applicationName**</li><li>**eventCount**</li></ul><p>*groupby* 参数可以与 *aggregationLevel* 参数结合使用。 例如：*&amp;groupby=failureName,market&amp;aggregationLevel=week*</p></p> |  否  |


### <a name="request-example"></a>请求示例

以下示例演示用于获取错误报告数据的多个请求。 将 *applicationId* 值替换为桌面应用程序的产品 ID。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/failurehits?applicationId=10238467886765136388&startDate=1/1/2018&endDate=2/1/2018&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/failurehits?applicationId=10238467886765136388&startDate=8/1/2017&endDate=8/31/2017&skip=0&filter=market eq 'US' and deviceType eq 'PC' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>响应


### <a name="response-body"></a>响应正文

| 值      | 类型    | 说明     |
|------------|---------|--------------|
| 值      | array   | 包含聚合错误报告数据的对象数组。 有关每个对象中的数据的详细信息，请参阅以下[错误值](#error-values)部分。     |
| @nextLink  | 字符串  | 如果存在数据的其他页，此字符串中包含的 URI 可用于请求数据的下一页。 例如，当请求的 **top** 参数设置为 10000，但查询的错误超过 10000 行时，就会返回此值。 |
| TotalCount | 整数 | 查询的数据结果中的行总数。     |


### <a name="error-values"></a>错误值

*Value* 数组中的元素包含以下值。

| 值           | 类型    | 说明        |
|-----------------|---------|---------------------|
| date            | string  | 错误数据的日期范围内的第一个日期，格式为 ```yyyy-mm-dd```。 如果请求指定某一天，此值就是该日期。 如果请求指定更长的日期范围，此值则是该日期范围的第一个日期。 如果请求指定的 *aggregationLevel* 值为 **hour**，则此值还包括时间值，其格式为 ```hh:mm:ss```。  |
| applicationId   | string  | 要为其检索错误数据的桌面应用程序的产品 ID。    |
| productName | string  | 从关联可执行文件的元数据派生的桌面应用程序的显示名称。   |
| appName | string  |  待定  |
| fileName | string  | 桌面应用程序的可执行文件的名称。   |
| failureName     | string  | 故障的名称，它由四个部分组成：一个或多个问题类、异常/错误检查代码、发生故障的映像的名称和相关的函数名称。  |
| failureHash     | 字符串  | 错误的唯一标识符。   |
| symbol          | 字符串  | 分配给该错误的符号。 |
| osBuild       | string  | 发生错误的操作系统的四部分内部版本号。  |
| osVersion       | string  | 用于指定在其上安装桌面应用程序的操作系统版本的以下字符串之一：<p/><ul><li><strong>Windows 7</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows 10</strong></li><li><strong>Windows Server 2016</strong></li><li><strong>Windows Server 1709</strong></li><li><strong>Unknown</strong></li></ul>   |   
| osRelease | string  | 用于指定发生了错误的操作系统版本或外部测试 Ring（作为操作系统版本内的亚组）的以下字符串之一。<p/><p>对于 Windows 10：</p><ul><li><strong>Version 1507</strong></li><li><strong>Version 1511</strong></li><li><strong>Version 1607</strong></li><li><strong>Version 1703</strong></li><li><strong>版本 1709</strong></li><li><strong>版本 1803</strong></li><li><strong>Release Preview</strong></li><li><strong>预览体验成员 - 快</strong></li><li><strong>预览体验成员 - 慢</strong></li></ul><p/><p>对于 Windows Server 1709：</p><ul><li><strong>RTM</strong></li></ul><p>对于 Windows Server 2016：</p><ul><li><strong>Version 1607</strong></li></ul><p>对于 Windows 8.1：</p><ul><li><strong>Update 1</strong></li></ul><p>对于 Windows 7：</p><ul><li><strong>Service Pack 1</strong></li></ul><p>如果操作系统版本或外部测试 Ring 未知，则此字段的值为 <strong>Unknown</strong>。</p> |
| eventType       | string  | 下列字符串之一，用于指示错误事件类型：<ul><li>**crash**</li><li>**hang**</li><li>**memory**</li><li>**jse**</li></ul>       |
| market          | 字符串  | 设备市场的 ISO 3166 国家/地区代码。   |
| deviceType      | 字符串  | 用于指定在其上发生错误的设备类型的以下字符串之一：<p/><ul><li><strong>PC</strong></li><li><strong>Server</strong></li><li><strong>Tablet</strong></li><li><strong>Unknown</strong></li></ul>    |
| applicationVersion     | string  |   发生错误的应用程序可执行文件的版本。    |
| eventCount      | 数字 | 归因于指定聚合级别的该错误的事件数目。      |


### <a name="response-example"></a>回复示例

以下示例举例说明此请求的 JSON 响应正文。

```json
{
  "Value": [
    {
      "date": "2018-02-01",
      "applicationId": "10238467886765136388",
      "productName": "Contoso Demo",
      "appName": "Contoso Demo",
      "fileName": "contosodemo.exe",
      "failureName": "SVCHOSTGROUP_localservice_IN_PAGE_ERROR_c0000006_hardware_disk!Unknown",
      "failureHash": "11242ef3-ebd8-d525-838d-b5497b225695",
      "symbol": "hardware_disk!Unknown",
      "osBuild": "10.0.15063.850",
      "osVersion": "Windows 10",
      "osRelease": "Version 1703",
      "eventType": "crash",
      "market": "US",
      "deviceType": "PC",
      "applicationVersion": "2.2.2.0",
      "eventCount": 0.0012422360248447205
    }
  ],
  "@nextLink": "desktop/failurehits?applicationId=10238467886765136388&aggregationLevel=week&startDate=2018/02/01&endDate2018/02/08&top=1&skip=1",
  "TotalCount": 21
}

```

## <a name="related-topics"></a>相关主题

* [运行状况报告](../publish/health-report.md)
* [使用 Microsoft Store 服务访问分析数据](access-analytics-data-using-windows-store-services.md)
* [获取桌面应用程序中的错误的详细信息](get-details-for-an-error-in-your-desktop-application.md)
* [获取桌面应用程序中的错误的堆栈跟踪](get-the-stack-trace-for-an-error-in-your-desktop-application.md)
* [下载桌面应用程序中错误的 CAB 文件](download-the-cab-file-for-an-error-in-your-desktop-application.md)
