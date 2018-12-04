---
description: 在 Microsoft Store 分析 API 中使用此方法获取 Xbox Live 分析数据。
title: 获取 Xbox Live 分析数据
ms.date: 06/04/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 服务, Microsoft Store 分析 API, Xbox Live 分析
ms.localizationpriority: medium
ms.openlocfilehash: 74c898630641e8b0d53a181d1874c6df62baaa78
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/03/2018
ms.locfileid: "8485479"
---
# <a name="get-xbox-live-analytics-data"></a>获取 Xbox Live 分析数据

在 Microsoft Store 分析 API 中使用此方法来获取玩你的[支持 Xbox Live 的游戏](../xbox-live/index.md)的客户的前 30 天常规分析数据，包括设备附件使用情况、Internet 连接类型、玩家分数分布、游戏统计数据，以及好友和关注者数据。 此信息也是在合作伙伴中心中的[Xbox 分析报告](../publish/xbox-analytics-report.md)中可用。

> [!IMPORTANT]
> 该方法只支持 Xbox 游戏或使用 Xbox Live 服务的游戏。 这些游戏必须经过[概念审批流程](../gaming/concept-approval.md)，其中包括 [Microsoft 合作伙伴](../xbox-live/developer-program-overview.md#microsoft-partners)发布的游戏以及通过 [ID@Xbox 计划](../xbox-live/developer-program-overview.md#id)提交的游戏。 该方法当前不支持通过 [Xbox Live 创意者计划](../xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md)发布的游戏。

针对支持 Xbox Live 的游戏的额外分析数据通过以下方法提供：
* [获取 Xbox Live 成就数据](get-xbox-live-achievements-data.md)
* [获取 Xbox Live 运行状况数据](get-xbox-live-health-data.md)
* [获取 Xbox Live 游戏中心数据](get-xbox-live-game-hub-data.md)
* [获取 Xbox Live 中心数据](get-xbox-live-club-data.md)
* [获取 Xbox Live 多人游戏数据](get-xbox-live-multiplayer-data.md)
* [获取 Xbox Live 并发使用情况数据](get-xbox-live-concurrent-usage-data.md)

## <a name="prerequisites"></a>先决条件

若要使用此方法，首先需要执行以下操作：

* 如果尚未开始操作，请先完成 Microsoft Store 分析 API 的所有[先决条件](access-analytics-data-using-windows-store-services.md#prerequisites)。
* [获取 Azure AD 访问令牌](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)，以供在此方法的请求标头中使用。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以获取新的令牌。

## <a name="request"></a>请求


### <a name="request-syntax"></a>请求语法

| 方法 | 请求 URI       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics``` |


### <a name="request-header"></a>请求头

| 标头        | 类型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授权 | 字符串 | 必需。 Azure AD 访问令牌的格式为 **Bearer** &lt;*token*&gt;。 |


### <a name="request-parameters"></a>请求参数

| 参数        | 类型   |  说明      |  必需  
|---------------|--------|---------------|------|
| applicationId | 字符串 | 你要检索常规 Xbox Live 数据的游戏的 [Store ID](in-app-purchases-and-trials.md#store-ids)。  |  是  |
| metricType | 字符串 | 指定要检索的 Xbox Live 分析数据的类型的字符串。 对于此方法，指定值 **productvalues**。  |  是  |


### <a name="request-example"></a>请求示例

以下示例演示了一个请求，该请求用于获取玩你的支持 Xbox Live 的游戏的客户的常规分析数据。 将 *applicationId* 值替换为你的游戏的 Store ID。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=productvalues HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>响应

此方法返回一个包含以下对象的 *Value* 数组。

| 对象      | 说明                  |
|-------------|---------------------------------------------------|
| ProductData   |   包含一个 [DeviceProperties](#deviceproperties) 对象以及一个 [UserProperties](#userproperties) 对象，它们包含过去 30 天内您的游戏的设备和用户分析数据。    |  
| XboxwideData   |  包含一个 [DeviceProperties](#deviceproperties) 对象以及一个 [UserProperties](#userproperties) 对象，它们包含过去 30 天内所有 Xbox Live 客户的平均设备和用户分析数据（百分比形式）。 包含此数据是为了对您的游戏数据进行比较。   |                                           


### <a name="deviceproperties"></a>DeviceProperties

此资源包含过去 30 天内您的游戏的设备使用情况数据或所有 Xbox Live 客户的平均设备使用情况数据。

| 值           | 类型    | 描述        |
|-----------------|---------|------|
|  applicationId               |    字符串     |  检索其分析数据的游戏的 [Store ID](in-app-purchases-and-trials.md#store-ids)。   |
|  connectionTypeDistribution               |    数组     |   包含指示 Xbox 有多少客户使用有线 Internet 连接，多少客户使用无线 Internet 连接的对象。 每个对象都有两个字符串字段： <ul><li>**conType**：指定连接类型。</li><li>**deviceCount**：在 **ProductData** 对象中，此字段指定使用该连接类型的游戏客户的数量。 在 **XboxwideData** 对象中，此字段指定使用该连接类型的所有 Xbox Live 客户的百分比。</li></ul>   |     
|  deviceCount               |   字符串      |  在 **ProductData** 对象中，此字段指定过去 30 天内用于玩你的游戏的客户设备的数量。 在 **XboxwideData** 对象中，此字段将为始终为 1，指示所有 Xbox Live 客户的数据的起始百分比为 100%。   |     
|  eliteControllerPresentDeviceCount               |   字符串      |  在 **ProductData** 对象中，此字段指定使用 Xbox Elite 无线控制器的游戏客户的数量。 在 **XboxwideData** 对象中，此字段指定使用 Xbox Elite 无线控制器的所有 Xbox Live 客户的百分比。  |     
|  externalDrivePresentDeviceCount               |   字符串      |  在 **ProductData** 对象中，此字段指定在 Xbox 上使用外部硬盘驱动器的游戏客户的数量。 在 **XboxwideData** 对象中，此字段指定在 Xbox 上使用外部硬盘驱动器的所有 Xbox Live 客户的百分比。  |


### <a name="userproperties"></a>UserProperties

此资源包含过去 30 天内您的游戏的用户数据或所有 Xbox Live 客户的平均用户数据。

| 值           | 类型    | 描述        |
|-----------------|---------|------|
|  applicationId               |    字符串     |   检索其分析数据的游戏的 [Store ID](in-app-purchases-and-trials.md#store-ids)。  |
|  UserCount               |    字符串     |   在 **ProductData** 对象中，此字段指定过去 30 天内玩你的游戏的客户的数量。 在 **XboxwideData** 对象中，此字段将为始终为 1，指示所有 Xbox Live 客户的数据的起始百分比为 100%。   |     
|  dvrUsageCounts               |   数组      |  包含表明多少客户使用了游戏 DVR 来录制和查看游戏的对象。 每个对象都有两个字符串字段： <ul><li>**dvrName**：指定所使用的游戏 DVR 功能。 可能的值为 **gameClipUploads**、**gameClipViews**、**screenshotUploads** 和 **screenshotViews**。</li><li>**userCount**：在 **ProductData** 对象中，此字段指定使用指定游戏 DVR 功能的游戏客户的数量。 在 **XboxwideData** 对象中，此字段指定使用指定游戏 DVR 功能的所有 Xbox Live 客户的百分比。</li></ul>   |     
|  followerCountPercentiles               |   array      |  包含提供有关客户关注者数的详细信息的对象。 每个对象都有两个字符串字段： <ul><li>**percentage**：目前，此值始终是 50，指示关注者数据以中值形式提供。</li><li>**value**：在 **ProductData** 对象中，此字段指定您的游戏客户的关注者的中值数量。 在 **XboxwideData** 对象中，此字段指定所有 Xbox Live 客户的关注者的中值数量。</li></ul>  |   
|  friendCountPercentiles               |   array      |  包含提供有关客户好友数的详细信息。 每个对象都有两个字符串字段： <ul><li>**percentage**：目前，此值始终是 50，指示好友数据以中值形式提供。</li><li>**value**：在 **ProductData** 对象中，此字段指定您的游戏客户的好友的中值数量。 在 **XboxwideData** 对象中，此字段指定所有 Xbox Live 客户的好友的中值数量。</li></ul>  |     
|  gamerScoreRangeDistribution               |   数组      |  包含提供有关客户玩家分数分布情况的详细信息。 每个对象都有两个字符串字段： <ul><li>**scoreRange**：以下字段提供其使用情况数据的玩家分数范围。 例如 **10K-25K**。</li><li>**userCount**：在 **ProductData** 对象中，此字段指定你的游戏客户在其玩过的所有游戏中其玩家分数属于指定范围的客户的数量。 在 **XboxwideData** 对象中，此字段指定所有 Xbox Live 客户在其玩过的所有游戏中其玩家分数属于指定范围的客户的百分比。</li></ul>  |
|  titleGamerScoreRangeDistribution               |   array      |  包含提供有关游戏玩家分数分布情况的详细信息的对象。 每个对象都有两个字符串字段： <ul><li>**scoreRange**：以下字段提供其使用情况数据的玩家分数范围。 例如 **100-200**。</li><li>**userCount**：在 **ProductData** 对象中，此字段指定你的游戏客户在你的游戏中的玩家分数属于指定范围的客户的数量。 在 **XboxwideData** 对象中，此字段指定所有 Xbox Live 客户在你的游戏中其玩家分数属于指定范围的客户的百分比。</li></ul>   |
|  socialUsageCounts               |   array      |  包含提供有关客户社交使用情况的详细信息的对象。 每个对象都有两个字符串字段： <ul><li>**scName**：社交使用情况类型。 例如 **gameInvites**和**textMessages**。</li><li>**userCount**：在 **ProductData** 对象中，此字段指定在你的游戏中参与指定社交使用情况类型的客户的数量。 在 **XboxwideData** 对象中，此字段指定所有参与指定社交使用情况类型的 Xbox Live 客户的百分比。</li></ul>   |
|  streamingUsageCounts               |   数组      |  包含提供有关客户流式处理使用情况的详细信息。 每个对象都有两个字符串字段： <ul><li>**stName**：流式处理平台的类型。 例如，**youtubeUsage**、**twitchUsage** 和 **mixerUsage**。</li><li>**userCount**：在 **ProductData** 对象中，此字段指定你的游戏中使用指定流式处理平台的客户的数量。 在 **XboxwideData** 对象中，此字段指定使用指定流式处理平台的所有 Xbox Live 客户的百分比。</li></ul>  |


### <a name="response-example"></a>响应示例

以下示例举例说明此请求的 JSON 响应正文。

```json
{
  "Value": [
    {
      "ProductData": {
        "DeviceProperties": [
          {
            "applicationId": "9NBLGGGZ5QDR",
            "connectionTypeDistribution": [
              {
                "conType": "WIRED",
                "deviceCount": "43806"
              },
              {
                "conType": "WIRELESS",
                "deviceCount": "104035"
              }
            ],
            "deviceCount": "148063",
            "eliteControllerPresentDeviceCount": "10615",
            "externalDrivePresentDeviceCount": "46388"
          }
        ],
        "UserProperties": [
          {
            "applicationId": "9NBLGGGZ5QDR",
            "userCount": "142345",
            "dvrUsageCounts": [
              {
                "dvrName": "gameClipUploads",
                "userCount": "31264"
              },
              {
                "dvrName": "gameClipViews",
                "userCount": "52236"
              },
              {
                "dvrName": "screenshotUploads",
                "userCount": "27051"
              },
              {
                "dvrName": "screenshotViews",
                "userCount": "45640"
              }
            ],
            "followerCountPercentiles": [
              {
                "percentage": "50",
                "value": "11"
              }
            ],
            "friendCountPercentiles": [
              {
                "percentage": "50",
                "value": "11"
              }
            ],
            "gamerScoreRangeDistribution": [
              {
                "scoreRange": "10K-25K",
                "userCount": "30015"
              },
              {
                "scoreRange": "25K-50K",
                "userCount": "20495"
              },
              {
                "scoreRange": "3K-10K",
                "userCount": "32438"
              },
              {
                "scoreRange": "50K-100K",
                "userCount": "10608"
              },
              {
                "scoreRange": "<3K",
                "userCount": "45726"
              },
              {
                "scoreRange": ">100K",
                "userCount": "3063"
              }
            ],
            "titleGamerScoreRangeDistribution": [
              {
                "scoreRange": "400-600",
                "userCount": "133875"
              },
              {
                "scoreRange": "800-1000",
                "userCount": "45960"
              },
              {
                "scoreRange": "<100",
                "userCount": "269137"
              },
              {
                "scoreRange": "≥1K",
                "userCount": "11634"
              },
              {
                "scoreRange": "100-200",
                "userCount": "334471"
              },
              {
                "scoreRange": "600-800",
                "userCount": "123044"
              },
              {
                "scoreRange": "200-400",
                "userCount": "396725"
              }
            ],
            "socialUsageCounts": [
              {
                "scName": "gameInvites",
                "userCount": "82390"
              },
              {
                "scName": "textMessages",
                "userCount": "91880"
              },
              {
                "scName": "partySessionCount",
                "userCount": "68129"
              }
            ],
            "streamingUsageCounts": [
              {
                "stName": "youtubeUsage",
                "userCount": "74092"
              },
              {
                "stName": "twitchUsage",
                "userCount": "13401"
              }
              {
                "stName": "mixerUsage",
                "userCount": "22907"
              }
            ]
          }
        ]
      },
      "XboxwideData": {
        "DeviceProperties": [
          {
            "applicationId": "XBOXWIDE",
            "connectionTypeDistribution": [
              {
                "conType": "WIRED",
                "deviceCount": "0.213677732584786"
              },
              {
                "conType": "WIRELESS",
                "deviceCount": "0.786322267415214"
              }
            ],
            "deviceCount": "1",
            "eliteControllerPresentDeviceCount": "0.0476609278128012",
            "externalDrivePresentDeviceCount": "0.173747147416134"
          }
        ],
        "UserProperties": [
          {
            "applicationId": "XBOXWIDE",
            "userCount": "1",
            "dvrUsageCounts": [
              {
                "dvrName": "gameClipUploads",
                "userCount": "0.173210623993245"
              },
              {
                "dvrName": "gameClipViews",
                "userCount": "0.202104713778096"
              },
              {
                "dvrName": "screenshotUploads",
                "userCount": "0.136682414274251"
              },
              {
                "dvrName": "screenshotViews",
                "userCount": "0.158057895120314"
              }
            ],
            "followerCountPercentiles": [
              {
                "percentage": "50",
                "value": "5"
              }
            ],
            "friendCountPercentiles": [
              {
                "percentage": "50",
                "value": "5"
              }
            ],
            "gamerScoreRangeDistribution": [
              {
                "scoreRange": "10K-25K",
                "userCount": "0.134709282586519"
              },
              {
                "scoreRange": "25K-50K",
                "userCount": "0.0549468789343825"
              },
              {
                "scoreRange": "50K-100K",
                "userCount": "0.017301313342277"
              },
              {
                "scoreRange": "3K-10K",
                "userCount": "0.216512780268453"
              },
              {
                "scoreRange": "<3K",
                "userCount": "0.573515440094644"
              },
              {
                "scoreRange": ">100K",
                "userCount": "0.00301430477372488"
              }
            ],
            "titleGamerScoreRangeDistribution": [
              {
                "scoreRange": "100-200",
                "userCount": "0.178055695637076"
              },
              {
                "scoreRange": "200-400",
                "userCount": "0.173283639825241"
              },
              {
                "scoreRange": "400-600",
                "userCount": "0.0986865193958259"
              },
              {
                "scoreRange": "600-800",
                "userCount": "0.0506375775462092"
              },
              {
                "scoreRange": "800-1000",
                "userCount": "0.0232398822856435"
              },
              {
                "scoreRange": "<100",
                "userCount": "0.456443551582991"
              },
              {
                "scoreRange": "≥1K",
                "userCount": "0.0196531337270126"
              }
            ],
            "socialUsageCounts": [
              {
                "scName": "gameInvites",
                "userCount": "0.460375855738335"
              },
              {
                "scName": "textMessages",
                "userCount": "0.429256324070832"
              },
              {
                "scName": "partySessionCount",
                "userCount": "0.378446577751268"
              },
              {
                "scName": "gamehubViews",
                "userCount": "0.000197115778147329"
              }
            ],
            "streamingUsageCounts": [
              {
                "stName": "youtubeUsage",
                "userCount": "0.330320919178683"
              },
              {
                "stName": "twitchUsage",
                "userCount": "0.040666241835399"
              }
              {
                "stName": "mixerUsage",
                "userCount": "0.140193816053558"
              }
            ]
          }
        ]
      }
    }
  ],
  "@nextLink": null,
  "TotalCount": 4
}
```

## <a name="related-topics"></a>相关主题

* [使用 Microsoft Store 服务访问分析数据](access-analytics-data-using-windows-store-services.md)
* [获取 Xbox Live 成就数据](get-xbox-live-achievements-data.md)
* [获取 Xbox Live 运行状况数据](get-xbox-live-health-data.md)
* [获取 Xbox Live 游戏中心数据](get-xbox-live-game-hub-data.md)
* [获取 Xbox Live 中心数据](get-xbox-live-club-data.md)
* [获取 Xbox Live 多人游戏数据](get-xbox-live-multiplayer-data.md)
* [获取 Xbox Live 并发使用情况数据](get-xbox-live-concurrent-usage-data.md)
