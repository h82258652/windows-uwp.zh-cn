---
description: 使用此方法在 Microsoft Store 分析 API 中获取的桌面应用程序见解数据。
title: 获取桌面应用程序的见解数据
ms.date: 07/31/2018
ms.topic: article
keywords: windows 10、 uwp、 存储区服务、 Microsoft Store 分析 API，见解
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 5545d27668b23e5b7ae91201421dfa4c92f9c8ed
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618132"
---
# <a name="get-insights-data-for-your-desktop-application"></a>获取桌面应用程序的见解数据

使用此方法在 Microsoft Store 分析 API 来获取见解数据中的与已添加到的桌面应用程序的运行状况指标[Windows 桌面应用程序](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program)。 此数据也位于[运行状况报告](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#health-report)合作伙伴中心中的桌面应用程序。

## <a name="prerequisites"></a>必备条件

若要使用此方法，首先需要执行以下操作：

* 完成 Microsoft Store 分析 API 的所有[先决条件](access-analytics-data-using-windows-store-services.md#prerequisites)（如果尚未这样做）。
* [获取 Azure AD 访问令牌](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)，以供在此方法的请求标头中使用。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以获取新的令牌。

## <a name="request"></a>请求


### <a name="request-syntax"></a>请求语法

| 方法 | 请求 URI       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/insights``` |


### <a name="request-header"></a>请求头

| 标头        | 在任务栏的搜索框中键入   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授权 | 字符串 | 必需。 Azure AD 访问令牌的格式为 **Bearer** *token*&lt;&gt;。 |


### <a name="request-parameters"></a>请求参数

| 参数        | 在任务栏的搜索框中键入   |  描述      |  必需  
|---------------|--------|---------------|------|
| applicationId | 字符串 | 你想要获取见解数据在桌面应用程序的产品 ID。 若要获取的桌面应用程序的产品 ID，请打开任意[分析报告，以便您在合作伙伴中心中的桌面应用程序](https://msdn.microsoft.com/library/windows/desktop/mt826504)(如**运行状况报告**)，并从 URL 检索产品 ID。 如果不指定此参数，响应正文将包含已注册到你的帐户的所有应用的 insights 数据。  |  否  |
| startDate | 日期 | 开始 insights 数据的日期范围内要检索的日期。 默认值为当前日期之前 30 天。 |  否  |
| endDate | 日期 | 最终的 insights 数据的日期范围内要检索的日期。 默认值为当前日期。 |  否  |
| filter | 字符串  | 在响应中筛选行的一条或多条语句。 每条语句包含的响应正文中的字段名称和值使用 **eq** 或 **ne** 运算符进行关联，并且语句可以使用 **and** 或 **or** 进行组合。 *filter* 参数中的字符串值必须使用单引号括起来。 例如，*筛选器 = 数据类型 eq 获取*。 <p/><p/>此方法目前只支持的筛选器**运行状况**。  | 否   |

### <a name="request-example"></a>请求示例

下面的示例演示了用于获取见解数据请求。 替换*applicationId*值与桌面应用程序的相应值。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/insights?applicationId=10238467886765136388&startDate=6/1/2018&endDate=6/15/2018&filter=dataType eq 'health' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>响应

### <a name="response-body"></a>响应正文

| 值      | 在任务栏的搜索框中键入   | 描述                  |
|------------|--------|-------------------------------------------------------|
| 值      | 数组  | 包含应用程序的 insights 数据的对象的数组。 有关每个对象中的数据的详细信息，请参阅[见解值](#insight-values)下面一节。                                                                                                                      |
| TotalCount | int    | 查询的数据结果中的行总数。                 |


### <a name="insight-values"></a>见解值

*Value* 数组中的元素包含以下值。

| 值               | 在任务栏的搜索框中键入   | 描述                           |
|---------------------|--------|-------------------------------------------|
| applicationId       | 字符串 | 为其检索 insights 数据的桌面应用程序的产品 ID。     |
| insightDate                | 字符串 | 我们发现特定度量值中的更改的日期。 此日期表示在其中我们检测到大量增加的一周结束，或减小相比前的一周的指标。 |
| dataType     | 字符串 | 一个字符串，指定此信息通知的常规分析区域。 目前，此方法仅支持**运行状况**。    |
| insightDetail          | 数组 | 一个或多个[InsightDetail 值](#insightdetail-values)表示以当前深入了解详细信息。    |


### <a name="insightdetail-values"></a>InsightDetail 值

| 值               | 在任务栏的搜索框中键入   | 描述                           |
|---------------------|--------|-------------------------------------------|
| FactName           | 字符串 | 一个字符串，指示当前的见解或当前维度描述的指标。 目前，此方法仅支持值**点击次数**。  |
| SubDimensions         | 数组 |  描述单个指标以深入了解的一个或多个对象。   |
| PercentChange            | 字符串 |  在您的整个客户群之间更改度量值所占百分比。  |
| DimensionName           | 字符串 |  当前维度中所述的指标的名称。 示例包括**EventType**，**市场**， **DeviceType**，以及**PackageVersion**。   |
| DimensionValue              | 字符串 | 描述当前维度中的指标值。 例如，如果**DimensionName**是**EventType**， **DimensionValue**可能**崩溃**或**挂起**.   |
| FactValue     | 字符串 | 检测到见解的日期的跃点数的绝对值。  |
| 方向 | 字符串 |  更改的方向 (**正**或**负**)。   |
| 日期              | 字符串 |  我们发现与当前的见解或当前维度相关的更改的日期。   |

### <a name="response-example"></a>响应示例

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

* [Windows 桌面应用程序](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program)
* [运行状况报告](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#health-report)
* [使用 Microsoft Store 服务的访问分析数据](access-analytics-data-using-windows-store-services.md)
