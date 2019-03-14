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
ms.openlocfilehash: fe86656756eab9d9286d68c2a37357a9b824561e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57636062"
---
# <a name="data-binding-in-depth"></a>深入了解数据绑定

**重要的 Api**

-   [**{x： 绑定} 标记扩展**](../xaml-platform/x-bind-markup-extension.md)
-   [**绑定类**](https://msdn.microsoft.com/library/windows/apps/BR209820)
-   [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713)
-   [**INotifyPropertyChanged**](https://msdn.microsoft.com/library/windows/apps/BR209899)

> [!NOTE]
> 本主题详细介绍数据绑定功能。 有关简短、实用的简介，请参阅[数据绑定概述](data-binding-quickstart.md)。

数据绑定是你的应用 UI 用来显示数据的一种方法，可以选择与该数据保持同步。 借助数据绑定，你可以将关注的数据从关注的 UI 中分离开来，从而可形成一个更简易的概念模型，并且使你的应用拥有更好的可读性、可测试性和可维护性。

当 UI首次显示时，你可以使用数据绑定以仅显示来自数据源的值，但不会对这些值中的更改做出响应。 这是一种模式的绑定称为*一次性*，和它适用于运行时期间不会更改值。 或者，可以选择"观察"的值，并在更改时，更新 UI。 此模式称为*单向*，和它适用于只读数据。 最后，你可以选择观察并更新，以便用户在 UI 中对值所做的更改能自动传回数据源。 此模式称为*双向*，和它非常适合读写数据。 下面是一些示例。

-   您可以使用一次性模式绑定[**映像**](https://msdn.microsoft.com/library/windows/apps/BR242752)到当前用户的照片。
-   您可以使用单向模式绑定[ **ListView** ](https://msdn.microsoft.com/library/windows/apps/BR242878)到实时新闻文章报纸部分按分组的集合。
-   您可以使用双向模式绑定[**文本框中**](https://msdn.microsoft.com/library/windows/apps/BR209683)窗体中的客户的名称。

独立的模式有两种类型的绑定，并且它们两者通常声明 UI 标记中。 你既可以选用 [{x:Bind} 标记扩展](https://msdn.microsoft.com/library/windows/apps/Mt204783)，也可以选用 [{Binding} 标记扩展](https://msdn.microsoft.com/library/windows/apps/Mt204782)。 还可以在同一应用中（甚至是同一 UI 元素上）混合使用这两个标记扩展。 {x： 绑定} 是用于 Windows 10 的新功能，它具有更好的性能。 除非另有明确说明，否则本主题中所述的所有详细信息均适用于这两种绑定。

**演示 {x： 绑定} 应用程序示例**

-   [{x:Bind} 示例](https://go.microsoft.com/fwlink/p/?linkid=619989)。
-   [QuizGame](https://github.com/Microsoft/Windows-appsample-quizgame)。
-   [XAML UI 基本示例](https://go.microsoft.com/fwlink/p/?linkid=619992)。

**演示 {Binding} 应用程序示例**

-   下载 [Bookstore1](https://go.microsoft.com/fwlink/?linkid=532950) 应用。
-   下载 [Bookstore2](https://go.microsoft.com/fwlink/?linkid=532952) 应用。

## <a name="every-binding-involves-these-pieces"></a>每个绑定均由以下部分构成

-   *绑定源*。 这是用于绑定的数据源，它可以是任意类的实例，其中该类具有其值要显示在 UI 中的成员。
-   *绑定目标*。 这是显示数据的 UI 中 [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/BR242362) 的 [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/BR208706)。
-   *绑定对象*。 这是用于将数据值从绑定源传递给绑定目标（或从绑定目标传递回绑定源）的组件。 绑定对象通过 [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) 或 [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) 标记扩展在 XAML 加载时进行创建。

在以下部分中，我们将进一步探讨绑定源、绑定目标和绑定对象。 我们将这些部分与关于将按钮内容绑定到名为 **NextButtonText** 的字符串属性（这属于名为 **HostViewModel** 的类）的示例链接在一起。

### <a name="binding-source"></a>绑定源

下面是一个非常基础的可用作绑定源的类实现。

如果您使用的[C + + WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)，然后添加新**Midl 文件 (.idl)** 项目到项目中，名为如下所示的 C + + WinRT 代码示例下面的列表。 使用这些新文件的内容替换为[MIDL 3.0](/uwp/midl-3/intro)显示在列表中，代码生成项目以生成`HostViewModel.h`和`.cpp`，然后将代码添加到生成的文件以匹配列表。 有关这些生成的文件的详细信息以及如何将它们复制到你的项目，请参阅[XAML 控制; 绑定到 C + + WinRT 属性](/windows/uwp/cpp-and-winrt-apis/binding-property)。

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

执行此操作的方法之一是，从 [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/BR242356) 派生代表绑定源的类，并通过 [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/BR242362) 公开数据值。 以上就是 [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/BR208706) 成为可观察绑定源的方式。 **FrameworkElements** 即时可用，是很好的绑定源。

让类成为可观察绑定源，以及成为类（已拥有基类）的必需项的更便捷方法是实现 [**System.ComponentModel.INotifyPropertyChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/system.componentmodel.inotifypropertychanged.aspx)。 实际上，这仅涉及到实现一个名为 **PropertyChanged** 的事件。 下面展示了使用 **HostViewModel** 的示例。

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
        this.PropertyChanged(this, new PropertyChangedEventArgs(propertyName));
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

因此，无需实现模式显示多次，上方，如果您使用的C#然后就可以从派生**BindableBase**您会发现中的低音类[QuizGame](https://github.com/Microsoft/Windows-appsample-quizgame)示例 （在"Common"的文件夹）。 下面是具体操作的一个示例。

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
> 对于 C + + / WinRT，任何运行时类的声明派生自的基类在应用程序中被称为*可组合*类。 并且，围绕可组合类的约束。 应用程序传递[Windows 应用认证工具包](../debug-test-perf/windows-app-certification-kit.md)测试用于通过 Visual Studio 和 Microsoft Store 验证提交 (并因此成功引入到 Microsoft Store 应用程序)、可组合类最终必须从 Windows 的基类派生。 这意味着非常根处的继承层次结构的类必须源自 windows.* 命名空间的类型。 如果需要运行时类派生自的基类&mdash;例如，若要实现**BindableBase**的所有视图模型，用于从派生类&mdash;则可以从派生[ **Windows.UI.Xaml.DependencyObject**](/uwp/api/windows.ui.xaml.dependencyobject)。

通过使用 [**String.Empty**](https://msdn.microsoft.com/library/windows/apps/xaml/system.string.empty.aspx) 或 **null** 参数引发 **PropertyChanged** 事件，以指示对象上的所有非索引器属性应重新读取。 可以引发该事件以指示对象的索引器属性已更改使用的自变量"项\[*索引器*\]"对于特定的索引器 (其中*索引器*是索引值），则值为"项\[\]"的所有索引器。

可以将绑定源视为其属性中包含数据的单个对象或一些对象的集合。 在C#和 Visual Basic 代码，您可以一次性绑定到实现的对象[ **List (Of T)** ](https://msdn.microsoft.com/library/windows/apps/xaml/6sh2ey19.aspx)以显示在运行时不会更改的集合。 对于可观察集合（在将项添加到集合或从集合中删除时进行观察），应改为单向绑定到 [**ObservableCollection(Of T)**](https://msdn.microsoft.com/library/windows/apps/xaml/ms668604.aspx)。 在 C++ 代码中，对于可观察集合和不可观察集合，均可绑定到 [**Vector&lt;T&gt;**](https://msdn.microsoft.com/library/dn858385.aspx)。 若要绑定到你自己的集合类，请按照下表中的指南进行操作。

|方案|C# 和 VB (CLR)|C++/WinRT|C++/CX|
|-|-|-|-|
|绑定到对象。|可以为任何对象。|可以为任何对象。|对象必须具有 [**BindableAttribute**](https://msdn.microsoft.com/library/windows/apps/Hh701872) 或实现 [**ICustomPropertyProvider**](https://msdn.microsoft.com/library/windows/apps/BR209878)。|
|从绑定的对象获取属性更改通知。|对象必须实现[ **INotifyPropertyChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/system.componentmodel.inotifypropertychanged.aspx)。| 对象必须实现[ **INotifyPropertyChanged**](https://msdn.microsoft.com/library/windows/apps/BR209899)。|对象必须实现[ **INotifyPropertyChanged**](https://msdn.microsoft.com/library/windows/apps/BR209899)。|
|绑定到集合。| [**List(Of T)**](https://msdn.microsoft.com/library/windows/apps/xaml/6sh2ey19.aspx)|[**IVector** ](/uwp/api/windows.foundation.collections.ivector_t_)的[ **IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)，或者[ **IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector)。 请参阅[XAML 控件的项; 绑定到 C + + WinRT 集合](../cpp-and-winrt-apis/binding-collection.md)并[集合使用 C + + WinRT](../cpp-and-winrt-apis/collections.md)。| [**Vector&lt;T&gt;**](https://msdn.microsoft.com/library/windows/apps/xaml/hh441570.aspx)|
|获取从绑定的集合的集合更改通知。|[**ObservableCollection(Of T)**](https://msdn.microsoft.com/library/windows/apps/xaml/ms668604.aspx)|[**IObservableVector** ](/uwp/api/windows.foundation.collections.iobservablevector_t_)的[ **IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)。|[**IObservableVector&lt;T&gt;**](https://msdn.microsoft.com/library/windows/apps/br226052.aspx)|
|实现支持绑定的集合。|扩展 [**List(Of T)**](https://msdn.microsoft.com/library/windows/apps/xaml/6sh2ey19.aspx) 或者实现 [**IList**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.ilist.aspx)、[**IList**](https://msdn.microsoft.com/library/windows/apps/xaml/5y536ey6.aspx)(Of [**Object**](https://msdn.microsoft.com/library/windows/apps/xaml/system.object.aspx))、[**IEnumerable**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.ienumerable.aspx) 或 [**IEnumerable**](https://msdn.microsoft.com/library/windows/apps/xaml/9eekhta0.aspx)(Of **Object**)。 绑定到通用 **IList(Of T)** 和 **IEnumerable(Of T)** 的操作不受支持。|实现[ **IVector** ](/uwp/api/windows.foundation.collections.ivector_t_)的[ **IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)。 请参阅[XAML 控件的项; 绑定到 C + + WinRT 集合](../cpp-and-winrt-apis/binding-collection.md)并[集合使用 C + + WinRT](../cpp-and-winrt-apis/collections.md)。|实现 [**IBindableVector**](https://msdn.microsoft.com/library/windows/apps/Hh701979)、[**IBindableIterable**](https://msdn.microsoft.com/library/windows/apps/Hh701957)、[**IVector**](https://msdn.microsoft.com/library/windows/apps/BR206631)&lt;[**Object**](https://msdn.microsoft.com/library/windows/apps/xaml/system.object.aspx)^&gt;、[**IIterable**](https://msdn.microsoft.com/library/windows/apps/BR226024)&lt;**Object**^&gt;、**IVector**&lt;[**IInspectable**](https://msdn.microsoft.com/library/BR205821)\*&gt; 或 **IIterable**&lt;**IInspectable**\*&gt;。 绑定到通用 **IVector&lt;T&gt;** 和 **IIterable&lt;T&gt;** 的操作不受支持。|
| 实现支持集合更改通知的集合。 | 扩展 [**ObservableCollection(Of T)**](https://msdn.microsoft.com/library/windows/apps/xaml/ms668604.aspx) 或实现（非通用）[**IList**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.ilist.aspx) 和 [**INotifyCollectionChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.specialized.inotifycollectionchanged.aspx)。|实现[ **IObservableVector** ](/uwp/api/windows.foundation.collections.iobservablevector_t_)的[ **IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)，或[ **IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector).|实现 [**IBindableVector**](https://msdn.microsoft.com/library/windows/apps/Hh701979) 和 [**IBindableObservableVector**](https://msdn.microsoft.com/library/windows/apps/Hh701974)。|
|实现支持增量加载的集合。|扩展 [**ObservableCollection(Of T)**](https://msdn.microsoft.com/library/windows/apps/xaml/ms668604.aspx) 或实现（非通用）[**IList**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.ilist.aspx) 和 [**INotifyCollectionChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.specialized.inotifycollectionchanged.aspx)。 此外，还实现 [**ISupportIncrementalLoading**](https://msdn.microsoft.com/library/windows/apps/Hh701916)。|实现[ **IObservableVector** ](/uwp/api/windows.foundation.collections.iobservablevector_t_)的[ **IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)，或[ **IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector). 此外，实现[ **ISupportIncrementalLoading**](https://msdn.microsoft.com/library/windows/apps/Hh701916)|实现 [**IBindableVector**](https://msdn.microsoft.com/library/windows/apps/Hh701979)、[**IBindableObservableVector**](https://msdn.microsoft.com/library/windows/apps/Hh701974) 和 [**ISupportIncrementalLoading**](https://msdn.microsoft.com/library/windows/apps/Hh701916)。|

你可以使用增量加载将列表控件绑定到任意的大型数据源，且仍获得高性能。 例如，你可以将列表控件绑定到必应图像查询结果，而无需一次性加载所有结果。 你只需立即加载部分结果，再根据需要加载其他结果。 若要支持增量加载，则必须实现[ **ISupportIncrementalLoading** ](https://msdn.microsoft.com/library/windows/apps/Hh701916)支持集合对数据源更改通知。 当数据绑定引擎请求更多数据时，你的数据源必须发出相应的请求、集成结果，然后发送相应的通知以更新 UI。

### <a name="binding-target"></a>绑定目标

在下面的两个示例**Button.Content**属性为绑定目标，并且其值设置为标记扩展声明绑定对象。 首先显示的是 [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783)，然后是 [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782)。 在标记中声明绑定这一做法很常见（其既便捷、易读又很实用）。 不过，应避免标记和以命令性方式（编程方式）创建 [**Binding**](https://msdn.microsoft.com/library/windows/apps/BR209820) 类的实例（除非需要）。

```xaml
<Button Content="{x:Bind ...}" ... />
```

```xaml
<Button Content="{Binding ...}" ... />
```

如果您使用的 C + + WinRT 或 Visual c + + 组件扩展 (C + + /cli CX)，则将需要添加[ **BindableAttribute** ](https://msdn.microsoft.com/library/windows/apps/Hh701872)属性为你想要使用任何运行时类[{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782)与标记扩展。

> [!IMPORTANT]
> 如果您使用的[C + + WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)，然后[ **BindableAttribute** ](https://msdn.microsoft.com/library/windows/apps/Hh701872)属性是如果你已安装 Windows SDK 版本 (Windows 10，版本 1809年 10.0.17763.0 可用)，或更高版本。 如果没有该属性，你将需要实现[ICustomPropertyProvider](/uwp/api/windows.ui.xaml.data.icustompropertyprovider)并[ICustomProperty](/uwp/api/windows.ui.xaml.data.icustomproperty)接口，以便能够使用[{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782)标记扩展插件。

### <a name="binding-object-declared-using-xbind"></a>使用 {x:Bind} 声明的绑定对象

在创作 [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) 标记之前，需先执行一个步骤。 我们需要从表示标记页面的类公开绑定源类。 我们会通过添加属性 (类型的**HostViewModel**在这种情况下) 到我们**MainPage**页类。

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

注意，该值即是我们为 **Path** 指定的值。 页面本身的上下文中解释此值，并在这种情况下路径通过引用**ViewModel**我们刚添加到的属性**MainPage**页。 该属性将返回 **HostViewModel** 实例，这样我们便可以点入该对象以访问 **HostViewModel.NextButtonText** 属性。 并且，我们将指定 **Mode** 以替代一次性绑定默认值 [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783)。

[  **Path**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.binding.path) 属性支持各种用于绑定到嵌套属性、附加属性以及整数和字符串索引器的语法选项。 有关详细信息，请参阅 [Property-path 语法](https://msdn.microsoft.com/library/windows/apps/Mt185586)。 绑定到字符串索引器会为你提供绑定到动态属性的效果，而无需实现 [**ICustomPropertyProvider**](https://msdn.microsoft.com/library/windows/apps/BR209878)。 有关其他设置，请参阅 [{x:Bind} 标记扩展](https://msdn.microsoft.com/library/windows/apps/Mt204783)。

为了说明这一点**HostViewModel.NextButtonText**确实可观察属性中，添加**单击**按钮，事件处理程序和更新的值**HostViewModel.NextButtonText**. 生成、 运行和单击按钮以查看该按钮的值**内容**更新。

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
> 将更改为[ **TextBox.Text** ](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textbox.text)发送到双向绑定源[**文本框**](https://msdn.microsoft.com/library/windows/apps/BR209683)失去焦点时，和不之后每次用户按键。

**DataTemplate 和 x： 数据类型**

在 [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/BR242348) 内（不论用作项模板、内容模板还是标头模板），**Path** 的值不在页面的上下文中进行解释，而在模板化的数据对象的上下文中进行解释。 在数据模板中使用 {x:Bind} ，以便在编译时可以对它的绑定进行验证（同时为它们生成有效代码）时，**DataTemplate** 需要使用 **x:DataType** 声明其数据对象的类型。 下面给出的示例可用作绑定到 **SampleDataGroup** 对象集合的项目控件的 **ItemTemplate**。

```xaml
<DataTemplate x:Key="SimpleItemTemplate" x:DataType="data:SampleDataGroup">
    <StackPanel Orientation="Vertical" Height="50">
      <TextBlock Text="{x:Bind Title}"/>
      <TextBlock Text="{x:Bind Description}"/>
    </StackPanel>
  </DataTemplate>
```

**在路径中的弱类型化对象**

例如，假设你有一个名为 SampleDataGroup 的类型，该类型实现名为 Title 的字符串属性。 并且你有一个属性 MainPage.SampleDataGroupAsObject，这是类型对象，但它实际上返回 SampleDataGroup 的实例。 由于在该类型对象上找不到 Title 属性，因此绑定 `<TextBlock Text="{x:Bind SampleDataGroupAsObject.Title}"/>` 将导致编译错误。 该错误的解决方法是向 Path 语法添加强制转换，如下所示：`<TextBlock Text="{x:Bind ((data:SampleDataGroup)SampleDataGroupAsObject).Title}"/>`。 下面是另一个示例，其中元素声明为对象但实际上是 TextBlock：`<TextBlock Text="{x:Bind Element.Text}"/>`。 而且强制转换可以解决该问题：`<TextBlock Text="{x:Bind ((TextBlock)Element).Text}"/>`。

**如果你的数据将以异步方式加载**

在编译时为你的页面在分部类中生成支持 **{x:Bind}** 的代码。 可以在 `obj` 文件夹中找到这些文件，其名称类似于（适用于 C#）`<view name>.g.cs`。 生成的代码包含可用于你的页面的 [**Loading**](https://msdn.microsoft.com/library/windows/apps/BR208706) 事件的处理程序，并且该处理程序在表示你的页面绑定的已生成类上调用 **Initialize** 方法。 **Initialize** 依次调用 **Update**，以开始在绑定源和目标之间移动数据。 只在页面或用户控件的第一个测量阶段之前引发 **Loading**。 因此异步加载你的数据时，可能未在调用 **Initialize** 时做好准备。 因此，加载数据之后，可以通过调用 `this.Bindings.Update();` 强制初始化一次性绑定。 如果您只需要以异步方式加载数据的一次性绑定它将不是它是具有单向绑定并侦听更改初始化这种方式更加经济。 如果你的数据未经过细微的更改并且它可能作为某个特定操作的一部分进行更新，则你可以执行一次性绑定，并且通过对 **Update** 的调用随时强制执行手动更新。

> [!NOTE]
> **{x： 绑定}** 不适合于后期绑定方案，如导航字典结构的 JSON 对象，也鸭子类型化。 "鸭子类型化"是一种弱形式键入基于词法属性名称匹配的 (在中，"如果它将指导、 通常，和类型的叫声像鸭子，然后它就是鸭子")。 鸭子类型化，绑定到与**年龄**属性将与同样满足**人员**或**葡萄酒**对象 (假设每个这些类型必须**年龄**属性)。 对于这些情况下，使用 **{Binding}** 标记扩展。

### <a name="binding-object-declared-using-binding"></a>使用 {Binding} 声明的绑定对象

如果您使用的 C + + WinRT 或 Visual c + + 组件扩展 (C + + /cli CX) 然后，使用[{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782)标记扩展，您将需要添加[ **BindableAttribute** ](https://msdn.microsoft.com/library/windows/apps/Hh701872)你想要将绑定到任何运行时类的属性。 若要使用[{x： 绑定}](https://msdn.microsoft.com/library/windows/apps/Mt204783)，不需要该属性。

```cppwinrt
// HostViewModel.idl
// Add this attribute:
[Windows.UI.Xaml.Data.Bindable]
runtimeclass HostViewModel : Windows.UI.Xaml.Data.INotifyPropertyChanged
...
```

> [!IMPORTANT]
> 如果您使用的[C + + WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)，然后[ **BindableAttribute** ](https://msdn.microsoft.com/library/windows/apps/Hh701872)属性是如果你已安装 Windows SDK 版本 (Windows 10，版本 1809年 10.0.17763.0 可用)，或更高版本。 如果没有该属性，你将需要实现[ICustomPropertyProvider](/uwp/api/windows.ui.xaml.data.icustompropertyprovider)并[ICustomProperty](/uwp/api/windows.ui.xaml.data.icustomproperty)接口，以便能够使用[{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782)标记扩展插件。

默认情况下，[{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) 假设你要绑定到标记页面的 [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713)。 因此，我们将页面的 **DataContext** 设置为绑定源类（本例中为 **HostViewModel** 类型）的实例。 下面的示例展示了用于声明绑定对象的标记。 我们使用了与前面“绑定目标”部分中所使用的相同 **Button.Content** 绑定目标，并绑定到 **HostViewModel.NextButtonText** 属性。

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

注意，该值即是我们为 **Path** 指定的值。 此值在页面 [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713) 的上下文中进行了解释，本示例已设置为 **HostViewModel** 的实例。 路径引用 **HostViewModel.NextButtonText** 属性。 我们可以省略 **Mode**，因为此处使用的是单向绑定默认值 [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782)。

UI 元素的 [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713) 默认值是已继承的其父元素的值。 显然可通过显式设置 **DataContext** 来替代该默认值，默认情况下，该值又会被子元素继承。 如果你希望多个绑定使用同一个源，则在元素上显式设置 **DataContext** 会很有用。

绑定对象具有 **Source** 属性，其默认为声明绑定所在 UI 元素的 [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713)。 你可以通过在绑定对象上显式设置 **Source**、**RelativeSource** 或 **ElementName** 来替代此默认值（有关详细信息，请参阅 [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782)）。

内部[ **DataTemplate**](https://msdn.microsoft.com/library/windows/apps/BR242348)，则[ **DataContext** ](https://msdn.microsoft.com/library/windows/apps/BR208713)自动设置为要模板化的数据对象。 下面给出的示例可用作项目控件的 **ItemTemplate**，绑定到具有名为 **Title** 和 **Description** 的字符串属性的任意类型的集合。

```xaml
<DataTemplate x:Key="SimpleItemTemplate">
    <StackPanel Orientation="Vertical" Height="50">
      <TextBlock Text="{Binding Title}"/>
      <TextBlock Text="{Binding Description"/>
    </StackPanel>
  </DataTemplate>
```

> [!NOTE]
> 默认情况下，更改为[ **TextBox.Text** ](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textbox.text)发送到双向绑定源[**文本框**](https://msdn.microsoft.com/library/windows/apps/BR209683)失去焦点。 若要在每次用户按键之后发送更改，则在标记的绑定对象上将 **UpdateSourceTrigger** 设置为 **PropertyChanged**。 当通过将 **UpdateSourceTrigger** 设置为 **Explicit** 将更改发送到绑定源时，也可完全控制这一情形。 然后，在文本框（通常为 [**TextBox.TextChanged**](https://msdn.microsoft.com/library/windows/apps/BR209683)）上处理事件，调用绑定目标上的 [**GetBindingExpression**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.getbindingexpression) 以获取 [**BindingExpression**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.bindingexpression.aspx) 对象，最后调用 [**BindingExpression.UpdateSource**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.bindingexpression.updatesource.aspx) 以编程方式更新数据源。

[  **Path**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.binding.path) 属性支持各种用于绑定到嵌套属性、附加属性以及整数和字符串索引器的语法选项。 有关详细信息，请参阅 [Property-path 语法](https://msdn.microsoft.com/library/windows/apps/Mt185586)。 绑定到字符串索引器会为你提供绑定到动态属性的效果，而无需实现 [**ICustomPropertyProvider**](https://msdn.microsoft.com/library/windows/apps/BR209878)。 [  **ElementName**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.binding.elementname) 属性对于元素间的绑定非常有用。 [  **RelativeSource**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.binding.relativesource) 属性具有多种用途，其中之一是用作模板化 [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/BR209391) 内绑定的更有效的替代方法。 有关其他设置，请参阅 [{Binding} 标记扩展](https://msdn.microsoft.com/library/windows/apps/Mt204782)和 [**Binding**](https://msdn.microsoft.com/library/windows/apps/BR209820) 类。

## <a name="what-if-the-source-and-the-target-are-not-the-same-type"></a>如果绑定源和绑定目标不是同一类型，会怎样？

如果你希望基于 Boolean 属性值控制 UI 元素的可见性，或者希望 UI 元素显示为某种颜色（这是数字值的范围或趋势函数），亦或是希望在需要字符串的 UI 元素属性中显示日期/时间值，则需要将值从一种类型转化为另一种类型。 存在这样的情形：其正确的解决方案是从绑定源类公开其他正确类型的属性，并在此处让转换逻辑保持封装和可测试。 但是，当你拥有大量源和目标属性或这两者的大型组合时，上述方案既不灵活也不可扩展。 在这种情况下，你有以下几个选项：

* 如果使用 {x:Bind}，则你可以直接绑定到函数以执行该转换。
* 或者可以指定一个值转换器，该转换器是一个旨在执行转换的对象。 

## <a name="value-converters"></a>值转换器

下面是适用于一次性或单向绑定的值转换器，可将 [**DateTime**](https://msdn.microsoft.com/library/windows/apps/xaml/system.datetime.aspx) 值转换为包含月份的字符串值。 该类实现 [**IValueConverter**](https://msdn.microsoft.com/library/windows/apps/BR209903)。

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

如果 [**Converter**](https://msdn.microsoft.com/library/windows/apps/hh701934) 参数是为绑定定义的，则绑定引擎会调用 [**Convert**](https://msdn.microsoft.com/library/windows/apps/hh701938) 和 [**ConvertBack**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.binding.converter) 方法。 从绑定源传递数据时，绑定引擎会调用 **Convert** 并将返回的数据传递给绑定目标。 从（用于双向绑定）绑定目标传递数据时，绑定引擎会调用 **ConvertBack** 并将返回的数据传递给绑定源。

转换器还具有可选参数：[**ConverterLanguage**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.binding.converterlanguage)，它允许指定要在转换中使用的语言和[ **ConverterParameter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.binding.converterparameter)，它允许将参数传递转换逻辑。 有关使用转换器参数的示例，请参阅 [**IValueConverter**](https://msdn.microsoft.com/library/windows/apps/BR209903)。

> [!NOTE]
> 如果在转换过程中没有错误，确实引发了异常。 而是返回 [**DependencyProperty.UnsetValue**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.dependencyproperty.unsetvalue)，它将停止数据传输。

若要显示在无法解析绑定源时所使用的默认值，则在标记的绑定上对象设置 **FallbackValue** 属性。 这对处理转换和格式错误很有用。 这对要绑定到可能未存在于异型绑定集合中所有对象上的源属性也同样有用。

如果你将文本控件绑定到某个值（不是字符串），则数据绑定引擎会将该值转换为字符串。 如果该值是引用类型，数据绑定引擎将通过调用 [**ICustomPropertyProvider.GetStringRepresentation**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.icustompropertyprovider.getstringrepresentation) 或 [**IStringable.ToString**](https://msdn.microsoft.com/library/Dn302136)（如果提供）来检索字符串值，否则将调用 [**Object.ToString**](https://msdn.microsoft.com/library/windows/apps/system.object.tostring.aspx)。 但是请注意，绑定引擎将忽略隐藏基类实现的任何 **ToString** 实现。 子类实现应替代基类 **ToString** 方法。 同样，在本机语言中，也会显示所有托管对象以实现 [**ICustomPropertyProvider**](https://msdn.microsoft.com/library/windows/apps/BR209878) 和 [**IStringable**](https://msdn.microsoft.com/library/Dn302135)。 但是，对 **GetStringRepresentation** 和 **IStringable.ToString** 的所有调用都路由到 **Object.ToString** 或该方法的替代，从不路由到隐藏基类实现的新 **ToString** 实现。

> [!NOTE]
> 从 Windows 10 版本 1607 开始，XAML 框架向 Visibility 转换器提供内置布尔值。 转换器将 **true** 映射到 **Visible** 枚举值并将 **false** 映射到 **Collapsed**，以便你可以将 Visibility 属性绑定到布尔值，而无需创建转换器。 若要使用内置转换器，你的应用的最低目标 SDK 版本必须为 14393 或更高版本。 当你的应用面向较早版本的 Windows 10 时，你无法使用它。 有关目标版本的详细信息，请参阅[版本自适应代码](https://msdn.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code)。

## <a name="function-binding-in-xbind"></a>{x:Bind} 中的函数绑定

{x:Bind} 使绑定步骤中的最后一步可以是一个函数。 这可以用于执行转换，以及执行依赖多个属性的绑定。 请参阅[ **x： 绑定中的函数**](function-bindings.md)

<span id="resource-dictionaries-with-x-bind"/>

## <a name="resource-dictionaries-with-xbind"></a>带有 {x:Bind} 的资源字典

[{x:Bind} 标记扩展](https://msdn.microsoft.com/library/windows/apps/Mt204783)依赖于代码生成，因此它需要一个包含可调用 **InitializeComponent** 的构造函数的代码隐藏文件（以供初始化生成的代码）。 通过实例化资源字典的类型（以便调用 **InitializeComponent**）而不是引用其文件名，来重用它。 下面是针对你拥有一个现有资源字典并且想要在其中使用 {x:Bind} 这一情形的示例。

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

[{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) 支持一种名为事件绑定的功能。 借助此功能，你可以使用绑定为事件指定处理程序（这是一种附加选项），但使用代码隐藏文件上的某一方法处理事件除外。 假设你的 **MainPage** 类上具有 **RootFrame** 属性。

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

重载方法不适用于借助此技术来处理事件。 此外，如果用于处理事件的方法包含一些参数，则这些参数均须根据各自对应的事件参数类型进行赋值。 在此情况下，[**Frame.GoForward**](https://msdn.microsoft.com/library/windows/apps/BR242693) 不重载且不含任何参数（但即使其包含两个 **object** 参数，也仍然有效）。 [**Frame.GoBack** ](https://msdn.microsoft.com/library/windows/apps/Dn996568)已重载，但是，因此我们不能使用该方法利用这种技术。

事件绑定技术类似于实现和使用命令（一个命令返回一种属性，该属性将返回实现 [**ICommand**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.input.icommand.aspx) 接口的对象）。 [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) 和 [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) 均适用于命令。 可使用 [QuizGame](https://github.com/Microsoft/Windows-appsample-quizgame) 示例（位于“Common”文件夹中）中提供的 **DelegateCommand** 帮助程序类，这样便无需多次实现命令模式。

## <a name="binding-to-a-collection-of-folders-or-files"></a>绑定到文件夹或文件集合

你可以使用 [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/BR227346) 命名空间中的 API 来检索文件夹和文件数据。 然而，各种 **GetFilesAsync**、**GetFoldersAsync** 和 **GetItemsAsync** 方法都不会返回适合绑定到列表控件的值。 必须绑定到 [**FileInformationFactory**](https://msdn.microsoft.com/library/windows/apps/Hh701422) 类的 [**GetVirtualizedFilesVector**](https://msdn.microsoft.com/library/windows/apps/Hh701428)、[**GetVirtualizedFoldersVector**](https://msdn.microsoft.com/library/windows/apps/Hh701430)和 [**GetVirtualizedItemsVector**](https://msdn.microsoft.com/library/windows/apps/BR207501) 方法的返回值。 下面的代码示例来自 [StorageDataSource 和 GetVirtualizedFilesVector 示例](https://go.microsoft.com/fwlink/p/?linkid=228621)，它将显示典型的使用模式。 请谨记在应用包清单中声明 **picturesLibrary** 功能，并确认你的 Pictures 库文件夹中有图片。

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

通常，你需要使用此方法来创建文件和文件夹信息的只读视图。 可以创建到文件和文件夹属性的双向绑定，例如，让用户在音乐视图中对歌曲进行评级。 然而，任何更改都不会永久保留，除非你调用相应的 **SavePropertiesAsync** 方法（例如，[**MusicProperties.SavePropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/BR207760)）。 当项目失去焦点时，你应该提交更改，因为这样可以触发选择重置。

注意，使用此技术的双向绑定仅适用于索引位置（例如“音乐”）。 你可以通过调用 [**FolderInformation.GetIndexedStateAsync**](https://msdn.microsoft.com/library/windows/apps/BR207627) 方法来确定是否索引某个位置。

还请注意，某个虚拟化矢量可能会在填充其值之前为某些项目返回 **null**。 例如，应该首先检查 **null**，然后才能使用绑定到虚拟化矢量的列表控件的 [**SelectedItem**](https://msdn.microsoft.com/library/windows/apps/BR209770) 值，或者改为使用 [**SelectedIndex**](https://msdn.microsoft.com/library/windows/apps/BR209768)。

## <a name="binding-to-data-grouped-by-a-key"></a>绑定到按键分组的数据

如果需要较长的项的平面集合 (丛书，例如，由**BookSku**类)，并通过使用一个公共属性作为键分组项 ( **BookSku.AuthorName**属性，例如) 然后结果称为分组的数据。 当对数据进行分组时，它将不再是简单集合。 分组的数据是组对象，其中每个组对象都有一系列

- 一个键，并
- 其属性与匹配该注册表项的项的集合。

若要再次充分的书籍示例，通过作者姓名对书籍进行分组的结果会导致其中每个组都有作者名称组的集合

- 一个键，即一个作者姓名和
- 一系列**BookSku**s 其**AuthorName**属性与组的键匹配。

通常情况下，若要显示集合，可将项目控件（例如 [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242828) 或 [**GridView**](https://msdn.microsoft.com/library/windows/apps/BR242878)）的 [**ItemsSource**](https://msdn.microsoft.com/library/windows/apps/BR242705) 直接绑定到返回集合的属性。 如果这是项的简单集合，则无需执行任何特殊操作。 但是，如果它是组对象的集合（像在绑定到分组数据时一样），则需要使用一个名为 [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833) 的中间对象，该对象位于项目控件和绑定源之间。 先将 **CollectionViewSource** 绑定到返回分组数据的属性，然后将项目控件绑定到 **CollectionViewSource**。 **CollectionViewSource** 的额外增值功能可用于跟踪当前项，以便你可以通过将多个项目控件全都绑定到同一 **CollectionViewSource** 来使其保持同步。 也可以通过 [**CollectionViewSource.View**](https://msdn.microsoft.com/library/windows/apps/BR209857) 属性返回的对象的 [**ICollectionView.CurrentItem**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.collectionviewsource.view) 属性，以编程方式访问当前项。

若要激活 [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833) 的分组功能，请将 [**IsSourceGrouped**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.collectionviewsource.issourcegrouped) 设置为 **true**。 是否还需要设置 [**ItemsPath**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.collectionviewsource.itemspath) 属性完全取决于你的组对象的创作方式。 组对象的创作方式有以下两种：“属于组”模式和“包含组”模式。 在“属于组”模式中，组对象派生自集合类型（例如，**List&lt;T&gt;**），因此事实上组对象本身就是项目组。 使用此模式，无需设置 **ItemsPath**。 在“包含组”模式中，组对象具有一个或多个集合类型属性（例如 **List&lt;T&gt;**），因此“包含组”的单个项目组采用单个属性的形式（而多个项目组则采用多个属性的形式）。 使用此模式，需要将 **ItemsPath** 设置为包含项目组的属性名。

下面的示例阐述了“包含组”模式。 页面类有一个名为 [**ViewModel**](https://msdn.microsoft.com/library/windows/apps/BR208713) 的属性，该属性将返回我们的视图模型的实例。 [  **CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833) 绑定到视图模型的 **Authors** 属性（**Authors** 是组对象的集合），还指定了它是包含分组项目的 **Author.BookSkus** 属性。 最后，[**GridView**](https://msdn.microsoft.com/library/windows/apps/BR242705) 绑定到 **CollectionViewSource** 且具有其已定义的组样式，这样它便可以采用分组形式呈现项目。

```csharp
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

你可以采用以下两种方式之一实现“属于组”模式。 一种方法是创作你自己的组类。 从 **List&lt;T&gt;** 派生类（其中 *T* 是项目类型）。 例如， `public class Author : List<BookSku>`。 另一种方式是，使用 [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) 表达式，从诸如 **BookSku** 项目的属性值等动态创建组对象（以及组类）。 此方法（仅维护项目的简单列表并将其动态分组在一起）是从云服务访问数据的应用的典型用法。 可以灵活地按作者或流派对书籍进行分组，而无需特殊组类，如 **Author** 和 **Genre**。

下面的示例使用 [LINQ](https://msdn.microsoft.com/library/bb397926.aspx) 阐述了“属于组”模式。 在本示例中我们按流派对书籍进行分组，并在组标题中显示流派名。 这由引用组 [**Key**](https://msdn.microsoft.com/library/windows/apps/bb343251.aspx) 的值的“Key”属性路径进行指示。

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

请记住，在将 [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) 用于数据模板时，需通过设置 **x:DataType** 值来指示要绑定到的类型。 如果类型是我们无法在标记中解释的泛型，则需要在组样式标头模板中改用 [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782)。

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

[  **SemanticZoom**](https://msdn.microsoft.com/library/windows/apps/Hh702601) 控件是使用户查看和导航分组数据的绝佳方法。 [Bookstore2](https://go.microsoft.com/fwlink/?linkid=532952) 示例应用演示了如何使用 **SemanticZoom**。 在该应用中，你可以查看按作者分组的书籍列表（放大视图），也可以缩小以查看作者的跳转列表（缩小视图）。 与在书籍列表中上下滚动相比，跳转列表提供了更快速的浏览方式。 实际上，放大视图和缩小视图是绑定到同一 **CollectionViewSource** 的 **ListView** 或 **GridView** 控件。

![SemanticZoom 图示](images/sezo.png)

当绑定到分层数据（如类别中的子类别）时，你可以选择使用一系列的项目控件，在 UI 中显示分层级别。 所选项目控件决定后续项目控件的内容。 你可以使列表保持同步，方法是将各列表绑定到其各自的 [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833) 并将 **CollectionViewSource** 实例一同绑定到一个链中。 这称为主视图/详细信息（或列表/详细信息）视图。 有关详细信息，请参阅[如何绑定到分层数据并创建主视图/详细信息视图](how-to-bind-to-hierarchical-data-and-create-a-master-details-view.md)。

<span id="debugging"/>

## <a name="diagnosing-and-debugging-data-binding-problems"></a>诊断并调试数据绑定问题

绑定标记包含属性名称（对于 C#，有时是字段和方法）。 因此，在重命名属性时，你还需要更改对其进行引用的任何绑定。 若忘记执行此操作，会导致出现一个典型的数据绑定 Bug，并且你的应用将不能编译或不能正常运行。

由 [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) 和 [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) 创建的绑定对象在功能上大致等同。 但是，{x:Bind} 具有绑定源的类型信息，并且会在编译时生成源代码。 借助 {x:Bind}，可使用剩余代码检测同一类型的问题。 这包括编译时对绑定表达式的验证，以及通过在作为页面的部分类生成的源代码中设置断点来调试。 可以在 `obj` 文件夹的文件中找到这些类，如文件夹名为（适用于 C#）`<view name>.g.cs`。 如果你有绑定方面的问题，请打开 Microsoft Visual Studio 调试器中的**出现未处理的异常时中断**。 该调试器将在该点处中断执行，以便你可以调试哪里出现了问题。 对于绑定源节点图形的每个部分，{x:Bind} 生成的代码均遵循相同的模式，你可以使用“调用堆栈”窗口中的信息来帮助确定导致该问题出现的调用顺序。

[{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) 不具有绑定源的类型信息。 但在通过附加的调试器运行你的应用时，Visual Studio 中的“输出”窗口中将显示所有绑定错误。

## <a name="creating-bindings-in-code"></a>使用代码创建绑定

**请注意**  此部分仅适用于[{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782)，这是因为您不能创建[{x： 绑定}](https://msdn.microsoft.com/library/windows/apps/Mt204783)在代码中的绑定。 不过，使用 [**DependencyObject.RegisterPropertyChangedCallback**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.dependencyobject.registerpropertychangedcallback.aspx) 同样可以获得 {x:Bind} 的某些相同优势，这使你可以在任何依赖属性上注册更改通知。

还可以使用规程代码而不是 XAML 来将 UI 元素连接到数据。 为此，请创建一个新 [**Binding**](https://msdn.microsoft.com/library/windows/apps/BR209820) 对象，设置相应的属性，然后调用 [**FrameworkElement.SetBinding**](https://msdn.microsoft.com/library/windows/apps/br244257.aspx) 或 [**BindingOperations.SetBinding**](https://msdn.microsoft.com/library/windows/apps/br244376.aspx)。 如果你希望在运行时选择绑定属性值或在多个控件中共享单个绑定，则以编程方式创建绑定将十分有用。 但是请注意，调用 **SetBinding** 后，无法更改绑定属性值。

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
| 索引器 | `{x:Bind Groups[2].Title}` | `{Binding Groups[2].Title}` | 绑定到集合中的指定项。 仅支持基于整数的索引。 | 
| 附加属性 | `{x:Bind Button22.(Grid.Row)}` | `{Binding Button22.(Grid.Row)}` | 附加属性用括号进行指定。 如果未在 XAML 命名空间中声明该属性，则在其前面加上 xml 命名空间，这应该映射到文档的标头处的代码命名空间中。 | 
| 强制转换 | `{x:Bind groups[0].(data:SampleDataGroup.Title)}` | 不需要。 | 强制转换用括号进行指定。 如果未在 XAML 命名空间中声明该属性，则在其前面加上 xml 命名空间，这应该映射到文档的标头处的代码命名空间中。 | 
| Converter | `{x:Bind IsShown, Converter={StaticResource BoolToVisibility}}` | `{Binding IsShown, Converter={StaticResource BoolToVisibility}}` | 转换器必须在 Page/ResourceDictionary 的根目录处或在 App.xaml 中进行声明。 | 
| ConverterParameter, ConverterLanguage | `{x:Bind IsShown, Converter={StaticResource BoolToVisibility}, ConverterParameter=One, ConverterLanguage=fr-fr}` | `{Binding IsShown, Converter={StaticResource BoolToVisibility}, ConverterParameter=One, ConverterLanguage=fr-fr}` | 转换器必须在 Page/ResourceDictionary 的根目录处或在 App.xaml 中进行声明。 | 
| TargetNullValue | `{x:Bind Name, TargetNullValue=0}` | `{Binding Name, TargetNullValue=0}` | 在绑定表达式的叶为 null 时使用。 对于字符串值，应使用单引号。 | 
| FallbackValue | `{x:Bind Name, FallbackValue='empty'}` | `{Binding Name, FallbackValue='empty'}` | 绑定的路径的任何部分（叶除外）为 null 时使用。 | 
| ElementName | `{x:Bind slider1.Value}` | `{Binding Value, ElementName=slider1}` | 使用 {x:Bind}，绑定到某一字段；Path 默认位于 Page 的根处，以便任意命名的元素均可通过其字段进行访问。 | 
| RelativeSource:Self | `<Rectangle x:Name="rect1" Width="200" Height="{x:Bind rect1.Width}" ... />` | `<Rectangle Width="200" Height="{Binding Width, RelativeSource={RelativeSource Self}}" ... />` | 借助 {x:Bind}，对元素进行命名并在 Path 中使用其名称。 | 
| RelativeSource:TemplatedParent | 不需要 | `{Binding <path>, RelativeSource={RelativeSource TemplatedParent}}` | 用于 {x： 绑定} 上 ControlTemplate TargetType 指示绑定到模板父级。 为 {Binding} 常规模板绑定可用于在控件模板中大多数使用。 但在使用 TemplatedParent 时，需要使用转换器或双向绑定。&lt; | 
| 来源 | 不需要 | `<ListView ItemsSource="{Binding Orders, Source={StaticResource MyData}}"/>` | 对于 {x： 绑定} 可以直接使用的命名的元素使用的属性或静态的路径。 | 
| 模式 | `{x:Bind Name, Mode=OneWay}` | `{Binding Name, Mode=TwoWay}` | 模式可以是一次性、单向或双向。 {x:Bind} defaults to OneTime; {Binding} defaults to OneWay. | 
| UpdateSourceTrigger | `{x:Bind Name, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}` | `{Binding UpdateSourceTrigger=PropertyChanged}` | UpdateSourceTrigger 可以是 Default、LostFocus 或 PropertyChanged。 {x:Bind} 不支持 UpdateSourceTrigger=Explicit。 {x:Bind} 可在所有情况下（TextBox.Text 除外，它使用 LostFocus 行为）使用 PropertyChanged 行为。 | 


