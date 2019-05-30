---
title: 使用补充属性
description: 介绍如何在 Windows 中实现它们对使用补充属性和详细信息
ms.date: 01/10/2017
ms.topic: article
keywords: windows 10，uwp，WinRT API，索引器搜索
localizationpriority: medium
ms.openlocfilehash: 2a77bfc37d853efd28bde9bc3043d072888822f2
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2019
ms.locfileid: "66369262"
---
# <a name="using-supplemental-properties"></a>使用补充属性  

## <a name="summary"></a>总结  
- 补充属性允许具有属性的标记文件的应用程序而无需更改的文件 
- 有用的无法修改的拥有属性很难计算或该文件 
- 使用补充属性等同于 Windows 属性系统上使用任何其他属性  

## <a name="introduction"></a>简介 
很多在过去几年中的令人兴奋的新应用程序需要运行的用户文件从超出基本操作，例如创建日期的文件中提取有用的属性上的 CPU 密集型操作。 这些应用程序进行一起对象识别在映像中，意向提取电子邮件和文本分析中组文档。 这由如何驱动现可在大多数使用者的 Pc 上强大的计算。   

使此元数据立即可搜索允许用户以指数级增长提高工作效率。 只需知道您的女儿图片很有趣，但能够搜索的她与她老奶奶的图片是很多有用。 它可以使用计算机可以了解更多个人，活动状态并在更多的体验。 像在计算机中的某人联系为了帮助您找到您珍爱的回忆。 

数十年时间，在 Windows 上的快速搜索解决方案已索引器和创意者更新中已更新以支持这些新方案。 应用现可使用除系统提取这些之外的其他属性的标记文件。 这些属性被视为第一个类成员  

## <a name="windows-properties"></a>Windows 属性 
[Windows 属性系统](https://docs.microsoft.com/windows/desktop/properties/windows-properties-system)已多年与文件进行交互的重要组成部分。 它允许应用从文件读取属性，而无需了解的所有不同的文件格式或语言文件可能是在内部结构。 所有的抽象化为您作为开发人员，您需要做的就是列表并指定按升序或降序。  

属性系统而使用 Windows 索引器，它从其作用域内的文件中读取所有属性，并将其存储。 更高版本时应用要求提供一组要进行的修改日期排序的文件夹中的所有.docx 但不包括那些作者 John Smith 的索引器可以返回列表，可立即。  

这些系统如何协同工作的缺点是，索引器，用于需要的所有属性它会存储有关要立即可用的文件。 这将限制它从了解有关更有趣的属性的需要更长时间来计算由于没有硬时间要求。  

使用属性但很容易，应用程序可以请求一组排序的属性有关的文件，类似于使用数据库，或可以通过类似使用搜索引擎的查询。 索引器将处理该查询，并返回结果。 这使开发人员能够灵活地组合其筛选器 （例如仅搜索 jpg 文件） 与该用户的查询 （文件名称以"鸟"开头）。 

## <a name="supplemental-properties"></a>补充属性 

补充属性具有相同行为对常规 Windows 属性有一个非常重要的区别 – 它们不会将文件添加到索引器时要写入。 必须由系统更高版本上的另一个应用程序添加补充属性。 它可能是完成两分钟的更高版本一次识别对象也可以是天更高版本。 

编写该属性后可以搜索、 筛选、 排序，或就像在系统上的任何其他属性进行分组。 也可以在中使用的系统上，其他属性组合的查询可以补充与否。 这样，您可以灵活地与现有的文件系统代码轻松结合补充属性，而无需执行重写。  

### <a name="example-scenarios"></a>示例方案 

有数千个不同的属性无法写入到的补充属性，但有几个关键方案，本教程中将保留追溯到：  

#### <a name="tagging-pictures-with-extracted-properties"></a>标记包含提取的属性的图片 
这些应用可以使用机器学习训练的模型要从系统不知道有关，如图中的对象的图像中提取功能。 然后，它可以提取它在图像中标识的对象并将它们添加到更高版本搜索或分组的属性系统。  

#### <a name="tagging-files-with-an-app-specific-id"></a>具有应用程序特定 ID 的标记文件 
许多文件同步应用程序使用其自己唯一的 ID 来跟踪文件，因为它们在服务器和客户端的各种设备之间移动。 同步客户端可以向属性系统写入此 ID，而不影响该文件。 此 ID 现在已经可用于更高版本的快速访问应用，可用于读取与同步提供程序通信时的系统上的任何其他应用程序。 

有许多其他选项使用补充属性，但这两种进行很好的示例，因为它们需要快速查找或搜索，是的系统不知道，并且不能添加到文件本身的信息片段。  

### <a name="using-supplemental-properties"></a>使用补充属性 
使用的补充属性等同于写入到文件系统的常规属性。 如果你熟悉使用 StorageFiles 和属性，您可以跳过此。 否则，让我们通过写出到文件的单个属性，然后稍后读取相同的属性返回的快速示例。  

### <a name="writing-supplemental-properties"></a>编写补充属性  
该示例仅将修改为简单起见，它找到的第一个文件，但通常应用程序会将属性添加到它找到的每个文件。  

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

如果位置之前写出属性编制了索引，则一项重要检查。 在此示例中我们正在使用查询选项进行筛选以仅索引位置。 如果这不是可行可以检查索引的状态的父文件夹 （文件。GetParentAsync()。GetIndexedStateAsync())。 无论哪种方式将产生相同的结果 

### <a name="reading-supplemental-properties"></a>阅读补充属性 
同样，读取补充属性是等同于阅读任何其他文件系统属性。 在此示例应用程序将只是一个属性从文件中读取它已经具有一个 StorageFile 作为，但它还可以同时读取的其他属性。  

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
还有一个检查以确保返回来自属性系统的值符合预期。 尽管不太可能，很可能已被清除值，因为您的应用程序编写了。 在下方提供详细信息中包含此文件。  

### <a name="implementation-notes"></a>实现说明 
有几个细微选项所做的补充属性设计中。 若要帮助你实现以下各节已复制的功能工程设计规范中。 它们提供一个初步了解到该功能的设计方式和原因的一些限制存在。 

### <a name="supplemental-properties-available"></a>可用的补充属性 
有可用来进行应用程序最初只有两个属性：System.Supplemental.ResourceId 和 System.Supplemental.AlbumID。 如果需要将它们添加的详细信息。 唱片集 ID 是一个多值字符串，可用于许多不同的应用程序和 ResourceId 用作云同步提供程序的唯一 ID。 

#### <a name="file-system-support"></a>文件系统支持 
由于 FAT 格式化可移动媒体是一个重要的方案，补充属性将支持 FAT 和 NTFS 驱动器。 这将确保，将可供所有用户，而不考虑其设备类型的补充属性。   

### <a name="non-indexed-locations"></a>非索引位置  
在桌面上有多个未编制索引的文件夹。 在这些情况下，应用程序可能仍然想要访问的补充属性。 但是，补充属性不可用外部已索引的位置。 此折中方案是出于几个原因：  

- 默认情况下进行索引的所有库和云存储位置。   
  这些是主要使用的 UWP 应用的位置。 不是索引 （系统或网络驱动器） 的其他位置，但它们不太常用于用于存储用户数据。 

- WinRT API 图面上设计假设索引器几乎总是可用。  
  因此，索引器目前已在大部分应用程序感兴趣的位置。 如果找到用户若要将数据存储在未索引的位置，最简单的解决方案将会将该位置添加到索引。 然后补充属性工作枚举将会更快，并应用将能够更改跟踪的位置。

### <a name="reading-or-writing-supplemental-properties-from-a-file-in-a-non-indexed-location"></a>读取或写入从 Non-Indexed 位置中的文件的补充属性 
然后在应用程序尝试写入到当前未编制索引的位置的补充属性的情况下，API 调用将引发异常。 这将是相同作为当有人尝试更新上.docx 文件 (无效 Args) System.Music.AlbumArtist 时引发的异常。  
 
### <a name="change-notifications"></a>更改通知：  
UWP 更改通知和更改跟踪将继续适用于的补充属性与在标准属性。 这样，便提供了要跟踪其应用程序之一发生的所有更改数据的应用 
  
### <a name="invalidating-properties"></a>使属性：  
修改或在系统上移动文件时，对文件的补充属性可能会过期。 如果数据无效，或需要更新，因此，系统只需提供他们找出自己的工具，应用将数据推送将有关在使用信息的。  
 
在这种情况中的文件被修改，但不是移动或重命名，则对该文件的补充属性将保持不变。 应用将能够注册以通过现有的 API 外围应用的更改通知，并根据需要更新属性。 
 
如果文件被移动，属性将会失效。 应用程序将接收更改通知的删除、 重命名或移动具体取决于如何创建完全操作完成。 应用程序已收到更改通知后它将能够检查该文件并根据需要更新对该文件的补充属性。 
 
### <a name="indexer-rebuilds"></a>重新生成索引器  
偶尔也会出现系统索引重新生成很多原因之一-属性架构可能会更改，用户无法启用 EDP，或只是数据库文件可能已损坏。 在这些情况下，将不被保留的补充属性。 我们考虑自己做的工作来尝试时重新生成索引，但出现了几个主要的阻塞程序保留的补充属性：  

### <a name="protecting-the-data"></a>保护数据 
在其中的数据库文件已损坏，不论是通过磁盘错误或恶意软件的情况下它将不可能以保护该文件中存储的数据。 它必须存储在系统上的其他位置或以某种方式独立于数据库的其余部分。 

由于我们已经正在执行大量工作，以便不太可能已损坏的索引，这将仍要降低这种情况下的发生率速率。  
维护期间重新生成的文件和其元数据之间的映射 

即使索引可以重新生成跨保护的数据，就无法知道时重新生成索引，是否该文件是否发生更改。 索引从文件保护的数据可能不再有效如果修改或移动文件。  
行为 

对于索引器重新生成所有的补充数据将会丢失。 应用将负责将数据重新放入索引器的重新生成期间已丢失。 这 does 置于应用了额外的负担，但被视为合理，因为它们将始终保存其所有数据的主状态。  

### <a name="recovering"></a>恢复 
一旦应用程序已经注意到重新生成索引，它们将负责更新在方便的补充属性。  
### <a name="privacy"></a>隐私 
一些可能会写入到文件的属性转到，以便用户不希望它们与其他应用程序共享。 应用应该能够指示其写入到属性的信息将会为专用其应用程序，只需几个其他应用程序，使用共享的或公共到系统上每个应用程序。  

虽然这可能是一个有趣的功能，对于某些功能的早期采用者，他们觉得，获取公共属性仍将继续增加对设计更多的值。 因此，它被标记为最好能够，并且我们应继续生成不支持以下隐藏值，如果所需的功能。 将其添加更高版本会打开更多方案，因此它需要重点关注任何设计过程中需要考虑。  

## <a name="conclusions"></a>结论 
就这么简单、 补充属性是在系统中存储更多的文件属性的简单方法。 使用它们当然是可选的但它会使您的应用程序边缘转移其他应用不能进行排序和搜索其数据的速度快。 

我们期待着看到应用程序开始使用这些属性。 如果有关于如何使用标头请告诉我们下面的注释中的任何问题 
