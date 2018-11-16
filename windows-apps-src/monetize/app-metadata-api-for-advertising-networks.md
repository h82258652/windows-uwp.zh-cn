---
author: Xansky
description: 了解如何使用应用元数据 REST API 访问应用的某些类型的元数据。 此 API 旨在供广告网络用于检索 Microsoft Store 中的应用的有关信息，以便它们可以改进向广告商销售广告空间的方式。
title: 广告网络的应用元数据 API
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, 广告网络, 应用元数据
ms.assetid: f0904086-d61f-4adb-82b6-25968cbec7f3
ms.localizationpriority: medium
ms.openlocfilehash: 9533b244174cc5770a68f866c722db1781fdd544
ms.sourcegitcommit: 9f8010fe67bb3372db1840de9f0be36097ed6258
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/16/2018
ms.locfileid: "7100331"
---
# <a name="app-metadata-api-for-advertising-networks"></a>广告网络的应用元数据 API

网络广告可以使用*应用元数据 API* 以编程方式检索 Microsoft Store 中的应用的相关元数据，其中包括详细信息（如描述和应用的 Store 一览的类别）以及该应用是否面向 13 岁以下的儿童。 对此 API 的访问当前仅限于 Microsoft 授予该 API 访问权限的开发人员。

本文详细说明了如何使用[应用元数据 API 门户](https://admetadata.portal.azure-api.net/)请求 API 的访问权限、如何获取访问该 API 的订阅密钥以及如何调用该 API。

## <a name="request-access"></a>请求访问权限

广告网络可以通过遵循以下说明请求应用元数据 API 的访问权限：

1. 转到应用元数据 API 门户的 [https://admetadata.portal.azure-api.net/signup](https://admetadata.portal.azure-api.net/signup) 页面。
2. 输入所需信息，然后单击**注册**按钮。
3. 在同一站点上，单击**产品**选项卡，然后单击**广告的应用详细信息**。
4. 在下一页上，单击**订阅**按钮。 这会向 Microsoft 提交对访问应用元数据 API 的请求。

请求提交后，你会在大约 24 小时内收到一封电子邮件，通知你请求已允许还是已拒绝。

<span id="get-key" />

## <a name="get-your-subscription-key"></a>获取订阅密钥

如果允许你访问应用元数据 API，请遵循以下说明获取订阅密钥。 必须在对该 API 调用的请求标头中传递此密钥。

1. 转到应用元数据 API 门户的 [https://admetadata.portal.azure-api.net/signin](https://admetadata.portal.azure-api.net/signin) 页面，然后使用你的电子邮件地址和密码进行登录。
2. 在站点右上角单击你的名字，然后单击**配置文件**。
3. 在该页的**你的订阅**部分，单击**主密钥**旁边的**显示**。 这就是你的订阅密钥。 复制该密钥，以便可以在以后调用该 API 时使用。

<span id="call-the-api" />

## <a name="call-the-api"></a>调用 API

拥有了订阅密钥后，随时可以通过你所选择的编程语言使用 HTTP REST 语法调用该 API。 有关 API 语法的信息，请参阅以下 [API 语法](#syntax)部分。 若要查看采用 C#、JavaScript、Python 和其他几种语言的代码示例，请单击应用元数据 API 门户的 **API** 选项卡、单击**应用详细信息**，然后查看该页面底部的**代码示例**部分。

也可以使用应用元数据 API 门户提供的 UI 调用该 API：
  1. 在门户中，单击 **API** 选项卡，然后单击**应用详细信息**。
  2. 在以下页面上，在 **app_id** 字段中输入要检索元数据的应用的 [app_id](#request-parameters)，在 **Ocp_Apim_Subscription-Key** 字段中输入你的订阅密钥。
  3. 单击**发送**。 响应内容将显示在该页面的底部。


<span id="syntax" />

## <a name="api-syntax"></a>API 语法

此方法具有以下请求语法。

| 方法 | 请求 URI                                                      |
|--------|------------------------------------------------------------------|
| GET   | ```https://admetadata.azure-api.net/v1/app/{app_id}``` |

<span/>
 

### <a name="request-header"></a>请求头

| 标头        | 类型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Ocp-Apim-Subscription-Key | 字符串 | 必需。 [从应用元数据 API 门户中检索](#get-key)到的订阅密钥。  |

<span/>

### <a name="request-parameters"></a>请求参数

| 名称        | 类型   | 描述                                                                 |
|---------------|--------|-----------------------|
| app_id | 字符串 | 必需。 要检索元数据的应用的 ID。 这可以是以下值之一：<br/><br/><ul><li>应用的应用商店 ID。 应用商店 ID 的一个示例是 9NBLGGH29DM8。</li><li>最初面向 Windows8.x 或 Windows Phone 8.x 生成的应用的产品 ID（有时也称为*应用 ID*）。 GUID 的产品 ID。</li></ul> |

<span/>

### <a name="request-example"></a>请求示例

以下示例演示了如何检索应用商店 ID 为 9NBLGGH29DM8 的应用的元数据。

```syntax
GET https://admetadata.azure-api.net/v1/app/9NBLGGH29DM8 HTTP/1.1
Ocp-Apim-Subscription-Key: <subscription key>
```

### <a name="response-body"></a>响应正文

以下示例演示了成功调用此方法的 JSON 响应正文。

```json
{
    "storeId":"9NBLGGH29DM8",
    "name":"Contoso Sports App",
    "description":"Catch the latest scores and replays using Contoso Sports App!",
    "phoneStoreGuid":"920217d7-90da-4019-99e8-46e4a6bd2594",
    "windowsStoreGuid":"d7e982e7-fbf3-42b5-9dad-72b65bd4e248",
    "storeCategory":"Sports",
    "iabCategory":"Sports",
    "iabCategoryId":"IAB17",
    "coppa":false,
    "downloadUrl":"https://www.microsoft.com/store/apps/9nblggh29dm8",
    "isLive":true,
    "iconUrls":[
      "//store-images.microsoft.com/image/apps.15753.13510798883747357.b6981489-6fdd-4fec-b655-a822247d5a76.33529096-a723-4204-9da9-a5dd8b328d4e"
    ],
    "type":"App",
    "devices":[
      "PC",
      "Phone"
    ],
    "platformVersions":[
      "Windows.Universal"
    ],
    "screenshotUrls":[
      "//store-images.microsoft.com/image/apps.15901.19810723133740207.c9781069-6fef-5fba-a055-c922051d59b6.40129896-d083-5604-ab79-aba68bfa084f"
    ]
}
```

有关响应正文中这些值的详细信息，请参阅下表。

| 值      | 类型   | 描述    |
|------------|--------|--------------------|
| storeId           | 字符串  | 应用的应用商店 ID。 应用商店 ID 的一个示例是 9NBLGGH29DM8。     |  
| name           | 字符串  | 应用名称。   |
| description           | 字符串  | 应用的应用商店一览中的描述。  |
| phoneStoreGuid           | 字符串  | 应用的产品 ID (Windows Phone 8.x)。 这是一个 GUID。  |
| windowsStoreGuid           | 字符串  | 应用的产品 ID (Windows8.x)。 这是一个 GUID。 |
| storeCategory           | 字符串  | 应用在应用商店中的类别。 有关受支持的值，请参阅适用于应用商店中的应用的[类别和子类别表](../publish/category-and-subcategory-table.md)。  |
| iabCategory           | 字符串  | 由美国互动广告局 (IAB) 定义的应用的内容类别。 例如，**新闻**或**体育**。 有关内容类别的列表，请参阅 IAB 网站上的 [IAB 技术实验室内容分类](https://www.iab.com/guidelines/iab-quality-assurance-guidelines-qag-taxonomy)页面。   |
| iabCategoryId           | 字符串  | 应用的内容类别的 ID。 例如，**IAB12** 为新闻类别的 ID，**IAB17** 为体育类别的 ID。 有关内容类别 ID 的列表，请参阅 [OpenRTB API 规范](http://www.iab.com/wp-content/uploads/2015/05/OpenRTB_API_Specification_Version_2_3_1.pdf)中的 5.1 节。 |
| coppa           | 布尔值  | 如果应用面向 13 岁以下的儿童，则为 true，因此有义务遵循《儿童在线隐私保护法》(COPPA)，否则，则为 false。  |
| downloadUrl           | 字符串  | 指向应用商店中应用一览的链接。 此链接采用格式 ```https://www.microsoft.com/store/apps/<Store ID>```。  |
| isLive           | 布尔值  | 如果应用商店中当前可提供此应用，则为 True；否则，为 false。  |
| iconUrls           | 数组  |  一个或多个字符串数组，包含与此应用关联的 URL 图标的相对路径。 若要检索图标，请向该 URL 添加前缀 *http* 或 *https*。  |
| type           | 字符串  | 以下字符串之一：**App** 或 **Game**。  |
| 设备           |  数组  | 一个或多个以下字符串数组，用于指定应用支持的设备类型：**台式机**、**手机**、**Xbox**、**IoT**、**服务器**和**全息操作系统**。  |
| platformVersions           | 数组  |  一个或多个以下字符串数组，用于指定应用支持的参数：**Windows.Universal**、**Windows.Windows8x** 和 **Windows.WindowsPhone8x**。  |
| screenshotUrls           | 数组  | 一个或多个字符串数组，包含此应用截屏 URL 的相对路径。 若要检索截屏，请向该 URL 添加前缀 *http* 或 *https*。  |

<span/>



 

 
