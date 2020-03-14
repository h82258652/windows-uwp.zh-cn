---
title: 在 Windows 上使用 Python 的脚本和自动化
description: 如何开始在 Windows 上将 Python 用于脚本、自动化和系统管理。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: python, windows 10, microsoft, python 系统管理, python 文件自动化, windows 上的 python 脚本, 在 windows 上设置 python, windows 上的 python 开发人员环境, windows 上的 python 开发环境, python 与 powershell, 适用于文件系统任务的 python 脚本
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: d465d46a0524345a45dff9b1cc7c425e4cb468a4
ms.sourcegitcommit: ca1b5c3ab905ebc6a5b597145a762e2c170a0d1c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/13/2020
ms.locfileid: "79208992"
---
# <a name="get-started-using-python-on-windows-for-scripting-and-automation"></a>开始在 Windows 上将 Python 用于脚本和自动化

下面是有关在 Windows 上设置开发人员环境和开始将 Python 用于脚本和自动执行文件系统操作的分步指南。

> [!NOTE]
> 本文介绍如何通过以 Windows 为中心的方法设置环境以使用 Python 中的一些有用库，这些库可以跨平台自动执行任务（如搜索文件系统、访问 Internet、分析文件类型等）。 对于特定于 Windows 的操作，请查看 [ctypes](https://docs.python.org/3/library/ctypes.html)（适用于 Python 的兼容 C 的外部函数库）、[winreg](https://docs.python.org/3/library/winreg.html)（向 Python 公开 Windows 注册表 API 的函数）和 [Python/WinRT](https://pypi.org/project/winrt/)（可用于从 Python 访问 Windows 运行时 API）。

## <a name="set-up-your-development-environment"></a>设置开发环境

使用 Python 编写执行文件系统操作的脚本时，建议[从 Microsoft Store 安装 Python](https://www.microsoft.com/p/python-37/9nj46sx7x90p?activetab=pivot:overviewtab)。 通过 Microsoft Store 安装会使用基本 Python3 解释器，但会为当前用户设置 PATH 设置（避免需要管理员访问权限），以及提供自动更新。

如果在 Windows 上将 Python 用于 Web 开发  ，建议使用适用于 Linux 的 Windows 子系统进行不同设置。 在我们的指南中可找到演练：[开始在 Windows 上将 Python 用于 Web 开发](./web-frameworks.md)。 如果不熟悉 Python，请尝试我们的指南：[开始在 Windows 上使用 Python（初学者）](./beginners.md)。 对于某些高级方案（如需要访问/修改 Python 的已安装文件、创建二进制文件的副本或直接使用 Python DLL），可能需要考虑直接从 [python.org](https://www.python.org/downloads/) 下载特定 Python 版本或考虑安装[替代实现](https://www.python.org/download/alternatives)，如 Anaconda、Jython、PyPy、WinPython、IronPython 等。建议仅当你是更高级的 Python 程序员并且有特定原因需要选择替代实现时才使用此方法。

## <a name="install-python"></a>安装 Python

使用 Microsoft Store 安装 Python：

1. 转到“开始”  菜单（左下方 Windows 图标），输入“Microsoft Store”，选择用于打开应用商店的链接。

2. 应用商店打开后，从右上方菜单中选择“搜索”  ，然后输入“Python”。 从“应用”下的结果中打开“Python 3.7”。 选择“获取”  。

3. Python 完成下载和安装过程后，使用“开始”  菜单（左下方 Windows 图标）打开 Windows PowerShell。 PowerShell 打开后，输入 `Python --version` 以确认已在计算机上安装了 Python3。

4. Python 的 Microsoft Store 安装包括 pip  （标准包管理器）。 通过 pip 可以安装和管理不属于 Python 标准库的其他包。 若要确认还可使用 pip 安装和管理包，请输入 `pip --version`。

## <a name="install-visual-studio-code"></a>安装 Visual Studio Code

通过使用 VS Code 作为文本编辑器/集成开发环境 (IDE)，可以利用 [IntelliSense](https://code.visualstudio.com/docs/editor/intellisense)（代码完成辅助）、[Linting](https://code.visualstudio.com/docs/python/linting)（可帮助避免在代码中发生错误）、[调试支持](https://code.visualstudio.com/docs/python/debugging)（可帮助在运行代码之后查找代码中的错误）、[代码片段](https://code.visualstudio.com/docs/editor/userdefinedsnippets)（小型可重用代码块的模板）以及[单元测试](https://code.visualstudio.com/docs/python/unit-testing)（使用不同类型的输入测试代码的接口）。

下载适用于 Windows 的 VS Code，并按照安装说明进行操作：[https://code.visualstudio.com](https://code.visualstudio.com)。

## <a name="install-the-microsoft-python-extension"></a>安装 Microsoft Python 扩展

需要安装 Microsoft Python 扩展，才能利用 VS Code 支持功能。 [了解详细信息](https://code.visualstudio.com/docs/languages/python)。

1. 通过输入 Ctrl+Shift+X  来打开 VS Code 扩展窗口（或使用菜单导航到“视图”   > “扩展”  ）。

2. 在顶部的“在应用商店中搜索扩展”  框中，输入：Python  。

3. 找到“Python (ms-python.python) by Microsoft”  扩展，然后选择绿色的“安装”  按钮。

## <a name="open-the-integrated-powershell-terminal-in-vs-code"></a>在 VS Code 中打开集成 PowerShell 终端

VS Code 包含一个[内置终端](https://code.visualstudio.com/docs/editor/integrated-terminal)，使你可以使用 PowerShell 打开 Python 命令行，从而在代码编辑器与命令行之间建立无缝工作流。

1. 在 VS Code 中打开终端，选择“视图”   > “终端”  ，或者使用快捷方式 Ctrl+'  （使用反撇号字符）。

    > [!NOTE]
    > 默认终端应为 PowerShell，但如果需要更改它，请使用 Ctrl+Shift+P  进入命令面板。 输入“终端:  选择默认 Shell”，会显示终端选项列表，其中包含 PowerShell、命令提示符、WSL 等。选择要使用的终端，然后输入 Ctrl+Shift+'  （使用反撇号）以创建新终端。

2. 在 VS Code 终端中，通过输入以下内容来打开 Python：`python`

3. 通过输入以下内容来尝试使用 Python 解释器：`print("Hello World")`。 Python 会返回语句“Hello World”。

    ![VS Code 中的 Python 命令行](../images/python-in-vscode.png)

4. 若要退出 Python，可以输入 `exit()`、`quit()` 或选择 Ctrl-Z。

## <a name="install-git-optional"></a>安装 Git（可选）

如果计划与其他人协作处理 Python 代码，或是在开放源代码站点（例如 GitHub）上托管项目，则 VS Code 支持[使用 Git 进行版本控制](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support)。 VS Code 中的“源代码管理”选项卡可跟踪所有更改，并直接在 UI 中内置了常见 Git 命令（add、commit、push、pull）。 需要先安装 Git，以便为“源代码管理”面板提供支持。

1. 从 [git-scm 网站](https://git-scm.com/download/win)下载并安装适用于 Windows 的 Git。

2. 其中包含了一个安装向导，该向导会询问一系列有关 Git 安装设置的问题。 建议使用所有默认设置，除非有特定原因需要更改某些设置。

3. 如果以前从未使用过 Git，则 [GitHub 指南](https://guides.github.com/)可以帮助入门。

## <a name="example-script-to-display-the-structure-of-your-file-system-directory"></a>用于显示文件系统目录结构的示例脚本

常见系统管理任务可能需要花费大量时间，但是借助 Python 脚本，可以自动执行这些任务，从而完全不会占用任何时间。 例如，Python 可以读取计算机文件系统的内容并执行各种操作（如打印文件和目录的大纲、将文件夹从一个目录移动到另一个目录或是重命名上百个文件）。 通常，如果手动执行这类任务，则它们可能会占用大量时间。 请改用 Python 脚本！

我们从一个简单脚本开始，该脚本会遍历目录树并显示目录结构。

1. 使用“开始”  "菜单（左下方 Windows 图标）打开 PowerShell。

2. 为项目创建目录：`mkdir python-scripts`，然后打开该目录：`cd python-scripts`。

3. 创建一些目录以用于我们的示例脚本：

    ```powershell
    mkdir food, food\fruits, food\fruits\apples, food\fruits\oranges, food\vegetables
    ```

4. 在这些目录中创建一些文件以用于我们的脚本：

    ```powershell
    new-item food\fruits\banana.txt, food\fruits\strawberry.txt, food\fruits\blueberry.txt, food\fruits\apples\honeycrisp.txt, food\fruits\oranges\mandarin.txt, food\vegetables\carrot.txt
    ```

5. 在 python-scripts 目录中创建新 python 文件：

    ```powershell
    mkdir src
    new-item src\list-directory-contents.py
    ```

6. 通过输入以下内容在 VS Code 中打开项目：`code .`

7. 通过输入 Ctrl+Shift+E  打开 VS Code 文件资源管理器窗口（或使用菜单导航到“视图”   > “资源管理器”  ），然后选择刚刚创建的 list-directory-contents.py 文件。 Microsoft Python 扩展会自动加载 Python 解释器。 可以在 VS Code 窗口底部查看已加载的解释器。

    > [!NOTE]
    > Python 是一种解释型语言，这意味着它会充当虚拟机，从而模拟物理计算机。 可以使用不同类型的 Python 解释器：Python 2、Python 3、Anaconda、PyPy 等。若要运行 Python 代码并获取 Python IntelliSense，必须向 VS Code 告知要使用的解释器。 建议坚持使用 VS Code 默认选择的解释器（在我们的示例中为 Python 3），除非有特定原因需要选择其他解释器。 若要更改 Python 解释器，请选择当前显示在 VS Code 窗口底部蓝色栏中的解释器，或打开“命令面板”  (Ctrl+Shift+P)，然后输入命令“Python:  选择解释器”。 这会显示当前已安装的 Python 解释器列表。 [详细了解如何配置 Python 环境](https://code.visualstudio.com/docs/python/environments)。

    ![在 VS Code 中选择 Python 解释器](../images/interpreterselection.gif)

8. 将以下代码粘贴到 list-directory-contents.py 文件中，然后选择“保存”  ：

    ```python
    import os

    root = os.path.join('..', 'food')
    for directory, subdir_list, file_list in os.walk(root):
        print('Directory:', directory)
        for name in subdir_list:
            print('Subdirectory:', name)
        for name in file_list:
            print('File:', name)
        print()
    ```

9. 打开 VS Code 集成终端（Ctrl+'  ，使用反撇号字符），然后进入刚刚在其中保存 Python 脚本的 src 目录：

    ```powershell
    cd src
    ```

10. 使用以下命令在 PowerShell 中运行脚本：

    ```powershell
    python3 .\list-directory-contents.py
    ```

    你应会看到如下所示的输出：

    ```powershell
    Directory: ..\food
    Subdirectory: fruits
    Subdirectory: vegetables

    Directory: ..\food\fruits
    Subdirectory: apples
    Subdirectory: oranges
    File: banana.txt
    File: blueberry.txt
    File: strawberry.txt

    Directory: ..\food\fruits\apples
    File: honeycrisp.txt

    Directory: ..\food\fruits\oranges
    File: mandarin.txt

    Directory: ..\food\vegetables
    File: carrot.txt
    ```

11. 通过直接在 PowerShell 终端中输入以下命令，使用 Python 将文件系统目录输出打印到自己的文本文件中：`python3 list-directory-contents.py > food-directory.txt`

恭喜！ 你刚刚编写了一个自动系统管理脚本，该脚本会读取你创建的目录和文件，并使用 Python 显示目录结构并打印到自己的文本文件中。

## <a name="example-script-to-modify-all-files-in-a-directory"></a>用于修改目录中的所有文件的示例脚本

此示例使用刚刚创建的文件和目录，通过将文件的上次修改日期添加到文件名开头来重命名每个文件。

1. 在 python-scripts  目录内的 src  文件夹中，为脚本创建新 Python 文件：

    ```powershell
    new-item update-filenames.py
    ```

2. 打开 update-filenames.py 文件，将以下代码粘贴到该文件中，并保存它：

    > [!NOTE]
    > os.getmtime 会返回以时钟周期表示的时间戳（这不容易理解）。 它必须先转换为标准日期时间字符串。

    ```python
    import datetime
    import os

    root = os.path.join('..', 'food')
    for directory, subdir_list, file_list in os.walk(root):
        for name in file_list:
            source_name = os.path.join(directory, name)
            timestamp = os.path.getmtime(source_name)
            modified_date = str(datetime.datetime.fromtimestamp(timestamp)).replace(':', '.')
            target_name = os.path.join(directory, f'{modified_date}_{name}')

            print(f'Renaming: {source_name} to: {target_name}')

            os.rename(source_name, target_name)
    ```

3. 测试 update-filenames.py 脚本，具体方式是运行它：`python3 update-filenames.py`，然后再次运行 list-directory-contents.py 脚本：`python3 list-directory-contents.py`

4. 你应会看到如下所示的输出：

    ```powershell
    Renaming: ..\food\fruits\banana.txt to: ..\food\fruits\2019-07-18 12.24.46.385185_banana.txt
    Renaming: ..\food\fruits\blueberry.txt to: ..\food\fruits\2019-07-18 12.24.46.391170_blueberry.txt
    Renaming: ..\food\fruits\strawberry.txt to: ..\food\fruits\2019-07-18 12.24.46.389174_strawberry.txt
    Renaming: ..\food\fruits\apples\honeycrisp.txt to: ..\food\fruits\apples\2019-07-18 12.24.46.395160_honeycrisp.txt
    Renaming: ..\food\fruits\oranges\mandarin.txt to: ..\food\fruits\oranges\2019-07-18 12.24.46.398151_mandarin.txt
    Renaming: ..\food\vegetables\carrot.txt to: ..\food\vegetables\2019-07-18 12.24.46.402496_carrot.txt

    PS C:\src\python-scripting\src> python3 .\list-directory-contents.py
    ..\food\
    Directory: ..\food
    Subdirectory: fruits
    Subdirectory: vegetables

    Directory: ..\food\fruits
    Subdirectory: apples
    Subdirectory: oranges
    File: 2019-07-18 12.24.46.385185_banana.txt
    File: 2019-07-18 12.24.46.389174_strawberry.txt
    File: 2019-07-18 12.24.46.391170_blueberry.txt

    Directory: ..\food\fruits\apples
    File: 2019-07-18 12.24.46.395160_honeycrisp.txt

    Directory: ..\food\fruits\oranges
    File: 2019-07-18 12.24.46.398151_mandarin.txt

    Directory: ..\food\vegetables
    File: 2019-07-18 12.24.46.402496_carrot.txt

    ```

5. 通过直接在 PowerShell 终端中输入以下命令，使用 Python 将前面追加了最近修改时间戳的新文件系统目录名称打印到自己的文本文件中：`python3 list-directory-contents.py > food-directory-last-modified.txt`

希望你了解了一些有关使用 Python 脚本自动执行基本系统管理任务的有趣内容。 当然，还有很多内容需要知道，但我们希望可以使你有一个良好的开端。 我们在下面共享了几个其他资源，可用于继续学习。

## <a name="additional-resources"></a>其他资源

- [Python 文档：文件和目录访问权限](https://docs.python.org/3.7/library/filesys.html)：有关处理文件系统以及使用模块读取文件属性、以可移植方式操作路径和创建临时文件的 Python 文档。
- [了解 Python：String_Formatting 教程](https://www.learnpython.org/en/String_Formatting)：有关使用“%”运算符进行字符串格式设置的更多信息。
- [应了解的 10 个 Python 文件系统方法](https://towardsdatascience.com/10-python-file-system-methods-you-should-know-799f90ef13c2)：有关使用 `os` 和 `shutil` 操作文件和文件夹的中级文章。
- [Python 漫游指南：系统管理](https://docs.python-guide.org/scenarios/admin/)：一个“有针对性的指南”，提供有关 Python 相关主题的概述和最佳做法。 此部分介绍系统管理工具和框架。 本指南位于 GitHub 上，因此可以提出问题并做出贡献。
