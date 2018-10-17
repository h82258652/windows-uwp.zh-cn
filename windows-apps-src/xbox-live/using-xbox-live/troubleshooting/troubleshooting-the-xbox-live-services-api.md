---
title: Xbox Live 服务 API 疑难解答
author: KevinAsgari
description: 了解如何在解决 Xbox Live API 问题时记录额外的错误信息。
ms.assetid: 3827bba1-902f-4f2d-ad51-af09bd9354c4
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Xbox live, xbox, 游戏, uwp, windows 10, xbox one, 疑难解答, 错误, 日志
ms.localizationpriority: medium
ms.openlocfilehash: dabc6458254c6ceec7995baa466de6dbddd76e18
ms.sourcegitcommit: 1c6325aa572868b789fcdd2efc9203f67a83872a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2018
ms.locfileid: "4744917"
---
# <a name="troubleshooting-the-xbox-live-apis"></a>Xbox Live API 疑难解答

## <a name="code"></a>代码

仅使用 Xbox Live 服务 API 层中的错误难以诊断故障。 可以向服务器提供额外的错误信息，如记录所有 RESTful 调用。 若要侦听此数据，请连接响应记录器并启用调试跟踪。 响应记录使你能够查看 HTTP 流量和 web 服务响应代码，这通常和 Fiddler 跟踪一样有用。

### <a name="c"></a>C++

以下代码示例已启用响应记录并将调试错误级别设为“详细”（你也可以将调试错误级别设为“错误”，以仅显示跟踪故障调用，或者设为“关闭”，以禁用跟踪）。 在 Visual Studio 中运行项目时，获得的调试输出将发送至“输出”窗格。  

```cpp

        // Set up debug tracing to the Output window in Visual Studio.
            xbox::services::system::xbox_live_services_settings::get_singleton_instance()->set_diagnostics_trace_level(
                xbox_services_diagnostics_trace_level::verbose
                );
```

你还可以选择将调试输出重定向至你自己的日志文件，如：

```cpp

        // Set up debug tracing of the Xbox Live Services API traffic to the game UI.
        m_xboxLiveContext->Settings->EnableServiceCallRoutedEvents = true;
        m_xboxLiveContext->Settings->ServiceCallRouted += ref new
        Windows::Foundation::EventHandler<Microsoft::Xbox::Services::XboxServiceCallRoutedEventArgs^>(
            [=] ( Platform::Object^, Microsoft::Xbox::Services::XboxServiceCallRoutedEventArgs^ args )
            {
                gameUI->Log(L"[URL]: " + args->HttpMethod + " " + args->Url->AbsoluteUri);
                gameUI->Log(L"");
                gameUI->Log(L"[Response]: " + args->HttpStatus.ToString() + " " + args->ResponseBody);
            });

```
