---
ms.topic: include
author: mattwojo
ms.author: mattwoj
ms.date: 10/04/2019
ms.openlocfilehash: f594600991f08a7dfda784ae127be2e6438dacbd
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/15/2019
ms.locfileid: "72314881"
---
安装 MongoDB：

1. 打开 WSL 终端（即Ubuntu 18.04）。
2. 更新 Ubuntu 包： `sudo apt update`
3. 更新包后，使用以下内容安装 MongoDB： `sudo apt-get install mongodb`
4. 确认安装并获取版本号： `mongod --version`

安装 MongoDB 后，需要知道3个命令：

1. `sudo service mongodb status`，用于检查数据库的状态。
2. `sudo service mongodb start`，开始运行数据库。
3. `sudo service mongodb stop` 以停止运行数据库。

> [!NOTE]
> 你可能会看到教程或文章中使用的命令 `sudo systemctl status mongodb`。 为了保持轻型，WSL 不包含 `systemd` （Linux 中的服务管理系统）。 相反，它使用 SysVinit 在计算机上启动服务。 你不会注意到差别，但如果教程建议使用 `sudo systemctl`，请改用： `sudo /etc/init.d/`。 例如，`sudo systemctl status mongodb`，WSL 将为 `sudo /etc/inid.d/mongodb status` .。。也可以使用 @no__t。

### <a name="run-your-mongo-database-in-a-local-server"></a>在本地服务器中运行 Mongo 数据库

1. 检查数据库的状态：`sudo service mongodb status` 您应该会看到 [Fail] 响应，除非您已经启动了数据库。

2. 启动数据库：`sudo service mongodb start` 现在应会看到一个 [OK] 响应。

3. 通过连接到数据库服务器并运行诊断命令来进行验证：`mongo --eval 'db.runCommand({ connectionStatus: 1 })'` 这会输出当前数据库版本、服务器地址和端口以及状态命令的输出。 如果响应中的 "确定" 字段的值为 `1`，则表示服务器正在运行。

4. 若要停止 MongoDB 服务的运行，请输入： `sudo service mongodb stop`

> [!NOTE]
> MongoDB 具有几个默认参数，包括在/data/db 中存储数据以及在端口27017上运行。 此外，@no__t 为守护程序（数据库的主机进程），`mongo` 是连接到 @no__t 的特定实例的命令行外壳。
