---
title: 在 WSL 2 上设置 NodeJS
description: 本指南可帮助你在适用于 Linux 的 Windows 子系统（WSL）上设置 node.js 开发环境。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS，node.js，windows 10，microsoft，学习 NodeJS，windows 上的节点，wsl 上的节点，windows 上的节点，在 windows 上安装节点，NodeJS with vs code，在 windows 上安装节点，在 windows 上进行开发，在 NODEJS 上安装节点，在 Windows 上安装节点适用于 Linux 的子系统
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: e5875f0bf7ce73d3615aa131d57c2384c73dd8a1
ms.sourcegitcommit: 60d2d15dd0d365f82e4e90e4bc34b40cf5b4a247
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2019
ms.locfileid: "72517836"
---
# <a name="set-up-your-nodejs-development-environment-with-wsl-2"></a>用 WSL 2 设置 node.js 开发环境

下面是一个循序渐进的指南，可帮助你使用适用于 Linux 的 Windows 子系统（WSL）设置 node.js 开发环境。 本指南当前要求安装并运行 Windows 预览体验预览版才能安装和使用[WSL 2](https://devblogs.microsoft.com/commandline/wsl-2-is-now-available-in-windows-insiders/)。 WSL 2 在 WSL 1 上的速度和性能显著提高，尤其是在 node.js 中。 用于 node.js web 开发的许多 npm 模块和教程是针对 Linux 用户编写的，并使用基于 Linux 的打包和安装工具。 大多数 web 应用还部署在 Linux 上，因此使用 WSL 2 将确保开发环境与生产环境之间的一致性。

> [!NOTE]
> 如果你在 Windows 上直接使用 node.js 或计划使用 Windows Server 生产环境，请参阅我们的指南，[直接在 windows 上设置 node.js 开发环境](./setup-on-windows.md)。

## <a name="install-windows-10-insider-preview-build"></a>安装 Windows 10 预览体验预览版本

1. **[安装最新版本的 Windows 10](https://www.microsoft.com/software-download/windows10)** ：选择 "**立即更新**" 以下载更新助手。 下载完成后，打开更新助手以查看当前是否正在运行最新版本的 Windows，如果不是，请选择 "助手" 窗口中的 "**立即更新**" 以更新您的计算机。 *（如果运行的是 Windows 10 的最新版本，则此步骤是可选的。）*

    ![Windows 更新助手](../images/windows-update-assistant2019.png)

2. **[转到 "开始 > 设置" > Windows 预览体验计划](ms-settings:windowsinsider)** "，然后选择"**开始**"，并**链接帐户**。

    ![Windows 预览体验计划设置](../images/windows-insider-program-settings.png)

3. **[注册为 Windows 预览体验](https://insider.windows.com/getting-started/#register)** 人员：如果未向预览体验计划注册，则需要使用[Microsoft 帐户](https://account.microsoft.com/account)执行此操作。

    ![Windows 预览体验注册](../images/windows-insider-account.png)

4. 选择接收**快速铃声**更新或**跳转到下一个 Windows 版本**内容。 确认并选择**稍后重新启动**。 在重新启动之前，需要更改几个附加设置。

    ![Windows 有问必答 Fast 环形](../images/windows-insider-fast.png)

## <a name="enable-windows-subsystem-for-linux-and-virtual-machine-platform"></a>启用适用于 Linux 和虚拟机平台的 Windows 子系统

1. 在仍处于**Windows 设置**中时，搜索 **"打开或关闭 windows 功能**"。
2. 出现**Windows 功能**列表后，滚动查找 "查找适用于 Linux 的**虚拟机平台**和**Windows 子系统**"，确保选中此复选框以启用这两者，然后选择 **"确定"** 。
3. 出现提示时，重启计算机。

    ![启用 Windows 功能](../images/windows-feature-settings.png)

## <a name="install-a-linux-distribution"></a>安装 Linux 分发版

有多个 Linux 分发可在 WSL 上运行。 可以在 Microsoft Store 中查找和安装收藏夹。 建议从[Ubuntu 18.04 LTS](https://www.microsoft.com/store/productId/9N9TNGVNDL3Q)开始，因为它是最新的、受欢迎的并且很受支持。

1. 打开此[Ubuntu 18.04 LTS](https://www.microsoft.com/store/productId/9N9TNGVNDL3Q)链接，打开 Microsoft Store，然后选择 "**获取**"。 *（这是一个相当大的下载，可能需要一段时间才能安装。）*

2. 下载完成后，在 "**开始**" 菜单中键入 "UBUNTU 18.04 LTS"，从 Microsoft Store 或 "启动" 中选择 "**启动**"。

3. 首次运行分发时，系统将要求你创建帐户名称和密码。 在此之后，默认情况下，你将以此用户的身份自动登录。 您可以选择任何用户名和密码。 它们不会影响你的 Windows 用户名。

    ![Microsoft Store 中的 Linux 分发版](../images/store-linux-distros.png)

可以通过输入以下内容来检查当前使用的 Linux 分发版： `lsb_release -dc`。 若要更新 Ubuntu 分发，请使用： `sudo apt update && sudo apt upgrade`。 建议定期更新以确保具有最新的包。 Windows 不会自动处理此更新。 有关适用于 Microsoft Store、替代安装方法或故障排除的其他 Linux 发行版的链接，请参阅[适用于 windows 10 的适用于 Linux 的 Windows 子系统安装指南](https://docs.microsoft.com/windows/wsl/install-win10)。

## <a name="install-wsl-2"></a>安装 WSL 2

WSL 2 是 WSL 中[体系结构的新版本](https://docs.microsoft.com/windows/wsl/wsl2-about)，它改变了 Linux 发行版与 Windows 交互的方式，提高了性能并增加了系统调用的完全兼容性。

1. 在 PowerShell 中，输入以下命令： `wsl -l`，查看计算机上已安装的 WSL 分发的列表。 现在，应会在此列表中看到 Ubuntu-18.04。
2. 现在，请输入以下命令： `wsl --set-version Ubuntu-18.04 2` 将 Ubuntu 安装设置为使用 WSL 2。
3. 验证每个已安装的分发版使用的 WSL 的版本： `wsl --list --verbose` （或 `wsl -l -v`）。

    ![适用于 Linux 的 Windows 子系统集版本](../images/wsl-versions.png)

> [!TIP]
> 你可以按照相同的说明（使用 PowerShell）将已安装的任何 Linux 分发版设置为 WSL 2，只需将 "Ubuntu-18.04" 更改为所需的已安装发行版的名称即可。 若要更改回 WSL 1，请运行与上面相同的命令，但将 "2" 替换为 "1"。  你还可以通过输入以下内容将 WSL 2 设置为任何新安装的分发的默认值 @no__t：

## <a name="install-nvm-nodejs-and-npm"></a>安装 nvm、node.js 和 npm

可以通过多种方式来安装 node.js。 建议使用版本管理器，因为版本的更改速度非常快。 您可能需要根据所使用的不同项目的需要在多个版本之间进行切换。 节点版本管理器（更常见地称为 nvm）是安装多个版本的 node.js 的最常见方法。 我们将逐步介绍安装 nvm 的步骤，然后使用它来安装 node.js 和 Node 包管理器（npm）。 在下一节中还将介绍[其他的版本管理器](#alternative-version-managers)。

> [!IMPORTANT]
> 在安装版本管理器之前，始终建议从操作系统中删除 node.js 或 npm 的任何现有安装，因为不同的安装类型可能会导致出现奇怪和混淆的冲突。 例如，可以随 Ubuntu 的 `apt-get` 命令一起安装的节点版本当前已过时。 有关删除以前安装的帮助，请参阅[如何从 ubuntu 删除 nodejs](https://askubuntu.com/questions/786015/how-to-remove-nodejs-from-ubuntu-16-04)。）

1. 打开 Ubuntu 18.04 命令行。
2. 安装卷（用于在命令行中通过 internet 下载内容的工具），其中包括： `sudo apt-get install curl`
3. 安装 nvm，其中包含： `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.34.0/install.sh | bash`
4. 若要验证安装，请输入： `command -v nvm` .。。如果你收到 "找不到命令" 或根本没有响应，请关闭当前终端，将其重新打开，然后重试。 有关[详细信息，请参阅 nvm github](https://github.com/nvm-sh/nvm)存储库。
5. 列出当前安装的节点版本（此时应为 "无"）： `nvm ls`

    ![显示无节点版本的 NVM 列表](../images/nvm-no-node.png)

6. 安装 node.js 的当前版本（用于测试最新的功能改进，但更有可能出现问题）： `nvm install node`
7. 安装 node.js 的最新稳定 LTS 版本（推荐）： `nvm install --lts`
8. 列出安装的节点的版本： `nvm ls` .。。现在应会看到刚刚安装的两个版本。

    ![显示 LTS 和当前节点版本的 NVM 列表](../images/nvm-node-installed.png)

9. 验证 node.js 是否已安装，并且当前默认版本为： `node --version`。 然后，验证是否已 npm，并使用： `npm --version` （还可以使用 `which node` 或 `which npm` 来查看用于默认版本的路径）。
10. 若要更改要用于项目的 node.js 版本，请创建新的项目目录 `mkdir NodeTest` 并输入目录 `cd NodeTest`，然后输入 `nvm use node` 以切换到当前版本，或 `nvm use --lts` 以切换到 LTS 版本。 你还可以使用已安装的任何其他版本的特定数量，如 `nvm use v8.2.1`。 （若要列出 node.js 可用的所有版本，请使用命令： `nvm ls-remote`）。

> [!TIP]
> 如果使用 NVM 安装 node.js 和 NPM，则不需要使用 SUDO 命令来安装新的包。

> [!NOTE]
> 发布时，NVM v 0.34.0 是可用的最新版本。 你可以在[GitHub 项目页上查看 NVM 的最新版本](https://github.com/nvm-sh/nvm)，并调整上述命令以包含最新版本。
使用卷安装较新版本的 NVM 将替换旧版本，并使已使用 NVM 的节点版本保持不变。 例如：`curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.0/install.sh | bash`

## <a name="alternative-version-managers"></a>备用版本管理器

尽管 nvm 目前是最常用的节点版本管理器，但有几个替代方法：

- [n](https://www.npmjs.com/package/n#installation)是一个长期 @no__t 1 的替代方法，它使用略微不同的命令完成相同的操作，并且通过 `npm` 而不是 bash 脚本来安装。
- [fnm](https://github.com/Schniz/fnm#using-a-script)是较新的版本管理器，其声明速度要快得多 `nvm`。 （它还使用[Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines/get-started/what-is-azure-pipelines?view=azure-devops)。）
- [Volta](https://github.com/volta-cli/volta#installing-volta)是 LinkedIn 团队的新版本管理器，用于声明提高速度和跨平台支持。
- [asdf](https://asdf-vm.com/#/core-manage-asdf-vm)是一种适用于多种语言的单一 CLI，如 ike gvm、nvm、rbenv & pyenv （等等）。
- [nvs](https://github.com/jasongin/nvs) （节点版本切换器）是一个跨平台 `nvm` 替代方法，可以与[VS Code 集成](https://github.com/jasongin/nvs/blob/master/doc/VSCODE.md)。

## <a name="install-your-favorite-code-editor"></a>安装你最喜欢的代码编辑器

建议将**Visual Studio Code**与用于 node.js 项目的**WSL 扩展**配合使用。 这会将 VS Code 拆分为 "客户端-服务器" 体系结构，并使客户端（用户界面）在 Windows 计算机上运行，而服务器（你的代码、Git、插件等）远程运行。

- 支持基于 Linux 的 Intellisense 和 linting。
- 你的项目将在 Linux 中自动生成。
- 你可以使用在 Linux 上运行的所有扩展（不[起毛，NPM Intellisense，ES6 代码段，等等](https://marketplace.visualstudio.com/items?itemName=waderyan.nodejs-extension-pack)）。

基于终端的文本编辑器（vim、emacs、nano）还有助于从控制台内部快速更改。 （[本文](https://medium.com/linode-cube/emacs-nano-or-vim-choose-your-terminal-based-text-editor-wisely-8f3826c92a68)将讲解一项不错的工作，介绍如何使用每个差异。）

> [!NOTE]
> 某些 GUI 编辑器（Atom、Sublime 文本、Eclipse）在访问 WSL 共享网络位置（\\wsl $ \Ubuntu\home @ no__t-1 时可能会遇到问题，并将尝试使用 Windows 工具构建 Linux 文件，这可能不是你所希望的。 VS Code 中的远程 WSL 扩展将为你处理此兼容性。

若要安装 VS Code 和 WSL 扩展：

1. [下载并安装适用于 Windows 的 VS Code](https://code.visualstudio.com)。 VS Code 也适用于 Linux，但适用于 Linux 的 Windows 子系统不支持 GUI 应用，因此我们需要在 Windows 上安装它。 不用担心，你仍可以使用远程-WSL 扩展与 Linux 命令行和工具集成。

2. 在 VS Code 上安装[WSL 扩展](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl)。 这使你可以将 WSL 用作集成开发环境，并将为你处理兼容性和路径。 [了解详情](https://code.visualstudio.com/docs/remote/remote-overview)。

> [!IMPORTANT]
> 如果已安装 VS Code，则需要确保将[1.35 发布](https://code.visualstudio.com/updates/v1_35)或更高版本，以便安装[远程 WSL 扩展](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl)。 建议不要在不使用 WSL 扩展的 VS Code 中使用 WSL，因为将失去对自动完成、调试、linting 等的支持。有趣的事实：此 WSL 扩展安装在 $HOME/.vscode-server/extensions。

### <a name="helpful-vs-code-extensions"></a>有用的 VS Code 扩展

虽然 VS Code 附带了许多用于 node.js 开发的功能，但有一些有用的扩展可考虑在[Node.js 扩展包](https://marketplace.visualstudio.com/items?itemName=waderyan.nodejs-extension-pack)中安装可用。 全部安装或选择，并选择对你来说最有用的。

安装 node.js 扩展包：

1. 在 VS Code 中打开 "**扩展**" 窗口（Ctrl + Shift + X）。

    现在，"扩展" 窗口分为三个部分（因为已安装了远程 WSL 扩展）。
    - "本地安装"：安装的扩展，用于 Windows 操作系统。
    - "WSL： Ubuntu-18.04-已安装"：安装用于 Ubuntu 操作系统（WSL）的扩展。
    - "推荐"：根据当前项目中的文件类型 VS Code 建议的扩展。

    ![本地和远程 VS Code 扩展](../images/vscode-extensions-local-remote.png)

2. 在 "扩展" 窗口顶部的搜索框中，输入： **Node Extension Pack** （或要查找的任何扩展名的名称）。 将为 VS Code 的本地实例或 WSL 实例安装扩展，具体取决于打开当前项目的位置。 可以通过选择 "VS Code" 窗口左下角的 "远程" 链接（绿色）来进行判断。 它将为你显示打开或关闭远程连接的选项。 在 "WSL： Ubuntu-18.04" 环境中安装 node.js 扩展。

    ![VS Code 远程链接](../images/wsl-remote-extension.png)

你可能需要考虑的几个附加扩展包括：

- [Chrome 调试器](https://code.visualstudio.com/blogs/2016/02/23/introducing-chrome-debugger-for-vs-code)：在服务器端通过 node.js 进行开发后，需要开发并测试客户端。 此扩展将您的 VS Code 编辑器与您的 Chrome 浏览器调试服务进行集成，使其更高效一些。
- [从其他编辑器键映射](https://marketplace.visualstudio.com/search?target=VSCode&category=Keymaps&sortBy=Downloads)：如果从其他文本编辑器（例如 Atom、Sublime、Vim、EMacs、记事本 + + 等）进行转换，则这些扩展可帮助你的环境直接感觉。
- [设置同步](https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync)：使你能够在使用 GitHub 的不同安装之间同步 VS Code 设置。 如果您在不同的计算机上工作，这有助于使您的环境在它们之间保持一致。

## <a name="install-windows-terminal-optional"></a>安装 Windows 终端（可选）

新的 Windows 终端启用多个选项卡（在命令提示符、PowerShell 或多个 Linux 分发之间快速切换）、自定义密钥绑定（创建自己的快捷键以打开或关闭选项卡、复制 + 粘贴等）、表情符号☺和自定义主题（配色方案、字体样式和字号、背景图像/模糊/透明度）。 [了解详情](https://devblogs.microsoft.com/commandline/)。

1. 获取[Microsoft Store 中的 Windows 终端（预览版）](https://www.microsoft.com/store/apps/9n0dx20hk701)：通过应用商店进行安装，将自动处理更新。

2. 安装完成后，打开 Windows 终端并选择 "**设置**" 以使用 @no__t 1 文件自定义终端。 [了解有关编辑 Windows 终端设置的详细信息](https://github.com/microsoft/terminal/blob/master/doc/user-docs/UsingJsonSettings.md)。

    ![Windows 终端设置](../images/windows-terminal-settings.png)

## <a name="set-up-git-optional"></a>设置 Git （可选）

如果你计划与他人进行协作，或在开源站点（例如 GitHub）上托管你的项目，VS Code 支持[使用 Git 进行版本控制](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support)。 VS Code 中的 "源代码管理" 选项卡跟踪所有更改，并在 UI 中内置内置的 Git 命令（添加、提交、推送和拉取）。

1. Git 随用于 Linux 发行版的 Windows 子系统一起安装，但是，你将需要设置 git 配置文件。 为此，请在终端输入： `git config --global user.name "Your Name"`，然后 `git config --global user.email "youremail@domain.com"`。 如果还没有 Git 帐户，可以[在 GitHub 上注册一个](https://github.com/join)帐户。 如果以前从未处理过 Git， [GitHub 指南](https://guides.github.com/)可帮助你入门。 如果需要编辑 git 配置，可以使用内置的文本编辑器，如 nano： `nano ~/.gitconfig`。

2. 建议向节点项目添加[.gitignore 文件](https://help.github.com/en/articles/ignoring-files)。 下面是[适用于 node.js 的 GitHub 默认 .gitignore 模板](https://github.com/github/gitignore/blob/master/Node.gitignore)。 如果选择[使用 GitHub 网站创建新的](https://help.github.com/articles/create-a-repo)存储库，则可以使用以下复选框来初始化存储库，其中包含一个自述文件、一个用于 node.js 项目的 .gitignore 文件和一个用于添加许可证（如果需要）的选项。

## <a name="next-steps"></a>后续步骤

现在已设置 node.js 开发环境。 若要开始使用 node.js 环境，请考虑尝试以下教程之一：

- [开始将 node.js 用于初学者](./beginners.md)
- [在 Windows 上开始处理 node.js web 框架](./web-frameworks.md)
- [开始将 node.js 应用连接到数据库](./databases.md)
- [开始将 Docker 容器与 node.js 配合使用](./containers.md)
