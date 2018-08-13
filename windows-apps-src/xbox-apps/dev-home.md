---
author: v-angraf
ms.assetid: a56156e4-7adb-bf37-527b-fc3243e04b46
title: 在控制台 （开发主页） 上的开发主页
description: 提供有关开发主页应用程序的一个 Xbox 的信息。
ms.author: v-angraf@microsoft.com
ms.date: 08/09/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
permalink: en-us/docs/xdk/dev-home.html
ms.localizationpriority: medium
ms.openlocfilehash: 3b802b9b53811e03e11ee3afd78f69db4bfd9986
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/13/2018
ms.locfileid: "1015356"
---
# <a name="developer-home-on-the-console-dev-home"></a>在控制台 （开发主页） 上的开发主页
   
  
开发主页上旨在帮助开发人员的效率 Xbox 一个开发工具包的工具体验。 开发主页提供可管理和配置您的开发工具包、 管理用户、 启动安装的标题和执行的功能捕获并跟踪。 我们将继续扩展功能以启用基于客户反馈的其他功能以及启用扩展性和添加您自己的工具的将来版本。   
   
  
我们是非常感兴趣您在开发主页和您最感兴趣查看该支持的方案的反馈。 请通过在**发送反馈**下的应用程序的主菜单中所述的方法或通过您开发人员帐户管理器 (DAM)，提供意见。   
   
  
若要启动开发主页上年 11 月 2015年或更高版本的恢复：  
 
   1. 通过移动主页，在左边缘或双单击结点按钮打开指南  
   1. 向下移动到**设置**（齿轮图标）   
   1. 选择**所有设置**  
   1. 从默认**开发人员**页上，选择**开发主页**（家庭图标）   

 ![](images/dev_home_icons.png)   
  
在早期恢复**特色内容**中选择主屏幕右侧的开发主页图块或查看 Xbox 一个管理器中的应用程序列表并启动**开发主页**。   
 ![](images/dev_home_1.png) 
<a id="ID4EBC"></a>

   

## <a name="user-interface"></a>用户界面  
   
  
开发主页用户界面的标头包含以下重要"概览"有关开发控制台的信息：   
 
   *  **控制台 IP:** 控制台当前 IP 地址。   
   *  **控制台名称：** 控制台当前主机名。  
   *  **沙盒：** 沙盒中的控制台的名称。  
   *  **操作系统版本：** 在控制台中运行的当前恢复版本。
   *  当前系统时间。   

   
  
开发主用户界面的其余部分分为以下页面。 在这些页面上的工具的详细信息，请参阅其各个主题。   
 
   *  [家](devhome-home.md)  
   *  [Xbox Live](devhome-live.md)  
   *  [Settings](devhome-settings.md)  
   *  [媒体捕获](devhome-capture.md)  
   *  [网络](devhome-networking.md)  
   *  [性能](devhome-performance.md)  

  
<a id="ID4EKE"></a>

   

## <a name="main-menu"></a>主菜单  
   
  
通过按控制器上的**菜单**按钮，您可以访问允许应用程序工作区，管理凭据访问网络位置和应用程序提供反馈信息的功能的配置的主菜单。   
  
<a id="ID4EUE"></a>

   

## <a name="snap-mode-ux"></a>对齐 UX 模式  
   
  
开发家庭、 网络和多人，如中的多个现有和即将发布工具设计用于对齐到侧时正在运行您的标题，以便您可以轻松访问工具时要测试。   
   
  
访问管理单元模式，突出显示相应的工具的标题，您在控制器上，按**视图**按钮并从上下文菜单中选择**对齐**:  
 ![](images/dev_home_4.png)   
  
开发人员主页将贴靠到右侧。 你可以通过像往常那样双击 Nexus 按钮来切换上下文。  
 ![](images/dev_home_5.png)  
<a id="ID4EKF"></a>

   

## <a name="customizing-dev-home"></a>自定义开发人员主页  
   
  
开发人员主页已设计为可自定义，并且具有个性化。 您可以配置应用程序以满足您的工作流，然后为工作区中保存的。 此工作区可以导出和导入，这使您可以将布局复制到其他作为控制台需要。 在**工作区**下主菜单中找到这些选项。 导出的文件将位于系统临时驱动器中`Dev Home\Workspaces`目录。   
 
<a id="ID4EVF"></a>

   

### <a name="resizing-and-reordering-tools"></a>调整大小和重新排序工具  
   
  
若要更改的大小或位置的工具，请使用上下文菜单按钮 （在控制器上的视图按钮） 标题时具有焦点。 从上下文菜单中，选择**移动**或**调整其大小**。   
 ![](images/dev_home_6.png)  
<a id="ID4EEG"></a>

   

### <a name="changing-theme-color-and-background-image"></a>更改主题色和背景图  
   
  
从主菜单中，您可以选择**工作区**，然后选择**更改主题颜色**。 选择新颜色，然后选择**保存**以更新用于会议状态中心突出显示的主题颜色。   
 ![](images/dev_home_7.png)  
<a id="ID4EVG"></a>

   

### <a name="setting-the-default-application-for-a-package"></a>设置包的默认应用程序  
   
  
如果包中包含多个应用程序，开发主页将允许您设置要启动的默认应用程序。 突出显示在启动器的程序包，然后按**A**按钮以打开可用应用程序列表。 突出显示您想要设置为默认和按**视图**按钮，然后从上下文菜单选择**设为默认值**。   
 ![](images/dev_home_setdefault.png)  
<a id="ID4EGH"></a>

   

### <a name="using-dev-home-to-register-and-launch-titles-from-a-network-share"></a>使用开发主页进行注册和启动从网络共享的标题  
   
  
从启动器，底部的已安装的应用程序和游戏列表中，您可以选择**注册从网络共享游戏**远程运行标题的松散文件版本的选项。   
 ![](images/dev_home_8.png)   
  
然后您可以输入您希望注册标题的 appxmanifest.xml 文件的网络路径。 开发主页将尝试注册为该共享中，使用任何现有的身份验证的标题和如果需要将新的网络凭据提示。 如果您需要访问 （例如到一台服务器上的符号链接的访问资源） 的其他共享您需要添加那些通过下面的选项。   
   
  
您可以管理这些存储的凭据 （以及添加其他的） 通过主菜单的**管理网络凭据**选项控制台上。   
 ![](images/dev_home_9.png)   
  
您可以控制台上当前查看的凭据，通过选择凭据的路径，并单击**A**按钮中编辑凭据和删除凭据，通过选择删除链接并单击**A**按钮。   
   
<a id="ID4EGAAC"></a>

   

## <a name="in-this-section"></a>本部分内容  
  
[主页 （开发主页）](devhome-home.md)  


&nbsp;&nbsp;提供对定期执行开发控制台的任务的快速访问。 
  
  
[Xbox Live 页 （开发主页）](devhome-live.md)  


&nbsp;&nbsp;捕获多人的信息，并显示 Xbox Live 服务的当前状态。 
  
  
[设置页 （开发主页）](devhome-settings.md)  


&nbsp;&nbsp;提供对开发控制台的各种设置的访问。 
  
  
[媒体捕获页 （开发主页）](devhome-capture.md)  


&nbsp;&nbsp;开发主页的**媒体捕获**页捕获控制台当前运行的标题的视频。 
  
  
[网络页 （开发主页）](devhome-networking.md)  


&nbsp;&nbsp;模拟以便解决问题的各种网络条件。 
  
  
[性能页 （开发主页）](devhome-performance.md)  


&nbsp;&nbsp;模拟各种磁盘活动和 CPU 使用率条件，以便解决问题。 
 