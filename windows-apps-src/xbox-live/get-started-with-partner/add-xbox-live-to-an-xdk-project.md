---
title: 向 XDK 项目添加 Xbox Live
author: KevinAsgari
description: 了解如何向新的或现有的 Xbox 开发人员工具包 (XDK) 项目添加 Xbox Live。
ms.assetid: fc6f987c-1a87-4ff5-b063-891591aa6653
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, xdk
ms.localizationpriority: medium
ms.openlocfilehash: 5563966c6f877bf02b5e58751173e6c425b25bfa
ms.sourcegitcommit: 5c9a47b135c5f587214675e39c1ac058c0380f4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/04/2018
ms.locfileid: "4357583"
---
# <a name="add-xbox-live-to-a-new-or-existing-xdk-project"></a>向新的或现有的 XDK 项目添加 Xbox Live

此主题概述了如何向新的或现有的 XDK 项目添加 Xbox Live。

过程如下：

- 设置你的 Xbox One 开发环境
- 获取你的 ID
- 配置你的开发主机
- 向二进制文件添加 TitleID 和 SCID


## <a name="setup-up-your-xbox-one-development-environment"></a>设置你的 Xbox One 开发环境
首先，通过遵循 XDK 文档中的“设置你的 Xbox One 开发环境”部分，设置主机。

## <a name="get-your-ids"></a>获取你的 ID

若要启用 Xbox Live 服务，你需要获取多个 ID，以配置你的开发工具包和主题作品。 可以使用相同的过程完成这些操作。

你将需要通过执行 [Xbox Live 服务配置](../xbox-live-service-configuration.md)中的操作来获取你的 ID

## <a name="configure-your-development-console"></a>配置你的开发主机

拥有 ID 后，请遵循[配置你的开发主机](configure-your-development-console.md)指南来设置你的开发主机。

## <a name="add-the-titleid-and-scid-to-your-binary"></a>向二进制文件添加 TitleID 和 SCID
虽然在平台级别上为每个开发工具包配置了沙盒，但 TitleID 和 SCID 绑定了特定的二进制文件。 要向二进制文件添加 TitleID 和 SCID，请通过在 <Extensions> 节点中添加新节点来修改此二进制文件的 Package.appxmanifest，如下所示：

```
<Applications>
    ...
    <Application ...>
      ...
      <Extensions>
        <mx:Extension Category="xbox.live">
           <mx:XboxLive TitleId="<your titleID>" PrimaryServiceConfigId="<your SCID>" RequireXboxLive="<boolean indicating Live requirement>" />
        </mx:Extension>
      </Extensions>
   </Application>
</Applications>
```

有关 AppxManifest.xml 文件的详细信息，请参阅“Visual Studio 中用于 Xbox One 开发的项目模板”。

查看应用程序清单方案获取应用程序清单方案的介绍。

**RequireXboxLive 标志** 如果将 RequireXboxLive 标志设置为 true，则主题作品不会启动，除非 Windows.Networking.Connectivity 连接级别以 XboxLiveAccess 的形式返回，且主题作品使用 Xbox Live 清除身份验证。 这可以确保主题作品采用了最新的内容更新。 如果在主题作品运行时连接丢失，则会暂停主题作品。

只有“需要 Internet”主题作品应将 RequireXboxLive 标记为 true，并注意，用这种方式标记你的主题作品无法保证主题作品的所需服务启动并运行。
