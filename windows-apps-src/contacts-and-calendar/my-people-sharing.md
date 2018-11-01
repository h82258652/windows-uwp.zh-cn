---
title: “我的人脉”共享
description: 介绍如何为“我的人脉”共享添加支持
author: muhsinking
ms.author: mukin
ms.date: 06/28/2017
ms.topic: article
keywords: windows 10，uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7084c4dde7bdf2d59842a04fe9fd52bc029c264a
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/31/2018
ms.locfileid: "5872426"
---
# <a name="my-people-sharing"></a>“我的人脉”共享

利用“我的人脉”功能，用户可以将联系人固定到其任务栏，这样，他们可以从 Windows 中的任何地方轻松地保持联系，而与他们进行联系所使用的应用程序无关。 现在，通过将文件从文件资源管理器拖到“我的人脉”固定区域，用户可以与他们固定的联系人分享内容。 他们还可以通过标准共享超级按钮与 Windows 联系人存储中的任何联系人共享。 请继续阅读，了解如何将应用程序启用为“我的人脉”共享目标。

![“我的人脉”共享面板](images/my-people-sharing.png)

## <a name="requirements"></a>要求

+ Windows 10 和 Microsoft Visual Studio 2017。 有关安装详细信息，请参阅[设置 Visual Studio](https://docs.microsoft.com/en-us/windows/uwp/get-started/get-set-up)。
+ C# 或类似面向对象的编程语言的基础知识。 若要开始使用 C#，请参阅[创建“Hello, world”应用](https://docs.microsoft.com/en-us/windows/uwp/get-started/create-a-hello-world-app-xaml-universal)。

## <a name="overview"></a>概述

你必须执行以下三个步骤，才能将应用程序启用为“我的人脉”共享目标：

1. [声明对应用程序清单中的 shareTarget 激活合约提供支持。](https://docs.microsoft.com/en-us/windows/uwp/contacts-and-calendar/my-people-sharing#declaring-support-for-the-share-contract)
2. [为用户可与之共用应用的联系人添加注释。](https://docs.microsoft.com/en-us/windows/uwp/contacts-and-calendar/my-people-sharing#annotating-contacts)
3. 支持同时运行应用程序的多个实例。  用户必须能够与完整版本的应用程序进行交互，并且同时也可以使用应用程序与其他人共享。 他们可以同时在多个共享窗口中使用该应用程序。 为了对此提供支持，应用程序需要能够同时运行多个视图。 若要了解如何执行此操作，请参阅文章[“显示应用的多个视图”](https://docs.microsoft.com/en-us/windows/uwp/layout/show-multiple-views)。

当你完成此操作后，应用程序将在“我的人脉”共享窗口中显示为共享目标，可以通过以下两种方式启动共享目标：
1. 通过共享超级按钮选择联系人。
2. 将文件拖放到固定到任务栏上的联系人。

## <a name="declaring-support-for-the-share-contract"></a>声明对“共享”合约提供支持

若要声明支持以应用程序作为共享目标，请首先在 Visual Studio 中打开应用程序。 从**解决方案资源管理器**中，右键单击 **package.appxmanifest** 并选择**打开方式**。 从菜单中，选择 **XML (文本)编辑器**，然后单击**确定**。 然后，对清单进行以下更改：


**之前**
```xml
<Applications>
    <Application Id="MyApp"
      Executable="$targetnametoken$.exe"
      EntryPoint="My.App">
    </Application>
</Applications>
```

**之后**

```xml
<Applications>
    <Application Id="MyApp"
      Executable="$targetnametoken$.exe"
      EntryPoint="My.App">
        <Extensions>
            <uap:Extension Category="windows.shareTarget">
                <uap:ShareTarget Description="Share with MyApp">
                    <uap:SupportedFileTypes>
                        <uap:SupportsAnyFileType/>
                    </uap:SupportedFileTypes>
                    <uap:DataFormat>Text</uap:DataFormat>
                    <uap:DataFormat>Bitmap</uap:DataFormat>
                    <uap:DataFormat>Html</uap:DataFormat>
                    <uap:DataFormat>StorageItems</uap:DataFormat>
                    <uap:DataFormat>URI</uap:DataFormat>
                </uap:ShareTarget>
            </uap:Extension>
         </Extensions>
    </Application>
</Applications>
```

此代码将添加对所有文件和数据格式的支持，但是你可以选择指定所支持的文件类型和数据格式（请参阅 [ShareTarget 类文档](https://docs.microsoft.com/en-us/uwp/schemas/appxpackage/appxmanifestschema/element-sharetarget)了解更多详细信息）。

## <a name="annotating-contacts"></a>为联系人添加注释

若要允许“我的人脉”共享窗口将应用程序显示为联系人的共享目标，你需要将联系人写入 Windows 联系人存储中。 若要了解如何编写联系人，请参阅[联系人卡片集成示例](https://github.com/Microsoft/Windows-universal-samples/tree/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/ContactCardIntegration)。 

为了在与联系人共享时使你的应用程序显示为“我的人脉”共享目标，应用程序必须为该联系人编写注释。 注释是应用程序中与联系人相关联的一些数据。 注释必须包含与 **ProviderProperties** 成员中所需视图相对应的可激活的类，并且声明对 **Share** 操作提供支持。

你可以在应用运行过程中随时为联系人添加注释，但通常，在将联系人添加到 Windows 联系人存储后，你应该立即为联系人添加注释。

```Csharp
if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 5))
{
    // Create a new contact annotation
    ContactAnnotation annotation = new ContactAnnotation();
    annotation.ContactId = myContact.Id;

    // Add appId and Share support to the annotation
    String appId = "MyApp_vqvv5s4y3scbg!App";
    annotation.ProviderProperties.Add("ContactShareAppID", appId);
    annotation.SupportedOperations = ContactAnnotationOperations::Share;

    // Save annotation to contact annotation list
    // Windows.ApplicationModel.Contacts.ContactAnnotationList 
    await contactAnnotationList.TrySaveAnnotationAsync(annotation);
}
```

“appId”是后跟“!” 和可激活的类 ID 的包系列名称。 若要查找包系列名称，打开**Package.appxmanifest**使用默认编辑器中，并在"打包"选项卡中查找。在这里，"应用"是与共享目标视图相对应的可激活类。

## <a name="running-as-a-my-people-share-target"></a>作为“我的人脉”共享目标运行

最后，若要运行该应用，请替代应用主类中的 [OnShareTargetActivated](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Application#Windows_UI_Xaml_Application_OnShareTargetActivated_Windows_ApplicationModel_Activation_ShareTargetActivatedEventArgs_) 方法，以处理共享目标激活。 [ShareTargetActivatedEventArgs.ShareOperation.Contacts](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.datatransfer.sharetarget.shareoperation#Properties) 属性将包含与之共享的联系人，如果这是标准共享操作（不是“我的人脉”共享），则将为空。

```Csharp
protected override void OnShareTargetActivated(ShareTargetActivatedEventArgs args)
{
    bool isPeopleShare = false;
    if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 5))
    {
        // Make sure the current OS version includes the My People feature before
        // accessing the ShareOperation.Contacts property
        isPeopleShare = (args.ShareOperation.Contacts.Count > 0);
    }

    if (isPeopleShare)
    {
        // Show share UI for MyPeople contact(s)
    }
    else
    {
        // Show standard share UI for unpinned contacts
    }
}
```

## <a name="see-also"></a>另请参阅
+ [添加“我的人脉”支持](my-people-support.md)
+ [ShareTarget 类](https://docs.microsoft.com/en-us/uwp/schemas/appxpackage/appxmanifestschema/element-sharetarget)
+ [联系人卡片集成示例](https://github.com/Microsoft/Windows-universal-samples/tree/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/ContactCardIntegration)
