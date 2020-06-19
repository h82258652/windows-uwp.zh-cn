---
Description: 如果你已经提供面向不同操作系统的程序包，你可以选择为不同的目标操作系统自定义应用商店一览的各个部分。
title: 创建特定于平台的 Store 一览
ms.assetid: 5BE66BE2-669C-49E0-8915-60F1027EF94A
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, uwp, 自定义, 一览, 描述, 之前
ms.localizationpriority: medium
ms.openlocfilehash: 475526662737eaa5b95267e36016e342c6652a89
ms.sourcegitcommit: 96b7be654a0922eeb421b5fa51ebfc586abe74fe
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/17/2020
ms.locfileid: "84945953"
---
# <a name="create-platform-specific-store-listings"></a>创建特定于平台的 Store 一览


如果你以前发布的应用具有面向不同操作系统的包，你可以选择在早期版本的操作系统（Windows 2.x 或更早版本和/或 Windows Phone 3.x 或更早版本）中为客户自定义你的部分商店列表。 

Windows 10 上的客户（包括 Xbox）将始终显示默认[商店列表](create-app-store-listings.md)。 除非已使用支持一个或多个早期版本操作系统的包发布了应用程序，否则不会看到用于创建平台特定的存储列表的选项。 

> [!IMPORTANT]
> 你不能再上传使用 Windows Phone 3.x SDK 生成的新 XAP 包。 已存储在 XAP 包中的应用将在 Windows 10 移动版设备上继续运行。 有关详细信息，请参阅此[博客文章](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store)。

如果希望提及仅出现在一个操作系统版本中的功能，或者希望提供特定于特定操作系统的屏幕快照（独立于设备类型），则特定于平台的存储清单会很有用。

> [!NOTE]
> 以某种语言创建特定于平台的应用商店一览时，不会针对应用支持的其他语言创建特定于平台的应用商店一览。 你将需要分别针对每种语言创建特定于平台的应用商店一览。 另请注意，无法[导入和导出存储区列出](import-and-export-store-listings.md)特定于平台的列表数据。


## <a name="creating-a-platform-specific-store-listing"></a>创建特定于平台的应用商店一览

在**商店列表**页面的顶部附近，如果你以前发布的应用包含支持早期版本（Windows 8.x 或更早版本和/Windows Phone 或更早版本）的包，则可以选择 "**创建特定于平台的应用商店" 列表**。 选择此选项后，系统将提示你从提交支持的目标操作系统版本中进行选择。 为应用所面向的所有早期操作系统版本创建平台特定的存储清单后，将无法再进行其他选择。

你可以使用默认（Windows 10）应用商店列表作为开始点，这将为你为默认商店列表输入的合适文本和图像提供相关内容;然后，你将能够在保存之前进行任何所需的更改。 你也可以从完全空白的应用商店一览中开始操作（如果你喜欢）。

单击**继续**后，**应用商店一览**页面将包含针对刚创建的特定于平台的应用商店一览的一个部分。 此部分将包括一组其自己的以下字段：**描述**（必填）、**此版本的最近更新**、**屏幕截图**、**应用磁贴图标**、**应用功能**和**其他系统要求**。 请确保将信息输入每个你希望在自定义应用商店一览中显示信息的字段，即使它是默认应用商店一览中的相同信息。 如果你将其中任一字段留空，则自定义应用商店一览中将不会为该字段显示任何信息。

> [!IMPORTANT]
> 无法为不同的操作系统版本自定义 Microsoft Store 一览中[其他信息](create-app-store-listings.md#additional-information)部分的字段。
> 
> 此外，由于 "默认[存储" 列表](create-app-store-listings.md)页中的某些字段仅适用于 Windows 10 上的客户，因此在创建平台特定的商店列表时，你将看不到所有相同的选项。 例如，无法将预告片添加到特定于平台的应用商店一览，因为预告片仅向使用 Windows 10 版本 1607 或更高版本的客户显示。 

你可以根据需要继续编辑特定于平台的列表，以便针对特定操作系统版本的客户进行更改。


## <a name="removing-a-platform-specific-store-listing"></a>删除特定于平台的应用商店一览

如果你已创建特定于平台的应用商店一览，但稍后决定还是向使用该操作系统的客户显示默认应用商店一览，请选择该一览旁边的**删除**链接。

在确认你希望向这些客户显示默认应用商店一览之后，请选择**确定**。 特定于平台的应用商店一览将被删除（适用于其存在的所有语言）并且该操作系统版本上的客户现在可看到你的默认应用商店一览。 如果你改变主意，则可以按照上面列出的步骤为该操作系统创建另一个特定于平台的应用商店一览。
 

 




