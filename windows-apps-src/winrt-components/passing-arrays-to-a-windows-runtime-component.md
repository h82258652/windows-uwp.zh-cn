---
title: 将数组传递到 Windows 运行时组件
description: 在通用 Windows 平台 (UWP) 中，参数要么用于输入，要么用于输出，决不可同时用于两者。 这意味着传递到某个方法的数组的内容以及数组本身要么用于输入，要么用于输出。
ms.assetid: 8DE695AC-CEF2-438C-8F94-FB783EE18EB9
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 49fb5ac5fbba5fad8123eb0167a2e00037725487
ms.sourcegitcommit: a20457776064c95a74804f519993f36b87df911e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71340514"
---
# <a name="passing-arrays-to-a-windows-runtime-component"></a>将数组传递到 Windows 运行时组件




在通用 Windows 平台 (UWP) 中，参数要么用于输入，要么用于输出，决不可同时用于两者。 这意味着传递到某个方法的数组的内容以及数组本身要么用于输入，要么用于输出。 如果数组的内容用于输入，该方法将从该数组进行读取而不是对其进行写入。 如果数组的内容用于输出，该方法将对该数组进行写入而不是从中进行读取。 这对于数组参数是一项问题，因为 .NET 中的数组是引用类型，即使数组引用按值传递（Visual Basic 中的**ByVal** ），数组的内容也是可变的。 [Windows 运行时元数据导出工具 (Winmdexp.exe)](https://docs.microsoft.com/dotnet/framework/tools/winmdexp-exe-windows-runtime-metadata-export-tool) 需要你通过将 ReadOnlyArrayAttribute 属性或 WriteOnlyArrayAttribute 属性应用于参数，指定数组（如果该数组未从上下文中清除）的预期用途。 数组用途已确定，如下所示：

-   对于返回值或输出参数（在 Visual Basic 中为带有 **OutAttribute** 属性的 [ByRef](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.outattribute) 参数），数组始终仅用于输出。 不会应用 ReadOnlyArrayAttribute 属性。 输出参数上允许有 WriteOnlyArrayAttribute 属性，但是多余的。

    > **警告**  Visual Basic 编译器不强制执行仅限输出的规则。 你绝不应从输出参数中进行读取；它可能包含 **Nothing**。 始终分配新数组。
 
-   不允许具有 **ref** 修饰符（在 Visual Basic 中为 **ByRef**）的参数。 Winmdexp.exe 会生成错误。
-   对于通过值传递的参数，必须通过应用 [ReadOnlyArrayAttribute](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.windowsruntime.readonlyarrayattribute) 属性或 [WriteOnlyArrayAttribute](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.windowsruntime.writeonlyarrayattribute) 属性来指定数组内容是用于输入还是用于输出。 同时指定这两个属性是一个错误。

如果某个方法必须接受用于输入的数组，请修改数组内容，并将该数组返回到调用方，然后将只读参数用于输入，而将只写参数（或返回值）用于输出。 以下代码显示了一种实现此模式的方法：

> [!div class="tabbedCodeSnippets"]
> ```csharp
> public int[] ChangeArray([ReadOnlyArray()] int[] input)
> {
>     int[] output = input.Clone();
>     // Manipulate the copy.
>     //   ...
>     return output;
> }
> ```
> ```vb
> Public Function ChangeArray(<ReadOnlyArray> input() As Integer) As Integer()
>     Dim output() As Integer = input.Clone()
>     ' Manipulate the copy.
>     '   ...
>     Return output
> End Function
> ```

我们建议你立即创建输入数组的副本，并处理该副本。 这有助于确保无论是否由 .NET 代码调用，方法的行为都相同。

## <a name="using-components-from-managed-and-unmanaged-code"></a>使用来自托管和非托管代码的组件


对于具有 ReadOnlyArrayAttribute 属性或 WriteOnlyArrayAttribute 属性的参数，其行为会有所不同，具体取决于调用方是在本机代码中编写还是在托管代码中编写。 如果调用方为本机代码（JavaScript 或 Visual C++ 组件扩展），对数组内容的处理如下所示：

-   ReadOnlyArrayAttribute：当调用跨应用程序二进制接口 (ABI) 边界时，将复制该数组。 如有必要，将转换元素。 因此，该方法对仅输入数组所做的任何意外更改对调用方均不可见。
-   WriteOnlyArrayAttribute：调用的方法不可以做任何有关原始数组内容的假设。 例如，该方法接收的数组可能不会进行初始化，也可能包含默认值。 该方法预计会设置数组中所有元素的值。

如果调用方是托管代码，则将原始数组提供给被调用方法，就像在 .NET 中的任何方法调用中一样。 数组内容在 .NET 代码中是可变的，因此，此方法对数组所做的任何更改都对调用方可见。 记住这一点很重要，因为它会影响为 Windows 运行时组件编写的单元测试。 如果测试在托管代码中编写，数组的内容将在测试期间显示为可变。

## <a name="related-topics"></a>相关主题

* [ReadOnlyArrayAttribute](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.windowsruntime.readonlyarrayattribute)
* [WriteOnlyArrayAttribute](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.windowsruntime.writeonlyarrayattribute)
* [使用 C# 和 Visual Basic 创建 Windows 运行时组件](creating-windows-runtime-components-in-csharp-and-visual-basic.md)
