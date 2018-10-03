---
author: jwmsft
description: 我们将介绍在使用 C#、Visual Basic 或 Visual C++ 组件扩展 (C++/CX) 作为编程语言并使用 XAML 进行 UI 定义时，针对 Windows 运行时应用中事件的编程概念。
title: 事件和路由事件概述
ms.assetid: 34C219E8-3EFB-45BC-8BBD-6FD937698832
ms.author: jimwalk
ms.date: 07/12/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6ca58613a5874cde10d2bb5322c3f930e1fbce44
ms.sourcegitcommit: e6daa7ff878f2f0c7015aca9787e7f2730abcfbf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/03/2018
ms.locfileid: "4309372"
---
# <a name="events-and-routed-events-overview"></a>事件和路由事件概述

**重要的 API**
-   [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911)
-   [**RoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br208809)

我们将介绍在使用 C#、Visual Basic 或 Visual C++ 组件扩展 (C++/CX) 作为编程语言并使用 XAML 进行 UI 定义时，针对 Windows 运行时应用中事件的编程概念。 你可以在 XAML 中的 UI 元素声明中为事件分配处理程序，或者在代码中添加处理程序。 Windows 运行时支持*路由事件*：借助此功能，某些输入事件和数据事件可由引发该事件的对象以外的对象来处理。 在定义控件模板或使用页面或版式容器时，路由事件十分有用。

## <a name="events-as-a-programming-concept"></a>事件即编程概念

通常而言，对 Windows 运行时应用进行编程时事件概念与最热门编程语言中的事件模型类似。 如果你已知道如何使用 Microsoft .NET 或 C++ 事件，那么你已领先一步。 但你无需深入了解事件模型概念，即可执行某些基本任务，例如附加处理程序。

当你使用 C#、Visual Basic 或 C++/CX 作为编程语言时，UI 是通过标记 (XAML) 定义的。 对于 XAML 标记语法，将事件与标记元素和运行时代码实体联系起来的某些原则与其他 Web 技术（例如 ASP.NET 或 HTML5）类似。

**注意**  为 XAML 定义的 UI 提供运行时逻辑的代码常常称为*代码隐藏*或代码隐藏文件。 在 Microsoft Visual Studio 解决方案视图中，此关系以图形方式显示，同时代码隐藏文件是一个独立、嵌套的文件，而不是它引用的 XAML 页面。

## <a name="buttonclick-an-introduction-to-events-and-xaml"></a>按钮.单击：事件和 XAML 简介

Windows 运行时应用的一个最常见的编程任务是捕获用户在 UI 上的输入。 例如，你的 UI 可能有一个按钮，用户必须单击它才能提交信息或更改状态。

通过生成 XAML 来定义 Windows 运行时应用的 UI。 该 XAML 通常为来自 Visual Studio 设计平面的输出。 此外，也可在纯文本编辑器或第三方 XAML 编辑器中编写 XAML。 生成该 XAML 时，你可以在定义所有其他建立该 UI 元素的 XAML 属性值的同时，连接各个 UI 元素的事件处理程序。

要连接 XAML 中的事件，需指定已在代码隐藏中定义或稍后定义的处理程序方法的字符串形式名称。 例如，该 XAML 会在其他属性（[x:Name 属性](x-name-attribute.md)，[**Content**](https://msdn.microsoft.com/library/windows/apps/br209366)）分配为特性的情况下定义 [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265) 对象，并通过引用名为 `ShowUpdatesButton_Click` 的方法为该按钮的 [**Click**](https://msdn.microsoft.com/library/windows/apps/br227737) 事件连接一个处理程序：

```xaml
<Button x:Name="showUpdatesButton"
  Content="{Binding ShowUpdatesText}"
  Click="ShowUpdatesButton_Click"/>
```

**提示**  *事件连接*是一个编程术语。 它是指进程或代码，凭此你可以指示某个事件的出现应调用命名处理程序方法。 在大部分过程代码模型中，事件连接是隐式或显式的“AddHandler”代码，用于命名事件和方法并通常涉及目标对象实例。 在 XAML 中，“AddHandler”是隐式的，事件连接完全由将事件命名为对象元素的属性名称和将处理程序命名为该属性的值组成。

然后，使用编程语言（用于你所有应用的代码和代码隐藏的语言）编写实际的处理程序。 在属性 `Click="ShowUpdatesButton_Click"` 中，你创建了一个合约：当对 XAML 进行标记编译和分析时，IDE 的生成操作和最终应用加载时 XAML 分析操作中的 XAML 标记编译步骤都可以找到一个作为该应用的代码的一部分且名为 `ShowUpdatesButton_Click` 的方法。 `ShowUpdatesButton_Click` 必须是一个方法，并且该方法要为 [**Click**](https://msdn.microsoft.com/library/windows/apps/br227737) 事件的任何处理程序都实现一个兼容的方法签名（基于一个委托）。 例如，此代码定义 `ShowUpdatesButton_Click` 处理程序。

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

此例中，`ShowUpdatesButton_Click` 方法基于 [**RoutedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br208812) 委托。 由于该委托以 MSDN 参考页面上 [**Click**](https://msdn.microsoft.com/library/windows/apps/br227737) 方法的语法进行命名，你便可确认该委托为待使用的委托。

**提示**  Visual Studio 提供了一种便捷方式，以供你在编辑 XAML 时命名事件处理程序和定义处理程序方法。 当在 XAML 文本编辑器中提供事件的属性名称时，稍等片刻就会显示 Microsoft IntelliSense 列表。 如果单击该列表中的**&lt;新建事件处理程序&gt;**，Microsoft Visual Studio 将基于元素的 **x:Name**（或类型名）、事件名称和数字后缀建议一个方法名称。 然后可以右键单击所选的事件处理程序名称，并单击“导航到事件处理程序”****。 此操作将直接导航到新插入的事件处理程序定义，如 XAML 页面代码隐藏文件的代码编辑器中所示。 事件处理程序已拥有正确的签名，包括 *sender* 参数和该事件所使用的事件数据类。 另外，如果代码隐藏文件中已存在一个具有正确签名的处理程序方法，该方法的名称会与**&lt;新建事件处理程序&gt;** 选项一起显示在自动完成下拉列表中。 此外，也可按下 Tab 键（作为快捷方式）来代替单击 IntelliSense 列表项。

## <a name="defining-an-event-handler"></a>定义事件处理程序

对于充当 UI 元素并在 XAML 中声明的对象，事件处理程序代码将在一个分部类中定义，该类用作 XAML 页面的代码隐藏。 事件处理程序是你编写的方法，是与 XAML 关联的分部类中的一部分。 这些事件处理程序基于一个特定事件使用的委托。 事件处理程序方法可以是公共的或私有的。 私有访问可以使用，原因在于 XAML 创建的处理程序和实例会在最终生成代码时合并在一起。 一般而言，我们建议让事件处理程序方法在类中保持私有。

**注意**  针对 C++ 的事件处理程序不会在分部类中定义，它们会在标头中声明为私有类成员。 C++ 项目的生成操作负责生成特定代码，这些代码支持适用于 C++ 的 XAML 类型体系和代码隐藏模型。

### <a name="the-sender-parameter-and-event-data"></a>*sender* 参数和事件数据

为事件编写的处理程序可以访问两个值，这两个值对于调用处理程序的每种情况都可以用作输入。 第一个值是 *sender*，它是处理程序所附加到的对象的引用。 *sender* 参数的类型设置为基础 **Object** 类型。 一种常见技术是将 *sender* 转换为一种更准确的类型。 如果期望检查或更改 *sender* 对象本身的状态，此技术很有用。 基于你自己的应用设计，你通常知道可将 *sender* 安全地转换到哪种类型（根据处理程序的附加位置或其他设计细节）。

第二个值为事件数据，它通常在语法定义中显示为 *e* 参数。 你可以通过查看委托（分配给你正在处理的特定事件）的 *e* 参数，然后使用 Visual Studio 中的 IntelliSense 或对象浏览器，发现事件数据的哪些属性可用。 或者可以使用 Windows 运行时参考文档。

对于一些事件，事件数据的具体属性值与知道已发生该事件同样重要。 这对于输入事件尤其如此。 对于指针事件，在事件发生时指针的位置可能很重要。 对于键盘事件，所有可能的按键都会引发 [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) 和 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) 事件。 要确定用户按下了哪个键，必须访问可供事件处理程序使用的 [**KeyRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh943072)。 有关处理输入事件的详细信息，请参阅[键盘交互](https://msdn.microsoft.com/library/windows/apps/mt185607)和[处理指针输入](https://msdn.microsoft.com/library/windows/apps/mt404610)。 输入事件和输入场景常常涉及到本文未介绍的其他考虑因素，例如针对指针事件的指针捕获，以及针对键盘事件的修改键和平台键代码。

### <a name="event-handlers-that-use-the-async-pattern"></a>使用 **async** 模式的事件处理程序

在某些情况下，可能想要在事件处理程序内使用采用 **async** 模式的 API。 例如，可以在 [**AppBar**](https://msdn.microsoft.com/library/windows/apps/hh701927) 中使用 [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265) 来显示文件选取器并与之交互。 但是，许多文件选取器 API 都是异步的。 必须在 **async**/awaitable 作用域中调用它们，编译器将强制执行此操作。 因此，你可以执行的操作是将 **async** 关键字添加到你的事件处理程序，以使该处理程序现在为 **async** **void**。 现在允许你的事件处理程序执行 **async**/awaitable 调用。

有关使用 **async** 模式的用户交互事件处理示例，请参阅[文件访问和选取器](https://msdn.microsoft.com/library/windows/apps/jj655411)（[创建你的第一个使用 C# 或 Visual Basic 的 Windows 运行时应用](https://msdn.microsoft.com/library/windows/apps/hh974581)系列的一部分）。 另请参阅 [使用 C 调用异步 API]。

## <a name="adding-event-handlers-in-code"></a>在代码中添加事件处理程序

XAML 不是向对象分配事件处理程序的唯一方式。 要在代码中向任何给定对象添加事件处理程序，包括无法在 XAML 中使用的对象，可以使用特定于语言的语法来添加事件处理程序。

在 C# 中，语法是使用 `+=` 运算符。 你可在运算符右侧引用事件处理程序方法名称来注册处理程序。

如果使用代码向运行时 UI 中显示的对象添加事件处理程序，一种常见的做法是添加这些处理程序来响应对象生存期事件或回调，例如 [**Loaded**](https://msdn.microsoft.com/library/windows/apps/br208723) 或 [**OnApplyTemplate**](https://msdn.microsoft.com/library/windows/apps/br208737)，这可使相关对象上的事件处理程序在运行时准备好处理用户发起的事件。 该示例展示了页面结构的 XAML 概括，同时提供了用于将事件处理程序添加到对象的 C# 语言语法。

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

**注意**  此外，还有一种更详细的语法。 在 2005 年，C# 添加了一个称为委托推断的功能，它使编译器能够推断新委托实例并实现以前更简单的语法。 详细语法在功能上等同于以前的示例，但显式创建了一个新委托实例，然后再注册它，进而避免利用委托推断。 这种显式的语法不太常见，但你仍会在一些代码示例中看到它。

```csharp
void LayoutRoot_Loaded(object sender, RoutedEventArgs e)
{
    textBlock1.PointerEntered += new PointerEventHandler(textBlock1_PointerEntered);
    textBlock1.PointerExited += new MouseEventHandler(textBlock1_PointerExited);
}
```

Visual Basic 语法有两种可能性。 一种类似于 C# 语法，将处理程序直接附加到实例。 这需要 **AddHandler** 关键字和取消引用处理程序方法名称的 **AddressOf** 运算符。

Visual Basic 语法的另一种选择是在事件处理程序上使用 **Handles** 关键字。 此技术适用于处理程序应在加载时存在于对象上并在整个对象生存期持久化的情况。 在 XAML 中定义的一个对象上使用 **Handles**，需要你提供一个 **Name** / **x:Name**。 此名称成为 **Handles** 语法的 *Instance.Event* 部分所需的实例限定符。 在此情况下，你无需一个基于对象生存期的事件处理程序，即可开始附加其他事件处理程序；在编译 XAML 页面时会创建 **Handles** 连接。

```vb
Private Sub textBlock1_PointerEntered(ByVal sender As Object, ByVal e As PointerRoutedEventArgs) Handles textBlock1.PointerEntered
' ...
End Sub
```

**注意**  Visual Studio 以及其 XAML 设计界面一般都提倡使用实例处理技术代替，而不是 **Handles** 关键字。 这是因为在 XAML 中建立事件处理程序连接是典型的设计人员-开发人员工作流中的一部分，并且 **Handles** 关键字技术与在 XAML 中连接事件处理程序不兼容。

在 C + + CX，你还使用**+=** 语法，但与基本 C# 形式有区别：

-   不存在委托推断，所以必须为委托实例使用 **ref new** 关键字。
-   委托构造函数有两个参数，并且需要目标对象作为第一个参数。 通常由你指定 **this**。
-   委托构造函数需将方法地址作为第二个参数，所以 **&** 引用运算符位于方法名称之前。

```cppwinrt
textBlock1().PointerEntered({this, &MainPage::TextBlock1_PointerEntered });
```

```cpp
textBlock1->PointerEntered += 
ref new PointerEventHandler(this, &BlankPage::textBlock1_PointerEntered);
```

### <a name="removing-event-handlers-in-code"></a>在代码中删除事件处理程序

通常不需要删除代码中的事件处理程序，即便事件处理程序是你在代码中添加的也是如此。 对于大多数 Windows 运行时对象（如页面和控件）来说，当它们从主 [**Window**](https://msdn.microsoft.com/library/windows/apps/br209041) 及其可视化树断开连接时，它们的对象生存期行为将销毁对象，而且任何委托引用也将被销毁。 .NET 通过垃圾收集完成此操作，并且采用 C++/CX 的 Windows 运行时默认情况下使用弱引用。

在极少数情况下，你希望明确删除事件处理程序。 其中包括：

-   你为静态事件添加的处理程序（不能按照传统的方式进行垃圾回收）。 例如，[**CompositionTarget**](https://msdn.microsoft.com/library/windows/apps/br228126) 和 [**Clipboard**](https://msdn.microsoft.com/library/windows/apps/br205867) 类的事件就是 Windows 运行时 API 中的静态事件。
-   你希望立即删除其中的处理程序计时的测试代码，或者你希望在运行时交换其中的旧/新事件处理程序的代码。
-   所实现的自定义 **remove** 访问器。
-   自定义的静态事件。
-   页面导航的处理程序。

[**FrameworkElement.Unloaded**](https://msdn.microsoft.com/library/windows/apps/br208748) 或 [**Page.NavigatedFrom**](https://msdn.microsoft.com/library/windows/apps/br227507) 是可能的事件触发器，它们在状态管理和对象生存期中具有合适的位置，以便你可以使用它们删除其他事件的处理程序。

例如，你可以使用以下代码，将名为 **textBlock1\_PointerEntered** 的事件处理程序从目标对象 **textBlock1** 中删除。

```csharp
textBlock1.PointerEntered -= textBlock1_PointerEntered;
```

```vb
RemoveHandler textBlock1.PointerEntered, AddressOf textBlock1_PointerEntered
```

如果事件是通过 XAML 属性添加的（即，处理程序是在所生成的代码中添加的），你还可以删除处理程序。 如果你为处理程序所附加到的元素添加了一个 **Name** 值，则上述操作更容易执行，因为这会在以后为代码提供对象引用；然而，你也可以浏览对象树，以便在对象没有 **Name** 的情况下查找必要的对象引用。

如果需要删除采用 C++/CX 的事件处理程序，则你将需要一个你应当已从 `+=` 事件处理程序注册的返回值中收到的注册令牌。 这是因为在 C++/CX 语法中，位于 `-=` 注销右侧的值是令牌，而不是方法名称。 对于 C++/CX，你无法删除作为 XAML 属性添加的处理程序，因为 C++/CX 生成的代码不保存令牌。

## <a name="routed-events"></a>路由事件

对于存在于大部分 UI 元素上的一组事件，采用 C#、Microsoft Visual Basic 或 C++/CX 的 Windows 运行时支持路由事件的概念。 这些事件适用于输入和用户交互方案，并且是在 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) 基类上实现的。 以下是属于路由事件的输入事件列表：

-   [**DoubleTapped**](https://msdn.microsoft.com/library/windows/apps/br208922)
-   [**DragEnter**](https://msdn.microsoft.com/library/windows/apps/br208923)
-   [**DragLeave**](https://msdn.microsoft.com/library/windows/apps/br208924)
-   [**DragOver**](https://msdn.microsoft.com/library/windows/apps/br208925)
-   [**Drop**](https://msdn.microsoft.com/library/windows/apps/br208926)
-   [**Holding**](https://msdn.microsoft.com/library/windows/apps/br208928)
-   [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941)
-   [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942)
-   [**ManipulationCompleted**](https://msdn.microsoft.com/library/windows/apps/br208945)
-   [**ManipulationDelta**](https://msdn.microsoft.com/library/windows/apps/br208946)
-   [**ManipulationInertiaStarting**](https://msdn.microsoft.com/library/windows/apps/br208947)
-   [**ManipulationStarted**](https://msdn.microsoft.com/library/windows/apps/br208950)
-   [**ManipulationStarting**](https://msdn.microsoft.com/library/windows/apps/br208951)
-   [**PointerCanceled**](https://msdn.microsoft.com/library/windows/apps/br208964)
-   [**PointerCaptureLost**](https://msdn.microsoft.com/library/windows/apps/br208965)
-   [**PointerEntered**](https://msdn.microsoft.com/library/windows/apps/br208968)
-   [**PointerExited**](https://msdn.microsoft.com/library/windows/apps/br208969)
-   [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208970)
-   [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208971)
-   [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208972)
-   [**PointerWheelChanged**](https://msdn.microsoft.com/library/windows/apps/br208973)
-   [**RightTapped**](https://msdn.microsoft.com/library/windows/apps/br208984)
-   [**Tapped**](https://msdn.microsoft.com/library/windows/apps/br208985)
-   [**GotFocus**](https://msdn.microsoft.com/library/windows/apps/br208927)
-   [**LostFocus**](https://msdn.microsoft.com/library/windows/apps/br208943)

路由事件是一种可能从一个子对象（*路由*）传递到对象树中它的每个后续父对象的事件。 UI 的 XAML 结构大体类似于此树，该树的根是 XAML 中的根元素。 真正的对象树可能与 XAML 元素嵌套稍有区别，因为对象树不包含 XAML 语言功能，例如属性元素标记。 你可以将路由事件视为从任何 XAML 对象元素中引发事件的子元素向包含它的父对象元素*浮升*。 该事件及其事件数据可以沿事件路由在多个对象上进行处理。 如果元素都不具有处理程序，则可能继续进行路由，直到达到根元素为止。

如果了解 Web 技术（例如动态 HTML 或 HTML5），你可能已熟悉*浮升*事件概念。

当一个路由事件沿它的事件路由浮升时，任何附加的事件处理程序都会访问事件数据的一个共享实例。 因此，如果任何事件数据对处理程序而言是可写的，对事件数据的任何更改将传递到下一个处理程序，而且可能不再表示来自于该事件的原始事件数据。 当一个事件具有路由事件的行为时，参考文档将包含有关路由行为的备注或其他表示法。

### <a name="the-originalsource-property-of-routedeventargs"></a>**RoutedEventArgs** 的 **OriginalSource** 属性

当一个事件朝一个事件路线浮升时，*sender* 不再与引发事件的对象是同一个对象。 相反，*sender* 是所调用的处理程序附加到的对象。

在某些情况下，*sender* 不是应关注的对象，你关注的是一些信息，例如在触发指针事件时指针在哪个可能的子对象上方，或者在用户按下键盘上的键时较大 UI 中的哪个对象拥有焦点。 对于这些情况，你可以使用 [**OriginalSource**](https://msdn.microsoft.com/library/windows/apps/br208810) 属性的值。 在路由上的所有点上，**OriginalSource** 都会报告引发事件的原始对象，而不是报告附加了处理程序的对象。 但是，对于 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) 输入事件，该原始对象常常是一个不会在页面级 UI 定义 XAML 中立即可见的对象。 相反，该原始源对象可能是控件的一个模板部分。 例如，如果用户将指针悬停在 [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265) 的边缘，对于大部分指针事件，**OriginalSource** 是 [**Template**](https://msdn.microsoft.com/library/windows/apps/br209465) 中的 [**Border**](https://msdn.microsoft.com/library/windows/apps/br209250) 模板部分，而不是 **Button** 本身。

**提示**  如果创建模板化控件，输入事件浮升将十分有用。 对于任何具有模板的控件，其客户都可能应用一个新模板。 尝试重新创建工作模板的客户可能会无意中清除默认模板中声明的某些事件处理功能。 你仍然可以通过在类定义中将处理程序附加为 [**OnApplyTemplate**](https://msdn.microsoft.com/library/windows/apps/br208737) 替代的一部分来提供控件级事件处理功能。 然后，你可以捕获在实例化时向上浮升至控件根的输入事件。

### <a name="the-handled-property"></a>**Handled** 属性

特定路由事件的多个事件数据类包含一个名为 **Handled** 的属性。 例如，请参阅 [**PointerRoutedEventArgs.Handled**](https://msdn.microsoft.com/library/windows/apps/hh943079)、[**KeyRoutedEventArgs.Handled**](https://msdn.microsoft.com/library/windows/apps/hh943073)、[**DragEventArgs.Handled**](https://msdn.microsoft.com/library/windows/apps/br242375)。 在任何情况下，**Handled** 都是可设置的布尔值属性。

将 **Handled** 属性设置为 **true** 会影响事件系统的行为。 当 **Handled** 为 **true** 时，大部分事件处理程序的路由都会停止；该事件不会沿该路由继续传递，因此也无法通知其他附加的处理程序所发生的特定事件情况。 在事件上下文中“handled”有何含义以及你的应用如何响应它，这完全取决于你。 基本上，**Handled** 是一个简单协议，用于使应用代码表明事件的发生无需浮升到任何容器，你的应用逻辑已负责需要完成的操作。 相反，应务必关注尚未处理的可能应该浮升的事件，以便内置系统或控件行为可以执行操作。例如，在选择控件的部分或项目中处理低级别事件可能会产生不利影响。 选择控件可能要查找输入事件，以知悉应该更改选择。

并非所有路由事件都可以使用此方式取消路由，你可以判断此类事件，因为它们没有 **Handled** 属性。 例如，[**GotFocus**](https://msdn.microsoft.com/library/windows/apps/br208927) 和 [**LostFocus**](https://msdn.microsoft.com/library/windows/apps/br208943) 可以浮升，但是它们会一直浮升到根，它们的事件数据类没有可影响该行为的 **Handled** 属性。

##  <a name="input-event-handlers-in-controls"></a>控件中的输入事件处理程序

特定的 Windows 运行时控件有时会在内部为输入事件使用 **Handled** 概念。 这可能使它看起来像一个从不会发生的输入事件，因为用户代码无法处理它。 例如，[**Button**](https://msdn.microsoft.com/library/windows/apps/br209265) 类包含专门处理一般输入事件 [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208971) 的逻辑。 它这么做是因为，按钮引发了 [**Click**](https://msdn.microsoft.com/library/windows/apps/br227737) 事件，该事件最初由指针点击输入触发，或是由其他输入模式触发，例如在聚焦某一按钮时可调用该按钮的 Enter 键等处理键。 出于类设计 **Button** 的目的，原始输入事件会从概念上进行处理，而类使用者（例如你的用户代码）实际与控件相关的 **Click** 事件进行交互。 Windows 运行时 API 参考中针对特定控件类的主题常常会提到该类实现的事件处理行为。 在某些情况下，可通过重写 **On***Event* 方法来更改此行为。 例如，可通过重写 [**Control.OnKeyDown**](https://msdn.microsoft.com/library/windows/apps/hh967982) 来更改 [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) 派生类响应键输入的方式。

##  <a name="registering-handlers-for-already-handled-routed-events"></a>注册已处理的路由事件的处理程序

前面我们已经提到，将 **Handled** 设置为 **true** 会阻止调用大部分处理程序。 但是，[**AddHandler**](https://msdn.microsoft.com/library/windows/apps/hh702399) 方法提供了一种技术，可通过该技术附加一个始终为该路由调用的处理程序，即使该路由中其他某些以前的处理程序已在共享事件数据中将 **Handled** 设置为 **true** 也是如此。 如果你使用的控件已在其内部组合元素中或针对特定于控件的逻辑处理了事件，但是你仍要从控件实例或应用 UI 响应它， 此技术将非常有用。 但是，此技术应谨慎使用，因为它可能与 **Handled** 的用途相矛盾，并且可能中断控件的既定交互。

只有具有相应路由事件标识符的路由事件可使用 [**AddHandler**](https://msdn.microsoft.com/library/windows/apps/hh702399) 事件处理技术，因为该标识符是 **AddHandler** 方法的必需输入。 请参阅 [**AddHandler**](https://msdn.microsoft.com/library/windows/apps/hh702399) 的参考文档，了解可获得路由事件标识符的事件列表。 大多数情况下，此列表与前面所述的路由事件列表基本相同。 唯一的区别在于，此列表中的最后两个事件（即 [**GotFocus**](https://msdn.microsoft.com/library/windows/apps/br208927) 和 [**LostFocus**](https://msdn.microsoft.com/library/windows/apps/br208943)）没有路由事件标识符，因此你不能针对这两个事件使用 **AddHandler**。

## <a name="routed-events-outside-the-visual-tree"></a>可视化树外部的路由事件

某些对象与主要可视化树具有一种关系，这种关系在概念上就像在主要可视元素上有一个覆盖图。 这些对象没有将所有树元素连接到可视根的常见父-子关系。 任何已显示的 [**Popup**](https://msdn.microsoft.com/library/windows/apps/br227842) 或 [**ToolTip**](https://msdn.microsoft.com/library/windows/apps/br227608) 都属于这种情况。 如果你希望从一个 **Popup** 或 **ToolTip** 处理路由事件，可在 **Popup** 或 **ToolTip** 内的特定 UI 元素上放置处理程序，而不是在 **Popup** 或 **ToolTip** 元素本身上。 不要依赖于为 **Popup** 或 **ToolTip** 内容执行的任何组合元素内的路由。 这是因为路由事件的事件路由仅适用于主要可视化树。 **Popup** 或 **ToolTip** 不应被视为子 UI 元素的父元素，并且绝不会接收路由事件，即使它尝试将 **Popup** 默认背景之类的内容用作输入事件的捕获区域。

## <a name="hit-testing-and-input-events"></a>点击测试和输入事件

确定某个元素是否对鼠标、触摸和触笔输入可见以及其在 UI 中的位置称为*点击测试*。 对于触摸操作以及特定于交互的事件或一个触摸操作引起的操作事件，一个元素必须对点击测试可见，以用作事件源并触发与该操作关联的事件。 否则，该操作会通过该元素传递到可与该输入交互的可视化树中的任意基础元素或父元素。 影响点击测试的因素有很多，但你可以通过检查给定元素的 [**IsHitTestVisible**](https://msdn.microsoft.com/library/windows/apps/br208933) 属性来确定该元素是否会引发输入事件。 只有当该元素符合以下条件时，该属性才返回 **true**：

-   元素的 [**Visibility**](https://msdn.microsoft.com/library/windows/apps/br208992) 属性值为 [**Visible**](https://msdn.microsoft.com/library/windows/apps/br209006)。
-   元素的 **Background** 或 **Fill** 属性值不是 **null**。 **null** [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) 值会导致透明性和点击测试不可见。 （若要使元素透明而且可执行点击测试，可使用 [**Transparent**](https://msdn.microsoft.com/library/windows/apps/hh748061) 画笔代替 **null**。）

**注意**  **Background** 和 **Fill** 不由 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) 定义，而是由不同的派生类定义，例如 [**Control**](https://msdn.microsoft.com/library/windows/apps/br209390) 和 [**Shape**](/uwp/api/Windows.UI.Xaml.Shapes.Shape)。 但你为前景和背景属性使用的画笔含义对点击测试和输入事件而言是相同的，无论是哪些子类实现了这些属性。

-   如果该元素为控件，那么它的 [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/br209419) 属性值必须为 **true**。
-   该元素必须具有实际的布局大小。 [**ActualHeight**](https://msdn.microsoft.com/library/windows/apps/br208707) 和 [**ActualWidth**](https://msdn.microsoft.com/library/windows/apps/br208709) 为 0 的元素不会引发输入事件。

某些控件对点击测试有特殊规则。 例如，[**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) 没有 **Background** 属性，但它仍然可在其大小的整个区域内进行点击测试。 [**Image**](https://msdn.microsoft.com/library/windows/apps/br242752) 和 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) 控件可在它们定义的矩形大小上执行点击测试，无论显示的媒体源文件中显示了何种透明内容，例如 alpha 通道。 由于该输入可由托管 HTML 处理并引发脚本事件，因此 [**WebView**](https://msdn.microsoft.com/library/windows/apps/br227702) 控件具有特殊的点击测试行为。

大部分 [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511) 类和 [**Border**](https://msdn.microsoft.com/library/windows/apps/br209250) 都不能在它们自己的后台进行点击测试，但仍然可以处理从它们包含的元素中路由的用户输入事件。

你可以确定哪些元素与用户输入事件的位置相同，而不论这些元素是否可进行点击测试。 要实现此目的，请调用 [**FindElementsInHostCoordinates**](https://msdn.microsoft.com/library/windows/apps/br243039) 方法。 顾名思义，该方法在相对于指定主机元素的位置查找元素。 但是，应用的转换和布局更改会调整元素的相对坐标系统，因此也会影响给定位置上找到的元素。

## <a name="commanding"></a>命令处理

少量 UI 元素支持*命令处理*。 命令处理在其基础实现中使用了与输入相关的路由事件，支持通过调用单个命令处理程序来处理相关的 UI 输入（某种指针操作，一种特定的加速键）。 如果命令处理可用于 UI 元素，可以考虑使用它的命令处理 API 代替任何具体的输入事件。 你通常将 **Binding** 引用用于为数据定义视图模型的类属性。 这些属性将保留可实现特定于语言的 **ICommand** 命令操作模式的命名命令。 有关详细信息，请参阅 [**ButtonBase.Command**](https://msdn.microsoft.com/library/windows/apps/br227740)。

## <a name="custom-events-in-the-windows-runtime"></a>Windows 运行时中的自定义事件

在定义自定义事件时，事件添加方式及其对于类设计的含义高度依赖于你所使用的编程语言。

-   对于 C# 和 Visual Basic，你定义了一个 CLR 事件。 你可以使用标准的 .NET 事件模式，但前提是你未使用自定义的访问器 (**add**/**remove**)。 其他提示：
    -   对于事件处理程序，最好使用 [**System.EventHandler<TEventArgs>**](https://msdn.microsoft.com/library/windows/apps/xaml/db0etb8x.aspx)，因为它能够以内置方式转换为 Windows 运行时一般事件委托 [**EventHandler<T>**](https://msdn.microsoft.com/library/windows/apps/br206577)。
    -   请勿将事件数据类以 [**System.EventArgs**](https://msdn.microsoft.com/library/windows/apps/xaml/system.eventargs.aspx) 为基础，因为它不会转换为 Windows 运行时。 使用现有的事件数据类，或者根本不使用基类。
    -   如果你使用的是自定义访问器，请参阅 [Windows 运行时组件中的自定义事件和事件访问器](https://msdn.microsoft.com/library/windows/apps/xaml/hh972883.aspx)。
    -   如果你不清楚什么是标准的 .NET 事件模式，请参阅[为自定义的 Silverlight 类定义事件](http://msdn.microsoft.com/library/dd833067.aspx)。 这是为 Microsoft Silverlight 编写的，但是它同样很好地汇总了标准 .NET 事件模式的代码和概念。
-   对于 C++/CX，请参阅[事件 (C++/CX)](https://msdn.microsoft.com/library/windows/apps/xaml/hh755799.aspx)。
    -   即便你以自己的方式使用自定义事件，也可以使用命名引用。 请勿对自定义事件使用 lambda，因为它会创建循环引用。

你不能声明针对 Windows 运行时的自定义路由事件；路由事件限于来自 Windows 运行时的集合。

自定义事件通常是在练习定义自定义控件的过程中定义的。 常见的模式是，拥有一个具有属性更改回调的依赖属性，而且还定义一个自定义事件，此自定义事件在部分或所有情况下由该依赖属性引发。 控件的使用者无权访问你定义的属性更改回调，但是提供通知事件是次佳措施。 有关详细信息，请参阅[自定义的依赖属性](custom-dependency-properties.md)。

## <a name="related-topics"></a>相关主题

* [XAML 概述](xaml-overview.md)
* [快速入门：触摸输入](https://msdn.microsoft.com/library/windows/apps/xaml/hh465387)
* [键盘交互](https://msdn.microsoft.com/library/windows/apps/mt185607)
* [.NET 事件和委托](http://go.microsoft.com/fwlink/p/?linkid=214364)
* [创建 Windows 运行时组件](https://msdn.microsoft.com/library/windows/apps/xaml/hh441572.aspx)
* [**AddHandler**](https://msdn.microsoft.com/library/windows/apps/hh702399)
