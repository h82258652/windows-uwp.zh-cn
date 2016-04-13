---
ms.assetid: 089660A2-7CAE-4911-9994-F619C5D22287
title: 设计面图上以及用于原型制作的示例数据
description: 也许是不可能或不需要（可能是出于隐私或性能的原因）为你的应用在 Microsoft Visual Studio 或 Blend for Visual Studio 中的设计图面上显示实时数据。
---
设计面图上以及用于原型制作的示例数据
=============================================================================================

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**注意** 你需要示例数据的程度（以及它可以给你带来多少帮助）取决于你的绑定是使用 [{Binding} 标记扩展](https://msdn.microsoft.com/library/windows/apps/Mt204782)还是使用 [{x: Bind} 标记扩展](https://msdn.microsoft.com/library/windows/apps/Mt204783)。 本主题中所述的技术基于对 [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713) 的使用，因此它们仅适用于 **{Binding}**。 但是，如果你使用的是 **{x:Bind}**，而你的绑定至少显示了设计面图上的占位符值（甚至是项目控件的占位符值），这样你便无需完全相同的示例数据。

也许是不可能或不需要（可能是出于隐私或性能的原因）为你的应用在 Microsoft Visual Studio 或 Blend for Visual Studio 中的设计图面上显示实时数据。 为了使你的控件填充数据（以便你可以处理应用的布局、模板和其他视觉属性），你可以通过各种方式使用设计时示例数据。 如果你正要生成一个草图（或原型）应用，则示例数据可能真的非常有用而且节省时间。 你可以在运行时在草图或原型中使用示例数据来阐明你的想法，而无需连接到真实且实时的数据。

在标记中设置 DataContext
-----------------------------

使用命令性代码（在代码隐藏中）设置页面或用户控件的 [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713) 来查看模型实例是相当常见的开发人员的做法。

``` csharp
public MainPage()
{
    InitializeComponent();
    this.DataContext = new BookstoreViewModel();
}
```

但是，如果你执行此操作，你的页面将不再是它本应该具有的“可设计”。 原因是当在 Visual Studio 或 Blend for Visual Studio 中打开你的 XAML 页面时，分配 **DataContext** 值的命令性代码将不再运行（实际上，没有执行你的任何代码隐藏）。 当然，XAML 工具的确会分析你的标记并将在其中声明的任何对象实例化，但实际上它们不会实例化你的页面类型本身。 因此，在你的控件或在“创建数据绑定”****对话框中，你将看不到任何数据，而且你的页面的样式设计和布局将会变得更有挑战性。

![稀疏的设计 UI。](images/displaying-data-in-the-designer-01.png)

第一个补救方法是尝试注释掉该 **DataContext** 分配，改为在页面标记中设置 **DataContext**。 这样，你的实时数据在设计时和运行时都会显示出来。 若要执行此操作，请首先打开你的 XAML 页面。 然后，在**“文档大纲”**窗口中，单击根可设计元素（通常带有标签 **\[Page\]**）来选择它。 在**“属性”**窗口中，查找**“DataContext”**属性（位于“通用”类别内），然后单击**“新建”**。 从**“选择对象”**对话框中单击你的视图模型类型，然后单击**“确定”**。

![用于设置 DataContext 的 UI。](images/displaying-data-in-the-designer-02.png)

下面是产生的标记的外观。

``` xml
<Page ... >
    <Page.DataContext>
        <local:BookstoreViewModel/>
    </Page.DataContext>
```

既然你的绑定可以进行解析，那么下面是设计图面的外观。 请注意，基于 **DataContext** 类型和可以绑定到的属性，**“创建数据绑定”**对话框中的**“路径”**选取器现已填充。

![可设计的 UI。](images/displaying-data-in-the-designer-03.png)

“创建数据绑定”****对话框仅需要从一种类型中进行工作，然而绑定却需要使用值初始化多个属性。 如果你不希望在设计时访问你的云服务（由于性能、购买数据传输、隐私问题，等等），则你的初始化代码可能会进行检查来查看你的应用是否在设计工具（如 Visual Studio 或 Blend for Visual Studio）中运行，以及在此情况下是否会加载示例数据以便仅在设计时使用。

``` csharp
if (Windows.ApplicationModel.DesignMode.DesignModeEnabled)
{
    // Load design-time books.
}
else
{
    // Load books from a cloud service.
}
```

如果你需要将参数传递到你的初始化代码，则可以使用视图模型定位器。 视图模型定位器是一个类，你可以将其放入应用资源中。 它具有公开视图模型的属性，而且你的页面的 **DataContext** 会绑定到该属性。 定位器或视图模型可以使用的另一种模式是依存关系注入，如果适用的话，它可以构造设计时或运行时数据提供程序（其中每个都实现一个共用接口）。

“来自类中的示例数据”，和设计时属性。
---------------------------------------------------------------------------------------

不管出于何种原因，如果上一部分中的选项没有一个适合你，则通过 XAML 工具的功能和设计时属性，你仍有足够的设计时数据选项可用。 其中一种不错的选择就是 Blend for Visual Studio 中的**“从类中创建示例数据”**功能。 你可以在“数据”****面板顶部的其中一个按钮中找到该命令。

你只需指定一个类以供命令使用即可。 然后该命令会为你做两件重要的事情。 首先，它会生成一个 XAML 文件，它包含了适用于以递归方式对你所选类的一个实例及其所有成员进行融合的示例数据（实际上，使用工具可以起到与 XAML 或 JSON 文件同等的作用）。 然后，它会使用你所选类的架构来填充**“数据”**面板。 你可以随后将来自**“数据”**面板的成员拖动到设计图面上来执行各种任务。 根据你拖动的内容和放置的位置，你可以将绑定添加到现有控件（使用 **{Binding}**），或创建新的控件并同时将它们绑定。 不管是哪一种情况，该操作还为你在页面的布局根上设置了设计时数据上下文 (**d:DataContext**)（如果其中一个未设置）。 该设计时数据上下文使用 **d:DesignData** 属性来从已生成的 XAML 文件中获取其示例数据（顺便提一下，你可以在你的项目中找到该文件，也可以对其进行编辑以便它包含所需的示例数据）。

``` xml
<Page ...
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">
    <Grid ... d:DataContext="{d:DesignData /SampleData/RecordingViewModelSampleData.xaml}"/>
        <ListView ItemsSource="{Binding Recordings}" ... />
        ...
    </Grid>
</Page>
```

各种 xmlns 声明意味着具有 **d:** 前缀的属性只有在设计时才会解释，而且只有在运行时才会忽略。 因此 **d:DataContext** 属性只会在设计时影响 [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713) 属性的值，而在运行时不起任何作用。 如果你愿意，甚至可以在标记中同时设置 **d:DataContext** 和 **DataContext**。 **d:DataContext** 将在设计时进行替代，而 **DataContext** 将在运行时进行替代。 这些相同的替代规则适用于所有设计时和运行时属性。

**d:DataContext** 属性及所有其他设计时属性都已记录在[设计时属性](http://go.microsoft.com/fwlink/p/?LinkId=272504)主题中，这对通用 Windows 平台 (UWP) 应用仍然有效。

[
            **CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833) 没有 **DataContext** 属性，不过它有 **Source** 属性。 因此，你可以使用 **d:Source** 属性在 **CollectionViewSource** 上设置仅设计时示例数据。

``` xml
    <Page.Resources>
        <CollectionViewSource x:Name="RecordingsCollection" Source="{Binding Recordings}"
            d:Source="{d:DesignData /SampleData/RecordingsSampleData.xaml}"/>
    </Page.Resources>

    ...

        <ListView ItemsSource="{Binding Source={StaticResource RecordingsCollection}}" ... />
    ...
```

为此，你需要有一个名为 `Recordings : ObservableCollection<Recording>` 的类，并编辑示例数据 XAML 文件以便它仅包含 **Recordings** 对象（**Recording** 对象位于其内部），如此处所示。

``` xml
<Quickstart:Recordings xmlns:Quickstart="using:Quickstart">
    <Quickstart:Recording ArtistName="Mollis massa" CompositionName="Cubilia metus"
        OneLineSummary="Morbi adipiscing sed" ReleaseDateTime="01/01/1800 15:53:17"/>
    <Quickstart:Recording ArtistName="Vulputate nunc" CompositionName="Parturient vestibulum"
        OneLineSummary="Dapibus praesent netus amet vestibulum" ReleaseDateTime="01/01/1800 15:53:17"/>
    <Quickstart:Recording ArtistName="Phasellus accumsan" CompositionName="Sit bibendum"
        OneLineSummary="Vestibulum egestas montes dictumst" ReleaseDateTime="01/01/1800 15:53:17"/>
</Quickstart:Recordings>
```

如果你使用 JSON 示例数据文件而不是 XAML，则必须设置 **Type** 属性。

``` xml
    d:Source="{d:DesignData /SampleData/RecordingsSampleData.json, Type=local:Recordings}"
```

到目前为止，我们一直在使用 **d:DesignData** 从 XAML 或 JSON 文件中加载设计时示例数据。 另一种替代选项是 **d:DesignInstance** 标记扩展，它指示设计时源基于 **Type** 属性指定的类。 下面提供了一个示例。

``` xml
    <CollectionViewSource x:Name="RecordingsCollection" Source="{Binding Recordings}"
        d:Source="{d:DesignInstance Type=local:Recordings, IsDesignTimeCreatable=True}"/>
        ```

The **IsDesignTimeCreatable** property indicates that the design tool should actually create an instance of the class, which implies that the class has a public default constructor, and that it populates itself with data (either real or sample). If you don't set **IsDesignTimeCreatable** (or if you set it to **False**) then you won't get sample data displayed on the design surface. All the design tool does in that case is to parse the class for its bindable properties and display these in the the **Data** panel and in the **Create Data Binding** dialog.

Sample data for prototyping
--------------------------------------------------------

For prototyping, you want sample data at both design-time and at run-time. For that use case, Blend for Visual Studio has the **New Sample Data** feature. You can find that command on one of the buttons at the top of the **Data** panel.

Instead of specifying a class, you can actually design the schema of your sample data source right in the **Data** panel. You can also edit sample data values in the **Data** panel: there's no need to open and edit a file (although, you can still do that if you prefer).

The **New Sample Data** feature uses [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713), and not **d:DataContext**, so that the sample data is available when you run your sketch or prototype as well as while you're designing it. And the **Data** panel really speeds up your designing and binding tasks. For example, simply dragging a collection property from the **Data** panel onto the design surface generates a data-bound items control and the necessary templates, all ready to build and run.

![Sample data for prototyping.](images/displaying-data-in-the-designer-04.png)

 

 






<!--HONumber=Mar16_HO1-->


