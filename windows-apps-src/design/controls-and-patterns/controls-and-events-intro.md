---
Description: You create the UI for your app by using controls such as buttons, text boxes, and combo boxes to display data and get user input. Here, we show you how to add controls to your app.
title: 控件和模式简介
ms.assetid: 64740BF2-CAA1-419E-85D1-42EE7E15F1A5
label: Intro to controls and patterns
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7ff3f89887235fc9c8d9d7afbbdea3d79bace810
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/07/2018
ms.locfileid: "8808742"
---
# <a name="intro-to-controls-and-patterns"></a>控件和模式简介

在 UWP 应用开发中，*控件*是一种显示内容或支持交互的 UI 元素。 通过使用按钮、文本框和组合框等控件来显示数据和获取用户输入，你可以为你的应用创建 UI。

> **重要 API**：[Windows.UI.Xaml.Controls 命名空间](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.aspx)

*模式* 是修改控件或组合多个控件来创造新内容的一种诀窍。 例如，[大纲/细节](master-details.md)模式是一种方法，你可以使用[SplitView](split-view.md)控件，用于应用导航。 同样，你可以自定义实现选项卡模式[NavigationView](navigationview.md)控件的模板。

在许多情况下，你可以按原样使用控件。 但 XAML 控件将函数与结构和外观分离开来，因此你可以进行各种级别的修改来使它们符合你的需求。 在[样式](../style/index.md)部分中，你可以了解如何使用 [XAML 样式](xaml-styles.md)和[控件模板](control-templates.md)来修改控件。

在此部分中，我们为可用于生成应用 UI 的每个 XAML 控件提供指南。 首先，本文介绍如何向应用添加控件。 对应用使用控件有 3 个关键步骤：

- 向应用 UI 添加控件。
- 设置控件的属性，如宽度、高度或前景色。
- 将代码添加到控件的事件处理程序，从而使其执行一些任务。 

## <a name="add-a-control"></a>添加控件
你可以通过多种方式将控件添加到应用：
 
- 使用诸如 Blend for Visual Studio 或 Microsoft Visual Studio Extensible Application Markup Language (XAML) 设计器的设计工具。 
- 在 Visual Studio XAML 编辑器中将控件添加到 XAML 标记中。 
- 在代码中添加控件。 当应用运行时会看到你在代码中添加的控件，但在 Visual Studio XAML 设计器中看不到。

在 Visual Studio 中，当你在应用中添加和操纵控件时，你可以使用许多程序功能，包括“工具箱”、XAML 设计器、XAML 编辑器以及“属性”窗口。 

Visual Studio“工具箱”会显示可在应用中使用的许多控件。 要将控件添加到应用，请在“工具箱”中双击该控件。 例如，如果双击 TextBox 控件，则会将此 XAML 添加到 XAML 视图。 

```xaml
<TextBox HorizontalAlignment="Left" Text="TextBox" VerticalAlignment="Top"/>
```

还可以将控件从“工具箱”拖动到 XAML 设计器。

## <a name="set-the-name-of-a-control"></a>设置控件的名称

若要在代码中使用某个控件，你可以设置其 [x:Name](../../xaml-platform/x-name-attribute.md) 属性并在代码中通过名称来引用该控件。 你可以在 Visual Studio“属性”窗口或 XAML 中设置名称。 下面是通过使用“属性”窗口顶部的“名称”文本框来设置当前选定控件名称的方法。

命名控件
1. 选择要命名的元素。
2. 在“属性”面板中，在“名称”文本框中键入名称。
3. 按 Enter 提交名称。

![Visual Studio 设计器中的 Name 属性](images/add-controls-control-name-designer.png)

下面介绍了如何在 XAML 编辑器中通过添加 x:Name 属性来设置控件的名称。

```xaml
<Button x:Name="Button1" Content="Button"/>
```

## <a name="set-the-control-properties"></a>设置控件属性 

你使用属性来指定控件的外观、内容以及其他属性。 使用设计工具添加控件时，Visual Studio 可能会为你设置某些控制大小、位置和内容的属性。 通过选择和操纵“设计”视图中的控件，你可以更改某些属性，如 Width、Height 或 Margin。 下图显示了“设计”视图中提供的某些大小调整工具。 

![在 Visual Studio 设计器中调整工具大小](images/add-controls-resizing-designer.png)

你可能希望让控件自动调整大小和位置。 这种情况下，你可以重置 Visual Studio 为你设置的大小和位置属性。

重置属性
1. 在“属性”面板中，单击属性值旁边的属性标记。 此时将打开“属性”菜单。
2. 在“属性”菜单中，单击“重置”。

![Visual Studio 属性重置菜单选项](images/add-controls-property-reset.png)

在 XAML 或代码中，你可以通过“属性”窗口设置控件属性。 例如，若要更改 Button 的前景色，你可以设置控件的 Foreground 属性。 下图显示了如何通过使用“属性”窗口中的颜色选取器来设置 Foreground 属性。 

![Visual Studio 设计器中的颜色选取器](images/add-controls-foreground-designer.png)

下面是在 XAML 编辑器中设置 Foreground 属性的方法。 注意打开的 Visual Studio IntelliSense 窗口，该窗口可以帮助你处理语法。 

![在 XAML 第 1 部分中的 Intellisense](images/add-controls-foreground-xaml.png)

![在 XAML 第 2 部分中的 Intellisense](images/add-controls-foreground-xaml-2.png)

下面是设置 Foreground 属性后的 XAML 结果。 

```xaml
<Button x:Name="Button1" Content="Button" 
        HorizontalAlignment="Left" VerticalAlignment="Top"
        Foreground="Beige"/>
```

下面介绍了如何在代码中设置 Foreground 属性。 

```csharp
Button1.Foreground = new SolidColorBrush(Windows.UI.Colors.Beige);
```

## <a name="create-an-event-handler"></a>创建事件处理程序 

每个控件都包含事件，从而使你可以对用户的操作或应用中的其他更改做出响应。 例如，Button 控件包含用户单击 Button 时引发的 Click 事件。 你可以创建一个名为事件处理程序的方法来处理事件。 你可以在 XAML 中或在代码中，将控件的事件与“属性”窗口中的事件处理程序方法相关联。 有关事件的详细信息，请参阅[事件和路由事件概述](../../xaml-platform/events-and-routed-events-overview.md)。

要创建事件处理程序，请选择控件，然后在“属性”窗口的顶部单击“事件”选项卡。 “属性”窗口会列出可供该控件使用的所有事件。 下面是 Button 的一些事件。

![Visual Studio 事件列表](images/add-controls-add-event-designer.png)

要使用默认名称创建事件处理程序，请在“属性”窗口中双击事件名称旁的文本框。 若要使用自定义名称创建事件处理程序，请将你选择的名称输入到文本框中并按 Enter。 随即会创建事件处理程序并在代码编辑器中打开代码隐藏文件。 该事件处理程序方法具有 2 个参数。 第一个参数是 `sender`，它是对处理程序所附加到的对象的引用。 `sender` 参数是 **Object** 类型。 如果你想在 `sender` 对象本身上检查或更改状态，通常需要将 `sender` 强制转换为更精确的类型。 基于你自己的应用设计，你想要一种可将 `sender` 安全转换到其中的类型（基于附加处理程序的位置）。 第二个值是事件数据，它通常在签名中显示为 `e` 或 `args` 参数。

以下代码处理名为 `Button1` 的 Button 的 Click 事件。 当你单击该按钮时，你单击的 Button 的 Foreground 属性将设置为 blue。 

```csharp
private void Button_Click(object sender, RoutedEventArgs e)
{
    Button b = (Button)sender;
    b.Foreground = new SolidColorBrush(Windows.UI.Colors.Blue);
}
```

你也可以在 XAML 中关联事件处理程序。 在 XAML 编辑器中，键入要处理的事件名称。 当你开始输入时，Visual Studio 会显示 IntelliSense 窗口。 指定事件后，你可以在 IntelliSense 窗口中双击 `<New Event Handler>`，从而使用默认名称创建新的事件处理程序，或者从列表中选择一个现有的事件处理程序。 

下面是显示的 IntelliSense 窗口。 它可帮你创建一个新的事件处理程序，或者选择一个现有事件处理程序。

![单击事件的 Intellisense](images/add-controls-add-event-xaml.png)

该示例显示如何在 XAML 中将 Click 事件与名为 Button_Click 的事件处理程序相关联。 

```xaml
<Button Name="Button1" Content="Button" Click="Button_Click"/>
```

你也可以将事件与实际代码中的事件处理程序相关联。 下面是在代码中关联事件处理程序的方法。

```csharp
Button1.Click += new RoutedEventHandler(Button_Click);
```

## <a name="related-topics"></a>相关主题

-   [按功能的控件索引](controls-by-function.md)
-   [Windows.UI.Xaml.Controls 命名空间](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.aspx)
-   [布局](../layout/index.md)
-   [样式](../style/index.md)
-   [可用性](../usability/index.md)
