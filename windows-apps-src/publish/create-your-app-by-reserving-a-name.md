---
Description: The first step in creating a new app in Partner Center is reserving an app name. 了解如何保留应用名称，并查找有关为应用选择出色名称的的建议。
title: 通过保留名称创建应用
keywords: Windows 10, uwp, 预留名称, 应用名称, 应用名, 名称, 产品名称, 命名, 保留名称, 标题, 名, 题目
ms.assetid: 6DC58A9A-DF47-4652-8D13-0AC9289F5950
ms.date: 10/31/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 79831e1e14ddf8c525f1697074e6a435513f5ea1
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259025"
---
# <a name="create-your-app-by-reserving-a-name"></a>通过保留名称创建应用

The first step in creating a new app in [Partner Center](https://partner.microsoft.com/dashboard) is reserving an app name. 每个保留名称（有时被称为应用的*标题*）在整个 Microsoft Store 中必须是唯一的。

即使你尚未开始构建应用，也可以为你的应用保留名称。 We recommend doing so as soon as possible, so that nobody else can use the name. 请注意，你将需要在三个月内提交应用，以便为你保留该名称。

在[上载应用程序包](upload-app-packages.md)时，[**Package/Properties/DisplayName**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-displayname) 值必须与为应用所保留的名称相匹配。 如果使用 Microsoft Visual Studio 创建应用程序包，则将为你填充此特性。

> [!IMPORTANT]
> You can reserve additional names for an app, and you may choose to use one of those in the published version of your app instead of the one you reserve when you first create your app in Partner Center. However, be aware that the first name you enter here will be used in some of your app's [identity details](view-app-identity-details.md), such as the **Package Family Name (PFN)** . These values may be visible to some users, and cannot be changed, so make sure that the name you reserve is appropriate for this use.


## <a name="create-your-app-by-reserving-a-new-name"></a>通过保留新名称创建应用

Reserving a name is the first step in creating an app in Partner Center. 

1.  在**概述**页面上，单击**创建新应用**。
2.  在文本框中，输入要使用的名称，然后选择**检查可用性**。 如果该名称可用，你将看到绿色复选标记。 （如果你输入的名称已经由另一个开发人员保留或使用，你将看到一条指示该名称不可用的消息。）
3.  单击**保留产品名称**。

现在已为你保留该名称，只要你准备就绪，就可以开始进行[提交](app-submissions.md)。 

> [!NOTE]
> 可能会发现自己无法保留某个名称，即使在 Microsoft Store 中并没有看到任何以该名称命名的应用也是如此。 这通常是因为其他开发人员已为其应用保留该名称，但尚未提交该应用。 如果你拥有某个名称的商标权或其他法律权利，但却无法保留该名称，或发现 Microsoft Store 中的其他应用在使用该名称，请[联系 Microsoft](https://www.microsoft.com/info/cpyrtInfrg.html)。

在保留某个名称后，你有三个月的时间来提交应用。 如果在三个月内未提交该应用，保留的名称将过期，其他开发人员将可以使用该名称命名应用。 如果你尝试采用已任其过期的名称提交应用，则可能会遇到错误。


## <a name="choosing-your-apps-name"></a>选择应用名称

为你的应用选择适当的名称非常重要。 你应该选取一个能够引起客户兴趣的名称，从而吸引他们进一步了解你的应用。 下面是有关选择良好应用名称的几点提示。

-   **Keep it short.** 在很多情况下，用来显示应用名称的空间非常有限，因此我们建议尽可能使用最短的名称。 虽然你的应用的名称最多可包含 256 个字符，但客户可能无法始终看到非常长的名称的末尾。
    > [!NOTE]
    > 在各种位置显示的实际字符数目可能有所不同，具体取决于分配的长度和你的应用名称所使用的字符类型。 例如，在 Windows 使用的 Segoe UI 字体中，10 个“W”字符占用的空间大约相当于 30 个“I”字符。 Because of this variation, be sure to test your app and verify how its name appears on its tiles (if you choose to overlay the app name), in search results, and within the app itself. 另外，请考虑应用要采用的每种语言。 请记住，东亚字符往往比拉丁字符宽，因此将显示的字符会比拉丁字符少。
-   **Be original.** 确保使用与众不同的应用名称，这样不易与现有应用产生混淆。
-   **Don't use names trademarked by others.** 确保你有权使用你保留的名称。 如果其他人将该名称注册为商标，他们有权投诉侵权，你将无法继续使用该名称。 如果在你的应用发布后发生这种情况，我们会将其从应用商店中删除。 你将需要更改你的应用名称以及出现在应用及其内容中该名称的所有实例，然后才能再次[提交你的应用](app-submissions.md)进行认证。
-   **Avoid adding differentiating info at the end of the name.** 如果将用于区分多个应用的信息添加到名称末尾，则客户可能会忽略此信息，尤其是在名称很长的情况下；这样所有应用看上去似乎都采用了相同名称。 If this is unavoidable, use different logos and app images to make it easier to differentiate one app from another.
-   **Don't include emojis in your name.** 你不能保留包含表情符号或其他不受支持的字符的名称。


## <a name="manage-additional-app-names"></a>管理其他应用名称

You can add and manage additional names on the **Manage app names** page in the **App management** section for each of your apps in Partner Center.

在某些情况下，可能希望保留用于同一应用的多个名称，例如要提供应用的多个语言版本并希望每个语言版本均使用不同的名称。 如果你要完全更改应用的名称，你将需要保留额外的名称。

在此页面上，你还可以删除你已保留但不想再使用的任何名称。

有关详细信息，请参阅[管理应用名称](manage-app-names.md)。

 

 




