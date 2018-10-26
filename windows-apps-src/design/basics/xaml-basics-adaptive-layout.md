---
author: muhsinking
title: 创建自适应布局教程
description: 本文介绍有关 XAML 中的自适应布局的基础知识
keywords: XAML, UWP, 入门
ms.author: mukin
ms.date: 08/30/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 000aa2d8f3684aa813b85076d9124a87a71b6a8c
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/26/2018
ms.locfileid: "5563298"
---
# <a name="tutorial-create-adaptive-layouts"></a>教程：创建自适应布局

本教程介绍关于使用 XAML 的自适应和定制布局功能的基础知识，利用这些功能，可以创建外观适用于任何设备的应用。 你将学习如何创建新的 DataTemplate、添加窗口吸附点，以及使用 VisualStateManager 和 AdaptiveTrigger 元素定制应用的布局。 我们将使用这些工具针对较小的设备屏幕优化图像编辑程序。 

你将使用的图像编辑程序有两个页面/屏幕：

**主页**：用于显示照片库视图，以及关于每个图像文件的一些信息。

![MainPage](../basics/images/xaml-basics/mainpage.png)

**详细信息页**：用于显示选定的单张照片。 利用浮出编辑菜单，可以修改、重命名和保存照片。

![DetailPage](../basics/images/xaml-basics/detailpage.png)

## <a name="prerequisites"></a>先决条件

* Visual Studio 2017：[下载 Visual Studio 2017 社区版（免费）](https://www.visualstudio.com/thank-you-downloading-visual-studio/?sku=Community&rel=15&campaign=WinDevCenter&ocid=wdgcx-windevcenter-community-download) 
* Windows 10SDK（10.0.15063.468 或更高版本）：[下载最新的 Windows SDK（免费）](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
* Windows 移动版仿真器：[下载 Windows 10 移动版仿真器（免费）](https://developer.microsoft.com/en-us/windows/downloads/sdk-archive)

## <a name="part-0-get-the-starter-code-from-github"></a>第 0 部分：从 github 获取起始代码

在本教程中，你将从一个简化版的 PhotoLab 示例开始。 

1. 转到[https://github.com/Microsoft/Windows-appsample-photo-lab](https://github.com/Microsoft/Windows-appsample-photo-lab)。 这将让你访问 GitHub 页上的示例。 
2. 接下来，你将需要克隆或下载示例。 单击**克隆或下载**按钮。 一个子菜单将出现。
    <figure>
        <img src="../basics/images/xaml-basics/clone-repo.png" alt="The Clone or download menu on GitHub">
        <figcaption>GitHub 页面上的 PhotoLab 示例的“克隆或下载”<b></b>菜单。</figcaption>
    </figure>

    **如果你不熟悉 GitHub:**
    
    a. 单击**下载 ZIP** 并在本地保存文件。 这将下载一个包含你需要的所有项目文件的 .zip 文件。
    b. 将该文件解压缩。 使用文件资源管理器导航到你刚才下载的 .zip 文件，右键单击它，然后选择**全部解压缩…**。c. 导航到示例的本地副本，然后访问 `Windows-appsample-photo-lab-master\xaml-basics-starting-points\adaptive-layout` 目录。    

    **如果你熟悉 GitHub：**

    a. 在本地克隆 repo 的主分支。
    b. 导航到 `Windows-appsample-photo-lab\xaml-basics-starting-points\adaptive-layout` 目录。

3. 通过单击 `Photolab.sln` 打开项目。

## <a name="part-1-run-the-mobile-emulator"></a>第 1 部分：运行移动版仿真器

在 Visual Studio 工具栏中，确保将解决方案平台设置为 x86 或 x64 而不是 ARM，然后将目标设备从本地计算机更改为你已安装的移动版仿真器之一（例如，5 英寸的 1GB Mobile Emulator 10.0.15063 WVGA）。 请尝试在你通过按 **F5** 选择的移动版仿真器中运行“照片库”应用。

一旦应用启动，你可能就会注意到，应用虽然能够正常工作，但在这么小的视区中看起来不太好看。 动态 Grid 元素通过减少所显示的列数来尝试适应有限的屏幕空间，但是所产生的布局看起来很普通并且不适合这么小的视区。

![移动布局：之后](../basics/images/xaml-basics/adaptive-layout-mobile-before.png)

## <a name="part-2-build-a-tailored-mobile-layout"></a>第 2 部分：生成定制的移动布局
为了使此应用在较小的设备上看起来很美观，我们将在 XAML 页面中创建一组单独的样式，并且仅在检测到移动设备时才使用这组样式。

### <a name="create-a-new-datatemplate"></a>创建新的 DataTemplate
我们将通过为图像创建新的 DataTemplate 来定制应用程序的库视图。 从解决方案资源管理器中打开 MainPage.xaml，并在 **Page.Resources** 标记内添加以下代码。

```XAML
<DataTemplate x:Key="ImageGridView_MobileItemTemplate"
              x:DataType="local:ImageFileInfo">

    <!-- Create image grid -->
    <Grid Height="{Binding ItemSize, ElementName=page}"
          Width="{Binding ItemSize, ElementName=page}">
        
        <!-- Place image in grid, stretching it to fill the pane-->
        <Image x:Name="ItemImage"
               Source="{x:Bind ImagePreview}"
               Stretch="UniformToFill">
        </Image>

    </Grid>
</DataTemplate>
```

此库模板通过消除图像周围的边框、除去每个缩略图下方的图像元数据（文件名、分级等），从而节省屏幕空间。 而我们会将每个缩略图显示为简单的正方形。

### <a name="add-metadata-to-a-tooltip"></a>将元数据添加到工具提示
我们仍然希望用户能够访问每个图像的元数据，因此我们将为每个图像项目添加一个工具提示。 在刚刚创建的 DataTemplate 的 **Image** 标记中添加以下代码。

```XAML
<Image ...>

    <!-- Add a tooltip to the image that displays metadata -->
    <ToolTipService.ToolTip>
        <ToolTip x:Name="tooltip">

            <!-- Arrange tooltip elements vertically -->
            <StackPanel Orientation="Vertical"
                        Grid.Row="1">

                <!-- Image title -->
                <TextBlock Text="{x:Bind ImageTitle, Mode=OneWay}"
                           HorizontalAlignment="Center"
                           Style="{StaticResource SubtitleTextBlockStyle}" />

                <!-- Arrange elements horizontally -->
                <StackPanel Orientation="Horizontal"
                            HorizontalAlignment="Center">

                    <!-- Image file type -->
                    <TextBlock Text="{x:Bind ImageFileType}"
                               HorizontalAlignment="Center"
                               Style="{StaticResource CaptionTextBlockStyle}" />

                    <!-- Image dimensions -->
                    <TextBlock Text="{x:Bind ImageDimensions}"
                               HorizontalAlignment="Center"
                               Style="{StaticResource CaptionTextBlockStyle}"
                               Margin="8,0,0,0" />
                </StackPanel>
            </StackPanel>
        </ToolTip>
    </ToolTipService.ToolTip>
</Image>
```

当你将鼠标悬停在缩略图上（或者长按触摸屏）时，将显示图像的标题、文件类型和尺寸。

### <a name="add-a-visualstatemanager-and-statetrigger"></a>添加 VisualStateManager 和 StateTrigger

我们现在已经为我们的数据创建了新布局，但该应用目前无法知道何时优先于默认样式使用此布局。 为了解决此问题，我们将需要添加 **VisualStateManager**。 将以下代码添加到 **RelativePanel** 页面根元素中。

```XAML
<VisualStateManager.VisualStateGroups>
    <VisualStateGroup>

        <!-- Add a new VisualState for mobile devices -->
        <VisualState x:Key="Mobile">

            <!-- Trigger visualstate when a mobile device is detected -->
            <VisualState.StateTriggers>
                <local:MobileScreenTrigger InteractionMode="Touch" />
            </VisualState.StateTriggers>

        </VisualState>
    </VisualStateGroup>
</VisualStateManager.VisualStateGroups>
```

这会添加一个新的 **VisualState** 和 **StateTrigger**，当应用检测到它在移动设备上运行时将触发它们（可以在 PhotoLab 目录内为你提供的 MobileScreenTrigger.cs 中找到此操作的逻辑）。 当 **StateTrigger** 启动时，该应用将使用分配给此 **VisualState** 的任何布局属性。

### <a name="add-visualstate-setters"></a>添加 VisualState 资源库
接下来，我们将使用 **VisualState** 资源库告诉 **VisualStateManager** 在触发状态时要应用哪些属性。 每个资源库都以特定 XAML 元素的一个属性为目标，并将其设置为给定值。 将以下代码添加到你刚创建的移动版 **VisualState** 中 **VisualState.StateTriggers** 元素的下面。 

```XAML
<VisualStateManager.VisualStateGroups>
    <VisualStateGroup>

        <VisualState x:Key="Mobile">
            ...

            <!-- Add setters for mobile visualstate -->
            <VisualState.Setters>

                <!-- Move GridView about the command bar -->
                <Setter Target="ImageGridView.(RelativePanel.Above)"
                        Value="MainCommandBar" />

                <!-- Switch to mobile layout -->
                <Setter Target="ImageGridView.ItemTemplate"
                        Value="{StaticResource ImageGridView_MobileItemTemplate}" />

                <!-- Switch to mobile container styles -->
                <Setter Target="ImageGridView.ItemContainerStyle"
                        Value="{StaticResource ImageGridView_MobileItemContainerStyle}" />

                <!-- Move command bar to bottom of the screen -->
                <Setter Target="MainCommandBar.(RelativePanel.AlignBottomWithPanel)"
                        Value="True" />
                <Setter Target="MainCommandBar.(RelativePanel.AlignLeftWithPanel)"
                        Value="True" />
                <Setter Target="MainCommandBar.(RelativePanel.AlignRightWithPanel)"
                        Value="True" />

                <!-- Adjust the zoom slider to fit mobile screens -->
                <Setter Target="ZoomSlider.Minimum"
                        Value="80" />
                <Setter Target="ZoomSlider.Maximum"
                        Value="180" />
                <Setter Target="ZoomSlider.TickFrequency"
                        Value="20" />
                <Setter Target="ZoomSlider.Value"
                        Value="100" />
            </VisualState.Setters>

        </VisualState>
    </VisualStateGroup>
</VisualStateManager.VisualStateGroups>

```

这些资源库会将图像库的 **ItemTemplate** 设置为我们在第一部分中创建的新 **DataTemplate**，并将命令栏和缩放滑块与屏幕底部对齐，以便你的拇指能够在手机屏幕上更加轻松地触及它们。

### <a name="run-the-app"></a>运行应用
现在，请尝试使用移动版仿真器运行应用。 是否会成功显示新布局？ 应该会看到较小缩略图的网格，如下所示。 如果你看到的仍然是旧布局，则你的 **VisualStateManager** 代码中可能有拼写错误。

![移动布局：之后](../basics/images/xaml-basics/adaptive-layout-mobile-after.png)

## <a name="part-3-adapt-to-multiple-window-sizes-on-a-single-device"></a>第 3 部分：在单个设备上适应多个窗口大小
创建新定制布局可解决移动设备响应式设计的难题，但是台式机和平板电脑的情况怎样？ 该应用在全屏中可能看起来很美观，但如果用户收缩窗口，则最终可能会出现难看的界面。 通过使用 **VisualStateManager** 在单个设备上适应多个窗口大小，我们可以确保最终用户体验始终感觉良好。

![小窗口：之前](../basics/images/xaml-basics/adaptive-layout-small-before.png)

### <a name="add-window-snap-points"></a>添加窗口吸附点
第一步是定义触发不同 **VisualStates** 所在的“吸附点”。 从解决方案资源管理器中打开 App.xaml，并在 **Application** 标记之间添加以下代码。

```XAML
<Application.Resources>
    <!--  window width adaptive snap points  -->
    <x:Double x:Key="MinWindowSnapPoint">0</x:Double>
    <x:Double x:Key="MediumWindowSnapPoint">641</x:Double>
    <x:Double x:Key="LargeWindowSnapPoint">1008</x:Double>
</Application.Resources>
```

这会为我们提供三个吸附点，利用这些吸附点，我们可以为三个窗口大小范围创建新的 **VisualStates**：
+ 小（宽度为 0 - 640 像素）
+ 中等（宽度为 641 - 1007 像素）
+ 大（宽度大于 1007 像素）

### <a name="create-new-visualstates-and-statetriggers"></a>创建新的 VisualStates 和 StateTriggers
接下来，我们将创建与每个吸附点相对应的 **VisualStates** 和 **StateTriggers**。 在 MainPage.xaml 中，将以下代码添加到在第 2 部分创建的 **VisualStateManager** 中。

```XAML
<VisualStateManager.VisualStateGroups>
    <VisualStateGroup>
    ...

        <!-- Large window VisualState -->
        <VisualState x:Key="LargeWindow">

            <!-- Large window trigger -->
            <VisualState.StateTriggers>
                <AdaptiveTrigger MinWindowWidth="{StaticResource LargeWindowSnapPoint}"/>
            </VisualState.StateTriggers>
     
        </VisualState>

        <!-- Medium window VisualState -->
        <VisualState x:Key="MediumWindow">

            <!-- Medium window trigger -->
            <VisualState.StateTriggers>
                <AdaptiveTrigger MinWindowWidth="{StaticResource MediumWindowSnapPoint}"/>
            </VisualState.StateTriggers>
        
        </VisualState>

        <!-- Small window VisualState -->
        <VisualState x:Key="SmallWindow">

            <!-- Small window trigger -->
            <VisualState.StateTriggers >
                <AdaptiveTrigger MinWindowWidth="{StaticResource MinWindowSnapPoint}"/>
            </VisualState.StateTriggers>

        </VisualState>

    </VisualStateGroup>
</VisualStateManager.VisualStateGroups>
```

### <a name="add-setters"></a>添加资源库
最后，将这些资源库添加至 **SmallWindow** 状态。

```XAML

<VisualState x:Key="SmallWindow">
    ...

    <!-- Small window setters -->
    <VisualState.Setters>

        <!-- Apply mobile itemtemplate and styles -->
        <Setter Target="ImageGridView.ItemTemplate"
                Value="{StaticResource ImageGridView_MobileItemTemplate}" />
        <Setter Target="ImageGridView.ItemContainerStyle"
                Value="{StaticResource ImageGridView_MobileItemContainerStyle}" />

        <!-- Adjust the zoom slider to fit small windows-->
        <Setter Target="ZoomSlider.Minimum"
                Value="80" />
        <Setter Target="ZoomSlider.Maximum"
                Value="180" />
        <Setter Target="ZoomSlider.TickFrequency"
                Value="20" />
        <Setter Target="ZoomSlider.Value"
                Value="100" />
    </VisualState.Setters>

</VisualState>

```

每当视区的宽度小于 641 像素时，这些资源库就会将移动版 **DataTemplate** 和样式应用于桌面应用。 它们还会调整缩放滑块以更好地适应小屏幕。

### <a name="run-the-app"></a>运行应用

在 Visual Studio 工具栏中，将目标设备设置为**本地计算机**，并运行该应用。 加载该应用时，请尝试更改窗口的大小。 当你将窗口缩小为小尺寸时，应该会看到应用切换为你在第 2 部分中创建的移动布局。

![小窗口：之后](../basics/images/xaml-basics/adaptive-layout-small-after.png)

## <a name="going-further"></a>深入探索

现在，你已完成本实验，并且拥有足够的自适应布局知识，可以自行进行进一步的实验。 请尝试向你之前添加的仅限于移动版的工具提示添加评级控件。 或者，作为一项较大的挑战，请尝试为较大的屏幕大小优化布局（考虑电视屏幕或 Surface Studio）

如果遇到问题，可以在[使用 XAML 定义页面布局](../layout/layouts-with-xaml.md)的以下部分中找到更多指南。

+ [视觉状态和状态触发器](https://docs.microsoft.com/en-us/windows/uwp/layout/layouts-with-xaml#visual-states-and-state-triggers)
+ [定制布局](https://docs.microsoft.com/en-us/windows/uwp/layout/layouts-with-xaml#tailored-layouts)

或者，如果你想要了解有关如何构建初始照片编辑应用的详细信息，请查看这些有关 XAML [用户界面](../basics/xaml-basics-ui.md)和[数据绑定](../../data-binding/xaml-basics-data-binding.md)的教程。

## <a name="get-the-final-version-of-the-photolab-sample"></a>获取 PhotoLab 示例的最终版本

本教程并不构建完整的照片编辑应用，因此，请务必查看[最终版本](https://github.com/Microsoft/Windows-appsample-photo-lab)以了解其他功能，如自定义动画和手机支持。