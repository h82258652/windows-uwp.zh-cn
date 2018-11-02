---
author: stevewhims
title: 导航入门
description: 导航入门
ms.assetid: F4DF5C5F-C886-4483-BBDA-498C4E2C1BAF
ms.author: stwhi
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9cb4550a7da3b9b547a1d723d5ae8da260149ba2
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/02/2018
ms.locfileid: "5940161"
---
# <a name="getting-started-navigation"></a>入门：导航


## <a name="adding-navigation"></a>添加导航

iOS 提供 **UINavigationController** 类以帮助应用内导航：可按下和弹出视图，以创建定义应用的**UIViewControllers**层次结构。

相比之下，包含多个视图的 windows 10 应用需要更多网站方法进行导航。 想象一下，用户在单击控件以其自己的方式浏览应用时，在页面间来回跳跃。 有关详细信息，请参阅[导航设计基础知识](https://msdn.microsoft.com/library/windows/apps/dn958438)。

管理 windows 10 应用中的此导航的方法之一是使用[**框架**](https://msdn.microsoft.com/library/windows/apps/br242682)类。 以下演练将向你展示如何尝试执行此操作。

继续使用之前启动的解决方案，打开 **MainPage.xaml** 文件，然后在 **“设计”** 视图中添加按钮。 将该按钮的 **Content** 属性从“Button”更改为“Go To Page”。 然后为按钮的 **Click** 事件创建一个处理程序，如下图所示。 如果忘记了如何执行此操作，可回顾之前部分中的操作实例（提示：双击 **“设计”** 视图中的按钮）。

![在 Visual Studio 中添加按钮及其 Click 事件](images/ios-to-uwp/vs-go-to-page.png)

让我们添加新页面。 在 **“解决方案”** 视图中，点击 **“项目”** 菜单，然后点击 **“添加新项”**。 如下图所示点击 **“空白页”**，然后点击 **“添加”**。

![在 Visual Studio 中添加新页面](images/ios-to-uwp/vs-add-new-page.png)

接下来，向 BlankPage.xaml 文件中添加一个按钮。 我们可以使用 AppBarButton 控件并为它添加一个返回箭头图像：在 **“XAML”** 视图中，在 `<Grid> </Grid>` 元素之间添加 ` <AppBarButton Icon="Back"/>`。

现在，我们可以为按钮添加一个事件处理程序：双击 **“设计”** 视图中的控件，Microsoft Visual Studio 会将文本“AppBarButton\_Click”添加到 **“Click”** 框中（如下图所示），然后在 BlankPage.xaml.cs 文件中添加和显示相应的事件处理程序。

![在 Visual Studio 中添加一个后退按钮及其 Click 事件](images/ios-to-uwp/vs-add-back-button.png)

如果返回到 BlankPage.xaml 文件的 **XAML** 视图，`<AppBarButton>` 元素的 Extensible Application Markup Language (XAML) 代码现在应该与以下内容类似：

` <AppBarButton Icon="Back" Click="AppBarButton_Click"/>`

返回到 BlankPage.xaml.cs 文件，并添加此代码以便在用户点击该按钮后转到上一页。

```csharp
private void AppBarButton_Click(object sender, RoutedEventArgs e)
{
    // Add the following line of code.    
    Frame.GoBack();
}
```

最后，打开 MainPage.xaml.cs 文件并添加此代码。 在用户点击该按钮之后，它将打开 BlankPage。

```csharp
private void Button_Click(object sender, RoutedEventArgs e)
{
    // Add the following line of code.
    Frame.Navigate(typeof(BlankPage1));
}
```

现在运行该程序。 点击“Go To Page”按钮以转到另一页，然后点击后退箭头按钮返回到上一页。

页面导航由 [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682) 类管理。 作为 iOS 中的**UINavigationController**类使用**pushViewController**和**popViewController**方法，适用于 UWP 应用**框架**类提供的[**导航**](https://msdn.microsoft.com/library/windows/apps/br242694)和[**GoBack**](https://msdn.microsoft.com/library/windows/apps/dn996568)方法。 **Frame** 类还有一个名为 [**GoForward**](https://msdn.microsoft.com/library/windows/apps/br242693) 的方法，该方法可以执行可能需要的操作。

此演练在每次导航到 BlankPage 时都会创建它的一个新实例。 （上一实例将被释放或自动*释放*）。 如果不希望每次都创建一个新实例，可将以下代码添加到 BlankPage.xaml.cs 文件中 BlankPage 类的构造函数。 这将启用 [**NavigationCacheMode**](https://msdn.microsoft.com/library/windows/apps/br227506) 行为。

```csharp
public BlankPage()
{
    this.InitializeComponent();
    // Add the following line of code.
    this.NavigationCacheMode = Windows.UI.Xaml.Navigation.NavigationCacheMode.Enabled;
}
```

你还可以获取或设置 **Frame** 类的 [**CacheSize**](https://msdn.microsoft.com/library/windows/apps/br242683) 属性，以管理在导航历史记录中可以缓存多少页面。

有关导航的详细信息，请参阅[导航](https://msdn.microsoft.com/library/windows/apps/mt187344)和 [XAML 个性化动画示例](http://go.microsoft.com/fwlink/p/?LinkID=242401)。

**注意**适用于使用 JavaScript 和 HTML 的 UWP 应用的导航信息，请参阅[快速入门： 使用单页导航](https://msdn.microsoft.com/library/windows/apps/hh452768)。
 
### <a name="next-step"></a>下一步

[入门：动画](getting-started-animation.md)

