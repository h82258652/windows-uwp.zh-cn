---
Description: When you finish creating your app's submission and click Submit to the Store, the submission enters the certification step.
title: 应用认证过程
ms.assetid: 0DCB4344-224D-4E5A-899F-FF7A89F23DBC
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10，uwp，发布，预处理，认证，版本，挂起，提交，发布，状态的时间
ms.localizationpriority: medium
ms.openlocfilehash: 733d5ff882d7ed7c574f6fe6fedd28b79c3913d9
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2018
ms.locfileid: "8710444"
---
# <a name="the-app-certification-process"></a>应用认证过程

当你完成应用提交的创建并单击**提交到 Microsoft Store** 时，提交将进入认证步骤。 此过程通常在几小时内完成，但在某些情况下可能需要最多三个工作日。 你的提交通过认证后，可能需要 24 小时客户都可以看到该应用一览，以便用于新的提交，或对程序包更新提交所做的更改。 如果你更新仅更改应用商店一览详细信息，将在一小时内完成发布流程。  发布你的提交，并在仪表板中的应用的状态将为**在应用商店**时，你将收到通知。

## <a name="preprocessing"></a>预处理

成功上载应用包并提交应用进行认证后，应用包将排队接受测试。 如果在预处理过程中检测到任何错误，我们会显示一条消息。 有关可能错误的详细信息，请参阅[解决提交错误](resolve-submission-errors.md)。

## <a name="certification"></a>认证

在此阶段中，将执行多个测试：

-   **安全性测试：** 第一项测试检查应用包中是否存在病毒和恶意软件。 如果应用未能通过这项测试，你将需要运行最新防病毒软件检查开发系统，然后在安全的系统上重新生成你的应用包。
-   **技术合规性测试：** 由 Windows 应用认证工具包测试技术合规性。 （你应该始终确保先[使用 Windows 应用认证工具包测试应用](../debug-test-perf/windows-app-certification-kit.md)，然后再将其提交至应用商店。）
-   **内容合规性：** 测试所需的时间会有所不同，具体取决于应用的复杂程度、包含的视觉内容量以及近期提交的应用数量。 请务必在[认证说明](notes-for-certification.md)页中提供测试者需注意的全部信息。

认证流程完成后，你将会收到一份认证报告，告知你的应用是否已通过认证。 如果应用未通过认证，该报告将指出未能通过哪项测试，或者未能满足哪项[策略](https://docs.microsoft.com/legal/windows/agreements/store-policies)。 修复问题后，你可以为应用创建新提交以再次开始认证过程。

## <a name="release"></a>发行

当你的应用通过认证时，就可以移动到**发布**过程。

- 如果你已指示应尽快 （默认选项） 发布你的提交，将立即开始发布过程。
- 如果这是第一次已发布应用，并且你指定**发布日期**[计划](configure-precise-release-scheduling.md#release)部分中，应用将推出根据你的**发布日期**选择。
- 如果你已使用[发布暂停选项](manage-submission-options.md#publishing-hold-options)指定应不会发布在特定日期之前，我们将等到该日期开始发布过程中，除非你选择**更改发布日期**。
- 如果你已使用[发布暂停选项](manage-submission-options.md#publishing-hold-options)指定你想要手动发布提交，我们将不会开始发布过程，直到你选择**立即发布**（或选择**更改发布日期**并选取特定日期）。


## <a name="publishing"></a>正在发布

你的应用包已经过数字签名，目的是防止它们在发布后遭到篡改。 一旦开始执行此阶段，你将再也无法取消提交或更改其发布日期。

对于新的应用和更新包括对应用包的更改，将在 24 小时内完成发布流程。 对于仅更改选项，例如应用商店一览详细信息，但不会更改应用的程序包的更新，发布过程将需要少于一小时。

当你的应用处于发布阶段时，你的应用提交的状态列中的**显示详细信息**链接可使你知道你的新的程序包和一览详细信息的应用商店时为每个受支持的操作系统版本上的客户提供。 尚未完成的步骤将显示**挂起**。 你的应用将继续处于发布阶段，直到此过程完成，意味着新的程序包和/或一览详细信息可供所有应用的潜在客户。

## <a name="in-the-store"></a>已在 Microsoft Store 

在成功完成上述步骤后，提交的状态将从**正在发布**更改为**已在 Microsoft Store**。 你的提交将在 Microsoft Store 中提供给客户以供其下载（除非你选择了另外的[可发现性](choose-visibility-options.md#discoverability)选项）。 

> [!NOTE]
> 我们还会在应用发布后对应用进行抽查，以便可以找出潜在问题并确保你的应用符合所有 [Microsoft Store 策略](https://docs.microsoft.com/legal/windows/agreements/store-policies)。 如果我们发现任何问题，将通知你该问题及其解决方法（如果适用）或是否已从应用商店中删除。

 

 

 




