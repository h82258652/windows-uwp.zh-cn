---
title: 为多人游戏配置 AppXManifest
author: KevinAsgari
description: 了解如何配置 UWP AppXManifest 以实现 Xbox Live 多人游戏邀请。
ms.assetid: 72f179e7-4705-4161-9b8a-4d6a1a05b8f7
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, 协议激活, 多人游戏
ms.localizationpriority: medium
ms.openlocfilehash: 35cdb8765ba40e1a6d4a7c624b1a81f37a8f7fa0
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/17/2018
ms.locfileid: "7165579"
---
# <a name="configure-your-appxmanifest-for-multiplayer"></a>为多人游戏配置 AppXManifest

如果满足以下条件，则需要对 Visual Studio 项目中的 .appxmanifest 文件执行一些更新：
- 你正在开发 UWP
- 你希望实现玩家邀请其他用户加入你的作品的功能

如果你未执行此步骤，则在受邀玩家接受游戏邀请后，将不为你的作品激活协议。

## <a name="open-your-packageappxmanifest"></a>打开 Package.appxmanifest

你的 Package.appxmanifest 文件通常位于 Visual Studio 项目的解决方案文件所在的目录中。  你也可以在解决方案资源管理器中找到该文件。

![](../../images/multiplayer/multiplayer_open_appxmanifest.png)

## <a name="add-new-entry"></a>添加新条目

你需要将以下内容添加到 Package.appxmanifest 文件的 ```<Applications>``` 下方的 ```<Extensions>``` 元素中

```
<Extensions>
  <uap:Extension Category="windows.protocol">
    <uap:Protocol Name="ms-xbl-multiplayer" />
  </uap:Extension>
</Extensions>
```

例如：

![](../../images/multiplayer/multiplayer_appxmanifest_changes.png)

保存和重新生成你的作品。  若要了解如何使用多人游戏管理器来实现将玩家邀请到你的作品中的功能，请参阅[与好友玩多人游戏](../multiplayer-manager/play-multiplayer-with-friends.md)
