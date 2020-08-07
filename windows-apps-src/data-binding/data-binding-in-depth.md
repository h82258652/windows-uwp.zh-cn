---
ms.assetid: 41E1B4F1-6CAF-4128-A61A-4E400B149011
title: 深入了解数据绑定
description: 数据绑定是你的应用 UI 用来显示数据的一种方法，可以选择与该数据保持同步。
ms.date: 10/05/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
ms.openlocfilehash: ee5985cc92dfc580c50d02f17c6d9f5777e8928d
ms.sourcegitcommit: dccba7256765116f06cf96143eb3cbaa12d7fe0b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/03/2020
ms.locfileid: "87523553"
---
# <a name="data-binding-in-depth"></a>深入了解数据绑定

**重要的 API**

-   [ **{x:Bind} 标记扩展**](../xaml-platform/x-bind-markup-extension.md)
-   [**Binding 类**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding)
-   [**DataContext**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.datacontext)
-   [**INotifyPropertyChanged**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.INotifyPropertyChanged)

> [!NOTE]
> 本主题详细介绍数据绑定功能。 有关简短、实用的简介，请参阅[数据绑定概述](data-binding-quickstart.md)。

本主题介绍通用 Windows 平台 (UWP) 应用程序中的数据绑定。 此处讨论的 API 位于 [Windows.UI.Xaml.Data 命名空间](/uwp/api/windows.ui.xaml.data)中  。

数据绑定是你的应用 UI 用来显示数据的一种方法，可以选择与该数据保持同步。 借助数据绑定，你可以将关注的数据从关注的 UI 中分离开来，从而形成一个更简易的概念模型，并且使应用具有更好的可读性、可测试性和可维护性。

当 UI首次显示时，你可以使用数据绑定以仅显示来自数据源的值，但不会对这些值中的更改做出响应。 这是一种称为“一次性”的绑定模式，非常适合运行时未更改的值  。 或者，你可以选择“观察”值并在其更改时更新 UI。 此模式称为“单向”，非常适合只读数据  。 最后，你可以选择观察并更新，以便用户在 UI 中对值所做的更改能自动传回数据源。 此模式称为“双向”，非常适合读写数据  。 下面是一些示例。

-   你可以使用一次性模式，将 [Image](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image) 绑定到当前用户的照片  。
-   你可以使用单向模式，将 [ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) 绑定到按报纸剪辑分组的实时新闻报道的集合  。
-   你可以使用双向模式，将 [TextBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) 绑定到表单中的客户名称  。

无论模式如何，都有两种类型的绑定，它们通常都在 UI 标记中进行声明。 你既可以选用 [{x:Bind} 标记扩展](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)，也可以选用 [{Binding} 标记扩展](https://docs.microsoft.com/windows/uwp/xaml-platform/binding-markup-extension)。 还可以在同一应用中（甚至是同一 UI 元素上）混合使用这两个标记扩展。 {x:Bind} 是 Windows 10 的新增内容，并且具备更好的性能。 除非另有明确说明，否则本主题中所述的所有详细信息均适用于这两种绑定。

**用于演示 {x:Bind} 的应用示例**

-   [{x:Bind} 示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlBind)。
-   [QuizGame](https://github.com/microsoft/Windows-appsample-networkhelper)。
-   [XAML UI 基本示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics)。

**用于演示 {Binding} 的应用示例**

-   下载 [Bookstore1](https://codeload.github.com/MicrosoftDocs/windows-topic-specific-samples/zip/Bookstore1Universal_10) 应用。
-   下载 [Bookstore2](https://codeload.github.com/MicrosoftDocs/windows-topic-specific-samples/zip/Bookstore2Universal_10) 应用。

## <a name="every-binding-involves-these-pieces"></a>每个绑定均由以下部分构成

-   *绑定源*。 这是用于绑定的数据源，它可以是任意类的实例，其中该类具有其值要显示在 UI 中的成员。
-   *绑定目标*。 这是显示数据的 UI 中 [**FrameworkElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyProperty) 的 [**DependencyProperty**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.FrameworkElement)。
-   *绑定对象*。 这是用于将数据值从绑定源传递给绑定目标（或从绑定目标传递回绑定源）的组件。 绑定对象通过 [{x:Bind}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension) 或 [{Binding}](https://docs.microsoft.com/windows/uwp/xaml-platform/binding-markup-extension) 标记扩展在 XAML 加载时进行创建。

在以下部分中，我们将进一步探讨绑定源、绑定目标和绑定对象。 我们将这些部分与关于将按钮内容绑定到名为 **NextButtonText** 的字符串属性（这属于名为 **HostViewModel** 的类）的示例链接在一起。

### <a name="binding-source"></a>绑定源

下面是一个非常基础的可用作绑定源的类实现。

如果使用的是 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)，请将新的 Midl 文件 (.idl) 项添加到项目中，如下面 C++/WinRT 代码示例清单中所示对其进行命名  。 将这些新文件的内容替换为列表中显示的 [MIDL 3.0](/uwp/midl-3/intro) 代码，生成项目以生成 `HostViewModel.h` 和 `.cpp`，然后将代码添加到生成的文件以匹配列表。 有关这些生成的文件以及如何将它们复制到项目中的详细信息，请参阅 [XAML 控件；绑定到 C++/WinRT 属性](/windows/uwp/cpp-and-winrt-apis/binding-property)。

```csharp
public class HostViewModel
{
    public HostViewModel()
    {
        this.NextButtonText = "Next";
    }

    public string NextButtonText { get; set; }
}
```

```cppwinrt
// HostViewModel.idl
namespace DataBindingInDepth
{
    runtimeclass HostViewModel
    {
        HostViewModel();
        String NextButtonText;
    }
}

// HostViewModel.h
// Implement the constructor like this, and add this field:
...
HostViewModel() : m_nextButtonText{ L"Next" } {}
...
private:
    std::wstring m_nextButtonText;
...

// HostViewModel.cpp
// Implement like this:
...
hstring HostViewModel::NextButtonText()
{
    return hstring{ m_nextButtonText };
}

void HostViewModel::NextButtonText(hstring const& value)
{
    m_nextButtonText = value;
}
...
```

**HostViewModel** 及其属性 **NextButtonText** 的实现仅适用于一次性绑定。 而单向绑定和双向绑定也极为常见，在这两种绑定中，UI 会自动更新以响应绑定源的数据值的更改。 为了让这些类型的绑定能正常工作，需要使绑定源对绑定对象“可观察”。 因此在我们的示例中，如果我们希望单向或双向绑定到 **NextButtonText** 属性，则该属性值在运行时所发生的任何更改均需对绑定对象可观察。

执行此操作的方法之一是，从 [**DependencyObject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject) 派生代表绑定源的类，并通过 [**DependencyProperty**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyProperty) 公开数据值。 以上就是 [**FrameworkElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.FrameworkElement) 成为可观察绑定源的方式。 **FrameworkElements** 即时可用，是很好的绑定源。

让类成为可观察绑定源，以及成为类（已拥有基类）的必需项的更便捷方法是实现 [**System.ComponentModel.INotifyPropertyChanged**](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged)。 实际上，这仅涉及到实现一个名为 **PropertyChanged** 的事件。 下面展示了使用 **HostViewModel** 的示例。

```csharp
...
using System.ComponentModel;
using System.Runtime.CompilerServices;
...
public class HostViewModel : INotifyPropertyChanged
{
    private string nextButtonText;

    public event PropertyChangedEventHandler PropertyChanged = delegate { };

    public HostViewModel()
    {
        this.NextButtonText = "Next";
    }

    public string NextButtonText
    {
        get { return this.nextButtonText; }
        set
        {
            this.nextButtonText = value;
            this.OnPropertyChanged();
        }
    }

    public void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        // Raise the PropertyChanged event, passing the name of the property whose value has changed.
        this.PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

```cppwinrt
// HostViewModel.idl
namespace DataBindingInDepth
{
    runtimeclass HostViewModel : Windows.UI.Xaml.Data.INotifyPropertyChanged
    {
        HostViewModel();
        String NextButtonText;
    }
}

// HostViewModel.h
// Add this field:
...
    winrt::event_token PropertyChanged(Windows::UI::Xaml::Data::PropertyChangedEventHandler const& handler);
    void PropertyChanged(winrt::event_token const& token) noexcept;

private:
    winrt::event<Windows::UI::Xaml::Data::PropertyChangedEventHandler> m_propertyChanged;
...

// HostViewModel.cpp
// Implement like this:
...
void HostViewModel::NextButtonText(hstring const& value)
{
    if (m_nextButtonText != value)
    {
        m_nextButtonText = value;
        m_propertyChanged(*this, Windows::UI::Xaml::Data::PropertyChangedEventArgs{ L"NextButtonText" });
    }
}

winrt::event_token HostViewModel::PropertyChanged(Windows::UI::Xaml::Data::PropertyChangedEventHandler const& handler)
{
    return m_propertyChanged.add(handler);
}

void HostViewModel::PropertyChanged(winrt::event_token const& token) noexcept
{
    m_propertyChanged.remove(token);
}
...
```

现在，**NextButtonText** 属性可供观察。 当创作到该属性的单向或双向绑定（将在稍后介绍操作方法）时，生成的绑定对象将订阅 **PropertyChanged** 事件。 引发该事件后，绑定对象的处理程序将接收一个包含已更改的属性名的参数。 这是绑定对象知道已更改和重新读取的属性值的方式。

这样你便无需多次实现上面所示的模式，如果使用的是 C#，则只需从 [QuizGame](https://github.com/microsoft/Windows-appsample-networkhelper) 示例（位于“Common”文件夹中）提供的 BindableBase 基类派生即可。 下面是具体操作的一个示例。

```csharp
public class HostViewModel : BindableBase
{
    private string nextButtonText;

    public HostViewModel()
    {
        this.NextButtonText = "Next";
    }

    public string NextButtonText
    {
        get { return this.nextButtonText; }
        set { this.SetProperty(ref this.nextButtonText, value); }
    }
}
```

```cppwinrt
// Your BindableBase base class should itself derive from Windows::UI::Xaml::DependencyObject. Then, in HostViewModel.idl, derive from BindableBase instead of implementing INotifyPropertyChanged.
```

> [!NOTE]
> 对于 C++/WinRT，从基类派生的任何运行时类（在应用程序中声明）都称为可组合类  。 且可组合类存在一些限制。 若要使应用程序通过 Visual Studio 和 Microsoft Store 用于验证提交的 [Windows 应用认证工具包](../debug-test-perf/windows-app-certification-kit.md)测试，使 Microsoft Store 可成功纳入该应用程序，可组合类必须最终派生自 Windows 基类。 这意味着继承层次结构的根类必须是源于 Windows.* 名称空间的类型。 如果确实需要从基类派生运行时类（例如，为要从中派生的所有视图模型实现 BindableBase 类），则可以从 [Windows.UI.Xaml.DependencyObject](/uwp/api/windows.ui.xaml.dependencyobject) 派生   。

通过使用 [**String.Empty**](https://docs.microsoft.com/dotnet/api/system.string.empty) 或 **null** 参数引发 **PropertyChanged** 事件，以指示对象上的所有非索引器属性应重新读取。 可以通过将“Item\[indexer\]”的参数用于特定索引器（其中 indexer 是索引值），或将“Item\[\]”的值用于所有索引器，引发该事件以指示对象上的索引器属性已更改   。

可以将绑定源视为其属性中包含数据的单个对象或一些对象的集合。 在 C# 和 Visual Basic 代码中，可以一次性绑定到实现 [List(Of T)](https://docs.microsoft.com/dotnet/api/system.collections.generic.list-1) 的对象，以显示运行时未更改的集合  。 对于可观察集合（在将项添加到集合或从集合中删除时进行观察），应改为单向绑定到 [**ObservableCollection(Of T)** ](https://docs.microsoft.com/dotnet/api/system.collections.objectmodel.observablecollection-1)。 在 C++/CX 代码中，对于可观察集合和不可观察集合，均可绑定到 [Vector&lt;T&gt;](https://docs.microsoft.com/cpp/cppcx/platform-collections-vector-class)，并且 C++/WinRT 具有其自己的类型  。 若要绑定到你自己的集合类，请按照下表中的指南进行操作。

|方案|C# 和 VB (CLR)|C++/WinRT|C++/CX|
|-|-|-|-|
|绑定到对象。|可以为任何对象。|可以为任何对象。|对象必须具有 [**BindableAttribute**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.BindableAttribute) 或实现 [**ICustomPropertyProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.ICustomPropertyProvider)。|
|从绑定对象中获取属性更改通知。|对象必须实现 [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged)  。| 对象必须实现 [INotifyPropertyChanged](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.INotifyPropertyChanged)  。|对象必须实现 [INotifyPropertyChanged](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.INotifyPropertyChanged)  。|
|绑定到集合。| [**List(Of T)** ](https://docs.microsoft.com/dotnet/api/system.collections.generic.list-1)|[IInspectable](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) 或 [IBindableObservableVector](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector) 的 [IVector](/uwp/api/windows.foundation.collections.ivector_t_)    。 请参阅 [XAML 项目控件；绑定到 C++/WinRT 集合](../cpp-and-winrt-apis/binding-collection.md)和[使用 C++/WinRT 的集合](../cpp-and-winrt-apis/collections.md)。| [**Vector&lt;T&gt;** ](https://docs.microsoft.com/cpp/cppcx/platform-collections-vector-class)|
|从绑定集合中获取集合更改通知。|[**ObservableCollection(Of T)** ](https://docs.microsoft.com/dotnet/api/system.collections.objectmodel.observablecollection-1)|[IInspectable](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) 的 [IObservableVector](/uwp/api/windows.foundation.collections.iobservablevector_t_)   。 例如，[winrt::single_threaded_observable_vector&lt;T&gt;](https://docs.microsoft.com/uwp/cpp-ref-for-winrt/single-threaded-observable-vector)  。|[**IObservableVector&lt;T&gt;** ](https://docs.microsoft.com/uwp/api/Windows.Foundation.Collections.IObservableVector_T_)。  [Vector&lt;T&gt;](https://docs.microsoft.com/cpp/cppcx/platform-collections-vector-class) 实现了此接口  。|
|实现支持绑定的集合。|扩展 [**List(Of T)** ](https://docs.microsoft.com/dotnet/api/system.collections.generic.list-1) 或者实现 [**IList**](https://docs.microsoft.com/dotnet/api/system.collections.ilist)、[**IList**](https://docs.microsoft.com/dotnet/api/system.collections.generic.ilist-1)(Of [**Object**](https://docs.microsoft.com/dotnet/api/system.object))、[**IEnumerable**](https://docs.microsoft.com/dotnet/api/system.collections.ienumerable) 或 [**IEnumerable**](https://docs.microsoft.com/dotnet/api/system.collections.generic.ienumerable-1)(Of **Object**)。 绑定到通用 **IList(Of T)** 和 **IEnumerable(Of T)** 的操作不受支持。|实现 [IInspectable](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) 的 [IVector](/uwp/api/windows.foundation.collections.ivector_t_)   。 请参阅 [XAML 项目控件；绑定到 C++/WinRT 集合](../cpp-and-winrt-apis/binding-collection.md)和[使用 C++/WinRT 的集合](../cpp-and-winrt-apis/collections.md)。|实现 [IBindableVector](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Interop.IBindableVector)、[IBindableIterable](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Interop.IBindableIterable)、[IVector](https://docs.microsoft.com/uwp/api/Windows.Foundation.Collections.IVector_T_)&lt;[Object](https://docs.microsoft.com/dotnet/api/system.object)^&gt;、[IIterable](https://docs.microsoft.com/uwp/api/Windows.Foundation.Collections.IIterable_T_)&lt;Object^&gt;、IVector&lt;[IInspectable](https://docs.microsoft.com/windows/desktop/api/inspectable/nn-inspectable-iinspectable)\*&gt; 或 IIterable&lt;IInspectable\*&gt;           。 绑定到通用 **IVector&lt;T&gt;** 和 **IIterable&lt;T&gt;** 的操作不受支持。|
| 实现支持集合更改通知的集合。 | 扩展 [**ObservableCollection(Of T)** ](https://docs.microsoft.com/dotnet/api/system.collections.objectmodel.observablecollection-1) 或实现（非通用）[**IList**](https://docs.microsoft.com/dotnet/api/system.collections.ilist) 和 [**INotifyCollectionChanged**](https://docs.microsoft.com/dotnet/api/system.collections.specialized.inotifycollectionchanged)。|实现 [IInspectable](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) 或 [IBindableObservableVector](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector) 的 [IObservableVector](/uwp/api/windows.foundation.collections.iobservablevector_t_)    。|实现 [**IBindableVector**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Interop.IBindableVector) 和 [**IBindableObservableVector**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Interop.IBindableObservableVector)。|
|实现支持增量加载的集合。|扩展 [**ObservableCollection(Of T)** ](https://docs.microsoft.com/dotnet/api/system.collections.objectmodel.observablecollection-1) 或实现（非通用）[**IList**](https://docs.microsoft.com/dotnet/api/system.collections.ilist) 和 [**INotifyCollectionChanged**](https://docs.microsoft.com/dotnet/api/system.collections.specialized.inotifycollectionchanged)。 此外，还实现 [**ISupportIncrementalLoading**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.ISupportIncrementalLoading)。|实现 [IInspectable](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) 或 [IBindableObservableVector](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector) 的 [IObservableVector](/uwp/api/windows.foundation.collections.iobservablevector_t_)    。 此外，还实现 [ISupportIncrementalLoading](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.ISupportIncrementalLoading) |实现 [**IBindableVector**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Interop.IBindableVector)、[**IBindableObservableVector**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Interop.IBindableObservableVector) 和 [**ISupportIncrementalLoading**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.ISupportIncrementalLoading)。|

你可以使用增量加载将列表控件绑定到任意的大型数据源，且仍获得高性能。 例如，你可以将列表控件绑定到必应图像查询结果，而无需一次性加载所有结果。 你只需立即加载部分结果，再根据需要加载其他结果。 为支持增量加载，你必须在支持集合更改通知的数据源上实现 [ISupportIncrementalLoading](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.ISupportIncrementalLoading)  。 当数据绑定引擎请求更多数据时，你的数据源必须发出相应的请求、集成结果，然后发送相应的通知以更新 UI。

### <a name="binding-target"></a>绑定目标

在下面的两个示例中，Button.Content  属性是绑定目标，并且其值已设置为用于声明绑定对象的标记扩展。 首先显示的是 [{x:Bind}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)，然后是 [{Binding}](https://docs.microsoft.com/windows/uwp/xaml-platform/binding-markup-extension)。 在标记中声明绑定这一做法很常见（其既便捷、易读又很实用）。 不过，应避免标记和以命令性方式（编程方式）创建 [**Binding**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding) 类的实例（除非需要）。

```xaml
<Button Content="{x:Bind ...}" ... />
```

```xaml
<Button Content="{Binding ...}" ... />
```

如果使用的是 C++/WinRT 或 Visual C++ 组件扩展 (C++/CX)，则需要将 [BindableAttribute](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.BindableAttribute) 属性添加到要使用 [{Binding}](https://docs.microsoft.com/windows/uwp/xaml-platform/binding-markup-extension) 标记扩展的运行时类  。

> [!IMPORTANT]
> 如果使用 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)，则在安装了 Windows SDK 版本 10.0.17763.0（Windows 10 版本 1809）或更高版本的情况下，可使用 [BindableAttribute](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.BindableAttribute) 属性  。 如果没有该属性，为了能够使用 [{Binding}](https://docs.microsoft.com/windows/uwp/xaml-platform/binding-markup-extension) 标记扩展，需要实现 [ICustomPropertyProvider](/uwp/api/windows.ui.xaml.data.icustompropertyprovider) 和 [ICustomProperty](/uwp/api/windows.ui.xaml.data.icustomproperty) 接口。

### <a name="binding-object-declared-using-xbind"></a>使用 {x:Bind} 声明的绑定对象

在创作 [{x:Bind}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension) 标记之前，需先执行一个步骤。 我们需要从表示标记页面的类公开绑定源类。 通过将相关属性（本例中为 HostViewModel 类型）添加到 MainPage 页面类，来执行此操作   。

```csharp
namespace DataBindingInDepth
{
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
            this.ViewModel = new HostViewModel();
        }
    
        public HostViewModel ViewModel { get; set; }
    }
}
```

```cppwinrt
// MainPage.idl
import "HostViewModel.idl";

namespace DataBindingInDepth
{
    runtimeclass MainPage : Windows.UI.Xaml.Controls.Page
    {
        MainPage();
        HostViewModel ViewModel{ get; };
    }
}

// MainPage.h
// Include a header, and add this field:
...
#include "HostViewModel.h"
...
    DataBindingInDepth::HostViewModel ViewModel();

private:
    DataBindingInDepth::HostViewModel m_viewModel{ nullptr };
...

// MainPage.cpp
// Implement like this:
...
MainPage::MainPage()
{
    InitializeComponent();

}

DataBindingInDepth::HostViewModel MainPage::ViewModel()
{
    return m_viewModel;
}
...
```

该操作完成后，即可仔细看看声明绑定对象的标记。 下面的示例使用与前面“绑定目标”部分中所使用的相同 **Button.Content** 绑定目标，并演示了将其绑定到 **HostViewModel.NextButtonText** 属性。

```xaml
<!-- MainPage.xaml -->
<Page x:Class="DataBindingInDepth.Mainpage" ... >
    <Button Content="{x:Bind Path=ViewModel.NextButtonText, Mode=OneWay}" ... />
</Page>
```

注意，该值即是我们为 **Path** 指定的值。 此值在页面的上下文中自行解释，在本例中路径首先用于引用我们刚添加到 MainPage 页面的 ViewModel 属性   。 该属性将返回 **HostViewModel** 实例，这样我们便可以点入该对象以访问 **HostViewModel.NextButtonText** 属性。 并且，我们将指定 **Mode** 以替代一次性绑定默认值 [{x:Bind}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)。

[  **Path**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.path) 属性支持各种用于绑定到嵌套属性、附加属性以及整数和字符串索引器的语法选项。 有关详细信息，请参阅 [Property-path 语法](https://docs.microsoft.com/windows/uwp/xaml-platform/property-path-syntax)。 绑定到字符串索引器会为你提供绑定到动态属性的效果，而无需实现 [**ICustomPropertyProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.ICustomPropertyProvider)。 有关其他设置，请参阅 [{x:Bind} 标记扩展](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)。

若要说明 HostViewModel.NextButtonText 属性确实可观察，请将 Click 事件处理程序添加到按钮，然后更新 HostViewModel.NextButtonText 的值    。 生成、运行并单击按钮以查看按钮的内容更新的值  。

```csharp
// MainPage.xaml.cs
private void Button_Click(object sender, RoutedEventArgs e)
{
    this.ViewModel.NextButtonText = "Updated Next button text";
}
```

```cppwinrt
// MainPage.cpp
void MainPage::ClickHandler(IInspectable const&, RoutedEventArgs const&)
{
    ViewModel().NextButtonText(L"Updated Next button text");
}
```

> [!NOTE]
> 当 [TextBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) 失去焦点时，对 [TextBox.Text](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.text) 的更改将发送到双向绑定源，这并非发生在每次用户按键之后   。

**DataTemplate 和 x:DataType**

在 [**DataTemplate**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DataTemplate) 内（不论用作项模板、内容模板还是标头模板），**Path** 的值不在页面的上下文中进行解释，而在模板化的数据对象的上下文中进行解释。 在数据模板中使用 {x:Bind} ，以便在编译时可以对它的绑定进行验证（同时为它们生成有效代码）时，**DataTemplate** 需要使用 **x:DataType** 声明其数据对象的类型。 下面给出的示例可用作绑定到 **SampleDataGroup** 对象集合的项目控件的 **ItemTemplate**。

```xaml
<DataTemplate x:Key="SimpleItemTemplate" x:DataType="data:SampleDataGroup">
    <StackPanel Orientation="Vertical" Height="50">
      <TextBlock Text="{x:Bind Title}"/>
      <TextBlock Text="{x:Bind Description}"/>
    </StackPanel>
  </DataTemplate>
```

**你的路径中的弱类型对象**

例如，假设你有一个名为 SampleDataGroup 的类型，该类型实现名为 Title 的字符串属性。 并且你有一个属性 MainPage.SampleDataGroupAsObject，该属性属于类型对象但它实际上返回 SampleDataGroup 的实例。 由于在该类型对象上找不到 Title 属性，因此绑定 `<TextBlock Text="{x:Bind SampleDataGroupAsObject.Title}"/>` 将导致编译错误。 该错误的解决方法是向 Path 语法添加强制转换，如下所示：`<TextBlock Text="{x:Bind ((data:SampleDataGroup)SampleDataGroupAsObject).Title}"/>`。 下面是另一个示例，其中元素声明为对象但实际上是 TextBlock：`<TextBlock Text="{x:Bind Element.Text}"/>`。 而且强制转换可以解决该问题：`<TextBlock Text="{x:Bind ((TextBlock)Element).Text}"/>`。

**如果你的数据异步加载**

在编译时为你的页面在分部类中生成支持 **{x:Bind}** 的代码。 可以在 `obj` 文件夹中找到这些文件，其名称类似于（适用于 C#）`<view name>.g.cs`。 生成的代码包含可用于你的页面的 [**Loading**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.FrameworkElement) 事件的处理程序，并且该处理程序在表示你的页面绑定的已生成类上调用 **Initialize** 方法。 **Initialize** 依次调用 **Update**，以开始在绑定源和目标之间移动数据。 只在页面或用户控件的第一个测量阶段之前引发 **Loading**。 因此异步加载你的数据时，可能未在调用 **Initialize** 时做好准备。 因此，加载数据之后，可以通过调用 `this.Bindings.Update();` 强制初始化一次性绑定。 如果只需针对异步加载的数据执行一次性绑定，则使用此方法对其进行初始化比使用以下方法方便：首先进行单向绑定，然后侦听更改。 如果你的数据未经过细微的更改并且它可能作为某个特定操作的一部分进行更新，则你可以执行一次性绑定，并且通过对 **Update** 的调用随时强制执行手动更新。

> [!NOTE]
> {x:Bind} 既不适用于后期绑定方案（例如导航 JSON 对象的字典结构），也不适用于鸭子类型  。 “鸭子类型”是基于属性名称的词法匹配的一种较弱形式的类型（“如果它的走路方式、游泳方式和叫声像鸭子，那么它就是一只鸭子”）。 借助鸭子类型，与 Age 属性的绑定将会同样满足 Person 或 Wine 对象（假定这些类型都具有 Age 属性）     。 对于这些情况，请使用 {Binding}  标记扩展。

### <a name="binding-object-declared-using-binding"></a>使用 {Binding} 声明的绑定对象

如果使用的是 C++/WinRT 或 Visual C++ 组件扩展 (C++/CX)，要使用 [{Binding}](https://docs.microsoft.com/windows/uwp/xaml-platform/binding-markup-extension) 标记扩展，则需要将 [BindableAttribute](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.BindableAttribute) 属性添加到要绑定到的运行时类  。 若要使用 [{x:Bind}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)，则不需要该属性。

```cppwinrt
// HostViewModel.idl
// Add this attribute:
[Windows.UI.Xaml.Data.Bindable]
runtimeclass HostViewModel : Windows.UI.Xaml.Data.INotifyPropertyChanged
...
```

> [!IMPORTANT]
> 如果使用 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)，则在安装了 Windows SDK 版本 10.0.17763.0（Windows 10 版本 1809）或更高版本的情况下，可使用 [BindableAttribute](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.BindableAttribute) 属性  。 如果没有该属性，为了能够使用 [{Binding}](https://docs.microsoft.com/windows/uwp/xaml-platform/binding-markup-extension) 标记扩展，需要实现 [ICustomPropertyProvider](/uwp/api/windows.ui.xaml.data.icustompropertyprovider) 和 [ICustomProperty](/uwp/api/windows.ui.xaml.data.icustomproperty) 接口。

默认情况下，[{Binding}](https://docs.microsoft.com/windows/uwp/xaml-platform/binding-markup-extension) 假设你要绑定到标记页面的 [**DataContext**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.datacontext)。 因此，我们将页面的 **DataContext** 设置为绑定源类（本例中为 **HostViewModel** 类型）的实例。 下面的示例展示了用于声明绑定对象的标记。 我们使用了与前面“绑定目标”部分中所使用的相同 **Button.Content** 绑定目标，并绑定到 **HostViewModel.NextButtonText** 属性。

```xaml
<Page xmlns:viewmodel="using:DataBindingInDepth" ... >
    <Page.DataContext>
        <viewmodel:HostViewModel x:Name="viewModelInDataContext"/>
    </Page.DataContext>
    ...
    <Button Content="{Binding Path=NextButtonText}" ... />
</Page>
```

```csharp
// MainPage.xaml.cs
private void Button_Click(object sender, RoutedEventArgs e)
{
    this.viewModelInDataContext.NextButtonText = "Updated Next button text";
}
```

```cppwinrt
// MainPage.cpp
void MainPage::ClickHandler(IInspectable const&, RoutedEventArgs const&)
{
    viewModelInDataContext().NextButtonText(L"Updated Next button text");
}
```

注意，该值即是我们为 **Path** 指定的值。 此值在页面 [**DataContext**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.datacontext) 的上下文中进行了解释，本示例已设置为 **HostViewModel** 的实例。 路径引用 **HostViewModel.NextButtonText** 属性。 我们可以省略 **Mode**，因为此处使用的是单向绑定默认值 [{Binding}](https://docs.microsoft.com/windows/uwp/xaml-platform/binding-markup-extension)。

UI 元素的 [**DataContext**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.datacontext) 默认值是已继承的其父元素的值。 显然可通过显式设置 **DataContext** 来替代该默认值，默认情况下，该值又会被子元素继承。 如果你希望多个绑定使用同一个源，则在元素上显式设置 **DataContext** 会很有用。

绑定对象具有 **Source** 属性，其默认为声明绑定所在 UI 元素的 [**DataContext**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.datacontext)。 你可以通过在绑定对象上显式设置 **Source**、**RelativeSource** 或 **ElementName** 来替代此默认值（有关详细信息，请参阅 [{Binding}](https://docs.microsoft.com/windows/uwp/xaml-platform/binding-markup-extension)）。

在 [DataTemplate](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DataTemplate) 内，[DataContext](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.datacontext) 已自动设置为模板化的数据对象   。 下面给出的示例可用作项目控件的 **ItemTemplate**，绑定到具有名为 **Title** 和 **Description** 的字符串属性的任意类型的集合。

```xaml
<DataTemplate x:Key="SimpleItemTemplate">
    <StackPanel Orientation="Vertical" Height="50">
      <TextBlock Text="{Binding Title}"/>
      <TextBlock Text="{Binding Description"/>
    </StackPanel>
  </DataTemplate>
```

> [!NOTE]
> 默认情况下，当 [TextBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) 失去焦点时，[TextBox.Text](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.text) 的更改将发送到双向绑定源   。 若要在每次用户按键之后发送更改，则在标记的绑定对象上将 **UpdateSourceTrigger** 设置为 **PropertyChanged**。 当通过将 **UpdateSourceTrigger** 设置为 **Explicit** 将更改发送到绑定源时，也可完全控制这一情形。 然后，在文本框（通常为 [**TextBox.TextChanged**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox)）上处理事件，调用绑定目标上的 [**GetBindingExpression**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.getbindingexpression) 以获取 [**BindingExpression**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.bindingexpression) 对象，最后调用 [**BindingExpression.UpdateSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.bindingexpression.updatesource) 以编程方式更新数据源。

[  **Path**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.path) 属性支持各种用于绑定到嵌套属性、附加属性以及整数和字符串索引器的语法选项。 有关详细信息，请参阅 [Property-path 语法](https://docs.microsoft.com/windows/uwp/xaml-platform/property-path-syntax)。 绑定到字符串索引器会为你提供绑定到动态属性的效果，而无需实现 [**ICustomPropertyProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.ICustomPropertyProvider)。 [  **ElementName**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.elementname) 属性对于元素间的绑定非常有用。 [  **RelativeSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.relativesource) 属性具有多种用途，其中之一是用作模板化 [**ControlTemplate**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate) 内绑定的更有效的替代方法。 有关其他设置，请参阅 [{Binding} 标记扩展](https://docs.microsoft.com/windows/uwp/xaml-platform/binding-markup-extension)和 [**Binding**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding) 类。

## <a name="what-if-the-source-and-the-target-are-not-the-same-type"></a>如果绑定源和绑定目标不是同一类型，会怎样？

如果你希望基于 Boolean 属性值控制 UI 元素的可见性，或者希望 UI 元素显示为某种颜色（这是数字值的范围或趋势函数），亦或是希望在需要字符串的 UI 元素属性中显示日期/时间值，则需要将值从一种类型转化为另一种类型。 存在这样的情形：其正确的解决方案是从绑定源类公开其他正确类型的属性，并在此处让转换逻辑保持封装和可测试。 但是，当你拥有大量源和目标属性或这两者的大型组合时，上述方案既不灵活也不可扩展。 在这种情况下，你有以下几个选项：

* 如果使用 {x:Bind}，则你可以直接绑定到函数以执行该转换。
* 或者可以指定一个值转换器，该转换器是一个旨在执行转换的对象。 

## <a name="value-converters"></a>值转换器

下面是适用于一次性或单向绑定的值转换器，可将 [**DateTime**](https://docs.microsoft.com/dotnet/api/system.datetime) 值转换为包含月份的字符串值。 该类实现 [**IValueConverter**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.IValueConverter)。

```csharp
public class DateToStringConverter : IValueConverter
{
    // Define the Convert method to convert a DateTime value to 
    // a month string.
    public object Convert(object value, Type targetType, 
        object parameter, string language)
    {
        // value is the data from the source object.
        DateTime thisdate = (DateTime)value;
        int monthnum = thisdate.Month;
        string month;
        switch (monthnum)
        {
            case 1:
                month = "January";
                break;
            case 2:
                month = "February";
                break;
            default:
                month = "Month not found";
                break;
        }
        // Return the value to pass to the target.
        return month;
    }

    // ConvertBack is not implemented for a OneWay binding.
    public object ConvertBack(object value, Type targetType, 
        object parameter, string language)
    {
        throw new NotImplementedException();
    }
}
```

```cppwinrt
// See the "Formatting or converting data values for display" section in the "Data binding overview" topic.
```

下面介绍了如何在绑定对象标记中使用该值转换器。

```xaml
<UserControl.Resources>
  <local:DateToStringConverter x:Key="Converter1"/>
</UserControl.Resources>
...
<TextBlock Grid.Column="0" 
  Text="{x:Bind ViewModel.Month, Converter={StaticResource Converter1}}"/>
<TextBlock Grid.Column="0" 
  Text="{Binding Month, Converter={StaticResource Converter1}}"/>
```

如果 [**Converter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.ivalueconverter.convert) 参数是为绑定定义的，则绑定引擎会调用 [**Convert**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.ivalueconverter.convertback) 和 [**ConvertBack**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converter) 方法。 从绑定源传递数据时，绑定引擎会调用 **Convert** 并将返回的数据传递给绑定目标。 从（用于双向绑定）绑定目标传递数据时，绑定引擎会调用 **ConvertBack** 并将返回的数据传递给绑定源。

转换器还有可选参数：[ConverterLanguage](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterlanguage)（该参数允许指定在转换中使用的语言）和 [ConverterParameter](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterparameter)（该参数允许为转换逻辑传递一个参数）   。 有关使用转换器参数的示例，请参阅 [**IValueConverter**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.IValueConverter)。

> [!NOTE]
> 如果在转换中存在错误，请不要引发异常。 而是返回 [**DependencyProperty.UnsetValue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyproperty.unsetvalue)，它将停止数据传输。

若要显示在无法解析绑定源时所使用的默认值，则在标记的绑定上对象设置 **FallbackValue** 属性。 这对处理转换和格式错误很有用。 这对要绑定到可能未存在于异型绑定集合中所有对象上的源属性也同样有用。

如果你将文本控件绑定到某个值（不是字符串），则数据绑定引擎会将该值转换为字符串。 如果该值是引用类型，数据绑定引擎将通过调用 [**ICustomPropertyProvider.GetStringRepresentation**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.icustompropertyprovider.getstringrepresentation) 或 [**IStringable.ToString**](https://docs.microsoft.com/windows/desktop/api/windows.foundation/nf-windows-foundation-istringable-tostring)（如果提供）来检索字符串值，否则将调用 [**Object.ToString**](https://docs.microsoft.com/dotnet/api/system.object.tostring#System_Object_ToString)。 但是请注意，绑定引擎将忽略隐藏基类实现的任何 **ToString** 实现。 子类实现应替代基类 **ToString** 方法。 同样，在本机语言中，也会显示所有托管对象以实现 [**ICustomPropertyProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.ICustomPropertyProvider) 和 [**IStringable**](https://docs.microsoft.com/windows/desktop/api/windows.foundation/nn-windows-foundation-istringable)。 但是，对 **GetStringRepresentation** 和 **IStringable.ToString** 的所有调用都路由到 **Object.ToString** 或该方法的替代，从不路由到隐藏基类实现的新 **ToString** 实现。

> [!NOTE]
> 从 Windows 10 版本 1607 开始，XAML 框架向 Visibility 转换器提供内置布尔值。 转换器将 **true** 映射到 **Visible** 枚举值并将 **false** 映射到 **Collapsed**，以便你可以将 Visibility 属性绑定到布尔值，而无需创建转换器。 若要使用内置转换器，你的应用的最低目标 SDK 版本必须为 14393 或更高版本。 当你的应用面向较早版本的 Windows 10 时，你无法使用它。 有关目标版本的详细信息，请参阅[版本自适应代码](https://docs.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code)。

## <a name="function-binding-in-xbind"></a>{x:Bind} 中的函数绑定

{x:Bind} 使绑定步骤中的最后一步可以是一个函数。 这可以用于执行转换，以及执行依赖多个属性的绑定。 请参阅 [x:Bind 中的函数](function-bindings.md) 

<span id="resource-dictionaries-with-x-bind"/>

## <a name="element-to-element-binding"></a>元素间的绑定

可以将一个 XAML 元素的属性绑定到另一个 XAML 元素的属性。 下面是在标记中进行的该操作的一个示例。

```xaml
<TextBox x:Name="myTextBox" />
<TextBlock Text="{x:Bind myTextBox.Text, Mode=OneWay}" />
```

> [!IMPORTANT]
> 有关使用 C++/WinRT 的元素间的绑定的必要工作流，请参阅[元素间的绑定](/windows/uwp/cpp-and-winrt-apis/binding-property#element-to-element-binding)。

## <a name="resource-dictionaries-with-xbind"></a>带有 {x:Bind} 的资源字典

[{x:Bind} 标记扩展](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)依赖于代码生成，因此它需要一个包含可调用 **InitializeComponent** 的构造函数的代码隐藏文件（以供初始化生成的代码）。 通过实例化资源字典的类型（以便调用 **InitializeComponent**）而不是引用其文件名，来重用它。 下面是针对你拥有一个现有资源字典并且想要在其中使用 {x:Bind} 这一情形的示例。

TemplatesResourceDictionary.xaml

```xaml
<ResourceDictionary
    x:Class="ExampleNamespace.TemplatesResourceDictionary"
    .....
    xmlns:examplenamespace="using:ExampleNamespace">
    
    <DataTemplate x:Key="EmployeeTemplate" x:DataType="examplenamespace:IEmployee">
        <Grid>
            <TextBlock Text="{x:Bind Name}"/>
        </Grid>
    </DataTemplate>
</ResourceDictionary>
```

TemplatesResourceDictionary.xaml.cs

```csharp
using Windows.UI.Xaml.Data;
 
namespace ExampleNamespace
{
    public partial class TemplatesResourceDictionary
    {
        public TemplatesResourceDictionary()
        {
            InitializeComponent();
        }
    }
}
```

MainPage.xaml

```xaml
<Page x:Class="ExampleNamespace.MainPage"
    ....
    xmlns:examplenamespace="using:ExampleNamespace">

    <Page.Resources>
        <ResourceDictionary>
            .... 
            <ResourceDictionary.MergedDictionaries>
                <examplenamespace:TemplatesResourceDictionary/>
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </Page.Resources>
</Page>
```

## <a name="event-binding-and-icommand"></a>事件绑定和 ICommand

[{x:Bind}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension) 支持一种名为事件绑定的功能。 借助此功能，你可以使用绑定为事件指定处理程序（这是一种附加选项），但使用代码隐藏文件上的某一方法处理事件除外。 假设你的 **MainPage** 类上具有 **RootFrame** 属性。

```csharp
public sealed partial class MainPage : Page
{
    ...
    public Frame RootFrame { get { return Window.Current.Content as Frame; } }
}
```

随即你可以将按钮的 **Click** 事件绑定到由 **RootFrame** 属性返回的 **Frame** 对象上的方法，如下所示。 注意，我们还将按钮的 **IsEnabled** 属性绑定到同一 **Frame** 的另一成员。

```xaml
<AppBarButton Icon="Forward" IsCompact="True"
IsEnabled="{x:Bind RootFrame.CanGoForward, Mode=OneWay}"
Click="{x:Bind RootFrame.GoForward}"/>
```

重载方法不适用于借助此技术来处理事件。 此外，如果用于处理事件的方法包含一些参数，则这些参数均须根据各自对应的事件参数类型进行赋值。 在此情况下，[**Frame.GoForward**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.goforward) 不重载且不含任何参数（但即使其包含两个 **object** 参数，也仍然有效）。 不过，因为 [Frame.GoBack](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.goback) 已重载，所以我们无法通过此技术使用该方法  。

事件绑定技术类似于实现和使用命令（一个命令返回一种属性，该属性将返回实现 [**ICommand**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.icommand) 接口的对象）。 [{x:Bind}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension) 和 [{Binding}](https://docs.microsoft.com/windows/uwp/xaml-platform/binding-markup-extension) 均适用于命令。 可使用 [QuizGame](https://github.com/microsoft/Windows-appsample-networkhelper) 示例（位于“Common”文件夹中）中提供的 **DelegateCommand** 帮助程序类，这样便无需多次实现命令模式。

## <a name="binding-to-a-collection-of-folders-or-files"></a>绑定到文件夹或文件集合

你可以使用 [**Windows.Storage**](https://docs.microsoft.com/uwp/api/Windows.Storage) 命名空间中的 API 来检索文件夹和文件数据。 然而，各种 **GetFilesAsync**、**GetFoldersAsync** 和 **GetItemsAsync** 方法都不会返回适合绑定到列表控件的值。 必须绑定到 [**FileInformationFactory**](https://docs.microsoft.com/uwp/api/windows.storage.bulkaccess.fileinformationfactory.getvirtualizedfilesvector) 类的 [**GetVirtualizedFilesVector**](https://docs.microsoft.com/uwp/api/windows.storage.bulkaccess.fileinformationfactory.getvirtualizedfoldersvector)、[**GetVirtualizedFoldersVector**](https://docs.microsoft.com/uwp/api/windows.storage.bulkaccess.fileinformationfactory.getvirtualizeditemsvector)和 [**GetVirtualizedItemsVector**](https://docs.microsoft.com/uwp/api/Windows.Storage.BulkAccess.FileInformationFactory) 方法的返回值。 下面的代码示例来自 [StorageDataSource 和 GetVirtualizedFilesVector 示例](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/master/Official%20Windows%20Platform%20Sample/Windows%208.1%20Store%20app%20samples/99866-Windows%208.1%20Store%20app%20samples/StorageDataSource%20and%20GetVirtualizedFilesVector%20sample)，它将显示典型的使用模式。 请谨记在应用包清单中声明 **picturesLibrary** 功能，并确认你的 Pictures 库文件夹中有图片。

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    var library = Windows.Storage.KnownFolders.PicturesLibrary;
    var queryOptions = new Windows.Storage.Search.QueryOptions();
    queryOptions.FolderDepth = Windows.Storage.Search.FolderDepth.Deep;
    queryOptions.IndexerOption = Windows.Storage.Search.IndexerOption.UseIndexerWhenAvailable;

    var fileQuery = library.CreateFileQueryWithOptions(queryOptions);

    var fif = new Windows.Storage.BulkAccess.FileInformationFactory(
        fileQuery,
        Windows.Storage.FileProperties.ThumbnailMode.PicturesView,
        190,
        Windows.Storage.FileProperties.ThumbnailOptions.UseCurrentScale,
        false
        );

    var dataSource = fif.GetVirtualizedFilesVector();
    this.PicturesListView.ItemsSource = dataSource;
}
```

通常，你需要使用此方法来创建文件和文件夹信息的只读视图。 可以创建到文件和文件夹属性的双向绑定，例如，让用户在音乐视图中对歌曲进行评级。 然而，任何更改都不会永久保留，除非你调用相应的 **SavePropertiesAsync** 方法（例如，[**MusicProperties.SavePropertiesAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.musicproperties.savepropertiesasync)）。 当项目失去焦点时，你应该提交更改，因为这样可以触发选择重置。

注意，使用此技术的双向绑定仅适用于索引位置（例如“音乐”）。 你可以通过调用 [**FolderInformation.GetIndexedStateAsync**](https://docs.microsoft.com/uwp/api/windows.storage.bulkaccess.folderinformation.getindexedstateasync) 方法来确定是否索引某个位置。

还请注意，某个虚拟化矢量可能会在填充其值之前为某些项目返回 **null**。 例如，应该首先检查 **null**，然后才能使用绑定到虚拟化矢量的列表控件的 [**SelectedItem**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selecteditem) 值，或者改为使用 [**SelectedIndex**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedindex)。

## <a name="binding-to-data-grouped-by-a-key"></a>绑定到按键分组的数据

若要获取项的扁平集合（例如，由 BookSku 类表示的书籍）并通过将某一常见属性（例如，BookSku.AuthorName 属性）用作某种键来对项进行分组，则将该结果称之为分组数据   。 当对数据进行分组时，它将不再是简单集合。 分组数据是组对象的集合，其中每个组对象都具有

- 一个键和
- 一个其属性与该键匹配的项集合。

再以书籍为例，按作者名称对书籍进行分组的结果即为作者名称组的集合，其中每组都具有

- 一个键（这是作者名称）和
- 一个其 AuthorName 属性与组键匹配的 BookSku 集合   。

通常情况下，若要显示集合，可将项目控件（例如 [**ListView**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) 或 [**GridView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView)）的 [**ItemsSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView) 直接绑定到返回集合的属性。 如果这是项的简单集合，则无需执行任何特殊操作。 但是，如果它是组对象的集合（像在绑定到分组数据时一样），则需要使用一个名为 [**CollectionViewSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource) 的中间对象，该对象位于项目控件和绑定源之间。 先将 **CollectionViewSource** 绑定到返回分组数据的属性，然后将项目控件绑定到 **CollectionViewSource**。 **CollectionViewSource** 的额外增值功能可用于跟踪当前项，以便你可以通过将多个项目控件全都绑定到同一 **CollectionViewSource** 来使其保持同步。 也可以通过 [**CollectionViewSource.View**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.icollectionview.currentitem) 属性返回的对象的 [**ICollectionView.CurrentItem**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.collectionviewsource.view) 属性，以编程方式访问当前项。

若要激活 [**CollectionViewSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource) 的分组功能，请将 [**IsSourceGrouped**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.collectionviewsource.issourcegrouped) 设置为 **true**。 是否还需要设置 [**ItemsPath**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.collectionviewsource.itemspath) 属性完全取决于你的组对象的创作方式。 组对象的创作方式有以下两种：“属于组”模式和“包含组”模式。 在“属于组”模式中，组对象派生自集合类型（例如，**List&lt;T&gt;** ），因此事实上组对象本身就是项目组。 使用此模式，无需设置 **ItemsPath**。 在“包含组”模式中，组对象具有一个或多个集合类型属性（例如 **List&lt;T&gt;** ），因此“包含组”的单个项目组采用单个属性的形式（而多个项目组则采用多个属性的形式）。 使用此模式，需要将 **ItemsPath** 设置为包含项目组的属性名。

下面的示例阐述了“包含组”模式。 页面类有一个名为 [**ViewModel**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.datacontext) 的属性，该属性将返回我们的视图模型的实例。 [  **CollectionViewSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource) 绑定到视图模型的 **Authors** 属性（**Authors** 是组对象的集合），还指定了它是包含分组项目的 **Author.BookSkus** 属性。 最后，[**GridView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView) 绑定到 **CollectionViewSource** 且具有其已定义的组样式，这样它便可以采用分组形式呈现项目。

```xaml
<Page.Resources>
    <CollectionViewSource
    x:Name="AuthorHasACollectionOfBookSku"
    Source="{x:Bind ViewModel.Authors}"
    IsSourceGrouped="true"
    ItemsPath="BookSkus"/>
</Page.Resources>
...
<GridView
ItemsSource="{x:Bind AuthorHasACollectionOfBookSku}" ...>
    <GridView.GroupStyle>
        <GroupStyle
            HeaderTemplate="{StaticResource AuthorGroupHeaderTemplateWide}" ... />
    </GridView.GroupStyle>
</GridView>
```

你可以采用以下两种方式之一实现“属于组”模式。 一种方法是创作你自己的组类。 从 **List&lt;T&gt;** 派生类（其中 *T* 是项目类型）。 例如，`public class Author : List<BookSku>`。 另一种方式是，使用 [LINQ](https://docs.microsoft.com/previous-versions/bb397926(v=vs.140)) 表达式，从诸如 **BookSku** 项目的属性值等动态创建组对象（以及组类）。 此方法（仅维护项目的简单列表并将其动态分组在一起）是从云服务访问数据的应用的典型用法。 可以灵活地按作者或流派对书籍进行分组，而无需特殊组类，如 **Author** 和 **Genre**。

下面的示例使用 [LINQ](https://docs.microsoft.com/previous-versions/bb397926(v=vs.140)) 阐述了“属于组”模式。 在本示例中我们按流派对书籍进行分组，并在组标题中显示流派名。 这由引用组 [**Key**](https://docs.microsoft.com/dotnet/api/system.linq.igrouping-2.key#System_Linq_IGrouping_2_Key) 的值的“Key”属性路径进行指示。

```csharp
using System.Linq;
...
private IOrderedEnumerable<IGrouping<string, BookSku>> genres;

public IOrderedEnumerable<IGrouping<string, BookSku>> Genres
{
    get
    {
        if (this.genres == null)
        {
            this.genres = from book in this.bookSkus
                          group book by book.genre into grp
                          orderby grp.Key
                          select grp;
        }
        return this.genres;
    }
}
```

请记住，在将 [{x:Bind}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension) 用于数据模板时，需通过设置 **x:DataType** 值来指示要绑定到的类型。 如果类型是我们无法在标记中解释的泛型，则需要在组样式标头模板中改用 [{Binding}](https://docs.microsoft.com/windows/uwp/xaml-platform/binding-markup-extension)。

```xaml
    <Grid.Resources>
        <CollectionViewSource x:Name="GenreIsACollectionOfBookSku"
        Source="{x:Bind Genres}"
        IsSourceGrouped="true"/>
    </Grid.Resources>
    <GridView ItemsSource="{x:Bind GenreIsACollectionOfBookSku}">
        <GridView.ItemTemplate x:DataType="local:BookTemplate">
            <DataTemplate>
                <TextBlock Text="{x:Bind Title}"/>
            </DataTemplate>
        </GridView.ItemTemplate>
        <GridView.GroupStyle>
            <GroupStyle>
                <GroupStyle.HeaderTemplate>
                    <DataTemplate>
                        <TextBlock Text="{Binding Key}"/>
                    </DataTemplate>
                </GroupStyle.HeaderTemplate>
            </GroupStyle>
        </GridView.GroupStyle>
    </GridView>
```

[  **SemanticZoom**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) 控件是使用户查看和导航分组数据的绝佳方法。 [Bookstore2](https://codeload.github.com/MicrosoftDocs/windows-topic-specific-samples/zip/Bookstore2Universal_10) 示例应用演示了如何使用 **SemanticZoom**。 在该应用中，你可以查看按作者分组的书籍列表（放大视图），也可以缩小以查看作者的跳转列表（缩小视图）。 与在书籍列表中上下滚动相比，跳转列表提供了更快速的浏览方式。 实际上，放大视图和缩小视图是绑定到同一 **CollectionViewSource** 的 **ListView** 或 **GridView** 控件。

![SemanticZoom 图示](images/sezo.png)

当绑定到分层数据（如类别中的子类别）时，你可以选择使用一系列的项目控件，在 UI 中显示分层级别。 所选项目控件决定后续项目控件的内容。 你可以使列表保持同步，方法是将各列表绑定到其各自的 [**CollectionViewSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource) 并将 **CollectionViewSource** 实例一同绑定到一个链中。 这称为主视图/详细信息（或列表/详细信息）视图。 有关详细信息，请参阅[如何绑定到分层数据并创建主视图/详细信息视图](how-to-bind-to-hierarchical-data-and-create-a-master-details-view.md)。

<span id="debugging"/>

## <a name="diagnosing-and-debugging-data-binding-problems"></a>诊断并调试数据绑定问题

绑定标记包含属性名称（对于 C#，有时是字段和方法）。 因此，在重命名属性时，你还需要更改对其进行引用的任何绑定。 若忘记执行此操作，会导致出现一个典型的数据绑定 Bug，并且你的应用将不能编译或不能正常运行。

由 [{x:Bind}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension) 和 [{Binding}](https://docs.microsoft.com/windows/uwp/xaml-platform/binding-markup-extension) 创建的绑定对象在功能上大致等同。 但是，{x:Bind} 具有绑定源的类型信息，并且会在编译时生成源代码。 借助 {x:Bind}，可使用剩余代码检测同一类型的问题。 这包括编译时对绑定表达式的验证，以及通过在作为页面的部分类生成的源代码中设置断点来调试。 可以在 `obj` 文件夹的文件中找到这些类，如文件夹名为（适用于 C#）`<view name>.g.cs`。 如果你有绑定方面的问题，请打开 Microsoft Visual Studio 调试器中的**出现未处理的异常时中断**。 该调试器将在该点处中断执行，以便你可以调试哪里出现了问题。 对于绑定源节点图形的每个部分，{x:Bind} 生成的代码均遵循相同的模式，你可以使用  “调用堆栈”窗口中的信息来帮助确定导致该问题出现的调用顺序。

[{Binding}](https://docs.microsoft.com/windows/uwp/xaml-platform/binding-markup-extension) 不具有绑定源的类型信息。 但在通过附加的调试器运行你的应用时，Visual Studio 中的  “输出”窗口中将显示所有绑定错误。

## <a name="creating-bindings-in-code"></a>使用代码创建绑定

**注意**  由于不能使用代码创建 [{x:Bind}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension) 绑定，因此此部分仅适用于 [{Binding}](https://docs.microsoft.com/windows/uwp/xaml-platform/binding-markup-extension)。 不过，使用 [**DependencyObject.RegisterPropertyChangedCallback**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject.registerpropertychangedcallback) 同样可以获得 {x:Bind} 的某些相同优势，这使你可以在任何依赖属性上注册更改通知。

还可以使用规程代码而不是 XAML 来将 UI 元素连接到数据。 为此，请创建一个新 [**Binding**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding) 对象，设置相应的属性，然后调用 [**FrameworkElement.SetBinding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.setbinding) 或 [**BindingOperations.SetBinding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.bindingoperations.setbinding)。 如果你希望在运行时选择绑定属性值或在多个控件中共享单个绑定，则以编程方式创建绑定将十分有用。 但是请注意，调用 **SetBinding** 后，无法更改绑定属性值。

以下示例演示了如何使用代码实现绑定。

```xaml
<TextBox x:Name="MyTextBox" Text="Text"/>
```

```csharp
// Create an instance of the MyColors class 
// that implements INotifyPropertyChanged.
MyColors textcolor = new MyColors();

// Brush1 is set to be a SolidColorBrush with the value Red.
textcolor.Brush1 = new SolidColorBrush(Colors.Red);

// Set the DataContext of the TextBox MyTextBox.
MyTextBox.DataContext = textcolor;

// Create the binding and associate it with the text box.
Binding binding = new Binding() { Path = new PropertyPath("Brush1") };
MyTextBox.SetBinding(TextBox.ForegroundProperty, binding);
```

```vbnet
' Create an instance of the MyColors class 
' that implements INotifyPropertyChanged. 
Dim textcolor As New MyColors()

' Brush1 is set to be a SolidColorBrush with the value Red. 
textcolor.Brush1 = New SolidColorBrush(Colors.Red)

' Set the DataContext of the TextBox MyTextBox. 
MyTextBox.DataContext = textcolor

' Create the binding and associate it with the text box.
Dim binding As New Binding() With {.Path = New PropertyPath("Brush1")}
MyTextBox.SetBinding(TextBox.ForegroundProperty, binding)
```

## <a name="xbind-and-binding-feature-comparison"></a>{x:Bind} 和 {Binding} 功能比较

| 功能 | {x:Bind} | {Binding} | 注释 |
|---------|----------|-----------|-------|
| Path 为默认属性 | `{x:Bind a.b.c}` | `{Binding a.b.c}` | | 
| Path 属性 | `{x:Bind Path=a.b.c}` | `{Binding Path=a.b.c}` | 在 x:Bind 中，Path 默认为 Page 的根，而非 DataContext 的根。 | 
| 索引 | `{x:Bind Groups[2].Title}` | `{Binding Groups[2].Title}` | 绑定到集合中的指定项。 仅支持基于整数的索引。 | 
| 附加属性 | `{x:Bind Button22.(Grid.Row)}` | `{Binding Button22.(Grid.Row)}` | 附加属性用括号进行指定。 如果未在 XAML 命名空间中声明该属性，则在其前面加上 xml 命名空间，这应该映射到文档的标头处的代码命名空间中。 | 
| 强制转换 | `{x:Bind groups[0].(data:SampleDataGroup.Title)}` | 不需要。 | 强制转换用括号进行指定。 如果未在 XAML 命名空间中声明该属性，则在其前面加上 xml 命名空间，这应该映射到文档的标头处的代码命名空间中。 | 
| Converter | `{x:Bind IsShown, Converter={StaticResource BoolToVisibility}}` | `{Binding IsShown, Converter={StaticResource BoolToVisibility}}` | 转换器必须在 Page/ResourceDictionary 的根目录处或在 App.xaml 中进行声明。 | 
| ConverterParameter, ConverterLanguage | `{x:Bind IsShown, Converter={StaticResource BoolToVisibility}, ConverterParameter=One, ConverterLanguage=fr-fr}` | `{Binding IsShown, Converter={StaticResource BoolToVisibility}, ConverterParameter=One, ConverterLanguage=fr-fr}` | 转换器必须在 Page/ResourceDictionary 的根目录处或在 App.xaml 中进行声明。 | 
| TargetNullValue | `{x:Bind Name, TargetNullValue=0}` | `{Binding Name, TargetNullValue=0}` | 在绑定表达式的叶为 null 时使用。 对于字符串值，应使用单引号。 | 
| FallbackValue | `{x:Bind Name, FallbackValue='empty'}` | `{Binding Name, FallbackValue='empty'}` | 绑定的路径的任何部分（叶除外）为 null 时使用。 | 
| ElementName | `{x:Bind slider1.Value}` | `{Binding Value, ElementName=slider1}` | 使用 {x:Bind}，绑定到某一字段；Path 默认位于 Page 的根处，以便任意命名的元素均可通过其字段进行访问。 | 
| RelativeSource:Self | `<Rectangle x:Name="rect1" Width="200" Height="{x:Bind rect1.Width}" ... />` | `<Rectangle Width="200" Height="{Binding Width, RelativeSource={RelativeSource Self}}" ... />` | 借助 {x:Bind}，对元素进行命名并在 Path 中使用其名称。 | 
| RelativeSource:TemplatedParent | 不需要 | `{Binding <path>, RelativeSource={RelativeSource TemplatedParent}}` | 借助 ControlTemplate 上的{x:Bind} TargetType，指示绑定到模板父级。 对于 {Binding}，在大多数情况下，常规模板绑定可在控件模板中使用。 但在使用 TemplatedParent 时，需要使用转换器或双向绑定。&lt; | 
| 源 | 不需要 | `<ListView ItemsSource="{Binding Orders, Source={StaticResource MyData}}"/>` | 对于 {x:Bind}，可以直接使用命名元素，使用属性或静态路径。 | 
| 模式 | `{x:Bind Name, Mode=OneWay}` | `{Binding Name, Mode=TwoWay}` | 模式可以是一次性、单向或双向。 {x:Bind} defaults to OneTime; {Binding} defaults to OneWay. | 
| UpdateSourceTrigger | `{x:Bind Name, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}` | `{Binding UpdateSourceTrigger=PropertyChanged}` | UpdateSourceTrigger 可以是 Default、LostFocus 或 PropertyChanged。 {x:Bind} 不支持 UpdateSourceTrigger=Explicit。 {x:Bind} 可在所有情况下（TextBox.Text 除外，它使用 LostFocus 行为）使用 PropertyChanged 行为。 | 


