---
将你的 UI 中的字符串资源放入资源文件中。 随后你可从代码或标记中引用这些字符串。
将 UI 字符串放入资源中
ms.assetid: E420B9BB-C0F6-4EC0-BA3A-BA2875B69722
将 UI 字符串放入资源中
template: detail.hbs
---

# 将 UI 字符串放入资源中


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**重要的 API**

-   [**ApplicationModel.Resources.ResourceLoader**](https://msdn.microsoft.com/library/windows/apps/br206014)
-   [**WinJS.Resources.processAll**](https://msdn.microsoft.com/library/windows/apps/br211864)

将你的 UI 中的字符串资源放入资源文件中。 随后你可从代码或标记引用这些字符串。

本主题介绍了将多个语言字符串资源添加到通用 Windows 应用的步骤以及简短测试它的方法。

## <span id="put_strings_into_resource_files__instead_of_putting_them_directly_in_code_or_markup."> </span> <span id="PUT_STRINGS_INTO_RESOURCE_FILES__INSTEAD_OF_PUTTING_THEM_DIRECTLY_IN_CODE_OR_MARKUP."> </span>将字符串放入资源文件中，而不是直接将它们放入代码或标记中。


1.  在 Visual Studio 中打开你的解决方案（或创建新解决方案）。

2.  在 Visual Studio 中打开 package.appxmanifest、转到**“应用程序”**选项卡，然后（在此示例中）将默认语言设置为“en-US”。 如果解决方案中有多个 package.appxmanifest 文件，请针对每个文件执行此操作。
    <br>**注意** 这将为项目指定默认语言。 如果用户的首选语言或显示语言与应用程序中提供的语言资源不匹配，将使用默认语言资源。
3.  创建用于包含资源文件的文件夹。
    1.  在“解决方案资源管理器”中，右键单击该项目（如果你的解决方案包含多个项目，则为共享的项目），然后依次选择**“添加”**>**“新文件夹”**。
    2.  将新文件夹命名为“Strings”。
    3.  如果没有在解决方案资源管理器中看见新文件夹，请在选中该项目时从 Microsoft Visual Studio 菜单中依次选择**“项目”**>**“显示所有文件”**。

4.  创建一个子文件夹和用于英语(美国)的资源文件。
    1.  右键单击“Strings”文件夹，并在它下面添加新文件夹。 将它命名为“en-US”。 资源文件将放置在已为 [BCP-47](http://go.microsoft.com/fwlink/p/?linkid=227302) 语言标记命名的文件夹中。 有关语言限定符以及常用语言标记列表的详细信息，请参阅[如何使用限定符命名资源](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324)。
    2.  右键单击 en-US 文件夹，然后依次选择**“添加”**>**“新建项目…”**。
    3.  **XAML：**选择“资源文件 (.resw)”。
        <br>**HTML：**选择“资源文件 (.resjson)”。

    4.  单击**“添加”**。 这将添加一个带有默认名称“Resources.resw”（对于 **XAML**）或“resources.rejson”（对于 **HTML**）的资源文件。 建议使用该默认文件名。 虽然应用可能会在其他文件内将其资源分区，但你必须谨慎地正确引用它们（请参阅[如何加载字符串资源](https://msdn.microsoft.com/library/windows/apps/xaml/hh965323)）。
    5.  **仅 XAML：**如果你的 .resx 文件仅具有来自之前 .NET 项目的字符串资源，请依次选择**“添加”**>**“现有项目…”**、添加 .resx 文件，然后将其重命名为 .resw。
    6.  打开该文件并使用编辑器来添加这些资源：

        **XAML：**

        Strings/en-US/Resources.resw
        ![添加资源, 英语](images/addresource-en-us.png)
        在此示例中，“Greeting.Text”和“Farewell”标识要显示的字符串。 “Greeting.Width”标识“Greeting”字符串的 Width 属性。 注释是为将字符串本地化为其他语言的翻译人员提供任何特殊说明的理想位置。

        **HTML：**

        新文件包含默认内容。 将该内容替换为以下内容（这可能与默认内容类似）：

        Strings/en-US/resources.resjson

        ```        json
        {
                "greeting"              : "Hello",
                "_greeting.comment"     : "A welcome greeting",

                "farewell"              : "Goodbye",
                "_farewell.comment"     : "A goodbye"
        }
        ```

        这是严格的 JavaScript 对象表示法 (JSON) 语法，其中每个名称/值对后面都必须放置一个逗号（最后一对除外）。 在此示例中，“greeting”和“farewell”将标识要显示的字符串。 其他对（“\_greeting.comment”和“\_farewell.comment”）是用于描述字符串的注释。 注释是为将字符串本地化为其他语言的翻译人员提供任何特殊说明的理想位置。

## <span id="associate_controls_to_resources."> </span> <span id="ASSOCIATE_CONTROLS_TO_RESOURCES."> </span>将控件关联到资源。


**仅 XAML：**

需要将每个需要进行文本本地化的控件与 .resw 文件关联。 可以使用如下 XAML 元素的 **x:Uid** 属性执行此操作：

```XAML
<TextBlock x:Uid="Greeting" Text="" />
```

对于资源名称，提供 **Uid** 属性值，再指定哪个属性将获得翻译的字符串（在本例中为 Text 属性）。 你可以为不同的语言指定其他属性/值，如 Greeting.Width，但对于此类与布局相关的属性要谨慎处理。 你应该尽力允许控件基于设备的屏幕动态布局。

请注意，附加属性在 resw 文件中的处理方式会有所不同（如 AutomationPeer.Name）。 你需要明确写出命名空间，如下所示：

```XAML
MediumButton.[using:Windows.UI.Xaml.Automation]AutomationProperties.Name</code></pre></td>
```

## <span id="add_string_resource_identifiers_to_code_and_markup."> </span> <span id="ADD_STRING_RESOURCE_IDENTIFIERS_TO_CODE_AND_MARKUP."> </span>向代码和标记中添加字符串资源标识符。


**XAML：**

在你的代码中，可以动态地引用字符串：

**C#**
```CSharp
var loader = new Windows.ApplicationModel.Resources.ResourceLoader();
var str = loader.GetString("Farewell");
```

**C++**
```ManagedCPlusPlus
auto loader = ref new Windows::ApplicationModel::Resources::ResourceLoader();
auto str = loader->GetString("Farewell");
```

**HTML：**

1.  将对 Windows JavaScript 库的引用添加到你的 HTML 文件（如果其中还没有这些引用）。

    **注意** 以下代码显示的是你在 Visual Studio 中创建新的**“空白应用(通用 Windows)”**JavaScript 项目时所生成的 Windows 项目的 default.html 文件 HTML。 请注意，该文件已经包含了对于 WinJS 的引用。

    ```    HTML
    <!-- WinJS references -->
    <link href="WinJS/css/ui-dark.css" rel="stylesheet" />
    <script src="WinJS/js/base.js"></script>
    <script src="WinJS/js/ui.js"></script>
    ```

2.  在 HTML 文件附带的 JavaScript 代码中，当加载 HTML 时调用 [**WinJS.Resources.processAll**](https://msdn.microsoft.com/library/windows/apps/br211864) 函数。

    ```    JavaScript
    WinJS.Application.onloaded = function(){
        WinJS.Resources.processAll();
    }
    ```
    
    如果将其他 HTML 加载到 [**WinJS.UI.Pages.PageControl**](https://msdn.microsoft.com/library/windows/apps/jj126158) 对象，则在页面控件的 [**IPageControlMembers.ready**](https://msdn.microsoft.com/library/windows/apps/hh770590) 方法中调用 [**WinJS.Resources.processAll**](https://msdn.microsoft.com/library/windows/apps/br211864)(*element*)，其中 *element* 是要加载的 HTML 元素（及其子元素）。 本示例基于[应用程序资源和本地化示例](http://go.microsoft.com/fwlink/p/?linkid=227301)的方案 6：

    ```    JavaScript
    var output;
    
    var page = WinJS.UI.Pages.define("/html/scenario6.html", {
        ready: function (element, options) {
            output = element.querySelector('#output');
            WinJS.Resources.processAll(output); // Refetch string resources
            WinJS.Resources.addEventListener("contextchanged", refresh, false);
        }
        unload: function () { 
            WinJS.Resources.removeEventListener("contextchanged", refresh, false); 
        } 
    });
    ```

3.  在 HTML 中，使用资源标识符（“greeting”和“farewell”）引用资源文件中的字符串资源。
    ```    HTML
    <h2><span data-win-res="{textContent: 'greeting';}"></span></h2>
    <h2><span data-win-res="{textContent: 'farewell'}"></span></h2>
    ```

4.  引用针对属性的字符串资源。

    ```    HTML
    <div data-win-res="{attributes: {'aria-label'; : 'String1'}}" >
    ```

    用于 HTML 替换的 data-win-res 属性的一般模式为 data-win-res="{*propertyname1*: '*resource ID*', *propertyname2*: '*resource ID2*'}"。

    **注意** 如果字符串不包含任何标记，则尽可能将资源绑定到 textContent 属性（而非 innerHTML）。 textContent 属性比 innerHTML 的替换速度快得多。

5.  引用 JavaScript 中的字符串资源。
    <span codelanguage="JavaScript"></span>
    ```    JavaScript
    var el = element.querySelector('#header');
    var res = WinJS.Resources.getString('greeting');
    el.textContent = res.value;
    el.setAttribute('lang', res.lang);
    ```

## <span id="add_folders_and_resource_files_for_two_additional_languages."> </span> <span id="ADD_FOLDERS_AND_RESOURCE_FILES_FOR_TWO_ADDITIONAL_LANGUAGES."> </span>为其他两种语言添加文件夹和资源文件。


1.  为德语在 Strings 文件夹下添加另一个文件夹。 将德语（德国）的文件夹命名为“de-DE”。
2.  在 de-DE 文件夹中创建另一个资源文件，并添加以下内容：

    **XAML：**

    strings/de-DE/Resources.resw

    ![添加资源, 德语](images/addresource-de-de.png)

    **HTML：**

    strings/de-DE/resources.resjson

    ```    json
    {
        "greeting"              : "Hallo",
        "_greeting.comment"     : "A welcome greeting.",

        "farewell"              : "Auf Wiedersehen",
        "_farewell.comment"     : "A goodbye."
    }
    ```

3.  为法语（法国）创建另一个文件夹，命名为“fr-FR”。 创建一个新的资源文件，并添加以下内容：

    **XAML：**

    strings/fr-FR/Resources.resw
    ![添加资源, 法语](images/addresource-fr-fr.png)
    **HTML：**

    strings/fr-FR/resources.resjson

    ```    json
    {
        "greeting"              : "Bonjour",
        "_greeting.comment"     : "A welcome greeting.",

        "farewell"              : "Au revoir",
        "_farewell.comment"     : "A goodbye."
    }
    ```

## <span id="build_and_run_the_app."> </span> <span id="BUILD_AND_RUN_THE_APP."> </span>生成并运行应用


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

## <span id="related_topics"> </span>相关主题


* [如何使用限定符命名资源](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324)
* [如何加载字符串资源](https://msdn.microsoft.com/library/windows/apps/xaml/hh965323)
* [BCP-47 语言标记](http://go.microsoft.com/fwlink/p/?linkid=227302)
 

 





<!--HONumber=Mar16_HO1-->


