---
title: 加密密钥
description: 本文显示了如何使用标准密钥派生函数来派生密钥以及如何使用对称密钥和非对称密钥来加密内容。
ms.assetid: F35BEBDF-28C5-4F91-A94E-F7D862B6ED59
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10，uwp 安全
ms.localizationpriority: medium
ms.openlocfilehash: 2b74eccd5f6138e5a9d670aa3a0a93239813cf4d
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/27/2018
ms.locfileid: "7714616"
---
# <a name="cryptographic-keys"></a>加密密钥




本文显示了如何使用标准密钥派生函数来派生密钥以及如何使用对称密钥和非对称密钥来加密内容。 

## <a name="symmetric-keys"></a>对称密钥


对称密钥加密又称为密钥加密，它要求用来解密的密钥与用来加密的密钥相同。 可使用 [**SymmetricKeyAlgorithmProvider**](https://msdn.microsoft.com/library/windows/apps/br241537) 类指定对称算法、创建密钥或导入密钥。 可将 [**CryptographicEngine**](https://msdn.microsoft.com/library/windows/apps/br241490) 类上的静态方法与算法和密钥结合使用来加密和解密数据。

对称密钥加密一般使用块加密和块加密模式。 块加密是一种对固定大小块操作的对称加密功能。 如果要加密的消息大于块长度，则必须使用块加密模式。 块加密模式是一种通过使用块加密构建的对称加密功能。 它作为一系列固定大小块加密纯文本。 应用支持以下模式：

-   ECB（电子密码本）模式单独加密每个消息块。 这不是安全的加密模式。
-   CBC（加密块链） 模式使用以前的加密文本块来混淆当前的块。 你必须确定第一个块使用哪个值。 此值称为初始化矢量 (IV)。
-   CCM（使用 CBC-MAC 的计数器） 模式将 CBC 块加密模式与消息验证代码 (MAC) 合并在一起。
-   GCM（Galois 计数器模式） 模式将计数器加密模式与 Galois 验证模式合并在一起。

有些模式（如 CBC）要求对第一个 ciphertext 块使用初始化矢量 (IV)。 下面是常用的初始化矢量。 在调用 [**CryptographicEngine.Encrypt**](https://msdn.microsoft.com/library/windows/apps/br241494) 时指定 IV。 大多数情况下，IV 不与相同密钥重复使用，这一点很重要。

-   固定 对要加密的所有消息使用相同的 IV。 这将泄漏信息，不建议使用此模式。
-   计数器 对每个块增量使用 IV。
-   随机可创建一个伪随机 IV。 可使用 [**CryptographicBuffer.GenerateRandom**](https://msdn.microsoft.com/library/windows/apps/br241392) 创建 IV。
-   Nonce 生成对要加密的每个消息使用唯一编号。 通常，nonce 是一个修改的消息或事务标识符。 nonce 不必保密，但不得在同一密钥下重用。

大多数模式都要求纯文本的长度是块大小的整数倍。 这通常要求填充纯文本来获取相应长度。

尽管分组加密对固定大小的数据块进行加密，但是流加密是结合纯文本位和伪随机位流（称为密钥流）来生成已加密文本的对称加密功能。 一些分组加密模式（如输出反馈模式 (OTF) 和计数器模式 (CTR)）将分组加密有效地转换为流加密。 但是，实际的流加密（如 RC4）的操作速度比分组加密模式能够获取的速度高。

以下示例说明如何使用 [**SymmetricKeyAlgorithmProvider**](https://msdn.microsoft.com/library/windows/apps/br241537) 类创建对称密钥并使用其加密和解密数据。

## <a name="asymmetric-keys"></a>不对称密钥


不对称密钥加密也称为公钥加密，使用公钥和私钥执行加密和解密。 这两种密钥虽然不同，但在算术上相关。 通常，私钥秘密保存并用于解密数据，而公钥会分发到感兴趣的各方并用于加密数据。 不对称加密也用于对数据进行签名。

因为非对称加密比对称加密慢得多，所以非对称加密很少用于直接加密大量数据。 非对称加密通常用于按以下方式加密密钥。

-   Alice 要求 Bob 仅向她发送已加密的邮件。
-   Alice 创建了一个私钥/公钥对，将其私钥保密，并发布了其公钥。
-   Bob 有一封要发给 Alice 的邮件。
-   Bob 创建了一个对称密钥。
-   Bob 使用其新对称密钥来加密他要发给 Alice 的邮件。
-   Bob 使用 Alice 的公钥来加密其对称密钥。
-   Bob 将已加密的邮件和已加密的对称密钥发送给 Alice（已包封）。
-   Alice 使用其私钥（来自私钥/公钥对）来解密 Bob 的对称密钥。
-   Alice 使用 Bob 的对称密钥来解密该邮件。

可使用 [**AsymmetricKeyAlgorithmProvider**](https://msdn.microsoft.com/library/windows/apps/br241478) 对象指定不对称算法或签名算法、创建或导入短暂密钥对，或者导入密钥对的公钥部分。

## <a name="deriving-keys"></a>派生密钥


通常有必要从共享机密派生附加密钥。 你可以使用 [**KeyDerivationAlgorithmProvider**](https://msdn.microsoft.com/library/windows/apps/br241518) 类以及 [**KeyDerivationParameters**](https://msdn.microsoft.com/library/windows/apps/br241524) 类中的以下特殊方法之一来派生密钥。

| 对象                                                                            | 说明                                                                                                                                |
|-----------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------|
| [**BuildForPbkdf2**](https://msdn.microsoft.com/library/windows/apps/br241525)    | 创建一个 KeyDerivationParameters 对象以在基于密码的密钥派生函数 2 (PBKDF2) 中使用。                                 |
| [**BuildForSP800108**](https://msdn.microsoft.com/library/windows/apps/br241526)  | 创建一个 KeyDerivationParameters 对象以在某个计数器模式的、基于哈希的邮件身份验证代码 (HMAC) 密钥派生函数中使用。 |
| [**BuildForSP80056a**](https://msdn.microsoft.com/library/windows/apps/br241527)  | 创建一个 KeyDerivationParameters 对象以在 SP800-56A 密钥派生函数中使用。                                                 |

 
