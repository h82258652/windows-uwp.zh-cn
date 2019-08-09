---
title: 有关在 Windows 上使用 Python 的常见问题
description: 有关在 Windows 上使用 Python 的常见问题
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
ms.prod: windows
ms.technology: hub
keywords: python, windows 10, microsoft, pip, py, 文件路径, PYTHONPATH, python 部署, python 打包
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: d944e16dc96f78efdece715778a13cd3fb2d9dbd
ms.sourcegitcommit: 789bfe3756c5c47f7324b96f482af636d12c0ed3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/09/2019
ms.locfileid: "68867350"
---
# <a name="frequently-asked-questions-about-using-python-on-windows"></a>有关在 Windows 上使用 Python 的常见问题

## <a name="why-cant-i-pip-install-a-certain-package"></a>为什么无法 "pip 安装" 某个包？

安装失败的原因有很多--在大多数情况下, 正确的解决方案是与包开发人员联系。

出现问题的最常见原因是尝试将安装到没有修改权限的位置。 例如, 默认安装位置可能需要管理权限, 但默认情况下, Python 不会有这样的安装。 最佳解决方案是创建虚拟环境并安装该环境。

某些包包含需要 C 或C++编译器才能安装的本机代码。 通常, 包开发人员应发布预编译的版本, 但通常不应这样做。 如果你[安装了 Visual Studio 的生成工具](https://visualstudio.microsoft.com/downloads/#build-tools-for-visual-studio-2019)并选择了C++选项, 则其中一些包可能会起作用, 但在大多数情况下, 你需要联系包开发人员。

[请参阅 StackOverflow 上的讨论](https://stackoverflow.com/questions/4750806/how-do-i-install-pip-on-windows/12476379)。

## <a name="what-is-pyexe"></a>什么是 py？

你可能会在计算机上安装了多个版本的 Python, 因为你正在使用不同类型的 Python 项目。 由于这些都使用`python`命令, 因此可能不是您所使用的 Python 的版本。 作为标准, 建议使用`python3`命令 (或`python3.7`选择特定的版本)。

[Py 启动器](https://docs.python.org/3/using/windows.html#launcher)会自动选择已安装的最新版本的 Python。 你还可以使用命令 ( `py -3.7`如) 选择特定版本, 或`py --list`查看可以使用的版本。 **但是**, 仅当使用从[Python.org](https://www.python.org/downloads/windows/)安装的 Python 版本时, py 启动器才能工作。从 Microsoft Store 安装 Python 时, `py` **不包含**命令。 对于 Linux、macOS、WSL 和 Microsoft Store 版本的 Python, 你应使用`python3` (或`python3.7`) 命令。

## <a name="why-does-running-pythonexe-open-the-microsoft-store"></a>为什么运行 python 会打开 Microsoft Store？

为了帮助新用户找到正确的 Python 安装, 我们添加了一个 Windows 快捷方式, 将直接转到 Microsoft Store 中发布的最新社区包版本。 此包可以轻松安装, 无需管理员权限, 并且会将默认值`python`和`python3`命令替换为实际的。

使用任意命令行参数运行快捷可执行文件将返回一个错误代码, 指示未安装 Python。 这是为了防止批处理文件和脚本在可能不适用时打开应用商店应用。

如果使用[python.org](https://www.python.org/downloads/windows/)的安装程序安装 Python, 并选择 "添加到路径" 选项, 则新`python`命令将优先于快捷方式。 请注意, 其他安装程序`python`的优先级可能低于内置快捷方式。

你可以禁用快捷方式, 而无需安装 Python, 只需打开 "管理应用执行别名" 即可, 查找 "应用安装程序" Python 条目并将其切换为 "关闭"。

## <a name="why-dont-file-paths-work-in-python-when-i-copy-paste-them"></a>为什么在复制粘贴时文件路径不能在 Python 中使用？

Python 字符串对特殊字符使用 "转义"。 例如, 若要将新的行字符插入字符串中, 请键入`\n`。 由于 Windows 上的文件路径使用反斜杠, 因此某些部分可能会转换为特殊字符。

若要在 Python 中将路径粘贴为字符串, 请`r`添加前缀。 这表示它是一个`raw`字符串, 不会使用除 "\" 之外的任何转义符 (可能需要删除路径中的最后一个反斜杠)。 因此, 路径可能如下所示: r "C:\Users\MyName\Documents\Document.txt"

在 Python 中使用路径时, 建议使用标准 pathlib 模块。 这使你可以将字符串转换为可执行路径操作的丰富路径对象, 而不管它是否使用正斜杠或反斜杠, 使你的代码在不同的操作系统上的工作效果更佳。

## <a name="what-is-pythonpath"></a>什么是 PYTHONPATH？

Python 使用 PYTHONPATH 环境变量来指定可从中导入模块的目录列表。 运行时, 可以检查`sys.path`变量以查看导入内容时要搜索的目录。

若要从命令提示符设置此变量, 请使用`set PYTHONPATH=list;of;paths`:。

若要从 PowerShell 设置此变量, 请`$env:PYTHONPATH=’list;of;paths’`在启动 Python 之前使用:。

**不**建议通过**环境变量**设置全局设置此变量, 因为它可能由任何版本的 Python 使用, 而不是你打算使用的。

## <a name="where-can-i-find-help-with-packaging-and-deployment"></a>在哪里可以找到有关打包和部署的帮助？

[Docker](https://code.visualstudio.com/docs/azure/docker):[VSCode 扩展](https://code.visualstudio.com/docs/azure/docker)可帮助你通过 Dockerfile 和 docker-compose.yml docker-compose.override.yml 模板快速打包和部署 (为你的项目生成正确的 docker 文件)。

[Azure Kubernetes Service (AKS)](https://docs.microsoft.com/azure/aks/)可用于部署和管理容器化应用程序, 同时按需缩放资源。

## <a name="what-if-i-need-to-work-across-different-machines"></a>如果需要在不同的计算机上工作, 该怎么办？

[设置同步](https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync)可让你在使用 GitHub 的不同安装之间同步 VS Code 设置。 如果您在不同的计算机上工作, 这有助于使您的环境在它们之间保持一致。

## <a name="what-if-im-used-to-using-pycharm-atom-sublime-text-emacs-or-vim"></a>如果我使用的是 PyCharm、Atom、Sublime Text、Emacs 或 Vim, 该怎么办？

VSCode 扩展[键映射](https://marketplace.visualstudio.com/search?target=VSCode&category=Keymaps&sortBy=Downloads)可以帮助你的环境在家中轻松地感觉。

## <a name="how-do-mac-shortcut-keys-map-to-windows-shortcut-keys"></a>Mac 快捷键如何映射到 Windows 快捷键？

某些键盘按钮和系统快捷方式在 Windows 计算机和 Macintosh 之间略有不同。 Microsoft 支持部门的此[键盘映射指南](https://support.microsoft.com/help/970299/keyboard-mappings-using-a-pc-keyboard-on-a-macintosh)涵盖了基础知识。
