---
description: 本文说明了如何将拖放操作添加到你的通用 Windows 平台 (UWP) 应用中。
title: 拖放
ms.assetid: A15ED2F5-1649-4601-A761-0F6C707A8B7E
author: awkoren
---
# 拖放

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


本文说明了如何将拖放操作添加到你的通用 Windows 平台 (UWP) 应用中。 拖放是一种与内容（例如图像和文件）交互的经典且自然的方法。 实现后，拖放会在所有方向上无缝运行，包括应用到应用、应用到桌面和桌面到应用。

## 设置有效区域

使用 [**AllowDrop**][AllowDrop] 和 [**CanDrag**][CanDrag] 属性指定用于拖放操作的有效应用区域。

下面的标记显示了如何将特定应用区域设置为在 XAML 中使用 [**AllowDrop**][AllowDrop] 的释放操作的有效区域。 如果用户尝试释放到别处，则系统将不会允许他们如此操作。 如果你希望用户能够将项目释放到应用上的任何位置，请将整个背景设置为释放目标。

[!code-xml[Main](./code/drag_drop/cs/MainPage.xaml#SnippetDropArea)]

在拖动时，通常需要具体了解哪些内容可拖动。 用户希望拖动某些项目（如图片），而不是应用中的全部内容。 下面是使用 XAML 设置 [**CanDrag**][CanDrag] 的方法。

[!code-xml[Main](./code/drag_drop/cs/MainPage.xaml#SnippetDragArea)]

你不必执行其他任何操作即可允许拖动，除非你想要自定义 UI（将在本文后面介绍）。 执行释放操作还需要几个步骤。

## 处理 DragOver 事件

当用户将某个项拖动到你的应用上方但尚未释放时，将触发 [**DragOver**][DragOver] 事件。 在此处理程序中，你需要使用 [**DragEventArgs.AcceptedOperation**][AcceptedOperation] 属性指定你的应用支持哪些类型的操作。 复制是最常见的操作。

[!code-cs[Main](./code/drag_drop/cs/MainPage.xaml.cs#SnippetGrid_DragOver)]

## 处理 Drop 事件

当用户在有效的释放区域中释放项目时，将发生 [**Drop**][Drop] 事件。 使用 [**DragEventArgs.DataView**][DataView] 属性处理它们。

为简单起见，在下面的示例中，我们假设用户已释放一张照片并进行访问。 实际上，用户可以同时释放格式不同的多个项目。 你的应用应通过以下方式处理这种可能情况：检查已释放何种类型的文件并相应地处理它们，在用户尝试执行应用不支持的操作时通知他们。

[!code-cs[Main](./code/drag_drop/cs/MainPage.xaml.cs#SnippetGrid_Drop)]

## 自定义 UI

系统提供用于拖放的默认 UI。 但是，你还可以选择通过设置自定义字幕和字形，或者选择完全不显示 UI 来自定义 UI 的各个部分。 若要自定义 UI，请使用 [**DragOver**][DragOver] 事件处理程序中的 [**DragUIOverride**][DragUiOverride] 属性。

[!code-cs[Main](./code/drag_drop/cs/MainPage.xaml.cs#SnippetGrid_DragOverCustom)]

 <!-- LINKS -->
[AllowDrop]: https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.ui.xaml.uielement.allowdrop.aspx
[CanDrag]: https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.ui.xaml.uielement.candrag.aspx
[DragOver]: https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.ui.xaml.uielement.dragover.aspx
[AcceptedOperation]: https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.ui.xaml.drageventargs.acceptedoperation.aspx
[DataView]: https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.ui.xaml.drageventargs.dataview.aspx
[DragUiOverride]: https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.ui.xaml.drageventargs.draguioverride.aspx
[Drop]: https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.ui.xaml.uielement.drop.aspx 



<!--HONumber=Mar16_HO5-->


