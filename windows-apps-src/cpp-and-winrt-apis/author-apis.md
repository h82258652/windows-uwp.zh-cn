---
description: 本主题展示了如何直接或间接使用 **winrt::implements** 基结构来创作 C++/WinRT API。
title: 使用 C++/WinRT 创作 API
ms.date: 07/08/2019
ms.topic: article
keywords: windows 10, uwp, 标准, c++, cpp, winrt, 投影的, 投影, 实现, 运行时类, 激活
ms.localizationpriority: medium
ms.openlocfilehash: ae8aca92d7eb7bc93ba4be99449d5e813238d744
ms.sourcegitcommit: d38e2f31c47434cd6dbbf8fe8d01c20b98fabf02
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/06/2019
ms.locfileid: "70393499"
---
# <a name="author-apis-with-cwinrt"></a>使用 C++/WinRT 创作 API

本主题展示了如何直接或间接使用 [winrt::implements](/uwp/cpp-ref-for-winrt/implements) 基结构来创作 [C++/WinRT API](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)  。 在此上下文中，“创作”的同义词有“生成”或“实现”    。 本主题介绍以下在 C++/WinRT 类型上实现 API 的情形（按此顺序）。

> [!NOTE]
> 本主题涉及 Windows 运行时组件，但其中的相关内容仅在 C++/WinRT 上下文中适用。 如果正在寻找有关涵盖所有 Windows 运行时语言的 Windows 运行时组件的内容，请参阅 [Windows 运行时组件](/windows/uwp/winrt-components/)。

- 你不是在创作一个 Windows 运行时类（运行时类）；你只是想要实现一个或多个 Windows 运行时接口以便在应用内进行本地使用  。 在此案例中，你直接从 winrt::implements 派生并实现相关函数  。
- 你正在创作一个运行时类  。 你可能正在创作一个要从某个应用中使用的组件。 或者，你可能正在创作一个要从 XAML 用户接口 (UI) 使用的类型，在此情况下，你在同一个编译单元内实现和使用一个运行时类。 在这些情况下，你使用工具为你生成派生自 winrt::implements 的类  。

在这两种情况下，实现 C++/WinRT API 的类型被称作“实现类型”  。

> [!IMPORTANT]
> 请务必区分实现类型与投影类型的概念。 投影类型将在[通过 C++/WinRT 使用 API](consume-apis.md) 中进行介绍。

## <a name="if-youre-not-authoring-a-runtime-class"></a>如果你没有创作运行时类 

最简单的方案是你要实现一个 Windows 运行时接口以进行本地使用。 你不需要运行时类；只需一个普通的 C++ 类。 例如，你可能会基于 [CoreApplication](/uwp/api/windows.applicationmodel.core.coreapplication) 编写一个应用  。

> [!NOTE]
> 有关安装和使用 C++/WinRT Visual Studio 扩展 (VSIX) 和 NuGet 包（两者共同提供项目模板，并生成支持）的信息，请参阅[适用于 C++/WinRT 的 Visual Studio 支持](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)。

在 Visual Studio 中，“Visual C++” > “Windows 通用” > 核心应用 (C++/WinRT)”项目模板阐释了 CoreApplication 模式     。 该模式首先将 [Windows::ApplicationModel::Core::IFrameworkViewSource](/uwp/api/windows.applicationmodel.core.iframeworkviewsource) 的实现传递给 [CoreApplication::Run](/uwp/api/windows.applicationmodel.core.coreapplication.run)   。

```cppwinrt
using namespace Windows::ApplicationModel::Core;
int __stdcall wWinMain(HINSTANCE, HINSTANCE, PWSTR, int)
{
    IFrameworkViewSource source = ...
    CoreApplication::Run(source);
}
```

CoreApplication 使用接口来创建应用的第一个视图  。 从概念上来说，IFrameworkViewSource 如下所示  。

```cppwinrt
struct IFrameworkViewSource : IInspectable
{
    IFrameworkView CreateView();
};
```

同样，从概念上来说，CoreApplication::Run 的实现执行此操作  。

```cppwinrt
void Run(IFrameworkViewSource viewSource) const
{
    IFrameworkView view = viewSource.CreateView();
    ...
}
```

因此，作为开发人员，应实现 IFrameworkViewSource 接口  。 C++/WinRT 具有基结构模板 [winrt::implements](/uwp/cpp-ref-for-winrt/implements)，从而可以很容易实现一个或多个接口，而不必求助于 COM 样式的编程  。 你可以从 implements 直接派生类型，然后实现接口的函数  。 操作方法如下。

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

这将由 IFrameworkViewSource 处理  。 下一步是返回实现 IFrameworkView 接口的对象  。 你也可以选择在 App 上实现该接口  。 下一个代码示例表示一个小型应用，该应用将至少会在桌面上运行一个窗口。

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

由于 App 类型是 IFrameworkViewSource，因此你可以传递一个到 Run     。

```cppwinrt
using namespace Windows::ApplicationModel::Core;
int __stdcall wWinMain(HINSTANCE, HINSTANCE, PWSTR, int)
{
    CoreApplication::Run(winrt::make<App>());
}
```

## <a name="if-youre-authoring-a-runtime-class-in-a-windows-runtime-component"></a>如果你正在 Windows 运行时组件中创作一个运行时类

如果类型打包在一个 Windows 运行时组件中以方便从应用程序中使用，则它需要是一个运行时类。 在 Microsoft 接口定义语言 (IDL) (.idl) 文件中声明运行时类（请参阅[将运行时类重构到 Midl 文件 (.idl) 中](#factoring-runtime-classes-into-midl-files-idl)）。

每个 IDL 文件生成一个 `.winmd` 文件，Visual Studio 会将所有这些合并为一个与根命名空间同名的文件。 最后生成的 `.winmd` 文件将是组件使用者将参考的文件。

下面是一个在 IDL 文件中声明运行时类的示例。

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

此 IDL 声明一个 Windows 运行时类。 运行时类是一个可通过现代 COM 接口进行激活和使用（通常跨可执行文件）的类型。 当你向项目和生成中添加 IDL 文件时，C++/WinRT 工具链（`midl.exe` 和 `cppwinrt.exe`）将会为你生成一个实现类型。 有关实际发挥作用的 IDL 文件工作流的示例，请参阅 [XAML 控件；绑定到 C++/WinRT 属性](binding-property.md)。

对于上面的示例 IDL，在名为 `\MyProject\MyProject\Generated Files\sources\MyRuntimeClass.h` 和 `MyRuntimeClass.cpp` 的源代码文件中，实现类型是一个名为 winrt::MyProject::implementation::MyRuntimeClass 的 C++ 结构存根  。

该实现类型如下所示。

```cppwinrt
// MyRuntimeClass.h
...
namespace winrt::MyProject::implementation
{
    struct MyRuntimeClass : MyRuntimeClassT<MyRuntimeClass>
    {
        MyRuntimeClass() = default;

        winrt::hstring Name();
        void Name(winrt::hstring const& value);
    };
}

// winrt::MyProject::factory_implementation::MyRuntimeClass is here, too.
```

请注意所使用的 F 边界多态模式（MyRuntimeClass 使用自身作为其基类 MyRuntimeClassT 的模板参数）   。 这也称为奇异递归模板模式 (CRTP)。 如果你向上查看继承链，你就会发现 MyRuntimeClass_base  。

```cppwinrt
template <typename D, typename... I>
struct MyRuntimeClass_base : implements<D, MyProject::IMyRuntimeClass, I...>
```

因此，在此情况中，继承层次结构的根同样是 [winrt::implements](/uwp/cpp-ref-for-winrt/implements) 基结构模板  。

有关更多详细信息、代码以及在 Windows 运行时组件创作 API 的演练，请参阅[使用 C++/WinRT 创作事件](author-events.md#create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component)。

## <a name="if-youre-authoring-a-runtime-class-to-be-referenced-in-your-xaml-ui"></a>如果你正在创作要在 XAML UI 中引用的运行时类

如果类型将由 XAML UI 引用，则它需要是一个运行时类，即使它与 XAML 是在同一项目中。 尽管它们通常是跨可执行文件边界进行激活的，但在实现它的编译单元内可以改用一个运行时类。

在此情况下，既会创作 API 同时又会使用 API  。 实现运行时类的过程与实现 Windows 运行时组件的过程实质上是相同的。 因此，请参阅上一节&mdash;[如果正在 Windows 运行时组件中创作运行时类](#if-youre-authoring-a-runtime-class-in-a-windows-runtime-component)。 唯一不同的细节在于，从 IDL 中，C++/WinRT 工具链不仅生成一个实现类型，而且还生成一个投影类型。 在此情况下只说明“MyRuntimeClass”可能是不明确的，认识到这一点很重要，因为有多个具有该名称的不同类型的实体  。

- MyRuntimeClass 是运行时类的名称  。 但这实际上是一种抽象：在 IDL 中声明并以某种编程语言实现。
- MyRuntimeClass 是 C++ 结构 winrt::MyProject::implementation::MyRuntimeClass（运行时类的 C++/WinRT 实现）的名称   。 正如我们看到的，如果有不同的实现和使用项目，则此结构仅存在于实现项目中。 这是实现类型或实现   。 此类型由 `cppwinrt.exe` 工具在文件 `\MyProject\MyProject\Generated Files\sources\MyRuntimeClass.h` 和 `MyRuntimeClass.cpp` 中生成。
- MyRuntimeClass 是采用 C++ 结构 winrt::MyProject::MyRuntimeClass 形式的投影类型的名称   。 如果有不同的实现和使用项目，则此结构仅存在于使用项目中。 这是投影类型或投影   。 此类型由 `cppwinrt.exe` 在文件 `\MyProject\MyProject\Generated Files\winrt\impl\MyProject.2.h` 中生成。

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

有关在运行时类上实现 INotifyPropertyChanged 接口的演练的示例，请参阅 [XAML 控件；绑定到 C++/WinRT 属性](binding-property.md)  。

有关在此情况下使用运行时类的过程将在[通过 C++/WinRT 使用 API](consume-apis.md#if-the-api-is-implemented-in-the-consuming-project) 中介绍。

## <a name="factoring-runtime-classes-into-midl-files-idl"></a>将运行时类重构到 Midl 文件 (.idl) 中

Visual Studio 项目和项模板为每个运行时类生成一个单独的 IDL 文件。 这样就可以在 IDL 文件及其生成的源代码文件之间形成逻辑对应关系。

不过，如果将项目的所有运行时类合并成单个 IDL 文件，则可显著缩短生成时间。 若要在它们之间建立复杂的（或循环的）`import` 依赖关系，则合并操作实际上是必需的。 你会发现，如果将运行时类放置在一起，则创作和查看它们会更容易。

## <a name="runtime-class-constructors"></a>运行时类构造函数

关于我们在上面看到的列表，要记住下面这些要点。

- 在 IDL 中声明的每个构造函数都会使得在实现类型和投影类型上生成一个构造函数。 将通过 IDL 声明的构造函数来使用不同编译单元中的运行时类  。
- 无论你是否有 IDL 声明的构造函数，在投影类型上都会生成一个接受 **std::nullptr_t** 的构造函数重载。 从同一编译单元中使用运行时类需要两个步骤，调用 **std::nullptr_t** 构造函数便是第一步   。 有关更多详细信息和代码示例，请参阅[通过 C++/WinRT 使用 API](consume-apis.md#if-the-api-is-implemented-in-the-consuming-project)。
- 如果你正在从同一编译单元使用运行时类，则还可以在实现类型上（请记住，这是在 `MyRuntimeClass.h` 中）直接实现非默认构造函数  。

> [!NOTE]
> 如果你预计运行时类将会从不同的编译单元使用（这很常见），则应在 IDL 中包括相应的构造函数（至少包括一个默认构造函数）。 这样一来，你在获得实现类型的同时还将获得一个工厂实现。
> 
> 如果你只想在同一编译单元内创作和使用运行时类，则不用在 IDL 中声明任何构造函数。 你不需要工厂实现，也不会生成它。 实现类型的默认构造函数将会被删除，但你可以轻松地编辑它并使之成为默认构造函数。
> 
> 如果你只想在同一编译单元内创作和使用运行时类，并且需要构造函数参数，则应直接在实现类型上创作你所需的构造函数。

## <a name="runtime-class-methods-properties-and-events"></a>运行时类方法、属性和事件

我们已经看到，工作流要使用 IDL 来声明运行时类及其成员，然后工具会生成原型和存根实现。 至于为运行时类的成员自动生成的那些原型，你可以  编辑它们，以便其传送与你在 IDL 中声明的类型不同的类型。 但仅当你在 IDL 中声明的类型可转发到你在已实现的版本中声明的类型时，才可以这么做。

下面是一些示例。

- 可以放宽对参数类型的要求。 例如，如果在 IDL 中，你的方法接受 **SomeClass**，那么可以选择在实现中将其更改为 **IInspectable**。 这会起作用，因为任何 **SomeClass** 均可转发到 **IInspectable**（当然，反之则不然）。
- 可以按值（而不是按引用）接受可复制的参数。 例如，将 `SomeClass const&` 更改为 `SomeClass const&`。 这在你需要避免将引用捕获到协同例程时是必要的（请参阅[参数传递](/windows/uwp/cpp-and-winrt-apis/concurrency#parameter-passing)）。
- 可以放宽对返回值的要求。 例如，可以将 **void** 更改为 [**winrt::fire_and_forget**](/uwp/cpp-ref-for-winrt/fire-and-forget)。

编写异步事件处理程序时，最后两个都非常有用。

## <a name="instantiating-and-returning-implementation-types-and-interfaces"></a>实例化和返回实现类型和接口

在这一节中，我们将以一个名为 MyType 的实现类型作为示例，该实现类型将实现 [IStringable](/uwp/api/windows.foundation.istringable) 和 [IClosable](/uwp/api/windows.foundation.iclosable) 接口    。

你可以直接从 [winrt::implements](/uwp/cpp-ref-for-winrt/implements)（这不是一个运行时类）派生 MyType   。

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

若要从 **MyType** 获得你可以使用或作为投影的一部分返回的 **IStringable** 或 **IClosable** 对象，应调用 [**winrt::make**](/uwp/cpp-ref-for-winrt/make) 函数模板。 make 将返回实现类型的默认接口  。

```cppwinrt
IStringable istringable = winrt::make<MyType>();
```

> [!NOTE]
> 但是，如果你从 XAML UI 引用类型，则在同一个项目中将会有一个实现类型和一个投影类型。 在这种情况下，make 将返回投影类型的实例  。 有关此情况的代码示例，请参阅 [XAML 控件；绑定到 C++/WinRT 属性](binding-property.md#add-a-property-of-type-bookstoreviewmodel-to-mainpage)。

我们仅可以使用 `istringable`（在上面的代码示例中）来调用 IStringable 接口的成员  。 但 C++/WinRT 接口（这是投影接口）派生自 [winrt::Windows::Foundation::IUnknown](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown)  。 因此，你可以对其调用 [IUnknown::as](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)（或 [IUnknown::try_as](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntry_as-function)）以查询其他也可使用或返回的投影类型或接口   。

```cppwinrt
istringable.ToString();
IClosable iclosable = istringable.as<IClosable>();
iclosable.Close();
```

如果你需要访问实现的所有成员并向调用方返回一个，则应使用 [winrt::make_self](/uwp/cpp-ref-for-winrt/make-self) 函数模板  。 make_self 将返回一个包装实现类型的 [winrt::com_ptr](/uwp/cpp-ref-for-winrt/com-ptr)   。 你可以访问其所有接口的成员（使用箭头运算符），可以将它按原样返回给调用方，或者对它调用 as 并将得到的接口对象返回给调用方  。

```cppwinrt
winrt::com_ptr<MyType> myimpl = winrt::make_self<MyType>();
myimpl->ToString();
myimpl->Close();
IClosable iclosable = myimpl.as<IClosable>();
iclosable.Close();
```

MyType 类不是投影的一部分；它是实现  。 但是，通过这种方法，你可以直接调用其实现方法，而无需虚拟函数调用的开销。 在上面的示例中，即使 MyType::ToString 在 IStringable 上使用与投影方法相同的签名，我们也还是直接调用非虚拟方法，而不必跨越应用程序二进制接口 (ABI)   。 com_ptr 只包含指向 MyType 结构的指针，因此你还可以通过 `myimpl` 变量和箭头运算符访问 MyType 的任何其他内部细节    。

假设你有一个接口对象，并且你恰好知道它是实现上的接口，则可以使用 [winrt::get_self](/uwp/cpp-ref-for-winrt/get-self) 函数模板返回到实现  。 同样，这种方法可以避免虚拟函数调用，并让你直接访问实现。

> [!NOTE]
> 如果你尚未安装 Windows SDK 版本 10.0.17763.0（Windows 10 版本 1809）或更高版本，则需调用 [winrt::from_abi](/uwp/cpp-ref-for-winrt/from-abi)，而不是 [winrt::get_self](/uwp/cpp-ref-for-winrt/get-self)   。

下面提供了一个示例。 [实现 BgLabelControl 自定义控件类](xaml-cust-ctrl.md#implement-the-bglabelcontrol-custom-control-class)中还有另一示例  。

```cppwinrt
void ImplFromIClosable(IClosable const& from)
{
    MyType* myimpl = winrt::get_self<MyType>(from);
    myimpl->ToString();
    myimpl->Close();
}
```

但只有原始接口对象保留有接口。 如果你想要保留它，则可以调用 [com_ptr::copy_from](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrcopy_from-function)   。

```cppwinrt
winrt::com_ptr<MyType> impl;
impl.copy_from(winrt::get_self<MyType>(from));
// com_ptr::copy_from ensures that AddRef is called.
```

实现类型本身不会从 winrt::Windows::Foundation::IUnknown 派生，因此它没有 as 函数   。 即便如此，也也可实例化一个，并访问其所有接口的成员。 但如果你这样做，请勿将原始实现类型实例返回给调用方。 而应使用上面显示的方法之一，返回一个投影接口或 com_ptr  。

```cppwinrt
MyType myimpl;
myimpl.ToString();
myimpl.Close();
IClosable ic1 = myimpl.as<IClosable>(); // error
```

如果你有一个实现类型的实例，并且需要将它传递给期望相应的投影类型的函数，那么你可以这样做。 实现类型上存在一个转换运算符（前提是实现类型是由 `cppwinrt.exe` 工具生成的），可以帮助完成此工作。 可以将实现类型值直接传递给期望得到相应投影类型值的方法。 从实现类型成员函数，可以将 `*this` 传递给期望得到相应投影类型值的方法。

```cppwinrt
// MyProject::MyType is the projected type; the implementation type would be MyProject::implementation::MyType.

void MyOtherType::DoWork(MyProject::MyType const&){ ... }

...

void FreeFunction(MyProject::MyOtherType const& ot)
{
    MyType myimpl;
    ot.DoWork(myimpl);
}

...

void MyType::MemberFunction(MyProject::MyOtherType const& ot)
{
    ot.DoWork(*this);
}
```

## <a name="deriving-from-a-type-that-has-a-non-default-constructor"></a>从一个具有非默认构造函数的类型派生

[ToggleButtonAutomationPeer::ToggleButtonAutomationPeer(ToggleButton)](/uwp/api/windows.ui.xaml.automation.peers.togglebuttonautomationpeer.-ctor#Windows_UI_Xaml_Automation_Peers_ToggleButtonAutomationPeer__ctor_Windows_UI_Xaml_Controls_Primitives_ToggleButton_) 是非默认构造函数的一个示例  。 由于没有默认构造函数，因此，若要构造一个 ToggleButtonAutomationPeer，你需要传递 owner   。 因此，如果你从 ToggleButtonAutomationPeer 派生，则需要提供一个接受 owner 并将它传递给基类的构造函数   。 让我们来看看实际的情况。

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

基类构造函数需要一个 ToggleButton  。 且 MySpecializedToggleButton 即是 ToggleButton    。

在你按照上面所述进行编辑（将构造函数参数传递给基类）之前，编译器将标记构造函数并指出：在一个名为 MySpecializedToggleButtonAutomationPeer_base&lt;MySpecializedToggleButtonAutomationPeer&gt; 的类型（在此情况中）上没有适当的默认构造函数  。 这实际上是实现类型的基类的基类。

## <a name="namespaces-projected-types-implementation-types-and-factories"></a>命名空间：投影类型、实现类型和工厂

如该主题中前面的内容所示，C++/WinRT 运行时类在多个命名空间中以多个 C++ 类的形式存在。 因此，名称 **MyRuntimeClass** 在 **winrt::MyProject** 命名空间中有一种含义，在 **winrt::MyProject::implementation** 命名空间中有另一种含义。 如果需要一个来自其他命名空间的名称，请注意你目前的上下文中有哪一个命名空间，然后使用命名空间前缀。 让我们进一步了解所讨论的命名空间。

- **winrt::MyProject**。 此命名空间包含投影类型。 投影类型的对象是一个代理，它实质上是一个指向后备对象的智能指针，该后备对象可能会在你的项目中的此处实现，也可能在另一个编译单元中实现。
- **winrt::MyProject::implementation**。 此命名空间包含实现类型。 实现类型的对象不是指针；它是一个值 &mdash; 一个完整的 C++ 堆栈对象。 不要直接构造实现类型；而应调用 [**winrt::make**](/uwp/cpp-ref-for-winrt/make)，将实现类型作为模板参数传递。 在本主题前面的部分中，我们已演示了实际发挥作用的 **winrt::make** 示例，并且 [XAML 控制；绑定到 C++/WinRT 属性](binding-property.md#add-a-property-of-type-bookstoreviewmodel-to-mainpage)中还有另一个示例。 另请参阅 [Diagnosing direct allocations](/windows/uwp/cpp-and-winrt-apis/diag-direct-alloc)（诊断直接分配）。
- **winrt::MyProject::factory_implementation**。 此命名空间包含工厂。 此命名空间中的对象支持 [**IActivationFactory**](/windows/win32/api/activation/nn-activation-iactivationfactory)。

下表显示了需要在不同的上下文中使用的最小命名空间限定。

|上下文中的命名空间|指定投影类型|指定投影类型|
|-|-|-|
|**winrt::MyProject**|`MyRuntimeClass`|`implementation::MyRuntimeClass`|
|**winrt::MyProject::implementation**|`MyProject::MyRuntimeClass`|`MyRuntimeClass`|

> [!IMPORTANT]
> 当想要从实现中返回投影类型时，请注意不要通过编写 `MyRuntimeClass myRuntimeClass;` 来实例化实现类型。 本主题中前面的[实例化和返回实现类型和接口](#instantiating-and-returning-implementation-types-and-interfaces)部分显示了适用于该情况的正确技术和代码。
>
> 在这种情况下，`MyRuntimeClass myRuntimeClass;` 存在的问题是，它在堆栈上创建 **winrt::MyProject::implementation::MyRuntimeClass** 对象。 （实现类型的）该对象的行为在某些方面类似于投影类型 &mdash; 你可以采用相同的方式调用它的方法；并且它甚至会转换为投影类型。 但作用域退出时，该对象将按照正常 C++ 规则析构。 因此，如果向该对象返回了投影类型（智能指针），那么该指针现在无关联。
>
> 此内存损坏类型的 bug 很难诊断。 因此，对于调试版本，C++/WinRT 断言可帮助你使用堆栈检测器捕获此错误。 但是，协同例程是在堆上分配的，因此如果让此错误出现在协同例程内，那么你不会得到有关此错误的帮助。 有关详细信息，请参阅 [Diagnosing direct allocations](/windows/uwp/cpp-and-winrt-apis/diag-direct-alloc)（诊断直接分配）。

## <a name="using-projected-types-and-implementation-types-with-various-cwinrt-features"></a>将投影类型和实现类型与各种 C++/WinRT 功能配合使用

下面列出了各种需要某个类型的 C++/WinRT 功能及其需要的类型（投影类型、实现类型或两者）。

|功能|接受|注释|
|-|-|-|
|`T`（表示智能指针）|投影|请参阅[命名空间：投影类型、实现类型和工厂](#namespaces-projected-types-implementation-types-and-factories)中有关错误地使用实现类型的警告。|
|`agile_ref<T>`|两者|如果使用实现类型，则该构造函数参数必须为 `com_ptr<T>`。|
|`com_ptr<T>`|实现|使用投影类型将生成错误：`'Release' is not a member of 'T'`。|
|`default_interface<T>`|两者|如果使用实现类型，则会返回第一个已实现的接口。|
|`get_self<T>`|实现|使用投影类型将生成错误：`'_abi_TrustLevel': is not a member of 'T'`。|
|`guid_of<T>()`|两者|返回默认接口的 GUID。|
|`IWinRTTemplateInterface<T>`<br>|投影|使用实现类型进行了编译，但这是一个错误 &mdash; 请参阅[命名空间：投影类型、实现类型和工厂](#namespaces-projected-types-implementation-types-and-factories)中的警告。|
|`make<T>`|实现|使用投影类型将生成错误：`'implements_type': is not a member of any direct or indirect base class of 'T'`。|
| `make_agile(T const&amp;)`|两者|如果使用实现类型，则该参数必须为 `com_ptr<T>`。|
| `make_self<T>`|实现|使用投影类型将生成错误：`'Release': is not a member of any direct or indirect base class of 'T'`|
| `name_of<T>`|投影|如果使用实现类型，则将获得默认接口的字符串化 GUID。|
| `weak_ref<T>`|两者|如果使用实现类型，则该构造函数参数必须为 `com_ptr<T>`。|

## <a name="opt-in-to-uniform-construction-and-direct-implementation-access"></a>选择加入统一构造和直接实现访问

此部分介绍一项可以选择加入的 C++/WinRT 2.0 功能，不过在新项目中，该功能是默认启用的。 对于现有项目，需要通过配置 `cppwinrt.exe` 工具来选择加入。 在 Visual Studio 中，将项目属性“常见属性”   > “C++/WinRT”   > “已优化”  设置为“是”  。 该操作的效果是将 `<CppWinRTOptimized>true</CppWinRTOptimized>` 添加到项目文件。 它与从命令行调用 `cppwinrt.exe` 时添加开关具有相同的效果。

`-opt[imize]` 开关启用通常称为“统一构造”的功能。  有了统一  构造，就可以通过 C++/WinRT 语言投影本身高效地创建并使用实现类型（通过组件实现的供应用程序使用的类型），不会造成任何加载器困难。

在介绍此功能之前，让我们先演示一下没有统一构造的情况。  我们将从下面这个示例性的 Windows 运行时类着手进行演示。

```idl
// MyClass.idl
namespace MyProject
{
    runtimeclass MyClass
    {
        MyClass();
        void Method();
        static void StaticMethod();
    }
}
```

作为熟悉 C++/WinRT 库的使用的 C++ 开发人员，你可能希望像下面这样使用该类。

```cppwinrt
using namespace winrt::MyProject;

MyClass c;
c.Method();
MyClass::StaticMethod();
```

如果所显示的使用代码并没有驻留在用于实现该类的组件中，则这样做很合理。 充当语言投影的 C++/WinRT 会将作为开发人员的你与 ABI（基于 COM 的应用程序二进制接口，由 Windows 运行时定义）隔离开来。 C++/WinRT 不直接调用实现，而是通过 ABI 来调用。

因此，在构造 **MyClass** 对象 (`MyClass c;`) 的代码行中，C++/WinRT 投影会调用 [**RoGetActivationFactory**](/windows/win32/api/roapi/nf-roapi-rogetactivationfactory) 来检索类或激活工厂，然后使用该工厂来创建对象。 最后一行同样使用该工厂进行调用（看起来是静态方法调用）。 所有这些都需要你的类进行注册并且你的模块实现 [**DllGetActivationFactory**](/previous-versions/br205771(v=vs.85)) 入口点。 C++/WinRT 有速度很快的工厂缓存，因此，对于使用你的组件的应用程序来说，这些都不会造成问题。 问题在于，在你的组件中，你刚才的操作有些问题。

首先，不管 C++/WinRT 工厂缓存有多快，通过 **RoGetActivationFactory** 进行调用的速度（或者甚至包括通过工厂缓存进行后续调用的速度）始终会慢于直接调用实现的速度。 很明显，对于本地定义的类型来说，依次调用 **RoGetActivationFactory**、[**IActivationFactory::ActivateInstance**](/windows/win32/api/activation/nf-activation-iactivationfactory-activateinstance)、[**QueryInterface**](/windows/win32/api/unknwn/nf-unknwn-iunknown-queryinterface(refiid_void)) 的效率不如使用 C++ `new` 表达式的效率高。 因此，在组件中创建对象时，有经验的 C++/WinRT 开发人员习惯于使用 [**winrt::make**](/uwp/cpp-ref-for-winrt/make) 或 [**winrt::make_self**](/uwp/cpp-ref-for-winrt/make-self) 帮助程序函数。

```cppwinrt
// MyClass c;
MyProject::MyClass c{ winrt::make<implementation::MyClass>() };
```

但是，如你所见，这根本谈不上方便，也不简洁。 必须使用帮助程序函数来创建对象，还必须在实现类型和投影类型之间进行区分。

其次，使用投影来创建类意味着会缓存其激活工厂。 通常情况下，这是你希望的，但如果工厂驻留在进行调用的模块 (DLL) 中，则必须有效地固定 DLL 并防止其卸载。 许多情况下，这没有关系，但某些系统组件必须支持卸载。 

这种情况下，我们有必要谈一谈“统一构造”这个术语。  不管创建代码是驻留在只使用类的项目中，还是驻留在对类进行实际实现的项目中，  你都可以自由地使用相同语法来创建对象。

```cppwinrt
// MyProject::MyClass c{ winrt::make<implementation::MyClass>() };
MyClass c;
```

使用 `-opt[imize]` 开关生成组件项目时，通过语言投影进行的调用会编译成对 **winrt::make** 函数的同一高效调用，该函数会直接创建实现类型。 这样会使得你的语法既简单又可以预测，避免了通过工厂进行调用带来的性能冲击，不需在此过程中固定组件。 除了组件项目，这还适用于 XAML 应用程序。 为同一应用程序中实现的类绕过 **RoGetActivationFactory** 以后，就可以构造这些类（无需注册），所采用的方式与这些类位于组件之外时所采用的方式完全相同。

统一构造适用于由工厂在后台进行的任何调用。  实际上，这意味着优化是同时针对构造函数和静态成员的。 下面还是该原始示例。

```cppwinrt
MyClass c;
c.Method();
MyClass::StaticMethod();
```

在没有 `-opt[imize]` 的情况下，第一个和最后一个语句需要通过工厂对象进行调用。  如果使用 `-opt[imize]`，则这两个语句都不需要那样进行调用。 这些调用是直接针对实现进行编译的，甚至可以使用内联的方式。 这就涉及到在讨论 `-opt[imize]` 时通常会用到的另一术语，即“直接实现”访问。 

语言投影是方便，但当你可以直接访问实现时，你可以而且应该利用直接访问，尽可能生成最有效的代码。 C++/WinRT 可以为你实现这一点，不会强制要求你舍弃投影的安全性和工作效率。

这是一项重大更改，因为组件必须“合作”，让语言投影进入并直接访问其实现类型。 由于 C++/WinRT 是一个仅头文件库，因此你可以在其中查看具体信息。 在没有 `-opt[imize]` 的情况下，可以通过投影将 **MyClass** 构造函数和 **StaticMethod** 成员定义如下。

```cppwinrt
namespace winrt::MyProject
{
    inline MyClass::MyClass() :
        MyClass(impl::call_factory<MyClass>([](auto&& f){
            return f.template ActivateInstance<MyClass>(); }))
    {
    }
    inline void MyClass::StaticMethod()
    {
        impl::call_factory<MyClass, MyProject::IClassStatics>([&](auto&& f) {
            return f.StaticMethod(); });
    }
}
```

没有必要完全遵循上面的代码；该代码的目的是为了表明两个调用都涉及调用名为 **call_factory** 的函数。 你可以据此认为，这些调用涉及工厂缓存，不直接访问实现。  如果使用 `-opt[imize]`，则根本就不定义这些函数， 而是通过投影来声明它们，其定义留给组件。

然后，组件就可以提供定义来直接调用实现。 现在，我们谈谈重大更改。 这些定义是在你使用 `-component` 和 `-opt[imize]` 时为你生成的，它们出现在名为 `Type.g.cpp` 的文件中，其中的 *Type* 是所实现的运行时类的名称。 这是你首次在现有项目中启用 `-opt[imize]` 时可能会遇到各种链接器错误的原因。 若要修复错误，需要将该生成的文件包括到实现中。

在我们的示例中，`MyClass.h` 可能如下所示（不管是否使用 `-opt[imize]`）。

```cppwinrt
// MyClass.h
#pragma once
#include "MyClass.g.h"
 
namespace winrt::MyProject::implementation
{
    struct MyClass : ClassT<MyClass>
    {
        MyClass() = default;
 
        static void StaticMethod();
        void Method();
    };
}
namespace winrt::MyProject::factory_implementation
{
    struct MyClass : ClassT<MyClass, implementation::MyClass>
    {
    };
}
```

`MyClass.cpp` 是整合这一切的地方。

```cppwinrt
#include "pch.h"
#include "MyClass.h"
#include "MyClass.g.cpp" // !!It's important that you add this line!!
 
namespace winrt::MyProject::implementation
{
    void MyClass::StaticMethod()
    {
    }
 
    void MyClass::Method()
    {
    }
}
```

因此，若要在现有项目中使用统一构造，需编辑每个实现的 `.cpp` 文件，以便在包括（和定义）实现类后 `#include <Sub/Namespace/Type.g.cpp>`。 该文件提供投影未定义的函数的定义。 下面是 `MyClass.g.cpp` 文件中的这些定义。

```cppwinrt
namespace winrt::MyProject
{
    MyClass::MyClass() :
        MyClass(make<MyProject::implementation::MyClass>())
    {
    }
    void MyClass::StaticMethod()
    {
        return MyProject::implementation::MyClass::StaticMethod();
    }
}
```

这样就可以高效地直接调用实现，避免调用工厂缓存，并且满足了链接器的要求，很好地完成了投影。

最后，`-opt[imize]` 会更改项目的 `module.g.cpp`（此文件用于实现 DLL 的 **DllGetActivationFactory** 和 **DllCanUnloadNow** 导出）的实现，通过消除 C++/WinRT 1.0 所需要的强类型耦合，大大加快了增量生成的速度。 这通常称为“类型擦除工厂”。  在没有 `-opt[imize]` 的情况下，为组件生成的 `module.g.cpp` 文件一开始就会包括所有实现类的定义&mdash;此示例中的 `MyClass.h`。 然后，它会直接创建每个类的实现工厂，如下所示。

```cppwinrt
if (requal(name, L"MyProject.MyClass"))
{
    return winrt::detach_abi(winrt::make<winrt::MyProject::factory_implementation::MyClass>());
}
```

同样，你不需要遵循所有细节。 需要注意的是，这需要为组件所实现的任何以及所有类进行完整的定义。 这可能会对内循环产生巨大影响，因为对单个实现的任何更改都会导致 `module.g.cpp` 重新编译。 在使用 `-opt[imize]` 的情况下，这不再是一个问题， 但所生成的 `module.g.cpp` 文件会出现两种情况。 第一种情况是，它不再包括任何实现类。 在此示例中，它根本不包括 `MyClass.h`， 而只是创建实现工厂，不需要对其实现有任何的了解。

```cppwinrt
void* winrt_make_MyProject_MyClass();
 
if (requal(name, L"MyProject.MyClass"))
{
    return winrt_make_MyProject_MyClass();
}
```

很明显，不需要包括其定义，只需由链接器解析 **winrt_make_Component_Class** 函数的定义即可。 当然，你不需考虑这一点，因为为你生成的 `MyClass.g.cpp` 文件（为了支持统一构造，你以前已包括了此文件）也定义该函数。 下面是为此示例生成的 `MyClass.g.cpp` 文件的完整内容。

```cppwinrt
void* winrt_make_MyProject_MyClass()
{
    return winrt::detach_abi(winrt::make<winrt::MyProject::factory_implementation::MyClass>());
}
namespace winrt::MyProject
{
    MyClass::MyClass() :
        MyClass(make<MyProject::implementation::MyClass>())
    {
    }
    void MyClass::StaticMethod()
    {
        return MyProject::implementation::MyClass::StaticMethod();
    }
}
```

如你所见，**winrt_make_MyProject_MyClass** 函数直接创建实现的工厂。 这一切意味着，你可以随意更改任何给定实现，根本不需要重新编译 `module.g.cpp`。 只有在添加或删除 Windows 运行时类的时候，才会更新 `module.g.cpp`，此时需将其重新编译。

## <a name="overriding-base-class-virtual-methods"></a>重写基类虚拟方法

如果基类和派生类都是应用定义类，则派生类可能出现虚拟方法问题，但虚拟方法是在祖父 Windows 运行时类中定义的。 实际上，如果从 XAML 类派生，则会出现这种情况。 本部分的其余内容是[派生类](/windows/uwp/cpp-and-winrt-apis/move-to-winrt-from-cx#derived-classes)中的示例的继续。

```cppwinrt
namespace winrt::MyNamespace::implementation
{
    struct BasePage : BasePageT<BasePage>
    {
        void OnNavigatedFrom(Windows::UI::Xaml::Navigation::NavigationEventArgs const& e);
    };

    struct DerivedPage : DerivedPageT<DerivedPage>
    {
        void OnNavigatedFrom(Windows::UI::Xaml::Navigation::NavigationEventArgs const& e);
    };
}
```

层次结构为 [**Windows::UI::Xaml::Controls::Page**](/uwp/api/windows.ui.xaml.controls.page) \<- **BasePage** \<- **DerivedPage**。 **BasePage::OnNavigatedFrom** 方法正确重写了 [**Page::OnNavigatedFrom**](/uwp/api/windows.ui.xaml.controls.page.onnavigatedfrom)，但 **DerivedPage::OnNavigatedFrom** 不重写 **BasePage::OnNavigatedFrom**。

在这里，**DerivedPage** 重用 **BasePage** 提供的 **IPageOverrides** vtable，这意味着它无法重写 **IPageOverrides::OnNavigatedFrom** 方法。 一个可能的解决方案是让 **BasePage** 本身充当一个模板类，将其实现完全置于标头文件中，但这会使得事情变得相当复杂，令人不可接受。

解决方法是在基类中将 **OnNavigatedFrom** 方法声明为显式虚拟方法。 这样，当 **DerivedPage::IPageOverrides::OnNavigatedFrom** 的 vtable 条目调用 **BasePage::IPageOverrides::OnNavigatedFrom** 时，生成器会调用 **BasePage::OnNavigatedFrom**，后者因其虚拟特性，最终会调用 **DerivedPage::OnNavigatedFrom**。

```cppwinrt
namespace winrt::MyNamespace::implementation
{
    struct BasePage : BasePageT<BasePage>
    {
        // Note the `virtual` keyword here.
        virtual void OnNavigatedFrom(Windows::UI::Xaml::Navigation::NavigationEventArgs const& e);
    };

    struct DerivedPage : DerivedPageT<DerivedPage>
    {
        void OnNavigatedFrom(Windows::UI::Xaml::Navigation::NavigationEventArgs const& e);
    };
}
```

这要求类层次结构的所有成员都认可 **OnNavigatedFrom** 方法的返回值和参数类型。 如果它们不认可，则应使用上面的版本作为虚拟方法，并将备用内容包装好。

## <a name="important-apis"></a>重要的 API
* [winrt::com_ptr 结构模板](/uwp/cpp-ref-for-winrt/com-ptr)
* [winrt::com_ptr::copy_from 函数](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrcopy_from-function)
* [winrt::from_abi 函数模板](/uwp/cpp-ref-for-winrt/from-abi)
* [winrt::get_self 函数模板](/uwp/cpp-ref-for-winrt/get-self)
* [winrt::implements 结构模板](/uwp/cpp-ref-for-winrt/implements)
* [winrt::make 函数模板](/uwp/cpp-ref-for-winrt/make)
* [winrt::make_self 函数模板](/uwp/cpp-ref-for-winrt/make-self)
* [winrt::Windows::Foundation::IUnknown::as 函数](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)

## <a name="related-topics"></a>相关主题
* [通过 C++/WinRT 使用 API](consume-apis.md)
* [XAML 控件; 绑定到 C++/WinRT 属性](binding-property.md#add-a-property-of-type-bookstoreviewmodel-to-mainpage)
