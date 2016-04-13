---
title: 在 Xbox 开发环境上设置你的 UWP
description: 在 Xbox 开发环境上设置和测试你的 UWP 的步骤。
area: Xbox
---

# 在 Xbox 开发环境上设置你的 UWP

Xbox 开发环境上的通用 Windows 平台 (UWP) 由通过本地网络连接到 Xbox One 控制台的开发电脑组成。
开发电脑需要 Windows 10、Visual Studio 2015 Update 2、Windows 10 SDK 预览版 14295 和各种支持工具。


本文介绍了设置和测试你的开发环境的步骤。

## Visual Studio 安装程序

1. 安装 Visual Studio 2015 Update 2 或更高版本。 有关详细信息以及安装方式，请参阅[适用于 Windows 10 的下载和工具](https://dev.windows.com/downloads)。

1. 安装 Visual Studio 2015 Update 2 时，请确保**“通用 Windows 应用开发工具”**复选框处于选中状态。

  ![安装 Visual Studio 2015 Update 2](images/vs_install_tools.png)

## Windows 10 SDK 安装程序

安装 Windows 10 SDK 预览版 14295。 有关安装信息，请参阅[下载适用于开发人员的 Insider Preview 更新](http://go.microsoft.com/fwlink/p/?LinkId=780552)。

  > **重要提示** 你需要安装最新的 SDK，但_不_需要安装操作系统的最新 Windows Insider Preview 版本。

## 创建你的第一个应用程序

1. 确保你的开发电脑与目标 Xbox One 控制台在同一个本地网络上。 通常，这意味着它们应使用相同的路由器并位于同一个子网上。 建议使用有线网络连接。

1. 确保你的 Xbox One 控制台处于开发人员模式下。  有关详细信息，请参阅[在 Xbox One 上启用开发人员模式](devkit-activation.md)。

1. 确定要用于 UWP 应用的编程语言。

1. 在开发电脑上，选择**“新建项目”**，然后依次选择**“Windows”/“通用”/“空白应用”**。

### 启动 C# 项目

  ![“新建项目”对话框](images/vs_universal_blank.jpg)

1. 在**“新建通用 Windows 项目”**对话框中选择默认选项。 如果出现**“开发人员模式”**对话框，单击**“确定”**。 将创建一个新的空白应用。

1. 为远程调试配置开发环境：

  1. 右键单击该项目，然后选择**“属性”**。
  1. 在**“调试”**选项卡上，将**“目标设备”**更改为**“远程计算机”**。
  1. 在**远程计算机**中，输入系统 IP 地址或 Xbox One 控制台的主机名。 有关获取 IP 地址或主机名的信息，请参阅 [Xbox One 工具简介](introduction-to-xbox-tools.md)。
  1. 在**“身份验证模式”**下拉列表中，选择**“通用(未加密协议)”**。

    ![C# BlankApp Property Pages](images/vs_remote.jpg)

### 启动 C++ 项目

  ![C++ 项目](images/vs_universal_cpp_blank.jpg)

1. 在**“新建通用 Windows 项目”**对话框中选择默认选项。 如果出现**“开发人员模式”**对话框，单击**“确定”**。 将创建一个新的空白应用。

1. 为远程调试配置开发环境：

   1. 右键单击该项目，然后选择**“属性”**。
   1. 在**“调试”**选项卡上，将**“要启动的调试器”**更改为**“远程计算机”**。
   1. 在“计算机名称”****中，输入系统 IP 地址或 Xbox One 控制台的主机名。 有关获取 IP 地址或主机名的信息，请参阅 [Xbox One 工具简介](introduction-to-xbox-tools.md)。
   1. 在**“身份验证类型”**下拉列表中，选择**“通用(未加密协议)”**。

    ![C++ BlankApp 属性页](images/vs_remote_cpp.jpg)

### 将你的设备与 Visual Studio 进行 PIN 配对

1. 保存你的设置，并确保你的 Xbox One 控制台处于开发人员模式下。

1. 按 F5。

1. 如果这是你第一次部署，你将获取来自 Visual Studio 的对话框，要求你对你的设备进行 PIN 配对。

  1. 若要获取 PIN，请从 Xbox One 控制台上的主屏幕打开**“开发人员主页”**。
  1. 选择**“与 Visual Studio 配对”**。

    ![Pair with Visual Studio dialog](images/devhome_visualstudio.png)

  1. 在**“与 Visual Studio 配对”**对话框中输入你的 PIN。 以下 PIN 只是一个示例；你的 PIN 将有所不同。

    ![Pair with Visual Studio PIN dialog](images/devhome_pin.png)

  1. 部署错误（如果有）将显示在**“输出”**窗口中。

恭喜，你已在 Xbox 上成功创建和部署了你的第一个 UWP 应用！



## 另请参阅
- [在 Xbox One 上启用开发人员模式](devkit-activation.md)  
- [适用于 Windows 10 的下载和工具](https://dev.windows.com/downloads)  
- [下载适用于开发人员的 Insider Preview 更新](http://go.microsoft.com/fwlink/?LinkId=780552)  
- [Xbox One 工具简介](introduction-to-xbox-tools.md) 
- [Xbox One 上的 UWP](index.md)

----


<!--HONumber=Mar16_HO5-->


