---
description: 可以通过将 ItemsSource 绑定到分层数据源来创建可展开的树视图，也可以自行创建并管理 TreeViewNode 对象。
title: 树视图
label: Tree view
template: detail.hbs
ms.date: 07/24/2019
ms.topic: article
ms.localizationpriority: medium
pm-contact: predavid
design-contact: ksulliv
dev-contact: joyate
doc-status: Published
dev_langs:
- csharp
- vb
ms.custom: RS5, 19H1
ms.openlocfilehash: 41674ecb468023ac6e97cc01d1867478e8a3d70d
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970422"
---
# <a name="treeview"></a>TreeView

XAML [TreeView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.treeview) 控件支持分层列表，其中具有包含嵌套项的展开节点和折叠节点。 它可用于说明你的用户界面中的文件夹结构或嵌套关系。

**TreeView** API 支持以下功能：

- N 级嵌套
- 选择单个或多个节点
- 将数据绑定到 **TreeView** 和 [TreeViewItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.treeviewitem) 上的 **ItemsSource** 属性
- **TreeViewItem** 充当 **TreeView** 项模板的根
- **TreeViewItem** 中任意类型的内容
- 在树视图之间拖放

**获取 Windows UI 库**

|  |  |
| - | - |
| ![WinUI 徽标](images/winui-logo-64x64.png) | TreeView  控件作为 Windows UI 库的一部分提供，该库是一个 Nuget 包，其中包含用于 Windows 应用的新控件和 UI 功能。 有关详细信息（包括安装说明），请参阅 [Windows UI 库](https://docs.microsoft.com/uwp/toolkits/winui/)。 |

> **Windows UI 库 API：** [TreeView 类](/uwp/api/microsoft.ui.xaml.controls.treeview)、[TreeViewNode 类](/uwp/api/microsoft.ui.xaml.controls.treeviewnode)、[TreeView.ItemsSource 属性](/uwp/api/microsoft.ui.xaml.controls.treeview.itemssource)
>
> **平台 API：** [TreeView 类](/uwp/api/windows.ui.xaml.controls.treeview)、[TreeViewNode 类](/uwp/api/windows.ui.xaml.controls.treeviewnode)、[TreeView.ItemsSource 属性](/uwp/api/windows.ui.xaml.controls.treeview.itemssource)

> [!TIP]
> 在本文档中，我们使用 XAML 中的 **muxc** 别名表示我们已包含在项目中的 Windows UI 库 API。 我们已将此项添加到我们的[页](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page)元素：`xmlns:muxc="using:Microsoft.UI.Xaml.Controls"`
>
>在后面的代码中，我们还使用 C# 中的 **muxc** 别名表示我们已包含在项目中的 Windows UI 库 API。 我们在文件顶部添加了此 **using** 语句：`using muxc = Microsoft.UI.Xaml.Controls;`

## <a name="is-this-the-right-control"></a>这是正确的控件吗？

- 当项目已嵌套列表项，并且演示项目与其对等项和节点的层次结构关系很重要时，使用 **TreeView**。

- 如果强调某个项目嵌套关系不是优先事项，则避免使用 **TreeView**。 对于大多数深化方案，适合使用常规列表视图。

## <a name="examples"></a>示例

<table>
<th align="left">XAML 控件库<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果已安装 <strong style="font-weight: semi-bold">XAML 控件库</strong>应用，请单击此处<a href="xamlcontrolsgallery:/item/TreeView">打开此应用，了解 TreeView 的实际应用</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="treeview-ui"></a>TreeView UI

树视图可以使用缩进和图标的组合来表示父节点和子节点之间的嵌套关系。 折叠的节点使用指向右边的 V 形图标，而展开的节点使用指向下方的 V 形图标。

![TreeView 中的 V 形图标](images/treeview-simple.png)

可以在树视图项目数据模板中使用图标来表示节点。 例如，如果显示文件系统层次结构，则可对父节点使用文件夹图标，对叶节点使用文件图标。

![同时使用 V 形图标和文件夹图标的 TreeView](images/treeview-icons.png)

## <a name="create-a-tree-view"></a>创建树视图

可以通过将 [ItemsSource](/uwp/api/windows.ui.xaml.controls.treeview.itemssource) 绑定到分层数据源来创建树视图，也可以自行创建并管理 **TreeViewNode** 对象。

要创建树视图，可以使用 [TreeView](/uwp/api/windows.ui.xaml.controls.treeview) 控件和 [TreeViewNode](/uwp/api/windows.ui.xaml.controls.treeviewnode) 对象层次结构。 可以通过向 **TreeView** 控件的 [RootNodes](/uwp/api/windows.ui.xaml.controls.treeview.rootnodes) 集合中添加一个或多个根节点来创建节点层次结构。 然后可以向每个 **TreeViewNode** 的 [Children](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.treeviewnode.children) 集合中添加多个节点。 可以通过嵌套树视图节点来创建任意数量的层次。

可以将分层数据源绑定到 [ItemsSource](/uwp/api/windows.ui.xaml.controls.treeview.itemssource) 属性以提供树视图内容，就像使用 [ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview) 的 **ItemsSource** 时所做的那样。 同样，可以使用 [ItemTemplate](/uwp/api/windows.ui.xaml.controls.treeview.itemtemplate)（以及可选的 [ItemTemplateSelector](/uwp/api/windows.ui.xaml.controls.treeview.itemtemplate)）来提供用于呈现项的 [DataTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.datatemplate)。

> [!IMPORTANT]
> **ItemsSource** 及其相关的 API 需要 Windows 10 版本 1809（[SDK 17763 或更高版本](https://developer.microsoft.com/windows/downloads/windows-10-sdk)）或 [Windows UI 库](https://docs.microsoft.com/uwp/toolkits/winui/)。
>
> **ItemsSource** 可以替代 **TreeView.RootNodes** 将内容置于 **TreeView** 控件中。 不能同时设置 **ItemsSource** 和 **RootNodes**。 使用 **ItemsSource** 时，系统会为你创建节点，你可以从 **TreeView.RootNodes** 属性访问它们。

下面是一个使用 XAML 声明的简单树视图示例。 通常以代码方式添加节点，但这里我们显示的是 XAML 层次结构，因为这有助于更直观地显示节点层次结构的创建方式。

```xaml
<muxc:TreeView>
    <muxc:TreeView.RootNodes>
        <muxc:TreeViewNode Content="Flavors"
                           IsExpanded="True">
            <muxc:TreeViewNode.Children>
                <muxc:TreeViewNode Content="Vanilla"/>
                <muxc:TreeViewNode Content="Strawberry"/>
                <muxc:TreeViewNode Content="Chocolate"/>
            </muxc:TreeViewNode.Children>
        </muxc:TreeViewNode>
    </muxc:TreeView.RootNodes>
</muxc:TreeView>
```

在大多数情况下，树视图显示来自一个数据源的数据，因此，通常以 XAML 方式声明根 **TreeView** 控件，但以代码或数据绑定方式添加 **TreeViewNode** 对象。

### <a name="bind-to-a-hierarchical-data-source"></a>绑定到分层数据源

若要使用数据绑定来创建树视图，请将分层集合设置为 **TreeView.ItemsSource** 属性。 然后在 **ItemTemplate** 中将子项集合设置为 **TreeViewItem.ItemsSource** 属性。

```xaml
<muxc:TreeView ItemsSource="{x:Bind DataSource}">
    <muxc:TreeView.ItemTemplate>
        <DataTemplate x:DataType="local:Item">
            <muxc:TreeViewItem ItemsSource="{x:Bind Children}"
                               Content="{x:Bind Name}"/>
        </DataTemplate>
    </muxc:TreeView.ItemTemplate>
</muxc:TreeView>
```

请参阅[使用数据绑定的树视图](#tree-view-using-data-binding)以了解完整代码。

#### <a name="items-and-item-containers"></a>项和项容器

如果使用 **TreeView.ItemsSource**，则可使用这些 API 从容器获取节点或数据项，反之亦然。

| **[TreeViewItem](/uwp/api/windows.ui.xaml.controls.treeviewitem)** | |
| - | - |
| [TreeView.ItemFromContainer](/uwp/api/windows.ui.xaml.controls.treeview.itemfromcontainer) | 获取指定的 **TreeViewItem** 容器的数据项。 |
| [TreeView.ContainerFromItem](/uwp/api/windows.ui.xaml.controls.treeview.containerfromitem) | 获取指定数据项的 **TreeViewItem** 容器。 |

| **[TreeViewNode](/uwp/api/windows.ui.xaml.controls.treeviewnode)** | |
| - | - |
| [TreeView.NodeFromContainer](/uwp/api/windows.ui.xaml.controls.treeview.nodefromcontainer) | 获取指定的 **TreeViewItem** 容器的 **TreeViewNode**。 |
| [TreeView.ContainerFromNode](/uwp/api/windows.ui.xaml.controls.treeview.containerfromnode) | 获取指定的 **TreeViewNode** 的 **TreeViewItem** 容器。 |

### <a name="manage-tree-view-nodes"></a>管理树视图节点

此树视图与之前以 XAML 创建的树视图相同，但节点是以代码方式创建的。

```xaml
<muxc:TreeView x:Name="sampleTreeView"/>
```

```csharp
private void InitializeTreeView()
{
    muxc.TreeViewNode rootNode = new muxc.TreeViewNode() { Content = "Flavors" };
    rootNode.IsExpanded = true;
    rootNode.Children.Add(new muxc.TreeViewNode() { Content = "Vanilla" });
    rootNode.Children.Add(new muxc.TreeViewNode() { Content = "Strawberry" });
    rootNode.Children.Add(new muxc.TreeViewNode() { Content = "Chocolate" });

    sampleTreeView.RootNodes.Add(rootNode);
}
```

```vb
Private Sub InitializeTreeView()
    Dim rootNode As New muxc.TreeViewNode With {.Content = "Flavors", .IsExpanded = True}
    With rootNode.Children
        .Add(New muxc.TreeViewNode With {.Content = "Vanilla"})
        .Add(New muxc.TreeViewNode With {.Content = "Strawberry"})
        .Add(New muxc.TreeViewNode With {.Content = "Chocolate"})
    End With
    sampleTreeView.RootNodes.Add(rootNode)
End Sub
```

可以使用以下 API 管理树视图的数据层次结构。

| **[TreeView](/uwp/api/windows.ui.xaml.controls.treeview)** | |
| - | - |
| [RootNodes](/uwp/api/windows.ui.xaml.controls.treeview.rootnodes) | 一个树视图可以有一个或多个根节点。 向 **RootNodes** 集合添加一个 **TreeViewNode** 对象会创建一个根节点。 根节点的 **Parent** 始终为 **null**。 根节点的 **Depth** 为 0。 |

| **[TreeViewNode](/uwp/api/windows.ui.xaml.controls.treeviewnode)** | |
| - | - |
| [Children](/uwp/api/windows.ui.xaml.controls.treeviewnode.children) | 向父节点的 **Children** 集合添加 **TreeViewNode** 对象可创建节点层次结构。 节点是其 **Children** 集合中的所有节点的 **Parent**。 |
| [HasChildren](/uwp/api/windows.ui.xaml.controls.treeviewnode.haschildren) | 如果节点有已实现的子级，则为 **true**。 **false** 表示空的文件夹或项目。 |
| [HasUnrealizedChildren](/uwp/api/windows.ui.xaml.controls.treeviewnode.hasunrealizedchildren) | 填充展开的节点时可以使用此属性。 请参阅本文稍后部分的[填充正在展开的节点](#fill-a-node-when-its-expanding)。 |
| [Depth](/uwp/api/windows.ui.xaml.controls.treeviewnode.depth) | 指示子节点距根节点的距离。 |
| [Parent](/uwp/api/windows.ui.xaml.controls.treeviewnode.parent) | 获取拥有此节点所属的 **Children** 集合的 **TreeViewNode**。 |

树视图使用 **HasChildren** 和 **HasUnrealizedChildren** 属性确定是否显示展开/折叠图标。 如果任一属性为 **true**，则显示图标；否则不显示。

## <a name="tree-view-node-content"></a>树视图节点内容

可以将树视图所表示的数据项存储在树视图的 [Content](/uwp/api/windows.ui.xaml.controls.treeviewnode.content) 属性中。

在前面的示例中，内容为简单的字符串值。 在这里，有一个树视图节点表示用户的 **Pictures** 文件夹，因此将图片库 [StorageFolder](/uwp/api/windows.storage.storagefolder) 分配给该节点的 **Content** 属性。

```csharp
StorageFolder picturesFolder = KnownFolders.PicturesLibrary;
muxc.TreeViewNode pictureNode = new muxc.TreeViewNode();
pictureNode.Content = picturesFolder;
```

```vb
Dim picturesFolder As StorageFolder = KnownFolders.PicturesLibrary
Dim pictureNode As New muxc.TreeViewNode With {.Content = picturesFolder}
```

> [!NOTE]
> 为了能够访问 **Pictures** 文件夹，需要在应用清单中指定“图片库”  功能。 有关详细信息，请参阅[应用功能声明](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations)。

可以提供 [DataTemplate](/uwp/api/windows.ui.xaml.datatemplate) 来指定数据项在树视图中的显示方式。

> [!NOTE]
> 在 Windows 10 版本 1803 中，必须重新设置 **TreeView** 控件模板；如果内容不是字符串，还必须指定自定义 **ItemTemplate**。 在后续版本中，设置 **ItemTemplate** 属性。 有关详细信息，请参阅 [TreeView.ItemTemplate](/uwp/api/windows.ui.xaml.controls.treeview.itemtemplate)。

### <a name="item-container-style"></a>项容器样式

不管使用 **ItemsSource** 还是 **RootNodes**，用于显示每个节点（称为“容器”）的实际元素都是 [TreeViewItem](/uwp/api/windows.ui.xaml.controls.treeviewitem) 对象。 可以修改 **TreeViewItem** 属性，使用 **TreeView** 的 [ItemContainerStyle](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.treeview.itemcontainerstyle) 或 [ItemContainerStyleSelector](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.treeview.itemcontainerstyleselector) 属性设置容器的样式。

以下示例显示了如何将展开的/折叠的字形更改为橙色的 +/- 符号。 在默认的 **TreeViewItem** 模板中，已将字形设置为使用 `Segoe MDL2 Assets` 字体。 设置 **Setter.Value** 属性时，可以在 XAML 所使用的格式中提供 Unicode 字符值，类似于 `Value="&#xE948;"`。

```xaml
<muxc:TreeView>
    <muxc:TreeView.ItemContainerStyle>
        <Style TargetType="muxc:TreeViewItem">
            <Setter Property="CollapsedGlyph" Value="&#xE948;"/>
            <Setter Property="ExpandedGlyph" Value="&#xE949;"/>
            <Setter Property="GlyphBrush" Value="DarkOrange"/>
        </Style>
    </muxc:TreeView.ItemContainerStyle>
    <muxc:TreeView.RootNodes>
        <muxc:TreeViewNode Content="Flavors"
               IsExpanded="True">
            <muxc:TreeViewNode.Children>
                <muxc:TreeViewNode Content="Vanilla"/>
                <muxc:TreeViewNode Content="Strawberry"/>
                <muxc:TreeViewNode Content="Chocolate"/>
            </muxc:TreeViewNode.Children>
        </muxc:TreeViewNode>
    </muxc:TreeView.RootNodes>
</muxc:TreeView>
```

### <a name="item-template-selectors"></a>项模板选择器

默认情况下，[TreeView](/uwp/api/windows.ui.xaml.controls.treeview) 显示每个节点的数据项的字符串表现形式。 可以通过设置 [ItemTemplate](/uwp/api/windows.ui.xaml.controls.treeview.itemtemplate) 属性更改所有节点的显示内容。 也可使用 [ItemTemplateSelector](/uwp/api/windows.ui.xaml.controls.treeview.itemtemplateselector)，根据项的类型或你指定的某个其他条件为树视图项选择不同的 [DataTemplate](/uwp/api/windows.ui.xaml.datatemplate)。

例如，在文件资源管理器应用中，可以将一个数据模板用于文件夹，将另一个用于文件。

![使用不同数据模板的文件夹和文件](images/treeview-icons.png)

下面举例说明了如何创建并使用项模板选择器。  有关详细信息，请查看 [DataTemplateSelector](/uwp/api/windows.ui.xaml.controls.datatemplateselector) 类。

> [!NOTE]
> 此代码属于一个更大的示例，不能独立运行。 若要查看完整示例（包括用于定义 `ExplorerItem` 的代码），请参阅 GitHub 上的 [Xaml-Controls-Gallery 存储库](https://github.com/microsoft/Xaml-Controls-Gallery)。 [TreeViewPage.xaml](https://github.com/microsoft/Xaml-Controls-Gallery/blob/1ecd85c908a8a1cb9a8201e548f58db379801e69/XamlControlsGallery/ControlPages/TreeViewPage.xaml) 和 [TreeViewPage.xaml.cs](https://github.com/Microsoft/Xaml-Controls-Gallery/blob/1ecd85c908a8a1cb9a8201e548f58db379801e69/XamlControlsGallery/ControlPages/TreeViewPage.xaml.cs) 包含相关的代码。

```xaml
<Page.Resources>
    <DataTemplate x:Key="FolderTemplate" x:DataType="local:ExplorerItem">
        <muxc:TreeViewItem ItemsSource="{x:Bind Children}">
            <StackPanel Orientation="Horizontal">
                <Image Width="20" Source="Assets/folder.png"/>
                <TextBlock Text="{x:Bind Name}" />
            </StackPanel>
        </muxc:TreeViewItem>
    </DataTemplate>

    <DataTemplate x:Key="FileTemplate" x:DataType="local:ExplorerItem">
        <muxc:TreeViewItem>
            <StackPanel Orientation="Horizontal">
                <Image Width="20" Source="Assets/file.png"/>
                <TextBlock Text="{Binding Name}"/>
            </StackPanel>
        </muxc:TreeViewItem>
    </DataTemplate>

    <local:ExplorerItemTemplateSelector
            x:Key="ExplorerItemTemplateSelector"
            FolderTemplate="{StaticResource FolderTemplate}"
            FileTemplate="{StaticResource FileTemplate}" />
</Page.Resources>

<Grid>
    <muxc:TreeView
        ItemsSource="{x:Bind DataSource}"
        ItemTemplateSelector="{StaticResource ExplorerItemTemplateSelector}"/>
</Grid>
```

```csharp
public class ExplorerItemTemplateSelector : DataTemplateSelector
{
    public DataTemplate FolderTemplate { get; set; }
    public DataTemplate FileTemplate { get; set; }

    protected override DataTemplate SelectTemplateCore(object item)
    {
        var explorerItem = (ExplorerItem)item;
        if (explorerItem.Type == ExplorerItem.ExplorerItemType.Folder) return FolderTemplate;

        return FileTemplate;
    }
}
```

传递给 [SelectTemplateCore](/uwp/api/windows.ui.xaml.controls.datatemplateselector.selecttemplatecore) 方法的对象的类型取决于你是通过设置 **ItemsSource** 属性来创建树视图，还是通过自行创建并管理 **TreeViewNode** 对象来这样做。

- 如果 **ItemsSource** 已设置，则对象类型取决于数据项类型。 在上一示例中，对象是 `ExplorerItem`，因此若要使用它，只需将它强制转换为 `ExplorerItem`: `var explorerItem = (ExplorerItem)item;` 即可。
- 如果 **ItemsSource** 未设置，而你是自行管理树视图节点的，则传递给 **SelectTemplateCore** 的对象是 **TreeViewNode**。 在这种情况下，可以从 [TreeViewNode.Content](/uwp/api/windows.ui.xaml.controls.treeviewnode.content) 属性获取数据项。

下面是一个数据模板选择器，来自后面显示的[图片和音乐库树视图](#pictures-and-music-library-tree-view)示例。 **SelectTemplateCore** 方法接收 **TreeViewNode**，后者可能将 [StorageFolder](/uwp/api/windows.storage.storagefolder) 或 [StorageFile](/uwp/api/windows.storage.storagefile) 用作其内容。 可以根据内容返回默认模板或针对音乐文件夹、图片文件夹、音乐文件或图片文件的模板。

```csharp
protected override DataTemplate SelectTemplateCore(object item)
{
    var node = (TreeViewNode)item;
    if (node.Content is StorageFolder)
    {
        var content = node.Content as StorageFolder;
        if (content.DisplayName.StartsWith("Pictures")) return PictureFolderTemplate;
        if (content.DisplayName.StartsWith("Music")) return MusicFolderTemplate;
    }
    else if (node.Content is StorageFile)
    {
        var content = node.Content as StorageFile;
        if (content.ContentType.StartsWith("image")) return PictureItemTemplate;
        if (content.ContentType.StartsWith("audio")) return MusicItemTemplate;
    }
    return DefaultTemplate;
}
```

```vb
Protected Overrides Function SelectTemplateCore(ByVal item As Object) As DataTemplate
    Dim node = CType(item, muxc.TreeViewNode)

    If TypeOf node.Content Is StorageFolder Then
        Dim content = TryCast(node.Content, StorageFolder)
        If content.DisplayName.StartsWith("Pictures") Then Return PictureFolderTemplate
        If content.DisplayName.StartsWith("Music") Then Return MusicFolderTemplate
    ElseIf TypeOf node.Content Is StorageFile Then
        Dim content = TryCast(node.Content, StorageFile)
        If content.ContentType.StartsWith("image") Then Return PictureItemTemplate
        If content.ContentType.StartsWith("audio") Then Return MusicItemTemplate
    End If

    Return DefaultTemplate
End Function
```

## <a name="interacting-with-a-tree-view"></a>与树视图交互

可以配置树视图以允许用户通过几种不同的方式与其交互：

- 展开或折叠节点
- 单选或多选项目
- 单击以调用项目

### <a name="expandcollapse"></a>展开/折叠

任何有子级的树视图节点都始终可以通过单击展开/折叠字形来展开或折叠节点。 也可以通过编程展开或折叠节点，并在节点状态发生更改时进行响应。

#### <a name="expandcollapse-a-node-programmatically"></a>以编程方式展开/折叠节点

在代码中，有两种方式可以展开或折叠树视图节点。

- [TreeView](/uwp/api/windows.ui.xaml.controls.treeview) 类具有 [Collapse](/uwp/api/windows.ui.xaml.controls.treeview.collapse) 和 [Expand](/uwp/api/windows.ui.xaml.controls.treeview.expand) 方法。 调用这些方法时，需要传入要展开或折叠的 **TreeViewNode**。

- 每个 [TreeViewNode](/uwp/api/windows.ui.xaml.controls.treeviewnode) 都有 [IsExpanded](/uwp/api/windows.ui.xaml.controls.treeviewnode.isexpanded) 属性。 可以使用此属性检查节点的状态或对其进行设置以更改状态。 也可以在 XAML 中设置此属性以设置节点的初始状态。

### <a name="fill-a-node-when-its-expanding"></a>填充正在展开的节点

可能需要在树视图中显示大量节点，否则无法提前知道树视图会有多少节点。 **TreeView** 控件未虚拟化，因此可以通过填充每个展开的节点和删除折叠的子节点来管理资源。

处理 [Expanding](/uwp/api/windows.ui.xaml.controls.treeview.expand) 事件并使用 [HasUnrealizedChildren](/uwp/api/windows.ui.xaml.controls.treeviewnode.hasunrealizedchildren) 属性可在节点展开时向节点添加子级。 **HasUnrealizedChildren** 属性指示是否需要填充节点或其 **Children** 集合是否已填充。 请务必记住，**TreeViewNode** 并不设置此值，你需要在应用节点中对其进行管理。

下面是这些 API 的使用示例。 请参阅本文末尾的完整示例代码了解上下文，包括 **FillTreeNode** 的实现。

```csharp
private void SampleTreeView_Expanding(muxc.TreeView sender, muxc.TreeViewExpandingEventArgs args)
{
    if (args.Node.HasUnrealizedChildren)
    {
        FillTreeNode(args.Node);
    }
}
```

```vb
Private Sub SampleTreeView_Expanding(sender As muxc.TreeView, args As muxc.TreeViewExpandingEventArgs)
    If args.Node.HasUnrealizedChildren Then
        FillTreeNode(args.Node)
    End If
End Sub
```

这不是必需的，但你可能还想处理 [Collapsed](/uwp/api/windows.ui.xaml.controls.treeview.collapsed) 事件并在父节点关闭时删除子节点。 当树视图有很多节点或节点数据使用大量资源时，这一点可能很重要。 应考虑每次打开节点时进行填充与保留已关闭节点的子级的性能影响。 最佳选择取决于你的应用。

下面是一个 **Collapsed** 事件处理程序示例。

```csharp
private void SampleTreeView_Collapsed(muxc.TreeView sender, muxc.TreeViewCollapsedEventArgs args)
{
    args.Node.Children.Clear();
    args.Node.HasUnrealizedChildren = true;
}
```

```vb
Private Sub SampleTreeView_Collapsed(sender As muxc.TreeView, args As muxc.TreeViewCollapsedEventArgs)
    args.Node.Children.Clear()
    args.Node.HasUnrealizedChildren = True
End Sub
```

### <a name="invoking-an-item"></a>调用项

用户可以调用操作（处理按钮等项）而不是选择项。 可以处理 [ItemInvoked](/uwp/api/windows.ui.xaml.controls.treeview.iteminvoked) 事件以响应此用户交互。

> [!NOTE]
> 与具有 [IsItemClickEnabled](/uwp/api/windows.ui.xaml.controls.listviewbase.isitemclickenabled) 属性的 **ListView** 不同，调用项功能在树视图中始终处于启用状态。 你仍然可以选择是否处理事件。

**[TreeViewItemInvokedEventArgs](/uwp/api/windows.ui.xaml.controls.treeviewiteminvokedeventargs) 类**

通过 **ItemInvoked** 事件参数可以访问已调用项。 [InvokedItem](/uwp/api/windows.ui.xaml.controls.treeviewiteminvokedeventargs.invokeditem) 属性具有已调用节点。 可以将其强制转换为 **TreeViewNode** 并从 **TreeViewNode.Content** 属性获取数据项。

下面是一个 **ItemInvoked** 事件处理程序示例。 数据项是一个 [IStorageItem](/uwp/api/windows.storage.istorageitem)，此示例仅显示关于文件和树的部分信息。 此外，如果节点为文件夹节点，节点会同时展开或折叠。 否则，仅当单击 V 形图标时才会展开或折叠节点。

```csharp
private void SampleTreeView_ItemInvoked(muxc.TreeView sender, muxc.TreeViewItemInvokedEventArgs args)
{
    var node = args.InvokedItem as muxc.TreeViewNode;
    if (node.Content is IStorageItem item)
    {
        FileNameTextBlock.Text = item.Name;
        FilePathTextBlock.Text = item.Path;
        TreeDepthTextBlock.Text = node.Depth.ToString();

        if (node.Content is StorageFolder)
        {
            node.IsExpanded = !node.IsExpanded;
        }
    }
}
```

```vb
Private Sub SampleTreeView_ItemInvoked(sender As muxc.TreeView, args As muxc.TreeViewItemInvokedEventArgs)
    Dim node = TryCast(args.InvokedItem, muxc.TreeViewNode)
    Dim item = TryCast(node.Content, IStorageItem)
    If item IsNot Nothing Then
        FileNameTextBlock.Text = item.Name
        FilePathTextBlock.Text = item.Path
        TreeDepthTextBlock.Text = node.Depth.ToString()
        If TypeOf node.Content Is StorageFolder Then
            node.IsExpanded = Not node.IsExpanded
        End If
    End If
End Sub
```

### <a name="item-selection"></a>项选择

**TreeView** 控件支持单选和多选。 默认情况下，节点选择处于关闭状态，但你可以设置 [TreeView.SelectionMode](/uwp/api/windows.ui.xaml.controls.treeview.selectionmode) 属性以允许选择节点。 [TreeViewSelectionMode](/uwp/api/windows.ui.xaml.controls.treeviewselectionmode) 值为 **None**、**Single** 和 **Multiple**。

#### <a name="multiple-selection"></a>多选

启用多选后，每个树节点旁会显示一个复选框，选中的项突出显示。 用户可以使用复选框选择或取消选择项；单击项仍将导致项被调用。

选择或取消选择父节点时，会选择或取消选择该节点下的所有子节点。 如果父节点下的部分（但并非所有）子节点处于选中状态，则父节点的复选框会以非确定的状态显示。

![树视图中的多选](images/treeview-selection.png)

选中的节点会添加到树视图的 [SelectedNodes](/uwp/api/windows.ui.xaml.controls.treeview.selectednodes) 集合中。 可以调用 [SelectAll](/uwp/api/windows.ui.xaml.controls.treeview.selectall) 方法来选择树视图中的所有节点。

> [!NOTE]
> 调用 **SelectAll** 会选择所有已实现的节点，而无论 **SelectionMode** 是什么。 为提供一致的用户体验，当 **SelectionMode** 为 **Multiple** 时应调用 **SelectAll**。

#### <a name="selection-and-realizedunrealized-nodes"></a>选择与已实现/未实现节点

如果树视图有未实现的节点，这些节点将不计入选择范围。 关于未实现节点的选择，需要记住以下事项。

- 选择一个父节点也会选择该父级下的所有已实现子级。 同样，如果选择了所有子节点，则父节点也会被选中。
- **SelectAll** 方法只会将已实现的节点添加到 **SelectedNodes** 集合中。
- 如果选择了具有未实现子级的父节点，该子级将在实现时被选中。

## <a name="code-examples"></a>代码示例

下面的代码示例演示树视图控件的各种功能。

### <a name="tree-view-using-xaml"></a>使用 XAML 的树视图

本示例介绍如何在 XAML 中创建简单的树视图结构。 此树视图显示用户可以选择的冰淇淋口味和配料，配料按类别排列。 多选已启用，当用户单击按钮时，所选项会显示在主应用 UI 中。

```xaml
<Page
    x:Class="TreeViewTest.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    xmlns:muxc="using:Microsoft.UI.Xaml.Controls"
    mc:Ignorable="d"
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}"
          Padding="100">
        <SplitView IsPaneOpen="True"
               DisplayMode="Inline"
               OpenPaneLength="296">
            <SplitView.Pane>
                <muxc:TreeView x:Name="DessertTree" SelectionMode="Multiple">
                    <muxc:TreeView.RootNodes>
                        <muxc:TreeViewNode Content="Flavors" IsExpanded="True">
                            <muxc:TreeViewNode.Children>
                                <muxc:TreeViewNode Content="Vanilla"/>
                                <muxc:TreeViewNode Content="Strawberry"/>
                                <muxc:TreeViewNode Content="Chocolate"/>
                            </muxc:TreeViewNode.Children>
                        </muxc:TreeViewNode>

                        <muxc:TreeViewNode Content="Toppings">
                            <muxc:TreeViewNode.Children>
                                <muxc:TreeViewNode Content="Candy">
                                    <muxc:TreeViewNode.Children>
                                        <muxc:TreeViewNode Content="Chocolate"/>
                                        <muxc:TreeViewNode Content="Mint"/>
                                        <muxc:TreeViewNode Content="Sprinkles"/>
                                    </muxc:TreeViewNode.Children>
                                </muxc:TreeViewNode>
                                <muxc:TreeViewNode Content="Fruits">
                                    <muxc:TreeViewNode.Children>
                                        <muxc:TreeViewNode Content="Mango"/>
                                        <muxc:TreeViewNode Content="Peach"/>
                                        <muxc:TreeViewNode Content="Kiwi"/>
                                    </muxc:TreeViewNode.Children>
                                </muxc:TreeViewNode>
                                <muxc:TreeViewNode Content="Berries">
                                    <muxc:TreeViewNode.Children>
                                        <muxc:TreeViewNode Content="Strawberry"/>
                                        <muxc:TreeViewNode Content="Blueberry"/>
                                        <muxc:TreeViewNode Content="Blackberry"/>
                                    </muxc:TreeViewNode.Children>
                                </muxc:TreeViewNode>
                            </muxc:TreeViewNode.Children>
                        </muxc:TreeViewNode>
                    </muxc:TreeView.RootNodes>
                </muxc:TreeView>
            </SplitView.Pane>

            <StackPanel Grid.Column="1" Margin="12,0">
                <Button Content="Select all" Click="SelectAllButton_Click"/>
                <Button Content="Create order" Click="OrderButton_Click" Margin="0,12"/>
                <TextBlock Text="Your flavor selections:" Style="{StaticResource CaptionTextBlockStyle}"/>
                <TextBlock x:Name="FlavorList" Margin="0,0,0,12"/>
                <TextBlock Text="Your topping selections:" Style="{StaticResource CaptionTextBlockStyle}"/>
                <TextBlock x:Name="ToppingList"/>
            </StackPanel>
        </SplitView>
    </Grid>
</Page>
```

```csharp
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using muxc = Microsoft.UI.Xaml.Controls;

namespace TreeViewTest
{
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
        }

        private void OrderButton_Click(object sender, RoutedEventArgs e)
        {
            FlavorList.Text = string.Empty;
            ToppingList.Text = string.Empty;

            foreach (muxc.TreeViewNode node in DessertTree.SelectedNodes)
            {
                if (node.Parent.Content?.ToString() == "Flavors")
                {
                    FlavorList.Text += node.Content + "; ";
                }
                else if (node.HasChildren == false)
                {
                    ToppingList.Text += node.Content + "; ";
                }
            }
        }

        private void SelectAllButton_Click(object sender, RoutedEventArgs e)
        {
            if (DessertTree.SelectionMode == muxc.TreeViewSelectionMode.Multiple)
            {
                DessertTree.SelectAll();
            }
        }
    }
}
```

```vb
Private Sub OrderButton_Click(sender As Object, e As RoutedEventArgs)
    FlavorList.Text = String.Empty
    ToppingList.Text = String.Empty
    For Each node As muxc.TreeViewNode In DessertTree.SelectedNodes
        If node.Parent.Content?.ToString() = "Flavors" Then
            FlavorList.Text += node.Content & "; "
        ElseIf node.HasChildren = False Then
            ToppingList.Text += node.Content & "; "
        End If
    Next
End Sub

Private Sub SelectAllButton_Click(sender As Object, e As RoutedEventArgs)
    If DessertTree.SelectionMode = muxc.TreeViewSelectionMode.Multiple Then
        DessertTree.SelectAll()
    End If
End Sub
```

### <a name="tree-view-using-data-binding"></a>使用数据绑定的树视图

本示例介绍如何创建与上一示例相同的树视图。 不过，是在代码中创建数据并将其绑定到树视图的 **ItemsSource** 属性，而不是使用 XAML 创建数据层次结构。 （在上一示例中显示的按钮事件处理程序也适用于此示例。）

```xaml
<Page
    x:Class="TreeViewTest.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    xmlns:muxc="using:Microsoft.UI.Xaml.Controls"
    xmlns:local="using:TreeViewTest"
    mc:Ignorable="d"
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}"
          Padding="100">
        <SplitView IsPaneOpen="True"
                   DisplayMode="Inline"
                   OpenPaneLength="296">
            <SplitView.Pane>
                <muxc:TreeView Name="DessertTree"
                                      SelectionMode="Multiple"
                                      ItemsSource="{x:Bind DataSource}">
                    <muxc:TreeView.ItemTemplate>
                        <DataTemplate x:DataType="local:Item">
                            <muxc:TreeViewItem
                                ItemsSource="{x:Bind Children}"
                                Content="{x:Bind Name}"/>
                        </DataTemplate>
                    </muxc:TreeView.ItemTemplate>
                </muxc:TreeView>
            </SplitView.Pane>

            <StackPanel Grid.Column="1" Margin="12,0">
                <Button Content="Select all"
                        Click="SelectAllButton_Click"/>
                <Button Content="Create order"
                        Click="OrderButton_Click"
                        Margin="0,12"/>
                <TextBlock Text="Your flavor selections:"
                           Style="{StaticResource CaptionTextBlockStyle}"/>
                <TextBlock x:Name="FlavorList" Margin="0,0,0,12"/>
                <TextBlock Text="Your topping selections:"
                           Style="{StaticResource CaptionTextBlockStyle}"/>
                <TextBlock x:Name="ToppingList"/>
            </StackPanel>
        </SplitView>
    </Grid>

</Page>
```

```csharp
using System.Collections.ObjectModel;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using muxc = Microsoft.UI.Xaml.Controls;

namespace TreeViewTest
{
    public sealed partial class MainPage : Page
    {
        private ObservableCollection<Item> DataSource = new ObservableCollection<Item>();

        public MainPage()
        {
            this.InitializeComponent();
            DataSource = GetDessertData();
        }

        private ObservableCollection<Item> GetDessertData()
        {
            var list = new ObservableCollection<Item>();

            Item flavorsCategory = new Item()
            {
                Name = "Flavors",
                Children =
                {
                    new Item() { Name = "Vanilla" },
                    new Item() { Name = "Strawberry" },
                    new Item() { Name = "Chocolate" }
                }
            };

            Item toppingsCategory = new Item()
            {
                Name = "Toppings",
                Children =
                {
                    new Item()
                    {
                        Name = "Candy",
                        Children =
                        {
                            new Item() { Name = "Chocolate" },
                            new Item() { Name = "Mint" },
                            new Item() { Name = "Sprinkles" }
                        }
                    },
                    new Item()
                    {
                        Name = "Fruits",
                        Children =
                        {
                            new Item() { Name = "Mango" },
                            new Item() { Name = "Peach" },
                            new Item() { Name = "Kiwi" }
                        }
                    },
                    new Item()
                    {
                        Name = "Berries",
                        Children =
                        {
                            new Item() { Name = "Strawberry" },
                            new Item() { Name = "Blueberry" },
                            new Item() { Name = "Blackberry" }
                        }
                    }
                }
            };

            list.Add(flavorsCategory);
            list.Add(toppingsCategory);
            return list;
        }

        private void OrderButton_Click(object sender, RoutedEventArgs e)
        {
            FlavorList.Text = string.Empty;
            ToppingList.Text = string.Empty;

            foreach (muxc.TreeViewNode node in DessertTree.SelectedNodes)
            {
                if (node.Parent.Content?.ToString() == "Flavors")
                {
                    FlavorList.Text += node.Content + "; ";
                }
                else if (node.HasChildren == false)
                {
                    ToppingList.Text += node.Content + "; ";
                }
            }
        }

        private void SelectAllButton_Click(object sender, RoutedEventArgs e)
        {
            if (DessertTree.SelectionMode == muxc.TreeViewSelectionMode.Multiple)
            {
                DessertTree.SelectAll();
            }
        }
    }

    public class Item
    {
        public string Name { get; set; }
        public ObservableCollection<Item> Children { get; set; } = new ObservableCollection<Item>();

        public override string ToString()
        {
            return Name;
        }
    }
}

```

### <a name="pictures-and-music-library-tree-view"></a>图片和音乐库树视图

本示例介绍如何创建树视图来显示用户的“图片”和“音乐”库的内容和结构   。 因为无法预知项目数，因此每个节点都在展开时填充，在折叠时清空。

这里使用一个自定义项模板来显示数据项，这些数据项类型为 [IStorageItem](/uwp/api/windows.storage.istorageitem)。

> [!IMPORTANT]
> 本示例中的代码需要使用 **picturesLibrary** 和 **musicLibrary** 功能。 有关文件访问的更多信息，请参阅[文件访问权限](../../files/file-access-permissions.md)、[枚举和查询文件和文件夹](../../files/quickstart-listing-files-and-folders.md)以及[音乐、图片和视频库中的文件和文件夹](../../files/quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md)。

```xaml
<Page
    x:Class="TreeViewTest.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:TreeViewTest"
    xmlns:muxc="using:Microsoft.UI.Xaml.Controls"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">
    <Page.Resources>
        <DataTemplate x:Key="TreeViewItemDataTemplate">
            <Grid Height="44">
                <TextBlock Text="{Binding Content.DisplayName}"
                           HorizontalAlignment="Left"
                           VerticalAlignment="Center"
                           Style="{ThemeResource BodyTextBlockStyle}"/>
            </Grid>
        </DataTemplate>

        <DataTemplate x:Key="MusicItemDataTemplate">
            <StackPanel Height="44" Orientation="Horizontal">
                <SymbolIcon Symbol="Audio" Margin="0,0,4,0"/>
                <TextBlock Text="{Binding Content.DisplayName}"
                           HorizontalAlignment="Left"
                           VerticalAlignment="Center"
                           Style="{ThemeResource BodyTextBlockStyle}"/>
            </StackPanel>
        </DataTemplate>

        <DataTemplate x:Key="PictureItemDataTemplate">
            <StackPanel Height="44" Orientation="Horizontal">
                <FontIcon FontFamily="Segoe MDL2 Assets" Glyph="&#xEB9F;"
                          Margin="0,0,4,0"/>
                <TextBlock Text="{Binding Content.DisplayName}"
                           HorizontalAlignment="Left"
                           VerticalAlignment="Center"
                           Style="{ThemeResource BodyTextBlockStyle}"/>
            </StackPanel>
        </DataTemplate>

        <DataTemplate x:Key="MusicFolderDataTemplate">
            <StackPanel Height="44" Orientation="Horizontal">
                <SymbolIcon Symbol="MusicInfo" Margin="0,0,4,0"/>
                <TextBlock Text="{Binding Content.DisplayName}"
                           HorizontalAlignment="Left"
                           VerticalAlignment="Center"
                           Style="{ThemeResource BodyTextBlockStyle}"/>
            </StackPanel>
        </DataTemplate>

        <DataTemplate x:Key="PictureFolderDataTemplate">
            <StackPanel Height="44" Orientation="Horizontal">
                <SymbolIcon Symbol="Pictures" Margin="0,0,4,0"/>
                <TextBlock Text="{Binding Content.DisplayName}"
                           HorizontalAlignment="Left"
                           VerticalAlignment="Center"
                           Style="{ThemeResource BodyTextBlockStyle}"/>
            </StackPanel>
        </DataTemplate>

        <local:ExplorerItemTemplateSelector
            x:Key="ExplorerItemTemplateSelector"
            DefaultTemplate="{StaticResource TreeViewItemDataTemplate}"
            MusicItemTemplate="{StaticResource MusicItemDataTemplate}"
            MusicFolderTemplate="{StaticResource MusicFolderDataTemplate}"
            PictureItemTemplate="{StaticResource PictureItemDataTemplate}"
            PictureFolderTemplate="{StaticResource PictureFolderDataTemplate}"/>
    </Page.Resources>

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <SplitView IsPaneOpen="True"
                   DisplayMode="Inline"
                   OpenPaneLength="296">
            <SplitView.Pane>
                <Grid>
                    <Grid.RowDefinitions>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition/>
                    </Grid.RowDefinitions>
                    <Button Content="Refresh tree" Click="RefreshButton_Click" Margin="24,12"/>
                    <muxc:TreeView x:Name="sampleTreeView" Grid.Row="1" SelectionMode="Single"
                              ItemTemplateSelector="{StaticResource ExplorerItemTemplateSelector}"
                              Expanding="SampleTreeView_Expanding"
                              Collapsed="SampleTreeView_Collapsed"
                              ItemInvoked="SampleTreeView_ItemInvoked"/>
                </Grid>
            </SplitView.Pane>

            <StackPanel Grid.Column="1" Margin="12,72">
                <TextBlock Text="File name:" Style="{StaticResource CaptionTextBlockStyle}"/>
                <TextBlock x:Name="FileNameTextBlock" Margin="0,0,0,12"/>

                <TextBlock Text="File path:" Style="{StaticResource CaptionTextBlockStyle}"/>
                <TextBlock x:Name="FilePathTextBlock" Margin="0,0,0,12"/>

                <TextBlock Text="Tree depth:" Style="{StaticResource CaptionTextBlockStyle}"/>
                <TextBlock x:Name="TreeDepthTextBlock" Margin="0,0,0,12"/>
            </StackPanel>
        </SplitView>
    </Grid>
</Page>
```

```csharp
using System;
using System.Collections.Generic;
using Windows.Storage;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using muxc = Microsoft.UI.Xaml.Controls;

namespace TreeViewTest
{
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
            InitializeTreeView();
        }

        private void InitializeTreeView()
        {
            // A TreeView can have more than 1 root node. The Pictures library
            // and the Music library will each be a root node in the tree.
            // Get Pictures library.
            StorageFolder picturesFolder = KnownFolders.PicturesLibrary;
            muxc.TreeViewNode pictureNode = new muxc.TreeViewNode();
            pictureNode.Content = picturesFolder;
            pictureNode.IsExpanded = true;
            pictureNode.HasUnrealizedChildren = true;
            sampleTreeView.RootNodes.Add(pictureNode);
            FillTreeNode(pictureNode);

            // Get Music library.
            StorageFolder musicFolder = KnownFolders.MusicLibrary;
            muxc.TreeViewNode musicNode = new muxc.TreeViewNode();
            musicNode.Content = musicFolder;
            musicNode.IsExpanded = true;
            musicNode.HasUnrealizedChildren = true;
            sampleTreeView.RootNodes.Add(musicNode);
            FillTreeNode(musicNode);
        }

        private async void FillTreeNode(muxc.TreeViewNode node)
        {
            // Get the contents of the folder represented by the current tree node.
            // Add each item as a new child node of the node that's being expanded.

            // Only process the node if it's a folder and has unrealized children.
            StorageFolder folder = null;

            if (node.Content is StorageFolder && node.HasUnrealizedChildren == true)
            {
                folder = node.Content as StorageFolder;
            }
            else
            {
                // The node isn't a folder, or it's already been filled.
                return;
            }

            IReadOnlyList<IStorageItem> itemsList = await folder.GetItemsAsync();

            if (itemsList.Count == 0)
            {
                // The item is a folder, but it's empty. Leave HasUnrealizedChildren = true so
                // that the chevron appears, but don't try to process children that aren't there.
                return;
            }

            foreach (var item in itemsList)
            {
                var newNode = new muxc.TreeViewNode();
                newNode.Content = item;

                if (item is StorageFolder)
                {
                    // If the item is a folder, set HasUnrealizedChildren to true.
                    // This makes the collapsed chevron show up.
                    newNode.HasUnrealizedChildren = true;
                }
                else
                {
                    // Item is StorageFile. No processing needed for this scenario.
                }

                node.Children.Add(newNode);
            }

            // Children were just added to this node, so set HasUnrealizedChildren to false.
            node.HasUnrealizedChildren = false;
        }

        private void SampleTreeView_Expanding(muxc.TreeView sender, muxc.TreeViewExpandingEventArgs args)
        {
            if (args.Node.HasUnrealizedChildren)
            {
                FillTreeNode(args.Node);
            }
        }

        private void SampleTreeView_Collapsed(muxc.TreeView sender, muxc.TreeViewCollapsedEventArgs args)
        {
            args.Node.Children.Clear();
            args.Node.HasUnrealizedChildren = true;
        }

        private void SampleTreeView_ItemInvoked(muxc.TreeView sender, muxc.TreeViewItemInvokedEventArgs args)
        {
            var node = args.InvokedItem as muxc.TreeViewNode;

            if (node.Content is IStorageItem item)
            {
                FileNameTextBlock.Text = item.Name;
                FilePathTextBlock.Text = item.Path;
                TreeDepthTextBlock.Text = node.Depth.ToString();

                if (node.Content is StorageFolder)
                {
                    node.IsExpanded = !node.IsExpanded;
                }
            }
        }

        private void RefreshButton_Click(object sender, RoutedEventArgs e)
        {
            sampleTreeView.RootNodes.Clear();
            InitializeTreeView();
        }
    }

    public class ExplorerItemTemplateSelector : DataTemplateSelector
    {
        public DataTemplate DefaultTemplate { get; set; }
        public DataTemplate MusicItemTemplate { get; set; }
        public DataTemplate PictureItemTemplate { get; set; }
        public DataTemplate MusicFolderTemplate { get; set; }
        public DataTemplate PictureFolderTemplate { get; set; }

        protected override DataTemplate SelectTemplateCore(object item)
        {
            var node = (muxc.TreeViewNode)item;

            if (node.Content is StorageFolder)
            {
                var content = node.Content as StorageFolder;
                if (content.DisplayName.StartsWith("Pictures")) return PictureFolderTemplate;
                if (content.DisplayName.StartsWith("Music")) return MusicFolderTemplate;
            }
            else if (node.Content is StorageFile)
            {
                var content = node.Content as StorageFile;
                if (content.ContentType.StartsWith("image")) return PictureItemTemplate;
                if (content.ContentType.StartsWith("audio")) return MusicItemTemplate;

            }
            return DefaultTemplate;
        }
    }
}
```

```vb
Public NotInheritable Class MainPage
    Inherits Page

    Public Sub New()
        InitializeComponent()
        InitializeTreeView()
    End Sub

    Private Sub InitializeTreeView()
        ' A TreeView can have more than 1 root node. The Pictures library
        ' and the Music library will each be a root node in the tree.
        ' Get Pictures library.
        Dim picturesFolder As StorageFolder = KnownFolders.PicturesLibrary
        Dim pictureNode As New muxc.TreeViewNode With {
        .Content = picturesFolder,
        .IsExpanded = True,
        .HasUnrealizedChildren = True
    }
        sampleTreeView.RootNodes.Add(pictureNode)
        FillTreeNode(pictureNode)

        ' Get Music library.
        Dim musicFolder As StorageFolder = KnownFolders.MusicLibrary
        Dim musicNode As New muxc.TreeViewNode With {
        .Content = musicFolder,
        .IsExpanded = True,
        .HasUnrealizedChildren = True
    }
        sampleTreeView.RootNodes.Add(musicNode)
        FillTreeNode(musicNode)
    End Sub

    Private Async Sub FillTreeNode(node As muxc.TreeViewNode)
        ' Get the contents of the folder represented by the current tree node.
        ' Add each item as a new child node of the node that's being expanded.

        ' Only process the node if it's a folder and has unrealized children.
        Dim folder As StorageFolder = Nothing
        If TypeOf node.Content Is StorageFolder AndAlso node.HasUnrealizedChildren Then
            folder = TryCast(node.Content, StorageFolder)
        Else
            ' The node isn't a folder, or it's already been filled.
            Return
        End If

        Dim itemsList As IReadOnlyList(Of IStorageItem) = Await folder.GetItemsAsync()
        If itemsList.Count = 0 Then
            ' The item is a folder, but it's empty. Leave HasUnrealizedChildren = true so
            ' that the chevron appears, but don't try to process children that aren't there.
            Return
        End If

        For Each item In itemsList
            Dim newNode As New muxc.TreeViewNode With {
            .Content = item
        }
            If TypeOf item Is StorageFolder Then
                ' If the item is a folder, set HasUnrealizedChildren to True.
                ' This makes the collapsed chevron show up.
                newNode.HasUnrealizedChildren = True
            Else
                ' Item is StorageFile. No processing needed for this scenario.
            End If
            node.Children.Add(newNode)
        Next

        ' Children were just added to this node, so set HasUnrealizedChildren to False.
        node.HasUnrealizedChildren = False
    End Sub

    Private Sub SampleTreeView_Expanding(sender As muxc.TreeView, args As muxc.TreeViewExpandingEventArgs)
        If args.Node.HasUnrealizedChildren Then
            FillTreeNode(args.Node)
        End If
    End Sub

    Private Sub SampleTreeView_Collapsed(sender As muxc.TreeView, args As muxc.TreeViewCollapsedEventArgs)
        args.Node.Children.Clear()
        args.Node.HasUnrealizedChildren = True
    End Sub

    Private Sub SampleTreeView_ItemInvoked(sender As muxc.TreeView, args As muxc.TreeViewItemInvokedEventArgs)
        Dim node = TryCast(args.InvokedItem, muxc.TreeViewNode)
        Dim item = TryCast(node.Content, IStorageItem)
        If item IsNot Nothing Then
            FileNameTextBlock.Text = item.Name
            FilePathTextBlock.Text = item.Path
            TreeDepthTextBlock.Text = node.Depth.ToString()
            If TypeOf node.Content Is StorageFolder Then
                node.IsExpanded = Not node.IsExpanded
            End If
        End If
    End Sub

    Private Sub RefreshButton_Click(sender As Object, e As RoutedEventArgs)
        sampleTreeView.RootNodes.Clear()
        InitializeTreeView()
    End Sub

End Class

Public Class ExplorerItemTemplateSelector
    Inherits DataTemplateSelector

    Public Property DefaultTemplate As DataTemplate
    Public Property MusicItemTemplate As DataTemplate
    Public Property PictureItemTemplate As DataTemplate
    Public Property MusicFolderTemplate As DataTemplate
    Public Property PictureFolderTemplate As DataTemplate

    Protected Overrides Function SelectTemplateCore(ByVal item As Object) As DataTemplate
        Dim node = CType(item, muxc.TreeViewNode)

        If TypeOf node.Content Is StorageFolder Then
            Dim content = TryCast(node.Content, StorageFolder)
            If content.DisplayName.StartsWith("Pictures") Then Return PictureFolderTemplate
            If content.DisplayName.StartsWith("Music") Then Return MusicFolderTemplate
        ElseIf TypeOf node.Content Is StorageFile Then
            Dim content = TryCast(node.Content, StorageFile)
            If content.ContentType.StartsWith("image") Then Return PictureItemTemplate
            If content.ContentType.StartsWith("audio") Then Return MusicItemTemplate
        End If

        Return DefaultTemplate
    End Function
End Class
```

### <a name="drag-and-drop-items-between-tree-views"></a>在树视图之间拖放项目

下面的示例演示如何创建两个可在彼此之间拖放项目的树视图。 当某个项目拖动到其他树视图时，它会添加到列表的末尾。 但是，项目可以在树视图中重新排序。 此示例也仅考虑具有一个根节点的树视图。

```xaml
<Page
    x:Class="TreeViewTest.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.ColumnDefinitions>
            <ColumnDefinition/>
            <ColumnDefinition/>
        </Grid.ColumnDefinitions>

        <TreeView x:Name="treeView1"
                  AllowDrop="True"
                  CanDragItems="True"
                  CanReorderItems="True"
                  DragOver="TreeView_DragOver"
                  Drop="TreeView_Drop"
                  DragItemsStarting="TreeView_DragItemsStarting"
                  DragItemsCompleted="TreeView_DragItemsCompleted"/>
        <TreeView x:Name="treeView2"
                  AllowDrop="True"
                  Grid.Column="1"
                  CanDragItems="True"
                  CanReorderItems="True"
                  DragOver="TreeView_DragOver"
                  Drop="TreeView_Drop"
                  DragItemsStarting="TreeView_DragItemsStarting"
                  DragItemsCompleted="TreeView_DragItemsCompleted"/>

    </Grid>

</Page>
```

```csharp
using System;
using Windows.ApplicationModel.DataTransfer;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;

namespace TreeViewTest
{
    public sealed partial class MainPage : Page
    {
        private TreeViewNode deletedItem;
        private TreeView sourceTreeView;

        public MainPage()
        {
            this.InitializeComponent();
            InitializeTreeView();
        }

        private void InitializeTreeView()
        {
            TreeViewNode parentNode1 = new TreeViewNode() { Content = "tv1" };
            TreeViewNode parentNode2 = new TreeViewNode() { Content = "tv2" };

            parentNode1.Children.Add(new TreeViewNode() { Content = "tv1FirstChild" });
            parentNode1.Children.Add(new TreeViewNode() { Content = "tv1SecondChild" });
            parentNode1.Children.Add(new TreeViewNode() { Content = "tv1ThirdChild" });
            parentNode1.Children.Add(new TreeViewNode() { Content = "tv1FourthChild" });
            parentNode1.IsExpanded = true;
            treeView1.RootNodes.Add(parentNode1);

            parentNode2.Children.Add(new TreeViewNode() { Content = "tv2FirstChild" });
            parentNode2.Children.Add(new TreeViewNode() { Content = "tv2SecondChild" });
            parentNode2.IsExpanded = true;
            treeView2.RootNodes.Add(parentNode2);
        }

        private void TreeView_DragOver(object sender, DragEventArgs e)
        {
            if (e.DataView.Contains(StandardDataFormats.Text))
            {
                e.AcceptedOperation = DataPackageOperation.Move;
            }
        }

        private async void TreeView_Drop(object sender, DragEventArgs e)
        {
            if (e.DataView.Contains(StandardDataFormats.Text))
            {
                string text = await e.DataView.GetTextAsync();
                TreeView destinationTreeView = sender as TreeView;

                if (destinationTreeView.RootNodes != null)
                {
                    TreeViewNode newNode = new TreeViewNode() { Content = text };
                    destinationTreeView.RootNodes[0].Children.Add(newNode);
                    deletedItem = newNode;
                }
            }
        }

        private void TreeView_DragItemsStarting(TreeView sender, TreeViewDragItemsStartingEventArgs args)
        {
            if (args.Items.Count == 1)
            {
                args.Data.RequestedOperation = DataPackageOperation.Move;
                sourceTreeView = sender;

                foreach (var item in args.Items)
                {
                    args.Data.SetText(item.ToString());
                }
            }
        }

        private void TreeView_DragItemsCompleted(TreeView sender, TreeViewDragItemsCompletedEventArgs args)
        {
            var children = sourceTreeView.RootNodes[0].Children;

            if (deletedItem != null)
            {
                for (int i = 0; i < children.Count; i++)
                {
                    if (children[i].Content.ToString() == deletedItem.Content.ToString())
                    {
                        children.RemoveAt(i);
                        break;
                    }
                }
            }

            sourceTreeView = null;
            deletedItem = null;
        }
    }
}
```

## <a name="related-articles"></a>相关文章

- [TreeView 类](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.treeview)
- [ListView 类](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview)
- [ListView 和 GridView](listview-and-gridview.md)
