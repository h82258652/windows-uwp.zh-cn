---
description: 本主题对 [C#](/visualstudio/get-started/csharp) 项目中的源代码移植到 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 项目中的等效项时所涉及的技术细节进行了全面分类。
title: 从 C# 移动到 C++/WinRT
ms.date: 07/15/2019
ms.topic: article
keywords: windows 10, uwp, 标准, c++, cpp, winrt, 投影, 端口, 迁移, C#
ms.localizationpriority: medium
ms.openlocfilehash: 734173812ff5a853abfb93eb34fcfa43b9f16872
ms.sourcegitcommit: 1e8f51d5730fe748e9fe18827895a333d94d337f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/28/2020
ms.locfileid: "87296205"
---
# <a name="move-to-cwinrt-from-c"></a>从 C# 移动到 C++/WinRT

本主题对 [C#](/visualstudio/get-started/csharp) 项目中的源代码移植到 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 项目中的等效项时所涉及的技术细节进行了全面分类。

有关移植某一通用 Windows 平台 (UWP) 应用示例的案例研究，请参阅对应的主题[将 Clipboard 示例从 C# 移植到 C++/WinRT](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp)。 可以通过按照该演练操作并在操作时为自己移植示例，来获取移植实践和体验。

## <a name="how-to-prepare-and-what-to-expect"></a>如何准备以及预期的结果

案例研究[将 Clipboard 示例从 C# 移植到 C++/WinRT](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp) 阐释了在将项目迁移到 C++/WinRT 时要做出的软件设计决策类型的示例。 因此，最好是通过深入了解现有代码的工作原理来准备迁移。 这样，可以大致了解应用的功能和代码的结构，然后，你做出的决策就会始终引领你向正确的方向前进。

在所需的移植更改的类型方面，可以将其分组为四个类别。

- [**移植语言投影**](#changes-that-involve-the-language-projection)。 Windows 运行时 (WinRT) 已*投影*到各种编程语言。 其中的每个语言投影均已设计为符合所涉及的编程语言的语言习惯。 对于 C#，某些 Windows 运行时类型被投影为 .NET 类型。 因此例如，你会将 [System.Collections.Generic.IReadOnlyList\<T\>](/dotnet/api/system.collections.generic.ireadonlylist-1) 转换回 [Windows.Foundation.Collections.IVectorView\<T\>](/uwp/api/windows.foundation.collections.ivectorview-1) 。 此外，在 C# 中，某些 Windows 运行时操作也被投影为方便的 C# 语言功能。 例如，在 C# 中，可以使用 `+=` 运算符语法来注册事件处理委托。 因此，你将转换语言功能，例如，转换回要执行的基本操作（在本示例中为事件注册）。
- [**移植语言语法**](#changes-that-involve-the-language-syntax)。 这些更改中的许多更改都是简单的机械转换（将一个符号替换为另一个符号）。 例如，将点 (`.`) 更改为双冒号 (`::`)。
- [**移植语言过程**](#changes-that-involve-procedures-within-the-language)。 其中的一些可能是简单、重复的更改（例如 `myObject.MyProperty` 到 `myObject.MyProperty()`）。 其他移植则需要更深层次的更改（例如，将涉及使用 **System.Text.StringBuilder** 的过程迁移到涉及使用 **std::wostringstream** 的过程）。
- [**与移植相关且特定于 C++/WinRT 的任务**](#porting-related-tasks-that-are-specific-to-cwinrt)。 Windows 运行时的某些详细信息会在幕后由 C# 隐式处理。 这些详细信息在 C++/WinRT 中是显式处理的。 例如，使用 `.idl` 文件来定义运行时类。

本主题的其余部分根据该分类进行组织。

## <a name="changes-that-involve-the-language-projection"></a>涉及语言投影的更改

||C#|C++/WinRT|另请参阅|
|-|-|-|-|
|非类型化对象|`object` 或 [**System.Object**](/dotnet/api/system.object)|[**Windows::Foundation::IInspectable**](/windows/win32/api/inspectable/nn-inspectable-iinspectable)|[移植 **EnableClipboardContentChangedNotifications** 方法](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#enableclipboardcontentchangednotifications)|
|投影命名空间|`using System;`|`using namespace Windows::Foundation;`||
||`using System.Collections.Generic;`|`using namespace Windows::Foundation::Collections;`||
|集合大小|`collection.Count`|`collection.Size()`|[移植 **BuildClipboardFormatsOutputString** 方法](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#buildclipboardformatsoutputstring)|
|典型集合类型|[IList\<T\>](/dotnet/api/system.collections.generic.ilist-1)，以及用用于添加元素的 Add 。|[IVector\<T\>](/uwp/api/windows.foundation.collections.ivector-1)，以及用于添加元素的 Append 。 如果在任意位置使用 std::vector，则通过 push_back 添加元素 。||
|只读集合类型|[**IReadOnlyList\<T\>** ](/dotnet/api/system.collections.generic.ireadonlylist-1)|[**IVectorView\<T\>** ](/uwp/api/windows.foundation.collections.ivectorview-1)|[移植 **BuildClipboardFormatsOutputString** 方法](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#buildclipboardformatsoutputstring)|
|作为类成员的事件处理程序委托|`myObject.EventName += Handler;`|`token = myObject.EventName({ get_weak(), &Class::Handler });`|[移植 **EnableClipboardContentChangedNotifications** 方法](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#enableclipboardcontentchangednotifications)|
|撤销事件处理程序委托|`myObject.EventName -= Handler;`|`myObject.EventName(token);`|[移植 **EnableClipboardContentChangedNotifications** 方法](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#enableclipboardcontentchangednotifications)|
|关联容器|[**IDictionary\<K, V\>** ](/dotnet/api/system.collections.generic.idictionary-2)|[**IMap\<K, V\>** ](/uwp/api/windows.foundation.collections.imap-2)||
|矢量成员访问|`x = v[i];`<br>`v[i] = x;`|`x = v.GetAt(i);`<br>`v.SetAt(i, x);`||

### <a name="registerrevoke-an-event-handler"></a>注册/撤销事件处理程序

在 C++/WinRT 中，可以使用多个语法选项注册/撤销事件处理程序委托，如[在 C++/WinRT 中使用委托处理事件](/windows/uwp/cpp-and-winrt-apis/handle-events)中所述。 另请参阅[移植 **EnableClipboardContentChangedNotifications** 方法](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#enableclipboardcontentchangednotifications)。

有时，例如，当事件接收方（处理事件的对象）即将销毁时，你需要撤销事件处理程序，以使事件源（引发事件的对象）不会调用已销毁的对象。 请参阅[撤销已注册的委托](/windows/uwp/cpp-and-winrt-apis/handle-events#revoke-a-registered-delegate)。 在此类情况下，请为事件处理程序创建一个 **event_token** 成员变量。 有关示例，请参阅[移植 **EnableClipboardContentChangedNotifications** 方法](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#enableclipboardcontentchangednotifications)。

还可以使用 XAML 标记注册事件处理程序。

```xaml
<Button x:Name="OpenButton" Click="OpenButton_Click" />
```

在 C# 中，**OpenButton_Click** 方法可以是专用的，但 XAML 仍然能够将它连接到 *OpenButton* 引发的 [**ButtonBase.Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) 事件。

在 C++/WinRT 中，**OpenButton_Click** 方法在你的[实现类型](/windows/uwp/cpp-and-winrt-apis/author-apis)中必须是公共的（如果你想要使用 XAML 标记注册该方法）。 如果只在命令性代码中注册事件处理程序，则该事件处理程序不需要是公共的。

```cppwinrt
namespace winrt::MyProject::implementation
{
    struct MyPage : MyPageT<MyPage>
    {
        void OpenButton_Click(
            winrt::Windows:Foundation::IInspectable const& sender,
            winrt::Windows::UI::Xaml::RoutedEventArgs const& args);
    }
};
```

也可将注册 XAML 页设置为你的实现类型的友元，并将 **OpenButton_Click** 设置为专用。

```cppwinrt
namespace winrt::MyProject::implementation
{
    struct MyPage : MyPageT<MyPage>
    {
    private:
        friend MyPageT;
        void OpenButton_Click(
            winrt::Windows:Foundation::IInspectable const& sender,
            winrt::Windows::UI::Xaml::RoutedEventArgs const& args);
    }
};
```

最后一种方案是你要移植的 C# 项目绑定到标记中的事件处理程序（有关该方案的更多背景信息，请参阅 [x:Bind 中的函数](/windows/uwp/data-binding/function-bindings)）。

```xaml
<Button x:Name="OpenButton" Click="{x:Bind OpenButton_Click}" />
```

只需将该标记更改为更简单的 `Click="OpenButton_Click"` 即可。 如果希望，也可将该标记保留原样。 要支持此操作，你只需在 IDL 中声明事件处理程序即可。

```idl
void OpenButton_Click(Object sender, Windows.UI.Xaml.RoutedEventArgs e);
```

> [!NOTE]
> 将函数声明为 `void`，即使你将它作为[发后不理](/windows/uwp/cpp-and-winrt-apis/concurrency-2#fire-and-forget) (Fire and forget) 来实现也是如此。

## <a name="changes-that-involve-the-language-syntax"></a>涉及语言语法的更改

||C#|C++/WinRT|另请参阅|
|-|-|-|-|
|访问修饰符|`public \<member\>`|`public:`<br>&nbsp;&nbsp;&nbsp;&nbsp;`\<member\>`|[移植 **Button_Click** 方法](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#button_click)|
|访问数据成员|`this.variable`|`this->variable`||
|异步行为|`async Task ...`|`IAsyncAction ...`||
|异步操作|`async Task<T> ...`|`IAsyncOperation<T> ...`||
|“发后不理”方法（意味着异步）|`async void ...`|`winrt::fire_and_forget ...`|[移植 **CopyButton_Click** 方法](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#copybutton_click)|
|访问枚举常量|`E.Value`|`E::Value`|[移植 **DisplayChangedFormats** 方法](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#displaychangedformats)|
|配合地等待|`await ...`|`co_await ...`|[移植 **CopyButton_Click** 方法](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#copybutton_click)|
|作为私有字段的投影类型集合|`private List<MyRuntimeClass> myRuntimeClasses = new List<MyRuntimeClass>();`|`std::vector`<br>`<MyNamespace::MyRuntimeClass>`<br>`m_myRuntimeClasses;`||
|GUID 构造|`private static readonly Guid myGuid = new Guid("C380465D-2271-428C-9B83-ECEA3B4A85C1");`|`winrt::guid myGuid{ 0xC380465D, 0x2271, 0x428C, { 0x9B, 0x83, 0xEC, 0xEA, 0x3B, 0x4A, 0x85, 0xC1} };`||
|命名空间分隔符|`A.B.T`|`A::B::T`||
|Null|`null`|`nullptr`|[移植 **UpdateStatus** 方法](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#updatestatus)|
|获取类型对象|`typeof(MyType)`|`winrt::xaml_typename<MyType>()`|[移植 **Scenarios** 属性](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#scenarios)|
|方法的参数声明|`MyType`|`MyType const&`|[参数传递](/windows/uwp/cpp-and-winrt-apis/concurrency#parameter-passing)|
|异步方法的参数声明|`MyType`|`MyType`|[参数传递](/windows/uwp/cpp-and-winrt-apis/concurrency#parameter-passing)|
|调用静态方法|`T.Method()`|`T::Method()`||
|字符串|`string` 或 **System.String**|[winrt::hstring](/uw/cpp-ref-for-winrt/hstring)|[C++/WinRT 中的字符串处理](/windows/uwp/cpp-and-winrt-apis/strings)|
|字符串文本|`"a string literal"`|`L"a string literal"`|[移植构造函数 **Current** 和 **FEATURE_NAME**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#the-constructor-current-and-feature_name)|
|推断（或推导）的类型|`var`|`auto`|[移植 **BuildClipboardFormatsOutputString** 方法](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#buildclipboardformatsoutputstring)|
|Using 指令|`using A.B.C;`|`using namespace A::B::C;`|[移植构造函数 **Current** 和 **FEATURE_NAME**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#the-constructor-current-and-feature_name)|
|原义/原始字符串文本|`@"verbatim string literal"`|`LR"(raw string literal)"`|[移植 **DisplayToast** 方法](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp##displaytoast)|

> [!NOTE]
> 如果头文件没有包含用于给定命名空间的 `using namespace` 指令，则必须完全限定该命名空间的所有类型名称；或者至少对它们进行充分限定，以使编译器可以找到它们。 有关示例，请参阅[移植 **DisplayToast** 方法](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp##displaytoast)。

### <a name="porting-classes-and-members"></a>移植类和成员

对于每种 C# 类型，都需要确定是将它移植到 Windows 运行时类型，还是移植到常规 C++ 类/结构/枚举。 有关详细信息以及演示如何做出这些决策的详细示例，请参阅[将 Clipboard 示例从 C# 移植到 C++/WinRT](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp)。

一个 C# 属性通常会成为一个访问器函数、一个赋值函数和一个支持数据成员。 有关详细信息和示例，请参阅[移植 **IsClipboardContentChangedEnabled** 属性](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#isclipboardcontentchangedenabled)。

对于非静态字段，请使其成为你的[实现类型](/windows/uwp/cpp-and-winrt-apis/author-apis)的数据成员。

C# 静态字段会成为 C++/WinRT 静态访问器和/或赋值函数。 有关详细信息和示例，请参阅[移植构造函数 **Current** 和 **FEATURE_NAME**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#the-constructor-current-and-feature_name)。

对于成员函数，你也需要为每个成员函数决定其是否属于 IDL，或者它是实现类型的公共成员函数还是私有成员函数。 有关详细信息以及如何决定的示例，请参阅 [**MainPage** 类型的 IDL](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#idl-for-the-mainpage-type)。

### <a name="porting-xaml-markup-and-asset-files"></a>移植 XAML 标记和资产文件

在[将 Clipboard 示例从 C# 移植到 C++/WinRT](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp)的案例中，我们能够在 C# 和 C++/WINRT 项目中使用相同的 XAML 标记（包括资源）和资产文件。 在某些情况下，需要编辑标记才能实现此目的。 请参阅[复制完成移植 **MainPage** 所需的 XAML 和样式](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#copy-the-xaml-and-styles-necessary-to-finish-up-porting-mainpage)。

## <a name="changes-that-involve-procedures-within-the-language"></a>涉及语言内过程的更改

||C#|C++/WinRT|另请参阅|
|-|-|-|-|
|异步方法中的生存期管理|N/A|`auto lifetime{ get_strong() };` 或<br>`auto lifetime = get_strong();`|[移植 **CopyButton_Click** 方法](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#copybutton_click)|
|处置|`using (var t = v)`|`auto t{ v };`<br>`t.Close(); // or let wrapper destructor do the work`|[移植 **CopyImage** 方法](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#copyimage)|
|构造对象|`new MyType(args)`|`MyType{ args }` 或<br>`MyType(args)`|[移植 **Scenarios** 属性](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#scenarios)|
|创建未初始化的引用|`MyType myObject;`|`MyType myObject{ nullptr };` 或<br>`MyType myObject = nullptr;`|[移植构造函数 **Current** 和 **FEATURE_NAME**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#the-constructor-current-and-feature_name)|
|使用参数将对象构造到变量中|`var myObject = new MyType(args);`|`auto myObject{ MyType{ args } };` 或 <br>`auto myObject{ MyType(args) };` 或 <br>`auto myObject = MyType{ args };` 或 <br>`auto myObject = MyType(args);` 或 <br>`MyType myObject{ args };` 或 <br>`MyType myObject(args);`|[移植 **Footer_Click** 方法](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#footer_click)|
|在不使用参数的情况下将对象构造到变量中|`var myObject = new T();`|`MyType myObject;`|[移植 **BuildClipboardFormatsOutputString** 方法](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#buildclipboardformatsoutputstring)|
|对象初始化速记|`var p = new FileOpenPicker{`<br>&nbsp;&nbsp;&nbsp;&nbsp;`ViewMode = PickerViewMode.List`<br>`};`|`FileOpenPicker p;`<br>`p.ViewMode(PickerViewMode::List);`||
|批量矢量操作|`var p = new FileOpenPicker{`<br>&nbsp;&nbsp;&nbsp;&nbsp;`FileTypeFilter = { ".png", ".jpg", ".gif" }`<br>`};`|`FileOpenPicker p;`<br>`p.FileTypeFilter().ReplaceAll({ L".png", L".jpg", L".gif" });`|[移植 **CopyButton_Click** 方法](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#copybutton_click)|
|循环访问集合|`foreach (var v in c)`|`for (auto&& v : c)`|[移植 **BuildClipboardFormatsOutputString** 方法](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#buildclipboardformatsoutputstring)|
|捕获异常|`catch (Exception ex)`|`catch (winrt::hresult_error const& ex)`|[移植 **PasteButton_Click** 方法](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#pastebutton_click)|
|异常详细信息|`ex.Message`|`ex.message()`|[移植 **PasteButton_Click** 方法](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#pastebutton_click)|
|获取属性值|`myObject.MyProperty`|`myObject.MyProperty()`|[移植 **NotifyUser** 方法](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#notifyuser)|
|设置属性值|`myObject.MyProperty = value;`|`myObject.MyProperty(value);`||
|递增属性值|`myObject.MyProperty += v;`|`myObject.MyProperty(thing.Property() + v);`<br>对于字符串，请切换到生成器||
|ToString()|`myObject.ToString()`|`winrt::to_hstring(myObject)`|[ToString()](#tostring)|
|语言字符串到 Windows 运行时字符串|N/A|`winrt::hstring{ s }`||
|字符串生成|`StringBuilder builder;`<br>`builder.Append(...);`|`std::wostringstream builder;`<br>`builder << ...;`|[字符串生成](#string-building)|
|字符串内插|`$"{i++}) {s.Title}"`|[**winrt::to_hstring**](/uwp/cpp-ref-for-winrt/to-hstring) 和/或 [**winrt::hstring::operator+** ](/uwp/cpp-ref-for-winrt/hstring#operator-concatenation-operator)|[移植 **OnNavigatedTo** 方法](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#onnavigatedto)|
|用于比较的空字符串|**System.String.Empty**|[**winrt::hstring::empty**](/uwp/cpp-ref-for-winrt/hstring#hstringempty-function)|[移植 **UpdateStatus** 方法](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#updatestatus)|
|创建空字符串|`var myEmptyString = String.Empty;`|`winrt::hstring myEmptyString{ L"" };`||
|字典操作|`map[k] = v; // replaces any existing`<br>`v = map[k]; // throws if not present`<br>`map.ContainsKey(k)`|`map.Insert(k, v); // replaces any existing`<br>`v = map.Lookup(k); // throws if not present`<br>`map.HasKey(k)`||
|类型转换（失败时引发）|`(MyType)v`|`v.as<MyType>()`|[移植 **Footer_Click** 方法](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#footer_click)|
|类型转换（失败时为 null）|`v as MyType`|`v.try_as<MyType>()`|[移植 **PasteButton_Click** 方法](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#pastebutton_click)|
|具有 x:Name 的 XAML 元素是属性|`MyNamedElement`|`MyNamedElement()`|[移植构造函数 **Current** 和 **FEATURE_NAME**](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#the-constructor-current-and-feature_name)|
|切换到 UI 线程|**CoreDispatcher.RunAsync**|**CoreDispatcher.RunAsync** 或 [**winrt::resume_foreground**](/uwp/cpp-ref-for-winrt/resume-foreground)|[移植 **NotifyUser** 方法](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#notifyuser)以及[移植 **HistoryAndRoaming** 方法](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#historyandroaming)|
|XAML 页的强制性代码中的 UI 元素构造|请参阅 [UI 元素构造](#ui-element-construction)|请参阅 [UI 元素构造](#ui-element-construction)||

以下部分更详细地介绍了该表中的某些项。

### <a name="ui-element-construction"></a>UI 元素构造

这些代码示例演示了 XAML 页面的强制性代码中的 UI 元素构造。

```csharp
var myTextBlock = new TextBlock()
{
    Text = "Text",
    Style = (Windows.UI.Xaml.Style)this.Resources["MyTextBlockStyle"]
};
```

```cppwinrt
TextBlock myTextBlock;
myTextBlock.Text(L"Text");
myTextBlock.Style(
    winrt::unbox_value<Windows::UI::Xaml::Style>(
        Resources().Lookup(
            winrt::box_value(L"MyTextBlockStyle")
        )
    )
);
```

### <a name="tostring"></a>ToString()

C# 类型提供 [Object.ToString](/dotnet/api/system.object.tostring) 方法。

```csharp
int i = 2;
var s = i.ToString(); // s is a System.String with value "2".
```

C++/ WinRT 不直接提供此工具，不过可以转为使用替代方法。

```cppwinrt
int i{ 2 };
auto s{ std::to_wstring(i) }; // s is a std::wstring with value L"2".
```

C++/WinRT 也支持 [**winrt::to_hstring**](/uwp/cpp-ref-for-winrt/to-hstring)，但仅限数目有限的一些类型。 对于任何其他需要字符串化的类型，你需要添加重载。

| Language | 将整数字符串化 | 将枚举字符串化 |
| - | - | - |
| C# | `string result = "hello, " + intValue.ToString();`<br>`string result = $"hello, {intValue}";` | `string result = "status: " + status.ToString();`<br>`string result = $"status: {status}";` |
| C++/WinRT | `hstring result = L"hello, " + to_hstring(intValue);` | `// must define overload (see below)`<br>`hstring result = L"status: " + to_hstring(status);` |

如果将枚举字符串化，则需提供 **winrt::to_hstring** 的实现。

```cppwinrt
namespace winrt
{
    hstring to_hstring(StatusEnum status)
    {
        switch (status)
        {
        case StatusEnum::Success: return L"Success";
        case StatusEnum::AccessDenied: return L"AccessDenied";
        case StatusEnum::DisabledByPolicy: return L"DisabledByPolicy";
        default: return to_hstring(static_cast<int>(status));
        }
    }
}
```

这些字符串化通常通过数据绑定来隐式使用。

```xaml
<TextBlock>
You have <Run Text="{Binding FlowerCount}"/> flowers.
</TextBlock>
<TextBlock>
Most recent status is <Run Text="{x:Bind LatestOperation.Status}"/>.
</TextBlock>
```

这些绑定会对被绑定属性执行 **winrt::to_hstring**。 至于第二个示例 (**StatusEnum**)，则必须提供你自己的 **winrt::to_hstring** 重载，否则会出现编译器错误。

另请参阅[移植 **Footer_Click** 方法](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#footer_click)。

### <a name="string-building"></a>字符串生成

C# 有一个内置的 [**StringBuilder**](/dotnet/api/system.text.stringbuilder) 类型，用于字符串生成。

| | C# | C++/WinRT |
|-|-|-|
| 字符串生成 | `StringBuilder builder;`<br>`builder.Append(...);` | `std::wostringstream builder;`<br>`builder << ...;` |
| 追加 Windows 运行时字符串（保留 null 值） | `builder.Append(s);` | `builder << std::wstring_view{ s };` |
| 添加新行 |`builder.Append(Environment.NewLine);` | `builder << std::endl;` |
| 访问结果 | `s = builder.ToString();` | `ws = builder.str();` |

另请参阅[移植 **BuildClipboardFormatsOutputString** 方法](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#buildclipboardformatsoutputstring)和[移植 **DisplayChangedFormats** 方法](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#displaychangedformats)。

### <a name="running-code-on-the-main-ui-thread"></a>在主 UI 线程上运行代码 

此示例取自[条形码扫描仪示例](/samples/microsoft/windows-universal-samples/barcodescanner/)。

要在 C# 项目的主 UI 线程上工作时，通常使用 [CoreDispatcher.RunAsync ](/uwp/api/windows.ui.core.coredispatcher.runasync) 方法，如下所示。

```csharp
private async void Watcher_Added(DeviceWatcher sender, DeviceInformation args)
{
    await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
        // Do work on the main UI thread here.
    });
}
```

更简单的是用 C++/WinRT 来表示。 请注意，假设我们希望在第一个暂停点（在本例中是 `co_await`）之后访问参数时，我们将按值接受参数。 有关详细信息，请参阅[参数传递](/windows/uwp/cpp-and-winrt-apis/concurrency#parameter-passing)。

```cppwinrt
winrt::fire_and_forget Watcher_Added(DeviceWatcher sender, winrt::DeviceInformation args)
{
    co_await Dispatcher();
    // Do work on the main UI thread here.
}
```

如果需要以默认优先级以外的优先级执行工作，请参阅 [winrt::resume_foreground](/uwp/cpp-ref-for-winrt/resume-foreground) 函数，该函数包含需要优先处理的重载。 有关演示如何等待调用 winrt::resume_foreground 的代码示例，请参阅[编程时仔细考虑线程相关性](/windows/uwp/cpp-and-winrt-apis/concurrency-2#programming-with-thread-affinity-in-mind)。

## <a name="porting-related-tasks-that-are-specific-to-cwinrt"></a>与移植相关且特定于 C++/WinRT 的任务

### <a name="define-your-runtime-classes-in-idl"></a>在 IDL 中定义运行时类

请参阅 [**MainPage** 类型的 IDL](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#idl-for-the-mainpage-type) 和[合并 `.idl` 文件](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#consolidate-your-idl-files)。

### <a name="include-the-cwinrt-windows-namespace-header-files-that-you-need"></a>包含所需的 C++/WinRT Windows 命名空间头文件

在 C++/WinRT 中，只要你希望使用来自 Windows 命名空间的类型，都需要包含对应的 C++/WinRT Windows 命名空间头文件。 有关示例，请参阅[移植 **NotifyUser** 方法](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#notifyuser)。

### <a name="boxing-and-unboxing"></a>装箱和取消装箱

C# 自动将标量装箱到对象中。 C++/WinRT 要求你显式调用 [**winrt::box_value**](/uwp/cpp-ref-for-winrt/box-value) 函数。 两种语言都要求你以显式方式取消装箱。 请参阅[使用 C++/WinRT 装箱和取消装箱](/windows/uwp/cpp-and-winrt-apis/boxing)。

在下面的表中，我们将使用这些定义。

| C# | C++/WinRT|
|-|-|
| `int i;` | `int i;` |
| `string s;` | `winrt::hstring s;` |
| `object o;` | `IInspectable o;`|

| 操作 | C# | C++/WinRT|
|-|-|-|
| 装箱 | `o = 1;`<br>`o = "string";` | `o = box_value(1);`<br>`o = box_value(L"string");` |
| 取消装箱 | `i = (int)o;`<br>`s = (string)o;` | `i = unbox_value<int>(o);`<br>`s = unbox_value<winrt::hstring>(o);` |

如果尝试取消值类型的 null 指针的装箱，C++/CX 和 C# 会引发异常。 C++/WinRT 将其视为编程错误，因此会崩溃。 在 C++/WinRT 中，请使用 [**winrt::unbox_value_or**](/uwp/cpp-ref-for-winrt/unbox-value-or) 函数来处理对象类型不符合预期的情况。

| 方案 | C# | C++/WinRT|
|-|-|-|
| 取消已知整数的装箱 |`i = (int)o;` | `i = unbox_value<int>(o);` |
| 如果 o 为 null | `System.NullReferenceException` | 崩溃 |
| 如果 o 不是装箱的整数 | `System.InvalidCastException` | 崩溃 |
| 取消整数的装箱，在为 null 的情况下使用回退；任何其他情况则崩溃 | `i = o != null ? (int)o : fallback;` | `i = o ? unbox_value<int>(o) : fallback;` |
| 尽可能取消整数的装箱；在任何其他情况下使用回退 | `i = as int? ?? fallback;` | `i = unbox_value_or<int>(o, fallback);` |

有关示例，请参阅[移植 **OnNavigatedTo** 方法](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#onnavigatedto)和[移植 **Footer_Click** 方法](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#footer_click)。

#### <a name="boxing-and-unboxing-a-string"></a>将字符串装箱和取消装箱

字符串在某些情况下是值类型，在另一些情况下是引用类型。 C# 和 C++/WinRT 对待字符串的方式有所不同。

ABI 类型 [**HSTRING**](/windows/win32/winrt/hstring) 是一个指向引用计数字符串的指针。 但是，它并非派生自 [**IInspectable**](/windows/win32/api/inspectable/nn-inspectable-iinspectable)，因此从技术上来说它不是一个对象。 另外，null **HSTRING** 表示空字符串。 将并非派生自 IInspectable 的项装箱时，需将其包装到 [IReference\<T\>](/uwp/api/windows.foundation.ireference_t_) 中，而 Windows 运行时会以 [PropertyValue](/uwp/api/windows.foundation.propertyvalue) 对象的形式提供标准实现（自定义类型以 [PropertyType::OtherType](/uwp/api/windows.foundation.propertytype) 形式报告）   。

C# 将 Windows 运行时字符串表示为引用类型，而 C++/WinRT 则将字符串投影为值类型。 这意味着装箱的 null 字符串可能有不同的表示形式，具体取决于你所采用的方法。

| 行为 | C# | C++/WinRT|
|-|-|-|
| 声明 | `object o;`<br>`string s;` | `IInspectable o;`<br>`hstring s;` |
| 字符串类型类别 | 引用类型 | 值类型 |
| null **HSTRING** 投影方式 | `""` | `hstring{}` |
| null 和 `""` 是否相同？ | 否 | 是 |
| null 的有效性 | `s = null;`<br>`s.Length` 引发 NullReferenceException | `s = hstring{};`<br>`s.size() == 0`（有效） |
| 如果将 null 字符串分配给对象 | `o = (string)null;`<br>`o == null` | `o = box_value(hstring{});`<br>`o != nullptr` |
| 如果将 `""` 分配给对象 | `o = "";`<br>`o != null` | `o = box_value(hstring{L""});`<br>`o != nullptr` |

基本装箱和取消装箱。

| 操作 | C# | C++/WinRT|
|-|-|-|
| 将字符串装箱 | `o = s;`<br>空字符串变为非 null 对象。 | `o = box_value(s);`<br>空字符串变为非 null 对象。 |
| 取消已知字符串的装箱 | `s = (string)o;`<br>Null 对象变为 null 字符串。<br>如果不是字符串，则引发 InvalidCastException。 | `s = unbox_value<hstring>(o);`<br>Null 对象崩溃。<br>如果不是字符串，则崩溃。 |
| 将可能的字符串取消装箱 | `s = o as string;`<br>Null 对象或非字符串变为 null 字符串。<br><br>或者<br><br>`s = o as string ?? fallback;`<br>Null 或非字符串变为 fallback。<br>空字符串被保留。 | `s = unbox_value_or<hstring>(o, fallback);`<br>Null 或非字符串变为 fallback。<br>空字符串被保留。 |

### <a name="making-a-class-available-to-the-binding-markup-extension"></a>使类可供 {Binding} 标记扩展使用

若要使用 {Binding} 标记扩展将数据绑定到数据类型，则请参阅[使用 {Binding} 绑定声明的对象](/windows/uwp/data-binding/data-binding-in-depth#binding-object-declared-using-binding)。

### <a name="consuming-objects-from-xaml-markup"></a>使用 XAML 标记中的对象

在 C# 项目中，可以使用 XAML 标记中的专用成员和命名元素。 但在 C++/WinRT 中，以 XAML [ **{x:Bind} 标记扩展**](/windows/uwp/xaml-platform/x-bind-markup-extension)形式使用的所有实体必须在 IDL 中以公开方式公开。

另外，绑定到布尔值时，在 C# 中会显示 `true` 或 `false`，但在 C++/WinRT 中会显示 Windows.Foundation.IReference`1\<Boolean\>。

有关详细信息和代码示例，请参阅[使用标记中的对象](/windows/uwp/cpp-and-winrt-apis/binding-property#consuming-objects-from-xaml-markup)。

### <a name="making-a-data-source-available-to-xaml-markup"></a>使数据源可供 XAML 标记使用

在 C++/WinRT 2.0.190530.8 及更高版本中，[winrt::single_threaded_observable_vector](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector) 创建一个可观测的支持 [IObservableVector](/uwp/api/windows.foundation.collections.iobservablevector_t_)\<T\> 和 IObservableVector\<IInspectable\> 的矢量  。 有关示例，请参阅 [移植 **Scenarios** 属性](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp#scenarios)。

你可以创作 **Midl 文件 (.idl)** ，如下所示（另请参阅[将运行时类重构到 Midl 文件 (.idl) 中](/windows/uwp/cpp-and-winrt-apis/author-apis#factoring-runtime-classes-into-midl-files-idl)）。

```idl
namespace Bookstore
{
    runtimeclass BookSku { ... }

    runtimeclass BookstoreViewModel
    {
        Windows.Foundation.Collections.IObservableVector<BookSku> BookSkus{ get; };
    }

    runtimeclass MainPage : Windows.UI.Xaml.Controls.Page
    {
        MainPage();
        BookstoreViewModel MainViewModel{ get; };
    }
}
```

实现方式如下。

```cppwinrt
// BookstoreViewModel.h
...
struct BookstoreViewModel : BookstoreViewModelT<BookstoreViewModel>
{
    BookstoreViewModel()
    {
        m_bookSkus = winrt::single_threaded_observable_vector<Bookstore::BookSku>();
        m_bookSkus.Append(winrt::make<Bookstore::implementation::BookSku>(L"To Kill A Mockingbird"));
    }
    
    Windows::Foundation::Collections::IObservableVector<Bookstore::BookSku> BookSkus();
    {
        return m_bookSkus;
    }

private:
    Windows::Foundation::Collections::IObservableVector<Bookstore::BookSku> m_bookSkus;
};
...
```

有关详细信息，请参阅 [XAML 项目控件；绑定到 C++/WinRT 集合](/windows/uwp/cpp-and-winrt-apis/binding-collection)和[使用 C++/WinRT 的集合](/windows/uwp/cpp-and-winrt-apis/collections)。

### <a name="making-a-data-source-available-to-xaml-markup-prior-to-cwinrt-201905308"></a>使数据源可供 XAML 标记使用（在 C++/WinRT 2.0.190530.8 之前）

XAML 数据绑定要求项源实现 [IIterable](/uwp/api/windows.foundation.collections.iiterable_t_)\<IInspectable\> 以及下述接口组合之一。

- **IObservableVector\<IInspectable\>**
- **IBindableVector** 和 **INotifyCollectionChanged**
- **IBindableVector** 和 **IBindableObservableVector**
- **IBindableVector** 本身（不会响应更改）
- **IVector\<IInspectable\>**
- **IBindableIterable**（会通过迭代将元素保存到专用集合中）

无法在运行时检测到泛型接口（例如 IVector\<T\>）。 每个 IVector\<T\> 都有不同的接口标识符 (IID)，它是 T 的函数 。任何开发人员都可以随意扩展 **T** 集，因此，很明显 XAML 绑定代码不可能知道要查询的完整集。 该限制对 C# 来说不成问题，因为每个实现 IEnumerable\<T\> 的 CLR 对象都会自动实现 IEnumerable 。 在 ABI 级别，这意味着每个实现 IObservableVector\<T\> 的对象都会自动实现 IObservableVector\<IInspectable\> 。

C++/WinRT 不提供该保证。 如果 C++/WinRT 运行时类会实现 IObservableVector\<T\>则我们无法假定还将以某种方式提供 IObservableVector\<IInspectable\> 的实现 。

因此，上一示例应如下所示。

```idl
...
runtimeclass BookstoreViewModel
{
    // This is really an observable vector of BookSku.
    Windows.Foundation.Collections.IObservableVector<Object> BookSkus{ get; };
}
```

实现如下。

```cppwinrt
// BookstoreViewModel.h
...
struct BookstoreViewModel : BookstoreViewModelT<BookstoreViewModel>
{
    BookstoreViewModel()
    {
        m_bookSkus = winrt::single_threaded_observable_vector<Windows::Foundation::IInspectable>();
        m_bookSkus.Append(winrt::make<Bookstore::implementation::BookSku>(L"To Kill A Mockingbird"));
    }
    
    // This is really an observable vector of BookSku.
    Windows::Foundation::Collections::IObservableVector<Windows::Foundation::IInspectable> BookSkus();
    {
        return m_bookSkus;
    }

private:
    Windows::Foundation::Collections::IObservableVector<Windows::Foundation::IInspectable> m_bookSkus;
};
...
```

如需访问 *m_bookSkus* 中的对象，则需将其 QI 回 **Bookstore::BookSku**。

```cppwinrt
Widget MyPage::BookstoreViewModel(winrt::hstring title)
{
    for (auto&& obj : m_bookSkus)
    {
        auto bookSku = obj.as<Bookstore::BookSku>();
        if (bookSku.Title() == title) return bookSku;
    }
    return nullptr;
}
```

### <a name="derived-classes"></a>派生类

若要从运行时类派生，基类必须是可组合类。 C# 不需要你执行任何特殊步骤即可将类变为可组合类，但 C++/WinRT 需要。 请使用 [unsealed 关键字](/uwp/midl-3/intro#base-classes)来指示你希望将类用作基类。

```idl
unsealed runtimeclass BasePage : Windows.UI.Xaml.Controls.Page
{
    ...
}
runtimeclass DerivedPage : BasePage
{
    ...
}
```

在[实现类型](/windows/uwp/cpp-and-winrt-apis/author-apis)的头文件中，必须先包含基类头文件，然后再包含为派生类自动生成的头文件。 否则会出现“将此类型用作表达式非法”之类的错误。

```cppwinrt
// DerivedPage.h
#include "BasePage.h"       // This comes first.
#include "DerivedPage.g.h"  // Otherwise this header file will produce an error.

namespace winrt::MyNamespace::implementation
{
    struct DerivedPage : DerivedPageT<DerivedPage>
    {
        ...
    }
}
```

## <a name="important-apis"></a>重要的 API
* [winrt 命名空间](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>相关主题
* [C# 教程](/visualstudio/get-started/csharp)
* [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/)
* [深入了解数据绑定](/windows/uwp/data-binding/data-binding-in-depth)