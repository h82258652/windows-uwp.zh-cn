---
title: Xbox Live C Api
author: KevinAsgari
description: 了解有关你可以使用与 Xbox Live 服务交互的平面 C API 模型。
ms.author: kevinasg
ms.date: 06/05/2018
ms.topic: article
keywords: xbox live，xbox，游戏，uwp，windows 10，xbox one、 c，xsapi
ms.localizationpriority: medium
ms.openlocfilehash: b8d48fbaa35ddfb81b8aba77e4129c19e2a55af7
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/09/2018
ms.locfileid: "6196173"
---
# <a name="introduction-to-the-xbox-live-c-apis"></a>Xbox Live C Api 简介

在 2018 年 6 月，到 XSAPI 添加了新的平面 C API 图层。 此新的 API 图层解决与 c + + 和 WinRT API 层出现一些问题。

C API 不涵盖 XSAPI 的所有功能，但正在处理其他功能。 所有 3 API 图层、 C、 c + + 和 WinRT 将继续支持，并随着时间的推移添加了其他功能。

> [!NOTE]
> C Api 目前仅适用于使用 Xbox 开发人员工具包 (XDK) 游戏。 它们并不支持这一次的 UWP 游戏。

## <a name="features-covered-by-the-c-apis"></a>涵盖 C Api 的功能

C API 目前支持以下功能和服务：

- 成就
- 状态
- 个人资料
- 社交
- 社交管理器

## <a name="benefits-of-the-c-api-for-xsapi"></a>Xsapi C API 的优势

- 允许游戏时调用 XSAPI 控制的内存分配。
- 允许游戏获得的线程处理时调用 XSAPI 的完全控制。
- 使用新 HTTP 库，libHttpClient，面向游戏开发人员。

你可以使用一起 c + + XSAPI C Api，但你不会使用 c + + Api 将获得上面列出的优势。

### <a name="managing-memory-allocations"></a>管理内存分配

使用新 C API，你现在可以指定它时分配内存，将调用 XSAPI 的回调函数。 如果你没有指定函数回调，XSAPI 将使用标准的内存分配例程。

若要手动指定内存例程，你可以执行以下操作：

- 在游戏开始：
  - 调用`XblMemSetFunctions(memAllocFunc, memFreeFunc)`指定用于分配和释放内存分配回调。
  - 调用`XblInitialize()`若要初始化的库实例。  
- 尽管游戏正在运行：
  - 调用的任何新的 C Api 在 XSAPI 中的分配或释放内存会导致 XSAPI 调用处理回调的指定的内存。  
- 当游戏退出：
  - 调用`XblCleanup()`以回收与 XSAPI 库相关联的所有资源。
  - 清理你的游戏的自定义内存管理器。

### <a name="managing-asynchronous-threads"></a>管理异步线程

C API 引入了新的异步线程调用完全控制的线程模型允许开发人员模式。 有关详细信息，请参阅[调用模式 XSAPI 平面 C 层异步调用](flatc-async-patterns.md)。

## <a name="migrating-code-to-use-c-xsapi"></a>迁移代码以使用 C XSAPI

可以与 XSAPI c + + Api 在项目中，并行使用 XSAPI C Api，因此我们建议你一次迁移一项功能。

C Api 和 c + + Api 是周围的常用的核心，只需使用不同的入口点，实际上只是精简包装器，因此该功能应保持不变。 但是，仅 C Api 可以充分利用的自定义内存和线程管理功能。

> [!IMPORTANT]
> 不能混合使用 C Api 的 XSAPI WinRT Api。

## <a name="where-to-view-the-c-apis"></a>若要查看 C Api 的位置

- [C API 标头文件](https://github.com/Microsoft/xbox-live-api/tree/master/Include/xsapi-c)
- [使用新 C Api 的示例代码](https://github.com/Microsoft/xbox-live-api/tree/master/InProgressSamples/Social/Xbox/C)
- [libHttpClient](https://github.com/Microsoft/libHttpClient)
