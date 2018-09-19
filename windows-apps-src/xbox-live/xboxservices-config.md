---
title: XboxServices.config
author: KevinAsgari
description: 描述用于将 UWP 游戏与 Xbox Live 配置相关联的 XboxServices.config 文件。
ms.author: kevinasg
ms.date: 03/29/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Xbox live, xbox, 游戏, uwp, windows 10, xbox one, 服务配置, xboxservices.config
ms.localizationpriority: medium
ms.openlocfilehash: db4e1dca1bf3968dc62b2ba60eac1033ad759663
ms.sourcegitcommit: 68fcac3288d5698a13dbcbd57f51b30592f24860
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/19/2018
ms.locfileid: "4060452"
---
# <a name="xboxservicesconfig-file-description"></a>XboxServices.config 文件描述

在开发启用 Xbox Live 的 UWP 游戏时，项目中必须包括 XboxServices.config 文件。  通过此文件，Xbox Live SDK 能够将你的游戏与开发人员中心应用和 Xbox Live 服务配置相关联。 此文件包含一个 JSON 对象，详细介绍服务配置 ID、作品 ID 等信息。

如果你要使用 Unity，利用 Xbox Live 插件来设计 Xbox Live 创意者计划游戏，Xbox Live 关联向导会自动创建该文件。

## <a name="xboxservicesconfig-fields"></a>XboxServices.config 字段

>[!NOTE]
> Xbox Live 关联向导创建的文件可能包含下述字段以外的其他字段，但服务不会使用这些字段。

配置文件的 JSON 对象中定义了以下字段：

字段 | 描述
--- | ---
PrimaryServiceConfigId  |  Xbox Live 服务配置 ID (SCID)。 在[开发人员中心仪表板](https://developer.microsoft.com/en-us/dashboard)上，可以在 **Xbox Live** 页面（对于创意者计划）或 **Xbox Live 设置**页面（对于所有 Xbox Live 游戏）中与你的应用相对应的**服务**部分下面找到此值。
TitleId  |  应用的十进制作品 ID。 在[开发人员中心仪表板](https://developer.microsoft.com/en-us/dashboard)上，可以在 **Xbox Live** 页面（对于创意者计划）或 **Xbox Live 设置**页面（对于所有 Xbox Live 游戏）中与你的应用相对应的**服务**部分下面找到此值。
XboxLiveCreatorsTitle  |  如果为“true”，则指示应用是 Xbox Live 创意者计划应用。 否则为“false”。
范围  |  **（可选）** 定义应用所用功能的范围。 有关详细描述，请见下文。

### <a name="scope-field"></a>“范围”字段

**范围**字段是可选字段，可用于指示游戏使用的功能。


如果未指定**范围**字段，则会将范围设置为默认值，该默认值取决于 **XboxLiveCreatorsTitle** 字段的值，如下表中所述：

XboxLiveCreatorsTitle 值 | 默认“范围”值
--- | ---
“true”  |  “xbl.signin xbl.friends”
“false”  |  “xboxlive.signin”



下面的列表描述了**范围**字段的有效值。

“范围”值 | 描述
--- | ---
xbl.signin  | 对创意者计划游戏包括登录功能。 对于创意者计划游戏是必需的。
xbl.friends | 对创意者计划游戏包括好友和社交排行榜功能。
xboxlive.signin | 对访问 Xbox Live 全部功能的游戏包括登录功能。 对于非创意者计划游戏是必需的。

目前，指定**范围**字段的唯一原因是，如果你要创建一款 Xbox Live 创意者计划游戏，并且你的游戏不需要访问好友列表或社交排行榜（限定在你的好友范围的排行榜）。 在这种情况下，你可以向 XboxServices.config 文件中添加下面的一行：

```
  "Scope" : "xbl.signin"
```

添加这一行后，当你首次启动应用时，可以防止 UWP 应用请求好友列表的访问权限。

## <a name="example-xboxservicesconfig-file"></a>示例 XboxServices.config 文件

```
{
  "PrimaryServiceConfigId": "00000000-0000-0000-0000-000064382e34",
  "TitleId": 9039138423,
  "XboxLiveCreatorsTitle": true,
  "Scope" : "xbl.signin xbl.friends"
}
```
