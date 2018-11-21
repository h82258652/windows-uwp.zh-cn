---
author: mtoepke
title: 应用对象和 DirectX
description: 使用 DirectX 的通用 Windows 平台 (UWP) 游戏不会使用许多 Windows UI 用户界面元素和对象。
ms.assetid: 46f92156-29f8-d65e-2587-7ba1de5b48a6
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, directx, 应用对象
ms.localizationpriority: medium
ms.openlocfilehash: 7e29a19410915836be3c54c0dc04a6d7dc29ceeb
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/21/2018
ms.locfileid: "7435452"
---
# <a name="the-app-object-and-directx"></a>应用对象和 DirectX



使用 DirectX 的通用 Windows 平台 (UWP) 游戏不会使用许多 Windows UI 用户界面元素和对象。 相反，因为它们在 Windows 运行时堆栈中的较低级别上运行，所以它们必须以更加基本的方式与用户界面框架互操作： 直接访问应用对象并与之互操作。 了解何时以及如何执行此互操作，以及作为 DirectX 开发人员， 你可以如何在 UWP 应用的开发中高效使用此模型。

请参阅有关不熟悉的图形条款或读取时遇到的概念信息[Direct3D 图形词汇表](../graphics-concepts/index.md)。

## <a name="the-important-core-user-interface-namespaces"></a>重要的核心用户界面命名空间


首先，让我们看一下必须包含（使用 **using**）在 UWP 应用中的 Windows 运行时命名空间。 获取更多详细信息。

-   [**Windows.ApplicationModel.Core**](https://msdn.microsoft.com/library/windows/apps/br205865)
-   [**Windows.ApplicationModel.Activation**](https://msdn.microsoft.com/library/windows/apps/br224766)
-   [**Windows.UI.Core**](https://msdn.microsoft.com/library/windows/apps/br208383)
-   [**Windows.System**](https://msdn.microsoft.com/library/windows/apps/br241814)
-   [**Windows.Foundation**](https://msdn.microsoft.com/library/windows/apps/br226021)

> **注意**如果你未开发 UWP 应用，请使用特定于 JavaScript 或 XAML 的库和命名空间，而不是这些命名空间中提供的类型中提供的用户界面组件。

 

## <a name="the-windows-runtime-app-object"></a>Windows 运行时应用对象


在你的 UWP 应用中，你希望获得一个窗口和一个视图提供程序，你可从该提供程序获得一个视图，也可将你的交换链（显示缓冲区）连接到该提供程序。 你也可以将此视图连接到正在运行的应用的特定于窗口的事件中。 要获得应用对象的父窗口（由 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 类型定义），可以创建一个实现 [**IFrameworkViewSource**](https://msdn.microsoft.com/library/windows/apps/hh700482) 的类型，就像我们在上一个代码段中所做的一样。

以下是使用核心用户界面框架获得一个窗口的一组基本步骤：

1.  创建实现 [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700478) 的类型。 这是你的视图。

    在此类型中，定义：

    -   一个 [**Initialize**](https://msdn.microsoft.com/library/windows/apps/hh700495) 方法，它接受一个 [**CoreApplicationView**](https://msdn.microsoft.com/library/windows/apps/br225017) 实例作为参数。 你可调用 [**CoreApplication.CreateNewView**](https://msdn.microsoft.com/library/windows/apps/dn297278) 来获取此类型的实例。 应用对象在应用启动时调用它。
    -   一个 [**SetWindow**](https://msdn.microsoft.com/library/windows/apps/hh700509) 方法，它接受一个 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 实例作为参数。 你可以访问你的新 [**CoreApplicationView**](https://msdn.microsoft.com/library/windows/apps/br225017) 实例上的 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br225019) 属性来获得此类型的实例。
    -   一个 [**Load**](https://msdn.microsoft.com/library/windows/apps/hh700501) 方法，它接受一个表示入口点的字符串作为唯一的参数。 应用对象在你调用此方法时提供入口点字符串。 这是设置资源的地方。 你在这里创建设备资源。 应用对象在应用启动时调用它。
    -   一个 [**Run**](https://msdn.microsoft.com/library/windows/apps/hh700505) 方法，它激活 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 对象并启动窗口事件调度程序。 应用对象在应用的进程启动时调用它。
    -   一个 [**Uninitialize**](https://msdn.microsoft.com/library/windows/apps/hh700523) 方法，它清除在对 [**Load**](https://msdn.microsoft.com/library/windows/apps/hh700501) 的调用中设置的资源。 应用对象在应用关闭时调用此方法。

2.  创建实现 [**IFrameworkViewSource**](https://msdn.microsoft.com/library/windows/apps/hh700482) 的类型。 这是你的视图提供程序。

    在此类型中，定义：

    -   一个名为 [**CreateView**](https://msdn.microsoft.com/library/windows/apps/hh700491) 的方法，可返回你的 [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700478) 实现（在步骤 1 中创建）的实例。

3.  将视图提供程序的一个实例从 **main** 传递到 [**CoreApplication.Run**](https://msdn.microsoft.com/library/windows/apps/hh700469)。

掌握了这些基础知识之后，让我们看一下扩展此方法所需的更多选项。

## <a name="core-user-interface-types"></a>核心用户界面类型


以下是 Windows 运行时中可能对你有帮助的其他核心用户界面类型：

-   [**Windows.ApplicationModel.Core.CoreApplicationView**](https://msdn.microsoft.com/library/windows/apps/br225017)
-   [**Windows.UI.Core.CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225)
-   [**Windows.UI.Core.CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211)

你可以使用这些类型访问应用的视图（具体来讲，访问绘制应用父窗口内容的位），并处理为该窗口引发的事件。 应用窗口的进程是一个*应用程序单线程单元* (ASTA)，它是隔离的并处理所有回调。

你的应用的视图由你的应用窗口的视图提供程序生成，并且在大多数情况下将由特定框架包或系统本身实现，因此你不需要亲自实现它。 如前所述，对于 DirectX，你需要实现一个精简视图提供程序。 以下组件和行为之间存在特定的 1 对 1 关系：

-   应用的视图，由 [**CoreApplicationView**](https://msdn.microsoft.com/library/windows/apps/br225017) 类型表示，并定义用于更新窗口的方法。
-   ASTA，定义应用的线程行为的属性。 你不能在 ASTA 上创建 COM STA 属性类型的实例。
-   视图提供程序，由你的应用从系统获取或由你实现。
-   父窗口，由 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 类型表示。
-   获取所有激活事件的源。 视图和窗口都有单独的激活事件。

总之，应用对象提供了一个视图提供程序工厂。 它创建一个视图提供程序并为应用实例化一个父窗口。 视图提供程序定义应用的父窗口的视图。 现在，让我们谈谈视图和父窗口的具体细节。

## <a name="coreapplicationview-behaviors-and-properties"></a>CoreApplicationView 行为和属性


[**CoreApplicationView**](https://msdn.microsoft.com/library/windows/apps/br225017) 表示当前的应用视图。 应用单一实例在初始化期间创建应用视图，但在激活之前，视图将保持休眠。 你可获得 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225)，可访问它之上的 [**CoreApplicationView.CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br225019) 属性来显示视图，你也可以向 [**CoreApplicationView.Activated**](https://msdn.microsoft.com/library/windows/apps/br225018) 事件注册委托来处理视图的激活和停用事件。

## <a name="corewindow-behaviors-and-properties"></a>CoreWindow 行为和属性


在应用对象初始化时，会创建父窗口（一个 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 实例）并传递给视图提供程序。 如果应用有一个窗口要显示，它会显示它，否则它会初始化视图。

[**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 提供了许多特定于输入和基本窗口行为的事件。 你可以向这些事件注册自己的委托来处理它们。

你也可以访问 [**CoreWindow.Dispatcher**](https://msdn.microsoft.com/library/windows/apps/br208264) 属性来获得窗口的窗口事件调度程序，该属性提供了一个 [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211) 实例。

## <a name="coredispatcher-behaviors-and-properties"></a>CoreDispatcher 行为和属性


你可以确定为具有 [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211) 类型的窗口调度的事件线程行为。 在此类型中，有一个特别重要的方法：即 [**CoreDispatcher.ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215) 方法，该方法启动窗口事件处理。 使用应用的错误选项调用此方法可能导致各种异常的事件处理行为。

| CoreProcessEventsOption 选项                                                           | 说明                                                                                                                                                                                                                                  |
|------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [**CoreProcessEventsOption.ProcessOneAndAllPending**](https://msdn.microsoft.com/library/windows/apps/br208217) | 调度队列中所有当前可用的事件。 如果没有正在等待的事件，则等待下一个新事件。                                                                                                                                 |
| [**CoreProcessEventsOption.ProcessOneIfPresent**](https://msdn.microsoft.com/library/windows/apps/br208217)     | 如果在队列中有正在等待的事件，则调度一个事件。 如果没有正在等待的事件，则不等待引发新事件，而是立即返回。                                                                                          |
| [**CoreProcessEventsOption.ProcessUntilQuit**](https://msdn.microsoft.com/library/windows/apps/br208217)        | 等待新事件并调度所有可用事件。 继续此行为，直到窗口关闭或者应用程序调用 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 实例上的 [**Close**](https://msdn.microsoft.com/library/windows/apps/br208260) 方法为止。 |
| [**CoreProcessEventsOption.ProcessAllIfPresent**](https://msdn.microsoft.com/library/windows/apps/br208217)     | 调度队列中所有当前可用的事件。 如果没有正在等待的事件，则立即返回。                                                                                                                                          |

 

使用 DirectX 的 UWP 应使用 [**CoreProcessEventsOption.ProcessAllIfPresent**](https://msdn.microsoft.com/library/windows/apps/br208217) 选项来防止可能中断图形更新的阻止行为。

## <a name="asta-considerations-for-directx-devs"></a>DirectX 开发中与 ASTA 有关的注意事项


用于定义 UWP 和 DirectX 应用的运行时表示形式的应用对象使用名为应用程序单线程单元 (ASTA) 的线程模型来托管你的应用的 UI 视图。 如果要开发 UWP 和 DirectX 应用，需要熟悉 ASTA 的属性，因为从 UWP 和 DirectX 应用调度的任何线程都必须使用 [**Windows::System::Threading**](https://msdn.microsoft.com/library/windows/apps/br229642) API 或使用 [**CoreWindow::CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211)。 （你可以通过从应用中调用 [**CoreWindow::GetForCurrentThread**](https://msdn.microsoft.com/library/windows/apps/hh701589) 来获取 ASTA 的 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 对象。）

作为 UWP DirectX 应用的开发人员，你需要注意的最重要的事是必须通过在 **main()** 上设置 **Platform::MTAThread** 使你的应用线程可以调度 MTA 线程。

```cpp
[Platform::MTAThread]
int main(Platform::Array<Platform::String^>^)
{
    auto myDXAppSource = ref new MyDXAppSource(); // your view provider factory 
    CoreApplication::Run(myDXAppSource);
    return 0;
}
```

当 UWP DirectX 应用的应用对象激活时，它会创建将用于 UI 视图的 ASTA。 新的 ASTA 线程将调用到视图提供程序工厂中（以为应用对象创建视图提供程序），因此你的视图提供程序代码将在该 ASTA 线程上运行。

同样，从 ASTA 分离的任何线程都必须位于 MTA 中。 请注意，所分离的任何 MTA 线程仍可能会产生重新进入问题并导致死锁。

如果你要移植要在 ASTA 线程上运行的现有代码，请牢记以下注意事项：

-   等待基元（如 [**CoWaitForMultipleObjects**](https://msdn.microsoft.com/library/windows/desktop/hh404144)）在 ASTA 中的行为与在 STA 中不同。
-   COM 调用模式循环在 ASTA 中以不同的方式运行。 当传出调用在进行中时，你可以不再接收不相关的调用。 例如，以下行为将从 ASTA 创建死锁（并立即使应用崩溃）：
    1.  ASTA 调用 MTA 对象并传递接口指针 P1。
    2.  稍后，ASTA 调用相同的 MTA 对象。 MTA 对象在其返回到 ASTA 之前调用 P1。
    3.  P1 无法进入 ASTA，因为它在进行不相关的调用时被阻止。 但是，MTA 线程在尝试对 P1 进行调用时被阻止。

    你可以通过执行以下操作来解决此问题：
    -   使用在并行模式库 (PPLTasks.h) 中定义的 **async** 模式
    -   尽快从应用的 ASTA（应用的主线程）调用 [**CoreDispatcher::ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215) 以允许任意调用。

    也就是说，你不能依赖于将无关调用立即提交到你的应用的 ASTA。 有关异步调用的详细信息， 请阅读[使用 C++ 进行异步编程](https://msdn.microsoft.com/library/windows/apps/mt187334)。

总的来说，在设计 UWP 应用时，使用应用的 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 和 [**CoreDispatcher::ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215) 的 [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211) 来处理所有 UI 线程，而不要尝试自行创建和管理 MTA 线程。 当你需要一个不能用 **CoreDispatcher** 处理的单独线程时，请使用异步模式，并按照前面提到的指南操作以避免出现重新进入问题。

 

 




