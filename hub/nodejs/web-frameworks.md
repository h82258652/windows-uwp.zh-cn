---
title: 在 Windows 上开始处理 node.js web 框架
description: 本指南可帮助你开始使用 Windows 上的 node.js web 框架。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS，node.js，windows 10，microsoft，学习 NodeJS，windows 上的节点，wsl 上的节点，windows 上的节点，在 windows 上安装节点，NodeJS with vs code，在 windows 上安装节点，在 windows 上进行开发，在 NODEJS 上安装节点，在 Windows 上安装节点适用于 Linux 的子系统
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: a8ce1d08136a74504e1b3bad26feadd61b72068f
ms.sourcegitcommit: 60d2d15dd0d365f82e4e90e4bc34b40cf5b4a247
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2019
ms.locfileid: "72517788"
---
# <a name="get-started-with-nodejs-web-frameworks-on-windows"></a>在 Windows 上开始处理 node.js web 框架

本指南可帮助你开始使用 Windows 上的 node.js web 框架，包括下 node.js、Nuxt 和 Gatsby。

## <a name="prerequisites"></a>必备条件

本指南假定你已完成[用 WSL 2 设置 node.js 开发环境](./setup-on-wsl2.md)的步骤，包括：

- 安装 Windows 10 Insider preview 内部版本18932或更高版本。
- 在 Windows 上启用 WSL 2 功能。
- 安装 Linux 分发版（Ubuntu 18.04 示例）。 可以通过以下方式检查此内容： `wsl lsb_release -a`
- 确保 Ubuntu 18.04 分发在 WSL 2 模式下运行。 （WSL 可以在 v1 或 v2 模式下运行分发。）可以通过打开 PowerShell 并输入以下内容来进行检查： `wsl -l -v`
- 使用 PowerShell 将 Ubuntu 18.04 设置为默认分发，使用： `wsl -s ubuntu 18.04`

## <a name="get-started-with-nextjs"></a>Next.js 入门

接下来，.js 是一个框架，用于基于响应 node.js、node.js、Webpack 和 Babel 创建服务器呈现的 JavaScript 应用。 它基本上是一个用于做出反应的项目样板，其中重点是最佳实践，这使你能够以简单、一致的方式创建 "通用" web 应用，几乎不需要任何配置。 这些 "通用" 服务器呈现的 web 应用有时也称为 "isomorphic"，这意味着在客户端和服务器之间共享代码。

创建下一个 .js 项目，其中包括安装下一个、响应和响应 dom：

1. 打开 WSL 终端（即Ubuntu 18.04）。

2. 创建新的项目文件夹： `mkdir NextProjects` 并输入该目录： "`cd NextProjects`"。

3. 安装下一 .js 并创建项目（将 "我的下一个应用" 替换为你想要调用应用的任何内容）： `npm create next-app my-next-app`。

4. 安装程序包后，将目录更改为新应用文件夹，`cd my-next-app`，然后使用 `code .` 在 VS Code 中打开下一个 .js 项目。 这将允许你查看已为你的应用程序创建的下一个 .js 框架。 请注意，VS Code 在 WSL 环境中打开你的应用程序（如 VS Code 窗口左下角的绿色选项卡中所述）。 这意味着，当你使用 VS Code 在 Windows OS 上进行编辑时，它仍会在 Linux OS 上运行你的应用程序。

    ![WSL-远程扩展](../images/wsl-remote-extension.png)

5. 接下来，需要知道3个命令：

    - `npm run dev`，用于运行具有热重装的开发实例、文件监视和任务重新运行。
    - 用于编译项目的 `npm run build`。
    - `npm start`，用于在生产模式下启动应用。

    打开 VS Code 中集成的 WSL 终端（**查看 > 终端**）。 请确保终端路径指向项目目录（`~/NextProjects/my-next-app$`）。 然后尝试使用以下内容运行新的下一个 node.js 应用的开发实例： `npm run dev`

6. 本地开发服务器将开始，并且在生成项目页后，终端将显示 "已成功编译[http://localhost:3000](http://localhost:3000)"。 选择 "此 localhost 链接"，在 web 浏览器中打开新的下一 .js 应用。

    ![在 localhost 中运行的下一个 .js 应用：3000](../images/next-app.png)

7. 在 VS Code 编辑器中打开 `pages/index.js` 文件。 查找页面标题 `<h1 className='title'>Welcome to Next.js!</h1>` 并将其更改为 `<h1 className='title'>This is my new Next.js app!</h1>`。 当你的 web 浏览器仍打开到 localhost：3000时，请保存你的更改，并注意热重新加载功能会自动在浏览器中编译和更新你的更改。

8. 让我们看一下接下来如何处理错误。 删除 `</h1>` 结束标记，使标题代码现在如下所示： `<h1 className='title'>This is my new Next.js app!`。 保存此更改，请注意，浏览器和终端中将显示 "无法编译" 错误，让你知道 `<h1>` 的结束标记应为。 替换 `</h1>` 结束标记，保存，然后重新加载页面。

您可以通过选择 F5 键，或在菜单栏中**查看 "查看 > 调试**" （Ctrl + Shift + D）和 "**查看 > 调试控制台**（Ctrl + shift + Y）"，将 VS Code 的调试程序与下一个 .js 应用程序结合使用。 如果在 "调试" 窗口中选择 "齿轮" 图标，将会创建一个启动配置（`launch.json`）文件，以便保存调试设置的详细信息。 若要了解详细信息，请参阅[VS Code 调试](https://code.visualstudio.com/docs/nodejs/nodejs-debugging)。

![VS Code 调试窗口并启动 json 配置图标](../images/vscode-debug-launch-configuration.png)

若要了解有关下一 .js 的详细信息，请参阅[下一篇 node.js 文档](https://nextjs.org/docs)。

## <a name="get-started-with-nuxtjs"></a>Nuxt.js 入门

Nuxt 是一个框架，用于基于 Vue、node.js、Webpack 和 Babel 创建服务器呈现的 JavaScript 应用。 它通过下一 .js 成为灵感。 它基本上是 Vue 的项目样板。 正如接下来，我们将重点关注最佳做法，并使你能够以简单、一致的方式创建 "通用" web 应用，几乎不需要任何配置。 这些 "通用" 服务器呈现的 web 应用有时也称为 "isomorphic"，这意味着在客户端和服务器之间共享代码。

若要创建 Nuxt 项目，该项目将包括回答一系列有关集成服务器端框架、UI 框架、测试框架、模式、模块以及要安装的 linter 的问题：

1. 打开 WSL 终端（即Ubuntu 18.04）。

2. 创建新的项目文件夹： `mkdir NuxtProjects` 并输入该目录： "`cd NuxtProjects`"。

3. 安装 Nuxt 并创建一个项目（将 "Nuxt" 应用程序替换为要调用应用程序的任何内容）： `npm create nuxt-app my-nuxt-app`

4. Nuxt 安装程序现在会询问以下问题：
    - 项目名称： nuxtjs-应用
    - 项目说明：我的 Nuxt 应用程序的说明。
    - 作者姓名：我使用 GitHub 别名。
    - 选择包管理器： "Yarn" 或 " **Npm** "，我们的示例使用 Npm。
    - 选择 UI framework：无，Ant 设计 Vue，启动 Vue，等等。 在此示例中，我们选择**Vuetify** ，但 Vue 社区创建了[比较这些 UI 框架](https://vue-community.org/guide/ecosystem/ui-libraries.html#summary-tldr)以帮助你选择最适合你的项目的更好摘要。
    - 选择自定义服务器框架： None、AdonisJs、Express、Fastify 等。 在此示例中，请选择 "**无**"，但可以在 Dev.to 站点上找到[2019-2020 服务器 framework 比较](https://dev.to/santypk4/introducing-the-best-10-node-js-frameworks-for-2019-and-2020-mcm)。
    - 选择 Nuxt 模块（使用空格键选择模块，或只需输入（如果不需要））： Axios （用于简化 HTTP 请求）或[PWA 支持](https://pwa.nuxtjs.org/)（用于添加服务辅助角色、.manifest 文件，等等）。 让我们不要为此示例添加模块。
    - 选择 linting 工具： **ESLint**、外观、不起毛的暂存文件。 接下来，选择 " **ESLint** " （一种用于分析代码的工具，并向你发出可能的错误警告）。
    - 选择测试框架： **None**、JEST、AVA。 请选择 "**无**"，因为本快速入门中不介绍测试。
    - 选择呈现模式：**通用（SSR）** 或单页应用（SPA）。 对于我们的示例，我们选择**通用（SSR）** ，但[Nuxt 文档](https://nuxtjs.org/guide#server-rendered-universal-ssr-)指出了某些差异--SSR 需要运行到服务器的 node.js 服务器-呈现你的应用程序和 SPA 用于静态宿主。
    - 选择开发工具： **jsconfig.json** （建议 VS Code 以便 Intellisense 代码完成工作）

5. 创建你的项目后，`cd my-nuxtjs-app` 进入你的 Nuxt 项目目录，然后输入 `code .` 在 VS Code WSL-远程环境中打开该项目。

    ![WSL-远程扩展](../images/wsl-remote-extension.png)

6. 安装 Nuxt 后，需要知道3个命令：

    - `npm run dev`，用于运行具有热重装的开发实例、文件监视和任务重新运行。
    - 用于编译项目的 `npm run build`。
    - `npm start`，用于在生产模式下启动应用。

    打开 VS Code 中集成的 WSL 终端（**查看 > 终端**）。 请确保终端路径指向项目目录（`~/NuxtProjects/my-nuxt-app$`）。 然后尝试使用以下 `npm run dev` 运行新的 Nuxt 应用程序的开发实例

6. 本地开发服务器将启动（为客户端和服务器编译显示某种酷的进度条）。 完成生成项目后，终端将显示 "已成功编译" 以及编译所需的时间。 将 web 浏览器指向[http://localhost:3000](http://localhost:3000)以打开新的 Nuxt 应用。

    ![在 localhost 中运行的 Nuxt 应用：3000](../images/nuxt-app.png)

7. 在 VS Code 编辑器中打开 `pages/index.vue` 文件。 查找页面标题 `<v-card-title class="headline">Welcome to the Vuetify + Nuxt.js template</v-card-title>` 并将其更改为 `<v-card-title class="headline">This is my new Nuxt.js app!</v-card-title>`。 当你的 web 浏览器仍打开到 localhost：3000时，请保存你的更改，并注意热重新加载功能会自动在浏览器中编译和更新你的更改。

8. 让我们看看 Nuxt 如何处理错误。 删除 `</v-card-title>` 结束标记，使标题代码现在如下所示： `<v-card-title class="headline">This is my new Nuxt.js app!`。 保存此更改，请注意，编译错误将显示在浏览器和终端中，让你知道缺少 `<v-card-title>` 的结束标记，以及在代码中可以找到错误的行号。 替换 `</v-card-title>` 结束标记，保存，然后重新加载页面。

你可以通过选择 F5 键，或通过**查看 > 调试**（Ctrl + Shift + D）并在菜单栏中**查看 > 调试控制台**（Ctrl + shift + Y）来使用 VS Code 的 Nuxt 调试器。 如果在 "调试" 窗口中选择 "齿轮" 图标，将会创建一个启动配置（`launch.json`）文件，以便保存调试设置的详细信息。 若要了解详细信息，请参阅[VS Code 调试](https://code.visualstudio.com/docs/nodejs/nodejs-debugging)。

![VS Code 调试窗口并启动 json 配置图标](../images/vscode-debug-launch-configuration.png)

若要了解有关 Nuxt 的详细信息，请参阅[Nuxt 指南](https://nuxtjs.org/guide)。

## <a name="get-started-with-gatsbyjs"></a>Gatsby.js 入门

Gatsby 是基于响应的静态站点生成器框架，而不是服务器呈现的，如下一篇 .js 和 Nuxt。 静态站点生成器在生成时生成静态 HTML。 它不需要服务器。 接下来，Nuxt 将在运行时（每次传入新请求时）生成 HTML。 它们需要运行服务器。 Gatsby 还规定了如何在应用中处理数据（通过 GraphQL），而在下 node.js 和 Nuxt 中，会将决策留给你。

创建 Gatsby 项目：

1. 打开 WSL 终端（即Ubuntu 18.04）。
2. 创建新的项目文件夹： `mkdir GatsbyProjects` 并输入该目录： `cd GatsbyProjects`
3. 使用 npm 安装 Gatsby CLI： `npm install -g gatsby-cli`。 安装后，请查看 `gatsby --version`的版本。
4. 创建 Gatsby 项目： `gatsby new my-gatsby-app`
5. 安装程序包后，将目录更改为新应用文件夹，`cd my-gatsby-app`，然后使用 `code .` 在 VS Code 中打开 Gatsby 项目。 这将允许你查看已使用 VS Code 的文件资源管理器为应用创建的 Gatsby 框架。 请注意，VS Code 在 WSL 环境中打开你的应用程序（如 VS Code 窗口左下角的绿色选项卡中所述）。 这意味着，当你使用 VS Code 在 Windows OS 上进行编辑时，它仍会在 Linux OS 上运行你的应用程序。

    ![WSL-远程扩展](../images/wsl-remote-extension.png)

6. 安装 Gatsby 后，需要知道3个命令：

    - 用于使用热重装运行开发实例的 `gatsby develop`。
    - 用于创建生产生成的 `gatsby build`。
    - `gatsby serve`，用于在生产模式下启动应用。

    打开 VS Code 中集成的 WSL 终端（**查看 > 终端**）。 请确保终端路径指向项目目录（`~/GatsbyProjects/my-gatsby-app$`）。 然后尝试使用以下内容运行新应用的开发实例： `gatsby develop`

7. 新 Gatsby 项目完成编译后，终端将显示 "你现在可以在浏览器中查看 Gatsby-默认值"。 [http://localhost:8000/](http://localhost:8000/)"。 选择 "此 localhost" 链接以查看在 web 浏览器中生成的新项目。

> [!NOTE]
> 你会注意到，你还会发现，你可以 "查看 GraphiQL （一个浏览器内的 IDE）来浏览你的站点的数据和架构： [http://localhost:8000/___graphql](http://localhost:8000/___graphql)。" GraphQL 将 Api 合并到内置于 Gatsby 中的自记录 IDE （GraphiQL）。 除了浏览站点的数据和架构，还可以执行 GraphQL 操作，例如查询、突变和订阅。 有关详细信息，请参阅[GraphiQL 简介](https://www.gatsbyjs.org/docs/running-queries-with-graphiql/)。

8. 在 VS Code 编辑器中打开 `src/pages/index.js` 文件。 查找页面标题 `<h1 >Hi people</h1>` 并将其更改为 `<h1 >Hi (Your Name)!</h1>`。 如果 web 浏览器仍打开到 localhost：8000，请保存更改，并注意热重新加载功能会自动在浏览器中编译和更新更改。

    ![在 localhost 中运行的 Gatsby 应用：3000](../images/gatsby-app.png)

9. 让我们看一下接下来如何处理错误。 删除 `</h1>` 结束标记，使标题代码现在如下所示： `<h1>Hi (Your Name)!`。 保存此更改，请注意，浏览器和终端中将显示 "无法编译" 错误，让你知道 `<h1>` 的结束标记应为。 替换 `</h1>` 结束标记，保存，然后重新加载页面。

您可以通过选择 F5 键，或在菜单栏中**查看 "查看 > 调试**" （Ctrl + Shift + D）和 "**查看 > 调试控制台**（Ctrl + shift + Y）"，将 VS Code 的调试程序与下一个 .js 应用程序结合使用。 如果在 "调试" 窗口中选择 "齿轮" 图标，将会创建一个启动配置（`launch.json`）文件，以便保存调试设置的详细信息。 若要了解详细信息，请参阅[VS Code 调试](https://code.visualstudio.com/docs/nodejs/nodejs-debugging)。

![VS Code 调试窗口并启动 json 配置图标](../images/vscode-debug-launch-configuration.png)

若要了解有关 Gatsby 的详细信息，请参阅[Gatsby 文档](https://www.gatsbyjs.org/docs/)。
