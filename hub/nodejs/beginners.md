---
title: 开始在 Windows 上使用 NodeJS（初学者）
description: 本指南可帮助初学者开始在 Windows 上进行 Node.js 开发。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS, Node.js, windows 10, microsoft, 学习 nodejs, windows 上的 node, windows 上的 node 初学者, windows 上的 node 开发, windows 上的 nodejs 开发人员
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: d40d701dc3ae973e0834d0b329527e69854b9e36
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86492862"
---
# <a name="get-started-using-nodejs-on-windows-for-beginners"></a>开始在 Windows 上使用 Node.js（初学者）

如果你不熟悉如何使用 Node.js，本指南介绍了一些基础知识，可帮助你入门。

## <a name="prerequisites"></a>必备条件

本指南假定你已经完成了[在本机 Windows 上设置 Node.js 开发环境](./setup-on-windows.md)的步骤，具体包括：

- 安装 Node.js 版本管理器。
- 安装 Visual Studio Code。

在 Windows 上直接安装 Node.js 是开始执行基础 Node.js 操作最直接的方式，只需进行极少的设置。

在你准备好使用 Node.js 来开发用于生产的应用程序后（通常涉及到部署到 Linux 服务器），我们建议[使用 WSL2 设置 Node.js 开发环境](./setup-on-wsl2.md)。 尽管可以在 Windows 服务器上部署 Web 应用，但[使用 Linux 服务器托管 Node.js 应用](https://azure.microsoft.com/develop/nodejs/)更为常见。

## <a name="types-of-nodejs-applications"></a>Node.js 应用程序的类型

Node.js 是主要用于创建 Web 应用程序的 JavaScript 运行时。 换句话说，它是 JavaScript 的服务器端实现，用于编写应用程序后端。 （尽管许多 Node.js 框架也可以处理前端。）以下是可以使用 Node.js 创建的应用的一些示例。

- **单页应用 (SPA)：** 这类 Web 应用在浏览器中运行，每次用其获取新数据时无需重新加载页面。 示例 SPA 包括社交网络应用、电子邮件或地图应用、联机文本工具或绘图工具等。
- **实时应用 (RTA)：** 这些 Web 应用使用户能够在创作者发布信息后立即接收该信息，而不要求用户（或软件）定期检查源以获取更新。 一些示例 RTA 包括即时消息传递应用或聊天室、可在浏览器中运行的在线多玩家游戏、联机协作文档、社区存储、视频会议应用。
- **数据流式处理应用**：此类应用（或服务）会在收到（或创建）数据/内容时立即发送它们，同时保持连接，以便根据需要继续下载后续数据、内容或组件。 示例包括音视频流式传输应用。
- **REST API**：这类接口提供数据，供他人的 Web 应用进行交互。 例如，日历 API 服务可以提供音乐会现场的日期和时间信息，以供他人的本地活动网站使用。
- **服务器端呈现的应用 (SSR)** ：此类 Web 应用可在客户端（浏览器/前端）和服务器（后端）上运行，允许动态页面显示任何已知内容（为其生成 HTML）并在未知内容可用时迅速进行抓取。 这些应用通常称为“同构”或“通用”应用程序。 SSR 使用 SPA 方法，因为它们不需要在每次使用时都重新加载。 不过，SSR 提供了一些重要性因人而异的优势，例如，让你的站点内容显示在 Google 搜索结果中，或者在 Twitter 或 Facebook 等社交媒体上分享应用链接时提供预览图像。 其潜在的缺点是需要 Node.js 服务器持续运行。 例如，为用户希望显示在搜索结果和社交媒体中的活动提供支持的社交网络应用可能得益于 SSR，但电子邮件应用作为 SPA 即可满足需求。 你还可以运行服务器呈现的非 SPA 应用（例如 WordPress 博客）。 如你所见，情况可能比较复杂，你需要确定重要事项。
- **命令行工具**：通过这些工具，可以自动执行重复性任务，然后将你的工具分发到大型 Node.js 生态系统。 cURL（即客户端 URL）是命令行工具的一个示例，用于从 Internet URL 下载内容。 cURL 常用于安装 Node.js 等工具（在本例中为 Node.js 版本管理器）。
- **硬件编程**：虽然不如 Web 应用常用，但 Node.js 也越来越多地被用于 IoT 领域，例如从传感器、信标、发射机、发动机或其他会生成大量数据的装置收集数据。 Node.js 可支持数据收集、数据分析、设备和服务器之间的通信往来以及基于分析的措施实施。 NPM 包含 80 多个包，用于 Arduino 控制器、Raspberry Pi、Intel IoT Edison、各种传感器和蓝牙设备。

## <a name="try-using-nodejs-in-vs-code"></a>尝试在 VS Code 中使用 Node.js

1. 打开命令行（命令提示符、PowerShell 或任何你喜欢的工具），并新建一个目录（`mkdir HelloNode`），然后进入该目录：`cd HelloNode`

2. 创建一个名为“app.js”、包含名为“msg”的变量的 JavaScript 文件：`echo var msg > app.js`

3. 在 VS Code 中打开该目录和 app.js文件：`code .`

4. 添加一个简单的字符串变量（“Hello World”），然后通过在“app.js”文件中输入此变量将字符串内容发送到控制台：

    ```js
    var msg = 'Hello World';
    console.log(msg);
    ```

5. 通过 Node.js 运行“app.js”文件： 选择“视图”   >   “终端”（或选择 Ctrl+`，注意使用反引号），在 VS Code 中直接打开终端。 如果需要更改默认终端，请选择下拉菜单，然后选择“选择默认 Shell”  。

6. 在终端中，输入 `node app.js`。 你应会看到输出：“Hello World”。

> [!NOTE]
> 请注意，你在“app.js”文件中键入 `console` 时，VS Code 将显示与 [`console`](https://developer.mozilla.org/docs/Web/API/Console) 对象相关的受支持的选项，以便你使用 IntelliSense 进行选择。 尝试使用其他 [JavaScript 对象](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects)对 Intellisense 进行试验。

> [!TIP]
> 如果打算使用多个命令行（Ubuntu、PowerShell、Windows 命令提示符等）或想要[自定义终端](https://github.com/microsoft/terminal/blob/master/doc/user-docs/UsingJsonSettings.md)（包括文本、背景颜色、键绑定、多窗口窗格等），请尝试使用 [Windows 终端](https://github.com/microsoft/terminal/blob/master/doc/user-docs/index.md)。

## <a name="set-up-a-basic-web-app-framework-by-using-express"></a>使用 Express 设置基本 Web 应用框架

Express 是一个灵活精简的小型 Node.js 框架，通过它可以更轻松地开发可处理多种请求类型（例如 GET、PUT、POST 和 DELETE）的 Web 应用。 Express 附带了一个应用程序生成器，它将自动为应用创建文件体系结构。

若要使用 Express.js 创建项目，请执行以下步骤：

1. 打开命令行（命令提示符、Powershell 或你喜欢的任何工具）。
2. 创建新项目文件夹 `mkdir ExpressProjects` 并输入以下目录：`cd ExpressProjects`
3. 使用 Express 创建一个 HelloWorld 项目模板：`npx express-generator HelloWorld --view=pug`

>[!NOTE]
> 此处我们将使用 `npx` 命令执行 Express.js Node 包，而不安装它（如果你愿意，也可以临时安装它）。 如果你尝试使用 `express` 命令，或使用 `express --version` 检查已安装的 Express 的版本，你将收到说明找不到 Express 的响应。 如果要全局安装 Express 以便反复使用，请使用 `npm install -g express-generator`。 可以使用 `npm list` 查看 npm 已安装的包。 已安装的包会按深度（嵌套目录的层数）列出。 你安装的包的深度将是 0。 此包的依赖项深度为 1，依赖项的依赖项深度为 2，依此类推。 若要了解详细信息，请参阅 Stackoverflow 上的 [npx 和 npm 的区别](https://stackoverflow.com/questions/50605219/difference-between-npx-and-npm)。

4. 使用 `code .`在 VS Code 中打开项目以检查 Express 包含的文件和文件夹

   Express 生成的文件将创建一个 Web 应用，该应用使用的体系结构最初可能会显得有点复杂。 VS Code“资源管理器”  窗口（按 Ctrl+Shift+E 可查看）会显示已创建以下文件和文件夹：

   - `bin`”。 包含启动应用的可执行文件。 它将启动服务器（如果未提供任何替代项，则在端口 3000 上），并设置基本的错误处理。 
   - `public`”。 包含所有公开访问的文件，具体包括 JavaScript 文件、CSS 样式表、字体文件、图像和用户连接到网站时所需的任何其他资产。
   - `routes`”。 包含应用程序的所有路线处理程序。 此文件夹中会自动创建两个文件（`index.js` 和 `users.js`），作为说明如何分离应用程序路线配置的示例。
   - `views`”。 包含模板引擎使用的文件。 Express 被配置为在调用 render 方法时在此文件夹中查找匹配的视图。 默认模板引擎是 Jade，但为支持 Pug，现已弃用 Jade，因此我们使用 `--view` 标记更改视图（模板）引擎。 使用 `express --help` 可查看 `--view` 标记选项及其他内容。
   - `app.js`”。 这是应用的起始点。 它加载所有内容并开始传送用户请求。 它基本充当着胶水的角色，把所有部件组织在一起。
   - `package.json`”。 包含项目说明、脚本管理器和应用清单。 其主要用途是跟踪应用的依赖项及其各自的版本。

5. 为构建和运行 HelloWorld Express 应用，现在需要安装 Expres 使用的依赖项（`package.json` 文件中定义的用于运行服务器等任务的包）。 在 VS Code 中，选择“视图”   >   “终端”（或选择 Ctrl+`，注意使用反引号）以打开终端，并确保你仍然位于“HelloWorld”项目目录中。 安装 Express 包依赖项：

```bash
npm install
```

6. 此时，你已为多页 Web 应用设置了框架，该 Web 应用可访问多种 API 及 HTTP 实用工具方法和中间件，因此你可以更轻松地创建可靠的 API。 输入以下命令，在虚拟服务器上启动 Express 应用：

```bash
npx cross-env DEBUG=HelloWorld:* npm start
```

> [!TIP]
> 上述命令中的 `DEBUG=myapp:*` 部分意味着，你指示 Node.js 出于调试目的而开启日志记录。 请记得将“myapp”替换为你的应用名称。 在 `package.json` 文件的“name”属性下可找到应用的名称。 使用 `npx cross-env` 可以在任何终端中设置 `DEBUG` 环境变量，但也可以使用特定于终端的方式对其进行设置。 `npm start` 命令指示 npm 运行 `package.json` 文件中的脚本。

7. 你现在可以打开 Web 浏览器并转到 localhost:3000 来查看这个正在运行的应用 

   ![屏幕截图显示了在浏览器中运行的 Express 应用](../images/express-app.png)

8. 现在，HelloWorld Express 应用在浏览器中本地运行，请尝试打开项目目录中的“views”文件夹并选择“index.pug”文件来进行更改。 打开后，将 `h1= title` 更改为 `h1= "Hello World!"`，并选择“保存”  (Ctrl+S)。 在 Web 浏览器中刷新 localhost:3000 URL，可查看更改  。

9. 若要停止运行 Express 应用，请在终端中输入：Ctrl+C 

## <a name="try-using-a-nodejs-module"></a>尝试使用 Node.js 模块

Node.js 提供的工具（部分为内置工具，许多其他工具可通过 npm 获得）可帮助你开发服务器端 Web 应用。 以下模块可帮助执行许多任务：

|工具               |用途                                                                                                  |
|:----------------- |:---------------------------------------------------------------------------------------------------------|
|gm 和 sharp          |图像操作，包括直接在 JavaScript 代码中进行编辑、调整大小和压缩等操作 |
|PDFKit             |PDF 生成                                                                                            |
|validator.js       |字符串验证                                                                                         |
|imagemin 和 UglifyJS2|缩小                                                                                              |
|spritesmith        |子画面表生成                                                                                   |
|winston            |日志记录                                                                                                  |
|commander.js       |创建命令行应用程序                                                                       |

让我们使用内置的 OS 模块获取有关计算机操作系统的信息：

1) 在命令行中，打开 Node.js CLI。 输入 `node` 后，你会看到出现 `>` 提示，告知你当前正在使用 Node.js

2) 若要确定你当前使用的操作系统，请输入 `os.platform()`（这应会返回响应，告知你正使用 Windows）

3) 若要检查 CPU 体系结构，请输入 `os.arch()`

4) 若要查看系统中可用的 CPU，请输入 `os.cpus()`

5) 输入 `.exit` 或按两次 Ctrl+C 可离开 Node.js CLI。

   > [!TIP]
   > 可以使用 Node.js OS 模块执行一些操作，例如检查平台并返回特定于平台的变量：适用于 Windows 开发的 Win32/.bat，适用于 Mac/unix、Linux 和 SunOS 等（例如 `var isWin = process.platform === "win32";`）的 darwin/.sh。

## <a name="next-steps"></a>后续步骤

在本指南中，你了解了有关使用 Node.js 可执行的操作的一些基本信息，尝试了在 VS Code 中使用 node.js 命令行，使用 Express.js 创建了一个简单的 Web 应用，并在 Web 浏览器中本地运行了该应用，然后尝试使用了几个内置的 Node.js 模块。 若要详细了解如何安装和使用某些热门 Node.js Web 框架，请继续阅读下一指南，其中介绍了 Next.js（基于 React 的服务器呈现的 Web 框架）、Nuxt.js（基于 Vue 的服务器呈现的 Web 框架）和 Gatsby（基于 React 的静态呈现的 Web 框架）。 也可以跳过这些内容，立即了解如何使用 MongoDB、PostgreSQL 数据库或 Docker 容器。

- [开始在 Windows 上使用 Node.js Web 框架](./web-frameworks.md)
- [开始将 Node.js 应用连接到数据库](https://docs.microsoft.com/windows/wsl/tutorials/wsl-database)
- [开始将 Docker 容器与 Node.js 结合使用](./containers.md)
