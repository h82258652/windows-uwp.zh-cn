---
title: MAC、哈希以及签名
description: 本文讨论了如何在通用 Windows 平台 (UWP) 应用中使用消息验证代码 (MAC)、哈希和签名检测消息篡改。
ms.assetid: E674312F-6678-44C5-91D9-B489F49C4D3C
author: msatranjr
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10，uwp 安全
ms.localizationpriority: medium
ms.openlocfilehash: e7b345e520b848a3637a44fa3c3b26172c7afef0
ms.sourcegitcommit: 106aec1e59ba41aae2ac00f909b81bf7121a6ef1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/15/2018
ms.locfileid: "4614091"
---
# <a name="macs-hashes-and-signatures"></a>MAC、哈希以及签名




本文讨论了如何在通用 Windows 平台 (UWP) 应用中使用消息验证代码 (MAC)、哈希和签名检测消息篡改。

## <a name="message-authentication-codes-macs"></a>消息验证代码 (MAC)


加密有助于防止未经授权的个人读取消息，但不能防止个人篡改消息。 更改的消息（即使该更改不会导致任何情况发生，只是一种无聊的行为）会产生实际成本。 消息验证代码 (MAC) 有助于防止消息篡改。 例如，考虑以下情景：

-   鲍勃和艾丽斯共享密钥并就要使用的 MAC 功能达成一致意见。
-   Bob 创建了消息并将该消息和密钥输入至 MAC 功能以检索 MAC 值。
-   Bob 将 \[unencrypted\] 消息和 MAC 值通过网络发送给 Alice。
-   Alice 使用密钥和消息作为 MAC 功能的输入。 她将生成的 MAC 值与鲍勃发送的 MAC 值进行比较。 如果二者相同，则消息在传输中未进行更改。

注意，偷听鲍勃和艾丽斯之间对话的第三方伊夫无法有效操纵消息。 伊夫没有私钥访问权限，因此，无法创建 MAC 值，该值看上去可让艾丽斯感觉出现的篡改消息合法。

创建消息验证代码仅确保原始消息未更改，并且通过使用共享密钥，消息哈希由具有该私钥访问权限的某人进行签名。

可以使用 [**MacAlgorithmProvider**](https://msdn.microsoft.com/library/windows/apps/br241530) 枚举可用的 MAC 算法并生成对称密钥。 可以使用 [**CryptographicEngine**](https://msdn.microsoft.com/library/windows/apps/br241490) 类上的静态方法来执行创建 MAC 值的必要加密。

数字签名为等同于私钥消息验证代码 (MAC) 的公钥。 尽管 MAC 使用私钥来使消息接收人验证传输过程中尚未更改消息，但是签名使用私钥/公钥对。

此示例代码说明如何使用 [**MacAlgorithmProvider**](https://msdn.microsoft.com/library/windows/apps/br241530) 类创建经过哈希的邮件身份验证代码 (HMAC)。

```cs
using Windows.Security.Cryptography;
using Windows.Security.Cryptography.Core;
using Windows.Storage.Streams;

namespace SampleMacAlgorithmProvider
{
    sealed partial class MacAlgProviderApp : Application
    {
        public MacAlgProviderApp()
        {
            // Initialize the application.
            this.InitializeComponent();

            // Initialize the hashing process.
            String strMsg = "This is a message to be authenticated";
            String strAlgName = MacAlgorithmNames.HmacSha384;
            IBuffer buffMsg;
            CryptographicKey hmacKey;
            IBuffer buffHMAC;

            // Create a hashed message authentication code (HMAC)
            this.CreateHMAC(
                strMsg,
                strAlgName,
                out buffMsg,
                out hmacKey,
                out buffHMAC);

            // Verify the HMAC.
            this.VerifyHMAC(
                buffMsg,
                hmacKey,
                buffHMAC);
        }

        void CreateHMAC(
            String strMsg,
            String strAlgName,
            out IBuffer buffMsg,
            out CryptographicKey hmacKey,
            out IBuffer buffHMAC)
        {
            // Create a MacAlgorithmProvider object for the specified algorithm.
            MacAlgorithmProvider objMacProv = MacAlgorithmProvider.OpenAlgorithm(strAlgName);

            // Demonstrate how to retrieve the name of the algorithm used.
            String strNameUsed = objMacProv.AlgorithmName;

            // Create a buffer that contains the message to be signed.
            BinaryStringEncoding encoding = BinaryStringEncoding.Utf8;
            buffMsg = CryptographicBuffer.ConvertStringToBinary(strMsg, encoding);

            // Create a key to be signed with the message.
            IBuffer buffKeyMaterial = CryptographicBuffer.GenerateRandom(objMacProv.MacLength);
            hmacKey = objMacProv.CreateKey(buffKeyMaterial);

            // Sign the key and message together.
            buffHMAC = CryptographicEngine.Sign(hmacKey, buffMsg);

            // Verify that the HMAC length is correct for the selected algorithm
            if (buffHMAC.Length != objMacProv.MacLength)
            {
                throw new Exception("Error computing digest");
            }
         }

        public void VerifyHMAC(
            IBuffer buffMsg,
            CryptographicKey hmacKey,
            IBuffer buffHMAC)
        {
            // The input key must be securely shared between the sender of the HMAC and 
            // the recipient. The recipient uses the CryptographicEngine.VerifySignature() 
            // method as follows to verify that the message has not been altered in transit.
            Boolean IsAuthenticated = CryptographicEngine.VerifySignature(hmacKey, buffMsg, buffHMAC);
            if (!IsAuthenticated)
            {
                throw new Exception("The message cannot be verified.");
            }
        }
    }
}
```

## <a name="hashes"></a>哈希


加密哈希函数获取任意长度的数据块并返回固定大小的位字符串。 哈希函数通常在对数据进行签名时使用。 由于大多数公钥签名操作是计算密集型的操作，因此对消息哈希进行签名（加密）比对原始消息进行签名通常更高效。 以下过程显示了一个常用的简化方案：

-   Bob 和 Alice 共享密钥并就要使用的 MAC 功能达成一致意见。
-   Bob 创建了消息并将该消息和密钥输入至 MAC 功能以检索 MAC 值。
-   Bob 将 \[unencrypted\] 消息和 MAC 值通过网络发送给 Alice。
-   Alice 使用密钥和消息作为 MAC 功能的输入。 她将生成的 MAC 值与鲍勃发送的 MAC 值进行比较。 如果二者相同，则消息在传输中未进行更改。

请注意，Alice 发送了一个未加密的消息。 只对哈希进行加密。 该过程只能确保原始消息未经修改，以及消息哈希是由对 Alice 的私钥（推测是 Alice）具有访问权限的人进行的签名，后者是通过使用通过使用 Alice 的公钥来实现的。

你可以使用 [**HashAlgorithmProvider**](https://msdn.microsoft.com/library/windows/apps/br241511) 类枚举可用的哈希算法并创建一个 [**CryptographicHash**](https://msdn.microsoft.com/library/windows/apps/br241498) 值。

数字签名为等同于私钥消息验证代码 (MAC) 的公钥。 但是 MAC 使用私钥使消息接受者能够验证消息在传输期间是否尚未改变，签名使用私钥/公钥对。

[**CryptographicHash**](https://msdn.microsoft.com/library/windows/apps/br241498) 对象可用于重复哈希不同的数据，不必在每次使用时重新创建该对象。 [**Append**](https://msdn.microsoft.com/library/windows/apps/br241499) 方法将新数据添加到要进行哈希操作的缓冲区。 [**GetValueAndReset**](https://msdn.microsoft.com/library/windows/apps/hh701376) 方法对数据进行哈希操作并重置对象以用于另一个用途。 下面的示例对此进行了展示。

```cs
public void SampleReusableHash()
{
    // Create a string that contains the name of the hashing algorithm to use.
    String strAlgName = HashAlgorithmNames.Sha512;

    // Create a HashAlgorithmProvider object.
    HashAlgorithmProvider objAlgProv = HashAlgorithmProvider.OpenAlgorithm(strAlgName);

    // Create a CryptographicHash object. This object can be reused to continually
    // hash new messages.
    CryptographicHash objHash = objAlgProv.CreateHash();

    // Hash message 1.
    String strMsg1 = "This is message 1.";
    IBuffer buffMsg1 = CryptographicBuffer.ConvertStringToBinary(strMsg1, BinaryStringEncoding.Utf16BE);
    objHash.Append(buffMsg1);
    IBuffer buffHash1 = objHash.GetValueAndReset();

    // Hash message 2.
    String strMsg2 = "This is message 2.";
    IBuffer buffMsg2 = CryptographicBuffer.ConvertStringToBinary(strMsg2, BinaryStringEncoding.Utf16BE);
    objHash.Append(buffMsg2);
    IBuffer buffHash2 = objHash.GetValueAndReset();

    // Hash message 3.
    String strMsg3 = "This is message 3.";
    IBuffer buffMsg3 = CryptographicBuffer.ConvertStringToBinary(strMsg3, BinaryStringEncoding.Utf16BE);
    objHash.Append(buffMsg3);
    IBuffer buffHash3 = objHash.GetValueAndReset();

    // Convert the hashes to string values (for display);
    String strHash1 = CryptographicBuffer.EncodeToBase64String(buffHash1);
    String strHash2 = CryptographicBuffer.EncodeToBase64String(buffHash2);
    String strHash3 = CryptographicBuffer.EncodeToBase64String(buffHash3);
}

```

## <a name="digital-signatures"></a>数字签名


数字签名为等同于私钥消息验证代码 (MAC) 的公钥。 但是 MAC 使用私钥使消息接受者能够验证消息在传输期间是否尚未改变，签名使用私钥/公钥对。

由于大多数公钥签名操作是计算密集型的操作，因此对消息哈希进行签名（加密）比对原始消息进行签名通常更高效。 发送者创建消息哈希，并且发送签名和（未加密）消息。 接收者计算该消息上的哈希，解密签名并将解密的签名计算为哈希值。 如果两者匹配，则接收者可以更加清楚地确定该消息确实来自发送者并且传输期间并未改变。

签名通过发送者的公钥只能确保原始消息未被改变以及消息哈希是由对私钥有访问权限的某个人进行的签名。

你可以使用 [**AsymmetricKeyAlgorithmProvider**](https://msdn.microsoft.com/library/windows/apps/br241478) 对象枚举可用的签名算法以及生成或导入密钥对。 你可以在 [**CryptographicHash**](https://msdn.microsoft.com/library/windows/apps/br241498) 类上使用静态方法对消息进行签名或验证签名。