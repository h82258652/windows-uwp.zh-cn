---
author: jnHs
Description: Manage submission options such as publishing hold options, notes for certification, and more.
title: 管理提交选项
ms.author: wdg-dev-content
ms.date: 05/09/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, 发布暂停, 发布日期, 发送提交以发布, 受限功能审核, windows 10, uwp, publishing hold, publish date, send submission to publish, restricted capability approval
ms.localizationpriority: medium
ms.openlocfilehash: 147f34c40cc5d2b612dcdd92edc0c76340cf58f7
ms.sourcegitcommit: a160b91a554f8352de963d9fa37f7df89f8a0e23
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/21/2018
ms.locfileid: "4121922"
---
# <a name="manage-submission-options"></a>管理提交选项

在应用提交过程的**提交选项**页面，你可以提供更多信息来帮助我们适当测试你的产品。 此步骤为可选项，但是适用于多种提交，建议执行此步骤。 如果你想要推迟发布过程，你还可以选择设置发布暂停选项。


## <a name="publishing-hold-options"></a>发布暂停选项

默认情况下，我们将在提交通过认证后尽快发布（或按照你在**定价和可用性**页面的[计划](configure-precise-release-scheduling.md)部分指定的任何日期）。 你可以选择在某个日期前或在你手动指示应该发布前暂停发布你的提交。 这个部分的选项如下所述。 


### <a name="publish-your-submission-as-soon-as-it-passes-certification-or-per-dates-you-specify"></a>在通过认证后尽快发布提交（或按指定日期）

**通过认证后立即发布此提交（或按照计划部分中选择的日期）** 是默认选择，这意味着你的提交在通过认证后即开始发布过程，除非你在**定价和可用性**页面的[计划](configure-precise-release-scheduling.md)部分配置了日期。   

对于大多数提交，我们建议保留**发布暂停选项**部分对此选项的设置。 如果你希望为提交指定特定的发布日期，请使用**通过认证后立即发布此提交（或按照计划部分中选择的日期）**。 保留此部分设置的默认选项不会导致在**计划**部分中所设置的日期之前发布提交。 在**计划**部分中选择的日期将用于确定当你的产品应用商店中提供给客户。


### <a name="publish-your-submission-manually"></a>手动发布你的提交

如果还不想设置发布日期，并且希望自己的提交保持未发布的状态，直到手动决定启动发布流程为止，则可选择**在我选择“立即发布”前不发布此提交**。 选择此选项意味着将不发布所提交的内容，直到自己做出指示为止。 后你提交通过认证，你可以将其发布通过在认证状态页上，选择**现在发布**或通过相同的方式选择特定日期，如下所述。


### <a name="start-publishing-your-submission-on-a-certain-date"></a>在特定日期开始发布你的提交

选择**于此日期开始发布此提交**来确保在特定日期之前不发布提交。 使用此选项，你的提交将在你指定的日期当天或之后立即发布。 该日期必须为 24 小时之后的一个日期。 除了日期，还可指定开始发布提交的时间。 

你可以更改此后，将产品提交的发布日期，只要它尚未进入发布步骤。 
 
如前文所述，如果你希望为提交指定特定的发布日期，请使用**通过认证后立即发布此提交（或按照计划部分中选择的日期）** 并将**发布暂停选项**保留为默认选择。 使用**于此日期开始发布此提交**选项意味着你的提交在这个日期之前不会开始发布过程，不过认证或发布期间的推迟可能导致实际发布日期晚于你选择的日期。 


## <a name="notes-for-certification"></a>认证说明

当你提交应用时，你可以选择使用**认证说明**部分向认证测试人员提供额外信息。 此信息有助于确保正确测试你的应用。 

有关详细信息，请参阅[认证说明](notes-for-certification.md)。


## <a name="restricted-capabilities"></a>受限功能

如果我们检测到你的程序包声明了任何[受限功能](../packaging/app-capability-declarations.md#restricted-capabilities)，你将需要在此部分提供信息才能获得批准。 对于每个功能，告诉我们你的应用需要声明该功能的原因以及如何使用该功能。 请务必提供尽可能详细的信息，以帮助我们了解你的产品需要声明该功能的原因。 

在认证过程中，我们的测试人员将审核你提供的信息，以确定是否批准你的提交使用该功能。 请注意，这可能会给你的提交增加一些额外的时间来完成认证过程。 如果我们批准你使用该功能，你的应用将继续进行认证过程的其余部分。 你向应用提交更新时，通常不必重复功能审批流程（除非你声明了其他功能）。 

如果我们不批准你使用该功能，你的提交将无法通过认证，我们将在认证报告中提供反馈。 然后，你可以选择创建新的提交并上传未声明该功能的程序包，或者在适用情况下解决与使用该功能有关的任何问题，然后在新提交中申请批准。

请注意，有些受限功能很少会获得批准。 有关每个受限功能的详细信息，请参阅[应用功能声明](../packaging/app-capability-declarations.md#restricted-capabilities)。

