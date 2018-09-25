---
author: libbymc
title: 创建具有 REST 后端的单页 Web 应用
description: 使用受欢迎的 Web 技术为 Microsoft Store 生成托管 Web 应用
keywords: 托管 Web 应用, HWA, REST API, 单页应用, SPA
ms.author: libbymc
ms.date: 05/10/2017
ms.topic: article
ms.prod: Microsoft Edge, Azure, Visual Studio Code
ms.technology: web
ms.localizationpriority: medium
ms.openlocfilehash: 42f11cbdd749a44c4ba0d8bc1a0397a4f2882257
ms.sourcegitcommit: 232543fba1fb30bb1489b053310ed6bd4b8f15d5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/25/2018
ms.locfileid: "4181468"
---
# <a name="create-a-single-page-web-app-with-rest-api-backend"></a>创建具有 REST 后端的单页 Web 应用

**使用受欢迎的全栈 Web 技术为 Microsoft Store 生成托管 Web 应用**

![单页 Web 应用形式的简单记忆游戏](images/fullstack.png)

此两部分教程提供新式全栈 Web 开发的快速导览，帮助你生成既可以在浏览器中运行，又可以以 Microsoft Store 的托管 Web 应用形式运行的简单记忆游戏。 在部分 I 中，你将为游戏后端生成简单的 REST API 服务。 通过在作为 API 服务的云中托管游戏逻辑，你保留游戏状态，让用户可以跨不同的设备一直玩同一个游戏实例。 在部分 II 中，你将使用响应布局以单页 Web 应用的形式生成前端 UI。

我们将使用一些最受欢迎的 Web 技术，其中包括用于服务器端开发的 [Node.js](https://nodejs.org/en/) 运行时和 [Express](http://expressjs.com/)、[Bootstrap](http://getbootstrap.com/) UI 框架、[Pug](https://www.npmjs.com/package/pug) 模板引擎，以及用于生成 RESTful API 的 [Swagger](http://swagger.io/tools/)。 你还可以获得用于云托管并使用 [Visual Studio Code](https://code.visualstudio.com/) 编辑器的 [Azure 门户](https://ms.portal.azure.com/)的相关经验。

## <a name="prerequisites"></a>必备条件

如果你的计算机上还没有这些资源，请使用以下下载链接：

 - [Node.js](https://nodejs.org/en/download/) - 请务必选择此选项以将 Node 添加到你的 PATH。

 - [Express 生成器](http://expressjs.com/en/starter/generator.html) - 安装 Node 后，安装 Express，方法是运行 `npm install express-generator -g`

 - [Visual Studio Code](https://code.visualstudio.com/)

如果你想要完成在 Microsoft Azure 上托管 API 服务和记忆游戏应用的最终步骤，你将需要[创建免费的 Azure 帐户](https://azure.microsoft.com/en-us/free/)（如果尚未这样做）。

如果你决定放弃（或推后）Azure 部分，只需跳过部分 I 和 II 的最后部分，这些部分介绍了 Microsoft Store 应用的 Azure 托管和打包。 你生成的 API 服务和 Web 应用将仍然在你的计算机上本地运行（分别从 `http://localhost:8000` 和 `http://localhost:3000`）。

## <a name="part-i-build-a-rest-api-backend"></a>部分 I：生成 REST API 后端

我们首先将生成一个简单的记忆游戏 API 来支持我们的记忆游戏 Web 应用。 我们将使用 [Swagger](http://swagger.io/) 来定义我们的 API，并生成基架代码和 Web UI 以进行手动测试。

如果你想要跳过此部分，并直接进入[部分 II：生成单页 Web 应用程序](#part-ii-build-a-single-page-web-appl)，那么这里有[部分 I 的完成代码](https://github.com/Microsoft/Windows-tutorials-web/tree/master/Single-Page-App-with-REST-API/backend)。按照*自述*说明在本地启动并运行代码，或参见 *5. 在 Azure 上托管 API 服务和启用 CORS* 从 Azure 运行。

### <a name="game-overview"></a>游戏概述

*记忆*（也称为[*注意力*](https://en.wikipedia.org/wiki/Concentration_(game))和[*配尔曼式记忆*](https://en.wikipedia.org/wiki/Pelmanism_(system))），是一个由一幅卡片组对构成的简单游戏。 卡片正面朝下放在桌子上，玩家检查卡片的数值，一次两张，寻找相同卡片。 每一轮后，卡片再次正面朝下摆放，除非找到相同的一对，找到后的两张卡片从游戏中清除。 游戏的目标是使用最少的翻牌次数找到所有牌对。

为指导起见，我们将使用的游戏结构非常简单：单一游戏，一个玩家。 但是，游戏逻辑是服务器端（而不是在客户端）保存游戏状态，以便你可以跨不同设备继续玩同一个游戏。

记忆游戏的数据结构只包含一排 JavaScript 对象，每个对象代表一张卡片，阵列中的索引充当卡片 ID。 在服务器上，每个卡片对象有一个值和一个**已清除**标记。 例如，一板有 2 组匹配（4 张卡片）可能随机生成，并像这样序列化。

```json
[
    { "cleared":"false",
        "value":"0",
    },
    { "cleared":"false",
        "value":"1",
    },
    { "cleared":"false",
        "value":"1",
    },
    { "cleared":"false",
        "value":"0",
    }
]
```

当板阵列传递到客户端时，数值键被从阵列中删除，以避免欺骗（例如，通过使用 F12 浏览器工具检查 HTTP 响应正文）。 下面介绍了相同的新游戏如何呈现给调用 **GET /game** REST 终结点的客户端：

```json
[{ "cleared":"false"},{ "cleared":"false"},{ "cleared":"false"},{ "cleared":"false"}]
```

说到终结点，记忆游戏服务将包括三个 REST API。

#### <a name="post-new"></a>POST /new
初始化指定大小（匹配项数目）的新游戏板。

| 参数 | 描述 |
|-----------|-------------|
| 初始*大小* |将被打乱放入游戏“板”的匹配对数量。 示例： `http://memorygameapisample/new?size=2`|

| 响应 | 描述 |
|----------|-------------|
| 200 OK | 请求大小的新记忆游戏已准备就绪。|
| 400 BAD REQUEST| 请求的大小超出可接受范围。|


#### <a name="get-game"></a>GET /game
恢复记忆游戏板的当前状态。

*无参数*

| 响应 | 描述 |
|----------|-------------|
| 200 OK | 返回卡片对象的 JSON 阵列。 每张卡片具有**已清除**属性，指示其匹配是否已被找到。 匹配的卡片还会报告它们的**数值**。 示例： `[{"cleared":"false"},{"cleared":"false"},{"cleared":"true","value":1},{"cleared":"true","value":1}]`|

#### <a name="put-guess"></a>PUT /guess
指定要揭开的卡片，检查与之前揭开的卡片是否匹配。

| 参数 | 描述 |
|-----------|-------------|
| 初始*卡片* | 要揭开的卡片的卡片 ID（游戏板阵列中的索引）。 每个完整的“猜牌”包含两张指定卡片（即，使用有效的唯一*卡片*值两次调用 **/guess**）。 示例： `http://memorygameapisample/guess?card=0`|

| 响应 | 描述 |
|----------|-------------|
| 200 OK | 返回 JSON，包含指定卡片的 **id** 和**数值**。 示例： `[{"id":0,"value":1}]`|
| 400 BAD REQUEST |  指定卡片出错。 请参见 HTTP 响应正文了解更多详细信息。|

### <a name="1-spec-out-the-api-and-generate-code-stubs"></a>1. 指出 API 并生成代码存根

我们将使用 [Swagger](http://swagger.io/) 将我们的记忆游戏 API 的设计转换为工作 Node.js 服务器代码。 下面介绍了如何将我们的[记忆游戏 API 定义为 Swagger 元数据](https://github.com/Microsoft/Windows-tutorials-web/blob/master/Single-Page-App-with-REST-API/backend/api.json)。 我们将使用它来生成服务器代码存根。

1. 创建一个新文件夹（比如在你本地的 *GitHub* 目录中），并下载包含记忆游戏 API 定义的 [**api.json**](https://raw.githubusercontent.com/Microsoft/Windows-tutorials-web/master/Single-Page-App-with-REST-API/backend/api.json?token=ACEfklXAHTeLkHYaI5plV20QCGuqC31cks5ZFhVIwA%3D%3D) 文件。 请确保你的文件夹名称不包含任何空格。

2. 打开你喜爱的该文件夹的 Shell（[或使用 Visual Studio Code 的集成终端！](https://code.visualstudio.com/docs/editor/integrated-terminal)），并运行以下节点包管理器 (NPM) 命令，为你的全局 (**-g**) Node 环境安装 [Yeoman](http://yeoman.io/) (yo) 代码基架工具和 Swagger 生成器：

    ```
    npm install -g yo
    npm install -g generator-swaggerize
    ```

3. 现在，我们可以使用 Swagger 生成服务器基架代码：

    ```
    yo swaggerize
    ```

4. **swaggerize** 命令会询问你几个问题。
    - swagger 文档的路径（或 URL）：**api.json**
    - 框架：**express**
    - 你希望怎样称呼此项目（YourFolderNameHere）：**[输入]**

    根据个人意愿回答其他所有问题；此信息主要是为 *package.json* 文件提供你的联系信息，以便你可以以 NPM 程序包的形式分发代码。

5. 最后，为你的新项目和 [Swagger UI](http://swagger.io/swagger-ui/) 支持安装所有依赖项（*package.json* 中已列出）。

    ```
    npm install
    npm install swaggerize-ui
    ```

    现在开始 VS 代码和**文件** > **打开文件夹…**，并移至 MemoryGameAPI 目录。 这是你刚才创建的 Node.js API 服务器！ 它使用受欢迎的 [ExpressJS](http://expressjs.com/en/4x/api.html) Web 应用程序框架来构建和运行项目。

### <a name="2-customize-the-server-code-and-setup-debugging"></a>2. 自定义服务器代码和设置调试

项目根中的 *Server.js* 文件充当服务器的“主”函数。 在 VS 代码中打开它，并向其复制以下内容。 通过生成的代码修改的行被添加注释，包含进一步说明。

```javascript
'use strict';

var port = process.env.PORT || 8000; // Better flexibility than hardcoding the port

var Http = require('http');
var Express = require('express');
var BodyParser = require('body-parser');
var Swaggerize = require('swaggerize-express');
var SwaggerUi = require('swaggerize-ui'); // Provides UI for testing our API
var Path = require('path');

var App = Express();
var Server = Http.createServer(App);

App.use(function(req, res, next) {  // Enable cross origin resource sharing (for app frontend)
    res.header('Access-Control-Allow-Origin', '*');
    res.header('Access-Control-Allow-Methods', 'GET,PUT,POST,OPTIONS');
    res.header('Access-Control-Allow-Headers', 'Content-Type, Authorization, Content-Length, X-Requested-With');

    // Prevents CORS preflight request (for PUT game_guess) from redirecting
    if ('OPTIONS' == req.method) {
      res.send(200);
    }
    else {
      next(); // Passes control to next (Swagger) handler
    }
});

App.use(BodyParser.json());
App.use(BodyParser.urlencoded({
    extended: true
}));

App.use(Swaggerize({
    api: Path.resolve('./config/swagger.json'),
    handlers: Path.resolve('./handlers'),
    docspath: '/swagger'   //  Hooks up the testing UI
}));

App.use('/', SwaggerUi({    // Serves the testing UI from our base URL
  docs: '/swagger'          //
}));

Server.listen(port, function () {  // Starts server with our modfied port settings
 });
```

接着，该运行你的服务器了！ 当我们进入 Node 时，让我们为 Node 调试设置 Visual Studio Code。 选择**调试**面板图标 (Ctrl+Shift+D)，然后选择齿轮图标（打开 launch.json），并将“配置”修改为此设置：

```json
"configurations": [
    {
        "type": "node",
        "request": "launch",
        "name": "Launch Program",
        "program": "${workspaceRoot}/server.js"
    }
]
```

现在，按 F5 并将你的浏览器打开到 [http://localhost:8000](http://localhost:8000)。 页面应打开到记忆游戏 API 的 Swagger UI，从这里，可以展开每种方法的详细信息和输入字段。 你甚至可以尝试调用 API，尽管它们的响应仅包含模拟数据（由 [Swagmock](https://www.npmjs.com/package/swagmock) 模块提供）。 现在是时候添加游戏逻辑以使这些 API 实际可用。

### <a name="3-set-up-your-route-handlers"></a>3. 设置路由处理程序

Swagger 文件 (config\swagger.json) 通过将它定义的每个 URL 路径映射到处理程序文件（在 \handlers 中），以及将为该路径定义的每个方法（例如，**GET**、**POST**）映射到该处理程序文件内的 `operationId`（函数），来指示我们的服务器如何处理各个客户端 HTTP 请求。

在我们程序的这一层，我们会在将各个客户端请求传递到数据模型前添加一些输入检查。 下载（或复制并粘贴）：

 - 此 [game.js](https://raw.githubusercontent.com/Microsoft/Windows-tutorials-web/master/Single-Page-App-with-REST-API/backend/handlers/game.js?token=ACEfkvhw6BUnkeSsZgnzVe086T5WLwjfks5ZFhW5wA%3D%3D) 代码到你的 **handlers\game.js** 文件
 - 此 [guess.js](https://raw.githubusercontent.com/Microsoft/Windows-tutorials-web/master/Single-Page-App-with-REST-API/backend/handlers/guess.js?token=ACEfkswel02rHVr0e61bVsNdpv_i1Rtuks5ZFhXPwA%3D%3D) 代码到你的 **handlers\guess.js** 文件
 - 此 [new.js](https://raw.githubusercontent.com/Microsoft/Windows-tutorials-web/master/Single-Page-App-with-REST-API/backend/handlers/new.js?token=ACEfkgk2QXJeRc0aaLzY5ulFsAR4Juidks5ZFhXawA%3D%3D) 代码到你的 **handlers\new.js** 文件

 你可以浏览这些文件中的注释了解关于更改的更多详细信息，但实际上，它们检查基本输入错误（例如，客户端请求的新游戏匹配小于一组），并根据需要发送描述性错误消息。 处理程序还将有效的客户端请求路由到其相应的数据文件（在 \data 中）以进行进一步处理。 让我们在下一步介绍这些内容。

### <a name="4-set-up-your-data-model"></a>4. 设置数据模型

现在应该将占位符数据模拟服务替换为我们记忆游戏板的实际数据模型。

我们程序的这一层表示内存卡本身，并提供大量游戏逻辑，包括“打乱”纸牌进行新的游戏，确定匹配卡片的对数，并跟踪游戏状态。 复制并粘贴：

 - 此 [game.js](https://raw.githubusercontent.com/Microsoft/Windows-tutorials-web/master/Single-Page-App-with-REST-API/backend/data/game.js?token=ACEfksAceJNQmhF82aHjQTx78jILYKfCks5ZFhX4wA%3D%3D) 代码到你的 **data\game.js** 文件
 - 此 [guess.js](https://raw.githubusercontent.com/Microsoft/Windows-tutorials-web/master/Single-Page-App-with-REST-API/backend/data/guess.js?token=ACEfkvY69Zr1AZQ4iXgfCgDxeinT21bBks5ZFhYBwA%3D%3D) 代码到你的 **data\guess.js** 文件
 - 此 [new.js](https://raw.githubusercontent.com/Microsoft/Windows-tutorials-web/master/Single-Page-App-with-REST-API/backend/data/new.js?token=ACEfkiqeDN0HjZ4-gIKRh3wfVZPSlEmgks5ZFhYPwA%3D%3D) 代码到你的 **data\new.js** 文件

为简单起见，我们将我们的游戏板存储在 Node 服务器的全局变量 (`global.board`) 中。 但实际上，你使用云存储（如 Google [Cloud Datastore](https://cloud.google.com/datastore/) 或 Azure [DocumentDB](https://azure.microsoft.com/en-us/services/documentdb/)）来让这成为同时支持多个游戏和玩家的可行记忆游戏 API 服务。

请确保你已将所有更改保存在 VS 代码内，并再次启动服务器（VS 代码中按 F5，通过 shell 使用 `npm start`，然后浏览到 [http://localhost:8000](http://localhost:8000)）以测试游戏 API。

每次按**试用！** 按钮（**/game**、**/guess** 或 **/new** 操作其中一个）时，请检查生成的**响应正文**和**响应代码**（如下），以确认所有部分都按预期工作。

尝试： 

1. 创建新 `size=2` 游戏。

    ![从 Swagger UI 启动新的记忆游戏](images/swagger_new.png)

2. 猜测几个数值。

    ![从 Swagger UI 猜卡片](images/swagger_guess.png)

3. 游戏过程中检查游戏板。

    ![从 Swagger UI 检查游戏状态](images/swagger_game.png)

如果一切正常，API 服务已准备好在 Azure 上托管了！ 如果你遇到问题，请尝试在 \data\game.js 中注释掉以下行。

```javascript
for (var i=0; i < board.length; i++){
    var card = {};
    card.cleared = board[i].cleared;

    // Only reveal cleared card values
    //if ("true" == card.cleared) {        // To debug the board, comment out this line
        card.value = board[i].value;
    //}                                    // And this line
    currentBoardState.push(card);
}
```

通过此更改，**GET /game** 方法将返回所有卡片数值（包括尚未清除的数值）。 在你为记忆游戏生成前端时，这是可以保留的非常有用的调试方法。

### <a name="5-optional-host-your-api-service-on-azure-and-enable-cors"></a>5.（可选）在 Azure 上托管 API 服务和启用 CORS

Azure 文档将引导你完成：

 - [通过 Azure 门户注册新 *API 应用*](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-rest-api#createapiapp)
 - [为 API 应用设置 Git 部署](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-rest-api#deploy-the-api-with-git)，以及
 - [将 API 应用代码部署到 Azure](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-rest-api#deploy-the-api-with-git)

注册应用时，应尝试区分你的*应用名称*（以避免命名与 *http://memorygameapi.azurewebsites.net* URL 上的其他请求差异有冲突）。

如果你已做到这一步，且 Azure 现在已为你的 swagger UI 服务，那么距离记忆游戏后端只剩一个最后步骤了。 从 [Azure 门户](https://portal.azure.com)，选择你新建的*应用服务*，然后选择或搜索 **CORS**（跨来源资源共享）选项。 在**允许的来源**下，添加星号 (`*`)，然后单击**保存**。 这样，当你在本地计算机上开发游戏时，你可以通过你的记忆游戏 API 前端，对 API 服务执行跨来源调用。 在完成记忆游戏前端并将其部署到 Azure 后，你可以将此项替换为你的 Web 应用的特定 URL。

### <a name="going-further"></a>深入探索

若要使记忆游戏 API 成为生产应用的可行后端服务，你将需要扩展代码，以支持多个玩家和游戏。 为此，你可能需要为你的 API 探索[身份验证](http://swagger.io/docs/specification/authentication/)（用于管理玩家身份）、[NoSQL 数据库](https://docs.microsoft.com/en-us/azure/documentdb/)（用于跟踪游戏和玩家），以及一些基本[单元测试](https://apigee.com/about/blog/developer/swagger-test-templates-test-your-apis)的使用。

下面是一些帮助你深入探索的有用资源：

 - [使用 Visual Studio Code 的高级 Node.js 调试](https://code.visualstudio.com/docs/nodejs/nodejs-debugging)

 - [Azure Web + 移动文档](https://docs.microsoft.com/en-us/azure/#pivot=services&panel=web)

 - [Azure DocumentDB 文档](https://docs.microsoft.com/en-us/azure/documentdb/index)

## <a name="part-ii-build-a-single-page-web-application"></a>部分 II： 生成单页 web 应用程序

既然你已在部分 I 中生成（或[下载](https://github.com/Microsoft/Windows-tutorials-web/tree/master/Single-Page-App-with-REST-API/backend)）了 [REST API 后端](#part-i-build-a-rest-api-backend)，你已准备好使用 [Node](https://nodejs.org/en/)、[Express](http://expressjs.com/) 和 [Bootstrap](http://getbootstrap.com/) 创建单页记忆游戏前端了。

此教程的部分 II 将带给你以下经验： 

* [Node.js](https://nodejs.org/en/)：创建托管你的游戏的服务器
* [jQuery](http://jquery.com/)：JavaScript 库
* [Express](http://expressjs.com/)：Web 应用程序框架
* [Pug](https://pugjs.org/)：（之前的 Jade）用于模板引擎
* [Bootstrap](http://getbootstrap.com/)：用于响应布局
* [Visual Studio Code](https://code.visualstudio.com/)：用于代码编写、Markdown 查看和调试

### <a name="1-create-a-nodejs-application-by-using-express"></a>1. 使用 Express 创建 Node.js 应用程序

让我们从使用 Express 创建 Node.js 项目开始吧。

1. 打开命令提示符并导航到要存储你的游戏的目录。 
2. 使用 Express 生成器，使用模板引擎 *Pug* 创建名为*记忆*的新应用程序。

    ```
    express --view=pug memory
    ```

3. 在记忆目录中，安装 package.json 文件中列出的依赖项。 在你的项目的根中创建 package.json 文件。 此文件包含 Node.js 应用所需的模块。  

    ```
    cd memory
    npm install
    ```

    运行此命令之后，你应该看到名为 node_modules 的文件夹，其包含此应用所需要的所有模块。 

4. 现在，运行你的应用程序。

    ```
    npm start
    ```

5. 通过转到 [http://localhost:3000/](http://localhost:3000/) 查看你的应用程序。

    ![http://localhost:3000/ 的屏幕截图](./images/express.png)

6. 通过在 memory\routes 目录中编辑 index.js 来更改 Web 应用的默认标题。 将下方行中的 `Express` 更改为 `Memory Game`（或其他你选择的标题）。

    ``` javascript
    res.render('index', { title: 'Express' });
    ```

7. 若要刷新应用以查看你的新标题，在命令提示符中按 **Crtl + C**、**Y** 停止应用，然后使用 `npm start` 重启它。

### <a name="2-add-client-side-game-logic-code"></a>2. 添加客户端游戏逻辑代码
你可以在 [Start](https://github.com/Microsoft/Windows-tutorials-web/tree/master/Single-Page-App-with-REST-API/frontend/Start) 文件夹中找到你需要的这半部分教程的文件。 如果你找不到了，完成的代码可以在 [Final](https://github.com/Microsoft/Windows-tutorials-web/tree/master/Single-Page-App-with-REST-API/frontend/Final) 文件夹中找到。 

1. 从 [Start](https://github.com/Microsoft/Windows-tutorials-web/tree/master/Single-Page-App-with-REST-API/frontend/Start) 文件夹内复制 scripts.js，并将其粘贴到 memory\public\javascripts 中。 此文件包含运行游戏所需的所有游戏逻辑，包括：

    * 创建/启动新游戏。
    * 还原存储在服务器上的游戏。
    * 根据用户选择绘制游戏板和卡片。
    * 翻转卡片。

2. 在 script.js 中，让我们从修改 `newGame()` 函数开始。 此函数：

    * 处理用户选择的游戏的大小。
    * 从服务器获取[游戏板阵列](#part-i-build-a-rest-api-backend)。
    * 调用 `drawGameBoard()` 函数将游戏板放到屏幕上。

    在 `newGame()` 内在 `// Add code from Part 2.2 here` 注释后面添加以下代码。

    ``` javascript
    // extract game size selection from user
    var size = $("#selectGameSize").val();

    // parse game size value
    size = parseInt(size, 10);
    ```

    此代码使用 `id="selectGameSize"`（我们稍后创建）从下拉菜单中检索值，并将其存储在变量 (`size`) 中。  我们使用 [`parseInt()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/parseInt) 函数分析下拉菜单中的字符串值以返回整数，这样我们可以将请求的游戏板的 `size` 传递到服务器。 

    我们使用此教程部分 I 中创建的 [`/new`](#part-i-build-a-rest-api-backend) 方法将选择的游戏板的大小发布到服务器。 此方法返回卡片和 `true/false` 值的单个 JSON 阵列，指示卡片是否已匹配。 

3. 接下来，填充还原所玩的最后一个游戏的 `restoreGame()` 函数。 为简单起见，应用将始终加载玩的最后一个游戏。 如果服务器上没有存储的游戏，使用下拉菜单启动新的游戏。 

    将此代码复制并粘贴到 `restoreGame()`。

   ``` javascript 
   // reset the game
   gameBoardSize = 0;
   cardsFlipped = 0;

   // fetch the game state from the server 
   $.get("http://localhost:8000/game", function (response) {
       // store game board size
       gameBoardSize = response.length;

       // draw the game board
       drawGameBoard(response);
   });
   ```

    现在，游戏将从服务器提取游戏状态。 有关此步骤使用的 [`/game`](#part-i-build-a-rest-api-backend) 方法的详细信息，请参阅此教程的部分 I。 如果你使用 Azure（或其他服务）来托管后端 API，请将上述 *localhost* 地址替换为生产 URL。

4. 现在，我们需要创建 `drawGameBoard()` 函数。  此函数：

    * 检测游戏的大小，并应用特定的 CSS 样式。
    * 在 HTML 中生成卡片。
    * 将卡片添加到页面。

    将此代码复制并粘贴到 *scripts.js* 的 `drawGameBoard()` 函数中：

    ``` javascript
    // create output
    var output = "";
    // detect board size CSS class
    var css = "";
    switch (board.length / 4) {
        case 1:
            css = "rows1";
            break;
        case 2:
            css = "rows2";
            break;
        case 3:
            css = "rows3";
            break;
        case 4:
            css = "rows4";
            break;   
    }
    // generate HTML for each card and append to the output
    for (var i = 0; i < board.length; i++) {
        if (board[i].cleared == "true") {
            // if the card has been cleared apply the .flip class
            output += "<div class=\"flipContainer col-xs-3 " + css + "\"><div class=\"cards flip matched\" id=\"" + i + "\" onClick=\"flipCard(this)\">\
                <div class=\"front\"><span class=\"glyphicon glyphicon-question-sign\"></span></div>\
                <div class=\"back\">" + lookUpGlyphicon(board[i].value) + "</div>\
                </div></div>";
        } else {
            output += "<div class=\"flipContainer col-xs-3 " + css + "\"><div class=\"cards\" id=\"" + i + "\" onClick=\"flipCard(this)\">\
                <div class=\"front\"><span class=\"glyphicon glyphicon-question-sign\"></span></div>\
                <div class=\"back\"></div>\
                </div></div>";
        }
    }
    // place the output on the page
    $("#game-board").html(output);
    ```

5. 接下来，我们需要完成 `flipCard()` 函数。  此函数处理大部分游戏逻辑，包括使用此教程部分 I 中开发的 [`/guess`](#part-i-build-a-rest-api-backend) 方法，从服务器获取所选卡片的数值。 如果你使用托管 REST API 后端的云，不要忘记将 *localhost* 地址替换为你的生产 URL。

    在 `flipCard()` 函数中，取消此代码的注释：

    ``` javascript
    // post this guess to the server and get this card's value
    $.ajax({
        url: "http://localhost:8000/guess?card=" + selectedCards[0],
        type: 'PUT',
        success: function (response) {
            // display first card value
            $("#" + selectedCards[0] + " .back").html(lookUpGlyphicon(response[0].value));

            // store the first card value
            selectedCardsValues.push(response[0].value);
        }
    });
    ```

> [!TIP] 
> 如果你使用 Visual Studio Code，选择所有你希望取消注释的代码行，然后按 Crtl + K、U

我们在这里使用的是部分 I 中创建的 [`jQuery.ajax()`](http://api.jquery.com/jQuery.ajax/) 和 **PUT** [`/guess`](#part-i-build-a-rest-api-backend) 方法。 

此代码按以下顺序执行。

* 用户选择的第一个卡片的 `id` 被作为第一个值添加到 selectedCards[] 阵列： `selectedCards[0]` 
* `selectedCards[0]` 中的值 (`id`) 使用 [`/guess`](#part-i-build-a-rest-api-backend) 方法发布到服务器
* 服务器响应该卡片的 `value`（整数）
* [Bootstrap glyphicon](http://getbootstrap.com/components/) 添加到卡片的 `id` 所在的卡片背面 `selectedCards[0]`
* 第一个卡片的 `value`（来自服务器）存储在 `selectedCardsValues[]` 阵列中： `selectedCardsValues[0]`。 

用户的第二次猜测遵循相同的逻辑。 如果用户所选的卡片具有相同的 ID（例如 `selectedCards[0] == selectedCards[1]`），则卡片匹配！ CSS 类 `.matched` 添加到匹配的卡片（变为绿色），卡片保持翻转状态。

现在，我们需要添加逻辑，以检查用户是否匹配了所有卡片并在游戏中胜出。 在 `flipCard()` 函数内，在 `//check if the user won the game` 注释下添加下面的代码行。 

``` javascript
if (cardsFlipped == gameBoardSize) {
    setTimeout(function () {
        output = "<div id=\"playAgain\"><p>You Win!</p><input type=\"button\" onClick=\"location.reload()\" value=\"Play Again\" class=\"btn\" /></div>";
        $("#game-board").html(output);
    }, 1000);
}   
```

如果翻转的卡片数量与游戏板大小相同（例如，`cardsFlipped == gameBoardSize`），则没有更多卡片要翻转，用户已在游戏中胜出。 我们会使用 `id="game-board"` 将一些简单的 HTML 添加到 `div`，以让用户知道他们已胜出，可以重玩游戏。  

### <a name="3-create-the-user-interface"></a>3. 创建用户界面 
现在，我们来通过创建用户界面，看看正在操作的所有代码。 在此教程中，我们使用模板引擎 [Pug](https://pugjs.org/)（正式 Jade）。  *Pug* 是编写 HTML 的对空格敏感的洁净语言。 下面提供了一个示例。 

```
body
    h1 Memory Game
    #container
        p We love tutorials!
```

成为

``` html
<body>
    <h1>Memory Game</h1>
    <div id="container">
        <p>We love tutorials!</p>
    </div>
</body>
```


1. 将 memory\views 中的 layout.pug 文件替换为 Start 文件夹中提供的 layout.pug 文件。 在 layout.pug 内，你将看到以下链接：

    * Bootstrap
    * jQuery
    * 自定义 CSS 文件
    * 我们刚才完成修改的 JavaScript 文件

2. 打开目录 memory\views 中的名为 index.pug 的文件。
此文件扩展 layout.pug 文件，并将呈现我们的游戏。 在 layout.pug 内，粘贴下面的代码行：

    ```
    extends layout
    block content  
        div
            form(method="GET")
            select(id="selectGameSize" class="form-control" onchange="newGame()")
                option(value="0") New Game
                option(value="2") 2 Matches
                option(value="4") 4 Matches
                option(value="6") 6 Matches
                option(value="8") 8 Matches         
            #game-board
                script restoreGame();
    ```

> [!TIP] 
> 请记住：Pug 对空格敏感。 请确保你的所有缩进都正确！

### <a name="4-use-bootstraps-grid-system-to-create-a-responsive-layout"></a>4. 使用 Bootstrap 的网格系统创建响应布局
Bootstrap 的[网格系统](http://getbootstrap.com/css/#grid)是一个动态网格系统，通过设备的视区更改来扩展网格。 此游戏中的卡片其布局使用 Bootstrap 的预定义网格系统类，包括：
* `.container-fluid`: 为网格指定动态容器
* `.row-fluid`: 指定动态行
* `.col-xs-3`: 指定列数

Bootstrap 的网格系统允许网格系统折叠为一个垂直列，就像你在移动设备的导航菜单上看到的。  但是，因为我们想要我们的游戏始终包含列，我们使用预定义的类 `.col-xs-3`，这会让网格始终保持水平。 

网格系统最多允许有 12 列。 由于我们只希望我们的游戏中有 4 列，所以我们使用类 `.col-xs-3`。 此类规定我们需要使用每个列来跨越之前提到的 12 个可用列中 3 个列的宽度。 此图像显示 12 列网格和 4 列网格，就像此游戏中使用的网格。

![12 列和 4 列 Bootstrap 网格](./images/grid.png)

1. 打开 scripts.js 并找到 `drawGameBoard()` 函数。  在我们为每个卡片生成 HTML 的代码块中，你能否使用 `class="col-xs-3"` 找到 `div` 元素？ 

2. 在 index.pug 内，让我们来添加之前提到的预定义 Bootstrap 类，以创建我们的动态布局。  将 index.pug 更改为以下项。

    ```
    extends layout

    block content   

        .container-fluid
            form(method="GET")
            select(id="selectGameSize" class="form-control" onchange="newGame()")
                option(value="0") New Game
                option(value="2") 2 Matches
                option(value="4") 4 Matches
                option(value="6") 6 Matches
                option(value="8") 8 Matches         
            #game-board.row-fluid 
                script restoreGame();
    ```

### <a name="5-add-a-card-flip-animation-with-css-transforms"></a>5. 使用 CSS 变换添加卡片翻转动画
将 memory\public\stylesheets 中的 style.css 文件替换为 Start 文件夹中的 style.css 文件。

使用 [CSS 变换](https://docs.microsoft.com/en-us/microsoft-edge/dev-guide/css/transforms)添加翻转动画将让卡片呈现逼真的 3D 翻转移动。 游戏中的卡片通过使用以下 HTML 结构创建，并以编程方式添加到游戏板（在之前显示的 `drawGameBoard()` 函数中）。

``` html
<div class="flipContainer">
    <div class="cards">
        <div class="front"></div>
        <div class="back"></div>
    </div>
</div>
```

1. 若要开始，为动画的父容器提供[视角](https://developer.mozilla.org/en-US/docs/Web/CSS/transform) (`.flipContainer`)。  这为其子元素提供立体感效应：数值越大，元素显示的位置距离用户越远。 我们来为 style.css 中的 `.flipContainer` 类添加以下视角。

    ``` css
    perspective: 1000px; 
    ```

2. 现在，将以下属性添加到 style.css 中的 `.cards` 类。 `.cards``div` 是实际上执行翻转动画、显示卡片正面或背面的元素。 

    ``` css
    transform-style: preserve-3d;
    transition-duration: 1s;
    ```

    [`transform-style`](https://developer.mozilla.org/en-US/docs/Web/CSS/transform-style) 属性建立 3D 呈现上下文，`.cards` 类（`.front` 和 `.back`）的子项是 3D 空间的成员。 添加 [`transition-duration`](https://developer.mozilla.org/en-US/docs/Web/CSS/transition-duration) 属性可以指定动画完成的秒数。 

3.  使用 [`transform`](https://developer.mozilla.org/en-US/docs/Web/CSS/transform) 属性，我们可以围绕 Y 轴旋转卡片。  将以下 CSS 添加到 `cards.flip`。

    ``` css
    transform: rotateY(180deg);
    ```

    `cards.flip` 中定义的样式在 `flipCard` 函数中使用 [`.toggleClass()`](http://api.jquery.com/toggleClass/) 打开和关闭。 

    ``` javascript
    $(card).toggleClass("flip");
    ```

    现在，当用户单击卡片，卡片将旋转 180 度。

### <a name="6-test-and-play"></a>6. 测试和玩游戏
恭喜你！ 你已完成了 Web 应用的创建！ 我们来测试一下。 

1. 在记忆目录中打开命令提示符，然后输入以下命令： `npm start`

2. 在你的浏览器中，转到 [http://localhost:3000/](http://localhost:3000/)，玩游戏吧！

3. 如果你遇到任何错误，你可以通过在键盘上按 F5 并键入 `Node.js` 来使用 Visual Studio Code 的 Node.js 调试工具。 有关 Visual Studio Code 的调试的详细信息，请查看此[文章](http://code.visualstudio.com/docs/editor/debugging#_launch-configurations)。 

    你还可以将你的代码与 Final 文件夹中提供的代码比较。

4. 若要停止游戏，在命令提示符中键入：**Ctrl + C**，**Y**。 

### <a name="going-further"></a>深入探索

现在你可以将你的应用部署到 Azure（或任何其他云托管服务），以跨不同设备外形进行测试，如手机、平板电脑和台式机。 （另外不要忘记跨不同浏览器进行测试！）你的应用准备好生产后，你可以将其轻松打包为*通用 Windows 平台* (UWP) 的*托管 Web 应用* (HWA)，从 Microsoft Store 分发。

发布到 Microsoft Store 的基本步骤是：

 1. 创建 [Windows 开发人员](https://developer.microsoft.com/en-us/store/register)帐户
 2. 使用应用提交[清单](https://docs.microsoft.com/en-us/windows/uwp/publish/app-submissions)
 3. 提交你的应用进行[认证](https://msdn.microsoft.com/windows/uwp/publish/the-app-certification-process)

下面是一些帮助你深入探索的有用资源：

 - [将你的应用程序开发项目部署到 Azure 网站](https://docs.microsoft.com/azure/cosmos-db/documentdb-nodejs-application#_Toc395783182)

 - [将 Web 应用程序转换为通用 Windows 平台 (UWP) 应用](https://docs.microsoft.com/en-us/windows/uwp/porting/hwa-create-windows)

 - [发布 Windows 应用](https://developer.microsoft.com/en-us/store/publish-apps)
