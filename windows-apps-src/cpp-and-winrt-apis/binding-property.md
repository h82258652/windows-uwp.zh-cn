---
author: stevewhims
description: 可有效地绑定到 XAML 项目控件的属性称为*可观测*属性。 本主题介绍如何实现和使用可观测属性以及如何将 XAML 控件绑定到该属性。
title: XAML 控件; 绑定到 C++/WinRT 属性
ms.author: stwhi
ms.date: 08/21/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 标准, c++, cpp, winrt, 投影, XAML, 控件, 绑定, 属性
ms.localizationpriority: medium
ms.openlocfilehash: 31913ae162bfe541d04f304db87b4dff962a8af4
ms.sourcegitcommit: 753dfcd0f9fdfc963579dd0b217b445c4b110a18
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/27/2018
ms.locfileid: "2862864"
---
# <a name="xaml-controls-bind-to-a-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt-property"></a>XAML 控件; 绑定到 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 属性
可有效地绑定到 XAML 项目控件的属性称为*可观测*属性。 这一想法基于称为*观察者模式*的软件设计模式。 本主题介绍如何在 C++/WinRT 中实现可观测属性以及如何将 XAML 控件绑定到这些属性。

> [!IMPORTANT]
> 有关支持你了解如何利用 C++/WinRT 来使用和创作运行时类的基本概述和术语，请参阅[通过 C++/WinRT 使用 API](consume-apis.md) 和[通过 C++/WinRT 创作 API](author-apis.md)。

## <a name="what-does-observable-mean-for-a-property"></a>对于属性来说，*可观测*意味着什么？
假设名为 **BookSku** 的运行时类有一个名为**标题**的属性。 如果 **BookSku** 选择每当**标题**的值发生更改时引发 [**INotifyPropertyChanged::PropertyChanged**](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged) 事件，则**标题**为一个可观测属性。 **BookSku** 的行为（引发或未引发该事件）确定其哪些属性（如果有）是可观测的。

XAML 文本元素或控件可检索更新的值然后将自行更新以显示新值，从而绑定到这些事件并处理事件。

> [!NOTE]
> 有关 C++/WinRT Visual Studio Extension (VSIX)（提供项目模板支持以及 C++/WinRT MSBuild 属性和目标）的安装和使用的信息，请参阅[针对 C++/WinRT 以及 VSIX 的 Visual Studio 支持](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix)。

## <a name="create-a-blank-app-bookstore"></a>创建空白应用 (Bookstore)
首先在 Microsoft Visual Studio 中创建新项目。 创建 **Visual C++ 空白应用 (C++/WinRT)** 项目，然后将其命名为 *Bookstore*。

我们将创作新类来表示具有可观测标题属性的书籍。 我们正在同一编译单元内创作和使用该类。 但我们希望能够从 XAML 绑定到此类，因此，它将成为一个运行时类。 而且我们将使用 C++/WinRT 来创作和使用它。

创作新的运行时类的第一步是将新的 **Midl 文件(.idl)** 项添加到项目。 将其命名为 `BookSku.idl`。 删除 `BookSku.idl` 的默认内容，然后粘贴到此运行时类声明中。

```idl
// BookSku.idl
namespace Bookstore
{
    runtimeclass BookSku : Windows.UI.Xaml.Data.INotifyPropertyChanged
    {
        String Title;
    }
}
```

> [!NOTE]
> 查看模型的类&mdash;实际上，任何您在您的应用程序中声明的运行时类&mdash;不需要一个基类派生。 声明上方的**BookSku**类是该示例。 它实现接口，但它并不从任何基类派生。
>
> 应用程序中声明的任何运行时类*执行*从基础派生类称为*可组合*类。 并且没有围绕可组合类的约束。 要通过使用 Visual Studio 和 Microsoft 存储验证提交的[Windows 应用程序证书工具包](../debug-test-perf/windows-app-certification-kit.md)测试应用程序 (因此要成功为 ingested 到 Microsoft 存储的应用程序)，可组合类必须最终从 Windows 基类派生。 这意味着 very 根目录的类继承层次结构的必须源自 Windows.* 命名空间的类型。 如果需要从基类派生的运行时类&mdash;例如，若要实现**BindableBase**类派生从您查看模型的所有&mdash;，然后您可以从[**Windows.UI.Xaml.DependencyObject**](/uwp/api/windows.ui.xaml.dependencyobject)派生。
>
> 视图模型是视图的抽象，因此它直接绑定到视图 （XAML 标记）。 数据模型是数据的抽象时，它具有消耗只能从您的视图模型，并且未绑定到 XAML 直接。 因此，您可以在不运行时类，但为 c + + 结构或类声明数据模型。 这些人不需要在 MIDL 中, 声明，您可以自由地使用任何您喜欢的继承层次结构。

保存文件并生成项目。 在生成过程中，`midl.exe` 工具会运行以创建描述该运行时类的 Windows 运行时元数据文件 (`\Bookstore\Debug\Bookstore\Unmerged\BookSku.winmd`)。 然后，`cppwinrt.exe` 工具运行以生成源代码文件，从而为你在创作和使用运行时类时提供支持。 这些文件包含让你开始实现已在 IDL 中声明的 **BookSku**运行时类的存根。 这些存根是 `\Bookstore\Bookstore\Generated Files\sources\BookSku.h` 和 `BookSku.cpp`。

将这些存根文件 `BookSku.h` 和 `BookSku.cpp` 从 `\Bookstore\Bookstore\Generated Files\sources\` 复制到项目文件夹中，即 `\Bookstore\Bookstore\`。 在**解决方案资源管理器**中，确保将**显示所有文件**切换为打开。 右键单击已复制的存根文件，然后单击**包括在项目中**。

## <a name="implement-booksku"></a>实现 **BookSku**
现在，让我们打开 `\Bookstore\Bookstore\BookSku.h` 和 `BookSku.cpp` 并实现运行时类。 在 `BookSku.h` 中，添加一个采用 [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) 的构造函数、一个用于存储标题字符串的私有成员以及另一个用于标题发生更改时我们将引发的事件的私有成员。 这些更改后您`BookSku.h`将如下所示。

```cppwinrt
// BookSku.h
#pragma once

#include "BookSku.g.h"

namespace winrt::Bookstore::implementation
{
    struct BookSku : BookSkuT<BookSku>
    {
        BookSku() = delete;
        BookSku(winrt::hstring const& title);

        winrt::hstring Title();
        void Title(winrt::hstring const& value);
        winrt::event_token PropertyChanged(Windows::UI::Xaml::Data::PropertyChangedEventHandler const& value);
        void PropertyChanged(winrt::event_token const& token);
    
    private:
        winrt::hstring m_title;
        winrt::event<Windows::UI::Xaml::Data::PropertyChangedEventHandler> m_propertyChanged;
    };
}
```

在 `BookSku.cpp` 中，实现如下所示的函数。

```cppwinrt
// BookSku.cpp
#include "pch.h"
#include "BookSku.h"

namespace winrt::Bookstore::implementation
{
    BookSku::BookSku(winrt::hstring const& title) : m_title{ title }
    {
    }

    winrt::hstring BookSku::Title()
    {
        return m_title;
    }

    void BookSku::Title(winrt::hstring const& value)
    {
        if (m_title != value)
        {
            m_title = value;
            m_propertyChanged(*this, Windows::UI::Xaml::Data::PropertyChangedEventArgs{ L"Title" });
        }
    }

    winrt::event_token BookSku::PropertyChanged(Windows::UI::Xaml::Data::PropertyChangedEventHandler const& handler)
    {
        return m_propertyChanged.add(handler);
    }

    void BookSku::PropertyChanged(winrt::event_token const& token)
    {
        m_propertyChanged.remove(token);
    }
}
```

在**标题**转变器函数中，我们检查是否设置了某个值正在以外的当前值。 和，如果是这样，我们更新标题也引发等于已更改属性的名称的参数的[**INotifyPropertyChanged::PropertyChanged**](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged)事件。 这样，用户界面 (UI) 将知道要重新查询的属性的值。

## <a name="declare-and-implement-bookstoreviewmodel"></a>声明并实现 **BookstoreViewModel**
主 XAML 页面将绑定到主视图模型。 而且该视图模型将有多个属性，包括其中一个类型 **BookSku**。 在此步骤中，我们将声明并实现主视图模型运行时类。

添加名为 `BookstoreViewModel.idl` 的新的 **Midl 文件(.idl)** 项。

```idl
// BookstoreViewModel.idl
import "BookSku.idl";

namespace Bookstore
{
    runtimeclass BookstoreViewModel
    {
        BookSku BookSku{ get; };
    }
}
```

保存并生成。 将 `BookstoreViewModel.h` 和 `BookstoreViewModel.cpp` 从 `Generated Files` 文件夹复制到项目文件夹中，然后将其包含在项目中。 打开这些文件并实现运行时类，如下所示。 注意如何，在`BookstoreViewModel.h`，我们正在包括`BookSku.h`，其声明的实现类型 (**winrt::Bookstore::implementation::BookSku**)。 我们从中删除还原默认构造函数和`= delete`。

```cppwinrt
// BookstoreViewModel.h
#pragma once

#include "BookstoreViewModel.g.h"
#include "BookSku.h"

namespace winrt::Bookstore::implementation
{
    struct BookstoreViewModel final : BookstoreViewModelT<BookstoreViewModel>
    {
        BookstoreViewModel();

        Bookstore::BookSku BookSku();

    private:
        Bookstore::BookSku m_bookSku{ nullptr };
    };
}
```

```cppwinrt
// BookstoreViewModel.cpp
#include "pch.h"
#include "BookstoreViewModel.h"

namespace winrt::Bookstore::implementation
{
    BookstoreViewModel::BookstoreViewModel()
    {
        m_bookSku = make<Bookstore::implementation::BookSku>(L"Atticus");
    }

    Bookstore::BookSku BookstoreViewModel::BookSku()
    {
        return m_bookSku;
    }
}
```

> [!NOTE]
> `m_bookSku` 的类型是投影类型 (**winrt::Bookstore::BookSku**)，而且你用于 **make** 的模板参数是实现类型 (**winrt::Bookstore::implementation::BookSku**)。 即使如此，**make** 也会返回投影类型的实例。

## <a name="add-a-property-of-type-bookstoreviewmodel-to-mainpage"></a>将类型 **BookstoreViewModel** 的属性添加到 **MainPage**
打开 `MainPage.idl`，这将声明表示主 UI 页面的运行时类。 添加导入语句以导入 `BookstoreViewModel.idl`，然后添加名为类型 **BookstoreViewModel** 的 MainViewModel 的只读属性。 此外删除**MyProperty**属性。 还要注意`import`指令下面的列表中。

```idl
// MainPage.idl
import "BookstoreViewModel.idl";

namespace Bookstore
{
    runtimeclass MainPage : Windows.UI.Xaml.Controls.Page
    {
        MainPage();
        BookstoreViewModel MainViewModel{ get; };
    }
}
```

保存文件。 项目将不会生成结束时，但现在构建很有用的事情进行操作，因为它将在其中实现运行时到**MainPage**类的源代码文件重新生成 (`\Bookstore\Bookstore\Generated Files\sources\MainPage.h`和`MainPage.cpp`)。 因此，继续操作，并立即生成。 您可以预期在此阶段，请参阅生成错误是**MainViewModel': 不是 winrt::Bookstore::implementation::MainPage' 的成员**。

如果省略的包括`BookstoreViewModel.idl`(的列表，请参阅`MainPage.idl`上方)，然后您将看到错误**预期 \ < near"MainViewModel"**。 另一个提示是确保保留所有类型中的同一命名空间： 所示的代码清单的命名空间。

若要解决我们期望看到此错误，您现在需要复制超出生成的文件的**MainViewModel**属性的取值存根 (`\Bookstore\Bookstore\Generated Files\sources\MainPage.h`和`MainPage.cpp`) 和到`\Bookstore\Bookstore\MainPage.h`和`MainPage.cpp`。

在`\Bookstore\Bookstore\MainPage.h`，包括`BookstoreViewModel.h`，其声明的实现类型 (**winrt::Bookstore::implementation::BookstoreViewModel**)。 添加一个私有成员以存储视图模型。 请注意，属性访问器函数（添加成员 m_mainViewModel）是根据 **Bookstore::BookstoreViewModel** 实现的，后者是投影类型。 实现类型是同一项目 （编译单元） 中为该应用程序，因此我们构造通过所需的构造函数重载 m_mainViewModel `nullptr_t`。 此外删除**MyProperty**属性。

```cppwinrt
// MainPage.h
...
#include "BookstoreViewModel.h"
...
namespace winrt::Bookstore::implementation
{
    struct MainPage : MainPageT<MainPage>
    {
        MainPage();

        Bookstore::BookstoreViewModel MainViewModel();

        void ClickHandler(Windows::Foundation::IInspectable const&, Windows::UI::Xaml::RoutedEventArgs const&);

    private:
        Bookstore::BookstoreViewModel m_mainViewModel{ nullptr };
    };
}
...
```

在`\Bookstore\Bookstore\MainPage.cpp`，呼叫 （带有实现类型） [**winrt::make**](/uwp/cpp-ref-for-winrt/make) m_mainViewModel 中分配的计划类型的新实例。 为书籍的标题分配一个初始值。 针对 MainViewModel 属性实现访问器。 最后，在按钮的事件处理程序中更新书籍的标题。 此外删除**MyProperty**属性。

```cppwinrt
// MainPage.cpp
#include "pch.h"
#include "MainPage.h"

using namespace winrt;
using namespace Windows::UI::Xaml;

namespace winrt::Bookstore::implementation
{
    MainPage::MainPage()
    {
        m_mainViewModel = make<Bookstore::implementation::BookstoreViewModel>();
        InitializeComponent();
    }

    void MainPage::ClickHandler(Windows::Foundation::IInspectable const& /* sender */, Windows::UI::Xaml::RoutedEventArgs const& /* args */)
    {
        MainViewModel().BookSku().Title(L"To Kill a Mockingbird");
    }

    Bookstore::BookstoreViewModel MainPage::MainViewModel()
    {
        return m_mainViewModel;
    }
}
```

## <a name="bind-the-button-to-the-title-property"></a>将按钮绑定到**标题**属性
打开 `MainPage.xaml`，其中包含主 UI 页面的 XAML 标记。 下面的列表中所示，从按钮，删除名称，并将其**内容**的属性值从文本更改为绑定表达式。 记下绑定表达式上的 `Mode=OneWay` 属性（单向从视图模型到 UI）。 没有该属性，UI 将不会响应属性更改事件。

```xaml
<Button Click="ClickHandler" Content="{x:Bind MainViewModel.BookSku.Title, Mode=OneWay}"/>
```

立即生成并运行该项目。 单击该按钮以执行 **Click** 事件处理程序。 该处理程序调用书籍的标题转变器函数；该转变器引发了让 UI 知道**标题**属性已发生更改的事件；而且按钮重新查询了该属性的值以更新其自己的**内容**值。

## <a name="using-the-binding-markup-extension-with-cwinrt"></a>使用 {Binding} 标记扩展 C + + / WinRT
当前发行版本的 C + + / WinRT，以便能够使用 {绑定} 标记扩展需要实现[ICustomPropertyProvider](/uwp/api/windows.ui.xaml.data.icustompropertyprovider)和[ICustomProperty](/uwp/api/windows.ui.xaml.data.icustomproperty)接口。

## <a name="important-apis"></a>重要的 API
* [INotifyPropertyChanged::PropertyChanged](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged)
* [winrt::make 函数模板](/uwp/cpp-ref-for-winrt/make)

## <a name="related-topics"></a>相关主题
* [通过 C++/WinRT 使用 API](consume-apis.md)
* [使用 C++/WinRT 创作 API](author-apis.md)
