---
author: mcleanbyron
ms.assetid: 2967C757-9D8A-4B37-8AA4-A325F7A060C5
description: "使用 Windows 应用商店分析 API 中的此方法，可获取给定日期范围和其他可选筛选器的评价数据。"
title: "获取应用评价"
ms.sourcegitcommit: 02131e641cdaa76256845b38bcc50aa42d718601
ms.openlocfilehash: bb0f912bd3380e21e04fa44f2c75244c6585f03a

---

# 获取应用评价


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

使用 Windows 应用商店分析 API 中的此方法，可获取给定日期范围和其他可选筛选器的评价数据。 此方法返回采用 JSON 格式的数据。

## 先决条件


若要使用此方法，你需要满足以下条件：

-   将需要用于调用此方法的 Azure AD 应用程序与你的开发人员中心帐户相关联。

-   针对你的应用程序获取 Azure AD 访问令牌。

有关详细信息，请参阅[使用 Windows 应用商店服务访问分析数据](access-analytics-data-using-windows-store-services.md)。

## 请求


### 请求语法

| 方法 | 请求 URI                                                      |
|--------|------------------------------------------------------------------|
| GET    | https://manage.devcenter.microsoft.com/v1.0/my/analytics/reviews |

 

### 请求标头

| 标头        | 类型   | 说明                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授权 | 字符串 | 必需。 Azure AD 访问令牌的格式为 **Bearer**&lt;*token*&gt;。 |

 

### 请求正文

<table>
<colgroup>
<col width="25%" />
<col width="25%" />
<col width="25%" />
<col width="25%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">参数</th>
<th align="left">类型</th>
<th align="left">说明</th>
<th align="left">必需</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">applicationId</td>
<td align="left">字符串</td>
<td align="left">要检索评价数据的应用的存储 ID。 存储 ID 在开发人员中心仪表板的[应用标识页](../publish/view-app-identity-details.md)上提供。 存储 ID 的一个示例是 9WZDNCRFJ3Q8。</td>
<td align="left">是</td>
</tr>
<tr class="even">
<td align="left">startDate</td>
<td align="left">date</td>
<td align="left">要检索的评价数据日期范围中的开始日期。 默认值为当前日期。</td>
<td align="left">否</td>
</tr>
<tr class="odd">
<td align="left">endDate</td>
<td align="left">date</td>
<td align="left">要检索的评价数据日期范围中的结束日期。 默认值为当前日期。</td>
<td align="left">否</td>
</tr>
<tr class="even">
<td align="left">top</td>
<td align="left">int</td>
<td align="left">要在请求中返回的数据行数。 如果未指定，最大值和默认值为 10000。 当查询中存在多行数据时，响应正文中包含的下一个链接可用于请求下一页数据。</td>
<td align="left">否</td>
</tr>
<tr class="odd">
<td align="left">skip</td>
<td align="left">int</td>
<td align="left">要在查询中跳过的行数。 使用此参数可以浏览较大的数据集。 例如，top=10000 和 skip=0，将检索前 10000 行数据；top=10000 和 skip=10000，将检索之后的 10000 行数据，依此类推。</td>
<td align="left">否</td>
</tr>
<tr class="even">
<td align="left">filter</td>
<td align="left">字符串</td>
<td align="left">在响应中筛选行的一条或多条语句。 有关详细信息，请参阅下面的[筛选器字段](#filter-fields)部分。</td>
<td align="left">否</td>
</tr>
<tr class="odd">
<td align="left">orderby</td>
<td align="left">字符串</td>
<td align="left">对每个评分的结果数据值进行排序的语句。 语法是 <em>orderby=field [order],field [order],...</em>。 <em>field</em> 参数可以是以下字符串之一。
<ul>
<li><strong>date</strong></li>
<li><strong>osVersion</strong></li>
<li><strong>market</strong></li>
<li><strong>deviceType</strong></li>
<li><strong>isRevised</strong></li>
<li><strong>packageVersion</strong></li>
<li><strong>deviceModel</strong></li>
<li><strong>productFamily</strong></li>
<li><strong>deviceScreenResolution</strong></li>
<li><strong>isTouchEnabled</strong></li>
<li><strong>reviewerName</strong></li>
<li><strong>reviewTitle</strong></li>
<li><strong>reviewText</strong></li>
<li><strong>helpfulCount</strong></li>
<li><strong>notHelpfulCount</strong></li>
<li><strong>responseDate</strong></li>
<li><strong>responseText</strong></li>
<li><strong>deviceRAM</strong></li>
<li><strong>deviceStorageCapacity</strong></li>
<li><strong>rating</strong></li>
</ul>
<p><em>order</em> 参数是可选的，可以是 <strong>asc</strong> 或 <strong>desc</strong>，用于指定每个字段的升序或降序排列。 默认值为 <strong>asc</strong>。</p>
<p>下面是一个 <em>orderby</em> 字符串的示例：<em>orderby=date,market</em></p></td>
<td align="left">否</td>
</tr>
</tbody>
</table>

 
### 筛选器字段

请求正文中的 *filter* 参数包含的一条或多条语句用于在响应中筛选行。 每条语句包含的字段和值使用 **eq** 或 **ne** 运算符进行关联，并且某些字段还支持 **contains**、**gt**、**lt**、**ge** 和 **le** 运算符。 语句可以使用 **and** 或 **or** 进行组合。

下面是一个 *filter* 字符串的示例：*filter=contains(reviewText,'great') and contains(reviewText,'ads') and deviceRAM lt 2048 and market eq 'US'*

有关支持的字段列表和每个字段支持的运算符，请参阅下表。 *filter* 参数中的字符串值必须使用单引号括起来。

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">字段</th>
<th align="left">支持的运算符</th>
<th align="left">说明</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">market</td>
<td align="left">eq, ne</td>
<td align="left">包含设备市场的 ISO 3166 国家/地区代码的字符串。</td>
</tr>
<tr class="even">
<td align="left">osVersion</td>
<td align="left">eq, ne</td>
<td align="left">以下字符串之一：
<ul>
<li><strong>Windows Phone 7.5</strong></li>
<li><strong>Windows Phone 8</strong></li>
<li><strong>Windows Phone 8.1</strong></li>
<li><strong>Windows Phone 10</strong></li>
<li><strong>Windows 8</strong></li>
<li><strong>Windows 8.1</strong></li>
<li><strong>Windows 10</strong></li>
<li><strong>Unknown</strong></li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">deviceType</td>
<td align="left">eq, ne</td>
<td align="left">以下字符串之一：
<ul>
<li><strong>PC</strong></li>
<li><strong>Tablet</strong></li>
<li><strong>Phone</strong></li>
<li><strong>IoT</strong></li>
<li><strong>Wearable</strong></li>
<li><strong>Server</strong></li>
<li><strong>Collaborative</strong></li>
<li><strong>Other</strong></li>
</ul></td>
</tr>
<tr class="even">
<td align="left">isRevised</td>
<td align="left">eq, ne</td>
<td align="left">指定 <strong>true</strong> 可筛选已修改的评价，否则指定 <strong>false</strong>。</td>
</tr>
<tr class="odd">
<td align="left">packageVersion</td>
<td align="left">eq, ne</td>
<td align="left">已评价的应用程序包版本。</td>
</tr>
<tr class="even">
<td align="left">deviceModel</td>
<td align="left">eq, ne</td>
<td align="left">应用已评价的设备的类型。</td>
</tr>
<tr class="odd">
<td align="left">productFamily</td>
<td align="left">eq, ne</td>
<td align="left">以下字符串之一：
<ul>
<li><strong>PC</strong></li>
<li><strong>Tablet</strong></li>
<li><strong>Phone</strong></li>
<li><strong>Wearable</strong></li>
<li><strong>Server</strong></li>
<li><strong>Collaborative</strong></li>
<li><strong>Other</strong></li>
</ul></td>
</tr>
<tr class="even">
<td align="left">deviceScreenResolution</td>
<td align="left">eq, ne</td>
<td align="left">设备屏幕分辨率采用&quot;<em>宽度</em> x <em>高度</em>&quot;格式。</td>
</tr>
<tr class="odd">
<td align="left">isTouchEnabled</td>
<td align="left">eq, ne</td>
<td align="left">指定 <strong>true</strong> 可筛选支持触摸的设备，否则指定 <strong>false</strong>。</td>
</tr>
<tr class="even">
<td align="left">reviewerName</td>
<td align="left">eq, ne</td>
<td align="left">评价者昵称。</td>
</tr>
<tr class="odd">
<td align="left">helpfulCount</td>
<td align="left">eq, ne</td>
<td align="left">评价标记为有用的次数。</td>
</tr>
<tr class="even">
<td align="left">notHelpfulCount</td>
<td align="left">eq, ne</td>
<td align="left">评价标记为无用的次数。</td>
</tr>
<tr class="odd">
<td align="left">reviewTitle</td>
<td align="left">eq, ne, contains</td>
<td align="left">评价的标题。</td>
</tr>
<tr class="even">
<td align="left">reviewText</td>
<td align="left">eq, ne, contains</td>
<td align="left">评价的文本内容。</td>
</tr>
<tr class="odd">
<td align="left">responseText</td>
<td align="left">eq, ne, contains</td>
<td align="left">回复的文本内容。</td>
</tr>
<tr class="even">
<td align="left">responseDate</td>
<td align="left">eq, ne</td>
<td align="left">回复的提交时间。</td>
</tr>
<tr class="odd">
<td align="left">deviceRAM</td>
<td align="left">eq, ne, gt, lt, ge, le</td>
<td align="left">物理 RAM（以 MB 为单位）。</td>
</tr>
<tr class="even">
<td align="left">deviceStorageCapacity</td>
<td align="left">eq, ne, gt, lt, ge, le</td>
<td align="left">主存储器磁盘容量（以 GB 为单位）。</td>
</tr>
<tr class="odd">
<td align="left">rating</td>
<td align="left">eq, ne, gt, lt, ge, le</td>
<td align="left">应用评分（以星级为单位）。</td>
</tr>
</tbody>
</table>

 

### 请求示例

以下示例演示用于获取评价数据的多个请求。 将 *applicationId* 值替换为你的应用的存储 ID。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/reviews?applicationId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/reviews?applicationId=9NBLGGGZ5QDR&startDate=8/1/2015&endDate=8/31/2015&skip=0&filter=contains(reviewText,'great') and contains(reviewText,'ads') and deviceRAM lt 2048 and market eq 'US' HTTP/1.1
Authorization: Bearer <your access token>
```

## 响应


### 响应正文

| 值      | 类型   | 说明                                                                                                                                                                                                                                                                            |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 值      | array  | 包含评价数据的对象数组。 有关每个对象中的数据的详细信息，请参阅以下[评价值](#review-values)部分。                                                                                                                                      |
| @nextLink  | 字符串 | 如果存在数据的其他页，此字符串中包含的 URI 可用于请求数据的下一页。 例如，当请求的 **top** 参数设置为 10000，但查询的购置数据超过 10000 行时，就会返回此值。 |
| TotalCount | int    | 查询的数据结果中的行总数。                                                                                                                                                                                                                             |

 
### 评价值

*Value* 数组中的元素包含以下值。

| 值                  | 类型    | 说明                                                                                                                                                                                                                          |
|------------------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| date                   | 字符串  | 评分数据的日期范围内的第一个日期。 如果请求指定了某一天，此值就是该日期。 如果请求指定了一周、月或其他日期范围，此值是该日期范围内的第一个日期。 |
| applicationId          | 字符串  | 要检索评分数据的应用的存储 ID。                                                                                                                                                                 |
| applicationName        | 字符串  | 应用的显示名称。                                                                                                                                                                                                         |
| market                 | 字符串  | 评分已提交的市场的 ISO 3166 国家/地区代码。                                                                                                                                                              |
| osVersion              | 字符串  | 评分已提交的操作系统版本。 有关支持的字符串列表，请参阅上述[筛选器字段](#filter-fields)部分。                                                                                               |
| deviceType             | 字符串  | 评分已提交的设备的类型。 有关支持的字符串列表，请参阅上述[筛选器字段](#filter-fields)部分。                                                                                           |
| isRevised              | 布尔值 | 值为 **true** 表示评价已修改；否则为 **false**。                                                                                                                                                       |
| packageVersion         | 字符串  | 已评价的应用程序包版本。                                                                                                                                                                                    |
| deviceModel            | 字符串  | 应用已评价的设备的类型。                                                                                                                                                                                    |
| productFamily          | 字符串  | 设备系列名称。 有关支持的字符串列表，请参阅上述[筛选器字段](#filter-fields)部分。                                                                                                                         |
| deviceScreenResolution | 字符串  | 设备屏幕分辨率采用“*宽度* x *高度*”格式。                                                                                                                                                                     |
| isTouchEnabled         | 布尔值 | 值为 **true** 表示触摸受支持；否则为 **false**。                                                                                                                                                             |
| reviewerName           | 字符串  | 评价者昵称。                                                                                                                                                                                                                   |
| helpfulCount           | 数字  | 评价标记为有用的次数。                                                                                                                                                                                   |
| notHelpfulCount        | 数字  | 评价标记为无用的次数。                                                                                                                                                                               |
| reviewTitle            | 字符串  | 评价的标题。                                                                                                                                                                                                             |
| reviewText             | 字符串  | 评价的文本内容。                                                                                                                                                                                                     |
| responseText           | 字符串  | 回复的文本内容。                                                                                                                                                                                                   |
| responseDate           | 字符串  | 回复的提交时间。                                                                                                                                                                                                   |
| deviceRAM              | 数字  | 物理 RAM（以 MB 为单位）。                                                                                                                                                                                                             |
| deviceStorageCapacity  | 数字  | 主存储器磁盘容量（以 GB 为单位）。                                                                                                                                                                                     |
| rating                 | 数字  | 应用评分（以星级为单位）。                                                                                                                                                                                                            |

 

### 响应示例

以下示例举例说明此请求的 JSON 响应正文。

```json
{
  "Value": [
    {
      "date": "2015-07-29",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso demo",
      "market": "US",
      "osVersion": "10.0.10240.16410",
      "deviceType": "PC",
      "isRevised": true,
      "packageVersion": "",
      "deviceModel": "Microsoft Corporation-Virtual Machine",
      "productFamily": "PC",
      "deviceRAM": -1,
      "deviceScreenResolution": "1024 x 768",
      "deviceStorageCapacity": 51200,
      "isTouchEnabled": false,
      "reviewerName": "ALeksandra",
      "rating": 5,
      "reviewTitle": "Love it",
      "reviewText": "Great app for demos and great dev response.",
      "helpfulCount": 0,
      "notHelpfulCount": 0,
      "responseDate": "2015-08-07T01:50:22.9874488Z",
      "responseText": "1"
    }
  ],
  "@nextLink": null,
  "TotalCount": 1
}
```

## 相关主题

* [使用 Windows 应用商店服务访问分析数据](access-analytics-data-using-windows-store-services.md)
* [获取应用购置](get-app-acquisitions.md)
* [获取 IAP 购置](get-in-app-acquisitions.md)
* [获取错误报告数据](get-error-reporting-data.md)
* [获取应用评分](get-app-ratings.md)



<!--HONumber=Jun16_HO5-->


