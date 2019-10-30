---
title: 在通用 Windows 平台 (UWP) 应用上运行按配置优化 (PGO)
description: 将按配置优化（PGO）应用到通用 Windows 平台（UWP）应用的循序渐进指南。
ms.date: 02/08/2017
ms.localizationpriority: medium
ms.topic: article
ms.openlocfilehash: c784812d2e070aba0857cb84e5729b1426717b8d
ms.sourcegitcommit: 05be6929cd380a9dd241cc1298fd53f11c93d774
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2019
ms.locfileid: "73062370"
---
# <a name="running-profile-guided-optimization-on-universal-windows-platform-apps"></a>在通用 Windows 平台应用上运行按配置优化 
 
本主题提供将按配置优化 (PGO) 应用到通用 Windows 平台 (UWP) 应用的分步指南。 并非适用于经典 win32 应用程序的所有步骤都适用于 UWP 应用，因此我们的目标是介绍合并 PGO 以使优化更轻松并且更易于供 UWP 开发人员访问所必需的过程。

以下是使用 Visual Studio 2015 更新 3 将 PGO 应用到默认 DirectX 11 应用 (UWP) 模板的基本演练。
 
本指南中的屏幕截图基于以下新项目： !["新建项目" 对话框](images/pgo-001.png)

若要将 PGO 应用到 DirectX 11 应用模板：

1. 将你的解决方案配置设置为“发布”，或选择将生成旨在发布的优化代码的解决方案配置。 尽管理论上你可以在调试版本上运行 PGO，但使用 PGO 优化本来不优化的版本将降低效率。 
 
 ![App1 窗口](images/pgo-002.png)
 
2. 在项目的属性（“属性” > “C/C++” > “优化”）中验证你是否正在使用**全程序优化**的 /GL 标志进行生成（这可能已由你的配置设置）。

 ![全程序优化](images/pgo-003.png)

3. 转到链接器属性（“属性” > “链接器” > “优化），然后将“链接时间代码生成设置为“按配置优化 - 检测 (LTCG:PGInstrument)”。
 
 ![链接时间代码生成](images/pgo-004.png)

4. 选择“生成解决方案”，然后选择“部署解决方案”。 

 ![“新建项目”对话框](images/pgo-005.png)
 
 你可以双重确认一切是否正常工作，方法是查看生成输出位置并验证已生成一个 .pgd 文件。 在此示例的情况中，这意味着以下文件与生成输出一起生成。
 
 `C:\Users\<USER>\Documents\Visual Studio 2015\Projects\App1\Release\App1\App1.pgd`

 默认情况下，该 .pgd 文件具有与你的可执行文件相同的名称。 你还可以通过更改“按配置优化数据库”链接器选项来更改所生成的 .pgd 文件的名称。 
 
5. 导航到你的 Visual Studio VC 二进制文件目录（默认情况下，这看起来类似于 `C:\Program Files (x86)\Microsoft Visual Studio 14.0\VC\bin`）。 对于 x86 可执行文件，复制 `pgort140.dll`；对于 x64 可执行文件，从 `amd64\pgort140.dll` 复制 x64 版本。 将 `pgort140.dll` 的相应版本粘贴到已部署程序包的根目录中。 对于此示例，路径为：

 `C:\Users\<USER>\Documents\Visual Studio 2015\Projects\App1\Release\App1\AppX\`

 此步骤是必需的，因为 UWP 应用只能加载存在于它们的程序包内的库。

 ![“新建项目”对话框](images/pgo-006.png)
 
6. 从“开始”菜单或从带有“开始执行(不调试)”选项的 Visual Studio“调试”菜单运行应用。 

 ![“新建项目”对话框](images/pgo-007.png)
 
7. 现在正在运行的版本已经过检测，并且正在生成 PGO 数据。 此时，你应该使应用程序运行某些你打算优化的最常见方案。 在程序运行完所需的方案后，查找位于你找到 `pgort140.dll` 相应版本的相同文件夹中的 pgosweep.exe 工具。 此外，Visual Studio (x86/x64) 本地工具命令提示将已经在其路径中具有相应的版本。 若要收集 PGO 数据，请在应用程序仍然在运行时运行以下命令，以生成将包含分析数据的 .pgc 文件：
 
  `pgosweep.exe <executable name> <output file>` 
 
  你还可以查看 pgosweep.exe 帮助 (`pgosweep.exe /help`) 以查看用于控制你收集 PGO 数据的方式的其他可选参数。
 
  将 .pgc 文件输出到 .pgd 所在的生成位置并将这些文件命名为 `<PGDName>!<RunIdentifier>.pgc` 是一个好主意。 对于此示例，这意味着：
 
  ```
  pgosweep.exe App1.exe "C:\Users\<USER>\Documents\Visual Studio 2015\Projects\App1\Release\App1\App1!1.pgc"
  ```
 
  进一步的收集也可以是 `App1!CoreScenario.pgc`、`App1!UseCase5.pgc` 等。如果这些 .pgc 文件以此方式命名，并且与 .pgd 一起位于生成输出位置，当在步骤 9 中链接时它们将自动合并。
 
8. OPTIONAL：默认情况下，按步骤 7 中指定方式命名并放置在.pgd 旁边的所有 .pgc 文件都将在链接时合并，并且权重相等，但你也可以对具体运行如何加权具有更大的控制权。 若要执行此操作，你将使用同样位于你首次找到 `pgort140.dll` 副本的相同文件夹中的 **pgomgr.exe** 工具。 例如，若要使用其他运行 3 倍的优先级合并 `CoreScenario` 运行，我可以使用以下命令。
 
 ```
 pgomgr.exe -merge:3 "C:\Users\<USER>\Documents\Visual Studio 2015\Projects\App1\Release\App1\App1!CoreScenario.pgc" "C:\Users\<USER>\Documents\Visual Studio 2015\Projects\App1\Release\App1\App1.pgd"
 ```
 
9. 在你生成一个或多个 pgc 文件并将它们放置在 .pgd 旁边或手动合并它们（步骤 8）后，我们现在可以使用链接器创建最终优化版本。 转回到你的链接器属性（“属性” > “链接器” > “优化”）并将“链接时间代码生成”设置为“按配置优化 - 优化 (LTCG:PGOptimize)”，然后验证“按配置优化数据库”是否正指向你打算使用的 .pgd（如果你未对此进行更改，一切都应按顺序进行）。

 ![“新建项目”对话框](images/pgo-009.png)
 
10. 现在，当项目生成时，链接器将调用 pgomgr.exe 以将所有 `<PGDName>!*.pgc` 文件以默认权重 1 合并到 .pgd 中，生成的应用程序将基于分析数据进行优化。

## <a name="see-also"></a>另请参阅
- [性能](performance-and-xaml-ui.md)

 

