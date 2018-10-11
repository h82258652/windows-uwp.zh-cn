---
title: 用 MonoGame 2D 创建 UWP 游戏
description: 简单的 UWP 游戏的 Microsoft 应用商店、 用 C# 和 MonoGame 编写
author: muhsinking
ms.author: mukin
ms.date: 03/06/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 5d5f7af2-41a9-4749-ad16-4503c64bb80c
ms.localizationpriority: medium
ms.openlocfilehash: d38465ce02e0aedf854094ede75fc33701b226a6
ms.sourcegitcommit: 8e30651fd691378455ea1a57da10b2e4f50e66a0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/10/2018
ms.locfileid: "4504101"
---
# <a name="create-a-uwp-game-in-monogame-2d"></a>用 MonoGame 2D 创建 UWP 游戏

## <a name="a-simple-2d-uwp-game-for-the-microsoft-store-written-in-c-and-monogame"></a>适用于 Microsoft Store、用 C# 和 MonoGame 编写的简单 2D UWP 游戏


![行走的恐龙的子画面表](images/JS2D_0.png)

## <a name="introduction"></a>简介

MonoGame 是一款轻型游戏开发框架。 本教程介绍了用 MonoGame 进行游戏开发的基础知识，包括如何加载内容、绘制子画面、添加动画效果和处理用户输入。 此外，还介绍了一些更高级的概念，如碰撞检测和对高分辨率屏幕按比例缩放等。 本教程需要 30-60 分钟。

## <a name="prerequisites"></a>必备条件
+   Windows 10 和 Microsoft Visual Studio 2017。  [单击此处了解如何设置 Visual Studio](https://docs.microsoft.com/en-us/windows/uwp/get-started/get-set-up)。
+ .NET 的桌面开发框架。 如果你没有安装，你可以通过重新运行 Visual Studio 安装程序和修改你安装的 Visual Studio 2017 获取它。
+   C# 或类似面向对象的编程语言的基础知识。 [单击此处以了解如何开始使用 C#](https://docs.microsoft.com/en-us/windows/uwp/get-started/create-a-hello-world-app-xaml-universal)。
+   熟悉基本的计算机科学概念如类、方法以及变量，将有所帮助。

## <a name="why-monogame"></a>为什么选择 MonoGame？
谈及游戏开发环境时，我们并不缺少选择。 从功能齐全的引擎（如 Unity）到全面、复杂的多媒体 API（如 DirectX），但这些都让人很难知道从何处入手。 MonoGame 是一组工具，其复杂度处于游戏引擎与更专业的 API（如 DirectX）之间。 它提供了易于使用的内容管道，以及用于创建在各种平台上运行的轻型游戏所需的所有功能。 最重要的是，MonoGame 应用用纯 C# 编写，并通过 Microsoft 应用商店或其他类似的分发平台，可以快速分发它们。

## <a name="get-the-code"></a>获取代码
如果你不想跟着教程逐步学习，只想了解 MonoGame 的实际应用，[请单击此处以获得完成后的应用](https://github.com/Microsoft/Windows-appsample-get-started-mg2d)。

在 Visual Studio 2017 中，打开项目，并按**F5**以运行样本。 首次进行此操作可能会需要一段时间，因为 Visual Studio 需要提取安装中缺少的所有 NuGet 包。

如果已完成此操作，则请跳至下一节“设置 MonoGame”，以查看代码的分步操作实例。

**注意：** 此示例中创建的游戏并不完整（或完全没有乐趣）。 其唯一目的是为了演示用 MonoGame 2d 游戏开发的所有核心概念。 你可随意使用此代码并进行改进，或者在掌握基础知识之后从头开始。

## <a name="set-up-monogame-project"></a>设置 MonoGame 项目
1. 从 [MonoGame.net](http://www.monogame.net/) 中安装适用于 Visual Studio 的 **MonoGame 3.6**。

2. 启动 Visual Studio 2017。

3. 转到**文件 -> 新建 -> 项目**

4. 在 Visual C# 项目模板下，选择 **MonoGame** 和 **MonoGame Windows 10 通用项目**

5. 将项目命名为“MonoGame2D”并选择“确定”。 创建好项目之后，它可能看上去会像错误百出，但在首次运行此项目并安装完所有缺少的 NuGet 包之后，这种情况就会消失。

6. 确保已将 **x86** 和**本地计算机**设为目标平台，然后按 **F5** 构建并运行空项目。 如已遵循以上步骤，则你应该会在构建完项目之后看到一个空的蓝色窗口。

## <a name="method-overview"></a>方法概述
创建完项目之后，现在请在**解决方案资源管理器**中打开 **Game1.cs**。 这是大多数游戏逻辑的存储位置。 创建新的 MonoGame 项目时，将在此自动生成许多关键方法。 下面我们来快速了解这些内容：

**public Game1()** 构造函数。 在本教程中，我们不会对此方法进行任何更改。

**protected override void Initialize()** 我们将在此初始化所使用的任何类变量。 游戏开始时将会调用此方法一次。

**protected override void LoadContent()** 此方法用于在游戏开始之前将内容（如 纹理、音频、字体）加载到内存。 和初始化一样，应用启动时也会调用此方法一次。

**protected override void UnloadContent()** 此方法用于卸载非内容管理器内容。 我们不会用到此方法。

**protected override void Update(GameTime gameTIme)** 每个游戏循环周期都会调用此方法一次。 我们在此更新游戏中使用的任何对象或变量的状态。 这包括对象的位置、速度或颜色等等。 这也是处理用户输入的地方。 简而言之，此方法用于处理游戏逻辑的每个部分（在屏幕上绘制对象除外）。
**protected override void Draw(GameTime gameTime)** 这是在屏幕上使用“Update”方法提供的位置绘制对象的地方。

## <a name="draw-a-sprite"></a>绘制子画面
你已运行新的 MonoGame 项目并看到了美丽的蓝天，让我们再添加一些地面背景。
在 MonoGame 中，2D 艺术将以“子画面”形式添加到应用中。 子画面只是一种作为单一实体操纵的计算机图形。 可以对子画面实施移动、缩放、成形、添加动画效果和组合操作，以在 2D 空间中创建你能想象到的任何效果。

### <a name="1-download-a-texture"></a>1. 下载纹理
对我们来说，第一个子画面会非常乏味。 [单击此处以下载此无特色的绿色矩形](https://github.com/Microsoft/Windows-appsample-get-started-mg2d/blob/master/MonoGame2D/Content/grass.png)。

### <a name="2-add-the-texture-to-the-content-folder"></a>2. 为“Content”文件夹添加纹理
- 打开**解决方案资源管理器**
- 右键单击 **Content** 文件夹中的** Content.mgcb** 并选择**打开方式**。 从弹出菜单中选择 **Monogame 管道**，并选择**确定**。
- 在新窗口中，右键单击**内容**项并选择**添加 -> 现有项目**。
- 在文件浏览器中查找并选择绿色矩形。
- 将此项命名为“grass.png”并选择**添加**。

### <a name="3-add-class-variables"></a>3. 添加类变量
若要将此图像加载为子画面纹理，请打开 **Game1.cs** 并添加以下类变量。

```CSharp
const float SKYRATIO = 2f/3f;
float screenWidth;
float screenHeight;
Texture2D grass;
```

SKYRATIO 变量用于表示场景中的蓝天与草地比例，在本案例中为 2/3。 **screenWidth** 和 **screenHeight** 用于跟踪应用窗口的大小，而 **grass** 则是我们存储绿色矩形的地方。

### <a name="4-initialize-class-variables-and-set-window-size"></a>4. 初始化类变量并设置窗口大小
**screenWidth** 和 **screenHeight** 变量仍需初始化，因此请将此代码添加到 **Initialize** 方法：

```CSharp
ApplicationView.PreferredLaunchWindowingMode = ApplicationViewWindowingMode.FullScreen;

screenHeight = (float)ApplicationView.GetForCurrentView().VisibleBounds.Height;
screenWidth = (float)ApplicationView.GetForCurrentView().VisibleBounds.Width;

this.IsMouseVisible = false;
```

除了获得屏幕的高度和宽度外，我们还可以将应用窗口模式设为**全屏**并隐藏鼠标。

### <a name="5-load-the-texture"></a>5. 加载纹理
若要将纹理加载到草地变量中，请将以下内容添加到 **LoadContent** 方法：

```CSharp
grass = Content.Load<Texture2D>("grass");
```

### <a name="6-draw-the-sprite"></a>6. 绘制子画面
若要绘制矩形，请将以下行添加到 **Draw** 方法：

```CSharp
GraphicsDevice.Clear(Color.CornflowerBlue);
spriteBatch.Begin();
spriteBatch.Draw(grass, new Rectangle(0, (int)(screenHeight * SKYRATIO),
  (int)screenWidth, (int)screenHeight), Color.White);
spriteBatch.End();
```

在这里，我们使用 **spriteBatch.Draw** 方法来将给定的纹理置于矩形对象的边界内。 **矩形**由通过左上角和右下角的 x 和 y 坐标定义。 使用先前定义的 **screenWidth**、**screenHeight** 和 **SKYRATIO** 变量，我们可以在屏幕下方 1/3 处绘制绿色矩形纹理。 如果运行程序，则你现在应该可以看到先前的蓝色背景已被绿色矩形部分覆盖。

![绿色矩形](images/monogame-tutorial-1.png)

## <a name="scale-to-high-dpi-screens"></a>为高分辨率屏幕进行缩放
如果在高像素监视器上运行 Visual Studio，如 Surface Pro 或 Surface Studio 上的监视器，则你会发现以上步骤中的绿色矩形不会完全覆盖屏幕下方的 1/3。 它可能会悬浮在屏幕的左下角上。 若要修复此情况并为所有设备提供统一的游戏体验，我们需要创建一种能够根据屏幕的像素缩放某些值的方法。

```CSharp
public float ScaleToHighDPI(float f)
{
  DisplayInformation d = DisplayInformation.GetForCurrentView();
  f *= (float)d.RawPixelsPerViewPixel;
  return f;
}
```

接下来，将 **Initialize** 方法中的 **screenHeight** 和 **screenWidth** 初始化值替换为以下值：

```CSharp
screenHeight = ScaleToHighDPI((float)ApplicationView.GetForCurrentView().VisibleBounds.Height);
screenWidth = ScaleToHighDPI((float)ApplicationView.GetForCurrentView().VisibleBounds.Width);
```

如果使用的是高分辨率屏幕并尝试马上运行应用，则你应该会看到绿色矩形已按预期覆盖住屏幕下方的 1/3。

## <a name="build-the-spriteclass"></a>构建 SpriteClass
在开始为子画面添加动画效果之前，我们将制作一个名为“SpriteClass”的新类，使我们能够降低子画面操纵的表层复杂性。

### <a name="1-create-a-new-class"></a>1. 创建新类
在**解决方案资源管理器**中，右键单击 **MonoGame2D（通用 Windows）** 并选择**添加 -> 类**。 将该类命名为“SpriteClass.cs”，然后选择**添加**。

### <a name="2-add-class-variables"></a>2. 添加类变量
将此类添加到刚创建的类中：

```CSharp
public Texture2D texture
{
  get;
}

public float x
{
  get;
  set;
}

public float y
{
  get;
  set;
}

public float angle
{
  get;
  set;
}

public float dX
{
  get;
  set;
}

public float dY
{
  get;
  set;
}

public float dA
{
  get;
  set;
}

public float scale
{
  get;
  set;
}
```

在这里，我们将设置用于绘制子画面并为其添加动画效果所需的类变量。 **x** 和 **y** 变量表示子画面当前在屏幕上的位置，而 **angle** 变量则表示子画面当前的角度（0 表示垂直，90 表示顺时针倾斜 90 度）。 请注意，对于此类，**x** 和 **y** 表示子画面**中心**的坐标（默认原点为左上角）。 这使得旋转子画面更加容易，因为无论原点位于何处，子画面均可旋转，并且围绕中心旋转还能给我们提供统一的旋转运动。

随后，我们将设置 **dX**、**dY** 和 **dA**，它们分别表示 **x**、**y** 和 **angle** 变量的每秒变化率。

### <a name="3-create-a-constructor"></a>3. 创建构造函数
创建 **SpriteClass** 实例时，我们通过 **Game1.cs** 为图形设备提供构造函数、纹理相对于项目文件夹的路径以及纹理相对于其原始尺寸的缩放比例。 开始游戏后，我们需要设置“Update”方法中的其他类变量。

```CSharp
public SpriteClass (GraphicsDevice graphicsDevice, string textureName, float scale)
{
  this.scale = scale;
  if (texture == null)
  {
    using (var stream = TitleContainer.OpenStream(textureName))
    {
      texture = Texture2D.FromStream(graphicsDevice, stream);
    }
  }
}
```

### <a name="4-update-and-draw"></a>4. 更新和绘制
我们还需将几个方法添加到 SpriteClass 声明中：

```CSharp
public void Update (float elapsedTime)
{
  this.x += this.dX * elapsedTime;
  this.y += this.dY * elapsedTime;
  this.angle += this.dA * elapsedTime;
}

public void Draw (SpriteBatch spriteBatch)
{
  Vector2 spritePosition = new Vector2(this.x, this.y);
  spriteBatch.Draw(texture, spritePosition, null, Color.White, this.angle, new Vector2(texture.Width/2, texture.Height/2), new Vector2(scale, scale), SpriteEffects.None, 0f);
}
```

Game1.cs 中的 **Update** 方法调用 **Update** SpriteClass 方法，后者用于根据其各自的变化率更新其子画面的 **x**、**y** 和 **angle** 值。

Game1.cs 中的 **Draw** 方法调用 **Draw** 方法，后者用于在游戏窗口中绘制子画面。

## <a name="user-input-and-animation"></a>用户输入和动画
现在，我们已经构建了 SpriteClass，接下来我们将用其创建两个新的游戏对象：第一个是玩家可通过箭头键和空格键控制的头像。 第二个是玩家必须避让的对象

### <a name="1-get-the-textures"></a>1. 获取纹理
对于玩家的头像，我们将使用 Microsoft 独有的忍者神猫，它骑在它信赖的霸王龙身上。 [单击此处以下载此图像](https://github.com/Microsoft/Windows-appsample-get-started-mg2d/blob/master/MonoGame2D/Content/ninja-cat-dino.png)。

现在就来谈谈玩家需要避让的障碍物。 忍者神猫和肉食恐龙都最讨厌什么呢？ 吃蔬菜！ [单击此处以下载此图像](https://github.com/Microsoft/Windows-appsample-get-started-mg2d/blob/master/MonoGame2D/Content/broccoli.png)。

Just as before with the green rectangle, add these images to **Content.mgcb** via the **MonoGame Pipeline**, naming them “ninja-cat-dino.png” and “broccoli.png” respectively.

### <a name="2-add-class-variables"></a>2. 添加类变量
将以下代码添加到 **Game1.cs** 中的类变量列表中：

```CSharp
SpriteClass dino;
SpriteClass broccoli;

bool spaceDown;
bool gameStarted;

float broccoliSpeedMultiplier;
float gravitySpeed;
float dinoSpeedX;
float dinoJumpY;
float score;

Random random;
```

**dino** and **broccoli** are our SpriteClass variables. **dino** will hold the player avatar, while **broccoli** holds the broccoli obstacle.

**spaceDown** 用于跟踪空格键是被按住还是被按下后再松开。

**gameStarted** 用于告诉我们用户是否已第一次开始游戏。

**broccoliSpeedMultiplier** determines how fast the broccoli obstacle moves across the screen.

**gravitySpeed** 用于确定玩家头像在跳跃后加速下降的速度。

**dinoSpeedX** 和 **dinoJumpY** 用于确定玩家头像移动和跳跃的速度。
得分用于跟踪玩家已成功躲避的障碍物数量。

最后，**random** 用于为花椰菜障碍物的行为添加一些随机性。

### <a name="3-initialize-variables"></a>3. 初始化变量
接下来，我们需要初始化这些变量。 将以下代码添加到“Initialize”方法中：

```CSharp
broccoliSpeedMultiplier = 0.5f;
spaceDown = false;
gameStarted = false;
score = 0;
random = new Random();
dinoSpeedX = ScaleToHighDPI(1000f);
dinoJumpY = ScaleToHighDPI(-1200f);
gravitySpeed = ScaleToHighDPI(30f);
```

请注意，对于高分辨率设备，需要缩放最后三个变量，因为它们是用于指定像素变化率的。

### <a name="4-construct-spriteclasses"></a>4. 构建 SpriteClasses
我们将在 **LoadContent** 方法中构建 SpriteClass 对象。 将此代码添加到已有对象中：

```CSharp
dino = new SpriteClass(GraphicsDevice, "Content/ninja-cat-dino.png", ScaleToHighDPI(1f));
broccoli = new SpriteClass(GraphicsDevice, "Content/broccoli.png", ScaleToHighDPI(0.2f));
```

花椰菜在游戏中的显示大小比我们期望的大得多，因此，我们需要将其缩小至其原始尺寸的 1/5。

### <a name="5-program-obstacle-behavior"></a>5. 设置障碍物行为
我们希望花椰菜在屏幕以外的地方繁殖，并朝玩家头像的方向生长，从而使玩家需要躲避它。 要实现此效果，请将此方法添加到 **Game1.cs** 类：

```CSharp
public void SpawnBroccoli()
{
  int direction = random.Next(1, 5);
  switch (direction)
  {
    case 1:
      broccoli.x = -100;
      broccoli.y = random.Next(0, (int)screenHeight);
      break;
    case 2:
      broccoli.y = -100;
      broccoli.x = random.Next(0, (int)screenWidth);
      break;
    case 3:
      broccoli.x = screenWidth + 100;
      broccoli.y = random.Next(0, (int)screenHeight);
      break;
    case 4:
      broccoli.y = screenHeight + 100;
      broccoli.x = random.Next(0, (int)screenWidth);
      break;
  }

  if (score % 5 == 0) broccoliSpeedMultiplier += 0.2f;

  broccoli.dX = (dino.x - broccoli.x) * broccoliSpeedMultiplier;
  broccoli.dY = (dino.y - broccoli.y) * broccoliSpeedMultiplier;
  broccoli.dA = 7f;
}
```

此方法的第一个部分用于通过两个随机数字来确定花椰菜对象在屏幕以外开始繁殖的点。

第二部分用于确定花椰菜的生长速度，这由当前的得分决定。 玩家每成功躲避 5 个花椰菜，其生长速度就会变得更快。

第三部分用于设置花椰菜子画面的移动方向。 花椰菜繁殖时，它会朝玩家头像（恐龙）的方向生长。 此外，我们还将 **dA** 值设为 7f，使得花椰菜在追逐玩家时在空中旋转。

### <a name="6-program-game-starting-state"></a>6. 设置游戏开始状态
在处理键盘输入之前，我们需要一个方法，以便为我们创建的两个对象设置初始游戏状态。 除了在应用运行时就立即开始游戏外，我们希望用户通过按空格键来手动开始游戏。 添加以下代码，以设置动画对象的初始状态并重置得分：

```CSharp
public void StartGame()
{
  dino.x = screenWidth / 2;
  dino.y = screenHeight * SKYRATIO;
  broccoliSpeedMultiplier = 0.5f;
  SpawnBroccoli();  
  score = 0;
}
```

### <a name="7-handle-keyboard-input"></a>7.处理键盘输入
接下来，我们需要通过键盘处理用户输入的新方法。 将此方法添加到 **Game1.cs**：

```CSharp
void KeyboardHandler()
{
  KeyboardState state = Keyboard.GetState();

  // Quit the game if Escape is pressed.
  if (state.IsKeyDown(Keys.Escape))
  {
    Exit();
  }

  // Start the game if Space is pressed.
  if (!gameStarted)
  {
    if (state.IsKeyDown(Keys.Space))
    {
      StartGame();
      gameStarted = true;
      spaceDown = true;
      gameOver = false;
    }
    return;
  }            
  // Jump if Space is pressed
  if (state.IsKeyDown(Keys.Space) || state.IsKeyDown(Keys.Up))
  {
    // Jump if the Space is pressed but not held and the dino is on the floor
    if (!spaceDown && dino.y >= screenHeight * SKYRATIO - 1) dino.dY = dinoJumpY;

    spaceDown = true;
  }
  else spaceDown = false;

  // Handle left and right
  if (state.IsKeyDown(Keys.Left)) dino.dX = dinoSpeedX * -1;

  else if (state.IsKeyDown(Keys.Right)) dino.dX = dinoSpeedX;
  else dino.dX = 0;
}
```

首先，我们需要使用四个 IF 语句：

如果按下 **Escape** 键，则第一个将退出游戏。

如果在游戏尚未开始时按下**空格**键，则第二个将会开始游戏。

如果按下**空格**键，则第三个将会通过更改其 **dY** 属性来使恐龙头像跳跃。 请注意，如果不在“地面”上（dino.y = screenHeight * SKYRATIO），则玩家无法跳跃，此外，如果是长按空格键而不是只按一次的话，玩家也无法跳跃。 游戏开始后，这将使恐龙无法跳跃，通过按下相同的按键即可开始游戏。

最后，最后一个条件从句用于检查是否按下向左或向右方向箭头，如果是，则相应地更改恐龙的 **dX** 属性。

**挑战：** 你能否使上面的键盘处理方法能够处理 WASD 输入方案以及箭头键？

### <a name="8-add-logic-to-the-update-method"></a>8. 为“Update”方法添加逻辑
接下来，我们需要将所有这些部分的逻辑添加到 **Game1.cs** 的 **Update** 方法中：

```CSharp
float elapsedTime = (float)gameTime.ElapsedGameTime.TotalSeconds;
KeyboardHandler(); // Handle keyboard input
// Update animated SpriteClass objects based on their current rates of change
dino.Update(elapsedTime);
broccoli.Update(elapsedTime);

// Accelerate the dino downward each frame to simulate gravity.
dino.dY += gravitySpeed;

// Set game floor so the player does not fall through it
if (dino.y > screenHeight * SKYRATIO)
{
  dino.dY = 0;
  dino.y = screenHeight * SKYRATIO;
}

// Set game edges to prevent the player from moving offscreen
if (dino.x > screenWidth - dino.texture.Width/2)
{
  dino.x = screenWidth - dino.texture.Width/2;
  dino.dX = 0;
}
if (dino.x < 0 + dino.texture.Width/2)
{
  dino.x = 0 + dino.texture.Width/2;
  dino.dX = 0;
}

// If the broccoli goes offscreen, spawn a new one and iterate the score
if (broccoli.y > screenHeight+100 || broccoli.y < -100 || broccoli.x > screenWidth+100 || broccoli.x < -100)
{
  SpawnBroccoli();
  score++;
}
```

### <a name="9-draw-spriteclass-objects"></a>9. 绘制 SpriteClass 对象
最后，在最后调用 **spriteBatch.Draw**后，请将以下代码添加到 **Game1.cs** 的 **Draw** 方法中：

```CSharp
broccoli.Draw(spriteBatch);
dino.Draw(spriteBatch);
```

在 MonoGame 中，对 **spriteBatch.Draw** 的新调用将会覆盖任何先前的调用。 这意味着，花椰菜和恐龙子画面将覆盖现有的草地子画面，因此，无论其位置如何，他们都不会再隐藏在草地后面。

现在尝试运行游戏，并通过箭头键和空格键移动恐龙。 如果遵照以上步骤，则你应该能够让你的头像在游戏窗口内移动，且花椰菜的生长速度将会不断加快。

![玩家头像和障碍物](images/monogame-tutorial-2.png)

## <a name="render-text-with-spritefont"></a>用 SpriteFont 呈现文本
使用以上代码，我们可以在场景后跟踪玩家的得分，但我们实际上并未告诉玩家得分。 此外，在应用启动时，我们还会提供一个相当不直观的介绍 - 玩家将会看到一个蓝色和绿色窗口，但是没法知道需要按空格键才能开始滚动。

若要修复这两个问题，我们将使用名为 **SpriteFont** 的新型 MonoGame 对象。

### <a name="1-create-spritefont-description-files"></a>1. 创建 SpriteFont 描述文件
在**解决方案资源管理器**中找到 **Content** 文件夹。 在此文件夹中，右键单击 **Content.mgcb** 文件并选择**打开方式**。 从弹出菜单中，选择 **MonoGame Pipeline**，然后按**确定**。 在新窗口中，右键单击**内容**项并选择**添加 -> 新项目**。 选择 **SpriteFont Description**，将其命名为“得分”并按**确定**。 然后，通过相同的步骤添加另一个名为“GameState”的 SpriteFont 描述。

### <a name="2-edit-descriptions"></a>2. 编辑描述
右键单击 **MonoGame 管道**中的 **Content** 文件夹，然后选择**打开文件位置**。 你应该会看到一个文件夹和刚刚创建的 SpriteFont 描述文件，以及已添加到“Content”文件夹的所有图像。 现在，你可以关闭并保存 MonoGame 管道窗口。 通过**文件资源管理器**，在文本编辑器（Visual Studio、NotePad++、Atom 等）中打开两个描述文件。

每个描述包含多个用于描述 SpriteFont 的值。 我们将进行一些更改：

在 **Score.spritefont** 中，将 **<Size>** 值从 12 改为 36。

在 **GameState.spritefont** 中，将 **<Size>** 值从 12 改为 72，并将 **<FontName>** 值从 Arial 改为 Agency。 Agency 是 Windows 10 计算机标配随带的另一种字体，将为我们的介绍屏幕添加别样的风格。

### <a name="3-load-spritefonts"></a>3. 加载 SpriteFont
返回 Visual Studio，我们将先为介绍初始屏幕添加新的纹理。 [单击此处以下载此图像](https://github.com/Microsoft/Windows-appsample-get-started-mg2d/blob/master/MonoGame2D/Content/start-splash.png)。

如同先前一样，右键单击“内容”并选择**添加 -> 现有项**即可为项目添加纹理。 将此新项命名为“start-splash.png”。

接下来，将以下类变量添加到 **Game1.cs**：

```CSharp
Texture2D startGameSplash;
SpriteFont scoreFont;
SpriteFont stateFont;
```

然后将这些行添加到 **LoadContent** 方法：

```CSharp
startGameSplash = Content.Load<Texture2D>("start-splash");
scoreFont = Content.Load<SpriteFont>("Score");
stateFont = Content.Load<SpriteFont>("GameState");
```

### <a name="4-draw-the-score"></a>4. 绘制得分
转到 **Game1.cs** 的 **Draw** 方法，并将以下代码添加到 **spriteBatch.End();** 前面

```CSharp
spriteBatch.DrawString(scoreFont, score.ToString(),
new Vector2(screenWidth - 100, 50), Color.Black);
```

以上代码使用我们创建的子画面描述（Arial 36 号）在屏幕右上角附近绘制玩家的当前得分。

### <a name="5-draw-horizontally-centered-text"></a>5. 绘制水平居中的文本
制作游戏时，你经常需要绘制水平居中或垂直居中的文本。 若要将介绍文本水平居中，请将此代码添加到 **spriteBatch.End();** 之前的 **Draw** 方法中。

```CSharp
if (!gameStarted)
{
  // Fill the screen with black before the game starts
  spriteBatch.Draw(startGameSplash, new Rectangle(0, 0,
  (int)screenWidth, (int)screenHeight), Color.White);

  String title = "VEGGIE JUMP";
  String pressSpace = "Press Space to start";

  // Measure the size of text in the given font
  Vector2 titleSize = stateFont.MeasureString(title);
  Vector2 pressSpaceSize = stateFont.MeasureString(pressSpace);

  // Draw the text horizontally centered
  spriteBatch.DrawString(stateFont, title,
  new Vector2(screenWidth / 2 - titleSize.X / 2, screenHeight / 3),
  Color.ForestGreen);
  spriteBatch.DrawString(stateFont, pressSpace,
  new Vector2(screenWidth / 2 - pressSpaceSize.X / 2,
  screenHeight / 2), Color.White);
  }
```

首先，我们需要创建两个字符串，分别代表我们想要绘制的文本的每一条线。 接下来，我们需要使用 **SpriteFont.MeasureString(String)** 方法测量每行的打印宽度和高度。 这将为我们提供如 **Vector2** 对象一样的尺寸，其中 **X** 属性包含其宽度，**Y** 属性包含其高度。

最后，我们绘制每条线。 若要将文本水平居中，我们需要使其位置矢量的 **X** 值等于 **screenWidth / 2 - textSize.X / 2**

**挑战：** 你应该对以上步骤做出哪些调整才能对文本进行垂直及水平居中？

尝试运行游戏。 你看到介绍初始屏幕了吗？ 花椰菜每繁殖一次，得分是否就递增了呢？

![介绍初始屏幕](images/monogame-tutorial-3.png)

## <a name="collision-detection"></a>碰撞检测
现在，我们已经让花椰菜围绕在你周围，并且花椰菜繁殖一次得分就会递增，但实际上没有办法输掉游戏。 我们需要使用一种方法来知道恐龙和花椰菜子画面是否碰撞，以宣布游戏结束。

### <a name="1-rectangular-collision"></a>1. 矩形碰撞
在游戏中检测到碰撞时，对象往往会被简化，以降低所涉及的数学知识的复杂性。 我们会将玩家头像和花椰菜障碍视作矩形，以检测两者之间是否存在碰撞。

打开 **SpriteClass.cs** 并添加新的类变量：

```CSharp
const float HITBOXSCALE = .5f;
```

此值表示碰撞检测的“宽松”程度（对玩家来说）。 如果值为 .5f，则恐龙可能与花椰菜碰撞的矩形边沿（常称为“有效命中体”）为全尺寸纹理的一半。 这会导致出现少数以下实例：两个纹理的角发生碰撞，但图像的所有部分实际上并未发生任何接触。 可以根据你自己的喜好随意调整此值。

接下来，为 **SpriteClass.cs** 添加新的方法：

```CSharp
public bool RectangleCollision(SpriteClass otherSprite)
{
  if (this.x + this.texture.Width * this.scale * HITBOXSCALE / 2 < otherSprite.x - otherSprite.texture.Width * otherSprite.scale / 2) return false;
  if (this.y + this.texture.Height * this.scale * HITBOXSCALE / 2 < otherSprite.y - otherSprite.texture.Height * otherSprite.scale / 2) return false;
  if (this.x - this.texture.Width * this.scale * HITBOXSCALE / 2 > otherSprite.x + otherSprite.texture.Width * otherSprite.scale / 2) return false;
  if (this.y - this.texture.Height * this.scale * HITBOXSCALE / 2 > otherSprite.y + otherSprite.texture.Height * otherSprite.scale / 2) return false;
  return true;
}
```

此方法用于检测两个矩形对象是否发生了碰撞。 此算法通过测试来确定矩形的任一面（或多个面）之间是否存在间隙。 如果存在间隙，则没有发生碰撞；如果没有间隙，则必定发生了碰撞。

### <a name="2-load-new-textures"></a>2. 加载新纹理

然后，打开 **Game1.cs** 并添加两个新的类变量，一个用于存储游戏结束子画面纹理，另一个布尔类变量用于跟踪游戏状态：

```CSharp
Texture2D gameOverTexture;
bool gameOver;
```

然后，初始化 **Initialize** 方法中的 **gameOver**：

```CSharp
gameOver = false;
```

最后，将纹理加载到 **LoadContent** 方法的 **gameOverTexture** 中：

```CSharp
gameOverTexture = Content.Load<Texture2D>("game-over");
```

### <a name="3-implement-game-over-logic"></a>3. 实施“游戏结束”逻辑
调用 **KeyboardHandler** 方法之后，将此代码添加到 **Update** 方法：

```CSharp
if (gameOver)
{
  dino.dX = 0;
  dino.dY = 0;
  broccoli.dX = 0;
  broccoli.dY = 0;
  broccoli.dA = 0;
}
```

这将导致在游戏结束时所有运动均停止，因此，恐龙和花椰菜子画面将冻结在其当前位置。

接下来，在 **Update** 方法末尾，在 **base.Update(gameTime)** 前面添加此行：

```CSharp
if (dino.RectangleCollision(broccoli)) gameOver = true;
```

这将调用我们在 **SpriteClass** 中创建的 **RectangleCollision** 方法，如果返回 True，则会将游戏标记为结束。

### <a name="4-add-user-input-for-resetting-the-game"></a>4. 添加用于重置游戏的用户输入
将此代码添加到 **KeyboardHandler** 方法，使用户能够在按 Enter 时重置游戏：

```CSharp
if (gameOver && state.IsKeyDown(Keys.Enter))
{
  StartGame();
  gameOver = false;
}
```

### <a name="5-draw-game-over-splash-and-text"></a>5. 绘制游戏结束初始屏幕和文本
最后，在首次调用 **spriteBatch.Draw** 后（此调用应用于绘制草地纹理），将此代码添加至“Draw”方法。

```CSharp
if (gameOver)
{
  // Draw game over texture
  spriteBatch.Draw(gameOverTexture, new Vector2(screenWidth / 2 - gameOverTexture.Width / 2, screenHeight / 4 - gameOverTexture.Width / 2), Color.White);

  String pressEnter = "Press Enter to restart!";

  // Measure the size of text in the given font
  Vector2 pressEnterSize = stateFont.MeasureString(pressEnter);

  // Draw the text horizontally centered
  spriteBatch.DrawString(stateFont, pressEnter, new Vector2(screenWidth / 2 - pressEnterSize.X / 2, screenHeight - 200), Color.White);
}
```

在这里，我们将使用与前面相同的方法来绘制水平居中的文本（重复使用我们在介绍初始屏幕中使用过的字体），以及将 **gameOverTexture** 置于窗口的上半部中间。

已完成！ 尝试再次运行游戏。 如果已遵循以上步骤，则在恐龙与花椰菜发生碰撞时，游戏应该就会结束，并且系统会提示玩家按 Enter 键重新开始游戏。

![游戏结束](images/monogame-tutorial-4.png)

## <a name="publish-to-the-microsoft-store"></a>发布到 Microsoft Store
因为我们的生成此游戏为 UWP 应用，就可以将此项目发布到 Microsoft Store。 此流程包含几个步骤。

你必须以 Windows 开发人员的身份[注册](https://developer.microsoft.com/en-us/store/register)。

你必须使用应用提交[清单](https://docs.microsoft.com/en-us/windows/uwp/publish/app-submissions)。

必须将此应用提交以进行[认证](https://docs.microsoft.com/en-us/windows/uwp/publish/the-app-certification-process)。

有关更多详细信息，请参阅[发布你的 UWP 应用](https://developer.microsoft.com/en-us/store/publish-apps)。
