---
author: mcleblanc
ms.assetid: B48E21AB-0EA5-444B-8333-393DD8D1B76D
title: 企业共享的存储
description: 企业共享的存储为要共享数据的业务线应用定义本地数据位置。
ms.author: markl
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5236767d4c02d873106c7b1799c8428d84cccd53
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/05/2018
ms.locfileid: "6038337"
---
# <a name="enterprise-shared-storage"></a>企业共享存储

共享的存储包含两个位置，其中的应用具有受限功能 **enterpriseDeviceLockdown**，而企业证书具有完整的读写访问权限。 请注意，**enterpriseDeviceLockdown** 功能允许应用使用设备锁定 API 和访问企业共享的存储文件夹。 有关该 API 的详细信息，请参阅 [**Windows.Embedded.DeviceLockdown**](http://go.microsoft.com/fwlink/?LinkId=699331) 命名空间。  

本地驱动器上的位置设置如下所示：
- \Data\SharedData\Enterprise\Persistent
- \Data\SharedData\Enterprise\Non-Persistent

## <a name="scenarios"></a>方案

企业共享的存储为以下方案提供支持。

- 可以在同一应用的不同实例之间，甚至是假定具有相应功能和证书的应用之间，共享某个应用实例内的数据。
- 你可以将本地硬盘驱动器上的数据存储在 \Data\SharedData\Enterprise\Persistent 文件夹中，即使重置了设备，仍然会保留该数据。
- 在设备上通过移动设备管理 (MDM) 服务操作文件，包括读取、写入和删除文件。 有关如何通过 MDM 服务使用企业共享存储的详细信息，请参阅 [EnterpriseExtFileSystem CSP](http://go.microsoft.com/fwlink/?LinkId=699333)。

## <a name="access-enterprise-shared-storage"></a>访问企业共享的存储

下面的示例演示了如何在程序包清单中声明访问企业共享存储的功能，以及如何通过使用 Windows.Storage.StorageFolder 类访问共享的存储文件夹。

在应用程序包清单中，包括以下功能：

```xml
<Package
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
  IgnorableNamespaces="uap mp rescap">

…

<Capabilities>
    <rescap:Capability Name="enterpriseDeviceLockdown"/>
</Capabilities>
```

若要访问共享的数据位置，应用将使用以下代码。

```csharp
using System;
using System.Collections.Generic;
using System.Diagnostics;
using Windows.Storage;

…

// Get the Enterprise Shared Storage folder.
var enterprisePersistentFolderRoot = @"C:\Data\SharedData\Enterprise\Persistent";

StorageFolder folder =
    await StorageFolder.GetFolderFromPathAsync(enterprisePersistentFolderRoot);

// Get the files in the folder.
IReadOnlyList<StorageFile> sortedItems =
    await folder.GetFilesAsync();

// Iterate over the results and print the list of files
// to the Visual Studio Output window.
foreach (StorageFile file in sortedItems)
    Debug.WriteLine(file.Name + ", " + file.DateCreated);
```

