---
author: Xansky
description: "本主题向你展示如何启动撰写短信对话框以允许用户发送短信。 你可以在显示该对话框之前，使用数据预填充短信的字段。 该消息将在用户点击发送按钮后发出。"
title: "发送短信"
ms.assetid: 4D7B509B-1CF0-4852-9691-E96D8352A4D6
keywords: "联系人, 短信, 发送"
translationtype: Human Translation
ms.sourcegitcommit: 3de603aec1dd4d4e716acbbb3daa52a306dfa403
ms.openlocfilehash: e00d2e9a701a2a23b5a98f2275abd55da12fa791

---

# 发送短信

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


本主题向你展示如何启动撰写短信对话框以允许用户发送短信。 你可以在显示该对话框之前，使用数据预填充短信的字段。 消息在用户点击“发送”按钮之前将不会发送。

## 启动撰写短信对话框

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

## 摘要和后续步骤

本主题已向你展示如何启动撰写短信对话框。 有关选择用作短信接收方的联系人的信息，请参阅[选择联系人](selecting-contacts.md)。 从 GitHub 下载[通用 Windows 应用示例](http://go.microsoft.com/fwlink/p/?linkid=619979)来查看更多有关如何使用后台任务发送和接收短信的示例。

## 相关主题

* [选择联系人](selecting-contacts.md)



<!--HONumber=Aug16_HO3-->


