---
author: Jwmsft
description: 你可以创建可扩展树视图的 ItemsSource 绑定到分层数据源，或者可以创建并自行管理 TreeViewNode 对象。
title: 树视图
label: Tree view
template: detail.hbs
ms.author: jimwalk
ms.date: 10/02/2018
ms.localizationpriority: medium
pm-contact: predavid
design-contact: ksulliv
dev-contact: joyate
doc-status: Published
dev_langs:
- csharp
- vb
ms.openlocfilehash: 36b81cf07b92760235a18f4474a14b7b55e0a7be
ms.sourcegitcommit: 49aab071aa2bd88f1c165438ee7e5c854b3e4f61
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2018
ms.locfileid: "4460968"
---
# <a name="treeview"></a>树视图

XAML 树视图控件支持分层列表，其中具有包含嵌套项的展开节点和折叠节点。 它可用于说明你的用户界面中的文件夹结构或嵌套关系。

TreeView API 支持以下功能：

- N 级嵌套
- 选择单个或多个节点
- （预览版）数据绑定到树视图和 TreeViewItem ItemsSource 属性
- （预览版）树视图项模板 root TreeViewItem
- （预览版）TreeViewItem 中的内容的任意类型
- （预览版）将拖放在树视图之间

| **获取 Windows UI 库** |
| - |
| 此控件是 Windows UI 库，包含新的控件和适用于 UWP 应用的 UI 功能的 NuGet 包的一部分。 有关详细信息，包括安装说明，请参阅[Windows UI 库概述](https://docs.microsoft.com/uwp/toolkits/winui/)。 |

| **平台 Api** | **Windows UI 库 Api** |
| - | - |
| [TreeView 类](/uwp/api/windows.ui.xaml.controls.treeview)， [TreeViewNode 类](/uwp/api/windows.ui.xaml.controls.treeviewnode)， [TreeView.ItemsSource 属性](/uwp/api/windows.ui.xaml.controls.treeview.itemssource) | [TreeView 类](/uwp/api/microsoft.ui.xaml.controls.treeview)， [TreeViewNode 类](/uwp/api/microsoft.ui.xaml.controls.treeviewnode)， [TreeView.ItemsSource 属性](/uwp/api/microsoft.ui.xaml.controls.treeview.itemssource) |

## <a name="is-this-the-right-control"></a>这是正确的控件吗？

- 当项目已嵌套列表项，并且演示项目与其对等项和节点的层次结构关系很重要时，使用树视图。

- 如果强调某个项目嵌套关系不是优先事项，则避免使用树视图。 对于大多数深化方案，适合使用常规列表视图。

## <a name="examples"></a>示例

<table>
<th align="left">XAML 控件库<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果你安装了该<strong style="font-weight: semi-bold">XAML 控件库</strong>应用，单击此处<a href="xamlcontrolsgallery:/item/TreeView">打开该应用并查看操作中的树视图</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="treeview-ui"></a>TreeView UI

树视图可以使用缩进和图标的组合来表示文件夹/父节点和非文件夹/子节点之间的嵌套关系。 折叠的节点使用指向右边的 V 形图标，而展开的节点使用指向下方的 V 形图标。

![TreeView 中的 V 形图标](images/treeview-simple.png)

你可以在树视图项目数据模板中使用图标来表示节点。 如果这样做，应只对表示文本文件夹的节点（如磁盘上的文件夹结构）使用文件夹图标。

![同时使用 V 形图标和文件夹图标的 TreeView](images/treeview-icons.png)

## <a name="create-a-tree-view"></a>创建树视图

你可以创建树视图的[ItemsSource](/uwp/api/windows.ui.xaml.controls.treeview.itemssource)绑定到分层数据源，或者可以创建并自行管理 TreeViewNode 对象。

要创建树视图，可以使用 [TreeView](/uwp/api/windows.ui.xaml.controls.treeview) 控件和 [TreeViewNode](/uwp/api/windows.ui.xaml.controls.treeviewnode) 对象层次结构。 通过向 TreeView 控件的[RootNodes](/uwp/api/windows.ui.xaml.controls.treeview.rootnodes)集合添加一个或多个根节点来创建节点层次结构。 然后可以向每个 TreeViewNode 的 Children 集合中添加多个节点。 你可以通过嵌套树视图节点来创建任意数量的层次。

从 Windows Insider Preview 中，你可以绑定分层数据源到[ItemsSource](/uwp/api/windows.ui.xaml.controls.treeview.itemssource)属性来提供的树视图内容，就像你处理 ListView 的 ItemsSource。 同样，使用[ItemTemplate](/uwp/api/windows.ui.xaml.controls.treeview.itemtemplate) （和可选[ItemTemplateSelector](/uwp/api/windows.ui.xaml.controls.treeview.itemtemplate)） 提供一个 datatemplate，呈现项目它。

> [!IMPORTANT]
> ItemsSource 是替代机制到 TreeView.RootNodes，将内容放到树视图控件。 不能同时设置 ItemsSource 和 RootNodes。 当你使用 ItemsSource 时，节点为你创建的并可以从 TreeView.RootNodes 属性访问它们。

下面是一个使用 XAML 声明的简单树视图示例。 通常以代码方式添加节点，但这里我们显示的是 XAML 层次结构，因为这有助于更直观地显示节点层次结构的创建方式。

```xaml
<TreeView>
    <TreeView.RootNodes>
        <TreeViewNode Content="Flavors" IsExpanded="True">
            <TreeViewNode.Children>
                <TreeViewNode Content="Vanilla"/>
                <TreeViewNode Content="Strawberry"/>
                <TreeViewNode Content="Chocolate"/>
            </TreeViewNode.Children>
        </TreeViewNode>
    </TreeView.RootNodes>
</TreeView>
```

在大多数情况下，树视图显示来自数据源，因此通常声明根 TreeView 控件在 XAML 中，但在代码中或使用数据绑定方式添加 TreeViewNode 对象。

### <a name="bind-to-a-hierarchical-data-source"></a>绑定到分层数据源

若要创建树视图使用数据绑定，设置 TreeView.ItemsSource 属性分层集合。 然后在 ItemTemplate，设置子项目集合的 TreeViewItem.ItemsSource 属性。

```xaml
<TreeView ItemsSource="{x:Bind DataSource}">
    <TreeView.ItemTemplate>
        <DataTemplate x:DataType="local:Item">
            <TreeViewItem ItemsSource="{x:Bind Children}"
                          Content="{x:Bind Name}"/>
        </DataTemplate>
    </TreeView.ItemTemplate>
</TreeView>
```

请参阅_使用数据绑定的树视图_示例部分获取的完整代码。

#### <a name="items-and-item-containers"></a>项目和项目容器

如果你使用 TreeView.ItemsSource，这些 Api 都是可用于从容器中获取的节点或数据项，反之亦然。

| **[TreeViewItem](/uwp/api/windows.ui.xaml.controls.treeviewitem)** | |
| - | - |
| [TreeView.ItemFromContainer](/uwp/api/windows.ui.xaml.controls.treeview.itemfromcontainer) | 指定 TreeViewItem 容器中获取数据项目。 |
| [TreeView.ContainerFromItem](/uwp/api/windows.ui.xaml.controls.treeview.containerfromitem) | 获取指定的数据项的 TreeViewItem 容器。 |

| **[TreeViewNode](/uwp/api/windows.ui.xaml.controls.treeviewnode)** | |
| - | - |
| [TreeView.NodeFromContainer](/uwp/api/windows.ui.xaml.controls.treeview.nodefromcontainer) | 获取适用于指定 TreeViewItem 容器的 TreeViewNode。 |
| [TreeView.ContainerFromNode](/uwp/api/windows.ui.xaml.controls.treeview.containerfromnode) | 获取指定 TreeViewNode TreeViewItem 容器。 |

### <a name="manage-tree-view-nodes"></a>管理树视图节点

此树视图与之前以 XAML 创建的树视图相同，但节点是以代码方式创建的。

```xaml
<TreeView x:Name="sampleTreeView"/>
```

```csharp
private void InitializeTreeView()
{
    TreeViewNode rootNode = new TreeViewNode() { Content = "Flavors" };
    rootNode.IsExpanded = true;
    rootNode.Children.Add(new TreeViewNode() { Content = "Vanilla" });
    rootNode.Children.Add(new TreeViewNode() { Content = "Strawberry" });
    rootNode.Children.Add(new TreeViewNode() { Content = "Chocolate" });

    sampleTreeView.RootNodes.Add(rootNode);
}
```

```vb
Private Sub InitializeTreeView()
    Dim rootNode As New TreeViewNode With {.Content = "Flavors", .IsExpanded = True}
    With rootNode.Children
        .Add(New TreeViewNode With {.Content = "Vanilla"})
        .Add(New TreeViewNode With {.Content = "Strawberry"})
        .Add(New TreeViewNode With {.Content = "Chocolate"})
    End With
    sampleTreeView.RootNodes.Add(rootNode)
End Sub
```

可以使用以下 API 管理树视图的数据层次结构。

| **[TreeView](/uwp/api/windows.ui.xaml.controls.treeview)** | |
| - | - |
| [RootNodes](/uwp/api/windows.ui.xaml.controls.treeview.rootnodes) | 一个树视图可以有一个或多个根节点。 向 RootNodes 集合添加一个 TreeViewNode 对象会创建一个根节点。 根节点的 **Parent** 始终为 **null**。 根节点的 **Depth** 为 0。 |

| **[TreeViewNode](/uwp/api/windows.ui.xaml.controls.treeviewnode)** | |
| - | - |
| [Children](/uwp/api/windows.ui.xaml.controls.treeviewnode.children) | 向父节点的 Children 集合添加 TreeViewNode 对象可创建节点层次结构。 节点是其 **Children** 集合中的所有节点的 **Parent**。 |
| [HasChildren](/uwp/api/windows.ui.xaml.controls.treeviewnode.haschildren) | 如果节点有已实现的子级，则为 **true**。 **false** 表示空的文件夹或项目。 |
| [HasUnrealizedChildren](/uwp/api/windows.ui.xaml.controls.treeviewnode.hasunrealizedchildren) | 填充展开的节点时可以使用此属性。 请参阅本文稍后部分的_填充正在展开的节点_。 |
| [深度 ](/uwp/api/windows.ui.xaml.controls.treeviewnode.depth) | 指示子节点距根节点的距离。 |
| [Parent 的子磁盘）](/uwp/api/windows.ui.xaml.controls.treeviewnode.parent) | 获取拥有此节点所属的 **Children** 集合的 TreeViewNode。 |

树视图使用 **HasChildren** 和 **HasUnrealizedChildren** 属性确定是否显示展开/折叠图标。 如果任一属性为 **true**，则显示图标；否则不显示。

## <a name="tree-view-node-content"></a>树视图节点内容

可以将树视图所表示的数据项存储在树视图的 [Content](/uwp/api/windows.ui.xaml.controls.treeviewnode.content) 属性中。

在前面的示例中，内容为简单的字符串值。 在这里，有一个树视图节点表示用户的 Pictures 文件夹，因此将图片库 [StorageFolder](/uwp/api/windows.storage.storagefolder) 分配给该节点的 Content 属性。

```csharp
StorageFolder picturesFolder = KnownFolders.PicturesLibrary;
TreeViewNode pictureNode = new TreeViewNode();
pictureNode.Content = picturesFolder;
```

```vb
Dim picturesFolder As StorageFolder = KnownFolders.PicturesLibrary
Dim pictureNode As New TreeViewNode With {.Content = picturesFolder}
```

你可以提供 [DataTemplate](/uwp/api/windows.ui.xaml.datatemplate) 来指定数据项在树视图中的显示方式。

> [!NOTE]
> 在 Windows 10 版本 1803 中，必须重新设置 TreeView 控件模板，如果内容不是字符串，还必须指定自定义 ItemTemplate。 有关更多信息，请参阅本文末尾的完整示例。 在更高版本，设置[TreeView.ItemTemplate](/uwp/api/windows.ui.xaml.controls.treeview.itemtemplate)属性。

### <a name="item-container-style"></a>项容器样式

你使用的是 ItemsSource 或 RootNodes，用于显示每个节点 – 的实际元素称为"容器"– [TreeViewItem](/uwp/api/windows.ui.xaml.controls.treeviewitem)对象。 你可以设置容器样式使用树视图的 ItemContainerStyle 或 ItemContainerStyleSelector 属性。

### <a name="item-template-selectors"></a>项目模板选择器

你可以选择设置不同的具体取决于项目类型的树视图项目 DataTemplate。 例如，在文件资源管理器应用中，可以使用一个数据模板的文件夹和另一个用于文件。

![文件夹和文件使用不同的数据模板](images/treeview-icons.png)

下面是如何创建和使用项目模板选择器的示例。

```xaml
<Page.Resources>
    <DataTemplate x:Key="FolderTemplate" x:DataType="local:ExplorerItem">
        <TreeViewItem ItemsSource="{x:Bind Children}">
            <StackPanel Orientation="Horizontal">
                <Image Width="20" Source="Assets/folder.png"/>
                <TextBlock Text="{x:Bind Name}" />
            </StackPanel>
        </TreeViewItem>
    </DataTemplate>

    <DataTemplate x:Key="FileTemplate" x:DataType="local:ExplorerItem">
        <TreeViewItem>
            <StackPanel Orientation="Horizontal">
                <Image Width="20" Source="Assets/file.png"/>
                <TextBlock Text="{Binding Name}"/>
            </StackPanel>
        </TreeViewItem>
    </DataTemplate>

    <local:ExplorerItemTemplateSelector
            x:Key="ExplorerItemTemplateSelector"
            FolderTemplate="{StaticResource FolderTemplate}"
            FileTemplate="{StaticResource FileTemplate}" />
</Page.Resources>

<Grid>
    <TreeView ItemsSource="{x:Bind DataSource}"
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

## <a name="interacting-with-a-tree-view"></a>与树视图交互

你可以配置树视图以允许用户通过几种不同的方式与其交互：

- 展开或折叠节点
- 单选或多选项目
- 单击以调用项目

### <a name="expandcollapse"></a>展开/折叠

任何有子级的树视图节点都始终可以通过单击展开/折叠字形来展开或折叠节点。 你也可以通过编程展开或折叠节点，并在节点状态发生更改时进行响应。

#### <a name="expandcollapse-a-node-programmatically"></a>以编程方式展开/折叠节点

在代码中，有两种方式可以展开或折叠树视图节点。

- [TreeView](/uwp/api/windows.ui.xaml.controls.treeview) 类具有 [Collapse](/uwp/api/windows.ui.xaml.controls.treeview.collapse) 和 [Expand](/uwp/api/windows.ui.xaml.controls.treeview.expand) 方法。 调用这些方法时，需要传入要展开或折叠的 TreeViewNode。

- 每个 [TreeViewNode](/uwp/api/windows.ui.xaml.controls.treeviewnode) 都有 [IsExpanded](/uwp/api/windows.ui.xaml.controls.treeviewnode.isexpanded) 属性。 可以使用此属性检查节点的状态或对其进行设置以更改状态。 你也可以在 XAML 中设置此属性以设置节点的初始状态。

### <a name="fill-a-node-when-its-expanding"></a>填充正在展开的节点

你可能需要在树视图中显示大量节点，或者无法提前知道树视图会有多少节点。 TreeView 控件不是虚拟化的，因此可以通过填充每个展开的节点和删除折叠的子节点来管理资源。

处理 [Expanding](/uwp/api/windows.ui.xaml.controls.treeview.expand) 事件并使用 [HasUnrealizedChildren](/uwp/api/windows.ui.xaml.controls.treeviewnode.hasunrealizedchildren) 属性可在节点展开时向节点添加子级。 HasUnrealizedChildren 属性指示是否需要填充节点或其 Children 集合是否已填充。 请务必记住，TreeViewNode 并不设置此值，你需要在应用节点中对其进行管理。

下面是这些 API 的使用示例。 请参阅本文末尾的完整示例代码了解上下文，包括“FillTreeNode”的实现。

```csharp
private void SampleTreeView_Expanding(TreeView sender, TreeViewExpandingEventArgs args)
{
    if (args.Node.HasUnrealizedChildren)
    {
        FillTreeNode(args.Node);
    }
}
```

```vb
Private Sub SampleTreeView_Expanding(sender As TreeView, args As TreeViewExpandingEventArgs)
    If args.Node.HasUnrealizedChildren Then
        FillTreeNode(args.Node)
    End If
End Sub
```

这不是必需的，但你可能还想处理 [Collapsed](/uwp/api/windows.ui.xaml.controls.treeview.collapsed) 事件并在父节点关闭时删除子节点。 当树视图有很多节点或节点数据使用大量资源时，这一点可能很重要。 应考虑每次打开节点时进行填充与保留已关闭节点的子级的性能影响。 最佳选择取决于你的应用。

下面是一个 Collapsed 事件处理程序示例。

```csharp
private void SampleTreeView_Collapsed(TreeView sender, TreeViewCollapsedEventArgs args)
{
    args.Node.Children.Clear();
    args.Node.HasUnrealizedChildren = true;
}
```

```vb
Private Sub SampleTreeView_Collapsed(sender As TreeView, args As TreeViewCollapsedEventArgs)
    args.Node.Children.Clear()
    args.Node.HasUnrealizedChildren = True
End Sub
```

### <a name="invoking-an-item"></a>调用项目

用户可以调用操作（处理按钮等项目）而不是选择项目。 你可以处理 [ItemInvoked](/uwp/api/windows.ui.xaml.controls.treeview.iteminvoked) 事件以响应此用户交互。

> [!NOTE]
> 与具有 [IsItemClickEnabled](/uwp/api/windows.ui.xaml.controls.listviewbase.isitemclickenabled) 属性的 ListView 不同，调用项目在树视图中始终处于启用状态。 你仍然可以选择是否处理事件。

**[TreeViewItemInvokedEventArgs](/uwp/api/windows.ui.xaml.controls.treeviewiteminvokedeventargs) 类**

ItemInvoked 事件参数向你提供访问调用项目。 [InvokedItem](/uwp/api/windows.ui.xaml.controls.treeviewiteminvokedeventargs.invokeditem) 属性具有已调用节点。 可以将其强制转换为 TreeViewNode 斌从 TreeViewNode.Content 属性获取数据项。

下面是一个 ItemInvoked 事件处理程序示例。 数据项是一个 [IStorageItem](/uwp/api/windows.storage.istorageitem)，此示例仅显示关于文件和树的部分信息。 此外，如果节点为文件夹节点，它展开或折叠节点在同一时间。 否则，仅当单击 V 形图标时才会展开或折叠节点。

```csharp
private void SampleTreeView_ItemInvoked(TreeView sender, TreeViewItemInvokedEventArgs args)
{
    var node = args.InvokedItem as TreeViewNode;
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
Private Sub SampleTreeView_ItemInvoked(sender As TreeView, args As TreeViewItemInvokedEventArgs)
    Dim node = TryCast(args.InvokedItem, TreeViewNode)
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

### <a name="item-selection"></a>项目选择

TreeView 控件支持单选和多选。 默认情况下，节点选择处于关闭状态，但你可以设置 [TreeView.SelectionMode](/uwp/api/windows.ui.xaml.controls.treeview.selectionmode) 属性以允许选择节点。 [TreeViewSelectionMode](/uwp/api/windows.ui.xaml.controls.treeviewselectionmode) 值为 **None**、**Single** 和 **Multiple**。

#### <a name="multiple-selection"></a>多选

启用多项选择后，每个树视图节点，旁边显示一个复选框和突出显示所选的项目。 用户可以使用复选框选择或取消选择项目；单击项目仍将导致项目调用。

选择或取消选择一个父节点将选择或取消选择该节点下的所有子元素。 如果一些，但不是全部会选择子级的父节点下，父节点的复选框显示为不确定 （装满黑盒）。

![树视图中的多选](images/treeview-selection.png)

选择或取消选择一个父节点将选择或取消选择该节点下的所有子元素。 如果一些，但不是全部会选择子级的父节点下，父节点的复选框显示为不确定 （装满黑盒）。

![树视图中的多选](images/treeview-selection.png)

选中的节点会添加到树视图的 [SelectedNodes](/uwp/api/windows.ui.xaml.controls.treeview.selectednodes) 集合中。 你可以调用 [SelectAll](/uwp/api/windows.ui.xaml.controls.treeview.selectall) 方法来选择树视图中的所有节点。

> [!NOTE]
> 调用 **SelectAll** 会选择所有已实现的节点，而无论 SelectionMode 是什么。 为提供一致的用户体验，当 SelectionMode 为 **Multiple** 时应调用 SelectAll。

#### <a name="selection-and-realizedunrealized-nodes"></a>选择与已实现/未实现节点

如果树视图有未实现的节点，这些节点将不计入选择范围。 关于未实现节点的选择，需要记住以下事项。

- 选择一个父节点也会选择该父级下的所有已实现子级。 同样，如果选择了所有子节点，则父节点也会被选中。
- SelectAll 方法只会将已实现的节点添加到 SelectedNodes 集合中。
- 如果选择了具有未实现子级的父节点，该子级将在实现时被选中。

## <a name="code-examples"></a>代码示例

### <a name="tree-view-using-xaml"></a>使用 XAML 的树视图

本示例介绍如何在 XAML 中创建简单的树视图结构。 此树视图显示用户可以选择的冰淇淋口味和配料，配料按类别排列。 多选已启用，当用户单击按钮时，SelectedItems 会显示在主应用 UI 中。

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}" Padding="100">
    <SplitView IsPaneOpen="True"
               DisplayMode="Inline"
               OpenPaneLength="296">
        <SplitView.Pane>
            <TreeView x:Name="DessertTree" SelectionMode="Multiple">
                <TreeView.RootNodes>
                    <TreeViewNode Content="Flavors" IsExpanded="True">
                        <TreeViewNode.Children>
                            <TreeViewNode Content="Vanilla"/>
                            <TreeViewNode Content="Strawberry"/>
                            <TreeViewNode Content="Chocolate"/>
                        </TreeViewNode.Children>
                    </TreeViewNode>

                    <TreeViewNode Content="Toppings">
                        <TreeViewNode.Children>
                            <TreeViewNode Content="Candy">
                                <TreeViewNode.Children>
                                    <TreeViewNode Content="Chocolate"/>
                                    <TreeViewNode Content="Mint"/>
                                    <TreeViewNode Content="Sprinkles"/>
                                </TreeViewNode.Children>
                            </TreeViewNode>
                            <TreeViewNode Content="Fruits">
                                <TreeViewNode.Children>
                                    <TreeViewNode Content="Mango"/>
                                    <TreeViewNode Content="Peach"/>
                                    <TreeViewNode Content="Kiwi"/>
                                </TreeViewNode.Children>
                            </TreeViewNode>
                            <TreeViewNode Content="Berries">
                                <TreeViewNode.Children>
                                    <TreeViewNode Content="Strawberry"/>
                                    <TreeViewNode Content="Blueberry"/>
                                    <TreeViewNode Content="Blackberry"/>
                                </TreeViewNode.Children>
                            </TreeViewNode>
                        </TreeViewNode.Children>
                    </TreeViewNode>
                </TreeView.RootNodes>
            </TreeView>
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
```

```csharp
private void OrderButton_Click(object sender, RoutedEventArgs e)
{
    FlavorList.Text = string.Empty;
    ToppingList.Text = string.Empty;

    foreach (TreeViewNode node in DessertTree.SelectedNodes)
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
    if (DessertTree.SelectionMode == TreeViewSelectionMode.Multiple)
    {
        DessertTree.SelectAll();
    }
}
```

```vb
Private Sub OrderButton_Click(sender As Object, e As RoutedEventArgs)
    FlavorList.Text = String.Empty
    ToppingList.Text = String.Empty
    For Each node As TreeViewNode In DessertTree.SelectedNodes
        If node.Parent.Content?.ToString() = "Flavors" Then
            FlavorList.Text += node.Content & "; "
        ElseIf node.HasChildren = False Then
            ToppingList.Text += node.Content & "; "
        End If
    Next
End Sub

Private Sub SelectAllButton_Click(sender As Object, e As RoutedEventArgs)
    If DessertTree.SelectionMode = TreeViewSelectionMode.Multiple Then
        DessertTree.SelectAll()
    End If
End Sub
```

### <a name="tree-view-using-data-binding"></a>使用数据绑定的树视图

此示例显示了如何创建与前面示例相同的树视图。 但是，而不是在 XAML 中创建的数据层次结构，并将数据在代码中创建绑定到在树视图的 ItemsSource 属性。 （在上一示例中显示的按钮事件处理程序适用于此示例还。）

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}" Padding="100">
    <SplitView IsPaneOpen="True"
               DisplayMode="Inline"
               OpenPaneLength="296">
        <SplitView.Pane>
            <TreeView Name="DessertTree"
                      SelectionMode="Multiple"
                      ItemsSource="{x:Bind DataSource}">
                <TreeView.ItemTemplate>
                    <DataTemplate x:DataType="local:Item">
                        <TreeViewItem ItemsSource="{x:Bind Children}"
                                      Content="{x:Bind Name}"/>
                    </DataTemplate>
                </TreeView.ItemTemplate>
            </TreeView>
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
```

```csharp
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

    // Button event handlers...
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
```

### <a name="pictures-and-music-library-tree-view"></a>图片和音乐库树视图

本示例介绍如何创建树视图来显示用户的图片和音乐库的内容和结构。 因为无法预知项目数，因此每个节点都在展开时填充，在折叠时清空。

这里使用一个自定义项目模板来显示数据项，这些数据项类型为 [IStorageItem](/uwp/api/windows.storage.istorageitem)。

> [!IMPORTANT]
> 本示例中的代码需要使用 picturesLibrary 和 musicLibrary 功能。 有关文件访问的更多信息，请参阅[文件访问权限](../../files/file-access-permissions.md)、[枚举和查询文件和文件夹](../../files/quickstart-listing-files-and-folders.md)以及[音乐、图片和视频库中的文件和文件夹](../../files/quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md)。

```xaml
<Page
    x:Class="TreeViewApp1.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:TreeViewApp1"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">
    <Page.Resources>
        <DataTemplate x:Key="TreeViewItemDataTemplate">
            <Grid Height="44">
                <TextBlock
                    Text="{Binding Content.DisplayName}"
                    HorizontalAlignment="Left"
                    VerticalAlignment="Center"
                    Style="{ThemeResource BodyTextBlockStyle}"/>
            </Grid>
        </DataTemplate>

        <Style TargetType="TreeView">
            <Setter Property="IsTabStop" Value="False" />
            <Setter Property="Template">
                <Setter.Value>
                    <ControlTemplate TargetType="TreeView">
                        <TreeViewList x:Name="ListControl"
                                      ItemTemplate="{StaticResource TreeViewItemDataTemplate}"
                                      ItemContainerStyle="{StaticResource TreeViewItemStyle}"
                                      CanDragItems="True"
                                      AllowDrop="True"
                                      CanReorderItems="True">
                            <TreeViewList.ItemContainerTransitions>
                                <TransitionCollection>
                                    <ContentThemeTransition />
                                    <ReorderThemeTransition />
                                    <EntranceThemeTransition IsStaggeringEnabled="False" />
                                </TransitionCollection>
                            </TreeViewList.ItemContainerTransitions>
                        </TreeViewList>
                    </ControlTemplate>
                </Setter.Value>
            </Setter>
        </Style>
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
                    <TreeView x:Name="sampleTreeView" Grid.Row="1" SelectionMode="Single"
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
    TreeViewNode pictureNode = new TreeViewNode();
    pictureNode.Content = picturesFolder;
    pictureNode.IsExpanded = true;
    pictureNode.HasUnrealizedChildren = true;
    sampleTreeView.RootNodes.Add(pictureNode);
    FillTreeNode(pictureNode);

    // Get Music library.
    StorageFolder musicFolder = KnownFolders.MusicLibrary;
    TreeViewNode musicNode = new TreeViewNode();
    musicNode.Content = musicFolder;
    musicNode.IsExpanded = true;
    musicNode.HasUnrealizedChildren = true;
    sampleTreeView.RootNodes.Add(musicNode);
    FillTreeNode(musicNode);
}

private async void FillTreeNode(TreeViewNode node)
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
        var newNode = new TreeViewNode();
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

private void SampleTreeView_Expanding(TreeView sender, TreeViewExpandingEventArgs args)
{
    if (args.Node.HasUnrealizedChildren)
    {
        FillTreeNode(args.Node);
    }
}

private void SampleTreeView_Collapsed(TreeView sender, TreeViewCollapsedEventArgs args)
{
    args.Node.Children.Clear();
    args.Node.HasUnrealizedChildren = true;
}

private void SampleTreeView_ItemInvoked(TreeView sender, TreeViewItemInvokedEventArgs args)
{
    var node = args.InvokedItem as TreeViewNode;
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
```

```vb
Public Sub New()
    InitializeComponent()
    InitializeTreeView()
End Sub

Private Sub InitializeTreeView()
    ' A TreeView can have more than 1 root node. The Pictures library
    ' and the Music library will each be a root node in the tree.
    ' Get Pictures library.
    Dim picturesFolder As StorageFolder = KnownFolders.PicturesLibrary
    Dim pictureNode As New TreeViewNode With {
        .Content = picturesFolder,
        .IsExpanded = True,
        .HasUnrealizedChildren = True
    }
    sampleTreeView.RootNodes.Add(pictureNode)
    FillTreeNode(pictureNode)

    ' Get Music library.
    Dim musicFolder As StorageFolder = KnownFolders.MusicLibrary
    Dim musicNode As New TreeViewNode With {
        .Content = musicFolder,
        .IsExpanded = True,
        .HasUnrealizedChildren = True
    }
    sampleTreeView.RootNodes.Add(musicNode)
    FillTreeNode(musicNode)
End Sub

Private Async Sub FillTreeNode(node As TreeViewNode)
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
        Dim newNode As New TreeViewNode With {
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

Private Sub SampleTreeView_Expanding(sender As TreeView, args As TreeViewExpandingEventArgs)
    If args.Node.HasUnrealizedChildren Then
        FillTreeNode(args.Node)
    End If
End Sub

Private Sub SampleTreeView_Collapsed(sender As TreeView, args As TreeViewCollapsedEventArgs)
    args.Node.Children.Clear()
    args.Node.HasUnrealizedChildren = True
End Sub

Private Sub SampleTreeView_ItemInvoked(sender As TreeView, args As TreeViewItemInvokedEventArgs)
    Dim node = TryCast(args.InvokedItem, TreeViewNode)
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
```

## <a name="related-articles"></a>相关文章

- [TreeView 类](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.treeview)
- [ListView 类](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx)
- [ListView 和 GridView](listview-and-gridview.md)
