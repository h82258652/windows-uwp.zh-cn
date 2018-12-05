---
Description: The following article describes all of the properties and elements within the toast content XML payload.
title: Toast 内容 XML 架构
ms.assetid: AF49EFAC-447E-44C3-93C3-CCBEDCF07D22
label: Toast content XML schema
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6b9535cd8c2dd82b0c209919080df9a88bb80ccc
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2018
ms.locfileid: "8750325"
---
# <a name="toast-content-xml-schema"></a>Toast 内容 XML 架构

 

以下内容介绍在 toast 内容 XML 负载内的所有属性和元素。

在以下 XML 架构中，“?”后缀表示属性为可选属性。

## <a name="ltvisualgt-and-ltaudiogt"></a>&lt;visual&gt; 和 &lt;audio&gt;

```
<toast launch? duration? activationType? scenario? >
  <visual lang? baseUri? addImageQuery? >
    <binding template? lang? baseUri? addImageQuery? >
      <text lang? hint-maxLines? >content</text>
      <image src placement? alt? addImageQuery? hint-crop? />
      <group>
        <subgroup hint-weight? hint-textStacking? >
          <text />
          <image />
        </subgroup>
      </group>
    </binding>
  </visual>
  <audio src? loop? silent? />
</toast>
```

**&lt;toast&gt; 中的属性**

launch?

-   launch? = string
-   这是可选属性。
-   当应用程序由 Toast 激活时向其传递的字符串。
-   根据 activationType 的值，此值可由前台中的应用在后台任务内接收或由从原始应用协议启动的另一个应用接收。
-   此字符串的格式和内容由应用根据其自身用途定义。
-   当用户点击或单击 Toast 来启动其关联应用时，启动字符串会向应用提供上下文，以允许该应用向用户显示与 Toast 内容相关的视图，而不是以其默认方式启动。
-   如果由于用户单击某个操作（而不是 Toast 的正文）而发生激活，开发人员会检索回在该 &lt;action&gt; 标记中预定义的“arguments”，而不是在 &lt;toast&gt; 标记中预定义的“launch”。

duration?

-   duration? = "short|long"
-   这是可选属性。 默认值为“short”。
-   此属性只适用于特定方案和 appCompat。 对于闹钟方案，你不再需要此属性。
-   我们不建议使用此属性。

activationType?

-   activationType? = "foreground | background | protocol | system"
-   这是可选属性。
-   默认值为“foreground”。

scenario?

-   scenario? = "default | alarm | reminder | incomingCall"
-   这是可选属性，默认值为“default”。
-   你不需要此属性，除非你的方案是弹出警报、提醒或来电。
-   不要将此属性仅用于使通知持续显示在屏幕上。

**&lt;visual&gt; 中的属性**

lang?

-   有关此可选属性的详细信息，请参阅[此元素架构文章](https://msdn.microsoft.com/library/windows/apps/br230847)。

baseUri?

-   有关此可选属性的详细信息，请参阅[此元素架构文章](https://msdn.microsoft.com/library/windows/apps/br230847)。

addImageQuery?

-   有关此可选属性的详细信息，请参阅[此元素架构文章](https://msdn.microsoft.com/library/windows/apps/br230847)。

**&lt;binding&gt; 中的属性**

template?

-   \[Important\] template? = "ToastGeneric"
-   如果你要使用任意新的自适应和交互式通知功能，请确保开始使用“ToastGeneric”模板而不是传统模板。
-   使用带有新操作的传统模板现在可能有效，但这不是预定用例，我们无法保证它会继续工作。

lang?

-   有关此可选属性的详细信息，请参阅[此元素架构文章](https://msdn.microsoft.com/library/windows/apps/br230847)。

baseUri?

-   有关此可选属性的详细信息，请参阅[此元素架构文章](https://msdn.microsoft.com/library/windows/apps/br230847)。

addImageQuery?

-   有关此可选属性的详细信息，请参阅[此元素架构文章](https://msdn.microsoft.com/library/windows/apps/br230847)。

**&lt;text&gt; 中的属性**

lang?

-   有关此可选属性的详细信息，请参阅[此元素架构文章](https://msdn.microsoft.com/library/windows/apps/br230847)。

**&lt;image&gt; 中的属性**

src

-   有关此必需属性的详细信息，请参阅[此元素架构文章](https://msdn.microsoft.com/library/windows/apps/br230844)。

placement?

-   placement? = "inline" | "appLogoOverride"
-   此属性是可选的。
-   这会指定将显示此图像的位置。
-   “inline”表示在 Toast 正文内部，在文本下； “appLogoOverride”表示替换应用程序图标（显示在 Toast 的左上角）。
-   对于每个位置值，你最多可以有一个图像。

alt?

-   有关此可选属性的详细信息，请参阅[此元素架构文章](https://msdn.microsoft.com/library/windows/apps/br230844)。

addImageQuery?

-   有关此可选属性的详细信息，请参阅[此元素架构文章](https://msdn.microsoft.com/library/windows/apps/br230844)。

hint-crop?

-   hint-crop? = "none" | "circle"
-   此属性是可选的。
-   “none”是默认值，表示没有裁剪。
-   “circle”将图像裁剪为圆形。 将此属性用于联系人的头像、用户图像等。

**&lt;audio&gt; 中的属性**

src?

-   有关此可选属性的详细信息，请参阅[此元素架构文章](https://msdn.microsoft.com/library/windows/apps/br230842)。

loop?

-   有关此可选属性的详细信息，请参阅[此元素架构文章](https://msdn.microsoft.com/library/windows/apps/br230842)。

silent?

-   有关此可选属性的详细信息，请参阅[此元素架构文章](https://msdn.microsoft.com/library/windows/apps/br230842)。

## <a name="schemas-ltactiongt"></a>架构：&lt;action&gt;


在以下 XML 架构中，“?”后缀表示属性为可选属性。

```
<toast>
  <visual>
  </visual>
  <audio />
  <actions>
    <input id type title? placeHolderContent? defaultInput? >
      <selection id content />
    </input>
    <action content arguments activationType? imageUri? hint-inputId />
  </actions>
</toast>
```

**&lt;input&gt; 中的属性**

id

-   id = string
-   此属性是必需的。
-   id 属性是必需的，并由开发人员用于在激活应用（在前台或后台）后检索用户输入。

type

-   type = "text | selection"
-   此属性是必需的。
-   它用于指定文本输入或预定义的选择列表中的输入。
-   在移动或桌面上，这用于指定你需要文本框输入还是列表框输入。

title?

-   title? = string
-   title 属性是可选的，它由开发人员用于为输入指定在出现提示时可供 shell 呈现的标题。
-   对于移动和桌面，此标题将显示在输入上方。

placeHolderContent?

-   placeHolderContent? = string
-   placeHolderContent 属性是可选的，并且是文本输入类型的灰色提示文本。 当输入类型不是“text”时，忽略此属性。

defaultInput?

-   defaultInput? = string
-   defaultInput 属性是可选的，用于提供默认输入值。
-   如果输入类型是“text”，将其视为字符串输入。
-   如果输入类型是“selection”，这应是此输入的元素内的可用选择之一的 id。

**&lt;selection&gt; 中的属性**

id

-   此属性是必需的。 它用于标识用户选择。 id 将返回到你的应用。

content

-   此属性是必需的。 它为此选择元素提供要显示的字符串。

**&lt;action&gt; 中的属性**

content

-   content = string
-   content 属性是必需的。 它提供在按钮上显示的文本字符串。

arguments

-   arguments = string
-   arguments 属性是必需的。 它描述应用定义的数据，在执行此操作的用户激活它以后应用可检索该数据。

activationType?

-   activationType? = "foreground | background | protocol | system"
-   activationType 属性是可选的，其默认值为“foreground”。
-   它描述此操作将导致的激活类型：前台、后台，或者通过协议启动来启动另一个应用或调用系统操作。

imageUri?

-   imageUri? = string
-   imageUri 是可选的，用于针对此操作提供一个图像图标，以在按钮内仅与文本内容一起显示。

hint-inputId

-   hint-inputId = string
-   hint-inpudId 属性是必需的。 它专门用于快速回复方案。
-   此值需要是要关联的输入元素的 id。
-   在移动和桌面中，这会将按钮放置在输入框的右侧。

## <a name="attributes-for-system-handled-actions"></a>用于系统处理的操作的属性


如果你不希望应用将通知的推迟/重新计划作为后台任务处理，系统可以处理推迟和取消通知的操作。 系统处理的操作可以组合（或单独指定），但我们不建议在没有取消操作的情况下实现推迟操作。

系统命令组合：SnoozeAndDismiss

```
<toast>
  <visual>
  </visual>
  <actions hint-systemCommands="SnoozeAndDismiss" />
</toast>
```

单独系统处理的操作

```
<toast>
  <visual>
  </visual>
  <actions>
  <input id="snoozeTime" type="selection" defaultInput="10">
    <selection id="5" content="5 minutes" />
    <selection id="10" content="10 minutes" />
    <selection id="20" content="20 minutes" />
    <selection id="30" content="30 minutes" />
    <selection id="60" content="1 hour" />
  </input>
  <action activationType="system" arguments="snooze" hint-inputId="snoozeTime" content=""/>
  <action activationType="system" arguments="dismiss" content=""/>
  </actions>
</toast>
```

若要构建单独的推迟和取消操作，请执行以下步骤：

-   Specify activationType = "system"
-   Specify arguments = "snooze" | "dismiss"
-   指定内容：
    -   如果希望操作上显示“snooze”和“dismiss”的本地化字符串，请将内容指定为空字符串：&lt;action content = ""/&gt;
    -   如果需要自定义字符串，只需提供其值：&lt;action content="Remind me later" /&gt;
-   指定输入：
    -   如果你不希望用户选择推迟间隔，而只是希望你的通知仅在系统定义的时间间隔内推迟一次（这在整个操作系统上都一致），则不要构建任何 &lt;input&gt;。
    -   如果你希望提供推迟间隔选择：
        -   在推迟操作中指定 hint-inputId
        -   将输入的 id 与推迟操作的 hint-inputId 相匹配：&lt;input id="snoozeTime"&gt;&lt;/input&gt;&lt;action hint-inputId="snoozeTime"/&gt;
        -   将选择 id 指定为以分钟为单位表示推迟间隔的 nonNegativeInteger：&lt;selection id="240" /&gt; 表示推迟 4 小时
        -   请确保 &lt;input&gt; 中的 defaultInput 值与 &lt;selection&gt; 子元素的 id 之一相匹配
        -   提供最多（但不多于）5 个 &lt;selection&gt; 值