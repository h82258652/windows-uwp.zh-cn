---
author: Karl-Bridge-Microsoft
Description: 了解如何使用更灵活、更自然的语音命令扩展 Cortana，以便用户可以在命令中的任何位置说出应用名称。
title: 在 Cortana 中支持自然语言形式的语音命令
ms.assetid: 281E068A-336A-4A8D-879A-D8715C817911
label: Support natural language voice commands
template: detail.hbs
---

# 在 Cortana 中支持自然语言形式的语音命令

使用更灵活、更自然的语音命令扩展 **Cortana**，使用户可以在命令中的任何位置说出应用名称。

**重要的 API**

-   [**Windows.ApplicationModel.VoiceCommands**](https://msdn.microsoft.com/library/windows/apps/dn706594)
-   [**语音命令定义 (VCD) 元素和属性 v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)


使用语音命令功能从应用中扩展 Cortana 功能需要用户指定要执行的应用以及命令或函数。 该操作通常通过在语音命令开始或结束时读出应用名称来完成。 例如，“Adventure Works，新增一个拉斯维加斯之旅。”

而通过这种方式指定应用程序名称可能会出现发音生硬、不自然甚至是表达不清楚等情况。 在许多情况下，如果你能够在其他位置使用命令说出应用名称，则可以使命令显得更为贴切和自然，并且有助于让与用户的交互变得更为直观和趣味盎然。 我们前面的示例“Adventure Works，新增一个拉斯维加斯之旅。” 可以换个说法“新增到拉斯维加斯的 Adventure Works 之旅。” 或者是“使用 Adventure Works 新增拉斯维加斯之旅。”

你可以将语音命令设置为支持应用名称作为下列形式：

-   前缀 - 位于命令短语之前
-   中缀 - 位于命令短语内
-   后缀 - 位于命令短语之后

**先决条件：  **

本主题基于[在 Cortana 中使用语音命令启动后台应用](launch-a-background-app-with-voice-commands-in-cortana.md)展开。 在这里，我们将继续通过名为 **Adventure Works** 的旅行规划和管理应用演示相关功能。

如果你还不熟悉通用 Windows 平台 (UWP) 应用开发，请仔细阅读这些主题来熟悉此处讨论的技术。

-   [创建你的第一个应用](https://msdn.microsoft.com/library/windows/apps/bg124288)
-   借助[事件和路由事件概述](https://msdn.microsoft.com/library/windows/apps/mt185584)了解事件

**用户体验指南：  **

有关如何将你的应用与 **Cortana** 集成的信息，请参阅 [Cortana 设计指南](https://msdn.microsoft.com/library/windows/apps/dn974233)；有关设计出既实用又有吸引力且支持语音的应用的有用提示，请参阅[语音设计指南](https://msdn.microsoft.com/library/windows/apps/dn596121)。

## <span id="Specify_an_AppName_element_in_the_VCD"></span><span id="specify_an_appname_element_in_the_vcd"></span><span id="SPECIFY_AN_APPNAME_ELEMENT_IN_THE_VCD"></span>在 VCD 中指定 **AppName** 元素


**AppName** 元素用于在语音命令中为应用指定一个用户友好名称。
```XML
<AppName>Adventure Works</AppName>
```

## <span id="Specify_where_the_app_name_can_be_spoken_in_the_voice_command"></span><span id="specify_where_the_app_name_can_be_spoken_in_the_voice_command"></span><span id="SPECIFY_WHERE_THE_APP_NAME_CAN_BE_SPOKEN_IN_THE_VOICE_COMMAND"></span>指定可在语音命令中说出应用名称的位置


**ListenFor** 元素具有一个 **RequireAppName** 属性，该属性用于指定可在语音命令中显示应用名称的位置。 该属性支持四个值。

1.  **BeforePhrase**

    默认值。

    指示用户必须在命令短语之前说出应用名称。

    在此处，Cortana 会听到 “Adventure Works，我的拉斯维加斯之旅的时间”。
```xml
<ListenFor RequireAppName="BeforePhrase"> show [my] trip to {destination} </ListenFor>
```

2.  **AfterPhrase**

    指示用户必须在命令短语之后说出应用名称。

    已本地化的介词连词短语列表由系统提供。 这包括诸如“using”、“with”和“on”等短语。

    在此处，Cortana 会听到诸如“在 Adventure Works 上显示我的下一个拉斯维加斯之旅”和“使用 Adventure Works 显示我的下一个拉斯维加斯之旅”等命令。
```xml
<ListenFor RequireAppName="AfterPhrase">show [my] next trip to {destination} </ListenFor>
```

3.  **BeforeOrAfterPhrase**

    指示用户必须在命令短语之前或之后说出应用名称。

    对于后缀版本，已本地化的介和连词短语列表由系统提供。 这包括诸如“using”、“with”和“on”等短语。

    在此处，Cortana 会听到诸如“Adventure Works，显示我的下一个拉斯维加斯之旅”或者“显示我的下一个拉斯维加斯之旅在 Adventure Works 上”等命令。
``` xml
<ListenFor RequireAppName="BeforeOrAfterPhrase">show [my] next trip to {destination}</ListenFor>
```

4.  **ExplicitlySpecified**

    指示用户必须准确说出应用名称在命令短语中所指定的位置。 不要求用户说出应用名称是在短语之前还是之后。

    你必须使用 **{builtin:AppName}** 标记显式引用你的应用名称。

    在此处，Cortana 会听到诸如“Adventure Works，显示我的下一个拉斯维加斯之旅”或者“显示我的下一个 Adventure Works 拉斯维加斯之旅”等命令。
```xml
<ListenFor RequireAppName="ExplicitlySpecified">show [my] next {builtin:AppName} trip to {destination} </ListenFor>
```

## <span id="Special_cases"></span><span id="special_cases"></span><span id="SPECIAL_CASES"></span>特殊情况

当你声明 **ListenFor** 元素（其中 **RequireAppName** 是“AfterPhrase”或“ExplicitlySpecified”）时，你必须确保符合以下特定要求：

1.  **{builtin:AppName}** 必须只显示一次（仅当 **RequireAppName** 为“ExplicitlySpecified”时）。

    通过此值，系统将无法推断出应用名称在语音命令中所处的位置。 你必须显式指定该位置。

2.  语音命令不能以 **PhraseTopic** 元素开头，该元素通常用于较大的词汇语音识别。 该元素前面必须至少有一个单词。

    如果在语音命令的任意位置中包含你的应用名称或者部分应用名称，这有助于将 **Cortana** 启动该应用的几率降至最低。

    下面是一个无效声明，可能会在用户说出类似于“向我显示评论 Kinect Adventure works”等短语时导致 **Cortana** 启动 **Adventure Works** 应用。
```xml
<ListenFor RequireAppName="ExplicitlySpecified">{searchPhrase} {builtin:AppName}</ListenFor>
```

3.  除了你的应用名称和对 **PhraseTopic** 元素的引用外，**ListenFor** 字符串中也必须至少含有两个单词。

    与案例 2 相似，你需要确保你的命令中包含足够多的语音内容，以将无意间启动你的应用的几率降至最低。

    这将有助于最大程度地增加你成功设置应用程序的几率，这样你的应用程序便不会在用户说出类似于“查找 Kinect Adventure works”等短语时错误地进行启动。

    下面是一个无效声明，可能会在用户说出类似于“Hey adventure works”或“查找 Kinect adventure works”等短语时导致 **Cortana** 启动 **Adventure Works** 应用。
```xml
<ListenFor RequireAppName="ExplicitlySpecified">Hey {builtin:AppName}</ListenFor>
<ListenFor RequireAppName="ExplicitlySpecified">Find {searchPhrase} {builtin:AppName}</ListenFor>
```

## <span id="Remarks"></span><span id="remarks"></span><span id="REMARKS"></span>备注

如果支持用户在 **Cortana** 中说出语音命令时采用更多变化形式，那么通常也会增加你的应用的使用率。

避免将“Hey \[应用名]”作为你的 **AppName** 或 **CommandPrefix**。 用户很有可能会说出“你好小娜”，以通过语音激活来调用 Cortana，然而在语音中包含“你好 \[应用名]”会使声音听起来不太自然。 例如，“你好小娜，在‘你好 Adventure Works’上显示我的下一个拉斯维加斯之旅”。

请考虑向现有语音命令添加中缀/后缀变体。 如此处所示，无需执行大量操作即可将一个附加属性添加到你的现有 **ListenFor** 元素并支持后缀变体。 与“你好小娜，Adventure Works，显示我的下一个拉斯维加斯之旅”相比，说出“你好小娜，在 Adventure Works 上显示我的下一个拉斯维加斯之旅”显得更为自然。

如果语音命令与现有的 **Cortana** 功能（呼叫、短信等）发生冲突，请考虑将你的应用名称用作前缀。 例如，“Adventure Works，有关拉斯维加斯之旅的消息 \[旅行社\]”。

## <span id="Complete_example"></span><span id="complete_example"></span><span id="COMPLETE_EXAMPLE"></span>完整示例


下面是一个 VCD 文件，用于演示各种提供更自然的语言语音命令的方式。

**注意** 它支持拥有多个 **ListenFor** 元素，每个元素都具有不同的 **RequireAppName** 属性值。 

```XML
<?xml version="1.0" encoding="utf-8"?>
<VoiceCommands xmlns="http://schemas.microsoft.com/voicecommands/1.1">
  <CommandSet xml:lang="en-us" Name="commandSet_en-us">
    <AppName>Adventure Works</AppName>
    <Example> When is my trip to Las Vegas? </Example>
    <Command Name="whenIsTripToDestination">
      <Example> When is my trip to Las Vegas?</Example>
      <ListenFor RequireAppName="BeforePhrase">
        when is my] trip to {destination} </ListenFor>

      <!-- This ListenFor command will set up Cortana to accept commands like 
           “Show my next trip to Las Vegas on Adventure Works”; “Show my next 
           trip to Las Vegas using Adventure Works” -->
      <ListenFor RequireAppName="AfterPhrase">
        show [my] next trip to {destination} </ListenFor>

      <!-- This ListenFor command will set up Cortana to accept commands when 
           the user specifies your app name either before or after the command. 
           “Adventure Works, show my next trip to Las Vegas”; 
           “Show my next trip to Last Vegas on Adventure works” -->
      <ListenFor RequireAppName="BeforeOrAfterPhrase">
        show [my] next trip to {destination} </ListenFor>

      <!-- This ListenFor command will set up Cortana to accept commands 
           when the user specifies your app name inline. 
           “Show my next Adventure Works trip to Las Vegas” -->
      <ListenFor RequireAppName="ExplicitlySpecified">
        show [my] next {builtin:AppName} trip to {destination} </ListenFor>

      <Feedback> Looking for trip to {destination} </Feedback>
      <Navigate />
    </Command>
    <PhraseList Label="destination">
      <Item> Las Vegas </Item>
      <Item> Dallas </Item>
      <Item> New York </Item>
    </PhraseList>
  </CommandSet>
  <!-- Other CommandSets for other languages -->
</VoiceCommands>
```

## <span id="related_topics"></span>相关文章


**开发人员**
* [Cortana 交互](cortana-interactions.md)
* [定义自定义识别约束](define-custom-recognition-constraints.md)
* [**VCD 元素和属性 v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)

**设计人员**
* [Cortana 设计指南](https://msdn.microsoft.com/library/windows/apps/dn974233)
* [语音设计指南](https://msdn.microsoft.com/library/windows/apps/dn596121)

**示例**
* [Cortana 语音命令示例](http://go.microsoft.com/fwlink/p/?LinkID=619899)
 

 






<!--HONumber=May16_HO2-->


