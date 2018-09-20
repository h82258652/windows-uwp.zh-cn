---
author: mcleanbyron
description: 在 Microsoft Store 分析 API 中使用此方法获取你的应用的见解数据。
title: 获取的见解数据
ms.author: mcleans
ms.date: 07/31/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10，uwp，应用商店服务，Microsoft Store 分析 API，见解
ms.localizationpriority: medium
ms.openlocfilehash: 53fbd91437e5dc702f8672c6cbadeea32a8a96bf
ms.sourcegitcommit: 4f6dc806229a8226894c55ceb6d6eab391ec8ab6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/20/2018
ms.locfileid: "4087036"
---
# <a name="get-insights-data"></a>获取的见解数据

若要获取的见解数据在 Microsoft Store 分析 API 中的此方法与购置、 运行状况和应用的使用情况指标给定的日期范围和其他可选筛选器的使用。 此信息也是在 Windows 开发人员中心仪表板中的[洞察报告](../publish/insights-report.md)中可用。

## <a name="prerequisites"></a>必备条件


若要使用此方法，首先需要执行以下操作：

* 如果尚未开始操作，请先完成 Microsoft Store 分析 API 的所有[先决条件](access-analytics-data-using-windows-store-services.md#prerequisites)。
* [获取 Azure AD 访问令牌](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)，以供在此方法的请求标头中使用。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以获取新的令牌。

## <a name="request"></a>请求


### <a name="request-syntax"></a>请求语法

| 方法 | 请求 URI       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/insights``` |


### <a name="request-header"></a>请求头

| 标头        | 类型   | 说明                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授权 | 字符串 | 必需。 Azure AD 访问令牌的格式为 **Bearer** &lt;*token*&gt;。 |


### <a name="request-parameters"></a>请求参数

| 参数        | 类型   |  说明      |  必需  
|---------------|--------|---------------|------|
| applicationId | 字符串 | 你想要检索的见解数据的应用[商店 ID](in-app-purchases-and-trials.md#store-ids) 。 如果不指定此参数，响应正文将包含注册到帐户的所有应用的见解数据。  |  否  |
| startDate | date | 开始菜单的见解数据日期范围中要检索的日期。 默认值为当前日期之前 30 天。 |  否  |
| endDate | date | 中的结束日期的见解数据日期范围以检索。 默认值为当前日期。 |  否  |
| filter | 字符串  | 在响应中筛选行的一条或多条语句。 每条语句包含的响应正文中的字段名称和值使用 **eq** 或 **ne** 运算符进行关联，并且语句可以使用 **and** 或 **or** 进行组合。 *filter* 参数中的字符串值必须使用单引号括起来。 例如， *filter = dataType eq 购置*。 <p/><p/>你可以指定以下筛选字段：<p/><ul><li><strong>购置</strong></li><li><strong>运行状况</strong></li><li><strong>使用情况</strong></li></ul> | 否   |

### <a name="request-example"></a>请求示例

下面的示例演示了一个请求用于获取的见解数据。 将 *applicationId* 值替换为你的应用的 Store ID。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/insights?applicationId=9NBLGGGZ5QDR&startDate=6/1/2018&endDate=6/15/2018&filter=dataType eq 'acquisition' or dataType eq 'health' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>响应

### <a name="response-body"></a>响应正文

| 值      | 类型   | 说明                  |
|------------|--------|-------------------------------------------------------|
| 值      | array  | 包含应用的见解数据的对象数组。 有关每个对象中的数据的详细信息，请参阅下面的[相关的见解值](#insight-values)部分。                                                                                                                      |
| TotalCount | int    | 查询的数据结果中的行总数。                 |


### <a name="insight-values"></a>相关的见解值

*Value* 数组中的元素包含以下值。

| 值               | 类型   | 描述                           |
|---------------------|--------|-------------------------------------------|
| applicationId       | 字符串 | 要为其检索的见解数据的应用商店 ID。     |
| insightDate                | 字符串 | 我们标识特定指标的更改的日期。 此日期表示的末尾一周中我们检测到了显著增加或减少相对于前的一周的指标。 |
| 数据类型     | 字符串 | 指定此 insight 描述的常规分析区域的以下字符串之一：<p/><ul><li><strong>购置</strong></li><li><strong>运行状况</strong></li><li><strong>使用情况</strong></li></ul>   |
| insightDetail          | array | 一个或多个[InsightDetail 值](#insightdetail-values)表示当前见解的详细信息。    |


### <a name="insightdetail-values"></a>InsightDetail 值

| 值               | 类型   | 说明                           |
|---------------------|--------|-------------------------------------------|
| FactName           | 字符串 | 以下值之一，用于指示规格，它的当前相关的见解和当前维度描述，具体取决于**数据类型**值。<ul><li>对于**运行状况**，此值始终是**点击次数**。</li><li>**购置**，此值始终是**AcquisitionQuantity**。</li><li>对于**使用情况**，此值可以是以下字符串之一：<ul><li><strong>DailyActiveUsers</strong></li><li><strong>EngagementDurationMinutes</strong></li><li><strong>DailyActiveDevices</strong></li><li><strong>DailyNewUsers</strong></li><li><strong>DailySessionCount</strong></li></ul></ul>  |
| SubDimensions         | array |  介绍相关的见解的单个跃点数的一个或多个对象。   |
| PercentChange            | 字符串 |  指标跨整个客户群的销售量更改百分比。  |
| 具有           | 字符串 |  度量当前维度中所述的名称。 示例包括**事件类型**、**市场**、 **DeviceType**、 **PackageVersion**、 **AcquisitionType**、 **AgeGroup**和**性别**。   |
| DimensionValue              | 字符串 | 的度量值所述的当前尺寸。 例如，如果**具有****事件类型**， **DimensionValue**可能**崩溃**或**挂起**。   |
| FactValue     | 字符串 | 绝对的度量值 insight 检测到的日期。  |
| Direction | 字符串 |  更改 （**正**或**负**） 的方向。   |
| 日期              | 字符串 |  我们确定与当前的见解或当前维度相关的更改的日期。   |

### <a name="response-example"></a>回复示例

以下示例举例说明此请求的 JSON 响应正文。

```json
{
  "Value": [
    {
      "applicationId": "9NBLGGGZ5QDR",
      "insightDate": "2018-06-03T00:00:00",
      "dataType": "health",
      "insightDetail": [
        {
          "FactName": "HitCount",
          "SubDimensions": [
            {
              "FactName:": "HitCount",
              "PercentChange": "21",
              "DimensionValue:": "DE",
              "FactValue": "109",
              "Direction": "Positive",
              "Date": "6/3/2018 12:00:00 AM",
              "DimensionName": "Market"
            }
          ],
          "DimensionValue": "crash",
          "Date": "6/3/2018 12:00:00 AM",
          "DimensionName": "EventType"
        },
        {
          "FactName": "HitCount",
          "SubDimensions": [
            {
              "FactName:": "HitCount",
              "PercentChange": "71",
              "DimensionValue:": "JP",
              "FactValue": "112",
              "Direction": "Positive",
              "Date": "6/3/2018 12:00:00 AM",
              "DimensionName": "Market"
            }
          ],
          "DimensionValue": "hang",
          "Date": "6/3/2018 12:00:00 AM",
          "DimensionName": "EventType"
        },
      ],
      "insightId": "9CY0F3VBT1AS942AFQaeyO0k2zUKfyOhrOHc0036Iwc="
    }
  ],
  "@nextLink": null,
  "TotalCount": 2
}
```

## <a name="related-topics"></a>相关主题

* [洞察报告](../publish/insights-report.md)
* [使用 Microsoft Store 服务访问分析数据](access-analytics-data-using-windows-store-services.md)
