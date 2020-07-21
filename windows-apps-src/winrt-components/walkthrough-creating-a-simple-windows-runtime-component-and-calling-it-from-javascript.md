---
title: 创建C#或 Visual Basic Windows 运行时组件并从 JavaScript 中调用该组件的演练
description: 本演练演示如何将 .NET 与 Visual Basic 或C#创建自己的 Windows 运行时类型打包在一个 Windows 运行时组件中，以及如何从使用 JavaScript 为 Windows 生成的 UWP 应用程序调用该组件。
ms.assetid: 1565D86C-BF89-4EF3-81FE-35367DB8D671
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b7fc8e899b402dea21a11a0c8dce09646a84dae5
ms.sourcegitcommit: cc9f5a16386be78c12821a975e43497a0693abba
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/18/2019
ms.locfileid: "72578163"
---
# <a name="walkthrough-of-creating-a-c-or-visual-basic-windows-runtime-component-and-calling-it-from-javascript"></a>创建C#或 Visual Basic Windows 运行时组件并从 JavaScript 中调用该组件的演练

本演练演示如何将 .NET 与 Visual Basic 或C#创建自己的 Windows 运行时类型打包到 Windows 运行时组件中，以及如何从 JavaScript 通用 WINDOWS 平台（UWP）应用程序调用该组件。

通过 Visual Studio，可以轻松地在使用C#或 Visual Basic 编写的 Windows 运行时组件（WRC）项目中创作和部署你自己的自定义 Windows 运行时类型，然后从 JavaScript 应用程序项目中引用该 WRC，并从该应用程序中使用这些自定义类型。

Windows 运行时类型可以在内部使用 UWP 应用程序中允许的任何 .NET 功能。

> [!NOTE]
> 有关详细信息，请参阅[Windows 运行时组件C# with and Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md)和[.net for UWP apps 概述](/dotnet/api/index?view=dotnet-uwp-10.0)。

在外部，类型的成员只能为其参数和返回值公开 Windows 运行时类型。 生成解决方案时，Visual Studio 将生成 .NET WRC 项目，然后执行创建 Windows 元数据（winmd）文件的生成步骤。 这是你的 Windows 运行时组件，即 Visual Studio 在你的应用中包含的组件。

> [!NOTE]
> .NET 会自动将一些常用的 .NET 类型（如基元数据类型和集合类型）映射到它们 Windows 运行时的等效项。 可以在 Windows 运行时组件的公共接口中使用这些 .NET 类型，并且会向组件的用户显示为相应 Windows 运行时类型。 请参阅[Windows 运行时带有C# and Visual Basic 的组件](creating-windows-runtime-components-in-csharp-and-visual-basic.md)。

## <a name="prerequisites"></a>先决条件：

- Windows 10
- [Microsoft Visual Studio](https://visualstudio.microsoft.com/downloads/)

## <a name="creating-a-simple-windows-runtime-class"></a>创建一个简单的 Windows 运行时类

本部分将创建一个 JavaScript UWP 应用程序，并将 Visual Basic 或C# Windows 运行时组件项目添加到解决方案中。 它演示了如何定义 Windows 运行时类型、如何从 JavaScript 创建类型的实例，以及如何调用静态和实例成员。 为了使焦点位于组件上，示例应用程序的可视显示是有意用低键的。

1. 在 Visual Studio 中，创建新的 JavaScript 项目：在菜单栏上，依次选择“文件”、“新建”、“项目”。 在“新建项目”对话框的“已安装模板”部分中，依次选择JavaScript、“Windows”和“通用”。 （如果 Windows 不可用，请确保使用的是 Windows 8 或更高版本。）选择“空白应用程序”模板，然后输入 SampleApp 作为项目名称。
2.  创建组件项目：在“解决方案资源管理器”中，打开 SampleApp 解决方案的快捷菜单，然后依次选择“添加”、“新建项目”将新的 C# 或 Visual Basic 项目添加到解决方案。 在“添加新项目”对话框的“已安装模板”部分中，选择“Visual Basic”或“Visual C#”，然后依次选择“Windows”、“通用”。 选择“Windows 运行时组件”模板，然后输入 **SampleComponent** 作为项目名称。
3.  将类名更改为 **Example**。 请注意，该类在默认情况下标记为 **public sealed**（Visual Basic 中为 **Public NotInheritable**）。 必须封装通过你的组件公开的所有 Windows 运行时类。
4.  将两个简单的成员添加到类：**static** 方法（在 Visual Basic 中为 **Shared** 方法）和实例属性：

    > [!div class="tabbedCodeSnippets"]
    > ```csharp
    > namespace SampleComponent
    > {
    >     public sealed class Example
    >     {
    >         public static string GetAnswer()
    >         {
    >             return "The answer is 42.";
    >         }
    >
    >         public int SampleProperty { get; set; }
    >     }
    > }
    > ```
    > ```vb
    > Public NotInheritable Class Example
    >     Public Shared Function GetAnswer() As String
    >         Return "The answer is 42."
    >     End Function
    >
    >     Public Property SampleProperty As Integer
    > End Class
    > ```

5.  可选：若要为新添加的成员启用 IntelliSense，请在“解决方案资源管理器”中，打开 SampleComponent 项目的快捷菜单，然后选择“生成”。
6.  在“解决方案资源管理器”的 JavaScript 项目中，打开“引用”的快捷菜单，然后选择“添加引用”打开“引用管理器”。 依次选择“项目”和“解决方案”。 选中 SampleComponent 项目的复选框，然后选择“确定”来添加引用。

## <a name="call-the-component-from-javascript"></a>通过 JavaScript 调用组件

若要通过 JavaScript 使用 Windows 运行时类型，请在由 Visual Studio 模板提供的 default.js 文件（位于项目的 js 文件夹中）的匿名函数中添加以下代码。 它应位于 app.oncheckpoint 事件处理程序之后、对 app.start 的调用之前。

```javascript
var ex;

function basics1() {
   document.getElementById('output').innerHTML =
        SampleComponent.Example.getAnswer();

    ex = new SampleComponent.Example();

   document.getElementById('output').innerHTML += "<br/>" +
       ex.sampleProperty;

}

function basics2() {
    ex.sampleProperty += 1;
    document.getElementById('output').innerHTML += "<br/>" +
        ex.sampleProperty;
}
```

请注意，每个成员名称的第一个字母从大写更改为小写。 此转换是 JavaScript 所提供支持的一部分以便自然使用 Windows 运行时。 命名空间和类名采用 Pascal 大小写形式。 成员名称采用 Camel 大小写形式，除了事件名称以外，它们全部采用小写。 请参阅[在 JavaScript 中使用 Windows 运行时](/scripting/jswinrt/using-the-windows-runtime-in-javascript)。 Camel 大小写规则可能容易使人混淆。 一串初始的大写字母通常显示为小写，但如果三个大写字母后跟一个小写字母，则只有前两个字母以小写字母显示：例如名为 IDStringKind 的成员显示为 idStringKind。 在 Visual Studio 中，你可以生成自己的 Windows 运行时组件项目，然后在 JavaScript 项目中使用 IntelliSense 查看正确的大小写。

与此类似，.NET 提供了支持，使你能够在托管代码中自然地使用 Windows 运行时。 本文的后续部分将对此进行讨论，其中[包含C#和 Visual Basic 的 Windows 运行时组件](creating-windows-runtime-components-in-csharp-and-visual-basic.md)以及[对 UWP 应用和 Windows 运行时的 .net 支持](/dotnet/standard/cross-platform/support-for-windows-store-apps-and-windows-runtime)。

## <a name="create-a-simple-user-interface"></a>创建简单的用户界面

在你的 JavaScript 项目中，打开默认 .html 文件然后更新正文，如以下代码所示。 此代码包含用于示例应用的完整控件集，并指定了单击事件的函数名称。

> **请注意**  首次运行应用时，仅支持 "Basics1" 和 "Basics2" 按钮。

```html
<body>
            <div id="buttons">
            <button id="button1" >Basics 1</button>
            <button id="button2" >Basics 2</button>

            <button id="runtimeButton1">Runtime 1</button>
            <button id="runtimeButton2">Runtime 2</button>

            <button id="returnsButton1">Returns 1</button>
            <button id="returnsButton2">Returns 2</button>

            <button id="events1Button">Events 1</button>

            <button id="btnAsync">Async</button>
            <button id="btnCancel" disabled="disabled">Cancel Async</button>
            <progress id="primeProg" value="25" max="100" style="color: yellow;"></progress>
        </div>
        <div id="output">
        </div>
</body>
```

在 JavaScript 项目的 css 文件夹中，打开 default.css。 如下所示修改正文部分，并添加样式以控制按钮的布局和输出文本的位置。

```css
body
{
    -ms-grid-columns: 1fr;
    -ms-grid-rows: 1fr 14fr;
    display: -ms-grid;
}

#buttons {
    -ms-grid-rows: 1fr;
    -ms-grid-columns: auto;
    -ms-grid-row-align: start;
}
#output {
    -ms-grid-row: 2;
    -ms-grid-column: 1;
}
```

现在添加事件侦听器注册代码，方法是在 default.js 的 app.onactivated 中向 processAll 调用添加 then 子句。 替换调用 setPromise 的现有代码行，并将其更改为以下代码：

```javascript
args.setPromise(WinJS.UI.processAll().then(function () {
    var button1 = document.getElementById("button1");
    button1.addEventListener("click", basics1, false);
    var button2 = document.getElementById("button2");
    button2.addEventListener("click", basics2, false);
}));
```

向 HTML 控件添加事件是比直接在 HTML 中添加单击事件处理程序更好的方式。 请参阅[创建 "Hello，World" 应用（JS）](/windows/uwp/get-started/create-a-hello-world-app-js-uwp)。

## <a name="build-and-run-the-app"></a>生成并运行应用

在生成之前，根据你的计算机将所有项目的目标平台按需更改为 ARM、x64 或 x86。

若要生成并运行解决方案，请选择 F5 键。 （如果你收到运行时错误消息，指出 SampleComponent 未定义，则表明缺少对类库项目的引用。）

Visual Studio 首先编译类库，然后执行运行 [Winmdexp.exe（Windows 运行时元数据导出工具）](/dotnet/framework/tools/winmdexp-exe-windows-runtime-metadata-export-tool)的 MSBuild 任务来创建你的 Windows 运行时组件。 该组件包含在 .winmd 文件中，此文件同时包含了托管代码和描述代码的 Windows 元数据。 当你编写在 Windows 运行时组件中无效的代码时，WinMdExp.exe 将产生生成错误消息，并且错误消息将显示在 Visual Studio IDE 中。 Visual Studio 将你的组件添加到 UWP 应用程序的应用程序包（.appx 文件），并生成相应的清单。

选择 Basics 1 按钮将静态 GetAnswer 方法的返回值分配到输出区域、创建 Example 类的实例，并在输出区域显示其 SampleProperty 属性的值。 输出如下所示：

``` syntax
"The answer is 42."
0
```

选择 Basics 2 按钮递增 SampleProperty 属性的值并在输出区域中显示新值。 字符串和数字等基元类型可用作参数类型和返回类型，并且可以在托管代码与 JavaScript 之间进行传递。 由于 JavaScript 中的数值以双精度浮点格式进行存储，它们可以转换为 .NET Framework 数值类型。

> **请注意**  默认情况下，只能在 JavaScript 代码中设置断点。 若要调试 Visual Basic 或C#代码，请参阅在中C#创建 Windows 运行时组件 Visual Basic。

若要停止调试并关闭应用，请从应用切换到 Visual Studio，然后选择 Shift+F5。

## <a name="using-the-windows-runtime-from-javascript-and-managed-code"></a>从 JavaScript 和托管代码使用 Windows 运行时

可以从 JavaScript 或托管代码调用 Windows 运行时。 Windows 运行时对象可以在这两者之间来回传递，并且事件可以从任一端进行处理。 但是，在这两种环境中使用 Windows 运行时类型的方式在某些细节上有所不同，因为 JavaScript 和 .NET 支持 Windows 运行时不同。 下面的示例使用 [Windows.Foundation.Collections.PropertySet](/uwp/api/windows.foundation.collections.propertyset) 类演示了这些差异。 在本例中，你在托管代码中创建 PropertySet 集合的一个实例，并注册一个事件处理程序来跟踪集合中的更改。 然后添加获取集合的 JavaScript 代码、注册其自己的事件处理程序，然后使用集合。 最后，添加通过托管代码对集合进行更改的方法，并显示处理托管异常的 JavaScript。

> **重要**  在此示例中，事件在 UI 线程上激发。 如果从后台线程触发事件（例如在异步调用中），你需要进行一些额外工作才能使 JavaScript 处理事件。 有关详细信息，请参阅[在 Windows 运行时组件中引发事件](raising-events-in-windows-runtime-components.md)。

在 SampleComponent 项目中，添加名为 PropertySetStats 的新 **public sealed** 类（Visual Basic 中为 **Public NotInheritable** 类）。 该类封装了 PropertySet 集合并处理其 MapChanged 事件。 事件处理程序将跟踪所发生的每种类型的更改数目，并且 DisplayStats 方法将生成格式为 HTML 的报告。 请注意附加的 **using** 语句（在 Visual Basic 中为 **Imports** 语句）；请谨慎地将其添加到现有 **using** 语句中，而不是覆盖它们。

> [!div class="tabbedCodeSnippets"]
> ```csharp
> using Windows.Foundation.Collections;
>
> namespace SampleComponent
> {
>     public sealed class PropertySetStats
>     {
>         private PropertySet _ps;
>         public PropertySetStats()
>         {
>             _ps = new PropertySet();
>             _ps.MapChanged += this.MapChangedHandler;
>         }
>
>         public PropertySet PropertySet { get { return _ps; } }
>
>         int[] counts = { 0, 0, 0, 0 };
>         private void MapChangedHandler(IObservableMap<string, object> sender,
>             IMapChangedEventArgs<string> args)
>         {
>             counts[(int)args.CollectionChange] += 1;
>         }
>
>         public string DisplayStats()
>         {
>             StringBuilder report = new StringBuilder("<br/>Number of changes:<ul>");
>             for (int i = 0; i < counts.Length; i++)
>             {
>                 report.Append("<li>" + (CollectionChange)i + ": " + counts[i] + "</li>");
>             }
>             return report.ToString() + "</ul>";
>         }
>     }
> }
> ```
> ```vb
> Imports System.Text
>
> Public NotInheritable Class PropertySetStats
>     Private _ps As PropertySet
>     Public Sub New()
>         _ps = New PropertySet()
>         AddHandler _ps.MapChanged, AddressOf Me.MapChangedHandler
>     End Sub
>
>     Public ReadOnly Property PropertySet As PropertySet
>         Get
>             Return _ps
>         End Get
>     End Property
>
>     Dim counts() As Integer = {0, 0, 0, 0}
>     Private Sub MapChangedHandler(ByVal sender As IObservableMap(Of String, Object),
>         ByVal args As IMapChangedEventArgs(Of String))
>
>         counts(CInt(args.CollectionChange)) += 1
>     End Sub
>
>     Public Function DisplayStats() As String
>         Dim report As New StringBuilder("<br/>Number of changes:<ul>")
>         For i As Integer = 0 To counts.Length - 1
>             report.Append("<li>" & CType(i, CollectionChange).ToString() &
>                           ": " & counts(i) & "</li>")
>         Next
>         Return report.ToString() & "</ul>"
>     End Function
> End Class
> ```

事件处理程序遵循熟悉的 .NET Framework 事件模式，但事件的发送方（在本例中为 PropertySet 对象）被强制转换为 IObservableMap&lt;string，object&gt; interface （IObservableMap Visual Basic 中的（），这是 Windows 运行时接口[IObservableMap&lt;K，V&gt;](/uwp/api/Windows.Foundation.Collections.IObservableMap_K_V_)的实例化。 （如果需要，可以将发件人转换为其类型。）而且，事件参数以接口而非对象的形式提供。

在 default.js 文件中添加 Runtime1 函数，如下所示。 此代码会创建一个 PropertySetStats 对象、获取其 PropertySet 集合，然后添加自己的事件处理程序 onMapChanged 函数以处理 MapChanged 事件。 对集合进行更改之后，runtime1 调用 DisplayStats 方法显示更改类型的摘要。

```javascript
var propertysetstats;

function runtime1() {
    document.getElementById('output').innerHTML = "";

    propertysetstats = new SampleComponent.PropertySetStats();
    var propertyset = propertysetstats.propertySet;

    propertyset.addEventListener("mapchanged", onMapChanged);

    propertyset.insert("FirstProperty", "First property value");
    propertyset.insert("SuperfluousProperty", "Unnecessary property value");
    propertyset.insert("AnotherProperty", "A property value");

    propertyset.insert("SuperfluousProperty", "Altered property value")
    propertyset.remove("SuperfluousProperty");

    document.getElementById('output').innerHTML +=
        propertysetstats.displayStats();
}

function onMapChanged(change) {
    var result
    switch (change.collectionChange) {
        case Windows.Foundation.Collections.CollectionChange.reset:
            result = "All properties cleared";
            break;
        case Windows.Foundation.Collections.CollectionChange.itemInserted:
            result = "Inserted " + change.key + ": '" +
                change.target.lookup(change.key) + "'";
            break;
        case Windows.Foundation.Collections.CollectionChange.itemRemoved:
            result = "Removed " + change.key;
            break;
        case Windows.Foundation.Collections.CollectionChange.itemChanged:
            result = "Changed " + change.key + " to '" +
                change.target.lookup(change.key) + "'";
            break;
        default:
            break;
     }

     document.getElementById('output').innerHTML +=
         "<br/>" + result;
}
```

在 JavaScript 中处理 Windows 运行时事件的方式明显不同于在 .NET Framework 代码中进行处理的方式。 JavaScript 事件处理程序只使用一个参数。 在 Visual Studio 调试器中查看此对象时，第一个属性是发送程序。 事件参数接口的成员还将直接在此对象中显示。

若要运行应用，请选择 F5 键。 如果未封装类，你将收到错误消息“当前不支持导出未封装类型‘SampleComponent.Example’。 请将其标记为已封装。”

选择“Runtime 1”按钮。 当元素进行添加或更改时，事件处理程序将显示更改，并且最后 DisplayStats 方法将进行调用以生成计数的摘要。 若要停止调试并关闭应用，请切换回 Visual Studio，然后选择 Shift+F5。

若要从托管代码向 PropertySet 集合添加两个以上的项，请将以下代码添加到 PropertySetStats 类：

> [!div class="tabbedCodeSnippets"]
> ```csharp
> public void AddMore()
> {
>     _ps.Add("NewProperty", "New property value");
>     _ps.Add("AnotherProperty", "A property value");
> }
> ```
> ```vb
> Public Sub AddMore()
>     _ps.Add("NewProperty", "New property value")
>     _ps.Add("AnotherProperty", "A property value")
> End Sub
> ```

此代码强调了在两个环境中针对 Windows 运行时类型的使用方式的另一个区别。 如果自行键入此代码，你将注意到 IntelliSense 未显示你在 JavaScript 代码中使用的 insert 方法。 相反，它会显示在 .NET 中的集合上经常出现的 Add 方法。 这是因为某些常用的集合接口在 Windows 运行时和 .NET 中具有不同的名称，但具有类似的功能。 当你在托管代码中使用这些接口时，它们将显示为相应的 .NET Framework 等效项。 [与和 Visual Basic Windows 运行时组件中对C# ](creating-windows-runtime-components-in-csharp-and-visual-basic.md)此进行了讨论。 当你在 JavaScript 中使用相同接口时，从 Windows 运行时进行的唯一更改只是成员名称开头的大写字母变为小写。

最后，若要调用使用异常处理的 AddMore 方法，请将 runtime2 函数添加到 default.js。

```javascript
function runtime2() {
   try {
      propertysetstats.addMore();
    }
   catch(ex) {
       document.getElementById('output').innerHTML +=
          "<br/><b>" + ex + "<br/>";
   }

   document.getElementById('output').innerHTML +=
       propertysetstats.displayStats();
}
```

采用与以前相同的方式添加事件处理程序注册代码。

```javascript
var runtimeButton1 = document.getElementById("runtimeButton1");
runtimeButton1.addEventListener("click", runtime1, false);
var runtimeButton2 = document.getElementById("runtimeButton2");
runtimeButton2.addEventListener("click", runtime2, false);
```

若要运行应用，请选择 F5 键。 依次选择“Runtime 1”和“Runtime 2”。 JavaScript 事件处理程序报告对集合进行的第一项更改。 但是第二项更改具有一个重复键。 .NET Framework 字典的用户期望 Add 方法引发异常，而该情况会如期发生。 JavaScript 处理 .NET 异常。

> **请注意**  无法通过 JavaScript 代码显示异常消息。 消息文本替换为堆栈跟踪。 有关详细信息，请参阅在C#和 Visual Basic 中创建 Windows 运行时组件中的 "引发异常"。

相比之下，当 JavaScript 调用带有重复键的 insert 方法时，项的值已发生更改。 这种行为差异是由于 JavaScript 和 .NET 支持 Windows 运行时的不同方式所致，如[Windows 运行时组件C#和 Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md)中所述。

## <a name="returning-managed-types-from-your-component"></a>从组件返回托管类型

如前所述，你可以在 JavaScript 代码与 C# 或 Visual Basic 代码之间来回传递本机 Windows 运行时类型。 大多数情况下，类型名称和成员名称在两种情况下均相同（除了成员名称在 JavaScript 中以小写字母开头以外）。 但是在前面的部分中，PropertySet 类似乎在托管代码中具有不同的成员。 （例如，在 JavaScript 中调用 insert 方法，在 .NET 代码中调用了 Add 方法。）本部分将探讨这些差异对传递给 JavaScript .NET Framework 类型的影响方式。

除了返回在组件中创建的或通过 JavaScript 传递到组件的 Windows 运行时类型，还可以将使用托管代码创建的托管类型返回给 JavaScript，就像这是相应的 Windows 运行时类型一样。 即使在运行时类的第一个简单示例中，成员的参数和返回值也是 Visual Basic 或 C# 基元类型，即 .NET Framework 类型。 若要面向集合对此进行演示，请将以下代码添加到 Example 类，从而创建返回字符串泛型字典（通过整数索引）的方法：

> [!div class="tabbedCodeSnippets"]
> ```csharp
> public static IDictionary<int, string> GetMapOfNames()
> {
>     Dictionary<int, string> retval = new Dictionary<int, string>();
>     retval.Add(1, "one");
>     retval.Add(2, "two");
>     retval.Add(3, "three");
>     retval.Add(42, "forty-two");
>     retval.Add(100, "one hundred");
>     return retval;
> }
> ```
> ```vb
> Public Shared Function GetMapOfNames() As IDictionary(Of Integer, String)
>     Dim retval As New Dictionary(Of Integer, String)
>     retval.Add(1, "one")
>     retval.Add(2, "two")
>     retval.Add(3, "three")
>     retval.Add(42, "forty-two")
>     retval.Add(100, "one hundred")
>     Return retval
> End Function
> ```

请注意，字典必须作为由 [Dictionary&lt;TKey, TValue&gt;](/dotnet/api/system.collections.generic.dictionary-2) 实现且映射到 Windows 运行时接口的接口返回。 在此情况下，接口为 IDictionary&lt;int, string&gt;（在 Visual Basic 中为 IDictionary(Of Integer, String)）。 当 Windows 运行时类型 IMap&lt;int, string&gt; 传递到托管代码时，它显示为 IDictionary&lt;int, string&gt;，相反地当托管类型传递到 JavaScript 时也是如此。

**重要**  当托管类型实现多个接口时，JavaScript 使用列表中第一个出现的接口。 例如，如果你将 Dictionary&lt;int, string&gt; 返回到 JavaScript 代码，它会显示为 IDictionary&lt;int, string&gt;，无论你指定哪个接口作为返回类型都是如此。 这意味着，如果第一个接口不包括显示在后续接口上的成员，JavaScript 将看不到该成员。

 

若要测试新方法并使用字典，请将 returns1 和 returns2 函数添加到 default.js：

```javascript
var names;

function returns1() {
    names = SampleComponent.Example.getMapOfNames();
    document.getElementById('output').innerHTML = showMap(names);
}

var ct = 7;

function returns2() {
    if (!names.hasKey(17)) {
        names.insert(43, "forty-three");
        names.insert(17, "seventeen");
    }
    else {
        var err = names.insert("7", ct++);
        names.insert("forty", "forty");
    }
    document.getElementById('output').innerHTML = showMap(names);
}

function showMap(map) {
    var item = map.first();
    var retval = "<ul>";

    for (var i = 0, len = map.size; i < len; i++) {
        retval += "<li>" + item.current.key + ": " + item.current.value + "</li>";
        item.moveNext();
    }
    return retval + "</ul>";
}
```

将事件注册代码添加到相同文件，然后将其作为其他事件注册代码块：

```javascript
var returnsButton1 = document.getElementById("returnsButton1");
returnsButton1.addEventListener("click", returns1, false);
var returnsButton2 = document.getElementById("returnsButton2");
returnsButton2.addEventListener("click", returns2, false);
```

还有一些有趣的内容可观察有关这段 JavaScript 代码的信息。 首先，它包含一个 showMap 函数，用于在 HTML 中显示字典的内容。 在 showMap 的代码中，注意迭代模式。 在 .NET 中，泛型 IDictionary 接口上没有第一种方法，而大小由 Count 属性返回，而不是按大小方法返回。 对于 JavaScript，IDictionary&lt;int, string&gt; 显示为 Windows 运行时类型 IMap&lt;int, string&gt;。 （请参阅 [IMap&lt;K,V&gt;](/uwp/api/Windows.Foundation.Collections.IMap_K_V_) 接口。）

在 returns2 函数中，如前面的示例所示，JavaScript 将调用 Insert 方法（在 JavaScript 中为 insert）向字典添加项目。

若要运行应用，请选择 F5 键。 若要创建和显示字典的初始内容，请选择“Returns 1”按钮。 若要向字典添加两个以上的条目，请选择“Returns 2”按钮。 请注意，条目将按插入顺序显示，正如你对 Dictionary&lt;TKey, TValue&gt; 的期望一样。 如果你希望对它们进行排序，可以从 GetMapOfNames 返回 Dictionary&lt;TKey, TValue&gt;。 （前面示例使用的 PropertySet 类与 Dictionary&lt;TKey, TValue&gt; 所具有的内部组织不同。）

当然，JavaScript 不是强类型语言，因此使用强类型的泛型集合可能会导致意料之外的结果。 再次选择“Returns 2”按钮。 JavaScript 帮助你将“7”强制转换为数值 7，并将存储在 ct 中的数值 7 强制转换为字符串。 它还将字符串“forty”强制转换为零。 但这只是开始。 再选择几次“Returns 2”按钮。 在托管代码中，Add 方法将生成重复键异常，即使值已强制转换为正确类型也是如此。 相比之下，Insert 方法将更新与现有键关联的值，并返回一个布尔值，用于指示新键是否已添加到字典中。 这是与键 7 关联的值保持变化的原因。

另一种意外的行为：如果你将未分配的 JavaScript 变量作为字符串参数传递，你得到的将是字符串“undefined”。 简而言之，请谨慎将 .NET Framework 集合类型传递给你的 JavaScript 代码。

> **请注意**  如果有大量文本要连接，可以通过将代码移动到 .NET Framework 方法并使用 StringBuilder 类来更有效地执行此操作，如 showMap 函数所示。

尽管无法从 Windows 运行时组件公开你自己的泛型类型，但你可以使用如下所示的代码返回 Windows 运行时类的 .NET Framework 泛型集合：

> [!div class="tabbedCodeSnippets"]
> ```csharp
> public static object GetListOfThis(object obj)
> {
>     Type target = obj.GetType();
>     return Activator.CreateInstance(typeof(List<>).MakeGenericType(target));
> }
> ```
> ```vb
> Public Shared Function GetListOfThis(obj As Object) As Object
>     Dim target As Type = obj.GetType()
>     Return Activator.CreateInstance(GetType(List(Of )).MakeGenericType(target))
> End Function
> ```

List&lt;T&gt; 实现 IList&lt;T&gt;，这在 JavaScript 中显示为 Windows 运行时类型 IVector&lt;T&gt;。

## <a name="declaring-events"></a>声明事件


你可以使用标准 .NET Framework 事件模式或 Windows 运行时使用的其他模式声明事件。 .NET Framework 支持 System.EventHandler&lt;TEventArgs&gt; 委托与 Windows 运行时 EventHandler&lt;T&gt; 委托之间的等价性，因此使用 EventHandler&lt;TEventArgs&gt; 是实现标准 .NET Framework 模式的好方法。 若要查看此工作原理，请向 SampleComponent 项目添加以下两个类：

> [!div class="tabbedCodeSnippets"]
> ```csharp
> namespace SampleComponent
> {
>     public sealed class Eventful
>     {
>         public event EventHandler<TestEventArgs> Test;
>         public void OnTest(string msg, long number)
>         {
>             EventHandler<TestEventArgs> temp = Test;
>             if (temp != null)
>             {
>                 temp(this, new TestEventArgs()
>                 {
>                     Value1 = msg,
>                     Value2 = number
>                 });
>             }
>         }
>     }
>
>     public sealed class TestEventArgs
>     {
>         public string Value1 { get; set; }
>         public long Value2 { get; set; }
>     }
> }
> ```
> ```vb
> Public NotInheritable Class Eventful
>     Public Event Test As EventHandler(Of TestEventArgs)
>     Public Sub OnTest(ByVal msg As String, ByVal number As Long)
>         RaiseEvent Test(Me, New TestEventArgs() With {
>                             .Value1 = msg,
>                             .Value2 = number
>                             })
>     End Sub
> End Class
>
> Public NotInheritable Class TestEventArgs
>     Public Property Value1 As String
>     Public Property Value2 As Long
> End Class
> ```

在 Windows 运行时中公开某个事件时，该事件参数类将继承自 System.Object。 它不会像在 .NET 中那样从 System.object 继承，因为 EventArgs 不是 Windows 运行时类型。

如果为事件声明自定义事件访问器（在 Visual Basic 中为 **Custom** 关键字），则必须使用 Windows 运行时事件模式。 请参阅[Windows 运行时组件中的自定义事件和事件访问器](custom-events-and-event-accessors-in-windows-runtime-components.md)。

若要处理 Test 事件，请将 events1 函数添加到 default.js。 events1 函数为 Test 事件创建事件处理程序函数，并立即调用 OnTest 方法引发事件。 如果你在事件处理程序的主体中放置一个断点，你可以看到传递到单个参数的对象包含了源对象和 TestEventArgs 的两个成员。

```javascript
var ev;

function events1() {
   ev = new SampleComponent.Eventful();
   ev.addEventListener("test", function (e) {
       document.getElementById('output').innerHTML = e.value1;
       document.getElementById('output').innerHTML += "<br/>" + e.value2;
   });
   ev.onTest("Number of feet in a mile:", 5280);
}
```

将事件注册代码添加到相同文件，然后将其作为其他事件注册代码块：

```javascript
var events1Button = document.getElementById("events1Button");
events1Button.addEventListener("click", events1, false);
```

## <a name="exposing-asynchronous-operations"></a>公开异步操作


.NET Framework 具有一组丰富的工具，用于基于 Task 和泛型 [Task&lt;TResult&gt;](/dotnet/api/system.threading.tasks.task-1) 类进行异步处理和并行处理。 若要在 Windows 运行时组件中公开基于任务的异步处理，请使用 Windows 运行时接口 [IAsyncAction](/windows/desktop/api/windows.foundation/nn-windows-foundation-iasyncaction)、[IAsyncActionWithProgress&lt;TProgress&gt;](/previous-versions/br205784(v=vs.85))、[IAsyncOperation&lt;TResult&gt;](/previous-versions/br205802(v=vs.85)) 和 [IAsyncOperationWithProgress&lt;TResult, TProgress&gt;](/previous-versions/br205807(v=vs.85))。 （在 Windows 运行时中，操作会返回结果，但执行不会。）

此部分演示了一个可取消的用于报告进度和返回结果的异步操作。 GetPrimesInRangeAsync 方法使用 [AsyncInfo](/dotnet/api/system.runtime.interopservices.windowsruntime) 类生成一个任务并将其取消和进度报告功能连接到 WinJS.Promise 对象。 首先将 GetPrimesInRangeAsync 方法添加到示例类：

> [!div class="tabbedCodeSnippets"]
> ```csharp
> using System.Runtime.InteropServices.WindowsRuntime;
> using Windows.Foundation;
>
> public static IAsyncOperationWithProgress<IList<long>, double>
> GetPrimesInRangeAsync(long start, long count)
> {
>     if (start < 2 || count < 1) throw new ArgumentException();
>
>     return AsyncInfo.Run<IList<long>, double>((token, progress) =>
>
>         Task.Run<IList<long>>(() =>
>         {
>             List<long> primes = new List<long>();
>             double onePercent = count / 100;
>             long ctProgress = 0;
>             double nextProgress = onePercent;
>
>             for (long candidate = start; candidate < start + count; candidate++)
>             {
>                 ctProgress += 1;
>                 if (ctProgress >= nextProgress)
>                 {
>                     progress.Report(ctProgress / onePercent);
>                     nextProgress += onePercent;
>                 }
>                 bool isPrime = true;
>                 for (long i = 2, limit = (long)Math.Sqrt(candidate); i <= limit; i++)
>                 {
>                     if (candidate % i == 0)
>                     {
>                         isPrime = false;
>                         break;
>                     }
>                 }
>                 if (isPrime) primes.Add(candidate);
>
>                 token.ThrowIfCancellationRequested();
>             }
>             progress.Report(100.0);
>             return primes;
>         }, token)
>     );
> }
> ```
> ```vb
> Imports System.Runtime.InteropServices.WindowsRuntime
>
> Public Shared Function GetPrimesInRangeAsync(ByVal start As Long, ByVal count As Long)
> As IAsyncOperationWithProgress(Of IList(Of Long), Double)
>
>     If (start < 2 Or count < 1) Then Throw New ArgumentException()
>
>     Return AsyncInfo.Run(Of IList(Of Long), Double)( _
>         Function(token, prog)
>             Return Task.Run(Of IList(Of Long))( _
>                 Function()
>                     Dim primes As New List(Of Long)
>                     Dim onePercent As Long = count / 100
>                     Dim ctProgress As Long = 0
>                     Dim nextProgress As Long = onePercent
>
>                     For candidate As Long = start To start + count - 1
>                         ctProgress += 1
>
>                         If ctProgress >= nextProgress Then
>                             prog.Report(ctProgress / onePercent)
>                             nextProgress += onePercent
>                         End If
>
>                         Dim isPrime As Boolean = True
>                         For i As Long = 2 To CLng(Math.Sqrt(candidate))
>                             If (candidate Mod i) = 0 Then
>                                 isPrime = False
>                                 Exit For
>                             End If
>                         Next
>
>                         If isPrime Then primes.Add(candidate)
>
>                         token.ThrowIfCancellationRequested()
>                     Next
>                     prog.Report(100.0)
>                     Return primes
>                 End Function, token)
>         End Function)
> End Function
> ```

GetPrimesInRangeAsync 是非常简单的质数查找程序，通过设计实现。 此处的重点是实现异步操作，因此简单性非常重要，并且当我们演示取消时，较慢的实现很有优势。 GetPrimesInRangeAsync 通过暴力方式查找质数：它将候选数除以所有小于或等于其平方根的整数，而不是只使用质数。 逐步执行此代码：

-   在开始异步操作之前执行整理活动，例如验证参数和针对无效输入引发异常。
-   此实现的关键是 [AsyncInfo.Run&lt;TResult, TProgress&gt;(Func&lt;CancellationToken, IProgress&lt;TProgress&gt;, Task&lt;TResult&gt;](/dotnet/api/system.runtime.interopservices.windowsruntime)&gt;) 方法，以及作为该方法唯一参数的委托。 委托必须接受取消令牌和报告进度的接口，并且必须返回使用这些参数的启动任务。 当 JavaScript 调用 GetPrimesInRangeAsync 方法时，将执行以下步骤（不一定按照此处提供的顺序）：

    -   [WinJS.Promise](/previous-versions/windows/apps/br211867(v=win.10)) 对象提供用于处理返回结果、响应取消和处理进度报告的函数。
    -   AsyncInfo.Run 方法创建一个取消源和一个实现 IProgress&lt;T&gt; 接口的对象。 对于委托，它将同时传递取消源中的 [CancellationToken](/dotnet/api/system.threading.cancellationtoken) 令牌，以及 [IProgress&lt;T&gt;](/dotnet/api/system.iprogress-1) 接口。

        > **请注意**  如果承诺对象不提供用于对取消做出反应的函数，则 system.runtime.interopservices.windowsruntime.asyncinfo 仍会传递可取消的令牌，并且仍可能会发生取消。 如果 Promise 对象未提供处理进度更新的函数，AsyncInfo.Run 仍提供实现 IProgress&lt;T&gt; 的对象，但将忽略报告。

    -   委托使用 [Task.Run&lt;TResult&gt;(Func&lt;TResult&gt;, CancellationToken](/dotnet/api/system.threading.tasks.task.run#System_Threading_Tasks_Task_Run__1_System_Func___0__System_Threading_CancellationToken_)) 方法创建使用令牌和进度接口的启动任务。 启动任务的委托由计算所需结果的 lambda 函数提供。 稍后对此进行详细讨论。
    -   AsyncInfo.Run 方法创建实现 [IAsyncOperationWithProgress&lt;TResult, TProgress&gt;](/uwp/api/Windows.Foundation.IAsyncOperationWithProgress_TResult_TProgress_) 接口的对象、将 Windows 运行时取消机制与令牌源连接，并将 Promise 对象的进度报告函数与 IProgress&lt;T&gt; 接口连接。
    -   IAsyncOperationWithProgress&lt;TResult, TProgress&gt; 接口将返回到 JavaScript。

-   由启动任务表示的 lambda 函数不使用任何参数。 由于它是 lambda 函数，它将有权访问令牌和 IProgress 接口。 每次计算候选数量时，lambda 函数将执行以下操作：

    -   查看是否已达到进度的下一百分点。 如果已达到，lambda 函数将调用 IProgress&lt;T&gt;.Report 方法，并且百分比将传递到 Promise 对象为报告进度指定的函数。
    -   如果操作已取消，使用取消令牌引发异常。 如果 [IAsyncInfo.Cancel](/uwp/api/windows.foundation.iasyncinfo.cancel) 方法（IAsyncOperationWithProgress&lt;TResult, TProgress&gt; 接口继承的方法）已调用，AsyncInfo.Run 方法设置的连接将确保取消令牌收到通知。
-   当 lambda 函数返回质数列表时，该列表将传递给 WinJS.Promise 对象为处理结果所指定的函数。

若要创建 JavaScript Promise 并设置取消机制，请将 asyncRun 和 asyncCancel 函数添加到 default.js。

```javascript
var resultAsync;
function asyncRun() {
    document.getElementById('output').innerHTML = "Retrieving prime numbers.";
    btnAsync.disabled = "disabled";
    btnCancel.disabled = "";

    resultAsync = SampleComponent.Example.getPrimesInRangeAsync(10000000000001, 2500).then(
        function (primes) {
            for (i = 0; i < primes.length; i++)
                document.getElementById('output').innerHTML += " " + primes[i];

            btnCancel.disabled = "disabled";
            btnAsync.disabled = "";
        },
        function () {
            document.getElementById('output').innerHTML += " -- getPrimesInRangeAsync was canceled. -- ";

            btnCancel.disabled = "disabled";
            btnAsync.disabled = "";
        },
        function (prog) {
            document.getElementById('primeProg').value = prog;
        }
    );
}

function asyncCancel() {    
    resultAsync.cancel();
}
```

不要忘记事件注册代码，像你之前操作的那样。

```javascript
var btnAsync = document.getElementById("btnAsync");
btnAsync.addEventListener("click", asyncRun, false);
var btnCancel = document.getElementById("btnCancel");
btnCancel.addEventListener("click", asyncCancel, false);
```

通过调用异步 GetPrimesInRangeAsync 方法，asyncRun 函数将创建 WinJS.Promise 对象。 然后，该对象将使用用于处理返回结果、响应错误（包括取消）和处理进度报告的三个函数。 在本例中，返回结果在输出区域中输出。 取消或完成将重置启动和取消操作的按钮。 进度报告将更新进度控件。

asyncCancel 函数只调用 WinJS.Promise 对象的取消方法。

若要运行应用，请选择 F5 键。 若要启动异步操作，请选择“异步”按钮。 下一步将发生什么情况取决于你的计算机速度有多快。 如果进度栏快速完成以至于你来不及看到此过程，请按十的一个或多个因数增加传递到 GetPrimesInRangeAsync 的起始数字大小。 你可以通过增加或减少要测试的数字计数，以微调操作的持续时间，但在起始数字的中间添加零将会产生更大的影响。 若要取消操作，请选择“取消异步”按钮。

## <a name="related-topics"></a>相关主题

* [适用于 UWP 应用的 .NET 概述](/previous-versions/windows/apps/br230302(v=vs.140))
* [适用于 UWP 应用的 .NET](/dotnet/api/index?view=dotnet-uwp-10.0)