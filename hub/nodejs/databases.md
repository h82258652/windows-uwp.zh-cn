---
title: 开始将 node.js 应用连接到数据库
description: 开始将 node.js 应用连接到 Windows 上的数据库。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS，node.js，windows 10，microsoft，学习 NodeJS，windows 上的节点，wsl 上的节点，windows 上的节点，在 windows 上安装节点，NodeJS with vs code，在 windows 上安装节点，在 windows 上进行开发，在 NODEJS 上安装节点，在 Windows 上安装节点适用于 Linux 的子系统
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: bdc3e3c944c4aeb25f5cf880fc4d31df1019da5a
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/15/2019
ms.locfileid: "72315111"
---
# <a name="get-started-connecting-nodejs-apps-to-a-database"></a>开始将 node.js 应用连接到数据库

Node.js 应用程序通常需要保存数据，这些数据可能会通过文件、本地存储、云服务或数据库发生。 此循序渐进指南将帮助你开始将 node.js 应用连接到数据库。 我们选择将重点放在两个常见选项上：MongoDB 和 PostgreSQL。

## <a name="prerequisites"></a>先决条件

本指南假定你已完成[用 WSL 2 设置 node.js 开发环境](./setup-on-wsl2.md)的步骤，包括：

- 安装 Windows 10 Insider preview 内部版本18932或更高版本。
- 在 Windows 上启用 WSL 2 功能。
- 安装 Linux 分发版（Ubuntu 18.04 示例）。 可以通过以下方式检查此项： `wsl lsb_release -a`
- 确保 Ubuntu 18.04 分发在 WSL 2 模式下运行。 （WSL 可以在 v1 或 v2 模式下运行分发。）可以通过打开 PowerShell 并输入以下内容来进行检查： `wsl -l -v`
- 使用 PowerShell 将 Ubuntu 18.04 设置为默认分布，使用： `wsl -s ubuntu 18.04`

## <a name="differences-between-mongodb-and-postgresql"></a>MongoDB 和 PostgreSQL 之间的差异

[!INCLUDE [Postgres vs Mongo](../includes/postgres-v-mongo.md)]

## <a name="install-mongodb"></a>安装 MongoDB

[!INCLUDE [Install and run Mongo](../includes/install-and-run-mongo.md)]

### <a name="vs-code-support-for-mongodb"></a>MongoDB VS Code 支持

VS Code 支持通过[Azure CosmosDB 扩展](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)使用 mongodb 数据库，则可以从 VS Code 中创建、管理和查询 mongodb 数据库。

若要了解详细信息，请访问 VS Code 文档：[使用 MongoDB](https://code.visualstudio.com/docs/azure/mongodb)。

有关详细信息，请参阅 MongoDB 文档：

- [使用 MongoDB 简介](https://docs.mongodb.com/manual/introduction/)
- [创建用户](https://docs.mongodb.com/manual/tutorial/create-users/)
- [连接到远程主机上的 MongoDB 实例](https://docs.mongodb.com/manual/mongo/#mongodb-instance-on-a-remote-host)
- [CRUD：Create、Read、Update、Delete @ no__t-0
- [参考文档](https://docs.mongodb.com/manual/reference/)

## <a name="install-postgresql"></a>安装 PostgreSQL

[!INCLUDE [Install and run PostgresQL](../includes/install-and-run-postgres.md)]

### <a name="vs-code-support-for-postgresql"></a>VS Code 对 PostgreSQL 的支持

VS Code 支持通过[PostgreSQL 扩展](https://marketplace.visualstudio.com/items?itemName=ms-ossdata.vscode-postgresql)使用 PostgreSQL 数据库，则可以从 VS Code 中创建、连接、管理和查询 PostgreSQL 数据库。

## <a name="set-up-profile-aliases"></a>设置配置文件别名

[!INCLUDE [Set up profile aliases](../includes/profile-aliases.md)]
