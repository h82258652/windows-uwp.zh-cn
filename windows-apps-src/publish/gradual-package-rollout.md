---
author: JnHs
Description: When you publish an update to a submission, you can choose to gradually roll out the updated packages to a percentage of your app’s customers on Windows 10.
title: 逐步程序包推出
ms.author: wdg-dev-content
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 65d578a6-4e26-484c-90af-b2cd916f3634
ms.localizationpriority: medium
ms.openlocfilehash: ab2db3d34ed223b318d65ec497cc0feb7cb16342
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/01/2018
ms.locfileid: "5880242"
---
# <a name="gradual-package-rollout"></a>逐步程序包推出

当你发布更新提交时，你可以选择逐步推出已更新的软件包到你的应用的客户的百分比 （包括 Xbox） 的 Windows 10 上。 这使你可以监视特定程序包的反馈和分析数据，从而确保在更广泛地推出更新前对此更新无虑。 你可以随时增加比例（或停止更新），而无需创建新的提交。 

> [!IMPORTANT]
> 推出选择将应用于所有程序包，但仅适用于运行的 OS 版本支持软件包外部测试版（Windows.Desktop 版本 10586 或更高版本、Windows.Mobile 版本 10586.63 或更高版本以及 Xbox）的客户，其中包括通过[应用商店托管（联机）许可](organizational-licensing.md)、[适用于企业的 Microsoft 应用商店](https://businessstore.microsoft.com/store)或[适用于教育的 Microsoft 应用商店](https://educationstore.microsoft.com/store)获取应用的所有客户。 逐步推出程序包时，使用较早 OS 版本的客户将不会从最新提交中获取程序包，直到你完成程序包推出，如以下所述。

请注意，所有客户会看到你通过最新提交输入的应用商店一览详细信息。 推出设置仅应用于客户针对新购置和现有客户的更新收到的程序包。

> [!TIP]
> 程序包推出会将程序包分配给按指定的比例随机选择的客户。 若要将特定程序包分配给你指定的所选客户，可以使用软件包外部测试版。 如果希望向一个外部测试版组逐步分配更新，还可以将此推出与软件包外部测试版组合起来。


## <a name="setting-the-rollout-percentage"></a>设置推出百分比

可以选择在已更新提交的“程序包”**** 页上推出你的更新。 为此，请选中显示“在此提交发布后，逐渐（仅向 Windows 10 客户）推出更新”**** 的框。 然后输入首次发布提交时应获取更新的客户的百分比。 例如，如果希望一开始只向应用的一小部分客户推出更新，可能会输入 5。

单击“更新”**** 保存选择。 应用完成认证过程后，将根据你针对新购置和现有客户的更新指定的百分比将程序包分配给客户。


## <a name="adjusting-the-rollout-after-the-submission-is-published"></a>在提交发布后调整推出

若要在提交已发布后调整推出，请转到应用的概述页。 可以拖动选择器更改从你的最新提交中获取程序包的客户的百分比。 单击“更新”**** 保存选择。 将开始根据你针对新购置和现有客户的更新指定的百分比将程序包分配给客户。


## <a name="completing-the-rollout"></a>完成推出

在可以创建新的提交之前，需要完成程序包推出。 可以**完成**推出并将最新程序包分配给所有客户，也可以**停止**推出以停止分配最新程序包。

如果你对更新有信心并想要将它提供给所有客户，请单击“完成程序包推出”**** 向所有客户分配最新程序包。

> [!TIP]
> 将推出百分比更改为 100% 并不能确保所有客户都将从最新提交中获取程序包，因为某些客户使用的 OS 版本可能不支持推出。 必须完成推出，才可以停止分配较旧的程序包，将所有现有客户更新为较新客户。

如果发现更新中存在问题，并且不想再分配它，可以单击“停止程序包推出”**** 停止从最新提交分配程序包。 停止程序包推出后，将不再向任何客户分配这些程序包；仅将以前提交中的程序包提供给任何新的或正在更新的客户。 不过，已具有较新程序包的任何客户可以保留这些程序包；它们无法回退到以前的版本。 若要向这些客户提供更新，需要使用你想让他们获取的程序包创建新的提交。 请注意，如果在下一个提交中使用逐步推出，将向具有你已停止推出的程序包的客户提供新更新（提供的顺序与向他们提供已停止推出程序包的顺序相同）。 新推出将介于你最后完成的提交和最新提交之间；停止推出程序包后，将不再向任何客户分配这些程序包。
