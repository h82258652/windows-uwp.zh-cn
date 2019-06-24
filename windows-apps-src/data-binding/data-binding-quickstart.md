---
ms.assetid: A9D54DEC-CD1B-4043-ADE4-32CD4977D1BF
title: 数据绑定概述
description: 本主题介绍了如何在通用 Windows 平台 (UWP) 应用中将控件（或其他 UI 元素）绑定到单个项目，或者将项目控件绑定到项目集合。
ms.date: 10/05/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cppcx
ms.openlocfilehash: dfe17fc64fd3e97f7562a7feca760b3a5d918f2e
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2019
ms.locfileid: "67318010"
---
# <a name="data-binding-overview"></a>数据绑定概述

本主题介绍了如何在通用 Windows 平台 (UWP) 应用中将控件（或其他 UI 元素）绑定到单个项目，或者将项目控件绑定到项目集合。 此外，我们还介绍了如何控制项的呈现、基于所选内容实现详细信息视图，以及转换数据以供显示。 有关更多详细信息，请参阅[深入了解数据绑定](data-binding-in-depth.md)。

## <a name="prerequisites"></a>先决条件

此主题假设你知道如何创建基本的 UWP 应用。 有关创建你的第一个 UWP 应用的说明，请参阅 [Windows 应用入门](https://docs.microsoft.com/windows/uwp/get-started/)。

## <a name="create-the-project"></a>创建项目

创建一个新的“空白应用程序(Windows 通用)”  项目。 将它命名为“Quickstart”。

## <a name="binding-to-a-single-item"></a>绑定到单个项目

每个绑定均由一个绑定目标和一个绑定源构成。 通常，绑定目标是控件或其他 UI 元素的属性，而绑定源是类实例（数据模型或视图模型）的属性。 本示例演示了如何将控件绑定到单个项目。 绑定目标是 **TextBlock** 的 **Text** 属性。 绑定源是一个名为 **Recording** 的简单类的实例，该类表示音频录制。 我们先来看一下类。

如果您使用的C#或C++/CX，然后将新类添加到你的项目，并将类命名**录制**。

如果您使用的[ C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)，然后添加新**Midl 文件 (.idl)** 项目到项目中，如中所示名为C++/WinRT 代码示例下面的列表。 这些新文件的内容替换为[MIDL 3.0](/uwp/midl-3/intro)显示在列表中，代码生成项目以生成`Recording.h`并`.cpp`并`RecordingViewModel.h`和`.cpp`，然后将代码添加到生成的文件若要匹配列表。 有关这些生成的文件的详细信息以及如何将它们复制到你的项目，请参阅[XAML 控制; 绑定到C++/WinRT 属性](/windows/uwp/cpp-and-winrt-apis/binding-property)。

```csharp
namespace Quickstart
{
    public class Recording
    {
        public string ArtistName { get; set; }
        public string CompositionName { get; set; }
        public DateTime ReleaseDateTime { get; set; }
        public Recording()
        {
            this.ArtistName = "Wolfgang Amadeus Mozart";
            this.CompositionName = "Andante in C for Piano";
            this.ReleaseDateTime = new DateTime(1761, 1, 1);
        }
        public string OneLineSummary
        {
            get
            {
                return $"{this.CompositionName} by {this.ArtistName}, released: "
                    + this.ReleaseDateTime.ToString("d");
            }
        }
    }
    public class RecordingViewModel
    {
        private Recording defaultRecording = new Recording();
        public Recording DefaultRecording { get { return this.defaultRecording; } }
    }
}
```

```cppwinrt
// Recording.idl
namespace Quickstart
{
    runtimeclass Recording
    {
        Recording(String artistName, String compositionName, Windows.Globalization.Calendar releaseDateTime);
        String ArtistName{ get; };
        String CompositionName{ get; };
        Windows.Globalization.Calendar ReleaseDateTime{ get; };
        String OneLineSummary{ get; };
    }
}

// RecordingViewModel.idl
import "Recording.idl";

namespace Quickstart
{
    runtimeclass RecordingViewModel
    {
        RecordingViewModel();
        Quickstart.Recording DefaultRecording{ get; };
    }
}

// Recording.h
// Add these fields:
...
#include <sstream>
...
private:
    std::wstring m_artistName;
    std::wstring m_compositionName;
    Windows::Globalization::Calendar m_releaseDateTime;
...

// Recording.cpp
// Implement like this:
...
Recording::Recording(hstring const& artistName, hstring const& compositionName, Windows::Globalization::Calendar const& releaseDateTime) :
    m_artistName{ artistName.c_str() },
    m_compositionName{ compositionName.c_str() },
    m_releaseDateTime{ releaseDateTime } {}

hstring Recording::ArtistName(){ return hstring{ m_artistName }; }
hstring Recording::CompositionName(){ return hstring{ m_compositionName }; }
Windows::Globalization::Calendar Recording::ReleaseDateTime(){ return m_releaseDateTime; }

hstring Recording::OneLineSummary()
{
    std::wstringstream wstringstream;
    wstringstream << m_compositionName.c_str();
    wstringstream << L" by " << m_artistName.c_str();
    wstringstream << L", released: " << m_releaseDateTime.MonthAsNumericString().c_str();
    wstringstream << L"/" << m_releaseDateTime.DayAsString().c_str();
    wstringstream << L"/" << m_releaseDateTime.YearAsString().c_str();
    return hstring{ wstringstream.str().c_str() };
}
...

// RecordingViewModel.h
// Add this field:
...
#include "Recording.h"
...
private:
    Quickstart::Recording m_defaultRecording{ nullptr };
...

// RecordingViewModel.cpp
// Implement like this:
...
Quickstart::Recording RecordingViewModel::DefaultRecording()
{
    Windows::Globalization::Calendar releaseDateTime;
    releaseDateTime.Year(1761);
    releaseDateTime.Month(1);
    releaseDateTime.Day(1);
    m_defaultRecording = winrt::make<Recording>(L"Wolfgang Amadeus Mozart", L"Andante in C for Piano", releaseDateTime);
    return m_defaultRecording;
}
...
```

```cppcx
// Recording.h
#include <sstream>
namespace Quickstart
{
    public ref class Recording sealed
    {
    private:
        Platform::String^ artistName;
        Platform::String^ compositionName;
        Windows::Globalization::Calendar^ releaseDateTime;
    public:
        Recording(Platform::String^ artistName, Platform::String^ compositionName,
            Windows::Globalization::Calendar^ releaseDateTime) :
            artistName{ artistName },
            compositionName{ compositionName },
            releaseDateTime{ releaseDateTime } {}
        property Platform::String^ ArtistName
        {
            Platform::String^ get() { return this->artistName; }
        }
        property Platform::String^ CompositionName
        {
            Platform::String^ get() { return this->compositionName; }
        }
        property Windows::Globalization::Calendar^ ReleaseDateTime
        {
            Windows::Globalization::Calendar^ get() { return this->releaseDateTime; }
        }
        property Platform::String^ OneLineSummary
        {
            Platform::String^ get()
            {
                std::wstringstream wstringstream;
                wstringstream << this->CompositionName->Data();
                wstringstream << L" by " << this->ArtistName->Data();
                wstringstream << L", released: " << this->ReleaseDateTime->MonthAsNumericString()->Data();
                wstringstream << L"/" << this->ReleaseDateTime->DayAsString()->Data();
                wstringstream << L"/" << this->ReleaseDateTime->YearAsString()->Data();
                return ref new Platform::String(wstringstream.str().c_str());
            }
        }
    };
    public ref class RecordingViewModel sealed
    {
    private:
        Recording ^ defaultRecording;
    public:
        RecordingViewModel()
        {
            Windows::Globalization::Calendar^ releaseDateTime = ref new Windows::Globalization::Calendar();
            releaseDateTime->Year = 1761;
            releaseDateTime->Month = 1;
            releaseDateTime->Day = 1;
            this->defaultRecording = ref new Recording{ L"Wolfgang Amadeus Mozart", L"Andante in C for Piano", releaseDateTime };
        }
        property Recording^ DefaultRecording
        {
            Recording^ get() { return this->defaultRecording; };
        }
    };
}

// Recording.cpp
#include "pch.h"
#include "Recording.h"
```

接下来，从表示标记页的类公开绑定源类。 我们通过将类型 **RecordingViewModel** 的属性添加到 **MainPage** 来执行此操作。

如果您使用的[ C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)，则第一次更新`MainPage.idl`。 生成项目，以重新生成`MainPage.h`和`.cpp`，并将这些生成的文件中的更改合并到你的项目。

```csharp
namespace Quickstart
{
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
            this.ViewModel = new RecordingViewModel();
        }
        public RecordingViewModel ViewModel{ get; set; }
    }
}
```

```cppwinrt
// MainPage.idl
// Add this property:
import "RecordingViewModel.idl";
...
RecordingViewModel ViewModel{ get; };
...

// MainPage.h
// Add this property and this field:
...
#include "RecordingViewModel.h"
...
    Quickstart::RecordingViewModel ViewModel();

private:
    Quickstart::RecordingViewModel m_viewModel{ nullptr };
...

// MainPage.cpp
// Implement like this:
...
MainPage::MainPage()
{
    InitializeComponent();
    m_viewModel = winrt::make<RecordingViewModel>();
}
Quickstart::RecordingViewModel MainPage::ViewModel()
{
    return m_viewModel;
}
...
```

```cppcx
// MainPage.h
...
#include "Recording.h"

namespace Quickstart
{
    public ref class MainPage sealed
    {
    private:
        RecordingViewModel ^ viewModel;
    public:
        MainPage();

        property RecordingViewModel^ ViewModel
        {
            RecordingViewModel^ get() { return this->viewModel; };
        }
    };
}

// MainPage.cpp
...
MainPage::MainPage()
{
    InitializeComponent();
    this->viewModel = ref new RecordingViewModel();
}
```

最后一步是将 **TextBlock** 绑定到 **ViewModel.DefaultRecording.OneLiner** 属性。

```xml
<Page x:Class="Quickstart.MainPage" ... >
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <TextBlock Text="{x:Bind ViewModel.DefaultRecording.OneLineSummary}"
    HorizontalAlignment="Center"
    VerticalAlignment="Center"/>
    </Grid>
</Page>
```

如果您使用的[ C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)，然后将需要删除**MainPage::ClickHandler**为要生成的项目顺序的函数。

下面是结果。

![绑定文本块](images/xaml-databinding0.png)

## <a name="binding-to-a-collection-of-items"></a>绑定到项目集合

一个常见情形是绑定到业务对象的集合。 在 C# 和 Visual Basic 中，通用 [**ObservableCollection&lt;T&gt;** ](https://docs.microsoft.com/dotnet/api/system.collections.objectmodel.observablecollection-1?redirectedfrom=MSDN) 类是数据绑定的一个很好的集合选择，因为它实现了 [**INotifyPropertyChanged**](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?redirectedfrom=MSDN) 和 [**INotifyCollectionChanged**](https://docs.microsoft.com/dotnet/api/system.collections.specialized.inotifycollectionchanged?redirectedfrom=MSDN) 接口。 当添加或删除项目或者列表本身的属性更改时，这些接口将向绑定提供更改通知。 如果你希望你的绑定控件使用集合中的对象属性更改进行更新，则业务对象也应该实现 **INotifyPropertyChanged**。 有关详细信息，请参阅[深入了解数据绑定](data-binding-in-depth.md)。

如果您使用的[ C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)，然后您可以了解有关绑定到可观察集合中的详细信息[XAML 控件的项; 绑定到C++/WinRT 集合](/windows/uwp/cpp-and-winrt-apis/binding-collection)。 如果您阅读该主题第一次，然后的意向C++/WinRT 代码列表如下所示将就更为明显。

下面这个示例演示了将 [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) 绑定到 `Recording` 对象的集合。 让我们先将该集合添加到视图模型。 只需将这些新成员添加到 **RecordingViewModel** 类。

```csharp
public class RecordingViewModel
{
    ...
    private ObservableCollection<Recording> recordings = new ObservableCollection<Recording>();
    public ObservableCollection<Recording> Recordings{ get{ return this.recordings; } }
    public RecordingViewModel()
    {
        this.recordings.Add(new Recording(){ ArtistName = "Johann Sebastian Bach",
            CompositionName = "Mass in B minor", ReleaseDateTime = new DateTime(1748, 7, 8) });
        this.recordings.Add(new Recording(){ ArtistName = "Ludwig van Beethoven",
            CompositionName = "Third Symphony", ReleaseDateTime = new DateTime(1805, 2, 11) });
        this.recordings.Add(new Recording(){ ArtistName = "George Frideric Handel",
            CompositionName = "Serse", ReleaseDateTime = new DateTime(1737, 12, 3) });
    }
}
```

```cppwinrt
// RecordingViewModel.idl
// Add this property:
...
#include <winrt/Windows.Foundation.Collections.h>
...
Windows.Foundation.Collections.IVector<IInspectable> Recordings{ get; };
...

// RecordingViewModel.h
// Change the constructor declaration, and add this property and this field:
...
    RecordingViewModel();
    Windows::Foundation::Collections::IVector<Windows::Foundation::IInspectable> Recordings();

private:
    Windows::Foundation::Collections::IVector<Windows::Foundation::IInspectable> m_recordings;
...

// RecordingViewModel.cpp
// Update/add implementations like this:
...
RecordingViewModel::RecordingViewModel()
{
    std::vector<Windows::Foundation::IInspectable> recordings;

    Windows::Globalization::Calendar releaseDateTime;
    releaseDateTime.Month(7); releaseDateTime.Day(8); releaseDateTime.Year(1748);
    recordings.push_back(winrt::make<Recording>(L"Johann Sebastian Bach", L"Mass in B minor", releaseDateTime));

    releaseDateTime = Windows::Globalization::Calendar{};
    releaseDateTime.Month(11); releaseDateTime.Day(2); releaseDateTime.Year(1805);
    recordings.push_back(winrt::make<Recording>(L"Ludwig van Beethoven", L"Third Symphony", releaseDateTime));

    releaseDateTime = Windows::Globalization::Calendar{};
    releaseDateTime.Month(3); releaseDateTime.Day(12); releaseDateTime.Year(1737);
    recordings.push_back(winrt::make<Recording>(L"George Frideric Handel", L"Serse", releaseDateTime));

    m_recordings = winrt::single_threaded_observable_vector<Windows::Foundation::IInspectable>(std::move(recordings));
}

Windows::Foundation::Collections::IVector<Windows::Foundation::IInspectable> RecordingViewModel::Recordings() { return m_recordings; }
...
```

```cppcx
// Recording.h
...
public ref class RecordingViewModel sealed
{
private:
    ...
    Windows::Foundation::Collections::IVector<Recording^>^ recordings;
public:
    RecordingViewModel()
    {
        ...
        releaseDateTime = ref new Windows::Globalization::Calendar();
        releaseDateTime->Year = 1748;
        releaseDateTime->Month = 7;
        releaseDateTime->Day = 8;
        Recording^ recording = ref new Recording{ L"Johann Sebastian Bach", L"Mass in B minor", releaseDateTime };
        this->Recordings->Append(recording);
        releaseDateTime = ref new Windows::Globalization::Calendar();
        releaseDateTime->Year = 1805;
        releaseDateTime->Month = 2;
        releaseDateTime->Day = 11;
        recording = ref new Recording{ L"Ludwig van Beethoven", L"Third Symphony", releaseDateTime };
        this->Recordings->Append(recording);
        releaseDateTime = ref new Windows::Globalization::Calendar();
        releaseDateTime->Year = 1737;
        releaseDateTime->Month = 12;
        releaseDateTime->Day = 3;
        recording = ref new Recording{ L"George Frideric Handel", L"Serse", releaseDateTime };
        this->Recordings->Append(recording);
    }
    ...
    property Windows::Foundation::Collections::IVector<Recording^>^ Recordings
    {
        Windows::Foundation::Collections::IVector<Recording^>^ get()
        {
            if (this->recordings == nullptr)
            {
                this->recordings = ref new Platform::Collections::Vector<Recording^>();
            }
            return this->recordings;
        };
    }
};
```

然后，将 [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) 绑定到 **ViewModel.Recordings** 属性。

```xml
<Page x:Class="Quickstart.MainPage" ... >
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <ListView ItemsSource="{x:Bind ViewModel.Recordings}"
        HorizontalAlignment="Center" VerticalAlignment="Center"/>
    </Grid>
</Page>
```

我们尚未提供适用于 **Recording** 类的数据模板，因此 UI 框架的最佳做法是针对 [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) 中的每个项来调用 [**ToString**](https://docs.microsoft.com/dotnet/api/system.object.tostring?redirectedfrom=MSDN#System_Object_ToString)。 **ToString** 的默认实现是返回类型名称。

![绑定列表视图](images/xaml-databinding1.png)

若要解决此问题，我们可以是重写[ **ToString** ](https://docs.microsoft.com/dotnet/api/system.object.tostring?redirectedfrom=MSDN#System_Object_ToString)返回的值**OneLineSummary**，或者我们可以提供一个数据模板。 数据模板选项是更常见解决方案和一个更灵活。 使用内容控件的 [**ContentTemplate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.contentcontrol.contenttemplate) 属性或项目控件的 [**ItemTemplate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate) 属性来指定数据模板。 下面是可用于设计适用于 **Recording** 的数据模板以及结果图示的两种方式。

```xml
<ListView ItemsSource="{x:Bind ViewModel.Recordings}"
HorizontalAlignment="Center" VerticalAlignment="Center">
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="local:Recording">
            <TextBlock Text="{x:Bind OneLineSummary}"/>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

![绑定列表视图](images/xaml-databinding2.png)

```xml
<ListView ItemsSource="{x:Bind ViewModel.Recordings}"
HorizontalAlignment="Center" VerticalAlignment="Center">
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="local:Recording">
            <StackPanel Orientation="Horizontal" Margin="6">
                <SymbolIcon Symbol="Audio" Margin="0,0,12,0"/>
                <StackPanel>
                    <TextBlock Text="{x:Bind ArtistName}" FontWeight="Bold"/>
                    <TextBlock Text="{x:Bind CompositionName}"/>
                </StackPanel>
            </StackPanel>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

![绑定列表视图](images/xaml-databinding3.png)

有关 XAML 语法的详细信息，请参阅[使用 XAML 创建 UI](https://docs.microsoft.com/windows/uwp/design/basics/xaml-basics-ui)。 有关控件布局的详细信息，请参阅[使用 XAML 定义布局](https://docs.microsoft.com/windows/uwp/layout/layouts-with-xaml)。

## <a name="adding-a-details-view"></a>添加详细信息视图

你可以选择在 [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) 项目中显示 **Recording** 对象的所有详细信息。 但这样做会占用大量空间。 不过，你可以在该项目中仅显示足够多的数据来标识它，然后在用户做出选择时，你可以在 UI 的单个部分（即，详细信息视图）中显示选定项的所有详细信息。 这种排列也称为主视图/详细信息视图或列表/详细信息视图。

有两种方法可用来执行此操作。 你可以将详细信息视图绑定到 [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) 的 [**SelectedItem**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selecteditem) 属性。 也可以使用[ **CollectionViewSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource)，在这种情况下绑定同时**ListView**和的详细信息视图到**CollectionViewSource**（这样做因此会负责的当前选定项）。 下面，显示了这两种技术，它们都提供相同的结果 （如图所示）。

> [!NOTE]
> 到目前为止，本主题中我们仅使用了 [{x:Bind} 标记扩展](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)，而将在下面介绍的这两种技术要求更为灵活（但性能较低）的 [{Binding} 标记扩展](https://docs.microsoft.com/windows/uwp/xaml-platform/binding-markup-extension)。

如果您使用的C++/WinRT 或视觉对象C++组件扩展 (C++/CX) 然后，使用[{Binding}](https://docs.microsoft.com/windows/uwp/xaml-platform/binding-markup-extension)标记扩展，您将需要添加[ **BindableAttribute**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.BindableAttribute)你想要将绑定到任何运行时类的属性。 若要使用[{x： 绑定}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)，不需要该属性。

> [!IMPORTANT]
> 如果您使用的[ C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)，然后[ **BindableAttribute** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.BindableAttribute)属性是如果你已安装 Windows SDK 版本 10.0.17763.0 (Windows 10，可用版本 1809年），或更高版本。 如果没有该属性，你将需要实现[ICustomPropertyProvider](/uwp/api/windows.ui.xaml.data.icustompropertyprovider)并[ICustomProperty](/uwp/api/windows.ui.xaml.data.icustomproperty)接口，以便能够使用[{Binding}](https://docs.microsoft.com/windows/uwp/xaml-platform/binding-markup-extension)标记扩展插件。

首先介绍的是 [**SelectedItem**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.selector.selecteditem) 技术。

```csharp
// No code changes necessary for C#.
```

```cppwinrt
// Recording.idl
// Add this attribute:
...
[Windows.UI.Xaml.Data.Bindable]
runtimeclass Recording
...
```

```cppcx
[Windows::UI::Xaml::Data::Bindable]
public ref class Recording sealed
{
    ...
};
```

只需对标记进行更改。

```xml
<Page x:Class="Quickstart.MainPage" ... >
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <StackPanel HorizontalAlignment="Center" VerticalAlignment="Center">
            <ListView x:Name="recordingsListView" ItemsSource="{x:Bind ViewModel.Recordings}">
                <ListView.ItemTemplate>
                    <DataTemplate x:DataType="local:Recording">
                        <StackPanel Orientation="Horizontal" Margin="6">
                            <SymbolIcon Symbol="Audio" Margin="0,0,12,0"/>
                            <StackPanel>
                                <TextBlock Text="{x:Bind CompositionName}"/>
                            </StackPanel>
                        </StackPanel>
                    </DataTemplate>
                </ListView.ItemTemplate>
            </ListView>
            <StackPanel DataContext="{Binding SelectedItem, ElementName=recordingsListView}"
            Margin="0,24,0,0">
                <TextBlock Text="{Binding ArtistName}"/>
                <TextBlock Text="{Binding CompositionName}"/>
                <TextBlock Text="{Binding ReleaseDateTime}"/>
            </StackPanel>
        </StackPanel>
    </Grid>
</Page>
```

对于 [**CollectionViewSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource) 技术，先添加 **CollectionViewSource** 作为页面资源。

```xml
<Page.Resources>
    <CollectionViewSource x:Name="RecordingsCollection" Source="{x:Bind ViewModel.Recordings}"/>
</Page.Resources>
```

然后，在 [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView)（无需再对其进行命名）和详细信息视图上调节绑定，以使用 [**CollectionViewSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource)。 请注意，如果将详细信息视图直接绑定到 **CollectionViewSource**，即表示你希望绑定到绑定中的当前项目，其中在集合本身上无法找到路径。 无需将 **CurrentItem** 属性指定为绑定路径，尽管由于不确定性你可以这样做）。

```xml
...
<ListView ItemsSource="{Binding Source={StaticResource RecordingsCollection}}">
...
<StackPanel DataContext="{Binding Source={StaticResource RecordingsCollection}}" ...>
...
```

而在每种情况下结果均相同。

> [!NOTE]
> 如果您使用的C++，则你的 UI 不会完全像下图： 的呈现**ReleaseDateTime**属性不相同。 请参阅有关此详细讨论下的一节。

![绑定列表视图](images/xaml-databinding4.png)

## <a name="formatting-or-converting-data-values-for-display"></a>设置数据值的格式或对其进行转换，以供显示

没有与上面的呈现问题。 **ReleaseDateTime**属性不是只是日期，它是[ **DateTime** ](/uwp/api/windows.foundation.datetime) (如果您使用的C++，则它是[ **日历**](/uwp/api/windows.globalization.calendar)). 因此，在C#，显示与不是我们需要更高的精度。 然后在C++作为类型名称呈现。 一种解决方案是添加到一个字符串属性**录制**返回的等效类`this.ReleaseDateTime.ToString("d")`。 命名该属性**ReleaseDate**将指示它返回一个日期和不日期和时间。 将其命名为 **ReleaseDateAsString** 可进一步指示它将返回一个字符串。

一个更灵活的解决方案是使用称为值转换器的工具。 下面是如何创作你自己的值转换器的示例。 如果您使用的C#，然后添加以下代码到您`Recording.cs`源代码文件。 如果您使用的C++/WinRT，然后添加一个新**Midl 文件 (.idl)** 项到项目中，名为如中所示C++/WinRT 代码示例列表下，生成项目以生成`StringFormatter.h`并`.cpp`，将这些文件添加到你的项目，然后将代码列表粘贴到它们。 此外将添加`#include "StringFormatter.h"`到`MainPage.h`。

```csharp
public class StringFormatter : Windows.UI.Xaml.Data.IValueConverter
{
    // This converts the value object to the string to display.
    // This will work with most simple types.
    public object Convert(object value, Type targetType,
        object parameter, string language)
    {
        // Retrieve the format string and use it to format the value.
        string formatString = parameter as string;
        if (!string.IsNullOrEmpty(formatString))
        {
            return string.Format(formatString, value);
        }

        // If the format string is null or empty, simply
        // call ToString() on the value.
        return value.ToString();
    }

    // No need to implement converting back on a one-way binding
    public object ConvertBack(object value, Type targetType,
        object parameter, string language)
    {
        throw new NotImplementedException();
    }
}
```

```cppwinrt
// StringFormatter.idl
namespace Quickstart
{
    runtimeclass StringFormatter : [default] Windows.UI.Xaml.Data.IValueConverter
    {
        StringFormatter();
    }
}

// StringFormatter.h
#pragma once

#include "StringFormatter.g.h"
#include <sstream>

namespace winrt::Quickstart::implementation
{
    struct StringFormatter : StringFormatterT<StringFormatter>
    {
        StringFormatter() = default;

        Windows::Foundation::IInspectable Convert(Windows::Foundation::IInspectable const& value, Windows::UI::Xaml::Interop::TypeName const& targetType, Windows::Foundation::IInspectable const& parameter, hstring const& language);
        Windows::Foundation::IInspectable ConvertBack(Windows::Foundation::IInspectable const& value, Windows::UI::Xaml::Interop::TypeName const& targetType, Windows::Foundation::IInspectable const& parameter, hstring const& language);
    };
}

namespace winrt::Quickstart::factory_implementation
{
    struct StringFormatter : StringFormatterT<StringFormatter, implementation::StringFormatter>
    {
    };
}

// StringFormatter.cpp
#include "pch.h"
#include "StringFormatter.h"
#include "StringFormatter.g.cpp"

namespace winrt::Quickstart::implementation
{
    Windows::Foundation::IInspectable StringFormatter::Convert(Windows::Foundation::IInspectable const& value, Windows::UI::Xaml::Interop::TypeName const& /* targetType */, Windows::Foundation::IInspectable const& /* parameter */, hstring const& /* language */)
    {
        // Retrieve the value as a Calendar.
        Windows::Globalization::Calendar valueAsCalendar{ value.as<Windows::Globalization::Calendar>() };

        std::wstringstream wstringstream;
        wstringstream << L"Released: ";
        wstringstream << valueAsCalendar.MonthAsNumericString().c_str();
        wstringstream << L"/" << valueAsCalendar.DayAsString().c_str();
        wstringstream << L"/" << valueAsCalendar.YearAsString().c_str();
        return winrt::box_value(hstring{ wstringstream.str().c_str() });
    }

    Windows::Foundation::IInspectable StringFormatter::ConvertBack(Windows::Foundation::IInspectable const& /* value */, Windows::UI::Xaml::Interop::TypeName const& /* targetType */, Windows::Foundation::IInspectable const& /* parameter */, hstring const& /* language */)
    {
        throw hresult_not_implemented();
    }
}
```

```cppcx
...
public ref class StringFormatter sealed : Windows::UI::Xaml::Data::IValueConverter
{
public:
    virtual Platform::Object^ Convert(Platform::Object^ value, TypeName targetType, Platform::Object^ parameter, Platform::String^ language)
    {
        // Retrieve the value as a Calendar.
        Windows::Globalization::Calendar^ valueAsCalendar = dynamic_cast<Windows::Globalization::Calendar^>(value);

        std::wstringstream wstringstream;
        wstringstream << L"Released: ";
        wstringstream << valueAsCalendar->MonthAsNumericString()->Data();
        wstringstream << L"/" << valueAsCalendar->DayAsString()->Data();
        wstringstream << L"/" << valueAsCalendar->YearAsString()->Data();
        return ref new Platform::String(wstringstream.str().c_str());
    }

    // No need to implement converting back on a one-way binding
    virtual Platform::Object^ ConvertBack(Platform::Object^ value, TypeName targetType, Platform::Object^ parameter, Platform::String^ language)
    {
        throw ref new Platform::NotImplementedException();
    }
};
...
```

> [注意 ！]有关C++/WinRT 代码列表更高版本，在`StringFormatter.idl`，我们使用[默认特性](https://docs.microsoft.com/windows/desktop/midl/default)声明**IValueConverter**为默认接口。 在列表中， **StringFormatter**具有只有一个构造函数，且不包括方法，因此为其生成没有默认接口。 `default`属性是如果你不会添加到的实例成员最佳**StringFormatter**，因为没有 QueryInterface 需要调用**IValueConverter**方法。 或者，可以提示默认值**IStringFormatter**接口以生成，并执行操作，添加批注在运行时类本身与[default_interface 属性](https://docs.microsoft.com/uwp/midl-3/predefined-attributes#the-default_interface-attribute)。 选项是如果你添加到的实例成员最佳**StringFormatter**通常比的方法调用的**IValueConverter**是，因为并没有 QueryInterface 需要调用实例成员。

现在，我们可以添加的实例**StringFormatter**作为页面资源并使用它的绑定中**TextBlock**显示**ReleaseDateTime**属性。

```xml
<Page.Resources>
    <local:StringFormatter x:Key="StringFormatterValueConverter"/>
</Page.Resources>
...
<TextBlock Text="{Binding ReleaseDateTime,
    Converter={StaticResource StringFormatterValueConverter},
    ConverterParameter=Released: \{0:d\}}"/>
...
```

正如您所看到上面，用于设置格式的灵活性我们使用标记将格式字符串传递到转换器通过转换器参数。 在本主题中，仅在所示的代码示例中C#值转换器使用该参数。 但您可以轻松地传递C++-设置样式格式字符串作为转换器参数，并使用，在你使用的格式设置的值转换器函数如**wprintf**或**swprintf**。

下面是结果。

![显示具有自定义格式的日期](images/xaml-databinding5.png)

> [!NOTE]
> 从 Windows 10，版本 1607 中，开始 XAML 框架提供了内置布尔值可见性转换器。 转换器 maps **，则返回 true**到**Visibility.Visible**枚举值和**false**到**Visibility.Collapsed**以便可以将绑定可见性属性设置为一个布尔值，而无需创建一个转换器。 若要使用内置转换器，你的应用的最低目标 SDK 版本必须为 14393 或更高版本。 当你的应用面向较早版本的 Windows 10 时，你无法使用它。 有关目标版本的详细信息，请参阅[版本自适应代码](https://docs.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code)。

## <a name="see-also"></a>请参阅
* [数据绑定](index.md)
