---
title: 向 3D Babylon.js 游戏 WebVR 支持
description: 了解如何将 WebVR 支持添加到现有的三维 Babylon.js 游戏。
author: abbycar
ms.author: abigailc
ms.date: 11/29/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: webvr、 边缘，web 开发、 babylon、 babylonjs、 babylon.js、 javascript
ms.localizationpriority: medium
ms.openlocfilehash: 41665e8719493bb658f9926947061b1b5f81a139
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/13/2018
ms.locfileid: "1018647"
---
# <a name="adding-webvr-support-to-a-3d-babylonjs-game"></a>向 3D Babylon.js 游戏 WebVR 支持

如果您已创建 Babylon.js 三维游戏并灵感它看起来很好虚拟现实 (VR) 中，使它成为现实本教程中按照简单的步骤。

我们将添加 WebVR 支持游戏如下所示。 继续操作并插入 Xbox 控制器试用一下 ！


<iframe height='300' scrolling='no' title='使用 Babylon.GUI Babylon.js dino 游戏' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/wrOvoj/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>请参阅笔<a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/wrOvoj/'>Babylon.js dino 游戏使用 Babylon.GUI</a>由 Microsoft 边缘文档 (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) 上<a href='https://codepen.io'>CodePen</a>。
</iframe>

这是有关平面屏幕，但内容也适用于三维游戏中 VR？
在本教程中，我们将指导几个步骤通过它来获取此并运行与 WebVR。 我们将使用可以点击到添加了对 WebVR Microsoft 边缘中支持的[Windows 混合现实](https://developer.microsoft.com/en-us/windows/mixed-reality)耳机。 我们对游戏应用这些更改后，您可以预期它也继续支持 WebVR 其他浏览器/耳机组合中。



## <a name="prerequisites"></a>必备条件

- 文本编辑器 （如[Visual Studio 代码](https://code.visualstudio.com/download)）
- 已插入到您的计算机 Xbox 控制器
- Windows 10 创意者更新
- 具有[所需的最低规格运行 Windows 混合现实](https://developer.microsoft.com/en-us/windows/mixed-reality/immersive_headset_setup)的计算机
- Windows 混合现实设备 （可选） 



## <a name="getting-started"></a>入门

若要开始的最简单方法是访问[Windows 教程 web GitHub repo](https://github.com/Microsoft/Windows-tutorials-web)，按绿色**克隆或下载**按钮，然后选择**在 Visual Studio 中打开**。

![克隆或下载按钮](images/3dclone.png)

如果不想克隆项目，则可以将其下载为 zip 文件。
然后，您将有两个文件夹、[之前](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before)和[之后](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/after)。 "Before"文件夹之前添加任何 VR 功能，以及"之后"文件夹是 VR 支持完成的游戏是我们的游戏。

之前和之后的文件夹包含这些文件：
-   **纹理 /** -包含任何游戏中使用的图像的文件夹。
-   **css /** -包含游戏 CSS 的文件夹。
-   **js /** -包含 JavaScript 文件的文件夹。 Main.js 文件是我们游戏，并将其他文件使用的库。
-   **模型 /** -包含 3D 模型的文件夹。 对于该游戏我们具有不复存在只有一个模型。
-   **index.html** -承载该游戏的呈现器的网页。 在 Microsoft 边缘中打开此页启动游戏。

您可以通过在 Microsoft 边缘中打开其各自的 index.html 文件测试两个版本的游戏。



## <a name="the-mixed-reality-portal"></a>混合的现实门户

如果您不熟悉 Windows 混合现实，并与兼容图形卡的计算机上安装了 Windows 10 创建者更新，请尝试从 Windows 10 中开始菜单打开**混合现实门户**应用程序。

![混合的现实门户搜索](images/mixed-reality-portal.png)

如果满足所有要求，然后可以打开的开发人员功能和模拟 Windows 混合现实耳麦插入到您的计算机。 如果您是幸运能够具有附近实际耳麦，插入并运行安装程序。

> [!IMPORTANT]
> 在本教程中所有的时间，必须打开在混合现实门户。

您现在已准备好体验与 Microsoft 边缘 WebVR。

## <a name="2d-ui-in-a-virtual-world"></a>在虚拟环境中的二维 UI

>[!NOTE]
> 获取[**之前**](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before)文件夹，若要获取 starter 示例。

[Babylon.GUI](https://doc.babylonjs.com/how_to/gui) VR 友好库，使您要创建简单，交互式用户界面适用于 VR 的工作和非 VR 显示。
一个对 Babylon.js，扩展`GUI`库是使用的 throuhout 创建 2D 元素的示例。


2D 文本`GUI`元素可以创建与几行，具体取决于您要调整的属性数量。
下面的代码段已在我们[**之前**](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before)的示例，但我们演练的最新动态。
我们首先进行[`AdvancedDynamicTexture`](https://doc.babylonjs.com/how_to/gui#advanceddynamictexture)对象建立 GUI 将涵盖。 本示例将此设置为`CreateFullScreenUI()`，这意味着我们的 UI 将覆盖整个屏幕。 使用`AdvancedDynamicTexture`我们创建，然后进行 2D 启动游戏使用时出现的文本框`GUI.Rectanlge()`和`GUI.TextBlock()`。


[**Main.js**](https://github.com/Microsoft/Windows-tutorials-web/blob/master/BabylonJS-game-with-WebVR/before/js/main.js#L157-L168)中添加此代码。
```javascript
// GUI
var advancedTexture = BABYLON.GUI.AdvancedDynamicTexture.CreateFullscreenUI("UI");

// Start UI
startUI = new BABYLON.GUI.Rectangle("start");
startUI.background = "black"
startUI.alpha = .8;
startUI.thickness = 0;
startUI.height = "60px";
startUI.width = "400px";
advancedTexture.addControl(startUI); 
var tex2 = new BABYLON.GUI.TextBlock();
tex2.text = "Stay away from the dinosaur! \n Plug in an Xbox controller and press A to start";
tex2.color = "white";
startUI.addControl(tex2); 
```


此用户界面中，一旦创建，但可使用打开或关闭切换可见`isVisible`根据游戏中发生了什么情况。
```javascript
startUI.isVisible = false;
```



## <a name="detecting-headsets"></a>检测耳麦

它是 VR 应用程序具有两种类型的照相机，以便可以支持多个方案的最佳实践。 对此游戏，我们将支持一个摄像头的使用耳机所以不需要使用耳麦为插入，以及另一个。 若要确定哪一个游戏将使用，我们首先必须检查以查看是否已检测到耳麦。 为此，我们将使用[`navigator.getVRDisplays()`](https://developer.mozilla.org/en-US/docs/Web/API/Navigator/getVRDisplays)。


将上面此代码添加`window.addEventListener('DOMContentLoaded')`中**main.js**。
```javascript
var headset;
// If a VR headset is connected, get its info
navigator.getVRDisplays().then(function (displays) {
    if (displays[0]) {
        headset = displays[0];
    }
});
```

与信息存储在`headset`变量，我们现在将能够选择适合用户摄像头。


## <a name="creating-and-selecting-the-initial-camera"></a>创建和选择初始照相机

可以使用与 Babylon.js，WebVR 要快速添加[`WebVRFreeCamera`](http://doc.babylonjs.com/classes/3.1/webvrfreecamera)。 此摄像机可以接受键盘输入，并使您能够使用 VR 耳麦以控制"标头"旋转。


### <a name="step-1-checking-for-headsets"></a>步骤 1： 检查的耳麦

对于我们回退摄像机，我们将使用[`UniversalCamera`](https://doc.babylonjs.com/classes/3.1/universalcamera)的正在使用原始的游戏。

我们将检查我们`headset`变量以确定是否我们可以使用`WebVRFreeCamera`摄像头。

替换`camera = new BABYLON.UniversalCamera("Camera", new BABYLON.Vector3(0, 18, -45), scene);`替换为以下代码。
```javascript
        if(headset){
            // Create a WebVR camera with the trackPosition property set to false so that we can control movement with the gamepad
            camera = new BABYLON.WebVRFreeCamera("vrcamera", new BABYLON.Vector3(0, 14, 0), scene, true, { trackPosition: false });
            camera.deviceScaleFactor = 1;
        } else {
            // No headset, use universal camera
            camera = new BABYLON.UniversalCamera("camera", new BABYLON.Vector3(0, 18, -45), scene);
        }
```


### <a name="step-2-activating-the-webvrfreecamera"></a>步骤 2： 激活 WebVRFreeCamera
若要激活此照相机大多数浏览器中的，用户必须执行请求的虚拟体验某些交互。
我们将挂接到鼠标单击为止此功能。


粘贴将`createScene()`后正常`camera.applyGravity = true;`。
```javascript
        scene.onPointerDown = function () {
            scene.onPointerDown = undefined
            camera.attachControl(canvas, true);
        }
```

在游戏单击现在创建如下所示，提示或显示游戏中耳机权利将立即终止如果用户已被接受之前提示。

![沉浸式提示](images/immersiveview.png)

我们还可以添加一段代码将显示`UniversalCamera`查看之后，我们切换到我们`WebVRFreeCamera`，允许用户查看游戏而不是一个蓝色的窗口。 

添加后的以下`engine.runRenderLoop(function () {`。
```javascript
            if (headset) {
                if (!(headset.isPresenting)) {
                    var camera2 = new BABYLON.UniversalCamera("Camera", new BABYLON.Vector3(0, 18, -45), scene);
                    scene.activeCamera = camera2;
                } else {
                    scene.activeCamera = camera;
                }
            }
```

### <a name="step-3-adding-gamepad-support"></a>步骤 3： 添加游戏板支持

由于`WebVRFreeCamera`最初不支持游戏，我们将将我们游戏板按钮映射到键盘箭头键。 我们将执行此操作通过很快找到它们到`inputs`的照相机属性。 通过添加左侧模拟继续有效的相应代码，上下左侧和右侧来匹配箭头键，我们游戏板处于回操作。


将此代码下方添加`scene.onPointerDown = function() {...}`呼叫。
``` javascript
    // Custom input, adding Xbox controller support for left analog stick to map to keyboard arrows
    camera.inputs.attached.keyboard.keysUp.push(211);    // Left analog up
    camera.inputs.attached.keyboard.keysDown.push(212);  // Left analog down
    camera.inputs.attached.keyboard.keysLeft.push(214);  // Left analog left
    camera.inputs.attached.keyboard.keysRight.push(213); // Left analog right
```


### <a name="step-4-give-it-a-try"></a>步骤 4： 赶快试试吧 ！

如果我们使用我们耳麦打开**index.html**和外接游戏控制器上蓝色游戏窗口, 的左侧的单击将切换到 VR 模式的我们游戏 ！ 继续操作并置于您的头戴式签出的结果。 


<iframe height='300' scrolling='no' title='使用 Babylon.GUI-WebVR Babylon.js dino 游戏' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/RjgpJd/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>请参阅笔<a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/RjgpJd/'>Babylon.js dino 游戏使用 Babylon.GUI-WebVR</a>由 Microsoft 边缘文档 (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) 上<a href='https://codepen.io'>CodePen</a>。
</iframe>


## <a name="conclusion"></a>结束语

恭喜你！ 您现在已完成的 Babylon.js 游戏 WebVR 支持。 从此处，您可以采取所学生成更好的游戏，或生成此关闭。
