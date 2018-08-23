---
author: jnHs
Description: When you finish creating your app's submission and click Submit to the Store, the submission enters the certification step.
title: 应用认证过程
ms.assetid: 0DCB4344-224D-4E5A-899F-FF7A89F23DBC
ms.author: wdg-dev-content
ms.date: 07/02/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10，uwp，发布，预处理，认证释放，挂起、 提交、 发布，状态，时间
ms.localizationpriority: medium
ms.openlocfilehash: 8372f316786d83d72dff8ef7a0a8fd53e5390743
ms.sourcegitcommit: 9c79fdab9039ff592edf7984732d300a14e81d92
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/23/2018
ms.locfileid: "2811006"
---
# <a name="the-app-certification-process"></a>应用认证过程

当你完成应用提交的创建并单击**提交到 Microsoft Store** 时，提交将进入认证步骤。 此过程通常在几小时内完成，但在某些情况下可能需要最多三个工作日。 您提交超过证书后，它可能需要达 24 小时，以便客户能够看到新提交，或更改更新提交到包的应用程序的列表。 如果您更新仅更改商店列表详细信息，将不会超过一小时内完成发布过程。  发布您的提交，和仪表板中的应用程序的状态将**在存储区**时，您将收到通知。

## <a name="preprocessing"></a>预处理

成功上载应用包并提交应用进行认证后，应用包将排队接受测试。 如果在预处理过程中检测到任何错误，我们会显示一条消息。 有关可能错误的详细信息，请参阅[解决提交错误](resolve-submission-errors.md)。

## <a name="certification"></a>认证

在此阶段中，将执行多个测试：

-   **安全性测试：** 第一项测试检查应用包中是否存在病毒和恶意软件。 如果应用未能通过这项测试，你将需要运行最新防病毒软件检查开发系统，然后在安全的系统上重新生成你的应用包。
-   **技术合规性测试：** 由 Windows 应用认证工具包测试技术合规性。 （你应该始终确保先[使用 Windows 应用认证工具包测试应用](../debug-test-perf/windows-app-certification-kit.md)，然后再将其提交至应用商店。）
-   **内容合规性：** 测试所需的时间会有所不同，具体取决于应用的复杂程度、包含的视觉内容量以及近期提交的应用数量。 请务必在[认证说明](notes-for-certification.md)页中提供测试者需注意的全部信息。

认证流程完成后，你将会收到一份认证报告，告知你的应用是否已通过认证。 如果应用未通过认证，该报告将指出未能通过哪项测试，或者未能满足哪项[策略](https://docs.microsoft.com/legal/windows/agreements/store-policies)。 修复问题后，你可以为应用创建新提交以再次开始认证过程。

## <a name="release"></a>发行

当您的应用程序传入证书时，系统已准备好移动到**发布**过程。

- 如果您已指示应为尽快 （默认选项） 发布您提交，将立即开始发布过程。
- 如果这是首次已发布应用程序，并在[计划](configure-precise-release-scheduling.md#release)部分中指定**发布日期**、 应用程序将变为可用根据您的**发布日期**选择。
- 如果您已使用[发布保留选项](manage-submission-options.md#publishing-hold-options)以指定它应不被释放在特定日期前，我们将等到该日期开始发布过程中，除非您选择**更改发行版的日期**。
- 如果您已使用[发布保留选项](manage-submission-options.md#publishing-hold-options)指定您希望手动发布提交，我们不会启动发布过程，直到您选择**立即发布**（或选择**更改发行版的日期**和选择特定日期）。


## <a name="publishing"></a>正在发布

你的应用包已经过数字签名，目的是防止它们在发布后遭到篡改。 一旦开始执行此阶段，你将再也无法取消提交或更改其发布日期。

有关新应用程序和更新，其中包括更改应用程序的包，将在 24 小时内完成发布过程。 对于仅更改选项，如商店列表的详细信息，但不更改的应用程序包的更新，发布过程需要一个小时以内。

发布阶段您的应用程序时，会显示您的应用程序提交状态列中的**显示详细信息**链接，让您了解您新包和商店列表详细信息何时适用于每个受支持的操作系统版本上的客户。 尚未完成的步骤将显示**挂起**。 您的应用程序将保留在之前过程已完成的发布阶段，表示新包和/或列表的详细信息可用于所有应用程序的潜在客户。

## <a name="in-the-store"></a>已在 Microsoft Store 

在成功完成上述步骤后，提交的状态将从**正在发布**更改为**已在 Microsoft Store**。 你的提交将在 Microsoft Store 中提供给客户以供其下载（除非你选择了另外的[可发现性](choose-visibility-options.md#discoverability)选项）。 

> [!NOTE]
> 我们还会在应用发布后对应用进行抽查，以便可以找出潜在问题并确保你的应用符合所有 [Microsoft Store 策略](https://docs.microsoft.com/legal/windows/agreements/store-policies)。 如果我们发现任何问题，将通知你该问题及其解决方法（如果适用）或是否已从应用商店中删除。

 

 

 




