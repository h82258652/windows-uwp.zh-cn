---
ms.assetid: 066711E0-D5C4-467E-8683-3CC64EDBCC83
title: 使用 C# 或 Visual Basic 调用异步 API
description: 通用 Windows 平台 (UWP) 包含许多异步 API，可确保应用在执行可能花费大量时间的任务时仍能保持响应。
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, C#, Visual Basic, 异步
ms.localizationpriority: medium
ms.openlocfilehash: 899af2ffd26419d4c8906d703d6708d202f8c150
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/10/2018
ms.locfileid: "8878308"
---
# <a name="call-asynchronous-apis-in-c-or-visual-basic"></a>使用 C# 或 Visual Basic 调用异步 API



通用 Windows 平台 (UWP) 包含许多异步 API，可确保应用在执行可能花费大量时间的任务时仍能保持响应。 本主题将介绍如何从采用 C# 或 Microsoft Visual Basic 的 UWP 使用异步方法。

异步 API 使你的应用无需等待大规模操作完成即可继续执行。 例如，从 Internet 下载信息的应用等待信息到达可能要花费数秒钟。 如果你使用同步方法来检索信息，则应用会在方法返回之前被阻止。 应用将不会响应用户交互，而且因为无响应的原因，所以用户可能会感到沮丧。 通过提供异步 API，UWP 有助于确保应用在执行长操作时仍能响应用户。

UWP 中的大多数异步 API 都没有对应的同步 API，因此需要确保了解如何在通用 Windows 平台 (UWP) 应用中一同使用异步 API 与 C# 或 Visual Basic。 下面将显示如何调用 UWP 的异步 API。

## <a name="using-asynchronous-apis"></a>使用异步 API


按照惯例，异步方法的名称应以“Async”结尾。 通常调用异步 API 是为了响应用户的操作，如在用户单击某个按钮时。 在事件处理程序中调用异步方法是使用异步 API 的最简单方法之一。 下面使用 **await** 运算符作为一个示例。

假设你拥有一个应用，该应用列出了某个位置中博客文章的标题。 该应用具有一个 [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265)，用户单击该按钮即可获取标题。 标题显示在 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652) 中。 当用户单击该按钮时，该应用仍然保持响应，同时等待获取博客文章的信息，这一点非常重要。 为了确保此响应，UWP 提供了一个用于下载源的异步方法 [**SyndicationClient.RetrieveFeedAsync**](https://msdn.microsoft.com/library/windows/apps/BR243460)。

以下示例通过调用异步方法 [**SyndicationClient.RetrieveFeedAsync**](https://msdn.microsoft.com/library/windows/apps/BR243460) 并等待结果，从而获取某个博客的博客文章列表。

> [!div class="tabbedCodeSnippets" data-resources="OutlookServices.Calendar"]
[!code-csharp[Main](./AsyncSnippets/csharp/MainPage.xaml.cs#SnippetDownloadRSS)]
[!code-vb[Main](./AsyncSnippets/vbnet/MainPage.xaml.vb#SnippetDownloadRSS)]

有关该示例，有几个重要事项。 首先，对异步方法 [**RetrieveFeedAsync**](https://msdn.microsoft.com/library/windows/apps/BR243460) 的调用，行 `SyndicationFeed feed = await client.RetrieveFeedAsync(feedUri)` 使用 **await** 运算符。 你可以将 **await** 运算符视为告知编译器你正在调用某个异步方法，该方法会导致编译器执行某些额外的工作，以便你无需进行这些工作。 接下来，事件处理程序的声明包含关键字 **async**。 必须将该关键字包含在其中使用 **await** 运算符的任何方法的方法声明中。

在本主题中，我们将不对编译器使用 **await** 运算符所执行的操作进行详细介绍，而是检查你的应用所执行的操作以便该操作是异步操作并且能够响应。 考虑使用同步代码时发生的情况。 例如，假设有一个名为 `SyndicationClient.RetrieveFeed` 的异步方法。 （这类方法不存在，但想象它存在。）如果你的应用包含行 `SyndicationFeed feed = client.RetrieveFeed(feedUri)`（而不是 `SyndicationFeed feed = await client.RetrieveFeedAsync(feedUri)`），应用将停止执行，直到 `RetrieveFeed` 的返回值可用。 当你的应用等待方法完成时，它无法响应任何其他事件，如另一个 [**Click**](https://msdn.microsoft.com/library/windows/apps/BR227737) 事件。 即，你的应用将被阻止，直到 `RetrieveFeed` 返回为止。

但如果你调用 `client.RetrieveFeedAsync`，则方法将启动检索并立即返回。 当你将 **await** 与 [**RetrieveFeedAsync**](https://msdn.microsoft.com/library/windows/apps/BR243460) 结合使用时，应用将临时退出事件处理程序。 然后，它便可以在 **RetrieveFeedAsync** 异步执行时处理其他事件。 这样便可以保持应用对用户进行响应。 当 **RetrieveFeedAsync** 完成并且 [**SyndicationFeed**](https://msdn.microsoft.com/library/windows/apps/BR243485) 可用时，应用一定会在 `SyndicationFeed feed = await client.RetrieveFeedAsync(feedUri)` 之后重新进入它停止的事件处理程序，并完成方法的剩余部分。

有关使用 **await** 运算符的好处是代码看上去与使用想象的 `RetrieveFeed` 方法的代码没有多大的不同。 可以在不使用 **await** 运算符的情况下，采用 C# 或 Visual Basic 编写异步代码，但所得到的代码通常会强调异步执行的机制。 这使得异步代码难以编写、理解和维护。 通过使用 **await** 运算符，你可以获得异步应用的优势，同时又不会使代码复杂。

## <a name="return-types-and-results-of-asynchronous-apis"></a>返回异步 API 的类型和结果


如果你跟随指向 [**RetrieveFeedAsync**](https://msdn.microsoft.com/library/windows/apps/BR243460) 的链接，那么你可能会注意到 **RetrieveFeedAsync** 的返回类型不是 [**SyndicationFeed**](https://msdn.microsoft.com/library/windows/apps/BR243485)， 而是 `IAsyncOperationWithProgress<SyndicationFeed, RetrievalProgress>`。 从原始语法看，异步 API 返回其中包含结果的对象。 尽管该对象很常见，但有时却很有用，若要将异步方法视为可等待的方法，**await** 运算符实际上是对该方法的返回值执行操作，而不是对该方法执行操作。 当你应用 **await** 运算符时，你得到的内容即为在该方法返回的对象上调用 **GetResult** 的结果。 在该示例中，**SyndicationFeed** 就是 **RetrieveFeedAsync.GetResult()** 的结果。

当你使用异步方法时，可检查签名以查看你将在等待由该方法返回的值后得到的内容。 UWP 中的所有异步 API 均可返回以下类型之一：

-   [**IAsyncOperation&lt;TResult&gt;**](https://msdn.microsoft.com/library/windows/apps/BR206598)
-   [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;**](https://msdn.microsoft.com/library/windows/apps/BR206594)
-   [**IAsyncAction**](https://msdn.microsoft.com/library/windows/apps/windows.foundation.iasyncaction.aspx)
-   [**IAsyncActionWithProgress&lt;TProgress&gt;**](https://msdn.microsoft.com/library/windows/apps/br206581.aspx)

异步方法的结果类型与 `      TResult` 类型参数相同。 没有 `TResult` 的类型没有结果。 你可以将结果视为 **void**。 在 Visual Basic 中，[Sub](https://msdn.microsoft.com/library/windows/apps/xaml/831f9wka.aspx) 过程等同于返回类型为 **void** 的方法。

下表给出了异步方法的示例并列出了每个方法的返回类型和结果类型。

| 异步方法                                                                           | 返回类型                                                                                                                                        | 结果类型                                       |
|-----------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------|
| [**SyndicationClient.RetrieveFeedAsync**](https://msdn.microsoft.com/library/windows/apps/BR243460)     | [**IAsyncOperationWithProgress&lt;SyndicationFeed, RetrievalProgress&gt;**](https://msdn.microsoft.com/library/windows/apps/BR206594)                                 | [**SyndicationFeed**](https://msdn.microsoft.com/library/windows/apps/BR243485) |
| [**FileOpenPicker.PickSingleFileAsync**](https://msdn.microsoft.com/library/windows/apps/JJ635275) | [**IAsyncOperation&lt;StorageFile&gt;**](https://msdn.microsoft.com/library/windows/apps/BR206598)                                                                                | [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/BR227171)          |
| [**XmlDocument.SaveToFileAsync**](https://msdn.microsoft.com/library/windows/apps/BR206284)                 | [**IAsyncAction**](https://msdn.microsoft.com/library/windows/apps/windows.foundation.iasyncaction.aspx)                                                                                                           | **void**                                          |
| [**InkStrokeContainer.LoadAsync**](https://msdn.microsoft.com/library/windows/apps/Hh701757)               | [**IAsyncActionWithProgress&lt;UInt64&gt;**](https://msdn.microsoft.com/library/windows/apps/br206581.aspx)                                                                   | **void**                                          |
| [**DataReader.LoadAsync**](https://msdn.microsoft.com/library/windows/apps/BR208135)                            | [**DataReaderLoadOperation**](https://msdn.microsoft.com/library/windows/apps/BR208120)，实现 **IAsyncOperation&lt;UInt32&gt;** 的自定义结果类。 | [**UInt32**](https://msdn.microsoft.com/library/windows/apps/br206598.aspx)                     |

 

[**适用于 UWP 应用的 .NET**](https://msdn.microsoft.com/library/windows/apps/xaml/br230232.aspx) 中定义的异步方法的返回类型为 [**Task**](https://msdn.microsoft.com/library/windows/apps/xaml/system.threading.tasks.task.aspx) 或 [**Task&lt;TResult&gt;**](https://msdn.microsoft.com/library/windows/apps/xaml/dd321424.aspx)。 返回 **Task** 的方法与 UWP 中返回 [**IAsyncAction**](https://msdn.microsoft.com/library/windows/apps/windows.foundation.iasyncaction.aspx) 的异步方法类似。 在任何情况下，异步方法的结果均为 **void**。 返回类型 **Task&lt;TResult&gt;** 类似于 [**IAsyncOperation&lt;TResult&gt;**](https://msdn.microsoft.com/library/windows/apps/BR206598)，因为在运行任务时，异步方法的结果与 `TResult` 类型参数的类型相同。 有关使用**适用于 UWP 应用的 .NET** 和任务的详细信息，请参阅[用于 Windows 运行时应用的 .NET 概述](https://msdn.microsoft.com/library/windows/apps/xaml/br230302.aspx)。

## <a name="handling-errors"></a>处理错误


当你使用 **await** 运算符从异步方法检索结果时，可以使用一个 **try/catch** 块来处理异步方法中发生的错误，与处理同步方法中的错误一样。 上一个示例将 **RetrieveFeedAsync** 方法和 **await** 操作包装到一个 **try/catch** 块中，以便在引发异常时处理错误。

当异步方法调用其他异步方法时，所有引发异常的异步方法都将被传播到外部方法。 这意味着你可以将一个 **try/catch** 块放在最外层的方法中，以便为嵌套异步方法捕获错误。 这同样与你为同步方法捕获异常的方式类似。 但不能在 **catch** 块中使用 **await**。

**提示**从 Microsoft Visual Studio2005 中的 C# 开始，你可以使用**await** **catch**块中。

## <a name="summary-and-next-steps"></a>摘要和后续步骤

下面我们介绍的调用异步方法的模式就是在事件处理程序中调用异步 API 时使用的最简单方法。 当在返回 **void** 或 Visual Basic 中的 **Sub** 的替代方法中调用异步方法时，也可以使用此模式。

当遇到 UWP 中的异步方法时，需要记住以下几点：

-   按照惯例，异步方法的名称应以“Async”结尾。
-   所有使用 **await** 运算符的方法都必须使用 **async** 关键字对其声明进行标记。
-   当应用查找 **await** 运算符时，应用仍然响应用户交互，并且同时执行异步方法。
-   等待由异步方法返回的值时将返回一个包含结果的对象。 多数情况下，包含在返回值内的结果（而非返回值本身）将非常有用。 可通过查看异步方法的返回类型，找到包含在结果内的值的类型。
-   使用异步 API 和 **async** 模式通常是改进应用响应的一种方法。

本主题中的示例所输出的文本如下所示：

``` syntax
Windows Experience Blog
PC Snapshot: Sony VAIO Y, 8/9/2011 10:26:56 AM -07:00
Tech Tuesday Live Twitter #Chat: Too Much Tech #win7tech, 8/8/2011 12:48:26 PM -07:00
Windows 7 themes: what’s new and what’s popular!, 8/4/2011 11:56:28 AM -07:00
PC Snapshot: Toshiba Satellite A665 3D, 8/2/2011 8:59:15 AM -07:00
Time for new school supplies? Find back-to-school deals on Windows 7 PCs and Office 2010, 8/1/2011 2:14:40 PM -07:00
Best PCs for blogging (or working) on the go, 8/1/2011 10:08:14 AM -07:00
Tech Tuesday – Blogging Tips and Tricks–#win7tech, 8/1/2011 9:35:54 AM -07:00
PC Snapshot: Lenovo IdeaPad U460, 7/29/2011 9:23:05 AM -07:00
GIVEAWAY: Survive BlogHer with a Sony VAIO SA and a Samsung Focus, 7/28/2011 7:27:14 AM -07:00
3 Ways to Stay Cool This Summer, 7/26/2011 4:58:23 PM -07:00
Getting RAW support in Photo Gallery & Windows 7 (…and a contest!), 7/26/2011 10:40:51 AM -07:00
Tech Tuesdays Live Twitter Chats: Photography Tips, Tricks and Essentials, 7/25/2011 12:33:06 PM -07:00
3 Tips to Go Green With Your PC, 7/22/2011 9:19:43 AM -07:00
How to: Buy a Green PC, 7/22/2011 9:13:22 AM -07:00
Windows 7 themes: the distinctive artwork of Cheng Ling, 7/20/2011 9:53:07 AM -07:00
```
