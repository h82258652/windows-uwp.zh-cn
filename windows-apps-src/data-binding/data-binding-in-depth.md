---
ms.assetid: 41E1B4F1-6CAF-4128-A61A-4E400B149011
title: 深入了解数据绑定
description: 数据绑定是你的应用 UI 用来显示数据的一种方法，可以选择与该数据保持同步。
---
# 深入了解数据绑定

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


** 重要的 API **

-   [**Binding 类**](https://msdn.microsoft.com/library/windows/apps/BR209820)
-   [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713)
-   [**INotifyPropertyChanged**](https://msdn.microsoft.com/library/windows/apps/BR209899)

**注意** 本主题将详细介绍数据绑定功能。 有关既简短又实用的简介，请参阅[数据绑定概述](data-binding-quickstart.md)。

 

数据绑定是你的应用 UI 用来显示数据的一种方法，可以选择与该数据保持同步。 借助数据绑定，你可以将关注的数据从关注的 UI 中分离开来，从而可形成一个更简易的概念模型，并且使你的应用拥有更好的可读性、可测试性和可维护性。

当 UI首次显示时，你可以使用数据绑定以仅显示来自数据源的值，但不会对这些值中的更改做出响应。 这称为一次性绑定，非常适合在运行时期间其值不会更改的数据。 或者，你可以选择“观察”值并在其更改时更新 UI。 这称为单向绑定，非常适合只读数据。 最后，你可以选择观察并更新，以便用户在 UI 中对值所做的更改能自动传回数据源。 这称为双向绑定，非常适合读写数据。 下面是一些示例。

-   你可以使用一次性绑定，将 [**Image**](https://msdn.microsoft.com/library/windows/apps/BR242752) 绑定到当前用户的照片。
-   你可以使用单向绑定，将 [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) 绑定到按报纸剪辑分组的实时新闻报道的集合。
-   你可以使用双向绑定，将 [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683) 绑定到表单中的客户名称。

有两种类型的绑定，它们通常都在 UI 标记中进行声明。 你既可以选用 [{x:Bind} 标记扩展](https://msdn.microsoft.com/library/windows/apps/Mt204783)，也可以选用 [{Binding} 标记扩展](https://msdn.microsoft.com/library/windows/apps/Mt204782)。 还可以在同一应用中（甚至是同一 UI 元素上）混合使用这两个标记扩展。 {x:Bind} 是 Windows 10 的新增内容，且具备更好的性能。 {Binding} 具有更多功能。 除非另有明确说明，否则本主题中介绍的所有详细信息均适用于这两种绑定。

**用于演示 {x:Bind} 的应用示例**

-   [{x:Bind} 示例](http://go.microsoft.com/fwlink/p/?linkid=619989)。
-   [QuizGame](https://github.com/Microsoft/Windows-appsample-quizgame)。
-   [XAML UI 基本示例](http://go.microsoft.com/fwlink/p/?linkid=619992)。

**用于演示 {Binding} 的应用示例**

-   下载 [Bookstore1](http://go.microsoft.com/fwlink/?linkid=532950) 应用。
-   下载 [Bookstore2](http://go.microsoft.com/fwlink/?linkid=532952) 应用。

每个绑定均由以下部分构成
------------------------------------

-   *绑定源*。 这是用于绑定的数据源，它可以是任意类的实例，其中该类具有其值要显示在 UI 中的成员。
-   *绑定目标*。 这是显示数据的 UI 中 [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/BR208706) 的 [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/BR242362)。
-   *绑定对象*。 这是用于将数据值从绑定源传递给绑定目标（或从绑定目标传递回绑定源）的组件。 绑定对象通过 [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) 或 [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) 标记扩展在 XAML 加载时进行创建。

在以下部分中，我们将进一步探讨绑定源、绑定目标和绑定对象。 我们将这些部分与关于将按钮内容绑定到名为 **NextButtonText** 的字符串属性（这属于名为 **HostViewModel** 的类）的示例链接在一起。

绑定源
--------------

下面是一个非常基础的可用作绑定源的类实现。

**注意** 如果要将 [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) 与 Visual C++ 组件扩展 (C++/CX) 结合使用，你需要将 [**BindableAttribute**](https://msdn.microsoft.com/library/windows/apps/Hh701872) 属性添加到你的绑定源类。 如果使用的是 [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783)，则不需要该属性。 有关代码段的信息，请参阅[添加详细信息视图](data-binding-quickstart.md#adding-a-details-view)。

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

**HostViewModel** 及其属性 **NextButtonText** 的实现仅适用于一次性绑定。 而单向绑定和双向绑定也极为常见，在这两种绑定中，UI 会自动更新以响应绑定源的数据值的更改。 为了让这些类型的绑定能正常工作，需要使绑定源对绑定对象“可观察”。 因此在我们的示例中，如果我们希望单向或双向绑定到 **NextButtonText** 属性，则该属性值在运行时所发生的任何更改均需对绑定对象可观察。

执行此操作的方法之一是，从 [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/BR242356) 派生代表绑定源的类，并通过 [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/BR242362) 公开数据值。 以上就是 [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/BR208706) 成为可观察绑定源的方式。 **FrameworkElements** 即时可用，是很好的绑定源。

让类成为可观察绑定源，以及成为类（已拥有基类）的必需项的更便捷方法是实现 [**System.ComponentModel.INotifyPropertyChanged**](T:System.ComponentModel.INotifyPropertyChanged)。 实际上，这仅涉及到实现一个名为 **PropertyChanged** 的事件。 下面展示了使用 **HostViewModel** 的示例。

**注意** 对于 C++/CX，应实现 [**Windows::UI::Xaml::Data::INotifyPropertyChanged**](https://msdn.microsoft.com/library/windows/apps/BR209899)，并且绑定源类必须具有 [**BindableAttribute**](https://msdn.microsoft.com/library/windows/apps/Hh701872) 或实现 [**ICustomPropertyProvider**](https://msdn.microsoft.com/library/windows/apps/BR209878)。

``` csharp
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

现在，**NextButtonText** 属性可供观察。 当创作到该属性的单向或双向绑定（将在稍后介绍操作方法）时，生成的绑定对象将订阅 **PropertyChanged** 事件。 引发该事件后，绑定对象的处理程序将接收一个包含已更改的属性名的参数。 这是绑定对象知道已更改和重新读取的属性值的方式。

这样你便无需多次实现上面所示的模式，只需从 [QuizGame](https://github.com/Microsoft/Windows-appsample-quizgame) 示例（位于“Common”文件夹中）提供的 **BindableBase** 基类派生即可。 下面是具体操作的一个示例。

``` csharp
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

通过使用 [**String.Empty**](T:System.String) 或 **null** 参数引发 **PropertyChanged** 事件，以指示对象上的所有非索引器属性应重新读取。 可以通过将“Item\[*indexer*\]”的参数用于特定索引器（其中 *indexer* 是索引值），或将“Item\[\]”的值用于所有索引器，引发该事件以指示对象上的索引器属性已更改。

可以将绑定源视为其属性中包含数据的单个对象或一些对象的集合。 在 C# 和 Visual Basic 代码中，可以一次性绑定到实现 [**List(Of T)**](T:System.Collections.Generic.List%601) 的对象，以显示运行时未更改的集合。 对于可观察集合（在将项添加到集合或从集合中删除时进行观察），应改为单向绑定到 [**ObservableCollection(Of T)**](T:System.Collections.ObjectModel.ObservableCollection%601)。 在 C++ 代码中，对于可观察集合和不可观察集合，均可绑定到 [**Vector<T>**](https://msdn.microsoft.com/en-us/library/dn858385.aspx)。 若要绑定到你自己的集合类，请按照下表中的指南进行操作。

| 方案                                                        | C# 和 VB (CLR)                                                                                                                                                                                                                                                                                                                                                                                                                   | C++/CX                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
|-----------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 绑定到对象。                                              | 可以为任何对象。                                                                                                                                                                                                                                                                                                                                                                                                                 | 对象必须具有 [**BindableAttribute**](https://msdn.microsoft.com/library/windows/apps/Hh701872) 或实现 [**ICustomPropertyProvider**](https://msdn.microsoft.com/library/windows/apps/BR209878)。                                                                                                                                                                                                                                                                                                             |
| 从绑定对象中获取属性更改更新。                | 对象必须实现 [**System.ComponentModel. INotifyPropertyChanged**](T:System.ComponentModel.INotifyPropertyChanged)。                                                                                                                                                                                                                                                                                                         | 对象必须实现 [**Windows.UI.Xaml.Data. INotifyPropertyChanged**](https://msdn.microsoft.com/library/windows/apps/BR209899)。                                                                                                                                                                                                                                                                                                                                                           |
| 绑定到集合。                                           | [**List(Of T)**](T:System.Collections.Generic.List%601)                                                                                                                                                                                                                                                                                                                                                                            | [**Platform::Collections::Vector<T>**](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh441570.aspx)                                                                                                                                                                                                                                                                                                                                                                                         |
| 从绑定集合中获取集合更改更新。          | [**ObservableCollection(Of T)**](T:System.Collections.ObjectModel.ObservableCollection%601)                                                                                                                                                                                                                                                                                                                                        | [**Platform::Collections::Vector<T>**](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh441570.aspx)                                                                                                                                                                                                                                                                                                                                                                                         |
| 实现支持绑定的集合。                   | 扩展 [**List(Of T)**](T:System.Collections.Generic.List%601) 或者实现 [**IList**](T:System.Collections.IList)、[**IList**](T:System.Collections.Generic.IList%601)(Of [**Object**](T:System.Object))、[**IEnumerable**](T:System.Collections.IEnumerable) 或 [**IEnumerable**](T:System.Collections.Generic.IEnumerable%601)(Of **Object**)。 绑定到通用 **IList(Of T)** 和 **IEnumerable(Of T)** 的操作不受支持。 | 实现 [**IBindableVector**](https://msdn.microsoft.com/library/windows/apps/Hh701979)、[**IBindableIterable**](https://msdn.microsoft.com/library/windows/apps/Hh701957)、[**IVector**](https://msdn.microsoft.com/library/windows/apps/BR206631)<[**Object**](T:System.Object)^>、[**IIterable**](https://msdn.microsoft.com/library/windows/apps/BR226024)<**Object**^>、**IVector**<[**IInspectable**](https://msdn.microsoft.com/library/BR205821)\*> 或 **IIterable**<**IInspectable**\*>。 绑定到通用 **IVector<T>** 和 **IIterable<T>** 的操作不受支持。 |
| 实现支持集合更改更新的集合。 | 扩展 [**ObservableCollection(Of T)**](T:System.Collections.ObjectModel.ObservableCollection%601) 或实现（非通用）[**IList**](T:System.Collections.IList) 和 [**INotifyCollectionChanged**](T:System.Collections.Specialized.INotifyCollectionChanged)。                                                                                                                                                               | 实现 [**IBindableVector**](https://msdn.microsoft.com/library/windows/apps/Hh701979) 和 [**IBindableObservableVector**](https://msdn.microsoft.com/library/windows/apps/Hh701974)。                                                                                                                                                                                                                                                                                                                       |
| 实现支持增量加载的集合。       | 扩展 [**ObservableCollection(Of T)**](T:System.Collections.ObjectModel.ObservableCollection%601) 或实现（非通用）[**IList**](T:System.Collections.IList) 和 [**INotifyCollectionChanged**](T:System.Collections.Specialized.INotifyCollectionChanged)。 此外，还实现 [**ISupportIncrementalLoading**](https://msdn.microsoft.com/library/windows/apps/Hh701916)。                                                          | 实现 [**IBindableVector**](https://msdn.microsoft.com/library/windows/apps/Hh701979)、[**IBindableObservableVector**](https://msdn.microsoft.com/library/windows/apps/Hh701974) 和 [**ISupportIncrementalLoading**](https://msdn.microsoft.com/library/windows/apps/Hh701916)。                                                                                                                                                                                                                                         |

 
你可以使用增量加载将列表控件绑定到任意的大型数据源，且仍获得高性能。 例如，你可以将列表控件绑定到必应图像查询结果，而无需一次性加载所有结果。 你只需立即加载部分结果，再根据需要加载其他结果。 为支持增量加载，你必须在支持集合更改通知的数据源上实现 [**ISupportIncrementalLoading**](https://msdn.microsoft.com/library/windows/apps/Hh701916)。 当数据绑定引擎请求更多数据时，你的数据源必须发出相应的请求、集成结果，然后发送相应的通知以更新 UI。

绑定目标
--------------

在下面的两个示例中，**Button.Content** 属性是绑定目标，并且其值已设置为用于声明绑定对象的标记扩展。 首先显示的是 [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783)，然后是 [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782)。 在标记中声明绑定这一做法很常见（其既便捷、易读又很实用）。 不过，应避免标记和以命令性方式（编程方式）创建 [**Binding**](https://msdn.microsoft.com/library/windows/apps/BR209820) 类的实例（除非需要）。

<!-- XAML lang specifier not yet supported in OP. Using XML for now. -->
``` xml
<Button Content="{x:Bind ...}" ... /> 
```

``` xml
<Button Content="{Binding ...}" ... /> 
```

Binding object declared using {x:Bind}
--------------------------------------

There's one step we need to do before we author our [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) markup. We need to expose our binding source class from the class that represents our page of markup. We do that by adding a property (of type **HostViewModel** in this case) to our **HostView** page class.

``` csharp
namespace QuizGame.View
{
    public sealed partial class HostView : Page
    {
        public HostView()
        {
            this.InitializeComponent();
            this.ViewModel = new HostViewModel();
        }
    
        public HostViewModel ViewModel { get; set; }
    }
}
``` 该操作完成后，即可立即仔细查看声明绑定对象的标记。 下面的示例使用与前面“绑定目标”部分中所使用的相同 **Button.Content** 绑定目标，并演示了将其绑定到 **HostViewModel.NextButtonText** 属性。

``` xml
<Page x:Class="QuizGame.View.HostView" ... >
    <Button Content="{x:Bind Path=ViewModel.NextButtonText, Mode=OneWay}" ... />
</Page>
```

Notice the value that we specify for **Path**. This value is interpreted in the context of the page itself, and in this case the path begins by referencing the **ViewModel** property that we just added to the **HostView** page. That property returns a **HostViewModel** instance, and so we can dot into that object to access the **HostViewModel.NextButtonText** property. And we specify **Mode**, to override the [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) default of one-time.

The [**Path**](https://msdn.microsoft.com/library/windows/apps/BR209820-path) property supports a variety of syntax options for binding to nested properties, attached properties, and integer and string indexers. For more info, see [Property-path syntax](https://msdn.microsoft.com/library/windows/apps/Mt185586). Binding to string indexers gives you the effect of binding to dynamic properties without having to implement [**ICustomPropertyProvider**](https://msdn.microsoft.com/library/windows/apps/BR209878). For other settings, see [{x:Bind} markup extension](https://msdn.microsoft.com/library/windows/apps/Mt204783).

**Note**  Changes to [**TextBox.Text**](https://msdn.microsoft.com/library/windows/apps/BR209683-text) are sent to a two-way bound source when the [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683) loses focus, and not after every user keystroke.

**DataTemplate and x:DataType**

Inside a [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/BR242348) (whether used as an item template, a content template, or a header template), the value of **Path** is not interpreted in the context of the page, but in the context of the data object being templated. So that its bindings can be validated (and efficient code generated for them) at compile-time, a **DataTemplate** needs to declare the type of its data object using **x:DataType**. The example given below could be used as the **ItemTemplate** of an items control bound to a collection of **SampleDataGroup** objects.

``` xml
<DataTemplate x:Key="SimpleItemTemplate" x:DataType="data:SampleDataGroup">
    <StackPanel Orientation="Vertical" Height="50">
      <TextBlock Text="{x:Bind Title}"/>
      <TextBlock Text="{x:Bind Description}"/>
    </StackPanel>
  </DataTemplate>
```

**Weakly-typed objects in your Path**

Consider for example that you have a type named SampleDataGroup, which implements a string property named Title. And you have a property MainPage.SampleDataGroupAsObject, which is of type object but which actually returns an instance of SampleDataGroup. The binding `<TextBlock Text="{x:Bind SampleDataGroupAsObject.Title}"/>` will result in a compile error because the Title property is not found on the type object. The remedy for this is to add a cast to your Path syntax like this: `<TextBlock Text="{x:Bind SampleDataGroupAsObject.(data:SampleDataGroup.Title)}"/>`. Here's another example where Element is declared as object but is actually a TextBlock: `<TextBlock Text="{x:Bind Element.Text}"/>`. And a cast remedies the issue: `<TextBlock Text="{x:Bind Element.(TextBlock.Text)}"/>`.

**如果你的数据异步加载** 编译时会为你的页面在分部类中生成支持 **{x:Bind}** 的代码。 可以在 `obj` 文件夹中找到这些文件，其名称类似于（适用于 C#）“<view name>.g.cs`. The generated code includes a handler for your page's [**Loading**](https://msdn.microsoft.com/library/windows/apps/BR208706-loading) event, and that handler calls the **Initialize** method on a generated class that represent's your page's bindings. **Initialize** in turn calls **Update** to begin moving data between the binding source and the target. **Loading** is raised just before the first measure pass of the page or user control. So if your data is loaded asynchronously it may not be ready by the time **Initialize** is called. So, after you've loaded data, you can force one-time bindings to be initialized by calling `this->Bindings->Update();”。 如果只需针对异步加载的数据执行一次性绑定，则使用此方法对其进行初始化比使用以下方法方便：首先进行单向绑定，然后侦听更改。 如果你的数据未经过细微的更改并且它可能作为某个特定操作的一部分进行更新，则你可以执行一次性绑定，并且通过对 **Update** 的调用随时强制执行手动更新。

**限制** 

**{x:Bind}** 既不适用于后期绑定方案（例如导航 JSON 对象的字典结构），也不适用于鸭子类型，该类型是基于属性名称的词法匹配的一种较弱形式的类型（“如果它的走路方式、游泳方式和叫声像鸭子，那么它就是一只鸭子”）。 借助鸭子类型，与 Age 属性的绑定将会同样满足 Person 或 Wine 对象。 对于这些方案，请使用 **{Binding}**。

默认情况下，使用 {Binding} ---------------------------------------

[{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) 声明的绑定对象假定绑定到标记页面的 [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713)。 因此，我们将页面的 **DataContext** 设置为绑定源类（本例中为 **HostViewModel** 类型）的实例。 下面的示例展示了用于声明绑定对象的标记。 我们使用了与前面“绑定目标”部分中所使用的相同 **Button.Content** 绑定目标，并绑定到 **HostViewModel.NextButtonText** 属性。

``` xml
<Page xmlns:viewmodel="using:QuizGame.ViewModel" ... >
    <Page.DataContext>
        <viewmodel:HostViewModel/>
    </Page.DataContext>
    ...
    <Button Content="{Binding Path=NextButtonText}" ... />
</Page>
```

Notice the value that we specify for **Path**. This value is interpreted in the context of the page's [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713), which in this example is set to an instance of **HostViewModel**. The path references the **HostViewModel.NextButtonText** property. We can omit **Mode**, because the [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) default of one-way works here.

The default value of [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713) for a UI element is the inherited value of its parent. You can of course override that default by setting **DataContext** explicitly, which is in turn inherited by children by default. Setting **DataContext** explicitly on an element is useful when you want to have multiple bindings that use the same source.

A binding object has a **Source** property, which defaults to the [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713) of the UI element on which the binding is declared. You can override this default by setting **Source**, **RelativeSource**, or **ElementName** explicitly on the binding (see [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) for details).

Inside a [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/BR242348), the [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713) is set to the data object being templated. The example given below could be used as the **ItemTemplate** of an items control bound to a collection of any type that has string properties named **Title** and **Description**.

``` xml
<DataTemplate x:Key="SimpleItemTemplate">
    <StackPanel Orientation="Vertical" Height="50">
      <TextBlock Text="{Binding Title}"/>
      <TextBlock Text="{Binding Description"/>
    </StackPanel>
  </DataTemplate>
```

**Note**  By default, changes to [**TextBox.Text**](https://msdn.microsoft.com/library/windows/apps/BR209683-text) are sent to a two-way bound source when the [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683) loses focus. To cause changes to be sent after every user keystroke, set **UpdateSourceTrigger** to **PropertyChanged** on the binding in markup. You can also completely take control of when changes are sent to the source by setting **UpdateSourceTrigger** to **Explicit**. You then handle events on the text box (typically [**TextBox.TextChanged**](https://msdn.microsoft.com/library/windows/apps/BR209683-textchanged)), call [**GetBindingExpression**](https://msdn.microsoft.com/library/windows/apps/BR208706-getbindingexpression) on the target to get a [**BindingExpression**](https://msdn.microsoft.com/library/windows/apps/BR209820expression) object, and finally call [**BindingExpression.UpdateSource**](https://msdn.microsoft.com/library/windows/apps/BR209820expression-updatesource) to programmatically update the data source.

The [**Path**](https://msdn.microsoft.com/library/windows/apps/BR209820-path) property supports a variety of syntax options for binding to nested properties, attached properties, and integer and string indexers. For more info, see [Property-path syntax](https://msdn.microsoft.com/library/windows/apps/Mt185586). Binding to string indexers gives you the effect of binding to dynamic properties without having to implement [**ICustomPropertyProvider**](https://msdn.microsoft.com/library/windows/apps/BR209878). The [**ElementName**](https://msdn.microsoft.com/library/windows/apps/BR209820-elementname) property is useful for element-to-element binding. The [**RelativeSource**](https://msdn.microsoft.com/library/windows/apps/BR209820-relativesource) property has several uses, one of which is as a more powerful alternative to template binding inside a [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/BR209391). For other settings, see [{Binding} markup extension](https://msdn.microsoft.com/library/windows/apps/Mt204782) and the [**Binding**](https://msdn.microsoft.com/library/windows/apps/BR209820) class.

What if the source and the target are not the same type?
--------------------------------------------------------

If you want to control the visibility of a UI element based on the value of a boolean property, or if you want to render a UI element with a color that's a function of a numeric value's range or trend, or if you want to display a date and/or time value in a UI element property that expects a string, then you'll need to convert values from one type to another. There will be cases where the right solution is to expose another property of the right type from your binding source class, and keep the conversion logic encapsulated and testable there. But that isn't flexible nor scalable when you have large numbers, or large combinations, of source and target properties. In that case you'll want to use something known as a value converter. This section describes how to implement and consume a value converter.

Here's a value converter, suitable for a one-time or a one-way binding, that converts a [**DateTime**](T:System.DateTime) value to a string value containing the month. The class implements [**IValueConverter**](https://msdn.microsoft.com/library/windows/apps/BR209903).

``` csharp
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

``` vbnet
Public Class DateToStringConverter
    Implements IValueConverter

    ' Define the Convert method to change a DateTime object to
    ' a month string.
    Public Function Convert(ByVal value As Object, -
        ByVal targetType As Type, ByVal parameter As Object, -
        ByVal language As String) As Object -
        Implements IValueConverter.Convert

        ' value is the data from the source object.
        Dim thisdate As DateTime = CType(value, DateTime)
        Dim monthnum As Integer = thisdate.Month
        Dim month As String
        Select Case (monthnum)
            Case 1
                month = "January"
            Case 2
                month = "February"
            Case Else
                month = "Month not found"
        End Select
        ' Return the value to pass to the target.
        Return month

    End Function

    ' ConvertBack is not implemented for a OneWay binding.
    Public Function ConvertBack(ByVal value As Object, -
        ByVal targetType As Type, ByVal parameter As Object, -
        ByVal language As String) As Object -
        Implements IValueConverter.ConvertBack

        Throw New NotImplementedException

    End Function
End Class
``` 下面介绍了如何在绑定对象标记中使用该值转换器。

``` xml
<UserControl.Resources>
  <local:DateToStringConverter x:Key="Converter1"/>
</UserControl.Resources>

...

<TextBlock Grid.Column="0" 
  Text="{x:Bind ViewModel.Month, Converter={StaticResource Converter1}}"/>

<TextBlock Grid.Column="0" 
  Text="{Binding Month, Converter={StaticResource Converter1}}"/>
```

The binding engine calls the [**Convert**](https://msdn.microsoft.com/library/windows/apps/BR209903-convert) and [**ConvertBack**](https://msdn.microsoft.com/library/windows/apps/BR209903-convertback) methods if the [**Converter**](https://msdn.microsoft.com/library/windows/apps/BR209820-converter) parameter is defined for the binding. When data is passed from the source, the binding engine calls **Convert** and passes the returned data to the target. When data is passed from the target (for a two-way binding), the binding engine calls **ConvertBack** and passes the returned data to the source.

The converter also has optional parameters: [**ConverterLanguage**](https://msdn.microsoft.com/library/windows/apps/BR209820-converterlanguage), which allows specifying the language to be used in the conversion, and [**ConverterParameter**](https://msdn.microsoft.com/library/windows/apps/BR209820-converterparameter), which allows passing a parameter for the conversion logic. For an example that uses a converter parameter, see [**IValueConverter**](https://msdn.microsoft.com/library/windows/apps/BR209903).

**Note**  If there is an error in the conversion, do not throw an exception. Instead, return [**DependencyProperty.UnsetValue**](https://msdn.microsoft.com/library/windows/apps/BR242362-unsetvalue), which will stop the data transfer.

To display a default value to use whenever the binding source cannot be resolved, set the **FallbackValue** property on the binding object in markup. This is useful to handle conversion and formatting errors. It is also useful to bind to source properties that might not exist on all objects in a bound collection of heterogeneous types.

If you bind a text control to a value that is not a string, the data binding engine will convert the value to a string. If the value is a reference type, the data binding engine will retrieve the string value by calling [**ICustomPropertyProvider.GetStringRepresentation**](https://msdn.microsoft.com/library/windows/apps/BR209878-getstringrepresentation) or [**IStringable.ToString**](https://msdn.microsoft.com/library/Dn302136) if available, and will otherwise call [**Object.ToString**](M:System.Object.ToString). Note, however, that the binding engine will ignore any **ToString** implementation that hides the base-class implementation. Subclass implementations should override the base class **ToString** method instead. Similarly, in native languages, all managed objects appear to implement [**ICustomPropertyProvider**](https://msdn.microsoft.com/library/windows/apps/BR209878) and [**IStringable**](https://msdn.microsoft.com/library/Dn302135). However, all calls to **GetStringRepresentation** and **IStringable.ToString** are routed to **Object.ToString** or an override of that method, and never to a new **ToString** implementation that hides the base-class implementation.

Resource dictionaries with {x:Bind}
-----------------------------------

The [{x:Bind} markup extension](https://msdn.microsoft.com/library/windows/apps/Mt204783) depends on code generation, so it needs a code-behind file containing a constructor that calls **InitializeComponent** (to initialize the generated code). You re-use the resource dictionary by instantiating its type (so that **InitializeComponent** is called) instead of referencing its filename. Here's an example of what to do if you have an existing resource dictionary and you want to use {x:Bind} in it.

TemplatesResourceDictionary.xaml

``` xml
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

``` csharp
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
``` MainPage.xaml ``` xml
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

Event binding and ICommand
--------------------------

[{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) supports a feature called event binding. With this feature, you can specify the handler for an event using a binding, which is an additional option on top of handling events with a method on the code-behind file. Let's say you have a **RootFrame** property on your **MainPage** class.

``` csharp
    public sealed partial class MainPage : Page
    {
        ....    
        public Frame RootFrame { get { return Window.Current.Content as Frame; } }
    }
``` 然后，你可以将按钮的 **Click** 事件绑定到由 **RootFrame** 属性返回的 **Frame** 对象上的方法，如下所示。 注意，我们还将按钮的 **IsEnabled** 属性绑定到同一 **Frame** 的另一成员。

``` xml
    <AppBarButton Icon="Forward" IsCompact="True"
    IsEnabled="{x:Bind RootFrame.CanGoForward, Mode=OneWay}"
    Click="{x:Bind RootFrame.GoForward}"/>
```

Overloaded methods cannot be used to handle an event with this technique. Also, if the method that handles the event has parameters then they must all be assignable from the types of all of the event's parameters, respectively. In this case, [**Frame.GoForward**](https://msdn.microsoft.com/library/windows/apps/BR242693) is not overloaded and it has no parameters (but it would still be valid even if it took two **object** parameters). [**Frame.GoBack**](https://msdn.microsoft.com/library/windows/apps/Dn996568) is overloaded, though, so we can't use that method with this technique.

The event binding technique is similar to implementing and consuming commands (a command is a property that returns an object that implements the [**ICommand**](T:System.Windows.Input.ICommand) interface). Both [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) and [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) work with commands. So that you don't have to implement the command pattern multiple times, you can use the **DelegateCommand** helper class that you'll find in the [QuizGame](https://github.com/Microsoft/Windows-appsample-quizgame) sample (in the "Common" folder).

## Binding to a collection of folders or files

You can use the APIs in the [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/BR227346) namespace to retrieve folder and file data. However, the various **GetFilesAsync**, **GetFoldersAsync**, and **GetItemsAsync** methods do not return values that are suitable for binding to list controls. Instead, you must bind to the return values of the [**GetVirtualizedFilesVector**](https://msdn.microsoft.com/library/windows/apps/Hh701422), [**GetVirtualizedFoldersVector**](https://msdn.microsoft.com/library/windows/apps/Hh701428), and [**GetVirtualizedItemsVector**](https://msdn.microsoft.com/library/windows/apps/Hh701430) methods of the [**FileInformationFactory**](https://msdn.microsoft.com/library/windows/apps/BR207501) class. The following code example from the [StorageDataSource and GetVirtualizedFilesVector sample](http://go.microsoft.com/fwlink/p/?linkid=228621) shows the typical usage pattern. Remember to declare the **picturesLibrary** capability in your app package manifest, and confirm that there are pictures in your Pictures library folder.

``` csharp
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
``` 通常，需要使用此方法来创建文件和文件夹信息的只读视图。 可以创建到文件和文件夹属性的双向绑定，例如，让用户在音乐视图中对歌曲进行评级。 然而，任何更改都不会永久保留，除非你调用相应的 **SavePropertiesAsync** 方法（例如，[**MusicProperties.SavePropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/BR207760)）。 当项目失去焦点时，你应该提交更改，因为这样可以触发选择重置。

注意，使用此技术的双向绑定仅适用于索引位置（例如“音乐”）。 你可以通过调用 [**FolderInformation.GetIndexedStateAsync**](https://msdn.microsoft.com/library/windows/apps/BR207627) 方法来确定是否索引某个位置。

还请注意，某个虚拟化矢量可能会在填充其值之前为某些项目返回 **null**。 例如，应该首先检查 **null**，然后才能使用绑定到虚拟化矢量的列表控件的 [**SelectedItem**](https://msdn.microsoft.com/library/windows/apps/BR209770) 值，或者改为使用 [**SelectedIndex**](https://msdn.microsoft.com/library/windows/apps/BR209768)。

绑定到按键分组的数据 -------------------------------- 如果获取项的简单集合（例如，由 **BookSku** 类表示的书籍）并通过将某一常见属性（例如，**BookSku.AuthorName** 属性）用作键来对项进行分组，则将该结果称为分组数据。 当对数据进行分组时，它将不再是简单集合。 分组数据是组对象的集合，其中每个组对象都具有 a) 一个键和 b) 一个其属性与该键匹配的项集合。 再以书籍为例，按作者名称对书籍进行分组的结果即为作者名称组的集合，其中每组都具有 a) 一个键（这是作者名称）和 b) 一个其 **AuthorName** 属性与组键匹配的 **BookSku** 集合。

通常情况下，若要显示集合，可将项目控件（例如 [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) 或 [**GridView**](https://msdn.microsoft.com/library/windows/apps/BR242705)）的 [**ItemsSource**](https://msdn.microsoft.com/library/windows/apps/BR242828) 直接绑定到返回集合的属性。 如果这是项的简单集合，则无需执行任何特殊操作。 但是，如果它是组对象的集合（像在绑定到分组数据时一样），则需要使用一个名为 [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833) 的中间对象，该对象位于项目控件和绑定源之间。 先将 **CollectionViewSource** 绑定到返回分组数据的属性，然后将项目控件绑定到 **CollectionViewSource**。 **CollectionViewSource** 的额外增值功能可用于跟踪当前项，以便你可以通过将多个项目控件全都绑定到同一 **CollectionViewSource** 来使其保持同步。 也可以通过 [**CollectionViewSource.View**](https://msdn.microsoft.com/library/windows/apps/BR209833-view) 属性返回的对象的 [**ICollectionView.CurrentItem**](https://msdn.microsoft.com/library/windows/apps/BR209857) 属性，以编程方式访问当前项。

若要激活 [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833) 的分组功能，请将 [**IsSourceGrouped**](https://msdn.microsoft.com/library/windows/apps/BR209833-issourcegrouped) 设置为 **true**。 是否还需要设置 [**ItemsPath**](https://msdn.microsoft.com/library/windows/apps/BR209833-itemspath) 属性完全取决于你的组对象的创作方式。 组对象的创作方式有以下两种：“属于组”模式和“包含组”模式。 在“属于组”模式中，组对象派生自集合类型（例如，**List<T>**），因此事实上组对象本身就是项目组。 使用此模式，无需设置 **ItemsPath**。 在“包含组”模式中，组对象具有一个或多个集合类型属性（例如 **List<T>**），因此“包含组”的单个项目组采用单个属性的形式（而多个项目组则采用多个属性的形式）。 使用此模式，需要将 **ItemsPath** 设置为包含项目组的属性名。

下面的示例阐述了“包含组”模式。 页面类有一个名为 [**ViewModel**](https://msdn.microsoft.com/library/windows/apps/BR208713) 的属性，该属性将返回我们的视图模型的实例。 [
            **CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833) 绑定到视图模型的 **Authors** 属性（**Authors** 是组对象的集合），还指定了它是包含分组项目的 **Author.BookSkus** 属性。 最后，[**GridView**](https://msdn.microsoft.com/library/windows/apps/BR242705) 绑定到 **CollectionViewSource** 且具有其已定义的组样式，这样它便可以采用分组形式呈现项目。

``` csharp
    <Page.Resources>
        <CollectionViewSource
        x:Name="AuthorHasACollectionOfBookSku"
        Source="{x:Bind ViewModel.Authors}"
        IsSourceGrouped="true"
        ItemsPath="BookSkus"/>
    </Page.Resources>
    ...

    <GridView
    ItemsSource="{Binding Source={StaticResource AuthorHasACollectionOfBookSku}}" ...>
        <GridView.GroupStyle>
            <GroupStyle
                HeaderTemplate="{StaticResource AuthorGroupHeaderTemplateWide}" ... />
        </GridView.GroupStyle>
    </GridView>
```

Note that the [**ItemsSource**](https://msdn.microsoft.com/library/windows/apps/BR242828) must use [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) (and not [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783)) because it needs to set the **Source** property to a resource. To see the above example in the context of the complete app, download the [Bookstore2](http://go.microsoft.com/fwlink/?linkid=532952) sample app. Unlike the markup shown above, [Bookstore2](http://go.microsoft.com/fwlink/?linkid=532952) uses {Binding} exclusively.

You can implement the "is-a-group" pattern in one of two ways. One way is to author your own group class. Derive the class from **List&lt;T&gt;** (where *T* is the type of the items). For example, `public class Author : List<BookSku>`. 另一种方式是，使用 [LINQ](http://msdn.microsoft.com/library/bb397926.aspx) 表达式，从诸如 **BookSku** 项目的属性值等动态创建组对象（以及组类）。 此方法（仅维护项目的简单列表并将其动态分组在一起）是从云服务访问数据的应用的典型用法。 可以灵活地按作者或流派对书籍进行分组，而无需特殊组类，如 **Author** 和 **Genre**。

下面的示例使用 [LINQ](http://msdn.microsoft.com/library/bb397926.aspx) 阐述了“属于组”模式。 在本示例中我们按流派对书籍进行分组，并在组标题中显示流派名。 这由引用组 [**Key**](P:System.Linq.IGrouping%602.Key) 的值的“Key”属性路径进行指示。

``` csharp
    using System.Linq;

    ...

    private IOrderedEnumerable<IGrouping<string, BookSku>> genres; public IOrderedEnumerable<IGrouping<string, BookSku>> Genres
    {
        get
        {
            if (this.genres == null)
            {
                this.genres = from book in this.bookSkus
                group book by book.genre into grp
                orderby grp.Key select grp;
            }
            return this.genres;
        }
    } ```

Remember that when using [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) with data templates we need to indicate the type being bound to by setting an **x:DataType** value. If the type is generic then we can't express that in markup so we need to use [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) instead in the group style header template.

``` xml
    <Grid.Resources>
        <CollectionViewSource x:Name="GenreIsACollectionOfBookSku"
        Source="{Binding Genres}"
        IsSourceGrouped="true"/>
    </Grid.Resources>
    <GridView ItemsSource="{Binding Source={StaticResource GenreIsACollectionOfBookSku}}">
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

A [**SemanticZoom**](https://msdn.microsoft.com/library/windows/apps/Hh702601) control is a great way for your users to view and navigate grouped data. The [Bookstore2](http://go.microsoft.com/fwlink/?linkid=532952) sample app illustrates how to use the **SemanticZoom**. In that app, you can view a list of books grouped by author (the zoomed-in view) or you can zoom out to see a jump list of authors (the zoomed-out view). The jump list affords much quicker navigation than scrolling through the list of books. The zoomed-in and zoomed-out views are actually **ListView** or **GridView** controls bound to the same **CollectionViewSource**.

![An illustration of a SemanticZoom](images/sezo.png)

When you bind to hierarchical data—such as subcategories within categories—you can choose to display the hierarchical levels in your UI with a series of items controls. A selection in one items control determines the contents of subsequent items controls. You can keep the lists synchronized by binding each list to its own [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833) and binding the **CollectionViewSource** instances together in a chain. This is called a master/details (or list/details) view. For more info, see [How to bind to hierarchical data and create a master/details view](how-to-bind-to-hierarchical-data-and-create-a-master-details-view.md).

Diagnosing and debugging data binding problems
-----------------------------------------------

Your binding markup contains the names of properties (and, for C#, sometimes fields and methods). So when you rename a property, you'll also need to change any binding that references it. Forgetting to do that leads to a typical example of a data binding bug, and your app either won't compile or won't run correctly.

The binding objects created by [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) and [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) are largely functionally equivalent. But {x:Bind} has type information for the binding source, and it generates source code at compile-time. With {x:Bind} you get the same kind of problem detection that you get with the rest of your code. That includes compile-time validation of your binding expressions, and debugging by setting breakpoints in the source code generated as the partial class for your page. These classes can be found in the files in your `obj` folder, with names like (for C#) `<view name>.g.cs`)。 如果你有绑定方面的问题，请打开 Microsoft Visual Studio 调试器中的**出现未处理的异常时中断**。 该调试器将在该点处中断执行，以便你可以调试哪里出现了问题。 对于绑定源节点图形的每个部分，{x:Bind} 生成的代码均遵循相同的模式，你可以使用**“调用堆栈”**窗口中的信息来帮助确定导致该问题出现的调用顺序。

[
            {Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) 不具有绑定源的类型信息。 但在通过附加的调试器运行你的应用时，Visual Studio 中的**“输出”**窗口中将显示所有绑定错误。

使用代码创建绑定 -------------------------

**注意** 由于不能使用代码创建 [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) 绑定，因此本部分仅适用于 [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782)。 不过，使用 [**DependencyProperty.RegisterPropertyChangedCallback**](https://msdn.microsoft.com/library/windows/apps/BR242356-registerpropertychangedcallback) 同样可以获得 {x:Bind} 的某些相同优势，这使你可以在任何依赖属性上注册更改通知。

还可以使用规程代码而不是 XAML 来将 UI 元素连接到数据。 为此，请创建一个新 [**Binding**](https://msdn.microsoft.com/library/windows/apps/BR209820) 对象，设置相应的属性，然后调用 [**FrameworkElement.SetBinding**](https://msdn.microsoft.com/library/windows/apps/BR208706-setbinding) 或 [**BindingOperations.SetBinding**](https://msdn.microsoft.com/library/windows/apps/BR209820operations-setbinding)。 如果你希望在运行时选择绑定属性值或在多个控件中共享单个绑定，则以编程方式创建绑定将十分有用。 但是请注意，调用 **SetBinding** 后，无法更改绑定属性值。

以下示例演示了如何使用代码实现绑定。

``` xml
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

``` vbnet
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

{x:Bind} 和 {Binding} 功能比较
------------------------------------------

| 功能 | {x:Bind} | {Binding} | 注释 |
|---------|----------|-----------|-------|
| Path 为默认属性 | `{x:Bind a.b.c}` | `{Binding a.b.c}` | | 
| Path 属性 | `{x:Bind Path=a.b.c}` | `{Binding Path=a.b.c}` | 在 x:Bind 中，Path 默认位于 Page 的根处，而非 DataContext。 | | 索引器 | `{x:Bind Groups[2].Title}` | `{Binding Groups[2].Title}` | 绑定到集合中的指定项。 仅支持基于整数的索引。 | | 附加属性 | `{x:Bind Button22.(Grid.Row)}` | `{Binding Button22.(Grid.Row)}` | 附加属性用括号进行指定。 如果未在 XAML 命名空间中声明该属性，则在其前面加上 xml 命名空间，这应该映射到文档的标头处的代码命名空间中。 | | 强制转换 | `{x:Bind groups[0].(data:SampleDataGroup.Title)}` | 不需要< | 强制转换用括号进行指定。 如果未在 XAML 命名空间中声明该属性，则在其前面加上 xml 命名空间，这应该映射到文档的标头处的代码命名空间中。 | 
| 转换器 | `{x:Bind IsShown, Converter={StaticResource BoolToVisibility}}` | `{Binding IsShown, Converter={StaticResource BoolToVisibility}}` | 转换器必须在 Page/ResourceDictionary 的根目录处或在 App.xaml 中进行声明。 | 
| ConverterParameter, ConverterLanguage | `{x:Bind IsShown, Converter={StaticResource BoolToVisibility}, ConverterParameter=One, ConverterLanguage=fr-fr}` | `{Binding IsShown, Converter={StaticResource BoolToVisibility}, ConverterParameter=One, ConverterLanguage=fr-fr}` | 转换器必须在 Page/ResourceDictionary 的根目录处或在 App.xaml 中进行声明。 | 
| TargetNullValue | `{x:Bind Name, TargetNullValue=0}` | `{Binding Name, TargetNullValue=0}` | 在绑定表达式的叶为 null 时使用。 对于字符串值，应使用单引号。 | 
| FallbackValue | `{x:Bind Name, FallbackValue='empty'}` | `{Binding Name, FallbackValue='empty'}` | 在绑定的路径的任何部分（叶除外）为 null 时使用。 | 
| ElementName | `{x:Bind slider1.Value}` | `{Binding Value, ElementName=slider1}` | 使用 {x:Bind}，绑定到某一字段；Path 默认位于 Page 的根处，以便任意命名的元素均可通过其字段进行访问。 | 
| RelativeSource: Self | `<Rectangle x:Name="rect1" Width="200" Height="{x:Bind rect1.Width}" ... />` | `<Rectangle Width="200" Height="{Binding Width, RelativeSource={RelativeSource Self}}" ... />` | With {x:Bind}, name the element and use its name in Path. | 
| RelativeSource: TemplatedParent | Not supported | `{Binding <path>, RelativeSource={RelativeSource TemplatedParent}}` | 在大多数情况下，常规模板绑定可在控件模板中使用。 但在使用 TemplatedParent 时，需要使用转换器或双向绑定。< | 
| 绑定源 | 不支持 | `<ListView ItemsSource="{Binding Orders, Source={StaticResource MyData}}"/>` | For {x:Bind} use a property or a static path instead. | 
| Mode | `{x:Bind Name, Mode=OneWay}` | `{Binding Name, Mode=TwoWay}` | Mode can be OneTime, OneWay, or TwoWay. {x:Bind} defaults to OneTime; {Binding} defaults to OneWay. | 
| UpdateSourceTrigger | Not supported | `<Binding UpdateSourceTrigger="[Default | PropertyChanged | Explicit]"/>` | {x:Bind} 可在除 TextBox.Text 等待失去焦点来更新绑定源之外的所有情况下使用 PropertyChanged 行为。 | 




<!--HONumber=Mar16_HO1-->


