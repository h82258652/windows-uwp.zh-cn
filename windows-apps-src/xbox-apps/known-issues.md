---
author: Mtoepke
title: "Xbox One 开发人员计划上的 UWP 已知问题"
description: 
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: a7b82570-1f99-4bc3-ac78-412f6360e936
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 4b13b9bbbc75de47ed69112680894d5e3f34d8a1
ms.lasthandoff: 02/08/2017

---

# <a name="known-issues-with-uwp-on-xbox-developer-program"></a>Xbox 开发人员计划上的 UWP 已知问题

本主题介绍 Xbox One 开发人员计划上的 UWP 已知问题。 有关此计划的详细信息，请参阅 [Xbox 上的 UWP](index.md)。 

\[如果你通过某个 API 参考主题中的链接转到此处，并且要查找通用设备系列 API 信息，请参阅 [Xbox 上尚不支持的 UWP 功能](http://go.microsoft.com/fwlink/?LinkID=760755)。\]

下表重点介绍可能遇到的某些已知问题，但该列表并没有包括所有问题。 

**我们希望收到你的反馈**，因此请在[开发通用 Windows 平台应用](https://social.msdn.microsoft.com/forums/windowsapps/home?forum=wpdevelop)论坛上报告你发现的任何问题。 

如果遇到问题，请阅读本主题中的信息、参阅[常见问题](frequently-asked-questions.md)以及使用论坛来寻求帮助。


<!--## Developing games-->

## <a name="issue-when-leaving-dev-mode"></a>退出开发人员模式时出现问题
有时，可能会发生你无法使开发人员模式退出使用 DevHome 或开发人员设置的情况。
这有两个可能的解决方法： 
1. 离开开发人员模式时取消选中标记为**删除旁加载的游戏和应用**的复选框
2. 转到“我的游戏和应用”，并卸载安装在控制台中的开发人员应用。
 
<!--## Memory limits for background apps are partially enforced
 
The maximum memory footprint for apps running in the background is 128 megabytes. In the current version of UWP on Xbox One, your app will be suspended if it is above this limit when it is moved to the background. This limit is not currently enforced if your app exceeds the limit while it is already running in the background—this means that if your app exceeds 128 MB while running in the background, it will still be able to allocate memory.
 
There is currently no workaround for this issue. Apps should govern their memory usage accordingly and continue to stay under the 128 MB limit while running in the background.-->
 
## <a name="deploying-from-vs-fails-with-parental-controls-turned-on"></a>在“家长控制”处于打开状态的情况下，无法通过 VS 进行部署

如果主机在“设置”中打开了“家长控制”，则通过 VS 启动你的应用会失败。

若要解决该问题，可以临时禁用“家长控制”，也可以：
1. 在“家长控制”处于关闭状态的情况下，将你的应用部署到该主机。
2. 打开“家长控制”。
3. 从主机启动你的应用。
4. 输入 PIN 或密码以允许应用启动。
5. 应用将启动。
6. 关闭应用。
7. 从 VS 中使用 F5 进行启动，然后应用将在没有任何提示的情况下启动。

此时，权限是_粘连的_，除非你注销用户，否则即使卸载并重新安装该应用也是如此。
 
有另外一种仅适用于子女帐户的免除类型。 子女帐户需要家长登录以授予权限，但当家长将选项选择为**始终**时，他们就可以允许孩子启动该应用。 该免除存储在云中且持续有效，即使孩子注销并重新登录也是如此。   

<!--### x86 vs. x64

By the time we release later this year, we will have great support for both x86 and x64, and we do support x86 in this preview. 
However, x64 has had much more testing to date (the Xbox shell and all of the apps running on the console today are x64), and so we recommend using x64 for your projects. 
This is particularly true for games.

If you decide to use x86, please report any issues you see on the forum.

Also see [Switching build flavors can cause deployment failures](known-issues.md#switching-build-flavors-can-cause-deployment-failures) later on this page.-->

<!--### Game engines

We have tested some popular game engines, but not all of them, and our test coverage for this preview has not been comprehensive. 
Your mileage may vary. 

The following game engines have been confirmed to work:
* [Construct 2](https://www.scirra.com/)

There are likely others that are working too. We would love to get your feedback on what you find. 
Please use the forum to report any issues you see.-->

## <a name="directx-12-support"></a>DirectX 12 支持

Xbox One 上的 UWP 支持 DirectX 11 功能级别 10。 DirectX 12 在此情况下不受支持。 

Xbox One（类似于所有的传统游戏主机）是一个专门的硬件，需要特定 SDK 才可以充分发挥其潜能。 如果你正在开发需要最大限度发挥 Xbox One 硬件潜能的游戏，你可以注册 [ID@XBOX](http://www.xbox.com/Developers/id) 计划来获取该 SDK 的访问权限（包括 DirectX 12 支持）。

<!-- ### Xbox One Developer Preview disables game streaming to Windows 10

Activating the Xbox One Developer Preview on your console will prevent you from streaming games from your Xbox One to the Xbox app on Windows 10, even if your console is set to retail mode. 
To restore the game streaming feature, you must leave the developer preview. -->

<!--## System resources for UWP apps and games on Xbox One

UWP apps and games running on Xbox One share resources with the system and other apps, and so the system governs the resources that are available to any one game or app. 
If you are running into memory or performance issues, this may be why. 
For more details, see [System resources for UWP apps and games on Xbox One](system-resource-allocation.md).-->

<!--
## Networking using traditional sockets

In this developer preview, inbound and outbound network access from the console that uses traditional TCP/UDP sockets (WinSock, Windows.Networking.Sockets) is not available. 
Developers can still use HTTP and WebSockets.
--> 

## <a name="blocked-networking-ports-on-xbox-one"></a>阻止 Xbox One 上的网络端口

限制 Xbox One 设备上的通用 Windows 平台 (UWP) 应用绑定到 [57344, 65535] 范围中的端口，非独占。 尽管在运行时可能显示已成功绑定到这些端口，但网络流量在到达应用前已悄然断掉。 应用应在允许条件下绑定到端口 0，这支持系统选择本地端口。 如果需要使用特定端口，端口号必须在 [1025, 49151] 范围中，而且应该查看 IANA 注册表并避免与之相冲突。 有关详细信息，请参阅[服务名称和传输协议端口号注册表](http://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.xhtml)。

## <a name="uwp-api-coverage"></a>UWP API 覆盖范围

并非所有 UWP API 在 Xbox 上都受支持。 对于我们已知不起作用的 API 列表，请参阅 [Xbox 上尚不支持的 UWP 功能](http://go.microsoft.com/fwlink/p/?LinkId=760755)。 如果你发现其他 API 的问题，请通过论坛报告它们。 

<!--## XAML controls do not look like or behave like the controls in the Xbox One shell

In this developer preview, the XAML controls are not in their final form. In particular:
* Gamepad X-Y navigation does not work reliably for all controls.
* Controls do not look like controls in the Xbox shell. This includes the control focus rectangle.
* Navigating between controls does not automatically make “navigation sounds.”

These issues will be addressed in a future developer preview.-->

<!--## Visual Studio and deployment issues

### Switching build flavors can cause deployment failures

Switching between Debug and Release builds, or between x86 and x64, or between Managed and .Net Native builds, can cause deployment failures. 

The simplest way to avoid these issues for this preview is to stick to Debug and one architecture. 

If you do hit this issue, uninstalling your app in the Collections app on your Xbox One will typically resolve it.

> ****&nbsp;&nbsp;Uninstalling your app from Windows Device Portal (WDP) will not resolve the issue.

If your issues persist, uninstall your app or game in the Collections app, leave Developer Mode, restart to Retail Mode and then switch back to Developer Mode.
You may also need to restart Visual Studio and clean your solution.

For more information, see the “Fixing deployment failures” section in [Frequently asked questions](frequently-asked-questions.md).

### Uninstalling an app while you are debugging it in Visual Studio will cause it to fail silently

Attempting to uninstall an app that is running under the debugger via the WDP “Installed Apps” tool will cause it to silently fail. 
The workaround is to stop debugging the app in Visual Studio before attempting to remove it via WDP.

### Visual Studio/Xbox PIN pairing failures

It is possible to get into a state where the PIN pairing between Visual Studio and your Xbox One gets out of sync. 
If PIN pairing fails, use the “Remove all pairings” button in Dev Home, restart Xbox One, restart your development PC, and then try again.--> 


<!--## Windows Device Portal (WDP) preview-->

<!--### Starting WDP from Dev Home crashes Dev Home

When you start WDP in Dev Home, it will cause Dev Home to crash after you have entered your user name and password and selected **Save**. 
The credentials are saved but WDP is not started. 
You can start WDP by restarting Xbox One.--> 

<!--### Disabling WDP in Dev Home does not work

If you disable WDP in Dev Home, it will be turned off. 
However, when you restart your Xbox One, WDP will be started again. 
You can work around this issue by using **Reset and keep my games & apps** to delete any stored state on your Xbox One. 
Go to Settings > System > Console info & updates > Reset console, and then select the **Reset and keep my games & apps** button.

> **Caution**&nbsp;&nbsp;Doing this will delete all saved settings on your Xbox One including wireless settings, user accounts and any game progress that has not been saved to cloud storage.

> **Caution**&nbsp;&nbsp;DO NOT select the **Reset and remove everything** button.
This will delete all of your games, apps, settings and content, deactivate Developer Mode, and remove you console from the Developer Preview group.

### The columns in the “Running Apps” table do not update predictably. 

Sometimes this is resolved by sorting a column on the table.-->

## <a name="navigating-to-wdp-causes-a-certificate-warning"></a>导航到 WDP 导致证书警告

你将收到已提供证书的警告（类似于以下屏幕截图），因为 Xbox One 控制台签名的安全证书不被视为众所周知的受信任发布者。 若要访问 Windows Device Portal，请单击**继续浏览此网站**。

![网站安全证书警告](images/security_cert_warning.jpg)

<!--## Dev Home

Occasionally, selecting the “Manage Windows Device Portal” option in Dev Home will cause Dev Home to silently exit to the Home screen. 
This is caused by a failure in the WDP infrastructure on the console and can be resolved by restarting the console.-->

## <a name="knownfoldersmediaserverdevices-caveat-on-xbox"></a>Xbox 上的 KnownFolders.MediaServerDevices 警告

在桌面上，媒体服务器与电脑“配对”，设备关联服务不断跟踪哪些服务器当前在线，因此初始文件系统查询可以立即返回当前在线的配对服务器的列表。

在 Xbox 上，没有用于添加或删除服务器的 UI，因此初始文件系统查询将始终返回空。 你必须创建一个查询并订阅 ContentsChanged 事件，同时在每次收到通知时刷新查询。 服务器将陆续出现，大多数会在 3 秒内被发现。

简单的示例代码：

```
namespace TestDNLA {

    public sealed partial class MainPage : Page {
        public MainPage() {
            this.InitializeComponent();
        }

        private async void FindFiles_Click(object sender, RoutedEventArgs e) {
            try {
                StorageFolder library = KnownFolders.MediaServerDevices;
                var folderQuery = library.CreateFolderQuery();
                folderQuery.ContentsChanged += FolderQuery_ContentsChanged;
                IReadOnlyList<StorageFolder> rootFolders = await folderQuery.GetFoldersAsync();
                if (rootFolders.Count == 0) {
                    Debug.WriteLine("No Folders found");
                } else {
                    Debug.WriteLine("Folders found");
                }
            } catch (Exception ex) {
                Debug.WriteLine("Error: " + ex.Message);
            } finally {
                Debug.WriteLine("Done");
            }
        }

        private async void FolderQuery_ContentsChanged(Windows.Storage.Search.IStorageQueryResultBase sender, object args) {
            Debug.WriteLine("Folder added " + sender.Folder.Name);
            IReadOnlyList<StorageFolder> topLevelFolders = await sender.Folder.GetFoldersAsync();
            foreach (StorageFolder topLevelFolder in topLevelFolders) {
                Debug.WriteLine(topLevelFolder.Name);
            }
        }
    }
}
```

## <a name="see-also"></a>另请参阅
- [常见问题](frequently-asked-questions.md)
- [Xbox One 上的 UWP](index.md)

