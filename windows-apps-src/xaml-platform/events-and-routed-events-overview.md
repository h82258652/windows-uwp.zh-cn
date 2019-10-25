---
description: 当使用、Visual Basic 或可视化C# C++组件扩展（C++/cx）作为编程语言，并在 UI 定义中使用 XAML 时，我们介绍了 Windows 运行时应用中事件的编程概念。
title: 事件和路由事件概述
ms.assetid: 34C219E8-3EFB-45BC-8BBD-6FD937698832
ms.date: 07/12/2018
ms.topic: article
keywords: windows 10，uwp
ms.localizationpriority: medium
ms.openlocfilehash: 759e47348198feedbf7b1e3ee2c0bfc2da1da671
ms.sourcegitcommit: f561efbda5c1d47b85601d91d70d86c5332bbf8c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/21/2019
ms.locfileid: "72690391"
---
# <a name="events-and-routed-events-overview"></a>事件和路由事件概述

**重要 Api**
- [**System.windows.uielement>** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement)
- [**System.windows.routedeventargs.handled**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.RoutedEventArgs)

当使用、Visual Basic 或可视化C# C++组件扩展（C++/cx）作为编程语言，并在 UI 定义中使用 XAML 时，我们介绍了 Windows 运行时应用中事件的编程概念。 您可以为 XAML 中的 UI 元素声明指定事件处理程序，也可以在代码中添加处理程序。 Windows 运行时支持*路由事件*：某些输入事件和数据事件可由触发事件的对象之外的对象处理。 在定义控件模板或使用页面或布局容器时，路由事件很有用。

## <a name="events-as-a-programming-concept"></a>作为编程概念的事件

一般而言，在对 Windows 运行时应用编程时，事件概念与最常用编程语言中的事件模型类似。 如果你知道如何处理 Microsoft .NET 或C++事件，就可以开始使用。 但您不需要了解有关事件模型的概念，以便执行一些基本任务，例如附加处理程序。

当使用C#、Visual Basic 或C++/cx 作为编程语言时，UI 在标记（XAML）中定义。 在 XAML 标记语法中，在标记元素和运行时代码实体之间连接事件的一些原则与其他 Web 技术（如 ASP.NET 或 HTML5）类似。

**请注意**  为 XAML 定义的 UI 提供运行时逻辑的代码通常称为*代码隐藏*文件或代码隐藏文件。 在 Microsoft Visual Studio 解决方案视图中，此关系以图形方式显示，代码隐藏文件是依赖的和嵌套的文件，而不是它所引用的 XAML 页面。

## <a name="buttonclick-an-introduction-to-events-and-xaml"></a>按钮。单击：事件和 XAML 简介

Windows 运行时应用最常见的编程任务之一是捕获 UI 的用户输入。 例如，UI 可能有一个按钮，用户必须单击该按钮才能提交信息或更改状态。

通过生成 XAML 为 Windows 运行时应用定义 UI。 此 XAML 通常是 Visual Studio 的设计图面中的输出。 你还可以在纯文本编辑器或第三方 XAML 编辑器中编写 XAML。 生成该 XAML 时，可以将事件处理程序用于各个 UI 元素，同时定义所有其他 XAML 属性，这些属性将建立该 UI 元素的属性值。

若要在 XAML 中传输事件，可指定已定义的处理程序方法的字符串格式名称，或稍后将在代码隐藏中定义的处理程序方法。 例如，此 XAML 定义了一个带有其他属性（[x：Name 特性](x-name-attribute.md)， [**Content**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.contentcontrol.content)）的[**Button**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button)对象，并通过引用名为 `ShowUpdatesButton_Click`的方法，将该按钮的[**Click**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click)事件的处理程序线路：

```xaml
<Button x:Name="showUpdatesButton"
  Content="{Binding ShowUpdatesText}"
  Click="ShowUpdatesButton_Click"/>
```

**Tip**  *事件布线*是一项编程术语。 它指的是指示事件发生的位置应调用命名处理程序方法的过程或代码。 在大多数过程代码模型中，事件布线是隐式或显式的 "AddHandler" 代码，可对事件和方法进行命名，并且通常涉及目标对象实例。 在 XAML 中，"AddHandler" 是隐式的，事件布线完全由将事件命名为对象元素的属性名称，并将处理程序命名为该属性的值。

用编程语言编写实际的处理程序，该处理程序使用的是应用程序的所有代码和代码隐藏。 使用属性 `Click="ShowUpdatesButton_Click"`，您已创建了一个协定，当 XAML 进行标记编译和分析时，您 IDE 的生成操作中的 XAML 标记编译步骤和最终 XAML 分析都可以在应用程序加载时找到名为 `ShowUpdatesButton_Click` 的方法，作为应用程序的一部分取消. `ShowUpdatesButton_Click` 必须是一个方法，该方法可为[**Click**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click)事件的任何处理程序实现兼容方法签名（基于委托）。 例如，此代码定义 `ShowUpdatesButton_Click` 处理程序。

```csharp
private void ShowUpdatesButton_Click (object sender, RoutedEventArgs e) 
{
    Button b = sender as Button;
    //more logic to do here...
}
```

```vb
Private Sub ShowUpdatesButton_Click(ByVal sender As Object, ByVal e As RoutedEventArgs)
    Dim b As Button = CType(sender, Button)
    '  more logic to do here...
End Sub
```

```cppwinrt
void winrt::MyNamespace::implementation::BlankPage::ShowUpdatesButton_Click(Windows::Foundation::IInspectable const& sender, Windows::UI::Xaml::RoutedEventArgs const& e)
{
    auto b{ sender.as<Windows::UI::Xaml::Controls::Button>() };
    // More logic to do here.
}
```

```cpp
void MyNamespace::BlankPage::ShowUpdatesButton_Click(Platform::Object^ sender, Windows::UI::Xaml::RoutedEventArgs^ e) 
{
    Button^ b = (Button^) sender;
    //more logic to do here...
}
```

在此示例中，`ShowUpdatesButton_Click` 方法基于[**RoutedEventHandler**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.routedeventhandler)委托。 你会知道，这是要使用的委托，因为你将在[**Click**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click)方法的语法中看到名为的委托。

**Tip**  Visual Studio 提供了一种简便的方法来命名事件处理程序，并在编辑 XAML 时定义处理程序方法。 在 XAML 文本编辑器中提供事件的属性名称时，请稍等片刻，直到 Microsoft IntelliSense 列表显示。 如果单击列表中的 " **&lt;新建事件处理程序"&gt;** ，则 Microsoft Visual Studio 会根据元素的**x：Name** （或类型名称）、事件名称和数字后缀建议方法名称。 然后，您可以右键单击所选事件处理程序的名称，然后单击 "**导航到事件处理程序**"。 这会直接导航到新插入的事件处理程序定义，如 XAML 页面的代码隐藏文件的 "代码编辑器" 视图中所示。 事件处理程序已具有正确的签名，其中包括*发送方*参数和事件使用的事件数据类。 此外，如果代码隐藏中已经存在具有正确签名的处理程序方法，则该方法的名称将显示在 "自动完成" 下拉下，并将 **&lt;New Event handler&gt;** "选项。 还可以按 Tab 键作为快捷方式，而不是单击 IntelliSense 列表项。

## <a name="defining-an-event-handler"></a>定义事件处理程序

对于作为 XAML 中的 UI 元素和声明的对象，事件处理程序代码在作为 XAML 页的代码隐藏的分部类中定义。 事件处理程序是作为与 XAML 关联的分部类的一部分编写的方法。 这些事件处理程序基于特定事件所使用的委托。 你的事件处理程序方法可以是公共或私有的。 私有访问的工作原理是因为 XAML 创建的处理程序和实例最终通过代码生成进行联接。 通常，我们建议您在类中将事件处理程序方法设为私有方法。

**请注意**  事件处理C++程序不会在分部类中定义，它们在标头中声明为私有类成员。 C++项目的生成操作负责生成支持 XAML 类型系统和的C++代码隐藏模型的代码。

### <a name="the-sender-parameter-and-event-data"></a>*发件人*参数和事件数据

为事件编写的处理程序可以访问两个值，这些值可用作调用处理程序的每种情况下的输入。 第一个这样的值是*sender*，后者是对附加处理程序的对象的引用。 *发件人*参数类型化为基**对象**类型。 常见的一种方法是将*发送方*转换为更精确的类型。 如果希望检查或更改*发送方*对象本身的状态，此方法非常有用。 根据您自己的应用程序设计，您通常知道将*发送*程序强制转换到的类型，基于处理程序的附加位置或其他设计细节。

第二个值是事件数据，通常在语法定义中显示为*e*参数。 通过查看为要处理的特定事件分配的委托的*e*参数，然后在 Visual Studio 中使用 IntelliSense 或对象浏览器，可以发现事件数据的哪些属性可用。 或者，您可以使用 Windows 运行时参考文档。

对于某些事件，事件数据的特定属性值与知道事件发生的情况非常重要。 这对于输入事件尤其如此。 对于指针事件，事件发生时指针的位置可能很重要。 对于键盘事件，所有可能的按键都将激发一个[**KeyDown**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keydown)和[**KeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keyup)事件。 若要确定用户按下的键，你必须访问可用于事件处理程序的[**KeyRoutedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.KeyRoutedEventArgs) 。 有关处理输入事件的详细信息，请参阅[键盘交互](https://docs.microsoft.com/windows/uwp/input-and-devices/keyboard-interactions)和[处理指针输入](https://docs.microsoft.com/windows/uwp/input-and-devices/handle-pointer-input)。 输入事件和输入方案通常具有本主题未涵盖的其他注意事项，如指针事件的指针捕获，以及键盘事件的修改键和平台键代码。

### <a name="event-handlers-that-use-the-async-pattern"></a>使用**异步**模式的事件处理程序

在某些情况下，需要使用事件处理程序中使用**异步**模式的 api。 例如，你可以使用[**AppBar**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBar)中的[**按钮**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button)来显示文件选取器并与其进行交互。 但是，许多文件选取器 Api 都是异步的。 它们必须在**异步**/awaitable 范围内调用，编译器将强制执行此设置。 因此，您可以执行的操作是向事件处理程序添加**async**关键字，以便处理程序现在为**async** **void**。 现在，你的事件处理程序允许进行**异步**/awaitable 调用。

有关使用**异步**模式的用户交互事件处理的示例，请参阅[文件访问和选取](https://docs.microsoft.com/previous-versions/windows/apps/jj655411(v=win.10))器（[使用C#或 Visual Basic 序列创建第一个 Windows 运行时应用](https://docs.microsoft.com/previous-versions/windows/apps/hh974581(v=win.10))的部分）。 另请参阅 [在 C 中调用异步 Api]。

## <a name="adding-event-handlers-in-code"></a>在代码中添加事件处理程序

XAML 不是向对象分配事件处理程序的唯一方法。 若要将事件处理程序添加到代码中的任何给定对象（包括无法在 XAML 中使用的对象），可以使用特定于语言的语法添加事件处理程序。

在C#中，语法是使用`+=`运算符。 通过在运算符右侧引用事件处理程序方法名称来注册该处理程序。

如果使用代码将事件处理程序添加到运行时 UI 中显示的对象，则常见的做法是添加此类处理程序以响应对象生存期事件或回调（如已[**加载**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.loaded)或[**OnApplyTemplate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.onapplytemplate)），以便事件处理程序相关对象在运行时已为用户启动的事件做好准备。 此示例演示了页面结构的 XAML 大纲，并提供了用于C#向对象添加事件处理程序的语言语法。

```xaml
<Grid x:Name="LayoutRoot" Loaded="LayoutRoot_Loaded">
  <StackPanel>
    <TextBlock Name="textBlock1">Put the pointer over this text</TextBlock>
...
  </StackPanel>
</Grid>
```

```csharp
void LayoutRoot_Loaded(object sender, RoutedEventArgs e)
{
    textBlock1.PointerEntered += textBlock1_PointerEntered;
    textBlock1.PointerExited += textBlock1_PointerExited;
}
```

**请注意**  存在更详细的语法。 在2005中C# ，添加了一个称为委托推理的功能，该功能可让编译器推断新的委托实例，并启用先前的更简单的语法。 详细语法在功能上与前面的示例相同，但在注册之前显式创建了一个新的委托实例，因此不会利用委托推理。 此显式语法不太常见，但你可能仍会在一些代码示例中看到它。

```csharp
void LayoutRoot_Loaded(object sender, RoutedEventArgs e)
{
    textBlock1.PointerEntered += new PointerEventHandler(textBlock1_PointerEntered);
    textBlock1.PointerExited += new MouseEventHandler(textBlock1_PointerExited);
}
```

Visual Basic 语法有两种可能。 一种是并行处理C#语法并直接将处理程序附加到实例。 这需要**AddHandler**关键字，还需要取消引用处理程序方法名称的**AddressOf**运算符。

Visual Basic 语法的另一种方法是对事件处理程序使用**Handles**关键字。 此方法适用于以下情况：在加载时，处理程序应在对象上存在，并且在对象的整个生存期内保持不变。 在 XAML 中定义的对象上使用**句柄**要求提供**名称** / **x：Name**。 此名称将成为**Handles**语法的*事件*部分所需的实例限定符。 在这种情况下，不需要基于对象生存期的事件处理程序来启动附加其他事件处理程序;在编译 XAML 页时，会创建**句柄**连接。

```vb
Private Sub textBlock1_PointerEntered(ByVal sender As Object, ByVal e As PointerRoutedEventArgs) Handles textBlock1.PointerEntered
' ...
End Sub
```

**请注意**  Visual STUDIO 及其 XAML 设计图面通常提升实例处理技术，而不是**Handles**关键字。 这是因为在 XAML 中建立事件处理程序配线是典型设计器开发人员工作流的一部分，而**Handles**关键字技术与 XAML 中的事件处理程序布线不兼容。

在C++/cx 中，还可以使用 **+=** 语法，但与基本C#形式不同：

- 不存在委托推理，因此必须对委托实例使用**ref new** 。
- 委托构造函数具有两个参数，并且需要目标对象作为第一个参数。 通常，你可以指定**此**。
- 委托构造函数要求方法地址作为第二个参数，因此 **&** 引用运算符在方法名称之前。

```cppwinrt
textBlock1().PointerEntered({this, &MainPage::TextBlock1_PointerEntered });
```

```cpp
textBlock1->PointerEntered += 
ref new PointerEventHandler(this, &BlankPage::textBlock1_PointerEntered);
```

### <a name="removing-event-handlers-in-code"></a>在代码中删除事件处理程序

通常不需要在代码中删除事件处理程序，即使您在代码中添加了事件处理程序也是如此。 大多数 Windows 运行时对象（如页面和控件）的对象生存期行为会在对象断开与主[**窗口**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window)及其可视化树的连接时销毁对象，并且还会销毁任何委托引用。 .NET 在默认情况下，通过垃圾回收C++和 Windows 运行时，并使用/cx 来使用弱引用。

在某些极少数情况下，您需要显式删除事件处理程序。 其中包括：

- 为静态事件添加的处理程序，不能以传统方式进行垃圾回收。 Windows 运行时 API 中静态事件的示例是[**CompositionTarget**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.CompositionTarget)和[**剪贴板**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.Clipboard)类的事件。
- 测试代码：希望立即删除处理程序的时间，或在运行时为事件交换旧的或新的事件处理程序的代码。
- 自定义**remove**访问器的实现。
- 自定义静态事件。
- 页面导航的处理程序。

[**NavigatedFrom**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.onnavigatedfrom)是可能的事件触发器，这些触发器在状态管理和对象生存期中具有适当的位置，以便可以使用它们删除其他事件的处理[**程序。** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.unloaded)

例如，你可以使用此代码从目标对象**textBlock1**中删除名为**textBlock1\_PointerEntered**的事件处理程序。

```csharp
textBlock1.PointerEntered -= textBlock1_PointerEntered;
```

```vb
RemoveHandler textBlock1.PointerEntered, AddressOf textBlock1_PointerEntered
```

如果事件是通过 XAML 属性添加的，则还可以删除处理程序，这意味着处理程序是在生成的代码中添加的。 如果为附加了处理程序的元素提供了**名称**值，则此操作会更容易，因为这会为后面的代码提供对象引用;但是，您还可以遍历对象树，以便在对象没有**名称**的情况下查找必要的对象引用。

如果需要在/Cx 中C++删除事件处理程序，则需要一个注册令牌，应从`+=`事件处理程序注册的返回值接收到该令牌。 这是因为在C++/cx 语法中使用的 `-=` 右侧的值是标记，而不是方法名称。 对于C++/cx，不能删除作为 XAML 特性添加的处理程序，因为/cx C++生成的代码不会保存令牌。

## <a name="routed-events"></a>路由事件

使用C#的 Windows 运行时，Microsoft Visual Basic 或C++/cx 支持路由事件的概念，该概念适用于大多数 UI 元素上存在的一组事件。 这些事件适用于输入和用户交互方案，它们是在[**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement)基类上实现的。 下面是作为路由事件的输入事件列表：

- [**BringIntoViewRequested**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.bringintoviewrequested)
- [**CharacterReceived**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.characterreceived)
- [**ContextCanceled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.contextcanceled)
- [**ContextRequested**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.contextrequested)
- [**DoubleTapped**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.doubletapped)
- [**System.windows.dragdrop.dragenter>** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.dragenter)
- [**System.windows.dragdrop.dragleave>** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.dragleave)
- [**拖动**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.dragover)
- [**DragStarting**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.dragstarting)
- [**击落**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.drop)
- [**DropCompleted**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.dropcompleted)
- [**GettingFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.gettingfocus)
- [**GotFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.gotfocus)
- [**容器**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.holding)
- [**KeyDown**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keydown)
- [**KeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keyup)
- [**LosingFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.losingfocus)
- [**LostFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.lostfocus)
- [**System.windows.uielement.manipulationcompleted>** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.manipulationcompleted)
- [**ManipulationDelta**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.manipulationdelta)
- [**System.windows.uielement.manipulationinertiastarting>** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.manipulationinertiastarting)
- [**System.windows.uielement.manipulationstarted>** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.manipulationstarted)
- [**System.windows.uielement.manipulationstarting>** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.manipulationstarting)
- [**NoFocusCandidateFound**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.nofocuscandidatefoundeventargs)
- [**PointerCanceled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointercanceled)
- [**PointerCaptureLost**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointercapturelost)
- [**PointerEntered**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerentered)
- [**PointerExited**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerexited)
- [**PointerMoved**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointermoved)
- [**PointerPressed**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerpressed)
- [**PointerReleased**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerreleased)
- [**PointerWheelChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerwheelchanged)
- [**System.windows.forms.control.previewkeydown>** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.previewkeydown.md)
- [**PreviewKeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.previewkeyup.md)
- [**PointerWheelChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerwheelchanged)
- [**RightTapped**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.righttapped)
- [**分**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.tapped)

路由事件是可能传递（*路由*）从子对象传递到对象树中的每个后续父对象的事件。 UI 的 XAML 结构接近此树，而该树的根为 XAML 中的根元素。 True 对象树可能与 XAML 元素嵌套略有不同，因为对象树不包含 XAML 语言功能，如属性元素标记。 可以从引发事件的任何 XAML 对象元素子元素（指向包含它的父对象元素）以*冒泡*方式构思路由事件。 可以在事件路由中处理多个对象的事件及其事件数据。 如果没有任何元素具有处理程序，则在到达根元素之前，路由可能一直持续。

如果你知道动态 HTML （DHTML）或 HTML5 等 Web 技术，则可能已熟悉*冒泡*事件概念。

当路由事件通过其事件路由冒泡时，任何附加的事件处理程序都将访问事件数据的共享实例。 因此，如果某个处理程序可以写入任何事件数据，则对事件数据所做的任何更改都将传递到下一个处理程序，并且可能不再表示事件中的原始事件数据。 当某个事件具有路由事件行为时，参考文档将包含有关路由行为的注释或其他表示法。

### <a name="the-originalsource-property-of-routedeventargs"></a>**System.windows.routedeventargs.handled**的**OriginalSource**属性

当某个事件冒泡事件路由时，*发送方*不再是事件引发对象的对象。 相反，*发送方*是附加正在调用的处理程序的对象。

在某些情况下，*发件人*并不有意思，你会对信息进行兴趣，例如，当指针事件被激发时指针位于哪一种可能的子对象上，或者当用户按下键盘键时，较大 UI 中的哪个对象处于焦点状态。 对于这些情况，可以使用[**OriginalSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.routedeventargs.originalsource)属性的值。 在路由上的所有点， **OriginalSource**将报告触发事件的原始对象，而不是附加处理程序的对象。 但对于[**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement)输入事件，该原始对象通常是一个对象，该对象在页面级 UI 定义 XAML 中不会立即可见。 相反，原始源对象可能是控件的模板化部分。 例如，如果用户将指针悬停在[**按钮**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button)的边缘上，则对于大多数指针事件， **OriginalSource**是[**模板**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.template)中的一个[**边框**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Border)模板部件，而不是**按钮**本身。

**提示**  输入事件冒泡在创建模板化控件时特别有用。 具有模板的任何控件都可以有一个由其使用者应用的新模板。 尝试重新创建工作模板的使用者可能会无意中消除在默认模板中声明的某些事件处理。 你仍可以通过在类定义中将处理程序附加为[**OnApplyTemplate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.onapplytemplate)重写的一部分，来提供控件级事件处理。 然后，可以捕获在实例化时向上冒泡到控件的根的输入事件。

### <a name="the-handled-property"></a>**处理**的属性

特定路由事件的多个事件数据类包含名为 "已**处理**" 的属性。 有关示例，请参阅[**PointerRoutedEventArgs**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.pointerroutedeventargs.handled)、 [**KeyRoutedEventArgs**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyroutedeventargs.handled)、 [**system.windows.drageventargs.effects**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.drageventargs.handled)。 在所有情况下，已**处理**都是可设置的布尔值属性。

将已**处理**的属性设置为**true**会影响事件系统的行为。 当**处理**为**true**时，路由将停止大多数事件处理程序;事件不会继续沿路由通知其他特定事件情况的附加处理程序。 "已处理" 指的是在事件上下文中，应用如何响应。 基本上，**处理**是一种简单的协议，该协议使应用程序代码能够确保发生事件时不需要将事件冒泡到任何容器，你的应用逻辑已处理了所需的操作。 相反，您一定要小心不要处理可能会冒泡的事件，以便内置系统或控件行为能够发挥作用。例如，处理选择控件的部分或项中的低级别事件可能会造成不利后果。 选择控件可能正在查找输入事件，以了解所选内容应该更改。

并非所有路由事件都可以通过这种方式取消路由，你可以指出，因为它们没有已**处理**的属性。 例如， [**GotFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.gotfocus)和[**LostFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.lostfocus)确实是冒泡的，但是它们始终都冒泡到根，而它们的事件数据类没有可影响该行为的已**处理**属性。

##  <a name="input-event-handlers-in-controls"></a>控件中的输入事件处理程序

特定 Windows 运行时控件有时会将**处理**的概念用于内部输入事件。 这可以使其看起来像是输入事件永远不会发生，因为用户代码无法对其进行处理。 例如， [**Button**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button)类包含特意处理一般输入事件[**PointerPressed**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerpressed)的逻辑。 这样做的原因是，按钮触发按下了指针按下的输入启动的[**Click**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click)事件，并按其他输入模式（如处理键，如 Enter 键，可在焦点上调用按钮）发出。 对于 "类设计"**按钮**，将对原始输入事件进行概念处理，而类使用者（例如用户代码）可以与控件相关的**Click**事件交互。 Windows 运行时 API 参考中的特定控制类的主题通常说明类实现的事件处理行为。 在某些情况下，可以**通过重写**_事件_方法来更改行为。 例如，可以通过重写[**控件 OnKeyDown**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.onkeydown)来更改[**TextBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox)派生类对键输入做出反应的方式。

##  <a name="registering-handlers-for-already-handled-routed-events"></a>正在为已处理的路由事件注册处理程序

之前我们说过，将设置**处理**为**true**会阻止调用大多数处理程序。 但[**AddHandler**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.addhandler)方法提供了一种方法，您可以在该方法中附加一个始终为该路由调用的处理程序，即使该路由中前面的其他处理程序已在共享事件数据中**被设置为** **true** 。 如果你正在使用的控件已在其内部合成中处理了该事件或对于特定于控件的逻辑，则此方法非常有用。 但仍希望从控件实例或应用程序 UI 对其做出响应。 但请谨慎使用此方法，因为它可能不符合已**处理**的目的，可能会中断控件的预期交互。

只有具有相应路由事件标识符的路由事件才能使用[**addhandler**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.addhandler)事件处理方法，因为该标识符是**AddHandler**方法的必需输入。 有关可用路由事件标识符的事件列表，请参阅[**AddHandler**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.addhandler)的参考文档。 大多数情况下，这是我们之前向您展示的路由事件的列表。 但列表中的最后两个例外是： [**GotFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.gotfocus)和[**LostFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.lostfocus)没有路由事件标识符，因此不能对其使用**AddHandler** 。

## <a name="routed-events-outside-the-visual-tree"></a>可视化树外的路由事件

某些对象参与与主要可视化树的关系，该关系在概念上类似于主要视觉对象上的重叠。 这些对象不是将所有树元素连接到 visual 根的常用父子关系的一部分。 这种情况适用于显示的任何[**弹出**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.Popup)项或[**工具提示**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ToolTip)。 如果要从**弹出**项或**工具提示**处理路由事件，请将处理程序放置在**弹出**项或**工具提示**内的特定 UI 元素上，而不是放在**popup**或**tooltip**元素本身。 不要依赖于为**Popup**或**ToolTip**内容执行的任何组合中的路由。 这是因为路由事件的事件路由仅适用于主可视化树。 **Popup**或**ToolTip**不会被视为子公司 UI 元素的父项，也不会接收路由事件，即使它正尝试使用诸如**Popup**默认背景之类的内容作为输入事件的捕获区域。

## <a name="hit-testing-and-input-events"></a>命中测试和输入事件

确定元素对鼠标、触摸和触笔输入是否可见的元素是否和位置称为*命中测试*。 对于触控操作以及对触控操作后果的特定于交互或操作的事件，必须将元素作为事件源可见，并激发与操作关联的事件。 否则，该操作将通过元素传递到可与该输入交互的可视化树中的任何基础元素或父元素。 有若干因素会影响命中测试，但你可以通过检查给定元素的[**system.windows.uielement.ishittestvisible**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.ishittestvisible)属性来确定该元素是否可以激发输入事件。 仅当元素满足以下条件时，此属性才返回**true** ：

- 元素的[**可见性**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.visibility)属性值为[**Visible**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Visibility)。
- 元素的**背景**或**Fill**属性值不为**null**。 **空**[**画笔**](/uwp/api/Windows.UI.Xaml.Media.Brush)值将导致透明度和命中测试不可见。 （若要使元素透明但也可测试，请使用[**透明**](https://docs.microsoft.com/uwp/api/windows.ui.colors.transparent)画笔，而不是**null**。）

**注意：**   **背景**和**填充**不是由[**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement)定义的，而是由不同的派生类（如[**控件**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control)和[**形状**](/uwp/api/Windows.UI.Xaml.Shapes.Shape)）定义的。 但对于命中测试和输入事件，使用的画笔的含义是相同的，无论哪个子类实现属性都是一样的。

- 如果该元素是控件，则其[**IsEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.isenabled)属性值必须为**true**。
- 元素必须具有布局中的实际维度。 一个元素，其中[**system.windows.frameworkelement.actualheight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.actualheight)和[**system.windows.frameworkelement.actualwidth**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.actualwidth)为0时不会激发输入事件。

某些控件具有用于命中测试的特殊规则。 例如， [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock)没有**背景**属性，但仍可在其维度的整个区域内实现可测试性。 [**图像**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image)和[**MediaElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaElement)控件在其已定义的矩形维度上可进行测试，而不考虑显示的媒体源文件中的 alpha 通道等透明内容。 [**Web 视图**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebView)控件具有特殊的命中测试行为，因为输入可以由托管的 HTML 处理并激发脚本事件。

大多数[**面板**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Panel)类和[**边框**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Border)不会在其自己的背景中进行命中测试，但仍可处理从其包含的元素路由的用户输入事件。

无论元素是否可测试，都可以确定与用户输入事件位于同一位置的元素。 为此，请调用[**FindElementsInHostCoordinates**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.visualtreehelper.findelementsinhostcoordinates)方法。 顾名思义，此方法在相对于指定宿主元素的位置查找元素。 但是，应用的转换和布局更改可以调整元素的相对坐标系统，从而影响在给定位置找到的元素。

## <a name="commanding"></a>命令

少量的 UI 元素支持*命令*。 命令在其基础实现中使用与输入相关的路由事件，并通过调用单个命令处理程序来支持处理相关的 UI 输入（特定的指针操作，即特定的快捷键）。 如果命令可用于 UI 元素，请考虑使用其命令 Api，而不是任何离散输入事件。 通常将**绑定**引用用于定义数据的视图模型的类的属性。 属性保存实现特定于语言的**ICommand**命令模式的命名命令。 有关详细信息，请参阅[**system.windows.controls.primitives.buttonbase.click>** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.command)。

## <a name="custom-events-in-the-windows-runtime"></a>Windows 运行时中的自定义事件

出于定义自定义事件的目的，如何添加事件和对类设计的含义非常依赖于所使用的编程语言。

- 对于C#和 Visual Basic，你将定义 CLR 事件。 您可以使用标准的 .NET 事件模式，只要您不使用自定义访问器（**添加**/**删除**）即可。 其他提示：
    - 对于事件处理程序，使用[**EventHandler<TEventArgs>** ](https://docs.microsoft.com/dotnet/api/system.eventhandler-1)是一种很好的方法，因为它具有对 Windows 运行时通用事件委托[**EventHandler<T>** ](https://docs.microsoft.com/uwp/api/windows.foundation.eventhandler)的内置转换。
    - 不要将事件数据类基于[**system.object**](https://docs.microsoft.com/dotnet/api/system.eventargs) ，因为它不会转换为 Windows 运行时。 使用现有事件数据类或根本不使用基类。
    - 如果使用的是自定义访问器，请参阅[Windows 运行时组件中的自定义事件和事件访问器](https://docs.microsoft.com/previous-versions/windows/apps/hh972883(v=vs.140))。
    - 如果你不清楚标准 .NET 事件模式的定义，请参阅[定义自定义 Silverlight 类的事件](https://docs.microsoft.com/previous-versions/windows/)。 这是针对 Microsoft Silverlight 编写的，但它仍是标准 .NET 事件模式的代码和概念的一个很好的汇总。
- 对于C++/cx，请参阅[事件C++（/cx）](https://docs.microsoft.com/cpp/cppcx/events-c-cx)。
    - 即使您自己使用自定义事件，也请使用命名引用。 不要将 lambda 用于自定义事件，而是可以创建循环引用。

不能为 Windows 运行时声明自定义路由事件;路由事件限制为来自 Windows 运行时的集。

定义自定义事件通常是在定义自定义控件的操作过程中完成的。 这是一种常见的模式，具有具有属性更改的回调的依赖属性，还可以定义在某些情况或所有情况下由依赖属性回调激发的自定义事件。 您的控件的使用者无法访问您定义的属性更改的回调，但有一个可用的通知事件是下一项最佳事情。 有关详细信息，请参阅[自定义依赖属性](custom-dependency-properties.md)。

## <a name="related-topics"></a>相关主题

* [XAML 概述](xaml-overview.md)
* [快速入门：触控输入](https://docs.microsoft.com/previous-versions/windows/apps/hh465387(v=win.10))
* [键盘交互](https://docs.microsoft.com/windows/uwp/input-and-devices/keyboard-interactions)
* [.NET 事件和委托](https://go.microsoft.com/fwlink/p/?linkid=214364)
* [创建 Windows 运行时组件](https://docs.microsoft.com/previous-versions/windows/apps/hh441572(v=vs.140))
* [**AddHandler**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.addhandler)
