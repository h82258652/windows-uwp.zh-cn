---
ms.topic: include
author: mattwojo
ms.author: mattwoj
ms.date: 10/04/2019
ms.openlocfilehash: aa1da2a65f95d92e895533ed37426b5e454c255b
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/15/2019
ms.locfileid: "72314891"
---
若要安装 PostgreSQL：

1. 打开 WSL 终端（即Ubuntu 18.04）。
2. 更新 Ubuntu 包： `sudo apt update`
3. 更新包后，请使用以下内容安装 PostgreSQL （和 contrib 包，其中包含一些有用的实用工具）： `sudo apt install postgresql postgresql-contrib`
4. 确认安装并获取版本号： `psql --version`

安装 PostgreSQL 后，需要知道3个命令：

1. `sudo service postgresql status`，用于检查数据库的状态。
2. `sudo service postgresql start`，开始运行数据库。
3. `sudo service postgresql stop` 以停止运行数据库。

### <a name="postgresql-user-setup"></a>PostgreSQL 用户设置

默认管理员用户 `postgres`）需要分配密码才能连接到数据库。 设置密码：

1. 输入命令： `sudo passwd postgres`
2. 系统将提示你输入新密码。
3. 关闭并重新打开终端。

### <a name="run-postgresql-with-psql-shell"></a>通过 psql shell 运行 PostgreSQL

[psql](https://www.postgresql.org/docs/10/app-psql.html)是一个基于终端的前端到 PostgreSQL。 它使你能够以交互方式键入查询，将它们发布到 PostgreSQL，并查看查询结果。 也可以从文件中输入。 此外，它还提供许多元命令和各种类似 shell 的功能，便于编写脚本和自动执行各种任务。

若要启动 psql shell：

1. 启动 postgres 服务： `sudo service postgresql start`
2. 连接到 postgres 服务并打开 psql shell： `sudo -u postgres psql`

成功输入 psql shell 后，你将看到命令行更改如下所示： `postgres=#`

> [!NOTE]
> 或者，你可以通过以下方式打开 psql shell：使用以下命令切换到 postgres 用户： `su - postgres`，然后输入以下命令： `psql`。

若要退出 postgres = # enter： `\q` 或使用快捷键：Ctrl+D

若要查看已在 PostgreSQL 安装上创建了哪些用户帐户，请从你的 WSL 终端使用： `psql -c "\du"` .。。或者只 `\du` （如果已打开 psql shell）。 此命令将显示列：帐户用户名、角色列表和角色组的成员。 若要退出到命令行，请输入： `q`。
