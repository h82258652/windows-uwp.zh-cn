---
author: drewbatgit
ms.assetid: A7E0DA1E-535A-459E-9A35-68A4150EE9F5
description: 本主题将概述如何向通用 Windows 平台 (UWP) 应用添加基于 PlayReady 硬件的数字版权管理 (DRM)。
title: 硬件 DRM
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6e898e342cb26ab86a74c30b5ced3dd3a9b68165
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/19/2018
ms.locfileid: "7279405"
---
# <a name="hardware-drm"></a>硬件 DRM


本主题将概述如何向通用 Windows 平台 (UWP) 应用添加基于 PlayReady 硬件的数字版权管理 (DRM)。

> [!NOTE] 
> 基于硬件的 PlayReady DRM 在许多设备上均受支持，包括电视机、手机以及平板电脑之类的 Windows 和非 Windows 设备。 对于要支持 PlayReady 硬件 DRM 的 Windows 设备，它必须运行 Windows 10，并且具有受支持的硬件配置。

越来越多的内容提供商选择基于硬件的保护，以授权在应用中播放完整高值内容。 为满足此要求，已向 PlayReady 添加了针对加密核心硬件实现的可靠支持。 此支持可以使多个设备平台安全播放高清 (1080p) 和超高清 (UHD) 内容。 利用硬件安全保护密钥材料（包括私钥、内容密钥和任何其他用于派生或解锁上述密钥的密钥材料）以及解密的压缩和未压缩视频示例。

## <a name="windows-tee-implementation"></a>Windows TEE 实现

本主题提供简要概述了 windows 10 如何实现受信任的执行环境 (TEE)。

Windows TEE 实现的详细信息不在本文档范围内。 但是，简要讨论标准移植工具包 TEE 端口和 Windows 端口之间的区别将非常有益。 Windows 可实现 OEM 代理层，并将序列化 PRITEE 函数调用传输到 Windows Media Foundation 子系统中的用户模式驱动程序。 这将最终传送至 Windows TrEE（受信任的执行环境）驱动程序或 OEM 图形驱动程序。 有关这些方法的详细信息已超出了本文档范围。 下图显示了 Windows 端口的常规组件交互。 如果想要开发 Windows PlayReady TEE 实现，可以联系 <WMLA@Microsoft.com>。

![Windows TEE 组件图](images/windowsteecomponentdiagram720.jpg)

## <a name="considerations-for-using-hardware-drm"></a>使用硬件 DRM 的注意事项

本主题提供在开发旨在使用硬件 DRM 的应用时应考虑的事项的简短列表。 如 [PlayReady DRM](playready-client-sdk.md#output-protection) 中所述，使用适用于 Windows 10 的 PlayReady HWDRM 时，所有输出保护均可从 Windows TEE 实现中强制执行，从而对输出保护行为产生一些影响：

-   **对未压缩的数字视频 270 的输出保护级别 (OPL) 的支持：** 适用于 Windows 10 的 PlayReady HWDRM 不支持向下分辨率，并将强制使用 HDCP。 我们建议 HWDRM 的高清内容具有大于 270 的 OPL（尽管这并不是必需的）。 另外，Microsoft 建议你在许可证中设置 HDCP 类型限制（Windows 10 上的 HDCP 版本 2.2）。
-   **不同于软件 DRM (SWDRM)，输出保护在基于最低功能的监视器的所有监视器上强制执行。** 例如，如果用户连接了两台监视器，其中一台显示器支持 HDCP，而另一台不支持，则即使仅在支持 HDCP 的监视器上呈现内容，但如果许可证需要 HDCP，播放也将失败。 在软件 DRM 中，只要仅在支持 HDCP 的监视器上呈现该内容，内容便会播放。
-   **HWDRM 不保证客户端一定可以使用 HWDRM 以及一定安全，除非内容密钥和许可证满足以下条件**：
    -   用于视频内容密钥的许可证必须至少具有 3000 的安全级别属性。
    -   音频必须加密为不同于视频的内容密钥，并且用于音频的许可证必须至少具有 2000 安全级别属性。 此外，音频可能保持清晰。
    
此外，使用 HWDRM 时应考虑以下各项：

-   不支持受保护的媒体进程 (PMP)。
-   Windows Media 视频（也称为 VC-1）不受支持（请参阅[替代硬件 DRM](#override-hardware-drm)）。
-   永久性许可证不支持多个图形处理单元 (GPU)。

若要在具有多个 GPU 的计算机上处理永久性许可证，请考虑以下方案：

1.  客户购买具有集成显卡的新计算机。
2.  客户使用在使用硬件 DRM 时获取永久性许可证的应用。
3.  永久性许可证现已绑定到该显卡的硬件密钥。
4.  然后，客户即可安装新的显卡。
5.  经过哈希处理的数据存储 (HDS) 中的所有许可证均将绑定到集成视频卡，但客户现在希望使用新安装的图形卡播放受保护的内容。

为了防止播放由于硬件无法解密许可证而失败，PlayReady 为其遇到的每个图形卡使用单独的 HDS。 这将导致 PlayReady 尝试为一部分内容获取许可证，但 PlayReady 已经具有该部分内容的许可证（即，在软件 DRM 的情况下或任何其他没有硬件更改的情况下，PlayReady 无需重新获取许可证）。 因此，如果应用在使用硬件 DRM 时获取了持久性许可证，它必须能够在最终用户安装（或卸载）显卡时，处理该许可证有效“丢失”的情况。 由于这不是常见情况，当硬件更改后内容不再播放时，你可能会决定处理支持电话，而不是搞清楚如何在客户端/服务器代码中处理硬件更改。

## <a name="override-hardware-drm"></a>替代硬件 DRM

本部分介绍如何在要播放的内容不支持硬件 DRM 时替代硬件 DRM (HWDRM)。

默认情况下，如果系统支持，则使用硬件 DRM。 但是，硬件 DRM 不支持某些内容。 其中一个示例为鸡尾酒内容。 另一个示例是使用 H.264 和 HEVC 之外的视频编解码器的所有内容。 另一个示例是 HEVC 内容，因为一些硬件 DRM 支持 HEVC 而另一些不支持。 因此，如果希望播放某条内容，但在问题系统上的硬件 DRM 不支持此操作，可以选择退出硬件 DRM。

以下示例将说明如何选择退出硬件 DRM。 只需在切换之前执行此操作。 此外，还要确保内存中没有任何 PlayReady 对象，否则行为将是未定义行为。

```js
var applicationData = Windows.Storage.ApplicationData.current;
var localSettings = applicationData.localSettings.createContainer("PlayReady", Windows.Storage.ApplicationDataCreateDisposition.always);
localSettings.values["SoftwareOverride"] = 1;
```

若要切换回硬件 DRM，请将 **SoftwareOverride** 值设置为 **0**。

对于每一次媒体播放，你都需要将 **MediaProtectionManager** 设置为：

```js
mediaProtectionManager.properties["Windows.Media.Protection.UseSoftwareProtectionLayer"] = true;
```

如果你是硬件 DRM 还是软件 DRM 是查看 C:\\Users\\ 的最佳方式&lt;用户名&gt;\\AppData\\Local\\Packages\\&lt;应用程序名称&gt;\\LocalCache\\PlayReady\\\*

-   如果存在 mspr.hds 文件，则使用的是软件 DRM。
-   如果你具有另一个 \*.hds 文件，则使用的是硬件 DRM。
-   也可以删除整个 PlayReady 文件夹，然后重新进行测试。

## <a name="detect-the-type-of-hardware-drm"></a>检测硬件 DRM 的类型

本部分介绍如何检测系统上支持哪些类型的硬件 DRM。

可以使用 [**PlayReadyStatics.CheckSupportedHardware**](https://msdn.microsoft.com/library/windows/apps/dn986441) 方法确定系统是否支持特定硬件 DRM 功能。 例如：

```csharp
bool isFeatureSupported = PlayReadyStatics.CheckSupportedHardware(PlayReadyHardwareDRMFeatures.HEVC);
```

[**PlayReadyHardwareDRMFeatures**](https://msdn.microsoft.com/library/windows/apps/dn986265) 枚举包含可查询的硬件 DRM 功能值有效列表。 若要确定硬件 DRM 是否受支持，请使用查询中的 **HardwareDRM** 成员。 若要确定硬件是否支持高效率视频编码 (HEVC)/H.265 编解码器，请使用查询中的 **HEVC** 成员。

还可以使用 [**PlayReadyStatics.PlayReadyCertificateSecurityLevel**](https://msdn.microsoft.com/library/windows/apps/windows.media.protection.playready.playreadystatics.playreadycertificatesecuritylevel.aspx) 属性获取客户端证书的安全级别，以确定硬件 DRM 是否受支持。 除非返回的证书安全级别大于或等于3000，否则将不个性化或预配客户端（在这种情况下此属性返回值为 0），或者硬件 DRM 将处于未使用状态（在这种情况下该属性将返回小于 3000 的值）。

### <a name="detecting-support-for-aes128cbc-hardware-drm"></a>检测 AES128CBC 硬件 DRM 的支持
从 Windows 10 版本 1709 开始，可以通过调用 **[PlayReadyStatics.CheckSupportedHardware](https://msdn.microsoft.com/library/windows/apps/dn986441)** 和指定枚举值 [**PlayReadyHardwareDRMFeatures.Aes128Cbc**](https://msdn.microsoft.com/library/windows/apps/dn986265) 来检测设备上的 AES128CBC 硬件加密的支持。 在以前版本的 Windows 10 中，指定此值将导致异常。 因此，应该通过调用 **[ApiInformation.IsApiContractPresent](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation.isapicontractpresent)** 并在调用 **CheckSupportedHardware** 之前指定主要合同版本 5 来检查枚举值是否存在。

```csharp
bool supportsAes128Cbc = ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 5);

if (supportsAes128Cbc)
{
    supportsAes128Cbc = PlayReadyStatics.CheckSupportedHardware(PlayReadyHardwareDRMFeatures.Aes128Cbc);
}
```

## <a name="see-also"></a>另请参阅
- [PlayReady DRM](playready-client-sdk.md)
