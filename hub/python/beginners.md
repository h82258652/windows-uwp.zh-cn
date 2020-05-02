---
title: 开始在 Windows 上使用 Python（初学者）
description: 一个指南，可在你刚开始接触在 Windows 上使用 Python 时帮助你入门。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: python, windows 10, microsoft, 了解 python, windows 上的 python（初学者）, 使用 microsoft store 安装 python, python 与 vs code, windows 上的 pygame
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: 688ae004dad8653e70d86b3b91652b6898c1e9d3
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "74881285"
---
# <a name="get-started-using-python-on-windows-for-beginners"></a>开始在 Windows 上使用 Python（初学者）

下面是一个分步指南，适用对了解 Python 感兴趣并使用 Windows 10 的初学者。

## <a name="set-up-your-development-environment"></a>设置开发环境

对于不熟悉 Python 的初学者，建议[从 Microsoft Store 安装 Python](https://www.microsoft.com/p/python-37/9nj46sx7x90p?activetab=pivot:overviewtab)。 通过 Microsoft Store 安装会使用基本 Python3 解释器，但会为当前用户设置 PATH 设置（避免需要管理员访问权限），以及提供自动更新。 如果处于教育环境中或所属组织限制计算机上的权限或管理访问权限，则这会特别有用。

如果在 Windows 上将 Python 用于 Web 开发  ，则建议为开发环境设置其他设置。 建议通过适用于 Linux 的 Windows 子系统安装和使用 Python，而不是直接在 Windows 上安装。 有关帮助，请参阅：[开始在 Windows 上将 Python 用于 Web 开发](./web-frameworks.md)。 如果对在操作系统上自动执行常见任务感兴趣，请参阅我们的指南：[开始在 Windows 上将 Python 用于脚本和自动化](./scripting.md)。 对于某些高级方案（如需要访问/修改 Python 的已安装文件、创建二进制文件的副本或直接使用 Python DLL），可能需要考虑直接从 [python.org](https://www.python.org/downloads/) 下载特定 Python 版本或考虑安装[替代实现](https://www.python.org/download/alternatives)，如 Anaconda、Jython、PyPy、WinPython、IronPython 等。建议仅当你是更高级的 Python 程序员并且有特定原因需要选择替代实现时才使用此方法。

## <a name="install-python"></a>安装 Python

使用 Microsoft Store 安装 Python：

1. 转到“开始”  菜单（左下方 Windows 图标），输入“Microsoft Store”，选择用于打开应用商店的链接。

2. 应用商店打开后，从右上方菜单中选择“搜索”  ，然后输入“Python”。 从“应用”下的结果中打开“Python 3.7”。 选择“获取”  。

3. Python 完成下载和安装过程后，使用“开始”  菜单（左下方 Windows 图标）打开 Windows PowerShell。 PowerShell 打开后，输入 `Python --version` 以确认已在计算机上安装了 Python3。

4. Python 的 Microsoft Store 安装包括 pip  （标准包管理器）。 通过 pip 可以安装和管理不属于 Python 标准库的其他包。 若要确认还可使用 pip 安装和管理包，请输入 `pip --version`。

## <a name="install-visual-studio-code"></a>安装 Visual Studio Code

通过使用 VS Code 作为文本编辑器/集成开发环境 (IDE)，可以利用 [IntelliSense](https://code.visualstudio.com/docs/editor/intellisense)（代码完成辅助）、[Linting](https://code.visualstudio.com/docs/python/linting)（可帮助避免在代码中发生错误）、[调试支持](https://code.visualstudio.com/docs/python/debugging)（可帮助在运行代码之后查找代码中的错误）、[代码片段](https://code.visualstudio.com/docs/editor/userdefinedsnippets)（小型可重用代码块的模板）以及[单元测试](https://code.visualstudio.com/docs/python/unit-testing)（使用不同类型的输入测试代码的接口）。

VS Code 还包含一个[内置终端](https://code.visualstudio.com/docs/editor/integrated-terminal)，使你可以使用 Windows 命令提示符、PowerShell 或是所喜欢的任何工具打开 Python 命令行，从而在代码编辑器与命令行之间建立无缝工作流。

1. 若要安装 VS Code，请下载适用于 Windows 的 VS Code：[https://code.visualstudio.com](https://code.visualstudio.com)。

2. 安装 VS Code 以后，还需安装 Python 扩展。 若要安装 Python 扩展，可以选择 [VS Code 市场链接](https://marketplace.visualstudio.com/items?itemName=ms-python.python)，也可以打开 VS Code 并在扩展菜单中搜索 **Python** (Ctrl+Shift+X)。

3. Python 是一种解释型语言，若要运行 Python 代码，必须向 VS Code 告知要使用的解释器。 建议坚持使用 Python 3.7，除非有特定原因需要选择其他解释器。 安装 Python 扩展以后，请选择 Python 3 解释器，具体方法是：打开“命令面板”  (Ctrl+Shift+P)，开始输入命令“Python:  选择解释器”进行搜索，然后选择命令。 在可用时，还可以使用底部状态栏上的“选择 Python 环境”  选项（它可能已显示所选解释器）。 该命令提供 VS Code 可以自动找到的可用解释器列表（包括虚拟环境）。 如果看不到所需解释器，请参阅[配置 Python 环境](https://code.visualstudio.com/docs/python/environments)。

    ![在 VS Code 中选择 Python 解释器](../images/interpreterselection.gif)

4. 若要在 VS Code 中打开终端，请选择“视图”   > “终端”  ，或者使用快捷方式 Ctrl+'  （使用反撇号字符）。 默认终端是 PowerShell。

5. 在 VS Code 终端中，只需通过输入以下命令即可打开 Python：`python`

6. 通过输入以下内容来尝试使用 Python 解释器：`print("Hello World")`。 Python 会返回语句“Hello World”。

    ![VS Code 中的 Python 命令行](../images/python-in-vscode.png)

## <a name="install-git-optional"></a>安装 Git（可选）

如果计划与其他人协作处理 Python 代码，或是在开放源代码站点（例如 GitHub）上托管项目，则 VS Code 支持[使用 Git 进行版本控制](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support)。 VS Code 中的“源代码管理”选项卡可跟踪所有更改，并直接在 UI 中内置了常见 Git 命令（add、commit、push、pull）。 需要先安装 Git，以便为“源代码管理”面板提供支持。

1. 从 [git-scm 网站](https://git-scm.com/download/win)下载并安装适用于 Windows 的 Git。

2. 其中包含了一个安装向导，该向导会询问一系列有关 Git 安装设置的问题。 建议使用所有默认设置，除非有特定原因需要更改某些设置。

3. 如果以前从未使用过 Git，则 [GitHub 指南](https://guides.github.com/)可以帮助入门。

## <a name="hello-world-tutorial-for-some-python-basics"></a>有关一些 Python 基础知识的 Hello World 教程

据创建者 Guido van Rossum 所说，Python 是一种“高级编程语言”，其核心设计理念全都与代码可读性以及使程序员可以采用几行代码来表达概念的语法相关。

Python 是一种解释型语言。 与所编写的代码需要转换为机器码才能由计算机处理器运行的编译型语言不同，Python 代码直接传递给解释器并直接运行。 只需键入代码并运行即可。 我们来试一试！

1. 打开 PowerShell 命令行后，输入 `python` 以运行 Python 3 解释器。 （某些说明更喜欢使用命令 `py` 或 `python3`，这些方法也适用）。 你会知道已成功执行，因为会显示一个 >>> 提示符（包含三个大于符号）。

2. 可以通过几种内置方法修改 Python 中的字符串。 使用以下命令创建变量：`variable = 'Hello World!'`。 按 Enter 以换行。

3. 使用以下命令打印变量：`print(variable)`。 这会显示文本“Hello World!”。

4. 使用以下命令算出字符串变量的长度（使用的字符数）：`len(variable)`。 这会显示使用了 12 个字符。 （请注意，空格在总长度中计为一个字符。）

5. 将字符串变量转换为大写字母：`variable.upper()`。 现在将字符串变量转换为小写字母：`variable.lower()`。

6. 统计在字符串变量中使用字母“l”的次数：`variable.count("l")`。

7. 在字符串变量中搜索特定字符，我们使用以下命令来查找感叹号：`variable.find("!")`。 这会显示感叹号是字符串第 11 个位置的字符。

8. 将感叹号替换为问号：`variable.replace("!", "?")`。

9. 若要退出 Python，可以输入 `exit()`、`quit()` 或选择 Ctrl-Z。

![此教程的 PowerShell 屏幕截图](../images/hello-world-basics.png)

希望你在使用 Python 的某些内置字符串修改方法时感受到乐趣。 现在来尝试使用 VS Code 创建 Python 程序文件并运行。

## <a name="hello-world-tutorial-for-using-python-with-vs-code"></a>将 Python 与 VS Code 一起使用的 Hello World 教程

VS Code 团队结合了一个出色的 [Python 入门教程](https://code.visualstudio.com/docs/python/python-tutorial#_start-vs-code-in-a-project-workspace-folder)，其中演练了如何使用 Python 创建 Hello World 程序、运行程序文件、配置和运行调试器以及安装 matplotlib  和 numpy  等包以在虚拟环境中创建图表。

1. 打开 PowerShell 并创建名为“hello”的空文件夹，导航到此文件夹，然后在 VS Code 中打开它：

    ```console
    mkdir hello
    cd hello
    code .
    ```

2. VS Code 打开后，在左侧“资源管理器”  窗口中会显示新的 hello  文件夹，在 VS Code 底部面板中打开命令行窗口，具体方法是按 Ctrl+'  （使用反撇号），或选择“视图”   > “终端”  。 在一个文件夹中启动 VS Code 会使该文件夹成为“工作区”。 VS Code 将特定于该工作区的设置存储在 .vscode/settings.json 中，这些设置与全局存储的用户设置分隔开来。

3. 继续 VS Code 文档中的教程：[创建 Python Hello World 源代码文件](https://code.visualstudio.com/docs/python/python-tutorial#_create-a-python-hello-world-source-code-file)。

## <a name="create-a-simple-game-with-pygame"></a>使用 Pygame 创建简单游戏

![运行示例游戏的 Pygame](../images/pygame-shmup.jpg)

Pygame 是一种用于编写游戏的常用 Python 包 - 鼓励学生学习编程，同时创建有趣的内容。 Pygame 会在新窗口中显示图形，因此无法在 WSL 的纯命令行方法下工作。 但是，如果按照本教程中所述通过 Microsoft Store 安装了 Python，则它会正常工作。

1. 安装 Python 后，通过键入 `python -m pip install -U pygame --user` 从命令行（或 VS Code 中的终端）安装 pygame。

2. 通过运行示例游戏来测试安装：`python -m pygame.examples.aliens`

3. 一切正常，游戏会打开一个窗口。 玩完游戏后，关闭该窗口。

下面介绍如何开始编写自己的游戏。

1. 打开 PowerShell（或 Windows 命令提示符）并创建名为“bounce”的空文件夹。 导航到此文件夹并创建名为“bounce.py”的文件。 在 VS Code 中打开该文件夹：

    ```powershell
    mkdir bounce
    cd bounce
    new-item bounce.py
    code .
    ```

2. 使用 VS Code 输入以下 Python 代码（或是复制并粘贴）：

    ```python
    import sys, pygame

    pygame.init()

    size = width, height = 640, 480
    dx = 1
    dy = 1
    x= 163
    y = 120
    black = (0,0,0)
    white = (255,255,255)

    screen = pygame.display.set_mode(size)

    while 1:

        for event in pygame.event.get():
            if event.type == pygame.QUIT: sys.exit()

        x += dx
        y += dy

        if x < 0 or x > width:   
            dx = -dx

        if y < 0 or y > height:
            dy = -dy

        screen.fill(black)

        pygame.draw.circle(screen, white, (x,y), 8)

        pygame.display.flip()
    ```

3. 将它保存为：`bounce.py`。

4. 在 PowerShell 终端中，通过输入以下内容来运行它：`python bounce.py`。

    ![运行下一个重要内容的 Pygame](../images/pygame.jpg)

尝试调整某些数字，以查看它们对弹力球的影响。

可在 [pygame.org](http://www.pygame.org) 上阅读有关使用 pygame 编写游戏的更多信息。

## <a name="resources-for-continued-learning"></a>用于继续了解的资源

建议通过以下资源来支持你继续了解 Windows 上的 Python 开发。

### <a name="online-courses-for-learning-python"></a>学习 Python 的在线课程

- [Microsoft Learn 上的 Python 简介](https://docs.microsoft.com/learn/modules/intro-to-python/)：尝试使用交互式 Microsoft Learn 平台，并获得完成该模块的经验积分，该模块涵盖了有关如何编写基本 Python 代码、声明变量以及使用控制台输入和输出的基础知识。 对于尚未设置 Python 开发环境的人员来说，交互式沙箱环境是一个不错的起点。

- [Pluralsight 上的 Python：8 个课程，29 小时](https://app.pluralsight.com/paths/skills/python)：Pluralsight 上的 Python 学习路径提供了涵盖与 Python 相关的各种主题的在线课程，包括用于衡量技能和查找知识缺口的工具。

- [LearnPython.org 教程](https://www.learnpython.org/)：使用 DataCamp 的人员提供的这些免费的交互式 Python 教程，无需安装或设置任何内容即可开始学习 Python。

- [Python.org 教程](https://docs.python.org/3/tutorial/index.html)：向读者通俗地介绍 Python 语言和系统的基本概念和功能。

- [在 Lynda.com 上学习 Python](https://www.lynda.com/Python-tutorials/Learning-Python/661773-2.html)：Python 的基本简介。

### <a name="working-with-python-in-vs-code"></a>在 VS Code 中使用 Python

- [ VS Code 中编辑 Python](https://code.visualstudio.com/docs/python/editing)：详细了解如何利用适用于 Python 的 VS Code 自动完成和 IntelliSense 支持，包括如何自定义其行为或者只是将它们关闭。

- [Linting Python](https://code.visualstudio.com/docs/python/linting)：Linting 是运行程序的过程，会分析代码以查找潜在错误。 了解 VS Code 为 Python 提供的不同形式的 linting 支持，以及如何进行设置。

- [调试 Python](https://code.visualstudio.com/docs/python/debugging)：调试是在计算机程序中识别并消除错误的过程。 本文介绍如何使用 VS Code 初始化和配置 Python 的调试、如何设置和验证断点、如何附加本地脚本、如何针对不同应用类型或在远程计算机上执行调试以及一些基本的故障排除。

- [对Python 进行单元测试](https://code.visualstudio.com/docs/python/unit-testing)：介绍了一些说明单元测试含义的背景、一个示例演练、如何启用测试框架、如何创建和运行测试、如何调试测试以及测试配置设置。 
