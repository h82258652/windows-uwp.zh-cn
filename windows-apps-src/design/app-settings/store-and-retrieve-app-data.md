---
Description: 了解如何存储和检索本地、漫游和临时应用程序数据。
title: 存储和检索设置及其他应用数据
ms.assetid: 41676A02-325A-455E-8565-C9EC0BC3A8FE
label: App settings and data
template: detail.hbs
ms.date: 11/14/2017
ms.topic: article
keywords: windows 10，uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0eb7ef49d0ce1876635dc36e84f43432c13e1791
ms.sourcegitcommit: f561efbda5c1d47b85601d91d70d86c5332bbf8c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/21/2019
ms.locfileid: "72690370"
---
# <a name="store-and-retrieve-settings-and-other-app-data"></a>存储和检索设置及其他应用数据

*应用程序数据*是由特定应用程序创建和管理的可变数据。 它包括运行时状态、应用设置、用户首选项、引用内容（如字典应用中的字典定义）和其他设置。 应用数据不同于*用户数据*，即用户在使用应用时创建和管理的数据。 用户数据包括文档或媒体文件、电子邮件或通信记录或保存用户创建的内容的数据库记录。 用户数据对于多个应用可能有用，也可能有意义。 通常，这是用户要作为实体独立于应用本身（例如文档）进行操作或传输的数据。

**有关应用数据的重要说明：** 应用程序数据的生存期与应用程序的生存期相关联。 如果删除了该应用，则所有应用数据都会丢失。 不要使用应用数据来存储用户数据，也不要使用用户认为有价值且不可替代的任何内容。 建议使用用户的库和 Microsoft OneDrive 来存储此类信息。 应用数据非常适合用于存储特定于应用的用户首选项、设置和收藏夹。

## <a name="types-of-app-data"></a>应用数据的类型

有两种类型的应用数据：设置和文件。

### <a name="settings"></a>设置

使用设置来存储用户首选项和应用程序状态信息。 利用应用数据 API，你可以轻松地创建和检索设置（本文后面会显示一些示例）。

以下是可用于应用设置的数据类型：

- **UInt8**、 **Int16**、 **UInt16**、 **Int32**、 **UInt32**、 **Int64**、 **UInt64**、 **Single**、 **Double**
- **布尔值**
- **Char16**，**字符串**
- [**DateTime**](/uwp/api/Windows.Foundation.DateTime)、 [ **TimeSpan**](/uwp/api/Windows.Foundation.TimeSpan)
    - 对于C#/.NET [ **，请使用**](/dotnet/api/system.timespan?view=dotnet-uwp-10.0)： [**system.object**](/dotnet/api/system.datetimeoffset?view=dotnet-uwp-10.0)、system.string
- **GUID**，[**点**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point)，[**大小**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Size)， [**Rect**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Rect)
- [**ApplicationDataCompositeValue**](/uwp/api/Windows.Storage.ApplicationDataCompositeValue)：必须以原子方式进行序列化和反序列化的一组相关应用设置。 使用复合设置轻松处理相互依赖的设置的原子更新。 系统在并发访问和漫游期间确保复合设置的完整性。 复合设置针对少量数据进行了优化，如果你将其用于大型数据集，则性能可能会很差。

### <a name="files"></a>文件

使用文件来存储二进制数据或启用您自己的自定义序列化类型。

## <a name="storing-app-data-in-the-app-data-stores"></a>在应用数据存储中存储应用程序数据


安装应用时，系统会为其提供设置和文件的每个用户的数据存储区。 您无需知道此数据的存在位置或方法，因为系统负责管理物理存储，从而确保数据与其他应用程序和其他用户保持隔离。 当用户将更新安装到应用时，系统还会保留这些数据存储的内容，并在卸载应用时完全干净地删除这些数据存储的内容。

在应用程序数据存储中，每个应用程序都有系统定义的根目录：一个用于本地文件，一个用于漫游文件，另一个用于临时文件。 应用可将新文件和新容器添加到这些根目录。

## <a name="local-app-data"></a>本地应用数据


对于需要在应用程序会话之间保留并且不适用于漫游应用程序数据的任何信息，应使用本地应用程序数据。 其他设备上不适用的数据也应该存储在此处。 存储的本地数据没有常规大小限制。 使用本地应用数据存储来了解对于漫游和大型数据集而言没有意义的数据。

### <a name="retrieve-the-local-app-data-store"></a>检索本地应用数据存储

必须先检索本地应用数据存储，然后才能读取或写入本地应用数据。 若要检索本地应用数据存储，请使用[**ApplicationData. LocalSettings**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localsettings)属性以[**windows.storage.applicationdatacontainer**](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationDataContainer)对象的形式获取应用的本地设置。 使用[**ApplicationData. LocalFolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localfolder)属性可获取[**StorageFolder**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFolder)对象中的文件。 使用[**ApplicationData. LocalCacheFolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localcachefolder)属性获取本地应用程序数据存储中的文件夹，您可以在其中保存不包含在备份和还原中的文件。

```CSharp
Windows.Storage.ApplicationDataContainer localSettings = 
    Windows.Storage.ApplicationData.Current.LocalSettings;
Windows.Storage.StorageFolder localFolder = 
    Windows.Storage.ApplicationData.Current.LocalFolder;
```

### <a name="create-and-retrieve-a-simple-local-setting"></a>创建和检索简单的本地设置

若要创建或写入设置，请使用[**windows.storage.applicationdatacontainer**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacontainer.values)属性访问在上一步中获取的 `localSettings` 容器中的设置。 此示例将创建一个名为 `exampleSetting`的设置。

```CSharp
// Simple setting

localSettings.Values["exampleSetting"] = "Hello Windows";
```

若要检索设置，请使用与创建该设置时所用的相同的[**windows.storage.applicationdatacontainer**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacontainer.values)属性。 此示例演示如何检索刚刚创建的设置。

```CSharp
// Simple setting
Object value = localSettings.Values["exampleSetting"];
```

### <a name="create-and-retrieve-a-local-composite-value"></a>创建和检索本地组合值

若要创建或写入一个组合值，请创建一个[**ApplicationDataCompositeValue**](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationDataCompositeValue)对象。 此示例创建一个名为 `exampleCompositeSetting` 的复合设置，并将其添加到 `localSettings` 容器中。

```CSharp
// Composite setting

Windows.Storage.ApplicationDataCompositeValue composite = 
    new Windows.Storage.ApplicationDataCompositeValue();
composite["intVal"] = 1;
composite["strVal"] = "string";

localSettings.Values["exampleCompositeSetting"] = composite;
```

此示例演示如何检索刚刚创建的复合值。

```CSharp
// Composite setting

Windows.Storage.ApplicationDataCompositeValue composite = 
   (Windows.Storage.ApplicationDataCompositeValue)localSettings.Values["exampleCompositeSetting"];

if (composite == null)
{
   // No data
}
else
{
   // Access data in composite["intVal"] and composite["strVal"]
}
```

### <a name="create-and-read-a-local-file"></a>创建和读取本地文件

若要创建和更新本地应用程序数据存储中的文件，请使用文件 Api，如[**CreateFileAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.createfileasync)和[**FileIO WriteTextAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileio.writetextasync)。 此示例在 `localFolder` 容器中创建一个名为 `dataFile.txt` 的文件，并将当前日期和时间写入文件。 [**CreationCollisionOption**](https://docs.microsoft.com/uwp/api/Windows.Storage.CreationCollisionOption)枚举中的**ReplaceExisting**值指示如果文件已存在，则替换它。

```csharp
async void WriteTimestamp()
{
   Windows.Globalization.DateTimeFormatting.DateTimeFormatter formatter = 
       new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("longtime");

   StorageFile sampleFile = await localFolder.CreateFileAsync("dataFile.txt", 
       CreationCollisionOption.ReplaceExisting);
   await FileIO.WriteTextAsync(sampleFile, formatter.Format(DateTimeOffset.Now));
}
```

若要在本地应用程序数据存储中打开和读取文件，请使用文件 Api，如[**StorageFolder. document.getfileasync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.getfileasync)、 [**StorageFile. GetFileFromApplicationUriAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.getfilefromapplicationuriasync)和 FileIO [ **。 ReadTextAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileio.readtextasync). 此示例打开在上一步中创建的 `dataFile.txt` 文件，并从该文件中读取日期。 有关从不同位置加载文件资源的详细信息，请参阅[如何加载文件资源](https://docs.microsoft.com/previous-versions/windows/apps/hh965322(v=win.10))。

```csharp
async void ReadTimestamp()
{
   try
   {
      StorageFile sampleFile = await localFolder.GetFileAsync("dataFile.txt");
      String timestamp = await FileIO.ReadTextAsync(sampleFile);
      // Data is contained in timestamp
   }
   catch (Exception)
   {
      // Timestamp not found
   }
}
```

## <a name="roaming-data"></a>漫游数据


如果在应用中使用漫游数据，用户可以轻松地在多个设备之间同步应用的应用数据。 如果用户在多个设备上安装你的应用程序，则 OS 会使应用程序数据保持同步，从而减少用户在其第二个设备上为你的应用程序所需执行的安装工作量。 漫游还使用户能够继续执行某个任务，如撰写列表，使其在其他设备上离中断。 在更新时，OS 会将漫游数据复制到云，并将数据同步到安装了该应用的其他设备。

操作系统限制每个应用程序可以漫游的应用程序数据的大小。 请参阅[**ApplicationData. RoamingStorageQuota**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingstoragequota)。 如果应用达到此限制，应用的任何应用数据都不会复制到云，直到应用的总漫游应用数据小于该限制。 出于此原因，最佳做法是仅对用户首选项、链接和小数据文件使用漫游数据。

应用的漫游数据在云中提供，只要用户在所需时间间隔内从某个设备进行访问。 如果用户运行的应用超过此时间间隔，则会从云中删除其漫游数据。 如果用户卸载应用，则不会自动从云中删除其漫游数据。 如果用户在时间间隔内重新安装应用，则漫游数据从云中同步。

### <a name="roaming-data-dos-and-donts"></a>漫游数据的待办事项

- 为用户首选项和自定义、链接和小数据文件使用漫游。 例如，使用漫游在所有设备中保留用户的背景颜色首选项。
- 使用漫游允许用户跨设备继续执行任务。 例如，将应用数据（例如，已起草电子邮件的内容）或读者应用中最近查看的页面的内容漫游。
- 通过更新应用数据来处理[**DataChanged**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.datachanged)事件。 当应用数据刚完成从云中同步时，将发生此事件。
- 漫游对内容的引用，而不是原始数据。 例如，漫游 URL，而不是联机文章的内容。
- 对于重要的时间关键设置，请使用与[**RoamingSettings**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingsettings)关联的*HighPriority*设置。
- 请勿漫游特定于设备的应用数据。 某些信息仅与本地相关，例如本地文件资源的路径名称。 如果决定漫游本地信息，请确保在辅助设备上的信息无效时可以恢复应用。
- 请勿漫游大型应用程序数据集。 应用可漫游的应用数据量有限制;使用[**RoamingStorageQuota**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingstoragequota)属性获取此最大值。 如果应用达到此限制，则在应用数据存储大小不再超过限制之前，不能漫游数据。 设计应用时，请考虑如何将绑定置于更大的数据上，使其不超过限制。 例如，如果保存游戏状态需要10KB，则该应用程序可能只允许用户存储最多10个游戏。
- 对于依赖于即时同步的数据，不要使用漫游。 Windows 不保证即时同步;如果用户处于脱机状态或处于高延迟网络，漫游可能会明显延迟。 确保您的 UI 不依赖于即时同步。
- 不要将漫游用于经常更改的数据。 例如，如果你的应用程序跟踪经常更改的信息（例如，每秒在歌曲中的位置），请勿将其存储为漫游应用程序数据。 相反，选择一种不太频繁的表示形式，它仍能提供良好的用户体验，如当前播放的歌曲。

### <a name="roaming-pre-requisites"></a>漫游先决条件

如果用户使用 Microsoft 帐户登录到其设备，则任何用户都可以受益于漫游应用程序数据。 但是，用户和组策略管理员可随时在设备上关闭漫游应用程序数据。 如果用户选择不使用 Microsoft 帐户或禁用漫游数据功能，她仍可以使用您的应用程序，但应用程序数据将是每个设备的本地应用程序数据。

如果用户将设备设为 "受信任"， [**PasswordVault**](https://docs.microsoft.com/uwp/api/Windows.Security.Credentials.PasswordVault)中存储的数据仅会转换。 如果设备不受信任，则在此保管库中保护的数据将不会漫游。

### <a name="conflict-resolution"></a>冲突解决

漫游应用程序数据不能同时在多台设备上同时使用。 如果在同步过程中出现冲突是因为在两个设备上更改了某个特定数据单元，则系统将始终优选最后写入的值。 这可确保应用使用最新的信息。 如果数据单位是复合设置，则在设置单元级别仍将发生冲突解决，这意味着将同步使用最新更改的复合。

### <a name="when-to-write-data"></a>写入数据的时间

根据所需的设置生存期，数据应在不同的时间编写。 应立即写入应用数据的不频繁或缓慢变化。 但是，通常只应定期（例如每5分钟一次）定期编写经常更改的应用数据，并在应用挂起时进行。 例如，当新歌曲开始播放时，音乐应用可能会写入 "当前歌曲" 设置，但是，歌曲中的实际位置只应在挂起时写入。

### <a name="excessive-usage-protection"></a>过度使用保护

系统具有各种保护机制，以避免不当使用资源。 如果应用数据没有按预期转换，则可能是该设备暂时受到了限制。 如果等待一段时间，则通常会自动解决这种情况，无需采取任何措施。

### <a name="versioning"></a>版本控制

应用数据可以利用版本控制从一个数据结构升级到另一个数据结构。 版本号与应用版本不同，可以设置为。 尽管不强制，但强烈建议你使用增加的版本号，因为如果你尝试转换到表示较新数据的较低数据版本号，则可能会出现不必要的复杂性（包括数据丢失）。

应用数据只在具有相同版本号的已安装应用之间漫游。 例如，版本2上的设备会相互转换数据，版本3上的设备将执行相同的操作，但不会在运行版本2的设备与运行版本3的设备之间进行漫游。 如果在其他设备上安装使用了不同版本号的新应用，则新安装的应用将同步与最高版本号关联的应用数据。

### <a name="testing-and-tools"></a>测试和工具

开发人员可以锁定其设备，以便触发漫游应用程序数据的同步。 如果似乎应用数据在某个时间范围内不会转换，请检查以下各项，并确保：

- 漫游数据未超出最大大小（有关详细信息，请参阅[**RoamingStorageQuota**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingstoragequota) ）。
- 文件已关闭并正确发布。
- 至少有两台设备运行相同版本的应用。


### <a name="register-to-receive-notification-when-roaming-data-changes"></a>注册以在漫游数据更改时接收通知

若要使用漫游应用数据，您需要注册漫游数据更改并检索漫游数据容器，以便您可以读取和写入设置。

1.  注册以在漫游数据更改时接收通知。

    漫游数据发生更改时， [**DataChanged**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.datachanged)事件会通知你。 此示例将 `DataChangeHandler` 设置为漫游数据更改的处理程序。

```csharp
void InitHandlers()
    {
       Windows.Storage.ApplicationData.Current.DataChanged += 
          new TypedEventHandler<ApplicationData, object>(DataChangeHandler);
    }

    void DataChangeHandler(Windows.Storage.ApplicationData appData, object o)
    {
       // TODO: Refresh your data
    }
```

2.  获取应用的设置和文件的容器。

    使用[**ApplicationData. RoamingSettings**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingsettings)属性获取设置，并使用[**ApplicationData**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingfolder)属性获取文件。

```csharp
Windows.Storage.ApplicationDataContainer roamingSettings = 
        Windows.Storage.ApplicationData.Current.RoamingSettings;
    Windows.Storage.StorageFolder roamingFolder = 
        Windows.Storage.ApplicationData.Current.RoamingFolder;
```

### <a name="create-and-retrieve-roaming-settings"></a>创建和检索漫游设置

使用[**windows.storage.applicationdatacontainer**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacontainer.values)属性访问在上一节中获取的 `roamingSettings` 容器中的设置。 此示例将创建一个名为 `exampleSetting` 的简单设置和一个名为 `composite`的复合值。

```csharp
// Simple setting

roamingSettings.Values["exampleSetting"] = "Hello World";
// High Priority setting, for example, last page position in book reader app
roamingSettings.values["HighPriority"] = "65";

// Composite setting

Windows.Storage.ApplicationDataCompositeValue composite = 
    new Windows.Storage.ApplicationDataCompositeValue();
composite["intVal"] = 1;
composite["strVal"] = "string";

roamingSettings.Values["exampleCompositeSetting"] = composite;

```

此示例将检索刚刚创建的设置。

```csharp
// Simple setting

Object value = roamingSettings.Values["exampleSetting"];

// Composite setting

Windows.Storage.ApplicationDataCompositeValue composite = 
   (Windows.Storage.ApplicationDataCompositeValue)roamingSettings.Values["exampleCompositeSetting"];

if (composite == null)
{
   // No data
}
else
{
   // Access data in composite["intVal"] and composite["strVal"]
}
```

### <a name="create-and-retrieve-roaming-files"></a>创建和检索漫游文件

若要创建和更新漫游应用数据存储中的文件，请使用文件 Api，例如[**StorageFolder. CreateFileAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.createfileasync)和[**WriteTextAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileio.writetextasync)。 此示例在 `roamingFolder` 容器中创建一个名为 `dataFile.txt` 的文件，并将当前日期和时间写入文件。 [**CreationCollisionOption**](https://docs.microsoft.com/uwp/api/Windows.Storage.CreationCollisionOption)枚举中的**ReplaceExisting**值指示如果文件已存在，则替换它。

```csharp
async void WriteTimestamp()
{
   Windows.Globalization.DateTimeFormatting.DateTimeFormatter formatter = 
       new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("longtime");

   StorageFile sampleFile = await roamingFolder.CreateFileAsync("dataFile.txt", 
       CreationCollisionOption.ReplaceExisting);
   await FileIO.WriteTextAsync(sampleFile, formatter.Format(DateTimeOffset.Now));
}
```

若要打开并读取漫游应用程序数据存储中的文件，请使用文件 Api，例如[**StorageFolder. document.getfileasync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.getfileasync)、 [**StorageFile. GetFileFromApplicationUriAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.getfilefromapplicationuriasync)和[**FileIO. ReadTextAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileio.readtextasync)。 此示例打开在上一部分中创建的 `dataFile.txt` 文件，并从该文件中读取日期。 有关从不同位置加载文件资源的详细信息，请参阅[如何加载文件资源](https://docs.microsoft.com/previous-versions/windows/apps/hh965322(v=win.10))。

```csharp
async void ReadTimestamp()
{
   try
   {
      StorageFile sampleFile = await roamingFolder.GetFileAsync("dataFile.txt");
      String timestamp = await FileIO.ReadTextAsync(sampleFile);
      // Data is contained in timestamp
   }
   catch (Exception)
   {
      // Timestamp not found
   }
}
```


## <a name="temporary-app-data"></a>临时应用数据


临时应用程序数据存储的工作方式类似于缓存。 它的文件不漫游，随时都可以删除。 系统维护任务可随时自动删除存储在此位置的数据。 用户还可以使用 "磁盘清理" 清除临时数据存储中的文件。 临时应用数据可用于在应用会话期间存储临时信息。 不能保证在应用程序会话结束时，此数据将保持不变，因为系统可能需要时回收已用空间。 该位置通过[**temporaryFolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.temporaryfolder)属性提供。

### <a name="retrieve-the-temporary-data-container"></a>检索临时数据容器

使用[**ApplicationData. TemporaryFolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.temporaryfolder)属性获取文件。 接下来的步骤使用此步骤中的 `temporaryFolder` 变量。

```csharp
Windows.Storage.StorageFolder temporaryFolder = ApplicationData.Current.TemporaryFolder;
```

### <a name="create-and-read-temporary-files"></a>创建和读取临时文件

若要创建和更新临时应用程序数据存储中的文件，请使用文件 Api，例如[**StorageFolder. CreateFileAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.createfileasync)和[**WriteTextAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileio.writetextasync)。 此示例在 `temporaryFolder` 容器中创建一个名为 `dataFile.txt` 的文件，并将当前日期和时间写入文件。 [**CreationCollisionOption**](https://docs.microsoft.com/uwp/api/Windows.Storage.CreationCollisionOption)枚举中的**ReplaceExisting**值指示如果文件已存在，则替换它。


```csharp
async void WriteTimestamp()
{
   Windows.Globalization.DateTimeFormatting.DateTimeFormatter formatter = 
       new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("longtime");

   StorageFile sampleFile = await temporaryFolder.CreateFileAsync("dataFile.txt", 
       CreateCollisionOption.ReplaceExisting);
   await FileIO.WriteTextAsync(sampleFile, formatter.Format(DateTimeOffset.Now));
}
```

若要打开并读取临时应用程序数据存储中的文件，请使用文件 Api，例如[**StorageFolder. document.getfileasync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.getfileasync)、 [**StorageFile. GetFileFromApplicationUriAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.getfilefromapplicationuriasync)和[**FileIO. ReadTextAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileio.readtextasync)。 此示例打开在上一步中创建的 `dataFile.txt` 文件，并从该文件中读取日期。 有关从不同位置加载文件资源的详细信息，请参阅[如何加载文件资源](https://docs.microsoft.com/previous-versions/windows/apps/hh965322(v=win.10))。

```csharp
async void ReadTimestamp()
{
   try
   {
      StorageFile sampleFile = await temporaryFolder.GetFileAsync("dataFile.txt");
      String timestamp = await FileIO.ReadTextAsync(sampleFile);
      // Data is contained in timestamp
   }
   catch (Exception)
   {
      // Timestamp not found
   }
}
```

## <a name="organize-app-data-with-containers"></a>利用容器组织应用数据


为了帮助你组织应用数据设置和文件，你可以创建容器（由[**windows.storage.applicationdatacontainer**](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationDataContainer)对象表示），而不是直接使用目录。 可以将容器添加到本地、漫游和临时应用数据存储中。 容器最多可以嵌套到32级别。

若要创建设置容器，请调用[**windows.storage.applicationdatacontainer. CreateContainer**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacontainer.createcontainer)方法。 此示例创建一个名为 `exampleContainer` 的本地设置容器，并添加一个名为 `exampleSetting`的设置。 [**ApplicationDataCreateDisposition**](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationDataCreateDisposition)枚举中的**Always**值指示如果容器尚不存在，则创建该容器。

```csharp
Windows.Storage.ApplicationDataContainer localSettings = 
    Windows.Storage.ApplicationData.Current.LocalSettings;
Windows.Storage.StorageFolder localFolder = 
    Windows.Storage.ApplicationData.Current.LocalFolder;

// Setting in a container
Windows.Storage.ApplicationDataContainer container = 
   localSettings.CreateContainer("exampleContainer", Windows.Storage.ApplicationDataCreateDisposition.Always);

if (localSettings.Containers.ContainsKey("exampleContainer"))
{
   localSettings.Containers["exampleContainer"].Values["exampleSetting"] = "Hello Windows";
}
```

## <a name="delete-app-settings-and-containers"></a>删除应用设置和容器


若要删除应用不再需要的简单设置，请使用[**ApplicationDataContainerSettings**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacontainersettings.remove)方法。 此示例 deletesthe 以前创建 `exampleSetting` 本地设置。

```csharp
Windows.Storage.ApplicationDataContainer localSettings = 
    Windows.Storage.ApplicationData.Current.LocalSettings;
Windows.Storage.StorageFolder localFolder = 
    Windows.Storage.ApplicationData.Current.LocalFolder;

// Delete simple setting

localSettings.Values.Remove("exampleSetting");
```

若要删除复合设置，请使用[**ApplicationDataCompositeValue**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacompositevalue.remove)方法。 此示例将删除在前面的示例中创建的本地 `exampleCompositeSetting` 复合设置。

```csharp
Windows.Storage.ApplicationDataContainer localSettings = 
    Windows.Storage.ApplicationData.Current.LocalSettings;
Windows.Storage.StorageFolder localFolder = 
    Windows.Storage.ApplicationData.Current.LocalFolder;

// Delete composite setting

localSettings.Values.Remove("exampleCompositeSetting");
```

若要删除容器，请调用[**windows.storage.applicationdatacontainer. DeleteContainer**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacontainer.deletecontainer)方法。 此示例将删除之前创建的本地 `exampleContainer` 设置容器。

```csharp
Windows.Storage.ApplicationDataContainer localSettings = 
    Windows.Storage.ApplicationData.Current.LocalSettings;
Windows.Storage.StorageFolder localFolder = 
    Windows.Storage.ApplicationData.Current.LocalFolder;

// Delete container

localSettings.DeleteContainer("exampleContainer");
```

## <a name="versioning-your-app-data"></a>对应用数据进行版本控制


您可以选择为应用程序提供应用程序数据的版本。 这使你能够创建应用程序的未来版本，该版本更改其应用程序数据的格式，而不会导致应用程序的早期版本出现兼容性问题。 应用检查数据存储中应用程序数据的版本，如果版本低于应用程序所需的版本，应用程序应将应用程序数据更新为新格式，并更新版本。 有关详细信息，请参阅 ApplicationData 属性和[**SetVersionAsync**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.setversionasync) [**方法。** ](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.version)

## <a name="related-articles"></a>相关文章

* [**ApplicationData**](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationData)
* [**ApplicationData. RoamingSettings**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingsettings)
* [**ApplicationData. RoamingFolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingfolder)
* [**ApplicationData. RoamingStorageQuota**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingstoragequota)
* [**ApplicationDataCompositeValue**](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationDataCompositeValue)
