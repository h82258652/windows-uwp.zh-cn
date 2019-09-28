---
title: 使用 C# 和 Visual Basic 创建 Windows 运行时组件
description: 从 .NET 4.5 开始，你可以使用托管代码创建你自己的 Windows 运行时类型，并将其打包到 Windows 运行时组件中。
ms.assetid: A5672966-74DF-40AB-B01E-01E3FCD0AD7A
ms.date: 12/04/2018
ms.topic: article
dev_langs:
- csharp
- vb
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c402b8e4ba98f55267a42c1bce1c16e6f090e80c
ms.sourcegitcommit: a20457776064c95a74804f519993f36b87df911e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71340379"
---
# <a name="windows-runtime-components-with-c-and-visual-basic"></a>使用 C# 和 Visual Basic 创建 Windows 运行时组件

你可以使用托管代码创建自己的 Windows 运行时类型，并将其打包到 Windows 运行时组件中。 你可以在使用C++、JavaScript、Visual Basic 或C#编写的通用 Windows 平台（UWP）应用中使用你的组件。 本主题概述了用于创建组件的规则，并讨论了有关 Windows 运行时的 .NET 支持的一些方面。 一般情况下，该支持设计为对 .NET 程序员透明可见。 但是，在你创建要与 JavaScript 或 C++ 一起使用的组件时，需要意识到这些语言支持 Windows 运行时的方法差异。

如果要创建仅用于以 Visual Basic 或C#编写的 uwp 应用的组件，并且该组件不包含 uwp 控件，则使用**类库**模板而不是**Windows 运行时组件**项目进行考虑Microsoft Visual Studio 中的模板。 简单类库所受限制较少。

## <a name="declaring-types-in-windows-runtime-components"></a>在 Windows 运行时组件中声明类型

在内部，组件中的 Windows 运行时类型可以使用 UWP 应用程序中允许的任何 .NET 功能。 有关详细信息，请参阅适用[于 UWP 应用的 .net](https://docs.microsoft.com/dotnet/api/index?view=dotnet-uwp-10.0)。

在外部，类型的成员只能为其参数和返回值公开 Windows 运行时类型。 以下列表描述了从 Windows 运行时组件公开的 .NET 类型的限制。

- 组件中的所有公共类型和成员的字段、参数和返回值必须是 Windows 运行时类型。 此限制包括你创作的 Windows 运行时类型以及 Windows 运行时本身提供的类型。 它还包括许多 .NET 类型。 包含这些类型是 .NET 提供的支持的一部分，它可以在托管代码 @ no__t-0your 代码中自然地使用 Windows 运行时，而不是使用熟悉的 .NET 类型，而不是基础 Windows 运行时类型。 例如，你可以使用诸如**Int32**和**Double**的 .net 基元类型、某些基本类型（如**DateTimeOffset**和**Uri**）以及一些常用的泛型接口类型，如**IEnumerable @ no__t-5T @ no__t-6**（Visual Basic 中的 IEnumerable （Of T）和**IDictionary @ No__t-8TKey，TValue @ no__t-9**。 请注意，这些泛型类型的类型参数必须是 Windows 运行时类型。 本主题后面的将[Windows 运行时类型传递给托管代码](#passing-windows-runtime-types-to-managed-code)和[将托管类型传递给 Windows 运行时](#passing-managed-types-to-the-windows-runtime)部分对此进行了讨论。

- 公共类和接口可以包含方法、属性和事件。 可以为事件声明委托，或使用**EventHandler @ no__t-1T @ no__t**委托。 公共类或接口不能：
    - 具有泛型性。
    - 实现一个接口，该接口不是 Windows 运行时接口（不过，您可以创建自己的 Windows 运行时接口并实现它们）。
    - 从不在 Windows 运行时中的类型派生 **，如 system.exception 和** **system.object**。

- 所有公共类型必须具有匹配程序集名的根命名空间，并且程序集名不得以“Windows”开头。

    > **提示**。 默认情况下，Visual Studio 项目具有与程序集名称匹配的命名空间名称。 在 Visual Basic 中，此默认命名空间的命名空间声明不会显示在代码中。

- 公共结构无法具有公共字段以外的任何成员，并且这些字段必须是值类型或字符串。
- 公共类必须是 **sealed**（在 Visual Basic 中是 **NotInheritable**）。 如果编程模型要求多态性，则可以创建一个公共接口，并在必须为多态的类中实现该接口。

## <a name="debugging-your-component"></a>调试组件

如果 UWP 应用和组件都是用托管代码生成的，则可以同时调试它们。

使用C++在 UWP 应用中测试组件时，可以同时调试托管代码和本机代码。 默认情况下仅调试本机代码。

## <a name="to-debug-both-native-c-code-and-managed-code"></a>调试本机 C++ 代码和托管代码
1.  打开 Visual C++ 项目的快捷菜单，并选择“属性”。
2.  在属性页面的“配置属性”下，选择“调试”。
3.  选择“调试器类型”，并在下拉列表框中将“仅限本机”更改为“混合(托管和本机)”。 选择“确定”。
4.  在本机代码和托管代码中设置断点。

使用 JavaScript 将组件作为 UWP 应用的一部分进行测试时，默认情况下该解决方案处于 JavaScript 调试模式。 在 Visual Studio 中，无法同时调试 JavaScript 和托管代码。

## <a name="to-debug-managed-code-instead-of-javascript"></a>调试托管代码而非 JavaScript
1.  打开 JavaScript 项目的快捷菜单，并选择“属性”。
2.  在属性页面的“配置属性”下，选择“调试”。
3.  选择“调试器类型”，并在下拉列表框中将“仅限脚本”更改为“仅限托管”。 选择“确定”。
4.  在托管代码中设置断点，并像往常一样调试。

## <a name="passing-windows-runtime-types-to-managed-code"></a>将 Windows 运行时类型传递到托管代码
如前面在[Windows 运行时组件中声明类型](#declaring-types-in-windows-runtime-components)部分中所述，某些 .net 类型可能出现在公共类的成员签名中。 这是 .NET 提供的支持的一部分，用于在托管代码中自然地使用 Windows 运行时。 它包含基元类型以及某些类和接口。 当你的组件从 JavaScript 或C++代码中使用时，请务必了解 .net 类型如何显示给调用方。 有关使用 JavaScript 的示例，请参阅[创建C#或 Visual Basic Windows 运行时组件并从 javascript 中调用该组件的演练](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)。 本部分讨论常用类型。

在 .NET 中，基元类型（如**Int32**结构）具有许多有用的属性和方法，例如**TryParse**方法。 相比之下，Windows 运行时中的基元类型和结构则仅拥有字段。 将这些类型传递给托管代码时，它们看起来像 .NET 类型，您可以像往常一样使用 .NET 类型的属性和方法。 下表总结了在 IDE 中自动进行的替换：

-   对于 Windows 运行时基元**Int32**、 **Int64**、 **Single**、 **Double**、 **Boolean**、 **String** （Unicode 字符的不可变集合）、 **Enum**、 **UInt32**、 **UInt64**和**Guid**，请使用 System 命名空间中的同名类型。
-   对于**UInt8**，请使用**system.object**。
-   对于**Char16**，请使用**system.object**。
-   对于**IInspectable**接口，请使用**system.object**。

如果C#或 Visual Basic 为上述任意类型提供语言关键字，则可改用 language 关键字。

除基元类型外，某些基本的常用 Windows 运行时类型在托管代码中显示为其 .NET 等效项。 例如，假设你的 JavaScript 代码使用了**Windows. 地基**类，并且你想要将其传递给C#或 Visual Basic 方法。 托管代码中的等效类型是 .NET **system.object**类，并且这是用于方法参数的类型。 你可以知道 Windows 运行时类型显示为 .NET 类型的时间，因为 Visual Studio 中的 IntelliSense 会在你编写托管代码时隐藏 Windows 运行时类型，并显示等效的 .NET 类型。 （通常，这两种类型具有相同名称。 但是，请注意， **Windows** node.js 结构在托管代码中显示为**system.exception，而不是作为** **system.object**。）

对于某些常用的集合类型，映射位于 Windows 运行时类型所实现的接口与相应 .NET 类型实现的接口之间。 与上面提到的类型一样，可以使用 .NET 类型声明参数类型。 这隐藏了类型之间的一些差异，并使编写 .NET 代码更自然。

下表列出了最常见的泛型接口类型以及其他常见的类和接口映射。 有关 .NET 映射的 Windows 运行时类型的完整列表，请参阅[Windows 运行时类型的 .net 映射](net-framework-mappings-of-windows-runtime-types.md)。

| Windows 运行时                                  | .NET                                    |
|-|-|
| Iiterable<t> @ no__t-0T @ no__t-1                               | IEnumerable @ no__t-0T @ no__t-1                              |
| IVector @ no__t-0T @ no__t-1                                 | IList @ no__t-0T @ no__t-1                                    |
| IVectorView @ no__t-0T @ no__t-1                             | IReadOnlyList @ no__t-0T @ no__t-1                            |
| IMap @ no__t-成功，V @ no__t-1                                 | IDictionary @ no__t-0TKey，TValue @ no__t-1                   |
| IMapView @ no__t-成功，V @ no__t-1                             | System.collections.generic.ireadonlydictionary<tkey @ no__t-0TKey，TValue @ no__t-1           |
| IKeyValuePair @ no__t-成功，V @ no__t-1                        | KeyValuePair @ no__t-0TKey，TValue @ no__t-1                  |
| IBindableIterable                                | IEnumerable                                       |
| IBindableVector                                  | IList                                             |
| Windows.UI.Xaml.Data.INotifyPropertyChanged      | System.ComponentModel.INotifyPropertyChanged      |
| Windows.UI.Xaml.Data.PropertyChangedEventHandler | System.ComponentModel.PropertyChangedEventHandler |
| Windows.UI.Xaml.Data.PropertyChangedEventArgs    | System.ComponentModel.PropertyChangedEventArgs    |

在某种类型实现多个接口时，你可以将所实现的任意接口用作成员的参数类型或返回类型。 例如，你可以将**字典 @ no__t-1int、string @ no__t-2** （Visual Basic 中**的字典（整数，string）** ）作为**IDictionary @ no__t-5int、string @ no__t-6**、 **system.collections.generic.ireadonlydictionary<tkey @ no__t-8int、string @ no__t-9**，或**IEnumerable @ No__t-KeyValuePair @ No__t-12TKey，TValue @ no__t-13 @ no__t-14**。

> [!IMPORTANT]
> JavaScript 使用托管类型实现的接口列表中首先显示的接口。 例如，如果您将**Dictionary @ no__t-1int、string @ no__t-2**返回给 JavaScript 代码，无论您指定哪一个接口作为返回类型，它都显示为**IDictionary @ no__t-4int，string @ no__t-5** 。 这意味着，如果第一个接口不包括显示在后续接口上的成员，JavaScript 将看不到该成员。

在 Windows 运行时中， **IMap @ no__t-1k，v @ no__t-2**和**IMapView @ no__t-4k，v @ no__t-5**将使用 IKeyValuePair 进行循环访问。 将它们传递给托管代码时，它们将显示为**IDictionary @ no__t-1TKey、TValue @ no__t-2**和**system.collections.generic.ireadonlydictionary<tkey @ no__t-4TKey、TValue @ no__t-5**，因此，你自然会使用 **@ KeyValuePair-no__t，TValue @ no__t**来枚举它们。

接口在托管代码中的显示方式影响实现这些接口的类型的显示方式。 例如， **PropertySet**类实现**IMap @ no__t-2k，V @ no__t**，后者在托管代码中显示为**IDictionary @ no__t-5TKey，TValue @ no__t-6**。 **PropertySet**看起来像是实现**了 IDictionary @ no__t-2TKey，TValue @ no__t-3**而不是**IMap @ no__t-no__t，V @-6**，因此在托管代码中，它**的行为**类似于中的**add**方法。网络字典。 它看起来没有**Insert**方法。 您可以在[创建C#或 Visual Basic Windows 运行时组件的主题演练中查看此示例，并从 JavaScript 中调用它](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)。

## <a name="passing-managed-types-to-the-windows-runtime"></a>将托管的类型传递到 Windows 运行时

如前一部分中所述，某些 Windows 运行时类型可以在组件成员的签名中显示为 .NET 类型，也可以在 IDE 中使用时在 Windows 运行时成员的签名中显示。 将 .NET 类型传递给这些成员或将其用作组件成员的返回值时，它们将在另一端作为相应的 Windows 运行时类型出现在另一端。 有关从 JavaScript 调用组件时这可能产生的影响的示例，请参阅[创建C#或 Visual Basic Windows 运行时组件的演练中的 "从组件返回托管类型" 部分，然后从中调用该组件JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)。

## <a name="overloaded-methods"></a>重载的方法

在 Windows 运行时中，可重载方法。 但是，如果使用相同数量的参数声明多个重载，则必须仅将[**windows.foundation.metadata.defaultoverloadattribute**](/uwp/api/windows.foundation.metadata.defaultoverloadattribute)属性应用于这些重载之一。 该重载是唯一能够通过 JavaScript 调用的重载。 例如，在以下代码中，接受 **int**（在 Visual Basic 中是 **Integer**）的重载是默认重载。

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

> 无关紧要JavaScript 允许向**OverloadExample**传递任何值，并将值强制转换为参数所需的类型。 可以通过 "42"、"42" 或42.3 调用**OverloadExample** ，但是所有这些值都将传递给默认重载。 上一示例中的默认重载分别返回0、42和42。

不能将**DefaultOverloadAttribut**e 特性应用到构造函数。 类中的所有构造函数必须具有不同数量的参数。

## <a name="implementing-istringable"></a>实现 IStringable

从 Windows 8.1 开始，Windows 运行时包括**IStringable**接口，该接口的单一方法**IStringable**提供的基本格式设置支持可与**Object. tostring**提供的格式支持。 如果选择在 Windows 运行时组件中导出的公共托管类型中实现**IStringable** ，则以下限制适用：

-   只能在 "类实现" 关系中定义**IStringable**接口，例如中C#的以下代码：

    ```cs
    public class NewClass : IStringable
    ```

    或以下 Visual Basic 代码：

    ```vb
    Public Class NewClass : Implements IStringable
    ```

-   无法在接口上实现**IStringable** 。
-   不能将参数声明为**IStringable**类型。
-   **IStringable**不能是方法、属性或字段的返回类型。
-   你无法通过使用如下所示的方法定义，从基类中隐藏**IStringable**实现：

    ```cs
    public class NewClass : IStringable
    {
       public new string ToString()
       {
          return "New ToString in NewClass";
       }
    }
    ```

    相反， **IStringable**实现必须始终重写基类实现。 只能通过对强类型类实例调用**ToString**实现来隐藏该实现。

> [!NOTE]
> 在各种条件下，从本机代码到实现**IStringable**或隐藏其**ToString**实现的托管类型的调用可能会产生意外行为。

## <a name="asynchronous-operations"></a>异步操作

若要在组件中实现异步方法，请将 "Async" 添加到方法名称的末尾，并返回一个表示异步操作或操作的 Windows 运行时接口：**IAsyncAction**、 **iasyncactionwithprogress<tprogress> @ no__t-2TProgress @ no__t**、 **iasyncoperation<tresult> @ no__t-5TResult @ no__t-** IAsyncOperationWithProgress、no__t @ 8TResult- **9**。

可以使用 .NET 任务（ [**Task**](/dotnet/api/system.threading.tasks.task)类和泛型[**Task @ no__t-4TResult @ no__t**](/dotnet/api/system.threading.tasks.task-1)类）来实现异步方法。 必须返回表示正在进行的操作的任务，例如从C#或 Visual Basic 中写入的异步方法返回的任务，或者返回从任务中返回的任务[。运行](/dotnet/api/system.threading.tasks.task.run)方法。 如果你使用构造函数创建任务，必须在返回它之前调用其 [Task.Start](/dotnet/api/system.threading.tasks.task.start) 方法。

使用 `await` （Visual Basic 中 `Await`）的方法需要 `async` 关键字（@no__t 中的 Visual Basic）。 如果从 Windows 运行时组件公开此类方法，请将 @no__t 关键字应用于传递给**Run**方法的委托。

对于不支持取消和进度报告的异步操作，你可以使用 [WindowsRuntimeSystemExtensions.AsAsyncAction](https://docs.microsoft.com/dotnet/api/system) 或 [AsAsyncOperation&lt;TResult&gt;](https://docs.microsoft.com/dotnet/api/system) 扩展方法来在相应的接口中打包任务。 例如，下面的代码通过使用任务来实现异步方法 **。 Run @ no__t-1TResult @ no__t-2**方法来启动任务。 **AsAsyncOperation @ no__t-1TResult @ no__t**扩展方法将任务作为 Windows 运行时异步操作返回。

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

下面的 JavaScript 代码演示如何使用[**WinJS**](https://docs.microsoft.com/previous-versions/windows/apps/br211867(v=win.10))对象调用方法。 然后，传递到该方法的函数在完成异步调用时执行。 StringList 参数包含**DownloadAsStringAsync**方法返回的字符串的列表，该函数执行所需的任何处理。

```javascript
function asyncExample(id) {

    var result = SampleComponent.Example.downloadAsStringAsync(id).then(
        function (stringList) {
            // Place code that uses the returned list of strings here.
        });
}
```

对于支持取消或进度报告的异步操作和操作，请使用[**system.runtime.interopservices.windowsruntime.asyncinfo**](/dotnet/api/system.runtime.interopservices.windowsruntime)类生成已启动的任务，并使用取消和进度挂钩任务的取消和进度报告功能报告相应 Windows 运行时接口的功能。 有关支持取消和进度报告的示例，请参阅[创建C#或 Visual Basic Windows 运行时组件的演练，并从 JavaScript 调用它](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)。

请注意，即使异步方法不支持取消或进度报告，也可以使用**system.runtime.interopservices.windowsruntime.asyncinfo**类的方法。 如果使用 Visual Basic lambda 函数或C#匿名方法，请不要为标记和[iprogress<t> @ no__t-3T @ no__t](https://docs.microsoft.com/dotnet/api/system.iprogress-1)接口提供参数。 如果你使用 C# lambda 函数，则提供令牌参数，但忽略它。 上面的示例使用 AsAsyncOperation @ no__t-0TResult @ no__t 方法，当你使用 System.runtime.interopservices.windowsruntime.asyncinfo 时，此示例如下所示[ **。运行 @ no__t-4TResult @ no__t-5 （Func @ no__t-6CancellationToken，Task @ no__t-7TResult @ no__t-8 @ no__t-9**](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.windowsruntime)）方法改为重载。

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

如果创建可以选择支持取消或进度报告的异步方法，请考虑添加没有参数的重载或**iprogress<t> @ no__t-1T @ no__t**接口。

## <a name="throwing-exceptions"></a>引发异常

你可以引发任何包括在适用于 Windows 应用的 .NET 中的异常类型。 你无法在 Windows 运行时组件中声明自己的公共异常类型，但可以声明并引发非公共类型。

如果组件不处理异常，将在调用组件的代码中引发相应异常。 向调用方显示异常的方式取决于调用语言支持 Windows 运行时的方式。

-   在 JavaScript 中，异常显示为堆栈跟踪替换异常消息的对象。 在 Visual Studio 中调试应用时，你可以看到显示在调试器异常对话框中标识为“WinRT 信息”的原始消息文本。 无法从 JavaScript 代码访问原始消息文本。

    > **提示**。 目前，堆栈跟踪包含托管异常类型，但我们不建议解析跟踪来标识异常类型。 改为使用 HRESULT 值，如本部分后面所述。

-   在 C++ 中，异常显示为平台异常。 如果托管异常的 HResult 属性可映射到特定平台异常的 HRESULT，则使用特定异常;否则，将引发[**Platform：： COMException**](https://docs.microsoft.com/cpp/cppcx/platform-comexception-class)异常。 托管异常的消息文本不适用于 C++ 代码。 如果已引发特定平台异常，将显示该异常类型的默认消息文本；否则，将不显示任何消息文本。 请参阅[异常 (C++/CX)](https://docs.microsoft.com/cpp/cppcx/exceptions-c-cx)。
-   在 C# 或 Visual Basic 中，异常是正常的托管异常。

在从组件引发异常时，你可以使 JavaScript 或 C++ 调用方处理异常变得更简单，方法是引发其 HResult 属性值特定于你的组件的非公共异常类型。 可通过异常对象的 number 属性向 JavaScript 调用方提供 HRESULT，并C++通过[COMException：： HRESULT](https://docs.microsoft.com/cpp/cppcx/platform-comexception-class#hresult)属性使用 HRESULT。

> [!NOTE]
> 为 HRESULT 使用负值。 正值解释为成功，并且在 JavaScript 或 C++ 调用方中没有引发任何异常。

## <a name="declaring-and-raising-events"></a>声明和引发事件

在你声明类型以保留事件数据时，请从 Object 而非 EventArgs 派生，因为 EventArgs 不属于 Windows 运行时类型。 使用[**EventHandler @ no__t-2TEventArgs @ no__t**](https://docs.microsoft.com/dotnet/api/system.eventhandler-1)作为事件的类型，并使用事件参数类型作为泛型类型参数。 与在 .NET 应用程序中一样，引发事件。

在通过 JavaScript 或 C++ 使用 Windows 运行时组件时，事件遵循这些语言期望的 Windows 运行时事件模式。 使用C#或 Visual Basic 中的组件时，该事件将显示为普通的 .net 事件。 本演练中提供了[一个C#示例，说明如何创建或 Visual Basic Windows 运行时组件并从 JavaScript 中调用它](/windows/uwp/winrt-components/walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript)。

如果要实现自定义事件访问器（在 Visual Basic 中通过 **Custom** 关键字声明事件），必须在实现中遵循 Windows 运行时事件模式。 请参阅[Windows 运行时组件中的自定义事件和事件访问器](custom-events-and-event-accessors-in-windows-runtime-components.md)。 请注意，在处理来自C#或 Visual Basic 代码的事件时，该事件仍显示为普通的 .net 事件。

## <a name="next-steps"></a>后续步骤

在创建了 Windows 运行时组件以供自己使用后，你可能会发现它封装的功能对其他开发人员也很有用。 若要打包组件以分配给其他开发人员，你有两个选择。 请参阅[分配托管的 Windows 运行时组件](https://docs.microsoft.com/previous-versions/windows/apps/jj614475(v=vs.140))。

有关 Visual Basic 和C#语言功能以及对 Windows 运行时的 .net 支持的详细信息，请参阅[Visual Basic 和C#语言参考](https://docs.microsoft.com/visualstudio/welcome-to-visual-studio-2015?view=vs-2015)。

## <a name="related-topics"></a>相关主题
* [适用于 UWP 应用的 .NET](https://docs.microsoft.com/dotnet/api/index?view=dotnet-uwp-10.0)
* [创建 C# 或 Visual Basic Windows 运行时组件并通过 JavaScript 调用此组件的演练](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)
