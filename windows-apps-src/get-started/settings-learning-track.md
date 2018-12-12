---
title: 在 UWP 应用中保存和加载设置
description: 了解如何在通用 Windows 平台应用中保存和加载应用设置。
ms.date: 05/07/2018
ms.topic: article
keywords: 入门, uwp, windows 10, 学习轨迹, 设置, 保存设置, 加载设置
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 4aa56bf24d2dfa1fd4ae1947a5b0edf7f312ea2f
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2018
ms.locfileid: "8931895"
---
# <a name="save-and-load-settings-in-a-uwp-app"></a>在 UWP 应用中保存和加载设置

本主题介绍开始在通用 Windows 平台 (UWP) 应用中加载和保存设置需要了解的内容。 介绍了主要 API，并提供了可帮助你了解详细信息的链接。

使用设置可以记住应用中用户可自定义的方面。 例如，新闻阅读器可以使用应用设置来保存要显示的新闻源以及用于阅读文章的字体。

我们将了解一下用来保存和加载应用设置的代码，包括本地和漫游设置。

## <a name="what-do-you-need-to-know"></a>需要了解哪些内容

使用应用设置存储配置数据，如用户首选项和应用状态。  设备特定的设置存储在本地。 在你安装应用的设备上应用的设置存储在漫游数据存储中。 设置在用户使用同一个 Microsoft 帐户登录并安装了相同应用版本的设备之间漫游。

设置可以使用以下数据类型：整数、加倍、浮点、字符、字符串、点、DateTimes，等等。 你还可以存储 [ApplicationDataCompositeValue](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationDataCompositeValue) 类的实例，当存在应被视为单元的多个设置时这很有用。 例如，用于在应用的阅读面板中显示文本的字体名称和点大小应该作为单个单元保存/还原。 当由于在一个设置之前漫游另一个设置而出现延时，这将防止一个设置与另一个设置不同步。

下面是你需要了解的用于保存或加载应用设置的主要 API：

- [Windows.Storage.ApplicationData.Current.LocalSettings](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationData#Windows_Storage_ApplicationData_LocalSettings) 从本地应用数据存储获取应用程序设置容器。 在此处存储由于表示特定于此设备的状态或太长而不适合在设备之间漫游的设置。
- [Windows.Storage.ApplicationData.Current.RoamingSettings](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingsettings#Windows_Storage_ApplicationData_RoamingSettings) 从漫游应用数据存储获取应用程序设置容器。 此数据在设备之间漫游。
- [Windows.Storage.ApplicationDataContainer](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacontainer) 是将应用设置表示为键/值对的容器。 使用此类创建和检索设置值。
- [Windows.Storage.ApplicationDataCompositeValue](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationDataCompositeValue) 表示应作为单元序列化的多个应用设置。 当一个设置不应独立于其他设置单独更新时，这非常有用。

## <a name="save-app-settings"></a>保存应用设置

在此说明中，我们将着重介绍两个简单的场景：本地保存和加载应用设置，在设备之间漫游组合字体/字体大小设置。

 ```csharp
// Save a setting locally on the device
ApplicationDataContainer localSettings = Windows.Storage.ApplicationData.Current.LocalSettings;
localSettings.Values["test setting"] = "a device specific setting";

// Save a composite setting that will be roamed between devices
ApplicationDataContainer roamingSettings = Windows.Storage.ApplicationData.Current.RoamingSettings;
Windows.Storage.ApplicationDataCompositeValue composite = new Windows.Storage.ApplicationDataCompositeValue();
composite["Font"] = "Calibri";
composite["FontSize"] = 11;
roamingSettings.Values["RoamingFontInfo"] = composite;
 ```

将设置保存到本地设备：通过使用  `Windows.Storage.ApplicationData.Current.LocalSettings` 首先为本地设置数据存储获取 **ApplicationDataContainer**。 你分配到此实例的键/值字典对在本地设备设置数据存储中保存。

使用类似的模式保存漫游设置。 首先通过 `Windows.Storage.ApplicationData.Current.RoamingSettings` 为漫游设置数据存储获取 **ApplicationDataContainer**。 然后将键/值对分配到此实例。  这些键/值对将在设备之间自动漫游。

在上述代码段中，**ApplicationDataCompositeValue** 存储多个键/值对。 当你有多个不应彼此脱离同步的设置时，复合值非常有用。 当你保存 **ApplicationDataCompositeValue** 时，值作为一个单元保存和加载或自动完成。 通过这种方式，相关的设置不会脱离同步，因为它们作为一个单元而不是单独漫游。

## <a name="load-app-settings"></a>加载应用设置

```csharp
// load a setting that is local to the device
ApplicationDataContainer localSettings = Windows.Storage.ApplicationData.Current.LocalSettings;
String localValue = localSettings.Values["test setting"] as string;

// load a composite setting that roams between devices
ApplicationDataContainer roamingSettings = Windows.Storage.ApplicationData.Current.RoamingSettings;
Windows.Storage.ApplicationDataCompositeValue composite = (ApplicationDataCompositeValue)roamingSettings.Values["RoamingFontInfo"];
if (composite != null)
{
    String fontName = composite["Font"] as string;
    int fontSize = (int)composite["FontSize"];
}
```

从本地设备加载设置：通过使用  `Windows.Storage.ApplicationData.Current.LocalSettings` 首先为本地设置数据存储获取 **ApplicationDataContainer** 实例。 然后使用它来检索键/值对。

按照类似的模式加载漫游设置。 首先通过 `Windows.Storage.ApplicationData.Current.RoamingSettings` 从漫游设置数据存储获取 **ApplicationDataContainer** 实例。 从该实例访问键/值对。 如果数据尚未漫游到你从其访问设置的设备，你将获得一个空 **ApplicationDataContainer**。 这就是上面的示例代码中有一个 `if (composite != null)` 检查的原因。

## <a name="useful-apis-and-docs"></a>有用的 API 和文档

以下是 API 和其他有用文档的核心摘要，可帮助你开始保存和加载应用设置。

### <a name="useful-apis"></a>有用的 API

| API | 描述 |
|------|---------------|
| [ApplicationData.LocalSettings](https://msdn.microsoft.com/library/windows/apps/windows.storage.applicationdata.temporaryfolder) | 从本地应用数据存储获取应用程序设置容器。 |
| [ApplicationData.RoamingSettings](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingsettings) | 从漫游应用数据存储获取应用程序设置容器。 |
| [ApplicationDataContainer](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacontainer) | 支持创建、删除、枚举和遍历容器层次结构的应用设置的容器。 |
| [Windows.UI.ApplicationSettings 命名空间](https://docs.microsoft.com/uwp/api/windows.ui.applicationsettings) | 提供你用于定义显示在 Windows shell 的设置窗格中的应用设置的类。 |

### <a name="useful-docs"></a>有用的文档

| 主题 | 描述 |
|-------|----------------|
| [应用设置指南](https://docs.microsoft.com/windows/uwp/design/app-settings/guidelines-for-app-settings) | 介绍有关创建和显示应用设置的最佳做法。 |
| [存储和检索设置以及其他应用数据](https://docs.microsoft.com/windows/uwp/design/app-settings/store-and-retrieve-app-data#create-and-read-a-local-file) | 保存和检索设置（包括漫游设置）演练。 |

## <a name="useful-code-samples"></a>有用的代码示例

| 代码示例 | 描述 |
|-----------------|---------------|
| [应用程序数据示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ApplicationData) | 场景 2-4 重点介绍设置 |
