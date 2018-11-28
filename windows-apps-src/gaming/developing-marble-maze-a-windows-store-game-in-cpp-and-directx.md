---
title: 开发 Marble Maze，一款使用 C++ 和 DirectX 的 UWP 游戏
description: 文档的这部分介绍了如何使用 DirectX 和 Visual C++ 创建 3D 通用 Windows 平台 (UWP) 游戏。
ms.assetid: 43f1977a-7e1d-614c-696e-7669dd8a9cc7
ms.date: 08/10/2017
ms.topic: article
keywords: windows 10, uwp, 游戏, 示例, directx, 3d
ms.localizationpriority: medium
ms.openlocfilehash: e61c96a1b4deb7dd1beb0233814f86ce1b5fb42c
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/28/2018
ms.locfileid: "7969908"
---
# <a name="developing-marble-maze-a-uwp-game-in-c-and-directx"></a>开发 Marble Maze，一款使用 C++ 和 DirectX 的 UWP 游戏




本主题介绍了如何使用 DirectX 和 Visual C++ 创建 3D 通用 Windows 平台 (UWP) 游戏。 名为 Marble Maze 的游戏涵盖多个外形规格，如平板电脑以及传统台式机和笔记本电脑。

> [!NOTE]
> 若要下载 Marble Maze 源代码，请参阅 [GitHub 上的示例](http://go.microsoft.com/fwlink/?LinkId=624011)。

> [!IMPORTANT]
> Marble Maze 演示了我们认为是创建 UWP 游戏的最佳做法的设计模式。 你可修改许多实现细节，以符合你自己的做法和你所开发游戏的独特需求。 你可随意使用更适合你的需求的不同技术或库。 （但是，始终要确保你的代码通过 [Windows 应用认证工具包](https://docs.microsoft.com/windows/uwp/debug-test-perf/windows-app-certification-kit)。）在我们认为此处所使用的实现对成功的游戏开发至关重要时，我们会在本文中着重介绍它。

 

## <a name="introducing-marble-maze"></a>Marble Maze 简介


我们选择 Marble Maze 是因为它相对基本，但仍然演示了大多数游戏中包含的丰富功能。 它展示了如何使用图形、输入处理和音频。 它还演示了规则和目标等游戏构成。

Marble Maze 类似于通常由一个包含洞和钢珠或玻璃珠的桌面迷宫游戏。 Marble Maze 的目标与桌面版本相同：倾斜迷宫以引导弹珠在尽可能短的时间内从出发点移动到迷宫终点，并且不让弹珠落入任何洞中。 Marble Maze 添加了检查点的概念。 如果弹珠落入一个洞中，游戏会在弹珠经过的最后一个检查点位置重新开始。

Marble Maze 为用户提供了多种方式来与游戏板交互。 如果你有一个启用了触摸或启用了加速计的设备，可使用这些设备移动游戏板。 也可使用 Xbox One 控制器或鼠标来控制游戏。

![Marble Maze 游戏的屏幕截图。](images/marblemaze-2.png)

## <a name="prerequisites"></a>先决条件


-   Windows 10 创意者更新
-   [Microsoft Visual Studio2017](https://www.visualstudio.com/downloads/)
-   C++ 编程知识
-   熟悉 DirectX 和 DirectX 术语
-   COM 的基础知识

## <a name="who-should-read-this"></a>谁应该阅读本文？


如果你感兴趣创建 3D 游戏或其他图形密集型应用程序的 windows 10，本文适合你。 我们希望你使用本文列出的原则和实践来创建自己的 UWP 游戏。 如果你了解一定的 C++ 和 DirectX 编程背景知识或对其有着强烈的兴趣，这将帮助你充分利用本文档。 如果你没有 DirectX 方面的经验，而有类似的 3D 图形编程环境方面的经验，仍将从本文档受益。

文档[操作实例：使用 DirectX 创建简单的 UWP 游戏](tutorial--create-your-first-uwp-directx-game.md)描述了另一个使用 DirectX 和 C++ 实现基本 3D 射击游戏的示例。

## <a name="what-this-documentation-covers"></a>本文档所涵盖的内容


本文档将介绍如何：

-   使用 Windows 运行时 API 和 DirectX 创建 UWP 游戏。
-   使用 [Direct3D](https://msdn.microsoft.com/library/windows/desktop/ff476080) 和 [Direct2D](https://msdn.microsoft.com/library/windows/desktop/dd370990) 处理可视内容，例如模型、纹理、顶点和像素着色器，以及 2D 覆盖。
-   集成各种输入机制，例如触摸、加速计和 Xbox One 控制器。
-   使用 [XAudio2](https://msdn.microsoft.com/library/windows/desktop/hh405049) 合并音乐和声音效果。

## <a name="what-this-documentation-does-not-cover"></a>本文未涵盖的内容


本文未涵盖游戏开发的以下方面。 这些方面将由其他资源介绍。

-   3D 游戏设计原则。
-   C++ 或 DirectX 编程基础。
-   如何设计资源，例如纹理、模型或音频。
-   如何排除游戏中的行为或性能问题。
-   如何准备在世界其他地方使用你的游戏。
-   如何向 Microsoft Store 认证和发布你的游戏。

Marble Maze 还使用 [DirectXMath](https://msdn.microsoft.com/library/windows/desktop/hh437833) 库处理 3D 几何图形并执行力学计算，例如碰撞。 本节未深入介绍 DirectXMath。 有关 Marble Maze 如何使用 DirectXMath 的详细信息，请参见源代码。

尽管 Marble Maze 提供了许多可重用的组件，但它不是一个完整的游戏开发框架。 在我们认为 Marble Maze 组件可在游戏中重用时，我们会在本文档中着重介绍它。

## <a name="next-steps"></a>后续步骤


我们建议你首先从 [Marble Maze 示例基础](marble-maze-sample-fundamentals.md)入手，了解 Marble Maze 结构和 Marble Maze 源代码遵循的一些编码和风格指南。 下表列出了本节中的文档，以便你更容易查阅它们。

## <a name="in-this-section"></a>本部分内容


| 标题                                                                                                                    | 描述                                                                                                                                                                                                                                        |
|--------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Marble Maze 示例基础](marble-maze-sample-fundamentals.md)                                                   | 概述游戏结构，以及源代码所遵循的一些代码和风格指南。                                                                                                                                 |
| [Marble Maze 应用程序结构](marble-maze-application-structure.md)                                               | 描述如何构建 Marble Maze 应用程序代码，以及 DirectX UWP 应用的结构与传统的桌面应用程序的结构有何不同。                                                                                    |
| [向 Marble Maze 示例添加可视内容](adding-visual-content-to-the-marble-maze-sample.md)                   | 描述在使用 Direct3D 和 Direct2D 时应记住的一些重要实践。 另外还描述了 Marble Maze 如何为可视内容应用这些实践。                                                                           |
| [向 Marble Maze 示例添加输入和交互性](adding-input-and-interactivity-to-the-marble-maze-sample.md) | 描述 Marble Maze 如何使用加速计、触摸和 Xbox One 控制器输入，使用户能够导航菜单和与游戏板交互。 另外还描述了在处理输入时应记住的一些最佳实践。 |
| [向 Marble Maze 示例添加音频](adding-audio-to-the-marble-maze-sample.md)                                     | 描述 Marble Maze 如何使用音频来向游戏体验添加音乐和声音效果。                                                                                                                                                  |

 

 

 




