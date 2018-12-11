---
description: 显示如何启动撰写电子邮件对话框以允许用户发送电子邮件。 你可以在显示该对话框之前，使用数据预填充电子邮件的字段。 该消息将在用户点击发送按钮后发出。
title: 发送电子邮件
ms.assetid: 74511E90-9438-430E-B2DE-24E196A111E5
keywords: 联系人, 电子邮件, 发送
ms.date: 10/11/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 1593ab8b547a464492a35aa7d49d38f667a8210b
ms.sourcegitcommit: 231065c899d0de285584d41e6335251e0c2c4048
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/10/2018
ms.locfileid: "8828074"
---
# <a name="send-email"></a>发送电子邮件

显示如何启动撰写电子邮件对话框以允许用户发送电子邮件。 你可以在显示该对话框之前，使用数据预填充电子邮件的字段。 该消息将在用户点击发送按钮后发出。

**本文内容**

-   [启动撰写电子邮件对话框](#launch-the-compose-email-dialog)
-   [摘要和后续步骤](#summary-and-next-steps)
-   [相关主题](#related-topics)

## <a name="launch-the-compose-email-dialog"></a>启动撰写电子邮件对话框

创建新 [**EmailMessage**](https://msdn.microsoft.com/library/windows/apps/Dn631270) 对象，并设置你要在撰写电子邮件对话框中预填充的数据。 调用 [**ShowComposeNewEmailAsync**](https://msdn.microsoft.com/library/windows/apps/Dn631269) 显示对话框。

``` cs
private async Task ComposeEmail(Windows.ApplicationModel.Contacts.Contact recipient,
    string subject, string messageBody)
{
    var emailMessage = new Windows.ApplicationModel.Email.EmailMessage();
    emailMessage.Body = messageBody;

    var email = recipient.Emails.FirstOrDefault<Windows.ApplicationModel.Contacts.ContactEmail>();
    if (email != null)
    {
        var emailRecipient = new Windows.ApplicationModel.Email.EmailRecipient(email.Address);
        emailMessage.To.Add(emailRecipient);
        emailMessage.Subject = subject;
    }

    await Windows.ApplicationModel.Email.EmailManager.ShowComposeNewEmailAsync(emailMessage);
}
```

>[!NOTE]
> 通过使用[EmailAttachment](https://docs.microsoft.com/uwp/api/windows.applicationmodel.email.emailattachment)类添加到电子邮件附件将显示仅在邮件应用。 如果用户已配置为其默认邮件程序的任何其他邮件程序，撰写窗口将显示而无需附件。 这是一个已知的问题。

## <a name="summary-and-next-steps"></a>摘要和后续步骤

本主题已向你展示如何启动撰写电子邮件对话框。 有关选择用作电子邮件接收方联系人的信息，请参阅[选择联系人](selecting-contacts.md)。 请参阅 [**PickSingleFileAsync**](https://msdn.microsoft.com/library/windows/apps/JJ635275) 以选择要用作电子邮件附件的文件。

## <a name="related-topics"></a>相关主题

* [选择联系人](selecting-contacts.md)
* [如何在调用文件选取器后继续使用你的 Windows Phone 应用](https://msdn.microsoft.com/library/windows/apps/xaml/Dn614994)
 

 
