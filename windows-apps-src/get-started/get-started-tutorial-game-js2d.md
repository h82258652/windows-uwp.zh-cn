---
title: 用 JavaScript 创建 UWP 游戏
description: 简单的 UWP 游戏的 Microsoft 应用商店、 用 JavaScript 和 CreateJS 编写
author: GrantMeStrength
ms.author: jken
ms.date: 02/09/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 01af8254-b073-445e-af4c-e474528f8aa3
ms.localizationpriority: medium
ms.openlocfilehash: 597451826958c355dad9f9380dbdc1264bc87883
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/02/2018
ms.locfileid: "5943732"
---
# <a name="create-a-uwp-game-in-javascript"></a>用 JavaScript 创建 UWP 游戏

## <a name="a-simple-2d-uwp-game-for-the-microsoft-store-written-in-javascript-and-createjs"></a>Microsoft Store 中用 JavaScript 和 CreateJS 编写的简单 2D UWP 游戏


![行走的恐龙的子画面表](images/JS2D_1.png)


## <a name="introduction"></a>简介


应用发布到 Microsoft 应用商店意味着你可以将其共享 （或其出售 ！） 与数百万人，许多不同设备上。  

若要将应用发布到 Microsoft Store，必须将其编写为 UWP （通用 Windows 平台） 应用。 不过，UWP 非常灵活，并且支持多种语言和框架。 为了证明这一点，以下示例为用 JavaScript 编写并且使用了多个 CreateJS 库的简单游戏，它展示了如何绘制子画面、创建游戏循环、支持键盘和鼠标以及适用于不同的屏幕尺寸。

此项目由使用 Visual Studio 的 JavaScript 创建。 除了一些细微差别外，还可将其托管在网站上，或者可对其进行调整以适应其他平台。 

**注意：** 这不是完整 （或好 ！） 游戏;它旨在展示使用 JavaScript 和第三方库将应用发布到 Microsoft 应用商店的准备工作。


## <a name="requirements"></a>要求

为了运行此项目，你需要具备以下条件：

* 一台运行当前版本 Windows 10 的 Windows 计算机（或虚拟机）。
* 一份 Visual Studio 副本。 可以从 [Visual Studio 主页](http://visualstudio.com) 下载免费的社区版 Visual Studio。

此项目使用 CreateJS JavaScript 框架。 CreateJS 是一组使用 MIT 许可证发布的免费工具，设计用于轻松创建基于子画面的游戏。 此项目已包含 CreateJS 库（在解决方案资源管理器视图中查找 *js/easeljs-0.8.2.min.js* 和 *js/preloadjs-0.6.2.min.js*）。 有关 CreateJS 的更多信息，请查看 [CreateJS 主页](http://www.createjs.com)。


## <a name="getting-started"></a>入门

此应用的完整源代码存储于 [GitHub](https://github.com/Microsoft/Windows-appsample-get-started-js2d)中。

最简单的使用方法是访问 GitHub，单击绿色的**克隆或下载**按钮，然后选择**在 Visual Studio 中打开**。 

![克隆存储库](images/JS2D_2.png)

也可以将项目下载为 zip 文件，或者通过任何其他标准方式使用 [GitHub 项目](https://msdn.microsoft.com/en-us/windows/uwp/get-started/get-uwp-app-samples)。

解决方案加载到 Visual Studio 后，你会看到多个文件，包括：

* Images/ - 包含 UWP 应用所需的各种图标、游戏的 SpriteSheet 和某些其他位图的文件夹。
* js/ - 包含 JavaScript 文件的文件夹。 main.js 文件为我们的游戏，其他文件为 EaselJS 和 PreloadJS。
* index.html - 包含用于托管游戏图形的画布对象的网页。.

现在，你可以运行此游戏！

按 **F5** 即可开始运行此应用。 你应看到一个打开的窗口，我们熟悉的恐龙站立于旖旎风光 （如果稀疏） 中。 现在，我们将测试该应用、解释某些重要部分并在进行的过程中探索其他功能。

![只是一个普通的恐龙，一只忍者神猫坐在其背部](images/JS2D_3.png)

**注释：** 出现了错误？ 请确保已安装 Visual Studio 及 Web 支持。 可以通过新建项目来检查是否支持 JavaScript，你需要重新安装 Visual Studio 并选中 *Microsoft Web 开发人员工具*复选框。

## <a name="walkthough"></a>操作实例

如果通过 F5 开始游戏，则可能会想知道发生了什么。 答案是"并不多"，因为许多代码当前被注释掉。到目前为止，你将看到所有是只有恐龙以及按空格键所请求。 

### <a name="1-setting-the-stage"></a>1. 设置舞台

如果打开并测试 **index.html**，则你将会看到它几乎是空的。 此文件为包含我们应用的默认网页，并且它只负责两项重要工作。 第一，它包括适用于 **EaselJS** 和 **PreloadJS** CreateJS 库的 JavaScript 源代码以及 **main.js**（我们自己的源代码文件）。
第二，它可定义&lt;画布&gt;标签，我们所有的图形均会显示在画布中。 &lt;画布&gt;是标准的 HTML5 文档组件。 我们将其命名为游戏画布，从而使 **main.js** 中的代码可以引用它。 顺便说一下，如果你要从头开始编写自己的 JavaScript 游戏，则你还需要将 **EaselJS** 和 **PreloadJS** 文件复制到你的解决方案，然后再创建画布对象。

EaselJS 可以为我们提供一个被称为*舞台*的新对象。 该舞台链接至画布，可用于显示图像和文本。 我们想要在此舞台上显示的任何对象都必须首先添加为子舞台，如下所示：

```
    stage.addChild(myObject);
```

你将看到代码行会在 **main.js** 中多次显示

现在就是打开 **main.js** 的好时机。

### <a name="2-loading-the-bitmaps"></a>2. 加载位图

EaselJS 可以为我们提供几种不同类型的图形对象。 我们可以创建简单的形状（如用于天空的蓝色矩形）或位图（例如我们将要添加的云彩）、文本对象和子画面。 子画面使用 (SpriteSheet) [http://createjs.com/docs/easeljs/classes/SpriteSheet.html]: 一个包含多个图像的单一位图。 例如，我们使用此 SpriteSheet 来存储恐龙动画的不同帧：

![行走的恐龙的子画面表](images/JS2D_4.png)

我们可以通过定义不同的帧以及此代码下的动画播放速度来让恐龙行走：

```
    // Define the animated dino walk using a spritesheet of images,
    // and also a standing-still state, and a knocked-over state.
    var data = {
        images: [loader.getResult("dino")],
        frames: { width: 373, height: 256 },
        animations: {
            stand: 0,
            lying: { 
                frames: [0, 1],
                speed: 0.1
            },
            walk: {
                frames: [0, 1, 2, 3, 2, 1],
                speed: 0.4
            }
        }
    }

    var spriteSheet = new createjs.SpriteSheet(data);
    dino_walk = new createjs.Sprite(spriteSheet, "walk");
    dino_stand = new createjs.Sprite(spriteSheet, "stand");
    dino_lying = new createjs.Sprite(spriteSheet, "lying");

```

现在，我们将为此舞台添加一些蓬松的云彩。 运行游戏后，它们将会在屏幕上飘动。 云彩图像已位于解决方案的 *images* 文件夹中。

查看 **main.js**，直到你找到 **init()** 函数。 游戏开始时将会调用此函数，并且我们将通过此函数来设置所有图形对象。

找到以下代码，然后将注释 (\\) 从引用此云彩图像的行中删除。

```
 manifest = [
        { src: "walkingDino-SpriteSheet.png", id: "dino" },
        { src: "barrel.png", id: "barrel" },
        { src: "fluffy-cloud-small.png", id: "cloud" },
    ];
```

加载资源（如图像）时，JavaScript 需要一些帮助，因此，我们使用可预加载图像的 CreateJS 库的一项被称为 [LoadQueue](http://www.createjs.com/docs/preloadjs/classes/LoadQueue.html) 的功能。 我们不确定加载图像需要花费多长时间，因此，我们使用 LoadQueue 来解决此问题。 图像可用后，队列将会通知我们它们已准备就绪。 为此，我们先创建一个列出所有图像的新对象，然后再创建一个 LoadQueue 对象。 在以下代码中，你将会看到当一切准备就绪时它会如何调用一个名为 **loadingComplete()** 的函数。

```
    // Now we create a special queue, and finally a handler that is
    // called when they are loaded. The queue object is provided by preloadjs.

    loader = new createjs.LoadQueue(false);
    loader.addEventListener("complete", loadingComplete);
    loader.loadManifest(manifest, true, "../images/");
```    

调用 **loadingComplete()** 函数后，图像已完成加载并且可供使用。 你将会看到一个用于创建云彩的被注释掉的部分，现在，其位图已可用。 删除注释，使其如下所示：

```
    // Create some clouds to drift by..
    for (var i = 0; i < 3; i++) {
        cloud[i] = new createjs.Bitmap(loader.getResult("cloud"));
        cloud[i].x = Math.random()*1024; // Random start location
        cloud[i].y = 64 + i * 48;
        stage.addChild(cloud[i]);
    }
```
此代码可以创建三个云彩对象（每个对象均使用预载图像），并定义其位置，然后将其添加到舞台。

再次运行此应用（按 F5），你将会看到显示出来的云彩。

### <a name="3-moving-the-clouds"></a>3. 移动云彩

现在，我们将让云彩移动。 实际上，移动云彩以及任何对象的秘密在于设置 [ticker](http://www.createjs.com/docs/easeljs/classes/Ticker.html) 函数，该函数将在一秒内被反复调用多次。 每调用一次该函数，就会在略有不同的位置重新绘制图像。

<p data-height="500" data-theme-id="23761" data-slug-hash="vxZVRK" data-default-tab="result" data-user="MicrosoftEdgeDocumentation" data-embed-version="2" data-pen-title="CreateJS - Animating clouds" data-preview="true" data-editable="true" class="codepen">请参阅 Pen <a href="https://codepen.io/MicrosoftEdgeDocumentation/pen/vxZVRK/">CreateJS - 为云彩添加动画效果</a>，Microsoft Edge 文档 (<a href="http://codepen.io/MicrosoftEdgeDocumentation">@MicrosoftEdgeDocumentation</a>)（位于 <a href="http://codepen.io">CodePen</a>）。</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>
 
执行该操作的代码已位于 **main.js** 文件中，由 CreateJS 库 EaselJS 提供。 显示如下：

```
    // Set up the game loop and keyboard handler.
    // The keyword 'tick' is required to automatically animated the sprite.
    createjs.Ticker.timingMode = createjs.Ticker.RAF;
    createjs.Ticker.addEventListener("tick", gameLoop);
```

此代码将在一秒内调用 30-60 帧之间的 **gameLoop()** 函数。 准确的速度取决于计算机的速度。

查找 **gameLoop()** 函数，你将会在最末尾处看到名为 **animateClouds()** 的函数。 对其进行编辑，使其不被注释掉。

```
    // Move clouds
    animateClouds();
```

如果你查看此函数的定义，那么你将会了解它是如何依次获取每朵云彩以及这些云彩是如何改变其 x 坐标的。 如果 x 坐标隐藏在屏幕侧面之外，则需要将其重置为最右边。 每朵云彩的移动速度稍有不同。

```
function animate_clouds()
{
    // Move the cloud sprites across the sky. If they get to the left edge, 
    // move them over to the right.

    for (var i = 0; i < 3; i++) {    
        cloud[i].x = cloud[i].x - (i+1);
        if (cloud[i].x < -128)
            cloud[i].x = width + 128;
    }
}
```

如果现在运行此应用，则你将会看到云彩已开始飘动。 我们终于完成了云彩移动！

### <a name="4-adding-keyboard-and-mouse-input"></a>4. 添加键盘和鼠标输入

一款无法与之互动的游戏不能被称为真正的游戏。 因此，我们需要使玩家能够使用键盘或鼠标进行操作。 返回 **loadingComplete()** 函数，你将会看到以下内容。 删除注释。

```
    // This code will call the method 'keyboardPressed' is the user presses a key.
    this.document.onkeydown = keyboardPressed;

    // Add support for mouse clicks
    stage.on("stagemousedown", mouseClicked);
```

现在，只要玩家点击按键或点击鼠标，我们就会调用两个函数。 两个事件均会调用 **userDidSomething()**，该函数通过查找游戏状态变量来决定现在进行的是什么游戏以及接下来需要发生的事情。

游戏状态是游戏中使用的常见设计模式。 所有一切均通过打点计时器调用的 **gameLoop()** 函数来进行。 gameLoop() 通过一个变量跟踪游戏是正在进行中，还是处于“游戏结束状态”或“准备就绪状态”或是创作者定义的任何其他状态。 此状态变量已在交换语句中经过测试，它用于定义调用了哪些其他函数。 因此，如果将状态设为“进行中”，则会调用用于使恐龙跳跃和让桶滚动的函数。 如果恐龙被杀死，则游戏状态变量将设为“游戏结束状态”，此时将会显示 消息“游戏结束！”。 如果你对游戏设计模式感兴趣，那么[游戏编程模式](http://gameprogrammingpatterns.com/)一书将对你非常有用。

再次尝试运行此应用，你最终将能够开始游戏。 按空格键（或单击鼠标或点击屏幕）继续游戏。 

你将会看到一只桶向你滚过来：在合适的时间再次按空格键或单击时，恐龙将会跳跃。 操作时间不对的话，游戏就会结束。

桶的动画方式与云彩相同（尽管它每次都会加快速度），我们可以通过检查恐龙与桶之间的相对位置来确保它们不会碰撞在一起：

```
 // Very simple check for collision between dino and barrel
                if ((barrel.x > 220 && barrel.x < 380)
                    &&
                    (!jumping))
                {
                    barrel.x = 380;
                    GameState = GameStateEnum.GameOver;
                }
```

如果恐龙未跳跃且桶就在附近，则代码会将状态变量更改为我们所称的*游戏结束*。 顾名思义，*游戏结束*将停止游戏。

至此，我们主要的游戏技巧介绍完毕。

### <a name="5-resizing-support"></a>5. 调整大小支持

我们马上就快讲完了！ 但在结束之前，我们还需要解决一个恼人的问题。 游戏正在进行时，尝试调整窗口大小。 你将会看到游戏会快速变得非常混乱，因为各对象不再位于其预期的位置。 我们可通过以下方法解决此问题：创建一个处理程序，用于在玩家调整窗口大小或将设备从横向旋转为纵向时生成窗口大小调整事件。

用于执行此操作的代码已存在（实际上，我们在游戏首次启动时已调用过此代码，以确保默认的窗口大小有效，因为启动 UWP 应用时，你无法确定窗口的实际尺寸）。

只需对此行取消注释即可在触发屏幕大小调整事件时调用此函数：

```
    // This code makes the app call the method 'resizeGameWindow' if the user resizes the current window.
     window.addEventListener('resize', resizeGameWindow);
```

如果再次运行此应用，则现在你应该可以调整窗口大小并获得更好的效果。

## <a name="publishing-to-the-microsoft-store"></a>发布到 Microsoft 应用商店

现在你有一个 UWP 应用，则可以将其发布到 Microsoft Store （假设已先了改进 ！） 

此流程包含几个步骤。

1. 你必须以 Windows 开发人员的身份[注册](https://developer.microsoft.com/en-us/store/register)。
2. 你必须使用应用提交[清单](https://msdn.microsoft.com/windows/uwp/publish/app-submissions)。
3. 必须将此应用提交以进行[认证](https://msdn.microsoft.com/windows/uwp/publish/the-app-certification-process)。

有关详细信息，请参阅[发布你的 UWP 应用](https://developer.microsoft.com/en-us/store/publish-apps)。

## <a name="suggestions-for-other-features"></a>其他功能建议。

接下来怎么做？ 以下是一些可添加到你的（未来的）获奖应用中的功能建议。

1. 声音效果。 CreateJS 库包括声音支持，它具有一个名为 [SoundJS](http://www.createjs.com/soundjs) 的库。
2. 游戏板支持。 提供有一个 [可用的 API](https://gamedevelopment.tutsplus.com/tutorials/using-the-html5-gamepad-api-to-add-controller-support-to-browser-games--cms-21345)。
3. 让我们打造一款更棒的游戏！ 这一部分由你决定，网上也提供有许多的资源。 

## <a name="other-links"></a>其他链接

* [使用 JavaScript 制作一款简单的 Windows 游戏](https://www.sitepoint.com/creating-a-simple-windows-8-game-with-javascript-game-basics-createjseaseljs/)
* [选取一款 HTML/JS 游戏引擎](https://html5gameengine.com/)
* [在基于 JS 的游戏中使用 CreateJS](https://blogs.msdn.microsoft.com/cbowen/2012/09/19/using-createjs-in-your-javascript-based-windows-8-game/)
* [LinkedIn Learning 上的游戏开发课程](https://www.linkedin.com/learning/topics/game-development)
