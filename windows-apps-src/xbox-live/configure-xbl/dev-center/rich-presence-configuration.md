---
title: 在合作伙伴中心的 Rich Presence 配置
author: aablackm
description: 了解如何在合作伙伴中心中配置完整状态字符串
ms.author: aablackm
ms.date: 02/27/2018
ms.topic: article
ms.localizationpriority: medium
keywords: Xbox Live，Xbox，游戏，uwp，windows 10，Xbox one，Rich Presence 字符串，合作伙伴中心
ms.openlocfilehash: cb16b7e8c3776f2509207f15a6c26341f9028fe5
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2018
ms.locfileid: "7578824"
---
# <a name="configure-rich-presence-in-partner-center"></a>在合作伙伴中心中配置完整状态

Rich Presence 字符串可显示用户的游戏内活动。 这些字符串将显示在**好友和俱乐部**列表中玩家的玩家代号以及 Xbox Live 用户个人资料中。 配置的 Rich Presence 字符串将附加到正在玩的游戏的名称后。 如果你要创建一个名为 BubblePop 的游戏，并且将 Rich Presence 字符串配置为“Popping bubbles with friends”，则你配置的字符串将产生状态“BubblePop - Popping bubbles with friends”。 下面你可以看到 Rich Presence 字符串将显示在上下文中。

在下面的屏幕截图中，Xbox Live 用户 **Last Roar** 和 **Lucha Uno** 正在玩使用 Rich Presence 字符串的游戏。

![好友列表示例](../../images/rich_presence/RichPresence_FriendsList_Screen.jpg)

在下面的屏幕截图中，你可以在 **Lucha Uno** 的个人资料中看到其 Rich Presence 字符串。

![个人资料示例](../../images/rich_presence/RichPresence_Config_ProfileScreen.jpg)

> [!IMPORTANT]
> Rich Presence 字符串不适用于 Xbox Live 创意者计划作品，因此不可为这些作品配置该字符串。 本文中的内容适用于 ID@Xbox 和托管的合作伙伴作品。

## <a name="requirements"></a>要求

在配置 Rich Presence 字符串之前，你和你的作品必须满足以下条件：

- 你必须拥有 Windows 开发帐户。
- 开发帐户必须注册 ID@Xbox 计划或者注册为托管合作伙伴开发者帐户。
- 你的游戏必须在合作伙伴中心中注册，并且启用了 Xbox live。

你可以使用完整状态字符串之前，必须在合作伙伴中心中将进行配置。

## <a name="rich-presence-configuration-page"></a>Rich Presence 配置页面

完整状态字符串配置为你在[合作伙伴中心](https://partner.microsoft.com/dashboard)中的游戏的 Xbox Live 服务的一部分。

通过以下说明导航到 Rich Presence 配置页：

1. Developer.microsoft.com 上，转到[合作伙伴中心](https://partner.microsoft.com/dashboard)。
2. 如果已请求登录，使用已注册的 Windows 开发人员帐户登录。
3. 从**概述**页面中选择支持 Xbox Live 的你的作品或应用。 不要选择创意者计划作品，因为将不会为 Rich Presence 字符串配置启用此类作品。
4. 单击**服务**下拉列表，选择 Xbox Live。
5. 向下滚动到 **Rich Presence** 链接并单击它。

Rich Presence 页面将显示该服务的简要说明、一个用于创建新 Rich Presence 字符串的按钮以及一个之前配置字符串的可搜索列表。 在此页中，你可以配置新字符串，还可以编辑并查看配置的字符串。

![示例 Rich Presence 字符串配置页面](../../images/rich_presence/RichPresence_ConfigPage_New.JPG)

> [!NOTE]
> 从 Data Platform 2017 更新起，像“Playing Net Runner - Multiplayer deathmatch on Moon Base with 10 kills.”这样的字符串 将不适用于开发人员。 Data Platform 2013 *变量*不可用于 Data Platform 2017 中。 本例中的变量为击杀数 10。 Data Platform 2017 更新版之后的等效字符串将是“Playing Net Runner - Multiplayer deathmatch on Moon Base.” “Playing Net Runner - In the menus”仍然是一个有效的 Rich Presence 字符串。

## <a name="create-a-new-rich-presence-string"></a>创建一个新 Rich Presence 字符串

要创建 Rich Presence 字符串，请单击标记为**新建 Rich Presence 字符串**的按钮。 你将看到填写**状态详细信息**的 UI，包括新 Rich Presence 字符串的**唯一 Rich Presence ID**以及**显示字符串**。

![新 Rich Presence 字符串 UI](../../images/rich_presence/RichPresence_Config_NewString.JPG)

**唯一 Rich Presence ID** - 唯一 Rich Presence ID 是一个用于识别 Rich Presence 字符串的字符串。 此字符串将用于设置你的游戏玩家的状态，它与你想要显示的特定字符串关联。 你的 ID 最多为 50 个字符。

**显示字符串** - 显示字符串是你要显示的字符串，它会附加到某些你的游戏玩家的状态后。 在这里，你可以填写要显示的 Rich Presence 字符串，以便玩家对你的游戏产生兴趣。 最多可以显示 100 个字符，不过有一些实例中将仅显示前 40 个字符。

为了创建新的 Rich Presence 字符串，请填写这些字段，然后按**保存**按钮。
单击“保存”后，将回到“Rich Presence 配置”页，你可以在此看到添加到配置字符串列表中的新 Rich Presence 字符串。

## <a name="review-edit-and-delete-strings"></a>查看、编辑和删除字符串

你可以在此看到“Rich Presence 配置”页，上面显示了几个配置的字符串。
![Rich Presence 页面配置示例](../../images/rich_presence/RichPresence_ConfigPage_Configured.JPG)

要查看之前创建的字符串，只需浏览“Rich Presence 配置”页上的列表。 你可以同时看到唯一 Rich Presence ID 和显示字符串。 当需要在你的作品代码中使用唯一 Rich Presence ID 来指定 Rich Presence 字符串时，这将非常有用。

要编辑 Rich Presence 字符串，只需单击你要编辑的字符串的**唯一 Rich Presence ID** 链接。 你将看到用于创建一个新 Rich Presence 字符串的相同 UI，其中已经填入当前字符串设置可供编辑。 进行编辑后，单击**保存**按钮以更新更改后的配置字符串。

要删除配置的 Rich Presence 字符串，单击“Rich Presence 配置”页上的**删除**链接，该链接与你要删除的 Rich Presence 字符串在同一行。 系统将提示你确认此删除。

## <a name="further-reading"></a>进一步阅读

要获取有关 Rich Presence 字符串功能以及如何实现它们的更深入概念信息，请阅读 [Rich Presence 文档](https://docs.microsoft.com/en-us/windows/uwp/xbox-live/social-platform/rich-presence-strings/rich-presence-strings-overview)。