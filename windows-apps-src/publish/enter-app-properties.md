---
author: jnHs
Description: The App properties page of the app submission process lets you define your app's category and indicate hardware preferences or other declarations.
title: 输入应用属性
ms.assetid: CDE4AF96-95A0-4635-9D07-A27B810CAE26
ms.author: wdg-dev-content
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, 游戏设置, 显示模式, 系统要求, 硬件要求, 最低硬件, 建议硬件, 隐私策略, 支持联系人信息, 应用网站, 支持信息
ms.localizationpriority: medium
ms.openlocfilehash: d23fb0cb3fb4668682df1957cbf0c88bcf8649c1
ms.sourcegitcommit: e4f3e1b2d08a02b9920e78e802234e5b674e7223
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/26/2018
ms.locfileid: "4213347"
---
# <a name="enter-app-properties"></a>输入应用属性

使用[应用提交过程](app-submissions.md)的**属性**页，你可以定义应用类别并输入其他信息和声明。 请务必在此页面上提供有关你的应用的完整准确的详细信息。


## <a name="category-and-subcategory"></a>类别和子类别

你必须指示 Microsoft Store 用于对应用进行归类的类别（和子类别/种类，如适用）。 提交应用需要指定类别。

有关详细信息，请参阅[类别和子类别表](category-and-subcategory-table.md)。


## <a name="support-info"></a>支持信息

此部分让你提供帮助客户更多了解你的应用以及如何获取支持的信息。

### <a name="privacy-policy-url"></a>隐私策略 URL

你有责任确保你的应用符合隐私法律法规的相关规定，并在此处提供有效的隐私策略 URL（如果需要）。

在此部分中，你必须指明你的应用是否访问、收集或传输任何[个人信息](https://docs.microsoft.com/legal/windows/agreements/store-policies#105-personal-information)。 如果你的回答为**是**，则必需提供隐私策略 URL。 否则，则为可选信息（但是，如果我们确定你的应用需要隐私策略，但你未提供，你的提交可能无法通过认证）。

> [!NOTE]
> 如果我们检测到你的程序包声明的[功能](../packaging/app-capability-declarations.md)可能允许访问、传输或收集个人信息，我们会将这个问题标记为**是**，你将需要输入隐私策略 URL。

若要帮助你确定应用是否需要隐私策略，请查看[应用开发人员协议](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement)和 [Microsoft Store 策略](https://docs.microsoft.com/legal/windows/agreements/store-policies#105-personal-information)。 

> [!NOTE]
> Microsoft 不会为你的应用提供默认的隐私策略。 同样，你的应用也不在任何 Microsoft 隐私策略的涵盖范围之内。 


### <a name="website"></a>网站

输入你的应用的网页 URL。 该 URL 必须指向你自己网站上的某个页面，而不是 Microsoft Store 中你的应用的 Web 一览。 此字段是可选的，但建议使用它。

### <a name="support-contact-info"></a>支持联系人信息

输入你的客户可以从中获得应用支持的网页 URL 或客户可以联系技术支持人员的电子邮件地址。 我们建议在所有提交中加入此信息，以便你的客户了解如何在需要时获取支持。 请注意，Microsoft 不会为你的客户提供对你的应用的支持。

> [!IMPORTANT]
> 如果你的应用或游戏在 Xbox 上提供，**支持联系人信息**字段是必填字段。 否则为可选字段（推荐）。


## <a name="game-settings"></a>游戏设置

仅当选择**游戏**作为你的产品类别时，才显示此选项。 可在此处指定你的游戏支持的功能。 此部分中提供的所有信息将显示在产品的应用商店一览中。

如果你的游戏支持多人游戏选项，请务必指示一个会话的最小玩家数量和最大玩家数量。 输入的最小玩家数量和最大玩家数量不能超过 1,000。

**跨平台多人游戏**指的是游戏支持 Windows 10 电脑和 Xbox 玩家之间的多人游戏会话。


## <a name="display-mode"></a>显示模式

此部分让你可以指示产品是否是要在电脑和/或 HoloLen 设备上的 [Windows Mixed Reality](https://developer.microsoft.com/windows/mixed-reality) 沉浸式（不是 2D）视图中运行。 如果你指示为是，则还需要：
- 在显示于**属性**页面下方的[系统要求](#system-requirements)部分，为 **Windows Mixed Reality 沉浸式头戴显示设备**选择**最低硬件**或**推荐硬件**。
- 指定**边界设置**（如果已选择电脑），以便用户了解它是只能在坐下或站立时使用，还是允许（或要求）用户在使用时四处移动。 

如果选择了**游戏**作为产品类别，你将看到**显示模式**选择中的其他选项，让你可以指示你的产品是支持 4K 分辨率视频输出、高动态范围 (HDR) 视频输出还是变量刷新频率显示。

如果你的产品不支持这些显示模式选项的任何一个，则所有复选框都保持不选。


## <a name="product-declarations"></a>产品声明

你可以在本部分中选中相应复选框以指示任意声明是否适用于你的应用。 无论应用是否提供给某些客户，这都会影响应用的显示方式，或客户使用应用的方式。

有关详细信息，请参阅[产品声明](app-declarations.md)。

## <a name="system-requirements"></a>系统要求

在本部分中，你可以选择指示是否需要或推荐某些硬件功能以使应用正常运行并与之正确交互。 可以选中每个想要指定“最低硬件要求”**** 和/或“推荐的硬件要求”**** 的硬件项的框（或指示相应选项）。

如果选择“推荐的硬件要求”**** 中的内容，这些项目将作为向使用 Windows 10 版本 1607 或更高版本的客户推荐的硬件而显示在产品的应用商店一览中。 使用较早的操作系统版本的客户将无法看到此信息。

如果选择“最低硬件要求”**** 中的内容，这些项目将作为使用 Windows 10 版本 1607 或更高版本的客户所必需的硬件而显示在产品的应用商店一览中。 使用较早的操作系统版本的客户将无法看到此信息。 应用商店可能还会向在缺少所需硬件的设备上查看应用一览的客户显示一条警告。 这不会阻止用户将你的应用下载到不具有相应硬件的设备，但他们无法在这些设备上对你的应用进行评分或评价。 

客户的行为将各有不同，具体取决于特定要求和客户的 Windows 版本：

- **对于使用 Windows 10 版本 1607 或更高版本的客户：**
     - 所有最低和推荐的要求都将显示在应用商店一览中。
     - 应用商店将检查所有最低要求，并且将向所用设备不符合这些要求的客户显示一条警告。
- **对于使用较早版本的 Windows 10 的客户：**
     - 对于大多数客户而言，所有最低和推荐的硬件要求都将显示在应用商店一览中（查看较早版本的应用商店客户端的客户将只能看到最低硬件要求）。
     - 应用商店将尝试验证指定为“最低硬件要求”**** 的项目，“内存”****、“DirectX”****、“视频内存”****、“图形”**** 和“处理器”**** 除外；这些项目不会得到验证，客户也将不会在不符合这些要求的设备上看到任何警告。 
- **对于使用 Windows 8.x 和较早版本或 Windows Phone 8.x 和较早版本的客户：**
     - 如果选中“触摸屏”**** 的“最低硬件要求”**** 框，此要求将显示在应用的应用商店一览中，并且使用缺少触摸屏的客户将在尝试下载该应用时看到警告。 应用商店一览不会验证或显示其他要求。

我们还建议将针对指定硬件的运行时检查添加到应用中，因为应用商店可能并不总是能够检测到客户的设备缺少所选功能，而且即使已显示警告，他们仍然能够下载应用。 如果想要完全阻止 UWP 应用下载到不满足内存或 DirectX 级别最低要求的设备上，可以在 [StoreManifest XML 文件](https://docs.microsoft.com/uwp/schemas/storemanifest/storemanifestschema2015/schema-root)中指定最低要求。

> [!TIP]
> 如果你的产品需要使用本部分中未列出的其他项才可正常运行，例如 3D 打印机或 USB 设备，则也可以在创建应用商店一览时输入[其他系统要求](create-app-store-listings.md#additional-system-requirements)。





