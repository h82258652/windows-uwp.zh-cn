---
title: Xbox 设备门户 REST API
description: 适用于 Xbox One 上的 UWP 的 API 参考。
ms.date: 10/25/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 5ae8e953-0465-487b-81dd-54a85c904daf
ms.localizationpriority: medium
ms.openlocfilehash: d8fdcf01d7d1f72624d73acf2d10ce28dfb75e04
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2018
ms.locfileid: "8709723"
---
# <a name="xbox-device-portal-rest-api"></a>Xbox 设备门户 REST API

本部分包含 Xbox 设备门户 REST API 参考主题，用于远程配置和管理你的主机。

| URI        | 描述 |
|------------|-------------|
|[/api/app/packagemanager/register](wdp-loose-folder-register-api.md)| 注册包含于 Loose 文件夹中的应用。 |
|[/api/app/packagemanager/upload](wdp-folder-upload.md)| 将整个文件夹上载到主机。 |
|[/ext/app/sshpins](uwp-sshpins-api.md)| 远程清除所有受信任的 SSH 固定。 若要进行 Visual Studio UWP 开发，将需要重新进行固定配对。 |
|[/ext/app/deployinfo](uwp-deployinfo-api.md)| 请求一个或多个已安装程序包的部署信息。 |
|[/ext/fiddler](wdp-fiddler-api.md)| 启用和禁用 Fiddler 网络跟踪。 |
|[/ext/httpmonitor/sessions](wdp-httpMonitor-api.md)| 获取来自 Xbox 上侧重型应用的 HTTP 流量。 |
|[/ext/networkcredential](uwp-networkcredentials-api.md)| 添加、删除或更新网络凭据。 |
|[/ext/remoteinput](uwp-remoteinput-api.md)| 将键盘、鼠标或控制器输入远程发送到 Xbox。 |
|[/ext/remoteinput/controllers](uwp-remoteinput-controllers-api.md)| 获取连接的物理控制器的数量或关闭所有物理控制器。 |
|[/ext/screenshot](wdp-media-capture-api.md)| 捕获主机当前显示的屏幕，格式为 PNG。 |
|[/ext/settings](wdp-xboxsettings-api.md)| 访问 Xbox One 开发人员设置。 |
|[/ext/smb/developerfolder](wdp-smb-api.md)| 通过开发电脑上的“文件资源管理器”访问控制台上的开发人员文件夹。 |
|[/ext/user](wdp-user-management.md)| 管理 Xbox One 主机上的用户。 |
|[/ext/xbox/info](wdp-xboxinfo-api.md)| 提供有关 Xbox One 设备的信息。 |
|[/ext/xboxlive/sandbox](wdp-sandbox-api.md)| 管理 Xbox Live 沙盒。 |

## <a name="see-also"></a>另请参阅

- [Xbox One 上的 UWP](index.md)
- [Windows 设备门户](../debug-test-perf/device-portal.md)