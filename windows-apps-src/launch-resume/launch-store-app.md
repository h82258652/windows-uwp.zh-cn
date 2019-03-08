---
title: 打开 Microsoft Store 应用
description: 本主题介绍了 ms-windows-store URI 方案。 您的应用程序可以使用此 URI 方案以启动 Microsoft Store 应用到应用商店中的特定页。
ms.assetid: 9A9C6576-1637-47D1-AC3B-D1A20D49E0FF
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: cda37ee9964a3e7e02f4e4ce3829a8b55e823692
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660892"
---
# <a name="launch-the-microsoft-store-app"></a>打开 Microsoft Store 应用



本主题介绍**ms windows 应用商店：** URI 方案。 您的应用程序可以使用此 URI 方案通过启动到存储区中的特定页的 Microsoft Store 应用[ **LaunchUriAsync** ](https://msdn.microsoft.com/library/windows/apps/hh701476)方法。

此示例显示了如何在“游戏”页面中打开 Microsoft Store：

```cs
bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-windows-store://navigatetopage/?Id=Games"));
```

## <a name="ms-windows-store-uri-scheme-reference"></a>ms-windows-应用商店：URI 方案引用

<table>
<tr><th>描述</th><th></th><th>URI 方案</th></tr>
<tr><td>启动应用商店的主页。</td><td /><td>ms-windows-store://home</td></tr>
<tr><td>启动应用商店中的顶级垂直类别内容。<p>注意：并非所有用户都有权访问所有纵向市场。</p>
</td><td /><td>
<p>ms-windows-store://navigatetopage/?Id=Apps </p>
<p>ms-windows-store://navigatetopage/?Id=Games</p>
<p>ms-windows-store://navigatetopage/?Id=Music</p>
<p>ms-windows-store://navigatetopage/?Id=Video</p>
<p>ms-windows-store://navigatetopage/?Id=LOB</p>
</td>
</tr>
<tr>
<td rowspan="4">启动产品的产品详细信息页面 (PDP)。 <p>Store ID 推荐用于 Windows 10 上的客户，其将适用于所有操作系统版本，但执行此操作时的早期方法 (例如：PFN) 仍然是支持的。</p>
<p>这些值可在<a href="https://partner.microsoft.com/dashboard">合作伙伴中心</a>上<a href="https://msdn.microsoft.com/library/windows/apps/mt148561.aspx">应用标识</a>页为每个应用的应用程序管理部分。</p>
</td>
<td>
Store ID <p>（建议）</p>
</td>
<td>
<p>ms-windows-store://pdp/?ProductId=9WZDNCRFHVJL</p>
</td>
</tr>
<tr>
<td>程序包系列名称 (PFN)</td>
<td>ms-windows-store://pdp/?PFN= Microsoft.Office.OneNote_8wekyb3d8bbwe
</td>
</tr>
<tr>
<td>产品 ID (Windows Phone 7.x/8.x)</td>
<td>ms-windows-store://pdp/?PhoneAppId=ca05b3ab-f157-450c-8c49-a1f127f5e71d</td>
</tr>
<tr>
<td>产品 ID (Windows 8.x)</td>
<td>ms-windows-store://pdp/?AppId=f022389f-f3a6-417e-ad23-704fbdf57117
</td>
</tr>
<tr>
<td rowspan="4">启动产品的写评论体验。</td>
<td>Store ID <p>（建议）</p></td>
<td>ms-windows-store://review/?ProductId=9WZDNCRFHVJL </td>
</tr>
<tr>
<td>程序包系列名称 (PFN)</td>
<td>ms-windows-store://review/?PFN= Microsoft.Office.OneNote_8wekyb3d8bbwe
</td>
</tr>
<tr>
<td>产品 ID (Windows Phone 7.x/8.x)</td>
<td>ms-windows-store://reviewapp/?AppId=ca05b3ab-f157-450c-8c49-a1f127f5e71d </td>
</tr>
<tr>
<td>产品 ID (Windows 8.x)</td>
<td>ms-windows-store://review/?AppId=f022389f-f3a6-417e-ad23-704fbdf57117 </td>
</tr>
<tr>
<td>启动与文件扩展名关联的产品的搜索。 </td>
<td />
<td>ms-windows-store://assoc/?FileExt=pdf
</td>
</tr>
<tr>
<td>启动与协议关联的产品的搜索。</td>
<td />
<td>ms-windows-store://assoc/?Protocol=ms-word </td>
</tr>
<tr>
<td>启动与一个或多个标记关联的产品的搜索。 标记应用逗号分隔。
</td>
<td />
<td>
<p>ms-windows-store://assoc/?Tags=Photos_Rich_Media_Edit </p>
<p>ms-windows-store://assoc/?Tags=Photos_Rich_Media_Edit, Camera_Capture_App</p>
</td>
</tr>
<tr>
<td>
启动特定查询的搜索。 允许查询中存在空格。
</td>
<td />
<td>ms-windows-store://search/?query=OneNote </td>
</tr>
<tr>
<td>启动类别中的产品的搜索。</td>
<td />
<td>
<p>ms-windows-store://browse/?type=Apps&amp;cat=Productivity</p>
<p>ms-windows-store://browse/?type=Apps&amp;cat=Health+%26+fitness </p>
</td>
</tr>
<tr>
<td>启动指定的发布者中产品的搜索。 允许名称中存在空格。
</td>
<td />
<td>ms-windows-store://publisher/?name=Microsoft Corporation
</td>
</tr>
<tr><td>启动下载和 更新页面。</td>
<td />
<td>ms-windows-store://downloadsandupdates </td>
</tr>
<tr>
<td>启动应用商店设置 页面。</td>
<td />
<td>ms-windows-store://settings </td>
</tr>
</table>

 

 
