---
title: 开始在 Windows 上使用 Node.js Web 框架
description: 本指南可帮助你开始在 Windows 上使用 Node.js Web 框架。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS, Node.js, windows 10, microsoft, 了解 nodejs, windows 上的 node, wsl 上的 node, windows 中 linux 上的 node, 在 windows 上安装 node, 具有 vs code 的 nodejs, 通过 windows 上的 node 进行开发, 通过 windows 上的 nodejs 进行开发, 在 WSL 上安装 node, 适用于 Linux 的 Windows 子系统上的 NodeJS
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: a8ce1d08136a74504e1b3bad26feadd61b72068f
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "72517788"
---
# <a name="get-started-with-nodejs-web-frameworks-on-windows"></a>开始在 Windows 上使用 Node.js Web 框架

本分步指南可帮助你开始在 Windows 上使用 Node.js Web 框架，包括 Next.js、Nuxt.js 和 Gatsby。

## <a name="prerequisites"></a>必备条件

本指南假定你已经完成了[使用 WSL 2 设置 Node.js 开发环境](./setup-on-wsl2.md)的步骤，具体包括：

- 安装 Windows 10 Insider Preview 内部版本 18932 或更高版本。
- 在 Windows 上启用 WSL 2 功能。
- 安装 Linux 分发版（本示例为 Ubuntu 18.04）。 可输入以下内容进行检查：`wsl lsb_release -a`
- 确保 Ubuntu 18.04 分发版在 WSL 2 模式下运行。 （WSL 在 v1 或 v2 模式下均可运行分发版。）可通过打开 PowerShell 并输入以下内容进行检查：`wsl -l -v`
- 使用 PowerShell，通过输入以下内容将 Ubuntu 18.04 设置为默认分发版：`wsl -s ubuntu 18.04`

## <a name="get-started-with-nextjs"></a>Next.js 入门

Next.js 是一个框架，用于基于 React.js、Node.js、Webpack 和 Babel.js 创建由服务器呈现的 JavaScript 应用。 它基本上就是 React 的项目样板，精巧之处在于其着眼于最佳做法，可使你以简单、一致的方式创建“通用”Web 应用而几乎不需要进行任何配置。 这些由服务器呈现的“通用”Web 应用有时也称为“同构”，意味着代码在客户端和服务器之间共享。

若要创建 Next.js 项目（包括安装 next、react 和 react-dom），请执行以下操作：

1. 打开 WSL 终端（即 Ubuntu 18.04）。

2. 创建新项目文件夹 `mkdir NextProjects` 并输入以下目录：`cd NextProjects`。

3. 安装 Next.js 并创建项目（将“my-next-app”替换为你喜欢的任意应用名称）：`npm create next-app my-next-app`。

4. 安装程序包后，将目录更改为新应用的文件夹 `cd my-next-app`，然后使用 `code .` 在 VS Code 中打开 Next.js 项目。 这将使你能够查看已为应用创建的 Next.js 框架。 请注意，VS Code 在 WSL-Remote 环境中打开了应用（如 VS Code 窗口左下角的绿色选项卡中所示）。 这意味着，当你使用 VS Code 在 Windows OS 上进行编辑时，它仍会在 Linux OS 上运行应用。

    ![WSL-Remote 扩展](../images/wsl-remote-extension.png)

5. 安装 Next.js 后，需要知道以下 3 个命令：

    - `npm run dev`，用于运行具有热重载、文件监视和任务重新运行的开发实例。
    - `npm run build`，用于编译项目。
    - `npm start`，用于在生产模式下启动应用。

    打开集成在 VS Code 中的 WSL 终端（“视图”>“终端”）  。 确保终端路径指向项目目录（即 `~/NextProjects/my-next-app$`）。 然后尝试使用以下命令运行新的 Next.js 应用的开发实例：`npm run dev`

6. 本地开发服务器将启动，项目页面生成好后，终端将显示“已成功编译 - 在 [http://localhost:3000](http://localhost:3000) 上准备就绪”。 选择此 localhost 链接以在 Web 浏览器中打开新的 Next.js 应用。

    ![在 localhost:3000 中运行的 Next.js 应用](../images/next-app.png)

7. 在 VS Code 编辑器中打开 `pages/index.js` 文件。 查找页面标题 `<h1 className='title'>Welcome to Next.js!</h1>`，并将其更改为 `<h1 className='title'>This is my new Next.js app!</h1>`。 在 Web 浏览器仍然打开到 localhost:3000 的情况下，保存你的更改；请注意，热重载功能会自动在浏览器中编译和更新更改。

8. 让我们看看 Next.js 是如何处理错误的。 删除 `</h1>` 结束标志以使标题代码现在如下所示：`<h1 className='title'>This is my new Next.js app!`。 保存此更改；请注意，浏览器和终端中将显示“无法编译”错误，以告知你需要 `<h1>` 的结束标志。 替换 `</h1>` 结束标志，保存，然后页面将重载。

你可以通过选择 F5 键或转到菜单栏中的“视图”>“调试”(Ctrl+Shift+D) 和“视图”>“调试控制台”(Ctrl+Shift+Y)，将 VS Code 的调试程序与 Next.js 应用结合使用   。 如果在“调试”窗口中选择“齿轮”图标，则将为你创建启动配置 (`launch.json`) 文件，以保存调试设置的详细信息。 若要了解详细信息，请参阅 [VS Code 调试](https://code.visualstudio.com/docs/nodejs/nodejs-debugging)。

![VS Code 调试窗口和 launch.json 配置图标](../images/vscode-debug-launch-configuration.png)

若要了解有关 Next.js 的详细信息，请参阅 [Next.js 文档](https://nextjs.org/docs)。

## <a name="get-started-with-nuxtjs"></a>Nuxt.js 入门

Nuxt.js 是一个框架，用于基于 Vue.js、Node.js、Webpack 和 Babel.js 创建由服务器呈现的 JavaScript 应用。 其灵感来自于 Next.js。 它基本上就是 Vue 的项目样板。 与 Next.js 一样，其精巧之处在于，其着眼于最佳做法，可使你以简单、一致的方式创建“通用”Web 应用而几乎不需要进行任何配置。 这些由服务器呈现的“通用”Web 应用有时也称为“同构”，意味着代码在客户端和服务器之间共享。

若要创建 Nuxt.js 项目（包括回答一系列有关要安装哪种集成服务器端框架、UI 框架、测试框架、模式、模块和 Linter 的问题），请执行以下操作：

1. 打开 WSL 终端（即 Ubuntu 18.04）。

2. 创建新项目文件夹 `mkdir NuxtProjects` 并输入以下目录：`cd NuxtProjects`。

3. 安装 Nuxt.js 并创建项目（将“my-nuxt-app”替换为你喜欢的任意应用名称）：`npm create nuxt-app my-nuxt-app`

4. Nuxt.js 安装程序现在将询问以下问题：
    - 项目名称：my-nuxtjs-app
    - 项目说明：Nuxt.js 应用的说明。
    - 作者姓名：我使用 GitHub 别名。
    - 选择包管理器：YARN 或 NPM - 我们以 NPM 为例  。
    - 选择 UI 框架：无、Ant Design Vue、Bootstrap Vue 等。 在此示例中，我们选择 Vuetify，但 Vue Community 创建了一个不错的 [UI 框架对比摘要](https://vue-community.org/guide/ecosystem/ui-libraries.html#summary-tldr)，可帮助你选择最适合自己项目的框架  。
    - 选择自定义服务器框架：无、AdonisJs、Express、Fastify 等。 在此示例中，我们选择“无”，但你可以在 Dev.to 网站上查找 [2019-2020 服务器框架比较](https://dev.to/santypk4/introducing-the-best-10-node-js-frameworks-for-2019-and-2020-mcm)  。
    - 选择 Nuxt.js 模块（使用空格键选择模块，或者，在不需要的情况下直接输入）：Axios（用于简化 HTTP 请求）或 [PWA 支持](https://pwa.nuxtjs.org/)（用于添加服务工作进程、manifest.json 文件等）。 在此示例中，我们不为此示例添加模块。
    - 选择 Lint 分析工具：ESLint、Prettier、Lint 暂存文件  。 选择 ESLint（一种用于分析代码以及警告潜在错误的工具）  。
    - 选择测试框架：无、Jest、AVA  。 选择“无”，因为我们不会在此快速入门中介绍测试  。
    - 选择呈现模式：“通用 (SSR)”或“单页应用 (SPA)”  。 在此示例中，我们选择“通用 (SSR)”，但 [Nuxt.js 文档](https://nuxtjs.org/guide#server-rendered-universal-ssr-)指出了它们的一些不同之处 - SSR 需要运行 Node.js 服务器才能在服务器端呈现应用，而 SPA 用于静态托管  。
    - 选择开发工具 jsconfig.json（建议用于 VS Code，以便 Intellisense 代码补全功能正常工作） 

5. 创建项目后，输入 `cd my-nuxtjs-app` 进入 Nuxt.js 项目目录，然后输入 `code .` 在 VS Code WSL-Remote 环境中打开项目。

    ![WSL-Remote 扩展](../images/wsl-remote-extension.png)

6. 安装 Nuxt.js 后，需要知道以下 3 个命令：

    - `npm run dev`，用于运行具有热重载、文件监视和任务重新运行的开发实例。
    - `npm run build`，用于编译项目。
    - `npm start`，用于在生产模式下启动应用。

    打开集成在 VS Code 中的 WSL 终端（“视图”>“终端”）  。 确保终端路径指向项目目录（即 `~/NuxtProjects/my-nuxt-app$`）。 然后尝试使用以下命令运行新的 Nuxt.js 应用的开发实例：`npm run dev`

6. 本地开发服务器将启动（为客户端和服务器编译显示某种很酷的进度栏）。 项目生成后，终端将显示“已成功编译”以及编译所需的时间。 将 Web 浏览器指向 [http://localhost:3000](http://localhost:3000) 以打开新的 Nuxt.js 应用。

    ![在 localhost:3000 中运行的 Nuxt.js 应用](../images/nuxt-app.png)

7. 在 VS Code 编辑器中打开 `pages/index.vue` 文件。 查找页面标题 `<v-card-title class="headline">Welcome to the Vuetify + Nuxt.js template</v-card-title>`，并将其更改为 `<v-card-title class="headline">This is my new Nuxt.js app!</v-card-title>`。 在 Web 浏览器仍然打开到 localhost:3000 的情况下，保存你的更改；请注意，热重载功能会自动在浏览器中编译和更新更改。

8. 让我们看看 Nuxt.js 是如何处理错误的。 删除 `</v-card-title>` 结束标志以使标题代码现在如下所示：`<v-card-title class="headline">This is my new Nuxt.js app!`。 保存此更改；请注意，浏览器和终端中将显示编译错误，以告知你 `<v-card-title>` 的结束标志缺失以及可在代码中找到错误的行号。 替换 `</v-card-title>` 结束标志，保存，然后页面将重载。

你可以通过选择 F5 键或转到菜单栏中的“视图”>“调试”(Ctrl+Shift+D) 和“视图”>“调试控制台”(Ctrl+Shift+Y)，将 VS Code 的调试程序与 Nuxt.js 应用结合使用   。 如果在“调试”窗口中选择“齿轮”图标，则将为你创建启动配置 (`launch.json`) 文件，以保存调试设置的详细信息。 若要了解详细信息，请参阅 [VS Code 调试](https://code.visualstudio.com/docs/nodejs/nodejs-debugging)。

![VS Code 调试窗口和 launch.json 配置图标](../images/vscode-debug-launch-configuration.png)

若要了解有关 Nuxt.js 的详细信息，请参阅 [Nuxt.js 指南](https://nuxtjs.org/guide)。

## <a name="get-started-with-gatsbyjs"></a>Gatsby.js 入门

Gatsby.js 是基于 React.js 的静态站点生成器框架，与由服务器呈现的框架（如 Next.js 和 Nuxt.js）相反。 静态站点生成器在生成时生成静态 HTML。 它不需要服务器。 Next.js 和 Nuxt.js 在运行时（每次传入新请求时）生成 HTML。 它们需要服务器才能运行。 Gatsby 还规定了如何在应用中处理数据（通过 GraphQL），而 Next.js 和 Nuxt.js 则将此决策交给你决定。

若要创建 Gatsby.js 项目，请执行以下操作：

1. 打开 WSL 终端（即 Ubuntu 18.04）。
2. 创建新项目文件夹 `mkdir GatsbyProjects` 并输入以下目录：`cd GatsbyProjects`
3. 使用以下 NPM 安装 Gatsby CLI：`npm install -g gatsby-cli`。 安装后，使用 `gatsby --version` 检查版本。
4. 创建 Gatsby.js 项目：`gatsby new my-gatsby-app`
5. 安装程序包后，将目录更改为新应用的文件夹 `cd my-gatsby-app`，然后使用 `code .` 在 VS Code 中打开 Gatsby 项目。 这将使你能够查看通过使用 VS Code 的文件资源管理器为应用创建的 Gatsby.js 框架。 请注意，VS Code 在 WSL-Remote 环境中打开了应用（如 VS Code 窗口左下角的绿色选项卡中所示）。 这意味着，当你使用 VS Code 在 Windows OS 上进行编辑时，它仍会在 Linux OS 上运行应用。

    ![WSL-Remote 扩展](../images/wsl-remote-extension.png)

6. 安装 Gatsby 后，需要知道以下 3 个命令：

    - `gatsby develop`，用于运行具有热重载的开发实例。
    - `gatsby build`，用于创建生产版本。
    - `gatsby serve`，用于在生产模式下启动应用。

    打开集成在 VS Code 中的 WSL 终端（“视图”>“终端”）  。 确保终端路径指向项目目录（即 `~/GatsbyProjects/my-gatsby-app$`）。 然后尝试使用以下命令运行新应用的开发实例：`gatsby develop`

7. 新 Gatsby 项目编译完成后，终端将显示“你现在可以在浏览器中查看 gatsby-starter-default。 [http://localhost:8000/](http://localhost:8000/)”。 选择此 localhost 链接以查看在 Web 浏览器中生成的新项目。

> [!NOTE]
> 你会注意到你的终端输出还会告知你以下内容，你可以“查看 GraphiQL（一个浏览器内置 IDE）来浏览站点的数据和架构：[http://localhost:8000/___graphql](http://localhost:8000/___graphql)”。 GraphQL 可将 API 合并到内置于 Gatsby 中的自编文档 IDE (GraphiQL) 中。 除了浏览站点的数据和架构之外，你还可以执行查询、变化和订阅之类的 GraphQL 操作。 有关详细信息，请参阅[GraphiQL 简介](https://www.gatsbyjs.org/docs/running-queries-with-graphiql/)。

8. 在 VS Code 编辑器中打开 `src/pages/index.js` 文件。 查找页面标题 `<h1 >Hi people</h1>`，并将其更改为 `<h1 >Hi (Your Name)!</h1>`。 在 Web 浏览器仍然打开到 localhost:8000 的情况下，保存你的更改；请注意，热重载功能会自动在浏览器中编译和更新更改。

    ![在 localhost:3000 中运行的 Gatsby.js 应用](../images/gatsby-app.png)

9. 让我们看看 Next.js 是如何处理错误的。 删除 `</h1>` 结束标志以使标题代码现在如下所示：`<h1>Hi (Your Name)!`。 保存此更改；请注意，浏览器和终端中将显示“无法编译”错误，以告知你需要 `<h1>` 的结束标志。 替换 `</h1>` 结束标志，保存，然后页面将重载。

你可以通过选择 F5 键或转到菜单栏中的“视图”>“调试”(Ctrl+Shift+D) 和“视图”>“调试控制台”(Ctrl+Shift+Y)，将 VS Code 的调试程序与 Next.js 应用结合使用   。 如果在“调试”窗口中选择“齿轮”图标，则将为你创建启动配置 (`launch.json`) 文件，以保存调试设置的详细信息。 若要了解详细信息，请参阅 [VS Code 调试](https://code.visualstudio.com/docs/nodejs/nodejs-debugging)。

![VS Code 调试窗口和 launch.json 配置图标](../images/vscode-debug-launch-configuration.png)

若要了解有关 Gatsby 的详细信息，请参阅 [Gatsby.js 文档](https://www.gatsbyjs.org/docs/)。
