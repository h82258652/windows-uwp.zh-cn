---
description: 在声明性 XAML 标记的窗体中定义 UI 的做法特别适合从 Windows Phone Silverlight 转换为通用 Windows 平台 (UWP) 应用。
title: 迁移 Windows Phone Silverlight XAML 和 UWP 的 UI
ms.assetid: 49aade74-5dc6-46a5-89ef-316dbeabbebe
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ac96354fa1752f655307a5dfd18678233e59c724
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372438"
---
#  <a name="porting-windowsphone-silverlight-xaml-and-ui-to-uwp"></a>迁移 Windows Phone Silverlight XAML 和 UWP 的 UI



上一主题是[疑难解答](wpsl-to-uwp-troubleshooting.md)。

在声明性 XAML 标记的窗体中定义 UI 的做法特别适合从 Windows Phone Silverlight 转换为通用 Windows 平台 (UWP) 应用。 你将发现，更新了系统资源键引用、更改了某些元素类型名称并将“clr-namespace”更改为“using”后，标记的一大部分将可兼容。 表示层中的大部分强制性代码（操作 UI 元素的视图模型和代码）也将易于移植。

## <a name="a-first-look-at-the-xaml-markup"></a>XAML 标记一览

在上一主题介绍了如何将复制你的 XAML 和代码隐藏文件到您的新 Windows 10 个 Visual Studio 项目。 你可能注意到的最早出现的 Visual Studio XAML 设计器中突出显示的问题之一是，XAML 文件的根中的 `PhoneApplicationPage` 元素对通用 Windows 平台 (UWP) 项目无效。 在上一主题中，保存一份 Visual Studio 生成时创建的 Windows 10 项目的 XAML 文件。 如果你打开该版本的 MainPage.xaml，你将看到根中的类型为 [**Page**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page)，此类型在 [**Windows.UI.Xaml.Controls**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls) 命名空间中。 因此，你可以将所有 `<phone:PhoneApplicationPage>` 元素更改为 `<Page>`（不要忘记属性元素语法），并且可以删除 `xmlns:phone` 声明。

查找与 Windows Phone Silverlight 类型相对应的 UWP 类型使用更多常规方法，您可以参考[Namespace 和类映射](wpsl-to-uwp-namespace-and-class-mappings.md)。

## <a name="xaml-namespace-prefix-declarations"></a>XAML 命名空间前缀声明


如果你在视图中使用自定义类型的实例（可能是视图模型实例或值转换器），则 XAML 标记中将具有 XAML 命名空间前缀声明。 Windows Phone Silverlight 和 UWP 之间不同，这些语法。 下面是一些可能的恶意活动：

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

**BitmapImage**处于**System.Windows.Media.Imaging**命名空间在 Windows Phone Silverlight 中，并使用同一文件中的指令允许**BitmapImage**没有命名空间的情况下使用如上面的段中所示的资格。 在这种情况下，你可以在 Visual Studio 中右键单击类型名称 (**BitmapImage**) 并在上下文菜单上使用 **Resolve** 命令以将新的命名空间指令添加到该文件。 在此情况下，添加 [**Windows.UI.Xaml.Media.Imaging**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging) 命名空间，这是该类型在 UWP 中所处的位置。 你可以删除 **System.Windows.Media.Imaging** using 指令，这是在以上代码段中移植类似代码所需的全部操作。 完成后，你将删除所有 Windows Phone Silverlight 命名空间。

在像这样将旧命名空间中的类型映射到新命名空间中的相同类型的简单案例中，可以使用 Visual Studio 的 **Find and Replace** 命令对源代码进行批量更改。 **Resolve** 命令是发现类型的新命名空间的绝佳方法。 作为另一个示例，你可以将所有“System.Windows”替换为“Windows.UI.Xaml”。 这实质上将移植所有引用该命名空间的 using 指令和完全限定的类型名称。

删除所有旧的 using 指令并添加新指令后，可以使用 Visual Studio 的 **Organize Usings** 命令为指令排序并删除未使用的指令。

有时只需修复少量的强制性代码，例如更改参数类型。 其他情况下，你将需要使用 UWP Api 而不是.NET Api 的 Windows 运行时 8.x 应用。 若要确定支持哪些 Api，请使用此迁移指南的其余部分结合[.NET for Windows Runtime 8.x 应用概述](https://docs.microsoft.com/previous-versions/windows/apps/br230302(v=vs.140))并[Windows 运行时引用](https://docs.microsoft.com/uwp/api/)。

并且，如果你希望转到项目构建阶段，可以注释或去掉任何非必要的代码。 然后循环访问一次一个问题，请参阅本部分中的以下主题 (和上一个主题：[故障排除](wpsl-to-uwp-troubleshooting.md))，直到任何生成和运行时问题是烫出和您的端口已完成。

## <a name="adaptiveresponsive-ui"></a>自适应/响应式 UI

因为 Windows 10 应用可以在可能广泛的设备上运行-每个都有其自己的屏幕大小和分辨率，将想要超越移植您应用程序的最小步骤并且你将想要调整 UI 以这些设备上查找最佳。 你可以使用自适应视觉状态管理器功能来动态检测窗口大小并更改布局作为响应，还可以使用 Bookstore2 案例研究主题中的[自适应 UI](wpsl-to-uwp-case-study-bookstore2.md) 部分中所示的有关如何执行此操作的示例。

## <a name="alarms-and-reminders"></a>警告和提醒

应该移植使用 **Alarm** 或 **Reminder** 类的代码，以使用 [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) 类创建和注册后台任务，并在相关的时间显示 Toast。 请参阅[后台处理](wpsl-to-uwp-business-and-data.md)和 [Toast](#toasts)。

## <a name="animation"></a>动画

作为关键帧动画和 from/to 动画的首选替代方法，UWP 动画库对 UWP 应用可用。 为了运行流畅、外观美观以及使你的应用看起来像内置应用一样与 Windows 集成，这些动画已经过了设计和微调。 请参阅[快速入门：对 UI 使用库动画进行动画处理](https://docs.microsoft.com/previous-versions/windows/apps/hh452703(v=win.10))。

如果你确实在 UWP 应用中使用关键帧动画或 from/to 动画，则可能希望了解新的平台所引入的独立动画和从属动画之间的区别。 请参阅[优化动画和媒体](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-animations-and-media)。 在 UI 线程上运行的动画（如对布局属性进行动画处理的动画）被称为从属动画，在新的平台上运行时，它们将没有效果，除非你执行以下两项操作之一。 你可以将它们重定目标为对不同的属性进行动画处理（例如 [**RenderTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransform)），从而使它们独立。 或者可以在动画元素上设置 `EnableDependentAnimation="True"` 以确认你要运行无法保证流畅运行的动画的意图。 如果你使用 Blend 或 Visual Studio 创作新的动画，那么将在必要时为你设置该属性。

## <a name="back-button-handling"></a>后退按钮处理

在 Windows 10 应用中，您可以使用单一方法来处理后退按钮和它将在所有设备上运行。 在移动设备上，该按钮作为设备上的电容性按钮或外壳中的按钮向你提供。 在桌面设备上，只要你的应用内可进行后退导航，你便可以向该应用的镶边添加一个按钮，它将显示在窗口化的应用的标题栏中或平板电脑模式下的任务栏中。 后退按钮事件是所有设备系列的通用概念，并且硬件或软件中实现的按钮会引发相同的 [**BackRequested**](https://docs.microsoft.com/uwp/api/windows.ui.core.systemnavigationmanager.backrequested) 事件。

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

所有这些方面大部分仍受支持，但是有命名空间差异。 例如，**System.Windows.Data.Binding** 映射到 [**Windows.UI.Xaml.Data.Binding**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding)、**System.ComponentModel.INotifyPropertyChanged** 映射到 [**Windows.UI.Xaml.Data.INotifyPropertyChanged**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.INotifyPropertyChanged)、**System.Collections.Specialized.INotifyPropertyChanged** 映射到 [**Windows.UI.Xaml.Interop.INotifyCollectionChanged**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Interop.INotifyCollectionChanged)。

Windows Phone Silverlight 应用程序栏和应用程序栏按钮不能像他们可以在 UWP 应用中绑定。 你可能具有强制性代码，用于构造应用栏及其按钮、将它们绑定到属性和本地化字符串并处理其事件。 如果有，你现在可以选择移植该强制性代码，方法是将其替换为绑定到属性和命令的声明性标记以及静态资源引用，从而逐渐增加你的应用的安全性和可维护性。 你可以使用 Visual Studio 或 Blend for Visual Studio 绑定 UWP 应用栏按钮并为其设置样式，就像任何其他 XAML 元素一样。 请注意，在 UWP 应用中，你使用的类型名称是 [**CommandBar**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CommandBar) 和 [**AppBarButton**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBarButton)。

UWP 应用的绑定相关的功能当前具有以下限制：

-   没有对数据输入验证以及 [**IDataErrorInfo**](https://docs.microsoft.com/dotnet/api/system.componentmodel.idataerrorinfo?redirectedfrom=MSDN) 和 [**INotifyDataErrorInfo**](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifydataerrorinfo?redirectedfrom=MSDN) 接口的内置支持。
-   [**绑定**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding)类不包括 Windows Phone Silverlight 中提供的扩展格式设置属性。 不过，你仍然可以实现 [**IValueConverter**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.IValueConverter) 以提供自定义格式。
-   [  **IValueConverter**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.IValueConverter) 方法将语言字符串视为参数而不是 [**CultureInfo**](https://docs.microsoft.com/dotnet/api/system.globalization.cultureinfo?redirectedfrom=MSDN) 对象。
-   [  **CollectionViewSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource) 类不内置支持排序和筛选以及对工作执行差异化分组。 有关详细信息，请参阅[深度数据绑定](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth)和[数据绑定示例](https://go.microsoft.com/fwlink/p/?linkid=226854)。

尽管仍很大程度上支持的相同绑定功能，但 Windows 10 提供了一个新的选项和更高的性能绑定机制调用编译使用 {x： 绑定} 标记扩展的绑定。 请参阅[数据绑定：通过 XAML 数据绑定到新的增强功能的应用的性能](https://channel9.msdn.com/Events/Build/2015/3-635)，并[x： 绑定示例](https://go.microsoft.com/fwlink/p/?linkid=619989)。

## <a name="binding-an-image-to-a-view-model"></a>将图像绑定到视图模型

你可以将 [**Image.Source**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image.source) 属性绑定到属于类型 [**ImageSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ImageSource) 的视图模型的任何属性。 下面是属性的 Windows Phone Silverlight 应用程序中的此类的典型实现：

```csharp
    // this.BookCoverImagePath contains a path of the form "/Assets/CoverImages/one.png".
    return new BitmapImage(new Uri(this.CoverImagePath, UriKind.Relative));
```

在 UWP 应用中，使用 ms-appx [URI 方案](https://docs.microsoft.com/previous-versions/windows/apps/jj655406(v=win.10))。 因此，你可以使其余的代码保持原样，还可以使用 **System.Uri** 构造函数的不同重载将 ms-appx URI 方案放置在基 URI 中并将路径的其余部分附加到其中。 如下所示：

```csharp
    // this.BookCoverImagePath contains a path of the form "/Assets/CoverImages/one.png".
    return new BitmapImage(new Uri(new Uri("ms-appx://"), this.CoverImagePath));
```

这样，视图模型的其余部分、图像路径属性中的路径值以及 XAML 标记中的绑定可以全部保持原样。

## <a name="controls-and-control-stylestemplates"></a>控件和控件样式/模板

Windows Phone Silverlight 应用程序使用控件中定义**Microsoft.Phone.Controls**命名空间和**System.Windows.Controls**命名空间。 XAML UWP 应用使用 [**Windows.UI.Xaml.Controls**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls) 命名空间中定义的控件。 体系结构和设计在 UWP 中的 XAML 控件是几乎与 Windows Phone Silverlight 控件相同。 但是，进行了一些更改以改进可用的控件组并将它们与 Windows 应用统一。 以下是具体示例。

| 控件名称 | “更改” |
|--------------|--------|
| ApplicationBar | [Page.TopAppBar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.topappbar) 属性。 |
| ApplicationBarIconButton | UWP 等效项是 [Glyph](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.fonticon.glyph) 属性。 PrimaryCommands 是 CommandBar 的内容属性。 XAML 分析器将元素的内部 xml 解释为其内容属性的值。 |
| ApplicationBarMenuItem | UWP 等效项是设置为菜单项文本的 [AppBarButton.Label](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton.label)。 |
| ContextMenu（在 Windows Phone 工具包中） | 对于单选浮出控件，请使用 [Flyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Flyout)。 |
| ControlTiltEffect.TiltEffect 类 | UWP 动画库中的动画内置于常用控件的默认样式中。 请参阅[创建指针操作动画](https://docs.microsoft.com/previous-versions/windows/apps/jj649432(v=win.10))。 |
| 带有分组数据的 LongListSelector | 可以协同使用的两种方式中的 Windows Phone Silverlight LongListSelector 函数。 第一，它可以显示按某个键分组的数据，例如按首字母分组的名称列表。 第二，它可以在两个语义视图之间“缩放”：项（例如名称）的分组列表和只有组键（例如首字母）本身的列表。 借助 UWP，你可以按照[列表和网格视图控件指南](https://docs.microsoft.com/windows/uwp/controls-and-patterns/lists)进行操作来显示分组数据。 |
| 带有平面数据的 LongListSelector | 出于性能原因，在很长的列表的情况下建议 LongListSelector 而不是 Windows Phone Silverlight 列表框，即使对于平面时，非分组数据。 在 UWP 应用中，[GridView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView) 优先用于项的长列表，无论数据是否适合分组。 |
| Panorama | 在 Windows Phone Silverlight Panorama 控件映射到[准则的 Windows 运行时 8.x 应用中的中心控件](https://docs.microsoft.com/windows/uwp/controls-and-patterns/hub)和 hub 控件的指导原则。 <br/> 请注意，Panorama 控件从最后一部分环绕到第一部分，并且其背景图像相对于具体部分在视差中移动。 [Hub](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Hub) 部分不会环绕，并且不使用视差。 |
| Pivot | Windows Phone Silverlight Pivot 控件 UWP 等效项是[Windows.UI.Xaml.Controls.Pivot](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot)。 它适用于所有设备系列。 |

**请注意**   PointerOver 视觉状态是在自定义样式/模板在 Windows 10 应用中，但 Windows Phone Silverlight 应用程序中不相关。 还有其他原因你现有的自定义样式/模板可能适用于 Windows 10 应用，包括系统资源键使用的，对集的使用，视觉状态和性能改进对 Windows 10 默认样式所做的更改 /模板。 我们建议适用于 Windows 10 编辑控件的默认模板的新副本，然后重新应用到的样式和模板自定义。

有关 UWP 控件的详细信息，请参阅[按功能列出的控件](https://docs.microsoft.com/windows/uwp/controls-and-patterns/controls-by-function)、[控件列表](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/)和[控件指南](https://developer.microsoft.com/windows/design/controls-patterns)。

##  <a name="design-language-in-windows10"></a>Windows 10 中的设计语言

有一些设计语言在 Windows Phone Silverlight 应用程序和 Windows 10 应用程序之间的差异。 有关所有详细信息，请参阅[设计](https://developer.microsoft.com/en-us/windows/apps/design)。 不考虑设计语言更改，我们的设计原则始终保持一致：关注细节却又力求简洁（专注于内容而不是外观），显著减少视觉元素，始终忠实于数字领域；使用可视化层次结构（尤其是版式）；基于网格进行设计；通过流畅的动画带给你生动的体验。

## <a name="localization-and-globalization"></a>本地化和全球化

对本地化的字符串，您可以重复使用.resx 文件从 Windows Phone Silverlight 项目在 UWP 应用项目中。 复制文件，将其添加到项目，并将其重命名为 Resources.resw，以便查找机制默认找到它。 将 **“生成操作”** 设置为**PRIResource**并将 **“复制到输出目录”** 设置为 **“不要复制”** 。 然后你可以通过在 XAML 元素上指定 **“x:Uid”** 属性来使用标记中的字符串。 请参阅[快速入门：使用字符串资源](https://docs.microsoft.com/previous-versions/windows/apps/hh965329(v=win.10))。

Windows Phone Silverlight 应用程序使用**CultureInfo**课程，帮助全球化应用程序。 UWP 应用使用 MRT（现代资源技术），此技术支持在运行时和在 Visual Studio 设计图面中动态加载应用资源（本地化、比例和主题）。 有关详细信息，请参阅[文件、数据和全球化指南](https://developer.microsoft.com/windows/design/usability)。

[  **ResourceContext.QualifierValues**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.resources.core.resourcecontext.qualifiervalues) 主题介绍了如何基于设备系列资源选择规格加载特定于设备系列的资源。

## <a name="media-and-graphics"></a>媒体和图形

当你阅读到有关 UWP 媒体和图形的内容时，请记住 Windows 设计准则鼓励尽量减少任何多余的内容，包括图形的复杂度和混乱度。 Windows 设计以整洁清晰的视觉效果、版式和动作为特征。 如果你的应用遵循相同的准则，那么它看起来将更像内置应用。

Windows Phone Silverlight 有**RadialGradientBrush**类型，而它不存在在 UWP 中，尽管其他[**画笔**](/uwp/api/Windows.UI.Xaml.Media.Brush)类型。 在某些情况下，你将可以使用位图获得类似的效果。 请注意，你可以使用 [Microsoft DirectX](https://docs.microsoft.com/windows/desktop/directx) 中的 Direct2D 和 XAML C++ UWP [创建径向渐变画笔](https://docs.microsoft.com/windows/desktop/Direct2D/how-to-create-a-radial-gradient-brush)。

Windows Phone Silverlight 有**System.Windows.UIElement.OpacityMask**属性，但属性不是成员的 UWP [**UIElement** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement)类型。 在某些情况下，你将可以使用位图获得类似的效果。 并且你可以使用 [Microsoft DirectX](https://docs.microsoft.com/windows/desktop/directx) 中的 Direct2D 和 XAML C++ UWP 应用[创建不透明蒙板](https://docs.microsoft.com/windows/desktop/Direct2D/opacity-masks-overview)。 但是，**OpacityMask** 的常见用例是使用适应浅色和深色主题的单个位图。 对于矢量图形，你可以使用与主题有关的系统画笔（例如下面所示的饼图）。 但是，若要创建与主题有关的位图（例如下面所示的复选标记），则需要使用其他方法。

![与主题有关的位图](images/wpsl-to-uwp-case-studies/wpsl-to-uwp-theme-aware-bitmap.png)

在 Windows Phone Silverlight 应用中，方法是使用 （中的位图的窗体） 的 alpha 掩码作为**OpacityMask**有关**矩形**前景画笔填充：

```xml
    <Rectangle Fill="{StaticResource PhoneForegroundBrush}" Width="26" Height="26">
        <Rectangle.OpacityMask>
            <ImageBrush ImageSource="/Assets/wpsl_check.png"/>
        </Rectangle.OpacityMask>
    </Rectangle>
```

将它移植到 UWP 应用的最简单的方法是使用 [**BitmapIcon**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.BitmapIcon)，如下所示：

```xml
    <BitmapIcon UriSource="Assets/winrt_check.png" Width="21" Height="21"/>
```

此处，winrt\_check.png 是 alpha 掩码形式的位图只作为 wpsl\_check.png，以及它很可能是相同的文件。 但是，你可能想要提供多个不同大小的 winrt\_check.png 用于不同的缩放比例。 有关详细信息和对 **Width** 和 **Height** 值进行更改的说明，请参阅本主题中的[视图或有效像素、观看距离和比例系数](#view-or-effective-pixels-viewing-distance-and-scale-factors)。

较常规的方法（适用于位图的浅色和深色主题之间有差异的情况）是使用两个图像资源：一个带有深色前景（用于浅色主题），而另一个带有浅色前景（用于深色主题）。 有关如何命名此集位图资产的详细信息，请参阅[定制你的资源的语言、 缩放性和其他限定符](../app-resources/tailor-resources-lang-scale-contrast.md)。 为一组图像文件正确命名后，你可以使用它们的根名称在摘要中引用它们，如下所示：

```xml
    <Image Source="Assets/winrt_check.png" Stretch="None"/>
```

在 Windows Phone Silverlight **UIElement.Clip**属性可以是任何可以使用 express 的形状**Geometry**通常在 XAML 中的标记中序列化和**StreamGeometry**微型语言。 在 UWP 中，[**Clip**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.clip) 属性的类型是 [**RectangleGeometry**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.RectangleGeometry)，以便你可以只剪裁矩形区域。 允许使用小型语言定义矩形这一做法过于宽松。 因此，若要在标记中移植裁剪区域，请替换 **Clip** 属性语法并使其成为与以下内容类似的属性元素语法：

```xml
    <UIElement.Clip>
        <RectangleGeometry Rect="10 10 50 50"/>
    </UIElement.Clip>
```

请注意，你可以凭借 [Microsoft DirectX](https://docs.microsoft.com/windows/desktop/directx) 中的 Direct2D 和 XAML C++ UWP 应用[使用任意几何图形作为层中的蒙板](https://docs.microsoft.com/windows/desktop/Direct2D/direct2d-layers-overview)。

## <a name="navigation"></a>导航

当你导航到 Windows Phone Silverlight 应用程序中的页时，您可以使用统一资源标识符 (URI) 寻址方案：

```csharp
    NavigationService.Navigate(new Uri("/AnotherPage.xaml", UriKind.Relative)/*, navigationState*/);
```

在 UWP 应用中，调用 [**Frame.Navigate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.navigate) 方法并指定目标页面的类型（如该页面的 XAML 标记定义的 **x:Class** 属性中所述）：


```csharp
    // In a page:
    this.Frame.Navigate(typeof(AnotherPage)/*, parameter*/);

    // In a view model, perhaps inside an ICommand implementation:
    var rootFrame = Windows.UI.Xaml.Window.Current.Content as Windows.UI.Xaml.Controls.Frame;
    rootFrame.Navigate(typeof(AnotherPage)/*, parameter*/);
```

在 WMAppManifest.xml 中定义的 Windows Phone Silverlight 应用程序的启动页面：

```xml
    <DefaultTask Name="_default" NavigationPage="MainPage.xaml" />
```

在 UWP 应用中，使用强制性代码定义启动页面。 下面是 App.xaml.cs 中一些演示方法的代码：

```csharp
    if (!rootFrame.Navigate(typeof(MainPage), e.Arguments))
```

URI 映射和片段导航是 URI 导航技术，因此它们不适用于 UWP 导航，后者不基于 URI。 URI 映射的存在是为了响应使用 URI 字符串标识目标页面的弱类型性质，如果页面移动到不同的页面，从而移动到不同的相对路径，此性质可能导致易坏性和可维护性问题。 UWP 应用使用基于类型的导航，它是强类型且经过编译器检查的，因此没有 URI 映射所解决的问题。 片段导航的用例是将一些上下文一起传递到目标页面，以便该页面可以使其内容的特定片段显示或者滚动到视图中。 调用 [**Navigate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.navigate) 方法时，可以通过传递导航参数实现相同的目标。

有关详细信息，请参阅[导航](https://docs.microsoft.com/windows/uwp/layout/navigation-basics)。

## <a name="resource-key-reference"></a>资源键引用

设计语言演变成适用于 Windows 10 和因此特定系统的样式已发生更改，以及已删除或重命名为很多系统资源键。 Visual Studio 中的 XAML 标记编辑器突出显示对无法解析的资源键的引用。 例如，XAML 标记编辑器将使用红色波形曲线为对样式键 `PhoneTextNormalStyle` 的引用加下划线。 如果未更正该错误，则应用将在你尝试将其部署到模拟器或设备时立即终止。 因此，请务必留意 XAML 标记的正确性。 而且你将发现 Visual Studio 是捕获此类问题的绝佳工具。

另请参阅下面的[文本](#text)。

## <a name="status-bar-system-tray"></a>状态栏（系统托盘）

系统托盘(使用 `shell:SystemTray.IsVisible` 在 XAML 标记中进行设置）现在称为状态栏，并默认处于显示状态。 你可以在强制性代码中通过调用 [**Windows.UI.ViewManagement.StatusBar.ShowAsync**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.statusbar.showasync) 和 [**HideAsync**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.statusbar.hideasync) 方法控制其可见性。

## <a name="text"></a>Text

文本（或版式）是 UWP 应用的重要方面，并且在移植时，你可能希望回顾你的视图的视觉设计，以便它们与新设计语言相协调。 使用这些图示查找可用的 UWP **TextBlock** 系统样式。 找到对应于你所用的 Windows Phone Silverlight 样式。 或者，可以创建您自己的通用样式，并将从 Windows Phone Silverlight 系统样式属性复制到那些。

![适用于 Windows 10 应用的 TextBlock 系统样式](images/label-uwp10stylegallery.png)

Windows 10 应用的系统 TextBlock 样式

在 Windows Phone Silverlight 应用中，默认字体系列是 Segoe WP。 在 Windows 10 应用中，默认字体系列是 Segoe UI。 因此，你的应用中的字体指标可能看起来不同。 如果你想要重现的 Windows Phone Silverlight 文本外观，则可以设置指标使用属性，如[ **LineHeight** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.lineheight)并[ **LineStackingStrategy**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.linestackingstrategy). 有关详细信息，请参阅[字体指南](https://docs.microsoft.com/windows/uwp/controls-and-patterns/fonts)和[设计 UWP 应用](https://developer.microsoft.com/en-us/windows/apps/design)。

## <a name="theme-changes"></a>主题更改

对于 Windows Phone Silverlight 应用程序，默认主题为深色默认情况下。 适用于 Windows 10 设备，默认主题已发生更改，但您可以控制使用声明在 App.xaml 中的请求的主题的主题。 例如，若要在所有设备上都使用深色主题，请将 `RequestedTheme="Dark"` 添加到根 Application 元素。

## <a name="tiles"></a>磁贴

适用于 UWP 应用的磁贴有行为类似于实时磁贴的 Windows Phone Silverlight 应用程序，但有一些差异。 例如，调用 **Microsoft.Phone.Shell.ShellTile.Create** 方法以创建辅助磁贴的代码应该移植为调用 [**SecondaryTile.RequestCreateAsync**](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.secondarytile.requestcreateasync)。 下面是之前和之后的示例中，第一次 Windows Phone Silverlight 版本：


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

使用 **Microsoft.Phone.Shell.ShellTile.Update** 方法或 **Microsoft.Phone.Shell.ShellTileSchedule** 类更新磁贴的代码应该移植为使用 [**TileUpdateManager**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileUpdateManager)、[**TileUpdater**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileUpdater)、[**TileNotification**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileNotification) 和/或 [**ScheduledTileNotification**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ScheduledTileNotification) 类。

有关磁贴、toast、锁屏提醒、横幅和通知的详细信息，请参阅[创建磁贴](https://docs.microsoft.com/previous-versions/windows/apps/hh868260(v=win.10))以及[使用磁贴、徽章和 Toast 通知](https://docs.microsoft.com/previous-versions/windows/apps/hh868259(v=win.10))。 有关用于 UWP 磁贴的视觉资源的大小的具体信息，请参阅[磁贴和 Toast 视觉资源](https://docs.microsoft.com/previous-versions/windows/apps/hh781198(v=win.10))。

## <a name="toasts"></a>Toast

使用 **Microsoft.Phone.Shell.ShellToast** 类显示 Toast 的代码应该移植为使用 [**ToastNotificationManager**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationManager)、[**ToastNotifier**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotifier)、[**ToastNotification**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotification) 和/或 [**ScheduledToastNotification**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ScheduledToastNotification) 类。 请注意，在移动设备上，“Toast”的面向消费者的术语是“横幅”。

请参阅[使用磁贴、锁屏提醒和 toast 通知](https://docs.microsoft.com/previous-versions/windows/apps/hh868259(v=win.10))。

## <a name="view-or-effective-pixels-viewing-distance-and-scale-factors"></a>视图或有效像素、观看距离和比例系数

Windows Phone Silverlight 应用程序和 Windows 10 应用在它们抽象的大小和实际的物理大小从 UI 元素的布局和分辨率的设备的方式不同。 Windows Phone Silverlight 应用程序使用视图像素为单位来执行此操作。 Windows 10 中，视图像素为单位的概念已被优化为的有效像素。 以下是该术语的解释、它的意义以及它所提供的额外价值。

术语“分辨率”是指像素密度的度量，而不是通常认为的像素计数。 “有效分辨率”是构成图像或字形的物理像素对肉眼解析的方法，因为设备的观看距离和物理像素大小之间有差异（像素密度是物理像素大小的倒数）。 有效分辨率是构建周围体验的良好指标，因为它是以用户为中心的。 通过了解所有因素并控制 UI 元素的大小，即可优化用户的体验。

向 Windows Phone Silverlight 应用程序，所有电话屏幕都是完全 480 像素宽的视图为、 无一例外，无论物理像素数具有屏幕，也不会其像素密度或物理大小的与。 这意味着**图像**具有元素`Width="48"`将完全任何的十分之一可运行 Windows Phone Silverlight 应用程序的手机屏幕的宽度。

到 Windows 10 应用，它是*不*所有设备都一些固定的有效像素数。 鉴于 UWP 应用可运行的设备种类广泛，这可能很明显。 不同设备的宽度的有效像素是不同的，范围从最小设备的 320 像素到中等大小监视器的 1024 像素，甚至更高宽度的有效像素。 你只需像往常那样继续使用可自动调整大小的元素和动态布局面板。 在某些情况下，你还需要将 XAML 标记中的 UI 元素的相关属性设置为固定大小。 根据应用运行所在的设备和用户所设置的显示设置，比例因子将自动应用于应用。 并且，该比例因子可使具有固定大小的任何 UI 元素在各种尺寸的屏幕上都能向用户显示一些大小恒定的触摸（和阅读）目标。 此外通过与动态布局结合使用，你的 UI 不仅能在不同设备上进行视觉上的缩放，还能改为执行任何必要的操作以将相应的内容量纳入可用空间。

由于 480 之前为固定的宽度像素尺寸手机的屏幕，而该值现有效像素通常更小的视图中，经验法则是将您的 Windows Phone Silverlight 应用程序标记中的任何维度乘以 0.8 的因素。

这样，应用便可在所有屏幕上提供最佳体验。我们建议你针对各种屏幕大小创建每个位图资源，其中每个资源均适用于特定的比例因子。 在大多数情况下，提供 100% 缩放、200% 缩放和 400% 缩放的资源（按优先级顺序）能在采用所有中间比例系数时均可提供极佳效果。

**请注意**  如果无论什么原因，你不能在多个大小中创建资产然后创建 100%比例的资产。 在 Microsoft Visual Studio 中，UWP 应用的默认项目模板仅使用一个大小提供品牌标识资源（磁贴图像和徽标），但这些资源并非 100% 缩放。 为自己的应用编写资源时，请按照本部分中的指南进行编写、提供 100%、200% 和 400% 尺寸，并使用资源包。

如果具有繁复的图案，则可能希望在更多尺寸中提供资源。 如果要从矢量图像开始，则生成采用任意比例系数的高质量资源相对容易。

我们不建议您尝试支持所有的缩放比例，但 Windows 10 应用的缩放比例的完整列表是 100%、 125%、 150%、 200%、 250%，300%到 400%。 如果你支持这些比例系数，应用商店将针对每台设备选取大小适合的资源，然后将仅下载这些资源。 应用商店将根据设备的 DPI 选择要下载的资产。

有关详细信息，请参阅[适用于 UWP 应用的响应式设计基础知识](https://docs.microsoft.com/windows/uwp/layout/screen-sizes-and-breakpoints-for-responsive-design)。

## <a name="window-size"></a>窗口大小

在 UWP 应用中，你可以使用命令式代码指定最小大小（宽度以及高度）。 默认的最小大小为 500x320 epx，这也是可接受的最小大小的最小值。 可接受的最小大小的最大值为 500x500epx。

```csharp
   Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().SetPreferredMinSize
        (new Size { Width = 500, Height = 500 });
```

下一主题是[针对 I/O、设备和应用模型进行移植](wpsl-to-uwp-input-and-sensors.md)。

## <a name="related-topics"></a>相关主题

* [Namespace 和类映射](wpsl-to-uwp-namespace-and-class-mappings.md)
