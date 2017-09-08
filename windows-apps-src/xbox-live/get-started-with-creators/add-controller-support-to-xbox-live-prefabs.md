---
title: "向 Xbox Live Prefabs 添加控制器支持"
author: KevinAsgari
description: "使用 Xbox Live Unity 插件向 Xbox Live Prefabs 添加控制器支持"
ms.assetid: 
ms.author: heba
ms.date: 07-14-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "xbox live, xbox, 游戏, uwp, windows 10, xbox one, unity, 控制器支持"
ms.openlocfilehash: dbf6c5af6fc205ae33ae7b5363b37af9d2da6aaf
ms.sourcegitcommit: a9e4be98688b3a6125fd5dd126190fcfcd764f95
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/21/2017
---
# <a name="add-controller-support-to-xbox-live-prefabs"></a>向 Xbox Live Prefabs 添加控制器支持

所有 Xbox Live Unity Plugin Prefabs 均支持在检查器中指定控制器输入。

例如，假设你有一个称为 `UserProfile1`，并基于 `UserProfile` prefab 的游戏对象。 如果你想将此游戏对象绑定到玩家 1，并让他们使用其 Xbox 控制器上的 `A` 按钮登录，只需在此检查器的 `Input Controller Button` 字段中写入 `joystick 1 button 0`。

  ![UserProfile Prefab 中的控制器支持](../images/unity/controller-support-example.png)

## <a name="all-prefab-controller-input-fields"></a>所有 Prefab 控制器输入字段
### <a name="userprofile-prefab"></a>UserProfile prefab
- **输入控制器按钮：**添加并登录 Xbox Live 用户。

### <a name="social-prefab"></a>社交 prefab
- **切换筛选器控制器按钮：**切换筛选器来显示“全部”好友或“联机”好友。

### <a name="leaderboard-prefab"></a>排行榜 prefab
- **首页控制器按钮：**使玩家转到排行榜条目的第一页。
- **末页控制器按钮：**使玩家转到排行榜条目的最后一页。
- **下一页控制器按钮：**使玩家转到排行榜条目的下一页。
- **上一页控制器按钮：**使玩家转到排行榜条目的上一页。
- **刷新控制器按钮：**刷新排行榜视图。


### <a name="game-save-ui-prefab"></a>游戏保存 UI prefab
- **生成新控制器按钮：**生成一个新的整数保存数据。
- **保存数据控制器按钮：**将当前数据保存到连接存储中。
- **加载数据控制器按钮：**加载当前保存在连接存储中的数据。
- **获取信息控制器按钮：**在连接存储中检索有关保存的容器的信息。
- **删除容器控制器按钮：**从连接存储中删除保存的容器

## <a name="xbox-controller-button-mappings"></a>Xbox 控制器按钮映射

有关 Unity 中的 Xbox 控制器按钮映射，请查看此 [Unity 控制器 Wiki 页](http://wiki.unity3d.com/index.php?title=Xbox360Controller)。
