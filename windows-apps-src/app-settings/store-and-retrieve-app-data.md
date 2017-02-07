---
author: mijacobs
Description: "了解如何存储和检索本地、漫游和临时应用数据。"
title: "存储和检索设置以及其他应用数据"
ms.assetid: 41676A02-325A-455E-8565-C9EC0BC3A8FE
label: App settings and data
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: 5f50e490caa5d1d88c2f8315dc47e15b0ae22a05
ms.openlocfilehash: 08ad1fbddc3db2c8329594419fefdc1aa0659092

---

# <a name="store-and-retrieve-settings-and-other-app-data"></a>存储和检索设置以及其他应用数据





*应用数据*是特定于具体应用的可变数据。 它包含运行时状态、用户首选项和其他设置。 应用数据不同于*用户数据*，它是用户使用应用时创建和管理的数据。 用户数据包含文档或媒体文件、电子邮件或通信脚本或保留用户所创建内容的数据库记录。 用户数据可能对于多个应用都非常有用或有意义。 通常，此为用户要操作或作为独立于应用自身的实体进行传输的数据，例如文档。

**关于应用数据的重要事项：**应用数据的生命周期与应用的生命周期相关联。 如果应用被删除，则会丢失所有应用数据。 不要使用应用数据存储用户数据或用户可能视作有价值和不可替代内容的任何数据。 我们建议使用用户的库和 Microsoft OneDrive 存储此类信息。 应用数据非常适合存储特定于应用的用户首选项、设置和收藏夹。

## <a name="types-of-app-data"></a>应用数据类型


应用数据有两类：设置和文件。

-   **设置**

    使用设置存储用户首选项和应用程序状态信息。 应用数据 API 使你能够轻松创建和检索设置（我们将在本文的后面部分介绍一些示例）。

    下面是可以用于应用设置的数据类型：

    -   **UInt8**、**Int16**、**UInt16**、**Int32**、**UInt32**、**Int64**、**UInt64**、**Single**、**Double**
    -   **布尔值**
    -   **Char16**、**String**
    -   [**DateTime**](https://msdn.microsoft.com/library/windows/apps/br206576)、[**TimeSpan**](https://msdn.microsoft.com/library/windows/apps/br225996)
    -   **GUID**、[**Point**](https://msdn.microsoft.com/library/windows/apps/br225870)、[**Size**](https://msdn.microsoft.com/library/windows/apps/br225995)、[**Rect**](https://msdn.microsoft.com/library/windows/apps/br225994)
    -   [**ApplicationDataCompositeValue**](https://msdn.microsoft.com/library/windows/apps/br241588)：一组必须按原子方式序列化和反序列化的相关应用设置。 使用复合设置可轻松处理相互依赖的设置的原子更新。 系统会在并发访问和漫游时确保复合设置的完整性。 复合设置针对少量数据进行了优化，如果将它们用于大型数据集，性能可能很差。
-   **文件**

    使用文件存储二进制数据，或支持自己的自定义序列化类型。

## <a name="storing-app-data-in-the-app-data-stores"></a>在应用数据存储中存储应用数据


安装应用时，系统会为设置和文件提供它自己的每用户数据存储。 你不需要知道这些数据位于何处或如何存储，因为系统会负责管理物理存储工作，这样可确保数据与其他应用和用户保持隔离状态。 系统还在用户向应用安装更新时保留这些数据存储的内容，并在卸载应用时完全且干净地删除这些数据存储的内容。

在每个应用的应用数据存储中，该应用拥有系统定义的根目录：一个用于本地文件，一个用于漫游文件，还有一个用于临时文件。 应用可以向这些根目录添加新文件和新容器。

## <a name="local-app-data"></a>本地应用数据


本地应用数据应用于需要在应用会话之间予以保留但不适合于漫游应用数据的任何信息。 不适用于其他设备的数据也应存储在此处。 存储的本地数据没有总大小限制。 使用本地应用数据存储来存储对漫游没有用的数据和大型数据集。

### <a name="retrieve-the-local-app-data-store"></a>检索本地应用数据存储

在读取或编写本地应用数据前，必须检索本地应用数据存储。 若要检索本地应用数据存储，请使用 [**ApplicationData.LocalSettings**](https://msdn.microsoft.com/library/windows/apps/br241622) 属性获取应用作为 [**ApplicationDataContainer**](https://msdn.microsoft.com/library/windows/apps/br241599) 对象的本地设置。 使用 [**ApplicationData.LocalFolder**](https://msdn.microsoft.com/library/windows/apps/br241621) 属性可以获取 [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) 对象中的文件。 使用 [**ApplicationData.LocalCacheFolder**](https://msdn.microsoft.com/library/windows/apps/dn633825) 属性来获取保存不包括在备份和还原中的文件的本地应用数据存储中的文件夹。

```CSharp
Windows.Storage.ApplicationDataContainer localSettings = 
    Windows.Storage.ApplicationData.Current.LocalSettings;
Windows.Storage.StorageFolder localFolder = 
    Windows.Storage.ApplicationData.Current.LocalFolder;
```

### <a name="create-and-retrieve-a-simple-local-setting"></a>创建和检索简单的本地设置

若要创建或编写设置，请使用 [**ApplicationDataContainer.Values**](https://msdn.microsoft.com/library/windows/apps/br241615) 属性访问我们在上一步中获取的 `localSettings` 容器中的设置。 此示例会创建一个名为 `exampleSetting` 的设置。

```CSharp
// Simple setting

localSettings.Values["exampleSetting"] = "Hello Windows";
```

若要检索该设置，请使用同一个用于创建设置的 [**ApplicationDataContainer.Values**](https://msdn.microsoft.com/library/windows/apps/br241615) 属性。 此示例显示了如何检索刚创建的设置。

```CSharp
// Simple setting
Object value = localSettings.Values["exampleSetting"];
```

### <a name="create-and-retrieve-a-local-composite-value"></a>创建和检索本地复合值

若要创建或编写复合值，请创建 [**ApplicationDataCompositeValue**](https://msdn.microsoft.com/library/windows/apps/br241588) 对象。 此示例会创建一个名为 `exampleCompositeSetting` 的复合设置并将它添加到 `localSettings` 容器中。

```CSharp
// Composite setting

Windows.Storage.ApplicationDataCompositeValue composite = 
    new Windows.Storage.ApplicationDataCompositeValue();
composite["intVal"] = 1;
composite["strVal"] = "string";

localSettings.Values["exampleCompositeSetting"] = composite;
```

此示例显示了如何检索刚创建的复合值。

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

若要创建和更新本地应用数据存储中的文件，请使用文件 API，如 [**Windows.Storage.StorageFolder.CreateFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227249) 和 [**Windows.Storage.FileIO.WriteTextAsync**](https://msdn.microsoft.com/library/windows/apps/hh701505)。 此示例会在 `localFolder` 容器中创建一个名为 `dataFile.txt` 的文件并将当前日期和时间写入该文件中。 [**CreationCollisionOption**](https://msdn.microsoft.com/library/windows/apps/br241631) 枚举中的 **ReplaceExisting** 值指示替换该文件（如果存在的话）。

```CSharp
async void WriteTimestamp()
{
   Windows.Globalization.DateTimeFormatting.DateTimeFormatter formatter = 
       new Windows.Globalization.DatetimeFormatting.DateTimeFormatter("longtime");

   StorageFile sampleFile = await localFolder.CreateFileAsync("dataFile.txt", 
       CreationCollisionOption.ReplaceExisting);
   await FileIO.WriteTextAsync(sampleFile, formatter.Format(DateTime.Now));
}
```

若要打开本地应用数据存储中的文件，请使用文件 API，如 [**Windows.Storage.StorageFolder.GetFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227272)、[**Windows.Storage.StorageFile.GetFileFromApplicationUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701741) 和 [**Windows.Storage.FileIO.ReadTextAsync**](https://msdn.microsoft.com/library/windows/apps/hh701482)。 此示例打开在上一步中创建的 `dataFile.txt` 文件并从该文件中读取日期。 有关从多个位置加载文件资源的详细信息，请参阅[如何加载文件资源](https://msdn.microsoft.com/library/windows/apps/xaml/hh965322)。

```CSharp
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


如果在应用中使用漫游数据，用户可轻松地在多个设备之间保持应用的应用数据同步。 如果用户在多个设备上安装了你的应用，操作系统将保持应用数据同步，减少用户需要在他们的第二个设备上为你的应用所做的设置工作量。 漫游还支持用户甚至在不同的设备上从他们离开的位置继续执行任务，例如撰写列表。 OS 在漫游数据更新时将它复制到云，并将该数据同步到已安装应用的其他设备。

操作系统限制了每个应用可漫游的应用数据大小。 请参阅 [**ApplicationData.RoamingStorageQuota**](https://msdn.microsoft.com/library/windows/apps/br241625)。 如果应用达到这一限制，在应用的总漫游应用数据再次少于该限制之前，不会将应用的任何应用数据复制到云。 出于此原因，最好的做法是仅为用户首选项、链接和小型数据文件使用漫游数据。

只要用户可在所需的时间间隔内从某个设备访问应用的漫游数据，这些数据就将存在于云中。 如果用户不会在比此时间间隔更长的时间内运行应用，它的漫游数据将从云中删除。 如果用户卸载应用，它的漫游数据不会自动从云中删除，将会保留。 如果用户在该时间间隔内重新安装该应用，则会从云中同步漫游数据。

### <a name="roaming-data-dos-and-donts"></a>漫游数据应做事项和禁止事项

-   将漫游用于用户首选项和自定义、链接以及小型数据文件。 例如，使用漫游在所有设备上保留用户的背景颜色首选项。
-   使用漫游以允许用户跨设备继续执行任务。 例如，诸如草稿电子邮件的内容或阅读器应用中最近查看的页面的漫游应用数据。
-   通过更新应用数据处理 [**DataChanged**](https://msdn.microsoft.com/library/windows/apps/br241620) 事件。 当应用数据从云中完成同步时，会发生该事件。
-   漫游会引用内容而不是原始数据。 例如，会漫游 URL 而不是联机文章的内容。
-   对于重要的、对时间敏感的设置，使用与 [**RoamingSettings**](https://msdn.microsoft.com/library/windows/apps/br241624) 相关联的 *HighPriority* 设置。
-   不要漫游特定于设备的应用数据。 某些信息仅在本地才合理，例如指向本地文件资源的路径名。 如果你决定漫游本地信息，请确保信息在第二台设备上无效时该应用可以进行恢复。
-   不要漫游较大的应用数据集。 应用可以漫游的应用数据量存在限制；使用 [**RoamingStorageQuota**](https://msdn.microsoft.com/library/windows/apps/br241625) 属性获取这个最大值。 如果应用达到该上限，在应用数据存储的大小不再超过该限制之前，不能漫游任何数据。 在你设计应用时，必须考虑如何为较大数据设置一个限制以免超过此限值。 例如，如果每保存一个游戏状态需要 10KB，则应用可能仅允许用户最多储存 10 个游戏。
-   不要为依赖即时同步的数据使用漫游。 Windows 不保证实现即时同步；如果用户脱机或在严重延迟的网络上，漫游可能会大大延迟。 请确保你的 UI 不依赖即时同步。
-   不要使用漫游频繁更改数据。 例如，如果你的应用跟踪频繁更改信息（例如歌曲中每秒的进度位置），则不要将此类信息存储为漫游应用数据。 相反，选取较不频繁但仍提供良好用户体验的表示形式，例如当前播放的歌曲。

### <a name="roaming-pre-requisites"></a>漫游先决条件

如果用户使用 Microsoft 帐户登录相应的设备，则任何用户都可以享受到漫游应用数据的益处。 但是，用户和组策略管理员可以随时在设备上关闭漫游应用数据。 如果用户选择不使用 Microsoft 帐户或者禁用漫游数据功能，她仍可以使用你的应用，但应用数据都将留在每台设备本地。

[**PasswordVault**](https://msdn.microsoft.com/library/windows/apps/br227081) 中存储的数据仅将在用户使设备成为“受信任”设备的情况下传输。 如果设备不受信任，则不会漫游在该保管库中安全存储的数据。

### <a name="conflict-resolution"></a>冲突解决

漫游应用数据不适合在多个设备上同时使用。 如果在同步期间由于两台设备的特定数据单位已更改而导致冲突，系统将始终倾向于最后写入的值。 这将确保应用使用的是最新信息。 如果该数据单元是一个设置组合，则还会在设置单元级别解决冲突，这意味着具有最新更改的组合将被同步。

### <a name="when-to-write-data"></a>何时写入数据

数据应在不同时间写入，具体取决于设置的预期生命周期。 更改较少且较慢的应用数据应立即写入。 然而，频繁更改的应用数据应每隔一定时间（例如每 5 分钟一次）定期写入，以及在应用暂停时写入。 例如，开始播放新歌曲时，音乐应用就可立即写入“当前歌曲”设置，不过，歌曲中的实际位置则仅应在应用暂停时写入。

### <a name="excessive-usage-protection"></a>过度使用保护

系统具有各种保护机制，以避免资源使用不当。 如果应用数据没有如期传输，可能是因为设备暂时受限。 等待一段时间，系统通常可以自动解决此情况，无需进行任何操作。

### <a name="versioning"></a>版本控制

应用数据可利用版本控制功能，从一个数据结构升级至另一数据结构。 此版本编号不同于应用版本，可随意设置。 虽不强制遵循，但强烈建议你使用递增的版本编号，因为如果你尝试向表示更新数据的较低数据版本编号传输，可能发生不良的并发情况（包括数据丢失）。

应用数据仅在版本编号相同的已安装应用之间漫游。 例如，版本 2 的设备彼此之间可传输数据，版本 3 的设备同样如此，但运行版本 2 和版本 3 的设备之间不能进行漫游。 如果你安装了在其他设备上使用各种版本编号的新应用，新安装的应用将同步与最高版本编号相关联的应用数据。

### <a name="testing-and-tools"></a>测试和工具

开发人员可锁定自己的设备，以触发漫游应用数据的同步。 如果应用数据看起来没有在特定时段进行传输，请检查以下项目并确保：

-   你的漫游数据未超过最大大小（有关详细信息，请参阅 [**RoamingStorageQuota**](https://msdn.microsoft.com/library/windows/apps/br241625)）。
-   你的文件已关闭且发布正确。
-   至少存在两台运行相同应用版本的设备。


### <a name="register-to-receive-notification-when-roaming-data-changes"></a>进行注册以在漫游数据发生更改时收到通知

若要使用漫游应用数据，需要对漫游数据更改进行注册，并且检索漫游数据容器，以便可以读取和写入设置。

1.  进行注册以在漫游数据发生更改时收到通知。

    在漫游数据发生更改时，[**DataChanged**](https://msdn.microsoft.com/library/windows/apps/br241620) 事件将通知你。 此示例将 `DataChangeHandler` 设置为用于漫游数据更改的处理程序。

```    CSharp
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

2.  获取应用的设置和文件容器。

    使用 [**ApplicationData.RoamingSettings**](https://msdn.microsoft.com/library/windows/apps/br241624) 属性可以获取设置，使用 [**ApplicationData.RoamingFolder**](https://msdn.microsoft.com/library/windows/apps/br241623) 属性可以获取文件。

```    CSharp
Windows.Storage.ApplicationDataContainer roamingSettings = 
        Windows.Storage.ApplicationData.Current.RoamingSettings;
    Windows.Storage.StorageFolder roamingFolder = 
        Windows.Storage.ApplicationData.Current.RoamingFolder;
```

### <a name="create-and-retrieve-roaming-settings"></a>创建和检索漫游设置

使用 [**ApplicationDataContainer.Values**](https://msdn.microsoft.com/library/windows/apps/br241615) 属性访问我们在前一部分中获取的 `roamingSettings` 容器中的设置。 此示例将创建名为 `exampleSetting` 的简单设置和名为 `composite` 的复合值。

```CSharp
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

此示例将检索刚创建的设置。

```CSharp
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

若要在漫游应用数据存储中创建和更新文件，请使用文件 API（如 [**Windows.Storage.StorageFolder.CreateFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227249) 和 [**Windows.Storage.FileIO.WriteTextAsync**](https://msdn.microsoft.com/library/windows/apps/hh701505)）。 此示例会在 `roamingFolder` 容器中创建一个名为 `dataFile.txt` 的文件并将当前日期和时间写入该文件中。 [**CreationCollisionOption**](https://msdn.microsoft.com/library/windows/apps/br241631) 枚举中的 **ReplaceExisting** 值指示替换该文件（如果存在的话）。

```CSharp
async void WriteTimestamp()
{
   Windows.Globalization.DateTimeFormatting.DateTimeFormatter formatter = 
       new Windows.Globalization.DatetimeFormatting.DateTimeFormatter("longtime");

   StorageFile sampleFile = await roamingFolder.CreateFileAsync("dataFile.txt", 
       CreationCollisionOption.ReplaceExisting);
   await FileIO.WriteTextAsync(sampleFile, formatter.Format(DateTime.Now));
}
```

若要在漫游应用数据存储中打开和读取文件，请使用文件 API（如 [**Windows.Storage.StorageFolder.GetFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227272)、[**Windows.Storage.StorageFile.GetFileFromApplicationUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701741) 和 [**Windows.Storage.FileIO.ReadTextAsync**](https://msdn.microsoft.com/library/windows/apps/hh701482)）。 此示例会打开在前一部分中创建的 `dataFile.txt` 文件并从该文件中读取日期。 有关从多个位置加载文件资源的详细信息，请参阅[如何加载文件资源](https://msdn.microsoft.com/library/windows/apps/xaml/hh965322)。

```CSharp
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


临时应用数据存储类似于缓存。 它的文件不会漫游，随时可以删除。 系统维护任务可以随时自动删除存储在此位置的数据。 用户还可以使用“磁盘清理”清除临时数据存储中的文件。 临时应用数据可用于存储应用会话期间的临时信息。 无法保证超出应用会话结束时间后仍将保留此数据，因为如有需要，系统可能回收已使用的空间。 位置通过 [**temporaryFolder**](https://msdn.microsoft.com/library/windows/apps/br241629) 属性提供。

### <a name="retrieve-the-temporary-data-container"></a>检索临时数据容器

使用 [**ApplicationData.TemporaryFolder**](https://msdn.microsoft.com/library/windows/apps/br241629) 属性获取文件。 后续步骤使用此步骤中的 `temporaryFolder` 变量。

```CSharp
Windows.Storage.StorageFolder temporaryFolder = ApplicationData.Current.TemporaryFolder;</code></pre></td>
</tr>
</tbody>
</table>
```

### <a name="create-and-read-temporary-files"></a>创建和读取临时文件

若要在临时应用数据存储中创建和更新文件，请使用文件 API（如 [**Windows.Storage.StorageFolder.CreateFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227249) 和 [**Windows.Storage.FileIO.WriteTextAsync**](https://msdn.microsoft.com/library/windows/apps/hh701505)）。 此示例会在 `temporaryFolder` 容器中创建一个名为 `dataFile.txt` 的文件并将当前日期和时间写入该文件中。 [**CreationCollisionOption**](https://msdn.microsoft.com/library/windows/apps/br241631) 枚举中的 **ReplaceExisting** 值指示替换该文件（如果存在的话）。


```CSharp
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">C#</th>
</tr>
</thead>
<tbody>
<tr class="odd">
async void WriteTimestamp()
{
   Windows.Globalization.DateTimeFormatting.DateTimeFormatter formatter = 
       new Windows.Globalization.DatetimeFormatting.DateTimeFormatter("longtime");

   StorageFile sampleFile = await temporaryFolder.CreateFileAsync("dataFile.txt", 
       CreateCollisionOption.ReplaceExisting);
   await FileIO.WriteTextAsync(sampleFile, formatter.Format(DateTime.Now));
}
```

若要在临时应用数据存储中打开和读取文件，请使用文件 API（如 [**Windows.Storage.StorageFolder.GetFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227272)、[**Windows.Storage.StorageFile.GetFileFromApplicationUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701741) 和 [**Windows.Storage.FileIO.ReadTextAsync**](https://msdn.microsoft.com/library/windows/apps/hh701482)）。 此示例打开在上一步中创建的 `dataFile.txt` 文件并从该文件中读取日期。 有关从多个位置加载文件资源的详细信息，请参阅[如何加载文件资源](https://msdn.microsoft.com/library/windows/apps/xaml/hh965322)。

```CSharp
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

## <a name="organize-app-data-with-containers"></a>使用容器组织应用数据


若要帮助你组织应用数据设置和文件，请创建容器（由 [**ApplicationDataContainer**](https://msdn.microsoft.com/library/windows/apps/br241599) 对象表示），而不是直接使用目录。 你可以向本地、漫游和临时应用数据存储添加容器。 容器的嵌套深度可达 32 层。

若要创建设置容器，请调用 [**ApplicationDataContainer.CreateContainer**](https://msdn.microsoft.com/library/windows/apps/br241611) 方法。 此示例将创建一个名为 `exampleContainer` 的本地设置容器并添加一个名为 `exampleSetting` 的设置。 [**ApplicationDataCreateDisposition**](https://msdn.microsoft.com/library/windows/apps/br241616) 枚举中的 **Always** 值指示已创建容器（如果尚不存在的话）。

```CSharp
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


若要删除应用不再需要的简单设置，请使用 [**ApplicationDataContainerSettings.Remove**](https://msdn.microsoft.com/library/windows/apps/br241608) 方法。 此示例将删除之前创建的 `exampleSetting` 本地设置。

```CSharp
Windows.Storage.ApplicationDataContainer localSettings = 
    Windows.Storage.ApplicationData.Current.LocalSettings;
Windows.Storage.StorageFolder localFolder = 
    Windows.Storage.ApplicationData.Current.LocalFolder;

// Delete simple setting

localSettings.Values.Remove("exampleSetting");
```

若要删除复合设置，请使用 [**ApplicationDataCompositeValue.Remove**](https://msdn.microsoft.com/library/windows/apps/br241597) 方法。 此示例将删除在之前示例中创建的本地 `exampleCompositeSetting` 复合设置。

```CSharp
Windows.Storage.ApplicationDataContainer localSettings = 
    Windows.Storage.ApplicationData.Current.LocalSettings;
Windows.Storage.StorageFolder localFolder = 
    Windows.Storage.ApplicationData.Current.LocalFolder;

// Delete composite setting

localSettings.Values.Remove("exampleCompositeSetting");
```

若要删除容器，请调用 [**ApplicationDataContainer.DeleteContainer**](https://msdn.microsoft.com/library/windows/apps/br241612) 方法。 此示例将删除之前创建的本地 `exampleContainer` 设置容器。

```CSharp
Windows.Storage.ApplicationDataContainer localSettings = 
    Windows.Storage.ApplicationData.Current.LocalSettings;
Windows.Storage.StorageFolder localFolder = 
    Windows.Storage.ApplicationData.Current.LocalFolder;

// Delete container

localSettings.DeleteContainer("exampleContainer");
```

## <a name="versioning-your-app-data"></a>对应用数据进行版本控制


视情况，也可以对应用的应用数据进行版本控制。 这将使你能够创建应用的未来版本，更改它的应用数据的格式，而不会导致与以前应用版本出现兼容性问题。 应用将检查数据存储中的应用数据版本，如果该版本低于应用想要的版本，应用应该将应用数据更新为新格式并更新该版本。 有关详细信息，请参阅 [**Application.Version**](https://msdn.microsoft.com/library/windows/apps/br241630) 属性和 [**ApplicationData.SetVersionAsync**](https://msdn.microsoft.com/library/windows/apps/hh701429) 方法。

## <a name="related-articles"></a>相关文章

* [**Windows.Storage.ApplicationData**](https://msdn.microsoft.com/library/windows/apps/br241587)
* [**Windows.Storage.ApplicationData.RoamingSettings**](https://msdn.microsoft.com/library/windows/apps/br241624)
* [**Windows.Storage.ApplicationData.RoamingFolder**](https://msdn.microsoft.com/library/windows/apps/br241623)
* [**Windows.Storage.ApplicationData.RoamingStorageQuota**](https://msdn.microsoft.com/library/windows/apps/br241625)
* [**Windows.Storage.ApplicationDataCompositeValue**](https://msdn.microsoft.com/library/windows/apps/br241588)





<!--HONumber=Dec16_HO2-->


