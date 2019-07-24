---
Description: 了解如何创建有效且用户为重点的通知, 使用户能够高效工作。
title: Toast UX 指南
label: Toast UX Guidance
template: detail.hbs
ms.date: 05/18/2018
ms.topic: article
keywords: windows 10, uwp, 通知, 收集, 组, ux, ux 指导, 指导, 操作, toast, 操作中心, noninterruptive, 有效通知, 产生干扰通知, 可操作, 管理, 组织
ms.localizationpriority: medium
ms.openlocfilehash: 111ac9a216b87e120e42c9db7761bd8588548029
ms.sourcegitcommit: 720d8778d94ef44d2f86d2f1f0ebb05d6420805d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/23/2019
ms.locfileid: "68415082"
---
# <a name="toast-notification-ux-guidance"></a>Toast 通知 UX 指南
通知是当今生活的必需部分;它们可帮助用户提高工作效率, 并与应用和网站保持同步, 并保持最新的更新。 不过, 如果在以用户为中心的方式设计时, 通知可以快速地从有用到解救和侵入。 你的通知只是在关闭时单击一次, 但一旦关闭, 它们就不会被打开。  因此, 请确保你的通知过于用户的屏幕空间和时间, 从而使此订婚通道保持打开状态。

> **重要的 API**：[Windows 社区工具包通知 nuget 包](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)

我们已经分析了我们的 Windows 遥测以及其他第一方和第三方案例研究, 为您提供了四条围绕着精彩通知案例的规则。  我们确信这些规则是通用的, 不管平台如何, 都将帮助你的通知对用户产生积极的影响。

## <a name="1-actionable-notifications"></a>1.可操作通知
可操作的通知允许用户提高工作效率, 而无需打开应用。  尽管应用程序的启动非常好, 但这并不是成功的唯一衡量, 使用户无需进入你的应用程序就可以使用非常强大的工具来令你的用户。

![具有输入文本框和按钮的可操作通知, 用于设置提醒和响应通知](images/actionable-notification-example01.png)

上面是利用操作的通知的示例。 完成任务的感觉是一项广泛的假象, 你可以通过发送其中包含可操作内容的通知, 将该感受带入你的应用或网站。 在企业和使用者方案中, 可操作的通知还可以缩短用户完成这些小型任务的操作时间, 从而提高工作效率。 建议包括用户经常要执行的操作, 或尝试训练用户执行的操作。  一些示例如下：
* 喜好、加入收藏夹 ...、标记或 starring 内容
* 批准或拒绝支出报表、休息时间、权限等。
* 内嵌回复消息、电子邮件、组聊天、评论等。
* 使用挂起的[更新](toast-pending-update.md)完成订单
* 在其他时间设置警报或提醒, 以及在日历上可能的预订时间

可操作的通知是非常强大的工具, 可帮助用户体验工作效率、完成任务, 并为应用程序或网站提供强大而高效的体验。  这里有很多机会! 如果你想要帮助灵感触发创意, 欢迎访问 windows 通知团队。

## <a name="2-timing-and-urgency"></a>2.计时和紧急性
与我们经常考虑通知的方式相反, 实时不一定是最好的! 我们强烈建议开发人员考虑用户, 如果他们发送的通知是紧急信息, 则为。 如果用户在尝试关注时被中断, 则可以轻松地将其与太多信息超载, 并获得失望。 Windows 提供了一些选项, 用于说明要发送的通知的侵入性:

**原始通知:** 由于许多原因, 使用[原始通知](raw-notification-overview.md)可能会很有用, 特别是在其最大限度地减少用户中断的情况下。  发送原始通知会使你的应用程序在后台唤醒, 因此你可以评估通知是否可立即在应用的上下文中交付。 如果您认为您应该立即向用户显示, 您可以从此处弹出[本地 toast](send-local-toast.md) 。  如果用户现在不需要查看, 则可以创建一个将在以后激发的[计划的 toast](https://blogs.msdn.microsoft.com/tiles_and_toasts/2016/09/30/quickstart-sending-an-alarm-in-windows-10/) 。


**虚影 toast:** 你还可以触发一个通知, 该通知将跳过屏幕右下角, 并改为直接将通知发送到操作中心。 这是通过将[SupressPopup 属性](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastnotification.suppresspopup)设置为 True 来完成的。 尽管可能存在一些怀疑不在操作中心外弹出通知的情况, 但对于在弹出 toast 上实时操作中心的 toast, 我们看到了2-3 倍。  当用户准备好接收通知并可以控制何时中断时, 用户的响应速度更快, 这就是为什么操作中心的内容可以更有效地 noninvasively 通知用户的原因。

## <a name="3-clear-out-the-clutter"></a>3.消除混乱
通知可能会持续很长时间 (默认为三天) 在操作中心内保持不变。  必须确保在此处显示的内容是最新的, 并且每次用户打开操作中心时都是相关的。 您将浪费用户的屏幕空间, 并占用可用于更高版本的产品的槽。  假设用户安装你的电子邮件管理应用, 并收到10封电子邮件和10条通知以及这些电子邮件。  根据所需的体验, 如果用户读取了相应的电子邮件, 或者打开了应用作为从操作中心删除旧杂乱的方式, 则可以考虑清除这些通知。

我们有一系列[ToastNotificationHistory](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastnotificationhistory) api, 可让你查看操作中心的内容, 并管理这些通知。 过于用户的屏幕空间, 并注意仅向用户显示相关和当前内容。

## <a name="4-keeping-organized"></a>4.保持井然有序
如前文所述, 操作中心的内容将保留三天。  若要帮助你的用户快速选择他们想要的信息, 请使用[标头](https://docs.microsoft.com/en-us/windows/uwp/design/shell/tiles-and-notifications/toast-headers)或[集合](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastcollection)在操作中心组织通知。 可在下方查看标题的示例。

![标头标记为 "露营!!" 的 Toast 示例](images/toast-headers-action-center.png)

以某种方式对这些通知进行分组, 以便将相关的内容保持在一起 (例如, 将不同的体育 leagues 分散到运动应用, 或按组聊天对邮件进行分类)。 集合是对通知进行分组的更直观的方式, 而标头则更微妙, 但这两种方法都允许用户更快地会审和获取通知。



以上四个要点是我们通过我们自己的遥测分析以及第一方和第三方试验获得的指导。 但请记住, 这些指导原则只是: 准则。  我们确信这些规则将有助于提高你的通知的参与度和效率, 但不能以用户为中心的思维, 也不能从你自己的数据中进行学习。  

## <a name="related-topics"></a>相关主题

* [Toast 内容](adaptive-interactive-toasts.md)
* [原始通知](raw-notification-overview.md)
* [等待更新](toast-pending-update.md)
* [GitHub 上的通知库 (Windows 社区工具包的一部分)](https://github.com/windows-toolkit/WindowsCommunityToolkit/tree/master/Microsoft.Toolkit.Uwp.Notifications)
