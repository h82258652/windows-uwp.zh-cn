---
author: mcleblanc
ms.assetid: A9D54DEC-CD1B-4043-ADE4-32CD4977D1BF
title: "数据绑定概述"
description: "本主题介绍了如何在通用 Windows 平台 (UWP) 应用中将控件（或其他 UI 元素）绑定到单个项目，或者将项目控件绑定到项目集合。"
ms.author: markl
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: d3be03785e977229c6aa38dba728e6ef6552a23e
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
<a name="data-binding-overview"></a>数据绑定概述
=====================

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


本主题介绍了如何在通用 Windows 平台 (UWP) 应用中将控件（或其他 UI 元素）绑定到单个项目，或者将项目控件绑定到项目集合。 此外，我们还介绍了如何控制项的呈现、基于所选内容实现详细信息视图，以及转换数据以供显示。 有关更多详细信息，请参阅[深入了解数据绑定](data-binding-in-depth.md)。

<a name="prerequisites"></a>先决条件
-------------------------------------------------------------------------------------------------------------

此主题假设你知道如何创建基本的 UWP 应用。 有关创建你的第一个 UWP 应用的说明，请参阅 [Windows 应用入门](https://developer.microsoft.com/windows/getstarted)。

<a name="create-the-project"></a>创建项目
---------------------------------------------------------------------------------------------------------------------------------

创建一个新的“空白应用程序(Windows 通用)”****项目。 将它命名为“Quickstart”。

<a name="binding-to-a-single-item"></a>绑定到单个项目
---------------------------------------------------------------------------------------------------------------------------------------------------------

每个绑定均由一个绑定目标和一个绑定源构成。 通常，绑定目标是控件或其他 UI 元素的属性，而绑定源是类实例（数据模型或视图模型）的属性。 本示例演示了如何将控件绑定到单个项目。 绑定目标是 **TextBlock** 的 **Text** 属性。 绑定源是一个名为 **Recording** 的简单类的实例，该类表示音频录制。 我们先来看一下类。

向项目添加一个新类、将其命名为 Recording.cs（如果使用的是 C#、C++，下面同样提供了代码片段），然后向其中添加此代码。

> [!div class="tabbedCodeSnippets"]
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
```cpp
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
                    return ref new Platform::String(wstringstream.str().c-str());
                }
            }
        };
        public ref class RecordingViewModel sealed
        {
        private:
            Recording^ defaultRecording;
        public:
            RecordingViewModel()
            {
                Windows::Globalization::Calendar^ releaseDateTime = ref new Windows::Globalization::Calendar();
                releaseDateTime->Month = 1;
                releaseDateTime->Day = 1;
                releaseDateTime->Year = 1761;
                this->defaultRecording = ref new Recording{ L"Wolfgang Amadeus Mozart", L"Andante in C for Piano", releaseDateTime };
            }
            property Recording^ DefaultRecording
            {
                Recording^ get() { return this->defaultRecording; };
            }
        };
    }
```

接下来，从表示标记页的类公开绑定源类。 我们通过将类型 **RecordingViewModel** 的属性添加到 **MainPage** 来执行此操作。

> [!div class="tabbedCodeSnippets"]
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
            public RecordingViewModel ViewModel { get; set; }
        }
    }
```
```cpp
    namespace Quickstart
    {
        public ref class MainPage sealed
        {
        private:
            RecordingViewModel^ viewModel;
        public:
            MainPage()
            {
                InitializeComponent();
                this->viewModel = ref new RecordingViewModel();
            }
            property RecordingViewModel^ ViewModel
            {
                RecordingViewModel^ get() { return this->viewModel; };
            }
        };
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

下面是结果。

![绑定文本块](images/xaml-databinding0.png)

<a name="binding-to-a-collection-of-items"></a>绑定到项目集合
------------------------------------------------------------------------------------------------------------------

一个常见情形是绑定到业务对象的集合。 在 C# 和 Visual Basic 中，通用 [**ObservableCollection&lt;T&gt;**](https://msdn.microsoft.com/library/windows/apps/xaml/ms668604.aspx) 类是数据绑定的一个很好的集合选择，因为它实现了 [**INotifyPropertyChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/system.componentmodel.inotifypropertychanged.aspx) 和 [**INotifyCollectionChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.specialized.inotifycollectionchanged.aspx) 接口。 当添加或删除项目或者列表本身的属性更改时，这些接口将向绑定提供更改通知。 如果你希望你的绑定控件使用集合中的对象属性更改进行更新，则业务对象也应该实现 **INotifyPropertyChanged**。 有关详细信息，请参阅[深入了解数据绑定](data-binding-in-depth.md)。

下面这个示例演示了将 [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) 绑定到 `Recording` 对象的集合。 让我们先将该集合添加到视图模型。 只需将这些新成员添加到 **RecordingViewModel** 类。

> [!div class="tabbedCodeSnippets"]
```csharp
    public class RecordingViewModel
    {
        ...
        private ObservableCollection<Recording> recordings = new ObservableCollection<Recording>();
        public ObservableCollection<Recording> Recordings { get { return this.recordings; } }
        public RecordingViewModel()
        {
            this.recordings.Add(new Recording() { ArtistName = "Johann Sebastian Bach",
            CompositionName = "Mass in B minor", ReleaseDateTime = new DateTime(1748, 7, 8) });
            this.recordings.Add(new Recording() { ArtistName = "Ludwig van Beethoven",
            CompositionName = "Third Symphony", ReleaseDateTime = new DateTime(1805, 2, 11) });
            this.recordings.Add(new Recording() { ArtistName = "George Frideric Handel",
            CompositionName = "Serse", ReleaseDateTime = new DateTime(1737, 12, 3) });
        }
    }
```
```cpp
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
            releaseDateTime->Month = 7;
            releaseDateTime->Day = 8;
            releaseDateTime->Year = 1748;
            Recording^ recording = ref new Recording{ L"Johann Sebastian Bach", L"Mass in B minor", releaseDateTime };
            this->Recordings->Append(recording);
            releaseDateTime = ref new Windows::Globalization::Calendar();
            releaseDateTime->Month = 2;
            releaseDateTime->Day = 11;
            releaseDateTime->Year = 1805;
            recording = ref new Recording{ L"Ludwig van Beethoven", L"Third Symphony", releaseDateTime };
            this->Recordings->Append(recording);
            releaseDateTime = ref new Windows::Globalization::Calendar();
            releaseDateTime->Month = 12;
            releaseDateTime->Day = 3;
            releaseDateTime->Year = 1737;
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

然后，将 [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) 绑定到 **ViewModel.Recordings** 属性。

```xml
<Page x:Class="Quickstart.MainPage" ... >
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <ListView ItemsSource="{x:Bind ViewModel.Recordings}"
        HorizontalAlignment="Center" VerticalAlignment="Center"/>
    </Grid>
</Page>
```

我们尚未提供适用于 **Recording** 类的数据模板，因此 UI 框架的最佳做法是针对 [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) 中的每个项来调用 [**ToString**](https://msdn.microsoft.com/library/windows/apps/system.object.tostring.aspx)。 **ToString** 的默认实现是返回类型名称。

![绑定列表视图](images/xaml-databinding1.png)

为了解决此问题，我们可以重写 [**ToString**](https://msdn.microsoft.com/library/windows/apps/system.object.tostring.aspx) 以返回 **OneLineSummary** 的值，或者提供一个数据模板。 数据模板选项更为常见且更为灵活。 使用内容控件的 [**ContentTemplate**](https://msdn.microsoft.com/library/windows/apps/BR209369) 属性或项目控件的 [**ItemTemplate**](https://msdn.microsoft.com/library/windows/apps/BR242830) 属性来指定数据模板。 下面是可用于设计适用于 **Recording** 的数据模板以及结果图示的两种方式。

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

有关 XAML 语法的详细信息，请参阅[使用 XAML 创建 UI](https://msdn.microsoft.com/library/windows/apps/Mt228349)。 有关控件布局的详细信息，请参阅[使用 XAML 定义布局](https://msdn.microsoft.com/library/windows/apps/Mt228350)。

<a name="adding-a-details-view"></a>添加详细信息视图
-----------------------------------------------------------------------------------------------------

你可以选择在 [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) 项目中显示 **Recording** 对象的所有详细信息。 但这样做会占用大量空间。 不过，你可以在该项目中仅显示足够多的数据来标识它，然后在用户做出选择时，你可以在 UI 的单个部分（即，详细信息视图）中显示选定项的所有详细信息。 这种排列也称为主视图/详细信息视图或列表/详细信息视图。

有两种方法可用来执行此操作。 你可以将详细信息视图绑定到 [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) 的 [**SelectedItem**](https://msdn.microsoft.com/library/windows/apps/BR209770) 属性。 或者，可以使用 [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833)：将 **ListView** 和详细信息视图同时绑定到 **CollectionViewSource**（这将为你处理当前选定的项目）。 下面介绍这两种技术，它们都提供图中所示的相同结果。

> [!NOTE]
> 到目前为止，本主题中我们仅使用了 [{x:Bind} 标记扩展](https://msdn.microsoft.com/library/windows/apps/Mt204783)，而将在下面介绍的这两种技术要求更为灵活（但性能较低）的 [{Binding} 标记扩展](https://msdn.microsoft.com/library/windows/apps/Mt204782)。

首先介绍的是 [**SelectedItem**](https://msdn.microsoft.com/library/windows/apps/BR209770) 技术。 如果你使用的是 Visual C++ 组件扩展 (C++/CX)，则因为我们将使用 [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782)，因此你需要将 [**BindableAttribute**](https://msdn.microsoft.com/library/windows/apps/Hh701872) 属性添加到 **Recording** 类。

```cpp
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

对于 [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833) 技术，先添加 **CollectionViewSource** 作为页面资源。

```xml
    <Page.Resources>
        <CollectionViewSource x:Name="RecordingsCollection" Source="{x:Bind ViewModel.Recordings}"/>
    </Page.Resources>
```

然后，在 [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878)（无需再对其进行命名）和详细信息视图上调节绑定，以使用 [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833)。 请注意，如果将详细信息视图直接绑定到 **CollectionViewSource**，即表示你希望绑定到绑定中的当前项目，其中在集合本身上无法找到路径。 无需将 **CurrentItem** 属性指定为绑定路径，尽管由于不确定性你可以这样做）。

```xml
    ...

    <ListView ItemsSource="{Binding Source={StaticResource RecordingsCollection}}">

    ...

    <StackPanel DataContext="{Binding Source={StaticResource RecordingsCollection}}" ...>
    ...
```

而在每种情况下结果均相同。

![绑定列表视图](images/xaml-databinding4.png)

<a name="formatting-or-converting-data-values-for-display"></a>设置数据值的格式或对其进行转换，以供显示
--------------------------------------------------------------------------------------------------------------------------------------------

以上呈现有一个小问题。 **ReleaseDateTime** 属性不仅仅是一个日期，还可以是 [**DateTime**](https://msdn.microsoft.com/library/windows/apps/xaml/system.datetime.aspx)，因此应以高于所需精度的精度来显示它。 一个解决方案是，将字符串属性添加到返回 `this.ReleaseDateTime.ToString("d")` 的 **Recording** 类。 将该属性命名为 **ReleaseDate** 可指示它将返回一个日期，而不是返回日期和时间。 将其命名为 **ReleaseDateAsString** 可进一步指示它将返回一个字符串。

一个更灵活的解决方案是使用称为值转换器的工具。 下面是如何创作你自己的值转换器的示例。 将此代码添加到你的 Recording.cs 源代码文件。

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

现在，我们可以添加 **StringFormatter** 的实例作为页面资源并将其用于绑定。 我们将格式字符串从标记传递到该转换器，以便于更为灵活地设置格式。

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

此处是结果。

![显示具有自定义格式的日期](images/xaml-databinding5.png)

> [!NOTE]
> 从 Windows 10 版本 1607 开始，XAML 框架向 Visibility 转换器提供内置布尔值。 转换器将 **true** 映射到 **Visible** 枚举值并将 **false** 映射到 **Collapsed**，以便你可以将 Visibility 属性绑定到布尔值，而无需创建转换器。 若要使用内置转换器，你的应用的最低目标 SDK 版本必须为 14393 或更高版本。 当你的应用面向较早版本的 Windows10 时，你无法使用它。 有关目标版本的详细信息，请参阅[版本自适应代码](https://msdn.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code)。

## <a name="see-also"></a>另请参阅
- [数据绑定](index.md)
