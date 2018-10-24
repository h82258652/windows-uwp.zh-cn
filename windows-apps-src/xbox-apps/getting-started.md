---
author: Mtoepke
title: Xbox One 上的 UWP 应用开发入门
description: 如何针对 UWP 开发设置电脑和 Xbox One。
ms.author: scotmi
ms.date: 10/12/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: da260b4f9f5f50d97d39af883217dfbae91a566e
ms.sourcegitcommit: 4b97117d3aff38db89d560502a3c372f12bb6ed5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/23/2018
ms.locfileid: "5442193"
---
# <a name="getting-started-with-uwp-app-development-on-xbox-one"></a>Xbox One 上的 UWP 应用开发入门

**注意** 遵循这些步骤以针对通用 Windows 平台 (UWP) 开发成功设置电脑和 Xbox One。 完成设置后，你可以在[适用于 Xbox One 的 UWP](index.md) 页面上了解有关 Xbox One 上的开发人员模式和生成 UWP 应用的详细信息。 

## <a name="before-you-start"></a>开始之前

在开始之前，你将需要执行以下操作：
-   设置了最新版本的 Windows 10 的电脑。
<!-- -  Install Microsoft Visual Studio 2015 Update 3 or Microsoft Visual Studio 2017.

    > [!NOTE]
    > Visual Studio 2017 is required if you are using the Windows 10, build 15063 SDK. -->

- 在 Xbox One 主机上具有至少 5 GB 的可用空间。

## <a name="setting-up-your-development-pc"></a>设置你的开发电脑

1.  安装 Visual Studio 2015 Update 3 或 Visual Studio 2017。

    如果你要安装 Visual Studio 2015 更新 3，请确保你选择**自定义**安装，然后选择**通用 Windows 应用开发工具**复选框-它不是默认安装的一部分。 如果你是 C++ 开发人员，确保选择“自定义安装”****，然后选择“C++”****。

    如果要安装 Visual Studio 2017，请确保选择**通用 Windows 平台开发**工作负载。 如果你是 c + + 开发人员，在**摘要**右侧窗格中，在**通用 Windows 平台开发**，确保你选择的**c + + 通用 Windows 平台工具**复选框。 它不是默认安装的一部分。

    有关详细信息，请参阅[设置 Xbox 开发环境上的 UWP](development-environment-setup.md)。

2.  安装最新的[Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk)。

3.  为你的开发电脑启用开发人员模式 (**设置 / 更新和安全 / 适用于开发人员 / 使用开发人员功能 / 开发人员模式**)。

现在，你的开发电脑已准备就绪，可以观看此视频，或者继续阅读以了解如何设置用于开发的 Xbox One，然后创建一个 UWP 应用并部署到 Xbox One。
</br>
</br>
<iframe src="https://channel9.msdn.com/Events/Xbox/App-Dev-on-Xbox/Get-started-with-App-Dev-on-Xbox/player#time=51s:paused" width="600" height="338"  allowFullScreen frameBorder="0"></iframe>

## <a name="setting-up-your-xbox-one-console"></a>设置 Xbox One 主机

1.  在 Xbox One 上激活开发人员模式。 下载应用，、 获取激活代码，并将其输入到你的开发人员中心帐户中的[管理 Xbox One 主机](https://partner.microsoft.com/xboxactivate)页面。 有关详细信息，请参阅 [Xbox One 开发人员模式激活](devkit-activation.md)。 

2.  打开**开发人员模式激活**应用，然后选择**切换并重启**。 恭喜，你现在具有处于开发人员模式下的 Xbox One！
  
  > [!NOTE]
  > 你的零售游戏和应用不会在开发人员模式下运行，但你创建的应用或游戏会在该模式下运行。 切换回零售模式以运行你最喜爱的游戏和应用。
    
  > [!NOTE]
  > 必须先在该主机上进行用户登录，然后才能在开发人员模式下将应用部署到你的 Xbox One。 可以使用你的现有 Xbox Live 帐户，也可以在开发人员模式下为你的主机创建新帐户。 

## <a name="creating-your-first-project-in-visual-studio"></a>在 Visual Studio 中创建你的第一个项目

有关更多详细信息，请参阅[设置 Xbox 开发环境上的 UWP](development-environment-setup.md)。

1.  **对于 C#**： 创建一个新的通用 Windows 项目，并在**解决方案资源管理器**中，右键单击项目并选择**属性**。 选择**调试**选项卡、 到**远程计算机**更改**目标设备**、 在**远程计算机**字段中，键入的 IP 地址或 Xbox One 主机的主机名和选择**通用 （未加密协议）** 中**身份验证模式**下拉列表。   

    你可以通过在主机上启动“开发人员主页”（“主页”右侧的大磁贴）并查看左上角找到你的 Xbox One IP 地址。 有关开发人员主页的详细信息，请参阅 [Xbox One 工具简介](introduction-to-xbox-tools.md)。  

2.  **对于 c + + 和 HTML/Javascript 项目**： 你遵循相似路径 C# 项目，但在项目属性转到**调试**选项卡，选择调试程序以打开下拉列表中，键入的 IP 地址或主机名中的**远程计算机****计算机名称**字段并选择**通用 （未加密协议）** **身份验证类型**字段中的控制台。

3. 在顶部菜单栏中的绿色播放按钮的左侧，从下拉列表中选择**x64** 。
   
4.  按 F5 后，你的应用将生成并开始在 Xbox One 上部署。
  
5.  第一次执行此操作时，Visual Studio 将提示你为 Xbox One 输入 PIN。 你可以通过在 Xbox One 上启动开发人员主页，然后选择**显示 Visual Studio pin**按钮获取 PIN。
  
6.  在配对后，你的应用将开始部署。 第一次执行此操作时可能有点慢（我们将所有工具复制到 Xbox），但是如果它不只需要几分钟，则可能出现了某些错误。 请确保你已遵循以上所有步骤（尤其是你是否已将“身份验证模式”**** 设置为“通用”****？），并且你正在使用与 Xbox One 的有线网络连接。  

7. 坐下来放松。 享受你的第一个在主机上运行的应用！  

## <a name="thats-it"></a>就这么简单！

![Hello World](images/getting-started-hello-world.png)

## <a name="see-also"></a>另请参阅  
- [常见问题](frequently-asked-questions.md)  
- [Xbox 开发人员计划上的 UWP 已知问题](known-issues.md)
- [Xbox One 上的 UWP](index.md) 
