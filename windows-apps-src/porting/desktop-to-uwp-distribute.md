---
author: awkoren
Description: "分配使用桌面到 UWP 桥转换的 UWP 应用"
Search.Product: eADQiWindows 10XVcnh
title: "分配使用桌面到 UWP 桥转换的 UWP 应用"
ms.author: alkoren
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: edff3787-cecb-4054-9a2d-1fbefa79efc4
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 0acb144d79c1d05d68cc7430f4cc99efeadc7c6b
ms.lasthandoff: 02/08/2017

---

# <a name="distribute-apps-converted-with-the-desktop-bridge"></a>分配使用桌面桥转换的应用

部署已转换的应用有三种主要方法：Windows 应用商店、旁加载和松散文件注册。  

## <a name="windows-store"></a>Windows 应用商店

Windows 应用商店是最便利的客户获取应用的方法。 若要开始使用，请在[使用桌面桥将现有应用和游戏发布到 Windows 应用商店](https://developer.microsoft.com/windows/projects/campaigns/desktop-bridge)中填写表格。 Microsoft 将联系你以开始培训过程。 

请注意，你需要是应用或游戏的开发人员和/或发布者才能将其引入 Windows 应用商店。 因此，请确保你的名称和电子邮件地址与你以下面的 URL 形式提交的网站匹配，以便我们可以验证你是否是开发人员和/或发布者。

## <a name="sideloading"></a>旁加载

旁加载提供跨多台计算机部署的简单方法。 在企业/业务线 (LOB) 方案中尤其有用，在这些方案中你需要对分配体验有更精细的控制，并且不希望涉及应用商店证书。

在通过旁加载应用部署应用前，你将需要使用证书对其签名。 有关创建证书的信息，请参阅[对 .Appx 包签名](https://msdn.microsoft.com/windows/uwp/porting/desktop-to-uwp-run-desktop-app-converter#deploy-your-converted-appx)。 

下面介绍如何导入你之前创建的证书。 你可以直接使用 CERTUTIL 导入证书，或者可以从你已签名的 appx 中安装它，就像客户所做的那样。 

若要通过 CERTUTIL 安装证书，请从管理员命令提示符中运行以下命令：

```cmd
Certutil -addStore TrustedPeople <testcert.cer>
```

若要像客户所做的那样从 appx 导入证书：

1.    在“文件资源管理器”中，右键单击你已使用测试证书签名的 appx，然后从上下文菜单中选择**属性**。
2.    单击或点击**数字签名**选项卡。
3.    单击或点击该证书并选择**详细信息**。
4.    单击或点击**查看证书**。
5.    单击或点击**安装证书**。
6.    在**存储位置**组中，选择**本地计算机**。
7.    依次单击或点击**下一步**和**确定**来确认 UAC 对话框。
8.    在证书导入向导的下一个屏幕中，将选定的选项更改为**将所有的证书放入下列存储**。
9.    单击或点击**浏览**。 在“选择证书存储”窗口中，向下滚动并选择**受信任人**，然后单击或点击**确定**。
10.    单击或点击**下一步**。 将出现新屏幕。 单击或点击**完成**。
11.    应出现确认对话框。 如果出现，请单击**确定**。 如果出现另一个指示证书存在问题的对话框，则可能需要执行某些证书疑难解答操作。

注意：若要使 Windows 信任证书，证书必须位于**证书(本地计算机) > 受信任根证书颁发机构 > 证书**节点或**证书(本地计算机) > 受信任人 > 证书**节点中。 仅这两个位置中的证书可以在本地计算机的上下文中验证证书信任。 否则，将显示类似于以下字符串的错误消息：

```CMD
"Add-AppxPackage : Deployment failed with HRESULT: 0x800B0109, A certificate chain processed,
but terminated in a rootcertificate which is not trusted by the trust provider.
(Exception from HRESULT: 0x800B0109) error 0x800B0109: The root certificate of the signature
in the app package must be trusted."
```

现在证书已获得信任，可使用 2 种方法安装程序包：通过 PowerShell 或只需双击应用包文件即可安装。  若要通过 PowerShell 安装，请运行以下 cmdlet：

```powershell
Add-AppxPackage <MyApp>.appx
```

### <a name="loose-file-registration"></a>松散文件注册

松散文件注册非常适用于调试目的，即文件放置在磁盘上的可供你轻松访问和更新的位置中，并且不需要签名或证书。  

若要在部署期间部署应用，请运行以下 PowerShell cmdlet： 

```Add-AppxPackage –Register AppxManifest.xml```

若要更新应用的 .exe 或 .dll 文件，只需将程序包中的现有文件替换为新文件、增加 AppxManifest.xml 中的版本号，然后再次运行上述命令。

注意以下情况： 

* 安装转换的应用的任何驱动器都必须设置为 NTFS 格式。
* 已转换的应用始终以交互用户身份运行。
