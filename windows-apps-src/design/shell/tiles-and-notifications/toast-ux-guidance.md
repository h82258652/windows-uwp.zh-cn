---
Description: 了解如何创建有效且专注于用户的通知来提高用户的工作效率和快乐。
title: Toast 通知的用户体验指南
label: Toast UX Guidance
template: detail.hbs
ms.date: 05/18/2018
ms.topic: article
keywords: windows 10、 uwp、 通知、 集合、 组、 用户体验、 用户体验指南，指导、 操作、 toast、 操作中心、 noninterruptive、 有效的通知、 非侵入式通知可操作的管理、 组织
ms.localizationpriority: medium
ms.openlocfilehash: 327a2add84343be3b972f7bb1f232298e7ef92ad
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2019
ms.locfileid: "67320731"
---
# <a name="toast-notification-ux-guidance"></a>Toast 通知用户体验指南
通知是现代生活; 的必要部分它们可帮助用户提高工作效率和参与与应用程序和网站，及使用的任何更新了解最新。 但是，通知可以快速打开从适用于 overbearing 和侵入性，如果它们不设计以用户为中心的方式。 通知是一个右键单击从正在关闭，并且不太可能已关闭后，它们将再次打开。  因此请确保您的通知是用户的屏幕空间和时间，不会冒犯他人，因此可以保持此 engagement 通道打开。

> **重要的 API**：[Windows 社区工具包通知 nuget 包](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)

我们已经分析了我们的 Windows 遥测，以及其他第一个和第三方案例研究，拿围绕就很好的通知情景的四个规则。  我们确信这些规则都普遍适用，无论使用什么平台，并将帮助您对用户产生积极的影响的通知。

## <a name="1-actionable-notifications"></a>1.可操作的通知
可操作的通知，用户可以高效工作，而无需打开您的应用程序。  尽管它非常具有应用程序中启动、 这不是唯一的度量值的成功，并使用户能够执行完成小任务而无需转到你的应用可能非常强大的工具中，要取悦用户。

![具有输入的文本框和按钮设置提醒并响应该通知可操作的通知](images/actionable-notification-example01.png)

上面是一条通知，利用操作的示例。 正在完成任务的感觉是普遍正感觉，，你可以将这种感觉到你的应用或网站来发送通知，其中包含的可操作的内容。 可操作的通知还可以帮助提高工作效率，同时在企业和使用者方案中，通过减少到操作用户时间经历用来完成这些较小的任务。 我们建议包括操作定期拍摄在你的用户或想要训练用户执行的操作。  一些示例如下：
* 根据需要收藏，标记，或男内容
* 批准或拒绝费用报表、 休假的权限，等等。
* 内联回复邮件，电子邮件，组聊天、 评论等。
* 完成排序使用[挂起更新](toast-pending-update.md)
* 另一次设置警报或提醒，以及可能在日历中预订时间

可操作的通知是非常强大的工具，帮助用户感觉提高工作效率，完成任务，并且具有与你的应用或网站的功能强大、 高效的体验。  有很多机会此处 ！ 如果需要帮助来触发想法，随意向 windows 通知团队联系。

## <a name="2-timing-and-urgency"></a>2.计时和紧急性
与我们通常对通知，相反实时不一定是最好 ！ 我们极力主张开发人员考虑一下用户以及要发送的通知是否紧迫的信息，或不。 用户可以轻松地重载具有太多的信息，并获取焦点尝试时将被中断的情况下感到沮丧。 Windows 提供了有关如何侵入性的要发送的通知，请考虑几个选项：

**原始通知：** 使用[原始通知](raw-notification-overview.md)十分有益出于许多原因，尤其当它面对 ot 最大程度减少用户中断。  发送原始通知将被唤醒您的应用程序在后台，这样就可以评估是否通知意义将立即在应用程序的上下文中。 如果您认为的内容应显示给用户，可以弹出[本地 toast](send-local-toast.md)从那里。  如果它是用户不需要以查看现在，可以创建[计划 toast](https://blogs.msdn.microsoft.com/tiles_and_toasts/2016/09/30/quickstart-sending-an-alarm-in-windows-10/) ，将在更高版本时激发。


**虚影 toast:** 也可以激发一条通知，将跳过弹出屏幕的右下角中，而是直接向操作中心发送通知。 这可以通过设置[SupressPopup 属性](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastnotification.suppresspopup)为 True。 虽然有一些怀疑围绕不弹出通知操作中心之外，我们看到 2-3 倍更高版本适用于通过生活在操作中心的 toast engagement 弹出 toast。  当他们已准备好接收通知，并可以控制当它们被中断，这就是为什么能够 noninvasively 通知用户更加有效地操作中心中的内容时，用户是响应速度更快。

## <a name="3-clear-out-the-clutter"></a>3.清除混乱
通知可以在操作中心中保留相当长的时间 （默认值 3 天）。  它是命令性，请确保位于此处的内容是最新且相关，每次用户打开操作中心。 您是用户的屏幕空间浪费，占用了可用于某些较新的槽。  让我们假设用户安装电子邮件管理应用，并接收 10 封电子邮件和电子邮件以及十个通知。  根据你所需的体验，可以考虑清除这些通知，如果用户具有读取相应的电子邮件，或打开了该应用程序作为一种方法从操作中心中删除旧的混乱。

我们提供了一系列[ToastNotificationHistory](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastnotificationhistory) Api，允许您看到哪些内容是在操作中心，以及管理这些通知。 是严格地遵循用户的屏幕空间，请注意，仅向用户显示相关和最新内容。

## <a name="4-keeping-organized"></a>4.保持有组织
正如前面提到的用于在三天 does 保留操作中心中的内容。  若要帮助用户找出快速寻找的信息，组织中使用操作中心通知[标头](https://docs.microsoft.com/en-us/windows/uwp/design/shell/tiles-and-notifications/toast-headers)或[集合](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastcollection)。 您所见的标头下面的示例。

![使用标头的 toast 通知示例标记为 Camping!!](images/toast-headers-action-center.png)

组合这些通知的方式，以便相关的内容保持在一起 （即认为划分不同的体育一在体育应用中，或对消息进行排序的群组聊天）。 集合是组通知更明确的方法，而标头却比较细微，但同时允许用户进行会审和更快地找出通知。

## <a name="other-resources"></a>其他资源
上述这些四个点是通过我们自己分析遥测数据，并通过第一个和第三方试验我们已找到有效的指南。 记住，但是，这些指导原则就是这样： 指导原则。  我们确信这些规则有助于提高参与度和生产力的通知，但需执行任何操作可以使用替换以用户为中心的构想，和你自己的数据中学习。  

如果立即发送到 UWP 应用的通知，则可以查看分析在通知中发生了什么情况[合作伙伴中心](https://partner.microsoft.com/dashboard)！ 使用时，这些数据包括免费[存储区服务 SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftStoreServicesSDK)或[WNS Api](https://docs.microsoft.com/en-us/windows/uwp/design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview)。 这些指标将可提供更详细地了解您的通知在 windows 平台上，会发生什么情况以及如何使用通知了解用户交互。 通过在左侧显示端参与上转到菜单中访问此数据 > 通知，然后单击通知页中的"分析"选项卡上。  该文件位于您将转到从合作伙伴中心将通知发送的同一位置。

## <a name="related-topics"></a>相关主题

* [Toast 通知内容](adaptive-interactive-toasts.md)
* [原始通知](raw-notification-overview.md)
* [挂起的更新](toast-pending-update.md)
* [GitHub （Windows 社区工具包的一部分） 上的通知库](https://github.com/windows-toolkit/WindowsCommunityToolkit/tree/master/Microsoft.Toolkit.Uwp.Notifications)
