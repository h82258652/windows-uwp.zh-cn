---
author: joannaleecy
title: DirectX 游戏的优化和高级主题
description: DirectX 游戏编程的优化和高级主题。
ms.assetid: b5f29fb2-3bcf-43b2-9a68-f8819473bf62
ms.author: joanlee
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 游戏, directx, 优化, 多重采样, 交换链
ms.localizationpriority: medium
ms.openlocfilehash: e1a9b16dcf8c40c2b1db4af172d97009563e677a
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/05/2018
ms.locfileid: "6025980"
---
# <a name="optimization-and-advanced-topics-for-directx-games"></a>DirectX 游戏的优化和高级主题

本部分提供有关优化 DirectX 游戏性能和其他高级主题的信息。

游戏异步编程主题讨论了使用异步编程并行处理部分组件以及使用多线程充分使用强大的 GPU 时所需考虑的各个事项。

在 Direct3D 11 中处理设备删除方案使用演练来说明使用 Direct3D 11 开发的游戏如何检测和响应重置、删除或更改图形适配器这些情况。

在 UWP 应用中使用多重采样主题提供了如何使用多重采样抗锯齿（一种图形技术，用于减少锯齿边缘在使用 Direct3D 构建的 UWP 游戏中的显示）的概述。

优化输入和呈现循环主题提供了有关如何选择适合的输入事件处理选项来管理输入延迟和优化呈现循环的指南。

利用 DXGI 1.3 交换链减少延迟主题介绍了如何通过等待交换链发送相应的时间信号以开始呈现新帧来减少有效的帧延迟。

交换链缩放和覆盖主题解释了如何提高呈现次数，方法是通过使用缩放后的交换链以比屏幕自身能够提供的分辨率更低的分辨率呈现实时游戏内容。 此外，它还介绍了如何通过硬件覆盖功能为设备创建覆盖交换链；该技术可用于缓解因使用交换链缩放而导致的 UI 缩小问题。

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">主题</th>
<th align="left">描述</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="asynchronous-programming-directx-and-cpp.md">游戏异步编程</a></p></td>
<td align="left"><p>了解使用 DirectX 进行异步编程和线程处理。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="handling-device-lost-scenarios.md">在 Direct3D 11 中处理设备删除方案</a></p></td>
<td align="left"><p>图形适配器被删除或重新初始化时，如何重新创建 Direct3D 和 DXGI 的设备界面链。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="multisampling--multi-sample-anti-aliasing--in-windows-store-apps.md">UWP 应用中的多重采样</a></p></td>
<td align="left"><p>在使用 Direct3D 构建的 UWP 游戏中使用多重采样。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="optimize-performance-for-windows-store-direct3d-11-apps-with-coredispatcher.md">优化输入和呈现循环</a></p></td>
<td align="left"><p>减少输入延迟和优化呈现循环。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="reduce-latency-with-dxgi-1-3-swap-chains.md">利用 DXGI 1.3 交换链减少延迟</a></p></td>
<td align="left"><p>使用 DXGI 1.3 减少有效的帧延迟。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="multisampling--scaling--and-overlay-swap-chains.md">交换链缩放和覆盖</a></p></td>
<td align="left"><p>创建已缩放的交换链以提高在移动设备上的呈现速度，以及使用覆盖交换链来提高视觉质量。</p></td>
</tr>
</tbody>
</table>