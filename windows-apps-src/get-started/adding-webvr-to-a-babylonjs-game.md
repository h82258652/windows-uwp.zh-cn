---
title: 将 WebVR 支持添加到 3D Babylon.js 游戏
description: 了解如何将 WebVR 支持添加到现有的 3D Babylon.js 游戏。
ms.date: 11/29/2017
ms.topic: article
keywords: webvr, edge, web 开发, babylon, babylonjs, babylon.js, javascript
ms.localizationpriority: medium
ms.openlocfilehash: 1d8029752790e19adc5eb4266615372fb346e001
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57638552"
---
# <a name="adding-webvr-support-to-a-3d-babylonjs-game"></a>将 WebVR 支持添加到 3D Babylon.js 游戏

如果你已使用 Babylon.js 创建了 3D 游戏，并认为它可能在虚拟现实 (VR) 中看起来很棒，请按照此教程中的几个简单步骤将这一想法变为现实。

我们将 WebVR 支持添加到此处显示的游戏。 继续操作并插入 Xbox 控制器来尝试！


<iframe height='300' scrolling='no' title='使用 Babylon.GUI Babylon.js dino 游戏' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/wrOvoj/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>请参阅笔<a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/wrOvoj/'>Babylon.js dino 游戏使用 Babylon.GUI</a>通过 Microsoft Edge 文档 (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) 上<a href='https://codepen.io'>CodePen</a>。
</iframe>

这是一个 3D 游戏，在平面屏幕上效果很好，但在 VR 中将如何？
在此教程中，我们将演练实现此想法并使用 WebVR 运行的几个步骤。 我们将使用可以在 Microsoft Edge 中利用 WebVR 附加支持的 [Windows Mixed Reality](https://developer.microsoft.com/en-us/windows/mixed-reality) 耳机。 我们将这些更改应用于游戏后，你还可以期待在支持 WebVR 的其他浏览器/耳机组合中使用。



## <a name="prerequisites"></a>必备条件

- 文本编辑器（如 [Visual Studio Code](https://code.visualstudio.com/download)）
- 插入到你的计算机的 Xbox 控制器
- Windows 10 Creators 更新
- 达到[运行 Windows Mixed Reality 的最低规格要求](https://developer.microsoft.com/en-us/windows/mixed-reality/immersive_headset_setup)的计算机
- Windows Mixed Reality 设备（可选） 



## <a name="getting-started"></a>即刻体验

最简单的入门方法是访问 [Windows-tutorials-web GitHub 存储库](https://github.com/Microsoft/Windows-tutorials-web)，按绿色**克隆或下载**按钮，然后选择**在 Visual Studio 中打开**。

![克隆或下载按钮](images/3dclone.png)

如果不想克隆项目，则可以将其下载为 zip 文件。
然后，你将有两个文件夹，[before](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before) 和 [after](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/after)。 “before”文件夹是我们添加任何 VR 功能以前的游戏，“after”文件夹是具有 VR 支持的完成的游戏。

“before”和“after”文件夹包含以下文件：
-   **纹理 /** -包含游戏中使用的任何映像的文件夹。
-   **css /** -包含有关游戏的 CSS 的文件夹。
-   **js /** -包含 JavaScript 文件的文件夹。 main.js 文件为我们的游戏，其他文件为使用的库。
-   **模型 /** -包含三维模型的文件夹。 在本游戏中，我们只有一个恐龙模型。
-   **index.html** -托管游戏的呈现器的网页。 在 Microsoft Edge 中打开此页将启动游戏。

可以通过在 Microsoft Edge 中打开其各自的 index.html 文件来测试这两个版本的游戏。



## <a name="the-mixed-reality-portal"></a>混合现实门户

如果你不熟悉 Windows Mixed Reality，并且已在具有兼容显卡的计算机上安装了 Windows 10 创意者更新，请尝试从 Windows 10 的“开始”菜单打开**混合现实门户**应用。

![混合现实门户搜索](images/mixed-reality-portal.png)

如果你达到所有要求，那么可以打开开发人员功能，并模拟插入到你的计算机中的 Windows Mixed Reality 耳机。 如果你足够幸运，手边有一个真正的耳机，将它插入，然后运行安装程序。

> [!IMPORTANT]
> 在学习此教程期间，必须始终打开混合现实门户。

现在，你准备就绪，从而可以享有使用 Microsoft Edge WebVR。

## <a name="2d-ui-in-a-virtual-world"></a>虚拟世界中的 2D UI

>[!NOTE]
> 抓住[**之前**](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before)文件夹，以获取初学者示例。

[Babylon.GUI](https://doc.babylonjs.com/how_to/gui)是 VR 友好库，从而使您若要创建简单，交互式用户界面，非常适合 VR 和非 VR 显示。
扩展到 Babylon.js，`GUI`库是使用的 throuhout 示例以创建 2D 元素。


2D 文本`GUI`可以使用几行，具体取决于你想要调整的多少属性创建元素。
下面的代码段已存在于我们[**之前**](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before)示例，但让我们演练发生了什么情况。
我们会首先将[ `AdvancedDynamicTexture` ](https://doc.babylonjs.com/how_to/gui#advanceddynamictexture)对象建立 GUI 将包含的内容。 该示例将其设置为`CreateFullScreenUI()`，这意味着我们的 UI 将覆盖整个屏幕。 与`AdvancedDynamicTexture`创建，然后我们使启动游戏使用时将显示一个 2D 文本框`GUI.Rectanlge()`和`GUI.TextBlock()`。


在添加此代码[ **main.js**](https://github.com/Microsoft/Windows-tutorials-web/blob/master/BabylonJS-game-with-WebVR/before/js/main.js#L157-L168)。
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


此用户界面中，一旦创建，但可以通过打开或关闭切换可见`isVisible`具体取决于该游戏中发生的情况。
```javascript
startUI.isVisible = false;
```



## <a name="detecting-headsets"></a>检测耳机

它是很好的做法 VR 应用程序，以便可以支持多个方案有两种类型的照相机。 对于此游戏，我们将支持一个需要插入工作耳机的摄像头，还有另一个不使用耳机的摄像头。 若要确定游戏将使用哪一个，我们必须先查看是否已检测到耳机。 为此，我们将使用[ `navigator.getVRDisplays()` ](https://developer.mozilla.org/en-US/docs/Web/API/Navigator/getVRDisplays)。


添加上面这段代码`window.addEventListener('DOMContentLoaded')`中**main.js**。
```javascript
var headset;
// If a VR headset is connected, get its info
navigator.getVRDisplays().then(function (displays) {
    if (displays[0]) {
        headset = displays[0];
    }
});
```

利用存储在 `headset` 变量中的信息，我们现在可以选择适合用户的摄像头。


## <a name="creating-and-selecting-the-initial-camera"></a>创建和选择的初始相机

可以通过使用 Babylon.js，WebVR 快速添加[ `WebVRFreeCamera` ](https://doc.babylonjs.com/classes/3.1/webvrfreecamera)。 此摄像头可以采取键盘输入，使你可以使用 VR 耳机来控制你的“头”旋转。


### <a name="step-1-checking-for-headsets"></a>第 1 步：检查耳机

对于我们回退照相机，我们将使用[ `UniversalCamera` ](https://doc.babylonjs.com/classes/3.1/universalcamera)当前使用的原始游戏中。

我们将首先检查我们`headset`变量以确定我们是否可以使用`WebVRFreeCamera`照相机。

将 `camera = new BABYLON.UniversalCamera("Camera", new BABYLON.Vector3(0, 18, -45), scene);` 替换为下面的代码。
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


### <a name="step-2-activating-the-webvrfreecamera"></a>步骤 2：激活 WebVRFreeCamera
若要在大多数浏览器中激活此摄像头，用户必须执行一些请求虚拟体验的交互。
我们将挂接到鼠标单击此功能。


将此代码粘贴在 `camera.applyGravity = true;` 后的 `createScene()` 函数内。
```javascript
        scene.onPointerDown = function () {
            scene.onPointerDown = undefined
            camera.attachControl(canvas, true);
        }
```

在游戏中的单击现在创建如下所示，在提示符下或显示游戏的耳机立即如果用户已接受之前的提示。

![沉浸式提示](images/immersiveview.png)

我们还可以添加一段代码将显示`UniversalCamera`查看之后，我们切换到我们`WebVRFreeCamera`，这样就允许用户查看而不是蓝色窗口游戏。 

后面添加以下`engine.runRenderLoop(function () {`。
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

### <a name="step-3-adding-gamepad-support"></a>步骤 3:添加游戏板支持

由于`WebVRFreeCamera`最初不支持游戏板，我们会将我们的游戏板按钮映射到键盘上的箭头键。 将为此，我们深入探讨`inputs`的照相机的属性。 通过为左摇杆向上、向下、向左和向右添加相应的代码来匹配箭头键，我们的游戏板已经回归。


下添加以下代码`scene.onPointerDown = function() {...}`调用。
``` javascript
    // Custom input, adding Xbox controller support for left analog stick to map to keyboard arrows
    camera.inputs.attached.keyboard.keysUp.push(211);    // Left analog up
    camera.inputs.attached.keyboard.keysDown.push(212);  // Left analog down
    camera.inputs.attached.keyboard.keysLeft.push(214);  // Left analog left
    camera.inputs.attached.keyboard.keysRight.push(213); // Left analog right
```


### <a name="step-4-give-it-a-try"></a>步骤 4：请试一试 ！

如果我们打开**index.html**与我们的耳机和游戏控制器插入，蓝色的游戏窗口的左侧的单击将到 VR 模式切换我们的游戏 ！ 继续游戏，戴上你的耳机，一试效果。 


<iframe height='300' scrolling='no' title='使用 Babylon.GUI-WebVR Babylon.js dino 游戏' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/RjgpJd/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>请参阅笔<a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/RjgpJd/'>Babylon.js dino 游戏使用 Babylon.GUI-WebVR</a>通过 Microsoft Edge 文档 (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) 上<a href='https://codepen.io'>CodePen</a>。
</iframe>


## <a name="conclusion"></a>结论

祝贺你！ 你现在已经完成了拥有 WebVR 支持的 Babylon.js 游戏的构建。 从这里，你可以利用所学到的知识构建更好的游戏，或继续完善这个游戏。
