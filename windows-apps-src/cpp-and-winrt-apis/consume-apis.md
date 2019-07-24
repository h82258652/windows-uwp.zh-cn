---
description: 本主题介绍了如何使用 C++/WinRT API，无论它们是由 Windows、第三方组件供应商还是由你自己实现的。
title: 通过 C++/WinRT 使用 API
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, 标准, c++, cpp, winrt, 投影的, 投影, 实现, 运行时类, 激活
ms.localizationpriority: medium
ms.openlocfilehash: 5a3d4b554fafeb2053e4e6af831c224b5eacd151
ms.sourcegitcommit: ba4a046793be85fe9b80901c9ce30df30fc541f9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/19/2019
ms.locfileid: "68328866"
---
# <a name="consume-apis-with-cwinrt"></a>通过 C++/WinRT 使用 API

本主题介绍如何使用 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) API，无论它们是 Windows 的一部分、由第三方组件供应商或自行实现。

## <a name="if-the-api-is-in-a-windows-namespace"></a>如果 API 位于 Windows 命名空间中
这是你使用 Windows 运行时 API 最常见的情况。 对于元数据中定义的 Windows 命名空间中的每个类型，C++/WinRT 都定义了 C++ 友好等效项（称为投影类型  ）。 投影类型具有与 Windows 类型相同的完全限定名称，但使用 C++ 语法放置于 C++ winrt  命名空间中。 例如，[Windows::Foundation::Uri  ](/uwp/api/windows.foundation.uri) 作为 winrt::Windows::Foundation::Uri  投影到 C++/WinRT。

以下是一个简单的代码示例。 若要将以下代码示例直接复制并粘贴到 Windows 控制台应用程序 (C++/WinRT) 项目的主源代码文件中，请先在项目属性中设置“不使用预编译的标头”   。

```cppwinrt
// main.cpp
#include <winrt/Windows.Foundation.h>

using namespace winrt;
using namespace Windows::Foundation;

int main()
{
    winrt::init_apartment();
    Uri contosoUri{ L"http://www.contoso.com" };
    Uri combinedUri = contosoUri.CombineUri(L"products");
}
```

包含的标头 `winrt/Windows.Foundation.h` 是 SDK 的一部分，可在文件夹 `%WindowsSdkDir%Include<WindowsTargetPlatformVersion>\cppwinrt\winrt\` 内找到。 该文件夹中的标头包含投影到 C++/WinRT 中的 Windows 命名空间类型。 在此示例中，`winrt/Windows.Foundation.h` 包含 winrt::Windows::Foundation::Uri  ，它是运行时类 [Windows::Foundation::Uri  ](/uwp/api/windows.foundation.uri) 的投影类型。

> [!TIP]
> 如果希望使用来自 Windows 命名空间的类型，请包括与该命名空间对应的 C++/WinRT 标头。 `using namespace` 指令是可选的，不过这种指令很方便。

在上述代码示例中，在初始化 C++/WinRT 后，我们将通过其公开记录的构造函数之一（本示例中为 [Uri(字符串)  ](/uwp/api/windows.foundation.uri.-ctor#Windows_Foundation_Uri__ctor_System_String_)）堆叠分配 winrt::Windows::Foundation::Uri  投影类型的值。 这是最常见的用例，也是一般情况下你所要做的全部工作。 在有了 C++/WinRT 投影类型值后，你可以将其视为实际 Windows 运行时类型的实例，因为它具有所有相同的成员。

事实上，该投影值是一个代理；它本质上只是支持对象的智能指针。 投影值的构造函数调用 [RoActivateInstance  ](https://docs.microsoft.com/windows/desktop/api/roapi/nf-roapi-roactivateinstance) 来创建 Windows 运行时支持类（本例中为 Windows.Foundation.Uri  ）的实例，并将该对象的默认接口存储在新投影值内。 如下所示，你对投影值的成员的调用实际上通过智能指针代理给支持对象；这是发生状态变化的地方。

![投影 Windows::Foundation::Uri 类型](images/uri.png)

当 `contosoUri` 值超出范围时，它将自行销毁，并将其引用发布到默认接口。 如果该引用是对支持 Windows 运行时 Windows.Foundation.Uri  对象的最后一个引用，支持对象也会自行销毁。

> [!TIP]
> 投影类型  是出于使用运行时类的 API 针对运行时类的包装器。 投影接口  是针对 Windows 运行时接口的包装器。

## <a name="cwinrt-projection-headers"></a>C++/WinRT 投影标头
若要使用 C++/WinRT 的 Windows 命名空间 API，你应使用来自 `%WindowsSdkDir%Include<WindowsTargetPlatformVersion>\cppwinrt\winrt` 文件夹的标头。 下级命名空间中的类型在其直接父命名空间中引用类型是很常见的。 因此，每个 C++/WinRT 投影标头将自动包括其父命名空间标头文件；因此你不需要  明确包括它。 不过，这样做也不会出现错误。

例如，对于 [Windows::Security::Cryptography::Certificates  ](/uwp/api/windows.security.cryptography.certificates) 命名空间，等效的 C++/WinRT 类型定义驻留在 `winrt/Windows.Security.Cryptography.Certificates.h` 中。 Windows::Security::Cryptography::Certificates  中的类型需要父 Windows::Security::Cryptography  命名空间中的类型；该命名空间中的类型可能需要其自己的父 Windows::Security  中的类型。

因此，当你包括 `winrt/Windows.Security.Cryptography.Certificates.h` 时，该文件反之将包括 `winrt/Windows.Security.Cryptography.h`；且 `winrt/Windows.Security.Cryptography.h` 将包括 `winrt/Windows.Security.h`。 这就是跟踪停止的位置，因为没有 `winrt/Windows.h`。 此可传递包含过程在第二级命名空间停止。

此过程间接包括为在父命名空间中定义的类提供必要的声明  和实现  的标头文件。

一个命名空间中类型的成员可以引用其他无关命名空间中的一个或多个类型。 为了让编译器能够成功编译这些成员定义，编译器需要查看所有这些类型关闭的类型声明。 因此，每个 C++/WinRT 投影标头均包括它需要声明  任何依赖类型的命名空间标头。 与父命名空间不同，此过程不  拉入所引用类型的实现  。

> [!IMPORTANT]
> 当你希望实际使用  在无关命名空间中声明的类型（实例化、调用方法等）时，你必须包括该类型的相应的命名空间标头文件。 仅自动包括声明  ，没有实现  。

例如，如果你仅包括 `winrt/Windows.Security.Cryptography.Certificates.h`，则会导致声明从这些命名空间（等等，间接）拉入。

- Windows.Foundation
- Windows.Foundation.Collections
- Windows.Networking
- Windows.Storage.Streams
- Windows.Security.Cryptography

换言之，某些 API 在你包括的标头中进行了前向声明。 但它们的定义在你尚未包括的标头中。 因此，如果你随后调用 [Windows::Foundation::Uri::RawUri  ](/uwp/api/windows.foundation.uri.rawuri)，那么你会收到指示成员未定义的链接器错误。 解决方法是明确 `#include <winrt/Windows.Foundation.h>`。 一般情况下，当你看到此类链接器错误时，应包括为该 API 的命名空间命名的标头，并重新生成。

## <a name="accessing-members-via-the-object-via-an-interface-or-via-the-abi"></a>通过对象、接口或通过 ABI 访问成员
使用 C++/WinRT 投影，Windows 运行时类的运行时表示形式将只是基础 ABI 接口。 不过，为方便起见，你可以通过类作者预期的方式根据类编码。 例如，你可以调用 [Uri  ](/uwp/api/windows.foundation.uri) 的 ToString  方法，就像它是该类的方法（实际上，再深入一层，它是单独的 IStringable  接口上的方法）。

`WINRT_ASSERT` 是宏定义，并且它扩展到 [_ASSERTE](/cpp/c-runtime-library/reference/assert-asserte-assert-expr-macros)。

```cppwinrt
Uri contosoUri{ L"http://www.contoso.com" };
WINRT_ASSERT(contosoUri.ToString() == L"http://www.contoso.com/"); // QueryInterface is called at this point.
```

这种便利是通过对相应接口的查询实现的。 不过始终在你的控制范围内。 你可以通过自己检索 IStringable 接口并直接使用它来放弃一点便利，从而提高一点性能。 在下方的代码示例中，你在运行时获取实际的 IStringable 接口指针（通过一次性查询）。 此后，你对 ToString  的调用是直接的，并且避免进一步调用 QueryInterface  。

```cppwinrt
...
IStringable stringable = contosoUri; // One-off QueryInterface.
WINRT_ASSERT(stringable.ToString() == L"http://www.contoso.com/");
```

如果你知道你将在同一个接口调用多个方法，你可能会选择使用此技巧。

顺便说一下，如果你确实希望访问 ABI 级别的成员，那么你可以这样做。 下方的代码示例显示了如何操作，[实现 C++/WinRT 与 ABI 之间的互操作](interop-winrt-abi.md)中还提供了更多详细信息和代码示例。

```cppwinrt
#include <Windows.Foundation.h>
#include <unknwn.h>
#include <winrt/Windows.Foundation.h>
using namespace winrt::Windows::Foundation;

int main()
{
    winrt::init_apartment();
    Uri contosoUri{ L"http://www.contoso.com" };

    int port{ contosoUri.Port() }; // Access the Port "property" accessor via C++/WinRT.

    winrt::com_ptr<ABI::Windows::Foundation::IUriRuntimeClass> abiUri{
        contosoUri.as<ABI::Windows::Foundation::IUriRuntimeClass>() };
    HRESULT hr = abiUri->get_Port(&port); // Access the get_Port ABI function.
}
```

## <a name="delayed-initialization"></a>延迟初始化

在 C++/WinRT 中，每个投影的类型都有一个特殊的 C++/WinRT **std::nullptr_t** 构造函数。 除了该构造函数，所有其他投影类型的构造函数（包括默认的构造函数）都会导致系统创建一个支持的 Windows 运行时对象，并为你提供它的智能指针。 因此，该规则适用于使用默认构造函数的任何地方，例如未初始化的本地变量、未初始化的全局变量以及未初始化的成员变量。

另一方面，如果你想要构造投影类型的变量，而无需它反过来构造支持的 Windows 运行时对象（以便你可以延迟该工作），你可以这样做。 使用该特殊 C++/WinRT **std::nullptr_t** 构造函数（C++/WinRT 投影已将它插入每个运行时类中）声明你的变量或字段。 在下面的代码示例中，我们将该特殊构造函数与 *m_gamerPicBuffer* 配合使用。

```cppwinrt
#include <winrt/Windows.Storage.Streams.h>
using namespace winrt::Windows::Storage::Streams;

#define MAX_IMAGE_SIZE 1024

struct Sample
{
    void DelayedInit()
    {
        // Allocate the actual buffer.
        m_gamerPicBuffer = Buffer(MAX_IMAGE_SIZE);
    }

private:
    Buffer m_gamerPicBuffer{ nullptr };
};

int main()
{
    winrt::init_apartment();
    Sample s;
    // ...
    s.DelayedInit();
}
```

除  **std::nullptr_t** 构造函数以外的投影类型上的所有构造函数都将导致创建后备 Windows 运行时对象。 **std::nullptr_t** 构造函数本质上不执行任何操作。 它预期投影对象会在后续时间初始化。 因此，不论运行时类是否具有默认的构造函数，你都可以使用此技巧实现有效的延迟初始化。

此注意事项会影响在其中调用默认构造函数的其他位置（如在向量和映射中）。 请考虑此代码示例，对它需要“空白应用(C++/WinRT)”  项目。

```cppwinrt
std::map<int, TextBlock> lookup;
lookup[2] = value;
```

进行分配会创建新 TextBlock  ，然后立即使用 `value` 覆盖它。 下面是补救措施。

```cppwinrt
std::map<int, TextBlock> lookup;
lookup.insert_or_assign(2, value);
```

另请参阅[默认构造函数如何影响集合](/windows/uwp/cpp-and-winrt-apis/move-to-winrt-from-cx#how-the-default-constructor-affects-collections)。

### <a name="dont-delay-initialize-by-mistake"></a>不要错误地延迟初始化

请注意不要错误地调用 **std:: nullptr_t** 构造函数。 编译器的冲突解决偏向于它而非工厂构造函数。 例如，请考虑这两个运行时类定义。

```idl
// GiftBox.idl
runtimeclass GiftBox
{
    GiftBox();
}

// Gift.idl
runtimeclass Gift
{
    Gift(GiftBox giftBox); // You can create a gift inside a box.
}
```

假设我们想要构造一个不在盒子内的 **Gift**（使用未初始化的 **GiftBox** 构造的 **Gift**）。 首先，让我们看看错误  的做法。 我们知道有一个接受 **GiftBox** 的 **Gift** 构造函数。 但是，如果我们想要传递 null **GiftBox**（通过统一初始化调用 **Gift** 构造函数，如下所示），那么我们不会  获得我们想要的结果。

```cppwinrt
// These are *not* what you intended. Doing it in one of these two ways
// actually *doesn't* create the intended backing Windows Runtime Gift object;
// only an empty smart pointer.

Gift gift{ nullptr };
auto gift{ Gift(nullptr) };
```

在此处得到的是一个未初始化的 **Gift**。 你无法通过未初始化的 **GiftBox** 得到 **Gift**。 下面是正确  的做法。

```cppwinrt
// Doing it in one of these two ways creates an initialized
// Gift with an uninitialized GiftBox.

Gift gift{ GiftBox{ nullptr } };
auto gift{ Gift(GiftBox{ nullptr }) };
```

在不正确的示例中，传递 `nullptr` 文本会以有利于延迟初始化构造函数的方式解析。 若要以有利于工厂构造函数的方式解析，参数的类型必须是 **GiftBox**。 仍然可以选择传递一个显式延迟初始化 **GiftBox**，如正确的示例中所示。

下一个示例也正确  ，因为参数的类型为 GiftBox，而不是 **std:: nullptr_t**。

```cppwinrt
GiftBox giftBox{ nullptr };
Gift gift{ giftBox }; // Calls factory constructor.
```

仅当传递 `nullptr` 文本时才会引起多义性。

## <a name="dont-copy-construct-by-mistake"></a>不要错误地复制构造。

此警告类似于上面的[不要错误地延迟初始化](#dont-delay-initialize-by-mistake)部分中所述的警告。

除了该延迟初始化构造函数之外，C++/WinRT 投影也会将复制构造函数注入到每个运行时类中。 它是一个单参数构造函数，接受与所构造对象相同的类型。 生成的智能指针指向其构造函数参数指向的同一后备 Windows 运行时对象。 结果是两个智能指针对象指向同一后备对象。

下面是我们将在代码示例中使用的运行时类定义。

```idl
// GiftBox.idl
runtimeclass GiftBox
{
    GiftBox(GiftBox biggerBox); // You can place a box inside a bigger box.
}
```

假设我们想要在一个较大的 **GiftBox** 内构造 **GiftBox**。

```cppwinrt
GiftBox bigBox{ ... };

// These are *not* what you intended. Doing it in one of these two ways
// copies bigBox's backing-object-pointer into smallBox.
// The result is that smallBox == bigBox.

GiftBox smallBox{ bigBox };
auto smallBox{ GiftBox(bigBox) };
```

 正确的做法是显式调用激活工厂。

```cppwinrt
GiftBox bigBox{ ... };

// These two ways call the activation factory explicitly.

GiftBox smallBox{
    winrt::get_activation_factory<GiftBox, IGiftBoxFactory>().CreateInstance(bigBox) };
auto smallBox{
    winrt::get_activation_factory<GiftBox, IGiftBoxFactory>().CreateInstance(bigBox) };
```

## <a name="if-the-api-is-implemented-in-a-windows-runtime-component"></a>如果在 Windows 运行时组件中实现此 API
无论你是自行创作该组件，还是该组件来自供应商，本部分均适用。

> [!NOTE]
> 有关安装和使用 C++/WinRT Visual Studio 扩展 (VSIX) 和 NuGet 包（两者共同提供项目模板，并生成支持）的信息，请参阅[适用于 C++/WinRT 的 Visual Studio 支持](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)。

在应用程序项目中，引用 Windows 运行时组件的 Windows 运行时元数据 (`.winmd`) 文件，然后生成。 在生成过程中，`cppwinrt.exe` 工具生成标准 C++ 库，该库全面描述（或投影  ）该组件的 API 接口。 换言之，生成的库包含该组件的投影类型。

然后，与 Windows 命名空间类型一样，你只需包含标头并通过其构造函数之一构造投影类型。 应用程序项目的启动代码注册运行时类，然后投影类型的构造函数调用 [RoActivateInstance  ](https://docs.microsoft.com/windows/desktop/api/roapi/nf-roapi-roactivateinstance) 来激活引用组件中的运行时类。

```cppwinrt
#include <winrt/BankAccountWRC.h>

struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    BankAccountWRC::BankAccount bankAccount;
    ...
};
```

有关更多详细信息、代码以及使用在 Windows 运行时组件实现的 API 的演练，请参阅[使用 C++/WinRT 创作事件](author-events.md#create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component)。

## <a name="if-the-api-is-implemented-in-the-consuming-project"></a>如果在使用的项目中实现 API
通过 XAML UI 使用的类型必须为运行时类，即使其位于与 XAML 相同的项目中。

在这种情况下，从运行时类的 Windows 运行时元数据 (`.winmd`) 中生成一个投影类型。 再次包含一个头文件，但这次是通过其 **std::nullptr_t** 构造函数来构造投影类型。 该构造函数不执行任何初始化，所以你接下来必须通过 [winrt::make  ](/uwp/cpp-ref-for-winrt/make) 帮助程序函数向该实例分配一个值，同时传递任何必要的构造函数参数。 在使用代码的同一项目中实现的运行时类无需进行注册，且无需通过 Windows 运行时/COM 激活进行实例化。

对于此代码示例，需要“空白应用(C++/WinRT)”  项目。

```cppwinrt
// MainPage.h
...
struct MainPage : MainPageT<MainPage>
{
    ...
    private:
        Bookstore::BookstoreViewModel m_mainViewModel{ nullptr };
        ...
    };
}
...
// MainPage.cpp
...
#include "BookstoreViewModel.h"

MainPage::MainPage()
{
    m_mainViewModel = winrt::make<Bookstore::implementation::BookstoreViewModel>();
    ...
}
```

有关更多详细信息、代码以及使用在使用的项目中实现的运行时类的演练，请参阅 [XAML 控件；绑定到 C++/WinRT 属性](binding-property.md#add-a-property-of-type-bookstoreviewmodel-to-mainpage)。

## <a name="instantiating-and-returning-projected-types-and-interfaces"></a>实例化和返回投影类型和接口
以下投影类型和实例的示例可能类似于使用的项目。 请记住，投影类型（如此示例中的一种投影类型）是工具生成的，而不是你自己创作的内容。

```cppwinrt
struct MyRuntimeClass : MyProject::IMyRuntimeClass, impl::require<MyRuntimeClass,
    Windows::Foundation::IStringable, Windows::Foundation::IClosable>
```

MyRuntimeClass  是投影类型；投影接口包括 IMyRuntimeClass  、IStringable  和 IClosable  。 本主题已展示你可用来实例化投影类型的不同方法。 以下是提醒和总结，这里使用 MyRuntimeClass  作为示例。

```cppwinrt
// The runtime class is implemented in another compilation unit (it's either a Windows API,
// or it's implemented in a second- or third-party component).
MyProject::MyRuntimeClass myrc1;

// The runtime class is implemented in the same compilation unit.
MyProject::MyRuntimeClass myrc2{ nullptr };
myrc2 = winrt::make<MyProject::implementation::MyRuntimeClass>();
```

- 你可以访问一个投影类型的所有接口的成员。
- 可以将投影类型返回到调用方。
- 投影类型和接口派生自 [winrt::Windows::Foundation::IUnknown  ](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown)。 因此，你可以对投影类型或接口调用 [IUnknown::as  ](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) 以查询其他也可使用或返回到调用方的投影接口。 as  成员函数的工作方式类似于 [QueryInterface  ](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_))。

```cppwinrt
void f(MyProject::MyRuntimeClass const& myrc)
{
    myrc.ToString();
    myrc.Close();
    IClosable iclosable = myrc.as<IClosable>();
    iclosable.Close();
}
```

## <a name="activation-factories"></a>激活工厂
创建 C++/WinRT 对象的便利直接的方式如下所示。

```cppwinrt
using namespace winrt::Windows::Globalization::NumberFormatting;
...
CurrencyFormatter currency{ L"USD" };
```

不过，有时你可能想要自己创建激活工厂，然后在方便时从其创建对象。 下面的一些示例向你展示了如何使用 [winrt::get_activation_factory  ](/uwp/cpp-ref-for-winrt/get-activation-factory) 函数模板来实现此目的。

```cppwinrt
using namespace winrt::Windows::Globalization::NumberFormatting;
...
auto factory = winrt::get_activation_factory<CurrencyFormatter, ICurrencyFormatterFactory>();
CurrencyFormatter currency = factory.CreateCurrencyFormatterCode(L"USD");
```

```cppwinrt
using namespace winrt::Windows::Foundation;
...
auto factory = winrt::get_activation_factory<Uri, IUriRuntimeClassFactory>();
Uri account = factory.CreateUri(L"http://www.contoso.com");
```

上面两个示例中的类是来自 Windows 命名空间的类型。 在接下来的示例中，BankAccountWRC::BankAccount  是在 Windows 运行时组件中实现的自定义类型。

```cppwinrt
auto factory = winrt::get_activation_factory<BankAccountWRC::BankAccount>();
BankAccountWRC::BankAccount account = factory.ActivateInstance<BankAccountWRC::BankAccount>();
```

## <a name="membertype-ambiguities"></a>成员/类型多义性

当成员函数的名称与类型的名称相同时，会产生多义性。 根据 C++ 的在成员函数中进行非限定名称查找的规则，必须先搜索类，然后才能在命名空间中进行搜索。  “替换失败不是错误 (SFINAE)”规则不适用（在对函数模板进行重载解析时适用）。 因此，如果类中的名称没有意义，则编译器不会继续查找更好的匹配&mdash;它会直接报告一个错误。

```cppwinrt
struct MyPage : Page
{
    void DoWork()
    {
        // This doesn't compile. You get the error
        // "'winrt::Windows::Foundation::IUnknown::as':
        // no matching overloaded function found".
        auto style{ Application::Current().Resources().
            Lookup(L"MyStyle").as<Style>() };
    }
}
```

在上面的代码中，编译器认为你是在将 [**FrameworkElement.Style()** ](/uwp/api/windows.ui.xaml.frameworkelement.style)（这在 C++/WinRT 中是成员函数）作为模板参数传递给 [**IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)。 解决方案是将名称 `Style` 强制解释为类型 [**Windows::UI::Xaml::Style**](/uwp/api/windows.ui.xaml.style)。

```cppwinrt
struct MyPage : Page
{
    void DoWork()
    {
        // One option is to fully-qualify it.
        auto style{ Application::Current().Resources().
            Lookup(L"MyStyle").as<Windows::UI::Xaml::Style>() };

        // Another is to force it to be interpreted as a struct name.
        auto style{ Application::Current().Resources().
            Lookup(L"MyStyle").as<struct Style>() };

        // If you have "using namespace Windows::UI;", then this is sufficient.
        auto style{ Application::Current().Resources().
            Lookup(L"MyStyle").as<Xaml::Style>() };

        // Or you can force it to be resolved in the global namespace (into which
        // you imported the Windows::UI::Xaml namespace when you did
        // "using namespace Windows::UI::Xaml;".
        auto style = Application::Current().Resources().
            Lookup(L"MyStyle").as<::Style>();
    }
}
```

非限定名称查找有一个特殊的例外，即，名称后面跟有 `::`，在这种情况下，它会忽略函数、变量和枚举值。 这样你就可以执行如下所示的代码。

```cppwinrt
struct MyPage : Page
{
    void DoSomething()
    {
        Visibility(Visibility::Collapsed); // No ambiguity here (special exception).
    }
}
```

对 `Visibility()` 的调用会解析为 [**UIElement.Visibility**](/uwp/api/windows.ui.xaml.uielement.visibility) 成员函数名称。 但是参数 `Visibility::Collapsed` 在 `Visibility` 一词后面跟有 `::`，因此系统会忽略方法名称，编译器会查找枚举类。

## <a name="important-apis"></a>重要的 API
* [QueryInterface 接口](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_))
* [RoActivateInstance 函数](https://docs.microsoft.com/windows/desktop/api/roapi/nf-roapi-roactivateinstance)
* [Windows::Foundation::Uri 类](/uwp/api/windows.foundation.uri)
* [winrt::get_activation_factory 函数模板](/uwp/cpp-ref-for-winrt/get-activation-factory)
* [winrt::make 函数模板](/uwp/cpp-ref-for-winrt/make)
* [winrt::Windows::Foundation::IUnknown 结构](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown)

## <a name="related-topics"></a>相关主题
* [在 C++/WinRT 中创作事件](author-events.md#create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component)
* [实现 C++/WinRT 与 ABI 之间的互操作](interop-winrt-abi.md)
* [C++/WinRT 简介](intro-to-using-cpp-with-winrt.md)
* [XAML 控件; 绑定到 C++/WinRT 属性](binding-property.md#add-a-property-of-type-bookstoreviewmodel-to-mainpage)
