---
author: jwmsft
ms.assetid: 00ECF6C7-0970-4D5F-8055-47EA49F92C12
title: 应用启动性能的最佳实践
description: 通过改进你处理启动和激活的方式，创建启动时间最短的通用 Windows 平台 (UWP) 应用。
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 25ddcc6c9ceaecd858733a0222c22c18682041b8
ms.sourcegitcommit: bdc40b08cbcd46fc379feeda3c63204290e055af
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/08/2018
ms.locfileid: "6152406"
---
# <a name="best-practices-for-your-apps-startup-performance"></a>应用启动性能的最佳实践


通过改进你处理启动和激活的方式，创建启动时间最短的通用 Windows 平台 (UWP) 应用。

## <a name="best-practices-for-your-apps-startup-performance"></a>应用的启动性能的最佳做法

用户认为你的应用是快还是慢，在某种程度上取决于启动应用所需的时间。 就本主题而言，应用的启动时间从用户启动应用起算，并在用户以某些有意义的方式与应用交互时截止。 本部分提供了关于如何在应用启动时获取更好的性能的建议。

### <a name="measuring-your-apps-startup-time"></a>评估应用的启动时间

在你实际评估应用的启动时间前，请确保启动你的应用几次。 这将为你的评估提供一个基准，并确保在尽可能短的启动时间内完成评估。

当你的 UWP 应用送达客户的计算机时，你的应用已使用 .NET Native 工具链进行编译。 .NET Native 是一种先进的编译技术，可将 MSIL 转换为可本机运行的计算机代码。 .NET Native 应用启动速度更快、使用的内存更少，并且比其对应的 MSIL 更省电。 使用 .NET Native 生成的应用程序可静态链接到自定义运行时，而使用新的聚合 .NET Core 生成的应用程序可以在所有设备运行，因此它们不依赖于内置 .NET 实现。 在你的开发计算机上，如果你在“发布”模式下生成你的应用，则应用默认使用 .NET Native；如果你在“调试”模式下生成你的应用，则应用默认使用 CoreCLR。 在 Visual Studio 中，你可以从“生成”页面的“属性”(C#) 或“我的项目”(VB) 中的“编译”-&gt;“高级”来配置它。 查找一个显示“使用 .NET Native 工具链编译”的复选框。

当然，你应该获取能代表最终用户所体验的衡量基准。 因此，如果你不确定是否要在你的开发计算机上将你的应用编译为本机代码，则可以在衡量应用的启动时间前，运行本机映像生成器 (Ngen.exe) 工具来预编译应用。

以下过程描述了如何运行 Ngen.exe 来编译你的应用。

**运行 Ngen.exe**

1.  至少运行你的应用一次，以确保 Ngen.exe 能检测到它。
2.  通过执行以下操作之一，打开“任务计划程序” ****：
    -   从开始屏幕中搜索“任务计划程序”。
    -   运行“taskschd.msc”。
3.  在 ****“任务计划程序”的左侧窗格，展开 **** "任务计划程序库"。
4.  展开 **** "Microsoft."
5.  展开 **** "Windows."
6.  选择 **** ".NET Framework"。
7.  从任务列表中选择 **** ".NET Framework NGEN 4.x"。

    如果你使用的是 64 位计算机，还有一个 **.NET Framework NGEN v4.x 64**。 如果你要构建 64 位应用，选择**** ".NET Framework NGEN v4.x 64"。

8.  在 **“操作”** 菜单上，单击 **“运行”**。

Ngen.exe 编译计算机上所有已被使用和不拥有本机映像的应用。 如果存在许多需要编译的应用，这会花费较长时间，但后续的运行会更快。

编译你的应用时，不再使用本机映像。 应用反而正好被编译，也即是指应用在运行时被编译。 你必须返回 Ngen.exe 来获取新的本机映像。

### <a name="defer-work-as-long-as-possible"></a>尽量推迟工作

要增加应用的启动时间，请仅处理必须要完成的工作，以让用户开始与应用交互。 如果你可延迟加载其他程序集，这样会十分有利。 常见语言运行时间加载首次使用的程序集。 如果你能将所加载的程序集数目降至最低，则应该能够改善应用的启动时间及其内存消耗。

### <a name="do-long-running-work-independently"></a>独立执行长时间的运行工作

即使应用的部分功能未齐全，该应用也可交互。 例如，如果你的应用显示需要些时间检索的数据，你可通过异步检索数据来确保独立于应用的启动代码的代码执行。 获得数据后，用数据来填充应用的用户界面。

多数用于检索数据的通用 Windows 平台 (UWP) API 都是异步的，因此无论如何你都可以异步检索数据。 有关异步 API 的详细信息，请参阅[使用 C# 或 Visual Basic 调用异步 API](https://msdn.microsoft.com/library/windows/apps/Mt187337)。 如果处理不使用异步 API 的工作，可以使用 Task 类来处理长时间运行的工作，以便不会阻止用户与应用交互。 这将使你的应用能够在加载数据时对用户作出响应。

如果你的应用花费很长时间来加载其部分 UI，则可以考虑在该区域添加一个字符串（如“获取最新数据”之类的提示信息），以便你的用户知道应用仍在运行。

## <a name="minimize-startup-time"></a>最小化启动时间

除了最简单应用之外的所有应用都需要一段长到可以察觉的时间来在激活时加载资源、分析 XAML、设置数据结构以及运行逻辑。 下面我们通过将激活过程分为三个阶段来对其进行分析。 我们还提供关于减少在每个阶段所花时间的提示，以及关于让应用启动的每个阶段更适合用户的技巧。

激活时段是指用户启动应用和该应用开始正常运行之间的那段时间。 这是一段很关键的时段，因为这是用户对你的应用的第一印象。 他们期望来自系统和应用的即时而连续的反馈。 应用不能快速启动时，用户会觉得系统和应用有问题或设计得很差。 更糟的是，如果应用激活耗时过长，进程生命期管理器 (PLM) 可能会终止它，或者用户可能会卸载它。

### <a name="introduction-to-the-stages-of-startup"></a>启动阶段简介

启动涉及到大量的事件操控，并且它们都需要进行正确协调，以便提供最佳用户体验。 在用户单击你的应用磁贴和应用程序内容显示之间的这段时间，将发生以下事件。

-   Windows shell 启动进程，Main 将进行调用。
-   创建 Application 对象。
    -   (ProjectTemplate) 构造函数将调用 InitializeComponent，从而导致对 App.xaml 进行分析以及创建对象。
-   引发 Application.OnLaunched 事件。
    -   (ProjectTemplate) 应用代码将创建一个帧，并导航到 MainPage。
    -   (ProjectTemplate) Mainpage 构造函数将调用 InitializeComponent，从而导致对 MainPage.xaml 进行分析以及创建对象。
    -   调用 (ProjectTemplate) Window.Current.Activate()。
-   XAML 平台运行布局过程，包括 Measure 和 Arrange。
    -   ApplyTemplate 将导致针对每个控件创建控件模板内容，这通常是启动的布局时间内主要执行的操作。
-   将调用呈现器来为所有窗口内容创建视觉效果。
-   框架将呈现在桌面窗口管理器 (DWM) 中。

### <a name="do-less-in-your-startup-path"></a>精简启动路径

不要让你的启动代码路径包含第一帧不需要的任何内容。

-   如果你的用户 dll 中包含第一帧呈现期间不需要的任何控件，请考虑延迟加载它们。
-   如果你的 UI 中有一部分依赖于云中的数据，请拆分该 UI。 先运行不依赖于云数据的 UI，然后异步运行依赖于云数据的 UI。 你还应考虑本地缓存数据，以便应用程序可以脱机工作，或不受较慢网络连接的影响。
-   如果你的 UI 正在等待数据，将显示进度 UI。
-   请留意涉及很多配置文件分析的应用设计或由代码动态生成的 UI。

### <a name="reduce-element-count"></a>减少元素计数

XAML 应用中的启动性能与启动期间创建的元素数直接关联。 创建的元素越少，启动应用所需的时间就越短。 作为粗略的基准，设定每个元素创建所需的时间为 1 毫秒。

-   在项目控件中使用的模板具有最大的影响力，因为它们会重复使用多次。 请参阅 [ListView 和 GridView UI 优化](optimize-gridview-and-listview.md)。
-   用户控件和控件模板将进行扩展，所以应将这些内容考虑在内。
-   如果你创建了不会在屏幕上显示的任意 XAML，则应判断 XAML 的这些部分是否应在启动期间创建。

[Visual Studio 实时可视化树](http://blogs.msdn.com/b/visualstudio/archive/2015/02/24/introducing-the-ui-debugging-tools-for-xaml.aspx)窗口会显示树中每个节点的子元素计数。

![实时可视化树。](images/live-visual-tree.png)

**使用延迟**。 无法通过折叠某个元素或将其不透明度设置为 0 来阻止该元素创建。 可使用 x:Load 或 x:DeferLoadStrategy 来延迟部分 UI 的加载，并在需要时加载它。 最好延迟处理在启动屏幕期间不可见的 UI，这样你便可以在需要时加载它，或作为一组延迟逻辑的一部分加载它。 若要触发加载，只需针对元素调用 FindName 即可。 有关示例和详细信息，请参阅 [x:Load 属性](../xaml-platform/x-load-attribute.md)和 [x:DeferLoadStrategy 属性](https://msdn.microsoft.com/library/windows/apps/Mt204785)。

**虚拟化**。 如果你的 UI 中有列表或 repeater 内容，强烈建议你使用 UI 虚拟化。 如果未虚拟化列表 UI，则在创建所有元素前需要花费一些开销，而这样可能会减慢启动速度。 请参阅 [ListView 和 GridView UI 优化](optimize-gridview-and-listview.md)。

应用程序性能不仅仅是原始性能，还包括感知方面。 更改操作顺序以便先出现视觉方面的内容，通常会让用户觉得应用程序的启动速度更快。 当内容显示在屏幕上时，用户会认为应用程序已加载。 通常情况下，应用程序需要执行多项操作作为启动的一部分，但并非所有这些操作都是显示 UI 所需的操作，因此应当延迟那些不必要的操作或使它们的优先级低于 UI。

本主题将讨论“第一帧”，它来源于动画/电视节目，并且是内容呈现给最终用户所需时长的测量方式。

### <a name="improve-startup-perception"></a>改善启动感知

让我们使用一个简单的在线游戏示例来识别一下启动的每个阶段以及在整个过程中为用户提供反馈的不同技巧。 在此示例中，激活的第一个阶段是指用户点击游戏磁贴和游戏开始运行其代码之间的这段时间。 在这段时间内，系统不向用户显示任何内容来指示正确的游戏已启动。 但提供一个初始屏幕来为系统提供该内容。 接下来，当游戏开始运行代码时，游戏会通过将静态初始屏幕替换为它自己的 UI 来通知用户激活的第一个阶段已完成。

激活的第二个阶段包括创建和初始化对于游戏至关重要的结构。 如果应用可以在激活的第一个阶段之后使用可用的数据快速创建其初始 UI，那么第二个阶段是微不足道的，你可以立即显示 UI。 否则，我们建议应用在进行初始化时显示一个加载页面。

加载页面的外观由你决定，并且可以使其外观像显示一个进度条或进度环一样简单。 关键点是应用指示它正在执行任务，直到做出响应的那一刻。 就该游戏来说，它想显示其初始屏幕，但 UI 要求将某些图像和声音从磁盘加载到内存中。 这些任务会花费几秒钟，因此应用通过将初始屏幕替换为加载页面来通知用户，该加载页面显示与游戏的主题相关的一个简单动画。

在游戏有一个最小的信息集来创建交互式 UI（该 UI 将替换加载页面）之后，第三个阶段将开始。 此时，在线游戏唯一可以使用的信息是应用从磁盘加载的内容。 该游戏可以提供足够的内容来创建交互式 UI，但是因为它是一个在线游戏，所以在其连接到 Internet 并下载某些附加信息之后，它才能正常运行。 在游戏获得正常运行所需的所有信息之后，用户才可以与 UI 进行交互，但是，那些需要来自 Web 的附加数据的功能应该提供关于内容仍在加载中的反馈。 应用进入完全正常运行的状态可能需要一些时间，因此尽快使功能可用很重要。

既然我们已确定了该在线游戏中激活的三个阶段，让我们将它们与实际代码联系起来吧。

### <a name="phase-1"></a>第 1 阶段

在应用启动之前，它需要告诉系统它希望显示为初始屏幕的内容。 如示例中所示，它是通过向应用部件清单中的 SplashScreen 元素提供图像颜色和背景颜色来完成此任务的。 在应用开始激活之后，Windows 显示此内容。

```xml
<Package ...>
  ...
  <Applications>
    <Application ...>
      <VisualElements ...>
        ...
        <SplashScreen Image="Images\splashscreen.png" BackgroundColor="#000000" />
        ...
      </VisualElements>
    </Application>
  </Applications>
</Package>
```

有关详细信息，请参阅[添加初始屏幕](https://msdn.microsoft.com/library/windows/apps/Mt187306)。

使用应用的构造函数仅初始化对于应用至关重要的数据结构。 仅第一次运行应用时会调用该构造函数，而不必在每次激活应用时调用它。 例如，对于已经运行过并置于后台中、然后通过搜索合约激活的应用，不会调用构造函数。

### <a name="phase-2"></a>第 2 阶段

激活应用存在许多原因，你可能希望以不同的方式处理每个原因。 你可以替代 [**OnActivated**](https://msdn.microsoft.com/library/windows/apps/BR242330)、[**OnCachedFileUpdaterActivated**](https://msdn.microsoft.com/library/windows/apps/Hh701797)、[**OnFileActivated**](https://msdn.microsoft.com/library/windows/apps/BR242331)、[**OnFileOpenPickerActivated**](https://msdn.microsoft.com/library/windows/apps/Hh701799)、[**OnFileSavePickerActivated**](https://msdn.microsoft.com/library/windows/apps/Hh701801)、[**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/BR242335)、[**OnSearchActivated**](https://msdn.microsoft.com/library/windows/apps/BR242336) 和 [**OnShareTargetActivated**](https://msdn.microsoft.com/library/windows/apps/Hh701806) 方法来处理每个激活原因。 在这些方法中，应用必须完成的一个事项是创建 UI，将其分配给 [**Window.Content**](https://msdn.microsoft.com/library/windows/apps/BR209051)，然后调用 [**Window.Activate**](https://msdn.microsoft.com/library/windows/apps/BR209046)。 此时会将初始屏幕替换为应用创建的 UI。 如果在激活时有足够的信息可供创建此视觉对象，那么此视觉对象可以是加载屏幕或应用的实际 UI。

> [!div class="tabbedCodeSnippets"]
> ```csharp
> public partial class App : Application
> {
>     // A handler for regular activation.
>     async protected override void OnLaunched(LaunchActivatedEventArgs args)
>     {
>         base.OnLaunched(args);
> 
>         // Asynchronously restore state based on generic launch.
> 
>         // Create the ExtendedSplash screen which serves as a loading page while the
>         // reader downloads the section information.
>         ExtendedSplash eSplash = new ExtendedSplash();
> 
>         // Set the content of the window to the extended splash screen.
>         Window.Current.Content = eSplash;
> 
>         // Notify the Window that the process of activation is completed
>         Window.Current.Activate();
>     }
> 
>     // a different handler for activation via the search contract
>     async protected override void OnSearchActivated(SearchActivatedEventArgs args)
>     {
>         base.OnSearchActivated(args);
> 
>         // Do an asynchronous restore based on Search activation
> 
>         // the rest of the code is the same as the OnLaunched method
>     }
> }
> 
> partial class ExtendedSplash : Page
> {
>     // This is the UIELement that's the game's home page.
>     private GameHomePage homePage;
> 
>     public ExtendedSplash()
>     {
>         InitializeComponent();
>         homePage = new GameHomePage();
>     }
> 
>     // Shown for demonstration purposes only.
>     // This is typically autogenerated by Visual Studio.
>     private void InitializeComponent()
>     {
>     }
> }
> ```
> ```vb
>     Partial Public Class App
>     Inherits Application
> 
>     ' A handler for regular activation.
>     Protected Overrides Async Sub OnLaunched(ByVal args As LaunchActivatedEventArgs)
>         MyBase.OnLaunched(args)
> 
>         ' Asynchronously restore state based on generic launch.
> 
>         ' Create the ExtendedSplash screen which serves as a loading page while the
>         ' reader downloads the section information.
>         Dim eSplash As New ExtendedSplash()
> 
>         ' Set the content of the window to the extended splash screen.
>         Window.Current.Content = eSplash
> 
>         ' Notify the Window that the process of activation is completed
>         Window.Current.Activate()
>     End Sub
> 
>     ' a different handler for activation via the search contract
>     Protected Overrides Async Sub OnSearchActivated(ByVal args As SearchActivatedEventArgs)
>         MyBase.OnSearchActivated(args)
> 
>         ' Do an asynchronous restore based on Search activation
> 
>         ' the rest of the code is the same as the OnLaunched method
>     End Sub
> End Class
> 
> Partial Friend Class ExtendedSplash
>     Inherits Page
> 
>     Public Sub New()
>         InitializeComponent()
> 
>         ' Downloading the data necessary for 
>         ' initial UI on a background thread.
>         Task.Run(Sub() DownloadData())
>     End Sub
> 
>     Private Sub DownloadData()
>         ' Download data to populate the initial UI.
> 
>         ' Create the first page. 
>         Dim firstPage As New MainPage()
> 
>         ' Add the data just downloaded to the first page
> 
>         ' Replace the loading page, which is currently 
>         ' set as the window's content, with the initial UI for the app
>         Window.Current.Content = firstPage
>     End Sub
> 
>     ' Shown for demonstration purposes only.
>     ' This is typically autogenerated by Visual Studio.
>     Private Sub InitializeComponent()
>     End Sub
> End Class 
> ```

在激活处理程序中显示加载页面的应用开始工作，以在后台中创建 UI。 在已创建该元素之后，其 [**FrameworkElement.Loaded**](https://msdn.microsoft.com/library/windows/apps/BR208723) 事件发生。 在事件处理程序中，你将窗口的内容（当前为加载屏幕）替换为新创建的主页。

对于初始化时段比较长的应用，显示加载页面至关重要。 除了提供关于激活过程的用户反馈之外，如果在激活过程开始之后 15 秒内未调用 [**Window.Activate**](https://msdn.microsoft.com/library/windows/apps/BR209046)，将终止激活过程。

> [!div class="tabbedCodeSnippets"]
> ```csharp
> partial class GameHomePage : Page
> {
>     public GameHomePage()
>     {
>         InitializeComponent();
> 
>         // add a handler to be called when the home page has been loaded
>         this.Loaded += ReaderHomePageLoaded;
> 
>         // load the minimal amount of image and sound data from disk necessary to create the home page.        
>     }
>     
>     void ReaderHomePageLoaded(object sender, RoutedEventArgs e)
>     {
>         // set the content of the window to the home page now that it's ready to be displayed.
>         Window.Current.Content = this;
>     }
> 
>     // Shown for demonstration purposes only.
>     // This is typically autogenerated by Visual Studio.
>     private void InitializeComponent()
>     {
>     }
> }
> ```
> ```vb
>     Partial Friend Class GameHomePage
>     Inherits Page
> 
>     Public Sub New()
>         InitializeComponent()
> 
>         ' add a handler to be called when the home page has been loaded
>         AddHandler Me.Loaded, AddressOf ReaderHomePageLoaded
> 
>         ' load the minimal amount of image and sound data from disk necessary to create the home page.        
>     End Sub
> 
>     Private Sub ReaderHomePageLoaded(ByVal sender As Object, ByVal e As RoutedEventArgs)
>         ' set the content of the window to the home page now that it's ready to be displayed.
>         Window.Current.Content = Me
>     End Sub
> 
>     ' Shown for demonstration purposes only.
>     ' This is typically autogenerated by Visual Studio.
>     Private Sub InitializeComponent()
>     End Sub
> End Class
> ```

有关使用延长的初始屏幕的示例，请参阅[初始屏幕示例](http://go.microsoft.com/fwlink/p/?linkid=234889)。

### <a name="phase-3"></a>第 3 阶段

不能仅仅因为应用显示了 UI 就认为它已完全可以使用了。 在我们的游戏示例中，对于需要来自 Internet 的数据的功能，UI 会显示占位符。 此时游戏将下载使应用完全可以正常运行所需的附加数据，并随着数据的获取逐步启用功能。

有时激活所需的许多内容可以与应用打包到一起。 对于简单的游戏，就是如此。 这样，激活过程会很简单。 但许多程序（例如新闻阅读器和照片查看器）必须从 Web 拉信息才能进入正常运行状态。 此数据可能会很大，并会花费相当长的时间来下载。 应用在激活过程中如何获取此数据可能会对用户对该应用的性能感知有巨大影响。

如果应用已尝试下载整个数据集（该应用需要该数据集才能实现激活的第 1 阶段或第 2 阶段中的功能），你可能在数分钟内显示一个加载页面，或者更糟一点，在数分钟内显示一个初始屏幕。 这使应用看起来就像是已挂起，或者会导致系统终止应用。 我们建议应用在第 2 阶段中下载最少数量的数据，以使用占位符元素显示交互式 UI，然后在第 3 阶段中逐步加载数据来替换占位符元素。 有关处理数据的详细信息，请参阅[优化 ListView 和 GridView](optimize-gridview-and-listview.md)。

应用对启动的每个阶段到底做出怎样的反应完全取决于你，但是，为用户提供尽可能多的反馈（初始屏幕、加载屏幕、数据加载时的 UI）会使用户感觉应用和系统作为一个整体而言速度是很快的。

### <a name="minimize-managed-assemblies-in-the-startup-path"></a>最小化启动路径中的托管程序集

可重用的代码经常以在一个项目中包含的多个模块 (DLL) 的形式出现。 加载这些模块要求访问磁盘，你可以想象得出来，这样做会增加开销。 虽然这对冷启动的影响最大，但对热启动同样有影响。 对于 C# 和 Visual Basic，CLR 将通过按需加载程序集尽可能力求延迟该开销。 即，在已执行的方法引用某个模块之前，CLR 不会加载该模块。 因此，请在启动代码中仅引用启动你的应用所必需的程序集，这样 CLR 就不会加载不必要的模块。 如果包含不必要的引用的启动路径中有未使用的代码路径，那么你可以将这些代码路径移动到其他方法，以避免不必要的负载。

减少模块负载的另一个方法是组合你的应用模块。 加载一个大型程序集花费的时间通常比加载两个小型程序集的时间要少。 该方法并非始终可用。并且，仅当组合模块不会对开发人员生产效率或代码可重用性造成实质性影响时，你才应组合模块。 你可以使用 [PerfView](http://go.microsoft.com/fwlink/p/?linkid=251609) 或 [Windows 性能分析器 (WPA)](https://msdn.microsoft.com/library/windows/apps/xaml/ff191077.aspx) 等工具来查明在启动时加载了哪些模块。

### <a name="make-smart-web-requests"></a>发出智能 Web 请求

通过以本地方式将应用的内容（包括 XAML、图像和对应用非常重要的任何其他文件）打包，可极大地缩短应用的加载时间。 磁盘操作的速度快于网络操作。 如果应用在初始化时需要某个特定文件，你可以通过从磁盘加载该文件（而不是从远程服务器检索该文件）来缩短总启动时间。

## <a name="journal-and-cache-pages-efficiently"></a>对页面进行高效日记记录和缓存

帧控件提供导航功能。 该功能提供到 Page 的导航（Navigate 方法）、导航日记记录（BackStack/ForwardStack 属性、GoForward/GoBack 方法）、页面缓存 (Page.NavigationCacheMode) 以及序列化支持（GetNavigationState 方法）。

需要注意的帧性能主要围绕日记记录和页面缓存展开。

**帧日记记录**。 当导航到带有 Frame.Navigate() 的页面时，当前页面的 PageStackEntry 将添加到 Frame.BackStack 集合中。 PageStackEntry 相对较小，但并未针对 BackStack 集合的大小内置任何限制。 用户可以循环导航，并且可以无限增大该集合。

PageStackEntry 还包括已传递给 Frame.Navigate() 方法的参数。 建议将该参数作为原始可序列化类型（如整数或字符串），以便 Frame.GetNavigationState() 方法可以正常运行。 不过，该参数可能会引用占用大量工作集或其他资源的对象，从而使 BackStack 中每个项所需的开销变得更大。 例如，你可能会将 StorageFile 用作参数，而使得 BackStack 将若干个文件保持打开。

因此，建议使导航参数保持较小，并限制 BackStack 的大小。 BackStack 是一个标准矢量（在 C# 中为 IList，而在 C++/CX 中则为 Platform::Vector），因此可以仅通过删除项来对其进行剪裁。

**页面缓存**。 默认情况下，当使用 Frame.Navigate 方法导航到页面时，将为该页面实例化新的实例。 同样，如果你使用 Frame.GoBack 导航回之前的页面，将为该页面分配新的实例。

而帧将提供一个可选的页面缓存来避免这些实例化。 若要将某一页面放入缓存，请使用 Page.NavigationCacheMode 属性。 如果将该模式设置为“Required”，将强制缓存页面；如果将该模式设置为“Enabled”，则允许缓存页面。 默认情况下，缓存大小为 10 个页面，不过可以使用 Frame.CacheSize 属性进行重写。 将缓存所有 Required 页面，如果该缓存的页面数少于 CacheSize Required 页面数，还将缓存 Enabled 页面。

页面缓存通过避免实例化来改善性能，进而提高导航性能。 如果过度缓存，页面缓存可能会降低性能，进而会对工作集造成影响。

因此，建议根据你的应用程序使用页面缓存。 例如，假设你有一个显示帧中的项目列表的应用，则当你点击某一项目时，它会将该帧定位到该项目的详细信息页面。 列表页面应该可以设置为缓存。 如果详细信息页面对所有项目都是相同的，它应该也可以缓存。 但是，如果详细信息页面较为异类，最好关闭缓存。
