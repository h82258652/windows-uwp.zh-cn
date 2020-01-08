---
title: 开始将 Docker 容器与 node.js 配合使用
description: 帮助你开始使用适用于 node.js 应用的 Docker 容器的循序渐进指南。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: ''
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: 9467224814b1e26f18031662f5e8d994a8fae1ac
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/06/2020
ms.locfileid: "75683670"
---
# <a name="get-started-using-docker-containers-with-nodejs"></a>开始将 Docker 容器与 node.js 配合使用

帮助你开始使用适用于 node.js 应用的 Docker 容器的循序渐进指南。

## <a name="prerequisites"></a>先决条件

本指南假定你已完成[用 WSL 2 设置 node.js 开发环境](./setup-on-wsl2.md)的步骤，包括：

- 安装 Windows 10 Insider preview 内部版本18932或更高版本。
- 在 Windows 上启用 WSL 2 功能。
- 安装 Linux 分发版（Ubuntu 18.04 示例）。 可以通过以下方式查看此内容： `wsl lsb_release -a`。
- 确保 Ubuntu 18.04 分发在 WSL 2 模式下运行。 （WSL 可以在 v1 或 v2 模式下运行分发。）可以通过打开 PowerShell 并输入以下内容来进行检查： `wsl -l -v`。
- 使用 PowerShell 将 Ubuntu 18.04 设置为默认分布，使用： `wsl -s ubuntu 18.04`。

## <a name="overview-of-docker-containers"></a>Docker 容器概述

**Docker**是一种用于使用容器创建、部署和运行应用程序的工具。 容器使开发人员可以使用所需的所有部件（库、框架、依赖项等）打包应用程序，并将其作为一个包交付。 使用容器可确保无论在运行该应用程序的计算机上的任何自定义设置或以前安装的库可能与用于编写和测试应用代码的计算机有所不同，应用都将运行相同的设置。 这使开发人员可以专注于编写代码，而无需担心将在哪个系统上运行代码。

Docker 容器类似于虚拟机，但不创建整个虚拟操作系统。 取而代之的是，Docker 允许应用使用与运行它的系统相同的 Linux 内核。 这允许应用包仅要求主机不在主计算机上，从而降低包大小和提高性能。

使用[Kubernetes](https://docs.microsoft.com/azure/aks/)之类的工具将 Docker 容器用于持久可用性，这是容器的普及的另一个原因。 这样就可以在不同的时间创建应用容器的多个版本。 可以动态替换每个容器（及其特定的微服务），而无需将整个系统用于更新或维护。 您可以准备新的容器，其中包含所有更新，设置用于生产的容器，并在新容器就绪后直接指向该容器。 你还可以使用容器来存档不同版本的应用程序，并在需要时将其作为安全回退来运行。

## <a name="install-docker-desktop-wsl-2-tech-preview"></a>安装 Docker Desktop WSL 2 技术预览

以前，WSL 1 无法直接运行 Docker 后台程序，但已通过 WSL 2 进行了更改，并可通过适用于 WSL 2 的 Docker 桌面显著提高速度和性能。

若要安装并运行 Docker Desktop WSL 2 技术预览：

1. 下载[Docker DESKTOP WSL 2 技术预览版安装程序](https://download.docker.com/win/edge/36883/Docker%20Desktop%20Installer.exe)。 （如果需要，你可以参考[安装程序文档](https://docs.docker.com/docker-for-windows/wsl-tech-preview/)）。

2. 打开刚刚下载的 Docker 安装程序。 安装向导将询问你是否要 "使用 Windows 容器而不是 Linux 容器"-将其取消选择，因为我们将使用 Linux 子系统。 Docker 将安装在默认 WSL 2 分发的托管目录中，并将包括 Docker 守护程序、CLI 和编写 CLI。

    ![Docker Desktop 启动](../images/install-docker-1.png)

3. 如果还没有 Docker ID，则需要通过访问以下项来设置一个： [https://hub.docker.com/signup](https://hub.docker.com/signup)。 ID 必须全部为小写字母数字字符。

4. 安装完成后，通过选择桌面上的快捷方式图标或在 Windows "开始" 菜单中找到它来启动 Docker Desktop。 Docker 图标将显示在任务栏的隐藏图标菜单中。 右键单击该图标可显示 Docker 命令菜单，并选择 "WSL 2 技术预览"。

5. "技术预览" 窗口打开后，选择 "**开始**" 以开始在 WSL 2 中运行 Docker 守护程序（后台进程）。 当 WSL 2 docker 后台程序启动时，将自动为其创建 docker CLI 上下文。

    ![Docker Desktop 启动](../images/start-docker.gif)

6. 若要确认是否已安装 Docker 并显示版本号，请打开命令行（WSL 或 PowerShell）并输入： `docker --version`

7. 运行简单的内置 Docker 映像，测试安装是否正常工作： `docker run hello-world`

下面是一些你应该知道的 Docker 命令：

- 在 Docker CLI 中输入以下命令以列出可用命令： `docker`
- 具有以下内容的特定命令的列表信息： `docker <COMMAND> --help`
- 列出计算机上的 docker 映像（此时为 "hello world" 映像），其中包含： `docker image ls --all`
- 列出计算机上的容器，其中包含： `docker container ls --all`
- 使用以下方式列出你在 WSL 2 上下文中可用的 Docker 系统统计信息和资源（CPU & 内存）： `docker info`
- 显示 docker 当前正在运行的位置，其中包括： `docker context ls`

你可以看到，Docker 正在 `default` （经典 Docker 后台程序）和 `wsl` 中运行的两个上下文（我们的建议使用技术预览版）。 （此外，`ls` 命令是 `list` 的简称，可互换使用。

![Powershell 中的 Docker 显示上下文](../images/docker-context.png)

> [!TIP]
> 尝试使用此[教程在 Docker 中心](https://hub.docker.com/?overlay=onboarding)构建示例 Docker 映像。 Docker 中心还包含多达数千个开源映像，这些映像可能与要容器化的应用的类型相匹配。 你可以下载图像（如此[Gatsby 框架容器](https://hub.docker.com/r/gatsbyjs/gatsby-dev-builds)或此[Nuxt 框架容器](https://hub.docker.com/r/hobord/nuxtexpress)），并将其扩展为你自己的应用程序代码。 你可以[从命令行](https://docs.docker.com/engine/reference/commandline/search/)或[docker 中心网站](https://hub.docker.com/search/?type=image)使用 Docker 搜索注册表。

## <a name="install-the-docker-extension-on-vs-code"></a>在 VS Code 上安装 Docker 扩展

Docker 扩展使你可以从 Visual Studio Code 轻松生成、管理和部署容器化应用程序。

1. 在 VS Code 中打开 "**扩展**" 窗口（Ctrl + Shift + X），然后搜索**Docker**。

2. 选择[Microsoft Docker 扩展](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker)并**安装**。 你将需要在安装后重新加载 VS Code 以启用该扩展。

    ![远程 WSL 中 VS Code 的 Docker 扩展](../images/docker-vscode-extension.png)

通过在 VS Code 上安装 Docker 扩展，你现在可以在下一节中使用快捷方式打开 `Dockerfile` 命令的列表： `Ctrl+Space`

详细了解如何[在 VS Code 中使用 Docker](https://code.visualstudio.com/docs/azure/docker)。

## <a name="create-a-container-image-with-dockerfile"></a>使用 DockerFile 创建容器映像

**容器映像**存储应用程序代码、库、配置文件、环境变量和运行时。 使用映像可确保容器中的环境是标准化的，并且仅包含生成和运行应用程序所需的内容。

**DockerFile**包含生成新容器映像所需的说明。 换句话说，此文件将生成一个容器映像，用于定义应用的环境，使其可在任何位置重现。

让我们使用[web 框架](./web-frameworks.md)指南中设置的下一个 .js 应用来构建容器映像。

1. 在 VS Code 中打开下一 .js 应用（确保远程 WSL 扩展按左下绿色选项卡中所示的方式运行）。 打开 WSL 中的 "集成 VS Code （**查看 > 终端**）"，并确保终端路径指向下一个 .js 项目目录（即 `~/NextProjects/my-next-app$`）。

2. 在下一个 .js 项目的根目录中创建一个名为 `Dockerfile` 的新文件，并添加以下内容：

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

3. 若要生成 docker 映像，请从项目的根目录中运行以下命令（但将 `<your_docker_username>` 替换为在 Docker 中心创建的用户名）： `docker build -t <your_docker_username>/my-nextjs-app .`

> [!NOTE]
> Docker 必须运行 WSL 技术预览版才能使此命令正常工作。 若要提醒如何启动 Docker，请参阅安装部分的[步骤 #4](#install-docker-desktop-wsl-2-tech-preview) 。 `-t` 标志指定要创建的映像的名称，在本例中为 "nextjs： v1"。 建议在创建映像时，始终[对标记名称使用版本 #](https://medium.com/@mccode/the-misunderstood-docker-tag-latest-af3babfd6375) 。 请确保在命令末尾包含句点，该时间段指定应使用当前工作目录来查找和复制下一个 .js 应用的生成文件。

4. 若要在容器中运行下一个 .js 应用的新 docker 映像，请输入以下命令： `docker run -d -p 3333:3000 <your_docker_username>/my-nextjs-app:v1`

5. `-p` 标志将端口 "3000" （应用程序在容器内运行的端口）绑定到计算机上的本地端口 "3333"，因此你现在可以将 web 浏览器指向[http://localhost:3333](http://localhost:3333)并查看以 Docker 容器映像形式运行的下一 node.js 应用程序。

> [!TIP]
> 我们使用 `FROM node:12` 来构建容器映像，该映像引用存储在 Docker 中心上的 node.js 版本12默认映像。 此默认 node.js 映像基于 Debian/Ubuntu Linux 系统，但有许多不同的 node.js 映像可供选择，但是，你可能需要考虑使用更轻量的内容，或者根据你的需要进行定制。 有关详细信息，请参阅[Docker Hub 上的 Node.js 映像注册表](https://hub.docker.com/_/node/)。

## <a name="upload-your-container-image-to-a-repository"></a>将容器映像上传到存储库

**容器存储库**在云中存储容器映像。 容器存储库通常会包含相关映像的集合（如不同版本），它们都可以轻松地进行设置和快速部署。 通常，可以通过安全 HTTPs 终结点访问容器存储库上的图像，使你可以通过任何系统、硬件或 VM 实例请求、推送或管理映像。

另一方面，**容器注册表**存储存储库的集合，以及索引、访问控制规则和 API 路径。 这些可以是公开或私下托管的。 [Docker 中心](https://hub.docker.com/)是一个开源 Docker 注册表，在运行 `docker push` 和 `docker pull` 命令时使用的默认值。 它可免费用于公共存储库，并需要支付专用存储库的费用。

若要将新的容器映像上传到 Docker 中心托管的存储库，请执行以下操作：

1. 登录到 Docker 中心。 系统将提示你输入在安装步骤中创建 Docker 中心帐户时所用的用户名和密码。 若要登录到终端中的 Docker，请输入： `docker login`

2. 若要获取已在计算机上创建的 docker 容器映像的列表，请输入： `docker image ls --all`

3. 将容器映像推送到 Docker Hub，并使用以下命令在其中创建新存储库： `docker push <your_docker_username>/my-nextjs-app:v1`

4. 你现在可以在 Docker Hub 上查看存储库，输入描述，并链接 GitHub 帐户（如果需要），方法是访问： https://cloud.docker.com/repository/list

5. 你还可以使用以下方式查看活动 Docker 容器的列表： `docker container ls` （或 `docker ps`）

6. 应会看到 "nextjs" 容器在端口 3333-> 3000/tcp 上处于活动状态。 你还可以在此处看到列出的 "容器 ID"。 若要停止运行容器，请输入以下命令： `docker stop <container ID>`

7. 通常情况下，容器停止后，也应被删除。 删除容器将清理它留下的任何资源。 删除容器后，在其图像文件系统中所做的任何更改都将永久丢失。 你将需要构建一个新图像来表示更改。 若要删除容器，请使用命令： `docker rm <container ID>`

详细了解如何[使用 Docker 构建容器化 web 应用程序](https://docs.microsoft.com/learn/modules/intro-to-containers/)。

## <a name="deploy-to-azure-container-registry"></a>部署到 Azure 容器注册表

[**Azure 容器注册表**](https://azure.microsoft.com/services/container-registry/)（ACR）使你可以在专用、经过身份验证的存储库中存储、管理和保护容器映像。 ACR 可与标准 Docker 命令兼容，可处理容器运行状况监视和维护等关键任务，并与[Kubernetes](https://docs.microsoft.com/azure/aks/intro-kubernetes)进行配对，以创建可缩放的业务流程系统。 可以通过源代码提交和基础映像更新等触发器按需生成或完全自动生成。 ACR 还利用了强大的 Azure 云网络来管理网络延迟、全球部署，并为使用[Azure App Service](https://docs.microsoft.com/azure/app-service/) （适用于 web 托管、移动后端、REST api）或[其他 Azure 云服务](https://azure.microsoft.com/product-categories/containers/)的任何人创建无缝的本机体验。

> [!IMPORTANT]
> 若要将容器部署到 Azure，你需要自己的 Azure 订阅，你可能会收到费用。 如果还没有 Azure 订阅，请在开始前[创建一个免费帐户](https://azure.microsoft.com/free/)。

有关创建 Azure 容器注册表和部署应用容器映像的帮助，请参阅练习：将[Docker 映像部署到 Azure 容器实例](https://docs.microsoft.com/learn/modules/intro-to-containers/7-exercise-deploy-docker-image-to-container-instance)。

## <a name="additional-resources"></a>其他资源

- [Azure 上的 Node.js](https://azure.microsoft.com/develop/nodejs/)
- 快速入门：[在 Azure 中创建 node.js web 应用](https://docs.microsoft.com/azure/app-service/app-service-web-get-started-nodejs)
- 联机课程：[在 Azure 中管理容器](https://docs.microsoft.com/learn/paths/administer-containers-in-azure/)
- 使用 VS Code：使用[Docker](https://code.visualstudio.com/docs/azure/docker)
- Docker 文档： [Docker DESKTOP WSL 2 技术预览](https://docs.docker.com/docker-for-windows/wsl-tech-preview/)
