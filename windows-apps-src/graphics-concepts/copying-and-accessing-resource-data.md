---
title: "复制和访问资源数据"
description: "用法标志指示应用程序应如何使用资源数据以将资源放置到性能最佳的内存区域。 跨资源复制资源数据，以便 CPU 或 GPU 能够访问数据而不会影响性能。"
ms.assetid: 6A09702D-0FF2-4EA6-A353-0F95A3EE34E2
keywords: "复制和访问资源数据"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: e26e6abf4b15584f8c04a837dcc6bd23aad0f1d0
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="copying-and-accessing-resource-data"></a>复制和访问资源数据


用法标志指示应用程序应如何使用资源数据以将资源放置到性能最佳的内存区域。 跨资源复制资源数据，以便 CPU 或 GPU 能够访问数据而不会影响性能。

没有必要考虑资源是在视频内存还是系统内存中创建，或决定运行时是否应该管理内存。 使用 WDDM（Windows 显示驱动程序型号）的体系结构，应用程序可创建带有不同用法标志的 Direct3D 资源，以指示应用程序应如何使用资源数据。 此驱动程序模型可将资源使用的内存虚拟化；鉴于预期的使用情况，操作系统/驱动程序/内存管理器负责将资源放置到性能最佳的内存区域。

默认情况下是针对用于 GPU 的资源。 有时，资源数据需要对 CPU 可用。 复制周围的资源数据，以便适当的处理器可以访问它，而不会影响需要对 API 方法工作方式有所了解的性能。

## <a name="span-idcopyingspanspan-idcopyingspanspan-idcopyingspancopying-resource-data"></a><span id="Copying"></span><span id="copying"></span><span id="COPYING"></span>复制资源数据


Direct3D 执行 Create 调用时，在内存中创建资源。 它们可以在视频内存、系统内存或任意其他类型的内存中创建。 由于 WDDM 驱动程序模型对此内存进行了虚拟化，因此应用程序不再需要跟踪在哪种类型的内存中创建资源。

理想情况下，所有资源都应位于视频内存中，以便 GPU 可以立即访问它们。 不过，有时 CPU 需要读取资源数据，或 GPU 需要访问 CPU 写入的资源数据。 Direct3D 通过请求应用程序指定用法来处理这些不同的方案，并在需要时提供几种用于复制资源数据的方法。

根据创建资源的方式，并非始终可以直接访问基础数据。 这可能意味着，此资源数据必须从源资源复制到其他适当的处理器可以访问的资源。 对于 Direct3D 而言，默认资源可以通过 GPU 直接访问，动态和暂存资源可以通过 CPU 直接访问。

创建某个资源后，其用法无法更改。 而是，将一个资源的内容复制到用其他用法创建的另一个资源。 可以将资源数据从一个资源复制到另一个资源，或将数据从内存复制到资源。

有两种主要的资源类型：可映射资源和非可映射资源。 使用动态或暂存用法创建的资源是可映射资源，而使用默认或不可变用法创建的资源是非可映射资源。

复制非可映射资源间的数据非常快速，因为这是最常见的情况，并且经过优化，可以很好地运行。 由于这些资源不可通过 CPU 直接访问，但它们已经过优化，以便 GPU 可以快速操作。

复制可映射资源间的数据问题更多，因为性能将取决于通过其创建资源的用法。 例如，GPU 可以相当迅速地读取动态资源，但不能写入它们，而 GPU 无法直接读取或写入暂存资源。

想要将数据从采用默认用法的资源复制到采用暂存用法的资源（以允许 CPU 读取数据 - 即，GPU 回读问题）的应用程序必须小心执行此操作。 请参阅下面的[访问资源数据](#accessing)。

## <a name="span-idaccessingspanspan-idaccessingspanspan-idaccessingspanaccessing-resource-data"></a><span id="Accessing"></span><span id="accessing"></span><span id="ACCESSING"></span>访问资源数据


访问资源要求映射资源；映射本质上意味着应用程序正在尝试向 CPU 授予对内存的访问权限。 映射资源（以便 CPU 可以访问基础内存）的过程可能会导致某些性能瓶颈，因此，对于如何和何时执行此任务必须格外小心。

如果应用尝试在错误的时间映射资源，性能可能会陷入停滞状态。 如果应用程序尝试在完成操作前访问操作结果，将发生管道停滞。

在错误的时间映射操作会强制 GPU 与 CPU 互相同步，可能会导致性能严重下降。 如果应用程序想要在 GPU 将资源复制到 CPU 可以映射的资源中之前访问资源，将发生此同步。

### <a name="span-idperformanceconsiderationsspanspan-idperformanceconsiderationsspanspan-idperformanceconsiderationsspanperformance-considerations"></a><span id="Performance_Considerations"></span><span id="performance_considerations"></span><span id="PERFORMANCE_CONSIDERATIONS"></span>性能注意事项

最好将电脑视为作为并行体系结构与两种主要处理器类型（一或多个 CPU 和一个或多个 GPU）共同运行的计算机。 与任何并行体系结构一样，当已为每个处理器计划足够的任务以避免其空闲，且一个处理器不会等待另一个处理器工作时，可以实现最佳性能。

对于 GPU/CPU 并行度而言，最坏的情况是需要强制一个处理器等待另一个处理器完成的工作结果。 Direct3D 通过使复制方法异步免除了此成本；方法返回时不一定执行了复制操作。

此方法的好处是，应用程序无需支付实际上复制数据的性能成本，直到 CPU 访问数据（调用 Map 时）。 如果在实际复制数据后调用 Map，不会发生性能丢失。 另一方面，如果在复制数据之前调用 Map 方法，会发生管道停滞。

Direct3D 中的异步调用（即绝大多数方法，尤其是渲染调用）存储在称为*命令缓冲区*的空间中。 此缓冲区位于图形驱动程序内部，并且用于基础硬件的批量调用，以便在 Microsoft Windows 中尽量少发生从用户模式到内核模式的昂贵切换。

在四种情况中的一种情况下，刷新命令缓冲区，从而导致用户/内核模式切换，如下所示。

1.  调用 Present。
2.  调用 Flush。
3.  命令缓冲区已满；其大小是动态的，并由操作系统和图形驱动程序控制。
4.  CPU 要求访问等待在命令缓冲区中执行的命令的结果。

在上述四种情况中，第四种对性能最关键。 如果应用程序发出复制资源或子资源的调用，此调用会在命令缓冲区排队。

然后，如果应用程序尝试在刷新命令缓冲区之前映射作为复制调用的目标的资源，则会发生管道停滞，这是因为，不仅需要执行 Copy 方法调用，还需要执行命令缓冲区中所有其他缓冲的命令。 这将导致 GPU 和 CPU 同步，因为 CPU 正在等待访问暂存资源，而 GPU 正在清空命令缓冲区，并将最终填充 CPU 需要的资源。 GPU 完成复制后，CPU 将开始访问暂存资源，但在此期间，GPU 将会处于空闲状态。

在运行时经常执行此操作将会严重降低性能。 因此，应谨慎完成映射使用默认用法创建的资源这一操作。 尝试映射相应的暂存资源之前，此应用程序需要等待足够长的时间，以便清空命令缓冲区并执行完所有命令。

应用程序应等待多久？ 至少两帧，因为这需要能够最大化地利用 CPU 和 GPU 之间的并行度。 GPU 的工作原理是，当应用程序通过将调用提交到命令缓冲区处理 N 帧时，GPU 正忙于执行来自上一帧（N-1 帧）的调用。

因此，如果应用程序想要映射源自视频内存的资源并复制 N 帧处的资源，当应用程序正在提交下一帧的调用时，此调用将实际上于 N+1 帧开始执行。 复制应该在应用程序处理 N+2 帧时完成。

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">帧</th>
<th align="left">GPU/CPU 状态</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">N</td>
<td align="left"><ul>
<li>CPU 发出当前帧的渲染调用。</li>
</ul></td>
</tr>
<tr class="even">
<td align="left">N+1</td>
<td align="left"><ul>
<li>GPU 正在执行 N 帧期间从 CPU 发送的调用。</li>
<li>CPU 发出当前帧的渲染调用。</li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">N+2</td>
<td align="left"><ul>
<li>GPU 已执行完 N 帧期间从 CPU 发送的调用。结果准备就绪。</li>
<li>GPU 正在执行 N+1 帧期间从 CPU 发送的调用。</li>
<li>CPU 发出当前帧的渲染调用。</li>
</ul></td>
</tr>
<tr class="even">
<td align="left">N+3</td>
<td align="left"><ul>
<li>GPU 已执行完 N+1 帧期间从 CPU 发送的调用。 结果准备就绪。</li>
<li>GPU 正在执行 N+2 帧期间从 CPU 发送的调用。</li>
<li>CPU 发出当前帧的渲染调用。</li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">N+4</td>
<td align="left">...</td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[资源](resources.md)

 

 




