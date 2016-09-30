---
author: mtoepke
title: "Marble Maze 示例基础"
description: "本文档介绍了 Marble Maze 项目的基本特征，例如它如何在 WIndows 运行时环境中使用 Visual C++、如何创建和构造它，以及如何生成它。"
ms.assetid: 73329b29-62e3-1b36-01db-b7744ee5b4c3
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 5a9df995078763df73542a4101e73e147517b1eb

---

# Marble Maze 示例基础


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


本文档介绍了 Marble Maze 项目的基本特征，例如它如何在 WIndows 运行时环境中使用 Visual C++、如何创建和构造它，以及如何生成它。 本文档还介绍了代码中使用的一些约定。

> **注意** 与本文档对应的示例代码位于 [DirectX Marble Maze 游戏示例](http://go.microsoft.com/fwlink/?LinkId=624011)中。

 
## 
本文档讨论了计划和开发通用 Windows 平台 (UWP) 游戏时的一些重要事项。

-   在 C++ 应用程序中使用 **DirectX 11 应用（通用 Windows）**模板创建你的 DirectX UWP 游戏。 使用 Visual Studio 像生成标准项目一样生成 UWP 应用项目。
-   Windows 运行时提供了各种类和接口，让你可以用一种更加现代、面向对象的方式开发 UWP 应用。
-   使用带乘幂号 (^) 的对象引用来管理 Windows 运行时变量的生命周期、使用 [**Microsoft::WRL::ComPtr**](https://msdn.microsoft.com/library/windows/apps/br244983.aspx) 来管理 COM 对象的生命周期，并使用 [**std::shared\_ptr**](https://msdn.microsoft.com/library/windows/apps/bb982026.aspx) 或 [**std::unique\_ptr**](https://msdn.microsoft.com/library/windows/apps/ee410601.aspx) 来管理所有其他堆分配的 C++ 对象的生命周期。
-   在大多数情况下，使用异常处理而不是结果代码来处理意外错误。
-   结合使用 SAL 注释和代码分析工具来帮助发现应用中的错误。

## 创建 Visual Studio 项目


如果已下载并解压缩了该示例，则可以在 Visual Studio 中打开 MarbleMaze.sln 解决方案文件，这些代码会展现在你的面前。 你还可以通过单击“浏览代码”****选项卡在 [DirectX Marble Maze 游戏示例](http://go.microsoft.com/fwlink/?LinkId=624011) MSDN 示例库页面上查看源代码。

当我们为 Marble Maze 创建了 Visual Studio 项目时，我们从一个现有项目开始。 但是，如果现有的项目没有提供你的 DirectX UWP 游戏所需的基本功能，建议你基于 Visual Studio **DirectX 11 应用（通用 Windows）**模板创建一个 项目，因为它提供了基本的有效 3D 应用程序。

**DirectX 11 应用（通用 Windows）**模板中一个重要的项目设置是 **/ZW** 选项，它使程序能够使用 Windows 运行时语言扩展。 在使用 Visual Studio 模板时默认已启用此选项。

> **注意** **/ZW** 选项与 **/clr** 等选项不兼容。对于 **/clr**，这意味着无法同时针对 .NET Framework 和 Windows 运行时开发同一个 Visual C++ 项目。

 

你从 Windows 应用商店获取的每个 UWP 应用都以应用包的形式提供。 应用包中包含一个程序包清单，后者包含有关应用的信息。 例如，你可指定应用的功能（如需要的访问受保护的系统资源或用户数据的能力）。 如果你确定应用需要某些功能，可使用程序包清单来声明所需的功能。 清单文件还允许指定项目属性，例如支持哪些设备旋转方向、磁贴图像和欢迎屏幕。 有关应用包的详细信息，请参阅[打包应用](https://msdn.microsoft.com/library/windows/apps/mt270969)。

##  生成、部署和运行游戏


像生成标准项目一样生成 UWP 应用项目。 （在菜单栏上，选择“生成”、“生成解决方案”****。）此生成步骤会编译代码，还会打包它以用作 UWP 应用。

生成项目后，你必须部署它。（在菜单栏上，选择“生成”、“部署解决方案”****。）当你从调试程序运行游戏时，Visual Studio 也会部署该项目。

部署项目后，选取 Marble Maze 磁贴以运行游戏。 也可以从 Visual Studio 的菜单栏中选择“调试”、“开始调试”****。

###  控制游戏

可使用触摸、加速计、Xbox 360 控制器或鼠标来控制 Marble Maze。

-   使用控制器上的方向板更改活动菜单项。
-   使用触摸、A 按钮、“开始”按钮或鼠标来挑选菜单项。
-   使用触摸、加速计、左操纵杆或鼠标来倾斜迷宫。
-   使用触摸、A 按钮、“开始”按钮或鼠标来关闭菜单，例如高分表。
-   使用“开始”按钮或 P 键暂停或恢复游戏。
-   使用控制器上的“后退”按钮或键盘上的 Home 键重新启动游戏。
-   高分表可见时，使用“后退”按钮或 Home 键清除所有分数。

##  代码转换


Windows 运行时是可用于创建仅在特殊应用程序环境中运行的 UWP 应用的编程接口。 此类应用使用经授权的函数、数据类型和设备，并且从 Windows 应用商店进行分配。 在最低级别上，Windows 运行时由一个应用程序二进制接口 (ABI) 组成。 ABI 是一个低级二进制合约，它使得 Windows 运行时 API 能够访问多种编程语言，例如 JavaScript、.NET 语言和 Visual C++。

为了从 JavaScript 和 .NET 调用 Windows 运行时 API，这些语言需要特定于每种语言环境的投影。 当你从 JavaScript 或 .NET 调用 Windows 运行时 API 时，你调用的是投影，而投影又会调用基础的 ABI 函数。 尽管你可以直接从 C++ 调用 ABI 函数，但 Microsoft 也为 C++ 提供了投影，因为这些投影可让使用 Windows 运行时 API 更简单，同时仍然保持较高的性能。 Microsoft 还提供了明确支持 Windows 运行时投影的 Visual C++ 的语言扩展。 其中很多语言扩展都和 C++/CLI 语言有着类似的语法。 但是，原生应用将此语法用于 Windows 运行时，而不是公共语言运行时 (CLR)。 对象引用或乘幂号 (^) 修饰符是这种新语法的一个重要部分，因为它支持以引用计数的方式自动删除运行时对象。 无需调用 **AddRef** 和 **Release** 等方法来管理 Windows 运行时对象的生命周期，运行时在没有其他组件引用该对象时删除它，例如在它离开范围或你将所有引用设置为 **nullptr** 时。 使用 Visual C++ 创建 UWP 应用的另一个重要部分是 **ref new** 关键字。 使用 **ref new** 而不是 **new** 来创建引用计数的 Windows 运行时对象。 有关详细信息，请参阅[类型系统 (C++/CX)](https://msdn.microsoft.com/library/windows/apps/hh755822)。

> **重要提示**  
当创建 Windows 运行时对象或创建 Windows 运行时组件时，只能使用 **^** 和 **ref new**。 当编写不使用 Windows 运行时的核心应用程序代码时，可以使用标准 C++ 语法。

Marble Maze 结合使用 **^** 和 [**Microsoft::WRL::ComPtr**](https://msdn.microsoft.com/library/windows/apps/br244983.aspx) 来管理堆分配的对象并且最大程度减少内存泄露。 我们建议你使用 ^ 来管理 Windows 运行时变量的生命周期、使用 **ComPtr** 来管理 COM 变量的生命周期（如使用 DirectX 时），以及使用 std::[**std::shared\_ptr**](https://msdn.microsoft.com/library/windows/apps/bb982026) 或 [**std::unique\_ptr**](https://msdn.microsoft.com/library/windows/apps/ee410601) 管理所有其他堆分配的 C++ 对象的生命周期。

 

有关可用于 UWP 应用的语言扩展的详细信息，请参阅 [Visual C++ 语言参考 (C++/CX)](https://msdn.microsoft.com/library/windows/apps/hh699871)。

###  错误处理

Marble Maze 使用异常处理作为处理意外错误的主要方式。 尽管游戏代码在传统上使用日志记录或错误代码（例如 **HRESULT** 值）来表示错误，但异常处理有两个主要优势。 首先，它可以使代码更易于读取和维护。 从代码角度讲，错误处理可将错误更方便地传播到可以处理该错误的例程。 使用错误代码通常需要每个函数明确传播错误。 第二个优势是，可以将 Visual Studio 调试器配置为在发生异常时中断，以便你可以在该错误的位置和上下文处立即停下。 Windows 运行时也广泛使用了异常处理。 因此，通过在你的代码中使用异常处理，你可以将所有错误处理组合到一个模型中。

我们建议在错误处理模型中使用以下约定：

-   使用异常传播意外错误。
-   不使用异常控制代码流。
-   仅捕获你可以安全处理且可以恢复的异常。 否则，不要捕获异常并允许应用终止。
-   当调用返回 **HRESULT** 的 DirectX 例程时，请使用 **DX::ThrowIfFailed** 函数。 此函数在 DirectXSample.h 中定义。如果所提供的 **HRESULT** 是一个错误代码，则 **ThrowIfFailed** 将引发异常。 例如，**E\_POINTER** 会导致 **ThrowIfFailed** 引发 [**Platform::NullReferenceException**](https://msdn.microsoft.com/library/windows/apps/hh755823.aspx)。

    使用 **ThrowIfFailed** 时，将 DirectX 调用放在单独一行，以帮助改善代码可读性，如下面的示例所示。

    ```cpp
    // Identify the physical adapter (GPU or card) this device is running on.
    ComPtr<IDXGIAdapter> dxgiAdapter;
    DX::ThrowIfFailed(
        dxgiDevice->GetAdapter(&dxgiAdapter)
        );
    ```

-   尽管我们建议避免使用 **HRESULT** 处理意外错误，但更重要的是避免使用异常处理来控制代码流。 因此，最好在必要时使用 **HRESULT** 返回值来控制代码流。

###  SAL 注释

结合使用 SAL 注释和代码分析工具来帮助发现应用中的错误。

通过使用 Microsoft 源代码注释语言 (SAL)，你可注释或描述函数使用其参数的方式。 SAL 注释也可描述返回值。 SAL 注释可与 C/C++ 代码分析工具一起发现 C 和 C++ 源代码中的可能缺陷。 该工具报告的常见编码错误包括：缓冲区溢出、未初始化的内存、null 指针取消引用，以及内存和资源泄漏。

考虑 **BasicLoader::LoadMesh** 方法，它在 BasicLoader.h 中声明。 此方法使用 \_In\_ 指定 *filename* 是一个输入参数（因此仅用于读取），使用 \_Out\_ 指定 *vertexBuffer* 和 *indexBuffer* 是输出参数（因此仅用于写入），以及使用 \_Out\_opt\_ 指定 *vertexCount* 和 *indexCount* 是可选的输出参数（可写入）。 因为 *vertexCount* 和 *indexCount* 是可选的输出参数，所以允许它们使用 **nullptr**。 C/C++ 代码分析工具检查对此方法的调用，以确保它传递的参数满足这些条件。

```cpp
void LoadMesh(
    _In_ Platform::String^ filename,
    _Out_ ID3D11Buffer** vertexBuffer,
    _Out_ ID3D11Buffer** indexBuffer,
    _Out_opt_ uint32* vertexCount,
    _Out_opt_ uint32* indexCount
    );
```

若要在应用上执行代码分析，在菜单栏上选择“生成”、“对解决方案运行代码分析”****。 有关代码分析的详细信息，请参阅[使用代码分析分析 C/C++ 代码质量](https://msdn.microsoft.com/library/windows/apps/ms182025.aspx)。

可用注释的完整列表在 sal.h 中定义。 有关详细信息，请参阅 [SAL 注释](https://msdn.microsoft.com/library/windows/apps/ms235402.aspx)。

## 后续步骤


有关如何构建 Marble Maze 应用程序代码，以及 DirectX UWP 应用的结构与传统桌面应用程序有何不同的信息，请参阅 [Marble Maze 应用程序结构](marble-maze-application-structure.md)。

## 相关主题


* [Marble Maze 应用程序结构](marble-maze-application-structure.md)
* [开发 Marble Maze，一款使用 C++ 和 DirectX 的 UWP 游戏](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)

 

 







<!--HONumber=Jun16_HO4-->


