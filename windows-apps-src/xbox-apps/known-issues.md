---
author: Mtoepke
title: Xbox 开发人员计划上的 UWP 已知问题
description: 列出 Xbox 开发人员计划上的 UWP 已知问题。
ms.author: mstahl
ms.date: 03/29/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: a7b82570-1f99-4bc3-ac78-412f6360e936
ms.localizationpriority: medium
ms.openlocfilehash: 798192dd898af5a7107087b4a9708e1a1d0cb9b5
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2018
ms.locfileid: "6657175"
---
# <a name="known-issues-with-uwp-on-xbox-developer-program"></a>Xbox 开发人员计划上的 UWP 已知问题

本主题介绍 Xbox One 开发人员计划上的 UWP 已知问题。 有关此计划的详细信息，请参阅 [Xbox 上的 UWP](index.md)。 

\[如果你通过某个 API 参考主题中的链接转到此处，并且要查找通用设备系列 API 信息，请参阅 [Xbox 上尚不支持的 UWP 功能](http://go.microsoft.com/fwlink/?LinkID=760755)。\]

下表重点介绍可能遇到的某些已知问题，但该列表并没有包括所有问题。 

**我们希望收到你的反馈**，因此请在[开发通用 Windows 平台应用](https://social.msdn.microsoft.com/forums/windowsapps/home?forum=wpdevelop)论坛上报告你发现的任何问题。 

如果遇到问题，请阅读本主题中的信息、参阅[常见问题](frequently-asked-questions.md)以及使用论坛来寻求帮助。

 
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

## <a name="storagefilecopyasync-fails-to-copy-encrypted-files-to-unencrypted-destination"></a>StorageFile.CopyAsync 无法将加密的文件复制到未加密的目标 

使用 StorageFile.CopyAsync 将加密的文件复制到未加密的目标时，调用将失败，并会出现以下异常：

```
System.UnauthorizedAccessException: Access is denied. (Excep_FromHResult 0x80070005)
```

这可能会影响想要将其应用包中部署的文件复制到另一个位置的 Xbox 开发人员。 其原因是程序包内容是在零售模式下在 Xbox 上加密的，但未在开发模式下加密。 因此，应用可能会在开发和测试期间按预期方式工作，但该应用一旦发布然后安装到零售 Xbox 上，就会失败。
 

## <a name="blocked-networking-ports-on-xbox-one"></a>阻止 Xbox One 上的网络端口

限制 Xbox One 设备上的通用 Windows 平台 (UWP) 应用绑定到 [57344, 65535] 范围中的端口，非独占。 尽管在运行时可能显示已成功绑定到这些端口，但网络流量在到达应用前已悄然断掉。 应用应在允许条件下绑定到端口 0，这支持系统选择本地端口。 如果需要使用特定端口，端口号必须在 [1025, 49151] 范围中，而且应该查看 IANA 注册表并避免与之相冲突。 有关详细信息，请参阅[服务名称和传输协议端口号注册表](http://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.xhtml)。

## <a name="uwp-api-coverage"></a>UWP API 覆盖范围

并非所有 UWP API 在 Xbox 上都受支持。 对于我们已知不起作用的 API 列表，请参阅 [Xbox 上尚不支持的 UWP 功能](http://go.microsoft.com/fwlink/p/?LinkId=760755)。 如果你发现其他 API 的问题，请通过论坛报告它们。 


## <a name="navigating-to-wdp-causes-a-certificate-warning"></a>导航到 WDP 导致证书警告

你将收到已提供证书的警告（类似于以下屏幕截图），因为 Xbox One 控制台签名的安全证书不被视为众所周知的受信任发布者。 若要访问 Windows Device Portal，请单击**继续浏览此网站**。

![网站安全证书警告](images/security_cert_warning.jpg)


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
