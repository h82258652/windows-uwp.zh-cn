---
title: 开始将 Docker 容器与 Node.js 结合使用
description: 本分步指南可帮助你开始将 Docker 容器与 Node.js 应用结合使用。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: ''
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: 9467224814b1e26f18031662f5e8d994a8fae1ac
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "75683670"
---
# <a name="get-started-using-docker-containers-with-nodejs"></a>开始将 Docker 容器与 Node.js 结合使用

本分步指南可帮助你开始将 Docker 容器与 Node.js 应用结合使用。

## <a name="prerequisites"></a>必备条件

本指南假定你已经完成了[使用 WSL 2 设置 Node.js 开发环境](./setup-on-wsl2.md)的步骤，具体包括：

- 安装 Windows 10 Insider Preview 内部版本 18932 或更高版本。
- 在 Windows 上启用 WSL 2 功能。
- 安装 Linux 分发版（本示例为 Ubuntu 18.04）。 可输入以下内容进行检查：`wsl lsb_release -a`。
- 确保 Ubuntu 18.04 分发版在 WSL 2 模式下运行。 （WSL 在 v1 或 v2 模式下均可运行分发版。）可通过打开 PowerShell 并输入以下内容进行检查：`wsl -l -v`。
- 使用 PowerShell，通过输入以下内容将 Ubuntu 18.04 设置为默认分发版：`wsl -s ubuntu 18.04`。

## <a name="overview-of-docker-containers"></a>Docker 容器概述

Docker 是一种工具，用于创建、部署和运行应用程序（通过使用容器）  。 容器使开发人员可以将应用与需要的所有部件（库、框架、依赖项等）打包为一个包一起交付。 使用容器可确保此应用的运行与之前相同，而不受任何自定义设置或运行该应用的计算机上先前安装的库的影响（运行应用的计算机可能与用于编写和测试应用代码的计算机不同）。 这使开发人员可以专注于编写代码，而无需操心将运行代码的系统。

Docker 容器与虚拟机类似，但不会创建整个虚拟操作系统。 相反，Docker 允许应用使用与运行它的系统相同的 Linux 内核。 这使得应用包能够仅要求主计算机上尚未安装的部件，从而降低包大小以及提高性能。

将 Docker 容器与 [Kubernetes](https://docs.microsoft.com/azure/aks/) 等工具结合使用以实现持续可用性是容器普及的另一个原因。 这样就可以在不同的时间创建应用容器的多个版本。 并且每个容器（及其特定的微服务）均可以动态更换，而无需停止整个系统进行更新或维护。 你可以准备一个包含所有更新的新容器，将该容器设置用于生产，并在新容器准备就绪后直接指向该容器。 你还可以使用容器对不同版本的应用进行存档，如有需要，还可将其作为安全回退保持运行。

## <a name="install-docker-desktop-wsl-2-tech-preview"></a>安装 Docker Desktop WSL 2 技术预览版

以前，WSL 1 无法直接运行 Docker 守护程序，但 WSL 2 使这种情况发生了变化，并且通过适用于 WSL 2 的 Docker Desktop，速度和性能得以显著提升。

若要安装并运行 Docker Desktop WSL 2 技术预览版，请执行以下步骤：

1. 下载 [Docker Desktop WSL 2 技术预览版安装程序](https://download.docker.com/win/edge/36883/Docker%20Desktop%20Installer.exe)。 （如果需要，可以参考[安装程序文档](https://docs.docker.com/docker-for-windows/wsl-tech-preview/)）。

2. 打开刚刚下载的 Docker 安装程序。 安装向导将询问你是否要“使用 Windows 容器而不是 Linux 容器”，请保持取消选中，因为我们将使用 Linux 子系统。 Docker 将安装在默认 WSL 2 分发版的托管目录中，并将包括 Docker 守护程序、CLI 和 Compose CLI。

    ![Docker Desktop 启动](../images/install-docker-1.png)

3. 如果还没有 Docker ID，则需要通过访问以下网站创建一个：[https://hub.docker.com/signup](https://hub.docker.com/signup)。 ID 必须全部为小写字母数字字符。

4. 安装完成后，通过选择桌面上的快捷方式图标，或在 Windows“开始”菜单中找到 Docker Desktop 来启动。 Docker 图标将显示在任务栏的隐藏图标菜单中。 右键单击该图标以显示 Docker 命令菜单，然后选择“WSL 2 技术预览版”。

5. “技术预览版”窗口打开后，选择“开始”以开始在 WSL 2 中运行 Docker 守护程序（后台进程）  。 WSL 2 Docker 守护程序启动后，将自动为其创建 docker CLI 上下文。

    ![Docker Desktop 启动](../images/start-docker.gif)

6. 若要确认 Docker 是否已安装以及显示版本号，请打开命令行（WSL 或 PowerShell）并输入：`docker --version`

7. 通过运行简单的内置 Docker 映像，测试安装是否正常工作：`docker run hello-world`

下面是一些你应该知道的 Docker 命令：

- 通过输入以下命令列出 Docker CLI 中可用的命令：`docker`
- 使用以下命令列出特定命令的信息：`docker <COMMAND> --help`
- 使用以下命令列出计算机上的 docker 映像（此时仅为 hello-world 映像）：`docker image ls --all`
- 使用以下命令列出计算机上的容器：`docker container ls --all`
- 使用以下命令列出你在 WSL 2 上下文中可使用的 Docker 系统统计信息和资源（CPU 和内存）：`docker info`
- 使用以下命令显示 docker 当前运行的位置：`docker context ls`

你可以看到运行 Docker 的上下文有两个 - `default`（经典 Docker 守护程序）和 `wsl`（技术预览版，我们建议使用此选项）。 （此外，`ls` 命令是 `list` 的简称，可互换使用）。

![Powershell 中的 Docker 显示上下文](../images/docker-context.png)

> [!TIP]
> 尝试按照 [Docker Hub 上的教程](https://hub.docker.com/?overlay=onboarding)生成示例 Docker 映像。 Docker Hub 还包含数千个开放源代码映像，这些映像可能与你要容器化的应用类型匹配。 你可以下载映像，例如此 [Gatsby.js 框架容器](https://hub.docker.com/r/gatsbyjs/gatsby-dev-builds)或此 [Nuxt.js 框架容器](https://hub.docker.com/r/hobord/nuxtexpress)，并使用你自己的应用程序代码对其进行扩展。 你可以使用[命令行中的 Docker](https://docs.docker.com/engine/reference/commandline/search/) 或 [Docker Hub 网站](https://hub.docker.com/search/?type=image)搜索注册表。

## <a name="install-the-docker-extension-on-vs-code"></a>在 VS Code 上安装 Docker 扩展

Docker 扩展使从 Visual Studio Code 生成、管理和部署容器化应用程序变得容易。

1. 在 VS Code 中打开“扩展”窗口 (Ctrl+Shift+X)，然后搜索 Docker   。

2. 选择 [Microsoft Docker 扩展](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker)，然后选择“安装”  。 你需要在安装后重载 VS Code 才能启用扩展。

    ![Remote-WSL 中 VS Code 上的 Docker 扩展](../images/docker-vscode-extension.png)

在 VS Code 上安装 Docker 扩展后，你现在可以通过以下快捷方式调出将在下一部分使用的 `Dockerfile` 命令列表：`Ctrl+Space`

详细了解如何[在 VS Code 中使用 Docker](https://code.visualstudio.com/docs/azure/docker)。

## <a name="create-a-container-image-with-dockerfile"></a>使用 DockerFile 创建容器映像

容器映像可存储你的应用程序代码、库、配置文件、环境变量和运行时  。 使用映像可确保容器中的环境是标准化的，且仅包含生成和运行应用程序所需的内容。

DockerFile 包含生成新容器映像所需的指令  。 换句话说，此文件将生成一个可定义应用环境的容器映像，这样它就可在任何位置重现。

让我们使用 [Web 框架](./web-frameworks.md)指南中设置的 Next.js 应用来生成容器映像。

1. 在 VS Code 中打开 Next.js 应用（确保 Remote-WSL 扩展按左下绿色选项卡中所示的方式运行）。 打开集成在 VS Code 中的 WSL 终端（“视图”>“终端”），并确保终端路径指向 Next.js 项目目录（即 `~/NextProjects/my-next-app$`）  。

2. 在 Next.js 项目的根目录中创建名为 `Dockerfile` 的新文件，并添加以下内容：

    ```docker
    # Specifies where to get the base image (Node v12 in our case) and creates a new container for it
    FROM node:12
    
    # Set working directory. Paths will be relative this WORKDIR.
    WORKDIR /usr/src/app
    
    # Install dependencies
    COPY package*.json ./
    RUN npm install
    
    # Copy source files from host computer to the container
    COPY . .
    
    # Build the app
    RUN npm run build
    
    # Specify port app runs on
    EXPOSE 3000

    # Run the app
    CMD [ "npm", "start" ]
    ```

3. 若要生成 docker 映像，请从项目的根目录运行以下命令（但将 `<your_docker_username>` 替换为在 Docker Hub 上创建的用户名）：`docker build -t <your_docker_username>/my-nextjs-app .`

> [!NOTE]
> Docker 必须与 WSL 技术预览版一起运行才能使此命令正常工作。 有关如何启动 Docker 的提醒，请参阅安装部分的[步骤 4](#install-docker-desktop-wsl-2-tech-preview)。 `-t` 标志指定要创建的映像的名称，在本例中为“my-nextjs-app:v1”。 建议在创建映像时始终[在标志名称上使用版本号](https://medium.com/@mccode/the-misunderstood-docker-tag-latest-af3babfd6375)。 请确保在命令末尾包含句点，以指定应用于查找和复制 Next.js 应用的生成文件的当前工作目录。

4. 若要在容器中运行 Next.js 应用的新 docker 映像，请输入以下命令：`docker run -d -p 3333:3000 <your_docker_username>/my-nextjs-app:v1`

5. `-p` 标志将端口“3000”（应用在容器内运行的端口）绑定到计算机上的本地端口“3333”，因此你现在可以将 Web 浏览器指向 [http://localhost:3333](http://localhost:3333)，并且可看到由服务器端呈现的 Next.js 应用程序作为 Docker 容器映像运行。

> [!TIP]
> 我们使用 `FROM node:12` 生成容器映像，它引用了存储在 Docker Hub 上的 Node.js 版本 12 默认映像。 此默认 Node.js 映像基于 Debian/Ubuntu Linux 系统，有多种不同的 Node.js 映像可供选择，但是，你可以考虑使用更轻量的或更适合你需求的映像。 有关详细信息，请参阅 [Docker Hub 上的 Node.js 映像注册表](https://hub.docker.com/_/node/)。

## <a name="upload-your-container-image-to-a-repository"></a>将容器映像上传到存储库

容器存储库将容器映像存储在云中  。 通常，容器存储库实际上将包含相关映像的集合（如不同版本），它们都可以用于轻松设置和快速部署。 通常情况下，可通过安全 HTTP 终结点访问容器存储库上的映像，这使得你可以通过任何系统、硬件或 VM 实例拉取、推送或管理映像。

另一方面，容器注册表可存储存储库的集合以及索引、访问控制规则和 API 路径  。 以上内容可以公开托管或私密托管。 [Docker Hub](https://hub.docker.com/) 是一个开放源代码 Docker 注册表，在运行 `docker push` 和 `docker pull` 命令时默认使用。 它可免费用于公共存储库，而对于专用存储库，则需要支付费用。

若要将新的容器映像上传到 Docker Hub 上托管的存储库，请执行以下操作：

1. 登录到 Docker Hub。 在安装步骤中，系统将提示你输入用于创建 Docker Hub 帐户的用户名和密码。 若要登录到终端中的 Docker，请输入：`docker login`

2. 若要获取已在计算机上创建的 docker 容器映像的列表，请输入：`docker image ls --all`

3. 使用以下命令将容器映像推送到 Docker Hub，在其中为容器映像创建新的存储库：`docker push <your_docker_username>/my-nextjs-app:v1`

4. 你现在可以在 Docker Hub 上查看存储库，输入描述以及链接 GitHub 帐户（如果需要），方法是访问： https://cloud.docker.com/repository/list

5. 你还可以使用以下命令查看活动 Docker 容器的列表：`docker container ls`（或 `docker ps`）

6. 你应会看到“my-nextjs-app:v1”容器在端口 3333 ->3000/tcp 上处于活动状态。 还可以看到在此处列出的“容器 ID”。 若要停止运行容器，请输入以下命令：`docker stop <container ID>`

7. 通常情况下，当容器停止后，还应将其删除。 删除容器会清除其留下的所有资源。 删除容器后，你在容器的映像文件系统中所做的任何更改都将永久丢失。 你将需要生成一个新映像来表示更改。 若要删除容器，请使用以下命令：`docker rm <container ID>`

详细了解如何[使用 Docker 生成容器化 Web 应用程序](https://docs.microsoft.com/learn/modules/intro-to-containers/)。

## <a name="deploy-to-azure-container-registry"></a>部署到 Azure 容器注册表

[Azure 容器注册表](https://azure.microsoft.com/services/container-registry/) (ACR) 使你可以在经过身份验证的专用存储库中存储、管理容器映像以及保障容器映像的安全  。 与标准 Docker 命令兼容，ACR 可以为你处理关键任务，例如容器运行状况监视和维护，以及与 [Kubernetes](https://docs.microsoft.com/azure/aks/intro-kubernetes) 配对以创建可缩放的编排系统。 按需生成，或通过触发器（如源代码提交和基础映像更新）完全自动执行生成。 ACR 还利用庞大的 Azure 云网络来管理网络延迟和全球部署，并为任何使用 [Azure 应用服务](https://docs.microsoft.com/azure/app-service/)（用于 Web 托管、移动后端和 REST API）或[其他 Azure 云服务](https://azure.microsoft.com/product-categories/containers/)的用户创建无缝的本机体验。

> [!IMPORTANT]
> 你需要自己的 Azure 订阅才能将容器部署到 Azure，且可能会向你收取费用。 如果你还没有 Azure 订阅，请在开始之前[创建一个免费帐户](https://azure.microsoft.com/free/)。

有关创建 Azure 容器注册表和部署应用容器映像的帮助，请参阅练习：[将 Docker 映像部署到 Azure 容器实例](https://docs.microsoft.com/learn/modules/intro-to-containers/7-exercise-deploy-docker-image-to-container-instance)。

## <a name="additional-resources"></a>其他资源

- [Azure 上的 Node.js](https://azure.microsoft.com/develop/nodejs/)
- 快速入门：[在 Azure 中创建 Node.js Web 应用](https://docs.microsoft.com/azure/app-service/app-service-web-get-started-nodejs)
- 在线课程：[在 Azure 中管理容器](https://docs.microsoft.com/learn/paths/administer-containers-in-azure/)
- 使用 VS Code：[使用 Docker](https://code.visualstudio.com/docs/azure/docker)
- Docker 文档：[Docker Desktop WSL 2 技术预览版](https://docs.docker.com/docker-for-windows/wsl-tech-preview/)
