---
title: 创建 DirectX 通用 Windows 平台 (UWP) 游戏
description: 在本套教程中，你将了解如何使用 DirectX 和 C++ 创建基本的通用 Windows 平台 (UWP) 游戏。
ms.assetid: 9edc5868-38cf-58cc-1fb3-8fb85a7ab2c9
keywords: DirectX 游戏示例, 游戏示例, 通用 Windows 平台 (UWP), Direct3D 11 游戏
ms.date: 12/01/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: dc602e2dd29231c1e6554d7ef55e9666a373fa31
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2018
ms.locfileid: "7702505"
---
# <a name="create-a-simple-universal-windows-platform-uwp-game-with-directx"></a>使用 DirectX 创建简单的通用 Windows 平台 (UWP) 游戏

在本套教程中，你将了解如何使用 DirectX 和 C++ 创建基本的通用 Windows 平台 (UWP) 游戏。 我们将介绍游戏的所有主要部分，包括加载资源（如艺术和网格）、创建主游戏循环、实现简单的呈现管道以及添加声音和控件的过程。

我们将向你介绍 UWP 游戏开发技巧和注意事项。 我们不提供完整的端到端游戏。 而重点介绍关键的 UWP DirectX 游戏开发概念， 并围绕这些概念阐述特定于 Windows 运行时的注意事项。

## <a name="objective"></a>目标

使用 UWP DirectX 游戏的基本概念和组件，更熟练地使用 DirectX 设计 UWP 游戏。

## <a name="what-you-need-to-know-before-starting"></a>开始之前需要了解的内容


在开始本教程之前，你需要熟悉这些主题。

-   Microsoft C++ Windows 运行时语言扩展 (C++/CX)。 这是对 Microsoft C++ 的更新，它合并了自动引用计数，并且是用于使用 DirectX 11.1 或 更高版本开发 UWP 游戏的语言。
-   基本线性代数和牛顿物理学概念。
-   基本图形编程术语。
-   基本的 Windows 编程概念。
-   基本熟悉 [Direct2D](https://msdn.microsoft.com/library/windows/apps/dd370990.aspx) 和 [Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/hh404569) API。

##  <a name="direct3d-uwp-shooting-game-sample"></a>Direct3D UWP 射击游戏示例


此示例实现一个简单的第一人称射击库，游戏内容是玩家射击移动目标上的球。 击中每个目标就奖给一组点数，玩家可以晋级 6 个难度不断增加的级别。 在级别末尾，将记录点数，并且玩家可以赢得最终得分。

该示例演示以下游戏概念：

-   DirectX 11.1 与 Windows 运行时之间的互操作。
-   第一人称 3D 视角和相机
-   立体 3D 效果
-   3D 中对象之间的冲突检测
-   处理玩家的鼠标、触摸和 Xbox 控制器控件输入
-   音频混合和播放
-   基本游戏状态机

![正在操作的游戏示例](images/simple-dx-game-overview.png)

| 主题 | 说明 |
|-------|-------------|
|[设置游戏项目](tutorial--setting-up-the-games-infrastructure.md) | 装配游戏的第一步是在 Microsoft Visual Studio 中设置项目，以便最大限度减少需要执行的代码基础结构工作量。 通过使用正确的模板和专门为游戏开发配置项目，可以节省大量的时间和减少不必要的麻烦。 我们将指导你完成设置和配置简单游戏项目的步骤。 |
| [定义游戏的 UWP 应用框架](tutorial--building-the-games-uwp-app-framework.md) | 构建允许 UWP DirectX 游戏对象与 Windows 交互的框架。 这包括 Windows 运行时属性（如暂停/恢复事件处理、窗口焦点和贴靠）。  |
| [游戏流管理](tutorial-game-flow-management.md) | 定义支持玩家和系统交互的高级状态机。 了解 UI 如何与整个游戏的状态机交互以及如何为 UWP 游戏创建事件处理程序。 |
| [定义主游戏对象](tutorial--defining-the-main-game-loop.md) | 定义如何通过创建规则进行游戏。 |
| [呈现框架 I：呈现简介](tutorial--assembling-the-rendering-pipeline.md) | 装配显示图形的呈现框架。 本主题分为两部分。 呈现简介介绍了如何呈现要在屏幕上显示的场景对象。 |
| [呈现框架 II：游戏呈现](tutorial-game-rendering.md) | 在呈现主题的第二部分，了解如何在呈现之前准备所需的数据。 |
| [添加用户界面](tutorial--adding-a-user-interface.md) | 添加简单的菜单选项和提醒显示组件，以向玩家提供反馈。 |
| [添加控件](tutorial--adding-controls.md) | 向游戏添加移动观看控件 &mdash; 基本触控、鼠标和游戏控制器控件。 |
| [添加声音](tutorial--adding-sound.md) | 了解如何使用 [XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415813) API 为游戏创建声音。 |
| [扩展游戏示例](tutorial-resources.md) | 帮助增加你的 DirectX 游戏开发知识（包括使用 XAML 创建覆盖）的资源。 |