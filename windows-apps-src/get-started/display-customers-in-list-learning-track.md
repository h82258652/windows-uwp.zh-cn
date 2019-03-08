---
title: 学习轨迹 - 以列表形式显示客户
description: 了解以列表形式显示客户对象的集合需要执行的操作。
ms.date: 05/07/2018
ms.topic: article
keywords: 入门, uwp, windows 10, 了解轨迹, 数据绑定, 列表
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: bd4a1f6747ea68623039b7eac22ac08aaa15d9ea
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651372"
---
# <a name="display-customers-in-a-list"></a>以列表形式显示客户

显示和处理 UI 中的实际数据对于许多应用的功能至关重要。 本文将向你介绍以列表形式显示客户对象的集合需要了解的内容。

这不是教程。 如果想查看教程，请参阅我们的[数据绑定教程](../data-binding/xaml-basics-data-binding.md)，它提供了分步指南体验。

我们将从快速讨论数据绑定开始 - 什么是数据绑定，它如何工作。 然后，我们向 UI 添加 **ListView**、添加数据绑定，并使用其他功能自定义数据绑定。 

## <a name="what-do-you-need-to-know"></a>需要了解哪些内容？

数据绑定是在其 UI 中显示应用数据的方式。 这允许在应用中*分离出关注内容*，将 UI 与其他代码分开。 这将创建更便于阅读和维护的更简洁的概念模型。

每个数据绑定有两个部分：

* 提供要绑定数据的源。
* 显示数据的 UI 中的目标。

若要实现数据绑定，你需要将代码添加到向绑定提供数据的源。 你还需要将两个标记扩展中的一个添加到你的 XAML 以指定数据源属性。 以下是两者之间的主要区别：

* [**x： 绑定**](../xaml-platform/x-bind-markup-extension.md)强类型，并在更好的性能的编译时生成代码。 x:Bind 默认为一次性绑定，其针对不改变的只读数据的快速显示进行了优化。
* [**绑定**](../xaml-platform/binding-markup-extension.md)是弱类型化并在运行时组合。 这将使性能比使用 x:Bind 时更差。 在几乎所有情况下，都应使用 x:Bind 而不是 Binding。 不过，你可能会在较旧的代码中遇到后者。 Binding 默认为单向数据传输，其针对可能在源改变的只读数据进行了优化。

建议你尽可能使用 **x:Bind**，我们将在本文的代码段中介绍它。 有关具体区别的详细信息，请参阅 [{x:Bind} 和 {Binding} 功能比较](../data-binding/data-binding-in-depth.md#xbind-and-binding-feature-comparison)。

## <a name="create-a-data-source"></a>创建数据源

首先，你需要一个表示客户数据的类。 为了向你提供一个参考点，我们将在此基本示例中介绍该过程：

```csharp
public class Customer
{
    public string Name { get; set; }
}
```

## <a name="create-a-list"></a>创建列表

你需要先创建列表来保存客户，然后才能够显示客户。 [List View](../design/controls-and-patterns/listview-and-gridview.md) 是一个非常适合此任务的基本 XAML 控件。 你的 ListView 当前需要在页面上有一个位置，并且很快需要一个 **ItemSource** 属性值。

```xaml
<ListView ItemsSource=""
    HorizontalAlignment="Center"
    VerticalAlignment="Center"/>
```

在将数据绑定到 ListView 后，我们鼓励你返回到文档，并尝试自定义最适合你需求的外观和布局。

## <a name="bind-data-to-your-list"></a>将数据绑定到列表

现在，你已制作了基本 UI 来保存绑定，接下来需要配置提供绑定的源。 下面是具体操作的一个示例：

```csharp
public sealed partial class MainPage : Page
{
    public ObservableCollection<Customer> Customers { get; }
        = new ObservableCollection<Customer>();

    public MainPage()
    {
        this.InitializeComponent();
          // Add some customers
        this.Customers.Add(new Customer() { Name = "NAME1"});
        this.Customers.Add(new Customer() { Name = "NAME2"});
        this.Customers.Add(new Customer() { Name = "NAME3"});
    }
}
```
```xaml
<ListView ItemsSource="{x:Bind Customers}"
    HorizontalAlignment="Center"
    VerticalAlignment="Center">
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="local:Customer">
            <TextBlock Text="{x:Bind Name}"/>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

[数据绑定概述](../data-binding/data-binding-quickstart.md#binding-to-a-collection-of-items)在其有关项目集合的绑定部分指导你解决类似问题。 这里的示例显示以下关键步骤：

* 在你的 UI 的代码隐藏中，创建一个类型 **ObservableCollection<T>** 的属性来保存客户对象。
* 将 ListView 的 **ItemSource** 绑定到该属性。
* 为 ListView 提供基本 **ItemTemplate**，这将配置各个项目在列表中的显示方式。

如果你希望自定义布局、添加项目选择，或调整刚刚创建的 **DataTemplate**，你可以随时查看[列表视图](../design/controls-and-patterns/listview-and-gridview.md)文档。 不过如果你想要编辑客户怎么办？

## <a name="edit-your-customers-through-the-ui"></a>通过 UI 编辑客户

你已经以列表形式显示了客户，而数据 B=binding 可以让你做得更多。 如果可以直接从 UI 编辑你的数据会怎样？ 若要执行此操作，我们首先来讨论一下三种数据绑定模式：

* *一次性*:此数据绑定，才会激活一次，并不会对更改做出响应。
* *单向*:此数据绑定将使用与数据源所做的任何更改来更新 UI。
* *双向*:此数据绑定将使用与数据源所做的任何更改更新 UI，并还与任何用户界面内所做的更改更新的数据。

如果你已遵循之前的代码段，你所做的绑定将使用 x:Bind 且不指定模式，这使其成为一次性绑定。 如果你想要直接从 UI 编辑客户，则需要将其更改为双向绑定，以便对数据的更改传递回客户对象。 [深入了解数据绑定](../data-binding/data-binding-in-depth.md)提供了详细信息。

如果数据源更改，双向绑定还将更新 UI。 若要实现此功能，你必须在源上实现 [**INotifyPropertyChanged**](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanged(d=robot).aspx)，并确保其属性设置器发起 **PropertyChanged** 事件。 常见做法是让它们调用 **OnPropertyChanged** 这样的帮助程序方法，如下所示：

```csharp
public class Customer : INotifyPropertyChanged
{
    private string _name;
    public string Name
    {
        get => _name;
        set
        {
            if (_name != value)
                {
                    _name = value;
                    this.OnPropertyChanged();
                }
        }
    }
    public event PropertyChangedEventHandler PropertyChanged;

    public void OnPropertyChanged([CallerMemberName] string propertyName = null) =>
        this.PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
}
```
然后使用 **TextBox** 而不是 **TextBlock** 将 ListView 中的文本变为可编辑，并确保将数据绑定上的**模式**设置为 **TwoWay**。

```xaml
<ListView ItemsSource="{x:Bind Customers}"
    HorizontalAlignment="Center"
    VerticalAlignment="Center">
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="local:Customer">
            <TextBox Text="{x:Bind Name, Mode=TwoWay}"/>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

确保实现此目的快速方法是通过 TextBox 控件和 OneWay 绑定添加第二个 ListView。 在编辑第一个列表时，第二个列表中的值将自动更改。

> [!NOTE]
> 在 ListView 内直接编辑是显示正在操作的双向绑定的一个简单方法，但会导致可用性问题。 如果你希望进一步深化自己的应用，请考虑使用[其他 XAML 控件](../design/controls-and-patterns/controls-and-events-intro.md)来编辑数据，并让 ListView 保持仅显示状态。

## <a name="going-further"></a>深入探索

现在，你已使用双向绑定创建了客户列表，请随时参阅我们链接给你的文档并进行试验。 如果你需要基本和高级绑定的分步操作实例，或调查[大纲/细节模式](../design/controls-and-patterns/master-details.md)等控件以制作更可靠的 UI，你还可以查看我们的[数据绑定教程](../data-binding/xaml-basics-data-binding.md)。

## <a name="useful-apis-and-docs"></a>有用的 API 和文档

以下是 API 和其他有用文档的核心简要，可帮助你了解如何处理数据绑定。

### <a name="useful-apis"></a>有用的 API

| API | 描述 |
|------|---------------|
| [数据模板](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DataTemplate) | 描述数据对象的可视结构，以允许在 UI 中显示特定元素。 |
| [x:Bind](../xaml-platform/x-bind-markup-extension.md) | 有关推荐的 x:Bind 标记扩展的文档。 |
| [Binding](../xaml-platform/binding-markup-extension.md) | 有关以前的 Binding 标记扩展的文档。 |
| [ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) | 显示垂直堆栈中的数据项的 UI 控件。 |
| [TextBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) | 在 UI 中显示可编辑文本数据的基本文本控件。 |
| [INotifyPropertyChanged](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanged(d=robot).aspx) | 让数据可供观察、将数据提供给数据绑定的接口。 |
| [ItemsControl](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsControl) | 此类的 **ItemsSource** 属性允许 ListView 绑定到数据源。 |

### <a name="useful-docs"></a>有用的文档

| 主题 | 描述 |
|-------|----------------|
| [深度中的数据绑定](../data-binding/data-binding-in-depth.md) | 数据绑定原则的基本概述 |
| [数据绑定概述](../data-binding/data-binding-quickstart.md) | 有关数据绑定的详细概念信息。 |
| [列表视图](../design/controls-and-patterns/listview-and-gridview.md) | 有关创建和配置 ListView 的信息（包括 **DataTemplate** 的实现） |

## <a name="useful-code-samples"></a>有用的代码示例

| 代码示例 | 描述 |
|-----------------|---------------|
| [数据绑定教程](../data-binding/xaml-basics-data-binding.md) | 数据绑定基础知识的分步指南体验。 |
| [ListView 和 GridView](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlListView) | 通过数据绑定探索更复杂的 ListView。 |
| [QuizGame](https://github.com/Microsoft/Windows-appsample-networkhelper) | 查看正在操作的数据绑定，包括 **INotifyPropertyChanged** 的标准实现的 **BindableBase** 类（位于“Common”文件夹中）。 |
