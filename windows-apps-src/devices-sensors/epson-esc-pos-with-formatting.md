---
author: PatrickFarley
ms.assetid: 70667353-152B-4B18-92C1-0178298052D4
title: "Epson ESC/POS（可进行格式设置）"
description: "了解如何使用 ESC/POS 命令语言来为服务点打印机设置文本的格式（如粗体和双倍大小字符）。"
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: f482f9f44a5f4acb9c5213745da3b8edd3613b62
ms.sourcegitcommit: d2ec178103f49b198da2ee486f1681e38dcc8e7b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/28/2017
---
# <a name="epson-escpos-with-formatting"></a>Epson ESC/POS（可进行格式设置）

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 的文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**重要的 API**

-   [**PointofService 打印机**](https://msdn.microsoft.com/library/windows/apps/Mt426652)
-   [**Windows.Devices.PointOfService**](https://msdn.microsoft.com/library/windows/apps/Dn298071)

了解如何使用 ESC/POS 命令语言来为服务点打印机设置文本的格式（如粗体和双倍大小字符）。

## <a name="escpos-usage"></a>ESC/POS 用法

Windows 服务点提供使用各种打印机，包括多个 Epson TM 系列打印机（有关受支持打印机的完整列表，请参阅 [PointofService 打印机](https://msdn.microsoft.com/library/windows/apps/Mt426652)页面）。 Windows 支持使用 ESC/POS 打印机控制语言的打印，该语言为与你的打印机通信提供高效且功能丰富的命令。

ESC/POS 是由 Epson 创建的命令系统，适用于各种 POS 打印机系统，旨在通过提供通用的适用性避免不兼容的命令集。 大多数现代打印机都支持 ESC/POS。

所有命令以 ESC 字符（ASCII 27，十六进制 1B）或 GS（ASCII 29，十六进制 1D）开头，后跟指定命令的其他字符。 正常文本仅发送到打印机，以换行符分隔。

[**Windows PointOfService API**](https://msdn.microsoft.com/library/windows/apps/Dn298071) 通过 **Print()** 或 **PrintLine()** 方法为你提供大部分功能。 但是，若要获取特定格式或发送特定命令，则必须使用 ESC/POS 命令、生成为字符串并发送到打印机。

## <a name="example-using-bold-and-double-size-characters"></a>使用粗体和双倍大小字符的示例

以下示例介绍如何使用 ESC/POS 命令以粗体和双倍大小字符进行打印。 请注意，每个命令都会生成为字符串，然后插入到 printJob 调用中。

```csharp
// … prior plumbing code removed for brevity
// this code assumed you've already created a receipt print job (printJob)
// and also that you've already checked the PosPrinter Capabilities to
// verify that the printer supports Bold and DoubleHighDoubleWide print modes

const string ESC = "\u001B";
const string GS = "\u001D";
const string InitializePrinter = ESC + "@";
const string BoldOn = ESC + "E" + "\u0001";
const string BoldOff = ESC + "E" + "\0";
const string DoubleOn = GS + "!" + "\u0011";  // 2x sized text (double-high + double-wide)
const string DoubleOff = GS + "!" + "\0";

printJob.Print(InitializePrinter);
printJob.PrintLine("Here is some normal text.");
printJob.PrintLine(BoldOn + "Here is some bold text." + BoldOff);
printJob.PrintLine(DoubleOn + "Here is some large text." + DoubleOff);

printJob.ExecuteAsync();
```

有关 ESC/POS（包括可用命令）的详细信息，请查看 [Epson ESC/POS 常见问题](http://content.epson.de/fileadmin/content/files/RSD/downloads/escpos.pdf)。 有关 [**Windows.Devices.PointOfService**](https://msdn.microsoft.com/library/windows/apps/Dn298071) 和所有可用功能的详细信息，请参阅 MSDN 上的 [PointofService 打印机](https://msdn.microsoft.com/library/windows/apps/Mt426652)。
