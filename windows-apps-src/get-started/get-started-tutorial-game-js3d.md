---
title: 入门教程 - 用 JavaScript 编写的 3D UWP 游戏
description: 适用于 Microsoft 应用商店、 用 JavaScript 及 three.js 编写的 UWP 游戏
author: abbycar
ms.author: abigailc
ms.date: 03/06/2017
ms.topic: article
keywords: windows 10，uwp
ms.assetid: fb4249b2-f93c-4993-9e4d-57a62c04be66
ms.localizationpriority: medium
ms.openlocfilehash: 0183e19135758f73dfea9b63535437ff9b66011a
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/03/2018
ms.locfileid: "5996146"
---
# <a name="creating-a-3d-javascript-game-using-threejs"></a>使用 three.js 创建 3D JavaScript 游戏

## <a name="introduction"></a>简介

对于 Web 开发人员或 JavaScript 工程师，使用 JavaScript 开发 UWP 应用是一种可以将你的应用轻松推向世界的方法。 无需担心需要学习诸如 C# 或 C++ 之类的语言！

在本示例中，我们将充分利用 **three.js** 库。 此库基于 WebGL 生成，后者是一个 API，用于在 Web 浏览器中呈现 2D 和 3D 图形。 **three.js** 采用了此复杂的 API 并对其进行了简化，从而使得 3D 开发更加简单。 


继续阅读之前，想要先行浏览一下我们即将制作的应用？ 可以在 CodePen 上查阅！

<iframe height='300' scrolling='no' title='恐龙游戏终极' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/NpKejy/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>请访问 <a href='https://codepen.io'>CodePen</a>，查看由 Microsoft Edge Docs (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) 提供支持的 Pen <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/NpKejy/'>恐龙游戏终极版</a>。
</iframe>

> [!NOTE] 
> 这不是完整的游戏;它旨在展示使用 JavaScript 和第三方库将应用发布到 Microsoft 应用商店的准备工作。


## <a name="requirements"></a>要求

为了运行此项目，你需要具备以下条件：
-   一台运行当前版本 Windows 10 的 Windows 计算机（或虚拟机）。
-   一份 Visual Studio 副本。 可以从 [Visual Studio 主页](http://visualstudio.com/) 下载免费的社区版 Visual Studio。
此项目使用了 **three.js** JavaScript 库。 **three.js** 使用 MIT 许可证发布。 此项目已包含该库（在解决方案资源管理器视图中查找 `js/libs`）。 有关此库的更多信息，请参阅 [**three.js**](https://threejs.org/) 主页。

## <a name="getting-started"></a>入门

此应用的完整源代码存储于 [GitHub](https://github.com/Microsoft/Windows-appsample-get-started-js3d)中。

最简单的入门方法是访问 GitHub，单击绿色的“克隆”或“下载”按钮，然后选择在 Visual Studio 中打开。 

![克隆或下载按钮](images/3dclone.png)

如果不想克隆项目，则可以将其下载为 zip 文件。
解决方案加载到 Visual Studio 后，你会看到多个文件，包括：
-   Images/ - 包含 UWP 应用所需的各种图标的文件夹。
- css/ - 包含要使用的 CSS 的文件夹。
-   js/ - 包含 JavaScript 文件的文件夹。 main.js 文件为我们的游戏，而其他文件为第三方库。
-   models/ - 包含 3D 模型的文件夹。 在本游戏中，我们只有一个恐龙 3D 模型。
-   index.html - 用于托管游戏呈现器的网页。

现在，你可以运行此游戏！

按 F5 开始运行应用。 你应该会看到一个打开的窗口，提示你单击屏幕。 你还将看到一只恐龙在背景中四处移动。 继续并关闭游戏，我们将开始测试此应用及其关键组件。

> [!NOTE] 
> 出现了错误？ 请确保已安装 Visual Studio 及 Web 支持。 可以通过新建项目来检查是否支持 JavaScript，你需要重新安装 Visual Studio 并选中 Microsoft Web 开发人员工具复选框。

## <a name="walkthrough"></a>操作实例

当启动此游戏时，你将会看到一个在屏幕上单击的提示。 [指针锁 API](https://developer.mozilla.org/docs/Web/API/Pointer_Lock_API) 用于使你能够通过鼠标环顾四周。 通过按 W、A、S、D/ 箭头键完成移动。
此游戏的目标是远离恐龙。 当恐龙离你足够近时，它将会追踪你，直到你退出有效范围，或者因太靠近而输掉游戏。

### <a name="1-setting-up-your-initial-html-file"></a>1. 设置初始 HTML 文件

在 **index.html** 内，你需要添加一些 HTML 才可开始使用。 此文件是包含我们应用的默认网页。

现在，我们需要将其设置为具有我们将使用的库以及用于呈现图形的 `div`（名为 `container`）。 我们还需要将其设置为指向我们的 **main.js**（我们的游戏代码）。


```html
<!DOCTYPE html>
<html lang='en'>

<head>
    <link rel="stylesheet" type="text/css" href="css/stylesheet.css" />
</head>

    <body>
        <div id='container'></div>
        <script src='js/libs/three.js'></script>
        <script src="js/controls/PointerLockControls.js"></script>
        <script src="js/main.js"></script>
    </body>

</html>
```


既然我们已准备好初学者 HTML，那就让我们转到 **main.js** 并制作一些图形吧！

### <a name="2-creating-your-scene"></a>2. 创建场景

在操作实例部分，我们将会添加游戏的基础知识。

我们将从详细说明 `scene` 开始。 你可以在 **three.js** 的 `scene` 中添加摄像头、对象和光。 你还需要一个呈现器，以便将你在摄像头中看到的内容呈现在场景中并显示出来。

在 **main.js** 中，我们将会制作一个名为 `init()` 的函数，该函数用于完成以上操作，并且会调用某些其他函数：

```javascript
var UNITWIDTH = 90; // Width of a cubes in the maze
var UNITHEIGHT = 45; // Height of the cubes in the maze

var camera, scene, renderer;

init();
animate();

function init() {
    // Create the scene where everything will go
    scene = new THREE.Scene();

    // Add some fog for effects
    scene.fog = new THREE.FogExp2(0xcccccc, 0.0015);

    // Set render settings
    renderer = new THREE.WebGLRenderer();
    renderer.setClearColor(scene.fog.color);
    renderer.setPixelRatio(window.devicePixelRatio);
    renderer.setSize(window.innerWidth, window.innerHeight);

    // Get the HTML container and connect renderer to it
    var container = document.getElementById('container');
    container.appendChild(renderer.domElement);

    // Set camera position and view details
    camera = new THREE.PerspectiveCamera(60, window.innerWidth / window.innerHeight, 1, 2000);
    camera.position.y = 20; // Height the camera will be looking from
    camera.position.x = 0;
    camera.position.z = 0;

    // Add the camera
    scene.add(camera);

    // Add the walls(cubes) of the maze
    createMazeCubes();

    // Add lights to the scene
    addLights();

    // Listen for if the window changes sizes and adjust
    window.addEventListener('resize', onWindowResize, false);
}

```

需要创建的其他函数包括：
- `createMazeCubes()`
- `addLights()`
- `onWindowResize()`
- `animate()` / `render()`
- 单位转换函数

#### <a name="createmazecubes"></a>createMazeCubes()

`createMazeCubes()` 函数将为我们的场景添加简单的立方体。 接着，我们将制作一个用于为我们的迷宫添加多个立方体的函数。

```javascript
function createMazeCubes() {

  // Make the shape of the cube that is UNITWIDTH wide/deep, and UNITHEIGHT tall
  var cubeGeo = new THREE.BoxGeometry(UNITWIDTH, UNITHEIGHT, UNITWIDTH);
  // Make the material of the cube and set it to blue
  var cubeMat = new THREE.MeshPhongMaterial({
    color: 0x81cfe0,
  });
  
  // Combine the geometry and material to make the cube
  var cube = new THREE.Mesh(cubeGeo, cubeMat);

  // Add the cube to the scene
  scene.add(cube);

  // Update the cube's position
  cube.position.y = UNITHEIGHT / 2;
  cube.position.x = 0;
  cube.position.z = -100;
  // rotate the cube by 30 degrees
  cube.rotation.y = degreesToRadians(30);
}

```

#### <a name="addlights"></a>addLights()

`addLights()` 函数是一个简单的函数，用于将创建的光分组并将其添加到场景。

```javascript
function addLights() {
  var lightOne = new THREE.DirectionalLight(0xffffff);
  lightOne.position.set(1, 1, 1);
  scene.add(lightOne);

  // Add a second light with half the intensity
  var lightTwo = new THREE.DirectionalLight(0xffffff, .5);
  lightTwo.position.set(1, -1, -1);
  scene.add(lightTwo);
}
```

#### <a name="onwindowresize"></a>onWindowResize()

只要事件侦听器听到已触发 `resize` 事件就会调用 `onWindowResize` 函数。 当用户调整窗口大小时就会发生此事件。 如果出现此情况，我们需要确保图像保持相应比例并且可以在整个窗口中看到。

```javascript
function onWindowResize() {

  camera.aspect = window.innerWidth / window.innerHeight;
  camera.updateProjectionMatrix();

  renderer.setSize(window.innerWidth, window.innerHeight);
}
```

#### <a name="animate"></a>animate()

我们需要使用的最后一个函数是 `animate()` 函数，它也会调用 `render()` 函数。 [`requestAnimationFrame()`](https://developer.mozilla.org/docs/Web/API/window/requestAnimationFrame) 函数用于持续更新我们的呈现器。 接着，我们会使用这些函数将呈现器更新为 Cool 动画，如在迷宫内四处移动。

```javascript
function animate() {
    render();
    // Keep updating the renderer
    requestAnimationFrame(animate);
}

function render() {
    renderer.render(scene, camera);
}
```

#### <a name="unit-conversion-functions"></a>单位转换函数

在 **three.js** 中，旋转是以弧度为单位的。 为了让事情更加简单，我们将继续添加一些函数，从而使我们能够轻松地在度数和弧度之间轻松转换。 


```javascript
function degreesToRadians(degrees) {
  return degrees * Math.PI / 180;
}

function radiansToDegrees(radians) {
  return radians * 180 / Math.PI;
}
```

谁会记得 30 度就是 0.523 弧度呢？ 通过在 `createMazeCubes()` 函数中使用 `degreesToRadians(30)` 获取旋转角度要简单得多。

___

尽管需要添加许多代码，但我们现在拥有了一个可呈现到 `container` 的漂亮的立方体！ 查看 CodePen 中的结果。

如果遇到问题，你可以复制和粘帖此 CodePen 中的所有 JavaScript 来加以解决，或者可以对其进行编辑，以调整某些光和更改部分颜色。 

<iframe height='300' scrolling='no' title='光、 相机、 立方体 ！' src='//codepen.io/MicrosoftEdgeDocumentation/embed/YZWygZ/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>请参阅 Pen<a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/YZWygZ/'>光，相机、 立方体 ！</a> 通过 Microsoft Edge 文档 (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) 可以在<a href='https://codepen.io'>CodePen</a>上。
</iframe>


### <a name="3-making-the-maze"></a>3. 制作迷宫

盯着一个立方体已经非常地让人激动了，更何况是整个由立方体组成的迷宫！ 创建级别的最快方式之一是将所有立方体排成一个 2D 数组，这是游戏社区中的一个广为人知的秘密。
 
![由 2D 数组组成的迷宫](images/dinomap.png)

将 1 置于立方体处并将 0 置于空白处使你能够轻松地手动创建/调整迷宫。

我们的实现方法是将旧的 `createMazeCubes()` 函数替换成使用嵌套循环创建和放置多个立方体的函数。 此外，我们还会创建一个名为 `collidableObjects` 的数组，并为其添加立方体，以实现本教程后面介绍的碰撞检测：

```javascript
var totalCubesWide; // How many cubes wide the maze will be
var collidableObjects = []; // An array of collidable objects used later

function createMazeCubes() {
  // Maze wall mapping, assuming even square
  // 1's are cubes, 0's are empty space
  var map = [
    [0, 0, 1, 0, 1, 0, 1, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 1, 0, 0, ],
    [0, 1, 0, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 1, 0, 0, ],
    [0, 1, 0, 0, 0, 0, 1, 0, 0, 0, 1, 1, 1, 1, 1, 0, 1, 1, 0, 0, ],
    [0, 1, 0, 0, 0, 0, 1, 0, 0, 0, 1, 0, 1, 0, 0, 0, 0, 0, 0, 0, ],
    [0, 1, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, ],
    [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, ],
    [1, 1, 1, 0, 1, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, ],
    [0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, ],
    [0, 0, 0, 0, 1, 1, 1, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, ],
    [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, ],
    [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 1, 0, 0, 1, 0, 1, ],
    [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 1, 0, 0, 1, 0, 0, ],
    [0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 1, 0, 1, 0, 1, 0, 0, 1, 1, 1, ],
    [0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 0, 0, ],
    [0, 0, 0, 0, 0, 0, 1, 0, 0, 1, 1, 1, 1, 0, 1, 0, 0, 0, 0, 0, ],
    [1, 1, 1, 0, 1, 1, 1, 1, 0, 1, 0, 1, 1, 0, 1, 0, 0, 0, 0, 0, ],
    [0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, ],
    [1, 1, 1, 0, 0, 0, 0, 0, 0, 1, 0, 1, 0, 1, 0, 0, 0, 0, 0, 0, ],
    [0, 0, 1, 0, 1, 1, 1, 1, 0, 0, 0, 1, 0, 1, 0, 0, 0, 0, 0, 0, ],
    [0, 0, 1, 1, 1, 0, 0, 0, 0, 1, 0, 1, 0, 1, 0, 0, 0, 0, 0, 0, ]
  ];

  // wall details
  var cubeGeo = new THREE.BoxGeometry(UNITWIDTH, UNITHEIGHT, UNITWIDTH);
  var cubeMat = new THREE.MeshPhongMaterial({
    color: 0x81cfe0,
  });

  // Keep cubes within boundry walls
  var widthOffset = UNITWIDTH / 2;
  // Put the bottom of the cube at y = 0
  var heightOffset = UNITHEIGHT / 2;
  
  // See how wide the map is by seeing how long the first array is
  totalCubesWide = map[0].length;

  // Place walls where 1`s are
  for (var i = 0; i < totalCubesWide; i++) {
    for (var j = 0; j < map[i].length; j++) {
      // If a 1 is found, add a cube at the corresponding position
      if (map[i][j]) {
        // Make the cube
        var cube = new THREE.Mesh(cubeGeo, cubeMat);
        // Set the cube position
        cube.position.z = (i - totalCubesWide / 2) * UNITWIDTH + widthOffset;
        cube.position.y = heightOffset;
        cube.position.x = (j - totalCubesWide / 2) * UNITWIDTH + widthOffset;
        // Add the cube
        scene.add(cube);
        // Used later for collision detection
        collidableObjects.push(cube);
      }
    }
  }
    // The size of the maze will be how many cubes wide the array is * the width of a cube
    mapSize = totalCubesWide * UNITWIDTH;
}

```

既然我们已经知道使用的立方体总数（及其大小），现在，我们可以使用计算得出的 `mapSize` 变量来设置地平面的大小。

```javascript
var mapSize;    // The width/depth of the maze

function createGround() {
    // Create ground geometry and material
    var groundGeo = new THREE.PlaneGeometry(mapSize, mapSize);
    var groundMat = new THREE.MeshPhongMaterial({ color: 0xA0522D, side: THREE.DoubleSide});

    var ground = new THREE.Mesh(groundGeo, groundMat);
    ground.position.set(0, 1, 0);
    // Rotate the place to ground level
    ground.rotation.x = degreesToRadians(90);
    scene.add(ground);
}
```

迷宫的最后一个部分是围墙，以便将所有一切围起来。 我们将使用一个循环来一次制作两个平面（墙），并使用我们在 `createGround()` 中计算得出的 `mapSize` 变量来确定其宽度。 新墙也将添加至我们的 `collidableObjects` 数组，以便进行后续的碰撞检测：

```javascript
function createPerimWalls() {
    var halfMap = mapSize / 2;  // Half the size of the map
    var sign = 1;               // Used to make an amount positive or negative

    // Loop through twice, making two perimeter walls at a time
    for (var i = 0; i < 2; i++) {
        var perimGeo = new THREE.PlaneGeometry(mapSize, UNITHEIGHT);
        // Make the material double sided
        var perimMat = new THREE.MeshPhongMaterial({ color: 0x464646, side: THREE.DoubleSide });
        // Make two walls
        var perimWallLR = new THREE.Mesh(perimGeo, perimMat);
        var perimWallFB = new THREE.Mesh(perimGeo, perimMat);

        // Create left/right wall
        perimWallLR.position.set(halfMap * sign, UNITHEIGHT / 2, 0);
        perimWallLR.rotation.y = degreesToRadians(90);
        scene.add(perimWallLR);
        // Used later for collision detection
        collidableObjects.push(perimWallLR);
        // Create front/back wall
        perimWallFB.position.set(0, UNITHEIGHT / 2, halfMap * sign);
        scene.add(perimWallFB);

        // Used later for collision detection
        collidableObjects.push(perimWallFB);

        sign = -1; // Swap to negative value
    }
}
```


不要忘记在 `init()` 函数中的 `createMazeCubes()` 后添加 `createGround()` 和 `createPerimWalls` 调用，从而能够对其进行编译！
___

现在，我们看到了一个漂亮的迷宫，但还没有炫酷之感，因为我们的摄像头卡滞在一个点上。 现在，我们需要将此游戏进行升级并添加一些摄像头控件。

自由地测试 CodePen 中的所有功能，如更改立方体的颜色，或者通过注释掉 `init()` 函数中的 `createGround()` 来删除地平面。


<iframe height='300' scrolling='no' title='迷宫构建' src='//codepen.io/MicrosoftEdgeDocumentation/embed/JWKYzG/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>请参阅 Pen <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/JWKYzG/'>迷宫构建</a>，Microsoft Edge 文档 (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>)（位于 <a href='https://codepen.io'>CodePen</a> 上）。
</iframe>

### <a name="4-allowing-the-player-to-look-around"></a>4. 让玩家可以四处查看

现在，是时候进入该迷宫并开始四处查看了。 为此，我们将使用 **PointerLockControls.js** 库和我们的摄像头。

**PoinerLockControls.js** 库使用鼠标来按鼠标移动的方向旋转摄像头，使玩家能够四处查看。 

首先，让我们先为 **index.html** 文件添加一些新的元素：

```html
<div id="blocker">
    <div id="instructions">
    <strong>Click to look!</strong>
    </div>
</div>

<script src="main.js"></script>
```

此外，你还需要使用本节最后的 CodePen 中的所有 CSS。 你应将其粘帖到 **stylesheet.css** 文件。

切换回 **main.js**，添加一些新的全局变量：用于存储控制器的 `controls`、用于跟踪控制器状态的`controlsEnabled` 以及用于掌握 **index.html** 中的 `blocker` 元素的 `blocker`：

```javascript
var controls;
var controlsEnabled = false;

// HTML elements to be changed
var blocker = document.getElementById('blocker');
```


现在，在 `init()` 函数中，我们可以创建一个新的 `PoinerLockControls` 对象，将其粘帖到我们的 `camera` 并添加 `camera`（通过 `controls.getObject()` 访问）。

```javascript
controls = new THREE.PointerLockControls(camera);
scene.add(controls.getObject());
```

摄像头现已连接，但我们仍需要让鼠标和控制器互动，从而使我们能够四处查看。 

对于此情况，[指针锁 API](https://docs.microsoft.com/microsoft-edge/dev-guide/dom/pointer-lock) 派上了用场，它使我们能够将鼠标移动与我们的摄像头连接起来。 此外，指针锁 API 还可以使鼠标消失，从而获得一种让人更加沉醉的体验。 按 ESC 即可结束鼠标与摄像头之间的连接并让鼠标再次显示。 添加 `getPointerLock()` 和 `lockChange()` 函数可以帮助我们实现该操作。

`getPointerLock()` 函数可以侦听何时单击了鼠标。 单击后，我们呈现的游戏（位于 `container` 元素中）将尝试控制鼠标。 此外，我们还需添加一个事件侦听器，用于检测玩家何时启用或禁用锁，该锁随后将调用 `lockChange()`。 

```javascript
function getPointerLock() {
  document.onclick = function () {
    container.requestPointerLock();
  }
  document.addEventListener('pointerlockchange', lockChange, false); 
}

```

我们的 `lockChange()` 函数需要禁用或启用控件和 `blocker` 元素。 检查用于鼠标事件的 [`pointerLockElement`](https://developer.mozilla.org/docs/Web/API/Document/pointerLockElement) 属性目标是否已设置为 `container` 后，我们就可以确定指针锁的状态。

```javascript
function lockChange() {
    // Turn on controls
    if (document.pointerLockElement === container) {
        // Hide blocker and instructions
        blocker.style.display = "none";
        controls.enabled = true;
    // Turn off the controls
    } else {
      // Display the blocker and instruction
        blocker.style.display = "";
        controls.enabled = false;
    }
}
```

现在，我们可以在 `init()` 函数之前添加 `getPointerLock()` 调用。
```javascript
// Get the pointer lock state
getPointerLock();
init();
animate();
```

---

至此，我们已经能够**四处查看**，但真正“精彩”之处是能够**四处移动**。 这些会涉及到数学中的矢量知识，如果没有一点数学知识，那 3D 图形就会是什么样呢？

<iframe height='300' scrolling='no' title='四处查看' src='//codepen.io/MicrosoftEdgeDocumentation/embed/gmwbMo/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>请参阅 Pen <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/gmwbMo/'>四处查看</a>，Microsoft Edge 文档 (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>)（位于 <a href='https://codepen.io'>CodePen</a> 上）。
</iframe>


### <a name="5-adding-player-movement"></a>5. 添加玩家移动

若要深入了解如何让我们的玩家移动，则需要我们回顾一下微积分知识。 我们需要为 `camera` 应用速度（移动）及特定矢量（方向）。

让我们添加一些用于跟踪玩家移动方向的更具全局性的变量，并设置初始的速度矢量：

```javascript
// Flags to determine which direction the player is moving
var moveForward = false;
var moveBackward = false;
var moveLeft = false;
var moveRight = false;

// Velocity vector for the player
var playerVelocity = new THREE.Vector3();

// How fast the player will move
var PLAYERSPEED = 800.0;

var clock;
```

在 `init()` 函数的开头，将 `clock` 设置为一个新的 `Clock` 对象。 这样一来，我们就可以跟踪其呈现新帧所需的时间差（增量）。 此外，你还需要添加用于收集用户输入的 `listenForPlayerMovement()` 调用。 

```
clock = new THREE.Clock();
listenForPlayerMovement();
```

我们的 `listenForPlayerMovement()` 函数用于翻转我们的方向状态。 在此函数的底部，我们还添加了两个用于等待按下和松开按键的事件侦听器。 触发任意事件后，我们随后将会进行检查，以确定它是否是我们想要触发或停止移动的键。

在本游戏中，我们进行了设置，因此，玩家可以通过 W、A、S、D 键或箭头键四处移动。

```javascript
function listenForPlayerMovement() {
    
    // A key has been pressed
    var onKeyDown = function(event) {

    switch (event.keyCode) {

      case 38: // up
      case 87: // w
        moveForward = true;
        break;

      case 37: // left
      case 65: // a
        moveLeft = true;
        break;

      case 40: // down
      case 83: // s
        moveBackward = true;
        break;

      case 39: // right
      case 68: // d
        moveRight = true;
        break;
    }
  };

  // A key has been released
    var onKeyUp = function(event) {

    switch (event.keyCode) {

      case 38: // up
      case 87: // w
        moveForward = false;
        break;

      case 37: // left
      case 65: // a
        moveLeft = false;
        break;

      case 40: // down
      case 83: // s
        moveBackward = false;
        break;

      case 39: // right
      case 68: // d
        moveRight = false;
        break;
    }
  };

  // Add event listeners for when movement keys are pressed and released
  document.addEventListener('keydown', onKeyDown, false);
  document.addEventListener('keyup', onKeyUp, false);
}
```

既然我们已经能够确定用户想要移动的方向（现已作为 `true` 存储于全局方向标记中），那么，我们就可以进行部分操作了。 此操作通过 `animatePlayer()` 函数实现。

此函数将从 `animate()` 内调用，用于传递 `delta` 以获取不同帧之间的时间差，从而使我们的移动在帧速率变化时不会看上去不同步。

```javascript
function animate() {
  render();
  requestAnimationFrame(animate);
  // Get the change in time between frames
  var delta = clock.getDelta();
  animatePlayer(delta);
}
```

现在到了最精彩的部分了！ 我们的动量矢量 (`playerVeloctiy`) 具有三个参数：`(x, y, z)`，其中 `y` 为垂直动量。 在本游戏中，我们并非进行任何跳跃操作，因此，我们只用到了 `x` 和 `z` 参数。 此矢量最开始均设为 (0, 0, 0)。

如下面的代码所示，我们需要进行一系列检查，以确定翻转为 `true` 的方向标记。 获得方向之后，我们将增加或减去 `x` 和 `y`，以在该方向上应用动量。 如果未按下任何移动键，则矢量将设回 `(0, 0, 0)`。


```javascript

function animatePlayer(delta) {
  // Gradual slowdown
  playerVelocity.x -= playerVelocity.x * 10.0 * delta;
  playerVelocity.z -= playerVelocity.z * 10.0 * delta;

  if (moveForward) {
    playerVelocity.z -= PLAYERSPEED * delta;
  } 
  if (moveBackward) {
    playerVelocity.z += PLAYERSPEED * delta;
  } 
  if (moveLeft) {
    playerVelocity.x -= PLAYERSPEED * delta;
  } 
  if (moveRight) {
    playerVelocity.x += PLAYERSPEED * delta;
  }
  if( !( moveForward || moveBackward || moveLeft ||moveRight)) {
    // No movement key being pressed. Stop movememnt
    playerVelocity.x = 0;
    playerVelocity.z = 0;
  }
  controls.getObject().translateX(playerVelocity.x * delta);
  controls.getObject().translateZ(playerVelocity.z * delta);
}
```

最后，我们将更新后的 `x` 和 `y` 值应用到摄像头，以转换为玩家的实际移动。

---

恭喜你！ 你现在已拥有一个可四处移动和查看、由玩家控制的摄像头。 我们仍然会穿墙而过，但这就是我们后续该担心的事情了。 接下来，我们将添加恐龙。

<iframe height='300' scrolling='no' title='可以四处移动' src='//codepen.io/MicrosoftEdgeDocumentation/embed/qrbKZg/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>请参阅笔<a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/qrbKZg/'>四处移动</a>，Microsoft Edge 文档 (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) 可以在<a href='https://codepen.io'>CodePen</a>上。
</iframe>

> [!NOTE]
> 如果在 UWP 应用中使用了这些控件，则可能会遇到移动迟滞和 `keyUp` 未注册事件。 我们将研究此问题，并且希望能够尽快修复示例中的这一问题！

### <a name="6-load-that-dino"></a>6. 加载恐龙！

如果已克隆或下载此项目存储库，则你将会看到一个内含 `dino.json` 的 `models` 文件夹。 此 JSON 文件是一个 3D 恐龙模型，它通过搅料机制作和导出。


我们必须添加更多的全局变量，以加载此恐龙：

```javascript
var DINOSCALE = 20;  // How big our dino is scaled to

var clock;
var dino;
var loader = new THREE.JSONLoader();

var instructions = document.getElementById('instructions');
```

既然我们已创建了 `JSONLoader`，那么，我们需要通过从文件收集而来的几何图形和材料传递 **dino.json** 路径和回调。
加载恐龙是一项异步任务，这意味着在恐龙完全加载完毕之前不会呈现任何内容。 在 **index.html** 中，我们将 `instructions` 元素中的字符串改为了 `"Loading..."`，使玩家能够了解到操作正在进行。

恐龙加载完毕之后，请将 `instructions` 元素更新为游戏的实际说明，并将 `animate()` 函数从 `init()` 末尾移至以下所示的回调函数末尾：

```javascript
   // load the dino JSON model and start animating once complete
    loader.load('./models/dino.json', function (geometry, materials) {


        // Get the geometry and materials from the JSON
        var dinoObject = new THREE.Mesh(geometry, new THREE.MultiMaterial(materials));

        // Scale the size of the dino
        dinoObject.scale.set(DINOSCALE, DINOSCALE, DINOSCALE);
        dinoObject.rotation.y = degreesToRadians(-90);
        dinoObject.position.set(30, 0, -400);
        dinoObject.name = "dino";
        scene.add(dinoObject);
        
        // Store the dino
        dino = scene.getObjectByName("dino"); 

        // Model is loaded, switch from "Loading..." to instruction text
        instructions.innerHTML = "<strong>Click to Play!</strong> </br></br> W,A,S,D or arrow keys = move </br>Mouse = look around";

        // Call the animate function so that animation begins after the model is loaded
        animate();
    });
```

---

现在，我们已载入了恐龙模型。 赶快去看看！

<iframe height='300' scrolling='no' title='添加恐龙' src='//codepen.io/MicrosoftEdgeDocumentation/embed/xqOwBw/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>请参阅 Pen <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/xqOwBw/'>添加恐龙</a>，Microsoft Edge 文档 (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>)（位于 <a href='https://codepen.io'>CodePen</a> 上）。
</iframe>

### <a name="7-move-that-dino"></a>7. 移动恐龙！

创建游戏 AI 非常复杂，因此，在本示例中，我们只为此恐龙提供简单的移动行为。 我们的恐龙可以径直移动、穿墙和进入远处的雾中。

若要执行此操作，请先添加全局变量 `dinoVelocity`。

```javascript
var DINOSPEED = 400.0;

var dinoVelocity = new THREE.Vector3();
```
 接下来，请从 `animation()` 函数调用 `animateDino()` 函数并添加以下代码：

```javascript
function animateDino(delta) {
    // Gradual slowdown
    dinoVelocity.x -= dinoVelocity.x * 10.0 * delta;
    dinoVelocity.z -= dinoVelocity.z * 10.0 * delta;

    dinoVelocity.z += DINOSPEED * delta;
    // Move the dino
    dino.translateZ(dinoVelocity.z * delta);
}
```
---

观看恐龙离开并不是非常有趣，但是在添加碰撞检测之后，这就变得有趣得多。

<iframe height='300' scrolling='no' title='移动恐龙-无碰撞' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/jBMbbL/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>请参阅 Pen <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/jBMbbL/'>移动恐龙 - 无碰撞</a>，Microsoft Edge 文档 (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>)（位于 <a href='https://codepen.io'>CodePen</a>）。
</iframe>

### <a name="8-collision-detection-for-the-player"></a>8. 适用于玩家的碰撞检测

现在，我们的玩家和恐龙都可以四处移动，但是，大家穿墙时还有一个恼人的问题。 当我们在本教程的前部首次添加立方体和墙时，我们已将其置于 `collidableObjects` 数组中。 我们将使用此数组来辨别玩家是否离其无法穿过的某个物体太近。

我们将使用光线投射器来确定何时将会相交。 你可以将光线投射器想象成一束来自某一特定方向、由摄像头发出的激光束，可以返回是否碰到物体及离该物体的准确距离的报告。

```javascript
var PLAYERCOLLISIONDISTANCE = 20;
```

我们将生成一个名为 `detectPlayerCollision()` 的新函数，该函数在玩家离可撞击物体过近时将会返回 `true`。
对于玩家，我们将会使用一个光线投射器，以根据玩家的移动方向更改光线投射器的指向方向。

若要执行此操作，我们需要创建一个未定义矩阵 `rotationMatrix`。 在检查移动方向时，我们将以定义的 `rotationMatrix` 或未定义（如果你向前移动）结束。
如果已定义，则 `rotationMatrix` 将会应用到控件方向。 

随后将会创建光线投射器，从摄像头开始并投向 `cameraDirection` 方向。


```javascript
function detectPlayerCollision() {
    // The rotation matrix to apply to our direction vector
    // Undefined by default to indicate ray should coming from front
    var rotationMatrix;
    // Get direction of camera
    var cameraDirection = controls.getDirection(new THREE.Vector3(0, 0, 0)).clone();

    // Check which direction we're moving (not looking)
    // Flip matrix to that direction so that we can reposition the ray
    if (moveBackward) {
        rotationMatrix = new THREE.Matrix4();
        rotationMatrix.makeRotationY(degreesToRadians(180));
    }
    else if (moveLeft) {
        rotationMatrix = new THREE.Matrix4();
        rotationMatrix.makeRotationY(degreesToRadians(90));
    }
    else if (moveRight) {
        rotationMatrix = new THREE.Matrix4();
        rotationMatrix.makeRotationY(degreesToRadians(270));
    }

    // Player is not moving forward, apply rotation matrix needed
    if (rotationMatrix !== undefined) {
        cameraDirection.applyMatrix4(rotationMatrix);
    }

    // Apply ray to player camera
    var rayCaster = new THREE.Raycaster(controls.getObject().position, cameraDirection);

    // If our ray hit a collidable object, return true
    if (rayIntersect(rayCaster, PLAYERCOLLISIONDISTANCE)) {
        return true;
    } else {
        return false;
    }
}
```

我们的 `detectPlayerCollision()` 函数依赖于 `rayIntersect()` 帮助程序函数。
这将使用一个光线反射器和一个值，表示在确定碰撞之前，我们与 `collidableObjects` 数组中的对象之间能够靠近的最小距离。

```javascript
function rayIntersect(ray, distance) {
    var intersects = ray.intersectObjects(collidableObjects);
    for (var i = 0; i < intersects.length; i++) {
        // Check if there's a collision
        if (intersects[i].distance < distance) {
            return true;
        }
    }
    return false;
}
```

既然我们可以确定何时将会发生碰撞，那我们就可以调整 `animatePlayer()` 函数：

```javascript
function animatePlayer(delta) {
    // Gradual slowdown
    playerVelocity.x -= playerVelocity.x * 10.0 * delta;
    playerVelocity.z -= playerVelocity.z * 10.0 * delta;

    // If no collision and a movement key is being pressed, apply movement velocity
    if (detectPlayerCollision() == false) {
        if (moveForward) {
            playerVelocity.z -= PLAYERSPEED * delta;
        }
        if (moveBackward) {
            playerVelocity.z += PLAYERSPEED * delta;
        } 
        if (moveLeft) {
            playerVelocity.x -= PLAYERSPEED * delta;
        }
        if (moveRight) {
            playerVelocity.x += PLAYERSPEED * delta;
        }

        controls.getObject().translateX(playerVelocity.x * delta);
        controls.getObject().translateZ(playerVelocity.z * delta);
    } else {
        // Collision or no movement key being pressed. Stop movememnt
        playerVelocity.x = 0;
        playerVelocity.z = 0;
    }
}
```

---

现在，我们已经具有了玩家碰撞检测，那就试试穿墙吧！

<iframe height='300' scrolling='no' title='移动玩家-碰撞' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/qraOeO/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>请参阅 Pen <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/qraOeO/'>移动玩家 - 碰撞</a>，Microsoft Edge 文档 (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>)（位于 <a href='https://codepen.io'>CodePen</a>）。
</iframe>


### <a name="9-collision-detection-and-animation-for-dino"></a>9. 面向恐龙的碰撞检测和动画

当恐龙离可碰撞物体太近时，我们需要使恐龙停止穿墙，而让其向任意方向移动。

首先，我们需要确定恐龙何时会发生碰撞。 

我们需要为碰撞距离设置另一个全局变量：

```javascript
var DINOCOLLISIONDISTANCE = 55;     
```

既然我们已经指定了预计恐龙会发生碰撞的距离，那么让我们添加一个与 `detectPlayerCollision()` 相似但比其简单得多的函数。
`detectDinoCollision` 函数非常简单，其中，我们始终将一个光线投射器设为从恐龙正面直射过来。 无需像在玩家碰撞中那样将其旋转。

```javascript
function detectDinoCollision() {
    // Get the rotation matrix from dino
    var matrix = new THREE.Matrix4();
    matrix.extractRotation(dino.matrix);
    // Create direction vector
    var directionFront = new THREE.Vector3(0, 0, 1);

    // Get the vectors coming from the front of the dino
    directionFront.applyMatrix4(matrix);

    // Create raycaster
    var rayCasterF = new THREE.Raycaster(dino.position, directionFront);
    // If we have a front collision, we have to adjust our direction so return true
    if (rayIntersect(rayCasterF, DINOCOLLISIONDISTANCE))
        return true;
    else
        return false;
}
```

我们可以简单看一下我们最终的 `animateDino()` 函数在与碰撞检测相关时是什么样子的。


```javascript
function animateDino(delta) {
    // Gradual slowdown
    dinoVelocity.x -= dinoVelocity.x * 10.0 * delta;
    dinoVelocity.z -= dinoVelocity.z * 10.0 * delta;


    // If no collision, apply movement velocity
    if (detectDinoCollision() == false) {
        dinoVelocity.z += DINOSPEED * delta;
        // Move the dino
        dino.translateZ(dinoVelocity.z * delta);

    } else {
        // Collision. Adjust direction
        var directionMultiples = [-1, 1, 2];
        var randomIndex = getRandomInt(0, 2);
        var randomDirection = degreesToRadians(90 * directionMultiples[randomIndex]);

        dinoVelocity.z += DINOSPEED * delta;
        dino.rotation.y += randomDirection;
    }
}
```

我们始终希望恐龙旋转 -90、90 或 180 度。 简单来说，我们已经在上面制作了 `directionMultiples` 数组，该数组乘以 90 即可得到这些数字。
若要随机选择旋转角度，我们添加了 `getRandomInt()` 帮助程序函数以获取值 0、1 或 2，它们代表数组的随机指标。

```javascript
function getRandomInt(min, max) {
    min = Math.ceil(min);
    max = Math.floor(max);
    return Math.floor(Math.random() * (max - min)) + min;
}
```

全部完成之后，我们将该数组的随机指标乘以 90 即可获得旋转度数（已转换为弧度）。
通过 `dino.rotation.y += randomDirection;` 将此值添加到恐龙的 `y` 旋转方向后，恐龙即可在发生碰撞时任意转向。


---

我们成功了！ 现在，我们的恐龙也享有了 AI，可以在迷宫内四处移动！

<iframe height='300' scrolling='no' title='移动恐龙-碰撞' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/bqwMXZ/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>请参阅笔<a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/bqwMXZ/'>移动恐龙-碰撞</a>，Microsoft Edge 文档 (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) 可以在<a href='https://codepen.io'>CodePen</a>上。
</iframe>

### <a name="10-starting-the-chase"></a>10. 开始追踪

当恐龙与玩家的距离在特定范围内时，我们希望恐龙开始追踪他们。 这只是一个示例，因此，我们并没有对恐龙追踪玩家应用任何高级算法。 相反，恐龙只会看到玩家并朝他们走过去。 在迷宫的开放部分，这种方法非常不错，但如果有一堵墙挡在中间，那恐龙就会被堵住。

我们将在 `animate()` 函数中添加一个布尔变量，该变量由 `triggerChase()` 返回的值决定：

```javascript
function animate() {
    render();
    requestAnimationFrame(animate);

    // Get the change in time between frames
    var delta = clock.getDelta();

    // If the player is in dino's range, trigger the chase
    var isBeingChased = triggerChase();

    animateDino(delta);
    animatePlayer(delta);
}
```

`triggerChase` 函数用于确定玩家是否处在恐龙的追踪范围内，并让恐龙始终面向玩家，使其能够朝玩家方向移动。 

```javascript
function triggerChase() {
    // Check if in dino detection range of the player
    if (dino.position.distanceTo(controls.getObject().position) < 300) {
        // Set the dino target's y value to the current y value. We only care about x and z for movement.
        var lookTarget = new THREE.Vector3();
        lookTarget.copy(controls.getObject().position);
        lookTarget.y = dino.position.y;

        // Make dino face camera
        dino.lookAt(lookTarget);

        // Get distance between dino and camera with a unit offset
        // Game over when dino is the value of CATCHOFFSET units away from camera
        var distanceFrom = Math.round(dino.position.distanceTo(controls.getObject().position)) - CATCHOFFSET;
        // Alert and display distance between camera and dino
        dinoAlert.innerHTML = "Dino has spotted you! Distance from you: " + distanceFrom;
        dinoAlert.style.display = '';
        return true;
        // Not in agro range, don't start distance countdown
    } else {
        dinoAlert.style.display = 'none';
        return false;
    }
}
```

`triggerChase` 的另一部分用于显示某些文本，让玩家知道其与恐龙之间的距离。 我们还引入了 `CATCHOFFSET`，以指定 `0` 表示的距离。 如果没有偏移，`0` 表示在玩家的正上方，这不会形成一个电影般的结尾。



```javascript
var dinoAlert = document.getElementById('dino-alert');
dinoAlert.style.display = 'none';
```

---

此时，如果玩家离得太近，那么疯狂的恐龙就会开始一直追踪玩家，直到其位于玩家的上方。
最后一步是在恐龙距离 `CATCHOFFSET` 个单位时添加一些游戏结束条件。

<iframe height='300' scrolling='no' title='追踪' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/NpRBqR/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>请参阅 Pen <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/NpRBqR/'>追踪</a>，Microsoft Edge 文档 (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>)（位于 <a href='https://codepen.io'>CodePen</a>）。
</iframe>


### <a name="11-ending-the-game"></a>11. 结束游戏


我们已经从简单立方体走了很远的距离，现在，是时候结束了。

首先，我们需要设置一个变量，以跟踪游戏是否结束：

```javascript
var gameOver = false;
```

现在，我们需要最后一次更新 `animate()` 函数，以确定恐龙是否离玩家太近。
如果恐龙挨得太近，那我们将会调用一个名为 `caught()` 的新函数，并且会让玩家和恐龙均停止移动；如果恐龙并未挨得太近，则照常继续并让玩家和恐龙四处移动。

```javascript
function animate() {
    render();
    requestAnimationFrame(animate);

    // Get the change in time between frames
    var delta = clock.getDelta();
    // Update our frames per second monitor

    // If the player is in dino's range, trigger the chase
    var isBeingChased = triggerChase();
    // If the player is too close, trigger the end of the game
    if (dino.position.distanceTo(controls.getObject().position) < CATCHOFFSET) {
        caught();
    // Player is at an undetected distance
    // Keep the dino moving and let the player keep moving too
    } else {
        animateDino(delta);
        animatePlayer(delta);
    }
}
```

如果恐龙抓到玩家，则 `caught()` 将会显示 `blocker` 元素并更新文本，以表示游戏失败。
`gameOver` 变量也会设为 `true`，以便让我们知道游戏结束。  


```javascript
function caught() {
    blocker.style.display = '';
    instructions.innerHTML = "GAME OVER </br></br></br> Press ESC to restart";
    gameOver = true;
    instructions.style.display = '';
    dinoAlert.style.display = 'none';
}
```


既然知道游戏是否结束，那么，我们就可以为 `lockChange()` 函数添加游戏结束检查。
当用户在游戏结束后按 ESC 时，添加的 `location.reload` 将会重新开始游戏。

```javascript
function lockChange() {
    if (document.pointerLockElement === container) {
        blocker.style.display = "none";
        controls.enabled = true;
    } else {
        if (gameOver) {
            location.reload();
        }
        blocker.style.display = "";
        controls.enabled = false;
    }
}
```

---

就这么简单！ 这是一个非常漫长的旅程，但现在我们已经成功拥有了一款用 **three.js** 制作的游戏。

返回到页面顶部，以查看[最终 CodePen](#introduction)！


## <a name="publishing-to-the-microsoft-store"></a>发布到 Microsoft 应用商店
现在你有一个 UWP 应用，则可以将其发布到 Microsoft Store （假设已先了改进 ！）有几个步骤的过程。

1.  你必须以 Windows 开发人员的身份[注册](https://developer.microsoft.com/store/register)。
2.  你必须使用应用提交[清单](https://msdn.microsoft.com/windows/uwp/publish/app-submissions)。
3.  必须将此应用提交以进行[认证](https://msdn.microsoft.com/windows/uwp/publish/the-app-certification-process)。
有关详细信息，请参阅[发布你的 UWP 应用](https://developer.microsoft.com/store/publish-apps)。

