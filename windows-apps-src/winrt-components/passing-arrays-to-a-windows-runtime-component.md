---
author: msatranjr
title: 将数组传递到 Windows 运行时组件
description: 在通用 Windows 平台 (UWP) 中，参数要么用于输入，要么用于输出，决不可同时用于两者。 这意味着传递到某个方法的数组的内容以及数组本身要么用于输入，要么用于输出。
ms.assetid: 8DE695AC-CEF2-438C-8F94-FB783EE18EB9
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e01c9e5698ec1d7a23298b46f6bde9e1bbf36b04
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/05/2018
ms.locfileid: "6026742"
---
# <a name="passing-arrays-to-a-windows-runtime-component"></a>将数组传递到 Windows 运行时组件




在通用 Windows 平台 (UWP) 中，参数要么用于输入，要么用于输出，决不可同时用于两者。 这意味着传递到某个方法的数组的内容以及数组本身要么用于输入，要么用于输出。 如果数组的内容用于输入，该方法将从该数组进行读取而不是对其进行写入。 如果数组的内容用于输出，该方法将对该数组进行写入而不是从中进行读取。 这会带来数组参数问题，因为 .NET Framework 中的数组是引用类型，而且即使通过值（在 Visual Basic 中为 **ByVal**）传递数组引用，数组的内容仍具有可变性。 [Windows 运行时元数据导出工具 (Winmdexp.exe)](https://msdn.microsoft.com/library/hh925576.aspx) 需要你通过将 ReadOnlyArrayAttribute 属性或 WriteOnlyArrayAttribute 属性应用于参数，指定数组（如果该数组未从上下文中清除）的预期用途。 数组用途已确定，如下所示：

-   对于返回值或输出参数（在 Visual Basic 中为带有 [OutAttribute](https://msdn.microsoft.com/library/system.runtime.interopservices.outattribute.aspx) 属性的 **ByRef** 参数），数组始终仅用于输出。 不会应用 ReadOnlyArrayAttribute 属性。 输出参数上允许有 WriteOnlyArrayAttribute 属性，但是多余的。

    > **警告**Visual Basic 编译器不强制执行仅输出规则。 你绝不应从输出参数中进行读取；它可能包含 **Nothing**。 始终分配新数组。
 
-   不允许具有 **ref** 修饰符（在 Visual Basic 中为 **ByRef**）的参数。 Winmdexp.exe 会生成错误。
-   对于通过值传递的参数，必须通过应用 [ReadOnlyArrayAttribute](https://msdn.microsoft.com/library/system.runtime.interopservices.windowsruntime.readonlyarrayattribute.aspx) 属性或 [WriteOnlyArrayAttribute](https://msdn.microsoft.com/library/system.runtime.interopservices.windowsruntime.writeonlyarrayattribute.aspx) 属性来指定数组内容是用于输入还是用于输出。 同时指定这两个属性是一个错误。

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

我们建议你立即创建输入数组的副本，并处理该副本。 这有助于确保不管你的组件是否由 .NET Framework 代码调用，该方法具有相同的行为。

## <a name="using-components-from-managed-and-unmanaged-code"></a>使用来自托管和非托管代码的组件


对于具有 ReadOnlyArrayAttribute 属性或 WriteOnlyArrayAttribute 属性的参数，其行为会有所不同，具体取决于调用方是在本机代码中编写还是在托管代码中编写。 如果调用方为本机代码（JavaScript 或 Visual C++ 组件扩展），对数组内容的处理如下所示：

-   ReadOnlyArrayAttribute：当调用跨应用程序二进制接口 (ABI) 边界时，将复制该数组。 如有必要，将转换元素。 因此，该方法对仅输入数组所做的任何意外更改对调用方均不可见。
-   WriteOnlyArrayAttribute：调用的方法不可以做任何有关原始数组内容的假设。 例如，该方法接收的数组可能不会进行初始化，也可能包含默认值。 该方法预计会设置数组中所有元素的值。

如果调用方为托管代码，则原始数组可用于调用的方法，就像在 .NET Framework 中使用任何方法调用那样。 数组内容在 .NET Framework 代码中具有可变性，因此该方法对数组所做的任何更改对调用方均可见。 记住这一点很重要，因为它会影响为 Windows 运行时组件编写的单元测试。 如果测试在托管代码中编写，数组的内容将在测试期间显示为可变。

## <a name="related-topics"></a>相关主题

* [ReadOnlyArrayAttribute](https://msdn.microsoft.com/library/system.runtime.interopservices.windowsruntime.readonlyarrayattribute.aspx)
* [WriteOnlyArrayAttribute](https://msdn.microsoft.com/library/system.runtime.interopservices.windowsruntime.writeonlyarrayattribute.aspx)
* [使用 C# 和 Visual Basic 创建 Windows 运行时组件](creating-windows-runtime-components-in-csharp-and-visual-basic.md)
