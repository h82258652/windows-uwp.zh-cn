---
Description: 声音有助于完成应用程序的用户体验，并为他们提供匹配 Windows 在所有平台上的外观的额外音频边缘。
label: Sound
title: 声音
template: detail.hbs
ms.assetid: 9fa77494-2525-4491-8f26-dc733b6a18f6
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: mattben
dev-contact: joyate
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: ce81088495716e43a32dab94d1d47d399dd6a309
ms.sourcegitcommit: bf95c8b29145a224957a940512394e6aa97cb90f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/17/2019
ms.locfileid: "71061912"
---
# <a name="sound"></a>声音

![主图](images/header-sound.svg)

可通过许多方式来使用声音增强应用。 可以使用声音来补充其他 UI 元素，从而使用户能够依靠音频识别事件。 对于视觉障碍用户，声音是有效的用户界面元素。 可以使用声音来创造气氛，并使用户沉浸在这种气氛中；例如，可以在拼图游戏背景下播放怪异的原声带，或者将恐怖音效用于恐怖/求生游戏。

## <a name="examples"></a>示例

<table>
<th align="left">XAML 控件库<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果已安装 <strong style="font-weight: semi-bold">XAML 控件库</strong>应用，请单击此处<a href="xamlcontrolsgallery:/item/Sound">打开此应用，了解声音的实际应用</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="sound-global-api"></a>声音全局 API

UWP 提供了一个可轻松访问的声音系统，使你只需“翻转开关”即可在你的整个应用中获取沉浸式的音频体验。

[**ElementSoundPlayer**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.elementsoundplayer) 是 XAML 中的集成声音系统，并且在所有默认控件都打开时自动播放声音。
```C#
ElementSoundPlayer.State = ElementSoundPlayerState.On;
```
**ElementSoundPlayer** 有三种不同的状态：**开**、**关**和**自动**。

如果设置为“关”  ，无论你的应用在何处运行，都始终不会播放声音。 如果设置为“开”  ，你的应用将在每个平台上播放声音。

启用 ElementSoundPlayer 也将自动启用空间音频（3D 声音）。 若要禁用 3D 声音（同时仍保持声音为开启状态），请禁用 ElementSoundPlayer 的 **SpatialAudioMode**： 

```C#
ElementSoundPlayer.SpatialAudioMode = ElementSpatialAudioMode.Off
```

**SpatialAudioMode** 属性可以采用以下值： 
- **自动**：空间音频将在声音开启时启用。 
- **Off**:空间音频始终关闭，即使声音处于开启状态。
- **打开**：始终播放空间音频。

若要详细了解空间音频以及 XAML 如何处理它，请参阅 [AudioGraph - 空间音频](/windows/uwp/audio-video-camera/audio-graphs#spatial-audio)。

### <a name="sound-for-tv-and-xbox"></a>电视和 Xbox 的声音

声音是10 英尺体验的关键部分，并且默认情况下，**ElementSoundPlayer** 的状态为“自动”  ，这意味着只有当应用在 Xbox 上运行时才可获取声音。
若要了解有关针对 Xbox 和电视进行设计的详细信息，请参阅[针对 Xbox 和电视进行设计](https://go.microsoft.com/fwlink/?LinkId=760736)。

## <a name="sound-volume-override"></a>音量覆盖

应用内的所有声音均可通过“音量”  控件呈灰显状态。 但是，应用内的声音音量不能*比系统音量更加响亮*。

若要设置应用音量级别，请调用：
```C#
ElementSoundPlayer.Volume = 0.5;
```
最大音量（相对于系统卷）为 1.0，最小音量为 0.0（基本上无提示）。

## <a name="control-level-state"></a>控制级别状态

如果不想使用控件的默认声音，可以禁用它。 可以通过控件上的 **ElementSoundMode** 完成此操作。

**ElementSoundMode** 有两种状态：“关”  和“默认”  。 当不对其进行设置时，则为“默认”  。 如果设置为“关”  ，*除了焦点声音之外*，控件播放的每种声音都将静音。

```XAML
<Button Name="ButtonName" Content="More Info" ElementSoundMode="Off"/>
```

```C#
ButtonName.ElementSoundState = ElementSoundMode.Off;
```

## <a name="is-this-the-right-sound"></a>这是正确的声音吗？

在创建自定义控件或更改现有控件的声音时，请务必了解系统提供的所有声音的用法。

每种声音均与某些基本的用户交互相关，并且尽管可以自定义这些声音以在任何交互上播放，但是本节可用于说明所有 UWP 应用上的声音体验应该保持一致性的方案。

### <a name="invoking-an-element"></a>调用元素

现在在我们的系统上触发的最常见控件声音是“调用”  声音。 此声音在用户通过在游戏板上点击/单击/输入/空间或按下“A”按钮来调用控件时播放。

通常情况下，此声音仅当用户通过[输入设备](../input/index.md)明确定位简单控件或控件部件时才播放。


若要从任何控件事件播放此声音，只需从 **ElementSoundPlayer** 调用 Play 方法，然后传入 **ElementSound.Invoke**：
```C#
ElementSoundPlayer.Play(ElementSoundKind.Invoke);
```

### <a name="showing--hiding-content"></a>显示和隐藏内容

在 XAML 中有很多浮出控件、对话框和可闪退的 UI，并且触发这些叠加项之一的任何操作都应调用“显示”  或“隐藏”  声音。

当叠加内容窗口引入视图时，应调用“显示”  声音：

```C#
ElementSoundPlayer.Play(ElementSoundKind.Show);
```
反之，当叠加内容窗口关闭（或者闪退）时，应调用“隐藏”  声音：

```C#
ElementSoundPlayer.Play(ElementSoundKind.Hide);
```
### <a name="navigation-within-a-page"></a>页面内的导航

在应用页面内的面板或视图之间导航时（请参阅[表和透视表](../controls-and-patterns/pivot.md)），通常是双向移动。 这意味着你可以移动到下一个视图/面板或者上一个视图/面板，而无需离开你所在的当前的应用页面。

**MovePrevious** 和 **MoveNext** 声音包含围绕此导航概念的音频体验。

当移动到视其为列表中*下一项*的视图/面板时，请调用：

```C#
ElementSoundPlayer.Play(ElementSoundKind.MoveNext);
```
此外，当移动到视其为列表中*上一项*的上一个视图/面板时，请调用：

```C#
ElementSoundPlayer.Play(ElementSoundKind.MovePrevious);
```
### <a name="back-navigation"></a>后退导航

当在应用内从当前页面导航到之前页面时，应调用 **GoBack** 声音：

```C#
ElementSoundPlayer.Play(ElementSoundKind.GoBack);
```
### <a name="focusing-on-an-element"></a>专注于元素

**焦点**声音仅在我们的系统中为隐式声音。 这意味着用户不直接与任何内容进行交互，但仍能听到声音。

当用户在应用内导航时发生聚焦，这可能与游戏板/键盘/遥控器或支架有关。 通常“焦点”  声音*不在 PointerEntered 或鼠标悬停事件上播放*。

若要在控件接收焦点时设置控件以播放“焦点”  声音，请调用：

```C#
ElementSoundPlayer.Play(ElementSoundKind.Focus);
```
### <a name="cycling-focus-sounds"></a>循环焦点声音

作为一项调用 **ElementSound.Focus** 的添加功能，声音系统将在每个导航触发器上默认循环播放 4 种不同的声音。 这意味着其中两种焦点声音播放后将不会确定即将播放的其他两种声音。

隐藏此循环功能的目的是为了防止焦点声音变得单调乏味以及防止被用户注意到；焦点声音播放次数最多，并且应该是最微妙的。

## <a name="get-the-sample-code"></a>获取示例代码

- [XAML 控件库示例](https://github.com/Microsoft/Xaml-Controls-Gallery) - 以交互式格式查看所有 XAML 控件。

## <a name="related-articles"></a>相关文章

* [针对 Xbox 和电视进行设计](/windows/uwp/design/devices/designing-for-tv)
* [ElementSoundPlayer 类文档](/uwp/api/windows.ui.xaml.elementsoundplayer)
