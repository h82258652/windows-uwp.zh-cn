---
title: C++ API 错误处理
author: KevinAsgari
description: 了解在通过 C++ API 进行 Xbox Live 服务调用时如何处理错误。
ms.assetid: 10b47e68-8b1f-4023-96a4-404f3f6a9850
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, 游戏, uwp, windows 10, xbox one, 错误, 处理
ms.localizationpriority: medium
ms.openlocfilehash: 4a9947180764d196579569536ec979fc31f3570d
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2018
ms.locfileid: "6263322"
---
# <a name="c-api-error-handling"></a>C++ API 错误处理

在 C++ API 中，大部分调用将会返回相应的 xbox_live_result<payload_type>，而不是发出异常。

## <a name="xboxliveresult-structure"></a>xbox_live_result 结构
xbox_live_result 具有 3 项：
1. 操作返回的错误，
2. 用于调试的特定错误消息和
3. 结果的有效负载（如果有错误，则可以为空）

你可以在 Xbox Live 文档中获得与 xbox_live_result 及错误代码相关的详细信息。

该结构如下所示：

```cpp
template<typename T>
class xbox_live_result
{
    const std::error_code& err();
    const std::string& err_message();
    T& payload();
};
```

**err** - 返回错误。  如果无错误，则为 NULL 参考值。  这相当于 C++ STL 错误，其中，你将通过调用 value() 获得原始值。  调用 message() 将获得字符串表示形式。  因此，如果错误代码意味着“参数无效”，则 ```err().message()``` 为文本“参数无效”。

**err_message** - 详细描述错误。  例如，如果 **err** 为“无效参数”，则 **err_message** 将详细描述哪个参数无效。

**payload** - 返回关注的项目。  例如，考虑可能通过调用 get_achievement 获得的 ```xbox_live_result<achievement>```。  在此示例中，有效负载是成就本身（如果不存在错误）。

## <a name="example"></a>示例

```cpp
// Function which returns an xbox_live_result
xbox_live_result<std::shared_ptr<title_presence_change_subscription>> presenceChangeSubscriptionResult =
xbox::services::presence::subscribe_to_title_presence_change(
    xboxUserId,
    titleId
    );

printf("Error value %d, string %s", achievementResult.err().value(), achievementResult.err().message());

// Would output:
// "0 Success" if successful
// "401 Unauthorized" if auth issue

if (!achievementResult.err())
{
  // Do things on success.  Payload will be populated if applicable.
  std::shared_ptr<title_presence_change_subscription> presenceChangeSubscription = presenceChangeSubscriptionResult->payload();

  // ...
}
else if (achievement.err() == xboxlive_error_code::http_status_403_forbidden)
{
  // Special handling for 403 errors
}
else if (achievementResult.err() == xbox_live_error_condition::auth)
{
  // Handle broad auth failures.  See below section for more info on xbox_live_error_condition
}

```

## <a name="using-xboxliveerrorcondition-to-test-against-broad-error-categories"></a>使用 xbox_live_error_condition 测试各种错误类别
在以上示例中，我们测试了 403 错误以及调用 ```xbox_live_error_condition::auth``` 产生的错误代码。

 使用 xbox_live_result err() 函数时，可以单独测试错误代码。  例如，对于 400 类错误，你可以具有以下单独测试和控制流：

* xbox_live_error_code::http_status_400_bad_request
* xbox_live_error_code::http_status_401_unauthorized
* xbox_live_error_code::http_status_403_forbidden
* 等

但一般情况下，这并不是你想要进行的操作，你想要的是将一类错误作为一个错误进行测试。  因此，你可以使用 ```xbox_live_error_condition``` 类中提供的枚举测试一类错误。  我们对相等运算符实施重载，这将会自动测试多个错误代码。  除了 ```auth``` 外，还有诸如 ```rta``` 和 ```http``` 之类的类别。  完整列表位于 *errors.h* 或 *xblsdk_cpp.chm* 中。

如需观看介绍此功能以及其他 C++ Xbox 服务 API 功能的视频，请参阅 *XSAPI：C++，无异常！* 下面的 [Xfest 2015 视频](https://developer.xboxlive.com/en-us/platform/documentlibrary/events/Pages/Xfest2015.aspx)中的 XFest 访谈
