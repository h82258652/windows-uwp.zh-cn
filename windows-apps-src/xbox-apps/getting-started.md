---
title: Xbox One 上的 UWP 应用开发入门
description: 如何针对 UWP 开发设置电脑和 Xbox One。
area: Xbox
---

#Xbox One 上的 UWP 应用开发入门

**仔细**遵循这些步骤以针对 UWP 开发成功设置电脑和 Xbox One。 完成设置后，你可以在[适用于 Xbox One 的 UWP](index.md) 页面上了解有关 Xbox One 上的开发人员模式和生成 UWP 应用的详细信息。 

## 开始之前
在开始之前，你将需要执行以下操作：
-   创建 [Windows 开发人员中心](https://dev.windows.com)帐户。
-   加入 [Windows 预览体验计划](https://insider.windows.com/)。 你将需要以下条件才能获得预览版 Windows SDK。
-   设置 Windows 10 电脑（包含最新的 Windows 10 insider 外部测试版的任何版本都有效）- 对于此预览版，我们的开发工具需要你运行 Windows 10。 
-   将 Xbox One 控制台连接到有线网络（无线网络可能有效，但使用有线连接会使当前性能更好）。
- 在 Xbox One 控制台上具有至少 30 GB 的可用空间。

## 设置你的开发电脑
1.  安装 Visual Studio 2015 Update 2。 确保你选择**自定义**安装并选中**“通用 Windows 应用开发工具”**复选框，它不是默认安装的一部分。 有关详细信息，请参阅[开发环境设置](development-environment-setup.md)（此外，如果你是 C++ 开发人员，请确保选择自定义安装并同时选择 C++）。

2.  安装 Windows 10 SDK 预览版 14295。 你可以从 [Windows 预览体验计划](http://go.microsoft.com/fwlink/p/?LinkId=780552)获取此版本。
  
  > **重要提示** 在电脑上安装此预览版 SDK 将阻止你向应用商店提交在此电脑上生成的应用，因此请不要在生产开发电脑上执行此操作。 

## 设置 Xbox One 控制台
1.  在 Xbox One 上激活开发人员模式。 下载应用、获取激活代码、将其输入开发人员中心帐户中的 xboxactivate 页面。 有关详细信息，请参阅[在 Xbox One 上启用开发人员模式](devkit-activation.md)。 

2.  等待 Xbox One 进行系统更新，以使其运行开发者预览版。 这可能需要最多 4 个小时。 如果你不希望等待，请“硬”重新启动你的控制台，方法是长按电源按钮 10 秒，然后重新打开它。 这将触发更新。  

3.  转到开发人员模式激活应用，然后选择**“切换并重新启动”**。 恭喜，你现在具有处于开发人员模式下的 Xbox One！
  
  > **注意** 你的零售游戏和应用不会在开发人员模式下运行，但你创建的应用或游戏会在该模式下运行。 切换回零售模式以运行你最喜爱的游戏和应用。

## 在 Visual Studio 2015 中创建你的第一个项目

有关更多详细信息，请参阅[开发环境设置](development-environment-setup.md)。

1.  对于 C#：创建一个新的通用 Windows 项目、转到项目属性中并选择**“调试”**选项卡、将**“目标设备”**更改为**“远程计算机”**、在**“远程计算机”**字段中键入 Xbox One 的 IP 地址或主机名，然后在**“身份验证模式”**下拉列表中选择**“通用(未加密协议)”**。   

    你可以通过在控制台上启动“开发人员主页”（“主页”右侧的大磁贴）并查看左上角来找到你的 Xbox One IP 地址。 有关开发人员主页的详细信息，请参阅 [Xbox One 工具简介](introduction-to-xbox-tools.md)。  

2.  对于 C++ 和 HTML/Javascript 项目：你遵循相似路径，但在项目属性中，转到**“调试”**选项卡、在调试器中选择**“远程计算机”**来打开下拉列表、在**“计算机名称”**字段中键入控制台的 IP 地址或主机名，然后在**“身份验证类型”**字段中选择**“通用(未加密协议)”**。
   
3.  按 F5 后，你的应用将生成并开始在 Xbox One 上部署。
  
4.  第一次执行此操作时，Visual Studio 将提示你为 Xbox One 输入 PIN。 你可以通过在 Xbox One 上启动“开发人员主页”并按**“与 Visual Studio 配对”**按钮来获取 PIN。
  
5.  在配对后，你的应用将开始部署。 你第一次执行此操作时可能有点慢（我们将所有工具复制到 Xbox），但是如果它不只需要几分钟，则可能出现了某些错误。 请确保你已遵循以上所有步骤（尤其是你是否已将**“身份验证”**设置为**“通用”**？），并且你正在使用与 Xbox One 的有线网络连接。  

6. 坐下来放松。 享受你的第一个在控制台上运行的应用！  
   ![Hello World](images/getting-started-hello-world.png)
   

## 另请参阅  
- [常见问题](frequently-asked-questions.md)  
- [已知问题](known-issues.md)
- [Xbox One 上的 UWP](index.md)


<!--HONumber=Mar16_HO5-->


