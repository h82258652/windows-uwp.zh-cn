---
description: 可有效地绑定到 XAML 项目控件的属性称为*可观测*属性。 本主题介绍了如何实现和使用可观测属性以及如何将 XAML 控件绑定到该属性。
title: XAML 控件；绑定到 C++/WinRT 属性
ms.date: 06/21/2019
ms.topic: article
keywords: windows 10, uwp, 标准, c++, cpp, winrt, 投影, XAML, 控件, 绑定, 属性
ms.localizationpriority: medium
ms.openlocfilehash: 06934c1c3b23c244fb32ffa957cffb926ffd1bb0
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "79209192"
---
# <a name="xaml-controls-bind-to-a-cwinrt-property"></a>XAML 控件；绑定到 C++/WinRT 属性
可有效地绑定到 XAML 项目控件的属性称为*可观测*属性。 这一想法基于称为“观察者模式”的软件设计模式  。 本主题介绍如何在 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 中实现可观测属性，以及如何将 XAML 控件绑定到这些属性（如需背景信息，请参阅[数据绑定](/windows/uwp/data-binding)）。

> [!IMPORTANT]
> 有关支持你了解如何利用 C++/WinRT 来使用和创作运行时类的基本概述和术语，请参阅[通过 C++/WinRT 使用 API](consume-apis.md) 和[通过 C++/WinRT 创作 API](author-apis.md)。

## <a name="what-does-observable-mean-for-a-property"></a>对于属性来说，可观测意味着什么  ？
假设名为 BookSku 的运行时类有一个名为“标题”的属性   。 如果 BookSku 选择每当“标题”的值发生更改时引发 [INotifyPropertyChanged::PropertyChanged](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged) 事件，则“标题”为一个可观测属性     。 BookSku 的行为（引发或未引发该事件）确定其属性是否可观测，有哪些可观测  。

XAML 文本元素或控件可检索更新的值并自行更新以显示新值，从而绑定到并处理这些事件。

> [!NOTE]
> 有关安装和使用 C++/WinRT Visual Studio 扩展 (VSIX) 和 NuGet 包（两者共同提供项目模板，并生成支持）的信息，请参阅[适用于 C++/WinRT 的 Visual Studio 支持](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)。

## <a name="create-a-blank-app-bookstore"></a>创建空白应用 (Bookstore)
首先在 Microsoft Visual Studio 中创建新项目。 创建“空白应用 (C++/WinRT)”项目，然后将其命名为 Bookstore   。

我们将创作新类来表示具有可观测标题属性的书籍。 我们要在同一编译单元内创作和使用该类。 但我们希望能够从 XAML 绑定到此类，因此，它将成为一个运行时类。 而且我们将使用 C++/WinRT 来创作和使用它。

创作新的运行时类的第一步是将新的 Midl 文件(.idl) 项添加到项目  。 将其命名为 `BookSku.idl`。 删除 `BookSku.idl` 的默认内容，然后粘贴到此运行时类声明中。

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
> 视图模型类无需从基类派生，实际上，在应用程序中声明的任何运行时类都是如此。 上述声明的 BookSku 类就是这样一个例子  。 它实现接口，但不从任何基类派生。
>
> 从基类派生的任何运行时类（在应用程序中声明）都称为可组合类   。 且可组合类存在一些限制。 若要使应用程序通过 Visual Studio 和 Microsoft Store 用于验证提交的 [Windows 应用认证工具包](../debug-test-perf/windows-app-certification-kit.md)测试，使 Microsoft Store 可成功纳入该应用程序，可组合类必须最终派生自 Windows 基类。 这意味着继承层次结构的根类必须是源于 Windows.* 名称空间的类型。 如果确实需要从基类派生运行时类（例如，为要从中派生的所有视图模型实现 BindableBase 类），则可以从 [Windows.UI.Xaml.DependencyObject](/uwp/api/windows.ui.xaml.dependencyobject) 派生   。
>
> 视图模型是视图的抽象，因此它直接绑定到视图（XAML 标记）。 数据模型是数据的抽象，只通过视图模型使用，不直接绑定到 XAML。 因此，可以将数据模型声明为 C++ 结构或类，而不是运行时类。 无需在 MIDL 中声明，并且可以随意使用任何喜欢的继承层次结构。

保存文件并生成项目。 在生成过程中，`midl.exe` 工具会运行以创建 Windows 运行时元数据文件 (`\Bookstore\Debug\Bookstore\Unmerged\BookSku.winmd`) 来描述该运行时类。 然后，`cppwinrt.exe` 工具运行以生成源代码文件，从而为你在创作和使用运行时类时提供支持。 这些文件包含存根，可用于开始实现在 IDL 中声明的 BookSku 运行时类  。 这些存根是 `\Bookstore\Bookstore\Generated Files\sources\BookSku.h` 和 `BookSku.cpp`。

右键单击项目节点，然后单击“打开文件资源管理器中的文件夹”  。 执行此操作，将在文件资源管理器中打开项目文件夹。 将这些存根文件 `BookSku.h` 和 `BookSku.cpp` 从 `\Bookstore\Bookstore\Generated Files\sources\` 文件夹复制到项目文件夹，即 `\Bookstore\Bookstore\`。 在“解决方案资源管理器”中选中项目节点，确保将“显示所有文件”打开   。 右键单击已复制的存根文件，然后单击“包括在项目中”  。

## <a name="implement-booksku"></a>实现 BookSku 
现在，让我们打开 `\Bookstore\Bookstore\BookSku.h` 和 `BookSku.cpp` 并实现运行时类。 在 `BookSku.h` 中，添加一个采用 [winrt::hstring](/uwp/cpp-ref-for-winrt/hstring) 的构造函数、一个用于存储标题字符串的私有成员以及另一个用于存储标题发生更改时所引发事件的私有成员  。 进行这些更改之后，`BookSku.h` 将如下所示。

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
#include "BookSku.g.cpp"

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

在“标题”转变器函数中，我们检查设置的值是否与当前值不同  。 如果是，我们将更新标题并且引发 [INotifyPropertyChanged::PropertyChanged](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged) 事件，其中包含一个等于已更改的属性的名称的参数  。 这样，用户界面 (UI) 将知道要重新查询的属性的值。

## <a name="declare-and-implement-bookstoreviewmodel"></a>声明并实现 BookstoreViewModel 
主 XAML 页面将绑定到主视图模型。 而且该视图模型将有多个属性，包括其中一个类型 BookSku  。 在此步骤中，我们将声明并实现主视图模型运行时类。

添加名为 `BookstoreViewModel.idl` 的新的 Midl 文件 (.idl) 项  。 另请参阅[将运行时类重构到 Midl 文件 (.idl) 中](/windows/uwp/cpp-and-winrt-apis/author-apis#factoring-runtime-classes-into-midl-files-idl)。

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

保存并生成。 将 `BookstoreViewModel.h` 和 `BookstoreViewModel.cpp` 从 `Generated Files\sources` 文件夹复制到项目文件夹中，然后将其包含在项目中。 打开这些文件并实现如下所示的运行时类。 注意在 `BookstoreViewModel.h` 中包括 `BookSku.h` 的方式，这声明了 BookSku（即 winrt::Bookstore::implementation::BookSku）的实现类型   。 我们将从默认构造函数中删除 `= default`。

```cppwinrt
// BookstoreViewModel.h
#pragma once
#include "BookstoreViewModel.g.h"
#include "BookSku.h"

namespace winrt::Bookstore::implementation
{
    struct BookstoreViewModel : BookstoreViewModelT<BookstoreViewModel>
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
#include "BookstoreViewModel.g.cpp"

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
> `m_bookSku` 的类型是投影类型 (winrt::Bookstore::BookSku)，而且你用于 [winrt::make](/uwp/cpp-ref-for-winrt/make) 的模板参数是实现类型 (winrt::Bookstore::implementation::BookSku)    。 即使如此，make 也会返回投影类型的实例  。

## <a name="add-a-property-of-type-bookstoreviewmodel-to-mainpage"></a>将类型 BookstoreViewModel 的属性添加到 MainPage  
打开 `MainPage.idl`，这将声明表示主 UI 页面的运行时类。 添加导入语句以导入 `BookstoreViewModel.idl`，然后添加名为类型 BookstoreViewModel 的 MainViewModel 的只读属性  。 此外删除 MyProperty 属性  。 另外请注意下表中的 `import` 指令。

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

保存该文件。 该项目目前不会完全生成，但是现在生成很有助益，因为它会重新生成实现 MainPage 运行时类的源代码文件（`\Bookstore\Bookstore\Generated Files\sources\MainPage.h` 和 `MainPage.cpp`）  。 因此，现在请继续生成。 此阶段可能会发生的生成错误是“MainViewModel”:不是“winrt::Bookstore::implementation::MainPage”的成员  。

如果未包含 `BookstoreViewModel.idl`（请参阅上述 `MainPage.idl` 的列表），在“MainViewModel”附近预期 \<  将会发生此错误。 另一个小提示是确保所有类型都保留在同一命名空间中：代码列表中所显示的命名空间。

若要解决预期发生的错误，则现在需要将 MainViewModel 属性的访问器存根从生成的文件（`\Bookstore\Bookstore\Generated Files\sources\MainPage.h` 和 `MainPage.cpp`）复制到 `\Bookstore\Bookstore\MainPage.h` 和 `MainPage.cpp` 。 操作步骤如下所示。

在 `\Bookstore\Bookstore\MainPage.h` 中包括 `BookstoreViewModel.h`，它为 BookstoreViewModel 声明实现类型（即 winrt::Bookstore::implementation::BookstoreViewModel）   。 添加私有成员以存储视图模型。 注意，属性访问器函数（以及成员 m_mainViewModel）根据 BookstoreViewModel 的投影类型（即 Bookstore::BookstoreViewModel）实现   。 实现类型与应用程序位于同一项目（编译单元），因此我们通过采用 **std::nullptr_t** 的构造函数重载来构造 m_mainViewModel。 此外删除 MyProperty 属性  。

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

在 `\Bookstore\Bookstore\MainPage.cpp` 中，调用 [winrt::make](/uwp/cpp-ref-for-winrt/make)（具有实现类型）将投影类型的新实例分配到 m_mainViewModel  。 为书籍的标题分配一个初始值。 针对 MainViewModel 属性实现访问器。 最后，在按钮的事件处理程序中更新书籍的标题。 此外删除 MyProperty 属性  。

```cppwinrt
// MainPage.cpp
#include "pch.h"
#include "MainPage.h"
#include "MainPage.g.cpp"

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

## <a name="bind-the-button-to-the-title-property"></a>将按钮绑定到“标题”属性 
打开 `MainPage.xaml`，其中包含主 UI 页面的 XAML 标记。 如下表所示，删除按钮中的名称，并将其 Content 属性值从文字更改为绑定表达式  。 注意绑定表达式上的 `Mode=OneWay` 属性（从视图模型到 UI 单向）。 没有该属性，UI 将不会响应属性更改事件。

```xaml
<Button Click="ClickHandler" Content="{x:Bind MainViewModel.BookSku.Title, Mode=OneWay}"/>
```

立即生成并运行该项目。 单击该按钮以执行 Click 事件处理程序  。 该处理程序调用书籍的标题转变器函数；该转变器引发了让 UI 知道“标题”属性已发生更改的事件；而且按钮重新查询了该属性的值以更新其自己的“内容”值   。

## <a name="using-the-binding-markup-extension-with-cwinrt"></a>配合使用 {Binding} 标记扩展与 C++/WinRT
对于当前发布的 C++/WinRT 版本，为了能够使用 {Binding} 标记扩展，需要实现 [ICustomPropertyProvider](/uwp/api/windows.ui.xaml.data.icustompropertyprovider) 和 [ICustomProperty](/uwp/api/windows.ui.xaml.data.icustomproperty) 接口。

## <a name="element-to-element-binding"></a>元素间的绑定

可以将一个 XAML 元素的属性绑定到另一个 XAML 元素的属性。 下面是在标记中进行的该操作的一个示例。

```xaml
<TextBox x:Name="myTextBox" />
<TextBlock Text="{x:Bind myTextBox.Text, Mode=OneWay}" />
```

需要在 Midl 文件 (.idl) 中将命名的 XAML 实体 `myTextBox` 声明为只读属性。

```idl
// MainPage.idl
runtimeclass MainPage : Windows.UI.Xaml.Controls.Page
{
    MainPage();
    Windows.UI.Xaml.Controls.TextBox myTextBox{ get; };
}
```

必须这样做的原因是： XAML 编译器进行验证所需的所有类型（包括在 [{x:Bind}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension) 中使用的类型）都是从 Windows 元数据 (WinMD) 读取的。 你只需将只读属性添加到 Midl 文件即可。 请勿实现它，因为自动生成的 XAML 代码隐藏会为你提供实现。

## <a name="consuming-objects-from-xaml-markup"></a>使用 XAML 标记中的对象

以 XAML [ **{x:Bind} 标记扩展**](/windows/uwp/xaml-platform/x-bind-markup-extension)形式使用的所有实体必须在 IDL 中以公开方式公开。 另外，如果 XAML 标记包含对另一元素的引用，且该引用也存在于标记中，则该标记的 getter 必须存在于 IDL 中。

```xaml
<Page x:Name="MyPage">
    <StackPanel>
        <CheckBox x:Name="UseCustomColorCheckBox" Content="Use custom color"
             Click="UseCustomColorCheckBox_Click" />
        <Button x:Name="ChangeColorButton" Content="Change color"
            Click="{x:Bind ChangeColorButton_OnClick}"
            IsEnabled="{x:Bind UseCustomColorCheckBox.IsChecked.Value, Mode=OneWay}"/>
    </StackPanel>
</Page>
```

*ChangeColorButton* 元素通过绑定引用 *UseCustomColorCheckBox* 元素。 因此，此页的 IDL 必须声明一个名为 *UseCustomColorCheckBox* 的只读属性，然后它才能供绑定访问。

*UseCustomColorCheckBox* 的点击事件处理程序委托使用经典的 XAML 委托语法，因此不需要在 IDL 中有一个条目，只需在实现类中处于公开状态即可。 另一方面，*ChangeColorButton* 也有一个 `{x:Bind}` 点击事件处理程序，该程序也必须进入 IDL 中。

```idl
runtimeclass MyPage : Windows.UI.Xaml.Controls.Page
{
    MyPage();

    // These members are consumed by binding.
    void ChangeColorButton_OnClick();
    Windows.UI.Xaml.Controls.CheckBox UseCustomColorCheckBox{ get; };
}
```

不需为 **UseCustomColorCheckBox** 属性提供一个实现。 XAML 代码生成器会为你这样做。

### <a name="binding-to-boolean"></a>绑定到布尔值

可以在诊断模式下这样做。

<syntaxhighlight lang="xml">
<TextBlock Text="{Binding CanPair}"/>
</syntaxhighlight>

这会在 C++/CX 中显示 `true` 或 `false`，但在 C++/WinRT 中则显示 **Windows.Foundation.IReference`1<Boolean>** 。

绑定到布尔值时使用 `x:Bind`。

```xaml
<TextBlock Text="{x:Bind CanPair}"/>
```

## <a name="important-apis"></a>重要的 API
* [INotifyPropertyChanged::PropertyChanged](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged)
* [winrt::make 函数模板](/uwp/cpp-ref-for-winrt/make)

## <a name="related-topics"></a>相关主题
* [通过 C++/WinRT 使用 API](consume-apis.md)
* [使用 C++/WinRT 创作 API](author-apis.md)
