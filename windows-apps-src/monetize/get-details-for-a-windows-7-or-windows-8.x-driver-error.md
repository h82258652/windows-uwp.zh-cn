---
author: mcleanbyron
ms.assetid: 2FBA0B73-17C6-4F25-A79D-63F2F262491A
description: "在 Windows 应用商店分析 API 中使用此方法，可获取 Windows 7 或 Windows 8.x 驱动程序错误的详细数据。 此方法仅用于 IHV。"
title: "获取 Windows 7 或 Windows 8.x 驱动程序错误的详细信息"
ms.author: mcleans
ms.date: 03/17/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, 应用商店服务, Windows 应用商店分析 API, 错误, 详细信息"
ms.openlocfilehash: fbf1ee456080ff228abde061dbfef859baf79465
ms.sourcegitcommit: 64cfb79fd27b09d49df99e8c9c46792c884593a7
translationtype: HT
---
# <a name="get-details-for-a-windows-7-or-windows-8x-driver-error"></a>获取 Windows 7 或 Windows 8.x 驱动程序错误的详细信息

在 Windows 应用商店分析 API 中使用此方法，可以 JSON 格式获取特定 Windows 7/Windows 8.x 驱动程序错误的详细数据。 可以使用此方法之前，必须首先使用[获取 Windows 7 和 Windows 8.x 驱动程序的错误报告数据](get-error-reporting-data-for-windows-7-and-windows-8.x-drivers.md)方法来检索希望获取详细信息的错误的 ID。

> [!NOTE]
> 此方法仅可供属于 [Windows 硬件开发人员中心计划](https://msdn.microsoft.com/windows/hardware/drivers/dashboard/get-started-with-the-hardware-dashboard)的开发者帐户使用。

## <a name="prerequisites"></a>必备条件


若要使用此方法，首先需要执行以下操作：

* 如果尚未开始操作，请先完成 Windows 应用商店分析 API 的所有[先决条件](access-analytics-data-using-windows-store-services.md#prerequisites)。
* [获取 Azure AD 访问令牌](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)，以供在此方法的请求标头中使用。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以获取新的令牌。
* 获取希望获取详细信息的错误的 ID。 若要获取此 ID，请使用[获取 Windows 7 和 Windows 8.x 驱动程序的错误报告数据](get-error-reporting-data-for-windows-7-and-windows-8.x-drivers.md)方法，并使用该方法的响应正文中的 **failureHash** 值。

## <a name="request"></a>请求


### <a name="request-syntax"></a>请求语法

| 方法 | 请求 URI                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/ihvdriver/failuredetails``` |

<span/> 

### <a name="request-header"></a>请求头

| 标头        | 类型   | 说明                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授权 | 字符串 | 必需。 Azure AD 访问令牌的格式为 **Bearer** &lt;*token*&gt;。 |

<span/> 

### <a name="request-parameters"></a>请求参数

| 参数        | 类型   |  说明      |  必填  
|---------------|--------|---------------|------|
| failureHash | 字符串 | 你希望获取详细信息的错误的唯一 ID。 若要获取感兴趣的错误的此值，请使用[获取 OEM 硬件错误报告数据](get-oem-hardware-error-reporting-data.md)方法，并使用该方法的响应正文中的 **failureHash** 值。 |  是  |
| startDate | date | 要检索的详细错误数据日期范围中的开始日期。 默认值为当前日期之前 30 天。 |  否  |
| endDate | date | 要检索的详细错误数据日期范围中的结束日期。 默认值为当前日期。 |  否  |
| top | int | 要在请求中返回的数据行数。 如果未指定，最大值和默认值为 10000。 当查询中存在多行数据时，响应正文中包含的下一个链接可用于请求下一页数据。 |  否  |
| skip | int | 要在查询中跳过的行数。 使用此参数可以浏览较大的数据集。 例如，top=10 和 skip=0，将检索前 10 行数据；top=10 和 skip=10，将检索之后的 10 行数据，依此类推。 |  否  |
| filter | 字符串  | 在响应中筛选行的一条或多条语句。 每条语句包含的响应正文中的字段名称和值使用 **eq** 或 **ne** 运算符进行关联，并且语句可以使用 **and** 或 **or** 进行组合。 *filter* 参数中的字符串值必须使用单引号括起来。 你可以指定响应正文中的以下字段：<p/><ul><li><strong>日期型</strong></li><li><strong>failureName</strong></li><li><strong>failureHash</strong></li><li><strong>symbol</strong></li><li><strong>osVersion</strong></li><li><strong>osName</strong></li><li><strong>eventType</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li><li><strong>driverName</strong></li><li><strong>driverVersion</strong></li><li><strong>oemName</strong></li><li><strong>oemModel</strong></li><li><strong>flightRing</strong></li><li><strong>architecture</strong></li><li><strong>cabType</strong></li><li><strong>cabExpirationTime</strong></li></ul> | 否   |
| orderby | 字符串 | 对结果数据值进行排序的语句。 语法是 <em>orderby=field [order],field [order],...</em>。 你可以指定响应正文中的以下字段：<p/><ul><li><strong>failureName</strong></li><li><strong>failureHash</strong></li><li><strong>cabType</strong></li><li><strong>cabExpirationTime</strong></li><li><strong>symbol</strong></li><li><strong>osVersion</strong></li><li><strong>osName</strong></li><li><strong>eventType</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li><li><strong>driverName</strong></li><li><strong>driverVersion</strong></li><li><strong>oemName</strong></li><li><strong>oemModel</strong></li><li><strong>flightRing</strong></li><li><strong>architecture</strong></li></ul><p><em>order</em> 参数是可选的，可以是 <strong>asc</strong> 或 <strong>desc</strong>，用于指定每个字段的升序或降序排列。 默认值为 <strong>asc</strong>。</p><p>下面是一个 <em>orderby</em> 字符串：<em>orderby=date,market</em></p> |  否  |

<span/> 

### <a name="request-example"></a>请求示例

以下示例演示用于获取详细错误数据的多个请求。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/ihvdriver/failuredetails?failureHash=012e33e3-dbc9-b12f-c124-9d9810f05d8b&startDate=2016-11-05&endDate=2016-11-06&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/ihvdriver/failuredetails?failureHash=012e33e3-dbc9-b12f-c124-9d9810f05d8b&startDate=2016-11-05&endDate=2016-11-06&top=10&skip=0&filter=market eq 'US' and deviceType eq 'PC' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>回复


### <a name="response-body"></a>响应正文

| 值      | 类型    | 说明    |
|------------|---------|------------|
| 值      | array   | 包含详细错误数据的对象数组。 有关每个对象中的数据的详细信息，请参阅以下表格。          |
| @nextLink  | 字符串  | 如果存在数据的其他页，此字符串中包含的 URI 可用于请求下一页数据。 例如，当请求的 **top** 参数设置为 10，但查询的错误超过 10 行时，就会返回此值。 |
| TotalCount | inumber | 查询的数据结果中的行总数。        |

<span/>

*Value* 数组中的元素包含以下值。

| 值           | 类型    | 说明     |
|-----------------|---------|----------------------------|
| date            | 字符串  | 错误数据的日期范围内的第一个日期。 如果请求指定了某一天，此值就是该日期。 如果请求指定了一周、月或其他日期范围，此值是该日期范围内的第一个日期。 |
| sellerId   | 字符串  | 与此开发人员帐户关联的卖家 ID 值（此值与开发人员中心帐户设置中的**卖家 ID** 值匹配）。 |
| failureName     | 字符串  | 错误的名称。 它与在 Windows 开发人员中心仪表板的[运行状况报告](../publish/health-report.md)的**故障**中显示的名称相同。            |
| failureHash     | 字符串  | 错误的唯一标识符。     |
| symbol     | 字符串  | 分配给该错误的符号。     |
| osVersion       | 字符串  | 发生错误的操作系统的四部分内部版本。    |
| osName       | 字符串  | 发生错误的操作系统的名称。  |
| eventType       | 字符串  | 发生错误的类型。      |
| market          | 字符串  | 设备市场的 ISO 3166 国家/地区代码。     |
| deviceType      | 字符串  | 出现错误的设备的类型。     |
| driverName     | 字符串  | 与此错误相关联的驱动程序的唯一名称。      |
| driverVersion  | 字符串  | 与此错误相关联的驱动程序的版本。   |
| architecture | 字符串 |  发生错误的设备的架构。  |
| oemName | 字符串 | 发生错误的设备的 OEM 名称。 |
| oemModel | 字符串 | 发生错误的设备型号的名称。 |
| flightRing | 字符串 | 发生错误的操作系统外部测试版的名称。 |
| clientDeviceId | 字符串 | 出现错误的设备的 ID。 |
| cabIdHash         | 字符串  | 与此错误相关联的 CAB 文件的唯一 ID。   |
| cabType         | 字符串  | CAB 文件的类型。   |
| cabExpirationTime  | 字符串  | CAB 文件已过期且不能再下载时的日期和时间，以 ISO 8601 格式表示。   |

<span/> 

### <a name="response-example"></a>回复示例

以下示例举例说明此请求的 JSON 响应正文。

```json
{
  "Value": [
    {
      "sellerId": "14313740",
      "architecture": "x64",
      "cabExpirationTime": "2017-06-12 23:51:11",
      "cabIdHash": "cc1797f9-b783-44d4-b0e5-46ae7ca668bc",
      "cabType": "MINI",
      "clientDeviceId": null,
      "date": "2017-03-14 23:51:11",
      "deviceType": "PC",
      "driverName": "fastboot.sys",
      "driverVersion": "2.1.1.0",
      "failureHash": "f060c0b6-fbe6-463f-a9f1-b7ebc1cc722f",
      "failureName": "0x109_18_2_LoadImageNotify",
      "market": "US",
      "oemModel": "C-122",
      "oemName": "Contoso",
      "osName": "Windows 8.1",
      "osVersion": "6.3.9600.9600"
    }]
}
```

## <a name="related-topics"></a>相关主题

* [获取 Windows 7 和 Windows 8.x 驱动程序的错误报告数据](get-error-reporting-data-for-windows-7-and-windows-8.x-drivers.md)
* [下载 Windows 7 或 Windows 8.x 驱动程序错误的 CAB 文件](download-the-cab-file-for-a-windows-7-or-windows-8.x-driver-error.md)
