---
title: Windows 运行时 8.x 到 UWP 案例研究：Bookstore1
ms.assetid: e4582717-afb5-4cde-86bb-31fb1c5fc8f3
description: 本主题介绍了将移植到 Windows 10 通用 Windows 平台 (UWP) 应用非常简单的通用 8.1 应用的案例研究。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: fad621eb9b6ef9ef863240644ae0fa46d41baacc
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371606"
---
# <a name="windows-runtime-8x-to-uwp-case-study-bookstore1"></a>Windows 运行时 8.x 到 UWP 案例研究：Bookstore1


本主题介绍了将移植到 Windows 10 通用 Windows 平台 (UWP) 应用非常简单的通用 8.1 应用的案例研究。 一个通用的 8.1 应用程序是指生成一个应用程序包以进行 Windows 8.1 和 Windows Phone 8.1 的不同的应用包。 Windows 10 中，您可以创建单个应用包，你的客户可以将安装到各种设备，并且这就是我们要在此案例研究。 请参阅 [UWP 应用指南](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide)。

我们将移植的应用包含绑定到视图模型的 **ListBox**。 该视图模型具有显示标题、作者和书籍封面的书籍列表。 书籍封面已将**生成操作**设置为**内容**，并将**复制到输出目录**设置为**不要复制**。

本部分中之前的主题介绍了平台之间的差异，并且提供有关将应用的各个方面从 XAML 标记移植到访问数据（通过绑定到视图模型）这一过程的详细信息和指南。 案例研究旨在通过在真实示例中实际显示指南来补充该指南。 案例研究假设你已阅读该指南，因此不会重复该指南。

**请注意**  时打开 Bookstore1Universal\_10 在 Visual Studio 中，如果看到消息"需要 Visual Studio 更新"，然后按照中的步骤[TargetPlatformVersion](w8x-to-uwp-troubleshooting.md)。

## <a name="downloads"></a>下载

[下载 Bookstore1\_81 通用 8.1 应用](https://go.microsoft.com/fwlink/?linkid=532946)。

[下载 Bookstore1Universal\_10 个 Windows 10 应用](https://go.microsoft.com/fwlink/?linkid=532950)。

## <a name="the-universal-81-app"></a>通用 8.1 应用

还有哪些 Bookstore1\_81-我们要到端口的应用，如下所示。 它只是一个垂直滚动的书籍列表框，位于应用名称和页面标题下。

![如何 bookstore1\-81 在 windows 上的外观](images/w8x-to-uwp-case-studies/c01-01-win81-how-the-app-looks.png)

Bookstore1\_上 Windows 81

![如何 bookstore1\-81 在 windows phone 上的外观](images/w8x-to-uwp-case-studies/c01-02-wp81-how-the-app-looks.png)

Bookstore1\_Windows Phone 上的 81

##  <a name="porting-to-a-windows10-project"></a>移植到 Windows 10 项目

Bookstore1\_81 解决方案是 8.1 通用应用项目，并且它包含这些项目。

-   Bookstore1\_81.Windows。 这是为 Windows 8.1 生成的应用包的项目。
-   Bookstore1\_81.WindowsPhone。 这是为 Windows Phone 8.1 生成应用包的项目。
-   Bookstore1\_81.Shared。 这是包含由其他两个项目同时使用的源代码、标记文件以及其他资源的项目。

对于此案例研究，我们针对要支持的设备提供[如果你有通用 8.1 应用](w8x-to-uwp-root.md)中所述的常用选项。 此处的决定是一个简单： 此应用具有相同的功能，并因此大多数都涉及到相同的代码，在其 Windows 8.1 和 Windows Phone 8.1 的窗体中执行。 因此，我们将端口共享项目 （和其他任何我们需要从其他项目） 的内容连接到 Windows 10 为目标的通用设备系列 （一个可以安装到广泛的设备上）。

它是非常快速的任务，用于创建新的项目在 Visual Studio 中，复制文件转移到它从 Bookstore1\_81，并将复制的文件包括在新项目。 首先创建一个新的空白应用程序（Windows 通用）项目。 其命名为 Bookstore1Universal\_10。 这些是要通过从 Bookstore1 复制的文件\_81 到 Bookstore1Universal\_10。

**从共享项目**

-   将复制包含书籍封面图像 PNG 文件的文件夹 (文件夹是\\资产\\CoverImages)。 复制该文件夹后，在 **“解决方案资源管理器”** 中，请确保将 **“显示所有文件”** 切换为打开。 右键单击你复制的文件夹，然后单击**包括在项目中**。 该命令的意思是将文件或文件夹“包括”在某个项目中。 每次你复制文件或文件夹、每个副本时，请在**解决方案资源管理器**中单击**刷新**，然后将文件或文件夹包括在项目中。 无需为你将在目标位置替换的文件执行此操作。
-   将复制包含视图模型的源文件的文件夹 (文件夹是\\ViewModel)。
-   复制 MainPage.xaml 并替换目标位置中的文件。

**从 Windows 项目**

-   复制 BookstoreStyles.xaml。 我们将使用此一个良好起点，因为此文件中的所有资源密钥将都解决在 Windows 10 应用程序中;等效 WindowsPhone 文件中的其中一些则不会。

编辑你刚才复制的源代码和标记文件，更改任何引用到 Bookstore1\_81 命名空间到 Bookstore1Universal\_10。 执行此操作的快速方法是使用**在文件中替换**功能。 视图模型中和任何其他强制性代码中都不需要更改任何代码。 不过，只是为了更加轻松地了解哪个版本的应用程序是否正在运行，更改返回的值**Bookstore1Universal\_10.BookstoreViewModel.AppName**属性从"BOOKSTORE1\_81"到"BOOKSTORE1UNIVERSAL\_10"。

现在，你可以执行生成和运行操作。 下面是如何完成后没有显式要尚未移植到 Windows 10 后看起来我们新的 UWP 应用。

![初始源代码发生更改的 Windows 10 应用](images/w8x-to-uwp-case-studies/c01-03-desk10-initial-source-code-changes.png)

Windows 10 应用程序与桌面设备上运行的初始源的代码更改

![初始源代码发生更改的 Windows 10 应用](images/w8x-to-uwp-case-studies/c01-04-mob10-initial-source-code-changes.png)

Windows 10 应用的移动设备上运行的初始源的代码更改

视图和视图模型正确地协同工作，并且 **ListBox** 处于运行状态。 我们只需修复样式。 在浅色主题的移动设备中，我们可以看到列表框的边框，但是该边框很容易处于隐藏状态。 并且版式太大，因此我们将更改我们正在使用的样式。 此外，如果我们希望应用看起来像默认状态，应用在桌面设备上运行时应使用浅色。 因此，我们将更改该设置。

## <a name="universal-styling"></a>通用样式设置

Bookstore1\_81 应用使用两个不同的资源字典 (BookstoreStyles.xaml) 来定制其样式应用于 Windows 8.1 和 Windows Phone 8.1 的操作系统。 这两个 BookstoreStyles.xaml 文件不包含完全我们需要对 Windows 10 应用的样式。 但好消息是，我们所需要的实际上比它们中的任意一个都要简单得多。 因此接下来的步骤将主要涉及到删除和简化我们的项目文件和标记。 具体步骤如下。 并且你可以使用本主题顶部的链接下载这些项目，并查看此处和案例研究末尾之间的所有更改结果。

-   若要缩短项目之间的间距，请在 MainPage.xaml 中查找 `BookTemplate` 数据模板并从根 **Grid** 中删除 `Margin="0,0,0,8"`。
-   同样在 `BookTemplate` 中，存在对 `BookTemplateTitleTextBlockStyle` 和 `BookTemplateAuthorTextBlockStyle` 的引用。 Bookstore1\_81 作为间接使用这些密钥，以便单个键的两个应用中有不同的实现。 我们不再需要该间接寻址；我们可以直接引用系统样式。 因此使用 `TitleTextBlockStyle` 和 `SubtitleTextBlockStyle` 分别替换这些引用。
-   现在，我们需要将 `LayoutRoot` 的 Background 设置为正确的默认值，以便无论使用何种主题，应用在所有设备上运行时都具有合适的外观。 将其从 `"Transparent"` 更改为 `"{ThemeResource ApplicationPageBackgroundThemeBrush}"`。
-   在 `TitlePanel` 中，将对 `TitleTextBlockStyle` 的引用（现在有点过大）更改为对 `CaptionTextBlockStyle` 的引用。 `PageTitleTextBlockStyle` 是另一个 Bookstore1\_81 我们不需要过更长的间接寻址。 将其更改为引用 `HeaderTextBlockStyle`。
-   我们不再需要在 **ListBox** 上设置任何特殊的 Background、Style 或 ItemContainerStyle，因此只需从标记中删除这三个属性及其值即可。 但是我们需要隐藏 **ListBox** 的边框，以便将 `BorderBrush="{x:Null}"` 添加到其中。
-   我们将不再引用 BookstoreStyles.xaml **ResourceDictionary** 文件中的任何资源。 你可以删除所有这些资源。 但不要删除 BookstoreStyles.xaml 文件本身：我们仍需要最后一次使用它，你将在下一部分中看到。

在样式设置操作的最后一步中，应用的外观如下所示。

![即将完成移植的 Windows 10 应用](images/w8x-to-uwp-case-studies/c01-05-desk10-almost-ported.png)

几乎移植桌面设备上运行 Windows 10 应用

![即将完成移植的 Windows 10 应用](images/w8x-to-uwp-case-studies/c01-06-mob10-almost-ported.png)

几乎移植移动设备上运行 Windows 10 应用

## <a name="an-optional-adjustment-to-the-list-box-for-mobile-devices"></a>对移动设备的列表框的可选调整

当应用在移动设备上运行时，列表框的背景在两种主题下都默认为浅色。 这可能是你偏好的样式，如果确实如此，则除了以下整理工作之外，没有其他要执行的操作：从项目中删除 BookstoreStyles.xaml 资源字典文件，并删除将其合并到 MainPage.xaml 的标记。

但控件的设计目的是你可以自定义它们的外观，同时使其行为不受影响。 因此，如果你希望列表框在深色主题下显示为深色（这是原始应用的外观），本部分介绍了一种可执行该操作的方法。

我们所进行的更改只需要在应用在移动设备上运行时，影响该应用即可。 因此，在移动设备系列上运行时，我们将使用略微自定义的列表框样式，然后我们将在任何其他设备上运行时继续使用默认样式。 为此，我们将创建 BookstoreStyles.xaml 的副本，并且为其分配一个特殊的 MRT 限定名称，这将导致它仅在移动设备上进行加载。

添加一个新的 **ResourceDictionary** 项目项并将其命名为 BookstoreStyles.DeviceFamily-Mobile.xaml。 你现在有两个逻辑名称均为 BookstoreStyles.xaml 的文件（这是你在标记和代码中使用的名称）。 但是，这两个文件具有不同的物理名称，因此它们可以包含不同的标记。 你可以对任何 xaml 文件使用此 MRT 限定的命名方案，但是请注意所有具有相同逻辑名称的 xaml 文件都共享单个 xaml.cs 代码隐藏文件（其中一个适用）。

为该列表框编辑控件模板的副本，并将它与 `BookstoreListBoxStyle` 的键一起存储在新的资源词典 BookstoreStyles.DeviceFamily-Mobile.xaml 中。 现在我们将对三个资源库进行简单的更改。

-   在前台资源库中，将该值更改为 `"{x:Null}"`。 请注意，在元素上将属性直接设置为 `"{x:Null}"` 与使用代码将其设置为 `null` 相同。 但是在资源库中使用 `"{x:Null}"` 的值具有独特的效果：它将以默认样式（对于相同属性）替代资源库并在目标元素上还原属性的默认值。
-   在后台资源库中，将该值更改为 `"Transparent"` 以删除该浅色背景。
-   在模板资源库中，找到名为 `Focused` 的视觉状态并删除其情节提要，使其成为空的标记。
-   从标记中删除所有其他资源库。

最后，将 `BookstoreListBoxStyle` 复制到 BookstoreStyles.xaml 并删除它的三个资源库，使其成为空的标记。 我们执行此操作的效果是，在除了移动设备之外的设备上，我们对 BookstoreStyles.xaml 和 `BookstoreListBoxStyle` 的引用将解析，但不会产生任何影响。

![已移植的 Windows 10 应用](images/w8x-to-uwp-case-studies/c01-07-mob10-ported.png)

移植移动设备上运行的 Windows 10 应用

## <a name="conclusion"></a>结束语

此案例研究介绍了移植非常简单的应用（可以认为是一个过分简单的应用）的过程。 例如，列表框可用于选择或者用于建立导航的上下文；应用导航到具有有关所点击项的更多详细信息的页面。 根据用户的选择，此特定应用不执行任何操作，并且它没有导航。 即便如此，案例研究可用于打破僵局、介绍移植过程和演示可在真实的 UWP App 中使用的重要技术。

我们还看到了移植视图模型过程通常很顺利的证据。 用户界面和外形规格支持是我们在移植时可能更需要注意的方面。

下一个案例研究是 [Bookstore2](w8x-to-uwp-case-study-bookstore2.md)，我们将从中了解访问和显示分组数据。
