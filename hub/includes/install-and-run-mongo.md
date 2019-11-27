---
ms.topic: include
author: mattwojo
ms.author: mattwoj
ms.date: 10/04/2019
ms.openlocfilehash: f594600991f08a7dfda784ae127be2e6438dacbd
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/15/2019
ms.locfileid: "72314881"
---
要安装 MongoDB，请执行以下操作：

1. 打开 WSL 终端（即 Ubuntu 18.04）。
2. 更新 Ubuntu 包：`sudo apt update`
3. 更新该包后，使用以下命令安装 MongoDB：`sudo apt-get install mongodb`
4. 确认安装并获取版本号：`mongod --version`

安装 MongoDB 后，需要知道以下 3 个命令：

1. `sudo service mongodb status` 用于检查数据库的状态。
2. `sudo service mongodb start` 用于开始运行数据库。
3. `sudo service mongodb stop` 用于停止运行数据库。

> [!NOTE]
> 你可能会看到在教程或文章中使用了命令 `sudo systemctl status mongodb`。 为了保持轻量，WSL 不包括 `systemd`（Linux 中的服务管理系统）。 它改为使用 SysVinit 在计算机上启动服务。 你应该不会注意到有什么区别，但是如果教程建议使用 `sudo systemctl`，请改用 `sudo /etc/init.d/`。 例如，对于 WSL，`sudo systemctl status mongodb` 应改用 `sudo /etc/inid.d/mongodb status`，或者也可以使用 `sudo service mongodb status`。

### <a name="run-your-mongo-database-in-a-local-server"></a>在本地服务器中运行 Mongo 数据库

1. 检查数据库的状态：`sudo service mongodb status` 除非已经启动数据库，否则应该会显示 [Fail] 响应。

2. 启动数据库：`sudo service mongodb start` 现在，应该会显示 [OK] 响应。

3. 通过连接到数据库服务器并运行诊断命令进行验证：`mongo --eval 'db.runCommand({ connectionStatus: 1 })'` 这将输出当前数据库版本、服务器地址和端口以及状态命令的输出。 响应中“ok”字段的值 `1` 表示服务器正在运行。

4. 要停止运行 MongoDB 服务，请输入：`sudo service mongodb stop`

> [!NOTE]
> MongoDB 有几个默认参数，包括在 /data/db 中存储数据和在端口 27017 上运行。 此外，`mongod` 是守护程序（数据库的主机进程），`mongo` 是连接到特定实例 `mongod` 的命令行 shell。
