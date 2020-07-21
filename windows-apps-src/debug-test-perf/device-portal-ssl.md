---
ms.assetid: e04ebe3f-479c-4b48-99d8-3dd4bb9bfaf4
title: 使用自定义的 SSL 证书预配设备门户
description: 了解如何通过在 HTTPS 通信中使用的自定义证书预配 Windows 设备门户。
ms.date: 04/19/2019
ms.topic: article
keywords: windows 10, uwp, 设备门户
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: ce4e45bc23f4efb636618bb4891b9d6e9d207490
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "63774150"
---
# <a name="provision-device-portal-with-a-custom-ssl-certificate"></a>使用自定义的 SSL 证书预配设备门户

在 Windows 10 Creators Update 中，Windows 设备门户添加了一种供设备管理员安装用于 HTTPS 通信的自定义证书的方法。

虽然你可以在自己的电脑上执行此操作，但此功能主要适用于建立了现有证书基础结构的企业。  

例如，某公司可能有一个证书颁发机构 (CA)，并使用该机构对通过 HTTPS 提供的 Intranet 网站证书进行签名。 此功能建立在该基础结构之上。

## <a name="overview"></a>概述

默认情况下，设备门户会生成一个自签名的根 CA，然后使用它对正在侦听的每个终结点的 SSL 证书进行签名。 这包括 `localhost`、`127.0.0.1` 和 `::1` (IPv6 localhost)。

另外，还包括设备的主机名（例如 `https://LivingRoomPC`）和分配给设备的每个链接-本地 IP 地址（每个网络适配器最多有两个 [IPv4、IPv6]）。
通过在设备门户中查看网络工具，可以查看设备的链接-本地 IP 地址。 对于 IPv4，这些地址将以 `10.` 或 `192.` 开头；对于 IPv6，将以 `fe80:` 开头。

在默认设置中，由于根 CA 不受信任，可能会在浏览器中显示证书警告。 具体来说，设备门户提供的 SSL 证书由浏览器或电脑不信任的根 CA 进行签名。 此问题可以通过创建受信任的新根 CA 来解决。

## <a name="create-a-root-ca"></a>创建根 CA

仅当公司（或家庭）未建立证书基础结构时，才应执行此操作，并且此操作只应执行一次。 以下 PowerShell 脚本会创建一个称为 _WdpTestCA.cer_ 的根 CA。 如果将此文件安装到本地计算机的受信任的根证书颁发机构，则会导致设备信任由该根 CA 签名的 SSL 证书。 可以（并且应该）在要连接到 Windows 设备门户的每台电脑上安装此 .cer 文件。  

```PowerShell
$CN = "PickAName"
$OutputPath = "c:\temp\"

# Create root certificate authority
$FilePath = $OutputPath + "WdpTestCA.cer"
$Subject =  "CN="+$CN
$rootCA = New-SelfSignedCertificate -certstorelocation cert:\currentuser\my -Subject $Subject -HashAlgorithm "SHA512" -KeyUsage CertSign,CRLSign
$rootCAFile = Export-Certificate -Cert $rootCA -FilePath $FilePath
```

创建 _WdpTestCA.cer_ 文件后，可以使用它对 SSL 证书进行签名。

## <a name="create-an-ssl-certificate-with-the-root-ca"></a>使用根 CA 创建 SSL 证书

SSL 证书有以下两个关键功能：通过加密保护连接，以及验证你实际上是否正在与浏览器栏中显示的地址（Bing.com、192.168.1.37 等）而非恶意的第三方通信。

以下 PowerShell 脚本会为 `localhost` 终结点创建 SSL 证书。 设备门户侦听的每个终结点都需要其自己的证书；可以将脚本中的 `$IssuedTo` 参数替换为设备的每个不同终结点：主机名、localhost 和 IP 地址。

```PowerShell
$IssuedTo = "localhost"
$Password = "PickAPassword"
$OutputPath = "c:\temp\"
$rootCA = Import-Certificate -FilePath C:\temp\WdpTestCA.cer -CertStoreLocation Cert:\CurrentUser\My\

# Create SSL cert signed by certificate authority
$IssuedToClean = $IssuedTo.Replace(":", "-").Replace(" ", "_")
$FilePath = $OutputPath + $IssuedToClean + ".pfx"
$Subject = "CN="+$IssuedTo
$cert = New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -Subject $Subject -DnsName $IssuedTo -Signer $rootCA -HashAlgorithm "SHA512"
$certFile = Export-PfxCertificate -cert $cert -FilePath $FilePath -Password (ConvertTo-SecureString -String $Password -Force -AsPlainText)
```

如果有多台设备，则可重复使用 localhost .pfx 文件，但是仍需单独为每台设备创建 IP 地址和主机名证书。

生成 .pfx 文件捆绑包时，需要将它们加载到 Windows 设备门户。

## <a name="provision-device-portal-with-the-certifications"></a>使用证书预配设备门户

对于已为设备创建的每个 .pfx 文件，都需要在提升的命令提示符下运行以下命令。

```cmd
WebManagement.exe -SetCert <Path to .pfx file> <password for pfx>
```

有关用法示例，请参阅下文：

```cmd
WebManagement.exe -SetCert localhost.pfx PickAPassword
WebManagement.exe -SetCert --1.pfx PickAPassword
WebManagement.exe -SetCert MyLivingRoomPC.pfx PickAPassword
```

安装证书后，只需重启服务，更改即可生效：

```cmd
sc stop webmanagement
sc start webmanagement
```

> [!TIP]
> IP 地址可能会随着时间的推移发生更改。
许多网络使用 DHCP 分发 IP 地址，因此设备并不总是会获得与之前相同的 IP 地址。 如果针对设备上的 IP 地址创建了证书，但该设备的地址已更改，则 Windows 设备门户会使用现有的自签名证书生成新的证书，并停止使用你创建的证书。 这会导致浏览器中再次出现证书警告页面。 因此，建议你通过可以在设备门户中设置的主机名连接到自己的设备。 无论 IP 地址如何，这些内容将保持不变。
