---
title: 在字符串与二进制数据之间转换
description: 此示例代码显示了在通用 Windows 平台 (UWP) 应用中，如何在字符串和二进制数据之间转换。
ms.assetid: AED4C74F-E63B-4980-BB4D-28ACCC1AB58B
author: msatranjr
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10，uwp 安全
ms.localizationpriority: medium
ms.openlocfilehash: b3c3a3f6f831186302fc32b1f510919da40c57cc
ms.sourcegitcommit: 7aa1933e6970f878faf50d59e1f799b90afd7cc7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2018
ms.locfileid: "3385129"
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