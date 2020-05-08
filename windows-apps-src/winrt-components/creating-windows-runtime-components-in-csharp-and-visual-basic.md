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
ms.openlocfilehash: 3ac5e08b328e8e094906d2e1793ea87fa2e66ae4
ms.sourcegitcommit: 91ac6db556fa2a49374fffb143f55fed864200ac
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/27/2020
ms.locfileid: "82173483"
---
# <a name="windows-runtime-components-with-c-and-visual-basic"></a>使用 C# 和 Visual Basic 创建 Windows 运行时组件

你可以使用托管代码创建自己的 Windows 运行时类型，并将其打包到 Windows 运行时组件中。 可以在使用 c + +、JavaScript、Visual Basic 或 c # 编写的通用 Windows 平台（UWP）应用中使用组件。 本主题概述了用于创建组件的规则，并讨论了有关 Windows 运行时的 .NET 支持的一些方面。 一般情况下，该支持设计为对 .NET 程序员透明可见。 但是，在你创建要与 JavaScript 或 C++ 一起使用的组件时，需要意识到这些语言支持 Windows 运行时的方法差异。

如果你正在创建仅用于以 Visual Basic 或 c # 编写的 UWP 应用的组件，并且该组件不包含 UWP 控件，则考虑使用**类库模板而**不是 Microsoft Visual Studio 中**Windows 运行时组件**项目模板。 简单类库所受限制较少。

## <a name="declaring-types-in-windows-runtime-components"></a>在 Windows 运行时组件中声明类型

在内部，组件中的 Windows 运行时类型可以使用 UWP 应用程序中允许的任何 .NET 功能。 有关详细信息，请参阅适用[于 UWP 应用的 .net](https://docs.microsoft.com/dotnet/api/index?view=dotnet-uwp-10.0)。

在外部，类型的成员只能为其参数和返回值公开 Windows 运行时类型。 以下列表描述了从 Windows 运行时组件公开的 .NET 类型的限制。

- 组件中的所有公共类型和成员的字段、参数和返回值必须是 Windows 运行时类型。 此限制包括你创作的 Windows 运行时类型以及 Windows 运行时本身提供的类型。 它还包括许多 .NET 类型。 包含这些类型是 .NET 提供的支持的一部分，它可以在托管代码&mdash;中自然地使用 Windows 运行时代码似乎使用熟悉的 .net 类型，而不是基础 Windows 运行时类型。 例如，你可以使用诸如**Int32**和**Double**的 .net 基元类型、某些基本类型（如**DateTimeOffset**和**Uri**）以及一些常用的泛型接口类型，如**ienumerable&lt;T&gt; ** （Visual Basic 中的 ienumerable （of））和**IDictionary&lt;TKey，TValue&gt;**。 请注意，这些泛型类型的类型参数必须是 Windows 运行时类型。 本主题后面的将[Windows 运行时类型传递给托管代码](#passing-windows-runtime-types-to-managed-code)和[将托管类型传递给 Windows 运行时](#passing-managed-types-to-the-windows-runtime)部分对此进行了讨论。

- 公共类和接口可以包含方法、属性和事件。 可以为事件声明委托，或使用**EventHandler&lt;T&gt; **委托。 公共类或接口不能：
    - 具有泛型性。
    - 实现一个接口，该接口不是 Windows 运行时接口（不过，您可以创建自己的 Windows 运行时接口并实现它们）。
    - 从不在 Windows 运行时中的类型派生 **，如 system.exception 和** **system.object**。

- 所有公共类型必须具有匹配程序集名的根命名空间，并且程序集名不得以“Windows”开头。

    > **提示**。 默认情况下，Visual Studio 项目具有与程序集名称匹配的命名空间名称。 在 Visual Basic 中，此默认命名空间的命名空间声明不会显示在代码中。

- 公共结构无法具有公共字段以外的任何成员，并且这些字段必须是值类型或字符串。
- 公共类必须是 **sealed**（在 Visual Basic 中是 **NotInheritable**）。 如果编程模型要求多态性，则可以创建一个公共接口，并在必须为多态的类中实现该接口。

## <a name="debugging-your-component"></a>调试组件

如果 UWP 应用和组件都是用托管代码生成的，则可以同时调试它们。

使用 c + + 在 UWP 应用中测试组件时，可以同时调试托管代码和本机代码。 默认情况下仅调试本机代码。

## <a name="to-debug-both-native-c-code-and-managed-code"></a>调试本机 C++ 代码和托管代码
1.  打开 Visual C++ 项目的快捷菜单，并选择“属性”****。
2.  在属性页面的“配置属性”**** 下，选择“调试”****。
3.  选择“调试器类型”****，并在下拉列表框中将“仅限本机”**** 更改为“混合(托管和本机)”****。 选择 **“确定”** 。
4.  在本机代码和托管代码中设置断点。

使用 JavaScript 将组件作为 UWP 应用的一部分进行测试时，默认情况下该解决方案处于 JavaScript 调试模式。 在 Visual Studio 中，无法同时调试 JavaScript 和托管代码。

## <a name="to-debug-managed-code-instead-of-javascript"></a>调试托管代码而非 JavaScript
1.  打开 JavaScript 项目的快捷菜单，并选择“属性”****。
2.  在属性页面的“配置属性”**** 下，选择“调试”****。
3.  选择“调试器类型”****，并在下拉列表框中将“仅限脚本”**** 更改为“仅限托管”****。 选择 **“确定”** 。
4.  在托管代码中设置断点，并像往常一样调试。

## <a name="passing-windows-runtime-types-to-managed-code"></a>将 Windows 运行时类型传递到托管代码
如前面在[Windows 运行时组件中声明类型](#declaring-types-in-windows-runtime-components)部分中所述，某些 .net 类型可能出现在公共类的成员签名中。 这是 .NET 提供的支持的一部分，用于在托管代码中自然地使用 Windows 运行时。 它包含基元类型以及某些类和接口。 当你的组件从 JavaScript 或 c + + 代码中使用时，请务必了解 .NET 类型如何显示给调用方。 有关使用 JavaScript 的示例，请参阅[创建 c # 或 Visual Basic Windows 运行时组件的演练，并从 javascript 调用它](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)。 本部分讨论常用类型。

在 .NET 中，基元类型（如**Int32**结构）具有许多有用的属性和方法，例如**TryParse**方法。 相比之下，Windows 运行时中的基元类型和结构则仅拥有字段。 将这些类型传递给托管代码时，它们看起来像 .NET 类型，您可以像往常一样使用 .NET 类型的属性和方法。 下表总结了在 IDE 中自动进行的替换：

-   对于 Windows 运行时基元**Int32**、 **Int64**、 **Single**、 **Double**、 **Boolean**、 **String** （不可变的 Unicode 字符集合）、 **Enum**、 **UInt32**、 **UInt64**和**Guid**，请使用 System 命名空间中具有相同名称的类型。
-   对于**UInt8**，请使用**system.object**。
-   对于**Char16**，请使用**system.object**。
-   对于**IInspectable**接口，请使用**system.object**。

如果 c # 或 Visual Basic 为上述任意类型提供语言关键字，则可改用 language 关键字。

除基元类型外，某些基本的常用 Windows 运行时类型在托管代码中显示为其 .NET 等效项。 例如，假设你的 JavaScript 代码使用了**Windows. 地基**类，并且你想要将其传递给 c # 或 Visual Basic 方法。 托管代码中的等效类型是 .NET **system.object**类，并且这是用于方法参数的类型。 你可以知道 Windows 运行时类型显示为 .NET 类型的时间，因为 Visual Studio 中的 IntelliSense 会在你编写托管代码时隐藏 Windows 运行时类型，并显示等效的 .NET 类型。 （通常，这两种类型具有相同名称。 但是，请注意， **Windows** node.js 结构在托管代码中显示为**system.exception，而不是作为** **system.object**。）

对于某些常用的集合类型，映射位于 Windows 运行时类型所实现的接口与相应 .NET 类型实现的接口之间。 与上面提到的类型一样，可以使用 .NET 类型声明参数类型。 这隐藏了类型之间的一些差异，并使编写 .NET 代码更自然。

下表列出了最常见的泛型接口类型以及其他常见的类和接口映射。 有关 .NET 映射的 Windows 运行时类型的完整列表，请参阅[Windows 运行时类型的 .net 映射](net-framework-mappings-of-windows-runtime-types.md)。

| Windows 运行时                                  | .NET                                    |
|-|-|
| IIterable&lt;T&gt;                               | IEnumerable&lt;T&gt;                              |
| IVector&lt;T&gt;                                 | IList&lt;T&gt;                                    |
| IVectorView&lt;T&gt;                             | IReadOnlyList&lt;T&gt;                            |
| IMap&lt;K，V&gt;                                 | IDictionary&lt;TKey，TValue&gt;                   |
| IMapView&lt;K, V&gt;                             | System.collections.generic.ireadonlydictionary<tkey&lt;TKey，TValue&gt;           |
| IKeyValuePair&lt;K，V&gt;                        | KeyValuePair&lt;TKey，TValue&gt;                  |
| IBindableIterable                                | IEnumerable                                       |
| IBindableVector                                  | IList                                             |
| Windows.UI.Xaml.Data.INotifyPropertyChanged      | System.ComponentModel.INotifyPropertyChanged      |
| Windows.UI.Xaml.Data.PropertyChangedEventHandler | System.ComponentModel.PropertyChangedEventHandler |
| Windows.UI.Xaml.Data.PropertyChangedEventArgs    | System.ComponentModel.PropertyChangedEventArgs    |

在某种类型实现多个接口时，你可以将所实现的任意接口用作成员的参数类型或返回类型。 例如，你可以将**字典&lt;int、string&gt; ** （**整数，string Visual Basic）** 作为**IDictionary&lt;int、string&gt;**、 **system.collections.generic.ireadonlydictionary<tkey&lt;int、string&gt;** 或**IEnumerable&lt;KeyValuePair&lt;TKey，TValue&gt;** 进行传递或返回。

> [!IMPORTANT]
> JavaScript 使用托管类型实现的、在接口列表中显示在最前面的接口。 例如，如果您将**字典&lt;int 和 string&gt; **返回到 JavaScript 代码，无论您指定哪一个接口作为返回类型，它都显示为**IDictionary&lt;int，string&gt; ** 。 这意味着，如果第一个接口不包括显示在后续接口上的成员，JavaScript 将看不到该成员。

在 Windows 运行时中，使用 IKeyValuePair 循环访问**IMap&lt;k、v&gt; **和**IMapView&lt;K，v&gt; ** 。 当你将它们传递给托管代码时，它们将显示为**&lt;IDictionary&gt; TKey、TValue**和**&lt;system.collections.generic.ireadonlydictionary<tkey&gt;TKey，TValue**，因此，你可以自然地使用**&lt;KeyValuePair，TKey&gt; **来枚举它们。

接口在托管代码中的显示方式将影响实现这些接口的类型的显示方式。 例如， **PropertySet**类实现**IMap&lt;K&gt;，V**，后者在托管代码中显示为**IDictionary&lt;TKey，TValue&gt;**。 **PropertySet**看起来像是实现**了&lt;IDictionary TKey、&gt; TValue**而不是**IMap&lt;K&gt;，V**，因此在托管代码中，它看起来**像是 .net**字典上的**add**方法。 它看起来没有**Insert**方法。 您可以在[创建 c # 或 Visual Basic Windows 运行时组件的主题演练中查看此示例，并从 JavaScript 中调用它](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)。

## <a name="passing-managed-types-to-the-windows-runtime"></a>将托管的类型传递到 Windows 运行时

如前一部分中所述，某些 Windows 运行时类型可以在组件成员的签名中显示为 .NET 类型，也可以在 IDE 中使用时在 Windows 运行时成员的签名中显示。 将 .NET 类型传递给这些成员或将其用作组件成员的返回值时，它们将在另一端作为相应的 Windows 运行时类型出现在另一端。 有关从 JavaScript 调用组件时这可能产生的影响的示例，请参阅[创建 c # 或 Visual Basic Windows 运行时组件的演练](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)中的 "从组件中返回托管类型" 部分，然后从 javascript 中调用它。

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

-   只能在 "类实现" 关系中定义**IStringable**接口，如 c # 中的以下代码：

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

若要在组件中实现异步方法，请将 "Async" 添加到方法名称的末尾，并返回一个表示异步操作或操作的 Windows 运行时接口： **IAsyncAction**、 **iasyncactionwithprogress<tprogress>&lt;TProgress&gt;**、 **iasyncoperation<tresult>&lt;TResult&gt;** 或**IAsyncOperationWithProgress&lt;TResult，TProgress&gt;**。

可以使用 .net 任务（[**任务**](/dotnet/api/system.threading.tasks.task)类和通用[**任务&lt;TResult&gt; **](/dotnet/api/system.threading.tasks.task-1)类）来实现异步方法。 必须返回表示正在进行的操作的任务，例如从使用 c # 或 Visual Basic 编写的异步方法中返回的任务，或者从该任务返回的任务[**。运行**](/dotnet/api/system.threading.tasks.task.run)方法。 如果你使用构造函数创建任务，必须在返回它之前调用其 [Task.Start](/dotnet/api/system.threading.tasks.task.start) 方法。

使用`await` （`Await`在 Visual Basic 中）的方法需要`async`关键字（`Async`在 Visual Basic 中）。 如果从 Windows 运行时组件公开此类方法，请将`async`关键字应用于传递给**Run**方法的委托。

对于不支持取消和进度报告的异步操作，你可以使用 [WindowsRuntimeSystemExtensions.AsAsyncAction](https://docs.microsoft.com/dotnet/api/system) 或 [AsAsyncOperation&lt;TResult&gt;](https://docs.microsoft.com/dotnet/api/system) 扩展方法来在相应的接口中打包任务。 例如，下面的代码使用任务来实现异步方法 **。运行&lt;TResult&gt; **方法来启动任务。 **AsAsyncOperation&lt;TResult&gt; **扩展方法将任务作为 Windows 运行时异步操作返回。

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

对于支持取消或进度报告的异步操作和操作，请使用[**system.runtime.interopservices.windowsruntime.asyncinfo**](/dotnet/api/system.runtime.interopservices.windowsruntime)类生成已启动的任务，并将该任务的取消和进度报告功能与相应 Windows 运行时接口的取消和进度报告功能挂钩。 有关支持取消和进度报告的示例，请参阅[创建 c # 或 Visual Basic Windows 运行时组件的演练，并从 JavaScript 调用它](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)。

请注意，即使异步方法不支持取消或进度报告，也可以使用**system.runtime.interopservices.windowsruntime.asyncinfo**类的方法。 如果使用 Visual Basic lambda 函数或 c # 匿名方法，请不要为标记和[**iprogress<t>&lt;t&gt; **](https://docs.microsoft.com/dotnet/api/system.iprogress-1)接口提供参数。 如果你使用 C# lambda 函数，则提供令牌参数，但忽略它。 使用&lt;AsAsyncOperation&gt; tresult 方法的上一个示例在改用[**system.runtime.interopservices.windowsruntime.asyncinfo&lt;&gt;（Func&lt;CancellationToken，Task&lt;TResult&gt;**](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.windowsruntime)）方法重载时与此类似。

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

如果创建可以选择支持取消或进度报告的异步方法，请考虑添加没有参数的重载或**iprogress<t>&lt;t&gt; **接口。

## <a name="throwing-exceptions"></a>抛出异常

你可以引发任何包括在适用于 Windows 应用的 .NET 中的异常类型。 你无法在 Windows 运行时组件中声明自己的公共异常类型，但可以声明并引发非公共类型。

如果组件不处理异常，将在调用组件的代码中引发相应异常。 向调用方显示异常的方式取决于调用语言支持 Windows 运行时的方式。

-   在 JavaScript 中，异常显示为堆栈跟踪替换异常消息的对象。 在 Visual Studio 中调试应用时，你可以看到显示在调试器异常对话框中标识为“WinRT 信息”的原始消息文本。 无法从 JavaScript 代码访问原始消息文本。

    > **提示**。目前，堆栈跟踪包含托管异常类型，但我们不建议你分析跟踪来标识异常类型。 改为使用 HRESULT 值，如本部分后面所述。

-   在 C++ 中，异常显示为平台异常。 如果托管异常的 HResult 属性可映射到特定平台异常的 HRESULT，则使用特定异常;否则，将引发[**Platform：： COMException**](https://docs.microsoft.com/cpp/cppcx/platform-comexception-class)异常。 托管异常的消息文本不适用于 C++ 代码。 如果已引发特定平台异常，将显示该异常类型的默认消息文本；否则，将不显示任何消息文本。 请参阅[异常 (C++/CX)](https://docs.microsoft.com/cpp/cppcx/exceptions-c-cx)。
-   在 C# 或 Visual Basic 中，异常是正常的托管异常。

在从组件引发异常时，你可以使 JavaScript 或 C++ 调用方处理异常变得更简单，方法是引发其 HResult 属性值特定于你的组件的非公共异常类型。 通过[**COMException：： hresult**](https://docs.microsoft.com/cpp/cppcx/platform-comexception-class#hresult)属性，通过异常对象的 number 属性和 c + + 调用方可以使用 HRESULT。

> [!NOTE]
> 为 HRESULT 使用负值。 正值解释为成功，并且在 JavaScript 或 C++ 调用方中没有引发任何异常。

## <a name="declaring-and-raising-events"></a>声明和引发事件

在你声明类型以保留事件数据时，请从 Object 而非 EventArgs 派生，因为 EventArgs 不属于 Windows 运行时类型。 使用[**EventHandler&lt;TEventArgs&gt; **](https://docs.microsoft.com/dotnet/api/system.eventhandler-1)作为事件的类型，并使用事件参数类型作为泛型类型参数。 与在 .NET 应用程序中一样，引发事件。

在通过 JavaScript 或 C++ 使用 Windows 运行时组件时，事件遵循这些语言期望的 Windows 运行时事件模式。 使用 c # 或 Visual Basic 中的组件时，事件显示为普通的 .NET 事件。 在[创建 c # 或 Visual Basic Windows 运行时组件的演练中提供了一个示例，并从 JavaScript 调用它](/windows/uwp/winrt-components/walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript)。

如果要实现自定义事件访问器（在 Visual Basic 中通过 **Custom** 关键字声明事件），必须在实现中遵循 Windows 运行时事件模式。 请参阅[Windows 运行时组件中的自定义事件和事件访问器](custom-events-and-event-accessors-in-windows-runtime-components.md)。 请注意，在处理 c # 或 Visual Basic 代码中的事件时，该事件仍显示为普通的 .NET 事件。

## <a name="next-steps"></a>后续步骤

在创建了 Windows 运行时组件以供自己使用后，你可能会发现它封装的功能对其他开发人员也很有用。 若要打包组件以分配给其他开发人员，你有两个选择。 请参阅[分配托管的 Windows 运行时组件](https://docs.microsoft.com/previous-versions/windows/apps/jj614475(v=vs.140))。

有关 Visual Basic 和 c # 语言功能以及对 Windows 运行时的 .NET 支持的详细信息，请参阅[Visual Basic 和 c # 语言参考](https://docs.microsoft.com/visualstudio/welcome-to-visual-studio-2015?view=vs-2015)。

## <a name="related-topics"></a>相关主题
* [适用于 UWP 应用的 .NET](https://docs.microsoft.com/dotnet/api/index?view=dotnet-uwp-10.0)
* [创建 C# 或 Visual Basic Windows 运行时组件并通过 JavaScript 调用此组件的演练](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)
