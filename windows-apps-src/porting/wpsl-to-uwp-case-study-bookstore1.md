---
author: stevewhims
ms.assetid: 2b63a4c8-b1c0-4c77-95ab-0b9549ba3c0e
description: 本主题介绍了一个非常简单 WindowsPhone Silverlight 应用移植到 windows 10 通用 Windows 平台 (UWP) 应用的案例研究。
title: WindowsPhone silverlight 移植到 UWP 案例研究： Bookstore1
ms.author: stwhi
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c335f607eb1897f79035850cd6a5af9e7a7a56dc
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/22/2018
ms.locfileid: "7579548"
---
# <a name="windowsphone-silverlight-to-uwp-case-study-bookstore1"></a>WindowsPhone silverlight 移植到 UWP 案例研究： Bookstore1


本主题介绍了一个非常简单 WindowsPhone Silverlight 应用移植到 Windows10Universal Windows 平台 (UWP) 应用的案例研究。 借助 windows 10，你可以创建的单个应用包的可供客户安装到种类广泛的设备，并且我们将在此案例研究中执行的操作。 请参阅 [UWP 应用指南](https://msdn.microsoft.com/library/windows/apps/dn894631)。

我们将移植的应用包含绑定到视图模型的 **ListBox**。 该视图模型具有显示标题、作者和书籍封面的书籍列表。 书籍封面已将**生成操作**设置为**内容**，并将**复制到输出目录**设置为**不要复制**。

本部分中之前的主题介绍了平台之间的差异，并且提供有关将应用的各个方面从 XAML 标记移植到访问数据（通过绑定到视图模型）这一过程的详细信息和指南。 案例研究旨在通过在真实示例中实际显示指南来补充该指南。 案例研究假设你已阅读该指南，因此不会重复该指南。

**注意**时在 Visual Studio 中打开 Bookstore1Universal\_10，如果你看到消息"需要 Visual Studio 更新"，然后按照[TargetPlatformVersion](wpsl-to-uwp-troubleshooting.md)中选择目标平台版本的步骤。

## <a name="downloads"></a>下载

[下载 Bookstore1WPSL8 WindowsPhone Silverlight 应用](http://go.microsoft.com/fwlink/?linkid=517053)。

[下载 Bookstore1Universal\_10 windows 10 应用](http://go.microsoft.com/fwlink/?linkid=532950)。

## <a name="the-windowsphone-silverlight-app"></a>WindowsPhone Silverlight 应用

Bookstore1WPSL8（我们将移植的应用）的外观如下。 它只是一个垂直滚动的书籍列表框，位于应用名称和页面标题下。

![Bookstore1WPSL8 的外观](images/wpsl-to-uwp-case-studies/c01-01-wpsl-how-the-app-looks.png)

## <a name="porting-to-a-windows10-project"></a>移植到 windows 10 项目

可快速完成以下任务：在 Visual Studio 中创建新项目、将文件从 Bookstore1WPSL8 复制到其中并将已复制的文件包含在新项目中。 首先创建一个新的空白应用程序（Windows 通用）项目。 将它命名为 Bookstore1Universal\_10。 这些是要从 Bookstore1WPSL8 复制到 Bookstore1Universal\_10 的文件。

-   复制包含书籍封面图像 PNG 文件的文件夹（该文件夹是 \\Assets\\CoverImages）。 复制该文件夹后，在 **“解决方案资源管理器”** 中，请确保将 **“显示所有文件”** 切换为打开。 右键单击你复制的文件夹，然后单击**包括在项目中**。 该命令的意思是将文件或文件夹“包括”在某个项目中。 每次你复制文件或文件夹时，请在**解决方案资源管理器**中单击**刷新**，然后将文件或文件夹包括在项目中。 无需为你将在目标位置替换的文件执行此操作。
-   复制包含视图模型源文件的文件夹（该文件夹是 \\ViewModel）。
-   复制 MainPage.xaml 并替换目标位置中的文件。

我们可以保留的 App.xaml 和 App.xaml.cs Visual Studio 生成为我们在 windows 10 项目中。

编辑你刚刚复制的源代码和标记文件，并将对 Bookstore1WPSL8 命名空间的任何引用更改为 Bookstore1Universal\_10。 执行此操作的快速方法是使用**在文件中替换**功能。 在视图模型源文件的强制性代码中，需要进行以下移植更改：

-   将 `System.ComponentModel.DesignerProperties` 更改为 `DesignMode`，然后对其使用 **Resolve** 命令。 删除 `IsInDesignTool` 属性并使用 IntelliSense 添加正确的属性名称：`DesignModeEnabled`。
-   对 `ImageSource` 使用 **Resolve** 命令。
-   对 `BitmapImage` 使用 **Resolve** 命令。
-   使用 `System.Windows.Media;` 和 `using System.Windows.Media.Imaging;` 删除。
-   将 **Bookstore1Universal\_10.BookstoreViewModel.AppName** 属性返回的值从“BOOKSTORE1WPSL8”更改为“BOOKSTORE1UNIVERSAL”。

在 MainPage.xaml 中，需要进行以下移植更改：

-   将 `phone:PhoneApplicationPage` 更改为 `Page`（不要忘记出现在属性元素语法中的相应项）。
-   删除 `phone` 和 `shell` 命名空间前缀声明。
-   在其余的命名空间前缀声明中将“clr-namespace”更改为“using”。

如果我们要尽快看到结果，可以选择经济地更正标记编译错误，即使这意味着临时删除标记。 但是，让我们保留通过执行此操作获取的债务记录。 它在此情况下如下所示。

1.  在 **MainPage.xaml** 的根 **Page** 元素中，删除 `SupportedOrientations="Portrait"`。
2.  在 **MainPage.xaml** 的根 **Page** 元素中，删除 `Orientation="Portrait"`。
3.  在 **MainPage.xaml** 的根 **Page** 元素中，删除 `shell:SystemTray.IsVisible="True"`。
4.  在 `BookTemplate` 数据模板中，删除对 `PhoneTextExtraLargeStyle` 和 `PhoneTextSubtleStyle` **TextBlock** 样式的引用。
5.  在 `TitlePanel` **StackPanel** 中，删除对 `PhoneTextNormalStyle` 和 `PhoneTextTitle1Style` **TextBlock** 样式的引用。

让我们先使用适用于移动设备系列的 UI，在这之后我们可以考虑其他外形规格。 现在，你可以生成并运行该应用。 下面是它在移动仿真器上所呈现的外观。

![移动设备上初始源代码发生更改的 UWP 应用](images/wpsl-to-uwp-case-studies/c01-02-mob10-initial-source-code-changes.png)

视图和视图模型正确地协同工作，并且 **ListBox** 处于运行状态。 我们通常只需修复样式设置并使图像显示。

## <a name="paying-off-the-debt-items-and-some-initial-styling"></a>支付债务项和一些初始样式设置

默认情况下，支持所有方向。 WindowsPhone Silverlight 应用将自身明确约束为仅纵向，因此债务项 \#1 # 1 和 \#2 支付通过转到新项目中的应用包清单并选中**纵向**下**支持方向**。

对于此应用，项 \#3 不是债务，因为默认情况下会显示状态栏（以前称为系统托盘）。 对于项 \#4 和 \#5，我们需要找到与已在使用 WindowsPhone Silverlight 样式相对应的四个通用 Windows 平台 (UWP) **TextBlock**样式。 你可以在仿真器中运行 WindowsPhone Silverlight 应用并将其比较[文本](wpsl-to-uwp-porting-xaml-and-ui.md)部分中的图示并行。 通过执行此操作，以及查看 WindowsPhone Silverlight 系统样式的属性，我们可以生成此表。

| Windows Phone Silverlight 样式键 | UWP 样式键          |
|-------------------------------------|------------------------|
| PhoneTextExtraLargeStyle            | TitleTextBlockStyle    |
| PhoneTextSubtleStyle                | SubtitleTextBlockStyle |
| PhoneTextNormalStyle                | CaptionTextBlockStyle  |
| PhoneTextTitle1Style                | HeaderTextBlockStyle   |
 
若要设置这些样式，你可以将它们键入标记编辑器，或者可以使用 Visual Studio XAML 工具设置它们，无需键入任何内容。 若要执行该操作，请右键单击 **TextBlock**，然后单击**编辑样式** &gt; **应用资源**。 若要对项模板中的 **TextBlock** 执行该操作，请右键单击 **ListBox**，然后依次单击**编辑其他模板** &gt; **编辑生成的项 (ItemTemplate)**。

项目后有一个 80% 不透明的白色背景，因为 **ListBox** 控件的默认样式将其背景设置为 `ListBoxBackgroundThemeBrush` 系统资源。 在 **ListBox** 上设置 `Background="Transparent"` 以清除该背景。 若要使项模板中的 **TextBlock** 向左对齐，请采用上述的相同方式再次编辑它，并在两个 **TextBlock** 上都设置 `"9.6,0"` 的 **Margin**。

完成该操作后，由于[更改与视图像素有关](wpsl-to-uwp-porting-xaml-and-ui.md)，因此我们需要检查尚未更改的大小维度（边距、宽度、高度等）并将其乘以 0.8。 例如，因此图像应该从 70x70px 更改为 56x56px。

但是，让我们在显示样式设置的结果前获取要呈现的图像。

## <a name="binding-an-image-to-a-view-model"></a>将图像绑定到视图模型

在 Bookstore1WPSL8 中，我们执行了以下操作：

```csharp
    // this.BookCoverImagePath contains a path of the form "/Assets/CoverImages/one.png".
    return new BitmapImage(new Uri(this.CoverImagePath, UriKind.Relative));
```

在 Bookstore1Universal 中，我们使用 ms-appx [URI 方案](https://msdn.microsoft.com/library/windows/apps/jj655406)。 因此，我们可以使其余的代码保持原样，可以使用 **System.Uri** 构造函数的不同重载将 ms-appx URI 方案放在基 URI 中，并在其中追加路径的其余部分。 如下所示：

```csharp
    // this.BookCoverImagePath contains a path of the form "/Assets/CoverImages/one.png".
    return new BitmapImage(new Uri(new Uri("ms-appx://"), this.CoverImagePath));
```

## <a name="universal-styling"></a>通用样式设置

现在，我们只需要进行一些最终样式设置的调整，并确认应用在桌面设备（和其他）外形规格以及移动设备上外观良好。 具体步骤如下。 并且你可以使用本主题顶部的链接下载这些项目，并查看此处和案例研究末尾之间的所有更改结果。

-   若要缩短项目之间的间距，请在 MainPage.xaml 中查找 `BookTemplate` 数据模板并从根 **Grid** 中删除 `Margin` 属性。
-   如果你想要为页面标题多提供一些空间，你可以在页面标题 **TextBlock** 上将 `-5.6` 的底部边距重置为 `0`。
-   现在，我们需要将 `LayoutRoot` 的 Background 设置为正确的默认值，以便无论使用何种主题，应用在所有设备上运行时都具有合适的外观。 将其从 `"Transparent"` 更改为 `"{ThemeResource ApplicationPageBackgroundThemeBrush}"`。

对于较复杂的应用，这将是我们使用[针对外形规格和用户体验进行移植](wpsl-to-uwp-form-factors-and-ux.md)中的指南的原因，并且真正充分利用现在可运行此类应用的每一种设备外形规格。 但对于此简单应用，我们可以在此处停止操作，并查看该应用在样式设置操作的最后一步中的外观。 实际上，该应用在移动设备和桌面设备上的外观均相同，尽管它并未充分利用较宽的外形规格上的空间（不过，我们将在以后的案例研究中调查如何实现该目的）。

请参阅[主题更改](wpsl-to-uwp-porting-xaml-and-ui.md)以查看如何控制你的应用的主题。

![已移植的 Windows 10 应用](images/w8x-to-uwp-case-studies/c01-07-mob10-ported.png)

在移动设备上运行的已移植的 windows 10 应用

## <a name="an-optional-adjustment-to-the-list-box-for-mobile-devices"></a>对移动设备的列表框的可选调整

当应用在移动设备上运行时，列表框的背景在两种主题下都默认为浅色。 这可能是你喜欢的样式，如果是，则无需执行其他任何操作。 但控件的设计目的是你可以自定义它们的外观，同时使其行为不受影响。 因此，如果你希望列表框在深色主题下显示为深色（这是原始应用的外观），请按照“可选调整”下的[这些说明](w8x-to-uwp-case-study-bookstore1.md)操作。

## <a name="conclusion"></a>总结

此案例研究介绍了移植非常简单的应用（可以认为是一个过分简单的应用）的过程。 例如，列表控件可用于选择或者用于建立导航的上下文；应用导航到具有有关所点击的项的更多详细信息的页面。 根据用户的选择，此特定应用不执行任何操作，并且它没有导航。 即便如此，案例研究仍可用于打破僵局、介绍移植过程以及演示可在真实的 UWP 应用中使用的重要技术。

下一个案例研究是 [Bookstore2](wpsl-to-uwp-case-study-bookstore2.md)，我们将从中了解访问和显示分组数据。
