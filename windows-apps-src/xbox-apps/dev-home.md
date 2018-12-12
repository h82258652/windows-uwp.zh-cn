---
ms.assetid: a56156e4-7adb-bf37-527b-fc3243e04b46
title: 在控制台 （开发人员主页） 上的开发人员主页
description: 提供有关开发人员主页应用适用于 Xbox One 的信息。
ms.date: 08/09/2017
ms.topic: article
keywords: windows 10, uwp
permalink: en-us/docs/xdk/dev-home.html
ms.localizationpriority: medium
ms.openlocfilehash: 4113df37446d93883cf395e7c1e86b1de6c1b328
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2018
ms.locfileid: "8936428"
---
# <a name="developer-home-on-the-console-dev-home"></a>在控制台 （开发人员主页） 上的开发人员主页
   
  
开发人员主页是 Xbox One 开发工具包上旨在提高开发人员工作效率的工具体验。 开发人员主页提供功能来管理和配置你的开发工具包，管理用户、 启动已安装的游戏以及执行捕获和跟踪。 在将来版本中我们将继续进行扩展以启用其他功能，具体取决于你的反馈，以及若要启用可扩展性和你自己的工具的添加功能。   
   
  
我们将非常感兴趣你在开发人员主页和你最感兴趣看到它支持的方案的反馈。 请通过在应用的主菜单下**发送反馈**所述的方法或通过你的开发人员帐户管理器 (DAM)，提供你的注释。   
   
  
若要启动开发人员主页上 2015 年 11 月或更高版本的恢复：  
 
   1. 通过将移动主页上, 向左或双单击 Nexus 按钮打开指南  
   1. 向下移动到**设置**（的齿轮图标）   
   1. 选择**所有设置**  
   1. 从默认**开发人员**页面中，选择**开发人员主页**（主页图标）   

 ![](images/dev_home_icons.png)   
  
在较早的恢复选择主屏幕右侧的开发人员主页磁贴中**特别推荐的内容**或在 Xbox One 管理器中查看应用程序列表和启动**开发人员主页**。   
 ![](images/dev_home_1.png) 
<a id="ID4EBC"></a>

   

## <a name="user-interface"></a>用户界面  
   
  
开发人员主页用户界面的标头包含以下重要"一览"信息开发主机：   
 
   *  **控制台 IP:** 控制台的当前的 IP 地址。   
   *  **控制台名称：** 当前控制台的主机名。  
   *  **沙盒：** 沙盒中的主机的名称。  
   *  **操作系统版本：** 在控制台运行的当前恢复版本。
   *  当前系统时间。   

   
  
开发人员主页 UI 的其余部分分为以下页面。 有关这些页面上的工具的详细信息，请参阅其单独的主题。   
 
   *  [家](devhome-home.md)  
   *  [Xbox Live](devhome-live.md)  
   *  [设置](devhome-settings.md)  
   *  [媒体捕获](devhome-capture.md)  
   *  [网络](devhome-networking.md)  
   *  [性能](devhome-performance.md)  

  
<a id="ID4EKE"></a>

   

## <a name="main-menu"></a>主菜单  
   
  
通过按控制器上的**菜单**按钮，你可以访问主菜单，允许应用工作区，能够管理凭据用于访问网络位置和信息的应用提供反馈的配置。   
  
<a id="ID4EUE"></a>

   

## <a name="snap-mode-ux"></a>贴靠模式 UX  
   
  
在开发人员主页，如网络和多人游戏，多个现有和即将推出的工具旨在用于贴靠到一侧时正在运行你的作品，以使你可以轻松访问工具在测试时。   
   
  
若要访问贴靠模式，突出显示标题的合适的工具、 在控制器上按**视图**按钮和从上下文菜单中选择**对齐**：  
 ![](images/dev_home_4.png)   
  
开发人员主页将贴靠到右侧。 你可以通过像往常那样双击 Nexus 按钮来切换上下文。  
 ![](images/dev_home_5.png)  
<a id="ID4EKF"></a>

   

## <a name="customizing-dev-home"></a>自定义开发人员主页  
   
  
开发人员主页已设计为可自定义，并且具有个性化。 你可以将应用配置为适合你的工作流，，，然后保存的用作工作区。 此工作区可导出和导入，允许你将布局复制到其他主机作为需要。 在主菜单下**工作区**中找到这些选项。 导出的文件将位于中的系统暂存驱动器上`Dev Home\Workspaces`目录。   
 
<a id="ID4EVF"></a>

   

### <a name="resizing-and-reordering-tools"></a>调整大小和重新排序工具  
   
  
若要更改的大小或的工具的位置，请使用上下文菜单按钮 （控制器上的视图按钮） 时游戏具有焦点。 从上下文菜单中选择**移动**或**调整大小**。   
 ![](images/dev_home_6.png)  
<a id="ID4EEG"></a>

   

### <a name="changing-theme-color-and-background-image"></a>更改主题色和背景图  
   
  
从主菜单中，你可以选择**工作区**，然后**更改主题色**。 选择一种新颜色并选择**保存**以更新用于焦点突出显示的主题色。   
 ![](images/dev_home_7.png)  
<a id="ID4EVG"></a>

   

### <a name="setting-the-default-application-for-a-package"></a>设置包的默认应用程序  
   
  
如果包中包含多个应用程序，开发人员主页将允许你设置的默认应用程序启动。 突出显示启动程序中的包，然后按**A**按钮以打开可用应用程序的列表。 突出显示你希望设为默认值并按**视图**按钮，然后从上下文菜单选择**设为默认值**。   
 ![](images/dev_home_setdefault.png)  
<a id="ID4EGH"></a>

   

### <a name="using-dev-home-to-register-and-launch-titles-from-a-network-share"></a>使用开发人员主页进行注册和启动游戏从网络共享  
   
  
从启动器，底部的已安装的应用和游戏列表中，你可以选择此选项，**注册从网络共享游戏**远程运行游戏的 loose 文件版本。   
 ![](images/dev_home_8.png)   
  
然后，你可以向你想要注册的标题的 appxmanifest.xml 文件输入网络路径。 开发人员主页将尝试注册任何现有凭据用于该共享，游戏，并且当需要将提示提供新的网络凭据。 如果你需要访问 （例如用于访问符号链接在单独的服务器上的资源） 的其他共享你将需要添加它们通过下面的选项。   
   
  
你可以管理这些存储的凭据 （并添加其他域） 通过在主菜单**管理网络凭据**选项在主机上。   
 ![](images/dev_home_9.png)   
  
你可以查看凭据当前在控制台上、 选择凭据的路径，然后单击**按钮**来编辑凭据和删除凭据，请选择删除链接，然后单击**一个**按钮。   
   
<a id="ID4EGAAC"></a>

   

## <a name="in-this-section"></a>本部分内容  
  
[主页 （开发人员主页）](devhome-home.md)  


&nbsp;&nbsp;提供了快速访问例行开发控制台执行的任务。 
  
  
[Xbox Live 页面 （开发人员主页）](devhome-live.md)  


&nbsp;&nbsp;捕获多人游戏的信息，并显示 Xbox Live 服务的当前状态。 
  
  
[设置页面 （开发人员主页）](devhome-settings.md)  


&nbsp;&nbsp;提供对各种设置用于开发主机访问。 
  
  
[媒体捕获页面 （开发人员主页）](devhome-capture.md)  


&nbsp;&nbsp;开发人员主页的**媒体捕获**页面捕获视频的游戏的当前在控制台上运行。 
  
  
[网络页面 （开发人员主页）](devhome-networking.md)  


&nbsp;&nbsp;模拟各种网络条件为了进行故障排除。 
  
  
[平均页面 （开发人员主页）](devhome-performance.md)  


&nbsp;&nbsp;模拟各种磁盘活动和 CPU 使用情况，为了进行故障排除。 
 