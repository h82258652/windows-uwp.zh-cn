---
author: jnHs
Description: "你可以在“程序包”页上传所要提交应用的所有程序包文件（.xap、.appx、.appxupload 和/或 .appxbundle）。 你可以在此步骤中上传适用于应用所面向的所有操作系统的程序包。"
title: "上传应用包"
ms.assetid: B1BB810D-3EAA-4FB5-B03C-1F01AFB2DE36
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 1bc2ce82688db20315113efc9b080b449a850f05
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="upload-app-packages"></a>上传应用包


你可以在**程序包**页上载要提交的应用的所有程序包文件（.appx、.appxupload、.appxbundle 和/或 .xap）。 你可以在此步骤中上传适用于应用所面向的所有操作系统的程序包。 当客户下载应用时，应用商店将自动向每个客户提供最适用于其设备的程序包。 上传程序包后，将看到一个表格，指示将以排名顺序[向特定 Windows 10 设备系列（如果适用，也包含早期 OS 版本）提供哪些程序包](#device-family-availability)。

有关程序包所包含的内容以及应如何构建程序包的详细信息，请参阅[应用包要求](app-package-requirements.md)。 你还需要了解[版本号如何影响交付给特定客户的程序包](package-version-numbering.md)以及[如何将程序包分配给不同的操作系统](guidance-for-app-package-management.md)。

## <a name="uploading-packages-to-your-submission"></a>将程序包上载到你的提交

若要上载程序包，请将其拖动到上载字段中或单击以浏览文件。 通过**“程序包”**页，你可以上传 .xap、.appx、.appxupload 和/或 .appxbundle 文件。

如果你已为应用创建了任何[软件包外部测试版](package-flights.md)，你将看到一个下拉列表，带有从其中一个软件包外部测试版中复制程序包的选项。 选择具有你想要引入的程序包的软件包外部测试版。 然后，你可以选择要包含在此提交中的任何或所有程序包。

> **重要提示**  对于 Windows 10，你应始终上传此处的 .appxupload 文件，而不是 .appx 或 .appxbundle。 有关打包适用于应用商店的 UWP 应用的详细信息，请参阅[打包适用于 Windows 10 的通用 Windows 应用](../packaging/packaging-uwp-apps.md)。

如果我们在验证程序包时检测到程序包问题，你将需要删除该程序包、修复该问题，然后尝试重新上载它。 有关详细信息，请参阅[解决程序包上载错误](resolve-package-upload-errors.md)。

你还可能会看到一条警告，告知你可能导致错误的问题，但不会阻止你继续提交。

## <a name="device-family-availability"></a>设备系列可用性

在已成功上传程序包后，“设备系列可用性”****部分将显示一个表格，指示将以排名顺序向特定 Windows 10 设备系列（如果适用，也包含早期 OS 版本）提供哪些程序包。 本部分还允许你选择是否向特定 Windows 10 设备系列上的客户提供提交。

> **注意** 如果尚未上载程序包，“设备系列可用性”****部分将显示带有复选框的 Windows 10 设备系列，以指示是否向这些设备系列上的客户提供提交。 表格将在上传程序包后显示。

你还会看到一个复选框，用于指示是否要允许 Microsoft 将应用提供给未来的 Windows 10 设备系列。 我们建议将此复选框保持选中状态，以便你的应用可随着新设备系列的引入而提供给更多潜在客户。

### <a name="choosing-which-device-families-to-support"></a>选择要支持哪些设备系列

你可以取消选中任何 Windows 10 设备系列的复选框（如果你不希望向该设备类型上的客户提供提交）。 如果未选中某个设备系列的复选框，该设备类型上的新客户将无法获取应用（尽管已拥有该应用的客户仍然可以使用它，并将获取提交的任何更新）。 

> **注意** 对于 **Windows 8/8.1** 和 **Windows Phone 8.x 及早期版本**，不存在任何复选框。 如果提交中包含的程序包可以在这些 OS 版本上运行，这些程序包将提供给客户。 若要停止向早期 OS 版本上的客户提供应用，将需要从提交中删除相应的程序包。

如果你的应用支持移动设备系列和桌面设备系列，建议你将“Windows 10 移动版”****和“Windows 10 桌面版”****复选框保持选中状态，除非你有特殊原因需要限制可以获取你的应用的 Windows 10 设备类型。 例如，你可能已创建 Windows 通用程序包，但你知道你仍需要在移动设备上测试该应用是否存在某些问题。 若要防止新客户在Windows 10 移动设备上下载该应用，你可以取消选中此处的“Windows 10 移动版”****复选框。 如果你之后确定可随时将该应用提供给 Windows 10 移动设备上的客户，可通过选中“Windows 10 移动版”****复选框创建新提交。

如果你的应用并非游戏（或者它是游戏并且已通过[概念审批](../gaming/concept-approval.md)过程），而你的提交包含使用 Windows 10 SDK 版本 14393 或更高版本编译的非特定程序包和/或 x64 UWP 程序包，则可以选中“Windows 10 Xbox”****复选框，来将该应用提供给 Xbox 上的客户。 

> **重要提示** 为了使应用能够在 Xbox 设备上启动，你必须包含使用 Windows SDK 版本 14393 或更高版本编译的非特定程序包或 x64 程序包。 但是，如果选中了 Windows 10 Xbox，则适用于 Xbox 的版本最高的程序包（即，面向 Xbox 或通用设备系列的非特定程序包或 x64 程序包）将始终提供给 Xbox 上的客户，即使它使用早期 SDK 版本进行编译也是如此。 因此，确保使用 Windows SDK 版本 14393 或更高版本对适用于 Xbox 的版本最高的程序包进行编译至关重要。 如果不是这样，将显示一条错误消息，指示系统 Xbox 客户将无法启动应用。 
> 
> 若要解决此错误，可以执行以下操作之一：
> -    将适用的程序包替换为使用 Windows SDK 版本 14393 或更高版本编译的新程序包。
> -    如果已有的程序包支持 Xbox 并使用 Windows SDK 版本 14393 或更高版本进行了编译，则增加它的版本号，以便它提交中版本最高的程序包。
> -    取消选中“Windows 10 Xbox”****复选框。
>     
> 如果仍然无法解决该问题，请联系支持人员。

如果已测试你的应用来确保应用在 Microsoft HoloLens 上正确运行，还可以选中“Windows 10 全息版”****复选框以向 HoloLens 客户提供该应用。 有关生成、测试以及发布全息应用的详细信息，请参阅 [Windows 全息版开发概述](http://dev.windows.com/holographic/development_overview)。

> **重要提示** 若要完全阻止特定 Windows 10 设备系列获取你的应用，你需要将 appx 清单中的 [**TargetDeviceFamily**](https://msdn.microsoft.com/library/windows/apps/dn986903) 元素更新为仅面向你想要支持的设备系列（即，Windows.Mobile 或 Windows.Desktop），而不是（针对通用设备系列）将其保留为 Windows.Universal 值，Microsoft Visual Studio 默认将该值包含在 appx 清单中。

了解下面一点很重要：你在此处所做的选择仅应用于全新购买。 已拥有你的应用的任何用户都可以继续使用它，并且将获得你提交的任何更新，即使你在此处删除了该设备系列也是如此。 这甚至也适用于在升级到 Windows 10 之前获取你的应用的客户。 例如，如果你先发布了一个带有 Windows Phone 8.1 程序包的应用，而在以后又将 Windows 10 (UWP) 程序包添加到面向通用设备系列的同一应用，将会向已拥有你的 Windows Phone 8.1 程序包的 Windows 10 移动客户提供此 Windows 10 (UWP) 程序包的更新，即使你已取消选中“Windows 10 移动版”****复选框也是如此（因为这并不是全新购买，而是一次更新）。 但是，如果你没有提供面向通用或移动设备系列的任何 Windows 10 (UWP) 程序包，则你的 Windows 10 移动客户将继续使用 Windows Phone 8.1 程序包。

有关设备系列的详细信息，请参阅[通用 Windows 平台 (UWP) 应用指南](https://msdn.microsoft.com/library/windows/apps/dn894631)和 [**TargetDeviceFamily**](https://msdn.microsoft.com/library/windows/apps/dn986903)。

### <a name="understanding-ranking"></a>了解分级

除了指示哪些 Windows 10 设备系列可以下载提交外，本部分还介绍了特定程序包中的哪一个将提供给特定设备系列。 如果你有多个程序包可以在某个设备系列上运行，该表格将基于程序包的版本号指示程序包的提供顺序。 有关应用商店如何基于版本号对程序包分级的详细信息，请参阅[程序包版本编号](package-version-numbering.md)。 

例如，假设你有两个程序包：Package_A.appxupload 和 Package_B.appxupload。 对于给定的设备系列，如果 Package_A.appxupload 排名第 1 而 Package_B.appxupload 排名第 2，这意味着当该设备类型上的客户获取应用时，应用商店将先尝试提供 Package_A.appxupload。 如果客户设备无法运行 Package_A.appxupload，应用商店将提供 Package_B.appxupload。 如果客户设备无法运行该设备系列的任何程序包（例如，当应用支持的**最低版本**高于客户设备上的版本时），客户将无法将应用下载到该设备。

> **注意** 当确定向给定客户提供哪个程序包时，不考虑 .xap 程序包中的版本号。 因此，如果你有多个相同等级的 .xap 程序包，你将看到一个星号而非数字，客户可能会收到任一程序包。 若要将客户从一个 .xap 程序包更新到较新的程序包，请确保在新提交中删除旧的 .xap 程序包。



## <a name="package-details"></a>程序包详细信息

当你成功上载程序包后，我们将按目标操作系统将其分组列出。 将显示程序包的名称、版本和体系结构。 有关详细信息（例如每个程序包的支持语言、应用功能和文件大小），请单击“显示详细信息”****。

如果你使用的是 [Windows 广告中介](../monetize/use-ad-mediation-to-maximize-revenue.md)，你还将看到一个指向为每个程序包配置广告中介的链接。

如果你需要将某个程序包从提交中删除，请单击每个程序包的“详细信息”****部分底部的“删除”****链接。

## <a name="removing-redundant-packages"></a>删除冗余程序包

如果我们检测到你的一个或多个冗余程序包，我们将显示一条警告，建议你从此提交中删除这些冗余程序包。 如果你之前已上载程序包，而现在又提供了支持同一组客户的更高版本的程序包，则往往会出现此情况。 在此情况下，再也不会有客户获得冗余程序包，因为你现在有一个更好（更高版本）的程序包来为这些客户提供支持。

当我们检测到你有冗余程序包时，我们将提供用于自动从此提交中删除所有冗余程序包的选项。 如果你愿意，还可以从此提交中单独删除程序包。

## <a name="gradual-package-rollout"></a>逐步部署程序包

如果提交是以前发布的应用的更新，你将看到一个复选框，指示“在此提交发布后，逐渐（仅向 Windows 10 客户）推出更新”****。 这样一来，你就可以选择将从提交获取程序包的客户比例，以便你可以监视反馈和分析数据，从而确保在更广泛地推出更新前对此更新无虑。 你可以随时增加比例（或停止更新），而无需创建新的提交。 

有关详细信息，请参阅[逐步推出程序包](gradual-package-rollout.md)。

## <a name="mandatory-update"></a>强制更新

如果提交是以前发布的应用的更新，你将看到一个复选框，指示“强制此更新”****。 这允许你为强制更新设置日期和时间，假定你已使用 Windows.Services.Store API，以允许应用采用编程方式检查程序包更新并下载和安装更新的程序包。 要使用此选项，应用必须面向 Windows 10 版本 1607 或更高版本。

有关详细信息，请参阅[为你的应用下载并安装程序包更新](../packaging/self-install-package-updates.md)。

 




