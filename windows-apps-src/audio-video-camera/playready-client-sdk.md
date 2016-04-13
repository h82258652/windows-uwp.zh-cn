---
ms.assetid: DD8FFA8C-DFF0-41E3-8F7A-345C5A248FC2
本主题介绍了如何向通用 Windows 平台 (UWP) 应用添加 PlayReady 保护的媒体内容。
PlayReady DRM
---

# PlayReady DRM

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


本主题介绍了如何向通用 Windows 平台 (UWP) 应用添加 PlayReady 保护的媒体内容。

PlayReady DRM 允许开发人员在强制执行内容提供商定义的访问规则的同时，创建能够向用户提供 PlayReady 内容的 UWP 应用。 本部分介绍了对适用于 Windows 10 的 Microsoft PlayReady DRM 所做的更改，以及如何修改 PlayReady UWP 应用以支持从以前的 Windows 8.1 版本到 Windows 10 版本所做的更改。
 
| 主题                                                                     | 描述                                                                                                                                                                                                                                                                             |
|---------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [硬件 DRM](hardware-drm.md)                                           | 本主题概述了如何向 UWP 应用添加基于 PlayReady 硬件的数字版权管理 (DRM)。                                                                                                                                                                 |
| [使用 PlayReady 的自适应流式处理](adaptive-streaming-with-playready.md) | 本文介绍如何将使用 Microsoft PlayReady 内容保护的多媒体内容自适应流式处理添加到通用 Windows 平台 (UWP) 应用。 此功能当前支持 Http 实时流 (HLS) 和 HTTP 动态流 (DASH) 内容的播放。 |

## PlayReady DRM 中的新增功能

下表介绍适用于 Windows 10 的 PlayReady DRM 的新增功能和所做更改。

-   添加了硬件数字版权管理 (DRM)。

    基于硬件的内容保护支持可以在多个设备平台上安全播放高清 (HD) 和超高清 (UHD) 内容。 利用硬件安全，可保护密钥材料（包括私钥、内容密钥和任何其他用于派生或解锁上述密钥的密钥材料）以及解密的压缩和未压缩视频示例。 使用硬件 DRM 时，两项未知启用程序（播放到未知/通过 downres 播放到未知）均没有意义，因为 HWDRM 管道始终知道所使用的输出。 有关详细信息，请参阅[硬件 DRM](hardware-drm.md)。

-   PlayReady 不再是 appX 框架组件，而是内置操作系统组件。 命名空间已从 **Microsoft.Media.PlayReadyClient** 更改为 [**Windows.Media.Protection.PlayReady**](https://msdn.microsoft.com/library/windows/apps/dn986454)。
-   以下定义 PlayReady 错误代码的标头现在是 Windows 软件开发工具包 (SDK) 的一部分：Windows.Media.Protection.PlayReadyErrors.h 和 Windows.Media.Protection.PlayReadyResults.h。
-   可主动获取非永久性许可证。

    以前版本的 PlayReady DRM 不支持主动获取非永久性许可证。 此功能已添加到此版本。 这可以减少到第一帧的时间。 有关详细信息，请参阅[在播放前主动获取非永久性许可证](#proactively_acquire_a_non_persistent_license_before_playback)。

-   在一条消息中可获取多个许可证。

    允许客户端应用在一条许可证获取消息中获取多个非永久性许可证。 这可以在用户仍然浏览内容库的同时，通过获取多条内容的许可证，减少到第一帧的时间；这将在用户选择要播放的内容时防止许可证获取延迟。 此外，它通过启用包含多个密钥标识符 (KID) 的内容标头，允许加密音频流和视频流以分隔密钥；这允许单个许可证获取内容文件中所有流的所有许可证，而不必使用自定义逻辑和多个许可证获取请求来实现相同的结果。

-   添加了实时过期时间支持，或有限持续时间许可证 (LDL)。

    提供了在许可证上设置实时到期和从到期许可证平滑过渡到另一个正在播放中的（有效）许可证的功能。 当与一条消息中的多个许可证获取结合时，此功能允许应用在用户仍然浏览内容库的同时，以异步方式获取几个 LDL，并且仅在用户已选择要播放的内容后获取更长持续时间许可证。 然后，播放将更快启动（因为已经有可用的许可证），并且由于应用在 LDL 到期时将获取更长持续时间许可证，所以将顺利继续播放直到内容末尾而不会中断。

-   添加的非永久性许可证链。
-   非永久性许可证上添加的基于时间限制的支持（包括到期、第一次播放后到期和实时到期）。
-   添加的 HDCP 类型 1（版本 2.2）策略支持。

    有关详细信息，请参阅[需要考虑的事项](#things_to_consider)。

-   Miracast 现在已隐式为输出。
-   添加了安全停止。

    安全停止为 PlayReady 设备能够有效地断定因任何给定内容而停止媒体播放的媒体流服务提供了方法。 此功能可确保你的媒体流服务能够准确地执行和报告给定帐户在不同设备上的使用限制。

-   添加了音频和视频许可证分离。

    单独的轨道可避免视频被解码为音频；实现更可靠的内容保护。 新兴标准要求对音轨和视频轨使用单独的密钥。

-   添加了 MaxResDecode。

    添加此功能是为了在即使具有功能更强的密钥（但不是许可证）的情况下，也将内容播放限制在最大分辨率以内。 它支持使用单个密钥对多个媒体流大小进行编码的情况。

以下新接口、类和枚举已添加到 PlayReady DRM：

-   [
            **IPlayReadyLicenseAcquisitionServiceRequest**](https://msdn.microsoft.com/library/windows/apps/dn986077) 接口
-   [
            **IPlayReadyLicenseSession**](https://msdn.microsoft.com/library/windows/apps/dn986080) 接口
-   [
            **IPlayReadySecureStopServiceRequest**](https://msdn.microsoft.com/library/windows/apps/dn986090) 接口
-   [
            **PlayReadyLicenseSession**](https://msdn.microsoft.com/library/windows/apps/dn986309) 类
-   [
            **PlayReadySecureStopIterable**](https://msdn.microsoft.com/library/windows/apps/dn986371) 类
-   [
            **PlayReadySecureStopIterator**](https://msdn.microsoft.com/library/windows/apps/dn986375) 类
-   [
            **PlayReadyHardwareDRMFeatures**](https://msdn.microsoft.com/library/windows/apps/dn986265) 枚举器

已创建新示例，用于演示如何使用 PlayReady DRM 的新功能。 可从 [http://go.microsoft.com/fwlink/p/?linkid=331670&clcid=0x409](http://go.microsoft.com/fwlink/p/?linkid=331670) 处下载该示例。

## 要考虑的事项

-   PlayReady DRM 现在支持 HDCP 类型 1 （版本 2.2 或更高版本）。 PlayReady 将强制实施设备许可证中的策略。 此功能可在 PlayReady 服务器 v3.0 SDK 许可证中启用（服务器在许可证中使用播放启用程序 **GUID** 控制此策略）。 有关详细信息，请参阅 [PlayReady 合规性和可靠性规则](http://www.microsoft.com/playready/licensing/compliance/)。
-   Windows Media 视频（也称为 VC-1）在硬件 DRM 中不受支持（请参阅[替代硬件 DRM](hardware-drm.md#override-hardware-drm)）。
-   PlayReady DRM 现在支持高效率视频编码 (HEVC /H.265) 视频压缩标准。 若要支持 HEVC，应用必须使用常用加密方案 (CENC) 版本 2 内容（它包括以明文形式离开内容的切片标头）。 有关详细信息，请参阅 ISO/IEC 23001 7 信息技术 -- MPEG 系统技术 - 部分 7：ISO 基本媒体文件格式文件中的常用加密（需要规范版本 ISO/IEC 23001-7:2015 或更高版本。） Microsoft 还建议将 CENC 版本 2 用于所有 HWDRM 内容。 此外，一些硬件 DRM 可支持 HEVC，而另一些硬件 DRM 则不支持（请参阅[替代硬件 DRM](hardware-drm.md#override-hardware-drm)）。
-   若要利用某些新的 PlayReady 3.0 功能（包括但不限于适用于基于硬件的客户端的 SL3000、在一个许可证获取消息中获取多个非永久性许可证以及对非永久性许可证的基于时间的限制），要求 PlayReady 服务器是 Microsoft PlayReady 服务器软件开发工具包 v3.0.2769 发行版本或更高版本。
-   根据内容许可证指定的输出保护策略，如果媒体播放连接的输出不支持这些要求，它将无法让最终用户使用。 下表列出了作为结果发生的常见错误集。 有关详细信息，请参阅 [PlayReady 合规性和可靠性规则](http://www.microsoft.com/playready/licensing/compliance/)。

| 错误                                                   | 值      | 描述                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|---------------------------------------------------------|------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ERROR\_GRAPHICS\_OPM\_OUTPUT\_DOES\_NOT\_SUPPORT\_HDCP  | 0xC0262513 | 许可证的输出保护策略要求监视器执行 HDCP，但 HDCP 无法执行。                                                                                                                                                                                                                                                                                                                                                                                              |
| MF\_E\_POLICY\_UNSUPPORTED                              | 0xC00D7159 | 许可证的输出保护策略要求监视器执行 HDCP 类型 1，但 HDCP 类型 1 无法执行。                                                                                                                                                                                                                                                                                                                                                                                |
| DRM\_E\_TEE\_OUTPUT\_PROTECTION\_REQUIREMENTS\_NOT\_MET | 0x8004CD22 | 此错误代码仅在硬件 DRM 下运行时才发生。 许可证输出保护策略要求监视器执行 HDCP，或减少内容的有效分辨率，但 HDCP 无法执行，并且内容的有效分辨率也无法减少（因为硬件 DRM 不支持降低内容分辨率）。 内容将在软件 DRM 下播放。 请参阅[使用硬件 DRM 的注意事项](hardware-drm.md#considerations-for-using-hardware-drm)。 |
| ERROR\_GRAPHICS\_OPM\_NOT\_SUPPORTED                    | 0xc0262500 | 图形驱动程序不支持输出保护。 例如，监视器通过 VGA 连接，或者未安装数字输出的相应图形驱动程序。 在后一种情况下，所安装的典型驱动程序是 Microsoft 基本显示适配器，并且安装相应图形驱动程序将解决该问题。                                                                                                                                                  |

## 先决条件

在开始创建 PlayReady 保护的 UWP 应用之前，需要在系统上安装以下软件：

-   Windows 10。
-   如果你正在为适用于 UWP 应用的 PlayReady DRM 编译任何示例，则必须使用 Microsoft Visual Studio 2015 或更高版本编译这些示例。 你仍可以使用 Microsoft Visual Studio 2013 编译适用于 Windows 8.1 应用商店应用的 PlayReady DRM 的任何示例。

如果你计划在应用上播放 MPEG-2/H.262 内容，还必须下载并安装 [Windows 8.1 Media Center Pack](http://go.microsoft.com/fwlink/p/?LinkId=626876)。

## PlayReady Windows 应用商店应用迁移指南

本部分包括有关如何将现有 PlayReady Windows 8.x 应用商店应用迁移到 Windows 10 的信息。

Windows 10 上的 PlayReady UWP 应用的命名空间已从 **Microsoft.Media.PlayReadyClient** 更改为 [**Windows.Media.Protection.PlayReady**](https://msdn.microsoft.com/library/windows/apps/dn986454)。 这意味着你将需要搜索旧命名空间并将其替换为代码中的新命名空间。 你仍将引用 winmd 文件。 它是 Windows 10 操作系统上的 windows.media.winmd 的一部分。 它作为 TH 的 Windows SDK 的一部分位于 windows.winmd 中。 对于 UWP，可在 windows.foundation.univeralappcontract.winmd 中引用它。

若要播放 PlayReady 保护的高清 (HD) 内容 (1080p) 和超高清 (UHD) 内容，将需要实现 PlayReady 硬件 DRM。 有关如何实现 PlayReady 硬件 DRM 的信息，请参阅[硬件 DRM](hardware-drm.md)。

硬件 DRM 不支持某些内容。 有关禁用硬件 DRM 并启用 DRM 软件的信息，请参阅[替代硬件 DRM](hardware-drm.md#override-hardware-drm)。

对于媒体保护管理器，请确保代码具有以下设置（如果代码尚无此设置）：

``` syntax
var mediaProtectionManager = new Windows.Media.Protection.MediaProtectionManager();

mediaProtectionManager.properties["Windows.Media.Protection.MediaProtectionSystemId"] = 
             '{F4637010-03C3-42CD-B932-B48ADF3A6A54}'
var cpsystems = new Windows.Foundation.Collections.PropertySet();
cpsystems["{F4637010-03C3-42CD-B932-B48ADF3A6A54}"] = 
                "Windows.Media.Protection.PlayReady.PlayReadyWinRTTrustedInput";
mediaProtectionManager.properties["Windows.Media.Protection.MediaProtectionSystemIdMapping"] = cpsystems;

mediaProtectionManager.properties["Windows.Media.Protection.MediaProtectionContainerGuid"] = 
                "{9A04F079-9840-4286-AB92-E65BE0885F95}";
```

## 在播放前主动获取非永久性许可证

本部分将介绍在播放开始前，如何主动获取非永久性许可证。

在用于 Windows 应用商店应用的以前版本的 PlayReady DRM 中，非永久性许可证只能在播放期间被动获取。 在此版本中，你可以在播放开始前主动获取非永久性许可证。

1.  主动创建可存储非永久性许可证的播放会话。 例如：

    ``` syntax
    var cpsystems = new Windows.Foundation.Collections.PropertySet();       
    cpsystems["{F4637010-03C3-42CD-B932-B48ADF3A6A54}"] = "Windows.Media.Protection.PlayReady.PlayReadyWinRTTrustedInput"; // PlayReady

    var pmpSystemInfo = new Windows.Foundation.Collections.PropertySet();
    pmpSystemInfo["Windows.Media.Protection.MediaProtectionSystemId"] = "{F4637010-03C3-42CD-B932-B48ADF3A6A54}";
    pmpSystemInfo["Windows.Media.Protection.MediaProtectionSystemIdMapping"] = cpsystems;
    var pmpServer = new Windows.Media.Protection.MediaProtectionPMPServer( pmpSystemInfo );
    ```

2.  将该播放会话绑定到许可证获取类。 例如：

    ``` syntax
    var licenseSessionProperties = new Windows.Foundation.Collections.PropertySet();
    licenseSessionProperties["Windows.Media.Protection.MediaProtectionPMPServer"] = pmpServer;
    var licenseSession = new Windows.Media.Protection.PlayReady.PlayReadyLicenseSession( licenseSessionProperties );
    ```

3.  创建许可证服务请求。 例如：

    ``` syntax
    var laSR = licenseSession.CreateLAServiceRequest();
    ```

4.  使用在步骤 3 中创建的服务请求执行许可证获取。 该许可证将存储在播放会话中。
5.  将播放会话绑定到媒体源以供播放。 例如：

    ``` syntax
    licenseSession.configureMediaProtectionManager( mediaProtectionManager );
    videoPlayer.msSetMediaProtectionManager( mediaProtectionManager );
    ```
    
## 添加安全停止

本部分将介绍如何向 UWP 应用添加安全停止。

安全停止为 PlayReady 设备能够有效地断定因任何给定内容而停止媒体播放的媒体流服务提供了方法。 此功能可确保你的媒体流服务能够准确地执行和报告给定帐户在不同设备上的使用限制。

发送安全停止质询的主要方案有两种：

-   在到达内容末尾或用户中途停止媒体演示文稿，因而媒体演示文稿停止时。
-   在上一个会话意外结束时（如因系统或应用崩溃）。 在启动或关机时，应用将查询任何未解决的安全停止会话，并将发送与任何其他媒体播放分离的质询。

有关安全停止的示例实现，请参阅 PlayReady 示例中的 securestop.cs 文件，该文件位于 [http://go.microsoft.com/fwlink/p/?linkid=331670&clcid=0x409](http://go.microsoft.com/fwlink/p/?linkid=331670)。

 

 






<!--HONumber=Mar16_HO1-->


