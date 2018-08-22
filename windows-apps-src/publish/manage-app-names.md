---
author: jnHs
Description: View the names that you've reserved for your app, reserve additional names (for other languages or to change your app's name), and delete reserved names that you don't need anymore.
title: 管理应用名称
ms.assetid: D95A6227-746E-4729-AE55-648A7102401C
ms.author: wdg-dev-content
ms.date: 8/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10，uwp，应用程序名称更改应用程序名称、 更新应用程序名称、 游戏名称、 产品名称
ms.localizationpriority: medium
ms.openlocfilehash: f0d2c6f72e2f69f0b768af55f9bddeb9bb008027
ms.sourcegitcommit: f2f4820dd2026f1b47a2b1bf2bc89d7220a79c1a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/22/2018
ms.locfileid: "2794538"
---
# <a name="manage-app-names"></a>管理应用名称

使用您查看所有已供您的应用程序的名称，**管理应用程序名称**可以保留其他名称 （针对其他语言或更改您的应用程序的名称），并删除不需要在您的名称。 通过展开任何您的应用程序的左侧的导航菜单中的**应用程序管理**部分，您可以在[Windows 开发人员中心仪表板](https://partner.microsoft.com/dashboard)中找到此页。


## <a name="reserve-additional-names-for-your-app"></a>为你的应用保留其他名称

可以保留多个应用名称以用于同一应用。 如果你以多种语言提供应用，而且要为不同的语言使用不同的名称，则该选项特别有用。 如下所述，还可以更改应用程序的名称以预留的新名称。

保留新的应用程序名称，请在**管理应用程序名称**页的**保留多个名称**部分查找文本框。 输入想要保留的名称，然后单击“检查可用性”****。 如果该名称可用，请单击**保留产品名称**。 如果需要，您可以通过重复这些步骤中，保留多个应用程序名称。

> [!NOTE]
> 有关保留应用名称的详细信息，以及为何某个名称可能不可用，请参阅[通过保留名称创建你的应用](create-your-app-by-reserving-a-name.md)。


## <a name="delete-app-names"></a>删除应用名称

如果你不再希望使用以前保留的名称，则可以通过在此处删除来释放它。 在执行此操作之前，请确保你确定要执行此操作，因为这意味着该名称将立即变为可供别人保留和使用的名称。

若要删除应用的保留名称之一，请找到你不再希望使用的名称，然后单击“删除”****。 在确认对话框中，再次单击“删除”**** 以确认。

请注意，您的应用程序必须至少一个保留的名称。 要完全删除您的仪表板，应用程序 （并发行版已保留应用程序的所有名称），单击从**应用程序概述**页上的**删除此应用程序**。 如果该应用有正在进行的提交，需要先删除该提交。 请注意，是否您已发布到商店的应用程序，您不能删除它从您的仪表板 （虽然可以使用**概述**页面上的**显示/隐藏产品**功能以隐藏它）。 


## <a name="rename-an-app-that-has-already-been-published"></a>重命名已发布的应用

如果应用已经在应用商店中，并且希望重命名该应用，则可以为它保留一个新名称（按照上述步骤操作），然后为该应用创建新提交即可。 

必须更新您的应用程序程序包的旧名称替换为新数据库并将更新的程序包上载到您提交。
- 首先，更新 Package.StoreAssociation.xml 文件以使用新名称，手动方式或通过使用 Visual Studio (**Project > 存储 > 关联的应用程序与存储...**)。有关详细信息，请参阅[包使用 Visual studio 创建的 UWP 应用程序](../packaging/packaging-uwp-apps.md)。
- 还需要更新应用部件清单中的 [**Package/Properties/DisplayName**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-displayname) 元素，以及更新包括应用名称的任何图形或文本。 
  > [!IMPORTANT]
  > 更改应用部件清单中的 **Package/Properties/DisplayName** 之前，请确保更新 Package.StoreAssociation.xml 文件，否则会出现错误。

若要更新存储列表，以便它使用的新名称，请转到[存储列表页上](create-app-store-listings.md)，为该语言和从**产品名称**下拉列表中选择的名称。 请务必查看您的描述和名称的任何提及的列表的其他部分并进行更新，如果需要。

> [!NOTE]
> 如果您的应用程序具有多个语言包和/或商店列表，您需要更新程序包和/或存储为每种语言名称需要更新的列表。

一旦您的应用程序已发布使用新名称，就可以删除不再需要使用任何旧名称。

> [!TIP]
> 使用名为其保留的仪表板中将显示每个应用程序。 如果您已按照上述步骤以重命名应用程序，并且您希望其出现在您使用的新名称的仪表板中，您必须 （通过**管理应用程序名称**页上，单击**删除**） 中删除的原始名称。 

 

 




