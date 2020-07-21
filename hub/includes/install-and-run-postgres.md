---
ms.topic: include
author: mattwojo
ms.author: mattwoj
ms.date: 10/04/2019
ms.openlocfilehash: aa1da2a65f95d92e895533ed37426b5e454c255b
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "72314891"
---
要安装 PostgreSQL，请执行以下操作：

1. 打开 WSL 终端（即 Ubuntu 18.04）。
2. 更新 Ubuntu 包：`sudo apt update`
3. 更新该包后，使用以下命令安装 PostgreSQL（和 -contrib 包，其中包含一些有用的实用程序）：`sudo apt install postgresql postgresql-contrib`
4. 确认安装并获取版本号：`psql --version`

安装 PostgreSQL 后，需要知道以下 3 个命令：

1. `sudo service postgresql status` 用于检查数据库的状态。
2. `sudo service postgresql start` 用于开始运行数据库。
3. `sudo service postgresql stop` 用于停止运行数据库。

### <a name="postgresql-user-setup"></a>PostgreSQL 用户设置

默认管理员用户 `postgres` 需要分配的密码才能连接到数据库。 要设置密码，请执行以下操作：

1. 输入命令：`sudo passwd postgres`
2. 系统将提示你输入新密码。
3. 关闭并重新打开终端。

### <a name="run-postgresql-with-psql-shell"></a>通过 psql shell 运行 PostgreSQL

[psql](https://www.postgresql.org/docs/10/app-psql.html) 是 PostgreSQL 的基于终端的前端。 它使你能够以交互方式键入查询，将其发布到 PostgreSQL，并查看查询结果。 或者，输入也可由文件提供。 此外，它还提供了许多元命令和各种类似于 shell 的功能，便于编写脚本和自动执行各种任务。

要启动 psql shell，请执行以下操作：

1. 启动 postgres 服务：`sudo service postgresql start`
2. 连接到 postgres 服务，并打开 psql shell：`sudo -u postgres psql`

成功输入 psql shell 后，将显示更改为如下所示的命令行：`postgres=#`

> [!NOTE]
> 或者，也可以通过使用 `su - postgres` 切换为 postgres 用户，然后输入命令 `psql` 来打开 psql shell。

要退出 postgres=# enter，请使用 `\q` 或使用快捷键：Ctrl+D

要查看在 PostgreSQL 安装上创建的用户帐户，请在 WSL 终端上使用 `psql -c "\du"`；如果已打开 psql shell，则仅使用 `\du`。 此命令将显示以下列：帐户用户名、角色属性列表和角色组成员。 要返回命令行，请输入：`q`。
