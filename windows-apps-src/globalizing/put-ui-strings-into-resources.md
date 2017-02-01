---
author: DelfCo
Description: "将你的 UI 的字符串资源放入资源文件中。 随后你可从代码或标记中引用这些字符串。"
title: "将 UI 字符串放入资源中"
ms.assetid: E420B9BB-C0F6-4EC0-BA3A-BA2875B69722
label: Put UI strings into resources
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: b258771c887d4422433522344b11130b7e9ed1e6
ms.openlocfilehash: dff1e6df480a1ffc0e2441c78b942ccd0ee2126c

---

# <a name="put-ui-strings-into-resources"></a>将 UI 字符串放入资源中
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

将你的 UI 的字符串资源放入资源文件中。 随后你可从代码或标记中引用这些字符串。

<div class="important-apis" >
<b>重要的 API</b><br/>
<ul>
<li>[**ApplicationModel.Resources.ResourceLoader**](https://msdn.microsoft.com/library/windows/apps/br206014)</li>
<li>[**WinJS.Resources.processAll**](https://msdn.microsoft.com/library/windows/apps/br211864)</li>
</ul>
</div>


本主题介绍将多个语言字符串资源添加到通用 Windows 应用的步骤以及简要测试它的方法。

## <a name="put-strings-into-resource-files-instead-of-putting-them-directly-in-code-or-markup"></a>将字符串放入资源文件中，而不是直接将它们放入代码或标记中。


1.  在 Visual Studio 中打开你的解决方案（或创建新解决方案）。

2.  在 Visual Studio 中打开 package.appxmanifest、转到“应用程序”****选项卡，然后（在此示例中）将默认语言设置为“en-US”。 如果解决方案中有多个 package.appxmanifest 文件，请针对每个文件执行此操作。
    <br>**注意** 这将为项目指定默认语言。 如果用户的首选语言或显示语言与应用程序中提供的语言资源不匹配，将使用默认语言资源。
3.  创建用于包含资源文件的文件夹。
    1.  在“解决方案资源管理器”中，右键单击该项目（如果你的解决方案包含多个项目，则为共享的项目），然后依次选择“添加”****&gt;“新建文件夹”****。
    2.  将新文件夹命名为“Strings”。
    3.  如果没有在解决方案资源管理器中看见新文件夹，请在选中该项目时从 Microsoft Visual Studio 菜单中依次选择“项目”****&gt;“显示所有文件”****。

4.  创建一个子文件夹和用于英语（美国）的资源文件。
    1.  右键单击“Strings”文件夹，并在它下面添加新文件夹。 将它命名为“en-US”。 资源文件将放置在已为 [BCP-47](http://go.microsoft.com/fwlink/p/?linkid=227302) 语言标记命名的文件夹中。 有关语言限定符以及常用语言标记列表的详细信息，请参阅[如何使用限定符命名资源](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324)。
    2.  右键单击 en-US 文件夹，然后依次选择**添加**&gt;**新建项目…**。
    3.  选择“资源文件 (.resw)”。

    4.  单击**添加**。 这将添加一个带有默认名称“Resources.resw”的资源文件。 建议使用该默认文件名。 虽然应用可能会在其他文件内将其资源分区，但你必须谨慎地正确参考它们（请参阅[如何加载字符串资源](https://msdn.microsoft.com/library/windows/apps/xaml/hh965323)）。
    5.  如果你的 .resx 文件仅具有来自之前 .NET 项目的字符串资源，请依次选择**添加** &gt; **现有项目…**，添加 .resx 文件并将其重命名为 .resw。
    6.  打开该文件并使用编辑器来添加这些资源：


        Strings/en-US/Resources.resw ![add resource, english](images/addresource-en-us.png) 在此示例中，“Greeting.Text”和“Farewell”标识要显示的字符串。 “Greeting.Width”标识“Greeting”字符串的 Width 属性。 注释是为将字符串本地化为其他语言的翻译人员提供任何特殊说明的理想位置。

## <a name="associate-controls-to-resources"></a>将控件关联到资源。

需要将每个需要进行文本本地化的控件与 .resw 文件关联。 可以使用如下 XAML 元素的 **x:Uid** 属性执行此操作：

```XML
<TextBlock x:Uid="Greeting" Text="" />
```

对于资源名称，提供 **Uid** 属性值，再指定哪个属性将获得翻译的字符串（在本例中为 Text 属性）。 你可以为不同的语言指定其他属性/值，如 Greeting.Width，但对于此类与布局相关的属性要谨慎处理。 你应该尽力允许控件基于设备的屏幕动态布局。

请注意，附加属性在 resw 文件中的处理方式会有所不同（如 AutomationPeer.Name）。 你需要明确写出命名空间，如下所示：

```XML
MediumButton.[using:Windows.UI.Xaml.Automation]AutomationProperties.Name</code></pre></td>
```

## <a name="add-string-resource-identifiers-to-code-and-markup"></a>向代码和标记中添加资源标识符。

在你的代码中，可以动态地引用字符串：

**C#**
```CSharp
var loader = new Windows.ApplicationModel.Resources.ResourceLoader();
var str = loader.GetString("Farewell");
```

**C++**
```cpp
auto loader = ref new Windows::ApplicationModel::Resources::ResourceLoader();
auto str = loader->GetString("Farewell");
```


## <a name="add-folders-and-resource-files-for-two-additional-languages"></a>为其他两种语言添加文件夹和资源文件。


1.  为德语在 Strings 文件夹下添加另一个文件夹。 将德语（德国）的文件夹命名为“de-DE”。
2.  在 de-DE 文件夹中创建另一个资源文件，并添加以下内容：

    strings/de-DE/Resources.resw

    ![添加资源，德语](images/addresource-de-de.png)


3.  为法语（法国）创建另一个文件夹，命名为“fr-FR”。 创建一个新的资源文件，并添加以下内容：

    strings/fr-FR/Resources.resw ![add resource, french](images/addresource-fr-fr.png)

## <a name="build-and-run-the-app"></a>构建并运行应用。


为默认显示语言测试应用。

1.  按 F5 来构建并运行此应用。
2.  请注意，“greeting”和“farewell”以用户的首选语言显示。
3.  退出应用。

为其他语言测试应用。

1.  在设备上弹出“设置”****。
2.  选择“时间和语言”****。
3.  选择“区域和语言”****（或在手机或手机仿真器上，选择“语言”****）。
4.  请注意，运行应用时显示的语言是列出的最顶端语言，即英语、德语或法语。 如果你的最顶端语言不是这三者之一，则应用将采用应用支持的语言列表中的下一种语言。
5.  如果你的计算机上没有这三种语言，通过单击“添加语言”****并将其添加到列表中来添加缺少的语言。
6.  若要使用其他语言测试该应用，请在列表中选择该语言并单击“设置为默认值”****（或在手机或手机仿真器上，在列表中长按该语言，然后点击“向上移动”****直到它位于顶部）。 然后运行应用。

## <a name="related-topics"></a>相关主题


* [如何使用限定符命名资源](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324)
* [如何加载字符串资源](https://msdn.microsoft.com/library/windows/apps/xaml/hh965323)
* [BCP-47 语言标记](http://go.microsoft.com/fwlink/p/?linkid=227302)
 

 






<!--HONumber=Dec16_HO2-->


