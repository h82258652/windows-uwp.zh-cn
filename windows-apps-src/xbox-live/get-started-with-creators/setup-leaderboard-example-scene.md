---
title: 在 Unity 中使用 Leaderboard 示例场景
description: 显示了正确设置 Unity 排行榜场景的步骤
ms.date: 04/24/2018
ms.topic: get-started-article
keywords: xbox live、 xbox、 游戏、 uwp、 windows 10 中，一个 xbox、 unity、 排行榜
ms.openlocfilehash: d931a0fdcbdec5dd9deb53876a19e1b475afcb93
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589742"
---
# <a name="the-leaderboard-example-scene-in-unity"></a>在 Unity 中 Leaderboard 示例场景

[Unity 插件](https://github.com/Microsoft/xbox-live-unity-plugin)托管数量的示例场景展示适用于 Unity 开发人员的 Xbox Live 服务。 一个此类场景是 leaderboard 示例场景。 在下面的屏幕截图中，你将看到 leaderboard 示例场景只是显示一个 Xbox Live 用户用于登录面板和面板包含排行榜。 如果此时 play 达到而无需添加到此场景，您会发现登录面板中填充了假的用户数据，但排行榜加载任何信息。 为了获得此示例场景中，从而加载实际的排行榜需要添加一些内容。

![排行榜场景屏幕快照](../images/unity/leaderboard-scene-1804.JPG)

## <a name="prerequisites"></a>必备条件

在 Xbox Live 排行榜基于在 Xbox Live 统计信息服务中的统计信息。 可以填充排行榜数据之前，需要具有某些与您的测试帐户相关联的统计信息。 如果你尚未添加统计信息对您的标题可以了解如何执行此操作通过阅读[添加到 Unity 项目的播放机统计信息和排行榜](add-stats-and-leaderboards-in-unity.md)。 该文章的统计信息部分中执行此操作后返回到此处以显示示例排行榜场景中的统计信息。

## <a name="the-leaderboard-inspector"></a>排行榜检查器

排行榜预设可具有许多可以在 leaderboard 脚本组件，如 UI 的检查器部分中更改的设置*主题*，排行榜，xbox 控制器设置，与关联的播放器和其他排行榜的设置。 您可以看到排行榜设置拆分为下面检查器中的几个不同部分。

![排行榜检查器屏幕截图](../images/unity/leaderboard_script_inspector.JPG)

### <a name="theme-and-display-settings"></a>主题和显示设置

此部分提供了一个设置名为*主题*。 这是简单的拖放向下该编辑器可以为您排行榜预设使用深色或浅色主题。 这将更改背景、 字体和图像颜色的预设。 在 unity player 中播放场景时，轻松看到效果。

![浅色主题](../images/unity/leaderboard_light_theme.JPG) ![深色主题](../images/unity/leaderboard_dark_theme.JPG)

### <a name="stat-configuration"></a>统计信息配置

本部分中，可确定的行填充排行榜时，将检索的数据。

- **播放机数目**-这决定了哪些播放机排行榜与相关联。
- **Stat** -用于填充排行榜数据的状态。 这是必需的排行榜预设可将数据加载。
- **排行榜类型**-此下拉列表菜单排行榜数据加载到应用筛选器。 如果*Global*选择排行榜将未经过筛选，并且将显示所选状态的值与每个播放机。如果*朋友*选择排行榜将筛选为仅显示播放机排行榜上也是在您的朋友列表中。 如果*收藏*选择排行榜将筛选为仅显示播放机排行榜上也是在您的收藏夹列表中。
- **条目计数**-范围为 1 到 100，指示将一次返回排行榜的行数的滑块。 此处设置的数将确定每页显示排行榜行数。

### <a name="controller-configuration"></a>控制器配置

排行榜预设可允许开发人员轻松地配置 Xbox 控制器使用。 预设的控制器配置节，可启用，并选择控制排行榜预设的按钮。

- **启用控制器输入**-简单单选按钮切换。 如果选中此选项然后可以使用 Xbox 控制器与预设可进行交互。 支持所需的控制器。
- **游戏杆数**-将指定的控制器可以与此排行榜预设进行交互。
- **下一步页面控制器按钮**-下拉列表菜单的按钮加载排行榜行的下一页面的控件。
- **前一页控制器按钮**-下拉列表菜单的按钮加载排行榜行的前一页的控件。
- **下一步的视图控制器按钮**-切换视图类型之间**所有**并**最近我**。
- **上一个视图控制器按钮**-切换视图类型之间**所有**并**最近我**。
- **垂直滚动输入轴**-字符串，它指定了哪些控制器轴是与滚动相关联。
- **滚动速度乘数**-确定控制器滚动速度。

> [!NOTE]
> 更改用于更改的页面或视图的排行榜的按钮也会更改用于每个函数排行榜按钮相关联的图片。 不需要更改的用户界面参考资料部分手动以匹配到按钮的图片。

### <a name="ui-references"></a>UI 引用

此部分控制图像和排行榜预设的常规构成。 本部分不需要更改为排行榜预设可成功地使用。 但是您可能需要对此部分以根据自己的目的为自定义查找范围的预设可进行调整。

## <a name="populating-the-unity-player-leaderboard-with-fake-data"></a>填充了 Unity 播放器排行榜 （包含假数据）

为了填充数据与 Unity 播放器排行榜需要将统计信息添加到排行榜预设。 查看排行榜预设检查器中的会显示，可能需要的类型对象`Stat Base`作为其脚本中的公共参数。 您可以将任一`State Base`键入预设`IntegerStat`， `DoubleStat`，或`StringStat`从 Xbox Live Unity 插件置入 prefabs 文件夹并将其放在排行榜中的此位置中预设。

![拖放到 Stat 预设](../images/unity/stat-to-leaderbaord-drag.gif)

现在在 Unity 中扮演场景，你将找到排行榜填入如下面的虚设数据。

![模拟数据填充排行榜屏幕快照](../images/unity/leaderboard-fake-data-1804.JPG)

## <a name="populating-a-visual-studio-built-project-with-real-data"></a>Visual Studio 生成项目使用实际数据填充

为了填充使用实际数据，你将需要生成您的游戏能够在您的计算机上本地运行的标题为排行榜。 你将需要本地生成，因为在 Unity 编辑器不能访问到 Xbox Live。 除了在生成项目以本地方式运行，需要在到五分之一的已初始化，并且已在标题的值在排行榜中配置状态。 若要将关联到你排行榜 stat 需要修改的 ID 和排行榜预设中的统计信息对象的显示名称。 ID 将需要以匹配的配置中 stat[合作伙伴中心](https://partner.microsoft.com/dashboard)。 已执行此操作后，生成你的项目中所述[生成的 Unity 项目中的 Xbox Live 配置部分](configure-xbox-live-in-unity.md#build-and-test-the-project)。 执行此项目为 x64 以在本地计算机为目标的生成应允许您使用真正的玩家代号登录并填充使用实际数据排行榜。