---
author: Karl-Bridge-Microsoft
Description: "了解如何在运行时使用语音识别结果，在语音命令定义 (VCD) 文件中访问和更新支持的短语列表（PhraseList 元素）。"
title: "动态修改 VCD 短语列表"
ms.assetid: 98024EAC-EC0E-44AA-AEC5-A611BA7C5884
label: Modify VCD phrase lists
template: detail.hbs
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: 623243b94cf8ef6b276f8f2971af7bbdbdece81c

---

# 动态修改 VCD 短语列表





**重要的 API**

-   [**Windows.ApplicationModel.VoiceCommands**](https://msdn.microsoft.com/library/windows/apps/dn706594)
-   [**VCD 元素和属性 v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)

在运行时使用语音识别结果，在语音命令定义 (VCD) 文件中访问和更新支持的短语列表（**PhraseList** 元素）。

如果语音命令特定于涉及一些用户定义或临时应用数据的任务，则在运行时动态修改短语列表会非常有用。 

例如，假设你有一个旅行应用（用户可以在其中输入目的地），并且你希望用户能够通过在说出“显示旅行&lt;目的地&gt;”后紧接着说出该应用名称来启动它。 在 **ListenFor** 元素本身中，你可以指定如下内容：`<ListenFor> Show trip to {destination}  </ListenFor>`，其中“destination”是 **PhraseList** 的 **Label** 属性值。

在运行时更新该短语列表无需为每个可能的目的地创建单独的 **ListenFor** 元素。 相反，当用户输入他们的行程时，你可以使用用户指定的目的地动态填充 **PhraseList**。 

有关 **PhraseList** 和其他 VCD 元素的详细信息，请参阅 [**VCD 元素和属性 v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593) 参考。

**先决条件：  **

本主题基于[在 Cortana 中使用语音命令启动前台应用](launch-a-foreground-app-with-voice-commands-in-cortana.md)生成。 在这里，我们将继续通过名为 **Adventure Works** 的旅行规划和管理应用演示相关功能。

如果你还不熟悉通用 Windows 平台 (UWP) 应用开发，请仔细阅读这些主题来熟悉此处讨论的技术。

-   [创建你的第一个应用](https://msdn.microsoft.com/library/windows/apps/bg124288)
-   借助[事件和路由事件概述](https://msdn.microsoft.com/library/windows/apps/mt185584)了解事件

**用户体验指南：  **

有关如何将你的应用与 **Cortana** 集成的信息，请参阅 [Cortana 设计指南](https://msdn.microsoft.com/library/windows/apps/dn974233)；有关设计出既实用又有吸引力且支持语音的应用的有用提示，请参阅[语音设计指南](https://msdn.microsoft.com/library/windows/apps/dn596121)。

## <span id="Identify_the_command"></span><span id="identify_the_command"></span><span id="IDENTIFY_THE_COMMAND"></span>标识命令和更新短语列表

下面是一个 VCD 文件示例，该文件定义了 **Command**“showTripToDestination”和 **PhraseList**，后者定义的三个选项用于我们的 **Adventure Works** 旅行应用中的目的地。 当用户在该应用中保存和删除目的地时，该应用将更新 **PhraseList** 中的相应选项。

```XML
<?xml version="1.0" encoding="utf-8"?>
<VoiceCommands xmlns="http://schemas.microsoft.com/voicecommands/1.1">
  <CommandSet xml:lang="en-us" Name="AdventureWorksCommandSet_en-us">
    <CommandPrefix> Adventure Works, </CommandPrefix>
    <Example> Show trip to London </Example>

    <Command Name="showTripToDestination">
      <Example> show trip to London  </Example>
      <ListenFor> show trip to {destination} </ListenFor>
      <Feedback> Showing trip to {destination} </Feedback>
      <Navigate/>
    </Command>

    <PhraseList Label="destination">
      <Item> London </Item>
      <Item> Dallas </Item>
      <Item> New York </Item>
    </PhraseList>

  </CommandSet>

<!-- Other CommandSets for other languages -->

</VoiceCommands>

```

若要更新 VCD 文件中的 **PhraseList** 元素，请获取包含短语列表的 **CommandSet** 元素。 将 **CommandSet** 元素的 **Name** 属性（**Name** 在 VCD 文件中必须唯一）用作密钥以访问 [**VoiceCommandManager.InstalledCommandSets**](https://msdn.microsoft.com/library/windows/apps/dn653257) 属性并获取 [**VoiceCommandSet**](https://msdn.microsoft.com/library/windows/apps/dn653258) 参考。

标识命令集之后，获取一个要修改的短语列表参考并调用 [**SetPhraseListAsync**](https://msdn.microsoft.com/library/windows/apps/dn653261) 方法；使用 **PhraseList** 元素的 **Label** 属性和字符串数组作为短语列表的新内容。

**注意** 如果你修改短语列表，将替换整个短语列表。 如果你要将新项目插入短语列表，必须在对 [**SetPhraseListAsync**](https://msdn.microsoft.com/library/windows/apps/dn653261) 的调用中同时指定现有项目和新项目。

在此示例中，我们使用附加目的地 Phoenix 更新前面示例中所示的 **PhraseList**。

```CSharp
Windows.ApplicationModel.VoiceCommands.VoiceCommnadDefinition.VoiceCommandSet commandSetEnUs;

if (Windows.ApplicationModel.VoiceCommands.VoiceCommandDefinitionManager.
      InstalledCommandSets.TryGetValue(
        "AdventureWorksCommandSet_en-us", out commandSetEnUs))
{
  await commandSetEnUs.SetPhraseListAsync(
    "destination", new string[] {“London”, “Dallas”, “New York”, “Phoenix”});
}
```

## <span id="Remarks"></span><span id="remarks"></span><span id="REMARKS"></span>备注


使用 **PhraseList** 限制识别适用于一组相对较小的单词。 当单词组过大（例如数百个单词）或者根本不应受限时，请使用 **PhraseTopic** 元素和 **Subject** 元素来优化语音识别结果的相关性，从而增强可扩展性。

在我们的示例中，我们具有一个带“搜索”**Scenario** 的 **PhraseTopic**，它通过“City\\State”**Subject** 得到了进一步优化。

```XML
<?xml version="1.0" encoding="utf-8"?>
<VoiceCommands xmlns="http://schemas.microsoft.com/voicecommands/1.1">
  <CommandSet xml:lang="en-us" Name="AdventureWorksCommandSet_en-us">
    <CommandPrefix> Adventure Works, </CommandPrefix>
    <Example> Show trip to London </Example>

    <Command Name="showTripToDestination">
      <Example> show trip to London  </Example>
      <ListenFor> show trip to {destination} </ListenFor>
      <Feedback> Showing trip to {destination} </Feedback>
      <Navigate/>
    </Command>

    <PhraseList Label="destination">
      <Item> London </Item>
      <Item> Dallas </Item>
      <Item> New York </Item>
    </PhraseList>

    <PhraseTopic Label="destination" Scenario="Search">
      <Subject>City/State</Subject>
    </PhraseTopic>

  </CommandSet>
```

## <span id="related_topics"></span>相关文章


**开发人员**
* [Cortana 交互](cortana-interactions.md)
* [在 Cortana 中使用语音命令启动前台应用](launch-a-foreground-app-with-voice-commands-in-cortana.md)
* [在 Cortana 中使用语音命令启动后台应用](launch-a-background-app-with-voice-commands-in-cortana.md)
* [**VCD 元素和属性 v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)

**设计人员**
* [Cortana 设计指南](https://msdn.microsoft.com/library/windows/apps/dn974233)
* [语音设计指南](https://msdn.microsoft.com/library/windows/apps/dn596121)

**示例**
* [Cortana 语音命令示例](http://go.microsoft.com/fwlink/p/?LinkID=619899)
 

 







<!--HONumber=Jun16_HO3-->


