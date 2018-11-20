---
author: drewbatgit
ms.assetid: dd2a1e01-c284-4d62-963e-f59f58dca61a
description: 本文介绍如何从设备导入媒体，包括搜索可用的媒体源、导入文件（如照片和 sidecar 文件）以及从源设备中删除导入的文件。
title: 导入媒体
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 36f8956e2ca167d98bb8e6ecf35ce3d131d7b032
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/19/2018
ms.locfileid: "7291827"
---
# <a name="import-media-from-a-device"></a>从设备导入媒体

本文介绍如何从设备导入媒体，包括搜索可用媒体源、导入文件（如视频、照片和 sidecar 文件）以及从源设备中删除导入的文件。

> [!NOTE] 
> 本文中的代码改编自 [**MediaImport UWP 应用示例**](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MediaImport)。 你可以从[**通用 Windows 应用示例 Git 存储库**](https://github.com/Microsoft/Windows-universal-samples)中克隆或下载此示例，以便在上下文中查看代码或将其用作你自己的应用的起始点。

## <a name="create-a-simple-media-import-ui"></a>创建简单的媒体导入 UI
本文中的示例使用最少的 UI 来启用核心媒体导入方案。 若要了解如何为媒体导入应用创建更可靠的 UI，请参阅[**导入示例**](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MediaImport)。 以下 XAML 创建带有以下控件的堆栈面板：
* 一个[**按钮**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Button)，用于发起对可从中导入媒体的源进行搜索的操作。
* 一个[**组合框**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.ComboBox)，用于列出所找到的媒体导入源，并从中进行选择。
* 一个 [**ListView**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.ListView) 控件，用于显示选定导入源中的媒体项，并从中进行选择。
* 一个**按钮**，用于发起从选定源导入媒体项的操作。
* 一个**按钮**，用于发起对已从选定源中导入的项进行删除的操作。
* 一个**按钮**，用于取消异步媒体导入操作。

[!code-xml[ImportXAML](./code/PhotoImport_Win10/cs/MainPage.xaml#SnippetImportXAML)]

## <a name="set-up-your-code-behind-file"></a>设置代码隐藏文件
添加 *using* 指令以包括此示例使用但尚未包括在默认项目模板中的命名空间。

[!code-cs[Using](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetUsing)]

## <a name="set-up-task-cancellation-for-media-import-operations"></a>为媒体导入操作设置任务取消

由于媒体导入操作可能需要很长时间，因此使用 [**IAsyncOperationWithProgress**](https://msdn.microsoft.com/library/windows/apps/br206594.aspx) 异步执行它们。 声明一个 [**CancellationTokenSource**](https://msdn.microsoft.com/library/system.threading.cancellationtokensource) 类型的类成员变量，该变量将用于在用户单击取消按钮时取消正在进行的操作。

[!code-cs[DeclareCts](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetDeclareCts)]

实现取消按钮的处理程序。 本文后面部分介绍的示例将在操作开始时初始化 **CancellationTokenSource**，并在操作完成时将其设置为 null。 在取消按钮处理程序中，检查令牌是否为 null，如果不是，则调用 [**Cancel**](https://msdn.microsoft.com/library/dd321955) 来取消该操作。

[!code-cs[OnCancel](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetOnCancel)]

## <a name="data-binding-helper-classes"></a>数据绑定帮助程序类

在典型的媒体导入方案中，你向用户显示可供导入的媒体项列表，可能有大量的媒体文件可供选择，通常你需要为每个媒体项显示缩略图。 出于此原因，当用户在列表中向下滚动时，此示例使用三个帮助程序类以增量方式将条目加载到 ListView 控件中。

* **IncrementalLoadingBase** 类 - 实现 [**IList**](https://msdn.microsoft.com/library/system.collections.ilist)、[**ISupportIncrementalLoading**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.isupportincrementalloading) 和 [**INotifyCollectionChanged**](https://msdn.microsoft.com/library/windows/apps/system.collections.specialized.inotifycollectionchanged(v=vs.105).aspx) 来提供基本增量加载行为。
* **GeneratorIncrementalLoadingClass** 类 - 提供增量加载基类的实现。
* **ImportableItemWrapper** 类 - [**PhotoImportItem**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportItem) 类周围的一个精简包装器，用于为每个已导入项的缩略图图像添加可绑定的 [**BitmapImage**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Media.Imaging.BitmapImage) 属性。

这些类在 [**MediaImport 示例**中提供，并且无需修改即可添加到你的项目。](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MediaImport) 在将帮助程序类添加到你的项目后，声明一个 **GeneratorIncrementalLoadingClass** 类型的类成员变量，此变量将在本示例后面部分中使用。

[!code-cs[GeneratorIncrementalLoadingClass](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetGeneratorIncrementalLoadingClass)]


## <a name="find-available-sources-from-which-media-can-be-imported"></a>查找可从中导入媒体的可用源

在查找源按钮的单击处理程序中，调用静态方法 [**PhotoImportManager.FindAllSourcesAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportManager.FindAllSourcesAsync) 来针对可从中导入媒体的设备启动系统搜索。 等待该操作完成后，循环访问返回的列表中的每个 [**PhotoImportSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportSource) 对象，并向**组合框** 添加一个条目，从而将 **Tag** 属性设置为源对象本身，以便在用户进行选择时可以轻松地检索它。

[!code-cs[FindSourcesClick](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetFindSourcesClick)]

声明一个类成员变量，用于存储用户选定的导入源。

[!code-cs[DeclareImportSource](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetDeclareImportSource)]

在导入源 **ComboBox** 的 [**SelectionChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Primitives.Selector.SelectionChanged) 处理程序中，将类成员变量设置为选定的源，然后调用 **FindItems** 帮助程序方法，此方法将在本文后面部分中进行介绍。 

[!code-cs[SourcesSelectionChanged](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetSourcesSelectionChanged)]

## <a name="find-items-to-import"></a>查找要导入的项

添加 [**PhotoImportSession**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportSession) 和 [**PhotoImportFindItemsResult**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportFindItemsResult) 类型的类成员变量，这些变量将在以下步骤中使用。

[!code-cs[DeclareImport](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetDeclareImport)]

在 **FindItems** 方法中，初始化 **CancellationTokenSource** 变量，以便可以使用它在必要时取消查找操作。 在 **try** 块内，通过在用户选择的 [**PhotoImportSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportSource) 对象上调用 [**CreateImportSession**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportSource.CreateImportSession) 来创建新的导入会话。 创建新的 [**Progress**](https://msdn.microsoft.com/library/hh193692.aspx) 对象以提供用于显示查找操作进度的回调。 接下来，调用 **[FindItemsAsync](https://docs.microsoft.com/uwp/api/windows.media.import.photoimportsession.finditemsasync)** 以启动查找操作。 提供一个 [**PhotoImportContentTypeFilter**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportContentTypeFilter) 值来指定应返回照片、视频还是两者都返回。 提供一个 [**PhotoImportItemSelectionMode**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportItemSelectionMode) 值来指定是返回全部媒体项、不返回任何媒体项还是仅返回新的媒体项，同时将它们的 [**IsSelected**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportItem.IsSelected) 属性设置为 true。 此属性绑定到我们的 ListBox 项模板中的每个媒体项的复选框。

**FindItemsAsync** 返回 [**IAsyncOperationWithProgress**](https://msdn.microsoft.com/library/windows/apps/br206594.aspx)。 扩展方法 [**AsTask**](https://msdn.microsoft.com/library/hh779750.aspx) 用于创建可等待、可使用取消令牌取消以及使用所提供的 **Progress** 对象报告进度的任务。

接下来，初始化数据绑定帮助程序类 **GeneratorIncrementalLoadingClass**。 当从等待状态返回时，**FindItemsAsync** 会返回 [**PhotoImportFindItemsResult**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportFindItemsResult) 对象。 此对象包含关于查找操作的状态信息，包括操作是否成功以及所找到的不同类型的媒体项的计数。 [**FoundItems**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportFindItemsResult.FoundItems) 属性包含表示所找到的媒体项的 [**PhotoImportItem**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportItem) 对象的列表。 **GeneratorIncrementalLoadingClass** 构造函数将要以增量方式加载的项的总计数以及生成要按需加载的新项的函数作为参数。 在此情况下，所提供的 lambda 表达式创建 **ImportableItemWrapper** 的新实例，该实例包装 **PhotoImportItem** 并包含每个项的缩略图。 初始化增量加载类后，将其设置为 UI 中的 **ListView** 控件的 [**ItemsSource**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.ItemsControl.ItemsSource) 属性。 现在，所找到的媒体项将以增量方式加载，并在列表中显示。

接下来，输出查找操作的状态信息。 典型应用会在 UI 中向用户显示此信息，但本示例仅将该信息输出到调试控制台。 最后，由于操作已完成，将取消令牌设置为 null。

[!code-cs[FindItems](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetFindItems)]

## <a name="import-media-items"></a>导入媒体项

在实现导入操作前，声明一个 [**PhotoImportImportItemsResult**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportImportItemsResult) 对象来存储导入操作的结果。 稍后将使用此对象删除已从源成功导入的媒体项。

[!code-cs[DeclareImportResult](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetDeclareImportResult)]

在启动媒体导入操作前，通过将 [**ProgressBar**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.ProgressBar) 控件的值设置为 0 来初始化 **CancellationTokenSource** 变量。

如果 **ListView** 控件中没有选定的项，则没有可导入的内容。 否则，初始化 [**Progress**](https://msdn.microsoft.com/library/hh193692.aspx) 对象以提供一个进度回调，用于更新进度栏控件的值。 为查找操作返回的 [**PhotoImportFindItemsResult**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportFindItemsResult) 的 [**ItemImported**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportFindItemsResult.ItemImported) 事件注册一个处理程序。 每当导入某个项时都会引发此事件，并且在本示例中，此事件会将每个已导入文件的名称输出到调试控制台。

调用 [**ImportItemsAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportFindItemsResult.ImportItemsAsync) 开始导入操作。 和查找操作一样，[**AsTask**](https://msdn.microsoft.com/library/hh779750.aspx) 扩展方法用于将返回的操作转换为可等待、报告进度和可取消的任务。

在导入操作完成后，可以从 [**ImportItemsAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportFindItemsResult.ImportItemsAsync) 返回的 [**PhotoImportImportItemsResult**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportImportItemsResult) 对象获取操作状态。 本示例将状态信息输出到调试控制台，然后最终将取消令牌设置为 null。

[!code-cs[ImportClick](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetImportClick)]

## <a name="delete-imported-items"></a>删除导入的项
若要从导入项的源中删除成功导入的项，请先初始化取消令牌，以便可以取消删除操作，并将进度栏值设置为 0。 确保从 [**ImportItemsAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportFindItemsResult.ImportItemsAsync) 返回的 [**PhotoImportImportItemsResult**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportImportItemsResult) 不为 null。 如果不是，则再次创建一个 [**Progress**](https://msdn.microsoft.com/library/hh193692.aspx) 对象来为删除操作提供进度回调。 调用 [**DeleteImportedItemsFromSourceAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportImportItemsResult.DeleteImportedItemsFromSourceAsync) 以开始删除导入的项。 使用 **AsTask** 将结果转换为具有进度和取消功能的可等待任务。 在等待后，返回的 [**PhotoImportDeleteImportedItemsFromSourceResult**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportDeleteImportedItemsFromSourceResult) 对象可用于获取和显示关于删除操作的状态信息。

[!code-cs[DeleteClick](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetDeleteClick)]








 


