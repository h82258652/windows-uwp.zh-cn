---
ms.assetid: e04ebe3f-479c-4b48-99d8-3dd4bb9bfaf4
title: 使用自定义的 SSL 证书预配 Device Portal
description: 待定
ms.date: 07/11/2017
ms.topic: article
keywords: windows 10，uwp，设备门户
ms.localizationpriority: medium
ms.openlocfilehash: faef15d523f56b6e45f77e0ccdbb2f5846f7a15a
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/03/2018
ms.locfileid: "8480739"
---
# <a name="provision-device-portal-with-a-custom-ssl-certificate"></a>使用自定义的 SSL 证书预配 Device Portal
在 Windows 10 创意者更新中，Windows Device Portal 添加设备管理员可以使用自定义证书安装在 HTTPS 通信的一种方法。 

尽管你可以在自己的电脑上执行此操作，此功能主要旨在提供位置中有一个现有的证书基础结构的企业。  

例如，公司可能拥有它使用 intranet 网站提供通过 HTTPS 的证书进行签名的证书颁发机构 (CA)。 此功能除此之外代表基础结构。 

## <a name="overview"></a>概述
默认情况下，Device Portal 生成自签名的根 CA，并使用的每个终结点正在侦听的 SSL 证书进行签名。 这包括`localhost`， `127.0.0.1`，并`::1`(IPv6 localhost)。

此外包括设备的主机名 (例如， `https://LivingRoomPC`) 和分配给设备的每个链接本地 IP 地址 (最多两个 [IPv4、 IPv6] 每个网络适配器)。 你可以通过查看 Device Portal 中的网络工具查看设备的链接本地 IP 地址。 他们将与开始菜单`10.`或`192.`为 IPv4，或`fe80:`ipv6。 

在默认设置中，可能会显示在浏览器中由于不受信任的根 CA 证书警告。 具体来说，由根的浏览器或电脑不信任的 CA 的 SSL 证书提供 Device Portal 进行签名。 这可以通过创建新的受信任的根 CA 固定。

## <a name="create-a-root-ca"></a>创建的根 CA

这应仅进行如果没有证书基础结构设置，你的公司 （或主页），并且应该仅进行一次。 下面的 PowerShell 脚本创建的根 CA 称为_WdpTestCA.cer_。 将此文件安装到本地计算机的受信任的根证书颁发机构将导致设备信任签名由此根 CA 的 SSL 证书。 你可以 （且应该） 安装你想要连接到 Windows Device Portal 每台电脑上的此.cer 文件。  

```PowerShell
$CN = "PickAName"
$OutputPath = "c:\temp\"

# Create root certificate authority
$FilePath = $OutputPath + "WdpTestCA.cer"
$Subject =  "CN="+$CN
$rootCA = New-SelfSignedCertificate -certstorelocation cert:\currentuser\my -Subject $Subject -HashAlgorithm "SHA512" -KeyUsage CertSign,CRLSign
$rootCAFile = Export-Certificate -Cert $rootCA -FilePath $FilePath
```

这创建后，你可以使用_WdpTestCA.cer_文件进行签名 SSL 证书。 

## <a name="create-an-ssl-certificate-with-the-root-ca"></a>创建具有根 CA 的 SSL 证书

SSL 证书包含两个关键功能： 保护通过加密连接，并验证你实际通信的浏览器栏中显示的地址 (Bing.com，192.168.1.37，等)，并不是恶意的第三方。

下面的 PowerShell 脚本创建的 SSL 证书`localhost`终结点。 Device Portal 侦听每个终结点需要其自己的证书。你可以替换`$IssuedTo`为你的设备在脚本中使用不同的终结点的每个参数： 主机名、 本地主机和 IP 地址。

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

如果你有多个设备，你可以重复使用 localhost.pfx 文件，但仍需要分别创建每个设备的 IP 地址和主机名证书。

在生成捆绑包的.pfx 文件后，你将需要将其加载到 Windows Device Portal。 

## <a name="provision-device-portal-with-the-certifications"></a>使用认证的预配设备门户

每个.pfx 文件，你已创建的设备，你将需要从提升的命令提示符运行以下命令。

```
WebManagement.exe -SetCert <Path to .pfx file> <password for pfx> 
```

请参阅下面的示例用法：
```
WebManagement.exe -SetCert localhost.pfx PickAPassword
WebManagement.exe -SetCert --1.pfx PickAPassword
WebManagement.exe -SetCert MyLivingRoomPC.pfx PickAPassword
```

一旦你已安装的证书，只需重新启动服务使更改生效：

```
sc stop webmanagement
sc start webmanagement
```

> [!TIP]
> IP 地址可随时间变化。
许多网络使用 DHCP 给出 IP 地址，所以设备不始终获取它们之前在具有相同的 IP 地址。 如果你已在设备上创建一个 IP 地址的证书，设备的地址已更改 Windows Device Portal 将生成一个新的证书，使用现有的自签名的证书，并且会停止使用你创建一个。 这将导致证书警告页面，就会再次出现在浏览器中。 出于此原因，我们建议连接到你通过相应的主机名，你可以设置设备门户中的设备。 这些将保持不变无论 IP 地址。
