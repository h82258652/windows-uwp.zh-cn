---
author: TerryWarwick
title: 相机条形码扫描仪配置
description: 相机条形码扫描仪启用或禁用
ms.author: jken
ms.date: 05/1/2018
ms.topic: article
keywords: windows 10, uwp, 服务点, pos
ms.localizationpriority: medium
ms.openlocfilehash: 35666f64c88ad56b8f5bd3052ebbee069ccaecfc
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2018
ms.locfileid: "6658806"
---
# <a name="enable-or-disable-the-software-decoder-that-ships-with-windows"></a>启用或禁用 Windows 附带的软件解码器
在 Windows 10 版本 1803 中，软件解码器默认安装和启用。  如果你不想使用相机条形码扫描仪，或者如果你已获取了使用 Windows.Devices.PointOfService.BarcodeScanner API 的第三方解码器且不希望两个同时使用，则可以禁用 Windows 附带的软件解码器。

## <a name="enable-or-disable-using-the-system-registry"></a>使用系统注册表启用或禁用
Windows 附带的软件解码器可以通过系统注册表启用或禁用：在 *HKLM\Software\Microsoft\PointOfService\BarcodeScanner* 下添加注册表项 *InboxDecoder*，并对 *Enable* 值进行如下设置。

| 值名称  | 值类型 | 值 | 状态 |
| ----------- | --------- | -------|--------|
| Enable      | DWORD     | 1（默认值）<br/>0 |  启用 Windows 附带的软件解码器 <br/> 禁用 Windows 附带的软件解码器 |


下面是你可以用来**禁用** Windows 附带的软件解码器的示例注册表文件：

```
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\PointOfService\BarcodeScanner\InboxDecoder]
"Enable"=dword:0000000


```  
    
使用此示例注册表文件来**启用** Windows 附带的软件解码器：

```
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\PointOfService\BarcodeScanner\InboxDecoder]
"Enable"=dword:0000001


```  

> [!Warning] 
> 如果注册表修改不正确，可能会发生严重问题。  为了增加保护，请在修改注册表前对其进行备份。  那么，如果发生问题，你也可以恢复注册表。  有关如何备份和恢复注册表的详细信息，请单击下面的文章编号以参阅 Microsoft 知识库中的文章： <br/><br/> [322756](http://support.microsoft.com/kb/322756) 如何备份和恢复 Windows 中的注册表。

> [!NOTE]
> 内置于 Windows 10 的软件解码器提供 [**Digimarc Corporation**](https://www.digimarc.com/) 的成果。
