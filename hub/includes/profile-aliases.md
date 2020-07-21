---
ms.topic: include
author: mattwojo
ms.author: mattwoj
ms.date: 10/04/2019
ms.openlocfilehash: 2f7a57f1652ecab81a70c39faa1b70c42ed6a3de
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "72314901"
---
键入 `sudo service mongodb start` 或 `sudo service postgres start` 和 `sudo -u postgrest psql` 可能会很繁琐。  但是，你可以考虑在 WSL 上的 `.profile` 文件中设置别名，使这些命令更便于使用、易于记忆。 

要设置自己的自定义别名或快捷方式来执行这些命令，请执行以下操作：

1. 打开 WSL 终端并输入 `cd ~`以确保位于根目录中。
2. 使用终端文本编辑器 Nano 打开 `.profile` 文件，该文件可控制终端的设置：`sudo nano .profile`
3. 在文件底部（请勿更改 `# set PATH` 设置），添加以下内容：

    ```bash
    # My Aliases
    alias start-pg='sudo service postgresql start'
    alias run-pg='sudo -u postgres psql'
    ```

这样你就可以输入 `start-pg` 开始运行 postgresql 服务，并输入 `run-pg` 来打开 psql shell。 `start-pg` 和 `run-pg` 可更改为所需的任何名称，但是请注意不要覆盖 postgres 已经使用的命令！

4. 添加新别名后，请使用 Ctrl + X 退出 Nano 文本编辑器 - 系统提示“保存并输入”时选择 **（是）（将文件名保留为** ）`Y``.profile`。
5. 关闭并重新打开 WSL 终端，然后尝试使用新的别名命令。
