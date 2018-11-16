---
author: jwmsft
title: “创建用户界面”教程
description: 本文介绍关于在 XAML 中构建用户界面的基础知识
keywords: XAML, UWP, 入门
ms.author: jimwalk
ms.date: 08/30/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 5d54df07cd5f2ccc32098b17fd7c656900cba978
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2018
ms.locfileid: "6849810"
---
# <a name="tutorial-create-a-user-interface"></a>教程：创建用户界面

在本教程中，您将了解如何通过以下方法为图像编辑程序创建基本 UI： 

+ 使用 Visual Studio 中的 XAML 工具（例如 XAML 设计器、工具箱、XAML 编辑器、“属性”面板和文档大纲）将控件和内容添加到你的 UI 中
+ 利用一些最常见的 XAML 布局面板，如 RelativePanel、网格和 StackPanel。

图像编辑程序有两个页面/屏幕：

**主页**：用于显示照片库视图，以及关于每个图像文件的一些信息。

![MainPage](images/xaml-basics/mainpage.png)

**详细信息页**：用于显示选定的单张照片。 利用浮出编辑菜单，可以修改、重命名和保存照片。

![DetailPage](images/xaml-basics/detailpage.png)


## <a name="prerequisites"></a>先决条件

* Visual Studio 2017：[下载 Visual Studio 2017 社区版（免费）](https://www.visualstudio.com/thank-you-downloading-visual-studio/?sku=Community&rel=15&campaign=WinDevCenter&ocid=wdgcx-windevcenter-community-download) 
* Windows 10SDK（10.0.15063.468 或更高版本）：[下载最新的 Windows SDK（免费）](https://developer.microsoft.com/windows/downloads/windows-10-sdk)

## <a name="part-0-get-the-starter-code-from-github"></a>第 0 部分：从 github 获取起始代码

在本教程中，你将从一个简化版的 PhotoLab 示例开始。 

1. 转到[https://github.com/Microsoft/Windows-appsample-photo-lab](https://github.com/Microsoft/Windows-appsample-photo-lab)。 这将让你访问 GitHub 页上的示例。 
2. 接下来，你将需要克隆或下载示例。 单击**克隆或下载**按钮。 一个子菜单将出现。
    <figure>
        <img src="images/xaml-basics/clone-repo.png" alt="The Clone or download menu on GitHub">
        <figcaption>GitHub 页面上的 PhotoLab 示例的“克隆或下载”<b></b>菜单。</figcaption>
    </figure>

    **如果你不熟悉 GitHub:**
    
    a. 单击**下载 ZIP** 并在本地保存文件。 这将下载一个包含你需要的所有项目文件的 .zip 文件。
    b. 将该文件解压缩。 使用文件资源管理器导航到你刚才下载的 .zip 文件，右键单击它，然后选择**全部解压缩…**。c. 导航到示例的本地副本，然后访问 `Windows-appsample-photo-lab-master\xaml-basics-starting-points\user-interface` 目录。    

    **如果你熟悉 GitHub：**

    a. 在本地克隆 repo 的主分支。
    b. 导航到 `Windows-appsample-photo-lab\xaml-basics-starting-points\user-interface` 目录。

3. 通过单击 `Photolab.sln` 打开项目。

## <a name="part-1-add-a-textblock-using-xaml-designer"></a>第 1 部分：使用 XAML 设计器添加 TextBlock

为了使创建 XAML UI 更轻松，Visual Studio 提供了一些工具。 利用 XAML 设计器，你可以将控件拖到设计图面上并查看它们的外观，然后再运行应用。 利用“属性”面板，你可以查看和设置设计器中处于活动状态的所有控件属性。 文档大纲显示了 UI 的 XAML 可视化树的父子结构。 利用 XAML 编辑器，你可以直接输入和修改 XAML 标记。

下面是标记了工具的 Visual Studio UI。

![Visual Studio 布局](images/xaml-basics/visual-studio-tools.png)

利用这些工具中的每个工具，都可以更加轻松地创建你的 UI，因此我们将在本教程中使用所有这些工具。 首先，你将使用 XAML 设计器添加控件。 

**使用 XAML 设计器添加控件：**

1. 在“解决方案资源管理器”中双击“MainPage.xaml”**** 打开它。 这将显示未添加任何 UI 元素的应用主页面。

2. 在执行进一步操作之前，你需要对 Visual Studio 进行一些调整。

    - 请确保将解决方案平台设置为 x86 或 x64，而不是 ARM。
    - 将主页面 XAML 设计器设置为显示 13.3 英寸的桌面预览。

    你应该会在窗口顶部附近看到两个设置，如下所示。

    ![VS 设置](images/xaml-basics/layout-vs-settings.png)

    现在，你可以运行该应用，但不会看到太多内容。 让我们添加一些 UI 元素以使内容变得更有趣。

3. 在工具箱中，展开**常见 XAML 控件**并查找 [TextBlock](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock) 控件。 将 TextBlock 拖到页面左上角附近的设计图面上。

    此 TextBlock 会添加到页面中，并且设计器会根据对你需要的布局的最佳猜测来设置一些属性。 TextBlock 周围会以蓝色高亮显示以指明它现在是活动对象。 请注意设计器添加的边距和其他设置。 你的 XAML 将如下所示。 如果其格式与下面不完全相同，也不用担心；为了更易于阅读，我们在此处进行了缩减。

    ```xaml
    <TextBlock x:Name="textBlock"
               HorizontalAlignment="Left"
               Margin="351,44,0,0"
               TextWrapping="Wrap"
               Text="TextBlock"
               VerticalAlignment="Top"/>
    ```

    在后面的步骤中，你将更新这些值。

4. 在“属性”面板中，将 TextBlock 的“名称”值从 **textBlock** 更改为
**TitleTextBlock**。 （请确保 TextBlock 仍然是活动对象。）

5. 在**通用**下面，将“文本”值更改为**集合**。

    ![TextBlock 属性](images/xaml-basics/text-block-properties.png)

    在 XAML 编辑器中，你的 XAML 现在如下所示。

    ```xaml
    <TextBlock x:Name="TitleTextBlock"
               HorizontalAlignment="Left"
               Margin="351,44,0,0"
               TextWrapping="Wrap"
               Text="Collection"
               VerticalAlignment="Top"/>
    ```

6. 若要定位 TextBlock，应首先删除 Visual Studio 添加的属性值。 在文档大纲中，右键单击 **TitleTextBlock**，然后依次选择**布局 > 重置全部**。

![文档大纲](images/xaml-basics/doc-outline-reset.png)

7. 在“属性”面板内的搜索框中输入 **margin** 可以轻松地查找 **Margin** 属性。 将左边距和下边距设置为 24。

    ![TextBlock 边距](images/xaml-basics/margins.png)

    边距可在页面上对元素进行最基本的定位。 它们可用于微调你的布局，但是使用大边距值（如 Visual Studio 添加的大边距值）会使 UI 难以适应各种屏幕大小，因此应该避免使用大边距值。

    有关详细信息，请参阅[对齐、边距和填充](../layout/alignment-margin-padding.md)。

8. 在“文档大纲”面板中，右键单击“TitleTextBlock”****，然后依次选择“编辑样式”>“应用资源”>“TitleTextBlockStyle”****。 这会对你的标题文本应用系统定义的样式。

    ```xaml
    <TextBlock x:Name="TitleTextBlock"
               TextWrapping="Wrap"
               Text="Collection"
               Margin="24,0,0,24"
               Style="{StaticResource TitleTextBlockStyle}"/>
    ```

9. 在“属性”面板内的搜索框中输入 **textwrapping** 可以查找 **TextWrapping** 属性。 单击 **TextWrapping** 属性的_属性标记_以打开其菜单。 (_属性标记_是每个属性值右侧的小框符号。 _属性标记_为黑色，表示该属性设置为非默认值。）在“属性”**** 菜单上，选择“重置”**** 以重置 TextWrapping 属性。

    Visual Studio 会添加此属性，但你应用的样式中已设置该属性，因此，此处不需要该属性。

你已将 UI 的第一部分添加到你的应用中！ 请立即运行该应用以查看其外观。

你可能已注意到，在 XAML 设计器中，你的应用在黑色背景中显示了白色文本，但当你运行它时，它在白色背景中显示了黑色文本。 这是因为，Windows 具有深色和浅色主题，并且默认主题因设备而异。 在电脑上，默认主题是“浅色”。 你可以单击 XAML 设计器顶部的齿轮图标以打开设备预览设置，并将主题更改为“浅色”以使 XAML 设计器中的应用看上去与电脑上的应用相同。

> [!NOTE]
> 在此部分教程中，你通过拖放添加了控件。 你也可以通过在工具箱中双击来添加控件。 试一下，看看 Visual Studio 生成的 XAML 有哪些区别。

## <a name="part-2-add-a-gridview-control-using-the-xaml-editor"></a>第 2 部分：使用 XAML 编辑器添加 GridView 控件

在第 1 部分中，你尝试了使用 XAML 设计器和 Visual Studio 提供的一些其他工具。 在此，你将使用 XAML 编辑器直接处理 XAML 标记。 随着你对 XAML 的了解越来越多，你可能会发现这是一种更有效的工作方式。

首先，你将根布局 [Grid](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid) 替换为 [**RelativePanel**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.relativepanel)。 利用 RelativePanel，可更加轻松地相对于面板或其他部分 UI 重新排列 UI 块。 你将在 [XAML 自适应布局](xaml-basics-adaptive-layout.md)教程中了解它的用处。 

然后，你将添加 [GridView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.gridview) 控件以显示你的数据。

**使用 XAML 编辑器添加控件**

1. 在 XAML 编辑器中，将根 **Grid** 更改为 **RelativePanel**。

    **之前**
    ```xaml
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
          <TextBlock x:Name="TitleTextBlock"
                     Text="Collection"
                     Margin="24,0,0,24"
                     Style="{StaticResource TitleTextBlockStyle}"/>
    </Grid>
    ```

    **之后**
    ```xaml
    <RelativePanel Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <TextBlock x:Name="TitleTextBlock"
                   Text="Collection"
                   Margin="24,0,0,24"
                   Style="{StaticResource TitleTextBlockStyle}"/>
    </RelativePanel>
    ```

    有关使用 **RelativePanel** 的布局的详细信息，请参阅[布局面板](https://docs.microsoft.com/windows/uwp/layout/layout-panels#relativepanel)。

2. 在 **TextBlock** 元素下面，添加名为“ImageGridView”的 **GridView 控件**。 设置 **RelativePanel** _附加属性_以将此控件放在标题文本下面，并使其横跨整个屏幕宽度。

    **添加以下 XAML**

    ```xaml
    <GridView x:Name="ImageGridView"
              Margin="0,0,0,8"
              RelativePanel.AlignLeftWithPanel="True"
              RelativePanel.AlignRightWithPanel="True"
              RelativePanel.Below="TitleTextBlock"/>
    ```

    **添加到以下 TextBlock 之后**
    ```xaml
    <RelativePanel Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <TextBlock x:Name="TitleTextBlock"
                   Text="Collection"
                   Margin="24,0,0,24"
                   Style="{StaticResource TitleTextBlockStyle}"/>
        
        <!-- Add the GridView here. -->

    </RelativePanel>
    ```

    有关“面板”附加属性的详细信息，请参阅[布局面板](https://docs.microsoft.com/windows/uwp/layout/layout-panels)。

3. 为了让 **GridView** 显示内容，你需要为其提供要显示的数据集。 打开 MainPage.xaml.cs 并查找 **GetItemsAsync** 方法。 此方法会填充一个称为 Images（这是我们已添加到 MainPage 的属性）的集合。

    在 **GetItemsAsync** 中的 **foreach** 循环后面，添加以下代码行。

    ```csharp
    ImageGridView.ItemsSource = Images;
    ```

    这会将 GridView 的 **ItemsSource** 属性设置为应用的 **Images** 集合，并为 **GridView** 提供要显示的内容。

这是运行应用并确保一切正常工作的好地方。 它应该如下所示。

![应用 UI 检查点 1](images/xaml-basics/layout-0.png)

你将注意到应用尚未显示图像。 默认情况下，它将显示集合中的数据类型的 ToString 值。 接下来，你将创建数据模板以定义数据的显示方式。

> [!NOTE]
> 你可以在[布局面板](https://docs.microsoft.com/windows/uwp/layout/layout-panels#relativepanel)文章中找到有关使用 **RelativePanel** 的布局的详细信息。 看一看，然后通过在 **TextBlock** 和 **GridView** 上设置 RelativePanel 附加属性，来实验一些其他布局。

## <a name="part-3-add-a-datatemplate-to-display-your-data"></a>第 3 部分：添加 DataTemplate 以显示你的数据

现在，你将创建 [DataTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.datatemplate)，以告诉 GridView 如何显示你的数据。 有关数据模板的完整说明，请参阅[项容器和模板](../controls-and-patterns/item-containers-templates.md)。

目前，你将仅添加占位符以帮助你创建所需的布局。 在 [XAML 数据绑定](../../data-binding/xaml-basics-data-binding.md)教程中，你将用 **ImageFileInfo** 类中的实际数据替换这些占位符。 如果你想要查看数据对象的外观，现在可以打开 ImageFileInfo.cs 文件。

**将数据模板添加到网格视图**

1. 打开 MainPage.xaml。

2. 若要显示分级，你可以使用 [Telerik 的 UI for UWP](https://github.com/telerik/UI-For-UWP) NuGet 程序包中的 **RadRating** 控件。 添加 XAML 命名空间引用以指定 Telerik 控件的命名空间。 将此项放在左 **Page** 标记中，紧靠在其他“xmlns:”条目后面。

    **添加以下 XAML**

    ```xaml
    xmlns:telerikInput="using:Telerik.UI.Xaml.Controls.Input"
    ```

    **添加到以下最后一个“xmlns:”条目后面**

    ```xaml
    <Page x:Name="page"
      x:Class="PhotoLab.MainPage"
      xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
      xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
      xmlns:local="using:PhotoLab"
      xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
      xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
      xmlns:telerikInput="using:Telerik.UI.Xaml.Controls.Input"
      mc:Ignorable="d"
      NavigationCacheMode="Enabled">
    ```

    有关 XAML 命名空间的详细信息，请参阅 [XAML 命名空间和命名空间映射](https://docs.microsoft.com/windows/uwp/xaml-platform/xaml-namespaces-and-namespace-mapping)。

3. 在“文档大纲”中，右键单击 **ImageGridView**。 在上下文菜单中，选择**编辑其他模板 > 编辑生成的项 (ItemTemplate) > 创建空...**。**创建资源**对话框将会打开。

4. 在此对话框中，将“名称”（键）值更改为 **ImageGridView_DefaultItemTemplate**，然后单击**确定**。

    单击**确定**时，会出现以下几种情况。

    - **DataTemplate** 将添加到 MainPage.xaml 的 Page.Resources 部分。

        ```xaml
        <Page.Resources>
            <DataTemplate x:Key="ImageGridView_DefaultItemTemplate">
                <Grid/>
            </DataTemplate>
        </Page.Resources>
        ```

    - “文档大纲”范围会被设置为 **DataTemplate**。

        创建完数据模板后，你可以单击“文档大纲”左上角中的向上箭头以返回到页面范围。

    - GridView 的 **ItemTemplate** 属性被设置为 **DataTemplate** 资源。

    ```xaml
        <GridView x:Name="ImageGridView"
                  Margin="0,0,0,8"
                  RelativePanel.AlignLeftWithPanel="True"
                  RelativePanel.AlignRightWithPanel="True"
                  RelativePanel.Below="TitleTextBlock"
                  ItemTemplate="{StaticResource ImageGridView_DefaultItemTemplate}"/>
    ```

5. 在 **ImageGridView_DefaultItemTemplate** 资源中，为根 **Grid** 提供一个值为 **300** 的高度和宽度以及一个值为 **8** 的边距。 然后，添加两行，并将第二行的高度设置为 **Auto**。

    **之前**
    ```xaml
    <Grid/>
    ```

    **之后**
    ```xaml
    <Grid Height="300"
          Width="300"
          Margin="8">
        <Grid.RowDefinitions>
            <RowDefinition />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>
    </Grid>
    ```

    有关“网格”布局的详细信息，请参阅[布局面板](https://docs.microsoft.com/windows/uwp/layout/layout-panels#grid)。

6. 将控件添加到网格。

    a. 在第一个网格行中添加 **Image** 控件。 此处将显示图像，但是目前，你将使用应用的应用商店徽标作为占位符。

    b. 添加 **TextBlock** 控件以显示图像的名称、文件类型和尺寸。 为此，你可以使用 **StackPanel** 控件排列文本块。

    有关 **StackPanel** 布局的详细信息，请参阅[布局面板](https://docs.microsoft.com/windows/uwp/layout/layout-panels#stackpanel)

    c. 将 **RadRating** 控件添加到外部（垂直）**StackPanel**。 将其放在内部（水平）**StackPanel** 的后面。

    **最终模板**

    ```xaml
    <Grid Height="300"
          Width="300"
          Margin="8">
        <Grid.RowDefinitions>
            <RowDefinition />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <Image x:Name="ItemImage"
               Source="Assets/StoreLogo.png"
               Stretch="Uniform" />

        <StackPanel Orientation="Vertical"
                    Grid.Row="1">
            <TextBlock Text="ImageTitle"
                       HorizontalAlignment="Center"
                       Style="{StaticResource SubtitleTextBlockStyle}" />
            <StackPanel Orientation="Horizontal"
                        HorizontalAlignment="Center">
                <TextBlock Text="ImageFileType"
                           HorizontalAlignment="Center"
                           Style="{StaticResource CaptionTextBlockStyle}" />
                <TextBlock Text="ImageDimensions"
                           HorizontalAlignment="Center"
                           Style="{StaticResource CaptionTextBlockStyle}"
                           Margin="8,0,0,0" />
            </StackPanel>

            <telerikInput:RadRating Value="3"
                                    IsReadOnly="True">
                <telerikInput:RadRating.FilledIconContentTemplate>
                    <DataTemplate>
                        <SymbolIcon Symbol="SolidStar"
                                    Foreground="White" />
                    </DataTemplate>
                </telerikInput:RadRating.FilledIconContentTemplate>
                <telerikInput:RadRating.EmptyIconContentTemplate>
                    <DataTemplate>
                        <SymbolIcon Symbol="OutlineStar"
                                    Foreground="White" />
                    </DataTemplate>
                </telerikInput:RadRating.EmptyIconContentTemplate>
            </telerikInput:RadRating>

        </StackPanel>
    </Grid>
    ```

现在，运行应用以查看 **GridView** 以及你刚刚创建的项模板。 但是，你可能不会看到分级控件，因为它的白星在白色背景中。 接下来，你将更改背景颜色。

![应用 UI 检查点 2](images/xaml-basics/layout-1.png)

## <a name="part-4-modify-the-item-container-style"></a>第 4 部分：修改项容器样式

项的控件模板包含用于显示状态的视觉对象，例如选择、将指针悬停在上方和对焦。 这些视觉对象呈现在数据模板的顶部或下方。 在此，你将修改控件模板的 **Background** 和 **Margin** 属性，以为 **GridView** 项提供灰色背景。

**修改项容器**

1. 在“文档大纲”中，右键单击 **ImageGridView**。 在上下文菜单中，选择**编辑其他模板 > 编辑生成的项目容器 (ItemContainerStyle) > 编辑副本...**。**创建资源**对话框将会打开。

2. 在此对话框中，将“名称”（键）值更改为 **ImageGridView_DefaultItemContainerStyle**，然后单击**确定**。

    默认样式副本会添加到 XAML 的 **Page.Resources** 部分中。

    ```xaml
    <Style x:Key="ImageGridView_DefaultItemContainerStyle" TargetType="GridViewItem">
        <Setter Property="FontFamily" Value="{ThemeResource ContentControlThemeFontFamily}"/>
        <Setter Property="FontSize" Value="{ThemeResource ControlContentThemeFontSize}"/>
        <Setter Property="Background" Value="{ThemeResource GridViewItemBackground}"/>
        <Setter Property="Foreground" Value="{ThemeResource GridViewItemForeground}"/>
        <Setter Property="TabNavigation" Value="Local"/>
        <Setter Property="IsHoldingEnabled" Value="True"/>
        <Setter Property="HorizontalContentAlignment" Value="Center"/>
        <Setter Property="VerticalContentAlignment" Value="Center"/>
        <Setter Property="Margin" Value="0,0,4,4"/>
        <Setter Property="MinWidth" Value="{ThemeResource GridViewItemMinWidth}"/>
        <Setter Property="MinHeight" Value="{ThemeResource GridViewItemMinHeight}"/>
        <Setter Property="AllowDrop" Value="False"/>
        <Setter Property="UseSystemFocusVisuals" Value="True"/>
        <Setter Property="FocusVisualMargin" Value="-2"/>
        <Setter Property="FocusVisualPrimaryBrush" Value="{ThemeResource GridViewItemFocusVisualPrimaryBrush}"/>
        <Setter Property="FocusVisualPrimaryThickness" Value="2"/>
        <Setter Property="FocusVisualSecondaryBrush" Value="{ThemeResource GridViewItemFocusVisualSecondaryBrush}"/>
        <Setter Property="FocusVisualSecondaryThickness" Value="1"/>
        <Setter Property="Template">
            <Setter.Value>
                <ControlTemplate TargetType="GridViewItem">
                <!-- XAML removed for clarity
                    <ListViewItemPresenter ... />
                -->   
                </ControlTemplate>
            </Setter.Value>
        </Setter>
    </Style>
    ```

    **GridViewItem** 默认样式将设置大量属性。 你始终应该从默认样式的副本开始，同时仅修改必需属性。 否则，视觉对象可能不按预期方式显示，因为某些属性未正确设置。

    像在上一步中一样，GridView 的 **ItemContainerStyle** 属性会设置为新的 **Style** 资源。

    ```xaml
        <GridView x:Name="ImageGridView"
                  Margin="0,0,0,8"
                  RelativePanel.AlignLeftWithPanel="True"
                  RelativePanel.AlignRightWithPanel="True"
                  RelativePanel.Below="TitleTextBlock"
                  ItemTemplate="{StaticResource ImageGridView_DefaultItemTemplate}"
                  ItemContainerStyle="{StaticResource ImageGridView_DefaultItemContainerStyle}"/>
    ```

3. 将 **Background** 属性的值更改为 **Gray**。

    **之前**
    ```xaml
        <Setter Property="Background" Value="{ThemeResource GridViewItemBackground}"/>
    ```

    **之后**
    ```xaml
        <Setter Property="Background" Value="Gray"/>
    ```

4. 将 **Margin** 属性的值更改为 **8**。

    **之前**
    ```xaml
        <Setter Property="Margin" Value="0,0,4,4"/>
    ```

    **之后**
    ```xaml
        <Setter Property="Margin" Value="8"/>
    ```

立即运行应用并查看其外观。 调整应用窗口大小。 **GridView** 负责为你重新排列图像，但是当为某些宽度时，应用窗口右侧会有大量空间。 如果图像居中放置，则看起来更美观。 接下来，我们将执行这项操作。

![应用 UI 检查点 3](images/xaml-basics/layout-2.png)

> [!Note]
> 如果你想要试验一下，请尝试将 Background 和 Margin 属性设置为不同的值，并查看一下效果如何。

## <a name="part-5-apply-some-final-adjustments-to-the-layout"></a>第 5 部分：对布局应用一些最后的调整

若要在页面中居中放置图像，你需要在页面中调整 Grid 的对齐方式。 或者，你需要在 **GridView** 中调整 Images 的对齐方式吗？ 这有关系吗？ 让我们来看一看。

有关对齐的详细信息，请参阅[对齐、边距和填充](../layout/alignment-margin-padding.md)。

（你可能会在这一步中尝试将 **GridView** 的 **Background** 设置为你最喜爱的颜色。 这样，你将可以更清楚地看到布局中发生的情况。）

**修改图像的对齐方式**

1. 在 **Gridview** 中，将 **HorizontalAlignment** 属性设置为 **Center**。

    **之前**
    ```xaml
        <GridView x:Name="ImageGridView"
                  Margin="0,0,0,8"
                  RelativePanel.AlignLeftWithPanel="True"
                  RelativePanel.AlignRightWithPanel="True"
                  RelativePanel.Below="TitleTextBlock"
                  ItemTemplate="{StaticResource ImageGridView_DefaultItemTemplate}"
                  ItemContainerStyle="{StaticResource ImageGridView_DefaultItemContainerStyle}"/>
    ```

    **之后**
    ```xaml
        <GridView x:Name="ImageGridView"
                  Margin="0,0,0,8"
                  RelativePanel.AlignLeftWithPanel="True"
                  RelativePanel.AlignRightWithPanel="True"
                  RelativePanel.Below="TitleTextBlock"
                  ItemTemplate="{StaticResource ImageGridView_DefaultItemTemplate}"
                  ItemContainerStyle="{StaticResource ImageGridView_DefaultItemContainerStyle}" 
                  HorizontalAlignment="Center"/>
    ```

2. 运行应用并调整窗口的大小。 向下滚动以查看更多图像。

    图像居中放置，看上去更美观。 但是，滚动条与 **GridView** 的边缘对齐，而不是与窗口边缘对齐。 若要解决此问题，你可以在 **GridView** 中居中放置图像，而不是在页面中居中放置 **GridView**。 这虽然需要多做一些工作，但最后看起来更美观。

3. 从上一个步骤中删除 **HorizontalAlignment** 设置。

4. 在“文档大纲”中，右键单击 **ImageGridView**。 在上下文菜单中，选择**编辑其他模板 > 编辑项目布局 (ItemsPanel) > 编辑副本...**。**创建资源**对话框将会打开。

5. 在此对话框中，将“名称”（键）值更改为 **ImageGridView_ItemsPanelTemplate**，然后单击**确定**。

    默认 **ItemsPanelTemplate** 副本会添加到 XAML 的 **Page.Resources** 部分中。 （和以前一样，**GridView** 将会更新以引用此资源。）

    ```xaml
    <ItemsPanelTemplate x:Key="ImageGridView_ItemsPanelTemplate">
        <ItemsWrapGrid Orientation="Horizontal" />
    </ItemsPanelTemplate>
    ```

    正如你使用各种面板在应用中布局控件一样，**GridView** 具有一个管理其项布局的内部面板。 现在，你有权访问此面板 (**ItemsWrapGrid**)，可以修改其属性以在 **GridView** 内更改项布局。

6. 在 **ItemsWrapGrid** 中，将 **HorizontalAlignment** 属性设置为 **Center**。

    **之前**
    ```xaml
    <ItemsPanelTemplate x:Key="ImageGridView_ItemsPanelTemplate">
        <ItemsWrapGrid Orientation="Horizontal" />
    </ItemsPanelTemplate>
    ```

    **之后**
    ```xaml
    <ItemsPanelTemplate x:Key="ImageGridView_ItemsPanelTemplate">
        <ItemsWrapGrid Orientation="Horizontal"
                       HorizontalAlignment="Center"/>
    </ItemsPanelTemplate>
    ```

7. 再次运行应用并调整窗口的大小。 向下滚动以查看更多图像。

![应用 UI 检查点 4](images/xaml-basics/layout-3.png)

现在，滚动条与窗口的边缘对齐。 太棒了！ 你已为你的应用创建了基本 UI。

## <a name="going-further"></a>深入探索

既然你已创建基本 UI，请查看以下其他教程（同样基于 PhotoLab 示例）： 

* [XAML 数据绑定教程](../../data-binding/xaml-basics-data-binding.md)中的“添加真实图像和数据”。
* [XAML 自适应布局教程](xaml-basics-adaptive-layout.md)中的“让 UI 适应不同的屏幕大小”。


## <a name="get-the-final-version-of-the-photolab-sample"></a>获取 PhotoLab 示例的最终版本

本教程并不构建完整的照片编辑应用，因此，请务必查看[最终版本](https://github.com/Microsoft/Windows-appsample-photo-lab)以了解其他功能，如自定义动画和手机支持。

