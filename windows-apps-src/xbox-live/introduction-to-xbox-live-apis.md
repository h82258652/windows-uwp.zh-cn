---
title: Xbox Live API 简介
author: KevinAsgari
description: 了解各种可用来与 Xbox Live 服务交互的 API 模型。
ms.assetid: 5918c3a2-6529-4f07-b44d-51f9861f91ec
ms.author: kevinasg
ms.date: 06/05/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1f295c1f1b432f90e12d3e628cd35a54412812ec
ms.sourcegitcommit: fbdc9372dea898a01c7686be54bea47125bab6c0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/08/2018
ms.locfileid: "4416808"
---
# <a name="introduction-to-xbox-live-apis"></a>Xbox Live API 简介

## <a name="use-xbox-live-services"></a>使用 Xbox Live 服务

可以使用两种方法从 Xbox Live 服务获取信息：

- 使用客户端 API，即 Xbox Live 服务 API (**XSAPI**)
- 直接调用 **Xbox Live REST 终结点**

使用 Xbox Live 服务 API (**XSAPI**) 的优点包括：

- 系统将为你处理身份验证、编码和 HTTP 发送和接收的详细信息。
- 包装器 API 的参数，以及从该 API 返回的数据，采用本机数据类型处理。因此，你无需执行 JSON 编码和解码。
- 直接调用 Web 服务涉及多个异步步骤，这些步骤由包装器 API 封装；这可以简化作品代码的读写过程。
- 某些功能，例如写入游戏事件，仅在 XSAPI 中提供。

直接使用 **Xbox Live REST 终结点**的优点包括：

- 可从 Web 服务调用 Xbox Live 终结点
- 可调用不包括在 XSAPI 中的终结点。  XSAPI 仅包括我们认为游戏将会使用的 API，因此，如果缺少任何内容，请通过论坛告知我们。
- 某些通过 REST 终结点提供的功能可能没有相应的 XSAPI 包装器。

你的游戏和应用并不仅限于使用这些方法之一。 你可以使用 XSAPI 包装器，并且仍可根据需要直接调用 REST 终结点。

## <a name="xbox-live-services-api-overview"></a>Xbox Live 服务 API 概述 ##

Xbox Live 服务 API (**XSAPI**) 公开了三个组的客户端 Api，它支持各种客户方案：

- [XSAPI WinRT API](#xsapi-winrt-based-api)
- [基于 XSAPI C++11 的 API](#xsapi-c++11-based-api)
- [XSAPI C 基于 API](#xsapi-c-based-api)（**新从 2018 年 6 月起**）

比较 Api:

### <a name="xsapi-winrt-based-api"></a>基于 XSAPI WinRT 的 API

- 支持使用 C++/CX、C# 和 JavaScript 编写的应用程序。
    - C++/CX 是一项 Microsoft C++ 扩展，可简化 WinRT 编程，例如将 ^ 用作 WinRT 指针。
- 支持面向 Xbox One XDK 平台和通用 Windows 平台 (UWP) x86、x64 及 ARM 体系结构的应用程序。
- 通过例外以包括 C++/CX 在内的所有语言处理错误。
- 还支持 C++/WinRT。  详细了解 C + + 可在中找到 WinRT[https://moderncpp.com/2016/10/13/cppwinrt-available-on-github/](https://moderncpp.com/2016/10/13/cppwinrt-available-on-github/)

下面是使用 C++/WinRT 调用 XSAPI WinRT API 的示例：

```c++
winrt::Windows::Xbox::System::User cppWinrtUser = winrt::Windows::Xbox::System::User::Users().GetAt(0);
winrt::Microsoft::Xbox::Services::XboxLiveContext xblContext(cppWinrtUser);
```

如果你要在迁移代码时组合使用 C++/CX 和 C++/WinRT，则也可以这样做，但是稍微有点复杂。  
下面是使用 C++/WinRT（如果是 C++/CX User^ 对象）调用 XSAPI WinRT API 的示例。

```c++
::Windows::Xbox::System::User^ user1 = ::Windows::Xbox::System::User::Users->GetAt(0);
winrt::Windows::Xbox::System::User cppWinrtUser(nullptr);
winrt::copy_from(cppWinrtUser, reinterpret_cast<winrt::ABI::Windows::Xbox::System::IUser*>(user1));
winrt::Microsoft::Xbox::Services::XboxLiveContext xblContext(cppWinrtUser);
```


### <a name="xsapi-c11-based-api"></a>基于 XSAPI C++11 的 API

- 使用跨平台 ISO 标准 C++11
- 支持使用 C++ 编写的应用程序
- 支持面向 Xbox One XDK 平台和通用 Windows 平台 (UWP) x86、x64 及 ARM 体系结构的应用程序。
- 通过 std::error_code 处理错误。
- 基于 C++11 的 API 是推荐的 API，适用于 C++ 游戏引擎，可提高性能和改进调试。
- 如果你已加入 Xbox Live 创意者计划，在包括 XSAPI 标头之前，请先定义 XBOX_LIVE_CREATORS_SDK。 这样一来，会将 API 图面区域仅限于可供 Xbox Live 创意者计划中的开发人员使用的区域，并更改登录方法以适用于创意者计划中的作品。  例如：

```c++
#define XBOX_LIVE_CREATORS_SDK
#include "xsapi\services.h"
```

- 还支持 C++/WinRT。  详细了解 C + + 可在中找到 WinRT[https://moderncpp.com/2016/10/13/cppwinrt-available-on-github/](https://moderncpp.com/2016/10/13/cppwinrt-available-on-github/)

若要将 C++/WinRT 与 XSAPI C++ API 结合使用，在包括 XSAPI 标头之前，请先定义 XSAPI_CPPWINRT。  例如：

```c++
#define XSAPI_CPPWINRT
#include "xsapi\services.h"
```

下面是使用 C++/WinRT 调用 XSAPI C++ API 的示例:

```c++
winrt::Windows::Xbox::System::User cppWinrtUser = winrt::Windows::Xbox::System::User::Users().GetAt(0);
std::shared_ptr<xbox::services::xbox_live_context> xboxLiveContext = std::make_shared<xbox::services::xbox_live_context>(cppWinrtUser);
```

### <a name="xsapi-c-based-api"></a>XSAPI C 基于 API

- 允许游戏时调用 XSAPI 控制的内存分配。
- 允许游戏获得的线程处理调用 XSAPI 时的完全控制。
- 使用新 HTTP 库，libHttpClient，面向游戏开发人员。

有关详细信息，请参阅[Xbox Live C Api 简介](xsapi-flat-c.md)。
