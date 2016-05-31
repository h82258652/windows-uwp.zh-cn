---
Description: author:QuinnRadich Windows 10 版本 1511 和开发人员工具更新继续提供受通用 Windows 平台支持的工具、功能和体验。
title: Windows 10 版本 1511 中面向开发人员的新增功能：2015 年 11 月
---

# Windows 10 版本 1511 中面向开发人员的新增功能：2015 年 11 月

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Windows 10 版本 1511 和开发人员工具更新继续提供受通用 Windows 平台支持的工具、功能和体验。 在 Windows 10 版本 1511 上[安装这些工具和 SDK](https://dev.windows.com/downloads) 后，你就可以随时[创建新的通用 Windows 应用](https://msdn.microsoft.com/library/windows/apps/bg124288)或了解如何使用 [Windows 上的现有应用代码](https://msdn.microsoft.com/library/windows/apps/mt238321)。

## 用户体验

新的 <a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.startscreen.aspx">Windows.UI.StartScreen.JumpList</a> 和 <a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.startscreen.aspx">Windows.UI.StartScreen.JumpListItem</a> 类为应用提供的功能有：以编程方式选择它们需要使用的系统管理的跳转列表类型、向其跳转列表添加自定义任务入口点以及向其跳转列表添加自定义组。

## 输入
                                        
* <a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.input.keyboarddeliveryinterceptor.aspx">键盘传递侦听器</a>
                                        
    使应用能够替代系统对原始键盘输入的处理，包括快捷键、访问键（或热键）、加速键和应用程序键，但不包括安全注意序列 (SAS) 组合键。

    安全注意序列 (SAS) 组合键（包括 Ctrl-Alt-Del 和 Windows-L）继续由系统处理。
                                        
* 同时适用于 <a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.core.corewindow.aspx">UWP 应用</a>和<a href="https://msdn.microsoft.com/library/windows/desktop/hh454903(v=vs.85).aspx">经典 Windows 应用</a>的指针输入跨进程链接。
                                        
    支持输入跨进程链接的新指针事件。    
                                        
* <a href="https://msdn.microsoft.com/library/windows/desktop/mt622165(v=vs.85).aspx">适用于经典桌面应用的墨迹演示器</a>
                                        
    墨迹表示器 API 使 Microsoft Win32 应用能够通过插入应用的 <a href="https://msdn.microsoft.com/library/windows/desktop/hh437371(v=vs.85).aspx">DirectComposition</a> 可视化树中的 <a href="https://msdn.microsoft.com/library/windows/desktop/windows.ui.input.inking.inkpresenter.aspx">InkPresenter</a> 对象管理墨迹输入（标准和改良）的输入、处理和呈现。    
                                    
## 网络
                                                                        
对于 Websocket 用户：<a href="https://msdn.microsoft.com/library/windows/apps/windows.storage.streams.datawriter.flushasync.aspx">MessageWebSocket.OutputStream.FlushAsync</a> 和 <a href="https://msdn.microsoft.com/library/windows/apps/windows.storage.streams.datawriter.flushasync.aspx">StreamWebSocket.OutputStream.FlushAsync</a> 已完全实现，并等待之前发出的 WriteAsync 调用完成。 请注意，如果 WebSocket 在你调用 <a href="https://msdn.microsoft.com/library/windows/apps/windows.storage.streams.datawriter.flushasync.aspx">FlushAsync</a> 时处于无效状态，这可能导致现有代码引发异常。    

新属性 <a href="https://msdn.microsoft.com/library/windows/apps/windows.web.http.filters.httpbaseprotocolfilter.aspx">CookieUsageBehavior</a> 已添加到现有的 <a href="https://msdn.microsoft.com/library/windows/apps/windows.web.http.filters.httpbaseprotocolfilter.aspx">Windows.Web.Http.Filters.HttpBaseProtocolFilter 类</a>。 这使得开发人员能够控制系统处理 Cookie 的方式。    
                                    
## ORTC
                                    
Microsoft Edge 现在实现了 <a href="https://msdn.microsoft.com/library/mt433097(v=vs.85).aspx">ORTC（对象实时通信）</a>，支持通过本地 Javascript API 直接在浏览器、移动设备和服务器之间进行 Web 实时音频/视频通话。 开发人员现在可以使用 ORTC API 在 Microsoft Edge 浏览器顶部生成支持群组视频通话、同时联播、可伸缩视频编码 (SVC) 等的高级实时音频/视频通信应用程序。    

有关在 Microsoft Edge 浏览器之间通过 ORTC API 进行 1:1 音频/视频通话的演示，请访问 <a href="/microsoft-edge/testdrive/demos/ortcdemo/">Test Drive 站点和演示</a>。 有关概述和代码示例演练，请访问 <a href="https://msdn.microsoft.com/library/mt588497(v=vs.85).aspx">ORTC 开发人员指南条目</a>。
                                        
## Microsoft Edge F12 开发人员工具
                                                                        
Microsoft Edge 向 F12 开发人员工具引入了一些出色的新改进，包括 <a href="https://wpdev.uservoice.com/forums/257854-microsoft-edge-developer">UserVoice</a> 中某些最受期待的功能。 了解关于 DOM 资源管理器、控制台、调试器、网络、性能、内存、仿真方面的新增功能，并了解新的实验工具，该工具允许你在强大的新功能完成开发之前先试用它们。 这些新工具内置于 TypeScript 中，并且会一直运行，因此无需重新加载。 此外，F12 开发人员工具文档现在是 <a href="http://dev.modern.ie/">Microsoft Edge 开发人员站点</a>的一部分，并且在 <a href="https://github.com/MicrosoftEdge/MicrosoftEdge-Documentation">GitHub</a> 上完整提供。 从此时起，这些文档将不仅受你的反馈影响，我们还会邀请你帮助我们共同打造文档。 有关 F12 开发人员工具的视频简介，请访问<a href="https://channel9.msdn.com/Blogs/One-Dev-Minute/Microsoft-Edge-F12-tools">第 9 频道的 One Dev Minute</a>。    
                                    
## Windows Hello
                                    
Windows Hello 支持你的应用通过面部或指纹识别来登录到 Windows 系统或设备。

提供程序 API 允许 IHV 和 OEM 在 UWP 中公开计算机影像的深度、红外和彩色相机（以及相关元数据），并在进行 Windows Hello 面部身份验证时指定一个相机。 <a href="http://go.microsoft.com/fwlink/?LinkId=691697">Windows.Devices.Perception</a> 命名空间包含允许 UWP 应用程序访问计算机影像相机的颜色、深度或红外数据的客户端 API。
                                    
## 新的游戏 API

使用新的 Windows.Gaming.UI.GameBar 类以便在显示或隐藏游戏栏时接收通知。    
                            
                                    
## 蓝牙 API
                                    
已添加并更新多个 API，以扩展对蓝牙 LE、设备枚举以及蓝牙中其他功能的支持。 请参阅 <a href="https://msdn.microsoft.com/library/windows/apps/windows.devices.bluetooth.aspx">Windows.Devices.Bluetooth</a> 命名空间。    
                                   
## 智能卡 API ## 

已向 <a href="https://msdn.microsoft.com/library/windows/apps/windows.devices.smartcards.aspx">Windows.Devices.SmartCards</a> 命名空间添加了多个 SmartCardCryptogram API，以支持安全密文付款协议。 使用主机卡仿真来支持触碰以支付的付款应用可以使用这些 API 获得额外的安全性和性能。 应用可以使用 TPM 创建密钥并保护有限使用的交易密钥。 应用还可以利用 NGC（下一代凭据）框架通过用户的 PIN 来保护密钥。 这些 API 会将密文生成委派给系统以增强性能。 这还可以防止其他应用访问这些密钥和密文。    
                                    
## 已更新的存储 API ## 
    
<a href="https://msdn.microsoft.com/library/windows/apps/windows.storage.downloadsfolder.aspx">Windows.Storage.DownloadsFolder 类</a><br />
现在，你的应用可以在“下载”文件夹内针对特定<a href="https://msdn.microsoft.com/library/windows/apps/windows.system.user.aspx">用户</a><a href="https://msdn.microsoft.com/library/windows/apps/windows.storage.downloadsfolder.createfileforuserasync.aspx">创建文件</a>或<a href="https://msdn.microsoft.com/library/windows/apps/windows.storage.downloadsfolder.createfolderforuserasync.aspx">创建文件</a>。
                                            
<a href="https://msdn.microsoft.com/library/windows/apps/windows.storage.storagelibrary.aspx">Windows.Storage.StorageLibrary 类</a><br />
现在，你的应用可以针对特定<a href="https://msdn.microsoft.com/library/windows/apps/windows.system.user.aspx">用户</a><a href="https://msdn.microsoft.com/library/windows/apps/windows.storage.storagelibrary.getlibraryforuserasync.aspx">获取指定的库</a>。
                                    
## Windows 应用认证工具包 ## 
                                    
Windows 应用认证工具包已通过改进的测试进行了更新。 有关更新的完整列表，请访问 <a href="/develop/app-certification-kit">Windows 应用认证工具包</a>页面。    
                                    
## 设计下载 ## 

查看适用于 Adobe Photoshop 的全新 UWP 应用设计模板。 我们还更新了 Microsoft PowerPoint 和 Adobe Illustrator 模板，并提供了 PDF 版本的指南。 <a href="/design/assets">访问设计下载页面</a>。    




<!--HONumber=May16_HO2-->


