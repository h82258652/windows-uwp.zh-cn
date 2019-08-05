---
description: 照片编辑器是一个 UWP 示例应用程序，其通过 C++/WinRT 语言投影展示开发。 此示例应用程序允许你从图片库检索照片，然后使用分类的照片效果编辑选择的图像。
title: 照片编辑器 C++/WinRT 示例应用程序
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, 标准, c++, cpp, winrt, 投影, 示例, 应用程序, 照片, 编辑器
ms.localizationpriority: medium
ms.openlocfilehash: 92aff51b6e5ba98d0f5fd157dd3a2dd57e861821
ms.sourcegitcommit: f8c354def02d5c82d195e4f629e6470110268223
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/30/2019
ms.locfileid: "68623374"
---
# <a name="photo-editor-cwinrt-sample-application"></a>照片编辑器 C++/WinRT 示例应用程序

> [!NOTE]
> 该示例面向 Windows 10 版本 1903（10.0 内部版本 18362）和 Visual Studio 2019，且已经过测试。 如果愿意，可以使用项目属性使项目重新面向 Windows 10 版本 1809（10.0 内部版本 17763），并/或使用 Visual Studio 2017 打开示例。

若要克隆或下载此示例应用程序，请参阅代码示例库中的[照片编辑器 C++/WinRT 示例应用程序](/samples/microsoft/windows-appsample-photo-editor/photo-editor-cwinrt-sample-application/)。

照片编辑器应用程序是一个通用 Windows 平台 (UWP) 示例应用程序，它通过 [C++/WinRT](intro-to-using-cpp-with-winrt.md) 语言投影展示开发。 此示例应用程序允许你从**图片**库检索照片，然后使用分类的照片效果编辑选择的图像。 在示例的源代码中，你将看到大量的常见做法 &mdash; 如[数据绑定](binding-property.md)和[异步操作](concurrency.md)&mdash; 使用 C++/WinRT 投影执行。 下面是示例演示的一些特定功能。

- 通过 Windows 运行时 (WinRT) API 使用标准 C++17 语法和库。
- 使用协同程序，包括使用 co_await、co_return、[IAsyncAction](/uwp/api/windows.foundation.iasyncaction) 和 [IAsyncOperation&lt;TResult&gt;](/uwp/api/windows.foundation.iasyncoperation_tresult_)   。
- 创建和使用自定义 Windows 运行时类（运行时类）投影类型和实现类型。 有关这些术语的详细信息，请参阅[通过 C++/WinRT 使用 API](consume-apis.md) 和[使用 C++/WinRT 创作 API](author-apis.md)。
- [事件处理](handle-events.md)，包括使用自动撤销事件令牌。
- 使用外部 Win2D NuGet 程序包和 [Windows::UI::Composition](/uwp/api/windows.ui.composition) 呈现图像效果。
- XAML 数据绑定，包括 [{x:Bind} 标记扩展](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)。
- XAML 样式和 UI 自定义，包括[连贯动画](../design/motion/connected-animation.md)。
