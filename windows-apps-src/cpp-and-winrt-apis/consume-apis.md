---
author: stevewhims
description: 本主题介绍如何使用 C++/WinRT API，无论它们是由 Windows、第三方组件供应商功或自行实现。
title: 通过 C++/WinRT 使用 API
ms.author: stwhi
ms.date: 04/18/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 标准, c++, cpp, winrt, 投影的, 投影, 实现, 运行时类, 激活
ms.localizationpriority: medium
ms.openlocfilehash: e777aca8f1d9f3892f67b10c785c056b14c8070c
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/03/2018
ms.locfileid: "1832241"
---
# <a name="consume-apis-with-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>通过 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 使用 API
> [!NOTE]
> **与在商业发行之前可能会进行实质性修改的预发布产品相关的一些信息。 Microsoft 对于此处提供的信息不作任何明示或默示的担保。**

本主题介绍如何使用 C++/WinRT API，无论它们是 Windows 的一部分、由第三方组件供应商或自行实现。

## <a name="if-the-api-is-in-a-windows-namespace"></a>如果 API 位于 Windows 命名空间中
这是你使用 Windows 运行时 API 最常见的情况。 以下是一个简单的代码示例。

```cppwinrt
#include <winrt/Windows.Foundation.h>

using namespace winrt;
using namespace Windows::Foundation;

int main()
{
    winrt::init_apartment();
    Uri contosoUri{ L"http://www.contoso.com" };
}
```

包含的标头 `winrt/Windows.Foundation.h` 是 SDK 的一部分，可在文件夹 `%WindowsSdkDir%Include<WindowsTargetPlatformVersion>\cppwinrt\winrt\` 内找到。 该文件夹中的标头包含投影到 C++/WinRT 中的 Windows API。 如果希望使用来自 Windows 命名空间的类型，请包括与该命名空间对应的 C++/WinRT 投影标头。 `using namespace`指令是可选的，不过这种指令很方便。

在此示例中，`winrt/Windows.Foundation.h` 包含运行时类 [**Windows::Foundation::Uri**](/uwp/api/windows.foundation.uri) 的投影类型。

> [!TIP]
> *投影类型*是出于使用运行时类的 API 针对运行时类的包装器。 *投影接口*是针对 Windows 运行时接口的包装器。

在上述代码示例中，在初始化 C++/WinRT 后，我们将通过其公开记录的构造函数之一（本示例中为 [**Uri(字符串)**](/uwp/api/windows.foundation.uri#Windows_Foundation_Uri__ctor_System_String_)）构造 **Uri** 投影类型。 为此，最常见用例就是这是你所要做的。

## <a name="if-the-api-is-implemented-in-a-windows-runtime-component"></a>如果在 Windows 运行时组件中实现此 API
无论你是自行创作该组件，还是该组件来自供应商，本部分均适用。

> [!NOTE]
> 有关 C++/WinRT Visual Studio Extension (VSIX)（提供项目模板支持以及 C++/WinRT MSBuild 属性和目标）的当前可用性的信息，请参阅 [Visual Studio 支持 C++/WinRT 和 VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix)。

在应用程序项目中，引用 Windows 运行时组件的 Windows 运行时元数据 (`.winmd`) 文件，然后生成。 在生成过程中，`cppwinrt.exe` 工具生成标准 C++ 库，该库全面描述&mdash;或*投影*&mdash;该组件的 API 接口。 换言之，生成的库包含该组件的投影类型。

然后，与 Windows 命名空间类型一样，你只需包含标头并通过其构造函数之一构造投影类型。 应用程序项目的启动代码注册运行时类，然后投影类型的构造函数调用 [**RoActivateInstance**](https://msdn.microsoft.com/library/br224646) 来激活引用组件中的运行时类。

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

在这种情况下，从运行时类的 Windows 运行时元数据 (`.winmd`) 中生成一个投影类型。 两样，包含一个标头，但这次是通过其 `nullptr` 构造函数构造投影类型。 该构造函数不执行任何初始化，所以你接下来必须通过 [**winrt::make**](/uwp/cpp-ref-for-winrt/make) 帮助程序函数向该实例分配一个值，同时传递任何必要的构造函数参数。 在使用代码的同一项目中实现的运行时类无需进行注册，且无需通过 Windows 运行时/COM 激活进行实例化。

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
    m_mainViewModel = make<Bookstore::implementation::BookstoreViewModel>();
    ...
}
```

有关更多详细信息、代码以及使用在使用的项目中实现的运行时类的演练，请参阅 [XAML 控件; 绑定到 C++/WinRT 属性](binding-property.md#add-a-property-of-type-bookstoreviewmodel-to-mainpage)。

## <a name="instantiating-and-returning-projected-types-and-interfaces"></a>实例化和返回投影类型和接口
以下投影类型和实例的示例可能类似于使用的项目。

```cppwinrt
struct MyRuntimeClass : MyProject::IMyRuntimeClass, impl::require<MyRuntimeClass,
    Windows::Foundation::IStringable, Windows::Foundation::IClosable>
```

**MyRuntimeClass** 是投影类型；投影接口包括 **IMyRuntimeClass**、**IStringable** 和 **IClosable**。 本主题已展示你可用来实例化投影类型的不同方法。 以下是提醒和总结，这里使用 **MyRuntimeClass** 作为示例。

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
- 投影类型和接口派生自 [**winrt::Windows::Foundation::IUnknown**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown)。 因此，你可以对投影类型或接口调用 [**IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) 以查询其他也可使用或返回到调用方的投影接口。 **as** 成员函数的工作方式类似于 [**QueryInterface**](https://msdn.microsoft.com/library/windows/desktop/ms682521)。

```cppwinrt
void f(MyProject::MyRuntimeClass const& myrc)
{
    myrc.ToString();
    myrc.Close();
    IClosable iclosable = myrc.as<IClosable>();
    iclosable.Close();
}
```

## <a name="important-apis"></a>重要的 API
* [winrt::make 函数模板](/uwp/cpp-ref-for-winrt/make)
* [winrt::Windows::Foundation::IUnknown::as](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)

## <a name="related-topics"></a>相关主题
* [在 C++/WinRT 中创作事件](author-events.md#create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component)
* [XAML 控件; 绑定到 C++/WinRT 属性](binding-property.md#add-a-property-of-type-bookstoreviewmodel-to-mainpage)
