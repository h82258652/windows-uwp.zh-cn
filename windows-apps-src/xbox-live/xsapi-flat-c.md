---
title: Xbox Live C Api
description: 了解可用于与 Xbox Live 服务交互的平面 C API 模型。
ms.date: 06/05/2018
ms.topic: article
keywords: xbox live、 xbox、 游戏、 uwp、 windows 10 中，一个 xbox、 c、 xsapi
ms.localizationpriority: medium
ms.openlocfilehash: a1c73661b561d586f9e28957c7caa6a1b1f9cb03
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57597522"
---
# <a name="introduction-to-the-xbox-live-c-apis"></a>Xbox Live C Api 简介

在 2018 年 6 月到 XSAPI 添加了一个新的平面 C API 层。 此新的 API 层解决了与 c + + 和 WinRT API 层出现一些问题。

C API 不涵盖所有 XSAPI 功能，但正在开发的其他功能。 所有 3 个 API 层、 C、 c + + 和 WinRT 将继续支持，并随着时间的推移添加了其他功能。

> [!NOTE]
> C Api 目前仅适用于使用 Xbox 开发人员工具包 (XDK) 的标题。 它们不支持这一次的 UWP 游戏。

## <a name="features-covered-by-the-c-apis"></a>涵盖的 C Api 的功能

C API 目前支持以下功能和服务：

- 成就
- 状态
- 配置文件
- 社交
- 社交管理器

## <a name="benefits-of-the-c-api-for-xsapi"></a>XSAPI C API 的优点

- 允许标题来调用 XSAPI 时控制的内存分配。
- 允许标题，以获得完整的处理时调用 XSAPI 线程控制。
- 使用新 HTTP 的库，libHttpClient，适用于游戏开发人员。

可以使用与 c + + XSAPI 一起 C Api，但不是会使用 c + + Api 获得以前上面列出的好处。

### <a name="managing-memory-allocations"></a>管理内存分配

使用新的 C API，你现在可以指定 XSAPI 每当它会尝试分配内存时将调用的回调函数。 如果未指定函数回调，XSAPI 将使用标准内存分配例程。

若要手动指定内存例程，请执行以下操作：

- 在游戏的开头：
  - 调用`XblMemSetFunctions(memAllocFunc, memFreeFunc)`指定为分配和释放内存分配回调。
  - 调用`XblInitialize()`初始化库实例。  
- 运行游戏时：
  - 调用的任何新的 C Api 中的 XSAPI 的分配或可用内存将导致 XSAPI 调用指定的内存处理回调。  
- 当退出游戏：
  - 调用`XblCleanup()`回收与 XSAPI 库关联的所有资源。
  - 清理您的游戏的自定义内存管理器。

### <a name="managing-asynchronous-threads"></a>管理异步线程

C API 引入了新的异步线程调用模式，使开发人员完全控制的线程模型。 有关详细信息，请参阅[XSAPI 的调用模式平面 C 层异步调用](flatc-async-patterns.md)。

## <a name="migrating-code-to-use-c-xsapi"></a>迁移代码以使用 C XSAPI

因此，我们建议一次迁移一项功能，可以与在项目中，XSAPI c + + Api 一起使用 XSAPI C Api。

C Api 和 c + + Api 是常见的核心，只需与不同入口点的实际上只是瘦包装，因此功能应保持不变。 但是，只有 C Api 可以充分利用自定义内存和线程管理功能。

> [!IMPORTANT]
> 不能混合使用 C Api XSAPI WinRT Api。

## <a name="where-to-view-the-c-apis"></a>若要查看 C Api 的位置

- [C API 头文件](https://github.com/Microsoft/xbox-live-api/tree/master/Include/xsapi-c)
- [使用新的 C Api 的示例代码](https://github.com/Microsoft/xbox-live-api/tree/master/InProgressSamples/Social/Xbox/C)
- [libHttpClient](https://github.com/Microsoft/libHttpClient)
