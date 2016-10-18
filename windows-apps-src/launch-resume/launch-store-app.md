---
author: TylerMSFT
title: "启动 Windows 应用商店应用"
description: "本主题介绍了 ms-windows-store URI 方案。 你的应用可以使用此 URI 方案将 Windows 应用商店应用启动到应用商店中的特定页面。"
ms.assetid: 9A9C6576-1637-47D1-AC3B-D1A20D49E0FF
translationtype: Human Translation
ms.sourcegitcommit: 39a012976ee877d8834b63def04e39d847036132
ms.openlocfilehash: b66ae37adec1b68653c0fe7d552a84f61d57acd9

---

# 启动 Windows 应用商店应用


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

本主题介绍了 **ms-windows-store:** URI 方案。 你的应用可以使用此 URI 方案将 Windows 应用商店应用启动到应用商店中的特定页面。

## ms-windows-store: URI 方案引用

<table>
<tr><th>说明</th><th></th><th>URI 方案</th></tr>
<tr><td>启动应用商店的主页。</td><td /><td>ms-windows-store://home</td></tr>
<tr><td>启动应用商店中的顶级垂直类别内容。<p>请注意：并非所有用户都可以访问所有垂直类别内容。</p>
</td><td /><td>
<p>ms-windows-store://navigatetopage/?Id=Apps </p>
<p>ms-windows-store://navigatetopage/?Id=Games</p>
<p>ms-windows-store://navigatetopage/?Id=Music</p>
<p>ms-windows-store://navigatetopage/?Id=Video</p>
<p>ms-windows-store://navigatetopage/?Id=LOB</p>
</td>
</tr>
<tr>
<td rowspan="4">启动产品的产品详细信息页面 (PDP)。 <p>应用商店 ID 已向所有使用 Windows 10 的客户推荐，并且将适用于所有操作系统版本，但执行此操作的早期方法（例如：PFN）仍受支持。</p>
<p>可以在每个应用的应用管理部分中“应用标识”<a href="https://msdn.microsoft.com/library/windows/apps/mt148561.aspx"></a>页面上的 Windows 开发人员中心仪表板中找到这些值。</p>
</td>
<td>
应用商店 ID <p>（建议）</p>
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
<td>ms-windows-store://pdp/?PhoneAppId=ca05b3ab-f157-450c-8c49-a1f127f5e71d </td>
</tr>
<tr>
<td>产品 ID (Windows 8.x)</td>
<td>ms-windows-store://pdp/?AppId=f022389f-f3a6-417e-ad23-704fbdf57117
</td>
</tr>
<tr>
<td rowspan="4">启动产品的写评论体验。</td>
<td>应用商店 ID <p>（建议）</p></td>
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

 

 



<!--HONumber=Aug16_HO3-->


