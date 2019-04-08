---
title: 使用 C# 和 Visual Basic 创建 Windows 运行时组件
description: 从 .NET Framework 4.5 开始，你可以使用托管代码创建自己的在 Windows 运行时组件中打包的 Windows 运行时类型。
ms.assetid: A5672966-74DF-40AB-B01E-01E3FCD0AD7A
ms.date: 12/04/2018
ms.topic: article
dev_langs:
- csharp
- vb
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5a7f2d2db5670b0102f589fcd6d764a239d3bb3f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57619962"
---
# <a name="creating-windows-runtime-components-in-c-and-visual-basic"></a>使用 C# 和 Visual Basic 创建 Windows 运行时组件
从.NET Framework 4.5 开始，你可以使用托管的代码来创建你自己的 Windows 运行时类型并将其打包在 Windows 运行时组件。 可以在通用 Windows 平台 (UWP) 应用中编写的 c + +、 JavaScript、 Visual Basic 中，使用你的组件或C#。 本主题概述了用于创建组件的规则，并讨论了有关 Windows 运行时的 .NET Framework 支持的一些方面。 一般情况下，该支持设计为对 .NET Framework 程序员透明可见。 但是，在你创建要与 JavaScript 或 C++ 一起使用的组件时，需要意识到这些语言支持 Windows 运行时的方法差异。

如果要创建仅在用 Visual Basic 编写的 UWP 应用中使用的组件或C#，并且该组件不包含 UWP 控件，则考虑使用**类库**模板而不是**Windows运行时组件**Microsoft Visual Studio 中的项目模板。 简单类库所受限制较少。

## <a name="declaring-types-in-windows-runtime-components"></a>声明 Windows 运行时组件中的类型

在内部，组件中的 Windows 运行时类型可以使用 UWP 应用中允许使用任何.NET Framework 功能。 有关详细信息，请参阅[适用于 UWP 应用的.NET](https://msdn.microsoft.com/library/windows/apps/mt185501)。

外部，类型成员可以公开 Windows 运行时类型只能作为其参数和返回值。 以下列表介绍从 Windows 运行时组件公开的.NET Framework 类型的限制。

- 组件中的所有公共类型和成员的字段、参数和返回值必须是 Windows 运行时类型。 此限制包括由 Windows 运行时本身提供的类型以及你创作的 Windows 运行时类型。 它还包括许多 .NET Framework 类型。 这些类型包括进来属于.NET Framework 提供的 Windows 运行时在托管代码中自然地使用的支持&mdash;你的代码会使用熟悉的.NET Framework 类型而不是基础的 Windows 运行时类型。 例如，如使用.NET Framework 基元类型**Int32**并**Double**，某些基本类型，如**DateTimeOffset**和**Uri**，以及一些常用泛型接口类型如**IEnumerable&lt;T&gt;**  (IEnumerable (Of T) 在 Visual Basic 中的) 和**IDictionary&lt;TKey，TValue&gt;**。 请注意，以下泛型类型的类型参数必须是 Windows 运行时类型。 部分中对此进行讨论[Windows 运行时类型传递给托管代码](#passing-windows-runtime-types-to-managed-code)并[托管类型传递给 Windows 运行时](#passing-managed-types-to-the-windows-runtime)，本主题中更高版本。

- 公共类和接口可以包含方法、属性和事件。 可以为您的事件声明委托或使用**EventHandler&lt;T&gt;** 委托。 公共类或接口不能：
    - 具有泛型性。
    - 实现不是 Windows 运行时接口的接口 （但是，可以创建自己的 Windows 运行时接口并实现它们）。
    - 派生自类型不是在 Windows 运行时，如**System.Exception**并**System.EventArgs**。

- 所有公共类型必须具有匹配程序集名的根命名空间，并且程序集名不得以“Windows”开头。

    > **提示**。 默认情况下，Visual Studio 项目具有与程序集名称匹配的命名空间名称。 在 Visual Basic 中，此默认命名空间的命名空间声明不会显示在代码中。

- 公共结构无法具有公共字段以外的任何成员，并且这些字段必须是值类型或字符串。
- 公共类必须是 **sealed**（在 Visual Basic 中是 **NotInheritable**）。 如果您的编程模型要求多形性，然后可以创建一个公共接口，并必须是多态类中实现该接口。

## <a name="debugging-your-component"></a>调试组件

如果使用托管代码构建 UWP 应用和组件，然后您可以调试这两个在同一时间。

若要测试你的组件作为使用 c + + 为 UWP 应用的一部分，您可以同时调试托管和本机代码。 默认情况下仅调试本机代码。

## <a name="to-debug-both-native-c-code-and-managed-code"></a>调试本机 C++ 代码和托管代码
1.  打开 Visual C++ 项目的快捷菜单，并选择“属性”。
2.  在属性页面的“配置属性”下，选择“调试”。
3.  选择“调试器类型”，并在下拉列表框中将“仅限本机”更改为“混合(托管和本机)”。 选择“确定”。
4.  在本机代码和托管代码中设置断点。

若要使用 JavaScript 的 UWP 应用的一部分来测试你的组件，默认情况下该解决方案是 JavaScript 中的调试模式。 在 Visual Studio 中，无法同时调试 JavaScript 和托管代码。

## <a name="to-debug-managed-code-instead-of-javascript"></a>调试托管代码而非 JavaScript
1.  打开 JavaScript 项目的快捷菜单，并选择“属性”。
2.  在属性页面的“配置属性”下，选择“调试”。
3.  选择“调试器类型”，并在下拉列表框中将“仅限脚本”更改为“仅限托管”。 选择“确定”。
4.  在托管代码中设置断点，并像往常一样调试。

## <a name="passing-windows-runtime-types-to-managed-code"></a>将 Windows 运行时类型传递到托管代码
正如前面提到的部分中[Windows 运行时组件中声明类型](#declaring-types-in-windows-runtime-components)，某些.NET Framework 类型可以出现在公共类的成员的签名。 这是支持 .NET Framework 在托管代码中自然使用 Windows 运行时的一部分。 它包含基元类型以及某些类和接口。 从 JavaScript 或 c + + 代码中使用组件，时，必须知道如何向调用方显示.NET Framework 类型。 请参阅[演练：创建一个简单的组件在C#或 Visual Basic 和从 JavaScript 中调用](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)使用 JavaScript 的示例。 本部分讨论常用类型。

在.NET Framework 中，基元类型如**Int32**结构具有许多有用的属性和方法，例如**TryParse**方法。 相比之下，Windows 运行时中的基元类型和结构则仅拥有字段。 在将这些类型传递到托管代码时，它们显示为 .NET Framework 类型，并且你可以像往常一样使用 .NET Framework 类型的属性和方法。 下表总结了在 IDE 中自动进行的替换：

-   对 Windows 运行时基元**Int32**， **Int64**，**单一**， **Double**，**布尔**， **字符串**（Unicode 字符的不可变集合），**枚举**， **UInt32**， **UInt64**，和**Guid**，在 System 命名空间中使用相同名称的类型。
-   有关**UInt8**，使用**System.Byte**。
-   有关**Char16**，使用**System.Char**。
-   有关**IInspectable**界面，请使用**System.Object**。

如果C#或 Visual Basic 语言关键字提供任何这些类型，则可以改为使用语言关键字。

除基元类型外，一些基本的常用 Windows 运行时类型作为其 .NET Framework 等效项显示在托管代码中。 例如，假设你的 JavaScript 代码使用**Windows.Foundation.Uri**类，并且你想要将其传递到C#或 Visual Basic 方法。 在托管代码中的等效类型是.NET Framework **System.Uri**类，并且这是用于方法参数类型。 你可以在 Windows 运行时类型显示为 .NET Framework 类型时进行区分，因为 Visual Studio 中的 IntelliSense 在编写托管代码时会隐藏 Windows 运行时类型并显示等效的 .NET Framework 类型。 （通常，这两种类型具有相同名称。 但请注意， **Windows.Foundation.DateTime**结构在托管代码中显示**System.DateTimeOffset**而不是作为**System.DateTime**。)

对于一些常用的集合类型，映射介于由 Windows 运行时类型实现的接口和由相应的 .NET Framework 类型实现的接口之间。 与上述类型一样，你使用 .NET Framework 类型声明参数类型。 这会隐藏类型之间的一些差异，并使编写 .NET Framework 代码变得更加自然。

下表列出了最常见的泛型接口类型以及其他常见的类和接口映射。 .NET Framework 映射的 Windows 运行时类型的完整列表，请参阅[Windows 运行时类型的.NET Framework 映射](net-framework-mappings-of-windows-runtime-types.md)。

| Windows 运行时                                  | .NET Framework                                    |
|-|-|
| IIterable&lt;T&gt;                               | IEnumerable&lt;T&gt;                              |
| IVector&lt;T&gt;                                 | IList&lt;T&gt;                                    |
| IVectorView&lt;T&gt;                             | IReadOnlyList&lt;T&gt;                            |
| IMap&lt;K，V&gt;                                 | IDictionary&lt;TKey，TValue&gt;                   |
| IMapView&lt;K，V&gt;                             | IReadOnlyDictionary&lt;TKey，TValue&gt;           |
| 2&gt;platform::collections::inputiterator&lt;ikeyvaluepair&lt;k&lt;K，V&gt;                        | KeyValuePair&lt;TKey，TValue&gt;                  |
| IBindableIterable                                | IEnumerable                                       |
| IBindableVector                                  | IList                                             |
| Windows.UI.Xaml.Data.INotifyPropertyChanged      | System.ComponentModel.INotifyPropertyChanged      |
| Windows.UI.Xaml.Data.PropertyChangedEventHandler | System.ComponentModel.PropertyChangedEventHandler |
| Windows.UI.Xaml.Data.PropertyChangedEventArgs    | System.ComponentModel.PropertyChangedEventArgs    |

在某种类型实现多个接口时，你可以将所实现的任意接口用作成员的参数类型或返回类型。 例如，您可以传递或返回**字典&lt;int，字符串&gt;** (**Dictionary （Of Integer，字符串）** 在 Visual Basic 中) 作为**IDictionary&lt;int，字符串&gt;**， **IReadOnlyDictionary&lt;int，字符串&gt;**，或**IEnumerable&lt;System.Collections.Generic.KeyValuePair&lt;TKey，TValue&gt;&gt;**。

> [!IMPORTANT]
> JavaScript 使用托管的类型实现的接口列表中第一个出现的接口。 例如，如果您返回**字典&lt;int，字符串&gt;** 向 JavaScript 代码，它显示为**IDictionary&lt;int，字符串&gt;** 无论所指定为返回类型的接口。 这意味着，如果第一个接口不包括显示在后续接口上的成员，JavaScript 将看不到该成员。

在 Windows 运行时， **IMap&lt;K，V&gt;** 并**IMapView&lt;K，V&gt;** 来使用 2&gt;platform::collections::inputiterator&lt;ikeyvaluepair&lt;k 循环访问。 时将其传递给托管代码，它们显示为**IDictionary&lt;TKey，TValue&gt;** 并**IReadOnlyDictionary&lt;TKey，TValue&gt;**，因此很自然地使用**System.Collections.Generic.KeyValuePair&lt;TKey，TValue&gt;** 来枚举它们。

接口在托管代码中的显示方式影响实现这些接口的类型的显示方式。 例如， **PropertySet**类实现**IMap&lt;K，V&gt;**，后者在托管代码中显示**IDictionary&lt;TKey，TValue&gt;**. **PropertySet**将显示为已实现**IDictionary&lt;TKey，TValue&gt;** 而不是**IMap&lt;K，V&gt;**，因此在托管它显示为具有的代码**外**方法，其行为类似**添加**于.NET Framework 字典的方法。 它不会显示为具有**插入**方法。 您可以查看此主题中的示例[演练：创建一个简单的组件在C#或 Visual Basic 和从 JavaScript 中调用](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)。

## <a name="passing-managed-types-to-the-windows-runtime"></a>将托管的类型传递到 Windows 运行时

如前面部分中所述，一些 Windows 运行时类型可在组件成员的签名或 Windows 运行时成员的签名中显示为 .NET Framework 类型（如果你在 IDE 中使用这些类型）。 在你将 .NET Framework 类型传递到这些成员或将它们用作组件成员的返回值时，它们将作为相应的 Windows 运行时类型显示为另一侧的代码。 有关从 JavaScript 中调用组件时，这可能产生的影响的示例，请参阅中的"返回托管从您的组件类型"部分[演练：创建一个简单的组件在C#或 Visual Basic 和从 JavaScript 中调用](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)。

## <a name="overloaded-methods"></a>重载的方法

在 Windows 运行时中，可重载方法。 但是，如果要声明多个重载具有相同数量的参数，则必须应用[ **Windows.Foundation.Metadata.DefaultOverloadAttribute** ](/uwp/api/windows.foundation.metadata.defaultoverloadattribute)仅其中一个重载的属性。 该重载是唯一能够通过 JavaScript 调用的重载。 例如，在以下代码中，接受 **int**（在 Visual Basic 中是 **Integer**）的重载是默认重载。

```csharp
public string OverloadExample(string s)
{
    return s;
}

[Windows.Foundation.Metadata.DefaultOverload()]
public int OverloadExample(int x)
{
    return x;
}
```

```vb
Public Function OverloadExample(ByVal s As String) As String
    Return s
End Function

<Windows.Foundation.Metadata.DefaultOverload> _
Public Function OverloadExample(ByVal x As Integer) As Integer
    Return x
End Function
```

> [重要]JavaScript 允许你将传递到的任何值**OverloadExample**，并强制转换为类型所需的参数值。 您可以调用**OverloadExample**四十二""、"42"或 42.3，但所有这些值都会传递给默认重载。 上一示例中的默认重载分别返回 0、 42 和 42。

无法应用**DefaultOverloadAttribut**e 属性与构造函数。 类中的所有构造函数必须具有不同数量的参数。

## <a name="implementing-istringable"></a>实现 IStringable

从 Windows 8.1 开始，Windows 运行时包括**IStringable**接口的单一方法**IStringable.ToString**，提供由相媲美的基本格式设置支持**Object.ToString**。 如果选择实现**IStringable**中的 Windows 运行时组件中导出的公共托管类型，以下限制适用：

-   您可以定义**IStringable**接口只能在"类实现"关系，如下面的代码C#:

    ```cs
    public class NewClass : IStringable
    ```

    或以下 Visual Basic 代码：

    ```vb
    Public Class NewClass : Implements IStringable
    ```

-   不能实现**IStringable**接口上。
-   不能声明为类型参数**IStringable**。
-   **IStringable**不能为方法、 属性或字段的返回类型。
-   您不能隐藏您**IStringable**通过使用如下所示的方法定义在基类中实现：

    ```cs
    public class NewClass : IStringable
    {
       public new string ToString()
       {
          return "New ToString in NewClass";
       }
    }
    ```

    相反， **IStringable.ToString**实现必须一直重写基类实现。 您可以隐藏**ToString**只能通过强类型的类实例上调用它的实现。

> [!NOTE]
> 下不同条件，将调用从本机代码到实现的托管类型**IStringable**或隐藏其**ToString**实现可以产生意外的行为。

## <a name="asynchronous-operations"></a>异步操作

若要在组件中实现异步方法，将"Async"添加到方法名称的末尾并返回一个表示异步操作的 Windows 运行时接口：**IAsyncAction**， **IAsyncActionWithProgress&lt;TProgress&gt;**， **IAsyncOperation&lt;TResult&gt;**，或**IAsyncOperationWithProgress&lt;TResult，TProgress&gt;**。

可以使用.NET Framework 任务 ( [**任务**](/dotnet/api/system.threading.tasks.task)类和泛型[**任务&lt;TResult&gt;**  ](/dotnet/api/system.threading.tasks.task-1)类) 到实现异步方法。 您必须返回表示正在进行的操作，如从编写的异步方法返回的任务的任务C#或 Visual Basic 中或从返回的任务[ **Task.Run** ](/dotnet/api/system.threading.tasks.task.run)方法。 如果你使用构造函数创建任务，必须在返回它之前调用其 [Task.Start](/dotnet/api/system.threading.tasks.task.start) 方法。

使用的方法`await`(`Await`在 Visual Basic 中) 需要`async`关键字 (`Async`在 Visual Basic 中)。 如果公开此类方法从 Windows 运行时组件，将应用`async`关键字的委托传递给**运行**方法。

对于不支持取消和进度报告的异步操作，你可以使用 [WindowsRuntimeSystemExtensions.AsAsyncAction](https://msdn.microsoft.com/library/system.windowsruntimesystemextensions.asasyncaction.aspx) 或 [AsAsyncOperation&lt;TResult&gt;](https://msdn.microsoft.com/library/hh779745.aspx) 扩展方法来在相应的接口中打包任务。 例如，下面的代码实现异步方法通过使用**Task.Run&lt;TResult&gt;** 方法来启动任务。 **AsAsyncOperation&lt;TResult&gt;** 扩展方法返回作为 Windows 运行时异步操作任务。

```csharp
public static IAsyncOperation<IList<string>> DownloadAsStringsAsync(string id)
{
    return Task.Run<IList<string>>(async () =>
    {
        var data = await DownloadDataAsync(id);
        return ExtractStrings(data);
    }).AsAsyncOperation();
}
```

```vb
Public Shared Function DownloadAsStringsAsync(ByVal id As String) _
     As IAsyncOperation(Of IList(Of String))

    Return Task.Run(Of IList(Of String))(
        Async Function()
            Dim data = Await DownloadDataAsync(id)
            Return ExtractStrings(data)
        End Function).AsAsyncOperation()
End Function
```

下面的 JavaScript 代码演示如何通过使用调用方法[ **WinJS.Promise** ](https://msdn.microsoft.com/library/windows/apps/br211867.aspx)对象。 然后，传递到该方法的函数在完成异步调用时执行。 StringList 参数包含返回的字符串的列表**DownloadAsStringAsync**方法，该函数会执行所需的任何处理。

```javascript
function asyncExample(id) {

    var result = SampleComponent.Example.downloadAsStringAsync(id).then(
        function (stringList) {
            // Place code that uses the returned list of strings here.
        });
}
```

对于异步操作和操作支持取消或进度报告的使用[ **AsyncInfo** ](/dotnet/api/system.runtime.interopservices.windowsruntime)类生成启动的任务并将挂接取消和进度报告该任务的取消和进度报告功能在适当的 Windows 运行时接口的功能。 有关支持取消和进度报告的示例，请参阅[演练：创建一个简单的组件在C#或 Visual Basic 和从 JavaScript 中调用](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)。

请注意，您可以使用的方法**AsyncInfo**类即使异步方法不支持取消或进度报告。 如果使用 Visual Basic lambda 函数或C#匿名方法中，不提供该令牌的参数和[ **IProgress&lt;T&gt;**  ](https://msdn.microsoft.com/library/hh138298.aspx)接口。 如果你使用 C# lambda 函数，则提供令牌参数，但忽略它。 上述示例中，使用 AsAsyncOperation&lt;TResult&gt;方法，如下所示，使用时[ **AsyncInfo.Run&lt;TResult&gt;(Func&lt;CancellationToken，任务&lt;TResult&gt;&gt;**](https://msdn.microsoft.com/library/hh779740.aspx)) 方法重载。

```csharp
public static IAsyncOperation<IList<string>> DownloadAsStringsAsync(string id)
{
    return AsyncInfo.Run<IList<string>>(async (token) =>
    {
        var data = await DownloadDataAsync(id);
        return ExtractStrings(data);
    });
}
```

```vb
Public Shared Function DownloadAsStringsAsync(ByVal id As String) _
    As IAsyncOperation(Of IList(Of String))

    Return AsyncInfo.Run(Of IList(Of String))(
        Async Function()
            Dim data = Await DownloadDataAsync(id)
            Return ExtractStrings(data)
        End Function)
End Function
```

如果您创建可以选择支持取消或进度报告的异步方法，请考虑添加没有取消令牌的参数的重载或**IProgress&lt;T&gt;** 接口。

## <a name="throwing-exceptions"></a>引发异常

你可以引发任何包括在适用于 Windows 应用的 .NET 中的异常类型。 你无法在 Windows 运行时组件中声明自己的公共异常类型，但可以声明并引发非公共类型。

如果组件不处理异常，将在调用组件的代码中引发相应异常。 向调用方显示异常的方式取决于调用语言支持 Windows 运行时的方式。

-   在 JavaScript 中，异常显示为堆栈跟踪替换异常消息的对象。 在 Visual Studio 中调试应用时，你可以看到显示在调试器异常对话框中标识为“WinRT 信息”的原始消息文本。 无法从 JavaScript 代码访问原始消息文本。

    > **提示**。 目前，堆栈跟踪包含托管的异常类型，但我们不建议分析跟踪来标识异常类型。 改为使用 HRESULT 值，如本部分后面所述。

-   在 C++ 中，异常显示为平台异常。 如果托管的异常的 HResult 属性可以映射到特定平台异常的 HRESULT，则使用特定异常;否则为[ **platform:: comexception** ](https://msdn.microsoft.com/library/windows/apps/xaml/hh710414.aspx)引发异常。 托管异常的消息文本不适用于 C++ 代码。 如果已引发特定平台异常，将显示该异常类型的默认消息文本；否则，将不显示任何消息文本。 请参阅[异常 (C++/CX)](https://msdn.microsoft.com/library/windows/apps/xaml/hh699896.aspx)。
-   在 C# 或 Visual Basic 中，异常是正常的托管异常。

在从组件引发异常时，你可以使 JavaScript 或 C++ 调用方处理异常变得更简单，方法是引发其 HResult 属性值特定于你的组件的非公共异常类型。 HRESULT 可供 JavaScript 调用方通过异常对象的 number 属性，以及通过 c + + 调用方[ **comexception:: Hresult** ](https://msdn.microsoft.com/library/windows/apps/xaml/hh710415.aspx)属性。

> [!NOTE]
> 使用你 HRESULT 值为负。 正值解释为成功，并且在 JavaScript 或 C++ 调用方中没有引发任何异常。

## <a name="declaring-and-raising-events"></a>声明和引发事件

在你声明类型以保留事件数据时，请从 Object 而非 EventArgs 派生，因为 EventArgs 不属于 Windows 运行时类型。 使用[ **EventHandler&lt;TEventArgs&gt;**  ](https://msdn.microsoft.com/library/db0etb8x.aspx)作为事件，并使用事件自变量类型作为泛型类型参数的类型。 就像在 .NET Framework 应用程序中一样引发该事件。

在通过 JavaScript 或 C++ 使用 Windows 运行时组件时，事件遵循这些语言期望的 Windows 运行时事件模式。 在通过 C# 或 Visual Basic 使用组件时，事件显示为普通的 .NET Framework 事件。 中提供了一个示例[演练：创建一个简单的组件在C#或 Visual Basic 和从 JavaScript 中调用](/windows/uwp/winrt-components/walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript)。

如果要实现自定义事件访问器（在 Visual Basic 中通过 **Custom** 关键字声明事件），必须在实现中遵循 Windows 运行时事件模式。 请参阅 [Windows 运行时组件中的自定义事件和事件访问器](custom-events-and-event-accessors-in-windows-runtime-components.md)。 请注意，在通过 C# 或 Visual Basic 代码处理事件时，它仍显示为普通的 .NET Framework 事件。

## <a name="next-steps"></a>后续步骤

在创建了 Windows 运行时组件以供自己使用后，你可能会发现它封装的功能对其他开发人员也很有用。 若要打包组件以分配给其他开发人员，你有两个选择。 请参阅[分配托管的 Windows 运行时组件](https://msdn.microsoft.com/library/jj614475.aspx)。

有关 Visual Basic 和 C# 语言功能以及 Windows 运行时的 .NET Framework 支持的详细信息，请参阅 [Visual Basic 和 C# 语言参考](https://msdn.microsoft.com/library/windows/apps/xaml/br212458.aspx)。

## <a name="related-topics"></a>相关主题
* [适用于 UWP 应用的.NET](https://msdn.microsoft.com/library/windows/apps/mt185501)
* [演练：创建简单的 Windows 运行时组件并从 JavaScript 中调用它](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)
