---
title: 在 WSL 2 上设置 NodeJS
description: 本指南可帮助你直接在适用于 Linux 的 Windows 子系统 (WSL) 上设置 Node.js 开发环境。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS, Node.js, windows 10, microsoft, 了解 nodejs, windows 上的 node, wsl 上的 node, windows 中 linux 上的 node, 在 windows 上安装 node, 具有 vs code 的 nodejs, 通过 windows 上的 node 进行开发, 通过 windows 上的 nodejs 进行开发, 在 WSL 上安装 node, 适用于 Linux 的 Windows 子系统上的 NodeJS
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: c987f5bea387c630a1b9ef23c928d7a1bb8fadfc
ms.sourcegitcommit: cf4bf0ab4ea9019c1edc2bb96387ce6cedbe91dd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/10/2020
ms.locfileid: "75835383"
---
# <a name="set-up-your-nodejs-development-environment-with-wsl-2"></a>使用 WSL 2 设置 Node.js 开发环境

下面是可帮助你使用适用于 Linux 的 Windows 子系统 (WSL) 设置 Node.js 开发环境的分步指南。 本指南当前要求你安装并运行 Windows Insider Preview 版本才能安装和使用 [WSL 2](https://devblogs.microsoft.com/commandline/wsl-2-is-now-available-in-windows-insiders/)。 WSL 2 比 WSL 1 显著提高了速度和性能，尤其对于 Node.js。 有关 Node.js Web 开发的许多 npm 模块和教程是面向 Linux 用户编写的，并使用基于 Linux 的打包和安装工具。 大多数 Web 应用也部署在 Linux 上，因此使用 WSL 2 会确保开发环境与生产环境之间的一致性。

> [!NOTE]
> 如果你致力于直接在 Windows 上使用 Node.js，或计划使用 Windows Server 生产环境，请参阅我们的指南以[直接在 Windows 上设置 Node.js 开发环境](./setup-on-windows.md)。

## <a name="install-windows-10-insider-preview-build"></a>安装 Windows 10 Insider Preview 版本

1. **[安装最新版本的 Windows 10](https://www.microsoft.com/software-download/windows10)** ：选择“立即更新”以下载更新助手  。 下载完成后，打开更新助手以查看当前是否正在运行最新版本的 Windows，如果不是，请选择助手窗口中的“立即更新”以更新计算机  。 *（如果运行的是最新版本的 Windows 10，则此步骤是可选的。）*

    ![Windows 更新助手](../images/windows-update-assistant2019.png)

2. **[转到“开始”>“设置”>“Windows 预览体验计划”](ms-settings:windowsinsider)** ：在“Windows 预览体验计划”窗口中，选择“开始使用”，然后“链接帐户”   。

    ![Windows 预览体验计划设置](../images/windows-insider-program-settings.png)

3. **[以 Windows 预览体验成员身份注册](https://insider.windows.com/getting-started/#register)** ：如果尚未注册预览体验计划，则需要使用 [Microsoft 帐户](https://account.microsoft.com/account)进行注册。

    ![Windows 预览体验注册](../images/windows-insider-account.png)

4. 选择接收“快圈”更新或“跳到下一个 Windows 版本”内容   。 确认并选择“稍后重新启动”  。 在重新启动之前，需要更改几个其他设置。

    ![Windows 预览体验快圈](../images/windows-insider-fast.png)

## <a name="enable-windows-subsystem-for-linux-and-virtual-machine-platform"></a>启用适用于 Linux 的 Windows 子系统和虚拟机平台

1. 如果仍在“Windows 设置”中，请搜索“打开或关闭 Windows 功能”   。
2. 出现“Windows 功能”列表后，滚动查找“虚拟机平台”和“适用于 Linux 的 Windows 子系统”，确保选中此框以启用这两者，然后选择“确定”     。
3. 出现提示时，重启计算机。

    ![启用 Windows 功能](../images/windows-feature-settings.png)

## <a name="install-a-linux-distribution"></a>安装 Linux 发行版本

可以在 WSL 上运行多个 Linux 发行版本。 可以在 Microsoft Store 中查找并安装最常用的版本。 建议从 [Ubuntu 18.04 LTS](https://www.microsoft.com/store/productId/9N9TNGVNDL3Q) 开始，因为它是最新常用版本，并且受到良好支持。

1. 打开此 [Ubuntu 18.04 LTS](https://www.microsoft.com/store/productId/9N9TNGVNDL3Q) 链接，打开 Microsoft Store，然后选择“获取”  。 （这属于大型下载，可能需要一段时间才能安装完成。） 

2. 下载完成之后，从 Microsoft Store 选择“启动”  ，或是通过在“开始”  菜单中输入“Ubuntu 18.04 LTS”来启动。

3. 首次运行发行版本时，系统会要求创建帐户名称和密码。 在此之后，默认情况下你会以此用户身份自动登录。 可以选择任意用户名和密码。 它们对 Windows 用户名没有任何影响。

    ![Microsoft Store 中的 Linux 发行版本](../images/store-linux-distros.png)

可以通过输入以下内容来检查当前使用的 Linux 发行版本：`lsb_release -dc`。 若要更新 Ubuntu 发行版本，请使用：`sudo apt update && sudo apt upgrade`。 建议定期更新以确保具有最新包。 Windows 不会自动处理此更新。 有关 Microsoft Store 中提供的其他 Linux 发行版本链接、替代安装方法或故障排除，请参阅[适用于 Linux 的 Windows 子系统安装指南 (Windows 10)](https://docs.microsoft.com/windows/wsl/install-win10)。

## <a name="install-wsl-2"></a>安装 WSL 2

WSL 2 是 WSL 中[体系结构的新版本](https://docs.microsoft.com/windows/wsl/wsl2-about)，它更改了 Linux 发行版本与 Windows 进行交互的方式，从而提高了性能并增加了完整系统调用兼容性。

1. 在 PowerShell 中，输入以下命令：`wsl -l` 用于查看计算机上已安装的 WSL 发行版本的列表。 你现在应该可以在此列表中看到 Ubuntu-18.04。
2. 现在，输入以下命令：`wsl --set-version Ubuntu-18.04 2` 用于设置 Ubuntu 安装以使用 WSL 2。
3. 验证每个已安装的发行版本使用的 WSL 版本：`wsl --list --verbose`（或 `wsl -l -v`）。

    ![适用于 Linux 的 Windows 子系统设置版本](../images/wsl-versions.png)

> [!TIP]
> 你可以按照相同的说明（使用 PowerShell）设置已安装到 WSL 2 的任何 Linux 发行版本，只需将“Ubuntu-18.04”更改为希望设为目标的已安装发行版本的名称即可。 要更改回 WSL 1，请运行与上面相同的命令，但将“2”替换为“1”。  你还可以通过输入以下内容将 WSL 2 设置为任何新安装的发行版本的默认值：`wsl --set-default-version 2`。

## <a name="install-nvm-nodejs-and-npm"></a>安装 nvm、node.js 和 npm

可通过多种方式安装 Node.js。 建议使用版本管理器，因为版本变更速度非常快。 你可能需要根据所使用的不同项目的需要在多个版本之间进行切换。 Node 版本管理器（通常称为 nvm）是安装多个版本的 Node.js 的最常见方法。 我们将演练安装 nvm 的步骤，然后使用它来安装 Node.js 和节点包管理器 (npm)。 下一节中还会介绍供考虑的[替代版本管理器](#alternative-version-managers)。

> [!IMPORTANT]
> 在安装版本管理器之前，始终建议从操作系统中删除 Node.js 或 npm 的任何现有安装，因为不同的安装类型可能会导致出现奇怪和混淆的冲突。 例如，可以使用 Ubuntu 的 `apt-get` 命令安装的 Node 版本当前已过时。 有关删除先前安装的帮助，请参阅[如何从 ubuntu 中删除 node.js](https://askubuntu.com/questions/786015/how-to-remove-nodejs-from-ubuntu-16-04)。

1. 打开 Ubuntu 18.04 命令行。
2. 使用以下命令安装 cURL（用于在命令行中从 Internet 下载内容的工具）：`sudo apt-get install curl`
3. 使用以下命令安装 nvm：`curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.2/install.sh | bash`
4. 若要验证安装，请输入：`command -v nvm`。此命令应返回“nvm”，如果你收到“找不到命令”或根本没有响应，请关闭当前终端，将其重新打开，然后重试。 [在 nvm github 存储库中了解详细信息](https://github.com/nvm-sh/nvm)。
5. 列出当前安装的 Node 版本（此时应为无）：`nvm ls`

    ![显示无 Node 版本的 NVM 列表](../images/nvm-no-node.png)

6. 安装 Node.js 的当前版本（用于测试最新的功能改进，但更容易出现问题）：`nvm install node`
7. 安装 Node.js 的最新稳定 LTS 版本（推荐）：`nvm install --lts`
8. 列出安装的 Node 版本：`nvm ls`。现在应会看到刚安装的两个版本。

    ![显示 LTS 和当前 Node 版本的 NVM 列表](../images/nvm-node-installed.png)

9. 使用以下命令验证 Node.js 是否已安装，以及是否为当前默认版本：`node --version`。 然后使用以下命令验证是否也有 npm：`npm --version`（还可以使用 `which node` 或 `which npm` 来查看用于默认版本的路径）。
10. 若要更改要用于项目的 Node.js 版本，请创建新的项目目录 `mkdir NodeTest`，输入目录 `cd NodeTest`，然后输入 `nvm use node` 切换到当前版本，或输入 `nvm use --lts` 切换到 LTS 版本。 你还可以使用已安装的任何其他版本的特定数量，如 `nvm use v8.2.1`。 （若要列出 Node.js 的所有可用版本，请使用以下命令：`nvm ls-remote`）。

> [!TIP]
> 如果要使用 NVM 安装 Node.js 和 NPM，则不需要使用 SUDO 命令来安装新包。

> [!NOTE]
> 发布时，NVM v0.35.2 是可用的最新版本。 你可以查看[最新版本的 NVM 的 GitHub 项目页](https://github.com/nvm-sh/nvm)，并调整上述命令以包含最新版本。
使用 cURL 安装较新版本的 NVM 将替换旧版本，并使已使用 NVM 进行安装的 Node 版本保持不变。 例如：`curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.36.0/install.sh | bash`

## <a name="alternative-version-managers"></a>替代版本管理器

虽然 nvm 目前是最常用的节点版本管理器，但需要考虑一些替代版本管理器：

- [n](https://www.npmjs.com/package/n#installation) 是长期存在的 `nvm` 替代方法，该方法使用略微不同的命令来完成相同的操作，并通过 `npm` 而不是 bash 脚本来安装。
- [fnm](https://github.com/Schniz/fnm#using-a-script) 是较新的版本管理器，它声称比 `nvm` 快得多。 （它还使用 [Azure 管道](https://docs.microsoft.com/azure/devops/pipelines/get-started/what-is-azure-pipelines?view=azure-devops)。）
- [Volta](https://github.com/volta-cli/volta#installing-volta) 是来自 LinkedIn 团队的新版本管理器，它声称改进了速度和跨平台支持。
- [asdf-vm](https://asdf-vm.com/#/core-manage-asdf-vm) 是适用于多种语言的单个 CLI，例如将 ike gvm、nvm、rbenv 和 pyenv（等）整合在一起。
- [nvs](https://github.com/jasongin/nvs)（Node 版本切换器）是跨平台的 `nvm` 替代方法，可[与 VS Code 集成](https://github.com/jasongin/nvs/blob/master/doc/VSCODE.md)。

## <a name="install-your-favorite-code-editor"></a>安装你最喜欢的代码编辑器

建议将 Visual Studio Code 与适用于 Node.js 项目的 Remote-WSL 扩展一起使用   。 这会将 VS Code 拆分为“客户端-服务器”体系结构，使客户端（用户界面）在 Windows 计算机上运行，而使服务器（你的代码、Git、插件等）远程运行。

- 支持基于 Linux 的 Intellisense 和 linting。
- 你的项目将在 Linux 中自动生成。
- 你可以使用在 Linux 上运行的所有扩展（[ES Lint、NPM Intellisense、ES6 snippets 等](https://marketplace.visualstudio.com/items?itemName=waderyan.nodejs-extension-pack)）。

基于终端的文本编辑器（vim、emacs、nano）还有助于从控制台内部快速更改。 （[本文](https://medium.com/linode-cube/emacs-nano-or-vim-choose-your-terminal-based-text-editor-wisely-8f3826c92a68)很好地介绍了差异以及如何使用每个编辑器。）

> [!NOTE]
> 某些 GUI 编辑器（Atom、Sublime Text、Eclipse）在访问 WSL 共享网络位置 (\\wsl$\Ubuntu\home\)) 时可能会遇到问题，并且将尝试使用 Windows 工具生成 Linux 文件，这可能不是你所希望的。 VS Code 中的 Remote-WSL 扩展将为你处理此兼容性。

安装 VS Code 和 Remote-WSL 扩展：

1. [下载和安装适用于 Windows 的 VS Code](https://code.visualstudio.com)。 VS Code 也适用于 Linux，但适用于 Linux 的 Windows 子系统不支持 GUI 应用，因此需要在 Windows 上安装它。 不必担心，仍可以使用 Remote - WSL 扩展与 Linux 命令行和工具集成。

2. 在 VS Code 上安装 [Remote - WSL 扩展](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl)。 这使你可以将 WSL 用作集成开发环境，并且会为你处理兼容性和路径。 [了解详细信息](https://code.visualstudio.com/docs/remote/remote-overview)。

> [!IMPORTANT]
> 如果已安装 VS Code，则需要确保具有 [1.35 5 月版本](https://code.visualstudio.com/updates/v1_35)或更高版本，以便安装 [Remote - WSL 扩展](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl)。 建议不要在没有 Remote - WSL 扩展的情况下在 VS Code 中使用 WSL，因为会失去对自动完成、调试、linting 等的支持。趣味事实：此 WSL 扩展安装在 $HOME/.vscode-server/extensions 中。

### <a name="helpful-vs-code-extensions"></a>有用的 VS Code 扩展

虽然 VS Code 附带了许多用于 Node.js 开发的功能，但有一些有用的扩展可考虑在 [Node.js 扩展包](https://marketplace.visualstudio.com/items?itemName=waderyan.nodejs-extension-pack)中安装。 全部安装或选择对你最有用的包。

安装 Node.js 扩展包：

1. 在 VS Code 中打开“扩展”  窗口 (Ctrl+Shift+X)。

    现在，“扩展”窗口分为三个部分（因为你已安装 Remote-WSL 扩展）。
    - “本地 - 已安装”：要与 Windows 操作系统一起使用而安装的扩展。
    - “WSL:Ubuntu-18.04-已安装”：要与 Ubuntu 操作系统 (WSL) 一起使用而安装的扩展。
    - “推荐”：根据当前项目中的文件类型由 VS Code 推荐的扩展。

    ![VS Code 扩展本地和远程](../images/vscode-extensions-local-remote.png)

2. 在“扩展”窗口顶部的搜索框中，输入：节点扩展包  （或要查找的任何扩展的名称）。 将为 VS Code 的本地实例或 WSL 实例安装扩展，具体取决于打开当前项目的位置。 可以通过选择“VS Code”窗口左下角的远程链接进行判断（以绿色显示）。 将向你提供打开或关闭远程连接的选项。 在“WSL:Ubuntu-18.04”环境中安装 Node.js 扩展。

    ![VS Code 远程链接](../images/wsl-remote-extension.png)

可能需要考虑的几个附加扩展包括：

- [Chrome 调试器](https://code.visualstudio.com/blogs/2016/02/23/introducing-chrome-debugger-for-vs-code)：在服务器端通过 Node.js 完成开发后，需要开发并测试客户端。 此扩展将 VS Code 编辑器与 Chrome 浏览器调试服务进行集成，以提高工作效率。
- [来自其他编辑器的键映射](https://marketplace.visualstudio.com/search?target=VSCode&category=Keymaps&sortBy=Downloads)：如果是从另一个文本编辑器（如 Atom、Sublime、Vim、eMacs、Notepad++ 等）进行转换，则这些扩展可帮助你的环境对此进行适应。
- [设置同步](https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync)：可以使用 GitHub 在不同安装之间同步 VS Code 设置。 如果在不同的计算机上工作，这有助于在它们之间保持一致的环境。

## <a name="install-windows-terminal-optional"></a>安装 Windows 终端（可选）

新的 Windows 终端启用多个选项卡（在命令提示符、PowerShell 或多个 Linux 发行版本之间快速切换）、自定义键绑定（创建自己的快捷键以打开或关闭选项卡、复制并粘贴等）、表情符号 ☺ 和自定义主题（配色方案、字体样式和大小、背景图像/模糊/透明度）。 [了解详细信息](https://devblogs.microsoft.com/commandline/)。

1. 获取 [Microsoft Store 中的 Windows 终端（预览版）](https://www.microsoft.com/store/apps/9n0dx20hk701)：通过 Microsoft Store 进行安装时，将自动处理更新。

2. 安装完成后，打开 Windows 终端并选择“设置”以使用 `profile.json` 文件自定义终端  。 [了解有关编辑 Windows 终端设置的详细信息](https://github.com/microsoft/terminal/blob/master/doc/user-docs/UsingJsonSettings.md)。

    ![Windows 终端设置](../images/windows-terminal-settings.png)

## <a name="set-up-git-optional"></a>设置 Git（可选）

如果计划与其他人协作，或是在开放源代码站点（如 GitHub）上托管项目，则 VS Code 支持[使用 Git 进行版本控制](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support)。 VS Code 中的“源代码管理”选项卡可跟踪所有更改，并直接在 UI 中内置了常见 Git 命令（add、commit、push、pull）。

1. Git 随适用于 Linux 的 Windows 子系统发行版本一起安装，但是，你将需要设置 git 配置文件。 为此，请在终端中输入：`git config --global user.name "Your Name"`，然后输入 `git config --global user.email "youremail@domain.com"`。 如果你还没有 Git 帐户，则可以[在 GitHub 上注册一个帐户](https://github.com/join)。 如果以前从未使用过 Git，则 [GitHub 指南](https://guides.github.com/)可以帮助入门。 如果需要编辑 git 配置，则可以使用内置文本编辑器（如 nano：`nano ~/.gitconfig`）来执行此操作。

2. 建议向 Node 项目添加 [.gitignore 文件](https://help.github.com/en/articles/ignoring-files)。 此处是 [GitHub 用于 Node.js 的默认 gitignore 模板](https://github.com/github/gitignore/blob/master/Node.gitignore)。 如果你选择[使用 GitHub 网站创建新的存储库](https://help.github.com/articles/create-a-repo)，则会出现可用于使用自述文件初始化存储库的复选框，设置用于 Node.js 项目的 .gitignore 文件，以及用于添加许可证（如果需要）的选项。

## <a name="next-steps"></a>后续步骤

你现在可以设置 Node.js 开发环境。 若要开始使用 Node.js 环境，请考虑尝试以下教程之一：

- [适用于初学者的 Node.js 入门](./beginners.md)
- [开始在 Windows 上使用 Node.js Web 框架](./web-frameworks.md)
- [开始将 Node.js 应用连接到数据库](./databases.md)
- [开始将 Docker 容器与 Node.js 配合使用](./containers.md)
