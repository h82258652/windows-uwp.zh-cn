---
author: msatranjr
title: 使用 C# 和 Visual Basic 创建 Windows 运行时组件
description: 从 .NET Framework 4.5 开始，你可以使用托管代码创建自己的在 Windows 运行时组件中打包的 Windows 运行时类型。
ms.assetid: A5672966-74DF-40AB-B01E-01E3FCD0AD7A
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4e3b9ed2d256fb9ea8d38690a703baf7fbd3e7f0
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/09/2018
ms.locfileid: "6191504"
---
# <a name="creating-windows-runtime-components-in-c-and-visual-basic"></a>使用 C# 和 Visual Basic 创建 Windows 运行时组件
从 .NET Framework 4.5 开始，你可以使用托管代码创建自己的在 Windows 运行时组件中打包的 Windows 运行时类型。 你可以将通用 Windows 平台 (UWP) 应用与 C++、JavaScript、Visual Basic 或 C# 一起使用。 本主题概述了用于创建的组件，规则，并讨论了.NET Framework 支持的 Windows 运行时中的某些方面。 一般情况下，该支持设计为对 .NET Framework 程序员透明可见。 但是，在你创建要与 JavaScript 或 C++ 一起使用的组件时，需要意识到这些语言支持 Windows 运行时的方法差异。

如果你要创建仅在 UWP 应用中与 Visual Basic 或 C# 一起使用的组件，并且该组件不包含 UWP 控件，请考虑使用“类库”**** 模板而非“Windows 运行时组件”**** 模板。 简单类库所受限制较少。

本主题包含以下部分：

## <a name="declaring-types-in-windows-runtime-components"></a>声明 Windows 运行时组件中的类型
在内部，组件中的 Windows 运行时类型可以使用任何通用 Windows 应用中允许的 .NET Framework 功能。 （有关详细信息，请参阅[适用于 UWP 应用的 .NET](https://msdn.microsoft.com/library/windows/apps/xaml/mt185501.aspx)。）在外部，类型成员仅可以为其参数公开 Windows 运行时类型并返回值。 下表介绍 Windows 运行时组件公开的 .NET Framework 类型的限制。

-   组件中的所有公共类型和成员的字段、参数和返回值必须是 Windows 运行时类型。

    此限制包括你创建的 Windows 运行时类型以及 Windows 运行时本身提供的类型。 它还包括许多 .NET Framework 类型。 包括这些类型是支持 .NET Framework 在托管代码中自然使用 Windows 运行时的一部分：你的代码看起来像使用熟悉的 .NET Framework 类型，而非基本的 Windows 运行时类型。 例如，你可以使用 .NET Framework 基元类型（例如 Int32 和双精度型）、某些基本类型（例如 DateTimeOffset 和 Uri）以及一些常用的泛型接口类型（例如 IEnumerable&lt;T&gt;，这在 Visual Basic 中是 IEnumerable(Of T) 和 IDictionary&lt;TKey,TValue&gt;）。 （请注意，这些泛型类型的类型参数必须是 Windows 运行时类型）。这 Windows 运行时类型传递到托管代码的部分中所述，并将托管的传递到 Windows 运行时，在本主题后面的类型。

-   公共类和接口可以包含方法、属性和事件。 你可以为事件声明委托或使用 EventHandler&lt;T&gt; 委托。 公共类或接口无法：

    -   具有泛型性。
    -   实现不是 Windows 运行时接口的接口。 （但是，你可以创建并实现自己的 Windows 运行时接口。）
    -   从 Windows 运行时之外的类型（例如 System.Exception 和 System.EventArgs）进行派生。
-   所有公共类型必须具有匹配程序集名的根命名空间，并且程序集名不得以“Windows”开头。

    > **提示**默认情况下，Visual Studio 项目具有匹配程序集名的命名空间名称。 在 Visual Basic 中，此默认命名空间的命名空间声明不会显示在代码中。

-   公共结构无法具有公共字段以外的任何成员，并且这些字段必须是值类型或字符串。
-   公共类必须是 **sealed**（在 Visual Basic 中是 **NotInheritable**）。 如果编程模型要求具有多形性，你可以创建公共接口，并且在必须为多形性的类上实现该接口。

## <a name="debugging-your-component"></a>调试组件
如果通用 Windows 应用和组件均通过托管代码生成，你可以同时调试它们。

如果要作为使用 C++ 的通用 Windows 应用的一部分测试你的组件，你可以同时调试托管代码和本机代码。 默认情况下仅调试本机代码。

## **<a name="to-debug-both-native-c-code-and-managed-code"></a>调试本机 C++ 代码和托管代码**
1.  打开 Visual C++ 项目的快捷菜单，并选择“属性”****。
2.  在属性页面的“配置属性”**** 下，选择“调试”****。
3.  选择“调试器类型”****，并在下拉列表框中将“仅限本机”**** 更改为“混合(托管和本机)”****。 选择“确定”****。
4.  在本机代码和托管代码中设置断点。

如果要作为使用 JavaScript 的通用 Windows 应用的一部分测试你的组件，解决方案默认处于 JavaScript 调试模式下。 在 Visual Studio 中，无法同时调试 JavaScript 和托管代码。

## **<a name="to-debug-managed-code-instead-of-javascript"></a>调试托管代码而非 JavaScript**
1.  打开 JavaScript 项目的快捷菜单，并选择“属性”****。
2.  在属性页面的“配置属性”**** 下，选择“调试”****。
3.  选择“调试器类型”****，并在下拉列表框中将“仅限脚本”**** 更改为“仅限托管”****。 选择“确定”****。
4.  在托管代码中设置断点，并像往常一样调试。

## <a name="passing-windows-runtime-types-to-managed-code"></a>将 Windows 运行时类型传递到托管代码
如之前在“声明 Windows 运行时组件中的类型”部分中所述，某些 .NET Framework 类型可以显示在公共类成员的签名中。 这是支持 .NET Framework 在托管代码中自然使用 Windows 运行时的一部分。 它包含基元类型以及某些类和接口。 在通过 JavaScript 或 C++ 代码使用组件时，请务必知晓向调用方显示 .NET Framework 类型的方式。 有关使用 JavaScript 的示例，请参阅[演练：使用 C# 或 Visual Basic 创建简单组件并通过 JavaScript 调用它](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)。 本部分讨论常用类型。

在 .NET Framework 中，基元类型（例如 Int32 结构）具有许多有用的属性和方法，例如 TryParse 方法。 相比之下，Windows 运行时中的基元类型和结构则仅拥有字段。 在将这些类型传递到托管代码时，它们显示为 .NET Framework 类型，并且你可以像往常一样使用 .NET Framework 类型的属性和方法。 下表总结了在 IDE 中自动进行的替换：

-   对于 Windows 运行时基元 Int32、Int64、单精度型、双精度型、布尔型、字符串（不可变的 Unicode 字符集合）、Enum、UInt32、UInt64 和 Guid，请在系统命名空间中使用相同名称的类型。
-   对于 UInt8，请使用 System.Byte。
-   对于 Char16，请使用 System.Char。
-   对于 IInspectable 接口，请使用 System.Object。

如果 C# 或 Visual Basic 为任意一种类型提供语言关键字，你可以改为使用语言关键字。

除基元类型外，一些基本的常用 Windows 运行时类型作为其 .NET Framework 等效项显示在托管代码中。 例如，假设 JavaScript 代码使用 Windows.Foundation.Uri 类，并且你希望将其传递到 C# 或 Visual Basic 方法。 托管代码中的等效类型是 .NET Framework System.Uri 类，并且这是用于方法参数的类型。 你可以在 Windows 运行时类型显示为 .NET Framework 类型时进行区分，因为 Visual Studio 中的 IntelliSense 在编写托管代码时会隐藏 Windows 运行时类型并显示等效的 .NET Framework 类型。 （通常，这两种类型具有相同名称。 但请注意，Windows.Foundation.DateTime 结构在托管代码中显示为 System.DateTimeOffset，而非 System.DateTime。）

对于一些常用的集合类型，映射介于由 Windows 运行时类型实现的接口和由相应的 .NET Framework 类型实现的接口之间。 与上述类型一样，你使用 .NET Framework 类型声明参数类型。 这会隐藏类型之间的一些差异，并使编写 .NET Framework 代码变得更加自然。 下表列出了最常见的泛型接口类型以及其他常见的类和接口映射。 有关 .NET Framework 映射的 Windows 运行时类型的完整列表，请参阅 Windows 运行时类型的 .NET Framework 映射。

| Windows 运行时                                  | .NET Framework                                    |
|--------------------------------------------------|---------------------------------------------------|
| IIterable&lt;T&gt;                               | IEnumerable&lt;T&gt;                              |
| IVector&lt;T&gt;                                 | IList&lt;T&gt;                                    |
| IVectorView&lt;T&gt;                             | IReadOnlyList&lt;T&gt;                            |
| IMap&lt;K，V&gt;                                 | IDictionary&lt;TKey，TValue&gt;                   |
| IMapView&lt;K，V&gt;                             | IReadOnlyDictionary&lt;TKey，TValue&gt;           |
| IKeyValuePair&lt;K，V&gt;                        | KeyValuePair&lt;TKey，TValue&gt;                  |
| IBindableIterable                                | IEnumerable                                       |
| IBindableVector                                  | IList                                             |
| Windows.UI.Xaml.Data.INotifyPropertyChanged      | System.ComponentModel.INotifyPropertyChanged      |
| Windows.UI.Xaml.Data.PropertyChangedEventHandler | System.ComponentModel.PropertyChangedEventHandler |
| Windows.UI.Xaml.Data.PropertyChangedEventArgs    | System.ComponentModel.PropertyChangedEventArgs    |

在某种类型实现多个接口时，你可以将所实现的任意接口用作成员的参数类型或返回类型。 例如，你可以将传递或返回一个字典&lt;int，string&gt; (Dictionary （Of Integer，String) 在 Visual Basic) 为 IDictionary&lt;int，string&gt;、 IReadOnlyDictionary&lt;int，string&gt;，或 IEnumerable&lt;System.Collections.Generic.KeyValuePair&lt;TKey，TValue&gt;&gt;。

**重要提示**JavaScript 使用最先显示在托管的类型实现的接口列表中的接口。 例如，如果你将 Dictionary&lt;int, string&gt; 返回到 JavaScript 代码，它会显示为 IDictionary&lt;int, string&gt;，无论你指定哪个接口作为返回类型都是如此。 这意味着，如果第一个接口不包括显示在后续接口上的成员，JavaScript 将看不到该成员。

在 Windows 运行时中，IMap&lt;K, V&gt; 和 IMapView&lt;K, V&gt; 使用 IKeyValuePair 进行迭代。 在将它们传递到托管代码时，它们显示为 IDictionary&lt;TKey, TValue&gt; 和 IReadOnlyDictionary&lt;TKey, TValue&gt;，所以自然而然地，你会使用 System.Collections.Generic.KeyValuePair&lt;TKey, TValue&gt; 枚举它们。

接口在托管代码中的显示方式影响实现这些接口的类型的显示方式。 例如，PropertySet 类实现 IMap&lt;K, V&gt;，而它在托管代码中显示为 IDictionary&lt;TKey, TValue&gt;。 PropertySet 显示为好像实现了 IDictionary&lt;TKey, TValue&gt; 而非 IMap&lt;K, V&gt;，所以在托管代码中，它似乎具有 Add 方法，其行为类似于 .NET Framework 字典上的 Add 方法。 它不会显示为具有 Insert 方法。 你可以查看本主题中的示例[演练： 采用 C# 或 Visual Basic 创建简单组件并通过 JavaScript 调用它](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)。

## <a name="passing-managed-types-to-the-windows-runtime"></a>将托管的类型传递到 Windows 运行时
如前面部分中所述，一些 Windows 运行时类型可在组件成员的签名或 Windows 运行时成员的签名中显示为 .NET Framework 类型（如果你在 IDE 中使用这些类型）。 在你将 .NET Framework 类型传递到这些成员或将它们用作组件成员的返回值时，它们将作为相应的 Windows 运行时类型显示为另一侧的代码。 有关此操作在通过 JavaScript 调用组件时产生的影响的示例，请参阅[演练：使用 C# 或 Visual Basic 创建简单组件并通过 JavaScript 调用它](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)中的“从组件返回托管的类型”部分。

## <a name="overloaded-methods"></a>重载的方法
在 Windows 运行时中，可重载方法。 但是，如果使用相同数量的参数声明多个重载，你必须将 [Windows.Foundation.Metadata.DefaultOverloadAttribute](https://msdn.microsoft.com/library/windows/apps/windows.foundation.metadata.defaultoverloadattribute.aspx) 属性仅应用到其中一个重载。 该重载是唯一能够通过 JavaScript 调用的重载。 例如，在以下代码中，接受 **int**（在 Visual Basic 中是 **Integer**）的重载是默认重载。

> [!div class="tabbedCodeSnippets"]
> ```csharp
> public string OverloadExample(string s)
> {
>     return s;
> }
> [Windows.Foundation.Metadata.DefaultOverload()]
> public int OverloadExample(int x)
> {
>     return x;
> }
> ```
> ```vb
> Public Function OverloadExample(ByVal s As String) As String
>     Return s
> End Function
> <Windows.Foundation.Metadata.DefaultOverload> _
> Public Function OverloadExample(ByVal x As Integer) As Integer
>     Return x
> End Function
> ```

 **警告**JavaScript 允许你将任何值传递到 OverloadExample，并且将该值强制转换为参数所需的类型。 你可以通过“四十二”、“42”或“42.3”调用 OverloadExample，但所有这些值均会传递到默认重载。 之前示例中的默认重载分别返回 0、42 和 42。

你无法将 DefaultOverloadAttribute 属性应用到构造函数。 类中的所有构造函数必须具有不同数量的参数。

## <a name="implementing-istringable"></a>实现 IStringable
从 Windows 8.1 开始，Windows 运行时包括 IStringable 接口，该接口的一项方法 IStringable.ToString 提供的基本格式支持可与 Object.ToString 提供的格式支持相媲美。 如果你确实选择在 Windows 运行时组件中导出的公共托管类型中实现 IStringable，将会应用以下限制：

-   仅可以在“类实现”关系中定义 IStringable 接口，例如以下使用 C# 编写的代码：

    ```cs
    public class NewClass : IStringable
    ```

    或以下 Visual Basic 代码：

    ```vb
    Public Class NewClass : Implements IStringable
    ```

-   不能在接口上实现 IStringable。
-   不能将参数声明为属于类 IStringable。
-   IStringable 不能是方法、属性或字段的返回类型。
-   不能使用如下方法定义从基类隐藏 IStringable 实现：

    ```cs
    public class NewClass : IStringable
    {
       public new string ToString()
       {
          return "New ToString in NewClass";
       }
    }
    ```

    相反，IStringable.ToString 实现必须始终重写基类实现。 你仅可以通过在强类型的类实例上调用 ToString 实现来隐藏它。

请注意，在许多情况下，对实现 IStringable 或隐藏其 ToString 实现的托管类型的本地代码调用会产生意外行为。

## <a name="asynchronous-operations"></a>异步操作
若要在组件中实现异步方法，请将“Async”添加到方法名称的末尾并返回一个表示异步操作的 Windows 运行时接口：IAsyncAction、IAsyncActionWithProgress&lt;TProgress&gt;、IAsyncOperation&lt;TResult&gt; 或 IAsyncOperationWithProgress&lt;TResult, TProgress&gt;。

你可以使用 .NET Framework 任务（[Task](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) 类和泛型 [Task&lt;TResult&gt;](https://msdn.microsoft.com/library/dd321424.aspx) 类）实现异步方法。 必须返回表示操作正在运行的任务，例如从使用 C# 或 Visual Basic 编写的异步方法返回的任务或从 [Task.Run](https://msdn.microsoft.com/library/system.threading.tasks.task.run.aspx) 方法返回的任务。 如果你使用构造函数创建任务，必须在返回它之前调用其 [Task.Start](https://msdn.microsoft.com/library/system.threading.tasks.task.start.aspx) 方法。

使用 await（在 Visual Basic 中是 Await）的方法需要 **async** 关键字（在 Visual Basic 中是 **Async**）。 如果从 Windows 运行时组件公开此类方法，请将 **async** 关键字应用于传递到 Run 方法的委托。

对于不支持取消和进度报告的异步操作，你可以使用 [WindowsRuntimeSystemExtensions.AsAsyncAction](https://msdn.microsoft.com/library/system.windowsruntimesystemextensions.asasyncaction.aspx) 或 [AsAsyncOperation&lt;TResult&gt;](https://msdn.microsoft.com/library/hh779745.aspx) 扩展方法来在相应的接口中打包任务。 例如，以下代码通过使用 Task.Run&lt;TResult&gt; 方法启动任务来实现异步方法。 AsAsyncOperation&lt;TResult&gt; 扩展方法将任务返回为 Windows 运行时异步操作。

> [!div class="tabbedCodeSnippets"]
> ```csharp
> public static IAsyncOperation<IList<string>> DownloadAsStringsAsync(string id)
> {
>     return Task.Run<IList<string>>(async () =>
>     {
>         var data = await DownloadDataAsync(id);
>         return ExtractStrings(data);
>     }).AsAsyncOperation();
> }
> ```
> ```vb
> Public Shared Function DownloadAsStringsAsync(ByVal id As String) _
>      As IAsyncOperation(Of IList(Of String))
>
>     Return Task.Run(Of IList(Of String))(
>         Async Function()
>             Dim data = Await DownloadDataAsync(id)
>             Return ExtractStrings(data)
>         End Function).AsAsyncOperation()
> End Function
> ```

以下 JavaScript 代码介绍如何使用 [WinJS.Promise](https://msdn.microsoft.com/library/windows/apps/br211867.aspx) 对象调用该方法。 然后，传递到该方法的函数在完成异步调用时执行。 stringList 参数包含 DownloadAsStringAsync 方法返回的字符串列表，并且该函数会执行处理所需的任何操作。

```javascript
function asyncExample(id) {

    var result = SampleComponent.Example.downloadAsStringAsync(id).then(
        function (stringList) {
            // Place code that uses the returned list of strings here.
        });
}
```

对于支持取消和进度报告的异步操作，请使用 [AsyncInfo](https://msdn.microsoft.com/library/system.runtime.interopservices.windowsruntime.asyncinfo.aspx) 类生成启动的任务，并将任务的取消和进度报告功能与相应的 Windows 运行时接口的取消和进度报告功能连接起来。 有关支持取消和进度报告的示例，请参阅[演练：使用 C# 或 Visual Basic 创建简单组件并通过 JavaScript 调用它](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)。

请注意，即使异步方法不支持取消或进度报告，也可以使用 AsyncInfo 类的方法。 如果你使用 Visual Basic lambda 函数或 C# 匿名方法，则不要为令牌和 [IProgress&lt;T&gt;](https://msdn.microsoft.com/library/hh138298.aspx) 接口提供参数。 如果你使用 C# lambda 函数，则提供令牌参数，但忽略它。 上一示例中，使用了 AsAsyncOperation&lt;TResult&gt;方法，如下所示，当你使用[AsyncInfo.Run&lt;TResult&gt;(Func&lt;CancellationToken，Task&lt;TResult&gt;](https://msdn.microsoft.com/library/hh779740.aspx)) 方法重载：

> [!div class="tabbedCodeSnippets"]
> ```csharp
> public static IAsyncOperation<IList<string>> DownloadAsStringsAsync(string id)
> {
>     return AsyncInfo.Run<IList<string>>(async (token) =>
>     {
>         var data = await DownloadDataAsync(id);
>         return ExtractStrings(data);
>     });
> }
> ```
> ```vb
> Public Shared Function DownloadAsStringsAsync(ByVal id As String) _
>     As IAsyncOperation(Of IList(Of String))
>
>     Return AsyncInfo.Run(Of IList(Of String))(
>         Async Function()
>             Dim data = Await DownloadDataAsync(id)
>             Return ExtractStrings(data)
>         End Function)
> End Function
> ```

如果你创建可以选择支持取消或进度报告的异步方法，请考虑添加缺少取消令牌或 IProgress&lt;T&gt; 接口的参数的重载。

## <a name="throwing-exceptions"></a>引发异常
你可以引发任何包括在适用于 Windows 应用的 .NET 中的异常类型。 你无法在 Windows 运行时组件中声明自己的公共异常类型，但可以声明并引发非公共类型。

如果组件不处理异常，将在调用组件的代码中引发相应异常。 向调用方显示异常的方式取决于调用语言支持 Windows 运行时的方式。

-   在 JavaScript 中，异常显示为堆栈跟踪替换异常消息的对象。 在 Visual Studio 中调试应用时，你可以看到显示在调试器异常对话框中标识为“WinRT 信息”的原始消息文本。 无法从 JavaScript 代码访问原始消息文本。

    > **提示**目前，堆栈跟踪包含托管的异常类型，但我们不推荐分析跟踪来标识异常类型。 改为使用 HRESULT 值，如本部分后面所述。

-   在 C++ 中，异常显示为平台异常。 如果托管异常的 HResult 属性能够映射到特定平台异常的 HRESULT，将使用该特定异常；否则，将引发 [Platform::COMException](https://msdn.microsoft.com/library/windows/apps/xaml/hh710414.aspx) 异常。 托管异常的消息文本不适用于 C++ 代码。 如果已引发特定平台异常，将显示该异常类型的默认消息文本；否则，将不显示任何消息文本。 请参阅[异常 (C++/CX)](https://msdn.microsoft.com/library/windows/apps/xaml/hh699896.aspx)。
-   在 C# 或 Visual Basic 中，异常是正常的托管异常。

在从组件引发异常时，你可以使 JavaScript 或 C++ 调用方处理异常变得更简单，方法是引发其 HResult 属性值特定于你的组件的非公共异常类型。 HRESULT 通过异常对象的编号属性提供给 JavaScript 调用方，并通过 [COMException::HResult](https://msdn.microsoft.com/library/windows/apps/xaml/hh710415.aspx) 属性提供给 C++ 调用方。

> **请注意**使用负值，为你的 HRESULT。 正值解释为成功，并且在 JavaScript 或 C++ 调用方中没有引发任何异常。

## <a name="declaring-and-raising-events"></a>声明和引发事件
在你声明类型以保留事件数据时，请从 Object 而非 EventArgs 派生，因为 EventArgs 不属于 Windows 运行时类型。 使用 [EventHandler&lt;TEventArgs&gt;](https://msdn.microsoft.com/library/db0etb8x.aspx) 作为事件类型，并将事件参数类型用作泛型类型参数。 就像在 .NET Framework 应用程序中一样引发该事件。

在通过 JavaScript 或 C++ 使用 Windows 运行时组件时，事件遵循这些语言期望的 Windows 运行时事件模式。 在通过 C# 或 Visual Basic 使用组件时，事件显示为普通的 .NET Framework 事件。 [演练：使用 C# 或 Visual Basic 创建简单组件并通过 JavaScript 调用它]()中提供了示例。

如果要实现自定义事件访问器（在 Visual Basic 中通过 **Custom** 关键字声明事件），必须在实现中遵循 Windows 运行时事件模式。 请参阅 [Windows 运行时组件中的自定义事件和事件访问器](custom-events-and-event-accessors-in-windows-runtime-components.md)。 请注意，在通过 C# 或 Visual Basic 代码处理事件时，它仍显示为普通的 .NET Framework 事件。

## <a name="next-steps"></a>后续步骤
在创建了 Windows 运行时组件以供自己使用后，你可能会发现它封装的功能对其他开发人员也很有用。 若要打包组件以分配给其他开发人员，你有两个选择。 请参阅[分配托管的 Windows 运行时组件](https://msdn.microsoft.com/library/jj614475.aspx)。

有关 Visual Basic 和 C# 语言功能以及 Windows 运行时的 .NET Framework 支持的详细信息，请参阅 [Visual Basic 和 C# 语言参考](https://msdn.microsoft.com/library/windows/apps/xaml/br212458.aspx)。

## <a name="related-topics"></a>相关主题
* [适用于.NET UWP 应用概述](https://msdn.microsoft.com/library/windows/apps/xaml/br230302.aspx)
* [适用于 UWP 应用的 .NET](https://msdn.microsoft.com/library/windows/apps/xaml/mt185501.aspx)
* [演练：创建简单的 Windows 运行时组件并通过 JavaScript 调用它](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)
