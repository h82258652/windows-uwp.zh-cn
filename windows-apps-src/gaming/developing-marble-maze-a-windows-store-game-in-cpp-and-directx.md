---
title: 开发*大理石迷宫* &mdash; a 使用 c + + 生成的用于 DirectX 的通用 Windows 平台（UWP）游戏
description: 本部分介绍如何使用 DirectX 和 c + + 创建3D 通用 Windows 平台（UWP）游戏。
ms.assetid: 43f1977a-7e1d-614c-696e-7669dd8a9cc7
ms.date: 08/10/2017
ms.topic: article
keywords: windows 10, uwp, 游戏, 示例, directx, 3d
ms.localizationpriority: medium
ms.openlocfilehash: d2c0c630090c178a54a0452ab3cc430ffee4a176
ms.sourcegitcommit: 20969781aca50738792631f4b68326f9171a3980
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/26/2020
ms.locfileid: "85409496"
---
# <a name="developing-marble-mazemdasha-universal-windows-platform-uwp-game-built-with-c-for-directx"></a>开发*大理石迷宫* &mdash; a 使用 c + + 生成的用于 DirectX 的通用 Windows 平台（UWP）游戏

本主题介绍如何使用 DirectX 和 c + + 创建3D 通用 Windows 平台（UWP）游戏。 游戏称为*大理石迷宫*，其中包括多个外形规格，如平板电脑、传统台式计算机和便携式计算机。

> [!NOTE]
> 若要下载*大理石迷宫*源代码，请参阅[GitHub 上的示例](https://github.com/microsoft/Windows-appsample-marble-maze)。

> [!IMPORTANT]
> *大理石迷宫*说明了设计模式，我们认为这是创建 UWP 游戏的最佳实践。 你可修改许多实现细节，以符合你自己的做法和你所开发游戏的独特需求。 你可随意使用更适合你的需求的不同技术或库。 （但是，始终要确保你的代码通过 [Windows 应用认证工具包](https://docs.microsoft.com/windows/uwp/debug-test-perf/windows-app-certification-kit)。）在我们认为此处所使用的实现对成功的游戏开发至关重要时，我们会在本文中着重介绍它。

## <a name="introducing-marble-maze"></a>引入*大理石迷宫*

我们选择了*大理石迷宫*，因为它是相对较基本的，但它仍演示了大多数游戏中的各种功能。 它展示了如何使用图形、输入处理和音频。 它还演示了规则和目标等游戏构成。

*大理石迷宫*类似于表顶部的 labyrinth 游戏，通常从包含孔和钢或玻璃大理石的框构造。 *大理石迷宫*的目标与表顶级版本相同：倾斜迷宫，以尽可能少地从迷宫的开头到最末尾处插入大理石，而不会让大理石落到任何孔洞。 *大理石迷宫*增加了检查点的概念。 如果弹珠落入一个洞中，游戏会在弹珠经过的最后一个检查点位置重新开始。

*大理石迷宫*为用户提供多种方式来与游戏板交互。 如果你有一个启用了触摸或启用了加速计的设备，可使用这些设备移动游戏板。 也可使用 Xbox One 控制器或鼠标来控制游戏。

![Marble Maze 游戏的屏幕截图。](images/marblemaze-2.png)

## <a name="prerequisites"></a>先决条件

-   Windows 10 创意者更新
-   [Microsoft Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
-   C++ 编程知识
-   熟悉 DirectX 和 DirectX 术语
-   COM 的基础知识

## <a name="who-should-read-this"></a>目标读者

如果你有兴趣创建适用于 Windows 10 的3D 游戏或其他图形密集型应用程序，这适用于你。 我们希望你使用本文列出的原则和实践来创建自己的 UWP 游戏。 如果你了解一定的 C++ 和 DirectX 编程背景知识或对其有着强烈的兴趣，这将帮助你充分利用本文档。 如果你没有 DirectX 方面的经验，而有类似的 3D 图形编程环境方面的经验，仍将从本文档受益。

文档[演练：使用 directx 创建简单的 UWP 游戏](tutorial--create-your-first-uwp-directx-game.md)介绍了使用 Directx 和 c + + 实现基本3d 拍摄游戏的另一个示例。

## <a name="what-this-documentation-covers"></a>本文档所涵盖的内容

本文档将介绍如何：

-   使用 Windows 运行时 API 和 DirectX 创建 UWP 游戏。
-   使用[Direct3D](https://docs.microsoft.com/windows/desktop/direct3d11/atoc-dx-graphics-direct3d-11)和[Direct2D](https://docs.microsoft.com/windows/desktop/Direct2D/direct2d-portal)来处理视觉对象内容（如模型、纹理、顶点和像素着色器）以及2d 叠加。
-   集成各种输入机制，例如触摸、加速计和 Xbox One 控制器。
-   使用 [XAudio2](https://docs.microsoft.com/windows/desktop/xaudio2/xaudio2-apis-portal) 合并音乐和声音效果。

## <a name="what-this-documentation-does-not-cover"></a>本文未涵盖的内容

本文未涵盖游戏开发的以下方面。 这些方面将由其他资源介绍。

-   3D 游戏设计原则。
-   C++ 或 DirectX 编程基础。
-   如何设计资源，例如纹理、模型或音频。
-   如何排除游戏中的行为或性能问题。
-   如何准备在世界其他地方使用你的游戏。
-   如何向 Microsoft Store 认证和发布你的游戏。

*大理石迷宫*还使用[DirectXMath](https://docs.microsoft.com/windows/desktop/dxmath/directxmath-portal)库来处理3d 几何并执行物理学计算，如冲突。 本节未深入介绍 DirectXMath。 有关*大理石迷宫*如何使用 DirectXMath 的详细信息，请参阅源代码。

尽管*大理石迷宫*提供许多可重用的组件，但它不是一个完整的游戏开发框架。 当我们考虑在游戏中可重复使用*大理石迷宫*组件时，我们在文档中强调了该组件。

## <a name="next-steps"></a>后续步骤

我们建议您首先了解大理石迷宫[示例基础知识](marble-maze-sample-fundamentals.md)，以了解大理石*迷宫*结构和*大理石迷宫*源代码遵循的一些编码和样式准则。 下表列出了本节中的文档，以便你更容易查阅它们。

## <a name="in-this-section"></a>本节内容

| Title                                                                                                                    | 说明                                                                                                                                                                                                                                        |
|--------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Marble Maze 示例基础](marble-maze-sample-fundamentals.md)                                                   | 概述游戏结构，以及源代码所遵循的一些代码和风格指南。                                                                                                                                 |
| [Marble Maze 应用程序结构](marble-maze-application-structure.md)                                               | 介绍如何构造*大理石迷宫*应用程序代码，以及如何将 DirectX UWP 应用的结构与传统桌面应用程序的结构不同。                                                                                    |
| [向 Marble Maze 示例添加可视内容](adding-visual-content-to-the-marble-maze-sample.md)                   | 描述在使用 Direct3D 和 Direct2D 时应记住的一些重要实践。 还介绍了*大理石迷宫*如何将这些做法应用于视觉内容。                                                                           |
| [向 Marble Maze 添加输入和交互性示例](adding-input-and-interactivity-to-the-marble-maze-sample.md) | 介绍如何在加速感应、触控和 Xbox one 控制器输入中使用*大理石迷宫*，使用户能够导航菜单并与游戏板交互。 另外还描述了在处理输入时应记住的一些最佳实践。 |
| [向 Marble Maze 添加音频示例](adding-audio-to-the-marble-maze-sample.md)                                     | 介绍如何使用*大理石迷宫*将音乐和声音效果添加到游戏体验中。                                                                                                                                                  |
