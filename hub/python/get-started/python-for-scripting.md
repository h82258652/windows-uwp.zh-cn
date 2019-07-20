---
title: Windows 上的 Python 脚本和自动化
description: 如何开始在 Windows 上使用 Python 进行脚本编写、自动化和系统管理。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
ms.prod: windows
ms.technology: hub
keywords: python, windows 10, microsoft, python 系统管理, python 文件自动化, windows 上的 python 脚本, 在 windows 上设置 python, windows 上的 python 开发人员环境, windows 上的 python 开发人员环境, windows 上的 python 开发环境, python, 适用于文件系统任务
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: 0571d442d17cdac8989df10d7c11f3e762ab6fb6
ms.sourcegitcommit: afb5157ec4bcb6588ac4cf74352688b30ed32257
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/19/2019
ms.locfileid: "68349464"
---
# <a name="get-started-using-python-on-windows-for-scripting-and-automation"></a>开始在 Windows 上使用 Python 进行脚本编写和自动化

下面是设置开发人员环境和开始使用 Python 在 Windows 上编写脚本和自动执行文件系统操作的循序渐进指南。

## <a name="set-up-your-development-environment"></a>设置开发环境

使用 Python 编写执行文件系统操作的脚本时, 我们建议您[从 Microsoft Store 安装 Python](https://www.microsoft.com/en-us/p/python-37/9nj46sx7x90p?activetab=pivot:overviewtab)。 

> [!IMPORTANT]
> 如果在 Windows 上使用 Python 进行**web 开发**, 则建议为开发环境设置其他设置。 通过适用于 Linux 的 Windows 子系统安装 Python, 而不是直接在 Windows 上安装。 在本指南中查找演练:[开始在 Windows 上使用 Python 进行 web 开发](./python-for-web.md)。 如果你不熟悉 Python, 请尝试我们的指南:[开始在 Windows 上使用 Python](./python-for-education.md)。 <br>对于某些高级方案, 你可能需要考虑直接从[python.org](https://www.python.org/downloads/windows/)下载特定的 Python 版本, 或考虑安装[备用](https://www.python.org/download/alternatives)项, 例如 Anaconda、Jython、PyPy、WinPython、IronPython 等。仅当你是更高级的 Python 程序员时, 才建议使用此方法, 具体原因是选择替代实现。

## <a name="install-python"></a>安装 Python

使用 Microsoft Store 安装 Python:

1. 中转到 "**开始**" 菜单 (左下方的窗口图标), 键入 "Microsoft Store", 选择用于打开应用商店的链接。

2. 打开存储区后, 选择右上方菜单中的 "**搜索**", 然后输入 "Python"。 从 "应用" 下的结果中打开 "Python 3.7"。 选择 "**获取**"。

3. Python 完成下载和安装过程后, 请使用 "**开始**" 菜单 (左下方的窗口图标) 打开 Windows PowerShell。 打开 PowerShell 后, 输入`Python --version`以确认已在计算机上安装 Python3。

4. Python 的 Microsoft Store 安装包含**pip**, 即标准包管理器。 Pip 允许你安装和管理不属于 Python 标准库的其他包。 若要确认还具有用于安装和管理包的 pip, 请输入`pip --version`。

## <a name="install-visual-studio-code"></a>安装 Visual Studio Code

通过使用 VS Code 作为文本编辑器/集成开发环境 (IDE), 可以利用[IntelliSense](https://code.visualstudio.com/docs/editor/intellisense) (代码完成帮助) [Linting](https://code.visualstudio.com/docs/python/linting) (有助于避免在代码中产生错误)、[调试支持](https://code.visualstudio.com/docs/python/debugging)(帮助你在中查找错误)运行后的代码)、[代码片段](https://code.visualstudio.com/docs/editor/userdefinedsnippets)(小型可重用代码块的模板) 以及[单元测试](https://code.visualstudio.com/docs/python/unit-testing)(使用不同类型的输入测试代码的接口)。

下载适用于 Windows 的 VS Code, 并按照安装[https://code.visualstudio.com](https://code.visualstudio.com)说明进行操作:。

## <a name="install-the-microsoft-python-extension"></a>安装 Microsoft Python 扩展

需要安装 Microsoft Python 扩展, 才能利用 VS Code 支持功能。 [了解详情](https://code.visualstudio.com/docs/languages/python)。

1. 通过输入**Ctrl + Shift + X**打开 "VS Code 扩展" 窗口 (或使用菜单导航到 "**视图** > " "**扩展**")。

2. 在 "Marketplace 的顶级**搜索扩展**" 框中, 输入:**Python**。

3. **通过 Microsoft 扩展查找 python (ms python python)** , 并选择 "绿色**安装**" 按钮。

## <a name="open-the-integrated-powershell-terminal-in-vs-code"></a>在 VS Code 中打开集成的 PowerShell 终端

VS Code 包含一个[内置终端](https://code.visualstudio.com/docs/editor/integrated-terminal), 使你能够使用 PowerShell 打开 Python 命令行, 并在代码编辑器和命令行之间建立无缝的工作流。

1. 在 VS Code 中打开终端, 选择 "**查看** > **终端**", 或者使用快捷方式**Ctrl + "** (使用反撇号字符)。

    > [!NOTE]
    > 默认终端应为 PowerShell, 但如果需要更改它, 请按**Ctrl + Shift + P**输入命令调色板。 输入**终端:选择 "默认** Shell", 将显示 "包含 PowerShell"、"命令提示符"、"WSL" 等的终端选项的列表。选择你想要使用的帐户, 然后输入**Ctrl + Shift + '** (使用反撇号) 以创建新终端。

2. 在 VS Code 终端, 通过输入以下内容打开 Python:`python`

3. 输入以下内容, 尝试使用 Python 解释`print("Hello World")`器:。 Python 将返回语句 "Hello World"。

    ![VS Code 中的 Python 命令行](../../images/python-in-vscode.png)

4. 若要退出 Python, 可以输入`exit()`、 `quit()`或, 然后选择 Ctrl + z。

## <a name="install-git-optional"></a>安装 Git (可选)

如果你计划在 Python 代码上与其他人进行协作, 或在开源站点 (例如 GitHub) 上托管你的项目, VS Code 支持[使用 Git 进行版本控制](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support)。 VS Code 中的 "源代码管理" 选项卡跟踪所有更改, 并在 UI 中内置内置的 Git 命令 (添加、提交、推送和拉取)。 首先需要安装 Git 才能打开源代码管理面板。

1. 从[git-scm 网站](https://git-scm.com/download/win)下载并安装适用于 Windows 的 Git。

2. 其中包含了一个安装向导, 该向导将询问一系列有关 Git 安装设置的问题。 建议使用所有默认设置, 除非您有特定原因要更改某些内容。

3. 如果以前从未处理过 Git, [GitHub 指南](https://guides.github.com/)可帮助你入门。

## <a name="example-script-to-display-the-structure-of-your-file-system-directory"></a>用于显示文件系统目录的结构的示例脚本

常见的系统管理任务可能需要花费大量时间, 但使用 Python 脚本时, 您可以自动执行这些任务, 这样就不需要任何时间了。 例如, Python 可以读取您的计算机的文件系统的内容并执行以下操作: 打印文件和目录的大纲, 将文件夹从一个目录移动到另一个目录, 或重命名上百个文件。 通常, 如果您要手动执行这些任务, 则这些任务可能会花费大量时间。 请改用 Python 脚本!

让我们从一个简单的脚本开始, 该脚本会遍历目录树并显示目录结构。

1. 使用 "**开始**" 菜单 (左下方的窗口图标) 打开 PowerShell。

2. 为项目创建目录: `mkdir python-scripts`, 然后打开该目录:。 `cd python-scripts`

3. 创建一些用于我们的示例脚本的目录:

    ```powershell
    mkdir food, food/fruits, food/fruits/apples, food/fruits/oranges, food/vegetables
    ```

4. 在这些目录中创建一些与我们的脚本一起使用的文件:

    ```powershell
    new-item food/fruits/banana.txt, food/fruits/strawberry.txt, food/fruits/blueberry.txt, food/fruits/apples/honeycrisp.txt, food/fruits/oranges/mandarin.txt, food/vegetables/carrot.txt
    ```

5. 在 python 脚本目录中创建新的 python 文件:

    ```powershell
    mkdir src
    new-item src/list-directory-contents.py
    ```

6. 通过输入以下内容在 VS Code 中打开项目:`code .`

7. 通过输入**Ctrl + Shift + E**打开 "VS Code 文件资源管理器" 窗口 (或使用菜单导航到 "**视图** > **资源管理器**"), 并选择刚创建的 list-directory-contents.py 文件。 Microsoft Python 扩展会自动加载 Python 解释器。 你可以查看 VS Code 窗口底部加载了哪个解释器。

    > [!NOTE]
    > Python 是一种解释型语言, 这意味着它作为虚拟机, 模拟物理计算机。 可使用不同类型的 Python 解释器:Python 2、Python 3、Anaconda、PyPy 等。若要运行 Python 代码并获取 Python IntelliSense, 必须告知 VS Code 要使用的解释器。 建议坚持 VS Code 默认选择的解释器 (在本例中为 Python 3), 除非您有特定原因要选择其他内容。 若要更改 Python 解释器, 请选择 "VS Code" 窗口底部蓝色栏中当前显示的解释器, 或打开**命令面板**(Ctrl + Shift + P), 然后输入**以下命令:选择解释**器。 这会显示当前已安装的 Python 解释器列表。 [详细了解如何配置 Python 环境](https://code.visualstudio.com/docs/python/environments)。

    ![在 VS Code 中选择 Python 解释器](../../images/interpreterselection.gif)

8. 将以下代码粘贴到 list-directory-contents.py 文件中, 然后选择 "**保存**":

    ```python
    import os

    root = '%s%s%s' % ('..', os.path.sep, 'food')
    for directory, subdir_list, file_list in os.walk(root):
        print('Directory: ' + directory)
        for name in subdir_list:
            print ('Subdirectory: ' + name)
        for name in file_list:
            print('File: ' + name)
        print(os.linesep)
    ```

9. 使用反撇号字符打开 VS Code 集成终端 (**Ctrl + '** ), 然后输入刚刚保存 Python 脚本的 src 目录:

    ```powershell
    cd src
    ```

10. 在 PowerShell 中运行以下脚本:

    ```powershell
    python3 .\list-directory-contents.py
    ```

    应会看到如下所示的输出:

    ```powershell
    Directory: ../food
    Subdirectory: fruits
    Subdirectory: vegetables

    Directory: ../food\fruits
    Subdirectory: apples
    Subdirectory: oranges
    File: banana.txt
    File: blueberry.txt
    File: strawberry.txt

    Directory: ../food\fruits\apples
    File: honeycrisp.txt

    Directory: ../food\fruits\oranges
    File: mandarin.txt

    Directory: ../food\vegetables
    File: carrot.txt
    ```

11. 通过直接在 PowerShell 终端中输入以下命令, 使用 Python 将文件系统目录输出打印到自己的文本文件中:`python3 list-directory-contents.py > food-directory.txt`

祝贺你！ 你刚编写了一个自动系统管理脚本, 该脚本将读取你创建的目录和文件, 并使用 Python 将目录结构显示并打印到自己的文本文件中。

## <a name="example-script-to-modify-all-files-in-a-directory"></a>用于修改目录中所有文件的示例脚本

此示例使用刚刚创建的文件和目录, 并通过将文件的上次修改日期添加到文件名开头来重命名每个文件。

1. 在**python 脚本**目录的**src**文件夹内, 为脚本创建一个新的 python 文件:

    ```powershell
    new-item update-filenames.py
    ```

2. 打开 update-filenames.py 文件, 将以下代码粘贴到该文件中, 并保存该文件:

    > [!NOTE]
    > getmtime 返回以计时周期表示的时间戳, 这不容易理解。 必须首先将其转换为标准日期时间字符串。

    ```python
    import datetime
    import os

    root = '%s%s%s' % ('..', os.path.sep, 'food')
    for directory, subdir_list, file_list in os.walk(root):
        for name in file_list:
            source_name = '%s%s%s' % (directory, os.path.sep, name)
            timestamp = os.path.getmtime(source_name)
            modified_date = str(datetime.datetime.fromtimestamp(timestamp)).replace(':', '.')
            target_name = '%s%s%s_%s' % (directory, os.path.sep, modified_date, name)

            print ('Renaming: %s to: %s' % (source_name, target_name))

            os.rename(source_name, target_name)
    ```

3. 运行 update-filenames.py 脚本`python3 update-filenames.py` , 并再次运行 list-directory-contents.py 脚本:`python3 list-directory-contents.py`

4. 应会看到如下所示的输出:

    ```powershell
    Renaming: ..\food\fruits\banana.txt to: ..\food\fruits\2019-07-18 12.24.46.385185_banana.txt
    Renaming: ..\food\fruits\blueberry.txt to: ..\food\fruits\2019-07-18 12.24.46.391170_blueberry.txt
    Renaming: ..\food\fruits\strawberry.txt to: ..\food\fruits\2019-07-18 12.24.46.389174_strawberry.txt
    Renaming: ..\food\fruits\apples\honeycrisp.txt to: ..\food\fruits\apples\2019-07-18 12.24.46.395160_honeycrisp.txt
    Renaming: ..\food\fruits\oranges\mandarin.txt to: ..\food\fruits\oranges\2019-07-18 12.24.46.398151_mandarin.txt
    Renaming: ..\food\vegetables\carrot.txt to: ..\food\vegetables\2019-07-18 12.24.46.402496_carrot.txt

    ~/src/python-scripting/src$ python3 .\list-directory-contents.py
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

5. 通过直接在 PowerShell 终端中输入以下命令, 使用 Python 打印带有最后修改的时间戳的新文件系统目录名称:`python3 list-directory-contents.py > food-directory-last-modified.txt`

希望你了解到使用 Python 脚本来自动执行基本系统管理任务的几个有趣的操作。 当然, 还有很多知识需要知道, 但我们希望这就是您在右页脚开始。 我们共享了几个额外资源, 以便继续学习。

## <a name="additional-resources"></a>其他资源

- [Python 文档:文件和目录访问](https://docs.python.org/3.7/library/filesys.html):有关使用文件系统和使用模块读取文件属性、以可移植方式操作路径以及创建临时文件的 Python 文档。
- [了解 Python:String_Formatting 教程](https://www.learnpython.org/en/String_Formatting):有关使用 "%" 运算符进行字符串格式设置的详细信息。
- [需要了解10个 Python 文件系统方法](https://towardsdatascience.com/10-python-file-system-methods-you-should-know-799f90ef13c2):有关用和`shutil`操作文件和文件夹的`os`中型文章。
- [Python 的 Hitchhikers 指南:系统管理](https://docs.python-guide.org/scenarios/admin/):"固执指南" 提供有关 Python 相关主题的概述和最佳实践。 本部分介绍系统管理工具和框架。 本指南托管在 GitHub 上, 因此你可以提出问题并做出贡献。
