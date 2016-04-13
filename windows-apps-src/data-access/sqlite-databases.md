---
ms.assetid: 5A47301A-2291-4FC8-8BA7-55DB2A5C653F
SQLite 数据库
SQLite 是一种无服务器的嵌入式数据库引擎。 本文介绍了如何使用包含在 SDK 中的 SQLite 库、如何将自己的 SQLite 库打包在通用 Windows 应用中或从源生成该库。
---
# SQLite 数据库

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 的文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


SQLite 是一种无服务器的嵌入式数据库引擎。 本文介绍了如何使用包含在 SDK 中的 SQLite 库、如何将自己的 SQLite 库打包在通用 Windows 应用中或从源生成该库。

## SQLite 是什么以及何时使用它

SQLite 是一个开源的无服务器嵌入式数据库。 这些年来，它已作为面向存储在许多平台和设备上的数据的主要设备端技术出现。 通用 Windows 平台 (UWP) 支持并建议使用 SQLite 实现跨所有 Windows 10 设备系列的本地存储。

SQLite 最适用于手机应用、面向 Windows 10 IoT 核心版（IoT 核心版）的嵌入式应用程序，以及作为企业关系数据库服务器 (RDBS) 数据的缓存。 它将满足大多数本地数据访问需求，除非这些数据需要大量的并发写入，或除了与大多数应用不同的大数据规模方案。

在媒体播放和游戏应用程序中，SQLite 还可用作文件格式来存储目录或其他资源（例如游戏级别），可以从 Web 服务器按原样下载该文件格式。

## 向 UWP 应用项目添加 SQLite

有三种方法将 SQLite 添加到 UWP 项目。

1.  [使用 SDK SQLite](#using-the-sdk-sqlite)
2.  [将 SQLite 包含在应用包内](#including-sqlite-in-the-app-package)
3.  [从 Visual Studio 中的源生成 SQLite](#building-sqlite-from-source-in-visual-studio)

### 使用 SDK SQLite

你可能想要使用包含在 UWP SDK 中的 SQLite 库来减小应用程序包的大小，并依赖于该平台来定期更新库。 使用 SDK SQLite 还可能带来性能优势（例如提供更快的启动时间），SQLite 库很有可能已加载在内存中以供系统组件使用。

若要引用 SDK SQLite，请将以下标头包含在项目中。 该标头还包含了平台中支持的 SQLite 版本。

`#include <winsqlite/winsqlite3.h>`

配置要链接到 winsqlite3.lib 的项目。 在**解决方案资源管理器**中，右键单击你的项目，然后依次选择**“属性”**>**“链接器”**>**“输入”**，然后将 winsqlite3.lib 添加到**“其他依赖项”**。

### 2. 将 SQLite 包含在应用包内

你有时可能想要打包自己的库而非使用 SDK 版本，例如，你可能想要在跨平台客户端中使用与 SDK 中所含 SQLite 版本不同的特定版本。

在 SQLite.org 提供的通用 Windows 平台 Visual Studio 扩展上或通过“扩展和更新”工具安装 SQLite 库。

![“扩展和更新”屏幕](./images/extensions-and-updates.png)

安装扩展后，在代码中引用以下头文件。

`#include <sqlite3.h>`

### 3. 从 Visual Studio 中的源生成 SQLite

你有时可能想要编译自己的 SQLite 二进制文件来使用[各编译器选项](http://www.sqlite.org/compile.html)减小文件大小、调整库的性能或定制为你的应用程序设置的功能。 SQLite 提供的选项可用于进行平台配置、设置默认参数值、设置大小限制、控制操作特点、启用正常情况下的功能关闭状态、禁用正常情况下的功能打开状态、省略功能、启用分析和调试以及在 Windows 上管理内存分配行为。

*将源添加到 Visual Studio 项目*

SQLite 源代码可在 [SQLite.org 下载页面](https://www.sqlite.org/download.html)上进行下载。 将此文件添加到你希望在其中使用 SQLite 的应用程序的 Visual Studio 项目。

*配置预处理器*

除了任何其他[编译时间选项](http://www.sqlite.org/compile.html)以外，始终使用 SQLITE\_OS\_WINRT 和 SQLITE\_API=\_\_declspec(dllexport)。

![“SQLite 属性页”屏幕](./images/property-pages.png)

## 管理 SQLite 数据库

可以使用 SQLite C API 创建、更新和删除 SQLite 数据库。 SQLite C API 的详细信息位于 SQLite.org [SQLite C/C++ 接口简介](http://www.sqlite.org/cintro.html)页面。

若要充分了解 SQLite 的工作方式，请从用于评估 SQL 语句的 SQL 数据库主要任务开始回顾。 请谨记两个对象：

-   [数据库连接句柄](https://www.sqlite.org/c3ref/sqlite3.html)
-   [准备好的语句对象](https://www.sqlite.org/c3ref/stmt.html)

有六个接口来执行对这些对象的数据库操作：

-   [sqlite3\_open()](https://web.archive.org/web/20141228070025/http:/www.sqlite.org/c3ref/open.html)
-   [sqlite3\_prepare()](https://web.archive.org/web/20141228070025/http:/www.sqlite.org/c3ref/prepare.html)
-   [sqlite3\_step()](https://web.archive.org/web/20141228070025/http:/www.sqlite.org/c3ref/step.html)
-   [sqlite3\_column()](https://web.archive.org/web/20141228070025/http:/www.sqlite.org/c3ref/column_blob.html)
-   [sqlite3\_finalize()](https://web.archive.org/web/20141228070025/http:/www.sqlite.org/c3ref/finalize.html)
-   [sqlite3\_close()](https://web.archive.org/web/20141228070025/http:/www.sqlite.org/c3ref/close.html)

 

 






<!--HONumber=Mar16_HO1-->


