---
author: Xansky
description: "显示如何启动撰写电子邮件对话框以允许用户发送电子邮件。 你可以在显示该对话框之前，使用数据预填充电子邮件的字段。 该消息将在用户点击发送按钮后发出。"
title: "发送电子邮件"
ms.assetid: 74511E90-9438-430E-B2DE-24E196A111E5
keywords: "联系人, 电子邮件, 发送"
translationtype: Human Translation
ms.sourcegitcommit: 252e144b2436f047f7b0849bb6e5aee87b2e3464
ms.openlocfilehash: ff09393af072eb8aee8c3001e7323cc20201da70

---

# 发送电子邮件

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


显示如何启动撰写电子邮件对话框以允许用户发送电子邮件。 你可以在显示该对话框之前，使用数据预填充电子邮件的字段。 该消息将在用户点击发送按钮后发出。

**本文内容**

-   [启动撰写电子邮件对话框](#launch-the-compose-email-dialog)
-   [摘要和后续步骤](#summary-and-next-steps)
-   [相关主题](#related-topics)

## 启动撰写电子邮件对话框

创建新 [**EmailMessage**](https://msdn.microsoft.com/library/windows/apps/Dn631270) 对象，并设置你要在撰写电子邮件对话框中预填充的数据。 调用 [**ShowComposeNewEmailAsync**](https://msdn.microsoft.com/library/windows/apps/Dn631269) 显示对话框。

``` cs
private async Task ComposeEmail(Windows.ApplicationModel.Contacts.Contact recipient, 
    string messageBody, 
    StorageFile attachmentFile)
{
    var emailMessage = new Windows.ApplicationModel.Email.EmailMessage();
    emailMessage.Body = messageBody;

    if (attachmentFile != null)
    {
        var stream = Windows.Storage.Streams.RandomAccessStreamReference.CreateFromFile(attachmentFile);

        var attachment = new Windows.ApplicationModel.Email.EmailAttachment(
            attachmentFile.Name,
            stream);

        emailMessage.Attachments.Add(attachment);
    }

    var email = recipient.Emails.FirstOrDefault<Windows.ApplicationModel.Contacts.ContactEmail>();
    if (email != null)
    {
        var emailRecipient = new Windows.ApplicationModel.Email.EmailRecipient(email.Address);
        emailMessage.To.Add(emailRecipient);
    }

    await Windows.ApplicationModel.Email.EmailManager.ShowComposeNewEmailAsync(emailMessage);
        
}
```

## 摘要和后续步骤

本主题已向你展示如何启动撰写电子邮件对话框。 有关选择用作电子邮件接收方联系人的信息，请参阅[选择联系人](selecting-contacts.md)。 请参阅 [**PickSingleFileAsync**](https://msdn.microsoft.com/library/windows/apps/JJ635275) 以选择要用作电子邮件附件的文件。

## 相关主题

* [选择联系人](selecting-contacts.md)
* [如何在调用文件选取器后继续使用你的 Windows Phone 应用](https://msdn.microsoft.com/library/windows/apps/xaml/Dn614994)
 

 







<!--HONumber=Aug16_HO3-->


