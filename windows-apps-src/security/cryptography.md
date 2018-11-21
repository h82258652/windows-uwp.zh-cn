---
title: 加密
description: 本文概述了通用 Windows 平台 (UWP) 应用可用的加密功能。 有关特定任务的详细信息，请参阅本文末尾的表。
ms.assetid: 9C213036-47FD-4AA4-99E0-84006BE63F47
author: msatranjr
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10，uwp 安全
ms.localizationpriority: medium
ms.openlocfilehash: 0caa3f63d8a92c75bdce10cdb277967dca21fafb
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2018
ms.locfileid: "7425858"
---
# <a name="cryptography"></a>加密




本文概述了通用 Windows 平台 (UWP) 应用可用的加密功能。 有关特定任务的详细信息，请参阅本文末尾的表。

## <a name="terminology"></a>术语


以下术语通常用于加密和公钥基础结构 (PKI)。

| 术语                        | 说明                                                                                                                                                                                           |
|-----------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 加密                  | 使用加密算法和密钥转换数据的过程。 转换的数据只能使用相同的算法和相同（对称）或相关（公共）密钥恢复。 |
| 解密                  | 将加密的数据返回其原始形式的过程。                                                                                                                                         |
| 纯文本                   | 最初指未加密的文本消息。 目前指任何未加密的数据。                                                                                                         |
| Ciphertext                  | 最初是指因加密而不可读的文本消息。 目前指任何加密的数据。                                                                                  |
| 哈希                     | 将可变长度数据转换为固定长度（通常较小）值的过程。 通过比较哈希，可以合理保证两个或多个数据相同。            |
| 签名                   | 数字数据的加密哈希通常用于验证数据的发送方或用于验证数据在传输过程中未被篡改。                                               |
| 算法                   | 加密数据的循序渐进过程。                                                                                                                                                         |
| 密钥                         | 用作加密算法的输入来加密和解密数据的随机或伪随机编号。                                                                                               |
| 对称密钥加密  | 加密和解密使用相同密钥的加密方法。 这也称作密钥加密。                                                                                      |
| 非对称密钥加密 | 加密和解密使用不同但算术上相关的密钥的加密方法。 这也称作公钥加密。                                                          |
| 编码                    | 为跨网络传输而对数字消息（包括证书）进行编码的过程。                                                                                                     |
| 算法提供程序          | 执行加密算法的 DLL。                                                                                                                                                      |
| 密钥存储提供程序        | 存储密钥材料的容器。 当前，密钥可以存储在软件、智能卡或受信任的平台模块 (TPM) 中。                                                                   |
| X.509 证书           | 一个数字文档，通常由证书颁发机构颁发，用于向其他相关方验证个人、系统或实体的身份。                                            |

 
## <a name="namespaces"></a>命名空间

以下命名空间可以用在应用中。

### <a name="windowssecuritycryptography"></a>Windows.Security.Cryptography

包含 CryptographicBuffer 类和静态方法，可让你：

-   在数据与字符串之间相互转换
-   在数据与字节数组之间相互转换
-   编码消息进行网络传输
-   在传输之后解码消息

### <a name="windowssecuritycryptographycertificates"></a>Windows.Security.Cryptography.Certificates

包含类、接口和枚举类型，可让你：

-   创建证书请求
-   安装证书响应
-   导入 PFX 文件形式的证书
-   指定并检索证书请求属性

### <a name="windowssecuritycryptographycore"></a>Windows.Security.Cryptography.Core

包含类和枚举类型，可让你：

-   加密和解密数据
-   对数据进行哈希操作
-   对数据进行签名和验证签名
-   创建、导入和导出密钥
-   使用非对称密钥算法提供程序
-   使用对称密钥算法提供程序
-   使用哈希算法提供程序
-   使用计算机身份验证代码 (MAC) 算法提供程序
-   使用密钥派生算法提供程序

### <a name="windowssecuritycryptographydataprotection"></a>Windows.Security.Cryptography.DataProtection

包含类，可让你：

-   异步加密和解密静态数据
-   异步加密和解密数据流

## <a name="crypto-and-pki-application-capabilities"></a>加密和 PKI 应用程序功能


利用为应用提供的简化应用程序编程接口，可以实现以下加密和公钥基础结构 (PKI) 功能。

### <a name="cryptography-support"></a>加密支持

你可以执行以下加密任务。 有关详细信息，请参阅 [**Windows.Security.Cryptography.Core**](https://msdn.microsoft.com/library/windows/apps/br241547) 命名空间。

-   创建对称密钥
-   执行对称加密
-   创建非对称密钥
-   执行非对称加密
-   派生基于密码的密钥
-   创建消息验证代码 (MAC)
-   哈希内容
-   数字签名内容

SDK 同时也为基于密码的数据保护提供简化的界面。 可使用此界面执行以下任务。 有关详细信息，请参阅 [**Windows.Security.Cryptography.DataProtection**](https://msdn.microsoft.com/library/windows/apps/br241585) 命名空间。

-   静态数据的异步保护
-   数据流的异步保护

### <a name="encoding-support"></a>编码支持

应用可以编码加密数据以在网络上传输，也可以解码从网络资源收到的数据。 有关详细信息，请参阅 [**Windows.Security.Cryptography**](https://msdn.microsoft.com/library/windows/apps/br241404) 命名空间中可用的静态方法。

### <a name="pki-support"></a>PKI 支持

应用可以执行以下 PKI 任务。 有关详细信息，请参阅 [**Windows.Security.Cryptography.Certificates**](https://msdn.microsoft.com/library/windows/apps/br241476) 命名空间。

-   创建证书
-   创建自签名证书
-   安装证书响应
-   导入 PFX 格式的证书
-   使用智能卡证书和密钥（sharedUserCertificates 功能集）
-   使用来自用户 MY 存储中的证书（sharedUserCertificates 功能集）

此外，还可以使用清单执行以下操作：

-   指定每个应用程序信任的根证书
-   指定每个对等应用程序信任的证书
-   明确禁用从系统信任继承
-   指定证书选择标准
    -   仅硬件证书
    -   与一组指定的颁发者关联的证书
    -   从应用程序存储中自动选择证书

## <a name="detailed-articles"></a>详细文章


以下文章提供有关安全方案的更多详细信息：

| 主题                                                                         | 说明                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
|-------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [证书](certificates.md)                                               | 本文将讨论如何在 UWP 应用中使用证书。 在公钥加密中使用数字证书将公钥绑定到个人、计算机或组织。 绑定身份主要用于针对一个实体来验证另一个。 例如，证书通常用来为用户验证 Web 服务器和为 Web 服务器验证用户。 你可以创建证书请求并安装或导入已颁发的证书。 还可以按照证书层次结构注册证书。 |
| [加密密钥](cryptographic-keys.md)                                   | 本文显示了如何使用标准密钥派生函数来派生密钥以及如何使用对称密钥和非对称密钥来加密内容。                                                                                                                                                                                                                                                                                                                                                                             |
| [数据保护](data-protection.md)                                         | 本文介绍了如何在 UWP 应用中使用 [Windows.Security.Cryptography.DataProtection](https://msdn.microsoft.com/library/windows/apps/br241585) 命名空间中的 [DataProtectionProvider](https://msdn.microsoft.com/library/windows/apps/br241559) 类加密和解密数字数据。                                                                                                                                                                                                                  |
| [MAC、哈希以及签名](macs-hashes-and-signatures.md)               | 本文讨论如何在 UWP 应用中使用消息验证代码 (MAC)、哈希以及签名来检测消息篡改。                                                                                                                                                                                                                                                                                                                                                                                |
| [有关加密的导出限制](export-restrictions-on-cryptography.md) | 使用此信息可以确定应用使用加密的方式是否可能会阻止它被列在 Microsoft Store 中。                                                                                                                                                                                                                                                                                                                                                                                            |
| [常见的加密任务](common-cryptography-tasks.md)                     | 这些文章提供常见的 UWP 加密任务的示例代码，这些任务包括创建随机数、比较缓冲区、在字符串和二进制数据之间转换、复制到字节数组和从字节数组复制，以及编码和解码数据等。                                                                                                                                                                                                                                                                                    |

 