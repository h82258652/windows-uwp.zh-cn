---
author: laurenhughes
title: 用资产包和包折叠进行开发
description: 了解如何使用资产包和包折叠高效组织你的应用。
ms.author: lahugh
ms.date: 04/30/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, 打包, 资产包布局, 资产包
ms.localizationpriority: medium
ms.openlocfilehash: 31c27430c850f861c8b97863521202a6dcab80f7
ms.sourcegitcommit: 82c3fc0b06ad490c3456ad18180a6b23ecd9c1a7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2018
ms.locfileid: "5475486"
---
# <a name="developing-with-asset-packages-and-package-folding"></a>用资产包和包折叠进行开发 

> [!IMPORTANT]
> 如果你打算将应用提交到 Microsoft Store，你需要联系 [Windows 开发人员支持](https://developer.microsoft.com/windows/support)并获得批准才能使用资产包和包折叠。

资产包可以减小总体包大小，并缩短将应用发布到 Microsoft Store 的时长。 有关资产包及其如何加快开发迭代速度的详细信息，请参阅[资产包简介](asset-packages.md)。

如果你正在考虑为应用使用资产包，或者已经确定要使用资产包，则你可能想了解资产包会给你的开发流程带来哪些改变。 简要地说，由于资产包具有包折叠特性，你的应用开发流程将保持不变。

## <a name="file-access-after-splitting-your-app"></a>拆分应用后的文件访问

要明白包折叠为什么不会影响你的开发流程，我们首先需要知道当你将应用拆分成多个包（资产包或资源包）时会发生什么情况。 

从高层次上看，将应用的部分文件拆分到其他包（非体系结构程序包）后，你将无法直接访问那些相对于你的代码运行位置的文件。 原因在于这些软件包跟你的体系结构程序包安装到了不同的目录。 例如，如果你进行游戏，你的游戏已经本地化为法语和德语。 你打算为 x86 和 x64 计算机构建，则你应该有你的游戏的应用程序包中的这些应用包文件：

-   MyGame_1.0_x86.appx
-   MyGame_1.0_x64.appx
-   MyGame_1.0_language-fr.appx
-   MyGame_1.0_language-de.appx

当你的游戏安装到用户的计算机中时，每个应用包文件将具有其自己的**WindowsApps**目录中的文件夹。 因此，对于运行 64 位 Windows 的法国用户，你的游戏将如下所示：

```example
C:\Program Files\WindowsApps\
|-- MyGame_1.0_x64
|   `-- …
|-- MyGame_1.0_language-fr
|   `-- …
`-- …(other apps)
```

请注意，并非适用于用户的文件不会在应用包安装 （x86 程序包和德语语言包）。 

对于该用户，你的游戏的主可执行文件将位于 **MyGame_1.0_x64** 文件夹中，它将从该文件夹运行，并且通常只能访问该文件夹中的文件。 要访问 **MyGame_1.0_language-fr** 文件夹中的文件，你必须使用 MRT API 或 PackageManager API。 MRT Api 能够从安装的语言自动选择最合适的文件，你可以了解有关[windows.applicationmodel.resources.core](https://docs.microsoft.com/uwp/api/windows.applicationmodel.resources.core)MRT Api 的详细信息。 或者，你也可以使用 [PackageManager 类](https://docs.microsoft.com/uwp/api/Windows.Management.Deployment.PackageManager)找到法语语言包的安装位置。 你不应该对应用程序包的安装位置做出任何假设，因为安装位置可能会发生变化，并且可能因用户而异。 

## <a name="asset-package-folding"></a>资产包折叠

如何访问资产包中的文件？ 你可以继续使用你正在用来访问体系结构程序包中任何其他文件的文件访问 API。 这是因为资产包文件在通过包折叠过程安装时会折叠到你的体系结构程序包中。 此外，由于资产包文件本来就应该是体系结构程序包中的文件，这意味着你在开发过程中从松散文件部署转换为打包部署时，不必更改所用的 API。 

为了更深入地了解包折叠的工作原理，我们来看一个示例。 假设你有一个游戏项目，其文件结构如下：

```example
MyGame
|-- Audios
|   |-- Level1
|   |   `-- ...
|   `-- Level2
|       `-- ...
|-- Videos
|   |-- Level1
|   |   `-- ...
|   `-- Level2
|       `-- ...
|-- Engine
|   `-- ...
|-- XboxLive
|   `-- ...
`-- Game.exe
```

如果你打算将游戏拆分成 3 个包：一个 x64 体系结构程序包、一个音频资产包和一个视频资产包。你的游戏将分成以下几个包：

```example
MyGame_1.0_x64.appx
|-- Engine
|   `-- ...
|-- XboxLive
|   `-- ...
`-- Game.exe
MyGame_1.0_Audios.appx
`-- Audios
    |-- Level1
    |   `-- ...
    `-- Level2
        `-- ...
MyGame_1.0_Videos.appx
`-- Videos
    |-- Level1
    |   `-- ...
    `-- Level2
        `-- ...
```

安装游戏时，将首先部署 x64 程序包。 然后，两个资产包仍会被部署到它们自己的文件夹，就像前面的示例 **MyGame_1.0_language-fr** 那样。 但是，由于包折叠，资产包文件还将硬链接以显示在 **MyGame_1.0_x64** 文件夹中。虽然这些文件在两个位置显示，但它们不会占用磁盘空间两次。 资产包文件显示的位置恰好是它们相对于程序包根目录的位置。 因此，部署后游戏的最终布局为：

```example 
C:\Program Files\WindowsApps\
|-- MyGame_1.0_x64
|   |-- Audios
|   |   |-- Level1
|   |   |   `-- ...
|   |   `-- Level2
|   |       `-- ...
|   |-- Videos
|   |   |-- Level1
|   |   |   `-- ...
|   |   `-- Level2
|   |       `-- ...
|   |-- Engine
|   |   `-- ...
|   |-- XboxLive
|   |   `-- ...
|   `-- Game.exe
|-- MyGame_1.0_Audios
|   `-- Audios
|       |-- Level1
|       |   `-- ...
|       `-- Level2
|           `-- ...
|-- MyGame_1.0_Videos
|   `-- Videos
|       |-- Level1
|       |   `-- ...
|       `-- Level2
|           `-- ...
`-- …(other apps)
```

为资产包使用包折叠时，你仍然可以按以前的方式访问已拆分到资产包中的文件（注意，体系结构文件夹具有与原始项目文件夹完全相同的结构），你可以添加资产包或在资产包之间移动文件而不对代码造成任何影响。 

下面，我们来看一个更复杂的包折叠示例。 假设你想要根据级别拆分文件，并且想要保留与原始项目文件夹相同的结构，则你的程序包应该如下所示：

```example
MyGame_1.0_x64.appx
|-- Engine
|   `-- ...
|-- XboxLive
|   `-- ...
`-- Game.exe
MyGame_Level1.appx
|-- Audios
|   `-- Level1
|       `-- ...
`-- Videos
    `-- Level1
        `-- ...

MyGame_Level2.appx
|-- Audios
|   `-- Level2
|       `-- ...
`-- Videos
    `-- Level2
        `-- ...
```
这将允许 **MyGame_Level1** 包中的 **Level1** 文件夹和文件与 **MyGame_Level2** 包中的 **Level2** 文件夹和文件在包折叠期间合并到 **Audios** 和 **Videos** 文件夹。 由此可见，作为一般规则，在映射文件中为打包文件指定的相对路径或 MakeAppx.exe 的[打包布局](packaging-layout.md)是你应在包折叠后用来访问它们的路径。 

最后，如果不同资产包中的两个文件具有相同的相对路径，将在包折叠期间导致冲突。 一旦发生冲突，应用部署将产生错误并失败。 此外，由于包折叠使用了硬链接，因此如果使用了资产包，你的应用将无法部署到非 NTFS 驱动器。 如果你知道用户可能会将你的应用移动到可移动驱动器，则不应该使用资产包。 


