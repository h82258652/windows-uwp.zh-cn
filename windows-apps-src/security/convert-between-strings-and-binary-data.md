---
title: 在字符串与二进制数据之间转换
description: 此示例代码显示了在通用 Windows 平台 (UWP) 应用中，如何在字符串和二进制数据之间转换。
ms.assetid: AED4C74F-E63B-4980-BB4D-28ACCC1AB58B
author: msatranjr
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10，uwp 安全
ms.localizationpriority: medium
ms.openlocfilehash: 030fe81685fbc6caea0b9847b366384476f6298f
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2018
ms.locfileid: "6837750"
---
# <a name="convert-between-strings-and-binary-data"></a>在字符串与二进制数据之间转换



此示例代码显示了在通用 Windows 平台 (UWP) 应用中，如何在字符串和二进制数据之间转换。

```cs
public void ConvertData()
{
    // Create a string to convert.
    String strIn = "Input String";

    // Convert the string to UTF16BE binary data.
    IBuffer buffUTF16BE = CryptographicBuffer.ConvertStringToBinary(strIn, BinaryStringEncoding.Utf16BE);

    // Convert the string to UTF16LE binary data.
    IBuffer buffUTF16LE = CryptographicBuffer.ConvertStringToBinary(strIn, BinaryStringEncoding.Utf16LE);

    // Convert the string to UTF8 binary data.
    IBuffer buffUTF8 = CryptographicBuffer.ConvertStringToBinary(strIn, BinaryStringEncoding.Utf8);
}
```