---
title: 使用 C# 和 Visual Basic 创建 Windows 运行时组件
description: 从 .NET Framework 4.5 开始，你可以使用托管代码创建自己的在 Windows 运行时组件中打包的 Windows 运行时类型。
ms.assetid: A5672966-74DF-40AB-B01E-01E3FCD0AD7A
ms.date: 12/04/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- vb
ms.openlocfilehash: 7dde2fb0411686294ebb8abc17192b2e45c61d7a
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2018
ms.locfileid: "8736492"
---
# <a name="creating-windows-runtime-components-in-c-and-visual-basic"></a>使用 C# 和 Visual Basic 创建 Windows 运行时组件
从.NET Framework 4.5 开始，你可以使用托管的代码来创建你自己的 Windows 运行时类型并将它们打包在 Windows 运行时组件中。 你可以在 c + +、 JavaScript、 Visual Basic 或 C# 编写的通用 Windows 平台 (UWP) 应用中使用你的组件。 本主题概述了用于创建组件，规则，并讨论了.NET Framework 支持的 Windows 运行时的某些方面。 一般情况下，该支持设计为对 .NET Framework 程序员透明可见。 但是，在你创建要与 JavaScript 或 C++ 一起使用的组件时，需要意识到这些语言支持 Windows 运行时的方法差异。

如果你要在 Visual Basic 或 C# 编写的 UWP 应用中仅创建使用组件，并且该组件不包含 UWP 控件，然后 onsider 使用**类库**模板而不**Windows 运行时组件**项目模板在 Microsoft Visual Studio。 简单类库所受限制较少。

## <a name="declaring-types-in-windows-runtime-components"></a>声明 Windows 运行时组件中的类型

在内部，在组件中的 Windows 运行时类型可以使用任何 UWP 应用中允许的.NET Framework 功能。 有关详细信息，请参阅[适用于 UWP 应用的.NET](https://msdn.microsoft.com/library/windows/apps/mt185501)。

在外部，类型成员仅可以为其参数公开 Windows 运行时类型并返回值。 下表介绍在从 Windows 运行时组件公开的.NET Framework 类型上的限制。

- 组件中的所有公共类型和成员的字段、参数和返回值必须是 Windows 运行时类型。 此限制包括由 Windows 运行时本身提供的类型以及创作 Windows 运行时类型。 它还包括许多 .NET Framework 类型。 这些类型包括属于.NET Framework 提供自然使用 Windows 运行时在托管代码中的支持&mdash;你的代码看起来使用熟悉的.NET Framework 类型，而不是基本的 Windows 运行时类型。 例如，你可以使用.NET Framework 基元类型，例如**Int32**和**双精度**、 某些基本类型，例如**DateTimeOffset**和**Uri**，以及一些常用的泛型接口类型，例如**IEnumerable&lt;T&gt; ** (IEnumerable (Of T) 在 Visual Basic) 和**IDictionary&lt;TKey，TValue&gt;**。 请注意，这些泛型类型的类型参数必须是 Windows 运行时类型。 这将在本主题后面的部分[Windows 运行时类型传递到托管代码](#passing-windows-runtime-types-to-managed-code)和[托管的 Windows 运行时类型传递](#passing-managed-types-to-the-windows-runtime)，进行讨论。

- 公共类和接口可以包含方法、属性和事件。 你可以为事件声明委托或使用**EventHandler&lt;T&gt;** 委托。 公共类或接口无法：
    - 具有泛型性。
    - 实现接口的不是 Windows 运行时接口 （但是，可以创建自己的 Windows 运行时接口并实现它们）。
    - 从 Windows 运行时，例如**System.Exception**和**System.EventArgs**中不存在的类型派生。

- 所有公共类型必须具有匹配程序集名的根命名空间，并且程序集名不得以“Windows”开头。

    > **提示**。 默认情况下，Visual Studio 项目具有匹配程序集名的命名空间名称。 在 Visual Basic 中，此默认命名空间的命名空间声明不会显示在代码中。

- 公共结构无法具有公共字段以外的任何成员，并且这些字段必须是值类型或字符串。
- 公共类必须是 **sealed**（在 Visual Basic 中是 **NotInheritable**）。 如果编程模型要求多态性，你可以创建公共接口，并且必须为多形性的类上实现该接口。

## <a name="debugging-your-component"></a>调试组件

如果你的 UWP 应用和组件均通过托管代码生成，然后你可以调试这两个在同一时间。

如果要作为使用 c + + 的 UWP 应用的一部分测试你的组件，你可以同时调试托管代码和本机代码。 默认情况下仅调试本机代码。

## <a name="to-debug-both-native-c-code-and-managed-code"></a>调试本机 C++ 代码和托管代码
1.  打开 Visual C++ 项目的快捷菜单，并选择“属性”****。
2.  在属性页面的“配置属性”**** 下，选择“调试”****。
3.  选择“调试器类型”****，并在下拉列表框中将“仅限本机”**** 更改为“混合(托管和本机)”****。 选择“确定”****。
4.  在本机代码和托管代码中设置断点。

如果要作为使用 JavaScript 的 UWP 应用的一部分测试你的组件，默认情况下的解决方案是在调试模式下的 JavaScript。 在 Visual Studio 中，无法同时调试 JavaScript 和托管代码。

## <a name="to-debug-managed-code-instead-of-javascript"></a>调试托管代码而非 JavaScript
1.  打开 JavaScript 项目的快捷菜单，并选择“属性”****。
2.  在属性页面的“配置属性”**** 下，选择“调试”****。
3.  选择“调试器类型”****，并在下拉列表框中将“仅限脚本”**** 更改为“仅限托管”****。 选择“确定”****。
4.  在托管代码中设置断点，并像往常一样调试。

## <a name="passing-windows-runtime-types-to-managed-code"></a>将 Windows 运行时类型传递到托管代码
如前文所述[Windows 运行时组件中的声明类型](#declaring-types-in-windows-runtime-components)部分，某些.NET Framework 类型可以显示在公共类成员的签名。 这是支持 .NET Framework 在托管代码中自然使用 Windows 运行时的一部分。 它包含基元类型以及某些类和接口。 使用组件时从 JavaScript 或 c + + 代码，务必了解你的.NET Framework 类型向调用方的显示方式。 有关使用 JavaScript 的示例，请参阅[演练：使用 C# 或 Visual Basic 创建简单组件并通过 JavaScript 调用它](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)。 本部分讨论常用类型。

在.NET Framework 中，基元类型，例如**Int32**结构具有许多有用的属性和方法，例如**TryParse**方法。 相比之下，Windows 运行时中的基元类型和结构则仅拥有字段。 在将这些类型传递到托管代码时，它们显示为 .NET Framework 类型，并且你可以像往常一样使用 .NET Framework 类型的属性和方法。 下表总结了在 IDE 中自动进行的替换：

-   Windows 运行时基元**Int32**、 **Int64**、**单个**、**双精度**、**布尔值**、**字符串**（不可变的 Unicode 字符集合）、**枚举**、 **UInt32**、 **UInt64**，以及**Guid**，在系统命名空间中使用相同的名称的类型。
-   对于**UInt8**，请使用**System.Byte**。
-   对于**Char16**，请使用**System.Char**。
-   对于**IInspectable**接口，请使用**System.Object**。

如果 C# 或 Visual Basic 为任意一种类型提供语言关键字，则可以改为使用语言关键字。

除基元类型外，一些基本的常用 Windows 运行时类型作为其 .NET Framework 等效项显示在托管代码中。 例如，假设 JavaScript 代码使用**Windows.Foundation.Uri**类，并且你想要将其传递到 C# 或 Visual Basic 方法。 在托管代码中的等效类型是.NET Framework **System.Uri**类，并且这是用于方法参数的类型。 你可以在 Windows 运行时类型显示为 .NET Framework 类型时进行区分，因为 Visual Studio 中的 IntelliSense 在编写托管代码时会隐藏 Windows 运行时类型并显示等效的 .NET Framework 类型。 （通常，这两种类型具有相同名称。 但是，请注意， **Windows.Foundation.DateTime**结构显示在托管代码为**System.DateTimeOffset** ，而**非 system.datetime**。）

对于一些常用的集合类型，映射介于由 Windows 运行时类型实现的接口和由相应的 .NET Framework 类型实现的接口之间。 与上述类型一样，你使用 .NET Framework 类型声明参数类型。 这会隐藏类型之间的一些差异，并使编写 .NET Framework 代码变得更加自然。

下表列出了最常见的泛型接口类型以及其他常见的类和接口映射。 有关.NET Framework 映射的 Windows 运行时类型的完整列表，请参阅[Windows 运行时类型的.NET Framework 映射](net-framework-mappings-of-windows-runtime-types.md)。

| Windows 运行时                                  | .NET Framework                                    |
|-|-|
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

在某种类型实现多个接口时，你可以将所实现的任意接口用作成员的参数类型或返回类型。 例如，你可以传递或返回**字典&lt;int，string&gt; ** (在 Visual Basic 中是**Dictionary （Of Integer，String）** ) 作为**IDictionary&lt;int，string&gt;**， **IReadOnlyDictionary&lt;int，string&gt; **，或者**IEnumerable&lt;System.Collections.Generic.KeyValuePair&lt;TKey，TValue&gt;**。

> [!IMPORTANT]
> JavaScript 使用最先显示在托管类型实现的接口列表中的接口。 例如，如果你返回**字典&lt;int，string&gt;** 到 JavaScript 代码，它将显示为**IDictionary&lt;int，string&gt;** 无论哪个接口你指定作为返回类型。 这意味着，如果第一个接口不包括显示在后续接口上的成员，JavaScript 将看不到该成员。

在 Windows 运行时中， **IMap&lt;K，V&gt;** 并**IMapView&lt;K，V&gt;** 使用 IKeyValuePair 进行迭代。 当将它们传递到托管代码时，它们显示为**IDictionary&lt;TKey，TValue&gt;** 并**IReadOnlyDictionary&lt;TKey，TValue&gt;**，所以自然而然地使用**System.Collections.Generic.KeyValuePair&lt;TKey，TValue&gt;** 枚举它们。

接口在托管代码中的显示方式影响实现这些接口的类型的显示方式。 例如， **PropertySet**类实现**IMap&lt;K，V&gt;**，它在托管代码中显示**IDictionary&lt;TKey，TValue&gt;**。 **PropertySet**显示为好像实现**IDictionary&lt;TKey，TValue&gt;** 而不是**IMap&lt;K，V&gt;**，所以在托管代码中它似乎具有**Add**方法，其行为类似于**Add**方法在.NET Framework 字典。 它不会显示为具有**Insert**方法。 你可以查看本主题中的示例[演练： 在 C# 或 Visual Basic 创建简单组件并通过 JavaScript 调用它](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)。

## <a name="passing-managed-types-to-the-windows-runtime"></a>将托管的类型传递到 Windows 运行时

如前面部分中所述，一些 Windows 运行时类型可在组件成员的签名或 Windows 运行时成员的签名中显示为 .NET Framework 类型（如果你在 IDE 中使用这些类型）。 在你将 .NET Framework 类型传递到这些成员或将它们用作组件成员的返回值时，它们将作为相应的 Windows 运行时类型显示为另一侧的代码。 有关此操作在通过 JavaScript 调用组件时产生的影响的示例，请参阅[演练：使用 C# 或 Visual Basic 创建简单组件并通过 JavaScript 调用它](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)中的“从组件返回托管的类型”部分。

## <a name="overloaded-methods"></a>重载的方法

在 Windows 运行时中，可重载方法。 但是，如果声明多个具有相同数量的参数的重载，你必须为这些重载之一应用[**修饰**](/uwp/api/windows.foundation.metadata.defaultoverloadattribute)属性。 该重载是唯一能够通过 JavaScript 调用的重载。 例如，在以下代码中，接受 **int**（在 Visual Basic 中是 **Integer**）的重载是默认重载。

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

> [重要]JavaScript 允许你将任何值传递到**OverloadExample**，并且将该值强制转换为参数所需的类型。 你可以调用**OverloadExample**通过"四十二"、"42"42.3，但所有这些值传递到默认重载。 前面的示例中的默认重载分别返回 0、 42 和 42。

你无法将**DefaultOverloadAttribut**e 属性应用到构造函数中。 类中的所有构造函数必须具有不同数量的参数。

## <a name="implementing-istringable"></a>实现 IStringable

从 Windows 8.1 开始，Windows 运行时包括**IStringable**接口，其方法**IStringable.ToString**，可提供由**Object.ToString**提供相媲美的基本格式支持。 如果你选择 Windows 运行时组件中导出的公共托管类型中实现**IStringable** ，将应用以下限制：

-   你可以仅在"类实现"关系，如 C# 中的以下代码中定义**IStringable**接口：

    ```cs
    public class NewClass : IStringable
    ```

    或以下 Visual Basic 代码：

    ```vb
    Public Class NewClass : Implements IStringable
    ```

-   不能在接口上实现**IStringable** 。
-   不能声明为属于类**IStringable**参数。
-   **IStringable**不能是方法、 属性或字段的返回类型。
-   通过使用以下方法定义，不能隐藏**IStringable**实现从基类：

    ```cs
    public class NewClass : IStringable
    {
       public new string ToString()
       {
          return "New ToString in NewClass";
       }
    }
    ```

    相反， **IStringable.ToString**实现必须始终重写基类实现。 你可以仅通过强类型的类实例上调用**ToString**实现来隐藏。

> [!NOTE]
> 在许多情况下，在托管类型实现**IStringable**或隐藏其**ToString**实现从本机代码调用会产生意外的行为。

## <a name="asynchronous-operations"></a>异步操作

若要在组件中实现一种异步方法，将"Async"添加到方法名称的末尾并返回一个表示异步操作的 Windows 运行时接口： **IAsyncAction**、 **IAsyncActionWithProgress&lt;TProgress&gt;**， **IAsyncOperation&lt;TResult&gt;**，或**IAsyncOperationWithProgress&lt;TResult，TProgress&gt;**。

你可以使用.NET Framework 任务 ([**任务**](/dotnet/api/system.threading.tasks.task)类和泛型[**任务&lt;TResult&gt;**](/dotnet/api/system.threading.tasks.task-1)类) 实现异步方法。 你必须返回表示操作正在运行，例如从 C# 或 Visual Basic 编写的异步方法返回的任务或从[**Task.Run**](/dotnet/api/system.threading.tasks.task.run)方法返回的任务的任务。 如果你使用构造函数创建任务，必须在返回它之前调用其 [Task.Start](/dotnet/api/system.threading.tasks.task.start) 方法。

使用的付款方式`await`(`Await`在 Visual Basic) 需要`async`关键字 (`Async`在 Visual Basic 中)。 如果公开 Windows 运行时组件中的此类的方法，应用`async`将传递到**Run**方法的委托的关键字。

对于不支持取消和进度报告的异步操作，你可以使用 [WindowsRuntimeSystemExtensions.AsAsyncAction](https://msdn.microsoft.com/library/system.windowsruntimesystemextensions.asasyncaction.aspx) 或 [AsAsyncOperation&lt;TResult&gt;](https://msdn.microsoft.com/library/hh779745.aspx) 扩展方法来在相应的接口中打包任务。 例如，以下代码通过使用实现一种异步方法**Task.Run&lt;TResult&gt;** 方法来启动任务。 **AsAsyncOperation&lt;TResult&gt;** 扩展方法将返回为 Windows 运行时异步操作的任务。

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

以下 JavaScript 代码显示了如何通过使用[**WinJS.Promise**](https://msdn.microsoft.com/library/windows/apps/br211867.aspx)对象调用方法。 然后，传递到该方法的函数在完成异步调用时执行。 StringList 参数包含**DownloadAsStringAsync**方法返回的字符串列表，并且该函数会执行任何处理所需的。

```javascript
function asyncExample(id) {

    var result = SampleComponent.Example.downloadAsStringAsync(id).then(
        function (stringList) {
            // Place code that uses the returned list of strings here.
        });
}
```

对于支持取消和进度报告的操作的异步操作，请使用[**AsyncInfo**](/dotnet/api/system.runtime.interopservices.windowsruntime)类生成启动的任务并挂钩的取消和进度报告将任务的取消和进度的功能与相应的 Windows 运行时接口的报告功能。 有关支持取消和进度报告的示例，请参阅[演练：使用 C# 或 Visual Basic 创建简单组件并通过 JavaScript 调用它](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)。

请注意，即使异步方法不支持取消或进度报告，你可以使用**AsyncInfo**类的方法。 如果你使用 Visual Basic lambda 函数或 C# 匿名方法，不提供令牌参数，并[**IProgress&lt;T&gt;**](https://msdn.microsoft.com/library/hh138298.aspx)接口。 如果你使用 C# lambda 函数，则提供令牌参数，但忽略它。 上一示例中，使用了 AsAsyncOperation&lt;TResult&gt;方法，如下所示，当你使用[**AsyncInfo.Run&lt;TResult&gt;(Func&lt;CancellationToken，Task&lt;TResult&gt;**](https://msdn.microsoft.com/library/hh779740.aspx)) 方法改为重载。

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

如果你创建一种异步方法，可以选择支持取消或进度报告，请考虑添加没有取消令牌参数的重载或者**IProgress&lt;T&gt;** 接口。

## <a name="throwing-exceptions"></a>引发异常

你可以引发任何包括在适用于 Windows 应用的 .NET 中的异常类型。 你无法在 Windows 运行时组件中声明自己的公共异常类型，但可以声明并引发非公共类型。

如果组件不处理异常，将在调用组件的代码中引发相应异常。 向调用方显示异常的方式取决于调用语言支持 Windows 运行时的方式。

-   在 JavaScript 中，异常显示为堆栈跟踪替换异常消息的对象。 在 Visual Studio 中调试应用时，你可以看到显示在调试器异常对话框中标识为“WinRT 信息”的原始消息文本。 无法从 JavaScript 代码访问原始消息文本。

    > **提示**。目前，堆栈跟踪包含托管的异常类型，但我们不推荐分析跟踪来标识异常类型。 改为使用 HRESULT 值，如本部分后面所述。

-   在 C++ 中，异常显示为平台异常。 如果托管的异常的 HResult 属性能够映射到特定平台异常的 HRESULT，则使用特定异常;否则， [**platform:: comexception**](https://msdn.microsoft.com/library/windows/apps/xaml/hh710414.aspx)引发异常。 托管异常的消息文本不适用于 C++ 代码。 如果已引发特定平台异常，将显示该异常类型的默认消息文本；否则，将不显示任何消息文本。 请参阅[异常 (C++/CX)](https://msdn.microsoft.com/library/windows/apps/xaml/hh699896.aspx)。
-   在 C# 或 Visual Basic 中，异常是正常的托管异常。

在从组件引发异常时，你可以使 JavaScript 或 C++ 调用方处理异常变得更简单，方法是引发其 HResult 属性值特定于你的组件的非公共异常类型。 适用于 JavaScript 调用方通过异常对象的编号属性，以及 c + + 调用方通过[**comexception:: Hresult**](https://msdn.microsoft.com/library/windows/apps/xaml/hh710415.aspx)属性的 HRESULT。

> [!NOTE]
> 为 HRESULT 使用负值。 正值解释为成功，并且在 JavaScript 或 C++ 调用方中没有引发任何异常。

## <a name="declaring-and-raising-events"></a>声明和引发事件

在你声明类型以保留事件数据时，请从 Object 而非 EventArgs 派生，因为 EventArgs 不属于 Windows 运行时类型。 使用[**EventHandler&lt;TEventArgs&gt;**](https://msdn.microsoft.com/library/db0etb8x.aspx)事件，并将事件参数类型用作泛型类型参数的类型。 就像在 .NET Framework 应用程序中一样引发该事件。

在通过 JavaScript 或 C++ 使用 Windows 运行时组件时，事件遵循这些语言期望的 Windows 运行时事件模式。 在通过 C# 或 Visual Basic 使用组件时，事件显示为普通的 .NET Framework 事件。 [演练：使用 C# 或 Visual Basic 创建简单组件并通过 JavaScript 调用它]()中提供了示例。

如果要实现自定义事件访问器（在 Visual Basic 中通过 **Custom** 关键字声明事件），必须在实现中遵循 Windows 运行时事件模式。 请参阅 [Windows 运行时组件中的自定义事件和事件访问器](custom-events-and-event-accessors-in-windows-runtime-components.md)。 请注意，在通过 C# 或 Visual Basic 代码处理事件时，它仍显示为普通的 .NET Framework 事件。

## <a name="next-steps"></a>后续步骤

在创建了 Windows 运行时组件以供自己使用后，你可能会发现它封装的功能对其他开发人员也很有用。 若要打包组件以分配给其他开发人员，你有两个选择。 请参阅[分配托管的 Windows 运行时组件](https://msdn.microsoft.com/library/jj614475.aspx)。

有关 Visual Basic 和 C# 语言功能以及 Windows 运行时的 .NET Framework 支持的详细信息，请参阅 [Visual Basic 和 C# 语言参考](https://msdn.microsoft.com/library/windows/apps/xaml/br212458.aspx)。

## <a name="related-topics"></a>相关主题
* [适用于 UWP 应用的 .NET](https://msdn.microsoft.com/library/windows/apps/mt185501)
* [演练：创建简单的 Windows 运行时组件并通过 JavaScript 调用它](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)
