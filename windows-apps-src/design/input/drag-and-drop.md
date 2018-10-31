---
description: 本文说明了如何将拖放操作添加到你的通用 Windows 平台 (UWP) 应用中。
title: 拖放
ms.assetid: A15ED2F5-1649-4601-A761-0F6C707A8B7E
author: msatranjr
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5dde0607b76d36b24c851c705ca62f71df052f65
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/31/2018
ms.locfileid: "5838836"
---
# <a name="drag-and-drop"></a>拖放

拖放是在应用程序内部或在 Windows 桌面上的应用程序之间传输数据的一种直观方式。 拖放操作可以让用户使用标准手势（用手指按住并平移或用鼠标或触笔按住并平移）在应用程序之间或在应用程序内部传输数据。

> **重要 API**：[CanDrag 属性](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.CanDrag)、[AllowDrop 属性](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.AllowDrop) 

拖动源（即触发拖动手势的应用程序或区域）通过填充数据包对象提供要传输的数据，而该数据包对象可以包含标准数据格式，包括文本、RTF、HTML、位图、存储项目或自定义数据格式。 源还指示其支持的操作种类：复制、移动或链接。 释放指针后，即会进行放置。 拖放目标（即指针下面的应用程序或区域）将会处理数据包并返回所执行操作的类型。

在拖放过程中，拖动 UI 以可视方式指示正在进行的拖放操作的类型。 此可视反馈最初由源提供，但当指针移动到目标上方时，可以由目标进行更改。

支持 UWP 的所有设备上都可以使用现代拖放操作。 它允许在任何种类的应用程序（包括经典 Windows 应用）内部或应用程序之间传输数据，但本文重点介绍适用于现代拖放操作的 XAML API。 实现后，拖放会在所有方向上无缝运行，包括应用到应用、应用到桌面和桌面到应用。

下面概述了在应用中启用拖放需要执行的操作：

1. 通过将某个元素的 **CanDrag** 属性设置为 true，在该元素上启用拖动。  
2. 构建数据包。 系统会自动处理图像和文本，但对于其他内容，你需要处理 **DragStarted** 和 **DragCompleted** 事件并使用它们构造你自己的数据包。 
3. 通过在可以接收所放置内容的所有元素上将 **AllowDrop** 属性设置为 **true**，启用放置。 
4. 处理 **DragOver** 事件，以便让系统知道元素可以接受何种类型的拖动操作。 
5. 处理 **Drop** 事件以接受放置的内容。 



## <a name="enable-dragging"></a>启用拖动

若要在某个元素上启用拖动，请将其 [**CanDrag**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.CanDrag) 属性设置为 **true**。 这会使该元素（以及在像 ListView 这样的集合的情况下它所包含的元素）可以进行拖动。

明确什么是可以拖动的。 用户并不希望在你的应用中拖动所有内容，只希望拖动某些项目，如图像或文本。 

下面显示了如何设置 [**CanDrag**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.CanDrag)。

[!code-xml[Main](./code/drag_drop/cs/MainPage.xaml#SnippetDragArea)]

你不必执行其他任何操作即可允许拖动，除非你想要自定义 UI（将在本文后面介绍）。 执行放置操作还需要几个步骤。

## <a name="construct-a-data-package"></a>构造数据包 

在大多数情况下，系统会为你构造数据包。 系统自动处理：
* 图像
* 文本 

对于其他内容，你需要处理 **DragStarted** 和 **DragCompleted** 事件并使用它们来构造你自己的 [DataPackage](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackage)。

## <a name="enable-dropping"></a>启用放置

以下标记显示了如何在 XAML 中使用 [**AllowDrop**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.AllowDrop) 将应用的特定区域设置为放置操作的有效区域。 如果用户尝试释放到别处，则系统将不会允许他们如此操作。 如果你希望用户能够将项目放置到应用上的任何位置，请将整个背景设置为拖放目标。

[!code-xml[Main](./code/drag_drop/cs/MainPage.xaml#SnippetDropArea)]


## <a name="handle-the-dragover-event"></a>处理 DragOver 事件

当用户将某个项拖动到你的应用上方但尚未释放时，将触发 [**DragOver**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.DragOver) 事件。 在此处理程序中，你需要使用 [**AcceptedOperation**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.DragEventArgs.AcceptedOperation) 属性指定你的应用支持哪些类型的操作。 复制是最常见的操作。

[!code-cs[Main](./code/drag_drop/cs/MainPage.xaml.cs#SnippetGrid_DragOver)]

## <a name="process-the-drop-event"></a>处理 Drop 事件

当用户在有效的释放区域中释放项目时，将发生 [**Drop**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.Drop) 事件。 使用 [**DataView**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.DragEventArgs.DataView) 属性处理它们。

为简单起见，在下面的示例中，我们假设用户已放置一张照片并直接进行访问。 实际上，用户可以同时放置格式不同的多个项目。 你的应用应通过以下方式处理这种可能情况：检查已放置文件的类型和数量并相应地进行处理。 如果用户正在尝试执行你的应用不支持的操作，你还应考虑通知用户。

[!code-cs[Main](./code/drag_drop/cs/MainPage.xaml.cs#SnippetGrid_Drop)]

## <a name="customize-the-ui"></a>自定义 UI

系统提供用于拖放的默认 UI。 但是，你还可以选择通过设置自定义字幕和字形，或者选择完全不显示 UI 来自定义 UI 的各个部分。 若要自定义 UI，请使用 [**DragEventArgs.DragUIOverride**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.DragEventArgs.DragUIOverride) 属性。

[!code-cs[Main](./code/drag_drop/cs/MainPage.xaml.cs#SnippetGrid_DragOverCustom)]

## <a name="open-a-context-menu-on-an-item-you-can-drag-with-touch"></a>在可通过触摸拖动的项目上打开上下文菜单

在使用触摸时，拖动 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement) 和打开其上下文菜单共享类似的触摸手势；每个都以长按开始。 下面介绍对于应用中同时支持两种操作的元素，系统如何区分两者： 

* 如果用户长按某个项目并在 500 毫秒内开始拖动它，将拖动项目，但不显示上下文菜单。 
* 如果用户长按但未在 500 毫秒内拖动，将打开上下文菜单。 
* 在上下文菜单打开后，如果用户尝试拖动项目（不抬起手指），将取消上下文菜单，并且拖动将开始。

## <a name="designate-an-item-in-a-listview-or-gridview-as-a-folder"></a>将 ListView 或 GridView 中的某个项目指定为文件夹

可以将 [**ListViewItem**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.ListViewItem) 或 [**GridViewItem**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.GridViewItem) 指定为文件夹。 这对树视图和文件资源管理器方案尤其有用。 若要执行此操作，请在该项目上将 [**AllowDrop**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.AllowDrop) 属性显式设置为 **True**。 

系统将自动显示释放到文件夹和非文件夹项目的相应动画。 你的应用代码必须继续处理文件夹项目上（以及非文件夹项目上）的 [**Drop**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.Drop) 事件，以便更新数据源并将释放的项目添加到目标文件夹。

## <a name="implementing-custom-drag-and-drop"></a>实现自定义拖放

[UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) 类会为你完成实现拖放操作的大部分工作。 但是，如果需要，你可以使用[Windows.ApplicationModel.DataTransfer.DragDrop.Core 命名空间](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.datatransfer.dragdrop.core)中的 Api 实现你自己的版本。

| 功能 | WinRT API |
| --- | --- |
|  启用拖动 | [CoreDragOperation](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.dragdrop.core.coredragoperation)  |
|  创建数据包 | [DataPackage](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackage)  |
| 将拖动切换到 shell  | [CoreDragOperation.StartAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.dragdrop.core.coredragoperation)  |
| 从 shell 接收放置  | [CoreDragDropManager](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.dragdrop.core.coredragdropmanager)<br/>[ICoreDropOperationTarget](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.dragdrop.core.icoredropoperationtarget)    |



## <a name="see-also"></a>另请参阅

* [App-to-app communication](index.md)
* [AllowDrop](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.allowdrop.aspx)
* [CanDrag](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.candrag.aspx)
* [DragOver](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.dragover.aspx)
* [AcceptedOperation](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.drageventargs.acceptedoperation.aspx)
* [DataView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.drageventargs.dataview.aspx)
* [DragUIOverride](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.drageventargs.draguioverride.aspx)
* [Drop](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.drop.aspx)
* [IsDragSource](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.isdragsource.aspx)
