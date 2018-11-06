---
author: mtoepke
title: 游戏音频
description: 了解如何开发音乐和声音并将其融入你的 DirectX 游戏，以及如何处理音频信号以创建动态和有方位感的声音。
ms.assetid: ab29297a-9588-c79b-24c5-3b94b85e74a8
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 游戏, 音频, directx
ms.localizationpriority: medium
ms.openlocfilehash: a0b0ae219ea7fd014b39eb8eb7a09049f7c632a2
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/05/2018
ms.locfileid: "6046062"
---
# <a name="audio-for-games"></a>游戏音频



了解如何开发音乐和声音并将其融入你的 DirectX 游戏，以及如何处理音频信号以创建动态和有方位感的声音。

对于音频编程，我们建议使用 DirectX 中的 XAudio2 库，我们在此处使用该库。 XAudio2 是一个低级别的音频库，可为游戏提供信号处理 和混合的基础，并支持各种格式。

你还可以使用 [Microsoft 媒体基础](https://msdn.microsoft.com/library/windows/desktop/ms694197)实现简单的声音和音乐播放。 Microsoft Media Foundation 专门为播放媒体文件和音频和视频流而设计，但也可以用于游戏，而且对游戏的影院场景或非交互式组件非常有用。

## <a name="concepts-at-a-glance"></a>简要介绍相关的概念


下面是我们在此部分中使用的一些音频编程概念。

-   信号是声音编程的基本单位，好比是图形中的像素。 处理信号的数字信号处理器 (DSP) 好比是游戏音频的像素着色器。 它们可以转换、合并和过滤信号。 通过对 DSP 编程，你可以根据需要更改游戏的声音效果和音乐，使其较为简单或者非常复杂。
-   语音是两个或多个信号的子混合复合体。 共有 3 种 XAudio2 语音对象：源语音、子混合语音和主语音。 源语音对客户提供的音频数据操作。 源语音和子混合语音将其输出发送到一个或多个子混合语音或主语音。 子混合语音和主语音将传入的所有音频混合在一起，并对结果进行操作。 主语音将音频数据写入音频设备。
-   混合是将若干离散语音（如声音效果和场景后台播放的背景音频）合并到信号流的过程。 子混合是合并若干离散信号（如属于引擎噪音的组件声音）并创建语音的过程。
-   音频格式。 音乐和声音效果可以存储为游戏的各种数字格式。 有未压缩格式（如 WAV）和压缩格式（如 MP3 和 OGG）。 采样的压缩率越高（通常由其比特率指示，比特率越低压缩损失越大），保真度就越差。 保真度因压缩方案和比特率而各不相同，因此需要通过实验找到最适合你的游戏的方案。
-   采样率和质量。 声音可以用不同的速率采样，使用较低速率采样的声音会得到较差的保真度。 CD 质量的采样率是 44.1 千赫（44100 赫兹）。 如果不需要高保真度的声音，则可以选择较低的采样率。 较高的采样率可能适合于专业音频应用程序，但是，除非你的游戏需要专业保真度的声音，否则可能不需要它们。
-   声音发射器（或源）。 在 XAudio2 中，声音发射器是发出声音的位置，它可能是暂时的背景噪音或者是游戏内唱机播放的摇滚乐曲。 可以通过世界坐标指定发射器。
-   声音侦听器。 声音侦听器通常是播放器，在更高级的游戏中也可能是处理从侦听器中收到的声音的 AI 实体。 你可以将该声音再混合到音频流中以发送到播放器，也可以使用它执行特定的游戏内操作，如唤醒标记为侦听器的 AI。

## <a name="design-considerations"></a>设计时的注意事项


音频是游戏设计和开发中非常重要的部分。 许多游戏玩家还记得从普通游戏提升到传奇地位就是因为一段难忘的配乐，或优秀的语音作品和声音混合，或者全部是一流的音频作品。 音乐和声音定义游戏的个性，并确立定义该游戏的主要动机，使之从其他类似的游戏中脱颖而出。 花些精力设计和开发游戏的音频配置文件非常值得。

专业 3D 音频可以添加 3D 图形提供之外的沉浸级别。 如果你在开发模拟世界或需要 电影风格的复杂游戏，请考虑使用 3D 专业音频技术真正将玩家带到你的游戏世界中。

## <a name="directx-audio-development-roadmap"></a>DirectX 音频开发路线图


### <a name="xaudio2-conceptual-resources"></a>XAudio2 概念资源

XAudio2 是 DirectX 的音频混合库，主要用于开发高性能的音频游戏引擎。 对于要向其现代游戏中添加声音效果和背景音乐的游戏开发人员，XAudio2 提供了音频图和混合引擎，该引擎具有低延迟且支持动态缓冲、同步采样准确播放以及隐式源速率转换。

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
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415813">XAudio2 简介</a></p></td>
<td align="left"><p>本主题提供 XAudio2 支持的音频编程功能列表。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415762">XAudio2 入门</a></p></td>
<td align="left"><p>本主题提供有关重要 XAudio2 概念、XAudio2 转换版本以及 RIFF 音频格式的信息。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415692">通用音频编程概念</a></p></td>
<td align="left"><p>本主题提供音频开发人员应该熟悉的通用音频概念的概述。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415825">XAudio2 语音</a></p></td>
<td align="left"><p>本主题概述了用于提交、操作以及控制音频数据的 XAudio2 语音。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415745">XAudio2 回调</a></p></td>
<td align="left"><p>本主题介绍用于防止在音频播放时发生中断的 XAudio 2 回调。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415739">XAudio2 音频图</a></p></td>
<td align="left"><p>本主题介绍 XAudio2 音频处理图形，这些处理图像从客户端获取一组音频流作为输入、处理它们并将最终结果提交给音频设备。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415756">XAudio2 音频效果</a></p></td>
<td align="left"><p>本主题介绍 XAudio2 音频效果，该效果获取传入的音频数据并对该数据进行一些操作（如混响效果），然后再传递该数据。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415821">使用 XAudio2 对音频数据进行流式处理</a></p></td>
<td align="left"><p>本主题介绍使用 XAudio2 对音频进行流式处理。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415714">X3DAudio</a></p></td>
<td align="left"><p>本主题介绍 X3DAudio，X3DAudio 是一个 API，与 XAudio2 结合使用可以创建从 3D 空间中的某个点所传入声音的幻觉。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415899">XAudio2 编程参考</a></p></td>
<td align="left"><p>本部分包含 XAudio2 API 的完整参考。</p></td>
</tr>
</tbody>
</table>

 

### <a name="xaudio2-how-to-resources"></a>XAudio2“操作方法”资源

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
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415779">如何：初始化 XAudio2</a></p></td>
<td align="left"><p>了解如何通过创建 XAudio2 引擎的一个实例以及创建一个控制语音来为音频播放初始化 XAudio2。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415781">如何：在 XAudio2 中加载音频数据文件</a></p></td>
<td align="left"><p>了解如何填充在 XAudio2 中播放音频数据所需的结构。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415787">如何：使用 XAudio2 播放声音</a></p></td>
<td align="left"><p>了解如何在 XAudio2 中播放之前加载的音频数据。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415794">如何：使用提交语音</a></p></td>
<td align="left"><p>了解如何将语音组设置为将其输出发送到相同的提交语音。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415769">如何：使用源语音回调</a></p></td>
<td align="left"><p>了解如何使用 XAudio2 源语音回调。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415774">如何：使用引擎回调</a></p></td>
<td align="left"><p>了解如何使用 XAudio2 引擎回调。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415767">如何：构建基本的音频处理图</a></p></td>
<td align="left"><p>了解如何创建一个从单个控制语音和单个源语音构造的音频处理图。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415772">如何：从音频图中动态添加或删除语音</a></p></td>
<td align="left"><p>了解如何向按照<a href="https://msdn.microsoft.com/library/windows/desktop/ee415767">如何：生成基本的音频处理图</a>中的步骤创建的图中添加或从中删除提交语音。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415789">如何：创建效果链</a></p></td>
<td align="left"><p>了解如何对语音应用效果链，以允许对该语音的音频数据进行自定义处理。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415730">如何：创建 XAPO</a></p></td>
<td align="left"><p>了解如何实现 <a href="https://msdn.microsoft.com/library/windows/desktop/ee415893"><strong>IXAPO</strong></a> 以创建处理对象 (XAPO) 的 XAudio2 音频。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415728">如何：向 XAPO 中添加运行时参数支持</a></p></td>
<td align="left"><p>了解如何通过实现 <a href="https://msdn.microsoft.com/library/windows/desktop/ee415896"><strong>IXAPOParameters</strong></a> 接口向 XAPO 中添加运行时参数支持。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415733">如何：在 XAudio2 中使用 XAPO</a></p></td>
<td align="left"><p>了解如何在 XAudio2 效果链中使用以 XAPO 形式实现的效果。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415723">如何：在 XAudio2 中使用 XAPOFX</a></p></td>
<td align="left"><p>了解如何在 XAudio2 效果链中使用 XAPOFX 中包含的效果之一。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415791">如何：对磁盘中的声音进行流式处理</a></p></td>
<td align="left"><p>了解如何通过创建单独的线程来读取音频缓冲，从而在 XAudio2 中流入音频数据，以及如何使用回调来控制该线程。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415798">如何：将 X3DAudio 与 XAudio2 集成</a></p></td>
<td align="left"><p>了解如何使用 X3DAudio 为 XAudio2 语音提供音量和音调值以及如何为 XAudio2 内置的混响效果提供参数。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415783">如何：按操作集组合音频方法</a></p></td>
<td align="left"><p>了解如何使用 XAudio2 操作集使一组方法调用同时起作用。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415765">在 XAudio2 中调试音频故障</a></p></td>
<td align="left"><p>了解如何为 XAudio2 设置调试日志记录级别。</p></td>
</tr>
</tbody>
</table>

 

### <a name="media-foundation-resources"></a>Media Foundation 资源

Media Foundation (MF) 是一个用于流音频和视频播放的媒体平台。 可以使用 Media Foundation API 对使用各种算法进行编码和压缩的音频和视频进行流式处理。 它不是为实时游戏播放方案设计的；而是为音频和视频组件更具线性的捕获和呈现提供强大的工具和广泛的编解码器支持。

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
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ms696274">关于媒体基础</a></p></td>
<td align="left"><p>本部分包含有关 Media Foundation API 的常规信息，以及用于支持它们的工具。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee663601">Media Foundation：基本概念</a></p></td>
<td align="left"><p>本主题介绍编写 Media Foundation 应用程序之前你将需要了解的一些概念。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ms696219">Media Foundation 体系结构</a></p></td>
<td align="left"><p>本部分介绍 Microsoft Media Foundation 的常规设计，以及媒体基元及其使用的处理管道。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/dd317910">音频/视频捕获</a></p></td>
<td align="left"><p>本主题介绍如何使用 Microsoft Media Foundation 来执行音频和视频捕获。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/dd317914">音频/视频播放</a></p></td>
<td align="left"><p>本主题介绍如何在应用中实现音频/视频播放。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/dd757927">Media Foundation 中支持的媒体格式</a></p></td>
<td align="left"><p>本主题列出了 Microsoft Media Foundation 本机支持的媒体格式。 （第三方可以通过编写自定义插件支持其他格式。）</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/dd318778">编码和文件创作</a></p></td>
<td align="left"><p>本主题介绍如何使用 Microsoft Media Foundation 来执行音频和视频编码以及创作媒体文件。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ff819508">Windows Media 编解码器</a></p></td>
<td align="left"><p>本主题介绍如何使用 Windows Media 音频和视频编解码器的功能来生成和使用压缩的数据流。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ms704847">Media Foundation 编程参考</a></p></td>
<td align="left"><p>本部分包含 Media Foundation API 的参考信息。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/aa371827">Media Foundation SDK 示例</a></p></td>
<td align="left"><p>本部分列出了演示如何使用 Media Foundation 的示例应用。</p></td>
</tr>
</tbody>
</table>

 

### <a name="windows-runtime-xaml-media-types"></a>Windows 运行时 XAML 媒体类型

如果使用 [DirectX-XAML 互操作](https://msdn.microsoft.com/library/windows/apps/hh825871)，你可以将 Windows 运行时 XAML 媒体 API 合并到使用 DirectX 和 C++ 的 UWP 应用中，从而获得更简单的游戏方案。

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
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/apps/br242926"><strong>Windows.UI.Xaml.Controls.MediaElement</strong></a></p></td>
<td align="left"><p>XAML 元素，它表示包含音频和/或视频的对象。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/apps/mt203788">音频、视频和相机</a></p></td>
<td align="left"><p>了解如何在通用 Windows 平台 (UWP) 应用中合并基本的音频和视频。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/apps/mt187272">MediaElement</a></p></td>
<td align="left"><p>了解如何在 UWP 应用中播放本地存储的媒体文件。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/apps/mt187272">MediaElement</a></p></td>
<td align="left"><p>了解如何在 UWP 应用中对低延迟的媒体文件进行流式传输。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/apps/mt282143">媒体转换</a></p></td>
<td align="left"><p>了解如何使用“播放到”合约将媒体从 UWP 应用流式传输到其他设备。</p></td>
</tr>
</tbody>
</table>

 

## <a name="reference"></a>参考


-   [XAudio2 简介](https://msdn.microsoft.com/library/windows/desktop/ee415813)
-   [XAudio2 编程指南](https://msdn.microsoft.com/library/windows/desktop/ee415737)
-   [Microsoft 媒体基础概述](https://msdn.microsoft.com/library/windows/desktop/ms694197)

 

## <a name="related-topics"></a>相关主题


-   [XAudio2 编程指南](https://msdn.microsoft.com/library/windows/desktop/ee415737)

 

 




