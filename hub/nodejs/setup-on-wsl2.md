---
title: 在 WSL 2 上设置 NodeJS
description: 本指南可帮助你直接在适用于 Linux 的 Windows 子系统 (WSL) 上设置 Node.js 开发环境。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS, Node.js, windows 10, microsoft, 了解 nodejs, windows 上的 node, wsl 上的 node, windows 中 linux 上的 node, 在 windows 上安装 node, 具有 vs code 的 nodejs, 通过 windows 上的 node 进行开发, 通过 windows 上的 nodejs 进行开发, 在 WSL 上安装 node, 适用于 Linux 的 Windows 子系统上的 NodeJS
ms.localizationpriority: medium
ms.date: 06/09/2020
ms.openlocfilehash: e8fb06cb1e68d5dfa7f23e6966f917c96eb79859
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493272"
---
# <a name="set-up-your-nodejs-development-environment-with-wsl-2"></a>使用 WSL 2 设置 Node.js 开发环境

下面是可帮助你使用适用于 Linux 的 Windows 子系统 (WSL) 设置 Node.js 开发环境的分步指南。

建议安装并运行更新后的 WSL 2，因为性能速度和系统调用兼容性方面的大幅提升（包括可运行 [Docker Desktop](https://docs.docker.com/docker-for-windows/wsl-tech-preview/#download) 的能力）将使你受益。 有关 Node.js Web 开发的许多 npm 模块和教程是面向 Linux 用户编写的，并使用基于 Linux 的打包和安装工具。 大多数 Web 应用也部署在 Linux 上，因此使用 WSL 2 会确保开发环境与生产环境之间的一致性。

> [!NOTE]
> 如果你致力于直接在 Windows 上使用 Node.js，或计划使用 Windows Server 生产环境，请参阅我们的指南以[直接在 Windows 上设置 Node.js 开发环境](./setup-on-windows.md)。

## <a name="install-wsl-2"></a>安装 WSL 2

要启用和安装 WSL 2，请按照 [WSL 安装文档](https://docs.microsoft.com/windows/wsl/install-win10)中的步骤操作。 这些步骤将包含选择 Linux 发行版（例如 Ubuntu）。

安装 WSL 2 和 Linux 发行版后，打开 Linux 发行版（可在 Windows 的开始菜单中找到），并使用命令 `lsb_release -dc` 查看版本和代码名称。

建议定期更新 Linux 发行版，包括在安装之后立即更新，以确保具有最新的包。 Windows 不会自动处理此更新。 要更新发行版，请使用命令：`sudo apt update && sudo apt upgrade`。  

## <a name="install-windows-terminal-optional"></a>安装 Windows 终端（可选）

新的 Windows 终端可启用多个选项卡（在多个 Linux 命令行、Windows 命令提示符、PowerShell 和 Azure CLI 等之间快速切换）、创建键绑定（用于打开或关闭选项卡、复制粘贴等的快捷方式键）、使用搜索功能，以及使用自定义主题（配色方案、字体样式和大小、背景图像/模糊/透明度）。 [了解详细信息](https://docs.microsoft.com/windows/terminal)。

1. [在 Microsoft Store 中获取 Windows 终端](https://www.microsoft.com/store/apps/9n0dx20hk701)：通过 Microsoft Store 进行安装时，将自动处理更新。

2. 安装完成后，打开 Windows 终端并选择“设置”以使用 `settings.json` 文件自定义终端。

    ![Windows 终端设置](../images/windows-terminal-settings.png)

## <a name="install-nvm-nodejs-and-npm"></a>安装 nvm、node.js 和 npm

可通过多种方式安装 Node.js。 建议使用版本管理器，因为版本变更速度非常快。 你可能需要根据所使用的不同项目的需要在多个版本之间进行切换。 Node 版本管理器（通常称为 nvm）是安装多个版本的 Node.js 的最常见方法。 我们将演练安装 nvm 的步骤，然后使用它来安装 Node.js 和节点包管理器 (npm)。 下一节中还会介绍供考虑的[替代版本管理器](#alternative-version-managers)。

> [!IMPORTANT]
> 在安装版本管理器之前，始终建议从操作系统中删除 Node.js 或 npm 的任何现有安装，因为不同的安装类型可能会导致出现奇怪和混淆的冲突。 例如，可以使用 Ubuntu 的 `apt-get` 命令安装的 Node 版本当前已过时。 有关删除先前安装的帮助，请参阅[如何从 ubuntu 中删除 node.js](https://askubuntu.com/questions/786015/how-to-remove-nodejs-from-ubuntu-16-04)。

1. 打开 Ubuntu 18.04 命令行。
2. 使用以下命令安装 cURL（用于在命令行中从 Internet 下载内容的工具）：`sudo apt-get install curl`
3. 使用以下命令安装 nvm：`curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash`

> [!NOTE]
> 发布时，NVM v0.35.3 是可用的最新版本。 你可以查看[最新版本的 NVM 的 GitHub 项目页](https://github.com/nvm-sh/nvm)，并调整上述命令以包含最新版本。
使用 cURL 安装较新版本的 NVM 将替换旧版本，并使已使用 NVM 进行安装的 Node 版本保持不变。 例如：`curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.36.0/install.sh | bash`

4. 若要验证安装，请输入：`command -v nvm`。此命令应返回“nvm”，如果你收到“找不到命令”或根本没有响应，请关闭当前终端，将其重新打开，然后重试。 [在 nvm github 存储库中了解详细信息](https://github.com/nvm-sh/nvm)。
5. 列出当前安装的 Node 版本（此时应为无）：`nvm ls`

    ![显示无 Node 版本的 NVM 列表](../images/nvm-no-node.png)

6. 安装 Node.js 的当前版本（用于测试最新的功能改进，但更容易出现问题）：`nvm install node`
7. 安装 Node.js 的最新稳定 LTS 版本（推荐）：`nvm install --lts`
8. 列出安装的 Node 版本：`nvm ls`。现在应会看到刚安装的两个版本。

    ![显示 LTS 和当前 Node 版本的 NVM 列表](../images/nvm-node-installed.png)

9. 使用以下命令验证 Node.js 是否已安装，以及是否为当前默认版本：`node --version`。 然后使用以下命令验证是否也有 npm：`npm --version`（还可以使用 `which node` 或 `which npm` 来查看用于默认版本的路径）。
10. 若要更改要用于项目的 Node.js 版本，请创建新的项目目录 `mkdir NodeTest`，输入目录 `cd NodeTest`，然后输入 `nvm use node` 切换到当前版本，或输入 `nvm use --lts` 切换到 LTS 版本。 你还可以使用已安装的任何其他版本的特定数量，如 `nvm use v8.2.1`。 （若要列出 Node.js 的所有可用版本，请使用以下命令：`nvm ls-remote`）。

如果要使用 NVM 安装 Node.js 和 NPM，则不需要使用 SUDO 命令来安装新包。

## <a name="alternative-version-managers"></a>替代版本管理器

虽然 nvm 目前是最常用的节点版本管理器，但需要考虑一些替代版本管理器：

- [n](https://www.npmjs.com/package/n#installation) 是长期存在的 `nvm` 替代方法，该方法使用略微不同的命令来完成相同的操作，并通过 `npm` 而不是 bash 脚本来安装。
- [fnm](https://github.com/Schniz/fnm#using-a-script) 是较新的版本管理器，它声称比 `nvm` 快得多。 （它还使用 [Azure 管道](https://docs.microsoft.com/azure/devops/pipelines/get-started/what-is-azure-pipelines?view=azure-devops)。）
- [Volta](https://github.com/volta-cli/volta#installing-volta) 是来自 LinkedIn 团队的新版本管理器，它声称改进了速度和跨平台支持。
- [asdf-vm](https://asdf-vm.com/#/core-manage-asdf-vm) 是适用于多种语言的单个 CLI，例如将 ike gvm、nvm、rbenv 和 pyenv（等）整合在一起。
- [nvs](https://github.com/jasongin/nvs)（Node 版本切换器）是跨平台的 `nvm` 替代方法，可[与 VS Code 集成](https://github.com/jasongin/nvs/blob/master/doc/VSCODE.md)。

## <a name="install-your-favorite-code-editor"></a>安装你最喜欢的代码编辑器

建议将 Visual Studio Code 与适用于 Node.js 项目的 Remote-WSL 扩展一起使用 。 这会将 VS Code 拆分为“客户端-服务器”体系结构，使客户端（用户界面）在 Windows 计算机上运行，而使服务器（你的代码、Git、插件等）远程运行。

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

1. 在 VS Code 中打开“扩展”窗口 (Ctrl+Shift+X)。

    现在，“扩展”窗口分为三个部分（因为你已安装 Remote-WSL 扩展）。
    - “本地 - 已安装”：要与 Windows 操作系统一起使用而安装的扩展。
    - “WSL:Ubuntu-18.04-已安装”：要与 Ubuntu 操作系统 (WSL) 一起使用而安装的扩展。
    - “推荐”：根据当前项目中的文件类型由 VS Code 推荐的扩展。

    ![VS Code 扩展本地和远程](../images/vscode-extensions-local-remote.png)

2. 在“扩展”窗口顶部的搜索框中，输入：节点扩展包（或要查找的任何扩展的名称）。 将为 VS Code 的本地实例或 WSL 实例安装扩展，具体取决于打开当前项目的位置。 可以通过选择“VS Code”窗口左下角的远程链接进行判断（以绿色显示）。 将向你提供打开或关闭远程连接的选项。 在“WSL:Ubuntu-18.04”环境中安装 Node.js 扩展。

    ![VS Code 远程链接](../images/wsl-remote-extension.png)

可能需要考虑的几个附加扩展包括：

- [Chrome 调试器](https://code.visualstudio.com/blogs/2016/02/23/introducing-chrome-debugger-for-vs-code)：在服务器端通过 Node.js 完成开发后，需要开发并测试客户端。 此扩展将 VS Code 编辑器与 Chrome 浏览器调试服务进行集成，以提高工作效率。
- [来自其他编辑器的键映射](https://marketplace.visualstudio.com/search?target=VSCode&category=Keymaps&sortBy=Downloads)：如果是从另一个文本编辑器（如 Atom、Sublime、Vim、eMacs、Notepad++ 等）进行转换，则这些扩展可帮助你的环境对此进行适应。
- [设置同步](https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync)：可以使用 GitHub 在不同安装之间同步 VS Code 设置。 如果在不同的计算机上工作，这有助于在它们之间保持一致的环境。

## <a name="set-up-git-optional"></a>设置 Git（可选）

要为 WSL 上的 NodeJS 项目设置 Git，请参阅 WSL 文档中的文章[开始在适用于 Linux 的 Windows 子系统上使用 Git](https://docs.microsoft.com/windows/wsl/tutorials/wsl-git)。

## <a name="next-steps"></a>后续步骤

你现在可以设置 Node.js 开发环境。 若要开始使用 Node.js 环境，请考虑尝试以下教程之一：

- [适用于初学者的 Node.js 入门](./beginners.md)
- [开始在 Windows 上使用 Node.js Web 框架](./web-frameworks.md)
- [开始将 Node.js 应用连接到数据库](https://docs.microsoft.com/windows/wsl/tutorials/wsl-database)
- [开始将 Docker 容器与 Node.js 结合使用](./containers.md)
