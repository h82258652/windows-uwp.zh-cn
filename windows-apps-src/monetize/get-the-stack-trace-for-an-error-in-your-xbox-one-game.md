---
author: Xansky
description: 在 Microsoft Store 分析 API 中使用此方法，可获取你的 Xbox One 中的错误堆栈跟踪游戏。
title: 获取你的 Xbox One 中的错误的堆栈跟踪游戏
ms.author: mhopkins
ms.date: 11/06/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 服务, Microsoft Store 分析 API, 堆栈跟踪, 错误
ms.localizationpriority: medium
ms.openlocfilehash: 78e65ad78079762ea5aabb95ddcaf4ce508b89bc
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/16/2018
ms.locfileid: "7173540"
---
# <a name="get-the-stack-trace-for-an-error-in-your-xbox-one-game"></a>获取你的 Xbox One 中的错误的堆栈跟踪游戏

使用 Microsoft Store 分析 API 中获取你的 Xbox One 中的错误堆栈跟踪游戏通过 Xbox 开发人员门户 (XDP) 引入并提供在 XDP 分析合作伙伴中心仪表板中的此方法。 此方法只能下载过去 30 天内发生的错误的堆栈跟踪。

你可以使用此方法之前，你都必须首先使用[获取 Xbox One 游戏中的错误的详细信息](get-details-for-an-error-in-your-xbox-one-game.md)的方法来检索与想要检索堆栈跟踪的错误相关联的 CAB 文件的 ID。

## <a name="prerequisites"></a>先决条件


若要使用此方法，首先需要执行以下操作：

* 如果尚未开始操作，请先完成 Microsoft Store 分析 API 的所有[先决条件](access-analytics-data-using-windows-store-services.md#prerequisites)。
* [获取 Azure AD 访问令牌](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)，以供在此方法的请求标头中使用。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以获取新的令牌。
* 获取与想要检索堆栈跟踪的错误相关联的 CAB 文件的 ID。 若要获取此 ID，使用[获取游戏的 Xbox One 中的错误的详细信息](get-details-for-an-error-in-your-xbox-one-game.md)的方法来检索你的应用中特定错误的详细信息，并使用该方法的响应正文中的**cabId**值。

## <a name="request"></a>请求


### <a name="request-syntax"></a>请求语法

| 方法 | 请求 URI                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/stacktrace``` |


### <a name="request-header"></a>请求头

| 标头        | 类型   | 说明                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授权 | 字符串 | 必需。 Azure AD 访问令牌的格式为 **Bearer** &lt;*token*&gt;。 |


### <a name="request-parameters"></a>请求参数

| 参数        | 类型   |  说明      |  必需  |
|---------------|--------|---------------|------|
| applicationId | 字符串 | 要为其检索堆栈跟踪 Xbox One 游戏的产品 ID。 若要获取你的游戏的产品 ID，请导航到 Xbox 开发人员门户 (XDP) 中你的游戏，并从 URL 中检索产品 ID。 或者，如果你从 Windows 合作伙伴中心分析报告下载你的运行状况数据，该.tsv 文件中包含的产品 ID。 |  是  |
| cabId | 字符串 | 获取与想要检索堆栈跟踪的错误相关联的 CAB 文件的唯一 ID。 若要获取此 ID，使用[获取游戏的 Xbox One 中的错误的详细信息](get-details-for-an-error-in-your-xbox-one-game.md)的方法来检索你的应用中特定错误的详细信息，并使用该方法的响应正文中的**cabId**值。 |  是  |

 
### <a name="request-example"></a>请求示例

下面的示例演示了如何为 Xbox One 游戏使用此方法获取堆栈跟踪。 *ApplicationId*值替换为你的游戏的产品 ID。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/stacktrace?applicationId=BRRT4NJ9B3D1&cabId=1336373323853 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>响应


### <a name="response-body"></a>响应正文

| 值      | 类型    | 说明                  |
|------------|---------|--------------------------------|
| 值      | array   | 一组对象，其中每个包含堆栈跟踪数据的一个帧。 有关每个对象中的数据的详细信息，请参阅以下[堆栈跟踪值](#stack-trace-values)部分。 |
| @nextLink  | 字符串  | 如果存在数据的其他页，此字符串中包含的 URI 可用于请求下一页数据。 例如，当请求的 **top** 参数设置为 10，但查询的错误超过 10 行时，就会返回此值。 |
| TotalCount | 整数 | 查询的数据结果中的行总数。          |


### <a name="stack-trace-values"></a>堆栈跟踪值

*Value* 数组中的元素包含以下值。

| 值           | 类型    | 描述      |
|-----------------|---------|----------------|
| level            | 字符串  |  此元素在调用堆栈中表示的帧编号。  |
| image   | 字符串  |   可执行文件或库映像的名称，包含在此堆栈帧中调用的函数。           |
| function | 字符串  |  在此堆栈帧中调用的函数名称。 这是你的游戏包括可执行文件或库的符号时才可用。              |
| offset     | 字符串  |  相对于函数开始的当前指令的字节偏移量。      |


### <a name="response-example"></a>响应示例

以下示例举例说明此请求的 JSON 响应正文。

```json
{
  "Value": [
    {
      "level": "0",
      "image": "Contoso.ContosoApp",
      "function": "Contoso.ContosoApp.MainPage.DoWork",
      "offset": "0x25C"
    }
    {
      "level": "1",
      "image": "Contoso.ContosoApp",
      "function": "Contoso.ContosoApp.MainPage.Initialize",
      "offset": "0x26"
    }
    {
      "level": "2",
      "image": "Contoso.ContosoApp",
      "function": "Contoso.ContosoApp.Start",
      "offset": "0x66"
    }
  ],
  "@nextLink": null,
  "TotalCount": 3
}

```

## <a name="related-topics"></a>相关主题

* [使用 Microsoft Store 服务访问分析数据](access-analytics-data-using-windows-store-services.md)
* [获取错误报告数据在 Xbox One 游戏](get-error-reporting-data-for-your-xbox-one-game.md)
* [获取游戏的 Xbox One 中的错误的详细信息](get-details-for-an-error-in-your-xbox-one-game.md)
* [下载你的 Xbox One 游戏中的错误的 CAB 文件](download-the-cab-file-for-an-error-in-your-xbox-one-game.md)
