---
title: 使用数据库在 Windows 上开始使用 Python
description: 本指南可帮助你开始在 Windows 上将 PostgreSQL 或 MongoDB 与 Python 配合使用。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: python，windows 10，postgresql，mongodb，postgres，mongo，microsoft，python on windows，在 windows 上安装 postgresql，在 windows 上安装 mongodb，使用带有 python 的 mongodb，在 postgresql 上使用 mongodb，postgresql
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: 9b1bdea86739f3d58b39cf7f0e6b8090474886f3
ms.sourcegitcommit: 60d2d15dd0d365f82e4e90e4bc34b40cf5b4a247
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2019
ms.locfileid: "72517777"
---
# <a name="get-started-using-postgresql-or-mongodb-with-python-on-windows"></a>在 Windows 上开始将 PostgreSQL 或 MongoDB 与 Python 配合使用

此循序渐进指南可帮助你开始将 Python 应用连接到数据库。 我们选择将重点放在两个常见选项上： PostgreSQL 和 MongoDB。

## <a name="differences-between-mongodb-and-postgresql"></a>MongoDB 和 PostgreSQL 之间的差异

[!INCLUDE [Postgres vs Mongo](../includes/postgres-v-mongo.md)]

> [!NOTE]
> 你可能还需要考虑如何将所使用的框架和工具集成到特定数据库系统中。 [Django web 框架](./web-frameworks.md#hello-world-tutorial-for-django)似乎更好地与 PostgreSQL 集成（请参阅[Django 文档](https://docs.djangoproject.com/en/2.2/ref/contrib/postgres/)和[psycopg2](https://github.com/psycopg/psycopg2)）。 [Flask web 框架](./web-frameworks.md#hello-world-tutorial-for-flask)似乎与 MongoDB 更好地集成（请参阅[MongoEngine](https://github.com/MongoEngine/flask-mongoengine)和[PyMongo](https://github.com/dcrosta/flask-pymongo)）。

## <a name="install-postgresql"></a>安装 PostgreSQL

[!INCLUDE [Install and run PostgresQL](../includes/install-and-run-postgres.md)]

### <a name="vs-code-support-for-postgresql"></a>VS Code 对 PostgreSQL 的支持

VS Code 支持通过[PostgreSQL 扩展](https://marketplace.visualstudio.com/items?itemName=ms-ossdata.vscode-postgresql)使用 PostgreSQL 数据库，则可以从 VS Code 中创建、连接、管理和查询 PostgreSQL 数据库。

## <a name="install-mongodb"></a>安装 MongoDB

[!INCLUDE [Install and run Mongo](../includes/install-and-run-mongo.md)]

### <a name="vs-code-support-for-mongodb"></a>MongoDB VS Code 支持

VS Code 支持通过[Azure CosmosDB 扩展](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)使用 mongodb 数据库，可以在 VS Code 中创建、连接、管理和查询 mongodb 数据库。

若要了解详细信息，请访问 VS Code 文档：[使用 MongoDB](https://code.visualstudio.com/docs/azure/mongodb)。

## <a name="set-up-profile-aliases"></a>设置配置文件别名

[!INCLUDE [Set up profile aliases](../includes/profile-aliases.md)]
