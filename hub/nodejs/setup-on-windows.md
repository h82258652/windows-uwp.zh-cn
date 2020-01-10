---
title: 在本机 Windows 上设置 NodeJS
description: 本指南可帮助你直接在 Windows 上设置 node.js 开发环境。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: Node.js、windows 10、本机 windows，直接在 windows 上
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: 456aac17f61ab0add3d35a48c74e151fa15e9e83
ms.sourcegitcommit: 8efeb6672f759b1ea7e3e9e2f90e764480791142
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2020
ms.locfileid: "75728468"
---
# <a name="set-up-your-nodejs-development-environment-directly-on-windows"></a>直接在 Windows 上设置 node.js 开发环境

下面是有关在本机 Windows 开发环境中开始使用 node.js 的分步指南。

## <a name="install-nvm-windows-nodejs-and-npm"></a>安装 nvm、node.js 和 npm

可以通过多种方式来安装 node.js。 建议使用版本管理器，因为版本的更改速度非常快。 您可能需要根据所使用的不同项目的需要在多个版本之间进行切换。 节点版本管理器（更常见地称为 nvm）是安装多个版本的 node.js 的最常见方法，但仅适用于 Mac/Linux，在 Windows 上不受支持。 相反，我们将逐步介绍安装 nvm 的步骤，然后使用它来安装 node.js 和 Node 包管理器（npm）。 在下一节中还将介绍[其他的版本管理器](#alternative-version-managers)。

> [!IMPORTANT]
> 在安装版本管理器之前，始终建议从操作系统中删除 node.js 或 npm 的任何现有安装，因为不同的安装类型可能会导致出现奇怪和混淆的冲突。 这包括删除任何现有的 nodejs 安装目录（例如，"C:\Program Files\nodejs"）。 NVM 生成的符号链接不会覆盖现有的（甚至是空的）安装目录。 有关删除以前安装的帮助，请参阅[如何从 Windows 完全删除 node.js](https://stackoverflow.com/questions/20711240/how-to-completely-remove-node-js-from-windows)。）

1. 在 internet 浏览器中打开[nvm 存储库](https://github.com/coreybutler/nvm-windows#node-version-manager-nvm-for-windows)，然后选择 "**立即下载**" 链接。
2. 下载最新版本的**nvm-setup**文件。
3. 下载完成后，打开 zip 文件，然后打开**nvm-setup**文件。
4. NVM-Windows 安装向导将引导你完成安装步骤，包括选择将在其中安装 NVM 和 node.js 的目录。

    ![NVM for Windows 安装向导](../images/install-nvm-for-windows-wizard.png)

5. 安装完成后。 打开 PowerShell，尝试使用 nvm 来列出当前安装的节点版本（此时应为 "无"）： `nvm ls`

    ![显示无节点版本的 NVM 列表](../images/windows-nvm-powershell-no-node.png)

6. 安装 node.js 的当前版本（用于测试最新的功能改进，但比 LTS 版本更可能出现问题）： `nvm install latest`
7. 首先通过查找当前 LTS 版本号的内容，安装 node.js 的最新稳定 LTS 版本： `nvm list available`，然后使用以下内容安装 LTS 版本号： `nvm install <version>` （将 `<version>` 替换为 `nvm install 12.14.0`数字）。

    ![可用版本的 NVM 列表](../images/windows-nvm-list.png)

8. 列出安装的节点的版本： `nvm ls` .。。现在应会看到刚刚安装的两个版本。

    ![显示已安装节点版本的 NVM 列表](../images/windows-nvm-node-installs.png)

9. 若要查看当前默认的 node.js 版本，请输入： `node --version`
10. 若要更改要用于项目的 node.js 版本，请 `mkdir NodeTest`创建新的项目目录，然后输入目录 `cd NodeTest`，然后输入 `nvm use <version>` 将 `<version>` 替换为要使用的版本号（ie v 10.16.3 '）。
11. 验证安装了哪个版本的 npm： `npm --version`，此版本号将自动更改为与当前版本的 node.js 关联的 npm 版本。

## <a name="alternative-version-managers"></a>备用版本管理器

尽管 windows nvm 目前是最常用的节点版本管理器，但有一些替代方法：

- [nvs](https://github.com/jasongin/nvs) （节点版本切换器）是一个跨平台 `nvm` 替代方法，可以与[VS Code 集成](https://github.com/jasongin/nvs/blob/master/doc/VSCODE.md)。

- [Volta](https://github.com/volta-cli/volta#installing-volta)是 LinkedIn 团队的新版本管理器，用于声明提高速度和跨平台支持。

若要将 Volta 安装为版本管理器（而不是 windows-nvm），请参阅其[入门指南](https://docs.volta.sh/guide/getting-started)的 " **windows 安装**" 部分，然后按照安装说明下载并运行其 windows installer。

> [!IMPORTANT]
> 安装 Volta 之前，必须确保在 Windows 计算机上启用[开发人员模式](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development#accessing-settings-for-developers)。

若要了解有关使用 Volta 在 Windows 上安装多个版本的 node.js 的详细信息，请参阅[Volta 文档](https://docs.volta.sh/guide/understanding#managing-your-toolchain)。

## <a name="install-your-favorite-code-editor"></a>安装你最喜欢的代码编辑器

建议[安装 VS Code](https://code.visualstudio.com)，以及 Node.js[扩展包](https://marketplace.visualstudio.com/items?itemName=waderyan.nodejs-extension-pack)，以便在 Windows 上通过 node.js 进行开发。 全部安装或选择，并选择对你来说最有用的。

安装 node.js 扩展包：

1. 在 VS Code 中打开 "**扩展**" 窗口（Ctrl + Shift + X）。
2. 在 "扩展" 窗口顶部的搜索框中，输入 "Node Extension Pack" （或要查找的任何扩展名的名称）。
3. 选择**安装**。 安装完成后，扩展将出现在 "**扩展**" 窗口的 "已启用" 文件夹中。 您可以通过选择新扩展的说明旁边的齿轮图标来禁用、卸载或配置设置。

你可能需要考虑的几个附加扩展包括：

- [Chrome 调试器](https://code.visualstudio.com/blogs/2016/02/23/introducing-chrome-debugger-for-vs-code)：在服务器端通过 node.js 进行开发后，需要开发并测试客户端。 此扩展将您的 VS Code 编辑器与您的 Chrome 浏览器调试服务进行集成，使其更高效一些。
- [从其他编辑器键映射](https://marketplace.visualstudio.com/search?target=VSCode&category=Keymaps&sortBy=Downloads)：如果从其他文本编辑器（例如 Atom、Sublime、Vim、EMacs、记事本 + + 等）进行转换，则这些扩展可帮助你的环境直接感觉。
- [设置同步](https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync)：使你能够在使用 GitHub 的不同安装之间同步 VS Code 设置。 如果在不同的计算机上工作，这有助于在它们之间保持一致的环境。

## <a name="install-git-optional"></a>安装 Git（可选）

如果你计划与他人进行协作，或在开源站点（例如 GitHub）上托管你的项目，VS Code 支持[使用 Git 进行版本控制](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support)。 VS Code 中的“源代码管理”选项卡可跟踪所有更改，并直接在 UI 中内置了常见 Git 命令（add、commit、push、pull）。 需要先安装 Git，以便为“源代码管理”面板提供支持。

1. 从 [git-scm 网站](https://git-scm.com/download/win)下载并安装适用于 Windows 的 Git。

2. 其中包含了一个安装向导，该向导会询问一系列有关 Git 安装设置的问题。 建议使用所有默认设置，除非有特定原因需要更改某些设置。

3. 如果以前从未使用过 Git，则 [GitHub 指南](https://guides.github.com/)可以帮助入门。

4. 建议向节点项目添加[.gitignore 文件](https://help.github.com/en/articles/ignoring-files)。 下面是[适用于 node.js 的 GitHub 默认 .gitignore 模板](https://github.com/github/gitignore/blob/master/Node.gitignore)。

## <a name="use-windows-subsystem-for-linux-for-production"></a>使用适用于 Linux 的 Windows 子系统进行生产

直接在 Windows 上使用 node.js 非常适合于学习和试验你可以执行的操作。 准备好生成可部署到基于 Linux 的服务器的生产就绪 web 应用后，建议使用适用于 Linux 版本2（WSL 2）的 Windows 子系统来开发 node.js web 应用。 许多 node.js 包和框架是使用 * nix 环境创建的，并且大多数 node.js 应用都部署在 Linux 上，因此在 WSL 上进行开发可确保开发环境和生产环境之间的一致性。 若要设置 WSL 开发环境，请参阅[使用 WSL 2 设置 node.js 开发环境](./setup-on-wsl2.md)。

> [!NOTE]
> 如果你使用的是（在某些情况下）需要在 Windows server 上托管 node.js 应用程序，则最常见的情况是[使用反向代理](https://medium.com/intrinsic/why-should-i-use-a-reverse-proxy-if-node-js-is-production-ready-5a079408b2ca)。 可以通过两种方法执行此操作：1）[使用 iisnode](https://harveywilliams.net/blog/installing-iisnode)或[直接](https://dev.to/petereysermans/hosting-a-node-js-application-on-windows-with-iis-as-reverse-proxy-397b)使用。 我们不会保留这些资源，建议[使用 Linux 服务器来托管 node.js 应用程序](https://docs.microsoft.com/azure/app-service/app-service-web-get-started-nodejs)。
