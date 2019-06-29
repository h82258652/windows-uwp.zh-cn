---
Description: 包页面是在其中你将上载的所有提交的应用包文件 （.appxupload、.appx、.appxbundle，和/或.xap）。
title: 上传应用包
ms.assetid: B1BB810D-3EAA-4FB5-B03C-1F01AFB2DE36
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10、 uwp、 包、 上传、 包上传
ms.localizationpriority: medium
ms.openlocfilehash: 97735a8e860f7c941cc35d77a21496696683640f
ms.sourcegitcommit: 4aef8c01ba9321401d5729a1ec6d46452ee76faf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/29/2019
ms.locfileid: "67468881"
---
# <a name="upload-app-packages"></a>上传应用包

**包**页是在其中你将上载的所有提交的应用包文件 （.msix、.msixupload、.msixbundle、.appx、.appxupload，和/或.appxbundle）。 可以上传你在此页上，在同一应用的所有包和应用商店客户下载您的应用程序时，将自动提供与最适合于其设备的包的每个客户。 上传程序包后，将看到一个表格，指示将以排名顺序[向特定 Windows 10 设备系列（如果适用，也包含早期 OS 版本）提供哪些程序包](#device-family-availability)。

> [!IMPORTANT]
> 自 2018 年 10 月 31 日起，新创建的产品不能包括面向 Windows 8.x/Windows 包 Phone 8.x 或更早版本。 有关详细信息，请参阅此[博客文章](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store)。

有关程序包所包含的内容以及应如何构建程序包的详细信息，请参阅[应用包要求](app-package-requirements.md)。 您还需要了解[如何版本号的影响的程序包传递到特定客户](package-version-numbering.md)并[如何管理包针对各种方案](guidance-for-app-package-management.md)。


## <a name="uploading-packages-to-your-submission"></a>将程序包上载到你的提交

若要上载程序包，请将其拖动到上载字段中或单击以浏览文件。 **包**页面将允许上传.msix、.msixupload、.msixbundle、.appx、.appxupload，和/或.appxbundle 文件。

> [!IMPORTANT]
> 对于 Windows 10 中，我们建议上传此处.msixupload 或.appxupload 文件而不是.msix、.appx、.msixbundle 或.appxbundle。  有关如何包装 UWP 应用以上架应用商店的详细信息，请参阅[使用 Visual Studio 打包 UWP 应用](../packaging/packaging-uwp-apps.md)。

如果你已为应用创建了任何[软件包外部测试版](package-flights.md)，你将看到一个下拉列表，带有从其中一个软件包外部测试版中复制程序包的选项。 选择具有你想要引入的程序包的软件包外部测试版。 然后，你可以选择要包含在此提交中的任何或所有程序包。

如果验证它时检测到错误的包，我们将显示一条消息，告知你为什么会这样。 你将需要删除包，解决此问题，然后尝试再次上传。 你还可能会看到一条警告，告知你可能导致错误的问题，但不会阻止你继续提交。


## <a name="device-family-availability"></a>设备系列可用性

在已成功上传程序包后，“设备系列可用性”  部分将显示一个表格，指示将以排名顺序向特定 Windows 10 设备系列（如果适用，也包含早期 OS 版本）提供哪些程序包。 本部分还允许选择是否向特定 Windows 10 设备系列上的客户提供提交。

有关详细信息，请参阅[设备系列可用性](device-family-availability.md)。


## <a name="package-details"></a>程序包详细信息

按目标操作系统进行分组，列出已上传的包。 将显示程序包的名称、版本和体系结构。 有关详细信息（例如每个程序包的支持语言、应用功能和文件大小），请单击“显示详细信息”  。

如果你需要将某个程序包从提交中删除，请单击每个程序包的“详细信息”  部分底部的“删除”  链接。


## <a name="removing-redundant-packages"></a>删除冗余程序包

如果我们检测到你的一个或多个冗余程序包，我们将显示一条警告，建议你从此提交中删除这些冗余程序包。 如果你之前已上载程序包，而现在又提供了支持同一组客户的更高版本的程序包，则往往会出现此情况。 在此情况下，再也不会有客户获得冗余程序包，因为你现在有一个更好（更高版本）的程序包来为这些客户提供支持。

当我们检测到你有冗余程序包时，我们将提供用于自动从此提交中删除所有冗余程序包的选项。 如果你愿意，还可以从此提交中单独删除程序包。


## <a name="gradual-package-rollout"></a>逐步部署程序包

如果提交是以前发布的应用的更新，你将看到一个复选框，指示“在此提交发布后，逐渐（仅向 Windows 10 客户）推出更新”  。 这样一来，你就可以选择将从提交获取程序包的客户比例，以便你可以监视反馈和分析数据，从而确保在更广泛地推出更新前对此更新无虑。 你可以随时增加比例（或停止更新），而无需创建新的提交。 

有关详细信息，请参阅[逐步推出程序包](gradual-package-rollout.md)。


## <a name="mandatory-update"></a>强制更新

如果提交是以前发布的应用的更新，你将看到一个复选框，指示“强制此更新”  。 这允许你为强制更新设置日期和时间，假定你已使用 Windows.Services.Store API，以允许应用采用编程方式检查程序包更新并下载和安装更新的程序包。 要使用此选项，应用必须面向 Windows 10 版本 1607 或更高版本。

有关详细信息，请参阅[为你的应用下载并安装程序包更新](../packaging/self-install-package-updates.md)。

 




