---
author: jnHs
Description: "查看此列表以帮助避免频繁地阻止应用通过认证的问题，或者在发布应用后，可能在点检查过程中标识的问题。"
title: "避免常见的认证失败"
ms.assetid: 9E9E3841-2F9B-42D4-B5F8-4C7C31E42E3D
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: f8b61d14b46614680b84da5aa7e4413159a0cfb1
ms.lasthandoff: 02/07/2017

---

# <a name="avoid-common-certification-failures"></a>避免常见的认证失败


查看此列表以帮助避免频繁地阻止应用通过认证的问题，或者在发布应用后，可能在点检查过程中标识的问题。

> **注意**  若要确保你的应用满足其中所列出的所有要求，你还应查看 [Windows 应用商店策略](https://msdn.microsoft.com/library/windows/apps/dn764944)。

 

-   仅在应用完成时提交你的应用。 欢迎使用你的应用提要来介绍即将推出的功能，但是请确保你的应用不包含未完成的部分、指向正在构建的网页的链接，或者其他任何会让客户觉得你的应用不完整的内容。

-   在提交应用前[使用 Windows 应用认证工具包测试应用](https://msdn.microsoft.com/library/windows/apps/mt186449)。

-   在多个不同配置上测试你的应用，以尽可能确保应用稳定。

-   确保您的应用在没有网络连接时不会崩溃。 即使您的应用需要连接网络才能实际使用，也应该能够在没有网络连接的情况下适当运行。
-   请确保您的应用的说明清晰地展示您的应用的用途。 有关帮助，请参阅[编写出色的应用说明](write-a-great-app-description.md)中提供的相关指南。

-   务必针对[年龄分级](age-ratings.md)部分中的所有问题提供完整并且准确无误的答案。

-   如果你的应用使用来自 [**Windows.ApplicationModel.Store**](https://msdn.microsoft.com/library/windows/apps/br225197) 命名空间的商用 API，请确保对应用进行测试并验证它是否可处理常见的异常情况。 此外，请确保你的应用使用 [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765) 类（而非 [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) 类，该类仅用于测试）。

-   除非你已经进行专门的工程处理和测试，确定了应用适用于辅助功能方案，否则不要[将应用声明为辅助应用](app-declarations.md#this-app-has-been-tested-to-meet-accessibility-guidelines)。

-   确保提供使用您的应用[时所需的信息](notes-for-certification.md)，例如如果您的应用要求用户登录某项服务，那么您需要提供测试帐户用户名和密码；您还可能需要说明访问隐藏的功能或锁定的功能所需的步骤。

 

 





