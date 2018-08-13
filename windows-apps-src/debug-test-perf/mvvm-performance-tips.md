---
author: jwmsft
ms.assetid: 159681E4-BF9E-4A57-9FEE-EC7ED0BEFFAD
title: MVVM 和语言性能提示
description: 本主题讨论了一些与选择软件设计模式和编程语言相关的性能注意事项。
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: d308fd8b8ded0ac737fc39c4760bc52d8414b3cb
ms.sourcegitcommit: ec18e10f750f3f59fbca2f6a41bf1892072c3692
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/14/2017
ms.locfileid: "894483"
---
# <a name="mvvm-and-language-performance-tips"></a>MVVM 和语言性能提示

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

本主题讨论了一些与选择软件设计模式和编程语言相关的性能注意事项。

## <a name="the-model-view-viewmodel-mvvm-pattern"></a>Model-View-ViewModel (MVVM) 模式

Model-View-ViewModel (MVVM) 模式在许多 XAML 应用中很常见。 （MVVM 是非常类似于 Fowler 对 Model-View-Presenter 模式的描述，但它专用于 XAML）。 MVVM 模式有个问题，就是它可能会无意间产生具有过多层和过多分配的应用。 MVVM 会产生这些应用。

-   **关注点分离**。 它始终有助于将一个问题划分为几个较小的部分，而且诸如 MVVM 或 MVC 的模式也是将应用（甚至是单个控件）划分为较小部分的一种方式：实际视图、视图的逻辑模型（视图模型）以及独立于视图的应用逻辑（模型）。 特别的是，这种受欢迎的工作流可以让设计人员拥有使用一种工具的视图、让开发人员拥有使用另一种工具的模型以及让设计集成者拥有使用两种工具的视图模型。
-   **单元测试**。 你可以对独立于视图的视图模型进行单元测试（并随后对模型也进行测试），因而不需要创建窗口、驱动输入等。 通过保持较小的视图，你可以测试很大一部分应用，而不必再创建一个窗口。
-   **用户体验更改的灵活性**。 当根据最终用户反馈对用户体验稍作调整时，视图会查看最常见的更改和最新的更改。 通过使视图分离，可更快地容纳这些视图并且无需对应用做太多的改动。

对于 MVVM 模式，有多个具体的定义，而且还存在有助于实现该模式的第三方框架。 但是严格遵循该模式的所有变体可能会导致应用的开销比调整多很多。

-   设计 XAML 数据绑定（{Binding} 标记扩展）在某种程度上是为了启用模型/视图模式。 但 {Binding} 会带来非比寻常的工作集和 CPU 开销。 创建 {Binding} 会导致一系列分配，而更新绑定目标可能会引起反射和装箱。 目前正在使用 {x:Bind} 标记扩展处理这些问题，以便在生成时编译绑定。 **建议：** 使用 {x:Bind}。
-   在 MVVM 中，通常使用 ICommand 将 Button.Click 连接到视图模型，如常见的 DelegateCommand 或 RelayCommand 帮助程序。 虽然这些命令是额外的分配，但包含了 CanExecuteChanged 事件侦听器、增加了工作集和页面的启动/导航时间。 **建议：** 请考虑将事件处理程序放在你的代码隐藏中，同时将它们附加到视图事件，并在引发这些事件时在你的视图模型上调用命令，这可以作为使用便利 ICommand 接口的备用方法。 你还需要添加额外代码，以便在该命令不可用时禁用“按钮”。
-   在 MVVM 中，通常使用所有可能的 UI 配置来创建“页面”，然后通过将“可见性”属性绑定到 VM 中的属性来折叠树的某些部分。 这会不必要地增加启动时间，也可能会增加工作集（因为树的某些部分可能永远不会变得可见）。 **建议：** 使用 [x:Load attribute](../xaml-platform/x-load-attribute.md) 或 [x:DeferLoadStrategy attribute](../xaml-platform/x-deferloadstrategy-attribute.md) 功能来延迟启动之外的树的不必要部分。 另外，还要为页面的不同模式创建单独的用户控件，并使用代码隐藏保持仅加载必要的控件。

## <a name="ccx-recommendations"></a>C++/CX 建议

-   **使用最新版本**。 对 C++/CX 编译器进行连续的性能改进。 确保使用最新的工具集生成你的应用。
-   **禁用 RTTI (/ GR-)**。 在编译器中，RTTI 默认处于打开状态，因此除非你的生成环境将其关闭，否则你可能正在使用它。 RTTI 的开销非常大，如果你的代码对其没有很大的依赖性，请将其关闭。 XAML 框架对你的代码使用 RTTI 没有任何要求。
-   **避免大量使用 ppltask**。 当调用异步 WinRT API 时，ppltask 非常便利，但它们的代码大小开销也非常大。 C++/CX 团队正致力于开发一种语言功能 await，它将提供更出色的性能。 同时，在代码的热路径中均衡使用 ppltask。
-   **避免在应用的“业务逻辑”中使用 C++/CX**。 C++/CX 旨在为访问 C++ 应用中的 WinRT API 提供一种便捷方式。 它将使用具有开销的包装器。 你应该避免在类的业务逻辑/模型内使用 C++/CX，并进行保留以便用于你的代码和 WinRT 之间的边界。