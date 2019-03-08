---
ms.assetid: a56156e4-7adb-bf37-527b-fc3243e04b46
title: 控制台上的开发人员主页（开发人员主页）
description: 提供有关 Xbox One 开发主页应用的信息。
ms.date: 08/09/2017
ms.topic: article
keywords: windows 10, uwp
permalink: en-us/docs/xdk/dev-home.html
ms.localizationpriority: medium
ms.openlocfilehash: 4113df37446d93883cf395e7c1e86b1de6c1b328
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57620842"
---
# <a name="developer-home-on-the-console-dev-home"></a>控制台上的开发人员主页（开发人员主页）
   
  
“开发人员主页”是 Xbox One 开发工具包上旨在提高开发人员工作效率的工具体验。 “开发人员主页”提供各种功能，可管理和配置开发工具包、管理用户、启动已安装的标题以及执行捕获和跟踪任务。 在未来版本中，我们将继续扩展各种功能，以便根据你的反馈提供其他功能，同时允许你扩展和添加自己的工具。   
   
  
我们很想了解你对“开发人员主页”的反馈以及你最感兴趣的希望受其支持的方案。 请通过该应用的主菜单中的**发送反馈**下方介绍的方法，或通过你的开发者帐户管理器 (DAM) 发表意见。   
   
  
若要在 2015 年 11 月或之后的恢复版本上启动“开发人员主页”：  
 
   1. 通过在主页上向左移动或双击 Nexus 按钮，打开指南  
   1. 向下移动到“设置”（齿轮图标）   
   1. 选择**所有设置**  
   1. 从默认的**开发人员**页面上，选择**开发人员主页**（主页图标）   

 ![](images/dev_home_icons.png)   
  
在较早的恢复版本上，在**特色内容**的主屏幕右侧选择“开发人员主页”磁贴，或者在 Xbox One Manager 中查看应用列表并启动**开发人员主页**。   
 ![](images/dev_home_1.png) 
<a id="ID4EBC"></a>

   

## <a name="user-interface"></a>用户界面  
   
  
“开发人员主页”用户界面的标头包含有关开发控制台的以下重要“概览”信息：   
 
   *  **控制台 IP:** 在控制台当前 IP 地址。   
   *  **控制台名称：** 在控制台的当前主机名。  
   *  **沙盒：** 控制台是沙盒的名称。  
   *  **操作系统版本：** 在控制台运行的当前恢复版本。
   *  当前系统时间。   

   
  
“开发人员主页”的其余 UI 分为以下页面。 有关这些页面上的工具的详细信息，请参阅各个主题。   
 
   *  [主页](devhome-home.md)  
   *  [Xbox Live](devhome-live.md)  
   *  [设置](devhome-settings.md)  
   *  [捕获媒体](devhome-capture.md)  
   *  [网络](devhome-networking.md)  
   *  [性能](devhome-performance.md)  

  
<a id="ID4EKE"></a>

   

## <a name="main-menu"></a>主菜单  
   
  
通过按控制器上的**菜单**按钮，你可以访问允许配置应用工作区的主菜单、用于管理网络位置访问凭据的功能以及关于提供对该应用的反馈的信息。   
  
<a id="ID4EUE"></a>

   

## <a name="snap-mode-ux"></a>贴靠模式 UX  
   
  
“开发人员主页”中的几个现有和即将推出的工具（例如网络和多人游戏）设计为在运行标题时贴靠到一侧，以便你能够在测试时轻松访问这些工具。   
   
  
若要访问贴靠模式，请突出显示相应工具的标题、在控制器上按**视图**按钮，并在上下文菜单中选择**贴靠**：  
 ![](images/dev_home_4.png)   
  
开发人员主页将贴靠到右侧。 你可以通过像往常那样双击 Nexus 按钮来切换上下文。  
 ![](images/dev_home_5.png)  
<a id="ID4EKF"></a>

   

## <a name="customizing-dev-home"></a>自定义开发人员主页  
   
  
开发人员主页已设计为可自定义，并且具有个性化。 你可以配置该应用以便满足你的工作流要求，然后将其另存为工作区。 此工作区可以导出和导入，允许你根据需要将布局复制到其他控制台。 你可以在主菜单的**工作区**下方找到这些选项。 导出的文件将位于系统暂存驱动器的 `Dev Home\Workspaces` 目录下。   
 
<a id="ID4EVF"></a>

   

### <a name="resizing-and-reordering-tools"></a>调整工具大小和对其重新排序  
   
  
若要更改工具的大小或位置，请在标题有焦点时使用上下文菜单按钮（控制器上的“视图”按钮）。 从上下文菜单中，选择**移动**或**调整大小**。   
 ![](images/dev_home_6.png)  
<a id="ID4EEG"></a>

   

### <a name="changing-theme-color-and-background-image"></a>更改主题色和背景图  
   
  
从主菜单中，你可以选择**工作区**，然后**更改主题色**。 选择一种新颜色，然后选择**保存**，以更新用于焦点突出显示的主题色。   
 ![](images/dev_home_7.png)  
<a id="ID4EVG"></a>

   

### <a name="setting-the-default-application-for-a-package"></a>设置程序包的默认应用  
   
  
如果程序包包含多个应用，“开发人员主页”将允许你设置要启动的默认应用。 突出显示启动器中的程序包，然后按**A**按钮以打开可用应用的列表。 突出显示你想要设置为默认应用的应用，按**视图**按钮，然后从上下文菜单中选择**设置为默认**。   
 ![](images/dev_home_setdefault.png)  
<a id="ID4EGH"></a>

   

### <a name="using-dev-home-to-register-and-launch-titles-from-a-network-share"></a>使用“开发人员主页”注册并启动网络共享中的标题  
   
  
从启动器中，在已安装的应用和游戏列表底部，你可以选择**从网络共享中注册游戏**选项，以远程运行标题的 Loose 文件版本。   
 ![](images/dev_home_8.png)   
  
然后，你可以输入要注册的标题的 appxmanifest.xml 文件的网络路径。 “开发人员主页”将尝试使用该共享的任何现有凭据来注册标题，并且如果需要，将提示提供新的网络凭据。 如果你需要访问其他共享（例如访问单独服务器上的符号链接资源），则需要通过以下选项添加它们。   
   
  
你可以在控制台上通过主菜单的**管理网络凭据**选项管理这些存储的凭据（并添加其他凭据）。   
 ![](images/dev_home_9.png)   
  
你可以查看控制台上的当前凭据，通过选择凭据路径并单击 **A** 按钮来编辑凭据，以及通过选择删除链接并单击 **A** 按钮来删除该凭据。   
   
<a id="ID4EGAAC"></a>

   

## <a name="in-this-section"></a>本部分内容  
  
[主页 （适用于开发人员主页）](devhome-home.md)  


&nbsp;&nbsp;提供了对开发控制台定期执行的任务的快速访问。 
  
  
[Xbox Live 页 （适用于开发人员主页）](devhome-live.md)  


&nbsp;&nbsp;捕获多人游戏信息并显示的 Xbox Live 服务的当前状态。 
  
  
[设置页 （适用于开发人员主页）](devhome-settings.md)  


&nbsp;&nbsp;提供对各种设置开发控制台访问。 
  
  
[捕获媒体的页 （适用于开发人员主页）](devhome-capture.md)  


&nbsp;&nbsp;**媒体捕获**页开发人员主页捕获当前正在运行的控制台的标题的视频。 
  
  
[网络页 （适用于开发人员主页）](devhome-networking.md)  


&nbsp;&nbsp;模拟各种网络条件进行故障排除。 
  
  
[性能页 （适用于开发人员主页）](devhome-performance.md)  


&nbsp;&nbsp;模拟不同的磁盘活动和 CPU 使用情况进行故障排除。 
 