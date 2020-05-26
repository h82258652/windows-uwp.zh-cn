---
title: 在 Windows 上使用 Python 进行 Web 开发
description: 如何开始在 Windows 上使用 Python 进行 Web 开发，包括针对 Flask 和 Django 等框架进行设置。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: python, windows 10, microsoft, windows 上的 python, 使用 wsl 的 python web, 使用适用于 linux 的 windows 子系统的 python web 应用, windows 上的 python web 开发, windows 上的 flask 应用, windows 上的 django 应用, python web, windows 上的 flask web 开发, windows 上的 django web 开发, 使用 python 的 windows web 开发, vs code python web 开发, 远程 wsl 扩展, ubuntu, wsl, venv, pip, microsoft python 扩展, 在 windows 上运行 python, 在 windows 上使用 python, 在 windows 上使用 python 构建
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: 3ae3b04738152ff1a142e1599cc05357006456b9
ms.sourcegitcommit: 2af814b7f94ee882f42fae8f61130b9cc9833256
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/20/2020
ms.locfileid: "83717136"
---
# <a name="get-started-using-python-for-web-development-on-windows"></a>开始在 Windows 上将 Python 用于 Web 开发

下面是有关通过适用于 Linux 的 Windows 子系统 (WSL)，开始在 Windows 上使用 Python 进行 Web 开发的分步指南。

## <a name="set-up-your-development-environment"></a>设置开发环境

建议在构建 Web 应用程序时在 WSL 上安装 Python。 有关 Python Web 开发的许多教程和说明是面向 Linux 用户编写的，并使用基于 Linux 的打包和安装工具。 大多数 Web 应用也部署在 Linux 上，因此这会确保开发环境与生产环境之间的一致性。

如果是将 Python 用于 Web 开发以外的其他工作，则建议使用 Microsoft Store 直接在 Windows 10 上安装 Python。 WSL 不支持 GUI 桌面或应用程序（例如 PyGame、Gnome、KDE 等）。 对于这些情况，请直接在 Windows 上安装并使用 Python。 如果不熟悉 Python，请参阅我们的指南：[开始在 Windows 上使用 Python（初学者）](./beginners.md)。 如果对在操作系统上自动执行常见任务感兴趣，请参阅我们的指南：[开始在 Windows 上将 Python 用于脚本和自动化](./scripting.md)。 对于某些高级方案，可能需要考虑直接从 [python.org](https://www.python.org/downloads/windows/) 下载特定 Python 版本或考虑安装[替代实现](https://www.python.org/download/alternatives)，如 Anaconda、Jython、PyPy、WinPython、IronPython 等。建议仅当你是更高级的 Python 程序员并且有特定原因需要选择替代实现时才使用此方法。

## <a name="install-windows-subsystem-for-linux"></a>安装适用于 Linux 的 Windows 子系统

通过 WSL，可运行与 Windows 和你喜欢的工具（如 Visual Studio Code 和 Outlook 等）直接集成的 GNU/Linux 命令行环境。

要启用和安装 WSL（或 WSL 2，由你的需求而定），请按照 [WSL 安装文档](https://docs.microsoft.com/windows/wsl/install-win10)中的步骤操作。 这些步骤将包含选择 Linux 发行版（例如 Ubuntu）。

安装 WSL 和 Linux 发行版后，打开 Linux 发行版（可在 Windows 的开始菜单中找到），并使用命令 `lsb_release -dc` 查看版本和代码名称。

建议定期更新 Linux 发行版，包括在安装之后立即更新，以确保具有最新的包。 Windows 不会自动处理此更新。 要更新发行版，请使用命令：`sudo apt update && sudo apt upgrade`。  

> [!TIP]
> 请考虑[从 Microsoft Store 安装新的 Windows 终端](https://www.microsoft.com/store/apps/9n0dx20hk701)，从而启用多个选项卡（在多个 Linux 命令行、Windows 命令提示符、PowerShell 和 Azure CLI 等之间快速切换）、创建键绑定（用于打开或关闭选项卡、复制粘贴等的快捷方式键）、使用搜索功能，以及设置自定义主题（配色方案、字体样式和大小、背景图像/模糊/透明度）。 [了解详细信息](https://docs.microsoft.com/windows/terminal)。

## <a name="set-up-visual-studio-code"></a>设置 Visual Studio Code

通过 VS Code 可利用 [IntelliSense](https://code.visualstudio.com/docs/editor/intellisense)、[Linting](https://code.visualstudio.com/docs/python/linting)、[调试支持](https://code.visualstudio.com/docs/python/debugging)、[代码片段](https://code.visualstudio.com/docs/editor/userdefinedsnippets)和[单元测试](https://code.visualstudio.com/docs/python/unit-testing)。 VS Code 与适用于 Linux 的 Windows 子系统完美集成，可提供一个[内置终端](https://code.visualstudio.com/docs/editor/integrated-terminal)，用于在代码编辑器和命令行之间建立无缝工作流，此外还通过直接内置在 UI 中的常用 Git 命令（添加、提交、推送、拉取）支持 [Git 进行版本控制](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support)。

1. [下载和安装适用于 Windows 的 VS Code](https://code.visualstudio.com)。 VS Code 也适用于 Linux，但适用于 Linux 的 Windows 子系统不支持 GUI 应用，因此需要在 Windows 上安装它。 不必担心，仍可以使用 Remote - WSL 扩展与 Linux 命令行和工具集成。

2. 在 VS Code 上安装 [Remote - WSL 扩展](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl)。 这使你可以将 WSL 用作集成开发环境，并且会为你处理兼容性和路径。 [了解详细信息](https://code.visualstudio.com/docs/remote/remote-overview)。

> [!IMPORTANT]
> 如果已安装 VS Code，则需要确保具有 [1.35 5 月版本](https://code.visualstudio.com/updates/v1_35)或更高版本，以便安装 [Remote - WSL 扩展](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl)。 建议不要在没有 Remote - WSL 扩展的情况下在 VS Code 中使用 WSL，因为会失去对自动完成、调试、linting 等的支持。趣味事实：此 WSL 扩展安装在 $HOME/.vscode-server/extensions 中。

## <a name="create-a-new-project"></a>创建新项目

让我们在 Linux (Ubuntu) 文件系统上创建一个新项目目录，我们随后会使用 VS Code 处理 Linux 应用和工具。

1. 关闭 VS Code，然后转到“开始”菜单（左下方 Windows 图标）并输入以下内容，以便打开 Ubuntu 18.04（WSL 命令行）：“Ubuntu 18.04”。

2. 在 Ubuntu 命令行中，导航到要在其中放置项目的位置，并为项目创建目录：`mkdir HelloWorld`。

![Ubuntu 终端](../images/ubuntu-terminal.png)

> [!TIP]
> 使用适用于 Linux 的 Windows 子系统 (WSL) 时要记住的一个重要事项是，现在是在两个不同文件系统之间工作：1) Windows 文件系统，以及 2) Linux 文件系统 (WSL)（对于我们的示例为 Ubuntu）。 需要注意安装包和存储文件的位置。 可以在 Windows 文件系统中安装一个版本的工具或包，并在 Linux 文件系统中安装完全不同的版本。 在 Windows 文件系统中更新工具不会影响 Linux 文件系统中的工具，反之亦然。 WSL 会将固定驱动器装载到计算机上 Linux 发行版本中的 `/mnt/<drive>` 文件夹下。 例如，Windows C: 驱动器装载在 `/mnt/c/` 下。 可以从 Ubuntu 终端访问 Windows 文件，并对这些文件使用 Linux 应用和工具，反之亦然。 考虑到许多 Web 工具最初是针对 Linux 所编写的，并部署在 Linux 生产环境中，因此建议在 Linux 文件系统中进行 Python Web 开发。 这还可避免混合文件系统语义（如 Windows 在文件名方面不区分大小写）。 也就是说，WSL 现在支持在 Linux 与 Windows 文件系统之间跳转，因此可以将文件托管在其中一个系统上。 [了解详细信息](https://devblogs.microsoft.com/commandline/do-not-change-linux-files-using-windows-apps-and-tools/)。

## <a name="install-python-pip-and-venv"></a>安装 Python、pip 和 venv

Ubuntu 18.04 LTS 已安装了 Python 3.6，但不附带你可能期望随其他 Python 安装一起获得的某些模块。 我们仍需要安装 pip、Python 的标准包管理器和 venv（用于创建和管理轻型虚拟环境的标准模块）。  

1. 打开 Ubuntu 终端并输入 `python3 --version`，以便确认已安装了 Python3。 这应该返回 Python 版本号。 如果需要更新 Python 版本，请先通过输入以下内容来更新 Ubuntu 版本：`sudo apt update && sudo apt upgrade`，然后使用 `sudo apt upgrade python3` 更新 Python。

2. 通过输入以下内容来安装 pip：`sudo apt install python3-pip`。 通过 pip 可以安装和管理不属于 Python 标准库的其他包。

3. 通过输入以下内容来安装 venv：`sudo apt install python3-venv`。

## <a name="create-a-virtual-environment"></a>创建虚拟环境

对于 Python 开发项目，使用虚拟环境是推荐最佳做法。 通过创建虚拟环境，可以将项目工具隔离开来，避免与其他项目的工具发生版本冲突。 例如，你可能在维护一个需要 Django 1.2 Web 框架的旧 Web 项目，但随后会进行一个使用 Django 2.2 的令人兴奋的新项目。 如果在虚拟环境外部全局更新 Django，则以后可能会遇到一些版本控制问题。 除了防止意外的版本冲突以外，虚拟环境允许在没有管理权限的情况下安装和管理包。

1. 打开终端，在 HelloWorld 项目文件夹中，使用以下命令创建名为 .venv 的虚拟环境：`python3 -m venv .venv`。

2. 若要激活虚拟环境，请输入：`source .venv/bin/activate`。 如果它已正常工作，则应该在命令提示符之前看到 (.venv)。 现在已准备好了一个可用于编写代码和安装包的独立环境。 使用完虚拟环境后，输入以下命令可停用它：`deactivate`。

    ![创建虚拟环境](../images/wsl-venv.png)

> [!TIP]
> 建议在计划处理项目的目录中创建虚拟环境。 由于每个项目都应具有自己的单独目录，这样各自具有自己的虚拟环境，因此无需唯一命名。 建议使用名称 .venv 以遵循 Python 约定。 如果安装在项目目录中，则某些工具（如 pipenv）也会默认为此名称。 你不会希望使用 .env，因为这会与环境变量定义文件冲突。 通常不推荐使用非点开头的名称，因为不需要 `ls` 经常提醒目录已存在。 还建议将 .venv 添加到 .gitignore 文件。 （此处是[GitHub 用于 Python 的默认 gitignore 模板](https://github.com/github/gitignore/blob/50e42aa1064d004a5c99eaa72a2d8054a0d8de55/Python.gitignore#L99-L106)，可供参考。）有关在 VS Code 中使用虚拟环境的更多信息，请参阅[在 VS Code中使用 Python 环境](https://code.visualstudio.com/docs/python/environments)。

## <a name="open-a-wsl---remote-window"></a>打开 WSL - Remote 窗口

VS Code 使用 Remote - WSL 扩展（之前已安装）将 Linux 子系统视为远程服务器。 这使你可以使用 WSL 作为集成开发环境。 [了解详细信息](https://code.visualstudio.com/docs/remote/wsl)。

1. 通过输入以下内容从 Ubuntu 终端在 VS Code 中打开项目文件夹：`code .`（“.”告知 VS Code 打开当前文件夹）。

2. 一个安全警报会从 Windows Defender 弹出，选择“允许访问”。 VS Code 打开后，你应该在左下角看到远程连接主机指示器，让你知道正在“WSL:Ubuntu-18.04”上进行编辑。

    ![VS Code 远程连接主机指示器](../images/wsl-remote-extension.png)

3. 关闭 Ubuntu 终端。 接下来会使用集成到 VS Code 中的 WSL 终端。

4. 通过按 Ctrl+'（使用反撇号字符）或选择“视图” > “终端”，在 VS Code 中打开 WSL 终端。 这会打开一个指向在 Ubuntu 终端中创建的项目文件夹路径的 bash (WSL) 命令行。

    ![带有 WSL 终端的 VS Code](../images/vscode-bash-remote.png)

## <a name="install-the-microsoft-python-extension"></a>安装 Microsoft Python 扩展

需要安装 Remote - WSL 的所有 VS Code 扩展。 已在 VS Code 上本地安装的扩展不会自动可用。 [了解详细信息](https://code.visualstudio.com/docs/remote/wsl#_managing-extensions)。

1. 通过输入 Ctrl+Shift+X 来打开 VS Code 扩展窗口（或使用菜单导航到“视图” > “扩展”）。

2. 在顶部的“在应用商店中搜索扩展”框中，输入：Python。

3. 找到“Python (ms-python.python) by Microsoft”扩展，然后选择绿色的“安装”按钮。

4. 扩展安装完成后，需要选择蓝色的“需要重新加载”按钮。 这会重新加载 VS Code 并在 VS Code 扩展窗口（其中显示已安装 Python 扩展）中显示“WSL:UBUNTU-18.04 - 已安装”部分。

## <a name="run-a-simple-python-program"></a>运行一个简单 Python 程序

Python 是一种解释型语言，支持不同类型的解释器（Python2、Anaconda、PyPy 等）。 VS Code 应默认为与项目关联的解释器。 如果有理由需要更改解释器，请选择当前显示在 VS Code 窗口底部蓝色栏中的解释器，或打开“命令面板”(Ctrl+Shift+P)，然后输入命令“Python:选择解释器”。 这会显示当前已安装的 Python 解释器列表。 [详细了解如何配置 Python 环境](https://code.visualstudio.com/docs/python/environments)。

我们来创建并运行一个简单 Python 程序作为测试，并确保已选择正确的 Python 解释器。

1. 通过输入 Ctrl+Shift+E 来打开 VS Code 文件资源管理器窗口（或使用菜单导航到“视图” > “资源管理器”）。

2. 如果集成 WSL 终端尚未打开，请通过输入 Ctrl+Shift+' 来打开它，并确保选择 HelloWorld python 项目文件夹。

3. 通过输入以下内容来创建一个 python 文件：`touch test.py`。 你应看到刚才创建的文件出现在资源管理器窗口中的 .venv 和 .vscode 文件夹（已在项目目录中）下。

4. 选择刚才在资源管理器窗口中创建的 test.py 文件以在 VS Code 中打开它。 由于文件名中的 .py 向 VS Code 告知这是 Python 文件，因此之前加载的 Python 扩展会自动选择并加载 Python 解释器（会显示在 VS Code 窗口底部）。

    ![在 VS Code 中选择 Python 解释器](../images/interpreterselection.gif)

5. 将此 Python 代码粘贴到 test.py 文件中，然后保存该文件 (Ctrl+S)： 

    ```python
    print("Hello World")
    ```

6. 若要运行刚才创建的 Python“Hello World”程序，请在 VS Code 资源管理器窗口中选择“test.py”文件，然后右键单击该文件以显示选项菜单。 选择“在终端中运行 Python 文件”。 或者，在集成 WSL 终端窗口中，输入 `python test.py` 以运行“Hello World”程序。 Python 解释器会在终端窗口中打印“Hello World”。

祝贺你。 已全部设置好，可创建和运行 Python 程序！ 现在，我们来尝试使用最受欢迎的 Python Web 框架中的两个创建 Hello World 应用：Flask 和 Django。

## <a name="hello-world-tutorial-for-flask"></a>适用于 Flask 的 Hello World 教程

[Flask](http://flask.pocoo.org/) 是一种适用于 Python 的 Web 应用程序框架。 在此简要教程中，会使用 VS Code 和 WSL 创建一个小型“Hello World”Flask 应用。

1. 转到“开始”菜单（左下方 Windows 图标）并输入以下内容，以便打开 Ubuntu 18.04（WSL 命令行）：“Ubuntu 18.04”。

2. 为项目创建目录：`mkdir HelloWorld-Flask`，然后执行 `cd HelloWorld-Flask` 以进入该目录。

3. 创建虚拟环境以安装项目工具：`python3 -m venv .venv`

4. 通过输入以下命令，在 VS Code 中打开 HelloWorld-Flask 项目：`code .`

5. 在 VS Code 中打开集成 WSL 终端（也称为 Bash），具体方法是输入 Ctrl+Shift+'（应已选择 HelloWorld-Flask 项目文件夹）。 关闭 Ubuntu 命令行，因为我们接下来会在与 VS Code 集成的 WSL 终端中工作。

6. 在 VS Code 中使用 Bash 终端激活在步骤 #3 中创建的虚拟环境：`source .venv/bin/activate`。 如果它已正常工作，则应该在命令提示符之前看到 (.venv)。

7. 通过输入以下内容，在虚拟环境中安装 Flask：`python3 -m pip install flask`。 通过输入以下内容来验证它是否已安装：`python3 -m flask --version`。

8. 为 Python 代码创建新文件：`touch app.py`

9. 在 VS Code 的文件资源管理器中打开 app.py 文件（`Ctrl+Shift+E`，然后选择 app.py 文件）。 这会激活 Python 扩展以选择解释器。 它应默认为“Python 3.6.8 64 位('.venv': venv)”。 请注意，它还会检测到虚拟环境。

    ![激活虚拟环境](../images/virtual-environment.png)

10. 在 app.py 中，添加代码以导入 Flask 并创建 Flask 对象的实例：

    ```python
    from flask import Flask
    app = Flask(__name__)
    ```

11. 此外在 app.py 中，添加一个返回内容（在本例中为简单字符串）的函数。 使用 Flask 的 app.route 修饰器将 URL 路由“/”映射到该函数：

    ```python
    @app.route("/")
    def home():
        return "Hello World! I'm using Flask."
    ```

    > [!TIP]
    > 可以根据映射到相同函数的不同路由的数量，对相同函数使用多个修饰器（每行一个）。

12. 保存 app.py 文件 (Ctrl+S)。

13. 在终端中，输入以下命令来运行应用：

    ```python
    python3 -m flask run
    ```

    这会运行 Flask 开发服务器。 默认情况下，开发服务器会查找 app.py。 运行 Flask 时，应看到类似于以下内容的输出：

    ```bash
    (env) user@USER:/mnt/c/Projects/HelloWorld$ python3 -m flask run
     * Environment: production
       WARNING: This is a development server. Do not use it in a production deployment.
       Use a production WSGI server instead.
     * Debug mode: off
     * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
    ```

14. 打开默认 Web 浏览器到呈现的页面，在终端中 Ctrl+单击http://127.0.0.1:5000/ URL。 应在浏览器中看到以下消息：

    ![Hello, Flask!](../images/hello-flask.png)

15. 请注意，在访问类似于“/”的 URL 时，调试终端中会出现一个消息，其中显示 HTTP 请求：

    ```bash
    127.0.0.1 - - [19/Jun/2019 13:36:56] "GET / HTTP/1.1" 200 -
    ```

16. 在终端中使用 Ctrl+C 停止应用。

> [!TIP]
> 如果要使用与 app.py 不同的文件名（如 program.py），请定义名为 FLASK_APP 的环境变量，并将其值设置为所选文件。 Flask 的开发服务器随后会使用 FLASK_APP 的值而不是默认文件 app.py。 有关更多信息，请参阅 [Flask 的命令行界面文档](http://flask.pocoo.org/docs/1.0/cli/)。

恭喜，你已使用 Visual Studio Code 和适用于 Linux 的 Windows 子系统创建了一个 Flask Web 应用程序！ 有关使用 VS Code 和 Flask 的更深入教程，请参阅 [Visual Studio Code中的 Flask 教程](https://code.visualstudio.com/docs/python/tutorial-flask)。

## <a name="hello-world-tutorial-for-django"></a>适用于 Django 的 Hello World 教程

[Django](https://www.djangoproject.com) 是一种适用于 Python 的 Web 应用程序框架。 在此简要教程中，会使用 VS Code 和 WSL 创建一个小型“Hello World”Django 应用。

1. 转到“开始”菜单（左下方 Windows 图标）并输入以下内容，以便打开 Ubuntu 18.04（WSL 命令行）：“Ubuntu 18.04”。

2. 为项目创建目录：`mkdir HelloWorld-Django`，然后执行 `cd HelloWorld-Django` 以进入该目录。

3. 创建虚拟环境以安装项目工具：`python3 -m venv .venv`

4. 通过输入以下命令，在 VS Code 中打开 HelloWorld-DJango 项目：`code .`

5. 在 VS Code 中打开集成 WSL 终端（也称为 Bash），具体方法是输入 Ctrl+Shift+'（应已选择 HelloWorld-Django 项目文件夹）。 关闭 Ubuntu 命令行，因为我们接下来会在与 VS Code 集成的 WSL 终端中工作。

6. 在 VS Code 中使用 Bash 终端激活在步骤 #3 中创建的虚拟环境：`source .venv/bin/activate`。 如果它已正常工作，则应该在命令提示符之前看到 (.venv)。

7. 使用以下在虚拟环境中安装 Django：`python3 -m pip install django`。 通过输入以下内容来验证它是否已安装：`python3 -m django --version`。

8. 接下来，运行以下命令来创建 Django 项目：

    ```bash
    django-admin startproject web_project .
    ```

    `startproject` 命令假设（通过在末尾使用 `.`）当前文件夹是项目文件夹，并在其中创建以下内容：

    - `manage.py`：项目的 Django 命令行管理实用工具。 使用 `python manage.py <command> [options]` 为项目运行管理命令。

    - 一个名为 `web_project` 的子文件夹，其中包含以下文件：
        - `__init__.py`：一个空文件，向 Python 告知此文件夹是 Python 包。
        - `wsgi.py`：供与 WSGI 兼容的 Web 服务器为项目提供服务的入口点。 通常将此文件原样保留，因为它为生产 Web 服务器提供挂钩。
        - `settings.py`：包含 Django 项目的设置，可以在开发 Web 应用的过程中进行修改。
        - `urls.py`：包含 Django 项目的目录，也可以在开发过程中进行修改。

9. 若要验证 Django 项目，请使用命令 `python3 manage.py runserver` 启动 Django 的开发服务器。 服务器在默认端口 8000 上运行，应会在终端窗口中看到类似于以下输出的输出：

    ```output
    Performing system checks...

    System check identified no issues (0 silenced).

    June 20, 2019 - 22:57:59
    Django version 2.2.2, using settings 'web_project.settings'
    Starting development server at http://127.0.0.1:8000/
    Quit the server with CONTROL-C.
    ```

    首次运行服务器时，它会在文件 `db.sqlite3` 中创建默认 SQLite 数据库，该数据库旨在用于开发，但是可以在生产中用于低容量 Web 应用。 此外，Django 的内置 Web 服务器旨在仅用于本地开发。 但是在部署到 Web 主机时，Django 会改用主机的 Web 服务器。 Django 项目中的 `wsgi.py` 模块负责挂钩到生产服务器。

    如果要使用与默认值 8000 不同的端口，请在命令行中指定端口号，如 `python3 manage.py runserver 5000`。

10. 在终端输出窗口中 `Ctrl+click``http://127.0.0.1:8000/` URL，以将默认浏览器打开到该地址。 如果 Django 安装正确且项目有效，则会看到默认页面。 VS Code 终端输出窗口还会显示服务器日志。

11. 完成后，关闭浏览器窗口，并按照终端输出窗口中的指示使用 `Ctrl+C` 在 VS Code 中停止服务器。

12. 现在，若要创建 Django 应用，请在项目文件夹（`manage.py` 驻留的位置）中运行管理实用工具的 `startapp` 命令：

    ```bash
    python3 manage.py startapp hello
    ```

    该命令会创建名为 `hello` 的文件夹，其中包含一些代码文件和一个子文件夹。 在这些文件下，会经常使用 `views.py` （包含用于定义 Web 应用中的页面的函数）和 `models.py`（包含用于定义数据对象的类）。 `migrations` 文件夹由 Django 的管理实用工具用于管理数据库版本，如本教程后面部分所述。 还有文件 `apps.py`（应用配置）、`admin.py`（用于创建管理界面）和 `tests.py` （用于测试）未在此处进行介绍。

13. 修改 `hello/views.py` 以匹配以下代码，这会为应用的主页创建单个视图：

    ```python
    from django.http import HttpResponse

    def home(request):
        return HttpResponse("Hello, Django!")
    ```

14. 使用以下内容创建文件 `hello/urls.py`。 `urls.py` 文件用于指定模式，以将不同 URL 路由到相应的视图。 下面的代码包含一个路由，用于将应用的根 URL（`""`）映射到刚才添加到 `hello/views.py` 的 `views.home` 函数：

    ```python
    from django.urls import path
    from hello import views

    urlpatterns = [
        path("", views.home, name="home"),
    ]
    ```

15. `web_project` 文件夹还包含 `urls.py` 文件，该文件是实际处理 URL 路由的位置。 打开 `web_project/urls.py` 并进行修改以匹配以下代码（如果需要，可以保留指导注释）。 此代码使用 `django.urls.include` 拉取应用的 `hello/urls.py`（这会使应用的路由包含在应用中）。 当项目包含多个应用时，此分隔会非常有用。

    ```python
    from django.contrib import admin
    from django.urls import include, path

    urlpatterns = [
        path("", include("hello.urls")),
    ]
    ```

16. 保存所有已修改的文件。

17. 在 VS Code 终端中，使用 `python3 manage.py runserver` 运行开发服务器，并打开浏览器到 `http://127.0.0.1:8000/` 以查看呈现“Hello, Django”的页面。

恭喜，你已使用 VS Code 和适用于 Linux 的 Windows 子系统创建了一个 Django Web 应用程序！ 有关使用 VS Code 和 Django 的更深入教程，请参阅 [Visual Studio Code中的 Django 教程](https://code.visualstudio.com/docs/python/tutorial-django)。

## <a name="additional-resources"></a>其他资源

- [VS Code 中的 Python 教程](https://code.visualstudio.com/docs/python/python-tutorial)：有关将 VS Code 作为 Python 环境的介绍性教程，主要介绍如何编辑、运行和调试代码。
- [VS Code 中的 Git 支持](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support)：了解如何在 VS Code 中使用 Git 版本控制基础知识。  
- [推出的 WSL 2 更新！](https://docs.microsoft.com/windows/wsl/wsl2-index)：此新版本更改了 Linux 发行版本与 Windows 的交互方式，从而提高了文件系统性能并增加了完整系统调用兼容性。
- [ Windows 上使用多个 Linux 发行版本](https://docs.microsoft.com/windows/wsl/wsl-config)：了解如何在 Windows 计算机上管理多个不同的 Linux 发行版本。
