---
description: 本主题介绍将 [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) 项目中的源代码移植到 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 项目中的等效项时所涉及的技术细节。
title: 从 C++/CX 移动到 C++/WinRT
ms.date: 01/17/2019
ms.topic: article
keywords: windows 10, uwp, 标准, c++, cpp, winrt, 投影, 端口, 迁移, C++/CX
ms.localizationpriority: medium
ms.openlocfilehash: c5f8b9548bba704a7035b014ca3728db8bcbcc16
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "80662410"
---
# <a name="move-to-cwinrt-from-ccx"></a>从 C++/CX 移动到 C++/WinRT

本主题介绍将 [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) 项目中的源代码移植到 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 项目中的等效项时所涉及的技术细节。

## <a name="porting-strategies"></a>移植策略

如果要将 C++/CX 代码逐渐移植到 C++/WinRT，那么可以这样做。 C++/CX 和 C++/WinRT 代码可以在同一个项目中共存，但是 XAML 编译器支持和 Windows 运行时组件除外。 对于这两种例外情况，需要在同一个项目中针对 C++/CX 或 C++/WinRT。

> [!IMPORTANT]
> 如果项目生成 XAML 应用程序，则一个建议工作流是首先在 Visual Studio 中使用 C++/WinRT 项目模板之一创建新项目（请参阅[适用于 C++/WinRT 的 Visual Studio 支持](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)）。 随后，开始从 C++/CX 项目复制源代码和标记。 可以使用“项目”  \>“添加新项...”  \>“Visual C++”   > “空白页(C++/WinRT)”  来添加新 XAML 页面。
>
> 或者，可以在移植 XAML C++/CX 项目时使用 Windows 运行时组件从中提出代码。 将尽可能多的 C++/CX 代码移动到一个组件中，然后将 XAML 项目更改为 C++/WinRT。 或是将 XAML 项目保留为 C++/CX，创建新的 C++/WinRT 组件，然后开始从 XAML 项目中移植出 C++/CX 代码并移植到该组件中。 还可以让 C++/CX 组件项目以及 C++/WinRT 组件项目处于同一个解决方案中，从应用程序项目引用两者，然后逐渐从一个项目移植到另一个项目。 有关在同一个项目中使用两种语言投影的更多详细信息，请参阅[实现 C++/WinRT 与 C++/CX 之间的互操作](interop-winrt-cx.md)。

> [!NOTE]
> [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) 和 Windows SDK 都在根命名空间 Windows  中声明类型。 投影到 C++/WinRT 的 Windows 类型具有与 Windows 类型相同的完全限定名称，但放置于 C++ winrt  命名空间中。 这些不同的命名空间可让你按照自己的节奏从 C++/CX 移植到 C++/WinRT。

请牢记上面提到的例外情况，将 C++/CX 项目移植到 C++/WinRT 的第一步是向它手动添加 C++/WinRT 支持（请参阅[适用于 C++/WinRT 的 Visual Studio 支持](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)）。 为此，请在项目中安装 [Microsoft.Windows.CppWinRT NuGet 包](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/)。 在 Visual Studio 中打开项目，然后单击“项目”\>“管理 NuGet 包...”   \>“浏览”，在搜索框中键入或粘贴 Microsoft.Windows.CppWinRT  ，在搜索结果中选择该项，然后单击“安装”以安装该项目的包。   这一更改的一个效果是对 C++/CX 的支持在项目中关闭。 让支持处于关闭状态是一个好办法，这样生成消息可帮助你找到（并移植）C++/CX 上的所有依赖项，也可将支持重新打开（在项目属性中，单击“C/C++”  \>“常规”  \>“使用 Windows 运行时扩展”  \>“是(/ZW)”  ），然后逐渐移植。

也可使用 Visual Studio 中的 C++/WinRT 项目属性页将以下属性手动添加到 `.vcxproj` 文件。 如需类似自定义选项（用于优化 `cppwinrt.exe` 工具的行为）的列表，请参阅 Microsoft.Windows.CppWinRT NuGet 包[自述文件](https://github.com/microsoft/cppwinrt/blob/master/nuget/readme.md#customizing)。

```xml
<syntaxhighlight lang="xml">
  <PropertyGroup Label="Globals">
    <CppWinRTProjectLanguage>C++/CX</CppWinRTProjectLanguage>
  </PropertyGroup>
</syntaxhighlight>
```

接下来，请确保项目属性“常规”  \>“目标平台版本”  设置为 10.0.17134.0（Windows 10 版本 1803）或更高版本。

在预编译的标头文件（通常为 `pch.h`）中，包括 `winrt/base.h`。

```cppwinrt
#include <winrt/base.h>
```

如果你包括了任何 C++/WinRT 投影 Windows API 标头（例如，`winrt/Windows.Foundation.h`），那么你无需像这样明确包括 `winrt/base.h`，因为它将自动为你包含在内。

如果你的项目还使用 [Windows 运行时 C++ 模板库 (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl) 类型，请参阅[从 WRL 迁移到 C++/WinRT](move-to-winrt-from-wrl.md)。

## <a name="file-naming-conventions"></a>文件命名约定

### <a name="xaml-markup-files"></a>XAML 标记文件

| | C++/CX | C++/WinRT |
| - | - | - |
| **开发人员 XAML 文件** | MyPage.xaml<br/>MyPage.xaml.h<br/>MyPage.xaml.cpp | MyPage.xaml<br/>MyPage.h<br/>MyPage.cpp<br/>MyPage.idl（见下） |
| **生成的 XAML 文件** | MyPage.xaml.g.h<br/>MyPage.xaml.g.hpp | MyPage.xaml.g.h<br/>MyPage.xaml.g.hpp<br/>MyPage.g.h |

请注意，C++/WinRT 会从 `*.h` 和 `*.cpp` 文件名中删除 `.xaml`。

C++/WinRT 添加了一个额外的开发人员文件，即 **Midl 文件 (.idl)** 。 C++/CX 在内部自动生成此文件，并将其添加到每个公开的和受保护的成员。 在 C++/WinRT 中，你自行创作并添加此文件。 如需更多详细信息、代码示例以及 IDL 创作演练，请参阅 [XAML 控件；绑定到 C++/WinRT 属性](/windows/uwp/cpp-and-winrt-apis/binding-property)。

另请参阅[将运行时类重构到 Midl 文件 (.idl) 中](/windows/uwp/cpp-and-winrt-apis/author-apis#factoring-runtime-classes-into-midl-files-idl)

### <a name="runtime-classes"></a>运行时类

C++/CX 不对头文件的名称施加限制；通常会将多个运行时类定义置于单个头文件中，小型类尤其如此。 但是，C++/WinRT 要求每个运行时类将自己的头文件以类名称来命名。 

| C++/CX | C++/WinRT |
| - | - |
| **Common.h**<br>`ref class A { ... }`<br>`ref class B { ... }` | **Common.idl**<br>`runtimeclass A { ... }`<br>`runtimeclass B { ... }` |
|  | **A.h**<br>`namespace implements {`<br>&nbsp;&nbsp;`struct A { ... };`<br>`}` |
|  | **B.h**<br>`namespace implements {`<br>&nbsp;&nbsp;`struct B { ... };`<br>`}` |

在 C++/CX 中，使用不同命名方式的头文件进行 XAML 自定义控制的做法不太常见（但仍属合法）。 需根据类名来重命名这些头文件。

| C++/CX | C++/WinRT |
| - | - |
| **A.xaml**<br>`<Page x:Class="LongNameForA" ...>` | **A.xaml**<br>`<Page x:Class="LongNameForA" ...>` |
| **A.h**<br>`partial ref class LongNameForA { ... }` | **LongNameForA.h**<br>`namespace implements {`<br>&nbsp;&nbsp;`struct LongNameForA { ... };`<br>`}` |

## <a name="header-file-requirements"></a>头文件要求

C++/CX 不要求你包括任何特殊的头文件，因为它会在内部根据 `.winmd` 文件自动生成头文件。 在 C++/CX 中，通常会对按名称使用的命名空间使用 `using` 指令。

```cppcx
using namespace Windows::Media::Playback;

String^ NameOfFirstVideoTrack(MediaPlaybackItem^ item)
{
    return item->VideoTracks->GetAt(0)->Name;
}
```

可以通过 `using namespace Windows::Media::Playback` 指令在不使用命名空间前缀的情况下编写 `MediaPlaybackItem`。 我们还接触了 `Windows.Media.Core` 命名空间，因为 `item->VideoTracks->GetAt(0)` 返回 **Windows.Media.Core.VideoTrack**。 但是，我们不需要在任何位置键入 **VideoTrack** 这个名称，因此不需要 `using Windows.Media.Core` 指令。

但是，C++/WinRT 要求你针对每个所使用的命名空间包括一个头文件，即使不为其命名。

```cppwinrt
#include <winrt/Windows.Media.Playback.h>
#include <winrt/Windows.Media.Core.h> // !!This is important!!

using namespace winrt;
using namespace Windows::Media::Playback;

winrt::hstring NameOfFirstVideoTrack(MediaPlaybackItem const& item)
{
    return item.VideoTracks().GetAt(0).Name();
}
```

另一方面，即使 **MediaPlaybackItem.AudioTracksChanged** 事件的类型为 **TypedEventHandler\<MediaPlaybackItem, Windows.Foundation.Collections.IVectorChangedEventArgs\>** ，我们也不需包括 `winrt/Windows.Foundation.Collections.h`，因为并未使用该事件。

C++/WinRT 还要求你针对 XAML 标记所使用的命名空间包括相应的头文件。

```xaml
<!-- MainPage.xaml -->
<Rectangle Height="400"/>
```

使用 **Rectangle** 类意味着你必须添加以下 include。

```cppwinrt
// MainPage.h
#include <winrt/Windows.UI.Xaml.Shapes.h>
```

如果忘记了头文件，则仍可正常编译，但会出现链接器错误，因为缺少 `consume_` 类。

## <a name="parameter-passing"></a>参数传递

编写 C++/CX 源代码时，在顶帽 (\^) 引用时，你将 C++/CX 类型作为函数参数传递。

```cppcx
void LogPresenceRecord(PresenceRecord^ record);
```

在 C++/WinRT 中，对于同步函数，默认情况下应该使用 `const&` 参数。 这将避免复制和互锁开销。 但你的协同程序应使用按值传递来确保它们按值捕获，并避免生命周期问题（更多详细信息，请参阅[利用 C++/WinRT 实现的并发和异步操作](concurrency.md)）。

```cppwinrt
void LogPresenceRecord(PresenceRecord const& record);
IASyncAction LogPresenceRecordAsync(PresenceRecord const record);
```

C++/WinRT 对象根本上是一个保留支持 Windows 运行时对象的接口指针的值。 在复制 C++/WinRT 对象时，编译器复制封装的接口指针，从而递增其引用计数。 副本的最终销毁涉及递减引用计数。 因此，仅在必要时产生复制开销。

## <a name="variable-and-field-references"></a>变量和字段引用

编写 C++/CX 源代码时，你使用顶帽 (\^) 变量引用 Windows 运行时对象，使用箭头 (-&gt;) 运算符来取消引用顶帽变量。

```cppcx
IVectorView<User^>^ userList = User::Users;

if (userList != nullptr)
{
    for (UINT32 iUser = 0; iUser < userList->Size; ++iUser)
    ...
```

移植到等效 C++/WinRT 代码时，可以通过删除顶帽，并将箭头运算符 (-&gt;) 更改为点运算符 (.)，来完成大量工作。 C++/WinRT 投影类型是值，而不是指针。

```cppwinrt
IVectorView<User> userList = User::Users();

if (userList != nullptr)
{
    for (UINT32 iUser = 0; iUser < userList.Size(); ++iUser)
    ...
```

C++/CX 顶帽引用的默认构造函数会将它初始化为 null。 下面是一个 C++/CX 代码示例，我们在其中创建一个具有正确类型，但是未初始化的变量/字段。 换句话说，它最初不引用 TextBlock  ；我们打算在以后分配引用。

```cppcx
TextBlock^ textBlock;

class MyClass
{
    TextBlock^ textBlock;
};
```

有关 C++/WinRT 中的等效项，请参阅[延迟初始化](consume-apis.md#delayed-initialization)。

## <a name="properties"></a>“属性”

C++/CX 语言扩展包括属性概念。 编写 C++/CX 源代码时，你可以像访问字段那样访问属性。 标准 C++ 没有属性概念，因此，在 C++/WinRT 中，你调用获取和设置函数。

在随后的示例中，XboxUserId  、UserState  、PresenceDeviceRecords  和 Size  全部都是属性。

### <a name="retrieving-a-value-from-a-property"></a>从属性检索值

下面介绍如何在 C++/CX 中获取属性值。

```cppcx
void Sample::LogPresenceRecord(PresenceRecord^ record)
{
    auto id = record->XboxUserId;
    auto state = record->UserState;
    auto size = record->PresenceDeviceRecords->Size;
}
```

等效的 C++/WinRT 源代码调用与属性同名但没有参数的函数。

```cppwinrt
void Sample::LogPresenceRecord(PresenceRecord const& record)
{
    auto id = record.XboxUserId();
    auto state = record.UserState();
    auto size = record.PresenceDeviceRecords().Size();
}
```

请注意，PresenceDeviceRecords  函数返回其本身具有 Size  函数的 Windows 运行时对象。 由于返回的对象也是 C++/WinRT 投影类型，因此我们使用点运算符调用 Size  来取消引用。

### <a name="setting-a-property-to-a-new-value"></a>将属性设置为新值

将属性设置为新值遵循类似模式。 首先，在 C++/CX 中。

```cppcx
record->UserState = newValue;
```

若要在 C++/WinRT 执行同等操作，调用与属性同名的函数，并传递参数。

```cppwinrt
record.UserState(newValue);
```

## <a name="creating-an-instance-of-a-class"></a>创建类的实例

你通过 C++/CX 对象的句柄来处理它，通常称为顶帽 (\^) 引用。 通过 `ref new` 关键字创建新对象，这反过来会调用 [RoActivateInstance  ](https://docs.microsoft.com/windows/desktop/api/roapi/nf-roapi-roactivateinstance) 来激活运行时类的新实例。

```cppcx
using namespace Windows::Storage::Streams;

class Sample
{
private:
    Buffer^ m_gamerPicBuffer = ref new Buffer(MAX_IMAGE_SIZE);
};
```

C++/WinRT 对象是一个值；因此你可以在堆栈上进行分配，或作为对象字段分配。 切勿  使用 `ref new`（或 `new`）来分配 C++/WinRT 对象。 在后台，仍然在调用 RoActivateInstance  。

```cppwinrt
using namespace winrt::Windows::Storage::Streams;

struct Sample
{
private:
    Buffer m_gamerPicBuffer{ MAX_IMAGE_SIZE };
};
```

如果初始化资源的成本很高，通常可以推迟资源的初始化，直到实际需要时再执行。 如前所述，C++/CX 顶帽引用的默认构造函数会将它初始化为 null。

```cppcx
using namespace Windows::Storage::Streams;

class Sample
{
public:
    void DelayedInit()
    {
        // Allocate the actual buffer.
        m_gamerPicBuffer = ref new Buffer(MAX_IMAGE_SIZE);
    }

private:
    Buffer^ m_gamerPicBuffer;
};
```

移植到 C++/WinRT 的同一个代码。 请注意 **std::nullptr_t** 构造函数的使用。 有关该构造函数的详细信息，请参阅[延迟初始化](consume-apis.md#delayed-initialization)。

```cppwinrt
using namespace winrt::Windows::Storage::Streams;

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
```

## <a name="how-the-default-constructor-affects-collections"></a>默认构造函数如何影响集合

C++ 集合类型使用默认构造函数，这可能导致意外的对象构造。

| 方案 | C++/CX | C++/WinRT（不正确） | C++/WinRT（正确） |
| - | - | - | - |
| 本地变量，一开始为空 | `TextBox^ textBox;` | `TextBox textBox; // Creates a TextBox!` | `TextBox textBox{ nullptr };` |
| 成员变量，一开始为空 | `class C {`<br/>&nbsp;&nbsp;`TextBox^ textBox;`<br/>`};` | `class C {`<br/>&nbsp;&nbsp;`TextBox textBox; // Creates a TextBox!`<br/>`};` | `class C {`<br/>&nbsp;&nbsp;`TextBox textbox{ nullptr };`<br/>`};` |
| 全局变量，一开始为空 | `TextBox^ g_textBox;` | `TextBox g_textBox; // Creates a TextBox!` | `TextBox g_textBox{ nullptr };` |
| 空引用的矢量 | `std::vector<TextBox^> boxes(10);` | `// Creates 10 TextBox objects!`<br/>`std::vector<TextBox> boxes(10);` | `std::vector<TextBox> boxes(10, nullptr);` |
| 在 map 中设置值 | `std::map<int, TextBox^> boxes;`<br/>`boxes[2] = value;` | `std::map<int, TextBox> boxes;`<br/>`// Creates a TextBox at 2,`<br/>`// then overwrites it!`<br/>`boxes[2] = value;` | `std::map<int, TextBox> boxes;`<br/>`boxes.insert_or_assign(2, value);` |
| 空引用的数组 | `TextBox^ boxes[2];` | `// Creates 2 TextBox objects!`<br/>`TextBox boxes[2];` | `TextBox boxes[2] = { nullptr, nullptr };` |
| 配对 | `std::pair<TextBox^, String^> p;` | `// Creates a TextBox!`<br/>`std::pair<TextBox, String> p;` | `std::pair<TextBox, String> p{ nullptr, nullptr };` |

### <a name="more-about-collections-of-empty-references"></a>有关空引用集合的详细信息

只要在 C++/CX 中有一个 **Platform::Array\^** （参见[移植 **Platform::Array\^** ](#port-platformarray)），即可将其移植到 C++/WinRT 中的 **std::vector**（事实上，可以将其移植到任何邻近的容器），而不是将其作为数组保留。 选择 **std::vector** 有多种优势。

例如，尽管存在用于创建固定大小的空引用矢量（参见上表）的速记，但没有用于创建空引用数组  的速记。 必须针对数组中的每个元素重复执行 `nullptr`。 如果构造的数目太少，则会默认构造额外的。

对于矢量，可以在初始化时使用空引用填充它（如上表所示），也可以借助如下所示的代码在初始化后使用空引用填充它。

```cppwinrt
std::vector<TextBox> boxes(10); // 10 default-constructed TextBoxes.
boxes.resize(10, nullptr); // 10 empty references.
```

### <a name="more-about-the-stdmap-example"></a>有关 **std::map** 示例的详细信息

**std::map** 的 `[]` 下标运算符的行为如下所示。

- 如果在 map 中发现此键，则返回现有值（可以将其覆盖）的引用。
- 如果未在 map 中发现此键，则在包含此键（在可移动的情况下已经移动）的 map 中创建新条目和默认构造的值  ，并返回此值（可以随后将其覆盖）的引用。

换言之，`[]` 运算符始终在 map 中创建一个条目。 这不同于 C#、Java 和 JavaScript。

## <a name="converting-from-a-base-runtime-class-to-a-derived-one"></a>从运行时基类转换为派生类

通常有一个已知引用派生类型对象的基类引用。 在 C++/CX 中，使用 `dynamic_cast` 将基类引用强制转换  为派生类引用。 `dynamic_cast` 实际上只是对 [QueryInterface  ](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_)) 的隐藏调用。 下面是一个典型示例 &mdash; 你要处理依赖属性更改事件，并且要从 DependencyObject  强制转换回拥有依赖属性的实际类型。

```cppcx
void BgLabelControl::OnLabelChanged(Windows::UI::Xaml::DependencyObject^ d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs^ e)
{
    BgLabelControl^ theControl{ dynamic_cast<BgLabelControl^>(d) };

    if (theControl != nullptr)
    {
        // succeeded ...
    }
}
```

等效 C++/WinRT 代码将 `dynamic_cast` 替换为对 [IUnknown::try_as  ](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntry_as-function) 函数的调用，该函数会封装 QueryInterface  。 还可以选择改为调用 [IUnknown::as  ](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)，这会在未返回对所需接口（具有所请求的类型的默认接口）的查询时引发异常。 下面是 C++/WinRT 代码示例。

```cppwinrt
void BgLabelControl::OnLabelChanged(Windows::UI::Xaml::DependencyObject const& d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs const& e)
{
    if (BgLabelControlApp::BgLabelControl theControl{ d.try_as<BgLabelControlApp::BgLabelControl>() })
    {
        // succeeded ...
    }

    try
    {
        BgLabelControlApp::BgLabelControl theControl{ d.as<BgLabelControlApp::BgLabelControl>() };
        // succeeded ...
    }
    catch (winrt::hresult_no_interface const&)
    {
        // failed ...
    }
}
```

## <a name="derived-classes"></a>派生类

若要从运行时类派生，基类必须是可组合类。  C++/CX 不需要你执行任何特殊步骤即可将类变为可组合类，但 C++/WinRT 需要。 请使用 [unsealed 关键字](/uwp/midl-3/intro#base-classes)来指示你希望将类用作基类。

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

在实现标头类中，在包括为派生类自动生成的标头之前，必须包括基类标头文件。 否则会出现“将此类型用作表达式非法”之类的错误。

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

## <a name="event-handling-with-a-delegate"></a>通过代理进行事件处理

下面介绍了在 C++/CX 中处理事件的典型示例，在本例中将 lambda 函数用作代理。

```cppcx
auto token = myButton->Click += ref new RoutedEventHandler([=](Platform::Object^ sender, RoutedEventArgs^ args)
{
    // Handle the event.
    // Note: locals are captured by value, not reference, since this handler is delayed.
});
```

它是 C++/WinRT 中的等效项。

```cppwinrt
auto token = myButton().Click([=](IInspectable const& sender, RoutedEventArgs const& args)
{
    // Handle the event.
    // Note: locals are captured by value, not reference, since this handler is delayed.
});
```

不使用 lambda 函数，你可以选择作为自由函数或指向成员函数的指针实现代理。 有关详细信息，请参阅[使用 C++/WinRT 中的代理来处理事件](handle-events.md)。

如果你正在从内部使用（不是跨二进制文件）事件和代理的 C++/CX 基本代码移植，[winrt::delegate  ](/uwp/cpp-ref-for-winrt/delegate) 将帮助你复制 C++/WinRT 中的这个模式。 另请参阅[项目中的参数化委托、简单信号和回调](author-events.md#parameterized-delegates-simple-signals-and-callbacks-within-a-project)。

## <a name="revoking-a-delegate"></a>撤销代理

在 C++/CX 中，使用 `-=` 运算符来撤销之前的事件注册。

```cppcx
myButton->Click -= token;
```

它是 C++/WinRT 中的等效项。

```cppwinrt
myButton().Click(token);
```

有关详细信息和选项，请参阅[撤销已注册的代理](handle-events.md#revoke-a-registered-delegate)。

## <a name="boxing-and-unboxing"></a>装箱和取消装箱

C++/CX 自动将标量装箱到对象中。 C++/WinRT 要求你显式调用 [**winrt::box_value**](/uwp/cpp-ref-for-winrt/box-value) 函数。 两种语言都要求你以显式方式取消装箱。 请参阅[使用 C++/WinRT 装箱和取消装箱](/windows/uwp/cpp-and-winrt-apis/boxing)。

在下面的表中，我们将使用这些定义。

| C++/CX | C++/WinRT|
|-|-|
| `int i;` | `int i;` |
| `String^ s;` | `winrt::hstring s;` |
| `Object^ o;` | `IInspectable o;`|

| 操作 | C++/CX | C++/WinRT|
|-|-|-|-|
| 装箱 | `o = 1;`<br>`o = "string";` | `o = box_value(1);`<br>`o = box_value(L"string");` |
| 取消装箱 | `i = (int)o;`<br>`s = (String^)o;` | `i = unbox_value<int>(o);`<br>`s = unbox_value<winrt::hstring>(o);` |

如果尝试取消值类型的 null 指针的装箱，C++/CX 和 C# 会引发异常。 C++/WinRT 将其视为编程错误，因此会崩溃。 在 C++/WinRT 中，请使用 [**winrt::unbox_value_or**](/uwp/cpp-ref-for-winrt/unbox-value-or) 函数来处理对象类型不符合预期的情况。

| 方案 | C++/CX | C++/WinRT|
|-|-|-|-|
| 取消已知整数的装箱 | `i = (int)o;` | `i = unbox_value<int>(o);` |
| 如果 o 为 null | `Platform::NullReferenceException` | 崩溃 |
| 如果 o 不是装箱的整数 | `Platform::InvalidCastException` | 崩溃 |
| 取消整数的装箱，在为 null 的情况下使用回退；任何其他情况则崩溃 | `i = o ? (int)o : fallback;` | `i = o ? unbox_value<int>(o) : fallback;` |
| 尽可能取消整数的装箱；在任何其他情况下使用回退 | `auto box = dynamic_cast<IBox<int>^>(o);`<br>`i = box ? box->Value : fallback;` | `i = unbox_value_or<int>(o, fallback);` |

### <a name="boxing-and-unboxing-a-string"></a>将字符串装箱和取消装箱

字符串在某些情况下是值类型，在另一些情况下是引用类型。 C++/CX 和 C++/WinRT 对待字符串的方式有所不同。

ABI 类型 [**HSTRING**](/windows/win32/winrt/hstring) 是一个指向引用计数字符串的指针。 但是，它并非派生自 [**IInspectable**](/windows/win32/api/inspectable/nn-inspectable-iinspectable)，因此从技术上来说它不是一个对象  。 另外，null **HSTRING** 表示空字符串。 将并非派生自 **IInspectable** 的项装箱时，需将其包装到 [**IReference\<T\>** ](/uwp/api/windows.foundation.ireference_t_) 中，而 Windows 运行时会以 [**PropertyValue**](/uwp/api/windows.foundation.propertyvalue) 对象的形式提供标准实现（自定义类型以 [**PropertyType::OtherType**](/uwp/api/windows.foundation.propertytype) 形式报告）。

C++/CX 将 Windows 运行时字符串表示为引用类型，而 C++/WinRT 则将字符串投影为值类型。 这意味着装箱的 null 字符串可能有不同的表示形式，具体取决于你所采用的方法。

另外，C++/CX 允许取消引用 null **String^** ，在这种情况下，其行为类似于字符串 `""`。

| 行为 | C++/CX | C++/WinRT|
|-|-|-|
| 声明 | `Object^ o;`<br>`String^ s;` | `IInspectable o;`<br>`hstring s;` |
| 字符串类型类别 | 引用类型 | 值类型 |
| null **HSTRING** 投影方式 | `(String^)nullptr` | `hstring{}` |
| null 和 `""` 是否相同？ | 是 | 是 |
| null 的有效性 | `s = nullptr;`<br>`s->Length == 0`（有效） | `s = hstring{};`<br>`s.size() == 0`（有效） |
| 如果将 null 字符串分配给对象 | `o = (String^)nullptr;`<br>`o == nullptr` | `o = box_value(hstring{});`<br>`o != nullptr` |
| 如果将 `""` 分配给对象 | `o = "";`<br>`o == nullptr` | `o = box_value(hstring{L""});`<br>`o != nullptr` |

基本装箱和取消装箱。

| 操作 | C++/CX | C++/WinRT|
|-|-|-|
| 将字符串装箱 | `o = s;`<br>空字符串变为 nullptr。 | `o = box_value(s);`<br>空字符串变为非 null 对象。 |
| 取消已知字符串的装箱 | `s = (String^)o;`<br>Null 对象变为空字符串。<br>如果不是字符串，则引发 InvalidCastException。 | `s = unbox_value<hstring>(o);`<br>Null 对象崩溃。<br>如果不是字符串，则崩溃。 |
| 将可能的字符串取消装箱 | `s = dynamic_cast<String^>(o);`<br>Null 对象或非字符串变为空字符串。 | `s = unbox_value_or<hstring>(o, fallback);`<br>Null 或非字符串变为 fallback。<br>空字符串被保留。 |

## <a name="concurrency-and-asynchronous-operations"></a>并发和异步操作

并行模式库 (PPL)（例如 [**concurrency::task**](/cpp/parallel/concrt/reference/task-class)）已更新，现在支持 C++/CX 顶帽引用。

对于 C++/WinRT，应改用协同程序和 `co_await`。 详细信息和代码示例，请参阅[利用 C++/WinRT 实现的并发和异步运算](/windows/uwp/cpp-and-winrt-apis/concurrency)。

## <a name="consuming-objects-from-xaml-markup"></a>使用 XAML 标记中的对象

在 C++/CX 项目中，可以使用 XAML 标记中的专用成员和命名元素。 但在 C++/WinRT 中，以 XAML [ **{x:Bind} 标记扩展**](/windows/uwp/xaml-platform/x-bind-markup-extension)形式使用的所有实体必须在 IDL 中以公开方式公开。

另外，绑定到布尔值时，在 C++/CX 中会显示 `true` 或 `false`，但在 C++/WinRT 中会显示 **Windows.Foundation.IReference`1\<布尔值\>** 。

有关详细信息和代码示例，请参阅[使用标记中的对象](/windows/uwp/cpp-and-winrt-apis/binding-property#consuming-objects-from-xaml-markup)。

## <a name="mapping-ccx-platform-types-to-cwinrt-types"></a>将 C++/CX 平台  类型映射到 C++/WinRT 类型

C++/CX 在平台  命名空间中提供了多个数据类型。 这些类型不是标准的 C++，因此只能在启用 Windows 运行时语言扩展（Visual Studio 项目属性“C/C++”   > “常规”   > “使用 Windows 运行时扩展”   > “是(/ZW)”  ）的情况下使用。 下表帮助你从平台  类型移植到 C++/WinRT 中的等效项。 完成后，由于 C++/WinRT 是标准 C++，因此你可以关闭 `/ZW` 选项。

| C++/CX | C++/WinRT |
| ---- | ---- |
| Platform::Agile\^  | [winrt::agile_ref  ](/uwp/cpp-ref-for-winrt/agile-ref) |
| Platform::Array\^  | 请参阅[移植 Platform::Array\^  ](#port-platformarray) |
| Platform::Exception\^  | [winrt::hresult_error  ](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) |
| Platform::InvalidArgumentException\^  | [winrt::hresult_invalid_argument  ](/uwp/cpp-ref-for-winrt/error-handling/hresult-invalid-argument) |
| Platform::Object\^  | winrt::Windows::Foundation::IInspectable  |
| Platform::String\^  | [winrt::hstring  ](/uwp/cpp-ref-for-winrt/hstring) |

### <a name="port-platformagile-to-winrtagile_ref"></a>将 Platform::Agile\^  移植到 winrt::agile_ref 

C++/CX 中的 Platform::Agile\^  类型表示可以从任何线程访问的 Windows 运行时类。 C++/WinRT 的等效项是 [winrt::agile_ref  ](/uwp/cpp-ref-for-winrt/agile-ref)。

在 C++/CX 中。

```cppcx
Platform::Agile<Windows::UI::Core::CoreWindow> m_window;
```

在 C++/WinRT 中。

```cppwinrt
winrt::agile_ref<Windows::UI::Core::CoreWindow> m_window;
```

### <a name="port-platformarray"></a>移植 Platform::Array\^ 

在 C++/CX 要求使用数组的情况下，C++/WinRT 允许使用任何相邻的容器。 请参阅[默认构造函数如何影响集合](#how-the-default-constructor-affects-collections)，了解为何可以使用 **std::vector**。

因此，只要在 C++/CX 中有 **Platform::Array\^** ，移植选项中就会包括使用初始值设定项列表、**std::array** 或 **std::vector** 的选项。 有关详细信息和代码示例，请参阅[标准初始值设定项列表](/windows/uwp/cpp-and-winrt-apis/std-cpp-data-types#standard-initializer-lists)和[标准数组和矢量](/windows/uwp/cpp-and-winrt-apis/std-cpp-data-types#standard-arrays-and-vectors)。

### <a name="port-platformexception-to-winrthresult_error"></a>将 Platform::Exception\^  移植到 winrt::hresult_error 

当 Windows 运行时 API 返回非 S\_OK HRESULT 时，Platform::Exception\^  类型在 C++/CX 中生成。 C++/WinRT 的等效项是 [winrt::hresult_error  ](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)。

若要移植到 C++/WinRT，将使用 Platform::Exception\^  的所有代码更改为使用 winrt::hresult_error  。

在 C++/CX 中。

```cppcx
catch (Platform::Exception^ ex)
```

在 C++/WinRT 中。

```cppwinrt
catch (winrt::hresult_error const& ex)
```

C++/WinRT 提供这些异常类。

| 例外类型 | 基类 | HRESULT |
| ---- | ---- | ---- |
| [winrt::hresult_error  ](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) | | 调用 [hresult_error::to_abi  ](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresult_errorto_abi-function) |
| [winrt::hresult_access_denied  ](/uwp/cpp-ref-for-winrt/error-handling/hresult-access-denied) | winrt::hresult_error  | E_ACCESSDENIED |
| [winrt::hresult_canceled  ](/uwp/cpp-ref-for-winrt/error-handling/hresult-canceled) | winrt::hresult_error  | ERROR_CANCELLED |
| [winrt::hresult_changed_state  ](/uwp/cpp-ref-for-winrt/error-handling/hresult-changed-state) | winrt::hresult_error  | E_CHANGED_STATE |
| [winrt::hresult_class_not_available  ](/uwp/cpp-ref-for-winrt/error-handling/hresult-class-not-available) | winrt::hresult_error  | CLASS_E_CLASSNOTAVAILABLE |
| [winrt::hresult_illegal_delegate_assignment  ](/uwp/cpp-ref-for-winrt/error-handling/hresult-illegal-delegate-assignment) | winrt::hresult_error  | E_ILLEGAL_DELEGATE_ASSIGNMENT |
| [winrt::hresult_illegal_method_call  ](/uwp/cpp-ref-for-winrt/error-handling/hresult-illegal-method-call) | winrt::hresult_error  | E_ILLEGAL_METHOD_CALL |
| [winrt::hresult_illegal_state_change  ](/uwp/cpp-ref-for-winrt/error-handling/hresult-illegal-state-change) | winrt::hresult_error  | E_ILLEGAL_STATE_CHANGE |
| [winrt::hresult_invalid_argument  ](/uwp/cpp-ref-for-winrt/error-handling/hresult-invalid-argument) | winrt::hresult_error  | E_INVALIDARG |
| [winrt::hresult_no_interface  ](/uwp/cpp-ref-for-winrt/error-handling/hresult-no-interface) | winrt::hresult_error  | E_NOINTERFACE |
| [winrt::hresult_not_implemented  ](/uwp/cpp-ref-for-winrt/error-handling/hresult-not-implemented) | winrt::hresult_error  | E_NOTIMPL |
| [winrt::hresult_out_of_bounds  ](/uwp/cpp-ref-for-winrt/error-handling/hresult-out-of-bounds) | winrt::hresult_error  | E_BOUNDS |
| [winrt::hresult_wrong_thread  ](/uwp/cpp-ref-for-winrt/error-handling/hresult-wrong-thread) | winrt::hresult_error  | RPC_E_WRONG_THREAD |

请注意，每个类（通过 hresult_error  基类）均提供 [to_abi  ](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresult_errorto_abi-function) 函数，其返回错误 HRESULT，并提供 [message  ](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresult_errormessage-function) 函数，其返回该 HRESULT 的字符串表示形式。

下面是在 C++/CX 中抛出异常的示例。

```cppcx
throw ref new Platform::InvalidArgumentException(L"A valid User is required");
```

以及 C++/WinRT 中的等效项。

```cppwinrt
throw winrt::hresult_invalid_argument{ L"A valid User is required" };
```

### <a name="port-platformobject-to-winrtwindowsfoundationiinspectable"></a>将 Platform::Object\^  移植到 winrt::Windows::Foundation::IInspectable 

与所有 C++/WinRT 类型一样，winrt::Windows::Foundation::IInspectable  属于值类型。 下面介绍如何初始化类型为 null 的变量。

```cppwinrt
winrt::Windows::Foundation::IInspectable var{ nullptr };
```

### <a name="port-platformstring-to-winrthstring"></a>从 Platform::String\^  移植到 winrt::hstring 

Platform::String\^  等同于 Windows 运行时 HSTRING ABI 类型。 对于 C++/WinRT，等效项是 [winrt::hstring  ](/uwp/cpp-ref-for-winrt/hstring)。 但使用 C++/WinRT，你可以使用 C++ 标准库宽字符串类型（如 std::wstring  ）和/或宽字符串文字调用 Windows 运行时 API。 有关更多详细信息和代码示例，请参阅 [C++/WinRT 中的字符串处理](strings.md)。

通过 C++/CX，你可以访问 [Platform::String::Data  ](https://docs.microsoft.com/cpp/cppcx/platform-string-class?view=vs-2019#data) 属性来作为 C 样式 const wchar_t\*  数组检索字符串（例如，将其传递到 std::wcout  ）。

```cppcx
auto var{ titleRecord->TitleName->Data() };
```

使用 C++/WinRT 也一样，你可以使用 [hstring::c_str  ](/uwp/api/windows.foundation.uri.-ctor#Windows_Foundation_Uri__ctor_System_String_) 函数获取 null 结尾的 C 样式字符串版本，就像从 std::wstring  获取的一样。

```cppwinrt
auto var{ titleRecord.TitleName().c_str() };
```

在实现获取或返回字符串的 API 时，通常要将使用 Platform::String\^  的任何 C++/CX 代码更改为使用 winrt::hstring  。

下面是获取字符串的 C++/CX API 的示例。

```cppcx
void LogWrapLine(Platform::String^ str);
```

对于 C++/WinRT，你可以像这样在 [MIDL 3.0](/uwp/midl-3) 中声明该 API。

```idl
// LogType.idl
void LogWrapLine(String str);
```

C++/WinRT 工具链随后将为你生成源代码，如下所示。

```cppwinrt
void LogWrapLine(winrt::hstring const& str);
```

#### <a name="tostring"></a>ToString()

C++/CX 类型提供 [Object::ToString](/cpp/cppcx/platform-object-class#tostring) 方法。

```cppcx
int i{ 2 };
auto s{ i.ToString() }; // s is a Platform::String^ with value L"2".
```

C++/ WinRT 不直接提供此工具，不过可以转为使用替代方法。

```cppwinrt
int i{ 2 };
auto s{ std::to_wstring(i) }; // s is a std::wstring with value L"2".
```

C++/WinRT 也支持 [**winrt::to_hstring**](/uwp/cpp-ref-for-winrt/to-hstring)，但仅限数目有限的一些类型。 对于任何其他需要字符串化的类型，你需要添加重载。

| Language | 将整数字符串化 | 将枚举字符串化 |
| - | - | - |
| C++/CX | `String^ result = "hello, " + intValue.ToString();` | `String^ result = "status: " + status.ToString();` |
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

#### <a name="string-building"></a>字符串生成

C++/CX 和 C++/WinRT 按照标准的 **std::wstringstream** 来生成字符串。

| | C++/CX | C++/WinRT |
|-|-|-|
| 追加字符串，保留 null | `stream.print(s->Data(), s->Length);` | `stream << std::wstring_view{ s };` |
| 追加字符串，在遇到第一个 null 时停止 | `stream << s->Data();` | `stream << s.c_str();` |
| 提取结果 | `ws = stream.str();` | `ws = stream.str();` |

#### <a name="more-examples"></a>更多示例

在下面的示例中，*ws* 是一个类型为 **std::wstring** 的变量。 另外，虽然 C++/CX 可以根据 8 位字符串来构造 **Platform::String**，C++/WinRT 并不这样做。

| 操作 | C++/CX | C++/WinRT |
| - | - | - |
| 根据文本构造字符串 | `String^ s = "hello";`<br>`String^ s = L"hello";` | `// winrt::hstring s{ "hello" }; // Doesn't compile`<br>`winrt::hstring s{ L"hello" };` |
| 从 **std::wstring** 进行转换，保留 null | `String^ s = ref new String(ws.c_str(),`<br>&nbsp;&nbsp;`(uint32_t)ws.size());` | `winrt::hstring s{ ws };`<br>`s = winrt::hstring(ws);`<br>`// s = ws; // Doesn't compile` |
| 从 **std::wstring** 进行转换，在遇到第一个 null 时停止 | `String^ s = ref new String(ws.c_str());` | `winrt::hstring s{ ws.c_str() };`<br>`s = winrt::hstring(ws.c_str());`<br>`// s = ws.c_str(); // Doesn't compile` |
| 转换为 **std::wstring**，保留 null | `std::wstring ws{ s->Data(), s->Length };`<br>`ws = std::wstring(s>Data(), s->Length);` | `std::wstring ws{ s };`<br>`ws = s;` |
| 转换为 **std::wstring**，在遇到一个 null 时停止 | `std::wstring ws{ s->Data() };`<br>`ws = s->Data();` | `std::wstring ws{ s.c_str() };`<br>`ws = s.c_str();` |
| 将文本传递给方法 | `Method("hello");`<br>`Method(L"hello");` | `// Method("hello"); // Doesn't compile`<br>`Method(L"hello");` |
| 将 **std::wstring** 传递给方法 | `Method(ref new String(ws.c_str(),`<br>&nbsp;&nbsp;`(uint32_t)ws.size()); // Stops on first null` | `Method(ws);`<br>`// param::winrt::hstring accepts std::wstring_view` |

## <a name="important-apis"></a>重要的 API
* [winrt::delegate 结构模板](/uwp/cpp-ref-for-winrt/delegate)
* [winrt::hresult_error 结构](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)
* [winrt::hstring 结构](/uwp/cpp-ref-for-winrt/hstring)
* [winrt 命名空间](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>相关主题
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [在 C++/WinRT 中创作事件](author-events.md)
* [利用 C++/WinRT 实现的并发和异步操作](concurrency.md)
* [通过 C++/WinRT 使用 API](consume-apis.md)
* [在 C++/WinRT 中使用委托处理事件](handle-events.md)
* [实现 C++/WinRT 与 C++/CX 之间的互操作](interop-winrt-cx.md)
* [Microsoft 接口定义语言 3.0 参考](/uwp/midl-3)
* [从 WRL 移动到 C++/WinRT](move-to-winrt-from-wrl.md)
* [C++/WinRT 中的字符串处理](strings.md)
