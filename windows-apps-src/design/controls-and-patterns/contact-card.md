---
Description: A button gives the user a way to trigger an immediate action.
title: 联系人卡片
ms.date: 03/07/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: kele
design-contact: tbd
dev-contact: tbd
doc-status: not-published
ms.localizationpriority: medium
ms.openlocfilehash: b04a8f616e9f6c7726a222f4a7264b9580ddee3a
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2018
ms.locfileid: "8749726"
---
# <a name="contact-card"></a>联系人卡片

联系人卡片显示[联系人](//docs.microsoft.com/uwp/api/Windows.ApplicationModel.Contacts.Contact)（UWP 用来表示用户和企业的机制）的联系人信息，例如姓名、电话号码和地址。  联系人卡片还允许用户编辑联系人信息。 你可以选择显示紧凑式联系人卡片或包含更多信息的完整联系人卡片。

> **重要 API**：[ShowContactCard 方法](/uwp/api/windows.applicationmodel.contacts.contactmanager#Windows_ApplicationModel_Contacts_ContactManager_ShowFullContactCard_Windows_ApplicationModel_Contacts_Contact_Windows_Foundation_Rect_)、[ShowFullContactCard 方法](/uwp/api/windows.applicationmodel.contacts.contactmanager#Windows_ApplicationModel_Contacts_ContactManager_ShowContactCard_Windows_ApplicationModel_Contacts_Contact_Windows_ApplicationModel_Contacts_FullContactCardOptions_)、[IsShowContactCardSupported 方法](/uwp/api/windows.applicationmodel.contacts.contactmanager.IsShowContactCardSupported)、[Contact 类](//docs.microsoft.com/uwp/api/Windows.ApplicationModel.Contacts.Contact)  

有两种显示联系人卡片的方式：  
* 以标准联系人卡片方式显示在可轻型消除的浮出控件中，当用户在联系人卡片外部单击时联系人卡片消失。 
* 以完整联系人卡片方式显示，占用更多空间，并且不可轻型消除，用户必须单击**关闭**才能关闭它。 


<figure>
    <img src="images/contact-card/contact-card-standard.png" alt="The full contact card">
    <figcaption>标准联系人卡片</figcaption>
</figure>

<figure>
    <img src="images/contact-card/contact-card-full.png" alt="The full contact card">
    <figcaption>完整联系人卡片</figcaption>
</figure>


## <a name="is-this-the-right-control"></a>这是正确的控件吗？

当你想要显示联系人的联系人信息时，请使用联系人卡片。 如果只想显示联系人的姓名和头像，请使用[个人图片控件](person-picture.md)。 


<!-- TODO: Add examples back when the contact card has been added. -->

<!-- ## Examples

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>If you have the <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> app installed, click here to <a href="xamlcontrolsgallery:/item/Button">open the app and see the Button in action</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Get the XAML Controls Gallery app (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Get the source code (GitHub)</a></li>
    </ul>
</td>
</tr>
</table> -->

## <a name="show-a-standard-contact-card"></a>显示标准联系人卡片

1. 通常，显示联系人卡片是因为用户单击了某些内容，例如按钮或[个人图片控件](person-picture.md)。 我们不希望隐藏该元素。 为避免隐藏它，我们需要创建一个 [Rect](/uwp/api/windows.foundation.rect) 来描述该元素的位置和大小。 

    让我们创建一个实用工具函数来实现此功能，稍后将使用该函数。
    ```csharp
    // Gets the rectangle of the element 
    public static Rect GetElementRectHelper(FrameworkElement element) 
    { 
        // Passing "null" means set to root element. 
        GeneralTransform elementTransform = element.TransformToVisual(null); 
        Rect rect = elementTransform.TransformBounds(new Rect(0, 0, element.ActualWidth, element.ActualHeight)); 
        return rect; 
    } 

    ```

2. 通过调用 [ContactManager.IsShowContactCardSupported](/uwp/api/windows.applicationmodel.contacts.contactmanager.IsShowContactCardSupported) 方法确定你是否能显示联系人卡片。 如果不支持，会显示一条错误消息。 （此示例假设将为了响应单击事件而显示联系人卡片。）
    ```csharp
    // Contact and Contact Managers are existing classes 
    private void OnUserClickShowContactCard(object sender, RoutedEventArgs e) 
    { 
        if (ContactManager.IsShowContactCardSupported()) 
        { 

    ```

3. 使用你在步骤 1 中创建的实用工具函数获取触发该事件的控件的边界（以便我们不会用联系人卡片覆盖它）。

    ```csharp
            Rect selectionRect = GetElementRect((FrameworkElement)sender); 
    ```

4. 获取要显示的 [Contact](//docs.microsoft.com/uwp/api/Windows.ApplicationModel.Contacts.Contact) 对象。 此示例只是创建一个简单的联系人，但你的代码应检索实际联系人。 

    ```csharp
                // Retrieve the contact to display
                var contact = new Contact(); 
                var email = new ContactEmail(); 
                email.Address = "jsmith@contoso.com"; 
                contact.Emails.Add(email); 
    ```
5. 通过调用 [ShowContactCard](/uwp/api/windows.applicationmodel.contacts.contactmanager#Windows_ApplicationModel_Contacts_ContactManager_ShowFullContactCard_Windows_ApplicationModel_Contacts_Contact_Windows_Foundation_Rect_) 方法显示联系人卡片。 

    ```csharp
            ContactManager.ShowFullContactCard(
                contact, selectionRect, Placement.Default); 
        } 
    } 
    ```

下面是完整的代码示例：

```csharp
// Gets the rectangle of the element 
public static Rect GetElementRect(FrameworkElement element) 
{ 
    // Passing "null" means set to root element. 
    GeneralTransform elementTransform = element.TransformToVisual(null); 
    Rect rect = elementTransform.TransformBounds(new Rect(0, 0, element.ActualWidth, element.ActualHeight)); 
    return rect; 
} 
 
// Display a contact in response to an event
private void OnUserClickShowContactCard(object sender, RoutedEventArgs e) 
{ 
    if (ContactManager.IsShowContactCardSupported()) 
    { 
        Rect selectionRect = GetElementRect((FrameworkElement)sender);

        // Retrieve the contact to display
        var contact = new Contact(); 
        var email = new ContactEmail(); 
        email.Address = "jsmith@contoso.com"; 
        contact.Emails.Add(email); 
    
        ContactManager.ShowContactCard(
            contact, selectionRect, Placement.Default); 
    } 
} 

```

## <a name="show-a-full-contact-card"></a>显示完整联系人卡片

要显示完整联系人卡片，请调用 [ShowFullContactCard](/uwp/api/windows.applicationmodel.contacts.contactmanager#Windows_ApplicationModel_Contacts_ContactManager_ShowContactCard_Windows_ApplicationModel_Contacts_Contact_Windows_ApplicationModel_Contacts_FullContactCardOptions_) 方法而不是 [ShowContactCard](/uwp/api/windows.applicationmodel.contacts.contactmanager#Windows_ApplicationModel_Contacts_ContactManager_ShowFullContactCard_Windows_ApplicationModel_Contacts_Contact_Windows_Foundation_Rect_)。

```csharp
private void onUserClickShowContactCard() 
{ 
   
    Contact contact = new Contact(); 
    ContactEmail email = new ContactEmail(); 
    email.Address = "jsmith@hotmail.com"; 
    contact.Emails.Add(email); 
 
 
    // Setting up contact options.     
    FullContactCardOptions fullContactCardOptions = new FullContactCardOptions(); 
 
    // Display full contact card on mouse click.   
    // Launch the People’s App with full contact card  
    fullContactCardOptions.DesiredRemainingView = ViewSizePreference.UseLess; 
     
 
    // Shows the full contact card by launching the People App. 
    ContactManager.ShowFullContactCard(contact, fullContactCardOptions); 
} 

```

## <a name="retrieving-real-contacts"></a>检索“实际”联系人

本文中的示例创建一个简单联系人。 在实际应用中，你可能希望检索现有联系人。 有关说明，请参阅[联系人和日历文章](/windows/uwp/contacts-and-calendar/)。




## <a name="related-articles"></a>相关文章
- [联系人和日历](/windows/uwp/contacts-and-calendar/)
- [联系人卡片示例](http://go.microsoft.com/fwlink/p/?LinkId=624040)
- [个人图片控件](/windows/uwp/controls-and-patterns/person-picture/)
