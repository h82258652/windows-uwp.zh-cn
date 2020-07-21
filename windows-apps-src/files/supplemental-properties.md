---
title: 使用补充属性
description: 介绍补充属性的用法及其在 Windows 中的实现方式的详细信息
ms.date: 01/10/2017
ms.topic: article
keywords: windows 10, uwp, WinRT API, 索引器, 搜索
localizationpriority: medium
ms.openlocfilehash: 2a77bfc37d853efd28bde9bc3043d072888822f2
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "66369262"
---
# <a name="using-supplemental-properties"></a>使用补充属性  

## <a name="summary"></a>摘要  
- 补充属性可让应用在不更改文件的情况下使用属性来标记文件 
- 如果某些属性很难计算，或者无法修改文件，则你可以使用补充属性 
- 补充属性的用法与 Windows 属性系统中任何其他属性的用法相同  

## <a name="introduction"></a>简介 
过去几年以来，有许多创新型应用需要对用户文件运行 CPU 密集型操作，以从文件中提取有用的属性，而这些属性不仅仅是创建日期之类的基本信息。 这些应用会在图像中执行对象识别，在电子邮件中提取意向，并执行文本分析以将文档分组到一起。 当今的大部分消费型电脑都可以提供驱动这些功能的算力。   

立即使这些元数据变得可搜索能使用户的工作效率呈指数级提高。 只是知道你的女儿在照片中并不让人惊奇，但如果能够在照片中搜索出她与她的奶奶，那么就能满足更多的需求。 这会使计算机的使用体验更个性化，且更贴近生活。 这种感觉类似于电脑中有个人可以帮助你找回珍贵的记忆。 

数十年来，Windows 中的快速搜索解决方案一直都是索引器，在创意者更新版中，索引器经过更新，可以支持上述方案。 现在，应用可以使用除系统提取的属性以外的其他属性来标记文件。 这些属性被视为一等公民  

## <a name="windows-properties"></a>Windows 属性 
多年以来，[Windows 属性系统](https://docs.microsoft.com/windows/desktop/properties/windows-properties-system)在与文件交互方面一直发挥着关键的作用。 应用可以使用该系统从文件中读取属性，而无需了解文件可能采用的所有不同格式或语言的内部结构。 系统将为开发人员抽象掉所有这些内部结构，他们只需请求列表，然后指定升序或降序即可。  

属性系统与 Windows 索引器相辅相成 – 它会从其所在范围内的文件中读取所有属性，并存储这些属性。 稍后当应用请求提供某个文件夹中要按修改日期排序的所有 .docx 文件列表，（排除 John Smith 创建的文件）时，索引器可以即时返回该列表。  

这两个系统协同工作的缺点是，索引器往往要求即时提供它要存储的有关某个文件的属性。 这会限制它了解其他需要花费更长时间计算的有用属性，因为时间要求很高。  

不过，属性很容易使用，应用可以请求有关文件的排序属性集（类似于使用数据库），或者可以使用搜索引擎传递查询。 索引器将处理查询并返回结果。 这样，开发人员便可以灵活地将其筛选器（例如仅搜索 jpg 文件）与用户的查询（以“bird”开头的文件名）相组合。 

## <a name="supplemental-properties"></a>补充属性 

补充属性的行为与普通的 Windows 属性类似，但有一个非常重要的差别 – 将文件添加到索引器时不会写入补充属性。 补充属性必须在一段时间后由系统上的另一个应用添加。 这段时间可能是完成对象识别后的两分钟，也可能是数日。 

写入属性后，可以像处理系统上的任何其他属性一样对其进行搜索、筛选、排序或分组。 它也可以在组合查询中与系统上的其他属性（无论是否为补充属性）一起使用。 这样，你便可以轻松、灵活地将补充属性与现有的文件系统代码相结合，而无需重新编写代码。  

### <a name="example-scenarios"></a>示例方案 

可将数千个不同的属性写入补充属性，本教程将会回顾几个关键的方案：  

#### <a name="tagging-pictures-with-extracted-properties"></a>使用提取的属性标记图片 
这些应用可以使用机器学习训练的模型从图像中提取系统并不知道的特征（例如图像中的对象）。 然后，它可以提取在图像中识别到的对象，并将其添加到属性系统，供稍后进行搜索或分组。  

#### <a name="tagging-files-with-an-app-specific-id"></a>使用应用特定的 ID 标记文件 
许多文件同步应用使用其自身的唯一 ID 来跟踪在服务器与各种客户端设备之间移动的文件。 同步客户端可以在不影响文件的情况下将此 ID 写入属性系统。 该应用稍后可以使用此 ID 进行快速访问，而系统上的任何其他应用在与同步提供程序通信时可以使用此 ID 读取数据。 

补充属性还有其他许多的用法选项，但这两种用法示例很有代表性，因为它们需要快速查找或搜索，是系统不知道的信息片段，且不能添加到文件本身。  

### <a name="using-supplemental-properties"></a>使用补充属性 
使用补充属性等同于将普通属性写入文件系统。 如果你对 StorageFile 和属性的用法很熟悉，则可以跳过本部分。 否则，我们将通过一个快速示例来演练如何将一个属性写出到文件，然后读回该属性。  

### <a name="writing-supplemental-properties"></a>写入补充属性  
为简单起见，该示例只会修改它找到的第一个文件，不过，应用通常会将属性添加到它所找到的每个文件。  

```csharp
// Only indexed jpg files are going to be used 
QueryOptions option = new QueryOptions(CommonFileQuery.DefaultQuery, new string[] { ".jpg" }); 
option.IndexerOption = IndexerOption.OnlyUseIndexer; 
// Typically an app would loop over all the files in the library, updating them all with the new 
// value. To make the sample easier to understand however, this app is only going to update the  
// first file it finds 
var query = KnownFolders.PicturesLibrary.CreateFileQueryWithOptions(option); 
StorageFile file = (await query.GetFilesAsync()).FirstOrDefault(); 
if (file == null) 
{ 
    log("No jpg file found in the library. Stopping"); 
    return; 
} 
log("Found file: " + file.Path); 
// Selecting the property to modify and writing it out 
List<KeyValuePair<string, object>> props = new List<KeyValuePair<string, object>>();             
props.Add(new KeyValuePair<string, object>("System.Supplemental.ResourceId", fileId)); 
await file.Properties.SavePropertiesAsync(props); 
```

在写出属性之前，必须检查位置是否已编制索引。 此示例将使用查询选项进行筛选，以便仅返回已编制索引的位置。 如果无法做到这一点，可以检查父文件夹的索引状态 (file.GetParentAsync().GetIndexedStateAsync())。 无论使用哪种方式，都会生成相同的结果 

### <a name="reading-supplemental-properties"></a>读取补充属性 
同样，读取补充属性等同于读取任何其他文件系统属性。 此示例中的应用只会从已包含 StorageFile 的文件中读取一个属性，但它也可以同时读取其他属性。  

```csharp
// An object to hold the result from the indexer, and a string to store  
// the value in once we have confirmed it is valid. 
object uncheckedResourceId; 
string resourceId = ""; 
// Fetching the key value pair from the indexer 
IDictionary<string,object> returnedProps =  
    await file.Properties.RetrievePropertiesAsync(new string[] { "System.Supplemental.ResourceId" });             
if (returnedProps.TryGetValue("System.Supplemental.ResourceId", out uncheckedResourceId)) 
{ 
    if (uncheckedResourceId != null && !String.IsNullOrEmpty(uncheckedResourceId.ToString())) 
    { 
        resourceId = uncheckedResourceId.ToString(); 
    } 
} 
```
该应用会通过检查来确保属性系统传回的值符合预期。 应用写入该值后，有可能该值已被清除，不过，这种情况非常少见。 下面将提供相关的详细信息。  

### <a name="implementation-notes"></a>实现说明 
在补充属性的设计中可以做出几个微妙的选择。 为帮助你完成实现，我们从功能的工程设计规范中复制了以下部分。 参考这些内容可以大致了解功能的设计方式，以及存在一些限制的原因。 

### <a name="supplemental-properties-available"></a>提供的补充属性 
最初只会提供两个可在应用中使用的属性：System.Supplemental.ResourceId 和 System.Supplemental.AlbumID。 如果需要，可以添加更多的属性。 AlbumID 是可用于许多不同应用程序的多值字符串，ResourceId 用作云同步提供程序的唯一 ID。 

#### <a name="file-system-support"></a>文件系统支持 
由于 FAT 格式的可移动媒体是一个重要方案，补充属性支持 FAT 和 NTFS 驱动器。 这可以确保补充属性可供所有用户使用，不管他们使用的设备类型是什么。   

### <a name="non-indexed-locations"></a>未编制索引的位置  
在桌面上，有许多文件夹未编制索引。 在这些情况下，应用可能仍然想要访问补充属性。 但是，补充属性不能在已编制索引位置的外部使用。 出于多种原因，我们对此问题采取了以下折衷方案：  

- 默认情况下，所有库和云存储位置已编制索引。   
  这些位置是 UWP 应用主要使用的位置。 其他某些位置未编制索引（系统或网络驱动器），但它们不太常用于存储用户数据。 

- WinRT API 图面设计假设索引器几乎始终可用。  
  因此，应用所需的大多数位置中已提供索引器。 如果发现用户需要在未编制索引的位置中存储数据，最简单的解决方法是将该位置添加到索引。 然后，补充属性将会正常运行，枚举将会加快速度，而应用可对位置进行更改跟踪。

### <a name="reading-or-writing-supplemental-properties-from-a-file-in-a-non-indexed-location"></a>在未编制索引位置的文件中读取或写入补充属性 
如果应用尝试将补充属性写入到当前未编制索引的位置，则 API 调用将会引发异常。 此异常与某人尝试更新 .docx 文件中的 System.Music.AlbumArtist 时引发的异常相同（无效的参数）。  
 
### <a name="change-notifications"></a>更改通知：  
与针对标准属性一样，可以继续针对补充属性使用 UWP 更改通知和更改跟踪。 这样，提供数据的应用便可以跟踪其某个应用发生的所有更改 
  
### <a name="invalidating-properties"></a>使属性失效：  
每当在系统上修改或移动文件时，文件中的补充属性可能会过期。 推送数据的应用包含有关数据是否有效或者是否需要更新的信息，因此，系统只会向其提供所需的工具，让它们自行判断。  
 
如果某个文件已修改，但未移动或重命名，则该文件中的所有补充属性将保持不变。 应用可以通过现有的 API 图面注册更改通知，并根据需要更新属性。 
 
如果文件已移动，则属性将会失效。 应用会根据操作的确切完成方式，来接收删除、重命名或移动操作的更改通知。 应用收到更改通知后，即可检查文件，并根据需要更新该文件中的补充属性。 
 
### <a name="indexer-rebuilds"></a>索引器重建  
有时，出于以下多种可能的原因之一，需要重建系统索引 – 属性架构发生更改、用户启用了 EDP，或者数据库文件已损坏。 在这种情况下，将不会保留补充属性。 我们曾经考虑过尽量在重建索引时保留补充属性，但面临着几种重大阻碍：  

### <a name="protecting-the-data"></a>保护数据 
如果磁盘错误或恶意软件导致数据库文件损坏，则无法保护该文件中存储的数据。 这些数据必须存储在系统上的其他位置，或者以某种方式将其与数据库中的其他数据相隔离。 

我们已付出大量努力，索引损坏的情况非常少见，因此数据库文件损坏的几率将会下降。  
重建期间保留文件与其元数据之间的映射 

尽管索引可以在重建期间保护数据，但重建索引时无法知道文件是否已更改。 如果文件已修改或移动，文件中受索引保护的数据可能不再有效。  
行为 

重建索引器时，所有补充数据将会丢失。 应用负责将重建期间丢失的数据放回到索引器中。 这确实会给应用带来额外的负担，但这种行为被认为是合理的，因为应用始终保留其所有数据的主状态。  

### <a name="recovering"></a>恢复 
让应用知道正在重建索引后，应用将负责在方便的时间更新补充属性。  
### <a name="privacy"></a>隐私 
用户可能不希望将写入文件的某些属性与其他应用程序共享。 应用应该能够指示其写入到属性中的信息是供这些属性的应用程序专用的、只与其他几个应用程序共享，或者向系统上的每个应用公开。  

尽管某些早期采用者可能对此功能非常感兴趣，但他们觉得，获取公共属性仍可以为设计带来许多的价值。 因此，此功能被标记为值得提供，同时我们应持续在不鼓励隐匿价值的情况下构建所需的功能。 添加此功能有助于今后实现更多的方案，因此，在任何设计中都必须考虑到这一点。  

## <a name="conclusions"></a>结论 
如前所述，使用补充属性可以十分方便地在系统中存储更多的文件属性。 当然，补充属性是选用性的，但是，相比于其他无法快速排序和搜索数据的应用，使用补充属性的应用更有优势。 

我们期待更多的应用开始使用这些属性。 如果你在补充属性的用法方面有任何疑问，请通过下面的“评论”告诉我们 
