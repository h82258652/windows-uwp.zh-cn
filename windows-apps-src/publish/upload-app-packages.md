---
author: jnHs
Description: "你可以在“程序包”页上传所要提交应用的所有程序包文件（.xap、.appx、.appxupload 和/或 .appxbundle）。 你可以在此步骤中上传适用于应用所面向的所有操作系统的程序包。"
title: "上传应用包"
ms.assetid: B1BB810D-3EAA-4FB5-B03C-1F01AFB2DE36
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: bb89968c261cc0c14f82375d0d708bbce5da9869

---

# 上传应用包


你可以在“程序包”****页上载所要提交应用的所有程序包文件（.xap、.appx、.appxupload 和/或 .appxbundle）。 你可以在此步骤中上传适用于应用所面向的所有操作系统的程序包。 当客户下载你的应用时，应用商店将浏览该应用的所有可用程序包，然后将自动向每个客户提供最适用于其设备的程序包。

有关程序包所包含的内容以及应如何构建程序包的详细信息，请参阅[应用包要求](app-package-requirements.md)。 你还需要了解[版本号如何影响交付给特定客户的程序包](package-version-numbering.md)以及[如何将程序包分配给不同的操作系统](guidance-for-app-package-management.md)。

## 将程序包上载到你的提交


若要上载程序包，请将其拖动到上载字段中或单击以浏览文件。 通过**“程序包”**页，你可以上传 .xap、.appx、.appxupload 和/或 .appxbundle 文件。

如果你已为应用创建了任何[软件包外部测试版](package-flights.md)，你将看到一个下拉列表，带有从其中一个软件包外部测试版中复制程序包的选项。 选择具有你想要引入的程序包的软件包外部测试版。 然后，你可以选择要包含在此提交中的任何或所有程序包。

> **重要提示** 对于 Windows 10，你应始终上载此处的 .appxupload 文件，而不是 .appx 或 .appxbundle。 有关打包适用于应用商店的 UWP 应用的详细信息，请参阅[打包适用于 Windows 10 的通用 Windows 应用](../packaging/packaging-uwp-apps.md)。

如果我们在验证程序包时检测到程序包问题，你将需要删除该程序包、修复该问题，然后尝试重新上载它。 有关详细信息，请参阅[解决程序包上载错误](resolve-package-upload-errors.md)。

你还可能会看到一条警告，告知你可能导致错误的问题，但不会阻止你继续提交。

## 程序包详细信息


当你成功上载程序包后，我们将按目标操作系统将其分组列出。 将显示程序包的名称、版本和体系结构。 有关详细信息（例如每个程序包的支持语言、应用功能和文件大小），请单击“详细信息”****。

如果你使用的是 [Windows 广告中介](../monetize/use-ad-mediation-to-maximize-revenue.md)，你还将看到一个指向为每个程序包配置广告中介的链接。

如果你需要将某个程序包从提交中删除，请单击每个程序包的“详细信息”****部分底部的“删除”****链接。

## 删除冗余程序包


如果我们检测到你的一个或多个冗余程序包，我们将显示一条警告，建议你从此提交中删除这些冗余程序包。 如果你之前已上载程序包，而现在又提供了支持同一组客户的更高版本的程序包，则往往会出现此情况。 在此情况下，再也不会有客户获得冗余程序包，因为你现在有一个更好（更高版本）的程序包来为这些客户提供支持。

当我们检测到你有冗余程序包时，我们将提供用于自动从此提交中删除所有冗余程序包的选项。 如果你愿意的话，你还可以从此提交中单独删除程序包。

## 具有 Visual Studio Application Insights 的程序包


我们建议你在程序包中使用 [Visual Studio Application Insights](http://go.microsoft.com/fwlink/?LinkId=615086)（或在生成程序包时通过选中“在 Windows 开发人员中心中显示遥测”框来启用它），以便我们可以向你提供[应用使用情况遥测详细信息](usage-report.md)。 如果你未在 Microsoft Visual Studio 中配置 Application Insights，当我们检测到某个程序包中包含它时，我们将显示一条消息以确认以下信息：提交你的程序包，即表示你同意启用有关你的开发者帐户的应用使用情况遥测。 你可以在“帐户设置”****中随时禁用应用使用情况遥测。

 

 







<!--HONumber=Jun16_HO4-->


