---
Description: 如果你已经提供面向不同操作系统的程序包，你可以选择为不同的目标操作系统自定义应用商店一览的各个部分。
title: 创建特定于平台的应用商店一览
ms.assetid: 5BE66BE2-669C-49E0-8915-60F1027EF94A
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, uwp, 自定义, 一览, 描述, 之前
ms.localizationpriority: medium
ms.openlocfilehash: 2d9a9e86397ca5e85697543f99481b43f438a063
ms.sourcegitcommit: 4aef8c01ba9321401d5729a1ec6d46452ee76faf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/29/2019
ms.locfileid: "67468969"
---
# <a name="create-platform-specific-store-listings"></a>创建特定于平台的应用商店一览


如果你以前发布的应用程序的不同的操作系统包的可以选择自定义您的应用商店列表上早期的 OS 版本的客户的部分 (Windows 8.x 或更早版本和/或 Windows Phone 8.x 或更早版本)。 

Windows 10 （包括 Xbox） 上的客户将始终看到默认[应用商店列表](create-app-store-listings.md)。 不会看到创建特定于平台的应用商店列表，除非你已发布应用程序与支持一个或多个早期的 OS 版本的包的选项。 

> [!IMPORTANT]
> 自 2018 年 10 月 31 日起，新创建的产品不能包括面向 Windows 8.x/Windows 包 Phone 8.x 或更早版本。 有关详细信息，请参阅此[博客文章](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store)。

如果想提一下仅在一个操作系统版本中出现的功能或想要提供特定于特定操作系统 （独立于设备类型） 的屏幕截图，特定于平台的应用商店列表很有用。

> [!NOTE]
> 创建特定于平台的一种语言中列出的存储区不会创建特定于平台的应用商店列表中你的应用支持其他语言。 你将需要分别针对每种语言创建特定于平台的应用商店一览。 另请注意，您不能[导入和导出列出数据存储区](import-and-export-store-listings.md)用于特定于平台的清单。


## <a name="creating-a-platform-specific-store-listing"></a>创建特定于平台的应用商店一览

顶部附近的你**应用商店列表**页上，如果您以前发布的应用程序包含支持早期的 OS 版本的包 ((Windows 8.x 或更早版本和/或 Windows Phone 8.x 或更早版本)，可以选择**创建特定于平台的应用程序应用商店列表**。 选择此选项后，系统将提示你从提交支持的目标操作系统版本中进行选择。 应用的目标已创建的所有早期的 OS 版本的特定于平台的应用商店列表后, 您将无法进行其他选择。

您可以使用默认值为 (Windows 10) 的应用商店列表作为起始点，这将显示通过适用的文本和您已输入的默认存储的图像列表;你将能够使你想保存之前的任何更改。 你也可以从完全空白的应用商店一览中开始操作（如果你喜欢）。

单击**继续**后，**应用商店一览**页面将包含针对刚创建的特定于平台的应用商店一览的一个部分。 此部分将包括一组其自己的以下字段：**描述**（必填）、**此版本的最近更新**、**屏幕截图**、**应用磁贴图标**、**应用功能**和**其他系统要求**。 请确保将信息输入每个你希望在自定义应用商店一览中显示信息的字段，即使它是默认应用商店一览中的相同信息。 如果你将其中任一字段留空，则自定义应用商店一览中将不会为该字段显示任何信息。

> [!IMPORTANT]
> 无法为不同的操作系统版本自定义 Microsoft Store 一览中[其他信息](create-app-store-listings.md#additional-information)部分的字段。
> 
> 此外，由于默认 [Microsoft Store 一览](create-app-store-listings.md)页面中的一些字段仅适用于使用 Windows 10 的客户，你将看不到创建特定于平台的 Microsoft Store 一览时显示的所有相同选项。 例如，无法将预告片添加到特定于平台的应用商店一览，因为预告片仅向使用 Windows 10 版本 1607 或更高版本的客户显示。 

您可以继续编辑特定于平台的列表，根据需要为在特定 OS 版本的客户进行更改。


## <a name="removing-a-platform-specific-store-listing"></a>删除特定于平台的应用商店一览

如果你已创建特定于平台的应用商店一览，但稍后决定还是向使用该操作系统的客户显示默认应用商店一览，请选择该一览旁边的**删除**链接。

在确认你希望向这些客户显示默认应用商店一览之后，请选择**确定**。 特定于平台的应用商店一览将被删除（适用于其存在的所有语言）并且该操作系统版本上的客户现在可看到你的默认应用商店一览。 如果你改变主意，则可以按照上面列出的步骤为该操作系统创建另一个特定于平台的应用商店一览。
 

 




