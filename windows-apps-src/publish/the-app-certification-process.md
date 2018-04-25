---
author: jnHs
Description: When you finish creating your app's submission and click Submit to the Store, the submission enters the certification step.
title: 应用认证过程
ms.assetid: 0DCB4344-224D-4E5A-899F-FF7A89F23DBC
ms.author: wdg-dev-content
ms.date: 03/09/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, 正在发布, 预处理, 认证, 版本, 挂起, 提交, 发布, 状态
ms.localizationpriority: high
ms.openlocfilehash: 0b2191808457401a41fe6bb0996d3f5a5ed4943d
ms.sourcegitcommit: 1eabcf511c7c7803a19eb31f600c6ac4a0067786
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="the-app-certification-process"></a>应用认证过程

当你完成应用提交的创建并单击**提交到 Microsoft Store** 时，提交将进入认证步骤。 此过程通常在几小时内完成，但在某些情况下可能需要最多三个工作日。 在你的提交通过认证后，客户最多可能需要 24 个小时即可在应用商店中看到该应用一览（或你对之前已发布应用的更新）。 在你的提交发布并向客户提供后，你会看到一条通知，并且仪表板中该应用的状态将为“已在应用商店”****。

## <a name="preprocessing"></a>预处理

成功上载应用包并提交应用进行认证后，应用包将排队接受测试。 如果在预处理过程中检测到任何错误，我们会显示一条消息。 有关可能错误的详细信息，请参阅[解决提交错误](resolve-submission-errors.md)。

## <a name="certification"></a>认证

在此阶段中，将执行多个测试：

-   **安全性测试：** 第一项测试检查应用包中是否存在病毒和恶意软件。 如果应用未能通过这项测试，你将需要运行最新防病毒软件检查开发系统，然后在安全的系统上重新生成你的应用包。
-   **技术合规性测试：**由 Windows 应用认证工具包测试技术合规性。 （你应该始终确保先[使用 Windows 应用认证工具包测试应用](../debug-test-perf/windows-app-certification-kit.md)，然后再将其提交至应用商店。）
-   **内容合规性：**测试所需的时间会有所不同，具体取决于应用的复杂程度、包含的视觉内容量以及近期提交的应用数量。 请务必在[认证说明](notes-for-certification.md)页中提供测试者需注意的全部信息。

认证流程完成后，你将会收到一份认证报告，告知你的应用是否已通过认证。 如果应用未通过认证，该报告将指出未能通过哪项测试，或者未能满足哪项[策略](https://docs.microsoft.com/legal/windows/agreements/store-policies)。 修复问题后，你可以为应用创建新提交以再次开始认证过程。

## <a name="release"></a>发行

当你的应用通过认证时，便可以前进到**发布**过程。 如果你已指示应尽快发布你的提交，此提交将立即执行。 如果你已指定应用不应在特定日期之前发布，我们将等到该日期，除非你单击**“更改发布日期”**链接。 如果你已指示要手动发布提交，那么只有在你通过单击**“立即发布”**按钮指示我们这样做之后，我们才会发布；或者如果你单击**“更改发布日期”**链接并选取特定日期，我们将在该日期发布。

## <a name="publishing"></a>正在发布

你的应用包已经过数字签名，目的是防止它们在发布后遭到篡改。 一旦开始执行此阶段，你将再也无法取消提交或更改其发布日期。

当你的应用处于发布阶段时，应用提交的“状态”列中的**“显示详细信息”**链接将通知你新的程序包和应用商店一览详细信息何时可以提供给每个使用受支持操作系统版本的客户。 尚未完成的步骤将显示**挂起**。 你的应用将继续处于发布阶段，直到此过程完成，意味着新的程序包和一览详细信息提供给应用的所有潜在客户为止。 这可能最多需要 24 小时。 

## <a name="in-the-store"></a>已在 Microsoft Store 

在成功完成上述步骤后，提交的状态将从**正在发布**更改为**已在 Microsoft Store**。 你的提交将在 Microsoft Store 中提供给客户以供其下载（除非你选择了另外的[可发现性](choose-visibility-options.md#discoverability)选项）。 

> [!NOTE]
> 我们还会在应用发布后对应用进行抽查，以便可以找出潜在问题并确保你的应用符合所有 [Microsoft Store 策略](https://docs.microsoft.com/legal/windows/agreements/store-policies)。 如果我们发现任何问题，将通知你该问题及其解决方法（如果适用）或是否已从应用商店中删除。

 

 

 




