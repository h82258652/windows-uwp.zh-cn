---
ms.topic: include
author: mattwojo
ms.author: mattwoj
ms.date: 10/04/2019
ms.openlocfilehash: 2f7a57f1652ecab81a70c39faa1b70c42ed6a3de
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/15/2019
ms.locfileid: "72314901"
---
键入 `sudo service mongodb start` 或 `sudo service postgres start`，`sudo -u postgrest psql` 可能会变得枯燥乏味。  但是，你可以考虑在 WSL 上的 `.profile` 文件中设置别名，使这些命令更快使用并且更容易记住。 

若要设置自己的自定义别名或快捷方式，请执行以下命令：

1. 打开你的 WSL 终端并输入 `cd ~` 以确保你在根目录中。
2. 打开 `.profile` 文件，该文件用终端文本编辑器、Nano： @no__t 控制终端的设置。
3. 在文件底部（请不要更改 @no__t 设置），添加以下内容：

    ```bash
    # My Aliases
    alias start-pg='sudo service postgresql start'
    alias run-pg='sudo -u postgres psql'
    ```

这将允许你输入 `start-pg`，开始运行 postgresql 服务，并 `run-pg` 打开 psql shell。 您可以更改 `start-pg` 并 `run-pg` 更改为所需的任何名称，只需注意不要覆盖 postgres 已使用的命令！

4. 添加新别名后，请使用**Ctrl + X**退出 Nano 文本编辑器--在系统提示保存并输入时，请选择 `Y` （是）（将文件名保留为 `.profile`）。
5. 关闭并重新打开你的 WSL 终端，然后尝试新的别名命令。
