---
title: 创建 DirectX 通用 Windows 平台 (UWP) 游戏
description: 在此教程集中，你将学习如何使用 DirectX 和[c + +/WinRT](/windows/uwp/cpp-and-winrt-apis/)创建名为**Simple3DGameDX**的基本通用 Windows 平台（UWP）示例游戏。
ms.assetid: 9edc5868-38cf-58cc-1fb3-8fb85a7ab2c9
keywords: DirectX 示例游戏，示例游戏，通用 Windows 平台（UWP），Direct3D 11 游戏
ms.date: 06/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 2e3007cd79546cba8961000cb2aae44b0b0536fe
ms.sourcegitcommit: 20969781aca50738792631f4b68326f9171a3980
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/26/2020
ms.locfileid: "85409566"
---
# <a name="create-a-simple-universal-windows-platform-uwp-game-with-directx"></a>使用 DirectX 创建简单的通用 Windows 平台 (UWP) 游戏

在此教程集中，你将学习如何使用 DirectX 和[c + +/WinRT](/windows/uwp/cpp-and-winrt-apis/)创建名为**Simple3DGameDX**的基本通用 Windows 平台（UWP）示例游戏。 游戏在简单的第一人称3D 拍摄库中发生。

> [!NOTE]
> 可从中下载**Simple3DGameDX**示例游戏的链接是[Direct3D 示例游戏](/samples/microsoft/windows-universal-samples/simple3dgamedx/)。 C + +/WinRT 源代码位于名为的文件夹中 `cppwinrt` 。 有关其他 UWP 示例应用的信息，请参阅[获取 UWP 应用示例](/windows/uwp/get-started/get-uwp-app-samples)。

这些教程涵盖了游戏的所有主要部分，其中包括用于加载资产（如艺术和网格）的流程、创建主要游戏循环、实现简单渲染管道以及添加声音和控制。

还会看到 UWP 游戏开发技术和注意事项。 我们将重点介绍关键 UWP DirectX 游戏开发概念，并围绕这些概念指出特定于 Windows 运行时的注意事项。

## <a name="objective"></a>目标

了解 UWP DirectX 游戏的基本概念和组件，并更轻松地使用 DirectX 设计 UWP 游戏。

## <a name="what-you-need-to-know"></a>需要了解的事项

对于本教程，您需要熟悉这些主题。

- [C + +/WinRT](/windows/uwp/cpp-and-winrt-apis/)。 C + +/WinRT 是适用于 Windows 运行时（WinRT） Api 的标准新式 c + + 17 语言投影，作为基于标头文件的库实现，旨在向您提供对新式 Windows Api 的一流访问。
- 基本线性代数和牛顿物理学概念。
- 基本图形编程术语。
- 基本的 Windows 编程概念。
- 基本熟悉 [Direct2D](/windows/desktop/Direct2D/direct2d-portal) 和 [Direct3D 11](/windows/desktop/direct3d11/how-to-use-direct3d-11) API。

##  <a name="direct3d-uwp-shooting-gallery-sample"></a>Direct3D UWP 拍摄库示例

**Simple3DGameDX**示例游戏实现了一个简单的第一人称3d 拍摄库，其中播放机在移动目标时激发球。 击中每个目标就奖给一组点数，玩家可以晋级 6 个难度不断增加的级别。 在级别末尾，将记录点数，并且玩家可以赢得最终得分。

该示例演示了这些游戏概念。

- DirectX 11.1 与 Windows 运行时之间的互操作。
- 第一人称 3D 视角和相机
- 立体 3D 效果
- 冲突-三维对象之间的检测
- 处理玩家的鼠标、触摸和 Xbox 控制器控件输入
- 音频混合和播放
- 基本游戏状态-计算机

![操作中的示例游戏](images/simple-dx-game-overview.png)

|主题|说明|
|-------|-------------|
|[设置游戏项目](tutorial--setting-up-the-games-infrastructure.md)|开发游戏的第一步是在 Microsoft Visual Studio 中设置一个项目。 为游戏开发专门配置项目后，可以在以后将其重新用作模板类型。|
|[定义游戏的 UWP 应用框架](tutorial--building-the-games-uwp-app-framework.md)|编写通用 Windows 平台（UWP）游戏的第一步是生成框架，该框架使应用对象能够与 Windows 交互。|
|[游戏流管理](tutorial-game-flow-management.md)|定义支持玩家和系统交互的高级状态机。 了解 UI 如何与整个游戏的状态机交互以及如何为 UWP 游戏创建事件处理程序。|
|[定义主游戏对象](tutorial--defining-the-main-game-loop.md)|现在，我们将查看示例游戏的主对象的详细信息，以及它实现的规则如何转换为与游戏世界的交互。|
|[呈现框架 I：呈现简介](tutorial--assembling-the-rendering-pipeline.md)|了解如何开发呈现管道以显示图形。 呈现简介。|
|[呈现框架 II：游戏呈现](tutorial-game-rendering.md)|了解如何装配显示图形的呈现管道。 游戏呈现、设置和准备数据。|
|[添加用户界面](tutorial--adding-a-user-interface.md)|了解如何向 DirectX UWP 游戏添加2D 用户界面覆盖区。|
|[添加控件](tutorial--adding-controls.md)|现在，我们来看看示例游戏如何实现3-d 游戏中的移动外观控制，以及如何开发基本的触摸、鼠标和游戏控制器控件。|
|[添加声音](tutorial--adding-sound.md)|使用 XAudio2 Api 开发一个简单的声音引擎，以播放游戏音乐和声音效果。|
|[扩展示例游戏](tutorial-resources.md)|了解如何为 UWP DirectX 游戏实现 XAML 覆盖。|
