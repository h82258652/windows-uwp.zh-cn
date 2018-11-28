---
ms.assetid: d305746a-d370-4404-8cde-c85765bf3578
description: 在 Microsoft Store 促销 API 中使用此方法管理促销性广告活动的目标市场配置文件。
title: 管理目标市场配置文件
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 促销 API, 广告活动
ms.localizationpriority: medium
ms.openlocfilehash: 0d84c6eb678bf884709e13ecefd81e64097ee738
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/28/2018
ms.locfileid: "7967821"
---
# <a name="manage-targeting-profiles"></a>管理目标市场配置文件


在 Microsoft Store 促销 API 中使用这些方法为促销性广告活动中的每个投放渠道选择要针对的用户、地理位置和广告资源类型。 可以在多个投放渠道中创建和重复使用目标市场配置文件。

有关目标市场配置文件与广告活动、投放渠道和创意之间关系的详细信息，请参阅[使用 Microsoft Store 服务开展广告活动](run-ad-campaigns-using-windows-store-services.md#call-the-windows-store-promotions-api)。

## <a name="prerequisites"></a>先决条件

若要使用这些方法，首先需要执行以下操作：

* 如果尚未开始操作，请先完成 Microsoft Store 促销 API 的所有[先决条件](run-ad-campaigns-using-windows-store-services.md#prerequisites)。
* [获取 Azure AD 访问令牌](run-ad-campaigns-using-windows-store-services.md#obtain-an-azure-ad-access-token)，以供在这些方法的请求标头中使用。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以获取新的令牌。

## <a name="request"></a>请求

这些方法具有以下 URI。

| 方法类型 | 请求 URI                                                      |  描述  |
|--------|------------------------------------------------------------------|---------------|
| POST   | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/targeting-profile``` |  创建新的目标市场配置文件。  |
| PUT    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/targeting-profile/{targetingProfileId}``` |  编辑通过 *targetingProfileId* 指定的目标市场配置文件。  |
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/targeting-profile/{targetingProfileId}``` |  获取通过 *targetingProfileId* 指定的目标市场配置文件。  |


### <a name="header"></a>Header

| 标头        | 类型   | 描述         |
|---------------|--------|---------------------|
| 授权 | 字符串 | 必需。 Azure AD 访问令牌的格式为 **Bearer** &lt;*token*&gt;。 |
| 跟踪 ID   | GUID   | 选填。 跟踪调用流的 ID。                                  |


### <a name="request-body"></a>请求正文

POST 和 PUT 方法需要一个 JSON 请求正文，其中包含[目标市场配置文件](#targeting-profile)对象的必填字段以及你要设置或更改的任何其他字段。


### <a name="request-examples"></a>请求示例

下面的示例演示如何调用 POST 方法来创建目标市场配置文件。

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/promotion/targeting-profile HTTP/1.1
Authorization: Bearer <your access token>

{
    "name": "Contoso App Campaign - Targeting Profile 1",
    "targetingType": "Manual",
    "age": [
      651,
      652],
    "gender": [
      700
    ],
    "country": [
      11,
      12
    ],
    "osVersion": [
      504
    ],
    "deviceType": [
      710
    ],
    "supplyType": [
      11470
    ]
}
```

下面的示例演示如何调用 GET 方法来检索目标市场配置文件。

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/targeting-profile/310023951  HTTP/1.1
Authorization: Bearer <your access token>
```

<span/>

## <a name="response"></a>响应

这些方法返回含有[目标市场配置文件](#targeting-profile)对象的 JSON 响应正文，目标市场配置文件对象包含有关已创建、已更新或已检索的目标市场配置文件的信息。 下面的示例展示了这些方法的响应正文。

```json
{
  "Data": {
    "id": 310021746,
    "name": "Contoso App Campaign - Targeting Profile 1",
    "targetingType": "Manual",
    "age": [
      651,
      652
    ],
    "gender": [
      700
    ],
    "country": [
      6,
      13,
      29
    ],
    "osVersion": [
      504,
      505,
      506,
      507,
      508
    ],
    "deviceType": [
      710,
      711
    ],
    "supplyType": [
      11470
    ]
  }
}
```

<span id="targeting-profile"/>

## <a name="targeting-profile-object"></a>目标市场配置文件对象

这些方法的请求和响应正文包含以下字段。 这张表列出了 POST 方法请求正文中的哪些字段是只读字段（意味着不能在 PUT 方法中更改它们）以及哪些字段是必填字段。

| 字段        | 类型   |  描述      |  只读  | 默认值  | POST 必填字段 |  
|--------------|--------|---------------|------|-------------|------------|
|  id   |  整数   |  目标市场配置文件的 ID。     |   是    |       |   否      |       
|  name   |  字符串   |   目标市场配置文件的名称。    |    否   |      |  是     |       
|  targetingType   |  字符串   |  以下值之一： <ul><li>**自动**： 指定此值可使 Microsoft 能够选择具体取决于你在合作伙伴中心中的应用的设置的目标市场配置文件。</li><li>**Manual**：指定此值可定义你自己的目标市场配置文件。</li></ul>     |  否     |  Auto    |   是    |       
|  age   |  数组   |   一个或多个整数，用于标识要针对的用户的年龄范围。 有关整数的完整列表，请参阅本文章中的[年龄值](#age-values)。    |    否    |  null    |     否    |       
|  gender   |  数组   |  一个或多个整数，用于标识要针对的用户的性别。 有关整数的完整列表，请参阅本文章中的[性别值](#gender-values)。       |  否    |  null    |     否    |       
|  country   |  数组   |  一个或多个整数，用于标识要针对的用户所在的国家或地区代码。 有关整数的完整列表，请参阅本文章中的[国家或地区代码值](#country-code-values)。    |  否    |  null   |      否   |       
|  osVersion   |  数组   |   一个或多个整数，用于标识要针对的用户的操作系统版本。 有关整数的完整列表，请参阅本文章中的[操作系统版本值](#osversion-values)。     |   否    |  null   |     否    |       
|  deviceType   | 数组    |  一个或多个整数，用于标识要针对的用户的设备类型。 有关整数的完整列表，请参阅本文章中的[设备类型值](#devicetype-values)。       |   否    |  null    |    否     |       
|  supplyType   |  数组   |  一个或多个整数，用于标识将展示活动广告的广告资源的类型。 有关整数的完整列表，请参阅本文章中的[供应类型值](#supplytype-values)。      |   否    |  null   |     否    |   |  


<span id="age-values"/>

### <a name="age-values"></a>年龄值

[TargetingProfile](#targeting-profile) 对象中的 *age* 字段包含一个或多个以下整数，用于标识要针对的用户的年龄范围。

|  *age* 字段的整数值  |  对应的年龄范围  |  
|---------------------------------|---------------------------|
|     651     |            13 到 17             |
|     652     |           18 到 24             |
|     653     |            25 到 34             |
|     654     |            35 到 49             |
|     655     |            50 及以上             |

要以编程方式获取 *age* 字段的支持值，你可以调用下面的 GET 方法。  对于 ```Authorization``` 标头，请以 **Bearer** &lt;*token*&gt; 形式传递你的 Azure AD 访问令牌。

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/reference/age
Authorization: Bearer <your access token>
```

下面的示例展示了此方法的响应正文。

```json
{
  "Data": {
    "Age": {
      "651": "Age13To17",
      "652": "Age18To24",
      "653": "Age25To34",
      "654": "Age35To49",
      "655": "Age50AndAbove"
    }
  }
}
```

<span id="gender-values"/>

### <a name="gender-values"></a>性别值

[TargetingProfile](#targeting-profile) 对象中的 *gender* 字段包含一个或多个以下整数，用于标识要针对的用户的性别。

|  *gender* 字段的整数值  |  对应的性别  |  
|---------------------------------|---------------------------|
|     700     |            男性             |
|     701     |           女性             |

要以编程方式获取 *gender* 字段的支持值，你可以调用下面的 GET 方法。  对于 ```Authorization``` 标头，请以 **Bearer** &lt;*token*&gt; 形式传递你的 Azure AD 访问令牌。

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/reference/gender
Authorization: Bearer <your access token>
```

下面的示例展示了此方法的响应正文。

```json
{
  "Data": {
    "Gender": {
      "700": "Male",
      "701": "Female"
    }
  }
}
```


<span id="osversion-values"/>

### <a name="os-version-values"></a>操作系统版本值

[TargetingProfile](#targeting-profile) 对象中的 *osVersion* 字段包含一个或多个以下整数，用于标识要针对的用户的操作系统版本。

|  *osVersion* 字段的整数值  |  对应的操作系统版本  |  
|---------------------------------|---------------------------|
|     500     |            Windows Phone 7             |
|     501     |           Windows Phone 7.1             |
|     502     |           Windows Phone 7.5             |
|     503     |           Windows Phone 7.8             |
|     504     |           Windows Phone 8.0             |
|     505     |           Windows Phone 8.1             |
|     506     |           Windows 8.0             |
|     507     |           Windows 8.1             |
|     508     |           Windows 10             |
|     509     |           Windows10 移动版             |

要以编程方式获取 *osVersion* 字段的支持值，你可以调用下面的 GET 方法。  对于 ```Authorization``` 标头，请以 **Bearer** &lt;*token*&gt; 形式传递你的 Azure AD 访问令牌。

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/reference/osversion
Authorization: Bearer <your access token>
```

下面的示例展示了此方法的响应正文。

```json
{
  "Data": {
    "OsVersion": {
      "500": "WindowsPhone70",
      "501": "WindowsPhone71",
      "502": "WindowsPhone75",
      "503": "WindowsPhone78",
      "504": "WindowsPhone80",
      "505": "WindowsPhone81",
      "506": "Windows80",
      "507": "Windows81",
      "508": "Windows10",
      "509": "WindowsPhone10"
    }
  }
}
```


<span id="devicetype-values"/>

### <a name="device-type-values"></a>设备类型值

[TargetingProfile](#targeting-profile) 对象中的 *deviceType* 字段包含一个或多个以下整数，用于标识要针对的用户的设备类型。

|  *deviceType* 字段的整数值  |  对应的设备类型  |  描述  |
|---------------------------------|---------------------------|---------------------------|
|     710     |  Windows   |  代表运行 Windows 10 或 Windows 8.x 桌面版的设备。  |
|     711     |  手机     |  代表运行 Windows 10 移动版、Windows Phone 8.x 或 Windows Phone 7.x 的设备。

要以编程方式获取 *deviceType* 字段的支持值，你可以调用下面的 GET 方法。  对于 ```Authorization``` 标头，请以 **Bearer** &lt;*token*&gt; 形式传递你的 Azure AD 访问令牌。

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/reference/devicetype
Authorization: Bearer <your access token>
```

下面的示例展示了此方法的响应正文。

```json
{
  "Data": {
    "DeviceType": {
      "710": "Windows",
      "711": "Phone"
    }
  }
}
```


<span id="supplytype-values"/>

### <a name="supply-type-values"></a>供应类型值

[TargetingProfile](#targeting-profile) 对象中的 *supplyType* 字段包含一个或多个以下整数，用于标识将展示活动广告的广告资源的类型。

|  *supplyType* 字段的整数值  |  对应的供应类型  |  描述  |
|---------------------------------|---------------------------|---------------------------|
|     11470     |  应用        |  指只在应用中展示的广告。  |
|     11471     |  通用        |  指在应用、Web 及其他显示表面上展示的广告。  |

要以编程方式获取 *supplyType* 字段的支持值，你可以调用下面的 GET 方法。  对于 ```Authorization``` 标头，请以 **Bearer** &lt;*token*&gt; 形式传递你的 Azure AD 访问令牌。

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/reference/supplytype
Authorization: Bearer <your access token>
```

下面的示例展示了此方法的响应正文。

```json
{
  "Data": {
    "SupplyType": {
      "11470": "App",
      "11471": "Universal"
    }
  }
}
```

<span id="country-code-values"/>

### <a name="country-code-values"></a>国家或地区代码值

[TargetingProfile](#targeting-profile) 对象中的 *country* 字段包含一个或多个以下整数，用于标识要针对的用户的 [ISO 3166-1 alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) 国家或地区代码。

|  *country* 字段的整数值  |  对应的国家或地区代码  |  
|-------------------------------------|------------------------------|
|     1      |            US                  |
|     2      |            AU                  |
|     3      |            AT                  |
|     4      |            BE                  |
|     5      |            BR                  |
|     6      |            CA                  |
|     7      |            DK                  |
|     8      |            FI                  |
|     9      |            FR                  |
|     10      |            DE                  |
|     11      |            GR                  |
|     12      |            HK                  |
|     13      |            IN                  |
|     14      |            IE                  |
|     15      |            IT                  |
|     16      |            JP                  |
|     17      |            LU                  |
|     18      |            MX                  |
|     19      |            NL                  |
|     20      |            NZ                  |
|     21      |            NO                  |
|     22      |            PL                  |
|     23      |            PT                  |
|     24      |            SG                  |
|     25      |            ES                  |
|     26      |            SE                  |
|     27      |            CH                  |
|     28      |            TW                  |
|     29      |            GB                  |
|     30      |            RU                  |
|     31      |            CL                  |
|     32      |            CO                  |
|     33      |            CZ                  |
|     34      |            HU                  |
|     35      |            ZA                  |
|     36      |            KR                  |
|     37      |            CN                  |
|     38      |            RO                  |
|     39      |            TR                  |
|     40      |            SK                  |
|     41      |            IL                  |
|     42      |            ID                  |
|     43      |            AR                  |
|     44      |            MY                  |
|     45      |            PH                  |
|     46      |            PE                  |
|     47      |            UA                  |
|     48      |            AE                  |
|     49      |            TH                  |
|     50      |            IQ                  |
|     51      |            VN                  |
|     52      |            CR                  |
|     53      |            VE                  |
|     54      |            QA                  |
|     55      |            SI                  |
|     56      |            BG                  |
|     57      |            LT                  |
|     58      |            RS                  |
|     59      |            HR                  |
|     60      |            HR                  |
|     61      |            LV                  |
|     62      |            EE                  |
|     63      |            IS                  |
|     64      |            KZ                  |
|     65      |            SA                  |
|     67      |            AL                  |
|     68      |            DZ                  |
|     70      |            AO                  |
|     72      |            AM                  |
|     73      |            AZ                  |
|     74      |            BS                  |
|     75      |            BD                  |
|     76      |            BB                  |
|     77      |            BY                  |
|     81      |            BO                  |
|     82      |            BA                  |
|     83      |            BW                  |
|     87      |            KH                  |
|     88      |            CM                  |
|     94      |            CD                  |
|     95      |            CI                  |
|     96      |            CY                  |
|     99      |            DO                  |
|     100      |            EC                  |
|     101      |            EG                  |
|     102      |            SV                  |
|     107      |            FJ                  |
|     108      |            GA                  |
|     110      |            GE                  |
|     111      |            GH                  |
|     114      |            GT                  |
|     118      |            HT                  |
|     119      |            HN                  |
|     120      |            JM                  |
|     121      |            JO                  |
|     122      |            KE                  |
|     124      |            KW                  |
|     125      |            KG                  |
|     126      |            LA                  |
|     127      |            LB                  |
|     133      |            MK                  |
|     135      |            MW                  |
|     138      |            MT                  |
|     141      |            MU                  |
|     145      |            ME                  |
|     146      |            MA                  |
|     147      |            MZ                  |
|     148      |            NA                  |
|     150      |            NP                  |
|     151      |            NI                  |
|     153      |            NG                  |
|     154      |            OM                  |
|     155      |            PK                  |
|     157      |            PA                  |
|     159      |            PY                  |
|     167      |            SN                  |
|     172      |            LK                  |
|     176      |            TZ                  |
|     180      |            TT                  |
|     181      |            TN                  |
|     184      |            UG                  |
|     185      |            UY                  |
|     186      |            UZ                  |
|     189      |            ZM                  |
|     为 190      |            ZW                  |
|     219      |            MD                  |
|     224      |            PS                  |
|     225      |            RE                  |
|     246      |            PR                  |

要以编程方式获取 *country* 字段的支持值，你可以调用下面的 GET 方法。  对于 ```Authorization``` 标头，请以 **Bearer** &lt;*token*&gt; 形式传递你的 Azure AD 访问令牌。

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/reference/country
Authorization: Bearer <your access token>
```

下面的示例展示了此方法的响应正文。

```json
{
  "Data": {
    "Country": {
      "1": "US",
      "2": "AU",
      "3": "AT",
      "4": "BE",
      "5": "BR",
      "6": "CA",
      "7": "DK",
      "8": "FI",
      "9": "FR",
      "10": "DE",
      "11": "GR",
      "12": "HK",
      "13": "IN",
      "14": "IE",
      "15": "IT",
      "16": "JP",
      "17": "LU",
      "18": "MX",
      "19": "NL",
      "20": "NZ",
      "21": "NO",
      "22": "PL",
      "23": "PT",
      "24": "SG",
      "25": "ES",
      "26": "SE",
      "27": "CH",
      "28": "TW",
      "29": "GB",
      "30": "RU",
      "31": "CL",
      "32": "CO",
      "33": "CZ",
      "34": "HU",
      "35": "ZA",
      "36": "KR",
      "37": "CN",
      "38": "RO",
      "39": "TR",
      "40": "SK",
      "41": "IL",
      "42": "ID",
      "43": "AR",
      "44": "MY",
      "45": "PH",
      "46": "PE",
      "47": "UA",
      "48": "AE",
      "49": "TH",
      "50": "IQ",
      "51": "VN",
      "52": "CR",
      "53": "VE",
      "54": "QA",
      "55": "SI",
      "56": "BG",
      "57": "LT",
      "58": "RS",
      "59": "HR",
      "60": "BH",
      "61": "LV",
      "62": "EE",
      "63": "IS",
      "64": "KZ",
      "65": "SA",
      "67": "AL",
      "68": "DZ",
      "70": "AO",
      "72": "AM",
      "73": "AZ",
      "74": "BS",
      "75": "BD",
      "76": "BB",
      "77": "BY",
      "81": "BO",
      "82": "BA",
      "83": "BW",
      "87": "KH",
      "88": "CM",
      "94": "CD",
      "95": "CI",
      "96": "CY",
      "99": "DO",
      "100": "EC",
      "101": "EG",
      "102": "SV",
      "107": "FJ",
      "108": "GA",
      "110": "GE",
      "111": "GH",
      "114": "GT",
      "118": "HT",
      "119": "HN",
      "120": "JM",
      "121": "JO",
      "122": "KE",
      "124": "KW",
      "125": "KG",
      "126": "LA",
      "127": "LB",
      "133": "MK",
      "135": "MW",
      "138": "MT",
      "141": "MU",
      "145": "ME",
      "146": "MA",
      "147": "MZ",
      "148": "NA",
      "150": "NP",
      "151": "NI",
      "153": "NG",
      "154": "OM",
      "155": "PK",
      "157": "PA",
      "159": "PY",
      "167": "SN",
      "172": "LK",
      "176": "TZ",
      "180": "TT",
      "181": "TN",
      "184": "UG",
      "185": "UY",
      "186": "UZ",
      "189": "ZM",
      "190": "ZW",
      "219": "MD",
      "224": "PS",
      "225": "RE",
      "246": "PR"
    }
  }
}
```

## <a name="related-topics"></a>相关主题

* [使用 Microsoft Store 服务开展广告市场活动](run-ad-campaigns-using-windows-store-services.md)
* [管理广告活动](manage-ad-campaigns.md)
* [管理广告活动的投放渠道](manage-delivery-lines-for-ad-campaigns.md)
* [管理广告活动的创意](manage-creatives-for-ad-campaigns.md)
* [获取广告活动效果数据](get-ad-campaign-performance-data.md)
