---
title: 有关在 Windows 上使用 Python 的常见问题解答
description: 有关在 Windows 上使用 Python 的常见问题解答
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: python, windows 10, microsoft, pip, py.exe, file paths, PYTHONPATH, python deployment, python packaging
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: 6dbf86e0f9435e44140159ebb2bcbc3d67928999
ms.sourcegitcommit: c8634b15b10bd196e7e2f876ae26e1205e160c91
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/29/2019
ms.locfileid: "74663554"
---
# <a name="frequently-asked-questions-about-using-python-on-windows"></a>有关在 Windows 上使用 Python 的常见问题解答

## <a name="why-cant-i-pip-install-a-certain-package"></a>为什么我不能“pip 安装”某些包？

安装失败的原因有很多 - 在大多数情况下，正确的解决方案是联系包开发人员。

出现问题的最常见原因是尝试将包安装到你无权修改的位置。 例如，默认的安装位置可能需要管理权限，但是默认情况下，Python 没有管理权限。 最佳解决方案是创建一个虚拟环境并在其中进行安装。

某些包包括本机代码，需要 C 或 C++ 编译器才能进行安装。 一般来说，包开发人员应发布预编译的版本，但通常没有发布。 如果[安装了适用于 Visual Studio 的生成工具](https://visualstudio.microsoft.com/downloads/#build-tools-for-visual-studio-2019)并选择了 C++ 选项，则某些包可能会正常运行，但是在大多数情况下，需要联系包开发人员。

[请关注有关 StackOverflow 的讨论](https://stackoverflow.com/questions/4750806/how-do-i-install-pip-on-windows/12476379)。

## <a name="what-is-pyexe"></a>什么是 py.exe？

由于要处理不同类型的 Python 项目，因此最终可能会在计算机上安装多个版本的 Python。 由于所有这些版本都使用 `python` 命令，因此你使用的是哪个版本的 Python 可能并不明显。 作为标准，建议使用 `python3` 命令（或 `python3.7` 以选择特定版本）。

[py.exe 启动器](https://docs.python.org/3/using/windows.html#launcher)将自动选择已安装的最新版本的 Python。 此外，还可以使用 `py -3.7` 之类的命令来选择特定版本，或者使用 `py --list` 来查看可使用的版本。 但是，仅当使用从 [python.org](https://www.python.org/downloads/windows/) 安装的 Python 版本时，py.exe 启动器才会正常运行  。从 Microsoft Store 安装 Python 时，不包含 `py` 命令  。 对于 Linux、macOS、WSL 和 Microsoft Store 版本的 Python，应使用 `python3`（或 `python3.7`）命令。

## <a name="why-does-running-pythonexe-open-the-microsoft-store"></a>为什么运行 python.exe 会打开 Microsoft Store？

为了帮助新用户找到正确的 Python 安装，我们向 Windows 添加了一个快捷方式，可直接转到 Microsoft Store 中发布的最新版本的社区包。 该包无需管理员权限即可轻松安装，并将默认的 `python` 和 `python3` 命令替换相应的真实命令。

使用任何命令行参数运行快捷方式可执行文件都将返回错误代码，指示未安装 Python。 这是为了防止批处理文件和脚本意外打开 Store 应用。

如果使用 [python.org](https://www.python.org/downloads/windows/) 中的安装程序安装 Python 并选择“添加到 PATH”选项，则新的 `python` 命令将优先于快捷方式。 请注意，其他安装程序可能以低于内置快捷方式的优先级添加 `python`  。

通过从“开始”打开“管理应用执行别名”，找到“应用安装程序”Python 条目并将其切换为“关闭”，无需安装 Python 即可禁用快捷方式。

## <a name="why-dont-file-paths-work-in-python-when-i-copy-paste-them"></a>当我复制粘贴文件路径时，为什么在 Python 中不起作用？

Python 字符串对特殊字符使用“转义符”。 例如，要在字符串中插入换行符，应键入 `\n`。 由于 Windows 上的文件路径使用反斜杠，因此某些部分可能已转换为特殊字符。

要将路径粘贴为 Python 中的字符串，请添加 `r` 前缀。 这表示它是一个 `raw` 字符串，除 \” 外，将不使用任何转义字符（可能需要删除路径中的最后一个反斜杠）。 因此，路径可能如下所示：`r"C:\Users\MyName\Documents\Document.txt"`

在 Python 中使用路径时，建议使用标准 pathlib 模块。 这样你就可以将字符串转换为丰富的 Path 对象，无论它使用正斜杠还是反斜杠，都可以一致地进行路径操作，从而使代码在不同的操作系统上可以更好地工作。

## <a name="what-is-pythonpath"></a>什么是 PYTHONPATH？

Python 使用 PYTHONPATH 环境变量来指定可以从中导入模块的目录列表。 运行时，可以检查 `sys.path` 变量以查看导入某些内容时将要搜索的目录。

要在“命令提示符”中设置此变量，请使用：`set PYTHONPATH=list;of;paths`。

要在 PowerShell 中设置此变量，请在启动 Python 之前使用：`$env:PYTHONPATH=’list;of;paths’`。

不建议通过“环境变量”设置全局设置此变量，因为使用它的可能是任何版本的 Python，而非要使用的版本   。

## <a name="where-can-i-find-help-with-packaging-and-deployment"></a>何处可以找到有关打包和部署的帮助？

[Docker](https://code.visualstudio.com/docs/azure/docker)：[VSCode 扩展](https://code.visualstudio.com/docs/azure/docker)有助于快速打包和部署 Dockerfile 和 docker-compose.yml 模板（为项目生成正确的 Docker 文件）。

借助 [Azure Kubernetes 服务 (AKS)](https://docs.microsoft.com/azure/aks/)，可以在按需缩放资源的同时部署和管理容器化应用程序。

## <a name="what-if-i-need-to-work-across-different-machines"></a>如果需要在不同的计算机上工作，该怎么办？

通过[设置同步](https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync)，可以使用 GitHub 在不同安装之间同步 VS Code 设置。 如果在不同的计算机上工作，这有助于在它们之间保持一致的环境。

## <a name="what-if-im-used-to-using-pycharm-atom-sublime-text-emacs-or-vim"></a>如果我习惯使用 PyCharm、Atom、Sublime Text、Emacs 或 Vim，该怎么办？

VSCode 扩展[键映射](https://marketplace.visualstudio.com/search?target=VSCode&category=Keymaps&sortBy=Downloads)有助于打造你熟悉的环境。

## <a name="how-do-mac-shortcut-keys-map-to-windows-shortcut-keys"></a>如何将 Mac 快捷键映射到 Windows 快捷键？

Windows 计算机和 Macintosh 的某些键盘按钮和系统快捷方式略有不同。 此 [Mac 到 Windows 的转换指南](../dev-environment/mac-to-windows.md)涵盖了基础知识。
