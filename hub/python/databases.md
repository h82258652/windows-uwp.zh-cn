---
title: 在 Windows 上通过数据库开始使用 Python
description: 本指南可帮助你在 Windows 上开始将 PostgreSQL 或 MongoDB 与 Python 结合使用。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: python, windows 10, postgresql, mongodb, postgres, mongo, microsoft, python on windows, install postgresql on windows, install mongodb on windows, use postgresql with python, use mongodb with python, postgresql on WSL, mongodb on WSL
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: 9b1bdea86739f3d58b39cf7f0e6b8090474886f3
ms.sourcegitcommit: 60d2d15dd0d365f82e4e90e4bc34b40cf5b4a247
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2019
ms.locfileid: "72517777"
---
# <a name="get-started-using-postgresql-or-mongodb-with-python-on-windows"></a>在 Windows 上开始将 PostgreSQL 或 MongoDB 与 Python 结合使用

本分步指南将帮助你开始将 Python 应用连接到数据库。 我们选择重点介绍两个热门的选择：PostgreSQL 和 MongoDB。

## <a name="differences-between-mongodb-and-postgresql"></a>MongoDB 和 PostgreSQL 之间的区别

[!INCLUDE [Postgres vs Mongo](../includes/postgres-v-mongo.md)]

> [!NOTE]
> 你可能还需要考虑所使用的框架和工具与特定数据库系统的集成情况。 [Django Web 框架](./web-frameworks.md#hello-world-tutorial-for-django)似乎与 PostgreSQL 集成得更好（请参阅 [Django 文档](https://docs.djangoproject.com/en/2.2/ref/contrib/postgres/)和 [psycopg2](https://github.com/psycopg/psycopg2)）。 [Flask Web 框架](./web-frameworks.md#hello-world-tutorial-for-flask)似乎与 MongoDB 集成得更好（请参阅 [MongoEngine](https://github.com/MongoEngine/flask-mongoengine) 和 [PyMongo](https://github.com/dcrosta/flask-pymongo)）。

## <a name="install-postgresql"></a>安装 PostgreSQL

[!INCLUDE [Install and run PostgresQL](../includes/install-and-run-postgres.md)]

### <a name="vs-code-support-for-postgresql"></a>对 PostgreSQL 的 VS Code 支持

VS Code 支持通过 [PostgreSQL 扩展](https://marketplace.visualstudio.com/items?itemName=ms-ossdata.vscode-postgresql)来处理 PostgreSQL 数据库，你可以在 VS Code 中创建、管理、查询和连接到 PostgreSQL 数据库。

## <a name="install-mongodb"></a>安装 MongoDB

[!INCLUDE [Install and run Mongo](../includes/install-and-run-mongo.md)]

### <a name="vs-code-support-for-mongodb"></a>对 MongoDB 的 VS Code 支持

VS Code 支持通过 [Azure CosmosDB 扩展](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)来处理 MongoDB 数据库，你可以在 VS Code 中创建、管理、查询和连接到 MongoDB 数据库。

有关详细信息，请访问 VS Code 文档：[使用 MongoDB](https://code.visualstudio.com/docs/azure/mongodb)。

## <a name="set-up-profile-aliases"></a>设置配置文件别名

[!INCLUDE [Set up profile aliases](../includes/profile-aliases.md)]
