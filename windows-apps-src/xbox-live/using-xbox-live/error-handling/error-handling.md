---
title: 错误处理
author: KevinAsgari
description: 了解如何在进行 Xbox Live 服务调用时处理错误。
ms.assetid: e433dfbd-488b-44ff-8333-1dcf0329cd60
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Xbox live, xbox, 游戏, uwp, windows 10, xbox one, 错误, 服务调用
ms.localizationpriority: medium
ms.openlocfilehash: c322239e65d019695b879e71032eee94dbcadc14
ms.sourcegitcommit: 5dda01da4702cbc49c799c750efe0e430b699502
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/21/2018
ms.locfileid: "4119806"
---
# <a name="error-handling"></a>错误处理

进行服务调用时，开发人员应特别注意。 进行网络调用时，你的作品应始终处理错误。 即便是在开发过程中持续运行的服务调用，在实际环境中也会因为各种原因出现故障。 例如，你的调用可能会因以下原因故障：

* 网络不可用。
* 服务太忙，返回 503 HTTP 状态代码。
* 请求无效，返回 400 HTTP 状态代码。
* 用户没有相应的权限，返回 403 HTTP 状态代码。
* 用户已被禁止，返回 401 HTTP 状态代码。
IXMLHTTPRequest2（供 WinRT 服务 API 调用）无法发送请求（ERROR_TIMEOUT、RPC_S_CALL_FAILED_DNE 等）
* 以上列表并不详尽。 以上大部分原因将会导致服务 API 层出现异常。 你的作品应当能够捕获这些异常并进行相应的处理。

Xbox 服务 API (XSAPI) 中有两种不同形式的错误处理，具体取决于你使用的 API：

请参阅 [C++ API 错误处理](error-handling-cpp.md)。

请参阅 [WinRT API 错误处理](error-handling-winrt.md)。
