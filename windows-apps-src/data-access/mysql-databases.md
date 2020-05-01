---
title: 在 UWP 应用中使用 MySQL 数据库
description: 在 UWP 应用中使用 MySQL 数据库。
ms.date: 03/28/2019
ms.topic: article
keywords: windows 10, uwp, MySQL, 数据库
ms.localizationpriority: medium
ms.openlocfilehash: bfed9c0a0c4198095b9be48fe71832bdfca67718
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "67713782"
---
# <a name="use-a-mysql-database"></a>使用 MySQL 数据库
本文包含启用在 UWP 应用中使用 MySQL 数据库功能所需的步骤。 另外，本文还包含一个小的代码片段，演示如何在代码中与数据库交互。

## <a name="set-up-your-solution"></a>设置解决方案

要将应用直接连接到 MySQL 数据库，请确保项目的最低版本以 Fall Creators Update（版本 16299）为目标。  可以在 UWP 项目的属性页中找到该信息。

![VisualStudio 中“目标属性”窗格的图片，显示目标版本和最低版本已设置为 Fall Creators Update](images/min-version-fall-creators.png)

打开“程序包管理器控制台”  （“视图”->“其他窗口”->“程序包管理器控制台”）。 使用 **Install-Package MySql.Data** 命令安装 MySQL 的驱动程序。 这样就可以通过编程方式访问 MySQL 数据库。

## <a name="test-your-connection-using-sample-code"></a>使用示例代码测试连接
下面通过示例演示了如何连接到远程 MySQL 数据库并从其读取数据。 请注意，需自定义 IP 地址、凭据和数据库名称。

```csharp
string M_str_sqlcon = "server=10.xxx.xx.xxx;user id=foo;password=bar;database=baz";
MySqlConnection mysqlcon = new MySqlConnection(M_str_sqlcon);
MySqlCommand mysqlcom = new MySqlCommand("select * from table1", mysqlcon);
mysqlcon.Open();
MySqlDataReader mysqlread = mysqlcom.ExecuteReader(CommandBehavior.CloseConnection);
while (mysqlread.Read())
{
    Debug.WriteLine(mysqlread.GetString(0)+":"+mysqlread.GetString(1));
}
mysqlcon.Close();
```
