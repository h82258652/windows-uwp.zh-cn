---
description: Windows.ApplicationModel.Contacts 命名空间提供了多个用来选择联系人的选项。
title: 选择联系人
ms.assetid: 35FEDEE6-2B0E-4391-84BA-5E9191D4E442
关键字：联系人, 选择
关键字：选择单个联系人
关键字：选择多个联系人
关键字：联系人, 选择多个
关键字：选择特定的联系人数据
关键字：联系人, 选择特定的数据
关键字：联系人, 选择特定的字段
---

# 选择联系人

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 的文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


[
            **Windows.ApplicationModel.Contacts**](https://msdn.microsoft.com/library/windows/apps/BR225002) 命名空间提供了多个用来选择联系人的选项。 下面，我们将向你介绍如何选择一个联系人或多个联系人，并且还介绍如何将联系人选取器配置为仅检索应用所需的联系人信息。

## 设置联系人选取器

创建一个 [**Windows.ApplicationModel.Contacts.ContactPicker**](https://msdn.microsoft.com/library/windows/apps/BR224913) 实例并将它分配给某个变量。

```cs
var contactPicker = new Windows.ApplicationModel.Contacts.ContactPicker();
```

## 设置选择模式（可选）

默认情况下，联系人选取器检索用户选择的联系人的所有可用数据。 [
            **SelectionMode**](https://msdn.microsoft.com/library/windows/apps/BR224913-selectionmode) 属性允许你将联系人选取器配置为仅检索你的应用所需的数据字段。 如果你只需要可用联系人数据的一个子集，那么更有效的方法是使用联系人选取器。

首先，将 [**SelectionMode**](https://msdn.microsoft.com/library/windows/apps/BR224913-selectionmode) 属性设置为 **Fields**：

```cs
contactPicker.SelectionMode = Windows.ApplicationModel.Contacts.ContactSelectionMode.Fields;
```

然后，使用 [**desiredFieldsWithContactFieldType**](https://msdn.microsoft.com/library/windows/apps/BR224913-desiredfieldswithcontactfieldtype) 属性指定你希望联系人选取器检索的字段。 该示例将联系人选取器配置为检索电子邮件地址：

``` cs
contactPicker.DesiredFieldsWithContactFieldType.Add(Windows.ApplicationModel.Contacts.ContactFieldType.Email);
```

## 启动选取器

```cs
Contact contact = await contactPicker.PickContactAsync();
```

如果你希望用户选择一个或多个联系人，请使用 [**pickContactsAsync**](https://msdn.microsoft.com/library/windows/apps/BR224913-pickcontactsasync)。

```cs
public IList&lt;Contact&gt; contacts;
contacts = await contactPicker.PickContactsAsync();
```

## 处理联系人

当选取器返回时，检查用户是否选择了任何联系人。 如果选择了联系人，则处理联系人信息。

此示例显示如何处理一个联系人。 下面我们检索联系人的姓名并将该姓名复制到称为 *OutputName* 的 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652) 控件中。

```cs
if (contact != null)
{
    OutputName.Text = contact.DisplayName;
}
else
{
    rootPage.NotifyUser(&quot;No contact was selected.&quot;, NotifyType.ErrorMessage);
}
```

该示例显示如何处理多个联系人。

```cs
if (contacts != null &amp;&amp; contacts.Count &gt; 0)
{
    foreach (Contact contact in contacts)
    {
        // Do something with the contact information.
    }
}
```

## 完整示例（一个联系人）

该示例使用联系人选取器检索一个联系人的姓名以及电子邮件地址、位置或电话号码。

```cs
// ...
using Windows.ApplicationModel.Contacts;
// ...

private async void PickAContactButton-Click(object sender, RoutedEventArgs e)
{
    ContactPicker contactPicker = new ContactPicker();

    contactPicker.DesiredFieldsWithContactFieldType.Add(ContactFieldType.Email);
    contactPicker.DesiredFieldsWithContactFieldType.Add(ContactFieldType.Address);
    contactPicker.DesiredFieldsWithContactFieldType.Add(ContactFieldType.PhoneNumber);

    Contact contact = await contactPicker.PickContactAsync();

    if (contact != null)
    {
        OutputFields.Visibility = Visibility.Visible;
        OutputEmpty.Visibility = Visibility.Collapsed;

        OutputName.Text = contact.DisplayName;

        AppendContactFieldValues(OutputEmails, contact.Emails);
        AppendContactFieldValues(OutputPhoneNumbers, contact.Phones);
        AppendContactFieldValues(OutputAddresses, contact.Addresses);
    }
    else
    {
        OutputEmpty.Visibility = Visibility.Visible;
        OutputFields.Visibility = Visibility.Collapsed;
    }
}

private void AppendContactFieldValues&lt;T&gt;(TextBlock content, IList&lt;T&gt; fields)
{
    if (fields.Count &gt; 0)
    {
        StringBuilder output = new StringBuilder();

        if (fields[0].GetType() == typeof(ContactEmail))
        {
            foreach (ContactEmail email in fields as IList&lt;ContactEmail&gt;)
            {
                output.AppendFormat(&quot;Email: {0} ({1})\n&quot;, email.Address, email.Kind);
            }
        }
        else if (fields[0].GetType() == typeof(ContactPhone))
        {
            foreach (ContactPhone phone in fields as IList&lt;ContactPhone&gt;)
            {
                output.AppendFormat(&quot;Phone: {0} ({1})\n&quot;, phone.Number, phone.Kind);
            }
        }
        else if (fields[0].GetType() == typeof(ContactAddress))
        {
            List&lt;String&gt; addressParts = null;
            string unstructuredAddress = &quot;&quot;;

            foreach (ContactAddress address in fields as IList&lt;ContactAddress&gt;)
            {
                addressParts = (new List&lt;string&gt; { address.StreetAddress, address.Locality, address.Region, address.PostalCode });
                unstructuredAddress = string.Join(&quot;, &quot;, addressParts.FindAll(s =&gt; !string.IsNullOrEmpty(s)));
                output.AppendFormat(&quot;Address: {0} ({1})\n&quot;, unstructuredAddress, address.Kind);
            }
        }

        content.Visibility = Visibility.Visible;
        content.Text = output.ToString();
    }
    else
    {
        content.Visibility = Visibility.Collapsed;
    }
}
```

## 完整示例（多个联系人）

该示例使用联系人选取器检索多个联系人，然后将联系人添加到名为 `OutputContacts` 的 [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) 控件中。

```cs
MainPage rootPage = MainPage.Current;
public IList&lt;Contact&gt; contacts;

private async void PickContactsButton-Click(object sender, RoutedEventArgs e)
{
    var contactPicker = new Windows.ApplicationModel.Contacts.ContactPicker();
    contactPicker.CommitButtonText = &quot;Select&quot;;
    contacts = await contactPicker.PickContactsAsync();

    // Clear the ListView.
    OutputContacts.Items.Clear();

    if (contacts.Count &gt; 0)
    {
        OutputContacts.Visibility = Windows.UI.Xaml.Visibility.Visible;
        OutputEmpty.Visibility = Visibility.Collapsed;

        foreach (Contact contact in contacts)
        {
            // Add the contacts to the ListView.
            OutputContacts.Items.Add(new ContactItemAdapter(contact));
        }
    }
    else
    {
        OutputEmpty.Visibility = Visibility.Visible;
    }         
}
```

``` cs
public class ContactItemAdapter
{
    public string Name { get; private set; }
    public string SecondaryText { get; private set; }

    public ContactItemAdapter(Contact contact)
    {
        Name = contact.DisplayName;
        if (contact.Emails.Count &gt; 0)
        {
            SecondaryText = contact.Emails[0].Address;
        }
        else if (contact.Phones.Count &gt; 0)
        {
            SecondaryText = contact.Phones[0].Number;
        }
        else if (contact.Addresses.Count &gt; 0)
        {
            List&lt;string&gt; addressParts = (new List&lt;string&gt; { contact.Addresses[0].StreetAddress, 
              contact.Addresses[0].Locality, contact.Addresses[0].Region, contact.Addresses[0].PostalCode });
              string unstructuredAddress = string.Join(&quot;, &quot;, addressParts.FindAll(s =&gt; !string.IsNullOrEmpty(s)));
            SecondaryText = unstructuredAddress;
        }
    }
}
```

## 摘要和后续步骤

现在，你基本了解了如何使用联系人选取器检索联系人信息。 从 GitHub 下载[通用 Windows 应用示例](http://go.microsoft.com/fwlink/p/?linkid=619979)来查看更多有关如何使用联系人和联系人选取器的示例。



<!--HONumber=Mar16_HO1-->


