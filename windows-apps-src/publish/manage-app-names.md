---
Description: 查看为应用保留的名称、保留其他名称（用于其他语言或者更改应用名），并删除不再需要的已保留名称。
title: 管理应用名称
ms.assetid: D95A6227-746E-4729-AE55-648A7102401C
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10、uwp、应用名称、更改应用名称、更新应用名称、游戏名称、产品名称
ms.localizationpriority: medium
ms.openlocfilehash: 38cedf40d4ecf997f6fbced2186cd5b27c6d5e4f
ms.sourcegitcommit: ca1b5c3ab905ebc6a5b597145a762e2c170a0d1c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/13/2020
ms.locfileid: "79210353"
---
# <a name="manage-app-names"></a>管理应用名称

可以通过 "**管理应用名称**" 查看为应用保留的所有名称，保留其他名称（适用于其他语言或更改应用的名称）以及删除不需要的名称。 可以通过在任何应用的左侧导航菜单中展开 "**应用程序管理**" 部分，在 "[合作伙伴中心](https://partner.microsoft.com/dashboard)" 中找到此页。

> [!IMPORTANT]
> 你可以保留应用的其他名称，也可以选择在应用的已发布版本中使用其中的一个，而不是在合作伙伴中心创建应用时所保留的名称。 但是，请注意，你为产品保留的名字将用于其中一些[标识详细信息](view-app-identity-details.md)，如**包系列名称（PFN）** 。 这些值对某些用户可能是可见的，并且不能更改，因此请确保您首先保留的名称适用于这种情况。


## <a name="reserve-additional-names-for-your-app"></a>为你的应用保留其他名称

可以保留多个应用名称以用于同一应用。 如果你以多种语言提供应用，而且要为不同的语言使用不同的名称，则该选项特别有用。 你还可以保留新名称，以便更改应用的名称，如下所述。

若要保留新应用名称，请在 "**管理应用名称**" 页的 "**保留更多名称**" 部分中找到该文本框。 输入想要保留的名称，然后单击“检查可用性”。 如果该名称可用，请单击**保留产品名称**。 如果需要，可以重复执行这些步骤，以保留多个应用名称。

> [!NOTE]
> 有关保留应用名称的详细信息，以及为何某个名称可能不可用，请参阅[通过保留名称创建你的应用](create-your-app-by-reserving-a-name.md)。


## <a name="delete-app-names"></a>删除应用名称

如果你不再希望使用以前保留的名称，则可以通过在此处删除来释放它。 在执行此操作之前，请确保你确定要执行此操作，因为这意味着该名称将立即变为可供别人保留和使用的名称。

若要删除应用的保留名称之一，请找到你不再希望使用的名称，然后单击“删除”。 在确认对话框中，再次单击“删除”以确认。

请注意，应用必须具有至少一个保留名称。 若要从合作伙伴中心完全删除应用（并发布已为该应用保留的所有名称），请单击 "**应用概述**" 页中的 "**删除此应用**"。 如果该应用有正在进行的提交，需要先删除该提交。 请注意，如果已将应用发布到应用商店，则无法从合作伙伴中心将其删除（尽管可以使用 "**概述**" 页上的 "**显示/隐藏产品**" 功能将其隐藏）。 


## <a name="rename-an-app-that-has-already-been-published"></a>重命名已发布的应用

如果应用已经在应用商店中，并且希望重命名该应用，则可以为它保留一个新名称（按照上述步骤操作），然后为该应用创建新提交即可。 

您必须更新应用程序的包以将旧名称替换为新名称，并将更新后的包上载到您的提交。
- 首先，将 Package.storeassociation.xml 文件更新为使用新名称，无论是手动还是使用 Visual Studio （**Project > Store > 将应用与应用商店关联 ...** ）。有关详细信息，请参阅[使用 Visual Studio 打包 UWP 应用](/windows/msix/package/packaging-uwp-apps)。
- 还需要更新应用部件清单中的 [**Package/Properties/DisplayName**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-displayname) 元素，以及更新包括应用名称的任何图形或文本。 
  > [!IMPORTANT]
  > 更改应用部件清单中的 **Package/Properties/DisplayName** 之前，请确保更新 Package.StoreAssociation.xml 文件，否则会出现错误。

若要更新商店列表以便其使用新名称，请在该语言的 "[商店列表" 页](create-app-store-listings.md)上，选择 "**产品名称**" 下拉列表中的 "名称"。 如果需要，请务必查看任何名称提及的说明和列表的其他部分。

> [!NOTE]
> 如果你的应用程序具有多种语言的包和/或存储列表，则需要更新该名称需要更新的每种语言的包和/或存储列表。

使用新名称发布应用后，可以删除不再需要使用的任何较旧的名称。

> [!TIP]
> 每个应用都将使用为其保留的名字显示在合作伙伴中心中。 如果已按照上述步骤重命名了某个应用，并且希望它使用新名称显示在合作伙伴中心，则必须在 "**管理应用名称**" 页上单击 "**删除**" 来删除原始名称。 

 

 




