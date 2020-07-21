---
title: 在本机 Windows 上设置 NodeJS
description: 本指南可帮助你直接在 Windows 上设置 Node.js 开发环境。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: Node.js, windows 10, 本机 windows, 直接在 windows 上
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: fe1943da8c1de4f4fced5dec67079522d83f9a19
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "82173463"
---
# <a name="set-up-your-nodejs-development-environment-directly-on-windows"></a>直接在 Windows 上设置 Node.js 开发环境

下面是有关在本机 Windows 开发环境中开始使用 Node.js 的分步指南。

## <a name="install-nvm-windows-nodejs-and-npm"></a>安装 vm-windows、node.js 和 npm

可通过多种方式安装 Node.js。 建议使用版本管理器，因为版本变更速度非常快。 你可能需要根据所使用的不同项目的需要在多个版本之间进行切换。 Node 版本管理器（通常称为 nvm）是安装多个版本的 Node.js 的最常见方法，但仅适用于 Mac/Linux，在 Windows 上不受支持。 相反，我们将逐步介绍安装 nvm-windows 的步骤，然后使用它来安装 Node.js 和 Node 包管理器 (npm)。 下一节中还会介绍供考虑的[替代版本管理器](#alternative-version-managers)。

> [!IMPORTANT]
> 在安装版本管理器之前，始终建议从操作系统中删除 Node.js 或 npm 的任何现有安装，因为不同的安装类型可能会导致出现奇怪和混淆的冲突。 这包括删除可能保留的任何现有的 nodejs 安装目录（例如，“C:\Program Files\nodejs”）。 NVM 生成的符号链接不会覆盖现有的（甚至是空的）安装目录。 有关删除先前安装的帮助，请参阅[如何从 Windows 中完全删除 node.js](https://stackoverflow.com/questions/20711240/how-to-completely-remove-node-js-from-windows)。

1. 在 Internet 浏览器中打开 [windows-nvm 存储库](https://github.com/coreybutler/nvm-windows#node-version-manager-nvm-for-windows)，然后选择“立即下载”  链接。
2. 下载最新版本的 nvm-setup.zip  文件。
3. 下载完成后，打开 zip 文件，然后打开 nvm-setup.exe  文件。
4. Setup-NVM-for-Windows 安装向导将引导你完成安装步骤，包括选择将在其中安装 nvm-windows 和 Node.js 的目录。

    ![NVM for Windows 安装向导](../images/install-nvm-for-windows-wizard.png)

5. 安装完成后， 打开 PowerShell，尝试使用 windows-nvm 来列出当前安装的 Node 版本（此时应为无）：`nvm ls`

    ![显示无 Node 版本的 NVM 列表](../images/windows-nvm-powershell-no-node.png)

6. 安装 Node.js 的当前版本（用于测试最新的功能改进，但比 LTS 版本更容易出现问题）：`nvm install latest`

7. 若要安装 Node.js 的最新稳定 LTS 版本（建议），首先通过 `nvm list available` 查找当前的 LTS 版本号，然后使用 `nvm install <version>` 安装 LTS 版本号（将 `<version>` 替换为版本号 `nvm install 12.14.0`）。

    ![可用版本的 NVM 列表](../images/windows-nvm-list.png)

8. 列出安装的 Node 版本：`nvm ls`。现在应会看到刚安装的两个版本。

    ![显示已安装的 Node 版本的 NVM 列表](../images/windows-nvm-node-installs.png)

9. 在安装所需的 Node.js 版本后，通过输入 `nvm use <version>`（请将 `<version>` 替换为版本号，即 `nvm use 12.9.0`）来选择要使用的版本。

10. 若要更改要用于项目的 Node.js 版本，请创建新的项目目录 `mkdir NodeTest`，输入目录 `cd NodeTest`，然后输入 `nvm use <version>`，将 `<version>` 替换为要使用的版本号（即 v10.16.3`）。

11. 验证哪个 npm 版本安装了 `npm --version`，此版本号将自动更改为与当前版本的 Node.js 关联的 npm 版本。

## <a name="alternative-version-managers"></a>替代版本管理器

虽然 windows-nvm 目前是最常用的节点版本管理器，但也有一些其他选择：

- [nvs](https://github.com/jasongin/nvs)（Node 版本切换器）是跨平台的 `nvm` 替代方法，可[与 VS Code 集成](https://github.com/jasongin/nvs/blob/master/doc/VSCODE.md)。

- [Volta](https://github.com/volta-cli/volta#installing-volta) 是来自 LinkedIn 团队的新版本管理器，它声称改进了速度和跨平台支持。

若要将 Volta 安装为版本管理器（而不是 windows-nvm），请参阅其[入门指南](https://docs.volta.sh/guide/getting-started)的“Windows 安装”  部分，然后按照安装说明下载并运行其 Windows 安装程序。

> [!IMPORTANT]
> 安装 Volta 之前，必须确保在 Windows 计算机上启用[开发人员模式](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development#accessing-settings-for-developers)。

若要了解有关使用 Volta 在 Windows 上安装多个版本的 Node.js 的详细信息，请参阅 [Volta 文档](https://docs.volta.sh/guide/understanding#managing-your-toolchain)。

## <a name="install-your-favorite-code-editor"></a>安装你最喜欢的代码编辑器

建议[安装 VS Code](https://code.visualstudio.com)和 [Node.js 扩展包](https://marketplace.visualstudio.com/items?itemName=waderyan.nodejs-extension-pack)，以便在 Windows 上通过 Node.js 进行开发。 全部安装或选择对你最有用的包。

安装 Node.js 扩展包：

1. 在 VS Code 中打开“扩展”  窗口 (Ctrl+Shift+X)。
2. 在“扩展”窗口顶部的搜索框中，输入：“Node 扩展包”（或要查找的任何扩展的名称）。
3. 选择“安装”  。 安装完成后，扩展将出现在“扩展”  窗口的“已启用”文件夹中。 可以通过选择新扩展说明旁的齿轮图标来禁用、卸载或配置设置。

可能需要考虑的几个附加扩展包括：

- [Chrome 调试器](https://code.visualstudio.com/blogs/2016/02/23/introducing-chrome-debugger-for-vs-code)：在服务器端通过 Node.js 完成开发后，需要开发并测试客户端。 此扩展将 VS Code 编辑器与 Chrome 浏览器调试服务进行集成，以提高工作效率。
- [来自其他编辑器的键映射](https://marketplace.visualstudio.com/search?target=VSCode&category=Keymaps&sortBy=Downloads)：如果是从另一个文本编辑器（如 Atom、Sublime、Vim、eMacs、Notepad++ 等）进行转换，则这些扩展可帮助你的环境对此进行适应。
- [设置同步](https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync)：可以使用 GitHub 在不同安装之间同步 VS Code 设置。 如果在不同的计算机上工作，这有助于在它们之间保持一致的环境。

## <a name="install-git-optional"></a>安装 Git（可选）

如果计划与其他人协作，或是在开放源代码站点（如 GitHub）上托管项目，则 VS Code 支持[使用 Git 进行版本控制](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support)。 VS Code 中的“源代码管理”选项卡可跟踪所有更改，并直接在 UI 中内置了常见 Git 命令（add、commit、push、pull）。 需要先安装 Git，以便为“源代码管理”面板提供支持。

1. 从 [git-scm 网站](https://git-scm.com/download/win)下载并安装适用于 Windows 的 Git。

2. 其中包含了一个安装向导，该向导会询问一系列有关 Git 安装设置的问题。 建议使用所有默认设置，除非有特定原因需要更改某些设置。

3. 如果以前从未使用过 Git，则 [GitHub 指南](https://guides.github.com/)可以帮助入门。

4. 建议向 Node 项目添加 [.gitignore 文件](https://help.github.com/en/articles/ignoring-files)。 此处是 [GitHub 用于 Node.js 的默认 gitignore 模板](https://github.com/github/gitignore/blob/master/Node.gitignore)。

## <a name="use-windows-subsystem-for-linux-for-production"></a>使用适用于 Linux 的 Windows 子系统进行生产

直接在 Windows 上使用 Node.js 非常适合学习和试验可执行的操作。 准备好生成可投入生产的 Web 应用后（通常部署到基于 Linux 的服务器），建议使用适用于 Linux 的 Windows 子系统版本 2 (WSL 2) 来开发 Node.js Web 应用。 许多 Node.js 包和框架是在 *nix 环境下创建的，并且大多数 Node.js 应用都部署在 Linux 上，因此在 WSL 上开发可确保开发环境和生产环境之间的一致性。 若要设置 WSL 开发环境，请参阅[使用 WSL 2 设置 Node.js 开发环境](./setup-on-wsl2.md)。

> [!NOTE]
> 如果需要在 Windows 服务器上托管 Node.js 应用程序，最常见的场景似乎是[使用反向代理](https://medium.com/intrinsic/why-should-i-use-a-reverse-proxy-if-node-js-is-production-ready-5a079408b2ca)。 可通过两种方式实现此目的：1) [使用 iisnode](https://harveywilliams.net/blog/installing-iisnode) 或[直接实现](https://dev.to/petereysermans/hosting-a-node-js-application-on-windows-with-iis-as-reverse-proxy-397b)。 我们不会保留这些资源，建议[使用 Linux 服务器来托管 Node.js 应用](https://docs.microsoft.com/azure/app-service/app-service-web-get-started-nodejs)。
