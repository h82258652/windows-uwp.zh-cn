---
title: 开始在 Windows 上使用 Python
description: 本指南可帮助你开始使用 Windows 上的 Python 的新品牌。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: python, windows 10, microsoft, 学习 python, 适用于初学者的 windows 上的 python, 通过 microsoft store 安装 python, 与 vs code 一起安装 python, windows 上的 pygame
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: 5c1861d76a98ff76b130f3012d730980482cda75
ms.sourcegitcommit: a28a32fff9d15ecf4a9d172cd0a04f4d993f9d76
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/12/2019
ms.locfileid: "68959096"
---
# <a name="get-started-using-python-on-windows-for-beginners"></a>开始在 Windows 上使用 Python

下面是有关使用 Windows 10 学习 Python 的入门指南。

## <a name="set-up-your-development-environment"></a>设置开发环境

对于不熟悉 Python 的新手, 我们建议[从 Microsoft Store 安装 Python](https://www.microsoft.com/en-us/p/python-37/9nj46sx7x90p?activetab=pivot:overviewtab)。 通过 Microsoft Store 安装将使用 basic Python3 解释器, 但会为当前用户 (避免需要管理员访问权限) 设置路径设置, 并提供自动更新。 如果你处于教育环境或组织中限制权限或管理访问权限的部分, 则此项特别有用。

如果在 Windows 上使用 Python 进行**web 开发**, 则建议为开发环境设置其他设置。 建议通过适用于 Linux 的 Windows 子系统安装和使用 Python, 而不是直接在 Windows 上安装。 有关帮助, 请参阅:[开始在 Windows 上使用 Python 进行 web 开发](./python-for-web.md)。 如果你有兴趣自动执行操作系统上的常见任务, 请参阅以下指南:[开始在 Windows 上使用 Python 进行脚本编写和自动化](./python-for-scripting.md)。 对于某些高级方案 (例如需要访问/修改 Python 的已安装文件、创建二进制文件的副本或直接使用 Python Dll), 你可能需要考虑直接从[python.org](https://www.python.org/downloads/)下载特定的 Python 版本, 或考虑安装一[种替代方法](https://www.python.org/download/alternatives), 如 Anaconda、Jython、PyPy、WinPython、IronPython 等。仅当你是更高级的 Python 程序员时, 才建议使用此方法, 具体原因是选择替代实现。

## <a name="install-python"></a>安装 Python

使用 Microsoft Store 安装 Python:

1. 中转到 "**开始**" 菜单 (左下方的窗口图标), 键入 "Microsoft Store", 选择用于打开应用商店的链接。

2. 打开存储区后, 选择右上方菜单中的 "**搜索**", 然后输入 "Python"。 从 "应用" 下的结果中打开 "Python 3.7"。 选择 "**获取**"。

3. Python 完成下载和安装过程后, 请使用 "**开始**" 菜单 (左下方的窗口图标) 打开 Windows PowerShell。 打开 PowerShell 后, 输入`Python --version`以确认已在计算机上安装 Python3。

4. Python 的 Microsoft Store 安装包含**pip**, 即标准包管理器。 Pip 允许你安装和管理不属于 Python 标准库的其他包。 若要确认还具有用于安装和管理包的 pip, 请输入`pip --version`。

## <a name="install-visual-studio-code"></a>安装 Visual Studio Code

通过使用 VS Code 作为文本编辑器/集成开发环境 (IDE), 可以利用[IntelliSense](https://code.visualstudio.com/docs/editor/intellisense) (代码完成帮助) [Linting](https://code.visualstudio.com/docs/python/linting) (有助于避免在代码中产生错误)、[调试支持](https://code.visualstudio.com/docs/python/debugging)(帮助你在中查找错误)运行后的代码)、[代码片段](https://code.visualstudio.com/docs/editor/userdefinedsnippets)(小型可重用代码块的模板) 以及[单元测试](https://code.visualstudio.com/docs/python/unit-testing)(使用不同类型的输入测试代码的接口)。

VS Code 还包含一个[内置终端](https://code.visualstudio.com/docs/editor/integrated-terminal), 使你能够使用 Windows 命令提示符、PowerShell 或你喜欢的任何方式打开 Python 命令行, 从而在你的代码编辑器和命令行之间建立无缝的工作流。

1. 若要安装 VS Code, 请下载适用于[https://code.visualstudio.com](https://code.visualstudio.com)Windows 的 VS Code:。

2. Python 是一种解释型语言, 若要运行 Python 代码, 必须告知 VS Code 要使用的解释器。 建议坚持使用 Python 3.7, 除非你有特定的原因要选择其他内容。 若要选择 python 3 解释器, 请打开**命令面板**(Ctrl + Shift + P), 开始键入**以下命令:选择 "** 解释器" 进行搜索, 并选择命令。 你还可以使用底部状态栏上的 "**选择 Python 环境**" 选项 (如果可用) (它可能已显示选定的解释器)。 该命令显示 VS Code 可以自动查找的可用解释器列表, 包括虚拟环境。 如果看不到所需的解释器, 请参阅[配置 Python 环境](https://code.visualstudio.com/docs/python/environments)。

    ![在 VS Code 中选择 Python 解释器](../../images/interpreterselection.gif)

3. 若要在 VS Code 中打开终端, 请选择 "**查看** > **终端**", 或者使用快捷方式**Ctrl + '** (使用反撇号字符)。 默认终端为 PowerShell。

4. 在 VS Code 终端中, 只需输入以下命令即可打开 Python:`python`

5. 输入以下内容, 尝试使用 Python 解释`print("Hello World")`器:。 Python 将返回语句 "Hello World"。

    ![VS Code 中的 Python 命令行](../../images/python-in-vscode.png)

## <a name="install-git-optional"></a>安装 Git (可选)

如果你计划在 Python 代码上与其他人进行协作, 或在开源站点 (例如 GitHub) 上托管你的项目, VS Code 支持[使用 Git 进行版本控制](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support)。 VS Code 中的 "源代码管理" 选项卡跟踪所有更改, 并在 UI 中内置内置的 Git 命令 (添加、提交、推送和拉取)。 首先需要安装 Git 才能打开源代码管理面板。

1. 从[git-scm 网站](https://git-scm.com/download/win)下载并安装适用于 Windows 的 Git。

2. 其中包含了一个安装向导, 该向导将询问一系列有关 Git 安装设置的问题。 建议使用所有默认设置, 除非您有特定原因要更改某些内容。

3. 如果以前从未处理过 Git, [GitHub 指南](https://guides.github.com/)可帮助你入门。

## <a name="hello-world-tutorial-for-some-python-basics"></a>有关某些 Python 基础知识的 Hello World 教程

根据其 creator Guido van Rossum, Python 是一种 "高级编程语言", 其核心设计理念全部与代码可读性和语法相关, 使程序员能够在几行代码中表达概念。 "

Python 是一种解释型语言。 与编译的语言不同, 你编写的代码需要转换为机器代码才能由计算机处理器运行, Python 代码直接传递给解释器并直接运行。 只需键入代码并运行代码。 试试吧!

1. 打开 PowerShell 命令行后, 输入`python`以运行 Python 3 解释器。 (某些指令更喜欢使用命令`py`或`python3`, 它们也应该有效。) 你将知道, 你会成功, 因为将显示一个 > > > 提示, 其中三个符号为三个。

2. 可以通过几种内置方法修改 Python 中的字符串。 使用以下方式创建变量: `variable = 'Hello World!'`。 对于新行, 请按 Enter。

3. 用以下内容打印变量`print(variable)`:。 这会显示文本 "Hello World!"。

4. 使用: `len(variable)`查找字符串变量的长度和使用的字符数。 这会显示使用了12个字符。 (请注意, 该空格在总长度中被计为一个字符。)

5. 将字符串变量转换为大写字母: `variable.upper()`。 现在将字符串变量转换为小写字母: `variable.lower()`。

6. 计算在字符串变量中使用字母 "l" 的次数: `variable.count("l")`。

7. 搜索字符串变量中的特定字符, 让我们查找感叹号, 使用: `variable.find("!")`。 这会显示感叹号位于字符串的第11个位置字符中。

8. 将感叹号替换为问号: `variable.replace("!", "?")`。

9. 若要退出 Python, 可以输入`exit()`、 `quit()`或, 然后选择 Ctrl + z。

![此教程的 PowerShell 屏幕截图](../../images/hello-world-basics.png)

希望使用 Python 的某些内置字符串修改方法时要开心。 现在, 请尝试创建 Python 程序文件并使用 VS Code 运行该文件。

## <a name="hello-world-tutorial-for-using-python-with-vs-code"></a>使用 Python 与 VS Code Hello World 教程

VS Code 团队已结合了有关 Python 的精彩[入门](https://code.visualstudio.com/docs/python/python-tutorial#_start-vs-code-in-a-project-workspace-folder)教程, 介绍如何使用 python 创建 Hello World 程序、运行程序文件、配置和运行调试器, 以及安装程序包 (例如*matplotlib*和*numpy*在虚拟环境中创建图形绘图。

1. 打开 PowerShell 并创建名为 "hello" 的空文件夹, 导航到此文件夹, 然后在 VS Code 中打开它:

    ```console
    mkdir hello
    cd hello
    code .
    ```

2. VS Code 打开后, 在左侧的**资源管理器**窗口中显示新的 " *hello* " 文件夹, 通过按**Ctrl + '** (使用反撇号) 或选择 "**查看** > ",在VSCode的底部面板中打开命令行窗口。**终端**。 通过在文件夹中开始 VS Code, 该文件夹将成为你的 "工作区"。 VS Code 存储特定于 vscode/settings 中的工作区的设置, 它们不同于全局存储的用户设置。

3. 继续 VS Code 文档中的教程:[创建 Python Hello World 源代码文件](https://code.visualstudio.com/docs/python/python-tutorial#_create-a-python-hello-world-source-code-file)。

## <a name="create-a-simple-game-with-pygame"></a>使用 Pygame 创建简单游戏

![运行示例游戏的 Pygame](../../images/pygame-shmup.jpg)

Pygame 是一种流行的 Python 包, 用于编写游戏-鼓励学生学习编程, 同时创建有趣的东西。 Pygame 在新窗口中显示图形, 因此它将无法在 WSL 的命令行方法下运行。 但是, 如果您通过本教程中所述的 Microsoft Store 安装了 Python, 它将正常工作。

1. 安装 Python 后, 通过键入`python -m pip install -U pygame --user`从命令行 (或 VS Code 内的终端) 安装 pygame。

2. 通过运行示例游戏来测试安装:`python -m pygame.examples.aliens`

3. 一切正常, 游戏就会打开一个窗口。 完成播放后, 关闭窗口。

下面介绍了如何开始编写自己的游戏。

1. 打开 PowerShell (或 Windows 命令提示符) 并创建一个名为 "弹跳" 的空文件夹。 导航到此文件夹并创建一个名为 "bounce.py" 的文件。 在 VS Code 中打开文件夹:

    ```powershell
    mkdir bounce
    cd bounce
    new-item bounce.py
    code .
    ```

2. 使用 "VS Code", 输入以下 Python 代码 (或复制并粘贴):

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

3. 将其另存`bounce.py`为:。

4. 从 PowerShell 终端, 通过输入以下内容来运行`python bounce.py`它:。

    ![Pygame 运行下一个大问题](../../images/pygame.jpg)

请尝试调整某些数字, 以查看它们对弹跳球的影响。

阅读有关通过 pygame 在[pygame.org](http://www.pygame.org)编写游戏的详细信息。

## <a name="resources-for-continued-learning"></a>用于持续学习的资源

建议通过以下资源来帮助你继续了解 Windows 上的 Python 开发。

### <a name="online-courses-for-learning-python"></a>学习 Python 的在线课程

- [Microsoft Learn 上的 Python 简介](https://docs.microsoft.com/en-us/learn/modules/intro-to-python/):尝试交互式 Microsoft Learn 平台并获得完成本模块的经验要点, 其中涵盖了有关如何编写基本 Python 代码、声明变量以及使用控制台输入和输出的基本知识。 交互式沙箱环境使其成为尚未设置 Python 开发环境的人员的理想位置。

- [Pluralsight 上的 Python:8个课程, 29](https://app.pluralsight.com/paths/skills/python)小时:Pluralsight 上的 Python 学习路径提供了涵盖各种与 Python 相关的主题的在线课程, 其中包括用于衡量技能和找出缺口的工具。

- [LearnPython.org 教程](https://www.learnpython.org/):开始学习 Python, 无需在 DataCamp 的人员中安装或设置这些免费交互式 Python 教程中的任何内容。

- [Python.org 教程](https://docs.python.org/3/tutorial/index.html):为读者提供 Python 语言和系统的基本概念和功能。

- [Lynda.com 上的学习 Python](https://www.lynda.com/Python-tutorials/Learning-Python/661773-2.html):Python 的基本简介。

### <a name="working-with-python-in-vs-code"></a>在 VS Code 中使用 Python

- [在 VS Code 中编辑 Python](https://code.visualstudio.com/docs/python/editing):详细了解如何利用适用于 Python 的 VS Code 自动完成功能和 IntelliSense 支持, 包括如何自定义其 behvior .。。或者只是将其关闭。

- [Linting Python](https://code.visualstudio.com/docs/python/linting):Linting 是运行程序的过程, 该程序将分析代码以查找可能的错误。 了解 VS Code 为 Python 提供的不同形式的 linting 支持, 以及如何对其进行设置。

- [调试 Python](https://code.visualstudio.com/docs/python/debugging):调试是指识别和删除计算机程序中的错误的过程。 本文介绍如何使用 VS Code 初始化和配置 Python 的调试, 如何设置和验证断点, 如何附加本地脚本, 如何针对不同的应用程序类型或远程计算机执行调试以及一些基本的故障排除。

- [单元测试 Python](https://code.visualstudio.com/docs/python/unit-testing):介绍了一些背景, 说明了单元测试的含义、示例演练、启用测试框架、创建和运行测试、调试测试和测试配置设置。 
