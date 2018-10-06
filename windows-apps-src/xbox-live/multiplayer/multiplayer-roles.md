---
title: 多人游戏角色
author: KevinAsgari
description: 了解有关使用角色定义 Xbox Live 多人游戏中的玩家角色。
ms.author: kevinasg
ms.date: 06/29/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Xbox live, xbox, 游戏, uwp, windows 10, xbox one, 多人游戏, 角色
ms.localizationpriority: medium
ms.openlocfilehash: 0ab0a8fd83e94af9a06582faebc2923eb459996b
ms.sourcegitcommit: 63cef0a7805f1594984da4d4ff2f76894f12d942
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/05/2018
ms.locfileid: "4390951"
---
# <a name="roles"></a>角色

对于某些游戏会话，你可能想要指定特定成员拥有特定游戏角色（如支持、医师、攻击等）。你可能还想为填补特定游戏角色的玩家保留游戏空位。 通过使用 Xbox Live 角色功能，此服务可跟踪哪些玩家分配到了哪些游戏角色，并强制可选择特定游戏角色的最大玩家人数。

角色的最常见用法是确定游戏会话的游戏特定角色。 例如，你可以使用满足以下条件的游戏模式：1 到 2 个支持类、至少 1 个坦克/重型类，以及不超过 5 个攻击类。

在另一种可能的方案中，你可能想指定游戏会话可以正好拥有 4 个游戏玩家、最多 8 个观众和 1 个解说员。

通过其他方法（例如会话浏览）填补剩余空位时，你还可以使用角色为好友保留空位。

## <a name="role-types"></a>角色类型

角色类型表示一组角色定义。 必须将每个角色定义为角色类型的一部分。 在多人游戏会话文档中定义角色类型。

只能向一个成员分配给定角色类型中的一种角色。 例如，如果“类”角色类型包含医师、坦克和伤害，则只能向一个成员分配给其中一种角色。

你可以定义多个角色类型，一位成员可以分配到各角色类型中的一个角色。 在之前的方案中，成员可能选择了医师角色，但可能分配到的是队长角色（如果单独角色类型中定义了队长角色）。

> **重要提示：** Xbox Live SDK 当前仅支持每个成员拥有单个角色类型和单个角色。

## <a name="role-type-properties"></a>角色类型属性

定义角色类型时，必须为角色类型指定下列信息：

* 角色类型的名称。 名称必须为小写字母数字形式，且长度不超过 100 个字符。
* 在角色类型中定义的角色是否由所有者管理。
* 是否可在会话有效期间更改在角色类型中定义的角色属性。
* 角色类型中包含的角色定义。

如果角色类型由所有者管理，这表示只有是会话所有者的成员才能将此类型的角色分配给成员。 如果角色类型并非由所有者管理，则成员可以将角色分配给自己。

你只能在拥有“hasOwners”功能集的会话上指定此角色类型由所有者管理。

> Xbox Live SDK 当前不支持所有者将角色分配给其他成员。

## <a name="role-properties"></a>角色属性

定义角色时，必须为每个角色指定下列信息：

* 角色的名称。 名称必须为小写字母数字形式，且长度不超过 100 个字符。
* 允许填补角色的最大成员数。 必须大于零。
* 应填补角色的目标成员数。 目标必须大于零，且小于或等于允许填补角色的最大成员人数。

向会话成员分配角色时，此信息会记录在多人游戏会话文档的成员角色中。

服务会强制可分配给角色的最大成员人数，但不会强制执目标人数。

## <a name="create-roles"></a>创建角色

通常在[会话模板](service-configuration/session-templates.md)中定义角色和角色类型。 服务支持会话创建期间的角色和角色类型定义，但是 Xbox Live SDK 不支持。

### <a name="define-role-types-and-roles-in-a-session-template"></a>在会话模板中定义角色类型和角色

当在 Xbox Live 配置期间创建会话模板时，你可以定义角色类型和角色。

在会话模板中，采用以下格式将角色类型和角色信息指定为基本级别“roleTypes”元素：

```json
"roleTypes": {
  "myroletype1": { // must be lowercase alpha-numeric.
    "ownerManaged": true, // Can only be true on sessions with the "hasOwners" capability set. If true, only the owner of the session can assign this role to members.
    "mutableRoleSettings": ["max", "target"], // Which role settings for roles in this role type can be modified throughout the life of the session. Exclude role settings to lock them.
    "roles": {
      "role1": { // must be lowercase alpha-numeric.
        "max": 3, // Max number of members assigned to this role at a time, enforced by MPSD.
        "target": 2 // Target number of members to assign this role to. Like max, but not enforced (can be exceeded).
      },
      "role2": {
        ...
      }
  },
  "myroletype2": {
    ...
  }
},
```

## <a name="retrieve-role-information-for-a-multiplayer-session"></a>为多人游戏会话检索角色信息

你可以从多人游戏会话、或多人游戏搜索句柄中，获取有关角色类型、角色以及分配给每个角色的成员数的信息。

在 Xbox Live SDK 中，有关角色类型和角色的信息存储在映射结构内。 C + + API 使用 `unordered_map` 类，WinRT API 使用 `IMapView` 类。

### <a name="get-the-role-information-from-a-search-handle"></a>从搜索句柄获取角色信息

在从搜索请求返回的 `multiplayer_search_handle_details` 对象中，你可以通过使用感兴趣的角色类型名称索引 `role_types` 映射来获取角色类型信息。

这会返回一个 `multiplayer_role_type` 对象。 你可以通过索引 `roles` 映射（会返回一个 `multiplayer_role_info` 对象）获取角色。

`multiplayer_role_info` 对象包含有关此角色的信息，包括 `max_members_count`、`member_xbox_user_ids`、`members_count` 和 `target_count`。

### <a name="get-the-role-information-from-a-search-handle"></a>从搜索句柄获取角色信息

从会话中获取角色信息的流程与从搜索句柄获取信息的流程类似，但是使用了一些不同的类。

在 `multiplayer_session`对象中，你可以通过引用 `session_role_types` 对象（此对象是 `multiplayer_session_role_types` 类）获取角色类型信息。 在此对象中，你可以使用感兴趣的角色类型名称索引 `role_types` 映射。

这会返回一个 `multiplayer_role_type` 对象。 你可以通过索引 `roles` 映射（会返回一个 `multiplayer_role_info` 对象）获取角色。

`multiplayer_role_info` 对象包含有关此角色的信息，包括 `max_members_count`、`member_xbox_user_ids`、`members_count` 和 `target_count`。

## <a name="change-mutable-role-settings"></a>更改可变角色设置

如果角色类型指示一些角色设置可更改（可变），你可以使用 `multiplayer_role_type.set_roles()` 方法修改可变设置。 只有标记为会话所有者的成员才能更改角色设置。

## <a name="assign-a-role-to-a-member"></a>向成员分配角色

目前，只有成员可在 Xbox Live SDK 中分配他们自己的角色。 在 `multiplayer_session` 对象中，你可以调用 `set_current_user_role_info(role_type, role_name)` 方法来指定当前成员的角色类型和角色。

如果在你尝试将会话写入服务时，此角色已满，则 MPSD 将拒绝写入。
