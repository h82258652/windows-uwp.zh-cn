---
author: PatrickFarley
ms.assetid: e04ebe3f-479c-4b48-99d8-3dd4bb9bfaf4
title: 使用自定义的 SSL 证书预配 Device Portal
description: 待定
ms.author: pafarley
ms.date: 07/11/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10，uwp，设备门户
ms.localizationpriority: medium
ms.openlocfilehash: 1192c200cd42ab28cc7e763c06fd8a5638aa3400
ms.sourcegitcommit: 106aec1e59ba41aae2ac00f909b81bf7121a6ef1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/15/2018
ms.locfileid: "4617794"
---
# <a name="provision-device-portal-with-a-custom-ssl-certificate"></a>使用自定义的 SSL 证书预配 Device Portal
在 Windows 10 创意者更新，Windows Device Portal 添加设备管理员使用自定义的证书安装在 HTTPS 通信的一种方法。 

尽管你可以在自己的电脑上执行此操作，此功能主要适用于企业就地具有现成的证书基础结构。  

例如，公司可能拥有它使用 intranet 网站提供通过 HTTPS 的证书进行签名的证书颁发机构 (CA)。 此功能的代表基础结构。 

## <a name="overview"></a>概述
默认情况下，Device Portal 生成自签名的根 CA，并使用的每个终结点正在侦听的 SSL 证书进行签名。 这包括`localhost`， `127.0.0.1`，并`::1`(IPv6 localhost)。

此外包括设备的主机名 (例如， `https://LivingRoomPC`) 和分配给设备每个链接本地 IP 地址 (最多两个 [IPv4、 IPv6] 每个网络适配器)。 你可以通过查看 Device Portal 中的网络工具查看设备的链接本地 IP 地址。 他们将开始使用`10.`或`192.`为 IPv4，或`fe80:`ipv6。 

在默认设置中，可能会显示在浏览器中由于不受信任的根 CA 证书警告。 具体来说，提供 Device Portal 的 SSL 证书进行签名的根的浏览器或电脑不信任的 CA。 这可以通过创建新的受信任的根 CA 固定。

## <a name="create-a-root-ca"></a>创建的根 CA

这仅应如果你的公司 （或主页） 没有证书基础结构设置，并应仅进行一次。 以下 PowerShell 脚本创建的根 CA 调用_WdpTestCA.cer_。 将此文件安装到本地计算机的受信任的根证书颁发机构将导致你的设备信任签名由此根 CA 的 SSL 证书。 你可以 （且应该） 安装此.cer 文件你想要连接到 Windows Device Portal 每台电脑上。  

```PowerShell
$CN = "PickAName"
$OutputPath = "c:\temp\"

# Create root certificate authority
$FilePath = $OutputPath + "WdpTestCA.cer"
$Subject =  "CN="+$CN
$rootCA = New-SelfSignedCertificate -certstorelocation cert:\currentuser\my -Subject $Subject -HashAlgorithm "SHA512" -KeyUsage CertSign,CRLSign
$rootCAFile = Export-Certificate -Cert $rootCA -FilePath $FilePath
```

这创建后，你可以使用_WdpTestCA.cer_文件签名 SSL 证书。 

## <a name="create-an-ssl-certificate-with-the-root-ca"></a>创建具有根 CA 的 SSL 证书

SSL 证书具有两个关键功能： 保护通过加密连接并验证你实际通信的浏览器栏中显示的地址 (Bing.com，192.168.1.37，等等) 并不是恶意的第三方。

以下 PowerShell 脚本创建的 SSL 证书`localhost`终结点。 Device Portal 侦听每个终结点需要其自己的证书。你可以替换`$IssuedTo`为你的设备在脚本中使用的不同终结点的每个参数： 主机名、 本地主机和 IP 地址。

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

如果你拥有多台设备，你可以重复使用 localhost.pfx 文件，但你将仍需要分别创建每个设备的 IP 地址和主机名证书。

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

一旦你已安装证书，只需重新启动服务使更改生效：

```
sc stop webmanagement
sc start webmanagement
```

> [!TIP]
> IP 地址可以随时间更改。
许多网络使用 DHCP 提供的 IP 地址，以便设备不始终获取它们之前在具有相同的 IP 地址。 如果你已在设备上创建一个 IP 地址的证书，设备的地址已更改 Windows Device Portal 将生成一个新的证书，使用现有的自签名的证书，并且会停止使用你创建的一个。 这将导致证书警告页面再次显示在你的浏览器。 出于此原因，我们建议连接到你的设备通过相应的主机名，你可以设置设备门户中。 这些将保持不变无论 IP 地址。
