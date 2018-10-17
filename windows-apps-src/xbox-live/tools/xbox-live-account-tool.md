---
title: Xbox Live 帐户工具
author: KevinAsgari
description: 了解如何使用 Xbox Live 帐户工具快速创建用于测试 Xbox Live 游戏的测试帐户。
ms.assetid: ec5959f9-1c60-4aa4-94a6-5d8bdcf77a96
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, 测试, 测试帐户
ms.localizationpriority: medium
ms.openlocfilehash: 55e2d46f59a8ecd2d8bac77e8ce61834a4249a88
ms.sourcegitcommit: 1c6325aa572868b789fcdd2efc9203f67a83872a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2018
ms.locfileid: "4750048"
---
# <a name="xbox-live-account-tool"></a>Xbox Live 帐户工具

## <a name="what-is-xbox-live-account-tool"></a>什么是 Xbox Live 帐户工具？
Xbox Live 帐户工具是一款设计用于帮助开发人员为测试游戏方案设置现有开发人员帐户的工具。 例如，你可以使用 Xbox Live 帐户工具更改开发人员帐户的玩家代号或为帐户的好友列表快速添加 1000 位关注者。

## <a name="what-can-i-do-with-xbox-live-account-tool"></a>你可以使用 Xbox Live 帐户工具做什么？
您可以：
  1. 查看用户的个人资料设置、XUID 和有效权限
  2. 为用户的社交图片，通过文本文件或 Xbox 开发人员平台 csv 添加关注者列表
  3. 管理用户的好友列表：对你关注的用户执行收藏、取消收藏、阻止和取消阻止操作，并查看他们是否也关注你
  4. 更改开发用户的信誉（并立即查看原始的信誉统计值）
  5. 更改用户的玩家代号

## <a name="where-can-i-find-xbox-live-account-tool"></a>我可以在哪找到 Xbox Live 帐户工具？
从 Xbox Live 工具程序包的一部分找不到 Xbox Live 帐户工具[https://aka.ms/xboxliveuwptools](https://aka.ms/xboxliveuwptools)。

## <a name="how-do-i-log-in"></a>如何登录？
你需要使用想要管理的用户凭据并指定正确的沙盒。 请确保开发人员帐户有权访问沙盒，否则登录可能会失败。 该工具设计用于使用沙盒的开发人员帐户。

## <a name="can-i-use-a-retail-account-or-does-it-have-to-be-a-sandboxed-account"></a>我能够使用零售帐户或者是否必须是沙盒帐户？
当然，你可以使用 Xbox Live 帐户工具管理零售帐户，但是并非所有功能均受支持。 例如，你无法更改零售用户的信誉。

## <a name="how-do-i-change-a-dev-users-gamertag"></a>如何更改开发用户的玩家代号？
导航至“玩家代号”选项卡并输入玩家代号。 玩家代号必须仅包含数字、字母和空格，并且长度只能是 15 个字符。 开发人员帐户玩家代号必须以 2 开头。 当前只支持一项更改。

## <a name="how-do-i-see-my-block-list"></a>如何查看我的阻止列表？
导航至“人脉”选项卡并选择“已阻止”列标题，以便将当前已阻止的用户排序。

## <a name="how-do-i-follow-a-large-group-of-users"></a>如何关注一大组用户？
如果你想要关注一个 XUID 列表，请将它们复制到文本文件。 导航至“关注”选项卡，选择“导入列表”，然后选择你的文件。 XUID 应填充到列表视图中。 单击“提交更改”即可关注这些用户。

## <a name="how-do-i-block-someone"></a>如何阻止某人？
导航至“人脉”选项卡并选择你想要阻止的用户。 按下“阻止”按钮，它们将被批量阻止。 如果你发现错误，只需稍后重试。

## <a name="how-do-i-change-my-dev-accounts-repuation"></a>如何更改我的开发人员帐户的信誉？
导航到“信誉”选项卡。选择你想要的默认值，然后按“提交更改”按钮即可提交请求。 如果请求成功，那么你应该可以看到信誉统计值更改。

## <a name="what-do-the-values-in-the-reputation-tab-mean"></a>“信誉”选项卡中的值代表什么？
整体信誉值通过三个子信誉值计算：公平比赛（多人游戏准则）、用户生成的内容（视频剪辑及类似内容）和通信（消息和语音）。 每个类别的原始值在 0 到 75 之间，其中越高的值表示用户的信誉越好。 OverallStatIsBad 将告诉你用户是否具有“避开我”信誉。

## <a name="whats-the-black-area-at-the-bottom"></a>底部的黑色区域是什么？
为了给你提供相关信息，我们认为调试输出打印在 UI 中会非常有用。 此区域将告诉你哪个工具负责打印遇到的任何错误。

## <a name="wheres-my-gamerpic"></a>我的玩家图片在哪？
我们已注意到以下 bug：某些开发人员帐户无法获取帐户创建时自动生成的玩家图片。

## <a name="why-are-things-happening-so-slowly"></a>为什么响应如此慢？
对于批处理操作（如添加关注者），工具将会自动执行批处理，以阻止大型请求。

## <a name="how-do-i-run-xbox-live-account-tool"></a>如何运行 Xbox Live 帐户工具？
将 Xbox Live SDK 提取到文件夹，然后双击 Tools\XboxLiveAccountTool.exe 文件即可运行。

## <a name="i-have-a-feature-request-how-do-i-get-my-feature-incorporated"></a>我有一个功能请求！ 如何将我的功能包括在内？
将任何功能请求告知你的 Microsoft 代表，我们将会采取相应的措施。
