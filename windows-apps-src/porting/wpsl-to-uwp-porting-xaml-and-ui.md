---
description: 声明性 XAML 标记的形式定义 UI 的做法非常好地将 WindowsPhone Silverlight 转换为通用 Windows 平台 (UWP) 应用。
title: 移植 WindowsPhone Silverlight XAML 和 UI 到 UWP
ms.assetid: 49aade74-5dc6-46a5-89ef-316dbeabbebe
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 153d73a75b48d61cb490a903c6657c42638c6674
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2018
ms.locfileid: "7707138"
---
#  <a name="porting-windowsphone-silverlight-xaml-and-ui-to-uwp"></a>移植 WindowsPhone Silverlight XAML 和 UI 到 UWP



上一主题是[疑难解答](wpsl-to-uwp-troubleshooting.md)。

声明性 XAML 标记的形式定义 UI 的做法非常好地将 WindowsPhone Silverlight 转换为通用 Windows 平台 (UWP) 应用。 你将发现，更新了系统资源键引用、更改了某些元素类型名称并将“clr-namespace”更改为“using”后，标记的一大部分将可兼容。 表示层中的大部分强制性代码（操作 UI 元素的视图模型和代码）也将易于移植。

## <a name="a-first-look-at-the-xaml-markup"></a>XAML 标记一览

上一主题向你展示了如何将 XAML 和代码隐藏复制到新 windows 10 Visual Studio 项目文件。 你可能注意到的最早出现的 Visual Studio XAML 设计器中突出显示的问题之一是，XAML 文件的根中的 `PhoneApplicationPage` 元素对通用 Windows 平台 (UWP) 项目无效。 在上一主题中，你保存创建 windows 10 项目时，Visual Studio 生成的 XAML 文件的副本。 如果你打开该版本的 MainPage.xaml，你将看到根中的类型为 [**Page**](https://msdn.microsoft.com/library/windows/apps/br227503)，此类型在 [**Windows.UI.Xaml.Controls**](https://msdn.microsoft.com/library/windows/apps/br227716) 命名空间中。 因此，你可以将所有 `<phone:PhoneApplicationPage>` 元素更改为 `<Page>`（不要忘记属性元素语法），并且可以删除 `xmlns:phone` 声明。

有关查找对应于 WindowsPhone Silverlight 类型的 UWP 类型的更多常规方法，你可以引用[Namespace 和类映射](wpsl-to-uwp-namespace-and-class-mappings.md)。

## <a name="xaml-namespace-prefix-declarations"></a>XAML 命名空间前缀声明


如果你在视图中使用自定义类型的实例（可能是视图模型实例或值转换器），则 XAML 标记中将具有 XAML 命名空间前缀声明。 它们的语法 WindowsPhone Silverlight 和 UWP 之间有所不同。 下面提供了一些例子：

```xml
    xmlns:ContosoTradingCore="clr-namespace:ContosoTradingCore;assembly=ContosoTradingCore"
    xmlns:ContosoTradingLocal="clr-namespace:ContosoTradingLocal"
```

将“clr-namespace”更改为“using”并删除任何程序集令牌和分号（将推断程序集） 结果如下所示：

```xml
    xmlns:ContosoTradingCore="using:ContosoTradingCore"
    xmlns:ContosoTradingLocal="using:ContosoTradingLocal"
```

你可能具有其类型由系统定义的资源：

```xml
    xmlns:System="clr-namespace:System;assembly=mscorlib"
    /* ... */
    <System:Double x:Key="FontSizeLarge">40</System:Double>
```

在 UWP 中，省略“System”前缀声明并改用（已经声明的）“x”前缀：

```xml
    <x:Double x:Key="FontSizeLarge">40</x:Double>
```

## <a name="imperative-code"></a>强制性代码


视图模型是引用 UI 类型的强制性代码所在的位置之一。 另一个位置是直接操作 UI 元素的任何代码隐藏文件。 例如，你可能发现一行代码（如以下代码）尚未编译：


```csharp
    return new BitmapImage(new Uri(this.CoverImagePath, UriKind.Relative));
```

**BitmapImage**是 WindowsPhone Silverlight，以及通过使用**System.Windows.Media.Imaging**命名空间中，同一文件中的指令允许**BitmapImage** ，而无需如以上代码段中所示的命名空间限定。 在这种情况下，你可以在 Visual Studio 中右键单击类型名称 (**BitmapImage**) 并在上下文菜单上使用 **Resolve** 命令以将新的命名空间指令添加到该文件。 在此情况下，添加 [**Windows.UI.Xaml.Media.Imaging**](https://msdn.microsoft.com/library/windows/apps/br243258) 命名空间，这是该类型在 UWP 中所处的位置。 你可以删除 **System.Windows.Media.Imaging** using 指令，这是在以上代码段中移植类似代码所需的全部操作。 完成后，你将已经删除所有 WindowsPhone Silverlight 命名空间。

在像这样将旧命名空间中的类型映射到新命名空间中的相同类型的简单案例中，可以使用 Visual Studio 的 **Find and Replace** 命令对源代码进行批量更改。 **Resolve** 命令是发现类型的新命名空间的绝佳方法。 作为另一个示例，你可以将所有“System.Windows”替换为“Windows.UI.Xaml”。 这实质上将移植所有引用该命名空间的 using 指令和完全限定的类型名称。

删除所有旧的 using 指令并添加新指令后，可以使用 Visual Studio 的 **Organize Usings** 命令为指令排序并删除未使用的指令。

有时只需修复少量的强制性代码，例如更改参数类型。 其他情况下，你将需要而不是.NET Api 为 Windows 运行时 8.x 应用使用 UWP Api。 若要标识哪些 Api 受支持，请使用此移植指南的其余部分与[.NET 为 Windows 运行时 8.x 应用的概述](https://msdn.microsoft.com/library/windows/apps/xaml/br230302.aspx)和[Windows 运行时参考](https://msdn.microsoft.com/library/windows/apps/br211377)相结合。

并且，如果你希望转到项目构建阶段，可以注释或去掉任何非必要的代码。 然后一次一个问题进行迭代，并参考本部分中的以下主题（和上一个主题：[疑难解答](wpsl-to-uwp-troubleshooting.md)），直到消除所有构建和运行时问题并且完成移植。

## <a name="adaptiveresponsive-ui"></a>自适应/响应式 UI

因为 windows 10 应用可以在种类可能很广泛的设备上运行，每个都有其自己的屏幕大小和分辨率 — 你会想要超出移植你的应用的最少步骤，你将需要定制你的 UI 以使其最佳这些设备上的外观。 你可以使用自适应视觉状态管理器功能来动态检测窗口大小并更改布局作为响应，还可以使用 Bookstore2 案例研究主题中的[自适应 UI](wpsl-to-uwp-case-study-bookstore2.md) 部分中所示的有关如何执行此操作的示例。

## <a name="alarms-and-reminders"></a>警告和提醒

应该移植使用 **Alarm** 或 **Reminder** 类的代码，以使用 [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) 类创建和注册后台任务，并在相关的时间显示 Toast。 请参阅[后台处理](wpsl-to-uwp-business-and-data.md)和 [Toast](#toasts)。

## <a name="animation"></a>动画

作为关键帧动画和 from/to 动画的首选替代方法，UWP 动画库对 UWP 应用可用。 为了运行流畅、外观美观以及使你的应用看起来像内置应用一样与 Windows 集成，这些动画已经过了设计和微调。 请参阅[快速入门：使用库动画创建 UI 动画](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)。

如果你确实在 UWP 应用中使用关键帧动画或 from/to 动画，则可能希望了解新的平台所引入的独立动画和从属动画之间的区别。 请参阅[优化动画和媒体](https://msdn.microsoft.com/library/windows/apps/mt204774)。 在 UI 线程上运行的动画（如对布局属性进行动画处理的动画）被称为从属动画，在新的平台上运行时，它们将没有效果，除非你执行以下两项操作之一。 你可以将它们重定目标为对不同的属性进行动画处理（例如 [**RenderTransform**](https://msdn.microsoft.com/library/windows/apps/br208980)），从而使它们独立。 或者可以在动画元素上设置 `EnableDependentAnimation="True"` 以确认你要运行无法保证流畅运行的动画的意图。 如果你使用 Blend 或 Visual Studio 创作新的动画，那么将在必要时为你设置该属性。

## <a name="back-button-handling"></a>后退按钮处理

在 windows 10 应用中，你可以使用单个方法处理后退按钮，并且它将适用于所有设备。 在移动设备上，该按钮作为设备上的电容性按钮或外壳中的按钮向你提供。 在桌面设备上，只要你的应用内可进行后退导航，你便可以向该应用的镶边添加一个按钮，它将显示在窗口化的应用的标题栏中或平板电脑模式下的任务栏中。 后退按钮事件是所有设备系列的通用概念，并且硬件或软件中实现的按钮会引发相同的 [**BackRequested**](https://msdn.microsoft.com/library/windows/apps/dn893596) 事件。

以下示例适用于所有设备系列，并且对将相同的处理操作应用于所有页面的情形以及不需要确认导航的情形（例如，就未保存的更改发出警告）十分有用。

```csharp
   // app.xaml.cs

    protected override void OnLaunched(LaunchActivatedEventArgs e)
    {
        [...]

        Windows.UI.Core.SystemNavigationManager.GetForCurrentView().BackRequested += App_BackRequested;
        rootFrame.Navigated += RootFrame_Navigated;
    }

    private void RootFrame_Navigated(object sender, NavigationEventArgs e)
    {
        Frame rootFrame = Window.Current.Content as Frame;

        // Note: On device families that have no title bar, setting AppViewBackButtonVisibility can safely execute 
        // but it will have no effect. Such device families provide a back button UI for you.
        if (rootFrame.CanGoBack)
        {
            Windows.UI.Core.SystemNavigationManager.GetForCurrentView().AppViewBackButtonVisibility = 
                Windows.UI.Core.AppViewBackButtonVisibility.Visible;
        }
        else
        {
            Windows.UI.Core.SystemNavigationManager.GetForCurrentView().AppViewBackButtonVisibility = 
                Windows.UI.Core.AppViewBackButtonVisibility.Collapsed;
        }
    }

    private void App_BackRequested(object sender, Windows.UI.Core.BackRequestedEventArgs e)
    {
        Frame rootFrame = Window.Current.Content as Frame;

        if (rootFrame.CanGoBack)
        {
            rootFrame.GoBack();
        }
    }
```

还有一个适用于所有设备系列的以编程方式退出应用的方法。

```csharp
   Windows.UI.Xaml.Application.Current.Exit();
```

## <a name="binding-and-compiled-bindings-with-xbind"></a>绑定，并且使用 {x:Bind} 编译绑定

绑定的主题包括：

-   将 UI 元素绑定到“数据”（即，视图模型的属性和命令）
-   将 UI 元素绑定到另一个 UI 元素
-   编写可观察的视图模型（即，当属性值更改时和命令的可用性更改时它将引发通知）。

所有这些方面大部分仍受支持，但是有命名空间差异。 例如，**System.Windows.Data.Binding** 映射到 [**Windows.UI.Xaml.Data.Binding**](https://msdn.microsoft.com/library/windows/apps/br209820)、**System.ComponentModel.INotifyPropertyChanged** 映射到 [**Windows.UI.Xaml.Data.INotifyPropertyChanged**](https://msdn.microsoft.com/library/windows/apps/br209899)、**System.Collections.Specialized.INotifyPropertyChanged** 映射到 [**Windows.UI.Xaml.Interop.INotifyCollectionChanged**](https://msdn.microsoft.com/library/windows/apps/hh702001)。

WindowsPhone Silverlight 应用栏和应用栏按钮无法像在 UWP 应用中一样绑定。 你可能具有强制性代码，用于构造应用栏及其按钮、将它们绑定到属性和本地化字符串并处理其事件。 如果有，你现在可以选择移植该强制性代码，方法是将其替换为绑定到属性和命令的声明性标记以及静态资源引用，从而逐渐增加你的应用的安全性和可维护性。 你可以使用 Visual Studio 或 Blend for Visual Studio 绑定 UWP 应用栏按钮并为其设置样式，就像任何其他 XAML 元素一样。 请注意，在 UWP 应用中，你使用的类型名称是 [**CommandBar**](https://msdn.microsoft.com/library/windows/apps/dn279427) 和 [**AppBarButton**](https://msdn.microsoft.com/library/windows/apps/dn279244)。

UWP 应用的绑定相关的功能当前具有以下限制：

-   没有对数据输入验证以及 [**IDataErrorInfo**](https://msdn.microsoft.com/library/system.componentmodel.idataerrorinfo.aspx) 和 [**INotifyDataErrorInfo**](https://msdn.microsoft.com/library/system.componentmodel.inotifydataerrorinfo.aspx) 接口的内置支持。
-   [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820)类不包括在 WindowsPhone Silverlight 中可用的扩展格式属性。 不过，你仍然可以实现 [**IValueConverter**](https://msdn.microsoft.com/library/windows/apps/br209903) 以提供自定义格式。
-   [**IValueConverter**](https://msdn.microsoft.com/library/windows/apps/br209903) 方法将语言字符串视为参数而不是 [**CultureInfo**](https://msdn.microsoft.com/library/system.globalization.cultureinfo.aspx) 对象。
-   [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/br209833) 类不内置支持排序和筛选以及对工作执行差异化分组。 有关详细信息，请参阅[深度数据绑定](https://msdn.microsoft.com/library/windows/apps/mt210946)和[数据绑定示例](http://go.microsoft.com/fwlink/p/?linkid=226854)。

尽管相同的绑定功能仍然大部分受支持，但 windows 10 提供的新选项和性能更佳绑定机制，称为编译绑定，它使用 {x: Bind} 标记扩展。 请参阅[数据绑定：通过对 XAML 数据绑定的新增功能提升应用性能](http://channel9.msdn.com/Events/Build/2015/3-635)和 [x:Bind 示例](http://go.microsoft.com/fwlink/p/?linkid=619989)。

## <a name="binding-an-image-to-a-view-model"></a>将图像绑定到视图模型

你可以将 [**Image.Source**](https://msdn.microsoft.com/library/windows/apps/br242760) 属性绑定到属于类型 [**ImageSource**](https://msdn.microsoft.com/library/windows/apps/br210107) 的视图模型的任何属性。 下面是此类属性的 WindowsPhone Silverlight 应用中的典型实现：

```csharp
    // this.BookCoverImagePath contains a path of the form "/Assets/CoverImages/one.png".
    return new BitmapImage(new Uri(this.CoverImagePath, UriKind.Relative));
```

在 UWP 应用中，使用 ms-appx [URI 方案](https://msdn.microsoft.com/library/windows/apps/jj655406)。 因此，你可以使其余的代码保持原样，还可以使用 **System.Uri** 构造函数的不同重载将 ms-appx URI 方案放置在基 URI 中并将路径的其余部分附加到其中。 如下所示：

```csharp
    // this.BookCoverImagePath contains a path of the form "/Assets/CoverImages/one.png".
    return new BitmapImage(new Uri(new Uri("ms-appx://"), this.CoverImagePath));
```

这样，视图模型的其余部分、图像路径属性中的路径值以及 XAML 标记中的绑定可以全部保持原样。

## <a name="controls-and-control-stylestemplates"></a>控件和控件样式/模板

WindowsPhone Silverlight 应用使用**Microsoft.Phone.Controls**命名空间和**System.Windows.Controls**命名空间中定义的控件。 XAML UWP 应用使用 [**Windows.UI.Xaml.Controls**](https://msdn.microsoft.com/library/windows/apps/br227716) 命名空间中定义的控件。 体系结构和 UWP 中的 XAML 控件的设计实质上是与 WindowsPhone Silverlight 控件相同。 但是，进行了一些更改以改进可用的控件组并将它们与 Windows 应用统一。 以下是具体示例。

| 控件名称 | 更改 |
|--------------|--------|
| ApplicationBar | [Page.TopAppBar](https://msdn.microsoft.com/library/windows/apps/hh702575) 属性。 |
| ApplicationBarIconButton | UWP 等效项是 [Glyph](https://msdn.microsoft.com/library/windows/apps/dn279538) 属性。 PrimaryCommands 是 CommandBar 的内容属性。 XAML 分析器将元素的内部 xml 解释为其内容属性的值。 |
| ApplicationBarMenuItem | UWP 等效项是设置为菜单项文本的 [AppBarButton.Label](https://msdn.microsoft.com/library/windows/apps/dn279261)。 |
| ContextMenu（在 Windows Phone 工具包中） | 对于单选浮出控件，请使用 [Flyout](https://msdn.microsoft.com/library/windows/apps/dn279496)。 |
| ControlTiltEffect.TiltEffect 类 | UWP 动画库中的动画内置于常用控件的默认样式中。 请参阅[创建指针操作动画](https://msdn.microsoft.com/library/windows/apps/xaml/jj649432)。 |
| 带有分组数据的 LongListSelector | WindowsPhone Silverlight longlistselector 以两种方法，可以配合使用。 第一，它可以显示按某个键分组的数据，例如按首字母分组的名称列表。 第二，它可以在两个语义视图之间“缩放”：项（例如名称）的分组列表和只有组键（例如首字母）本身的列表。 借助 UWP，你可以按照[列表和网格视图控件指南](https://msdn.microsoft.com/library/windows/apps/mt186889)进行操作来显示分组数据。 |
| 带有平面数据的 LongListSelector | 出于性能原因，在很长列表时，我们建议而不是 WindowsPhone Silverlight LongListSelector 列表框中，即使对于平面、 非分组的数据。 在 UWP 应用中，[GridView](https://msdn.microsoft.com/library/windows/apps/br242705) 优先用于项的长列表，无论数据是否适合分组。 |
| Panorama | 中心控件中将 WindowsPhone Silverlight Panorama 控件映射到[的 Windows 运行时 8.x 应用中的中心控件指南](https://msdn.microsoft.com/library/windows/apps/dn449149)和指南。 <br/> 请注意，Panorama 控件从最后一部分环绕到第一部分，并且其背景图像相对于具体部分在视差中移动。 [Hub](https://msdn.microsoft.com/library/windows/apps/dn251843) 部分不会环绕，并且不使用视差。 |
| Pivot | WindowsPhone Silverlight Pivot 控件的 UWP 等效项是[Windows.UI.Xaml.Controls.Pivot](https://msdn.microsoft.com/library/windows/apps/dn608241)。 它适用于所有设备系列。 |

**注意** PointerOver 视觉状态是与中自定义样式/模板在 windows 10 应用中，而非 WindowsPhone Silverlight 应用。 存在其他原因，你现有的自定义样式/模板可能适用于 windows 10 应用，包括你正在使用的系统资源键集的视觉状态和对 windows 10 默认样式所做的性能改进的更改 /模板。 我们建议为 windows 10 编辑一个控件默认模板的全新副本，然后重新应用你的样式和模板的自定义的。

有关 UWP 控件的详细信息，请参阅[按功能列出的控件](https://msdn.microsoft.com/library/windows/apps/mt185405)、[控件列表](https://msdn.microsoft.com/library/windows/apps/mt185406)和[控件指南](https://msdn.microsoft.com/library/windows/apps/dn611856)。

##  <a name="design-language-in-windows10"></a>在 windows 10 设计语言

存在一些在设计语言中 WindowsPhone Silverlight 应用和 windows 10 应用之间的差异。 有关所有详细信息，请参阅[设计](http://dev.windows.com/design)。 不考虑设计语言更改，我们的设计原则始终保持一致：关注细节却又力求简洁（专注于内容而不是外观），显著减少视觉元素，始终忠实于数字领域；使用可视化层次结构（尤其是版式）；基于网格进行设计；通过流畅的动画带给你生动的体验。

## <a name="localization-and-globalization"></a>本地化和全球化

对于本地化字符串，你可以重新使用.resx 文件从 WindowsPhone Silverlight 项目在 UWP 应用项目中。 复制文件，将其添加到项目，并将其重命名为 Resources.resw，以便查找机制默认找到它。 将 **“生成操作”** 设置为**PRIResource**并将 **“复制到输出目录”** 设置为 **“不要复制”**。 然后你可以通过在 XAML 元素上指定 **“x:Uid”** 属性来使用标记中的字符串。 请参阅[快速入门：使用字符串资源](https://msdn.microsoft.com/library/windows/apps/xaml/hh965329)。

WindowsPhone Silverlight 应用使用**CultureInfo**类帮助实现应用全球化。 UWP 应用使用 MRT（现代资源技术），此技术支持在运行时和在 Visual Studio 设计图面中动态加载应用资源（本地化、比例和主题）。 有关详细信息，请参阅[文件、数据和全球化指南](https://msdn.microsoft.com/library/windows/apps/dn611859)。

[**ResourceContext.QualifierValues**](https://msdn.microsoft.com/library/windows/apps/br206071) 主题介绍了如何基于设备系列资源选择规格加载特定于设备系列的资源。

## <a name="media-and-graphics"></a>媒体和图形

当你阅读到有关 UWP 媒体和图形的内容时，请记住 Windows 设计准则鼓励尽量减少任何多余的内容，包括图形的复杂度和混乱度。 Windows 设计以整洁清晰的视觉效果、版式和动作为特征。 如果你的应用遵循相同的准则，那么它看起来将更像内置应用。

WindowsPhone Silverlight 具有**RadialGradientBrush**类型不是在 UWP 中，存在尽管其他[**画笔**](/uwp/api/Windows.UI.Xaml.Media.Brush)类型。 在某些情况下，你将可以使用位图获得类似的效果。 请注意，你可以使用 [Microsoft DirectX](https://msdn.microsoft.com/library/windows/desktop/ee663274) 中的 Direct2D 和 XAML C++ UWP [创建径向渐变画笔](https://msdn.microsoft.com/library/windows/desktop/dd756679)。

WindowsPhone Silverlight 具有**System.Windows.UIElement.OpacityMask**属性，但该属性不是 UWP [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911)类型的成员。 在某些情况下，你将可以使用位图获得类似的效果。 并且你可以使用 [Microsoft DirectX](https://msdn.microsoft.com/library/windows/desktop/ee663274) 中的 Direct2D 和 XAML C++ UWP 应用[创建不透明蒙板](https://msdn.microsoft.com/library/windows/desktop/ee329947)。 但是，**OpacityMask** 的常见用例是使用适应浅色和深色主题的单个位图。 对于矢量图形，你可以使用与主题有关的系统画笔（例如下面所示的饼图）。 但是，若要创建与主题有关的位图（例如下面所示的复选标记），则需要使用其他方法。

![与主题有关的位图](images/wpsl-to-uwp-case-studies/wpsl-to-uwp-theme-aware-bitmap.png)

在 WindowsPhone Silverlight 应用的技术是以用作 alpha 蒙板 （采用位图形式） **OpacityMask** **矩形**填充的前景画笔：

```xml
    <Rectangle Fill="{StaticResource PhoneForegroundBrush}" Width="26" Height="26">
        <Rectangle.OpacityMask>
            <ImageBrush ImageSource="/Assets/wpsl_check.png"/>
        </Rectangle.OpacityMask>
    </Rectangle>
```

将它移植到 UWP 应用的最简单的方法是使用 [**BitmapIcon**](https://msdn.microsoft.com/library/windows/apps/dn279306)，如下所示：

```xml
    <BitmapIcon UriSource="Assets/winrt_check.png" Width="21" Height="21"/>
```

此处，winrt\_check.png 是采用位图形式的 alpha 蒙板，就像 wpsl\_check.png 一样，并且它完全可以是同一个文件。 但是，你可能会需要提供多个不同大小的 winrt\_check.png 以用于不同的比例系数。 有关详细信息和对 **Width** 和 **Height** 值进行更改的说明，请参阅本主题中的[视图或有效像素、观看距离和比例系数](#view-or-effective-pixels-viewing-distance-and-scale-factors)。

较常规的方法（适用于位图的浅色和深色主题之间有差异的情况）是使用两个图像资源：一个带有深色前景（用于浅色主题），而另一个带有浅色前景（用于深色主题）。 有关如何这组位图资源命名的详细信息，请参阅[定制语言、 比例和其他限定符的资源](../app-resources/tailor-resources-lang-scale-contrast.md)。 为一组图像文件正确命名后，你可以使用它们的根名称在摘要中引用它们，如下所示：

```xml
    <Image Source="Assets/winrt_check.png" Stretch="None"/>
```

在 WindowsPhone Silverlight **UIElement.Clip**属性可以是任何形状，你可以使用**几何**表示，并通常在 XAML 标记中**StreamGeometry**小型语言序列化。 在 UWP 中，[**Clip**](https://msdn.microsoft.com/library/windows/apps/br208919) 属性的类型是 [**RectangleGeometry**](https://msdn.microsoft.com/library/windows/apps/br210259)，以便你可以只剪裁矩形区域。 允许使用小型语言定义矩形这一做法过于宽松。 因此，若要在标记中移植裁剪区域，请替换 **Clip** 属性语法并使其成为与以下内容类似的属性元素语法：

```xml
    <UIElement.Clip>
        <RectangleGeometry Rect="10 10 50 50"/>
    </UIElement.Clip>
```

请注意，你可以凭借 [Microsoft DirectX](https://msdn.microsoft.com/library/windows/desktop/ee663274) 中的 Direct2D 和 XAML C++ UWP 应用[使用任意几何图形作为层中的蒙板](https://msdn.microsoft.com/library/windows/desktop/dd756654)。

## <a name="navigation"></a>导航

当导航到的 WindowsPhone Silverlight 应用中的页面时，你将使用统一资源标识符 (URI) 寻址方案：

```csharp
    NavigationService.Navigate(new Uri("/AnotherPage.xaml", UriKind.Relative)/*, navigationState*/);
```

在 UWP 应用中，调用 [**Frame.Navigate**](https://msdn.microsoft.com/library/windows/apps/br242694) 方法并指定目标页面的类型（如该页面的 XAML 标记定义的 **x:Class** 属性中所述）：


```csharp
    // In a page:
    this.Frame.Navigate(typeof(AnotherPage)/*, parameter*/);

    // In a view model, perhaps inside an ICommand implementation:
    var rootFrame = Windows.UI.Xaml.Window.Current.Content as Windows.UI.Xaml.Controls.Frame;
    rootFrame.Navigate(typeof(AnotherPage)/*, parameter*/);
```

在 WMAppManifest.xml 中定义的 WindowsPhone Silverlight 应用的启动页面：

```xml
    <DefaultTask Name="_default" NavigationPage="MainPage.xaml" />
```

在 UWP 应用中，使用强制性代码定义启动页面。 下面是 App.xaml.cs 中一些演示方法的代码：

```csharp
    if (!rootFrame.Navigate(typeof(MainPage), e.Arguments))
```

URI 映射和片段导航是 URI 导航技术，因此它们不适用于 UWP 导航，后者不基于 URI。 URI 映射的存在是为了响应使用 URI 字符串标识目标页面的弱类型性质，如果页面移动到不同的页面，从而移动到不同的相对路径，此性质可能导致易坏性和可维护性问题。 UWP 应用使用基于类型的导航，它是强类型且经过编译器检查的，因此没有 URI 映射所解决的问题。 片段导航的用例是将一些上下文一起传递到目标页面，以便该页面可以使其内容的特定片段显示或者滚动到视图中。 调用 [**Navigate**](https://msdn.microsoft.com/library/windows/apps/br242694) 方法时，可以通过传递导航参数实现相同的目标。

有关详细信息，请参阅[导航](https://msdn.microsoft.com/library/windows/apps/mt187344)。

## <a name="resource-key-reference"></a>资源键引用

设计语言已针对 windows 10 进行开展，因此某些系统样式已发生更改，并已删除或重命名许多系统资源键。 Visual Studio 中的 XAML 标记编辑器突出显示对无法解析的资源键的引用。 例如，XAML 标记编辑器将使用红色波形曲线为对样式键 `PhoneTextNormalStyle` 的引用加下划线。 如果未更正该错误，则应用将在你尝试将其部署到模拟器或设备时立即终止。 因此，请务必留意 XAML 标记的正确性。 而且你将发现 Visual Studio 是捕获此类问题的绝佳工具。

另请参阅下面的[文本](#text)。

## <a name="status-bar-system-tray"></a>状态栏（系统托盘）

系统托盘(使用 `shell:SystemTray.IsVisible` 在 XAML 标记中进行设置）现在称为状态栏，并默认处于显示状态。 你可以在强制性代码中通过调用 [**Windows.UI.ViewManagement.StatusBar.ShowAsync**](https://msdn.microsoft.com/library/windows/apps/dn610343) 和 [**HideAsync**](https://msdn.microsoft.com/library/windows/apps/dn610339) 方法控制其可见性。

## <a name="text"></a>文本

文本（或版式）是 UWP 应用的重要方面，并且在移植时，你可能希望回顾你的视图的视觉设计，以便它们与新设计语言相协调。 使用这些图示查找可用的 UWP **TextBlock** 系统样式。 查找对应于你使用的 WindowsPhone Silverlight 样式的样式。 或者，你可以创建你自己的通用样式并复制到这些 WindowsPhone Silverlight 系统样式的属性。

![适用于 Windows 10 应用的 TextBlock 系统样式](images/label-uwp10stylegallery.png)

Windows 10 应用的 TextBlock 系统样式

在 WindowsPhone Silverlight 应用中，默认字体系列是 Segoe WP。 在 windows 10 应用中，默认字体系列是 Segoe UI。 因此，你的应用中的字体指标可能看起来不同。 如果你想要重现 WindowsPhone Silverlight 文本的外观，可以设置你自己的指标使用[**LineHeight**](https://msdn.microsoft.com/library/windows/apps/br209671)和[**LineStackingStrategy**](https://msdn.microsoft.com/library/windows/apps/br244362)等属性。 有关详细信息，请参阅[字体指南](https://msdn.microsoft.com/library/windows/apps/hh700394.aspx)和[设计 UWP 应用](http://dev.windows.com/design)。

## <a name="theme-changes"></a>主题更改

对于 WindowsPhone Silverlight 应用，默认主题默认为深色。 对于 windows 10 设备，默认主题已更改，但你可以控制使用声明所请求的主题在 App.xaml 中的主题。 例如，若要在所有设备上都使用深色主题，请将 `RequestedTheme="Dark"` 添加到根 Application 元素。

## <a name="tiles"></a>磁贴

适用于 UWP 应用的磁贴具有行为类似于 WindowsPhone Silverlight 应用，动态磁贴，尽管存在一些差异。 例如，调用 **Microsoft.Phone.Shell.ShellTile.Create** 方法以创建辅助磁贴的代码应该移植为调用 [**SecondaryTile.RequestCreateAsync**](https://msdn.microsoft.com/library/windows/apps/br230606)。 下面是之前和之后的示例中，首先 WindowsPhone Silverlight 版本：


```csharp
    var tileData = new IconicTileData()
    {
        Title = this.selectedBookSku.Title,
        WideContent1 = this.selectedBookSku.Title,
        WideContent2 = this.selectedBookSku.Author,
        SmallIconImage = this.SmallIconImageAsUri,
        IconImage = this.IconImageAsUri
    };

    ShellTile.Create(this.selectedBookSku.NavigationUri, tileData, true);
```

第二个是 UWP 等效项：

```csharp
    var tile = new SecondaryTile(
        this.selectedBookSku.Title.Replace(" ", string.Empty),
        this.selectedBookSku.Title,
        this.selectedBookSku.ArgumentString,
        this.IconImageAsUri,
        TileSize.Square150x150);

    await tile.RequestCreateAsync();
```

使用 **Microsoft.Phone.Shell.ShellTile.Update** 方法或 **Microsoft.Phone.Shell.ShellTileSchedule** 类更新磁贴的代码应该移植为使用 [**TileUpdateManager**](https://msdn.microsoft.com/library/windows/apps/br208622)、[**TileUpdater**](https://msdn.microsoft.com/library/windows/apps/br208628)、[**TileNotification**](https://msdn.microsoft.com/library/windows/apps/br208616) 和/或 [**ScheduledTileNotification**](https://msdn.microsoft.com/library/windows/apps/hh701637) 类。

有关磁贴、toast、锁屏提醒、横幅和通知的详细信息，请参阅[创建磁贴](https://msdn.microsoft.com/library/windows/apps/xaml/hh868260)以及[使用磁贴、徽章和 Toast 通知](https://msdn.microsoft.com/library/windows/apps/xaml/hh868259)。 有关用于 UWP 磁贴的视觉资源的大小的具体信息，请参阅[磁贴和 Toast 视觉资源](https://msdn.microsoft.com/library/windows/apps/hh781198)。

## <a name="toasts"></a>Toast

使用 **Microsoft.Phone.Shell.ShellToast** 类显示 Toast 的代码应该移植为使用 [**ToastNotificationManager**](https://msdn.microsoft.com/library/windows/apps/br208642)、[**ToastNotifier**](https://msdn.microsoft.com/library/windows/apps/br208653)、[**ToastNotification**](https://msdn.microsoft.com/library/windows/apps/br208641) 和/或 [**ScheduledToastNotification**](https://msdn.microsoft.com/library/windows/apps/br208607) 类。 请注意，在移动设备上，“Toast”的面向消费者的术语是“横幅”。

请参阅[使用磁贴、锁屏提醒和 toast 通知](https://msdn.microsoft.com/library/windows/apps/xaml/hh868259)。

## <a name="view-or-effective-pixels-viewing-distance-and-scale-factors"></a>视图或有效像素、观看距离和比例系数

WindowsPhone Silverlight 应用和 windows 10 应用不同的大小和离开的实际物理大小的 UI 元素的布局和设备的分辨率抽象的方法。 WindowsPhone Silverlight 应用使用视图像素来执行此操作。 Windows 10，视图像素的概念已优化为有效像素。 以下是该术语的解释、它的意义以及它所提供的额外价值。

术语“分辨率”是指像素密度的度量，而不是通常认为的像素计数。 “有效分辨率”是构成图像或字形的物理像素对肉眼解析的方法，因为设备的观看距离和物理像素大小之间有差异（像素密度是物理像素大小的倒数）。 有效分辨率是构建周围体验的良好指标，因为它是以用户为中心的。 通过了解所有因素并控制 UI 元素的大小，你可以优化用户的体验。

对于 WindowsPhone Silverlight 应用，所有手机屏幕宽都是完全 480 视图像素，无一例外，无论多少物理像素屏幕有，或者其像素密度和物理大小是什么。 这意味着，与**图像**元素`Width="48"`将是可以运行 WindowsPhone Silverlight 应用的任何手机的屏幕宽度的十分之一完全。

对于 windows 10 应用，它是** 这种情况，所有设备都是某个固定有效像素数宽。 鉴于 UWP 应用可运行的设备种类广泛，这可能很明显。 不同设备的宽度的有效像素是不同的，范围从最小设备的 320 像素到中等大小监视器的 1024 像素，甚至更高宽度的有效像素。 你只需像往常那样继续使用可自动调整大小的元素和动态布局面板。 在某些情况下，你还需要将 XAML 标记中的 UI 元素的相关属性设置为固定大小。 根据应用运行所在的设备和用户所设置的显示设置，比例因子将自动应用于应用。 并且，该比例因子可使具有固定大小的任何 UI 元素在各种尺寸的屏幕上都能向用户显示一些大小恒定的触摸（和阅读）目标。 此外通过与动态布局结合使用，你的 UI 不仅能在不同设备上进行视觉上的缩放，还能改为执行任何必要的操作以将相应的内容量纳入可用空间。

由于 480 以前的固定的宽度视图像素手机大小屏幕，并且该值现在通常以有效像素为单位较小，经验法则是 WindowsPhone Silverlight 应用标记中的任何维度乘以系数 0.8。

这样，应用便可在所有屏幕上提供最佳体验。我们建议你针对各种屏幕大小创建每个位图资源，其中每个资源均适用于特定的比例因子。 在大多数情况下，提供 100% 缩放、200% 缩放和 400% 缩放的资源（按优先级顺序）能在采用所有中间比例系数时均可提供极佳效果。

**注意**如果出于任何原因无法创建资产的多个大小，则创建 100%缩放的资源。 在 Microsoft Visual Studio 中，UWP 应用的默认项目模板仅使用一个大小提供品牌标识资源（磁贴图像和徽标），但这些资源并非 100% 缩放。 为自己的应用编写资源时，请按照本部分中的指南进行编写、提供 100%、200% 和 400% 尺寸，并使用资源包。

如果具有繁复的图案，则可能希望在更多尺寸中提供资源。 如果要从矢量图像开始，则生成采用任意比例系数的高质量资源相对容易。

我们不建议你尝试支持所有比例系数，但适用于 windows 10 应用的比例系数的完整列表为 100%、 125%、 150%、 200%、 250%、 300%和 400%。 如果你支持这些比例系数，应用商店将针对每台设备选取大小适合的资源，然后将仅下载这些资源。 应用商店将根据设备的 DPI 选择要下载的资源。

有关详细信息，请参阅[适用于 UWP 应用的响应式设计基础知识](https://msdn.microsoft.com/library/windows/apps/dn958435)。

## <a name="window-size"></a>窗口大小

在 UWP 应用中，你可以使用命令式代码指定最小大小（宽度以及高度）。 默认的最小大小为 500x320 epx，这也是可接受的最小大小的最小值。 可接受的最小大小的最大值为 500x500epx。

```csharp
   Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().SetPreferredMinSize
        (new Size { Width = 500, Height = 500 });
```

下一主题是[针对 I/O、设备和应用模型进行移植](wpsl-to-uwp-input-and-sensors.md)。

## <a name="related-topics"></a>相关主题

* [命名空间和类映射](wpsl-to-uwp-namespace-and-class-mappings.md)
