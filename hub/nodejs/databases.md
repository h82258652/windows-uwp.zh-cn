---
title: 开始将 Node.js 应用连接到数据库
description: 开始在 Windows 上将 Node.js 应用连接到数据库。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS, Node.js, windows 10, microsoft, 了解 nodejs, windows 上的 node, wsl 上的 node, windows 中 linux 上的 node, 在 windows 上安装 node, 具有 vs code 的 nodejs, 通过 windows 上的 node 进行开发, 通过 windows 上的 nodejs 进行开发, 在 WSL 上安装 node, 适用于 Linux 的 Windows 子系统上的 NodeJS
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: 63c47107538d8744201f83ea1be24cfaf3193f4f
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "72517817"
---
# <a name="get-started-using-mongodb-or-postgresql-with-nodejs-on-windows"></a>在 Windows 上开始使用具有 Node.js 的 MongoDB 或 PostgreSQL

Node.js 应用程序通常需要保存数据，可能会通过文件、本地存储、云服务或数据库来保存。 本分步指南将帮助你开始将 Node.js 应用连接到数据库。 我们选择重点介绍两个热门的选择：MongoDB 和 PostgreSQL。

## <a name="prerequisites"></a>必备条件

本指南假定你已经完成了[使用 WSL 2 设置 Node.js 开发环境](./setup-on-wsl2.md)的步骤，具体包括：

- 安装 Windows 10 Insider Preview 内部版本 18932 或更高版本。
- 在 Windows 上启用 WSL 2 功能。
- 安装 Linux 分发版（本示例为 Ubuntu 18.04）。 可输入以下内容进行检查：`wsl lsb_release -a`
- 确保 Ubuntu 18.04 分发版在 WSL 2 模式下运行。 （WSL 在 v1 或 v2 模式下均可运行分发版。）可通过打开 PowerShell 并输入以下内容进行检查：`wsl -l -v`
- 使用 PowerShell，通过输入以下内容将 Ubuntu 18.04 设置为默认分发版：`wsl -s ubuntu 18.04`

## <a name="differences-between-mongodb-and-postgresql"></a>MongoDB 和 PostgreSQL 之间的区别

[!INCLUDE [Postgres vs Mongo](../includes/postgres-v-mongo.md)]

## <a name="install-mongodb"></a>安装 MongoDB

[!INCLUDE [Install and run Mongo](../includes/install-and-run-mongo.md)]

### <a name="vs-code-support-for-mongodb"></a>对 MongoDB 的 VS Code 支持

VS Code 支持通过 [Azure CosmosDB 扩展](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)来处理 MongoDB 数据库，你可以在 VS Code 中创建、管理和查询 MongoDB 数据库。

有关详细信息，请访问 VS Code 文档：[使用 MongoDB](https://code.visualstudio.com/docs/azure/mongodb)。

有关详细信息，请参阅 MongoDB 文档：

- [使用 MongoDB 简介](https://docs.mongodb.com/manual/introduction/)
- [创建用户](https://docs.mongodb.com/manual/tutorial/create-users/)
- [连接到远程主机上的 MongoDB 实例](https://docs.mongodb.com/manual/mongo/#mongodb-instance-on-a-remote-host)
- [CRUD：创建、读取、更新、删除](https://docs.mongodb.com/manual/crud/)
- [参考文档](https://docs.mongodb.com/manual/reference/)

## <a name="install-postgresql"></a>安装 PostgreSQL

[!INCLUDE [Install and run PostgresQL](../includes/install-and-run-postgres.md)]

### <a name="vs-code-support-for-postgresql"></a>对 PostgreSQL 的 VS Code 支持

VS Code 支持通过 [PostgreSQL 扩展](https://marketplace.visualstudio.com/items?itemName=ms-ossdata.vscode-postgresql)来处理 PostgreSQL 数据库，你可以在 VS Code 中创建、管理、查询和连接到 PostgreSQL 数据库。

## <a name="set-up-profile-aliases"></a>设置配置文件别名

[!INCLUDE [Set up profile aliases](../includes/profile-aliases.md)]
