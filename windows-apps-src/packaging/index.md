---
author: laurenhughes
ms.assetid: 1abcbb13-80f0-4bf1-a812-649ee8bd1915
title: "打包应用"
description: "本部分包含或链接到有关打包通用 Windows 平台 (UWP) 应用的文章。"
translationtype: Human Translation
ms.sourcegitcommit: 28cd2b2a922a20e0b9ffc4d1ca65f6a55e92aa8f
ms.openlocfilehash: 8aa4bfc8004b4d0739fe09e6e49e66de372569c4

---
# <a name="packaging-apps"></a>将应用打包

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

## <a name="purpose"></a>作用

本部分包含或链接到有关打包通用 Windows 平台 (UWP) 应用的文章。

| 主题 | 说明 |
|-------|-------------|
| [打包 UWP 应用](packaging-uwp-apps.md) | 若要出售你的 UWP 应用或将其分配给其他用户，你需要为其创建一个 appxupload 程序包。 当你创建 appxupload 时，将生成另一个 appx 程序包用于测试和旁加载。 你可以通过将 appx 程序包旁加载到设备来直接分配你的应用。 本文介绍了配置、创建和测试 UWP 应用包的过程。 有关旁加载的详细信息，请参阅[使用 DISM 旁加载应用](http://go.microsoft.com/fwlink/?LinkID=231020)。 |
| [使用 MakeAppx.exe 工具创建应用包](create-app-package-with-makeappx-tool.md) | MakeAppx.exe 创建、加密、解密应用程序包和捆绑包，并从中提取文件。 |
| [使用 WinAppDeployCmd.exe 工具安装应用](install-universal-windows-apps-with-the-winappdeploycmd-tool.md) | Windows 应用程序部署 (WinAppDeployCmd.exe) 是一个命令行工具，可用于将 UWP 应用从 Windows 10 计算机部署到任意 Windows 10 移动版设备。 当 Windows 10 移动版设备通过 USB 进行连接或无需 Microsoft Visual Studio 或该应用的解决方案即可连接到同一子网时，可使用此工具部署 .appx 程序包。 本文介绍如何使用此工具安装 UWP 应用。 |
| [设置 UWP 应用的自动生成](auto-build-package-uwp-apps.md) | 如果你希望在自动生成过程中打包应用，本主题介绍如何使用 Visual Studio Team Services (VSTS) 执行此操作。 |
| [应用功能声明](app-capability-declarations.md) | 功能必须在你的 UWP 应用的[程序包清单](https://msdn.microsoft.com/library/windows/apps/BR211474)中声明，以便可用于访问某些 API 或资源（如图片、音乐）或者设备（如相机或麦克风）。 |
| [为你的应用下载并安装程序包更新](self-install-package-updates.md) | 你的 UWP 应用可以以编程方式检查程序包更新，并安装这些更新。 你的应用还可以查询已在 Windows 开发人员中心仪表板上标记为必需的程序包，以及完成安装该必需更新前禁用功能。 本文介绍了如何执行这些任务。 |
 



<!--HONumber=Dec16_HO1-->


