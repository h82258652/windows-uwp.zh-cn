---
author: drewbatgit
ms.assetid: DD8FFA8C-DFF0-41E3-8F7A-345C5A248FC2
description: 本主题介绍了如何向通用 Windows 平台 (UWP) 应用添加 PlayReady 保护的媒体内容。
title: PlayReady DRM
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 79c1cd5b83c013bdf601022aa7fec9e661b80857
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2018
ms.locfileid: "7423903"
---
# <a name="playready-drm"></a>PlayReady DRM



本主题介绍如何向通用 Windows 平台 (UWP) 应用添加 PlayReady 保护的媒体内容。

PlayReady DRM 允许开发人员在强制执行内容提供商定义的访问规则的同时，创建能够向用户提供 PlayReady 内容的 UWP 应用。 本部分介绍的 windows 10 以及如何修改 PlayReady UWP 应用以支持从以前的 Windows8.1 版本到 windows 10 版本所做的更改 Microsoft PlayReady DRM 所做的更改。
 
| 主题                                                                     | 说明                                                                                                                                                                                                                                                                             |
|---------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [硬件 DRM](hardware-drm.md)                                           | 本主题概述了如何向 UWP 应用添加基于 PlayReady 硬件的数字版权管理 (DRM)。                                                                                                                                                                 |
| [使用 PlayReady 的自适应流式处理](adaptive-streaming-with-playready.md) | 本文介绍如何将使用 Microsoft PlayReady 内容保护的多媒体内容自适应流式处理添加到通用 Windows 平台 (UWP) 应用。 此功能当前支持 Http 实时流 (HLS) 和 HTTP 动态流 (DASH) 内容的播放。 |

## <a name="whats-new-in-playready-drm"></a>PlayReady DRM 中的新增功能

下表介绍了新功能和对适用于 windows 10 的 PlayReady DRM 所做的更改。

-   添加了硬件数字版权管理 (HWDRM)。

    基于硬件的内容保护支持可以在多个设备平台上安全播放高清 (HD) 和超高清 (UHD) 内容。 利用硬件安全，可保护密钥材料（包括私钥、内容密钥和任何其他用于派生或解锁上述密钥的密钥材料）以及解密的压缩和未压缩视频示例。 使用硬件 DRM 时，两项未知启用程序（播放到未知/通过 downres 播放到未知）均没有意义，因为 HWDRM 管道始终知道所使用的输出。 有关详细信息，请参阅[硬件 DRM](hardware-drm.md)。

-   PlayReady 不再是 appX 框架组件，而是内置操作系统组件。 命名空间已从 **Microsoft.Media.PlayReadyClient** 更改为 [**Windows.Media.Protection.PlayReady**](https://msdn.microsoft.com/library/windows/apps/dn986454)。
-   以下定义 PlayReady 错误代码的标头现在是 Windows 软件开发工具包 (SDK) 的一部分：Windows.Media.Protection.PlayReadyErrors.h 和 Windows.Media.Protection.PlayReadyResults.h。
-   可主动获取非永久性许可证。

    以前版本的 PlayReady DRM 不支持主动获取非永久性许可证。 此功能已添加到此版本。 这可以减少到第一帧的时间。 有关详细信息，请参阅[在播放前主动获取非永久性许可证](#proactively-acquire-a-non-persistent-license-before-playback)。

-   在一条消息中可获取多个许可证。

    允许客户端应用在一条许可证获取消息中获取多个非永久性许可证。 这可以在用户仍然浏览内容库的同时，通过获取多条内容的许可证，减少到第一帧的时间；这将在用户选择要播放的内容时防止许可证获取延迟。 此外，它通过启用包含多个密钥标识符 (KID) 的内容标头，允许加密音频流和视频流以分隔密钥；这允许单个许可证获取内容文件中所有流的所有许可证，而不必使用自定义逻辑和多个许可证获取请求来实现相同的结果。

-   添加了实时过期时间支持，或有限持续时间许可证 (LDL)。

    提供了在许可证上设置实时到期和从到期许可证平滑过渡到另一个正在播放中的（有效）许可证的功能。 当与一条消息中的多个许可证获取结合时，此功能允许应用在用户仍然浏览内容库的同时，以异步方式获取几个 LDL，并且仅在用户已选择要播放的内容后获取更长持续时间许可证。 然后，播放将更快启动（因为已经有可用的许可证），并且由于应用在 LDL 到期时将获取更长持续时间许可证，所以将顺利继续播放直到内容末尾而不会中断。

-   添加的非永久性许可证链。
-   非永久性许可证上添加的基于时间限制的支持（包括到期、第一次播放后到期和实时到期）。
-   添加了 HDCP 类型 1（Windows10 上的版本 2.2）策略支持。

    有关详细信息，请参阅[需要考虑的事项](#things-to-consider)。

-   Miracast 现在已隐式为输出。
-   添加了安全停止。

    安全停止为 PlayReady 设备能够有效地断定因任何给定内容而停止媒体播放的媒体流服务提供了方法。 此功能可确保你的媒体流服务能够准确地执行和报告给定帐户在不同设备上的使用限制。

-   添加了音频和视频许可证分离。

    单独的轨道可避免视频被解码为音频；实现更可靠的内容保护。 新兴标准要求对音轨和视频轨使用单独的密钥。

-   添加了 MaxResDecode。

    添加此功能是为了在即使具有功能更强的密钥（但不是许可证）的情况下，也将内容播放限制在最大分辨率以内。 它支持使用单个密钥对多个媒体流大小进行编码的情况。

以下新接口、类和枚举已添加到 PlayReady DRM：

-   [**IPlayReadyLicenseAcquisitionServiceRequest**](https://msdn.microsoft.com/library/windows/apps/dn986077) 接口
-   [**IPlayReadyLicenseSession**](https://msdn.microsoft.com/library/windows/apps/dn986080) 接口
-   [**IPlayReadySecureStopServiceRequest**](https://msdn.microsoft.com/library/windows/apps/dn986090) 接口
-   [**PlayReadyLicenseSession**](https://msdn.microsoft.com/library/windows/apps/dn986309) 类
-   [**PlayReadySecureStopIterable**](https://msdn.microsoft.com/library/windows/apps/dn986371) 类
-   [**PlayReadySecureStopIterator**](https://msdn.microsoft.com/library/windows/apps/dn986375) 类
-   [**PlayReadyHardwareDRMFeatures**](https://msdn.microsoft.com/library/windows/apps/dn986265) 枚举器

已创建新示例，用于演示如何使用 PlayReady DRM 的新功能。 可以从 [http://go.microsoft.com/fwlink/p/?linkid=331670&clcid=0x409](http://go.microsoft.com/fwlink/p/?linkid=331670) 下载示例。

## <a name="things-to-consider"></a>注意事项

-   PlayReady DRM 现在支持 HDCP 类型 1（在 HDCP 版本 2.1 或更高版本中受支持）。 PlayReady 将强制实施设备许可证中的 HDCP 类型限制策略。 在 Windows10 上，此策略将强制仅使用 HDCP 2.2 或更高版本。 此功能可在 PlayReady 服务器 v3.0 SDK 许可证中启用（服务器在许可证中使用 HDCP 类型限制 GUID 控制此策略）。 有关详细信息，请参阅 [PlayReady 合规性和可靠性规则](http://www.microsoft.com/playready/licensing/compliance/)。
-   Windows Media 视频（也称为 VC-1）在硬件 DRM 中不受支持（请参阅[替代硬件 DRM](hardware-drm.md#override-hardware-drm)）。
-   PlayReady DRM 现在支持高效率视频编码 (HEVC /H.265) 视频压缩标准。 若要支持 HEVC，应用必须使用常用加密方案 (CENC) 版本 2 内容（它包括以明文形式离开内容的切片标头）。 有关详细信息，请参阅 ISO/IEC 23001 7 信息技术 -- MPEG 系统技术 - 部分 7：ISO 基本媒体文件格式文件中的常用加密（需要规范版本 ISO/IEC 23001-7:2015 或更高版本。） Microsoft 还建议将 CENC 版本 2 用于所有 HWDRM 内容。 此外，一些硬件 DRM 可支持 HEVC，而另一些硬件 DRM 则不支持（请参阅[替代硬件 DRM](hardware-drm.md#override-hardware-drm)）。
-   若要利用某些新的 PlayReady 3.0 功能（包括但不限于适用于基于硬件的客户端的 SL3000、在一个许可证获取消息中获取多个非永久性许可证以及对非永久性许可证的基于时间的限制），要求 PlayReady 服务器是 Microsoft PlayReady 服务器软件开发工具包 v3.0.2769 发行版本或更高版本。
-   根据内容许可证指定的输出保护策略，如果媒体播放连接的输出不支持这些要求，它将无法让最终用户使用。 下表列出了作为结果发生的常见错误集。 有关详细信息，请参阅 [PlayReady 合规性和可靠性规则](http://www.microsoft.com/playready/licensing/compliance/)。

| 错误                                                   | 值      | 说明                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|---------------------------------------------------------|------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ERROR\_GRAPHICS\_OPM\_OUTPUT\_DOES\_NOT\_SUPPORT\_HDCP  | 0xC0262513 | 许可证的输出保护策略要求监视器执行 HDCP，但 HDCP 无法执行。                                                                                                                                                                                                                                                                                                                                                                                              |
| MF\_E\_POLICY\_UNSUPPORTED                              | 0xC00D7159 | 许可证的输出保护策略要求监视器执行 HDCP 类型 1，但 HDCP 类型 1 无法执行。                                                                                                                                                                                                                                                                                                                                                                                |
| DRM\_E\_TEE\_OUTPUT\_PROTECTION\_REQUIREMENTS\_NOT\_MET | 0x8004CD22 | 此错误代码仅在硬件 DRM 下运行时才发生。 许可证输出保护策略要求监视器执行 HDCP，或减少内容的有效分辨率，但 HDCP 无法执行，并且内容的有效分辨率也无法减少（因为硬件 DRM 不支持降低内容分辨率）。 内容将在软件 DRM 下播放。 请参阅[使用硬件 DRM 的注意事项](hardware-drm.md#considerations-for-using-hardware-drm)。 |
| ERROR\_GRAPHICS\_OPM\_NOT\_SUPPORTED                    | 0xc0262500 | 图形驱动程序不支持输出保护。 例如，监视器通过 VGA 连接，或者未安装数字输出的相应图形驱动程序。 在后一种情况下，所安装的典型驱动程序是 Microsoft 基本显示适配器，并且安装相应图形驱动程序将解决该问题。                                                                                                                                                  |

## <a name="output-protection"></a>输出保护

以下部分介绍在 PlayReady 许可证中将适用于 Windows10 的 PlayReady DRM 与输出保护策略结合使用时的行为。

PlayReady DRM 支持包含在 **Microsoft PlayReady 可扩展媒体权限规范**中的输出保护级别。 本文档可在 PlayReady 授权产品附带的文档包中找到。

> [!NOTE]
> 可由授权服务器设置的输出保护级别的允许值受 [PlayReady 合规性规则](https://www.microsoft.com/playready/licensing/compliance/)约束。

PlayReady DRM 仅允许在输出连接器上播放使用输出保护策略的内容，如 PlayReady 合规性规则中所指定的。 有关 PlayReady 合规性规则中所指定的输出连接器条款的详细信息，请参阅 [PlayReady 合规性和可靠性规则的定义条款](https://www.microsoft.com/playready/licensing/compliance/)。

本部分重点介绍使用适用于 Windows10 的 PlayReady DRM 和适用于 Windows10 的 PlayReady 硬件 DRM（还可用于某些 Windows 客户端）的输出保护方案。 借助 PlayReady HWDRM，所有输出保护都将从 Windows TEE 实现中强制执行（请参阅[硬件 DRM](hardware-drm.md)）。 因此，一些行为不同于使用 PlayReady SWDRM（软件 DRM）时的行为：

* 对未压缩数字视频 270 的输出保护级别 (OPL) 的支持：适用于 Windows10 的 PlayReady HWDRM 不支持向下分辨率，并将强制使用 HDCP（高带宽数字内容保护）。 建议 HWDRM 的高清内容应具有大于 270 的 OPL（尽管这并不是必需的）。 另外，你应在许可证中设置 HDCP 类型限制（HDCP 版本 2.2 或更高版本）。
* 与 SWDRM 不同，使用 HWDRM 时，输出保护在基于最低功能监视器的所有监视器上强制执行。 例如，如果用户连接了两台监视器，其中一台支持 HDCP，而另一台不支持。即使仅在支持 HDCP 的监视器上呈现内容，但如果许可证需要 HDCP，播放也将失败。 在 SWDRM 中可播放内容，前提是仅在支持 HDCP 的监视器上呈现该内容。
* HWDRM 不能保证由客户端和安全使用，除非内容密钥和许可证满足以下条件：
    * 用于视频内容密钥的许可证必须至少具有 3000 安全级别。
    * 音频必须加密为不同于视频的内容密钥，并且用于音频的许可证必须至少具有 2000 安全级别。 此外，音频可能保持清晰。
* 所有 SWDRM 方案均要求，用于音频和/或视频内容密钥的 PlayReady 许可证的最低安全级别低于或等于 2000。

### <a name="output-protection-levels"></a>输出保护级别

下表概述了 PlayReady 许可证中各种 OPL 间的映射，以及适用于 Windows10 的 PlayReady DRM 如何强制实现它们。

#### <a name="video"></a>视频

<table>
    <tr>
        <th rowspan="2">OPL</th>
        <th>压缩的数字视频</th>
        <th colspan="2">未压缩的数字视频</th>
        <th>模拟电视</th>
    </tr>
    <tr>
        <th>Any</th>
        <th colspan="2">HDMI、DVI、DisplayPort、MHL</th>
        <th>分量、复合</th>
    </tr>
    <tr>
        <th>100</th>
        <td rowspan="6">不适用\*</td>
        <td colspan="2">传递内容</td>
        <td>传递内容</td>
    </tr>
    <tr>
        <th>150</th>
        <td colspan="2" rowspan="2">不适用\*</td>
        <td>当使用 CGMS-A CopyNever 或者无法使用 CGMS-A 时，传递内容</td>
    </tr>
    <tr>
        <th>200</th>
        <td>当使用 CGMS-A CopyNever 时，传递内容</td>
    </tr>
    <tr>
        <th>250</th>
        <td colspan="2">尝试使用 HDCP，不过无论结果如何，均传递内容</td>
        <td rowspan="5">不适用\*</td>
    </tr>
    <tr>
        <th>270</th>
        <td><b>SWDRM</b>：尝试使用 HDCP。 如果 HDCP 无法使用，电脑将每帧的有效分辨率限制为 520,000 像素，并传递内容</td>
        <td><b>HWDRM</b>：使用 HDCP 传递内容。 如果 HDCP 无法使用，将阻止对 HDMI/DVI 端口的播放</td>
    </tr>
    <tr>
        <th>300</th>
        <td colspan="2">
            <p>
                **当 HDCP 类型限制未定义时：** 使用 HDCP 传递内容。 如果 HDCP 无法使用，将阻止对 HDMI/DVI 端口的播放。
            </p>
            <p>
                **当 HDCP 类型限制已定义时**：使用 HDCP 2.2 传递内容，并将内容流类型设置为 1。 如果 HDCP 无法使用或内容流类型无法设置为 1，将阻止对 HDMI/DVI 端口的播放。
            </p>
        </td>
    </tr>
    <tr>
        <th>400</th>
        <td rowspan="2">Windows10 永远不会将压缩的数字视频内容传递到输出，无论后续 OPL 值如何。 有关压缩的数字视频内容的详细信息，请参阅 <a href="https://www.microsoft.com/playready/licensing/compliance/">PlayReady 产品的合规性规则</a>。</td>
        <td colspan="2" rowspan="2">不适用\*</td>
    </tr>
    <tr>
        <th>500</th>
    </tr>
</table>
<br/>

\* 并非所有输出保护级别的值都可以通过授权许可服务器设置。 有关详细信息，请参阅 [PlayReady 合规性规则](https://www.microsoft.com/playready/licensing/compliance/)。

#### <a name="audio"></a>Audio

<table>
    <tr>
        <th rowspan="2">OPL</th>
        <th>压缩的数字音频</th>
        <th>未压缩的数字音频</th>
        <th>模拟或 USB 音频</th>
    </tr>
    <tr>
        <th>HDMI、DisplayPort、MHL</th>
        <th>HDMI、DisplayPort、MHL</th>
        <th>Any</th>
    </tr>
    <tr>
        <th>100</th>
        <td rowspan="3">传递内容</td>
        <td>传递内容</td>
        <td rowspan="5">传递内容</td>
    </tr>
    <tr>
        <th>150</th>
        <td rowspan="4">不传递内容</td>
    </tr>
    <tr>
        <th>200</th>
    </tr>
    <tr>
        <th>250</th>
        <td>在 HDMI、DisplayPort 或 MHL 上使用 HDCP 或者使用 SCMS 并设置为 CopyNever 时，传递内容</td>
    </tr>
    <tr>
        <th>300</th>
        <td>在 HDMI、DisplayPort 或 MHL 上使用 HDCP 时，传递内容</td>
    </tr>
</table>
<br/>

### <a name="miracast"></a>Miracast

PlayReady DRM 允许你在使用 HDCP 2.0 或更高版本后立即通过 Miracast 输出播放内容。 但在 Windows10 上，Miracast 视为*数字*输出。 有关 Miracast 方案的详细信息，请参阅 [PlayReady 合规性规则](https://www.microsoft.com/playready/licensing/compliance/)。 下表概述了 PlayReady 许可证中各种 OPL 间的映射，以及 PlayReady DRM 如何在 Miracast 输出上强制实现它们。

<table>
    <tr>
        <th>OPL</th>
        <th>压缩的数字音频</th>
        <th>未压缩的数字音频</th>
        <th>压缩的数字视频</th>
        <th>未压缩的数字视频</th>
    </tr>
    <tr>
        <th>100</th>
        <td rowspan="4">当使用 HDCP 2.0 或更高版本时，传递内容。 如果它无法使用，则不传递内容</td>
        <td>当使用 HDCP 2.0 或更高版本时，传递内容。 如果它无法使用，则不传递内容</td>
        <td rowspan="6">不适用\*</td>
        <td>当使用 HDCP 2.0 或更高版本时，传递内容。 如果它无法使用，则不传递内容</td>
    </tr>
    <tr>
        <th>150</th>
        <td rowspan="3">不传递内容</td>
        <td rowspan="2">不适用\*</td>
    </tr>
    <tr>
        <th>200</th>
    </tr>
    <tr>
        <th>250</th>
        <td rowspan="2">当使用 HDCP 2.0 或更高版本时，传递内容。 如果它无法使用，则不传递内容</td>
    </tr>
    <tr>
        <th>270</th>
        <td colspan="2">不适用\*</td>
    </tr>
    <tr>
        <th>300</th>
        <td>当使用 HDCP 2.0 或更高版本时，传递内容。 如果它无法使用，则不传递内容</td>
        <td>不传递内容</td>
        <td>
            <p>
                **当 HDCP 类型限制未定义时：** 当使用 HDCP 2.0 或更高版本时，传递内容。 如果它无法使用，则不传递内容。
            </p>
            <p>
                **当 HDCP 类型限制已定义时：** 使用 HDCP 2.2 传递内容，并将内容流类型设置为 1。 如果 HDCP 无法使用或内容流类型无法设置为 1，则不传递内容。
            </p>        
        </td>
    </tr>
    <tr>
        <th>400</th>
        <td rowspan="2" colspan="2">不适用\*</td>
        <td rowspan="2">Windows10 永远不会将压缩的数字视频内容传递到输出，无论后续 OPL 值如何。 有关压缩的数字视频内容的详细信息，请参阅 <a href="https://www.microsoft.com/playready/licensing/compliance/">PlayReady 产品的合规性规则</a>。</td>
        <td rowspan="2">不适用\*</td>
    </tr>
    <tr>
        <th>500</th>
    </tr>
</table>
<br/>

\* 并非所有输出保护级别的值都可以通过授权许可服务器设置。 有关详细信息，请参阅 [PlayReady 合规性规则](https://www.microsoft.com/playready/licensing/compliance/)。

### <a name="additional-explicit-output-restrictions"></a>其他显式输出限制

下表描述了显式数字视频输出保护限制的适用于 Windows10 的 PlayReady DRM 实现。

<table>
    <tr>
        <th>方案</th>
        <th>GUID</th>
        <th>如果...</th>
        <th>则...</th>
    </tr>
    <tr>
        <th>最大有效分辨率解码大小</th>
        <td>9645E831-E01D-4FFF-8342-0A720E3E028F</td>
        <td>连接的输出包括：数字视频输出、Miracast、HDMI、DVI 等。</td>
        <td>
            <p>
                仅在以下情况下传递内容：  
            </p>
            <ul>
                <li>(a) 帧的宽度必须小于或等于最大帧宽度（以像素为单位），并且帧的高度必须小于或等于最大帧高度（以像素为单位），或</li>
                <li>(b) 帧的高度必须小于或等于最大帧宽度（以像素为单位），并且帧的宽度必须小于或等于最大帧高度（以像素为单位）</li>
            </ul>                   
        </td>
    </tr>
    <tr>
        <th>HDCP 类型限制</th>
        <td>ABB2C6F1-E663-4625-A945-972D17B231E7</td>
        <td>连接的输出包括：数字视频输出、Miracast、HDMI、DVI 等。</td>
        <td>使用 HDCP 2.2 传递内容，并将内容流类型设置为 1。 如果 HDCP 2.2 无法使用或内容流类型无法设置为 1，则不传递内容。 还必须指定未压缩的数字视频输出保护级别的值大于或等于 271</td>
    </tr>
</table>
<br/>

下表描述了显式模拟视频输出保护限制的适用于 Windows10 的 PlayReady DRM 实现。

<table>
    <tr>
        <th>方案</th>
        <th>GUID</th>
        <th>如果...</th>
        <th colspan="2">则...</th>
    </tr>
    <tr>
        <th>模拟计算机监视器</th>
        <td>D783A191-E083-4BAF-B2DA-E69F910B3772</td>
        <td>连接的输出包括：VGA、DVI&ndash;模拟等。</td>
        <td> <b>SWDRM：</b>电脑将每帧的有效分辨率限制为 520,000 epx，并传递内容</td>
        <td><b>HWDRM：</b>不传递内容</td>
    </tr>
    <tr>
        <th>模拟组件</th>
        <td>811C5110-46C8-4C6E-8163-C0482A15D47E</td>
        <td>连接的输出包括：组件</td>
        <td><b>SWDRM：</b>电脑将每帧的有效分辨率限制为 520,000 epx，并传递内容</td>
        <td><b>HWDRM：</b>不传递内容</td>
    </tr>
    <tr>
        <th rowspan="2">模拟电视输出</th>
        <td>2098DE8D-7DDD-4BAB-96C6-32EBB6FABEA3</td>
        <td>模拟电视 OPL 小于 151</td>
        <td colspan="2">必须使用 CGMS-A</td>
    </tr>
    <tr>
        <td>225CD36F-F132-49EF-BA8C-C91EA28E4369</td>
        <td>模拟电视 OPL 小于 101，并且许可证不包含 2098DE8D-7DDD-4BAB-96C6-32EBB6FABEA3</td>
        <td colspan="2">必须尝试使用 CGMS，但无论结果如何，都可能会播放内容</td>
    </tr>
    <tr>
        <th>自动增益控制和色条</th>
        <td>C3FD11C6-F8B7-4D20-B008-1DB17D61F2DA</td>
        <td>将分辨率小于或等于 520,000 px 的内容传递到模拟电视输出</td>
        <td colspan="2">根据表 3.5.7.3，当分辨率小于 520,000 px 时，仅为组件视频和 PAL 模式设置 AGC；当分辨率小于 520,000 px 时，为 NTSC 设置 AGC 和色条信息。 在合规性规则中</td>
    </tr>
    <tr>
        <th>仅数字输出</th>
        <td>760AE755-682A-41E0-B1B3-DCDF836A7306</td>
        <td>连接的输出为模拟</td>
        <td colspan="2">不传递内容</td>
    </tr>
</table>
<br/>

> [!NOTE]
> 当使用适配器硬件保护装置（例如“Mini DisplayPort to VGA”）用于播放时，Windows10 会将输出视为数字视频输出，并且无法强制执行模拟视频策略。

下表描述了支持在其他环境中播放的适用于 Windows10 的 PlayReady DRM 实现。

<table>
    <tr>
        <th>方案</th>
        <th>GUID</th>
        <th>如果...</th>
        <th colspan="2">则...</th>
    </tr>
    <tr>
        <th>未知输出</th>
        <td>786627D8-C2A6-44BE-8F88-08AE255B01A7</td>
        <td>如果无法合理地确定输出，则无法使用图形驱动程序建立 OPM</td>
        <td><b>SWDRM：</b>传递内容</td>
        <td><b>HWDRM：</b>不传递内容</td>
    </tr>
    <tr>
        <th>具有限制的未知输出</th>
        <td>B621D91F-EDCC-4035-8D4B-DC71760D43E9</td>
        <td>如果无法合理地确定输出，则无法使用图形驱动程序建立 OPM</td>
        <td> <b>SWDRM：</b>电脑将每帧的有效分辨率限制为 520,000 epx，并传递内容</td>
        <td><b>HWDRM：</b>不传递内容</td>
    </tr>
</table>
<br/>

## <a name="prerequisites"></a>先决条件

在开始创建 PlayReady 保护的 UWP 应用之前，需要在系统上安装以下软件：

-   Windows 10。
-   如果你正在为 PlayReady DRM 编译任何示例适用于 UWP 应用，则必须使用 Microsoft Visual Studio2015 或更高版本编译这些示例。 你仍可以使用 Microsoft Visual Studio2013 编译任何于 Windows8.1 应用商店应用的 PlayReady DRM 的示例。

<!--This is no longer available-->
<!--If you are planning to play back MPEG-2/H.262 content on your app, you must also download and install [Windows 8.1 Media Center Pack](http://go.microsoft.com/fwlink/p/?LinkId=626876).-->

## <a name="playready-uwp-app-migration-guide"></a>PlayReady UWP 应用迁移指南

本部分包含有关如何将现有 PlayReady Windows 8.x 应用商店应用迁移到 windows 10 的信息。

Windows 10 上的 PlayReady UWP 应用的命名空间已从**Microsoft.Media.PlayReadyClient**更改为[**Windows.Media.Protection.PlayReady**](https://msdn.microsoft.com/library/windows/apps/dn986454)。 这意味着你将需要搜索旧命名空间并将其替换为代码中的新命名空间。 你仍将引用 winmd 文件。 它是 windows.media.winmd 的 windows 10 操作系统上的一部分。 它作为 TH 的 Windows SDK 的一部分位于 windows.winmd 中。 对于 UWP，可在 windows.foundation.univeralappcontract.winmd 中引用它。

若要播放 PlayReady 保护的高清 (HD) 内容 (1080p) 和超高清 (UHD) 内容，将需要实现 PlayReady 硬件 DRM。 有关如何实现 PlayReady 硬件 DRM 的信息，请参阅[硬件 DRM](hardware-drm.md)。

硬件 DRM 不支持某些内容。 有关禁用硬件 DRM 并启用 DRM 软件的信息，请参阅[替代硬件 DRM](hardware-drm.md#override-hardware-drm)。

对于媒体保护管理器，请确保代码具有以下设置（如果代码尚无此设置）：

```cs
var mediaProtectionManager = new Windows.Media.Protection.MediaProtectionManager();

mediaProtectionManager.Properties["Windows.Media.Protection.MediaProtectionSystemId"] = 
             '{F4637010-03C3-42CD-B932-B48ADF3A6A54}'
var cpsystems = new Windows.Foundation.Collections.PropertySet();
cpsystems["{F4637010-03C3-42CD-B932-B48ADF3A6A54}"] = 
                "Windows.Media.Protection.PlayReady.PlayReadyWinRTTrustedInput";
mediaProtectionManager.Properties["Windows.Media.Protection.MediaProtectionSystemIdMapping"] = cpsystems;

mediaProtectionManager.Properties["Windows.Media.Protection.MediaProtectionContainerGuid"] = 
                "{9A04F079-9840-4286-AB92-E65BE0885F95}";
```

## <a name="proactively-acquire-a-non-persistent-license-before-playback"></a>在播放前主动获取非永久性许可证

本部分将介绍在播放开始前，如何主动获取非永久性许可证。

在用于 Windows 应用商店应用的以前版本的 PlayReady DRM 中，非永久性许可证只能在播放期间被动获取。 在此版本中，你可以在播放开始前主动获取非永久性许可证。

1.  主动创建可存储非永久性许可证的播放会话。 例如：

    ```cs
    var cpsystems = new Windows.Foundation.Collections.PropertySet();       
    cpsystems["{F4637010-03C3-42CD-B932-B48ADF3A6A54}"] = "Windows.Media.Protection.PlayReady.PlayReadyWinRTTrustedInput"; // PlayReady

    var pmpSystemInfo = new Windows.Foundation.Collections.PropertySet();
    pmpSystemInfo["Windows.Media.Protection.MediaProtectionSystemId"] = "{F4637010-03C3-42CD-B932-B48ADF3A6A54}";
    pmpSystemInfo["Windows.Media.Protection.MediaProtectionSystemIdMapping"] = cpsystems;
    var pmpServer = new Windows.Media.Protection.MediaProtectionPMPServer( pmpSystemInfo );
    ```

2.  将该播放会话绑定到许可证获取类。 例如：

    ```cs
    var licenseSessionProperties = new Windows.Foundation.Collections.PropertySet();
    licenseSessionProperties["Windows.Media.Protection.MediaProtectionPMPServer"] = pmpServer;
    var licenseSession = new Windows.Media.Protection.PlayReady.PlayReadyLicenseSession( licenseSessionProperties );
    ```

3.  创建许可证服务请求。 例如：

    ```cs
    var laSR = licenseSession.CreateLAServiceRequest();
    ```

4.  使用在步骤 3 中创建的服务请求执行许可证获取。 该许可证将存储在播放会话中。
5.  将播放会话绑定到媒体源以供播放。 例如：

    ```cs
    licenseSession.configureMediaProtectionManager( mediaProtectionManager );
    videoPlayer.msSetMediaProtectionManager( mediaProtectionManager );
    ```
    
## <a name="query-for-protection-capabilities"></a>保护功能查询
从 Windows 10 版本 1703 开始，你可以查询 HW DRM 功能，如解码编解码器、分辨率和输出保护 (HDCP)。 执行查询时使用 [**IsTypeSupported**](https://docs.microsoft.com/uwp/api/windows.media.protection.protectioncapabilities.istypesupported) 方法，该方法采用表示要查询支持的功能的字符串和执行查询所适用的主要系统的字符串。 有关受支持的字符串值列表，请参阅 [**IsTypeSupported**](https://docs.microsoft.com/uwp/api/windows.media.protection.protectioncapabilities.istypesupported) 的 API 引用页面。 以下代码示例演示了如何使用此方法。  

    ```cs
    using namespace Windows::Media::Protection;

    ProtectionCapabilities^ sr = ref new ProtectionCapabilities();

    ProtectionCapabilityResult result = sr->IsTypeSupported(
    L"video/mp4; codecs=\"avc1.640028\"; features=\"decode-bpp=10,decode-fps=29.97,decode-res-x=1920,decode-res-y=1080\"",
    L"com.microsoft.playready");

    switch (result)
    {
        case ProtectionCapabilityResult::Probably:
        // Queue up UHD HW DRM video
        break;

        case ProtectionCapabilityResult::Maybe:
        // Check again after UI or poll for more info.
        break;

        case ProtectionCapabilityResult::NotSupported:
        // Do not queue up UHD HW DRM video.
        break;
    }
    ```
## <a name="add-secure-stop"></a>添加安全停止

本部分将介绍如何向 UWP 应用添加安全停止。

安全停止为 PlayReady 设备能够有效地断定因任何给定内容而停止媒体播放的媒体流服务提供了方法。 此功能可确保你的媒体流服务能够准确地执行和报告给定帐户在不同设备上的使用限制。

发送安全停止质询的主要方案有两种：

-   在到达内容末尾或用户中途停止媒体演示文稿，因而媒体演示文稿停止时。
-   在上一个会话意外结束时（如因系统或应用崩溃）。 在启动或关机时，应用将查询任何未解决的安全停止会话，并将发送与任何其他媒体播放分离的质询。

有关安全停止的示例实现，请参阅 PlayReady 示例中的 securestop.cs 文件，该文件位于 [http://go.microsoft.com/fwlink/p/?linkid=331670&clcid=0x409](http://go.microsoft.com/fwlink/p/?linkid=331670)。

## <a name="use-playready-drm-on-xbox-one"></a>在 Xbox One 上使用 PlayReady DRM

若要在 Xbox One 上的 UWP 应用中使用 PlayReady DRM，首先需要注册你要用于发布应用以授权使用 PlayReady 的[合作伙伴中心](https://partner.microsoft.com/dashboard)帐户。 可以通过两种方式之一完成此操作：

* 在 Microsoft 请求权限上提供你的联系人信息。
* 通过将发送到你的合作伙伴中心帐户和公司名称申请获得授权[pronxbox@microsoft.com](mailto:pronxbox@microsoft.com)。

获得授权后，你将需要向应用清单添加额外 `<DeviceCapability>`。 由于应用清单设计器中当前没有可用的设置，你必须手动添加它。 请按照以下步骤配置它：

1. 在 Visual Studio 中打开该项目后，打开**解决方案资源管理器**，然后右键单击 **Package.appxmanifest**。
2. 选择**打开方式...**、选择 **XML(文本)编辑器**，然后单击**确定**。
3. 在 `<Capabilities>` 标记之间，添加以下 `<DeviceCapability>`：

    ```xml
    <DeviceCapability Name="6a7e5907-885c-4bcb-b40a-073c067bd3d5" />
    ```

4. 保存文件。

最后，在 Xbox One 上使用 PlayReady 时需要注意：开发工具包具有 SL150 限制（即它们不能播放 SL2000 或 SL3000 内容）。 零售设备能够播放安全级别较高的内容，但若要在开发工具包上测试你的应用，你需要使用 SL150 内容。 可以通过以下方式之一测试此内容：

* 使用需要 SL150 许可证的特选测试内容。
* 实现逻辑，以便仅某些经身份验证的测试帐户能够获取某些内容的 SL150 许可证。

使用对于你的公司和产品而言最合理的方法。


## <a name="see-also"></a>另请参阅
- [媒体播放](media-playback.md)




