---
title: Windows 上的 Python Web 开发
description: 如何开始在 Windows 上使用 Python 进行 web 开发, 包括针对 Flask 和 Django 等框架进行设置。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: windows 上的 python、windows 10、microsoft、python、具有 wsl 的 python web 应用、适用于 linux 的 python web 应用、windows 上的 python web 开发、windows 上的 flask 应用、windows 上的 django 应用、windows 上的应用、python web、windows 上的 flask web devwindows web dev with python, vs code python web dev, remote wsl extension, ubuntu, wsl, venv, pip, microsoft python extension, 在 windows 上运行 python, 在 windows 上使用 python, 在 windows 上使用 python 生成
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: fa6da9f5151d9457aafd255c9d10c91e3d219cee
ms.sourcegitcommit: a28a32fff9d15ecf4a9d172cd0a04f4d993f9d76
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/12/2019
ms.locfileid: "68959078"
---
# <a name="get-started-using-python-for-web-development-on-windows"></a>开始在 Windows 上使用 Python 进行 web 开发

下面是使用适用于 Linux 的 Windows 子系统 (WSL) 在 Windows 上开始使用 Python 进行 web 开发的循序渐进指南。

## <a name="set-up-your-development-environment"></a>设置开发环境

我们建议在生成 web 应用程序时在 WSL 上安装 Python。 Python web 开发的许多教程和说明都是针对 Linux 用户编写的, 并使用基于 Linux 的打包和安装工具。 大多数 web 应用还部署在 Linux 上, 因此, 这将确保你的开发环境与生产环境之间的一致性。

如果你使用的是 web 开发以外的其他内容, 则我们建议你使用 Microsoft Store 直接在 Windows 10 上安装 Python。 WSL 不支持 GUI 桌面或应用程序 (如 PyGame、Gnome、KDE 等)。 在这些情况下, 请在 Windows 上直接安装并使用 Python。 如果你不熟悉 Python, 请参阅以下指南:[开始在 Windows 上使用 Python](./python-for-education.md)。 如果你有兴趣自动执行操作系统上的常见任务, 请参阅以下指南:[开始在 Windows 上使用 Python 进行脚本编写和自动化](./python-for-scripting.md)。 对于某些高级方案, 你可能需要考虑直接从[python.org](https://www.python.org/downloads/windows/)下载特定的 Python 版本, 或考虑安装[备用](https://www.python.org/download/alternatives)项, 例如 Anaconda、Jython、PyPy、WinPython、IronPython 等。仅当你是更高级的 Python 程序员时, 才建议使用此方法, 具体原因是选择替代实现。

## <a name="enable-windows-subsystem-for-linux"></a>启用适用于 Linux 的 Windows 子系统

通过 WSL, 你可以运行 GNU/Linux 环境 (包括大多数命令行工具、实用工具和应用程序), 直接在 Windows 上进行修改, 并与 Windows 文件系统和常用工具 (如 Visual Studio Code) 完全集成。 在启用 WSL 之前, 请检查你是否拥有[最新版本的 Windows 10](https://www.microsoft.com/software-download/windows10)。

若要在您的计算机上启用 WSL, 您需要:

1. 转到 "**开始**" 菜单 (左下方的窗口图标), 键入 "打开或关闭 windows 功能", 然后选择指向 "**控制面板**" 的链接以打开 " **windows 功能**" 弹出菜单。 在列表中找到 "适用于 Linux 的 Windows 子系统" 并选中复选框以启用该功能。

2. 出现提示时重新启动计算机。

## <a name="install-a-linux-distribution"></a>安装 Linux 分发版

有多个 Linux 分发可在 WSL 上运行。 可以在 Microsoft Store 中查找和安装收藏夹。 建议从[Ubuntu 18.04 LTS](https://www.microsoft.com/store/productId/9N9TNGVNDL3Q)开始, 因为它是最新的、受欢迎的并且很受支持。

1. 打开此[Ubuntu 18.04 LTS](https://www.microsoft.com/store/productId/9N9TNGVNDL3Q)链接, 打开 Microsoft Store, 然后选择 "**获取**"。 *(这是一个相当大的下载, 可能需要一段时间才能安装。)*

2. 下载完成后, 在 "**开始**" 菜单中键入 "UBUNTU 18.04 LTS", 从 Microsoft Store 或 "启动" 中选择 "**启动**"。

3. 首次运行分发时, 系统将要求你创建帐户名称和密码。 在此之后, 默认情况下, 你将以此用户的身份自动登录。 您可以选择任何用户名和密码。 它们不会影响你的 Windows 用户名。

可以通过输入以下内容来检查当前使用的 Linux 分发版: `lsb_release -d`。 若要更新 Ubuntu 分发, 请使用`sudo apt update && sudo apt upgrade`:。 建议定期更新以确保具有最新的包。 Windows 不会自动处理此更新。 有关适用于 Microsoft Store、替代安装方法或故障排除的其他 Linux 发行版的链接, 请参阅[适用于 windows 10 的适用于 Linux 的 Windows 子系统安装指南](https://docs.microsoft.com/windows/wsl/install-win10)。

## <a name="set-up-visual-studio-code"></a>设置 Visual Studio Code

通过使用 VS Code, 充分利用[IntelliSense](https://code.visualstudio.com/docs/editor/intellisense)、 [Linting](https://code.visualstudio.com/docs/python/linting)、[调试支持](https://code.visualstudio.com/docs/python/debugging)、[代码片段](https://code.visualstudio.com/docs/editor/userdefinedsnippets)和[单元测试](https://code.visualstudio.com/docs/python/unit-testing)。 VS Code 与适用于 Linux 的 Windows 子系统完美集成, 提供[内置终端](https://code.visualstudio.com/docs/editor/integrated-terminal)在代码编辑器和命令行之间建立无缝的工作流, 此外还支持使用通用 Git[进行版本控制的 git](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support)直接内置于 UI 中的命令 (添加、提交、推送、拉取)。

1. [下载并安装适用于 Windows 的 VS Code](https://code.visualstudio.com)。 VS Code 也适用于 Linux, 但适用于 Linux 的 Windows 子系统不支持 GUI 应用, 因此我们需要在 Windows 上安装它。 不用担心, 你仍可以使用远程-WSL 扩展与 Linux 命令行和工具集成。

2. 在 VS Code 上安装[WSL 扩展](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl)。 这使你可以将 WSL 用作集成开发环境, 并将为你处理兼容性和路径。 [了解详情](https://code.visualstudio.com/docs/remote/remote-overview)。

> [!IMPORTANT]
> 如果已安装 VS Code, 则需要确保将[1.35 发布](https://code.visualstudio.com/updates/v1_35)或更高版本, 以便安装[远程 WSL 扩展](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl)。 建议不要在不使用 WSL 扩展的 VS Code 中使用 WSL, 因为将失去对自动完成、调试、linting 等的支持。趣味事实:此 WSL 扩展安装在 $HOME/.vscode-server/extensions。

## <a name="create-a-new-project"></a>创建新项目

让我们在 Linux (Ubuntu) 文件系统上创建一个新的项目目录, 然后, 我们将使用 VS Code 来处理 Linux 应用和工具。

1. 转到 "**开始**" 菜单 (左下方的窗口图标), 然后键入以下内容, 关闭 VS Code 并打开 Ubuntu 18.04 (你的 WSL 命令行):"Ubuntu 18.04"。

2. 在 Ubuntu 命令行中, 导航到要在其中放置项目的位置, 并为其创建目录: `mkdir HelloWorld`。

![Ubuntu 终端](../../images/ubuntu-terminal.png)

> [!TIP]
> 使用适用于 Linux 的 Windows 子系统 (WSL) 时, 要记住的重要一点是,**你现在在两个不同的文件系统之间工作**:1) 你的 Windows 文件系统 (WSL) 是你的 Linux 文件系统 (), 它是示例的 Ubuntu。 需要注意安装包和存储文件的位置。 你可以在 Windows 文件系统中安装工具或包的一个版本, 并在 Linux 文件系统中安装完全不同的版本。 更新 Windows 文件系统中的工具将对 Linux 文件系统中的工具无效, 反之亦然。 WSL 会将计算机上的固定驱动器装载到 Linux<drive>发行版中的/mnt/文件夹下。 例如, Windows C: 驱动器安装在下`/mnt/c/`。 可以从 Ubuntu 终端访问 Windows 文件, 并对这些文件使用 Linux 应用和工具, 反之亦然。 建议在适用于 Python web 开发的 Linux 文件系统中工作, 因为最初为 Linux 编写了大部分 web 工具, 并在 Linux 生产环境中进行了部署。 它还避免了混合文件系统语义 (如 Windows 在文件名上不区分大小写)。 也就是说, WSL 现在支持在 Linux 和 Windows 文件系统之间跳转, 因此你可以将文件托管在其中的系统上。 [了解详情](https://devblogs.microsoft.com/commandline/do-not-change-linux-files-using-windows-apps-and-tools/)。 我们也很高兴地分享了[WSL2 即将推出的 Windows](https://devblogs.microsoft.com/commandline/wsl-2-is-now-available-in-windows-insiders/) , 并会提供一些重大改进。 你[现在可以在 Windows 预览体验内部版本18917上试用](https://docs.microsoft.com/windows/wsl/wsl2-install)。

## <a name="install-python-pip-and-venv"></a>安装 Python、pip 和 venv

Ubuntu 18.04 LTS 附带了 Python 3.6, 但不附带一些你可能希望在其他 Python 安装中获得的模块。 我们仍需要安装**pip**、用于 Python 的标准包管理器和**venv**, 这是用于创建和管理轻型虚拟环境的标准模块。  

1. 打开 Ubuntu 终端并输入: `python3 --version`, 确认已安装 Python3。 这应会返回 Python 版本号。 如果需要更新你的 Python 版本, 请首先输入: `sudo apt update && sudo apt upgrade`, 然后使用`sudo apt upgrade python3`更新 Python 来更新 Ubuntu 版本。

2. 通过输入以下内容来`sudo apt install python3-pip`安装 pip:。 Pip 允许你安装和管理不属于 Python 标准库的其他包。

3. 输入以下内容安装 venv `sudo apt install python3-venv`:。

## <a name="create-a-virtual-environment"></a>创建虚拟环境

对于 Python 开发项目, 建议使用虚拟环境。 通过创建虚拟环境, 你可以将项目工具隔离开来, 并避免与其他项目的工具存在版本冲突。 例如, 你可能需要维护一个需要 Django 1.2 web 框架的旧 web 项目, 但随后使用 Django 2.2 就会获得令人兴奋的新项目。 如果在虚拟环境外全局更新 Django, 以后可能会遇到一些版本控制问题。 除了防止意外的版本控制冲突以外, 虚拟环境允许您在没有管理权限的情况下安装和管理包。

1. 打开终端, 并在*HelloWorld*项目文件夹中使用以下命令创建名为的虚拟环境 **。 venv**: `python3 -m venv .venv`。

2. 若要激活虚拟环境, 请输入`source .venv/bin/activate`:。 如果它有效, 你应该在命令提示符之前看到 **(. venv)** 。 现在, 你有了一个可供编写代码和安装包的独立环境。 完成虚拟环境后, 请输入以下命令将其停用: `deactivate`。

    ![创建虚拟环境](../../images/wsl-venv.png)

> [!TIP]
> 建议在计划项目的目录中创建虚拟环境。 由于每个项目都应具有自己的单独目录, 因此, 每个项目都具有自己的虚拟环境, 因此无需唯一命名。 我们建议使用**venv**来遵循 Python 约定。 如果安装在项目目录中, 某些工具 (如 pipenv) 也默认为此名称。 不希望使用与环境变量定义文件冲突的**env。** 通常不建议非点前导名称, 因为不需要`ls`持续提醒您该目录存在。 我们还建议将 **. venv**添加到 .gitignore 文件。 (下面是适用于[Python 的 GitHub 默认 .gitignore 模板](https://github.com/github/gitignore/blob/50e42aa1064d004a5c99eaa72a2d8054a0d8de55/Python.gitignore#L99-L106)供参考。)有关在 VS Code 中使用虚拟环境的详细信息, 请参阅[在 VS Code 中使用 Python 环境](https://code.visualstudio.com/docs/python/environments)。

## <a name="open-a-wsl---remote-window"></a>打开 WSL-远程窗口

VS Code 使用远程 WSL 扩展 (之前安装) 将 Linux 子系统视为远程服务器。 这使你可以使用 WSL 作为集成开发环境。 [了解详情](https://code.visualstudio.com/docs/remote/wsl)。 

1. 输入以下内容, 从 Ubuntu 终端 VS Code 打开项目文件夹: `code .` ("." 告诉 VS Code 打开当前文件夹)。

2. 将从 Windows Defender 弹出一个安全警报, 并选择 "允许访问"。 打开 VS Code 后, 你应该会在左下角看到远程连接主机指示器, 让你知道在 WSL 上**进行编辑:Ubuntu-18.04**。

    ![VS Code 远程连接主机指示器](../../images/wsl-remote-extension.png)

3. 关闭 Ubuntu 终端。 向前移动将使用集成到 VS Code 中的 WSL 终端。

4. 通过按**Ctrl + '** (使用反撇号字符) 或选择 "**查看** > **终端**", 在 VS Code 中打开 WSL 终端。 这会打开一个 bash (WSL) 命令行, 此命令行打开到你在 Ubuntu 终端中创建的项目文件夹路径。

    ![VS Code 与 WSL 终端](../../images/vscode-bash-remote.png)

## <a name="install-the-microsoft-python-extension"></a>安装 Microsoft Python 扩展

你将需要安装 WSL 的任何 VS Code 扩展。 已在 VS Code 本地安装的扩展将无法自动使用。 [了解详情](https://code.visualstudio.com/docs/remote/wsl#_managing-extensions)。

1. 通过输入**Ctrl + Shift + X**打开 "VS Code 扩展" 窗口 (或使用菜单导航到 "**视图** > " "**扩展**")。

2. 在 "Marketplace 的顶级**搜索扩展**" 框中, 输入:**Python**。

3. **通过 Microsoft 扩展查找 python (ms python python)** , 并选择 "绿色**安装**" 按钮。

4. 扩展安装完成后, 你将需要选择 "**需要重新加载**" 按钮。 这会重载 VS Code 并显示**WSL:"VS Code 扩展" 窗口**中的 "UBUNTU-18.04-已安装" 部分显示已安装 Python 扩展。

## <a name="run-a-simple-python-program"></a>运行简单的 Python 程序

Python 是一种解释型语言, 支持不同类型的 interpretors (Python2、Anaconda、PyPy 等)。 VS Code 应默认为与项目关联的解释器。 如果有理由更改它, 请选择 "VS Code" 窗口底部蓝色栏中当前显示的解释器, 或打开**命令面板**(Ctrl + Shift + P), 然后输入以下命令 **:选择解释**器。 这会显示当前已安装的 Python 解释器列表。 [详细了解如何配置 Python 环境](https://code.visualstudio.com/docs/python/environments)。

让我们创建并运行一个简单的 Python 程序作为测试, 并确保已选择正确的 Python 解释器。

1. 通过输入**Ctrl + Shift + E**打开 "VS Code 文件资源管理器" 窗口 (或使用菜单导航到 "**视图** > **资源管理器**")。

2. 如果它尚未打开, 请按**Ctrl + Shift + '** 打开集成的 WSL 终端, 并确保已选中**HelloWorld** python 项目文件夹。

3. 通过输入以下内容创建 python 文件`touch test.py`:。 你应看到刚才创建的文件显示在你的 "资源管理器" 窗口中的 venv 和. vscode 文件夹下的项目目录中。

4. 选择刚在 "资源管理器" 窗口中创建的**test.py**文件, 在 VS Code 中打开它。 由于文件名中的 py 告诉 VS Code 这是一个 Python 文件, 因此你之前加载的 Python 扩展会自动选择并加载一个 Python 解释器, 该解释器将显示在 VS Code 窗口的底部。

    ![在 VS Code 中选择 Python 解释器](../../images/interpreterselection.gif)

5. 将此 Python 代码粘贴到你的 test.py 文件中, 然后保存该文件 (Ctrl + S): 

    ```python
    print("Hello World")
    ```

6. 若要运行我们刚刚创建的 Python "Hello World" 程序, 请在 "VS Code 资源管理器" 窗口中选择**test.py**文件, 然后右键单击该文件以显示选项的菜单。 选择 **"在终端中运行 Python 文件"** 。 或者, 在集成的 WSL 终端窗口中, 输入`python test.py` : 运行 "Hello World" 程序。 Python 解释器会在终端窗口中打印 "Hello World"。

恭喜. 一切都已设置为创建和运行 Python 程序! 现在, 让我们尝试使用两个最受欢迎的 Python web 框架创建 Hello World 应用:Flask 和 Django。

## <a name="hello-world-tutorial-for-flask"></a>Flask Hello World 教程

[Flask](http://flask.pocoo.org/)是适用于 Python 的 web 应用程序框架。 在此 brief 教程中, 你将使用 VS Code 和 WSL 创建一个小型的 "Hello World" Flask 应用。

1. 转到 "**开始**" 菜单 (左下方的窗口图标), 然后键入以下内容, 打开 Ubuntu 18.04 (你的 WSL 命令行):"Ubuntu 18.04"。

2. 为项目创建目录: `mkdir HelloWorld-Flask`, 然后`cd HelloWorld-Flask`输入目录。

3. 创建虚拟环境以安装项目工具:`python3 -m venv .venv`

4. 输入以下命令, 在 VS Code 中打开**HelloWorld Flask**项目:`code .`

5. 在 VS Code 中, 通过输入**Ctrl + Shift + '** (您的**HelloWorld-Flask**项目文件夹应已选中) 打开集成 WSL 终端 (也称为 Bash)。 *关闭 Ubuntu 命令行, 因为我们将在与 VS Code 前进的 WSL 终端中进行工作。*

6. 使用 VS Code 中的 Bash 终端激活在步骤 #3 中创建的虚拟环境: `source .venv/bin/activate`。 如果它有效, 你应该在命令提示符之前看到 (. venv)。

7. 输入以下内容, 在虚拟环境中安装`python3 -m pip install flask`Flask:。 通过输入以下内容来验证它是否`python3 -m flask --version`已安装:。

8. 为 Python 代码创建新文件:`touch app.py`

9. 在 VS Code的文件资源管理器 (`Ctrl+Shift+E`, 然后选择 app.py 文件) 中打开 app.py 文件。 这会激活 Python 扩展以选择解释器。 它应默认为**Python 3.6.8 64 位 (venv): venv)** 。 请注意, 它还检测到你的虚拟环境。

    ![激活的虚拟环境](../../images/virtual-environment.png)

10. 在**app.py**中, 添加代码以导入 Flask 并创建 Flask 对象的实例:

    ```python
    from flask import Flask
    app = Flask(__name__)
    ```

11. 同样, 在**app.py**中, 添加一个返回内容的函数, 在本例中为简单字符串。 使用 Flask 的**应用程序。路由**修饰器将 URL 路由 "/" 映射到该函数:

    ```python
    @app.route("/")
    def home():
        return "Hello World! I'm using Flask."
    ```

    > [!TIP]
    > 对于同一函数, 可以使用多个修饰器, 每行一个, 具体取决于要映射到同一个函数的不同路由的数量。

12. 保存**app.py**文件 (**Ctrl + S**)。

13. 在终端中, 输入以下命令运行应用:

    ```python
    python3 -m flask run
    ```

    这将运行 Flask 开发服务器。 默认情况下, 开发服务器将查找**app.py** 。 运行 Flask 时, 应会看到类似于下面的输出:

    ```bash
    (env) user@USER:/mnt/c/Projects/HelloWorld$ python3 -m flask run
     * Environment: production
       WARNING: This is a development server. Do not use it in a production deployment.
       Use a production WSGI server instead.
     * Debug mode: off
     * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
    ```

14. 打开默认 web 浏览器到呈现的页面, 然后在终端中 http://127.0.0.1:5000/ 按**Ctrl 并单击**该 URL。 你应在浏览器中看到以下消息:

    ![你好, Flask!](../../images/hello-flask.png)

15. 请注意, 在访问类似于 "/" 的 URL 时, 调试终端中会出现一条消息, 其中显示了 HTTP 请求:

    ```bash
    127.0.0.1 - - [19/Jun/2019 13:36:56] "GET / HTTP/1.1" 200 -
    ```

16. 在终端中使用**Ctrl + C**停止应用。

> [!TIP]
> 如果要使用与**app.py**不同的文件名 (如**program.py**), 请定义名为**FLASK_APP**的环境变量, 并将其值设置为所选的文件。 然后, Flask 的开发服务器使用**FLASK_APP**的值, 而不使用默认的文件**app.py**。 有关详细信息, 请参阅[Flask 的命令行接口文档](http://flask.pocoo.org/docs/1.0/cli/)。

恭喜, 你已使用 Visual Studio Code 和适用于 Linux 的 Windows 子系统创建了 Flask web 应用程序! 有关使用 VS Code 和 Flask 的更深入教程, 请参阅[Visual Studio Code 中的 Flask 教程](https://code.visualstudio.com/docs/python/tutorial-flask)。

## <a name="hello-world-tutorial-for-django"></a>Django Hello World 教程

[Django](https://www.djangoproject.com)是适用于 Python 的 web 应用程序框架。 在此 brief 教程中, 你将使用 VS Code 和 WSL 创建一个小型的 "Hello World" Django 应用。

1. 转到 "**开始**" 菜单 (左下方的窗口图标), 然后键入以下内容, 打开 Ubuntu 18.04 (你的 WSL 命令行):"Ubuntu 18.04"。

2. 为项目创建目录: `mkdir HelloWorld-Django`, 然后`cd HelloWorld-Django`输入目录。

3. 创建虚拟环境以安装项目工具:`python3 -m venv .venv`

4. 输入以下命令, 在 VS Code 中打开**HelloWorld DJango**项目:`code .`

5. 在 VS Code 中, 通过输入**Ctrl + Shift + '** (您的**HelloWorld-Django**项目文件夹应已选中) 打开集成 WSL 终端 (也称为 Bash)。 *关闭 Ubuntu 命令行, 因为我们将在与 VS Code 前进的 WSL 终端中进行工作。*

6. 使用 VS Code 中的 Bash 终端激活在步骤 #3 中创建的虚拟环境: `source .venv/bin/activate`。 如果它有效, 你应该在命令提示符之前看到 (. venv)。

7. 在虚拟环境中安装 Django 命令: `python3 -m pip install django`。 通过输入以下内容来验证它是否`python3 -m django --version`已安装:。

8. 接下来, 运行以下命令以创建 Django 项目:

    ```bash
    django-admin startproject web_project .
    ```

    该`startproject`命令假定`.`当前文件夹是项目文件夹, 并在该文件夹中创建以下内容:

    - `manage.py`：项目的 Django 命令行管理实用工具。 使用`python manage.py <command> [options]`为项目运行管理命令。

    - 名为`web_project`的子文件夹, 其中包含以下文件:
        - `__init__.py`: 指示 Python 的空文件此文件夹是一个 Python 包。
        - `wsgi.py`: 用于 WSGI 的 web 服务器的入口点, 用于为你的项目提供服务。 通常将此文件保留原样, 因为它提供了用于生产 web 服务器的挂钩。
        - `settings.py`: 包含 Django 项目的设置, 你可以在开发 web 应用过程中修改这些设置。
        - `urls.py`: 包含 Django 项目的目录, 你还可以在开发过程中对其进行修改。

9. 若要验证 Django 项目, 请使用命令`python3 manage.py runserver`启动 Django 的开发服务器。 服务器在默认端口8000上运行, 你应该会在终端窗口中看到类似于以下输出的输出:

    ```output
    Performing system checks...

    System check identified no issues (0 silenced).

    June 20, 2019 - 22:57:59
    Django version 2.2.2, using settings 'web_project.settings'
    Starting development server at http://127.0.0.1:8000/
    Quit the server with CONTROL-C.
    ```

    首次运行服务器时, 它会在文件`db.sqlite3`中创建一个默认的 SQLite 数据库, 该数据库用于开发目的, 但可用于在生产中用于低容量 web 应用。 此外, Django 的内置 web 服务器*仅*用于本地开发目的。 但是, 当你部署到 web 主机时, Django 将改用主机的 web 服务器。 Django `wsgi.py`项目中的模块负责挂钩到生产服务器。

    如果要使用不同于默认8000的端口, 请在命令行中指定端口号, 例如`python3 manage.py runserver 5000`。

10. `Ctrl+click`" `http://127.0.0.1:8000/`终端输出" 窗口中的 URL, 用于将默认浏览器打开到该地址。 如果 Django 安装正确且项目有效, 你将看到默认页面。 "VS Code 终端输出" 窗口还显示服务器日志。

11. 完成后, 请关闭浏览器窗口, 并使用`Ctrl+C`中所述的在 VS Code 中停止服务器。

12. 现在, 若要创建 Django 应用, 请在项目文件夹 ( `startapp`其中`manage.py`驻留) 中运行管理实用工具的命令:

    ```bash
    python3 manage.py startapp hello
    ```

    命令创建一个名`hello`为的文件夹, 该文件夹包含多个代码文件和一个子文件夹。 在这些情况下, 经常使用`views.py` (包含用于定义 web 应用中的页的函数) 和`models.py` (包含定义数据对象的类)。 此`migrations`文件夹由 Django 的管理实用工具用来管理数据库版本, 如本教程后面部分所述。 此外, 还提供了`apps.py`文件 (应用配置`admin.py` ) (用于创建管理界面) 和`tests.py` (适用于测试), 此处未介绍这些文件。

13. 修改`hello/views.py`以匹配以下代码, 这将为应用的主页创建单个视图:

    ```python
    from django.http import HttpResponse

    def home(request):
        return HttpResponse("Hello, Django!")
    ```

14. 使用以下内容创建`hello/urls.py`文件。 在`urls.py`文件中指定模式, 将不同的 url 路由到相应的视图。 下面的代码包含一个路由, 用于将应用程序的根 URL`""`() 映射`views.home`到刚刚添加到`hello/views.py`的函数:

    ```python
    from django.urls import path
    from hello import views

    urlpatterns = [
        path("", views.home, name="home"),
    ]
    ```

15. 该`web_project`文件夹还包含一个`urls.py`文件, 该文件是 URL 路由的实际处理方式。 打开`web_project/urls.py`并对其进行修改以匹配以下代码 (如果需要, 可以保留指导注释)。 此代码会在应用程序`hello/urls.py` `django.urls.include`中拉入应用程序, 使应用程序中包含应用程序的路由。 当项目包含多个应用时, 此隔离非常有用。

    ```python
    from django.contrib import admin
    from django.urls import include, path

    urlpatterns = [
        path("", include("hello.urls")),
    ]
    ```

16. 保存所有已修改的文件。

17. 在 VS Code 终端中, 使用`python manage.py runserver`运行开发服务器, 并打开浏览器以`http://127.0.0.1:8000/`查看呈现 "Hello, Django" 的页面。

恭喜, 你已使用 VS Code 和适用于 Linux 的 Windows 子系统创建了 Django web 应用程序! 有关使用 VS Code 和 Django 的更深入教程, 请参阅[Visual Studio Code 中的 Django 教程](https://code.visualstudio.com/docs/python/tutorial-django)。

## <a name="additional-resources"></a>其他资源

- [VS Code 的 Python 教程](https://code.visualstudio.com/docs/python/python-tutorial):介绍如何 VS Code 为 Python 环境, 主要介绍如何编辑、运行和调试代码。
- [VS Code 中的 Git 支持](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support):了解如何在 VS Code 中使用 Git 版本控制基础知识。  
- [了解 WSL 2!](https://docs.microsoft.com/windows/wsl/wsl2-index):此新版本更改了 Linux 分发与 Windows 的交互方式, 增加了文件系统性能并增加了系统调用的完全兼容性。
- [在 Windows 上使用多个 Linux 分发](https://docs.microsoft.com/windows/wsl/wsl-config):了解如何在 Windows 计算机上管理多个不同的 Linux 分发。
