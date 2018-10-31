---
title: Xbox Live 的摘要页
author: KevinAsgari
description: 介绍如何充分利用 Xbox Live 的摘要视图
ms.assetid: ''
ms.author: kevinasg
ms.date: 10/19/2018
ms.topic: article
ms.localizationpriority: medium
keywords: Xbox live，Xbox，游戏，uwp，windows 10，Xbox one，xbox live 摘要，摘要，发布，xbox live 历史记录、 命令栏、 历史记录选项卡、 摘要表
ms.openlocfilehash: c2f8f661b55d105e1f1410bafd09903f55882aaa
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/31/2018
ms.locfileid: "5833791"
---
# <a name="the-xbox-live-configuration-summary-page"></a>Xbox Live 配置摘要页

你可以使用[合作伙伴中心](https://developer.microsoft.com/dashboard)配置为你的游戏的 Xbox Live。 合作伙伴中心与你将能够管理针对每个游戏的沙盒的服务配置。
若要配置你的游戏的 Xbox Live 方面，导航到你的游戏的 Xbox Live 部分位于**服务** > **Xbox Live**。 在此页上，你会发现你在选定沙盒上的当前配置的快照。 你将找到内容已配置并发布到沙盒以及的明细。

## <a name="sandbox-selector"></a>沙盒选择器

 [沙盒](../../xbox-live-sandboxes.md)是现在可以之间切换，也可以选择通过展开的顶级导航项目。 UI 为选项卡顶部显示标题的沙盒。 相关联的上下文中是沙盒的每个选项卡中显示的信息。  

![切换沙盒选项卡的图像](../../images/summary/sandbox-tabs1.gif)

 你可以通过选择添加其他沙盒"+"这将提供一个对话框，其中指定你想要复制的配置中的沙盒和哪些沙盒你想要将复制到配置。  

 ![扩展到新的沙盒选项卡的图像](../../images/summary/sandbox-tabs2.gif)

## <a name="command-bar"></a>命令栏

如上文所显示的页面始终是沙盒的上下文中因此命令栏公开它的正下方显示的所有操作可以在给定沙盒中执行。 向你提供的命令是：  

* **导出**-这为你提供一个包含位于沙盒内的所有配置的文档的 zip 文件。
* **导入**-允许你提供包含一次上载将在沙盒中可用的有效 XBL 文档的 zip 文件。
* **认证**的发布到证书沙盒的当前配置。  *你还可以使用发布按钮，并更改目标为证书以完成此操作。*
* **历史记录**-打开选项卡，显示的内容以及创建者信息。 你可以打开此选项卡上，在任何页面上的，并且它将筛选该页面上创建的对象。
* **发布**-允许你选择你的沙盒源和目标。 选择后，将运行验证，从而让你知道是否您可以发布配置。 如果允许，选择发布将设置你的沙盒的配置，以便你可以测试此配置中同时使用相应的沙盒。  
  
  
![在命令栏的图像](../../images/summary/command-bar.png)  

## <a name="summary-table"></a>摘要表

UI 现在提供有意义倾斜设置的不同的所有配置允许的内容已配置，什么是可选的并发布到 retail 之前仍需要什么眼视图。  

* **详细信息**--介绍了什么已配置为 （这包括你已创建，但尚未发布的对象） 的当前沙盒内的特定功能
* **由于上次发布**– 这可让你知道你已创建了尚未发布到测试沙盒哪些新配置
* **状态**– 通知你该功能是否准备好发布到 retail。 必须解决任何标记为"不随时可供零售"，开发人员自行 \ 对标记为"可选"的行。

*以前，选择"测试 button"后, 验证会运行，并且你才会知道必须纠正; 你需要一个问题这简化了该过程中，并创建更好的 UX*  
  
![在命令栏的图像](../../images/summary/summary-table.png)  

## <a name="history-pane"></a>历史记录窗格

历史记录窗格显示在沙盒中创建了哪些对象，并指示者和时间。 在摘要页面上所有对象创建并发布沙盒上所做的操作，将显示历史记录窗格。 但是，当你打开该窗格位于如成就的特定页面时你将看到仅成就历史记录允许你轻松地筛选历史记录搜索。  

![历史记录窗格的图像](../../images/summary/history.png)  

## <a name="best-practices"></a>最佳实践

* 进行一些编辑操作以确保它们是在你的测试帐户和设备上实时后，请将更改发布到沙盒。
* 使用新选项卡视图、 摘要表和历史记录窗格可帮助你快速识别什么是发布的位置。
* 在极端情况下需要执行此你可以使用的沙盒之间的 XML 比较这两个沙盒来获取两者导出功能文档和中使用等之外比较工具打开它们。
* 导出可用于获取可以提交到你自己的源代码文件的本地版本。 这样一来，如果你每个丢失实际上不会丢失其任何配置。 可以在源代码中取出本地配置并导入回该沙盒。