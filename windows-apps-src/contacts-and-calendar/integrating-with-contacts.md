---
author: normesta
description: 显示如何将你的应用添加到联系人卡片中的操作旁边
MSHAttr: PreferredLib:/library/windows/apps
title: 将你的应用与联系人卡片上的操作关联起来
ms.author: normesta
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 联系人, 联系人卡片, 注释
ms.assetid: 0edabd9c-ecfb-4525-bc38-53f219d744ff
ms.localizationpriority: medium
ms.openlocfilehash: eb1c01a4fe370f899da185dc39b7d3abe6a1904e
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2018
ms.locfileid: "5840782"
---
# <a name="connect-your-app-to-actions-on-a-contact-card"></a>将你的应用与联系人卡片上的操作关联起来

你的应用可以显示在联系人卡片或微型联系人卡片上的操作旁边。 用户可以选择你的应用来执行某项操作，如打开个人资料页面、打电话或发送消息。

![联系人卡片和微型联系人卡片](images/all-contact-cards.png)

若要开始，请查找现有联系人或创建新的联系人。 接下来，创建*批注*和一些程序包清单条目来描述应用支持哪些操作。 然后，编写用于执行这些操作的代码。

有关更完整的示例，请参阅[联系人卡片集成示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ContactCardIntegration)。

## <a name="find-or-create-a-contact"></a>查找或创建联系人

如果你的应用帮助用户联系其他人，请在 Windows 中搜索联系人，然后为他们添加批注。 如果你的应用管理联系人，可以将他们添加到 Windows 联系人列表，然后为他们添加批注。

### <a name="find-a-contact"></a>查找联系人

使用姓名、电子邮件地址或电话号码来查找联系人。

```cs
ContactStore contactStore = await ContactManager.RequestStoreAsync();

IReadOnlyList<Contact> contacts = null;

contacts = await contactStore.FindContactsAsync(emailAddress);

Contact contact = contacts[0];
```

### <a name="create-a-contact"></a>创建联系人

如果你的应用更类似于通讯簿，请创建联系人，然后将他们添加到联系人列表。

```cs
Contact contact = new Contact();
contact.FirstName = "TestContact";

ContactEmail email = new ContactEmail();
email.Address = "TestContact@contoso.com";
email.Kind = ContactEmailKind.Other;
contact.Emails.Add(email);

ContactPhone phone = new ContactPhone();
phone.Number = "4255550101";
phone.Kind = ContactPhoneKind.Mobile;
contact.Phones.Add(phone);

ContactStore store = await
    ContactManager.RequestStoreAsync(ContactStoreAccessType.AppContactsReadWrite);

ContactList contactList;

IReadOnlyList<ContactList> contactLists = await store.FindContactListsAsync();

if (0 == contactLists.Count)
    contactList = await store.CreateContactListAsync("TestContactList");
else
    contactList = contactLists[0];

await contactList.SaveContactAsync(contact);

```

## <a name="tag-each-contact-with-an-annotation"></a>使用批注标记每个联系人

使用你的应用可执行的操作（例如，视频通话和消息传送）列表来标记每个联系人。

然后，将联系人的 ID 与你的应用在内部用于标识该用户的 ID 关联起来。

```cs
ContactAnnotationStore annotationStore = await
   ContactManager.RequestAnnotationStoreAsync(ContactAnnotationStoreAccessType.AppAnnotationsReadWrite);

ContactAnnotationList annotationList;

IReadOnlyList<ContactAnnotationList> annotationLists = await annotationStore.FindAnnotationListsAsync();
if (0 == annotationLists.Count)
    annotationList = await annotationStore.CreateAnnotationListAsync();
else
    annotationList = annotationLists[0];

ContactAnnotation annotation = new ContactAnnotation();
annotation.ContactId = contact.Id;
annotation.RemoteId = "user22";

annotation.SupportedOperations = ContactAnnotationOperations.Message |
  ContactAnnotationOperations.AudioCall |
  ContactAnnotationOperations.VideoCall |
 ContactAnnotationOperations.ContactProfile;

await annotationList.TrySaveAnnotationAsync(annotation);
```

## <a name="register-for-each-operation"></a>为每个操作注册

在程序包清单中，为你在批注中列出的每个操作注册。

通过将协议处理程序添加到清单的 ``Extensions`` 元素来注册。

```xml
<Extensions>
  <uap:Extension Category="windows.protocol">
    <uap:Protocol Name="ms-contact-profile">
      <uap:DisplayName>TestProfileApp</uap:DisplayName>
    </uap:Protocol>
  </uap:Extension>
  <uap:Extension Category="windows.protocol">
    <uap:Protocol Name="ms-ipmessaging">
      <uap:DisplayName>TestMsgApp</uap:DisplayName>
    </uap:Protocol>
  </uap:Extension>
  <uap:Extension Category="windows.protocol">
    <uap:Protocol Name="ms-voip-video">
      <uap:DisplayName>TestVideoApp</uap:DisplayName>
    </uap:Protocol>
  </uap:Extension>
  <uap:Extension Category="windows.protocol">
    <uap:Protocol Name="ms-voip-call">
      <uap:DisplayName>TestCallApp</uap:DisplayName>
    </uap:Protocol>
  </uap:Extension>
</Extensions>
```
你还可以在 Visual Studio 中的清单设计器的“声明”**** 选项卡中添加这些协议处理程序。

![清单设计器的“声明”选项卡](images/manifest-designer-protocols.png)

## <a name="find-your-app-next-to-actions-in-a-contact-card"></a>在联系人卡片中的操作旁边找到你的应用

打开“人脉”应用。 你的应用显示在你在批注和程序包清单中指定的每个操作旁边。

![联系人卡片](images/a-contact-card.png)

如果用户为某个操作选择你的应用，当用户下次打开联系人卡片时，它将显示为该操作的默认应用。

## <a name="find-your-app-next-to-actions-in-a-mini-contact-card"></a>在微型联系人卡片中的操作旁边找到你的应用

在微型联系人卡片中，你的应用显示在表示操作的选项卡中。

![微型联系人卡片](images/mini-contact-card.png)

“邮件”**** 应用等应用会打开微型联系人卡片。 你的应用也可以打开它们。 此代码演示如何执行该操作。

```cs
public async void OpenContactCard(object sender, RoutedEventArgs e)
{
    // Get the selection rect of the button pressed to show contact card.
    FrameworkElement element = (FrameworkElement)sender;

    Windows.UI.Xaml.Media.GeneralTransform buttonTransform = element.TransformToVisual(null);
    Windows.Foundation.Point point = buttonTransform.TransformPoint(new Windows.Foundation.Point());
    Windows.Foundation.Rect rect =
        new Windows.Foundation.Rect(point, new Windows.Foundation.Size(element.ActualWidth, element.ActualHeight));

   // helper method to find a contact just for illustrative purposes.
    Contact contact = await findContact("contoso@contoso.com");

    ContactManager.ShowContactCard(contact, rect, Windows.UI.Popups.Placement.Default);

}
```

若要查看更多具有微型联系人卡片的示例，请参阅[联系人卡片示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ContactCards)。

和联系人卡片一样，每个选项卡都会记住用户上次使用的应用，因此用户可以轻松地返回到你的应用。

## <a name="perform-operations-when-users-select-your-app-in-a-contact-card"></a>当用户在联系人卡片中选择你的应用时执行操作

在 **App.cs** 文件中替代 [Application.OnActivated](https://msdn.microsoft.com/library/windows/apps/br242330) 方法，并将用户导航到你的应用中的页面。 [联系人卡片集成示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ContactCardIntegration)演示了执行该操作的一种方法。

在页面的代码隐藏文件中，替代 [Page.OnNavigatedTo](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.onnavigatedto.aspx) 方法。 联系人卡片向此方法传递操作名称和用户 ID。

若要启动视频或音频通话，请参阅此示例：[VoIP 示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/VoIP)。 你将在 [WIndows.ApplicationModel.Calls](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.calls.aspx) 命名空间中找到完整的 API。

若要促进消息传送，请参阅 [Windows.ApplicationModel.Chat](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.chat.aspx) 命名空间。

你还可以启动另一个应用。 这是此代码的作用。

```cs
protected override async void OnNavigatedTo(NavigationEventArgs e)
{
    base.OnNavigatedTo(e);

    var args = e.Parameter as ProtocolActivatedEventArgs;
    // Display the result of the protocol activation if we got here as a result of being activated for a protocol.

    if (args != null)
    {
        var options = new Windows.System.LauncherOptions();
        options.DisplayApplicationPicker = true;

        options.TargetApplicationPackageFamilyName = “ContosoApp”;

        string launchString = args.uri.Scheme + ":" + args.uri.Query;
        var launchUri = new Uri(launchString);
        await Windows.System.Launcher.LaunchUriAsync(launchUri, options);
    }
}
```

```args.uri.scheme``` 属性包含操作名称，而 ```args.uri.Query``` 属性包含用户 ID。
