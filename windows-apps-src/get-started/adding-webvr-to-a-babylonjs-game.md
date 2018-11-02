---
title: 将 WebVR 支持添加到 3D Babylon.js 游戏
description: 了解如何将 WebVR 支持添加到现有的 3D Babylon.js 游戏。
author: abbycar
ms.author: abigailc
ms.date: 11/29/2017
ms.topic: article
keywords: webvr，edge，web 开发、 babylon、 babylonjs、 babylon.js javascript
ms.localizationpriority: medium
ms.openlocfilehash: 72681c3f91fc2dcbfcc4e4531359d6d668e18b80
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/02/2018
ms.locfileid: "5936336"
---
# <a name="adding-webvr-support-to-a-3d-babylonjs-game"></a>将 WebVR 支持添加到 3D Babylon.js 游戏

如果你已使用 Babylon.js 创建 3D 游戏，并认为它可能如下出色虚拟现实 (VR) 中，确保实现此教程中遵循简单的步骤。

我们将添加到游戏如下所示的 WebVR 支持。 继续并试用一下 Xbox 控制器插入 ！


<iframe height='300' scrolling='no' title='使用 Babylon.GUI Babylon.js 恐龙游戏' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/wrOvoj/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>请参阅笔<a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/wrOvoj/'>Babylon.js 恐龙游戏使用 Babylon.GUI</a> ，Microsoft Edge 文档 (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) 可以在<a href='https://codepen.io'>CodePen</a>上。
</iframe>

这是有关平面的屏幕，但什么良好工作的 3D 游戏 VR？
在本教程中，我们将演练几个步骤它采用若要获取此和使用 WebVR 运行。 我们将使用可以点击进入 WebVR 在 Microsoft Edge 中添加了对支持[Windows Mixed Reality](https://developer.microsoft.com/en-us/windows/mixed-reality)耳机。 我们将这些更改应用于游戏后，你可以预期它还支持 WebVR 其他浏览器/头戴显示设备组合中工作。



## <a name="prerequisites"></a>先决条件

- 在文本编辑器 （如[Visual Studio Code](https://code.visualstudio.com/download)）
- 插入到你的计算机 Xbox 控制器
- Windows 10 创意者更新
- 具有[所需的最小规格运行 Windows Mixed Reality](https://developer.microsoft.com/en-us/windows/mixed-reality/immersive_headset_setup)的计算机
- Windows Mixed Reality 设备 （可选） 



## <a name="getting-started"></a>入门

若要开始使用的最简单方法是访问[Windows 教程 web GitHub 存储库](https://github.com/Microsoft/Windows-tutorials-web)中，按绿色**克隆或下载**按钮，然后选择**在 Visual Studio 中打开**。

![克隆或下载按钮](images/3dclone.png)

如果不想克隆项目，则可以将其下载为 zip 文件。
然后，你将有两个文件夹、[之前](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before)和[之后](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/after)。 添加任何 VR 功能，以及"后"文件夹是 VR 支持的完成的游戏之前，"之前"文件夹将为我们的游戏。

之前和之后的文件夹包含这些文件：
-   **纹理 /** -包含任何游戏中使用的图像的文件夹。
-   **css /** -包含为该游戏的 CSS 的文件夹。
-   **js /** -包含 JavaScript 文件的文件夹。 Main.js 文件为我们的游戏，和其他文件为使用的库。
-   **模型 /** -包含 3D 模型的文件夹。 对于此游戏中，我们有只有一个模型中的，恐龙 3d 模型。
-   **index.html** -用于托管游戏的呈现器的网页。 在 Microsoft Edge 中打开此页面启动游戏。

你可以通过在 Microsoft Edge 中打开其各自的 index.html 文件来测试两个版本的游戏。



## <a name="the-mixed-reality-portal"></a>在混合的现实门户

如果你不熟悉 Windows Mixed Reality，并具有兼容的显卡的计算机上安装了 Windows 10 创意者更新，请尝试从 Windows 10 中的开始菜单打开**混合现实门户**应用。

![混合的现实门户搜索](images/mixed-reality-portal.png)

如果满足所有要求，然后可以打开开发人员功能并模拟 Windows Mixed Reality 耳机插入到你的计算机。 如果你不幸运不足以具有附近，实际头戴显示设备插入它，然后运行安装程序。

> [!IMPORTANT]
> 在本教程中始终必须打开在混合现实门户。

你现在已经准备好体验 WebVR 与 Microsoft Edge。

## <a name="2d-ui-in-a-virtual-world"></a>在虚拟世界中的 2D UI

>[!NOTE]
> 获取[**之前**](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before)文件夹，以获取初学者示例。

[Babylon.GUI](https://doc.babylonjs.com/how_to/gui) VR 友好库，使你要创建简单，交互式用户界面适合 VR 该工作和非 VR 显示。
一个扩展到 Babylon.js，`GUI`库是使用的 throuhout 该示例来创建 2D 元素。


2D 文本`GUI`可以使用几行，具体取决于你想要调整多少属性创建元素。
以下代码段都已经位于我们[**之前**](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before)的示例中，但我们正在发生的操作实例。
首先，我们将[`AdvancedDynamicTexture`](https://doc.babylonjs.com/how_to/gui#advanceddynamictexture)若要建立 GUI 将介绍的对象。 该示例将此设置为`CreateFullScreenUI()`，这意味着我们的 UI 将覆盖整个屏幕。 使用`AdvancedDynamicTexture`我们创建，然后进行 2D 文本框中，游戏使用启动后，会显示`GUI.Rectanlge()`和`GUI.TextBlock()`。


在[**main.js**](https://github.com/Microsoft/Windows-tutorials-web/blob/master/BabylonJS-game-with-WebVR/before/js/main.js#L157-L168)中添加此代码。
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


此 UI 是可见后创建，但可以通过打开或关闭切换`isVisible`具体取决于在游戏中发生的情况。
```javascript
startUI.isVisible = false;
```



## <a name="detecting-headsets"></a>检测头戴显示设备

最好 VR 应用程序，以便可以支持多个方案有两种类型的相机。 对于此游戏，我们将支持一个需要使用耳机接通，而另一个使用没有头戴显示设备的相机。 若要确定哪一个游戏将使用，我们必须首先检查以查看是否已检测到耳机。 若要执行该操作，我们将使用[`navigator.getVRDisplays()`](https://developer.mozilla.org/en-US/docs/Web/API/Navigator/getVRDisplays)。


添加此代码上述`window.addEventListener('DOMContentLoaded')` **main.js**中。
```javascript
var headset;
// If a VR headset is connected, get its info
navigator.getVRDisplays().then(function (displays) {
    if (displays[0]) {
        headset = displays[0];
    }
});
```

信息存储在`headset`变量，我们现在能够选择最适合用户的相机。


## <a name="creating-and-selecting-the-initial-camera"></a>创建并选择初始相机

与 Babylon.js、 WebVR 可以快速补位通过使用[`WebVRFreeCamera`](http://doc.babylonjs.com/classes/3.1/webvrfreecamera)。 此相机可能键盘输入，并且使你能够使用 VR 头戴显示设备来控制"标头"旋转。


### <a name="step-1-checking-for-headsets"></a>步骤 1： 检查头戴显示设备

对于我们的回退摄像头，我们将使用[`UniversalCamera`](https://doc.babylonjs.com/classes/3.1/universalcamera)，当前使用原始游戏中。

我们将查看我们`headset`变量，以确定是否我们可以使用`WebVRFreeCamera`相机。

替换`camera = new BABYLON.UniversalCamera("Camera", new BABYLON.Vector3(0, 18, -45), scene);`使用下面的代码。
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
若要激活此相机大多数浏览器中的，用户必须执行一些交互的请求的虚拟体验。
我们将挂钩取决于鼠标单击此功能。


将代码粘贴到内`createScene()`函数后`camera.applyGravity = true;`。
```javascript
        scene.onPointerDown = function () {
            scene.onPointerDown = undefined
            camera.attachControl(canvas, true);
        }
```

在游戏中的单击现在创建一条提示，如下所示，或显示游戏头戴显示设备中立即如果用户已接受之前的提示。

![沉浸式提示](images/immersiveview.png)

我们还可以添加代码，将显示一条`UniversalCamera`我们切换到之前查看我们`WebVRFreeCamera`，从而允许用户看一下游戏而不是一个蓝色窗口。 

添加后的进行以下`engine.runRenderLoop(function () {`。
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

由于`WebVRFreeCamera`最初不支持游戏板，我们会将我们的游戏板按钮映射到键盘箭头键。 我们将执行此操作很快找到它们到`inputs`相机的属性。 通过添加左摇杆的相应代码向上、 向下、 左和向右以符合箭头键时，我们的游戏板是重新登录操作。


将此代码下添加`scene.onPointerDown = function() {...}`调用。
``` javascript
    // Custom input, adding Xbox controller support for left analog stick to map to keyboard arrows
    camera.inputs.attached.keyboard.keysUp.push(211);    // Left analog up
    camera.inputs.attached.keyboard.keysDown.push(212);  // Left analog down
    camera.inputs.attached.keyboard.keysLeft.push(214);  // Left analog left
    camera.inputs.attached.keyboard.keysRight.push(213); // Left analog right
```


### <a name="step-4-give-it-a-try"></a>步骤 4： 试一试 ！

如果我们使用我们头戴显示设备打开**index.html** ，游戏控制器已接通电源蓝色游戏窗口左侧的单击将切换到 VR 模式的我们的游戏 ！ 继续并放在头戴显示设备，以查看结果。 


<iframe height='300' scrolling='no' title='使用 Babylon.GUI-WebVR Babylon.js 恐龙游戏' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/RjgpJd/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>请参阅笔<a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/RjgpJd/'>Babylon.js 恐龙游戏使用 Babylon.GUI-WebVR</a> ，Microsoft Edge 文档 (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) 可以在<a href='https://codepen.io'>CodePen</a>上。
</iframe>


## <a name="conclusion"></a>总结

恭喜你！ 你现在可以使用 WebVR 支持的完整 Babylon.js 游戏。 在这里，你可以采取你学生成更好的游戏，或关闭此生成。
