---
author: Karl-Bridge-Microsoft
Description: The core text APIs in the Windows.UI.Text.Core namespace enable a Universal Windows Platform (UWP) app to receive text input from any text service supported on Windows devices.
title: 自定义文本输入概述
ms.assetid: 58F5F7AC-6A4B-45FC-8C2A-942730FD7B74
label: Custom text input
template: detail.hbs
keywords: 键盘, 文本, 核心文本, 自定义文本, 文本服务框架, 输入, 用户交互
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 14a2811f59b8de33db51b255aee8892abf553198
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/02/2018
ms.locfileid: "5935842"
---
# <a name="custom-text-input"></a>自定义文本输入



[**Windows.UI.Text.Core**](https://msdn.microsoft.com/library/windows/apps/dn958238) 命名空间中的核心文本 API 支持通用 Windows 平台 (UWP) 应用通过 Windows 设备上受支持的任何文本服务接收文本输入。 该 API 类似于[文本服务框架](https://msdn.microsoft.com/library/windows/desktop/ms629032) API，因此应用不需要详细了解该文本服务。 这使应用接收的文本可以是任何语言以及来自任何输入类型，例如键盘、语音或笔。

> **重要 API**：[**Windows.UI.Text.Core**](https://msdn.microsoft.com/library/windows/apps/dn958238)、[**CoreTextEditContext**](https://msdn.microsoft.com/library/windows/apps/dn958158)

## <a name="why-use-core-text-apis"></a>为什么使用核心文本 API？


对于许多应用而言，XAML 或 HTML 文本框控件对于文本输入和编辑已够用。 然而，如果你的应用需要处理复杂文本方案（如字处理应用），你可能需要自定义文本编辑控件的灵活性。 你可以使用 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) 键盘 API 来创建你的文本编辑控件，但是这些控件无法提供某种方式来接收基于合成的文本输入（它需要支持东亚语言）。

在你需要创建自定义文本编辑控件时，请改用 [**Windows.UI.Text.Core**](https://msdn.microsoft.com/library/windows/apps/dn958238) API。 这些 API 旨在向你提供处理文本输入时以及在任何语言方面的大量灵活性，并提供最适合你的应用的文本体验。 使用核心文本 API 生成的文本输入和编辑控件可以接收来自 Windows 设备上的所有现有文本输入法的文本输入，从电脑上基于[文本服务框架](https://msdn.microsoft.com/library/windows/desktop/ms629032)的输入法编辑器 (IME) 和手写输入到移动设备上的 WordFlow 键盘（该键盘可以提供自动更正、预测和听写）。

## <a name="architecture"></a>体系结构


下面简单介绍了文本输入系统。

-   “应用程序”表示托管使用核心文本 API 生成的自定义编辑控件的 UWP 应用。
-   [**Windows.UI.Text.Core**](https://msdn.microsoft.com/library/windows/apps/dn958238) API 有助于通过 Windows 与文本服务进行通信。 文本编辑控件和文本服务之间的通信主要通过 [**CoreTextEditContext**](https://msdn.microsoft.com/library/windows/apps/dn958158) 对象进行处理，该对象提供的方法和事件有利于通信。

![核心文本体系结构图示](images/coretext/architecture.png)

## <a name="text-ranges-and-selection"></a>文本范围和选择


编辑控件提供的空间可用于文本输入，并且用户可在此空间的任意位置编辑文本。 我们将在此处介绍核心文本 API 所使用的文本定位系统，以及范围和选定在此系统中的表示方式。

### <a name="application-caret-position"></a>应用程序插入光标位置

与核心文本 API 一起使用的文本范围以插入光标位置的形式表示。 “应用程序插入光标位置 (ACP)”是从零开始的数字，表示紧接插入光标前的文本流从头开始算起的字符数，如下面所示。

![示例文本流图示](images/coretext/stream-1.png)
### <a name="text-ranges-and-selection"></a>文本范围和选择

文本范围和选择由包含两个字段的 [**CoreTextRange**](https://msdn.microsoft.com/library/windows/apps/dn958201) 结构表示：

| 字段                  | 数据类型                                                                 | 说明                                                                      |
|------------------------|---------------------------------------------------------------------------|----------------------------------------------------------------------------------|
| **StartCaretPosition** | **Number** \[JavaScript\] | **System.Int32** \[.NET\] | **int32** \[C++\] | 范围的起始位置是紧接第一个字符前的 ACP。 |
| **EndCaretPosition**   | **Number** \[JavaScript\] | **System.Int32** \[.NET\] | **int32** \[C++\] | 范围的结束位置是紧接最后一个字符后的 ACP。     |

 

例如，在前面显示的文本范围中，范围 \[0, 5\] 可指定单词 “Hello”。 **StartCaretPosition** 必须始终小于或等于 **EndCaretPosition**。 范围 \[5, 0\] 无效。

### <a name="insertion-point"></a>插入点

通常称为插入点的当前插入光标位置通过将 **StartCaretPosition** 设置为等于 **EndCaretPosition** 来表示。

### <a name="noncontiguous-selection"></a>非连续选择

一些编辑控件支持非连续选择。 例如，Microsoft Office 应用支持多个任意选择，并且许多源代码编辑器支持列选择。 但是，核心文本 API 并不支持非连续选择。 编辑控件必须仅报告单个连续选择（通常是非连续选择的活动子范围）。

例如，考虑此文本流：

![示例文本流图示](images/coretext/stream-2.png) 有两种选择：\[0, 1\] 和 \[6, 11\]。 编辑控件必须仅报告其中一个选择：\[0, 1\] 或 \[6, 11\]。

## <a name="working-with-text"></a>使用文本


[**CoreTextEditContext**](https://msdn.microsoft.com/library/windows/apps/dn958158) 类通过 [**TextUpdating**](https://msdn.microsoft.com/library/windows/apps/dn958176) 事件、[**TextRequested**](https://msdn.microsoft.com/library/windows/apps/dn958175) 事件和 [**NotifyTextChanged**](https://msdn.microsoft.com/library/windows/apps/dn958172) 方法支持 Windows 和编辑控件之间的文本流。

你的编辑控件通过 [**TextUpdating**](https://msdn.microsoft.com/library/windows/apps/dn958176) 事件接收文本，这些事件在用户与文本输入法（如键盘、语音或 IME）交互时生成。

当在你的编辑控件中更改文本时（例如，通过将文本粘贴到该控件中），需要通过调用 [**NotifyTextChanged**](https://msdn.microsoft.com/library/windows/apps/dn958172) 来通知 Windows。

如果文本服务需要新的文本，将引发 [**TextRequested**](https://msdn.microsoft.com/library/windows/apps/dn958175) 事件。 必须在 **TextRequested** 事件处理程序中提供新的文本。

### <a name="accepting-text-updates"></a>接受文本更新

由于文本更新请求表示用户想要输入的文本，因此你的编辑控件通常应接受这些请求。 在 [**TextUpdating**](https://msdn.microsoft.com/library/windows/apps/dn958176) 事件处理程序中，这些操作按你的编辑控件要求进行：

1.  将 [**CoreTextTextUpdatingEventArgs.Text**](https://msdn.microsoft.com/library/windows/apps/dn958236) 中指定的文本插入 [**CoreTextTextUpdatingEventArgs.Range**](https://msdn.microsoft.com/library/windows/apps/dn958234) 中指定的位置。
2.  将选定内容放置在 [**CoreTextTextUpdatingEventArgs.NewSelection**](https://msdn.microsoft.com/library/windows/apps/dn958233) 中指定的位置。
3.  通知系统更新已成功，方法为将 [**CoreTextTextUpdatingEventArgs.Result**](https://msdn.microsoft.com/library/windows/apps/dn958235) 设置为 [**CoreTextTextUpdatingResult.Succeeded**](https://msdn.microsoft.com/library/windows/apps/dn958237)。

例如，这是编辑控件在用户键入“d”之前的状态。 该插入点位于 \[10, 10\]。

![示例文本流图示](images/coretext/stream-3.png) 当用户键入“d”时，将引发 [**TextUpdating**](https://msdn.microsoft.com/library/windows/apps/dn958176) 事件并带有以下 [**CoreTextTextUpdatingEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn958229) 数据：

-   [**Range**](https://msdn.microsoft.com/library/windows/apps/dn958234) = \[10, 10\]
-   [**Text**](https://msdn.microsoft.com/library/windows/apps/dn958236) = "d"
-   [**NewSelection**](https://msdn.microsoft.com/library/windows/apps/dn958233) = \[11, 11\]

在你的编辑控件中，应用指定的更改并将 [**Result**](https://msdn.microsoft.com/library/windows/apps/dn958235) 设置为 **Succeeded**。 下面是该控件在应用更改后的状态。

![示例文本流图示](images/coretext/stream-4.png)
### <a name="rejecting-text-updates"></a>拒绝文本更新

由于请求的范围是不应进行更改的编辑控件区域，因此有时无法应用文本更新。 在这种情况下，不应该应用任何更改。 相反，应通知系统更新失败，方法为将 [**CoreTextTextUpdatingEventArgs.Result**](https://msdn.microsoft.com/library/windows/apps/dn958235) 设置为 [**CoreTextTextUpdatingResult.Failed**](https://msdn.microsoft.com/library/windows/apps/dn958237)。

例如，请考虑只接受电子邮件地址的编辑控件。 由于电子邮件地址不能含有空格，因此应拒绝空格；以便在针对空格键引发 [**TextUpdating**](https://msdn.microsoft.com/library/windows/apps/dn958176) 事件时，只需在你的编辑控件中将 [**Result**](https://msdn.microsoft.com/library/windows/apps/dn958235) 设置为 **Failed**。

### <a name="notifying-text-changes"></a>通知文本更改

有时，你的编辑控件会对文本进行更改，例如在粘贴或自动更正文本时。 在这些情况下，必须通过调用 [**NotifyTextChanged**](https://msdn.microsoft.com/library/windows/apps/dn958172) 方法向文本服务通知这些更改。

例如，这是编辑控件在用户粘贴“World”之前的状态。 该插入点位于 \[6, 6\]。

![示例文本流图示](images/coretext/stream-5.png) 用户执行粘贴操作，编辑控件结束时会带有以下文本：

![示例文本流图示](images/coretext/stream-4.png) 当发生此情况时，应调用带有以下参数的 [**NotifyTextChanged**](https://msdn.microsoft.com/library/windows/apps/dn958172)：

-   *modifiedRange* = \[6, 6\]
-   *newLength* = 5
-   *newSelection* = \[11, 11\]

按顺序处理陆续生成的一个或多个 [**TextRequested**](https://msdn.microsoft.com/library/windows/apps/dn958175) 事件以更新文本服务所使用的文本。

### <a name="overriding-text-updates"></a>覆盖文本更新

在你的编辑控件中，你可能希望覆盖文本更新以提供自动更正功能。

例如，请考虑提供支持形式化缩略的更正功能的编辑控件。 这是编辑控件在用户键入空格键以触发更正之前的状态。 该插入点位于 \[3, 3\]。

![示例文本流图示](images/coretext/stream-6.png) 用户按空格键，将引发相应的 [**TextUpdating**](https://msdn.microsoft.com/library/windows/apps/dn958176) 事件。 编辑控件将接受文本更新。 这是编辑控件在完成更正之前所处的短暂状态。 该插入点位于 \[4, 4\]。

![示例文本流图示](images/coretext/stream-7.png) 在 [**TextUpdating**](https://msdn.microsoft.com/library/windows/apps/dn958176) 事件处理程序之外，编辑控件将进行以下更正。 这是编辑控件在完成更正之后的状态。 该插入点位于 \[5, 5\]。

![示例文本流图示](images/coretext/stream-8.png) 当发生此情况时，应调用带有以下参数的 [**NotifyTextChanged**](https://msdn.microsoft.com/library/windows/apps/dn958172)：

-   *modifiedRange* = \[1, 2\]
-   *newLength* = 2
-   *newSelection* = \[5, 5\]

按顺序处理陆续生成的一个或多个 [**TextRequested**](https://msdn.microsoft.com/library/windows/apps/dn958175) 事件以更新文本服务所使用的文本。

### <a name="providing-requested-text"></a>提供请求的文本

请务必确保文本服务具有正确的文本，特别是已存在于编辑控件中的文本（例如，通过加载文档，或由编辑控件插入的文本（如前面的部分所述），以便提供诸如自动更正或预测的功能。 因此，每当引发 [**TextRequested**](https://msdn.microsoft.com/library/windows/apps/dn958175) 事件时，都必须向当前在你的编辑控件中的文本提供指定范围。

[**CoreTextTextRequest**](https://msdn.microsoft.com/library/windows/apps/dn958227) 中的 [**Range**](https://msdn.microsoft.com/library/windows/apps/dn958221) 可以多次指定你的编辑控件不能按原样容纳的某个范围。 例如，**Range** 大于发生 [**TextRequested**](https://msdn.microsoft.com/library/windows/apps/dn958175) 事件时的编辑控件的大小，或者 **Range** 的末尾超出范围。 在这些情况下，应返回有意义的任何范围，该范围通常是请求的范围的子集。

## <a name="related-articles"></a>相关文章

**示例**
* [自定义编辑控件示例](https://go.microsoft.com/fwlink/?linkid=831024)
**存档示例**
* [XAML 文本编辑示例](http://go.microsoft.com/fwlink/p/?LinkID=251417)


