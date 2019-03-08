---
description: 可有效地绑定到 XAML 项目控件的属性称为*可观测*属性。 本主题介绍如何实现和使用可观测属性以及如何将 XAML 控件绑定到该属性。
title: XAML 控件; 绑定到 C++/WinRT 属性
ms.date: 08/21/2018
ms.topic: article
keywords: windows 10, uwp, 标准, c++, cpp, winrt, 投影, XAML, 控件, 绑定, 属性
ms.localizationpriority: medium
ms.openlocfilehash: 4033327fa51b0801583a518a0dea055f59e57fc8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57616622"
---
# <a name="xaml-controls-bind-to-a-cwinrt-property"></a>XAML 控件; 绑定到 C++/WinRT 属性
可有效地绑定到 XAML 项目控件的属性称为*可观测*属性。 这一想法基于称为*观察者模式*的软件设计模式。 本主题演示如何实现可观察量属性中的[C + + WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)，以及如何将 XAML 控件绑定到它们。

> [!IMPORTANT]
> 有关支持你了解如何利用 C++/WinRT 来使用和创作运行时类的基本概述和术语，请参阅[通过 C++/WinRT 使用 API](consume-apis.md) 和[通过 C++/WinRT 创作 API](author-apis.md)。

## <a name="what-does-observable-mean-for-a-property"></a>对于属性来说，*可观测*意味着什么？
假设名为 **BookSku** 的运行时类有一个名为**标题**的属性。 如果 **BookSku** 选择每当**标题**的值发生更改时引发 [**INotifyPropertyChanged::PropertyChanged**](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged) 事件，则**标题**为一个可观测属性。 **BookSku** 的行为（引发或未引发该事件）确定其哪些属性（如果有）是可观测的。

XAML 文本元素或控件可检索更新的值然后将自行更新以显示新值，从而绑定到这些事件并处理事件。

> [!NOTE]
> 了解安装和使用 C + + WinRT Visual Studio 扩展 (VSIX) （可提供项目模板支持） 请参阅[Visual Studio 支持 C + + WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)。

## <a name="create-a-blank-app-bookstore"></a>创建空白应用 (Bookstore)
首先在 Microsoft Visual Studio 中创建新项目。 创建**Visual c + +** > **Windows Universal** > **空白应用 (C + + WinRT)** 项目，并将其命名*书店*.

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
> 在视图模型类&mdash;事实上，在你的应用程序中声明任何运行时类&mdash;不需要派生自的基类。 **BookSku**上面已声明的类是一个示例说明。 它实现了接口，但它不会从任何基类派生。
>
> 在应用程序中声明任何运行时类的*does*从基类派生类被称为*可组合*类。 并且，围绕可组合类的约束。 应用程序传递[Windows 应用认证工具包](../debug-test-perf/windows-app-certification-kit.md)测试用于通过 Visual Studio 和 Microsoft Store 验证提交 (并因此成功引入到 Microsoft Store 应用程序)、可组合类最终必须从 Windows 的基类派生。 这意味着非常根处的继承层次结构的类必须源自 windows.* 命名空间的类型。 如果需要运行时类派生自的基类&mdash;例如，若要实现**BindableBase**的所有视图模型，用于从派生类&mdash;则可以从派生[ **Windows.UI.Xaml.DependencyObject**](/uwp/api/windows.ui.xaml.dependencyobject)。
>
> 视图模型是视图的抽象，因此它直接绑定到视图 （XAML 标记）。 数据模型是抽象的数据，并且它仅从您的视图模型，使用并不直接绑定到 XAML。 因此，您可以声明你的数据模型不是作为运行时类，而作为 c + + 结构或类。 它们无需在 MIDL 中, 声明，您可以随意使用任何您喜欢的继承层次结构。

保存文件并生成项目。 在生成过程中，`midl.exe` 工具会运行以创建描述该运行时类的 Windows 运行时元数据文件 (`\Bookstore\Debug\Bookstore\Unmerged\BookSku.winmd`)。 然后，`cppwinrt.exe` 工具运行以生成源代码文件，从而为你在创作和使用运行时类时提供支持。 这些文件包含让你开始实现已在 IDL 中声明的 **BookSku**运行时类的存根。 这些存根是 `\Bookstore\Bookstore\Generated Files\sources\BookSku.h` 和 `BookSku.cpp`。

右键单击项目节点，然后单击**在文件资源管理器中打开文件夹**。 这将在文件资源管理器中打开项目文件夹。 将存根 （stub） 文件，复制`BookSku.h`并`BookSku.cpp`从`\Bookstore\Bookstore\Generated Files\sources\`文件夹和到项目文件夹中，即`\Bookstore\Bookstore\`。 在 **“解决方案资源管理器”** 中，请确保将 **“显示所有文件”** 切换为打开。 右键单击已复制的存根文件，然后单击**包括在项目中**。

## <a name="implement-booksku"></a>实现 **BookSku**
现在，让我们打开 `\Bookstore\Bookstore\BookSku.h` 和 `BookSku.cpp` 并实现运行时类。 在 `BookSku.h` 中，添加一个采用 [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) 的构造函数、一个用于存储标题字符串的私有成员以及另一个用于标题发生更改时我们将引发的事件的私有成员。 进行这些更改之后, 你`BookSku.h`将如下所示。

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

在中**标题**转变器函数，我们检查是否正在设置值不同于当前值。 如果存在，我们更新的标题，并且还会引发[ **INotifyPropertyChanged::PropertyChanged** ](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged)与已更改的属性的名称相同的参数的事件。 这样，用户界面 (UI) 将知道要重新查询的属性的值。

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

保存并生成。 将 `BookstoreViewModel.h` 和 `BookstoreViewModel.cpp` 从 `Generated Files` 文件夹复制到项目文件夹中，然后将其包含在项目中。 打开这些文件，并实现运行时类，如下所示。 请注意如何，请在`BookstoreViewModel.h`，包含`BookSku.h`，其声明的实现类型 (**winrt::Bookstore::implementation::BookSku**)。 我们要通过删除还原的默认构造函数和`= delete`。

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
        m_bookSku = winrt::make<Bookstore::implementation::BookSku>(L"Atticus");
    }

    Bookstore::BookSku BookstoreViewModel::BookSku()
    {
        return m_bookSku;
    }
}
```

> [!NOTE]
> 类型`m_bookSku`是提取的类型 (**winrt::Bookstore::BookSku**)，并使用与模板参数[ **winrt::make** ](/uwp/cpp-ref-for-winrt/make)是实现类型 (**winrt::Bookstore::implementation::BookSku**)。 即使如此，**make** 也会返回投影类型的实例。

## <a name="add-a-property-of-type-bookstoreviewmodel-to-mainpage"></a>将类型 **BookstoreViewModel** 的属性添加到 **MainPage**
打开 `MainPage.idl`，这将声明表示主 UI 页面的运行时类。 添加导入语句以导入 `BookstoreViewModel.idl`，然后添加名为类型 **BookstoreViewModel** 的 MainViewModel 的只读属性。 此外删除**MyProperty**属性。 另请注意`import`指令下面的列表中。

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

保存文件。 项目将不会生成完成时间，但现在构建是有意义的事情要做，因为它会在其中的源代码文件重新生成**MainPage**运行时类实现 (`\Bookstore\Bookstore\Generated Files\sources\MainPage.h`和`MainPage.cpp`)。 请继续并立即生成。 您有望看到在此阶段生成错误的原因在于**MainViewModel： 不是成员 winrt::Bookstore::implementation::MainPage**。

如果省略的 include `BookstoreViewModel.idl` (请参阅的列表中`MainPage.idl`上方)，然后您将看到错误**应为\<附近"MainViewModel"**。 请确保将所有类型都保留在同一个命名空间中另一个提示是： 在代码清单所示的命名空间。

若要解决我们希望看到的错误，您现在需要将复制取值函数的存根**MainViewModel**超出所生成的文件的属性 (`\Bookstore\Bookstore\Generated Files\sources\MainPage.h`并`MainPage.cpp`) 和到`\Bookstore\Bookstore\MainPage.h`和`MainPage.cpp`。

在中`\Bookstore\Bookstore\MainPage.h`，包括`BookstoreViewModel.h`，其声明的实现类型 (**winrt::Bookstore::implementation::BookstoreViewModel**)。 添加私有成员来存储视图的模型。 请注意，属性访问器函数（添加成员 m_mainViewModel）是根据 **Bookstore::BookstoreViewModel** 实现的，后者是投影类型。 实现类型是同一个项目 （编译单元） 中的应用程序，因此我们构造了通过采用构造函数重载 m_mainViewModel `nullptr_t`。 此外删除**MyProperty**属性。

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

在中`\Bookstore\Bookstore\MainPage.cpp`，调用[ **winrt::make** ](/uwp/cpp-ref-for-winrt/make) （与实现类型） 将提取的类型的新实例分配给 m_mainViewModel。 为书籍的标题分配一个初始值。 针对 MainViewModel 属性实现访问器。 最后，在按钮的事件处理程序中更新书籍的标题。 此外删除**MyProperty**属性。

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
        m_mainViewModel = winrt::make<Bookstore::implementation::BookstoreViewModel>();
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
打开 `MainPage.xaml`，其中包含主 UI 页面的 XAML 标记。 下面的列表中所示，从按钮，删除名称并将更改其**内容**属性值从文本到绑定表达式。 记下绑定表达式上的 `Mode=OneWay` 属性（单向从视图模型到 UI）。 没有该属性，UI 将不会响应属性更改事件。

```xaml
<Button Click="ClickHandler" Content="{x:Bind MainViewModel.BookSku.Title, Mode=OneWay}"/>
```

立即生成并运行该项目。 单击该按钮以执行 **Click** 事件处理程序。 该处理程序调用书籍的标题转变器函数；该转变器引发了让 UI 知道**标题**属性已发生更改的事件；而且按钮重新查询了该属性的值以更新其自己的**内容**值。

## <a name="using-the-binding-markup-extension-with-cwinrt"></a>使用 {Binding} 标记扩展和 C + + WinRT
当前发行版本的 C + + / WinRT，以便可以使用 {Binding} 标记扩展，你将需要实现[ICustomPropertyProvider](/uwp/api/windows.ui.xaml.data.icustompropertyprovider)并[ICustomProperty](/uwp/api/windows.ui.xaml.data.icustomproperty)接口。

## <a name="important-apis"></a>重要的 API
* [INotifyPropertyChanged::PropertyChanged](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged)
* [winrt::make 函数模板](/uwp/cpp-ref-for-winrt/make)

## <a name="related-topics"></a>相关主题
* [使用 Api 使用 C + + WinRT](consume-apis.md)
* [创作 Api 使用 C + + WinRT](author-apis.md)
