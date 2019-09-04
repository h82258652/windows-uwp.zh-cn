---
title: 相机条形码扫描仪配置
description: 相机条形码扫描仪启用或禁用
ms.date: 04/08/2019
ms.topic: article
keywords: windows 10, uwp, 服务点, pos
ms.localizationpriority: medium
ms.openlocfilehash: 8394f79e9581101d6def0f1568cb000ffdee5987
ms.sourcegitcommit: 0dec04de501a3db6b22dfd4a320fc09b5c4a21b5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/04/2019
ms.locfileid: "70243317"
---
# <a name="enable-or-disable-the-software-decoder-that-ships-with-windows"></a>启用或禁用 Windows 附带的软件解码器

在 Windows 10 版本 1803 中，软件解码器默认安装和启用。  如果你不想使用相机条形码扫描仪，或者如果你已获取了使用 Windows.Devices.PointOfService.BarcodeScanner API 的第三方解码器且不希望两个同时使用，则可以禁用 Windows 附带的软件解码器。

## <a name="enable-or-disable-using-the-system-registry"></a>使用系统注册表启用或禁用

Windows 附带的软件解码器可以通过系统注册表启用或禁用：在 *HKLM\Software\Microsoft\PointOfService\BarcodeScanner* 下添加注册表项 *InboxDecoder*，并对 *Enable* 值进行如下设置。

| 值名称  | 值类型 | ReplTest1 | 状态 |
| ----------- | --------- | -------|--------|
| 启用      | DWORD     | 1（默认值）<br/>0 |  启用 Windows 附带的软件解码器 <br/> 禁用 Windows 附带的软件解码器 |

下面是你可以用来**禁用** Windows 附带的软件解码器的示例注册表文件：

```text
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\PointOfService\BarcodeScanner\InboxDecoder]
"Enable"=dword:0000000
```  

使用此示例注册表文件来**启用** Windows 附带的软件解码器：

```text
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\PointOfService\BarcodeScanner\InboxDecoder]
"Enable"=dword:0000001
```  

> [!Warning]
> 如果注册表修改不正确，可能会发生严重问题。  为了增加保护，请在修改注册表前对其进行备份。  那么，如果发生问题，你也可以恢复注册表。  有关如何备份和恢复注册表的详细信息，请单击下面的文章编号以参阅 Microsoft 知识库中的文章： <br/><br/> [322756](https://support.microsoft.com/help/322756/how-to-back-up-and-restore-the-registry-in-windows) 如何备份和恢复 Windows 中的注册表。

> [!NOTE]
> 内置于 Windows 10 的软件解码器提供 [**Digimarc Corporation**](https://www.digimarc.com/) 的成果。

## <a name="see-also"></a>请参阅

### <a name="samples"></a>示例

- [条形码扫描器示例](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
