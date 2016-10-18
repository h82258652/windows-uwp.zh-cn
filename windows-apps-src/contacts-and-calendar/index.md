---
author: Xansky
description: "如何在 UWP 应用中使用联系人和日历信息。"
title: "联系人和日历"
ms.assetid: b7e53ab5-2828-4fb7-8656-2bec70b3467f
translationtype: Human Translation
ms.sourcegitcommit: 5c0f6ef1f1a346a66ca554a415d9f24c8a314ae1
ms.openlocfilehash: da73790ca9aec3fa16295eac4880c7b80db033ab

---

# 联系人和日历

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

你可以让你的用户访问其联系人和约会，以便他们可以彼此共享内容、电子邮件、日历信息或消息，或共享你设计的任何功能。

若要查看你的应用访问联系人和约会的几种不同方法，请参阅以下主题：

| 主题 | 说明 |
|-------|-------------|
| [选择联系人](selecting-contacts.md) | [<strong>Windows.ApplicationModel.Contacts</strong>](https://msdn.microsoft.com/library/windows/apps/BR225002) 命名空间提供了多个用于选择联系人的选项。 下面，我们将向你介绍如何选择一个联系人或多个联系人，并且还介绍如何将联系人选取器配置为仅检索应用所需的联系人信息。 |
| [发送电子邮件](sending-email.md) | 显示如何启动撰写电子邮件对话框以允许用户发送电子邮件。 你可以在显示该对话框之前，使用数据预填充电子邮件的字段。 该消息将在用户点击发送按钮后发出。 |
| [发送短信](sending-an-sms-message.md) | 本主题向你展示如何启动撰写短信对话框以允许用户发送短信。 你可以在显示该对话框之前，使用数据预填充短信的字段。 该消息将在用户点击发送按钮后发出。 |
| [管理约会](managing-appointments.md) | 通过 [<strong>Windows.ApplicationModel.Appointments</strong>](https://msdn.microsoft.com/library/windows/apps/Dn263359) 命名空间，你可以在用户的日历应用中创建和管理约会。 我们将在此处向你介绍如何创建约会、将其添加到日历应用、在日历应用中替换它以及从日历应用中删除它。 我们还将介绍如何显示日历应用的时间跨度以及创建一个约会循环对象。 |
| [将你的应用与联系人卡片上的操作关联起来](integrating-with-contacts.md) | 介绍如何使应用显示在联系人卡片或微型联系人卡片上的操作旁边。 用户可以选择你的应用来执行某项操作，如打开个人资料页面、打电话或发送消息。 |
| [向“人脉”应用提供社交源](integrating-social-feeds-into-contact-cards.md) | 将社交源数据从数据库集成到“人脉”应用。 你的源数据将显示在“人脉”应用的“最近更新”<strong></strong>或联系人的“个人资料”<strong></strong>页面中。 |

 

## 相关主题

* [约会 API 示例](http://go.microsoft.com/fwlink/p/?linkid=309836)
* [联系人管理器 API 示例](http://go.microsoft.com/fwlink/p/?LinkID=310079)
* [联系人选取器应用示例](http://go.microsoft.com/fwlink/p/?linkid=231575)
* [处理联系人操作示例](http://go.microsoft.com/fwlink/p/?LinkID=320151)
* [联系人卡片集成示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ContactCardIntegration)
* [社交信息示例](https://github.com/Microsoft/Windows-Social-Samples/tree/master/SocialInfoSampleApp)



<!--HONumber=Aug16_HO5-->


