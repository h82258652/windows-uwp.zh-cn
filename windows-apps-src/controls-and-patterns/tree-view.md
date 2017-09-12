---
author: Jwmsft
Description: "使用树视图示例代码创建可扩展树。"
title: "树视图"
label: Tree view
template: detail.hbs
ms.openlocfilehash: c7ad99d20fe30ea4b94ad62de45b3832aae3805e
ms.sourcegitcommit: b42d14c775efbf449a544ddb881abd1c65c1ee86
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2017
---
# <a name="hierarchical-layout-with-treeview"></a>带有树视图的层次结构布局
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

树视图是一种层次结构列表模式，带有包含嵌套项的展开和折叠节点。 嵌套项可以是其他节点，也可以是常规列表项。 你可以使用 [ListView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx) 生成一个树视图，以演示 UI 中的文件夹结构或嵌套关系。

[树视图示例](http://go.microsoft.com/fwlink/?LinkId=785018)是使用 **ListView** 生成的参考实现。 它不是单独的控件。 Microsoft Edge 浏览器中的“收藏夹”窗格中看到的树视图使用此参考实现。

该示例支持：
- N 级嵌套
- 展开/折叠节点
- 在树视图内拖放节点
- 内置辅助功能

![参考示例中的树视图](images/tree-view-sample.png) | ![Edge 浏览器中的树视图](images/tree-view-edge.png)
-- | --
树视图参考示例 | Edge 浏览器中的树视图

## <a name="is-this-the-right-pattern"></a>这是正确的模式吗？

- 当项目已嵌套列表项，并且演示项目与其对等项和节点的层次结构关系很重要时，使用树视图。

- 如果强调某个项目嵌套关系不是优先事项，则避免使用树视图。 对于大多数深化方案，适合使用常规列表视图。

## <a name="treeview-ui-structure"></a>树视图 UI 结构

你可以在树视图中使用图标来表示节点。 可以使用缩进和图标的组合来表示文件夹/父节点和非文件夹/子节点之间的嵌套关系。 下面是如何执行此操作的指南。

### <a name="icons"></a>图标

使用图标指示某个项目是节点，以及该节点所处的状态（展开或折叠）。

#### <a name="chevron"></a>V 形

为了保持一致，折叠的节点应使用指向右边的 V 形，而展开的节点应使用指向下方的 V 形。

![在树视图中使用 V 形图标](images/treeview_chevron.png)

#### <a name="folder"></a>文件夹

将文件夹图标仅用于文件夹的文本表示形式。

![在树视图中使用文件夹图标](images/treeview_folder.png)

#### <a name="chevron-and-folder"></a>V 形和文件夹

仅当树视图中的非节点列表项也具有图标时，才应将 V 形和文件夹结合使用。

![在树视图中将 V 形和文件夹一起使用](images/treeview_chevron_folder.png)

#### <a name="redlines-for-indentation-of-folders-and-non-folder-nodes"></a>用于文件夹和非文件夹节点缩进的红线

将下面的屏幕截图中的红线用于文件夹和非文件夹节点的缩进。

![用于文件夹和非文件夹节点缩进的红线](images/treeview_chevron_folder_indent_rl.png)

## <a name="building-a-treeview"></a>生成树视图

树视图具有以下主类。 参考实现中定义并包含所有这些主类。

> **注意**&nbsp;&nbsp;树视图实现为使用 C++ 编写的 [Windows 运行时组件](https://msdn.microsoft.com/windows/uwp/winrt-components/index)，因此使用任何语言的 UWP 应用都可以引用它。 在示例中，树视图代码位于 *cpp/Control* 文件夹中。 C# 没有对应的 *cs/Control* 文件夹。

- `TreeNode` 类为树视图实现层次结构布局。 它还将要绑定到它的数据保留在项模板中。
- `TreeView` 类实现 ItemClick、文件夹的展开/折叠以及拖动启动的事件。
- `TreeViewItem` 类实现放置操作的事件。
- `ViewModel` 类平展 TreeViewItems 的列表，以便键盘导航和拖放等操作可以从 ListView 继承。

## <a name="create-a-data-template-for-your-treeviewitem"></a>为 TreeViewItem 创建数据模板

下面是为文件夹和非文件夹类型项设置数据模板的 XAML 部分。
- 若要将 ListViewItem 指定为文件夹，你将需要在 ListViewItem 上将 [AllowDrop](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.allowdrop.aspx) 属性显式设置为 **true**。 此 XAML 向你显示执行此操作的一种方法。
- 若要将 ListViewItem 指定为非文件夹，你不需要在 ListViewItem 本身上指定任何属性。 只需在 ListView 上将 AllowDrop 属性设置为 True。
- 可以使用展开/折叠文件夹图标或 V 形在视觉上指示某个文件夹处于展开还是折叠状态。
- 可以使用转换器选择本示例中所示的展开和折叠状态所需的不同图标。

```xaml
<!-- MainPage.xaml -->
<DataTemplate x:Key="TreeViewItemDataTemplate">
    <StackPanel Orientation="Horizontal" Height="40" Margin="{Binding Depth, Converter={StaticResource IntToIndConverter}}" AllowDrop="{Binding Data.IsFolder}">
        <FontIcon x:Name="expandCollapseChevron"
                  Glyph="{Binding IsExpanded, Converter={StaticResource expandCollapseGlyphConverter}}"
                  Visibility="{Binding Data.IsFolder, Converter={StaticResource booleanToVisibilityConverter}}"                           
                  FontSize="12"
                  Margin="12,8,12,8"
                  FontFamily="Segoe MDL2 Assets"                          
                  />
        <Grid>
            <FontIcon x:Name ="expandCollapseFolder"
                      Glyph="{Binding IsExpanded, Converter={StaticResource folderGlyphConverter}}"
                      Foreground="#FFFFE793"
                      FontSize="16"
                      Margin="0,8,12,8"
                      FontFamily="Segoe MDL2 Assets"
                      Visibility="{Binding Data.IsFolder, Converter={StaticResource booleanToVisibilityConverter}}"
                      />

            <FontIcon x:Name ="nonFolderIcon"
                      Glyph="&#xE160;"
                      Foreground="{ThemeResource SystemControlForegroundBaseLowBrush}"
                      FontSize="12"
                      Margin="20,8,12,8"
                      FontFamily="Segoe MDL2 Assets"
                      Visibility="{Binding Data.IsFolder, Converter={StaticResource inverseBooleanToVisibilityConverter}}"
                      />

            <FontIcon x:Name ="expandCollapseFolderOutline"
                      Glyph="{Binding IsExpanded, Converter={StaticResource folderOutlineGlyphConverter}}"
                      Foreground="#FFECC849"
                      FontSize="16"
                      Margin="0,8,12,8"
                      FontFamily="Segoe MDL2 Assets"
                      Visibility="{Binding Data.IsFolder, Converter={StaticResource booleanToVisibilityConverter}}"/>
        </Grid>

        <TextBlock Text="{Binding Data.Name}"
                   HorizontalAlignment="Stretch"
                   VerticalAlignment="Center"  
                   FontWeight="Medium"
                   FontFamily="Segoe MDL2 Assests"                           
                   Style="{ThemeResource BodyTextBlockStyle}"/>
    </StackPanel>
</DataTemplate>
```

## <a name="set-up-the-data-in-your-treeview"></a>在树视图中设置数据

下面是用于在树视图示例中设置数据的代码。

```csharp
 public MainPage()
 {
     this.InitializeComponent();

     TreeNode workFolder = CreateFolderNode("Work Documents");
     workFolder.Add(CreateFileNode("Feature Functional Spec"));
     workFolder.Add(CreateFileNode("Feature Schedule"));
     workFolder.Add(CreateFileNode("Overall Project Plan"));
     workFolder.Add(CreateFileNode("Feature Resource allocation"));
     sampleTreeView.RootNode.Add(workFolder);

     TreeNode remodelFolder = CreateFolderNode("Home Remodel");
     remodelFolder.IsExpanded = true;
     remodelFolder.Add(CreateFileNode("Contactor Contact Information"));
     remodelFolder.Add(CreateFileNode("Paint Color Scheme"));
     remodelFolder.Add(CreateFileNode("Flooring woodgrain types"));
     remodelFolder.Add(CreateFileNode("Kitchen cabinet styles"));

     TreeNode personalFolder = CreateFolderNode("Personal Documents");
     personalFolder.IsExpanded = true;
     personalFolder.Add(remodelFolder);

     sampleTreeView.RootNode.Add(personalFolder);
 }

 private static TreeNode CreateFileNode(string name)
 {
     return new TreeNode() { Data = new FileSystemData(name) };
 }

 private static TreeNode CreateFolderNode(string name)
 {
     return new TreeNode() { Data = new FileSystemData(name) { IsFolder = true } };
 }
```

完成上述步骤后，你将获得一个完全填充的树视图/层次结构布局，其中内置了 n 级嵌套、对展开/折叠文件夹和在文件夹之间拖放的支持，以及辅助功能。

若要为用户提供从树视图中添加/删除项目的功能，我们建议你添加上下文菜单以向用户公开这些选项。


## <a name="related-articles"></a>相关文章

- [树视图示例](http://go.microsoft.com/fwlink/?LinkId=785018)
- [**ListView**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx)
- [ListView 和 GridView](listview-and-gridview.md)
