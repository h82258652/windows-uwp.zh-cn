---
title: 创建随机数
description: 此示例代码显示了如何在通用 Windows 平台 (UWP) 应用中创建随机数或在加密中使用的缓冲区。
ms.assetid: 15746824-F93A-4DC7-836E-EBA916D2CFD3
author: awkoren
---

# 创建随机数


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

此示例代码显示了如何在通用 Windows 平台 (UWP) 应用中创建随机数或在加密中使用的缓冲区。

```cs
public string GenerateRandomData()
{
    // Define the length, in bytes, of the buffer.
    uint length = 32;

    // Generate random data and copy it to a buffer.
    IBuffer buffer = CryptographicBuffer.GenerateRandom(length);

    // Encode the buffer to a hexadecimal string (for display).
    string randomHex = CryptographicBuffer.EncodeToHexString(buffer);

    return randomHex;
}

public uint GenerateRandomNumber()
{
    // Generate a random number.
    uint random = CryptographicBuffer.GenerateRandomNumber();
    return random;
}
```

<!--HONumber=May16_HO2-->


