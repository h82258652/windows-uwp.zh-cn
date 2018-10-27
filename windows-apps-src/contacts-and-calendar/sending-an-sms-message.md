---
author: normesta
description: 本主题向你展示如何启动撰写短信对话框以允许用户发送短信。 你可以在显示该对话框之前，使用数据预填充短信的字段。 该消息将在用户点击发送按钮后发出。
title: 发送短信
ms.assetid: 4D7B509B-1CF0-4852-9691-E96D8352A4D6
keywords: 联系人, 短信, 发送
ms.author: normesta
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 06d84646685c6944ab0e816b42cf6fb2125f8a57
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2018
ms.locfileid: "5691669"
---
# <a name="send-an-sms-message"></a>发送短信

本主题向你展示如何启动撰写短信对话框以允许用户发送短信。 你可以在显示该对话框之前，使用数据预填充短信的字段。 该消息将在用户点击发送按钮后发出。

若要调用此代码，声明在程序包清单中的**聊天**、 **smsSend**，以及**chatSystem**功能。 这是[受限的功能](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations#special-and-restricted-capabilities)，但你可以在应用中使用它们。 仅当你打算将应用发布到应用商店，你需要批准。 请参阅[帐户类型、 位置和费用](https://docs.microsoft.com/windows/uwp/publish/account-types-locations-and-fees)。

## <a name="launch-the-compose-sms-dialog"></a>启动撰写短信对话框

创建新 [**ChatMessage**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.chat.chatmessage) 对象，并设置你要在撰写电子邮件对话框中预填充的数据。 调用 [**ShowComposeSmsMessageAsync**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.chat.chatmessagemanager.showcomposesmsmessageasync) 显示对话框。

```cs
private async void ComposeSms(Windows.ApplicationModel.Contacts.Contact recipient,
    string messageBody,
    StorageFile attachmentFile,
    string mimeType)
{
    var chatMessage = new Windows.ApplicationModel.Chat.ChatMessage();
    chatMessage.Body = messageBody;

    if (attachmentFile != null)
    {
        var stream = Windows.Storage.Streams.RandomAccessStreamReference.CreateFromFile(attachmentFile);

        var attachment = new Windows.ApplicationModel.Chat.ChatMessageAttachment(
            mimeType,
            stream);

        chatMessage.Attachments.Add(attachment);
    }

    var phone = recipient.Phones.FirstOrDefault<Windows.ApplicationModel.Contacts.ContactPhone>();
    if (phone != null)
    {
        chatMessage.Recipients.Add(phone.Number);
    }
    await Windows.ApplicationModel.Chat.ChatMessageManager.ShowComposeSmsMessageAsync(chatMessage);
}
```

你可以使用下面的代码以确定是否正在运行该应用在设备处于无法发送短信。

```csharp
if (Windows.Foundation.Metadata.ApiInformation.IsTypePresent("Windows.ApplicationModel.Chat"))
{
   // Call code here.
}
```

## <a name="summary-and-next-steps"></a>摘要和后续步骤

本主题已向你展示如何启动撰写短信对话框。 有关选择用作短信接收方的联系人的信息，请参阅[选择联系人](selecting-contacts.md)。 从 GitHub 下载[通用 Windows 应用示例](http://go.microsoft.com/fwlink/p/?linkid=619979)来查看更多有关如何使用后台任务发送和接收短信的示例。

## <a name="related-topics"></a>相关主题

* [选择联系人](selecting-contacts.md)
