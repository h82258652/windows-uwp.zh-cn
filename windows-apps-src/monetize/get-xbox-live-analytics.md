---
description: 在 Microsoft Store 分析 API 中使用此方法获取 Xbox Live 分析数据。
title: 获取 Xbox Live 分析数据
ms.date: 06/04/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 服务, Microsoft Store 分析 API, Xbox Live 分析
ms.localizationpriority: medium
ms.openlocfilehash: 5ab41001f7331defc6d37b0561e2844392ccca3c
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2019
ms.locfileid: "67321853"
---
# <a name="get-xbox-live-analytics-data"></a>获取 Xbox Live 分析数据

在 Microsoft Store 分析 API 中使用此方法来获取玩你的[支持 Xbox Live 的游戏](https://docs.microsoft.com/gaming/xbox-live/index.md)的客户的前 30 天常规分析数据，包括设备附件使用情况、Internet 连接类型、玩家分数分布、游戏统计数据，以及好友和关注者数据。 此信息也位于[Xbox 的分析报告](../publish/xbox-analytics-report.md)在合作伙伴中心。

> [!IMPORTANT]
> 该方法只支持 Xbox 游戏或使用 Xbox Live 服务的游戏。 这些游戏必须经过[概念审批流程](../gaming/concept-approval.md)，其中包括 [Microsoft 合作伙伴](https://docs.microsoft.com/gaming/xbox-live/developer-program-overview.md#microsoft-partners)发布的游戏以及通过 [ID@Xbox 计划](https://docs.microsoft.com/gaming/xbox-live/developer-program-overview.md#id)提交的游戏。 该方法当前不支持通过 [Xbox Live 创意者计划](https://docs.microsoft.com/gaming/xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md)发布的游戏。

针对支持 Xbox Live 的游戏的额外分析数据通过以下方法提供：
* [获取 Xbox Live 成就数据](get-xbox-live-achievements-data.md)
* [获取 Xbox Live 的运行状况数据](get-xbox-live-health-data.md)
* [获取 Xbox Live 游戏中心数据](get-xbox-live-game-hub-data.md)
* [获取 Xbox Live 俱乐部数据](get-xbox-live-club-data.md)
* [获取 Xbox Live 多玩家数据](get-xbox-live-multiplayer-data.md)
* [获取 Xbox Live 并发使用情况数据](get-xbox-live-concurrent-usage-data.md)

## <a name="prerequisites"></a>系统必备

若要使用此方法，首先需要执行以下操作：

* 完成 Microsoft Store 分析 API 的所有[先决条件](access-analytics-data-using-windows-store-services.md#prerequisites)（如果尚未这样做）。
* [获取 Azure AD 访问令牌](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)，以供在此方法的请求标头中使用。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以获取新的令牌。

## <a name="request"></a>请求


### <a name="request-syntax"></a>请求语法

| 方法 | 请求 URI       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics``` |


### <a name="request-header"></a>请求头

| Header        | 在任务栏的搜索框中键入   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授权 | string | 必需。 Azure AD 访问令牌的格式为 **Bearer** *token*&lt;&gt;。 |


### <a name="request-parameters"></a>请求参数

| 参数        | 在任务栏的搜索框中键入   |  描述      |  必需  
|---------------|--------|---------------|------|
| applicationId | string | 你要检索常规 Xbox Live 数据的游戏的 [Store ID](in-app-purchases-and-trials.md#store-ids)。  |  是  |
| metricType | string | 指定要检索的 Xbox Live 分析数据的类型的字符串。 对于此方法，指定值 **productvalues**。  |  是  |


### <a name="request-example"></a>请求示例

以下示例演示了一个请求，该请求用于获取玩你的支持 Xbox Live 的游戏的客户的常规分析数据。 将 *applicationId* 值替换为你的游戏的 Store ID。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=productvalues HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>响应

此方法返回一个包含以下对象的 *Value* 数组。

| Object      | 描述                  |
|-------------|---------------------------------------------------|
| ProductData   |   包含一个 [DeviceProperties](#deviceproperties) 对象以及一个 [UserProperties](#userproperties) 对象，它们包含过去 30 天内您的游戏的设备和用户分析数据。    |  
| XboxwideData   |  包含一个 [DeviceProperties](#deviceproperties) 对象以及一个 [UserProperties](#userproperties) 对象，它们包含过去 30 天内所有 Xbox Live 客户的平均设备和用户分析数据（百分比形式）。 包含此数据是为了对您的游戏数据进行比较。   |                                           


### <a name="deviceproperties"></a>DeviceProperties

此资源包含过去 30 天内您的游戏的设备使用情况数据或所有 Xbox Live 客户的平均设备使用情况数据。

| 值           | 在任务栏的搜索框中键入    | 描述        |
|-----------------|---------|------|
|  applicationId               |    string     |  检索其分析数据的游戏的 [Store ID](in-app-purchases-and-trials.md#store-ids)。   |
|  connectionTypeDistribution               |    array     |   包含指示 Xbox 有多少客户使用有线 Internet 连接，多少客户使用无线 Internet 连接的对象。 每个对象都有两个字符串字段： <ul><li>**conType**:指定连接类型。</li><li>**deviceCount**:在中**ProductData**对象，此字段指定使用的连接类型的游戏的客户数。 在 **XboxwideData** 对象中，此字段指定使用该连接类型的所有 Xbox Live 客户的百分比。</li></ul>   |     
|  deviceCount               |   string      |  在 **ProductData** 对象中，此字段指定过去 30 天内用于玩你的游戏的客户设备的数量。 在 **XboxwideData** 对象中，此字段将为始终为 1，指示所有 Xbox Live 客户的数据的起始百分比为 100%。   |     
|  eliteControllerPresentDeviceCount               |   string      |  在 **ProductData** 对象中，此字段指定使用 Xbox Elite 无线控制器的游戏客户的数量。 在 **XboxwideData** 对象中，此字段指定使用 Xbox Elite 无线控制器的所有 Xbox Live 客户的百分比。  |     
|  externalDrivePresentDeviceCount               |   string      |  在 **ProductData** 对象中，此字段指定在 Xbox 上使用外部硬盘驱动器的游戏客户的数量。 在 **XboxwideData** 对象中，此字段指定在 Xbox 上使用外部硬盘驱动器的所有 Xbox Live 客户的百分比。  |


### <a name="userproperties"></a>UserProperties

此资源包含过去 30 天内您的游戏的用户数据或所有 Xbox Live 客户的平均用户数据。

| ReplTest1           | 在任务栏的搜索框中键入    | 描述        |
|-----------------|---------|------|
|  applicationId               |    string     |   检索其分析数据的游戏的 [Store ID](in-app-purchases-and-trials.md#store-ids)。  |
|  UserCount               |    string     |   在 **ProductData** 对象中，此字段指定过去 30 天内玩你的游戏的客户的数量。 在 **XboxwideData** 对象中，此字段将为始终为 1，指示所有 Xbox Live 客户的数据的起始百分比为 100%。   |     
|  dvrUsageCounts               |   array      |  包含表明多少客户使用了游戏 DVR 来录制和查看游戏的对象。 每个对象都有两个字符串字段： <ul><li>**dvrName**:指定游戏 DVR 功能使用。 可能的值为 **gameClipUploads**、**gameClipViews**、**screenshotUploads** 和 **screenshotViews**。</li><li>**userCount**:在中**ProductData**对象，此字段指定使用指定的游戏 DVR 功能的游戏的客户数。 在 **XboxwideData** 对象中，此字段指定使用指定游戏 DVR 功能的所有 Xbox Live 客户的百分比。</li></ul>   |     
|  followerCountPercentiles               |   array      |  包含提供有关客户关注者数的详细信息的对象。 每个对象都有两个字符串字段： <ul><li>**百分比**:目前，此值始终为 50，指示作为中的值提供的跟踪块数据。</li><li>**值**:在中**ProductData**对象，此字段指定为您的游戏客户的关注者数中值。 在 **XboxwideData** 对象中，此字段指定所有 Xbox Live 客户的关注者的中值数量。</li></ul>  |   
|  friendCountPercentiles               |   array      |  包含提供有关客户好友数的详细信息。 每个对象都有两个字符串字段： <ul><li>**百分比**:目前，此值始终为 50，指示作为中的值提供的友元数据。</li><li>**值**:在中**ProductData**对象，此字段指定为您的游戏客户好友数中值。 在 **XboxwideData** 对象中，此字段指定所有 Xbox Live 客户的好友的中值数量。</li></ul>  |     
|  gamerScoreRangeDistribution               |   array      |  包含提供有关客户玩家分数分布情况的详细信息。 每个对象都有两个字符串字段： <ul><li>**scoreRange**:所有范围以下字段为其提供使用情况数据。 例如 **10K-25K**。</li><li>**userCount**:在中**ProductData**对象，此字段指定的已播放的所有游戏的指定范围中具有所有游戏的客户数。 在 **XboxwideData** 对象中，此字段指定所有 Xbox Live 客户在其玩过的所有游戏中其玩家分数属于指定范围的客户的百分比。</li></ul>  |
|  titleGamerScoreRangeDistribution               |   array      |  包含提供有关游戏玩家分数分布情况的详细信息的对象。 每个对象都有两个字符串字段： <ul><li>**scoreRange**:所有范围以下字段为其提供使用情况数据。 例如 **100-200**。</li><li>**userCount**:在中**ProductData**对象，此字段指定的具有所有指定的范围为您的游戏中的游戏的客户数。 在 **XboxwideData** 对象中，此字段指定所有 Xbox Live 客户在你的游戏中其玩家分数属于指定范围的客户的百分比。</li></ul>   |
|  socialUsageCounts               |   array      |  包含提供有关客户社交使用情况的详细信息的对象。 每个对象都有两个字符串字段： <ul><li>**scName**:社交使用情况的类型。 例如 **gameInvites**和**textMessages**。</li><li>**userCount**:在中**ProductData**对象，此字段指定参加了您的游戏客户数在指定的社交用法类型。 在 **XboxwideData** 对象中，此字段指定所有参与指定社交使用情况类型的 Xbox Live 客户的百分比。</li></ul>   |
|  streamingUsageCounts               |   array      |  包含提供有关客户流式处理使用情况的详细信息。 每个对象都有两个字符串字段： <ul><li>**stName**:流式处理平台的类型。 例如，**youtubeUsage**、**twitchUsage** 和 **mixerUsage**。</li><li>**userCount**:在中**ProductData**对象，此字段指定的已使用指定的流式处理平台的游戏的客户数。 在 **XboxwideData** 对象中，此字段指定使用指定流式处理平台的所有 Xbox Live 客户的百分比。</li></ul>  |


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

* [使用 Microsoft Store 服务的访问分析数据](access-analytics-data-using-windows-store-services.md)
* [获取 Xbox Live 成就数据](get-xbox-live-achievements-data.md)
* [获取 Xbox Live 的运行状况数据](get-xbox-live-health-data.md)
* [获取 Xbox Live 游戏中心数据](get-xbox-live-game-hub-data.md)
* [获取 Xbox Live 俱乐部数据](get-xbox-live-club-data.md)
* [获取 Xbox Live 多玩家数据](get-xbox-live-multiplayer-data.md)
* [获取 Xbox Live 并发使用情况数据](get-xbox-live-concurrent-usage-data.md)
