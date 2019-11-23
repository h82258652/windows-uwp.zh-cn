---
title: 开始在 Windows 上使用 NodeJS
description: 帮助初学者在 Windows 上开始使用 node.js 开发的指南。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS，node.js，windows 10，microsoft，学习 NodeJS，windows 上的节点，适用于初学者的 windows 上的节点，在 windows 上通过 windows 开发开发人员，在 windows 上开发 NodeJS
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: 24a2ea5288a627ed884d549ca3b6f9c6930ce34e
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/15/2019
ms.locfileid: "72315131"
---
# <a name="get-started-using-nodejs-on-windows-for-beginners"></a>开始在 Windows 上使用 node.js

如果你不熟悉如何使用 node.js，本指南将帮助你开始了解一些基础知识。

## <a name="prerequisites"></a>必备条件

本指南假定你已完成[用 WSL 2 设置 node.js 开发环境](./setup-on-wsl2.md)的步骤，包括：

- 安装 Windows 10 Insider preview 内部版本18932或更高版本。
- 在 Windows 上启用 WSL 2 功能。
- 安装 Linux 分发版（Ubuntu 18.04 示例）。 可以通过以下方式检查此内容： `wsl lsb_release -a`
- 确保 Ubuntu 18.04 分发在 WSL 2 模式下运行。 （WSL 可以在 v1 或 v2 模式下运行分发。）可以通过打开 PowerShell 并输入以下内容来进行检查： `wsl -l -v`
- 将 Ubuntu 18.04 设置为默认分发，使用 PowerShell，使用： `wsl -s ubuntu 18.04`

> [!NOTE]
> 尽管使用适用于 Linux 的 Windows 子系统还有一些其他设置步骤，但使用 WSL VS Code 2 和远程 WSL 扩展时，将提供流畅的 node.js 开发工作流，并与大部分工具、操作方法文章、教程和[部署环境](https://docs.microsoft.com/en-us/azure/javascript/tutorial-vscode-azure-app-service-node-01)一致。 但是，如果你承诺在 Windows 上使用 node.js，请查看本指南，以便[直接在 windows 上设置 node.js 开发环境](./setup-on-windows.md)。 如果你使用的是（在某些情况下）需要在 Windows server 上托管 node.js 应用程序，则最常见的情况是[使用反向代理](https://medium.com/intrinsic/why-should-i-use-a-reverse-proxy-if-node-js-is-production-ready-5a079408b2ca)。 可以通过两种方法执行此操作：1）[使用 iisnode](https://harveywilliams.net/blog/installing-iisnode)或[直接](https://dev.to/petereysermans/hosting-a-node-js-application-on-windows-with-iis-as-reverse-proxy-397b)使用。 我们不会保留这些资源，建议[使用 Linux 服务器来托管 node.js 应用程序](https://azure.microsoft.com/en-us/develop/nodejs/)。

## <a name="types-of-nodejs-applications"></a>Node.js 应用程序的类型

Node.js 是主要用于创建 web 应用程序的 JavaScript 运行时。 换句话说，它是用于编写应用程序后端的 JavaScript 的服务器端实现。 （尽管许多 node.js 框架还可以处理前端。）下面是你可能使用 node.js 创建的一些示例。

- **单页应用（spa）** ：这些是在浏览器内工作的 web 应用，在每次使用页面获取新数据时，无需重新加载页面。 例如，Spa 包括社交网络应用、电子邮件或地图应用、联机文本或绘图工具等。
- **实时应用（rta）** ：这些是 web 应用，使用户能够在作者发布信息时立即接收信息，而不要求用户（或软件）定期检查源以进行更新。 例如，Rta 包括即时消息应用程序或聊天室、可在浏览器中播放的在线多玩家游戏、联机协作文档、社区存储、视频会议应用，等等。
- **数据流式处理应用程序**：这些应用程序（或服务）在保持连接打开的情况下发送数据/内容（或创建），以便继续根据需要下载数据、内容或组件。 一些示例包括视频和音频流式处理应用。
- **REST api**：这些是为其他人的 web 应用提供数据以与进行交互的接口。 例如，日历 API 服务可以为其他人的本地事件网站使用的音乐会场所提供日期和时间。
- **服务器端呈现的应用程序（SSRs）** ：这些 web 应用可在客户端（浏览器/前端）和服务器（后端）上运行，这允许动态显示的页面（生成的 HTML），并快速获取未知的内容。 这通常称为 "isomorphic" 或 "通用" 应用程序。 SSRs 使用 SPA 方法，因为它们不需要在每次使用时都重新加载。 不过，SRAs 提供了几个可能或对你不重要的好处，例如，在你的网站上进行内容显示在 Google 搜索结果中，并在 Twitter 或 Facebook 等社交媒体上共享指向你的应用程序的链接时提供预览图像。 潜在的缺点是它们需要运行 node.js 服务器。 就示例而言，支持用户希望出现在搜索结果中的事件和社交媒体的社交网络应用可能会受益于 SSR，而电子邮件应用程序可以作为 SPA。 您还可以运行服务器呈现的非 SPA 应用程序，这些应用程序类似于 WordPress 博客。 如您所见，事情会变得很复杂，只需决定重要内容。
- **命令行工具**：可让你自动执行重复的任务，并将你的工具分散到大型 node.js 生态系统中。 命令行工具的一个示例是线路，它用于客户端 URL，并用于从 internet URL 下载内容。 卷通常用于安装诸如 node.js 的功能，或在我们的示例中为 node.js 版本管理器。
- **硬件编程**：尽管与 web 应用相比不太常见，但 node.js 正在不断增加 IoT 使用的普及性，如从传感器收集数据、信标、发送器、汽车或生成大量数据的任何数据。 Node.js 可以启用数据收集，分析数据，在设备与服务器之间来回通信，并根据分析采取措施。 NPM 包含80个以上的包用于 Arduino 控制器、raspberry pi、Intel IoT Edison、各种传感器和蓝牙设备。

## <a name="try-using-nodejs-in-vs-code"></a>尝试在 VS Code 中使用 node.js

1. 打开 Ubuntu 终端并创建新目录： "`mkdir HelloNode`"，然后输入目录： `cd HelloNode`

2. 创建名为 "node.js" 的空 JavaScript 文件： `touch app.js`

3. 在 VS Code：： `code .` 中打开目录和空文件

4. 在 node.js 中创建一个简单的字符串变量，并通过在 "app.config" 文件中输入此内容将字符串的内容发送到控制台：

    ```js
    var msg = 'Hello World';
    console.log(msg);
    ```

5. 用 node.js 运行 "node.js" 文件。 选择 "**查看** > **终端**"，然后选择 "反撇号"，使用 "" 字符，在 VS Code 中打开 Ubuntu 终端。 如果需要更改默认终端，请选择下拉菜单，然后选择 "**选择默认 Shell**"。 然后选择 " **WSL** " 作为要用于 VS Code 的默认终端外壳。

6. 在终端中，输入： `node app.js`。 应会看到以下输出： "Hello World"。

> [!NOTE]
> 请注意，由于你已安装了远程 WSL 扩展，因此，你的目录将在 Ubuntu Linux 系统上运行的远程环境中打开，如 VS Code 窗口左下角的绿色选项卡中所示。 此外，请注意，当你在 "app.config" 文件中键入 `console` 时，VS Code 显示与[`console`](https://developer.mozilla.org/docs/Web/API/Console)对象相关的支持选项，以便你使用 IntelliSense 进行选择。 尝试使用其他[JavaScript 对象](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects)体验 Intellisense。

> [!TIP]
> 如果计划使用多个命令行（Ubuntu、PowerShell、Windows 命令提示符等），或者如果要[自定义终端](https://github.com/microsoft/terminal/blob/master/doc/user-docs/UsingJsonSettings.md)（包括文本、背景色、键绑定等），请考虑尝试使用新的[Windows 终端](https://github.com/microsoft/terminal/blob/master/doc/user-docs/index.md)。

## <a name="set-up-a-basic-web-app-framework-by-using-express"></a>使用 Express 设置基本 web 应用框架

Express 是一个最小、灵活、简化的 node.js 框架，使开发可处理多种类型请求的 web 应用（如 GET、PUT、POST 和 DELETE）变得更加容易。 Express 附带了一个应用程序生成器，它会自动为应用程序创建文件体系结构。

使用 node.js 创建项目：

1. 打开 WSL 终端（Ubuntu 18.04）。
2. 创建新的项目文件夹： `mkdir ExpressProjects` 并输入该目录： "`cd ExpressProjects`"。
3. 使用 Express 创建 HelloWorld 项目模板： `npx express-generator HelloWorld --view=pug`。

>[!NOTE]
> 我们使用此处 `npx` 命令来执行 Express node.js 节点包，而无需实际安装它（或根据你想要的方式对其进行临时安装）。 如果尝试使用 `express` 命令或检查使用以下版本的快速安装版本： `express --version`，你将收到找不到 Express 的响应。 如果要全局安装 Express 以便反复使用，请使用： `npm install -g express-generator`。 您可以使用 `npm list`查看已由 npm 安装的包的列表。 它们将按深度（深层嵌套目录的数目）列出。 安装的包的深度为0。 此包的依赖关系将为深度为1，深度为2的更多依赖项，依此类推。 若要了解详细信息，请参阅 Stackoverflow 上的[npx 与 npm 之间的差异](https://stackoverflow.com/questions/50605219/difference-between-npx-and-npm)。

4. 通过在 VS Code 中打开项目来检查 Express 包含的文件和文件夹，其中包含： `code .`

   Express 生成的文件将创建一个 web 应用，该应用使用一种体系结构，该体系结构最初会显得有点巨大。 你将在 "VS Code**资源管理器**" 窗口（按 Ctrl + Shift + E 查看）中看到生成了以下文件和文件夹：

   - `bin`。 包含启动应用程序的可执行文件。 它将触发服务器（如果未提供任何替代项，则在端口3000上），并设置基本错误处理。 
   - `public`。 包含所有公开访问的文件，其中包括 JavaScript 文件、CSS 样式表、字体文件、图像以及用户连接到网站时所需的任何其他资产。
   - `routes`。 包含应用程序的所有路由处理程序。 在此文件夹中自动生成了两个文件（`index.js` 和 `users.js`），用作如何分离应用程序的路由配置的示例。
   - `views`。 包含模板引擎使用的文件。 如果调用了 render 方法，则将 Express 配置为在此处查找匹配的视图。 默认模板引擎为 Index.jade，但 Index.jade 已不推荐使用 Pug，因此我们使用 `--view` 标志来更改视图（模板）引擎。 您可以通过使用 `express --help`来查看 `--view` 标志选项和其他选项。
   - `app.js`。 应用的起点。 它会加载所有内容并开始为用户请求提供服务。 它基本上是将所有部分都存放在一起的胶水。
   - `package.json`。 包含项目说明、脚本管理器和应用程序清单。 其主要用途是跟踪应用的依赖项及其各自的版本。

5. 你现在需要安装 Express 使用的依赖项，以便生成并运行 HelloWorld Express 应用（用于运行服务器的任务（如 `package.json` 文件中的定义）。 在 VS Code 中，通过选择 "**查看** > **终端**" 打开 WSL 终端（或使用反撇号字符选择 "Ctrl +"），确保仍在 "HelloWorld" 项目目录中。 安装 Express 包依赖项：

```bash
npm install
```

6. 此时，你已为多页 web 应用设置了框架，该框架有权访问大量 Api 和 HTTP 实用工具方法和中间件，使创建可靠的 API 变得更加容易。 通过输入以下内容在虚拟服务器上启动 Express 应用：

```bash
DEBUG=HelloWorld:* npm start
```

> [!TIP]
> 上述命令 `DEBUG=myapp:*` 部分表示你要出于调试目的而要启用日志记录的 node.js。 请记得将 "myapp" 替换为应用名称。 你可以在包文件中的 "名称" 属性下找到你的应用名称。 `npm start` 命令正在告诉 npm 在 package 文件中运行脚本。

7. 你现在可以通过打开 web 浏览器并转到： **localhost： 3000**来查看正在运行的应用程序。

   ![在浏览器中运行的 Express 应用程序的屏幕截图](../images/express-app.png)

8. 现在，HelloWorld Express 应用在浏览器中以本地方式运行，请尝试通过打开项目目录中的 "视图" 文件夹并选择 "pug" 文件进行更改。 打开后，将 `h1= title` 更改为 `h1= "Hello World!"` 并选择 "**保存**" （Ctrl + S）。 通过在 web 浏览器上刷新**localhost： 3000** URL 来查看你的更改。

9. 若要停止运行 Express 应用，请在终端中输入： **Ctrl + C**

## <a name="try-using-a-nodejs-module"></a>尝试使用 Node.js 模块

Node.js 提供的工具可帮助你开发服务器端 web 应用、一些内置的，还可以通过 npm 获得更多的功能。 这些模块可帮助执行许多任务：

|Tool               |用途                                                                                                  |
|:----------------- |:---------------------------------------------------------------------------------------------------------|
|gm，锐          |直接在 JavaScript 代码中进行图像操作，包括编辑、调整大小、压缩等 |
|PDFKit             |PDF 生成                                                                                            |
|node.js       |字符串验证                                                                                         |
|imagemin, UglifyJS2|缩减                                                                                              |
|spritesmith        |动画页面生成                                                                                   |
|winston            |日志记录                                                                                                  |
|node.js       |创建命令行应用程序                                                                       |

让我们使用内置操作系统模块获取有关计算机操作系统的一些信息：

1) 在 WSL （Ubuntu 18.04）终端中，打开 node.js CLI。 输入后，你将看到 `>` 提示，告诉你你正在使用 node.js： `node`

2) 若要确定你当前正在使用的操作系统（这应该会返回响应，让你知道你在 Windows 上），请输入： `os.platform()`

3) 若要检查 CPU 体系结构，请输入： `os.arch()`

4) 若要查看系统中可用的 Cpu，请输入： `os.cpus()`

5) 输入 `.exit` 或通过选择 Ctrl + C 两次，保留 node.js CLI。

   > [!TIP]
   > 您可以使用 node.js OS 模块执行一些操作，例如检查平台并返回特定于平台的变量： Win32/.bat for Windows 开发、达尔文/. sh for Mac/unix、Linux、SunOS 等（例如 `var isWin = process.platform === "win32";`）。

## <a name="next-steps"></a>后续步骤

在本指南中，你已了解有关使用 node.js 执行哪些操作的一些基本信息，已尝试在 VS Code 中使用 node.js 命令行，并使用 node.js 创建了一个简单的 web 应用，并在 web 浏览器中本地运行它，然后尝试使用几个内置 node.js 模块。 若要详细了解如何安装和使用某些流行的 node.js web 框架，请继续学习下一指南，其中包含下一个 node.js （基于响应的服务器呈现的 web 框架）、Nuxt （基于 Vue 的服务器呈现的 web 框架）和 Gatsby （基于响应的静态呈现的 web 框架。 还可以跳过了解如何使用 MongoDB 或 PostgreSQL 数据库或 Docker 容器。

- [在 Windows 上开始处理 node.js web 框架](./web-frameworks.md)
- [开始将 node.js 应用连接到数据库](./databases.md)
- [开始将 Docker 容器与 node.js 配合使用](./containers.md)
