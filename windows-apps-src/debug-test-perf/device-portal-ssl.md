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
ms.sourcegitcommit: 9a17266f208ec415fc718e5254d5b4c08835150c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/28/2018
ms.locfileid: "2881670"
---
# <a name="provision-device-portal-with-a-custom-ssl-certificate"></a>使用自定义的 SSL 证书预配 Device Portal
Windows 10 创建者更新，在 Windows 设备门户添加一种设备管理员安装使用的自定义证书中的 HTTPS 通信的方法。 

虽然可以在您自己的 PC 上执行此操作，此功能具有现成的现有证书基础结构的企业版主要适用。  

例如，公司可能必须采用 https 提供服务的 intranet 网站的证书进行签名的证书颁发机构 (CA)。 此功能除此之外代表基础结构。 

## <a name="overview"></a>概述
默认情况下，设备门户生成自签名的根 CA，并使用的每个终结点正在侦听的 SSL 证书进行签名。 这包括`localhost`， `127.0.0.1`，和`::1`(IPv6 localhost)。

此外包含的设备的主机名 (例如， `https://LivingRoomPC`) 和为设备分配每个链接-本地 IP 地址 (最多两个 [IPv4，IPv6] 每个网络适配器)。 通过查看设备门户中的网络工具，还可以看到设备的链接-本地 IP 地址。 将开始`10.`或`192.`ipv4，或`fe80:`ipv6。 

在默认安装，可能会出现在浏览器中因不受信任的根 CA 证书警告。 具体而言，SSL 证书提供设备门户进行了签名根浏览器或 PC 不信任的 CA。 这可以通过创建新的受信任的根 CA 固定。

## <a name="create-a-root-ca"></a>创建根 CA

这应该仅执行如果您的公司 （或主页） 没有设置证书基础结构，并应仅一次完成。 以下 PowerShell 脚本创建根 CA 调用_WdpTestCA.cer_。 安装到本地计算机的受信任的根证书颁发机构的此文件将导致您的设备，以信任由此根 CA 签名的 SSL 证书。 您可以 （并且应该） 安装在您想要连接到 Windows 设备门户每台 PC 上的此.cer 文件。  

```PowerShell
$CN = "PickAName"
$OutputPath = "c:\temp\"

# Create root certificate authority
$FilePath = $OutputPath + "WdpTestCA.cer"
$Subject =  "CN="+$CN
$rootCA = New-SelfSignedCertificate -certstorelocation cert:\currentuser\my -Subject $Subject -HashAlgorithm "SHA512" -KeyUsage CertSign,CRLSign
$rootCAFile = Export-Certificate -Cert $rootCA -FilePath $FilePath
```

创建此后，您可以使用_WdpTestCA.cer_文件进行签名 SSL 证书。 

## <a name="create-an-ssl-certificate-with-the-root-ca"></a>使用根 CA 创建 SSL 证书

SSL 证书具有两个重要功能： 保护您的连接通过加密和验证您实际通信的浏览器栏中显示的地址 (Bing.com，192.168.1.37，等) 和不恶意第三方。

以下 PowerShell 脚本创建 SSL 证书的`localhost`终结点。 每个终结点设备门户侦听需要其自己的证书;您可以替换`$IssuedTo`与每个不同的终结点在脚本中为您的设备的参数： 主机名、 localhost 和 IP 地址。

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

如果您有多个设备，您可以重复使用的 localhost.pfx 文件，但仍需要单独创建每个设备的 IP 地址和主机名证书。

当生成.pfx 文件的绑定时，您将需要进行加载到 Windows 设备门户。 

## <a name="provision-device-portal-with-the-certifications"></a>使用认证的设置设备门户

对于每个.pfx 文件已创建的设备，您需要从提升的命令提示符处运行以下命令。

```
WebManagement.exe -SetCert <Path to .pfx file> <password for pfx> 
```

请参阅下面的示例使用：
```
WebManagement.exe -SetCert localhost.pfx PickAPassword
WebManagement.exe -SetCert --1.pfx PickAPassword
WebManagement.exe -SetCert MyLivingRoomPC.pfx PickAPassword
```

您一次安装证书，只需重新启动服务，以便所做的更改生效：

```
sc stop webmanagement
sc start webmanagement
```

> [!TIP]
> IP 地址可随时间变化。
许多网络使用 DHCP 透露 IP 地址，因此设备不总是获得它们之前具有相同的 IP 地址。 如果您已在设备上创建一个 IP 地址的证书和设备的地址已更改，Windows 设备门户将生成一个新的证书，使用现有的自签名的证书，并将停止使用您创建。 这将导致再次显示在浏览器将证书警告页。 因此，建议在连接到相应的主机名，您可以在设备门户中设置通过设备。 这些将保持不变无论 IP 地址。
