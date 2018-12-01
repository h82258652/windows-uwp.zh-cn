---
description: 熟悉 Visual Studio 环境
title: 熟悉 Visual Studio 环境
ms.assetid: 7FBB50A2-6D22-4082-B333-5153DADDDE9A
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 82cb45dae1a4b9b1a9db8fabc044edf8157f1eb1
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "8342545"
---
# <a name="getting-started-getting-around-in-visual-studio"></a>入门：熟悉 Visual Studio 环境


## <a name="getting-around-in-microsoft-visual-studio"></a>熟悉 Microsof Visual Studio 环境

现在让我们回到先前创建的项目，并且看看如何在 Microsoft Visual Studio 集成开发环境 (IDE) 中进行开发。

如果你是一名 Xcode 开发人员，那应该很熟悉下面的默认视图：左侧窗格中的是源文件，中心窗格中的是编辑器（UI 或源代码），右侧窗格中的是控件及其属性。

![Xcode 开发环境](images/ios-to-uwp/xcode-ide.png)

Microsoft Visual Studio 与此非常相似，只不过默认视图的控件在“工具箱”**** 的左侧。 源文件在右侧的“解决方案资源管理器”**** 中，而属性在“解决方案资源管理器”**** 窗格下的“属性”**** 中，如下所示：

![Visual Studio 开发环境](images/ios-to-uwp/vs-ide.png)

如果你对此感到些许陌生，那么你一定很高兴地知道，在 Visual Studio 中可以重新排列窗格、将源文件放置在屏幕左侧并将工具箱放置在屏幕右侧。 实际上，可单击并拖动任何窗格的标题栏，以将其重新放置，而且 Visual Studio 将显示告诉你标题栏在释放后会停靠在哪里的阴影框。 许多窗格在其标题栏上还有一个细小的绘图固定图标。 此图标可让你按其现在的样子固定面板，即锁定在某个位置。 取消固定窗格，而且它可以折叠以节省空间：如果显示器的尺寸较小，这一点将很有用。 如果扰乱了操作（不必担心，我们都做过这事），请选择“窗口”**** 菜单中的“重置窗口布局”**** 以还原顺序。

## <a name="adding-controls-setting-their-properties-and-responding-to-events"></a>添加控件、设置控件属性并响应事件

现在让我们将一些控件添加到你的项目中。 然后，我们将更改控件的一些属性，并编写一些代码来响应某个控件的事件。

若要在 Xcode 中添加控件，请打开所需 .xib 文件或情节提要，然后按如下所示的方式拖放控件，如“圆角矩形按钮”**** 或“标签”****：

![在 Xcode 中设计 UI](images/ios-to-uwp/xcode-add-button-label.png)

让我们在 Visual Studio 中执行一些类似的操作。 在“工具箱”**** 中，拖动“按钮”**** 控件，然后将其拖放到 MainPage.xaml 文件的设计图面。

对TextBlock**** 控件执行相同操作，使其外观如下所示：

![在 Visual Studio 中设计 UI](images/ios-to-uwp/vs-add-button-label.png)

与 Xcode 在 .xib 或情节提要文件中隐藏布局和绑定信息不同，Visual Studio 鼓励你编辑 XAML 文件（这些文件使用丰富、可编辑、具有声明性、类似于 XML 的语言存储详细信息）。 有关 Extensible Application Markup Language (XAML) 的详细信息，请参阅 [XAML 概述](https://msdn.microsoft.com/library/windows/apps/mt185595)。 目前，请知晓“设计”**** 窗格中显示的所有内容均在“XAML”**** 窗格中定义。 “XAML”**** 窗格使你能够在必要时进行精细控制，并且随着进一步了解，可以快速手动开发用户界面代码。 不过，目前我们仅关注“设计”**** 窗格和“属性”**** 窗格。

让我们来更改按钮的详细信息。 你将知道，若要在 Xcode 中更改按钮名称，可在其属性面板中更改“标题”**** 字段值。

使用 Visual Studio 时，将执行非常相似的操作。 在“设计”**** 窗格中，点击按钮赋予其焦点。 然后在“属性”**** 窗格中，将“内容”**** 值从“按钮”更改为“Press Me”。 接下来，通过将“名称”**** 值从“&lt;No Name&gt;”更改为“myButton”，更新按钮控件名称，如下所示：

![Visual Studio 中的“按钮属性”窗口](images/ios-to-uwp/vs-button-properties.png)

现在让我们写一些代码，以便在用户点击该按钮时将TextBlock**** 控件的内容更改为“Hello, World!” 。

在 Xcode 中，通过编写代码，然后将此代码与控件关联（通常通过将按钮控件拖动至源代码的方法），从而将事件与控件关联，如下所示：

![在 Xcode 中将按钮与事件关联](images/ios-to-uwp/xcode-add-button-event.png)

```swift
// Swift implementation.

@IBAction func buttonPressed(sender: UIButton) {
    
}
```

Visual Studio 与此相似。 在“属性”**** 的右上角是一个闪电按钮。 与所选控件关联的可能事件在此列出，如下所示：

![Visual Studio 中的按钮事件列表](images/ios-to-uwp/vs-button-event.png)

若要为按钮的单击事件添加代码，请先在“设计”**** 窗格中选择按钮。 接下来，单击闪电按钮，然后双击名称“单击”**** 旁边的空白框。 然后，Visual Studio 会将事件“myButton\_Click”添加到“单击”**** 框中，随后在 MainPage.xaml.cs 文件中添加并显示相应事件处理程序，如下所示。

```csharp
private void myButton_Click(object sender, RoutedEventArgs e)
{

}
```

现在让我们关联TextBlock**** 控件。 在 Xcode 中，将按钮控件拖动至源代码文件，以将控件与其定义关联，如下所示。

![在 Xcode 中将标签与其定义关联](images/ios-to-uwp/xcode-add-button-reference.png)

```swift
// Swift implentation.

@IBOutlet weak var myLabel : UILabel
```

在 Visual Studio 中，无需关联控件，因为系统始终会为你执行此操作。 让我们来更改某些属性：

1.  点击“MainPage.xaml 文件”选项卡。
2.  在“设计”**** 窗格中，点击TextBlock**** 控件。
3.  在“属性”**** 窗格中，点击扳手按钮以显示其属性。
4.  在“名称”**** 框中，将“&lt;No Name&gt;”更改为“myLabel”。

![Visual Studio 中的“标签属性”窗口](images/ios-to-uwp/vs-label-properties.png)

现在让我们将一些代码添加到按钮的单击事件。 为此，点击 MainPage.xaml.cs 文件，并将以下代码添加到 myButton\_Click 事件处理程序。

```csharp
private void myButton_Click(object sender, RoutedEventArgs e)
{
    // Add the following line of code.    
    myLabel.Text = "Hello, World!";
}
```

此操作类似于在 Swift 中编写的内容：

```swift
@IBAction func buttonPressed(sender: UIButton) {
    myLabel.text = "Hello, World!"
}
```

最后，若要运行应用，请选择“调试”**** 菜单，然后选择“开始调试”****（或直接按 F5）。 在应用启动之后，单击“点击我”按钮，将看到标签的内容从“TextBlock”更改为“Hello, World!”，如下图所示。

![运行第一个演练的结果：Hello, World!](images/ios-to-uwp/vs-hello-world.png)

要退出该应用，返回到 Visual Studio，请点击“调试”**** 菜单，然后点击“停止调试”****（或者直接按 Shift + F5）。 请注意，Visual Studio 可让你在多种不同的设备上尝试应用，以查看其在每台设备的执行方式。

## <a name="next-step"></a>下一步

[入门：常见控件](getting-started-common-controls.md)

