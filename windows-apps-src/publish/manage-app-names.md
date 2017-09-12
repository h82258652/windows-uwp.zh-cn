---
author: jnHs
Description: "查看为应用保留的名称、保留其他名称（用于其他语言或者更改应用名），并删除不再需要的已保留名称。"
title: "管理应用名称"
ms.assetid: D95A6227-746E-4729-AE55-648A7102401C
ms.author: wdg-dev-content
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 539ccc671ae3f66c6c17a077ac1c09d989d86fdc
ms.sourcegitcommit: d2ec178103f49b198da2ee486f1681e38dcc8e7b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/28/2017
---
# <a name="manage-app-names"></a>管理应用名称


查看为应用保留的所有名称、保留其他名称（用于其他语言或者更改应用名），并删除不需要的名称。 若要执行此操作，请转到 Windows 开发人员中心仪表板中任意应用的“应用管理”****部分中的“管理应用名称”****页。

## <a name="reserve-additional-names-for-your-app"></a>为你的应用保留其他名称

可以保留多个应用名称以用于同一应用。 如果你以多种语言提供应用，而且要为不同的语言使用不同的名称，则该选项特别有用。 还可以保留新名称以更改应用名称。

在“管理应用名称”****页面的“保留更多名称”****部分中，你将会看到文本框。 输入想要保留的名称，然后单击“检查可用性”****。 如果该名称可用，请单击**保留产品名称**。

> [!NOTE]
> 有关保留应用名称的详细信息，以及为何某个名称可能不可用，请参阅[通过保留名称创建你的应用](create-your-app-by-reserving-a-name.md)。

如果需要，你可以继续在此处保留其他应用名称。

## <a name="delete-app-names"></a>删除应用名称

如果你不再希望使用以前保留的名称，则可以通过在此处删除来释放它。 在执行此操作之前，请确保你确定要执行此操作，因为这意味着该名称将立即变为可供别人保留和使用的名称。

若要删除应用的保留名称之一，请找到你不再希望使用的名称，然后单击“删除”****。 在确认对话框中，再次单击“删除”****以确认。

请注意，你的应用需要至少具有一个保留名称。 若要从仪表板完全删除应用（释放已为该应用保留的所有名称），可以从**应用概述**页面单击**删除此应用**。 如果该应用有正在进行的提交，需要先删除该提交。 如果已将应用发布到应用商店，则无法从仪表板删除应用（但可以使用**概述**页面上的**显示/隐藏产品**功能来隐藏该应用）。 

## <a name="rename-an-app-that-has-already-been-published"></a>重命名已发布的应用

如果应用已经在应用商店中，并且希望重命名该应用，则可以为它保留一个新名称（按照上述步骤操作），然后为该应用创建新提交即可。

必须更新应用的程序包来包含新名称，才能使应用商店在新名称下显示该应用。 首先，更新 Package.StoreAssociation.xml 文件以使用新名称，可以手动更新，或使用 Visual Studio（**项目 > 应用商店 > 将应用与应用商店关联...**）进行更新。 有关详细信息，请参阅[打包 UWP 应用](../packaging/packaging-uwp-apps.md)。

还需要更新应用部件清单中的 [**Package/Properties/DisplayName**](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-1-displayname) 元素，以及更新包括应用名称的任何图形或文本。 

> [!IMPORTANT]
> 更改应用部件清单中的 **Package/Properties/DisplayName** 之前，请确保更新 Package.StoreAssociation.xml 文件，否则会出现错误。

如果该元素中出现应用的应用商店一览，则还要查看一览并更改名称。 使用新名称发布应用后，可以删除不再需要使用的旧名称。

 

 




