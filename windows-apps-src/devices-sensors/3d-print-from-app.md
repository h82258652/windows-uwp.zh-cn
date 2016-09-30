---
author: PatrickFarley
title: "从应用进行 3D 打印"
description: "了解如何向通用 Windows 应用添加 3D 打印功能。 本主题介绍如何启动 3D 打印对话框，但要先确保 3D 模型可打印并且格式正确。"
ms.assetid: D78C4867-4B44-4B58-A82F-EDA59822119C
translationtype: Human Translation
ms.sourcegitcommit: 61d9f5c1fca1ad2e26f052b901361813975ae357
ms.openlocfilehash: e68a9c681974152bc0d4dfa58e824f80e77dc51f

---

# 从应用进行 3D 打印


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 的文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**重要的 API**

-   [**Windows.Graphics.Printing3D**](https://msdn.microsoft.com/library/windows/apps/dn998169)

了解如何向通用 Windows 应用添加 3D 打印功能。 本主题介绍如何将 3D 几何数据加载到应用中以及如何启动 3D 打印对话框，但要先确保 3D 模型可打印并且格式正确。 有关这些操作过程的工作示例，请参阅 [3D 打印 UWP 示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/3DPrinting)。

## 类设置


在将具有 3D 打印功能的类中，添加 [Windows.Graphics.Printing3D](https://msdn.microsoft.com/library/windows/apps/dn998169) 命名空间。

[!code-cs[3DPrintNamespace](./code/3dprinthowto/cs/MainPage.xaml.cs#Snippet3DPrintNamespace)]

将在此特定指南中使用以下其他命名空间：

[!code-cs[OtherNamespaces](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetOtherNamespaces)]

接下来，为你的类提供一些有用的成员字段。 声明一个 [Print3DTask](https://msdn.microsoft.com/library/windows/apps/dn998044) 对象以用作对要传递到打印驱动程序的打印任务的引用。 声明一个 [StorageFile](https://msdn.microsoft.com/library/windows/apps/br227171) 对象以保留原始的 3D 数据文件。 最后，声明一个 [Printing3D3MFPackage](https://msdn.microsoft.com/library/windows/apps/dn998063) 对象，它表示准备打印的包含所有必要元数据的 3D 模型。

[!code-cs[DeclareVars](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetDeclareVars)]

## 创建一个简单的 UI


此示例使用三个用户控件：一个加载按钮（将文件移入程序内存中）、一个修复按钮（将在必要时修改文件）和一个打印按钮（将启动打印作业）。 以下代码会在类的 XAML 文件中生成这些按钮（及其单击事件处理程序）：

[!code-xml[按钮](./code/3dprinthowto/cs/MainPage.xaml#SnippetButtons)]

针对 UI 反馈添加 **TextBlock**。

[!code-xml[OutputText](./code/3dprinthowto/cs/MainPage.xaml#SnippetOutputText)]

## 获取 3D 数据


应用获取要打印的 3D 几何图形数据的方法将有所不同。 应用可能会从 3D 扫描检索数据、从 Web 资源提取模型数据或者使用公式以编程方式生成 3D 网格。 为了简便起见，本指南将从文件资源管理器将 3D 数据文件（几种常见文件类型的任意一种）加载到程序内存中。

在 `OnLoadClick` 方法中，使用 [FileOpenPicker](https://msdn.microsoft.com/library/windows/apps/br207847) 类将单个文件加载到应用的内存中。

[!code-cs[FileLoad](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetFileLoad)]

## 使用 3D Builder 转换为 3D 制造格式 (.3mf)

现在，你可以将 3D 数据文件加载到应用的内存中。 但是，3D 几何图形数据有很多不同的格式，而且并非都适合 3D 打印。 Windows 10 针对所有 3D 打印任务使用 3D 生成格式 (.3mf) 文件类型。

> **注意** 3MF 文件类型提供了大量此教程中未涉及的功能。 若要了解有关 3MF 及其为 3D 产品的生产商和消费者提供的功能的详细信息，请参阅 [3MF 规范](http://3mf.io/what-is-3mf/3mf-specification/)。 若要了解如何使用 Windows 10 API 来充分利用这些功能，请参阅[生成 3MF 程序包](https://msdn.microsoft.com/windows/uwp/devices-sensors/generate-3mf)教程。

幸运的是，[3D Builder](https://www.microsoft.com/store/apps/3d-builder/9wzdncrfj3t6) 应用可以打开最常见 3D 格式的文件并将其另存为 .3mf 文件。 在此示例中，文件类型可能会有所不同，不过有一种简单的解决方法，即打开 3D Builder 并提示用户将导入的数据另存为 .3mf 文件，然后重新加载它。

> **注意** 除了转换文件格式之外，**3D Builder** 还提供了一些简单的工具，可用来编辑模型、添加颜色数据以及执行其他特定于打印的操作，因此将其集成到处理 3D 打印的应用通常很有用。

[!code-cs[FileCheck](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetFileCheck)]

## 修复模型数据以供 3D 打印

并非所有 3D 模型数据都可以进行打印，即使是 .3mf 格式。 为了使打印机能够正确地确定要填充哪些空间，哪些空间留空，要打印的模型必须是一个单一无缝的复杂模型、具有面向外部的表面法线并具有流形几何图形。 这些方面的问题可能会以各种各样的形式突然出现，而且在复杂的形状中难以发现。 幸运的是，当前软件解决方案通常足以将原始几何图形转换为可打印的 3D 形状。 使用 `OnFixClick` 方法即可完成此操作。

3D 数据文件必须经过转换才能实现 [IRandomAccessStream](https://msdn.microsoft.com/library/windows/apps/br241731)，然后才可用于生成 [Printing3DModel](https://msdn.microsoft.com/library/windows/apps/mt203679) 对象。

[!code-cs[RepairModel](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetRepairModel)]

**Printing3DModel** 现已修复并且可打印。 使用 **SaveModelToPackageAsync** 将模型分配给创建类时声明的 Printing3D3MFPackage 对象。

[!code-cs[SaveModel](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetSaveModel)]

## 执行打印任务：创建一个 TaskRequested 处理程序


稍后，在向用户显示 3D 打印对话框并且用户选择开始打印时，应用将需要向 3D 打印管道传递所需的参数。 3D 打印 API 将引发 **TaskRequested** 事件。 必须编写一种方法来适当地处理此事件。 如往常一样，它必须与其事件的类型相同：**TaskRequested** 事件具有参数 [Print3DManager](https://msdn.microsoft.com/library/windows/apps/dn998029)（它的发送方对象）和 [Print3DTaskRequestedEventArgs](https://msdn.microsoft.com/library/windows/apps/dn998051) 对象，这包含大部分相关信息。 其返回类型是 **void**。

[!code-cs[MyTaskTitle](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetMyTaskTitle)]

此方法的核心用途是使用 *args* 对象将 **Printing3D3MFPackage** 向下发送至管道。 **Print3DTaskRequestedEventArgs** 类型有一个属性：**Request**。 它属于 [Print3DTaskRequest](https://msdn.microsoft.com/library/windows/apps/dn998050) 类型，表示一个打印作业请求。 其方法 **CreateTask** 使程序能够为打印作业提交正确的信息，它还会返回一个对 [Print3DTask](https://msdn.microsoft.com/library/windows/apps/dn998044) 对象（将向下发送至 3D 打印管道）的引用。

**CreateTask** 具有以下输入参数：一个用于打印作业名称的**string**、一个用于要使用的打印机 ID 的 **string** 和一个 **Print3DTaskSourceRequestedHandler** 委托。 引发 **3DTaskSourceRequested** 事件时会自动调用该委托（该操作由 API 本身完成）。 需要着重注意的是，该委托会在打印作业启动时调用，它负责提供正确的 3D 打印程序包。

**Print3DTaskSourceRequestedHandler** 将接受一个参数，即可提供要发送的数据的 [Print3DTaskSourceRequestedArgs](https://msdn.microsoft.com/library/windows/apps/dn998056) 对象。 作为此类的一种公共方法，**SetSource** 将接受要打印的程序包。 实现 **Print3DTaskSourceRequestedHandler** 委托，如下所示：

[!code-cs[SourceHandler](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetSourceHandler)]

接下来，使用新定义的委托 `sourceHandler` 调用 **CreateTask**：

[!code-cs[CreateTask](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetCreateTask)]

返回的 **Print3DTask** 会分配给开始时声明的类变量。 你现在可以（可选）使用此引用来处理该任务引发的某些事件：

[!code-cs[可选](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetOptional)]

> **注意** 必须实现 `Task_Submitting` 和 `Task_Completed` 方法才能向这些事件注册它们。

## 执行打印任务：打开 3D 打印对话框


从应用打印所需的最后一位代码是可启动 3D 打印对话框的代码。 与传统打印对话框窗口一样，3D 打印对话框提供了许多必要打印规范，并允许用户选择要使用的打印机（是通过 USB 连接还是通过网络连接）。

首先，向 **TaskRequested** 事件注册 `MyTaskRequested` 方法。

[!code-cs[RegisterMyTaskRequested](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetRegisterMyTaskRequested)]

注册 **TaskRequested** 事件处理程序后，可以调用方法 **ShowPrintUIAsync**，从而在当前应用程序窗口中显示 3D 打印对话框。

[!code-cs[ShowDialog](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetShowDialog)]

最后，最好是在应用恢复控件后取消注册事件处理程序：[!code-cs[DeregisterMyTaskRequested](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetDeregisterMyTaskRequested)]

## 相关主题

[生成 3MF 程序包](https://msdn.microsoft.com/windows/uwp/devices-sensors/generate-3mf)

[3D 打印 UWP 示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/3DPrinting)
 

 







<!--HONumber=Jun16_HO4-->


