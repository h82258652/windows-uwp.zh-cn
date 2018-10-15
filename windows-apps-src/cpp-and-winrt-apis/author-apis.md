---
author: stevewhims
description: 本主题介绍如何直接或间接使用 **winrt::implements** 基结构来创作 C++/WinRT API。
title: 使用 C++/WinRT 创作 API
ms.author: stwhi
ms.date: 10/03/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 标准, c++, cpp, winrt, 投影的, 投影, 实现, 运行时类, 激活
ms.localizationpriority: medium
ms.openlocfilehash: 2476161954c1d4d49fcf9f8f74cd1b7cf9180c0a
ms.sourcegitcommit: 106aec1e59ba41aae2ac00f909b81bf7121a6ef1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/15/2018
ms.locfileid: "4610961"
---
# <a name="author-apis-with-cwinrt"></a>使用 C++/WinRT 创作 API

本主题演示如何创作[C + + WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)通过使用[**winrt:: implements**](/uwp/cpp-ref-for-winrt/implements) Api 直接或间接基结构。 在此上下文中，*创作*的同义词有*生成*或*实现*。 本主题介绍以下在 C++/WinRT 类型上实现 API 的情形（按此顺序）。

- 你*不是*在创作一个 Windows 运行时类（运行时类）；你只是想要实现一个或多个 Windows 运行时接口以便在应用内进行本地使用。 在此案例中，你直接从 **winrt::implements** 派生并实现相关函数。
- 你*正在*创作一个运行时类。 你可能正在创作一个要从某个应用中使用的组件。 或者，你可能正在创作一个要从 XAML 用户接口 (UI) 使用的类型，在此情况下，你在同一个编译单元内实现和使用一个运行时类。 在这些情况下，你使用工具为你生成派生自 **winrt::implements** 的类。

在这两种情况下，实现 C++/WinRT API 的类型被称作*实现类型*。

> [!IMPORTANT]
> 请务必区分实现类型与投影类型的概念。 投影类型将在[通过 C++/WinRT 使用 API](consume-apis.md) 中介绍。

## <a name="if-youre-not-authoring-a-runtime-class"></a>如果你*没有*创作运行时类
最简单的方案是你要实现一个 Windows 运行时接口以进行本地使用。 你不需要运行时类；只需一个普通的 C++ 类。 例如，你可能会基于 [**CoreApplication**](/uwp/api/windows.applicationmodel.core.coreapplication) 编写一个应用。

> [!NOTE]
> 有关 C++/WinRT Visual Studio Extension (VSIX)（提供项目模板支持以及 C++/WinRT MSBuild 属性和目标）的安装和使用的信息，请参阅[对 C++/WinRT 的 Visual Studio 支持以及 VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix)。

在 Visual Studio 中， **Visual c + +** > **Windows 通用** > **核心应用 (C + + WinRT)** 项目模板阐释**CoreApplication**模式。 该模式首先将 [**Windows::ApplicationModel::Core::IFrameworkViewSource**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource) 的实现传递给 [**CoreApplication::Run**](/uwp/api/windows.applicationmodel.core.coreapplication.run)。

```cppwinrt
using namespace Windows::ApplicationModel::Core;
int __stdcall wWinMain(HINSTANCE, HINSTANCE, PWSTR, int)
{
    IFrameworkViewSource source = ...
    CoreApplication::Run(source);
}
```

**CoreApplication** 使用接口来创建应用的第一个视图。 从概念上来说，**IFrameworkViewSource** 如下所示。

```cppwinrt
struct IFrameworkViewSource : IInspectable
{
    IFrameworkView CreateView();
};
```

同样，从概念上来说，**CoreApplication::Run** 的实现执行此操作。

```cppwinrt
void Run(IFrameworkViewSource viewSource) const
{
    IFrameworkView view = viewSource.CreateView();
    ...
}
```

因此，作为开发人员，应实现 **IFrameworkViewSource** 接口。 C++/WinRT 具有基结构模板 [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements)，从而可以很容易实现一个或多个接口，而不必求助于 COM 样式的编程。 你可以从 **implements** 直接派生类型，然后实现接口的函数。 操作方法如下。

```cppwinrt
// App.cpp
...
struct App : implements<App, IFrameworkViewSource>
{
    IFrameworkView CreateView()
    {
        return ...
    }
}
...
```

这将由 **IFrameworkViewSource** 处理。 下一步是返回实现 **IFrameworkView** 接口的对象。 你也可以选择在 **App** 上实现该接口。 下一个代码示例表示一个小型应用，该应用将至少会在桌面上运行一个窗口。

```cppwinrt
// App.cpp
...
struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    IFrameworkView CreateView()
    {
        return *this;
    }

    void Initialize(CoreApplicationView const &) {}

    void Load(hstring const&) {}

    void Run()
    {
        CoreWindow window = CoreWindow::GetForCurrentThread();
        window.Activate();

        CoreDispatcher dispatcher = window.Dispatcher();
        dispatcher.ProcessEvents(CoreProcessEventsOption::ProcessUntilQuit);
    }

    void SetWindow(CoreWindow const & window)
    {
        // Prepare app visuals here
    }

    void Uninitialize() {}
};
...
```

由于 **App** 类型*是* **IFrameworkViewSource**，因此你可以传递一个到 **Run**。

```cppwinrt
using namespace Windows::ApplicationModel::Core;
int __stdcall wWinMain(HINSTANCE, HINSTANCE, PWSTR, int)
{
    CoreApplication::Run(App{});
}
```

## <a name="if-youre-authoring-a-runtime-class-in-a-windows-runtime-component"></a>如果你正在 Windows 运行时组件中创作一个运行时类
如果类型打包在一个 Windows 运行时组件中以便从应用程序中使用，则它需要是一个运行时类。

> [!TIP]
> 建议通过各自的接口定义语言 (IDL) 文件 (`.idl`) 声明每个运行时类，以便在编辑 IDL 文件时优化生成性能，并使 IDL 文件能与其生成的源代码文件进行逻辑通信。 Visual Studio 将所有生成的 `.winmd` 文件合并为一个与根命名空间同名的文件。 最后生成的 `.winmd` 文件将是组件使用者将参考的文件。

下面是一个示例。

```idl
// MyRuntimeClass.idl
namespace MyProject
{
    runtimeclass MyRuntimeClass
    {
        // Declaring a constructor (or constructors) in the IDL causes the runtime class to be
        // activatable from outside the compilation unit.
        MyRuntimeClass();
        String Name;
    }
}
```

此 IDL 声明一个 Windows 运行时类。 运行时类是一个可通过现代 COM 接口进行激活和使用（通常跨可执行文件）的类型。 当你向项目和生成中添加 IDL 文件时，C++/WinRT 工具链（`midl.exe` 和 `cppwinrt.exe`）将会为你生成一个实现类型。 对于上面的示例 IDL，在名为 `\MyProject\MyProject\Generated Files\sources\MyRuntimeClass.h` 和 `MyRuntimeClass.cpp` 的源代码文件中，实现类型是一个名为 **winrt::MyProject::implementation::MyRuntimeClass** 的 C++ 结构存根。

该实现类型如下所示。

```cppwinrt
// MyRuntimeClass.h
...
namespace winrt::MyProject::implementation
{
    struct MyRuntimeClass : MyRuntimeClassT<MyRuntimeClass>
    {
        MyRuntimeClass() = default;

        hstring Name();
        void Name(hstring const& value);
    };
}

// winrt::MyProject::factory_implementation::MyRuntimeClass is here, too.
```

请注意所使用的 F 边界多态模式（**MyRuntimeClass** 使用自身作为其基类 **MyRuntimeClassT** 的模板参数）。 这也称为奇异递归模板模式 (CRTP)。 如果你向上查看继承链，你会发现 **MyRuntimeClass_base**。

```cppwinrt
template <typename D, typename... I>
struct MyRuntimeClass_base : implements<D, MyProject::IMyRuntimeClass, I...>
```

因此，在此情况中，继承层次结构的根同样是 [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) 基结构模板。

有关更多详细信息、代码以及在 Windows 运行时组件创作 API 的演练，请参阅[使用 C++/WinRT 创作事件](author-events.md#create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component)。

## <a name="if-youre-authoring-a-runtime-class-to-be-referenced-in-your-xaml-ui"></a>如果你正在创作要在 XAML UI 中引用的运行时类
如果类型将由 XAML UI 引用，则它需要是一个运行时类，即使它与 XAML 是在同一项目中。 尽管它们通常是跨可执行文件边界进行激活的，但在实现它的编译单元内可以改用一个运行时类。

在此情况下，你既创作 API *同时又*使用 API。 实现运行时类的过程与实现 Windows 运行时组件的过程实质上是相同的。 因此，[如果你正在 Windows 运行时组件中创作运行时类](#if-youre-authoring-a-runtime-class-in-a-windows-runtime-component)，请参阅上一节。 唯一不同的细节在于，从 IDL 中， C++/WinRT 工具链不仅生成一个实现类型，而且还生成一个投影类型。 在此情况下只说明“**MyRuntimeClass**”可能是不明确的，认识到这一点很重要，因为有多个具有该名称的不同类型的实体。

- **MyRuntimeClass** 是运行时类的名称。 但这实际上是一种抽象：在 IDL 中声明并以某种编程语言实现。
- **MyRuntimeClass** 是 c++ 结构 **winrt::MyProject::implementation::MyRuntimeClass**（运行时类的 C++/WinRT 实现）的名称。 正如我们看到的，如果有不同的实现和使用项目，则此结构仅存在于实现项目中。 这是*实现类型*或*实现*。 此类型由`cppwinrt.exe` 工具在文件 `\MyProject\MyProject\Generated Files\sources\MyRuntimeClass.h` 和 `MyRuntimeClass.cpp` 中生成。
- **MyRuntimeClass** 是采用 C++ 结构 **winrt::MyProject::MyRuntimeClass** 形式的投影类型的名称。 如果有不同的实现和使用项目，则此结构仅存在于使用项目中。 这是*投影类型* 或*投影*。 此类型由 `cppwinrt.exe` 在文件 `\MyProject\MyProject\Generated Files\winrt\impl\MyProject.2.h` 中生成。

![投影类型和实现类型](images/myruntimeclass.png)

下面是与本主题相关的投影类型部分。

```cppwinrt
// MyProject.2.h
...
namespace winrt::MyProject
{
    struct MyRuntimeClass : MyProject::IMyRuntimeClass
    {
        MyRuntimeClass(std::nullptr_t) noexcept {}
        MyRuntimeClass();
    };
}
```

有关在运行时类上实现 **INotifyPropertyChanged** 接口的演练的示例，请参阅 [XAML 控件; 绑定到 C++/WinRT 属性](binding-property.md)。

有关在此情况下使用运行时类的过程将在[通过 C++/WinRT 使用 API](consume-apis.md#if-the-api-is-implemented-in-the-consuming-project) 中介绍。

## <a name="runtime-class-constructors"></a>运行时类构造函数
关于我们在上面看到的列表，要记住下面这些要点。

- 你在 IDL 中声明的每个构造函数都会使得在实现类型和投影类型上生成一个构造函数。 将通过 IDL 声明的构造函数来使用*不同*编译单元中的运行时类。
- 无论你是否有 IDL 声明构造函数，在投影类型上都会生成一个接收 `nullptr_t` 的构造函数重载。 从*同一*编译单元中使用运行时类需要两个步骤，调用 `nullptr_t` 构造函数便是*第一步*。 有关更多详细信息和代码示例，请参阅[通过 C++/WinRT 使用 API](consume-apis.md#if-the-api-is-implemented-in-the-consuming-project)。
- 如果你正在从*同一*编译单元使用运行时类，则还可以在实现类型上（请记住，这是在 `MyRuntimeClass.h` 中）直接实现非默认构造函数。

> [!NOTE]
> 如果你预计运行时类将会从不同的编译单元使用（这很常见），则应在 IDL 中包括相应的构造函数（至少包括一个默认构造函数）。 这样一来，你在获得实现类型的同时还将获得一个工厂实现。
> 
> 如果你只想在同一编译单元内创作和使用运行时类，则不用在 IDL 中声明任何构造函数。 你不需要工厂实现，也不会生成它。 实现类型的默认构造函数将会被删除，但你可以轻松地编辑它并使之成为默认构造函数。
> 
> 如果你只想在同一编译单元内创作和使用运行时类，并且需要构造函数参数，则应直接在实现类型上创作你所需的构造函数。

## <a name="instantiating-and-returning-implementation-types-and-interfaces"></a>实例化和返回实现类型和接口
在这一节中，我们将以一个名为 **MyType** 的实现类型作为示例，该实现类型将实现 [**IStringable**](/uwp/api/windows.foundation.istringable) 和 [**IClosable**](/uwp/api/windows.foundation.iclosable) 接口。

你可以直接从 [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements)（这不是一个运行时类）派生 **MyType**。

```cppwinrt
#include <winrt/Windows.Foundation.h>

using namespace winrt;
using namespace Windows::Foundation;

struct MyType : implements<MyType, IStringable, IClosable>
{
    winrt::hstring ToString(){ ... }
    void Close(){}
};
```

或者，也可以从 IDL（这是一个运行时类）生成它。

```idl
// MyType.idl
namespace MyProject
{
    runtimeclass MyType: Windows.Foundation.IStringable, Windows.Foundation.IClosable
    {
        MyType();
    }    
}
```

若要从 **MyType** 获得你可以使用或作为投影的一部分返回的 **IStringable** 或 **IClosable** 对象，可以调用 [**winrt::make**](/uwp/cpp-ref-for-winrt/make) 函数模板。 **使**返回实现类型的默认接口。

```cppwinrt
IStringable istringable = winrt::make<MyType>();
```

> [!NOTE]
> 但是，如果你从 XAML UI 引用类型，则在同一个项目中将会有一个实现类型和一个投影类型。 在此情况下，**请**返回投影类型的实例。 有关此情况的代码示例，请参阅 [XAML 控件; 绑定到 C++/WinRT 属性](binding-property.md#add-a-property-of-type-bookstoreviewmodel-to-mainpage)。

我们仅可以使用 `istringable`（在上面的代码示例中）来调用 **IStringable** 接口的成员。 但 C++/WinRT 接口（这是投影接口）派生自 [**winrt::Windows::Foundation::IUnknown**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown)。 因此，你可以调用[**iunknown:: As**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) （或[**Try_as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntryas-function)） 对其查询其他投影的类型或接口，你也可使用或返回这些接口。

```cppwinrt
istringable.ToString();
IClosable iclosable = istringable.as<IClosable>();
iclosable.Close();
```

如果你需要访问实现的所有成员并向调用方返回一个，则应使用 [**winrt::make_self**](/uwp/cpp-ref-for-winrt/make-self) 函数模板。 **make_self** 将返回一个包装实现类型的 [**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr)。 你可以访问其所有接口的成员（使用箭头运算符），可以将它按原样返回给调用方，或者对它调用 **as** 并将得到的接口对象返回给调用方。

```cppwinrt
winrt::com_ptr<MyType> myimpl = winrt::make_self<MyType>();
myimpl->ToString();
myimpl->Close();
IClosable iclosable = myimpl.as<IClosable>();
iclosable.Close();
```

**MyType** 类不是投影的一部分；它是实现。 但是，通过这种方法，你可以直接调用其实现方法，而无需虚拟函数调用的开销。 在上面的示例中，即使 **MyType::ToString** 在 **IStringable** 上使用与投影方法相同的签名，我们也还是直接调用非虚拟方法，而不必跨越应用程序二进制接口 (ABI)。 **com_ptr** 只包含指向 **MyType** 结构的指针，因此你还可以通过 `myimpl` 变量和箭头运算符访问 **MyType** 的任何其他内部细节。

在以下情况： 你有一个接口对象，并且你恰好知道它是实现上的接口，然后你可以回到使用[**winrt::get_self**](/uwp/cpp-ref-for-winrt/get-self)函数模板的实现。 同样，这种方法可以避免虚拟函数调用，并让你直接访问实现。

> [!NOTE]
> 如果你尚未安装了 Windows SDK 版本 10.0.17763.0 (Windows 10 版本 1809年)，或更高版本，则你需要调用[**winrt:: from_abi**](/uwp/cpp-ref-for-winrt/from-abi)而不是[**winrt::get_self**](/uwp/cpp-ref-for-winrt/get-self)。

下面提供了一个示例。 [实现**BgLabelControl**自定义控件类](xaml-cust-ctrl.md#implement-the-bglabelcontrol-custom-control-class)中有另一个示例。

```cppwinrt
void ImplFromIClosable(IClosable const& from)
{
    MyType* myimpl = winrt::get_self<MyType>(from);
    myimpl->ToString();
    myimpl->Close();
}
```

但只有原始接口对象保留有接口。 如果*你*想要保留它，则可以调用 [**com_ptr::copy_from**](/uwp/cpp-ref-for-winrt/com-ptr#comptrcopyfrom-function)。

```cppwinrt
winrt::com_ptr<MyType> impl;
impl.copy_from(winrt::get_self<MyType>(from));
// com_ptr::copy_from ensures that AddRef is called.
```

实现类型本身不会从 **winrt::Windows::Foundation::IUnknown** 派生，因此它没有 **as** 函数。 即便如此，你可以实例化一个，并访问其所有接口的成员。 但如果你这样做，则不要将原始实现类型实例返回给调用方。 而应使用上面显示的方法之一，返回一个投影接口或 **com_ptr**。

```cppwinrt
MyType myimpl;
myimpl.ToString();
myimpl.Close();
IClosable ic1 = myimpl.as<IClosable>(); // error
```

如果你有一个实现类型的实例，并且需要将它传递给期望相应的投影类型的函数，那么你可以这样做。 实现类型上存在一个转换运算符 (前提是该实现类型由`cppwinrt.exe`工具) 的实现此目的。

## <a name="deriving-from-a-type-that-has-a-non-default-constructor"></a>从具有非默认构造函数的类型派生
[**ToggleButtonAutomationPeer::ToggleButtonAutomationPeer(ToggleButton)**](/uwp/api/windows.ui.xaml.automation.peers.togglebuttonautomationpeer.-ctor#Windows_UI_Xaml_Automation_Peers_ToggleButtonAutomationPeer__ctor_Windows_UI_Xaml_Controls_Primitives_ToggleButton_)是非默认构造函数的示例。 由于没有默认构造函数，因此，若要构造一个 **ToggleButtonAutomationPeer**，你需要传递 *owner*。 因此，如果你从 **ToggleButtonAutomationPeer** 派生，则需要提供一个接受 *owner* 并将它传递给基类的构造函数。 让我们来看看实际的情况。

```idl
// MySpecializedToggleButton.idl
namespace MyNamespace
{
    runtimeclass MySpecializedToggleButton :
        Windows.UI.Xaml.Controls.Primitives.ToggleButton
    {
        ...
    };
}
```

```idl
// MySpecializedToggleButtonAutomationPeer.idl
namespace MyNamespace
{
    runtimeclass MySpecializedToggleButtonAutomationPeer :
        Windows.UI.Xaml.Automation.Peers.ToggleButtonAutomationPeer
    {
        MySpecializedToggleButtonAutomationPeer(MySpecializedToggleButton owner);
    };
}
```

生成的实现类型的构造函数如下所示。

```cppwinrt
// MySpecializedToggleButtonAutomationPeer.cpp
...
MySpecializedToggleButtonAutomationPeer::MySpecializedToggleButtonAutomationPeer
    (MyNamespace::MySpecializedToggleButton const& owner)
{
    ...
}
...
```

唯一缺少的环节是，你需要将该构造函数参数传递给基类。 还记得我们上面提到过的 F 边界多态模式吗？ 在熟悉该模式（与 C++/WinRT 使用的相同）的细节之后，你可以查明基类的名称（或者你可以查找实现类的标头文件）。 这就是在此情况下调用基类构造函数的方式。

```cppwinrt
// MySpecializedToggleButtonAutomationPeer.cpp
...
MySpecializedToggleButtonAutomationPeer::MySpecializedToggleButtonAutomationPeer
    (MyNamespace::MySpecializedToggleButton const& owner) : 
    MySpecializedToggleButtonAutomationPeerT<MySpecializedToggleButtonAutomationPeer>(owner)
{
    ...
}
...
```

基类构造函数需要一个 **ToggleButton**。 **MySpecializedToggleButton** *是一个* **ToggleButton**.

在你按照上面所述进行编辑（将构造函数参数传递给基类）之前，编译器将标记构造函数并指出：在一个名为 **MySpecializedToggleButtonAutomationPeer_base&lt;MySpecializedToggleButtonAutomationPeer&gt;** 的类型（在此情况中）上没有适当的默认构造函数。 这实际上是实现类型的基类的基类。

## <a name="important-apis"></a>重要的 API
* [winrt::com_ptr 结构模板](/uwp/cpp-ref-for-winrt/com-ptr)
* [winrt::com_ptr::copy_from 函数](/uwp/cpp-ref-for-winrt/com-ptr#comptrcopyfrom-function)
* [winrt::from_abi 函数模板](/uwp/cpp-ref-for-winrt/from-abi)
* [winrt::get_self 函数模板](/uwp/cpp-ref-for-winrt/get-self)
* [winrt::implements 结构模板](/uwp/cpp-ref-for-winrt/implements)
* [winrt::make 函数模板](/uwp/cpp-ref-for-winrt/make)
* [winrt::make_self 函数模板](/uwp/cpp-ref-for-winrt/make-self)
* [winrt::Windows::Foundation::IUnknown::as 函数](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)

## <a name="related-topics"></a>相关主题
* [通过 C++/WinRT 使用 API](consume-apis.md)
* [XAML 控件; 绑定到 C++/WinRT 属性](binding-property.md#add-a-property-of-type-bookstoreviewmodel-to-mainpage)
