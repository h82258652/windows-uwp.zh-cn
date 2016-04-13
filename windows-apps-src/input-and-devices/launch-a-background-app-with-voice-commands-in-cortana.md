---
Description: 除了在 Cortana 内使用语音命令访问系统功能外，你还可以使用可指定要在应用内执行的操作或命令的语音命令，通过后台应用中的特性和功能扩展 Cortana。
title: 在 Cortana 中使用语音命令启动后台应用
ms.assetid: DF5B530C-57DD-4CA5-B3BE-1A0B3695C9C6
label: Launch a background app
template: detail.hbs
---

# 通过 Cortana 使用语音命令激活后台应用


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 的文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**重要的 API**

-   [**Windows.ApplicationModel.VoiceCommands**](https://msdn.microsoft.com/library/windows/apps/dn706594)
-   [**VCD 元素和属性 v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)

除了在 **Cortana** 内使用语音命令访问系统功能外，你还可以使用可指定要执行的操作或命令的语音命令，通过应用（作为后台任务）中的特性和功能扩展 **Cortana**。 当应用在后台处理语音命令时，它不会获得焦点。 相反，它会通过 **Cortana** 画布和 **Cortana** 语音返回所有反馈和结果。

应用可以在前台激活（应用获取焦点），也可以在后台激活（**Cortana** 保留焦点），具体取决于交互的复杂程度。 例如，需要其他上下文或用户输入（例如将消息发送给特定联系人）的语音命令最好通过前台应用处理，而基本命令（如列出即将到来的旅行）可以通过后台应用使用 **Cortana** 处理。

如果你希望使用语音命令在前台激活应用，请参阅[通过 Cortana 使用语音命令激活前台应用](launch-a-foreground-app-with-voice-commands-in-cortana.md)。

> **注意**  
> 语音命令是在语音命令定义 (VCD) 文件中定义的具有特定意图的单个发音，通过 **Cortana** 指向一个已安装的应用。

> VCD 文件定义一个或多个语音命令，每个语音命令都具有一种特殊意图。

> 语音命令定义的复杂程度可能有所不同。 它可以支持任何语音命令，从单个受限发音到一组更灵活的自然语言发音，所有发音都表示相同的意图。

为了演示后台应用功能，我们将使用 [Cortana 语音命令示例](http://go.microsoft.com/fwlink/p/?LinkID=619899)中的名为 **Adventure Works** 的旅行规划和管理应用。

以下是与 **Cortana** 画布集成的 **Adventure Works** 应用的概述。

![Cortana 画布概述 ](images/cortana-overview.png)

若要在没有 **Cortana** 的情况下查看 **Adventure Works** 旅行，用户需要启动该应用并导航到**“新旅行”**页面。

使用语音命令通过 **Cortana** 在后台启动应用，用户只需说出“Adventure Works，我去拉斯维加斯的旅行是什么时候？”。 应用将处理命令，然后 **Cortana** 将显示结果以及应用图标和其他应用信息（如果提供）。 以下是一个基本旅行查询和 **Cortana** 结果屏幕的示例，屏幕同时显示并说出“你下次到拉斯维加斯的旅行在八月一日”。

![在后台使用 Adventure Works 应用的基本查询和结果屏幕](images/cortana-backgroundapp-result.png)

以下是使用语音或键盘输入添加语音命令功能并通过应用中的后台功能扩展 **Cortana** 的基本步骤：

1.  创建 **Cortana** 在后台调用的应用服务（请参阅 [**Windows.ApplicationModel.AppService**](https://msdn.microsoft.com/library/windows/apps/dn921731)）。
2.  创建 VCD 文件。 这是一个 XML 文档，可以定义在激活应用时用户可说出以启动操作或调用命令的所有语音命令。 请参阅 [**VCD elements and attributes v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)。
3.  当启动应用时，在 VCD 文件中注册命令集。
4.  处理应用服务的后台激活和语音命令的执行。
5.  在 **Cortana** 内显示并说出对语音命令的相应反馈。

**先决条件：**

如果你还不熟悉通用 Windows 平台 (UWP) 应用开发，请仔细阅读这些主题来熟悉此处讨论的技术。

-   [创建你的第一个应用](https://msdn.microsoft.com/library/windows/apps/bg124288)
-   借助[事件和路由事件概述](https://msdn.microsoft.com/library/windows/apps/mt185584)了解事件

**用户体验指南：**

有关如何将你的应用与 **Cortana** 集成的信息，请参阅 [Cortana 设计指南](https://msdn.microsoft.com/library/windows/apps/dn974233)；有关设计出既实用又有吸引力且支持语音的应用的有用提示，请参阅[语音设计指南](https://msdn.microsoft.com/library/windows/apps/dn596121)。

## <span id="Create_a_new_solution_with_a_primary_project_in_Visual_Studio"> </span> <span id="create_a_new_solution_with_a_primary_project_in_visual_studio"> </span> <span id="CREATE_A_NEW_SOLUTION_WITH_A_PRIMARY_PROJECT_IN_VISUAL_STUDIO"> </span>使用主项目在 Visual Studio 中创建一个新解决方案


1.  启动 Microsoft Visual Studio 2015。

    将出现 Visual Studio 2015 起始页。

2.  在“文件”****菜单上，依次选择“新建”****>“项目”****。

    会出现“新建项目”****对话框。 可以在对话框的左侧窗格中选择要显示模板的类型。

3.  在左侧窗格中，依次展开**“已安装”>“模板”>“Visual C\#”>“Windows”**，然后选取**“通用”**模板组。 对话框的中心窗格会显示适用于通用 Windows 平台 (UWP) 应用的项目模板的列表。
4.  在中心窗格中，选择“空白应用(通用 Windows)”****模板。

    “空白应用”****模板会创建一个最基本的 UWP 应用，该应用可以编译和运行，但不包含任何用户界面控件或数据。 本教程将指导你向该应用添加控件。

5.  在**“名称”**文本框中，键入项目名称。 在此示例中，我们使用“AdventureWorks”。
6.  单击**“确定”**可创建项目。

    Microsoft Visual Studio 将创建项目并在**“解决方案资源管理器”**中显示该项目。


## <span id="Add_image_assets_to_primary_project_and_specify_them_in_the_app_manifest"> </span> <span id="add_image_assets_to_primary_project_and_specify_them_in_the_app_manifest"> </span> <span id="ADD_IMAGE_ASSETS_TO_PRIMARY_PROJECT_AND_SPECIFY_THEM_IN_THE_APP_MANIFEST"> </span>将图像资源添加到主项目，并在应用清单中指定它们
      
UWP 应用可以基于特定设置和设备功能（高对比度、有效像素、区域设置等）自动选择最合适的图像。 你只需提供图像，并确保在不同资源版本的应用项目中使用相应的命名约定和文件夹组织。 如果未能提供推荐的资源版本，辅助功能、本地化和图像质量将受到影响，具体取决于用户首选项、功能、设备类型和位置。

有关高对比度和比例系数的图像资源的更多详细信息，请参阅[磁贴和图标资源指南](https://msdn.microsoft.com/en-us/windows/uwp/controls-and-patterns/tiles-and-notifications-app-assets)。

使用限定符命名资源。 资源限定符是一种文件夹和文件名修饰符，可标识应使用一种特定资源版本的上下文。

标准命名约定是 `foldername/qualifiername-value[_qualifiername-value]/filename.qualifiername-value[_qualifiername-value].ext`。 例如，`images/en-US/logo.scale-100_contrast-white.png`，可以在代码中仅使用根文件夹和文件名指代：`images/logo.png`。 请参阅[如何使用限定符命名资源](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh965324.aspx)。

我们建议在字符串资源文件（如 `en-US\resources.resw`）上标记默认语言，在图像（如 `logo.scale-100.png`）上标记默认比例系数，即使你当前不计划提供本地化或多种分辨率的资源也是如此。 但是，我们建议你至少为 100、200 和 400 比例系数提供资源。

> *重要提示

> 在 **Cortana** 画布的标题区域使用的应用图标是在“Package.appxmanifest”文件中指定的 44x44 方形徽标图标。 

> 你还可以在 **Cortana** 画布的内容区域显示的查询中指定每个项的图标。 结果图标的有效图像大小为：

> - 68w x 68h 
> - 68w x 92h 
> - 280w x 140h 

在 [**VoiceCommandResponse**](https://msdn.microsoft.com/library/windows/apps/dn974182) 传递给 [**VoiceCommandServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn974204) 之前，不会验证该内容磁贴。 如果你将 **VoiceCommandResponse** 对象传递给包含内容磁贴的 **Cortana**，并且该磁贴带有不遵守这些大小比率的图像，则可能发生异常。 

在 **Adventure Works** 应用 (VoiceCommandService\\AdventureWorksVoiceCommandService.cs) 中的本示例中，我们使用 [**TitleWith68x68IconAndText**](https://msdn.microsoft.com/library/windows/apps/dn974169) 磁贴模板在 [**VoiceCommandContentTile**](https://msdn.microsoft.com/library/windows/apps/dn974168) 上指定一个简单灰色正方形（“GreyTile.png”）。 徽标变量位于 VoiceCommandService\\Images 中，并且可使用 [**GetFileFromApplicationUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701741) 方法对其进行检索。

```CSharp
var destinationTile = new VoiceCommandContentTile();

destinationTile.ContentTileType = 
  VoiceCommandContentTileType.TitleWith68x68IconAndText;
destinationTile.Image = 
  await StorageFile.GetFileFromApplicationUriAsync(
    new Uri("ms-appx:///AdventureWorks.VoiceCommands/Images/GreyTile.png"));
```

## <span id="Create_an_app_service_project"> </span> <span id="create_an_app_service_project"> </span> <span id="CREATE_AN_APP_SERVICE_PROJECT"> </span>创建应用服务项目

<ol>
    <li>
    Right-click your Solution name, select **New > Project**.
    </li>
    <li>
    Under **Installed > Templates > Visual C# > Windows > Universal**, select **Windows Runtime Component**. This is the component that implements the app service (**[Windows.ApplicationModel.AppService](https://msdn.microsoft.com/library/windows/apps/dn921731)**).
    </li>
    <li>
    Type a name for the project (for example, "VoiceCommandService") and click **OK**.
    </li>
    <li>
    In **Solution Explorer**, select the "VoiceCommandService" project and rename the "Class1.cs" file generated by Visual Studio. For the **Adventure Works** example we use "AdventureWorksVoiceCommandService.cs".
    </li>
    <li>
    Click **Yes** when asked if you want to rename all occurrences of "Class1.cs". 
    </li>
    <li>
    In the "AdventureWorksVoiceCommandService.cs" file:
        <ol type="i">
 <li>
 添加以下 using 指令。  
 ```using Windows.ApplicationModel.Background;```
 </li>
 <li>
 当你创建新项目时，项目名称将用作所有文件中的默认根命名空间。 重命名该命名空间以将应用服务代码嵌套在主项目下。 例如，`namespace AdventureWorks.VoiceCommands`。 
 </li>
 <li>
 在“解决方案资源管理器”中，右键单击该应用服务项目名称，然后选择**“属性”**。 
 </li>
 <li>
 在**“库”**选项卡上，使用此相同值更新**“默认命名空间”**字段（在此示例中为“AdventureWorks.VoiceCommands”）。 
 </li>
 <li>
 创建一个用于实现 [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794) 接口的新类。 此类需要一个 [**Run**](https://msdn.microsoft.com/library/windows/apps/br224811) 方法，该方法是 Cortana 识别语音命令时的入口点。 
 </li>
        </ol>
    </li>
</ol>

以下是 **Adventure Works** 应用中的基本后台任务类。 稍后我们将补充更多详细信息。
> **注意**   
> 后台任务类本身和后台任务项目中的所有其他类都需要是密封公共类。
 
``` csharp
namespace AdventureWorks.VoiceCommands
{
    ...
    
    /// <summary>
    /// The VoiceCommandService implements the entry point for all voice commands.
    /// The individual commands supported are described in the VCD xml file. 
    /// The service entry point is defined in the appxmanifest.
    /// </summary>
    public sealed class AdventureWorksVoiceCommandService : IBackgroundTask
    {
        ...
        
        /// <summary>
        /// The background task entrypoint. 
        /// 
        /// Background tasks must respond to activation by Cortana within 0.5 seconds, and must 
        /// report progress to Cortana every 5 seconds (unless Cortana is waiting for user
        /// input). There is no execution time limit on the background task managed by Cortana,
        /// but developers should use plmdebug (https://msdn.microsoft.com/en-us/library/windows/hardware/jj680085%28v=vs.85%29.aspx)
        /// on the Cortana app package in order to prevent Cortana timing out the task during
        /// debugging.
        /// 
        /// The Cortana UI is dismissed if Cortana loses focus. 
        /// The background task is also dismissed even if being debugged. 
        /// Use of Remote Debugging is recommended in order to debug background task behaviors. 
        /// Open the project properties for the app package (not the background task project), 
        /// and enable Debug -> "Do not launch, but debug my code when it starts". 
        /// Alternatively, add a long initial progress screen, and attach to the background task process while it executes.
        /// </summary>
        /// <param name="taskInstance">Connection to the hosting background service process.</param>
        public void Run(IBackgroundTaskInstance taskInstance)
        {
        
          //
          // TODO: Insert code 
          //
          //
        
    }        
  }
}
```

<ol start="7">
    <li>
    Declare your background task as an **AppService** in the app manifest.
    <ol type="i">
        <li>
        In **Solution Explorer**, right click the "Package.appxmanifest" file and select **View Code**. 
        </li>
        <li>
        Find the [**Application**](https://msdn.microsoft.com/library/windows/apps/dn934738) element.
        </li>
        <li>
        Add an [**Extensions**](https://msdn.microsoft.com/library/windows/apps/dn934720) element to the [**Application**](https://msdn.microsoft.com/library/windows/apps/dn934738) element.
        </li>
        <li>
        Add a [**uap:Extension**](https://msdn.microsoft.com/library/windows/apps/dn986788) element to the [**Extensions**](https://msdn.microsoft.com/library/windows/apps/dn934720) element.
        </li>
        <li>将 **Category** 属性添加到 **uap:Extension** 元素，并将 **Category** 属性的值设置为“windows.appService”。
        </li>
        <li>
        Add an **EntryPoint** attribute to the **uap:Extension** element and set the value of the **EntryPoint** attribute to the name of the class that implements [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794), in this case "AdventureWorks.VoiceCommands.AdventureWorksVoiceCommandService".
        </li>
        <li>
        Add a [**uap:AppService**](https://msdn.microsoft.com/library/windows/apps/dn934779) element to the **uap:Extension** element.
        </li>
        <li>
        Add a **Name** attribute to the [**uap:AppService**](https://msdn.microsoft.com/library/windows/apps/dn934779) element and set the value of the **Name** attribute to a name for the app service, in this case "AdventureWorksVoiceCommandService".
        </li>
        <li>
        Add a second [**uap:Extension**](https://msdn.microsoft.com/library/windows/apps/dn986788) element to [**Extensions**](https://msdn.microsoft.com/library/windows/apps/dn934720).
        </li>
        <li>
        Add a **Category** attribute to this [**uap:Extension**](https://msdn.microsoft.com/library/windows/apps/dn986788) element and set the value of the **Category** attribute to "windows.personalAssistantLaunch".
        </li>
    </li> 
    </ol>
    </li>    
</ol>  

下面是 Adventure Works 应用中的清单：
```xml
<Package>
  <Applications>
    <Application>

      <Extensions>
        <uap:Extension Category="windows.appService" 
          EntryPoint="CortanaBack1.VoiceCommands.CortanaBack1VoiceCommandService">
          <uap:AppService Name="CortanaBack1VoiceCommandService"/>
        </uap:Extension>
        <uap:Extension Category="windows.personalAssistantLaunch"/>
      </Extensions>

    <Application>
  <Applications>
</Package>
```

<ol start="8">
    <li>
    Add this app service project as a reference in the primary project. 
    <ol type="i">
        <li>
        Right click **References**. 
        </li>
        <li>
        Select **Add Reference...** 
        </li>
        <li>
        In the **Reference Manager** dialog, expand **Projects** and select the app service project. 
        </li>
        <li>
        Click OK. 
        </li>
    </ol>
    </li>
</ol>

## <span id="Create_a_VCD_file"> </span> <span id="create_a_vcd_file"> </span> <span id="CREATE_A_VCD_FILE"> </span>创建 VCD 文件


1. 在 Visual Studio 中，右键单击你的主项目名称，然后依次选择**“添加”>“新建项目”**。 添加 **XML 文件**。
2. 为 [**VCD**](https://msdn.microsoft.com/library/windows/apps/dn706593) 文件键入名称（在本示例中为“AdventureWorksCommands.xml”），然后单击“添加”。 
3. 在**“解决方案资源管理器”**中，选择 [**VCD**](https://msdn.microsoft.com/library/windows/apps/dn706593) 文件。
4.  在**“属性”**窗口中，将**“生成操作”**设置为**“内容”**，然后将**“复制到输出目录”**设置为**“如果较新则复制”**。

## <span id="Edit_the_VCD_file"> </span> <span id="edit_the_vcd_file"> </span> <span id="EDIT_THE_VCD_FILE"> </span>编辑 VCD 文件

1. 添加一个带有指向“http://schemas.microsoft.com/voicecommands/1.2”的 **xmlns** 属性的 **VoiceCommands** 元素。

2. 针对你的应用支持的每种语言，创建一个包含你的应用支持的语音命令的 [**CommandSet**](https://msdn.microsoft.com/library/windows/apps/dn722331) 元素。

  你可以声明多个 [**CommandSet**](https://msdn.microsoft.com/library/windows/apps/dn722331) 元素，每个都带有不同的 [**xml:lang**](https://msdn.microsoft.com/library/windows/apps/dn722331) 属性以使你的应用可用于不同的市场。 例如，用于美国的应用可能有一个英语版本的 [**CommandSet**](https://msdn.microsoft.com/library/windows/apps/dn722331) 和一个西班牙语版本的 [**CommandSet**](https://msdn.microsoft.com/library/windows/apps/dn722331)。

  >  **注意**  
  为了激活应用并使用语音命令启动操作，该应用必须注册一个 VCD 文件，此文件包含带有与用户为其设备选择的语音语言相匹配的语言的 [**CommandSet**](https://msdn.microsoft.com/library/windows/apps/dn722331)。 语音语言位于**“设置”>“系统”>“语音”>“语音语言”**中。

3. 为要支持的每个命令添加 **Command** 元素。

  [
            **VCD**](https://msdn.microsoft.com/library/windows/apps/dn706593) 文件中声明的每个 **Command** 都必须包含以下信息：

  - 你的应用程序用于在运行时标识语音命令的 **Name** 属性。 
  - **Example** 元素，其中包含一个描述用户可以如何调用命令的短语。 当用户说“我能说什么？”、“帮助”或当他们点击**“查看详细信息”**时，**Cortana** 会显示此示例。    
  -   **ListenFor** 元素，其中包含你的应用识别为命令的字词或短语。 每个 **ListenFor** 元素都可以包含对一个或多个包含与该命令相关的特定字词的 **PhraseList** 元素的引用。
  > **注意**  
  **ListenFor** 元素无法以编程方式修改。 但是，与 **ListenFor** 元素相关联的 **PhraseList** 元素可以以编程方式修改。 应用程序应该基于用户在使用应用时所生成的数据集在运行时修改 **PhraseList** 的内容。 请参阅[动态修改语音命令定义 (VCD) 短语列表](dynamically-modify-voice-command-definition--vcd--phrase-lists.md)。

  -   **Feedback** 元素，其中包含在启动应用程序时供 **Cortana** 显示并说出的文本。

**Navigate** 元素，用于指示将应用激活到前台的语音命令。 在此示例中，```showTripToDestination``` 命令是前台任务。

**VoiceCommandService** 元素，用于指示在后台激活应用的语音命令。 此元素的 **Target** 属性值应与 package.appxmanifest 文件中的 [**uap:AppService**](https://msdn.microsoft.com/library/windows/apps/dn934779) 元素的 **Name** 属性值匹配。 在此示例中，```whenIsTripToDestination``` 和 ```cancelTripToDestination``` 命令是后台任务，用于将应用服务的名称指定为“AdventureWorksVoiceCommandService”。

有关更多详细信息，请参阅 [**VCD 元素和属性 v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593) 参考。

下面是 [**VCD**](https://msdn.microsoft.com/library/windows/apps/dn706593) 文件的一部分，该文件用于为 **Adventure Works** 应用定义 zh-cn 语音命令。

```xml
<?xml version="1.0" encoding="utf-8" ?>
<VoiceCommands xmlns="http://schemas.microsoft.com/voicecommands/1.2">
  <CommandSet xml:lang="en-us" Name="AdventureWorksCommandSet_en-us">
    <AppName> Adventure Works </AppName>
    <Example> Show trip to London </Example>

    <Command Name="showTripToDestination">
      <Example> Show trip to London </Example>
      <ListenFor RequireAppName="BeforeOrAfterPhrase"> show [my] trip to {destination} </ListenFor>
      <ListenFor RequireAppName="ExplicitlySpecified"> show [my] {builtin:AppName} trip to {destination} </ListenFor>
      <Feedback> Showing trip to {destination} </Feedback>
      <Navigate />
    </Command>

    <Command Name="whenIsTripToDestination">
      <Example> When is my trip to Las Vegas?</Example>
      <ListenFor RequireAppName="BeforeOrAfterPhrase"> when is [my] trip to {destination}</ListenFor>
      <ListenFor RequireAppName="ExplicitlySpecified"> when is [my] {builtin:AppName} trip to {destination} </ListenFor>
      <Feedback> Looking for trip to {destination}</Feedback>
      <VoiceCommandService Target="AdventureWorksVoiceCommandService"/>
    </Command>
    
    <Command Name="cancelTripToDestination">
      <Example> Cancel my trip to Las Vegas </Example>
      <ListenFor RequireAppName="BeforeOrAfterPhrase"> cancel [my] trip to {destination}</ListenFor>
      <ListenFor RequireAppName="ExplicitlySpecified"> cancel [my] {builtin:AppName} trip to {destination} </ListenFor>
      <Feedback> Cancelling trip to {destination}</Feedback>
      <VoiceCommandService Target="AdventureWorksVoiceCommandService"/>
    </Command>

    <PhraseList Label="destination">
      <Item>London</Item>
      <Item>Las Vegas</Item>
      <Item>Melbourne</Item>
      <Item>Yosemite National Park</Item>
    </PhraseList>
  </CommandSet>
```

## <span id="Install_the_VCD_commands"> </span> <span id="install_the_vcd_commands"> </span> <span id="INSTALL_THE_VCD_COMMANDS"> </span>安装 VCD 命令

你的应用必须运行一次才能安装 VCD。 

>  **注意**  
语音命令数据不会在应用安装时保留。 为了确保应用的语音命令数据保持完好，请考虑每次应用启动或激活时都初始化你的 VCD 文件，或者保留指示当前是否已安装 VCD 的设置。

在“app.xaml.cs”文件中：

1. 添加以下 using 指令：  
```csharp
using Windows.Storage;
```
2. 使用 async 修饰符标记“OnLaunched”方法。  
```csharp
protected async override void OnLaunched(LaunchActivatedEventArgs e)
```
3. 在 [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) 处理程序中调用 [**InstallCommandDefinitionsFromStorageFileAsync**](https://msdn.microsoft.com/library/windows/apps/dn708205) 以注册系统应识别的语音命令。

  在 Adventure Works 示例中，我们先定义 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 对象。 

  然后，我们将调用 [**GetFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227272) 以使用 “AdventureWorksCommands.xml”文件对其进行初始化。

  该 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 对象随后将传递给 [**InstallCommandDefinitionsFromStorageFileAsync**](https://msdn.microsoft.com/library/windows/apps/dn708205)。    
```CSharp
try
{
  // Install the main VCD. 
  StorageFile vcdStorageFile = 
    await Package.Current.InstalledLocation.GetFileAsync(
      @"AdventureWorksCommands.xml");

  await Windows.ApplicationModel.VoiceCommands.VoiceCommandDefinitionManager.
    InstallCommandDefinitionsFromStorageFileAsync(vcdStorageFile);

  // Update phrase list.
  ViewModel.ViewModelLocator locator = App.Current.Resources["ViewModelLocator"] as ViewModel.ViewModelLocator;
  if(locator != null)
  {
     await locator.TripViewModel.UpdateDestinationPhraseList();
  }
}
catch (Exception ex)
{
  System.Diagnostics.Debug.WriteLine("Installing Voice Commands Failed: " + ex.ToString());
}```

## <span id="Handle_activation_and_execute_voice_commands"></span><span id="handle_activation_and_execute_voice_commands"></span><span id="HANDLE_ACTIVATION_AND_EXECUTE_VOICE_COMMANDS"></span>Handle activation

Specify how your app responds to subsequent voice command activations (after it has been launched at least once and the voice command sets have been installed).

1.  Confirm that your app was activated by a voice command.

    Override the [**Application.OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330) event and check whether [**IActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224727).[**Kind**](https://msdn.microsoft.com/library/windows/apps/br224728) is [**VoiceCommand**](https://msdn.microsoft.com/library/windows/apps/br224693).

2.  Determine the name of the command and what was spoken.

    Get a reference to a [**VoiceCommandActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn609755) object from the [**IActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224727) and query the [**Result**](https://msdn.microsoft.com/library/windows/apps/dn609758) property for a [**SpeechRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/dn631432) object.

    To determine what the user said, check the value of [**Text**](https://msdn.microsoft.com/library/windows/apps/dn631441) or the semantic properties of the recognized phrase in the [**SpeechRecognitionSemanticInterpretation**](https://msdn.microsoft.com/library/windows/apps/dn631443) dictionary.

3.  Take the appropriate action in your app, such as navigating to the desired page.

For this example, we refer back to the VCD in Step 3: Edit the VCD file.

Once we get the speech-recognition result for the voice command, we get the command name from the first value in the [**RulePath**](https://msdn.microsoft.com/library/windows/apps/dn631438) array. As the VCD file defined more than one possible voice command, we need to compare the value against the command names in the VCD and take the appropriate action.

The most common action an application can take is to navigate to a page with content relevant to the context of the voice command. For this example, we navigate to a **TripPage** page and pass in the value of the voice command, how the command was input, and the recognized "destination" phrase (if applicable). Alternatively, the app could send a navigation parameter to the [**SpeechRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/dn631432) when navigating to the page.

You can find out whether the voice command that launched your app was actually spoken, or whether it was typed in as text, from the [**SpeechRecognitionSemanticInterpretation.Properties**](https://msdn.microsoft.com/library/windows/apps/dn631445) dictionary using the **commandMode** key. The value of that key will be either "voice" or "text". If the value of the key is "voice", consider using speech synthesis ([**Windows.Media.SpeechSynthesis**](https://msdn.microsoft.com/library/windows/apps/dn278951)) in your app to provide the user with spoken feedback.

Use the [**SpeechRecognitionSemanticInterpretation.Properties**](https://msdn.microsoft.com/library/windows/apps/dn631445) to find out the content spoken in the **PhraseList** or **PhraseTopic** constraints of a **ListenFor** element. The dictionary key is the value of the **Label** attribute of the **PhraseList** or **PhraseTopic** element. Here, we show how to access the value of **{destination}** phrase.

``` csharp
/// <summary>
/// Entry point for an application activated by some means other than normal launching. 
/// This includes voice commands, URI, share target from another app, and so on. 
/// 
/// NOTE:
/// A previous version of the VCD file might remain in place 
/// if you modify it and update the app through the store. 
/// Activations might include commands from older versions of your VCD. 
/// Try to handle these commands gracefully.
/// </summary>
/// <param name="args">Details about the activation method.</param>
protected override void OnActivated(IActivatedEventArgs args)
{
    base.OnActivated(args);

    Type navigationToPageType;
    ViewModel.TripVoiceCommand? navigationCommand = null;

    // Voice command activation.
    if (args.Kind == ActivationKind.VoiceCommand)
    {
        // Event args can represent many different activation types. 
        // Cast it so we can get the parameters we care about out.
        var commandArgs = args as VoiceCommandActivatedEventArgs;

        Windows.Media.SpeechRecognition.SpeechRecognitionResult speechRecognitionResult = commandArgs.Result;

        // Get the name of the voice command and the text spoken. 
        // See VoiceCommands.xml for supported voice commands.
        string voiceCommandName = speechRecognitionResult.RulePath[0];
        string textSpoken = speechRecognitionResult.Text;

        // commandMode indicates whether the command was entered using speech or text.
        // Apps should respect text mode by providing silent (text) feedback.
        string commandMode = this.SemanticInterpretation("commandMode", speechRecognitionResult);
        
        switch (voiceCommandName)
        {
            case "showTripToDestination":
                // Access the value of {destination} in the voice command.
                string destination = this.SemanticInterpretation("destination", speechRecognitionResult);

                // Create a navigation command object to pass to the page. 
                navigationCommand = new ViewModel.TripVoiceCommand(
                    voiceCommandName,
                    commandMode,
                    textSpoken,
                    destination);

                // Set the page to navigate to for this voice command.
                navigationToPageType = typeof(View.TripDetails);
                break;
            default:
                // If we can't determine what page to launch, go to the default entry point.
                navigationToPageType = typeof(View.TripListView);
                break;
        }
    }
    // Protocol activation occurs when a card is clicked within Cortana (using a background task).
    else if (args.Kind == ActivationKind.Protocol)
    {
        // Extract the launch context. In this case, we're just using the destination from the phrase set (passed
        // along in the background task inside Cortana), which makes no attempt to be unique. A unique id or 
        // identifier is ideal for more complex scenarios. We let the destination page check if the 
        // destination trip still exists, and navigate back to the trip list if it doesn't.
        var commandArgs = args as ProtocolActivatedEventArgs;
        Windows.Foundation.WwwFormUrlDecoder decoder = new Windows.Foundation.WwwFormUrlDecoder(commandArgs.Uri.Query);
        var destination = decoder.GetFirstValueByName("LaunchContext");

        navigationCommand = new ViewModel.TripVoiceCommand(
                                "protocolLaunch",
                                "text",
                                "destination",
                                destination);

        navigationToPageType = typeof(View.TripDetails);
    }
    else
    {
        // If we were launched via any other mechanism, fall back to the main page view.
        // Otherwise, we'll hang at a splash screen.
        navigationToPageType = typeof(View.TripListView);
    }

    // Repeat the same basic initialization as OnLaunched() above, taking into account whether
    // or not the app is already active.
    Frame rootFrame = Window.Current.Content as Frame;

    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active.
    if (rootFrame == null)
    {
        // Create a frame to act as the navigation context and navigate to the first page.
        rootFrame = new Frame();
        App.NavigationService = new NavigationService(rootFrame);

        rootFrame.NavigationFailed += OnNavigationFailed;

        // Place the frame in the current window.
        Window.Current.Content = rootFrame;
    }

    // Since we're expecting to always show a details page, navigate even if 
    // a content frame is in place (unlike OnLaunched).
    // Navigate to either the main trip list page, or if a valid voice command
    // was provided, to the details page for that trip.
    rootFrame.Navigate(navigationToPageType, navigationCommand);

    // Ensure the current window is active
    Window.Current.Activate();
}

/// <summary>
/// Returns the semantic interpretation of a speech result. 
/// Returns null if there is no interpretation for that key.
/// </summary>
/// <param name="interpretationKey">The interpretation key.</param>
/// <param name="speechRecognitionResult">The speech recognition result to get the semantic interpretation from.</param>
/// <returns></returns>
private string SemanticInterpretation(string interpretationKey, SpeechRecognitionResult speechRecognitionResult)
{
  return speechRecognitionResult.SemanticInterpretation.Properties[interpretationKey].FirstOrDefault();
}
```

## <span id="Handle_the_voice_command_in_the_app_service"> </span> <span id="handle_the_voice_command_in_the_app_service"> </span> <span id="HANDLE_THE_VOICE_COMMAND_IN_THE_APP_SERVICE"> </span>在应用服务中处理语音命令


在应用服务中处理语音命令。


1.  将以下 using 指令添加到你的语音命令服务文件，在本示例中为“AdventureWorksVoiceCommandService.cs”。
```csharp
using Windows.ApplicationModel.VoiceCommands;
using Windows.ApplicationModel.Resources.Core;
using Windows.ApplicationModel.AppService;
```

2.  采取服务延迟，以免应用服务在处理语音命令时终止。
3.  确认你的后台任务正在作为由语音命令激活的应用服务运行。

    1.  将 [**IBackgroundTaskInstance.TriggerDetails**](https://msdn.microsoft.com/library/windows/apps/br224802) 转换为 [**Windows.ApplicationModel.AppService.AppServiceTriggerDetails**](https://msdn.microsoft.com/library/windows/apps/dn921727)。
    2.  在“Package.appxmanifest”文件中检查 [**IBackgroundTaskInstance.TriggerDetails.Name**](https://msdn.microsoft.com/library/windows/apps/br224807) 是否是应用服务的名称。

4.  使用 [**IBackgroundTaskInstance.TriggerDetails**](https://msdn.microsoft.com/library/windows/apps/br224802) 创建到 **Cortana** 的 [**VoiceCommandServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn974204) 以检索语音命令。
5.  注册 [**VoiceCommandServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn974204).[**VoiceCommandCompleted**](https://msdn.microsoft.com/library/windows/apps/dn706584) 的事件处理程序，以便在应用服务由于用户取消而关闭时接收通知。
6.  注册 [**IBackgroundTaskInstance.Canceled**](https://msdn.microsoft.com/library/windows/apps/br224798) 的事件处理程序以便在应用服务由于意外失败而关闭后接收通知。
7.  确定命令名称和所说的内容。

    1.  使用 [**VoiceCommand**](https://msdn.microsoft.com/library/windows/apps/dn974162).[**CommandName**](https://msdn.microsoft.com/library/windows/apps/dn706589) 属性确定语音命令的名称。
    2.  若要确定用户说出的内容，请在 [**SpeechRecognitionSemanticInterpretation**](https://msdn.microsoft.com/library/windows/apps/dn631443) 字典中检查 [**Text**](https://msdn.microsoft.com/library/windows/apps/dn631441) 的值或已识别短语的语义式属性。

7.  在你的应用服务中采取相应的操作。
8.  通过 **Cortana** 显示并说出对语音命令的相应反馈。

    1.  确定希望 **Cortana** 为响应语音命令而向用户显示和说出的字符串，并创建 [**VoiceCommandResponse**](https://msdn.microsoft.com/library/windows/apps/dn974182) 对象。 有关如何选择 **Cortana** 显示和说出的反馈字符串的指南，请参阅 [Cortana 设计指南](https://msdn.microsoft.com/library/windows/apps/dn974233)。
    2.  使用 [**VoiceCommandServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn974204) 实例向 **Cortana** 报告进度和完成情况，方法是使用 **VoiceCommandServiceConnection** 对象调用 [**ReportProgressAsync**](https://msdn.microsoft.com/library/windows/apps/dn706579) 或 [**ReportSuccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn706580)。

    对于此示例，我们可重新参考“步骤 3：编辑 VCD 文件”中的 VCD。

```csharp
public sealed class VoiceCommandService : IBackgroundTask
    {
      private BackgroundTaskDeferral serviceDeferral;
      VoiceCommandServiceConnection voiceServiceConnection;

      public async void Run(IBackgroundTaskInstance taskInstance)
      {
      //Take a service deferral so the service isn&#39;t terminated.
        this.serviceDeferral = taskInstance.GetDeferral();

        taskInstance.Canceled += OnTaskCanceled;

        var triggerDetails = 
          taskInstance.TriggerDetails as AppServiceTriggerDetails;

        if (triggerDetails != null &amp;&amp; 
          triggerDetails.Name == "AdventureWorksVoiceServiceEndpoint")
        {
          try
          {
 voiceServiceConnection = 
   VoiceCommandServiceConnection.FromAppServiceTriggerDetails(
     triggerDetails);

 voiceServiceConnection.VoiceCommandCompleted += 
   VoiceCommandCompleted;

 VoiceCommand voiceCommand = await
 voiceServiceConnection.GetVoiceCommandAsync();

 switch (voiceCommand.CommandName)
 {
   case "whenIsTripToDestination":
   {
     var destination = 
       voiceCommand.Properties["destination"][0];
     SendCompletionMessageForDestination(destination);
     break;
   }

   // As a last resort, launch the app in the foreground.
   default:
     LaunchAppInForeground();
     break;
 }
          }
          finally
          {
 if (this.serviceDeferral != null)
 {
   // Complete the service deferral.
   this.serviceDeferral.Complete();
 }
          }
        }
      }

      private void VoiceCommandCompleted(
        VoiceCommandServiceConnection sender, 
        VoiceCommandCompletedEventArgs args)
      {
        if (this.serviceDeferral != null)
        {
          // Insert your code here.
          // Complete the service deferral.
          this.serviceDeferral.Complete();
        }
      }

      private async void SendCompletionMessageForDestination(
        string destination)
      {
        // Take action and determine when the next trip to destination
        // Insert code here.
        
        // Replace the hardcoded strings used here with strings 
        // appropriate for your application.

        // First, create the VoiceCommandUserMessage with the strings 
        // that Cortana will show and speak.
        var userMessage = new VoiceCommandUserMessage();
        userMessage.DisplayMessage = "Here’s your trip.";
        userMessage.SpokenMessage = "Your trip to Vegas is on August 3rd.";

        // Optionally, present visual information about the answer.
        // For this example, create a VoiceCommandContentTile with an 
        // icon and a string.
        var destinationsContentTiles = new List<VoiceCommandContentTile>();

        var destinationTile = new VoiceCommandContentTile();
        destinationTile.ContentTileType = 
          VoiceCommandContentTileType.TitleWith68x68IconAndText;
        // The user can tap on the visual content to launch the app. 
        // Pass in a launch argument to enable the app to deep link to a 
        // page relevant to the item displayed on the content tile.
        destinationTile.AppLaunchArgument = 
          string.Format("destination={0}”, “Las Vegas");
        destinationTile.Title = "Las Vegas";
        destinationTile.TextLine1 = "August 3rd 2015";
        destinationsContentTiles.Add(destinationTile);

        // Create the VoiceCommandResponse from the userMessage and list    
        // of content tiles.
        var response = 
          VoiceCommandResponse.CreateResponse(
 userMessage, destinationsContentTiles);

        // Cortana will present a “Go to app_name” link that the user 
        // can tap to launch the app. 
        // Pass in a launch to enable the app to deep link to a page 
        // relevant to the voice command.
        response.AppLaunchArgument = 
          string.Format("destination={0}”, “Las Vegas");
        
        // Ask Cortana to display the user message and content tile and 
        // also speak the user message.
        await voiceServiceConnection.ReportSuccessAsync(response);
      }

      private async void LaunchAppInForeground()
      {
        var userMessage = new VoiceCommandUserMessage();
        userMessage.SpokenMessage = "Launching Adventure Works";

        var response = VoiceCommandResponse.CreateResponse(userMessage);

        // When launching the app in the foreground, pass an app 
        // specific launch parameter to indicate what page to show.
        response.AppLaunchArgument = "showAllTrips=true";

        await voiceServiceConnection.RequestAppLaunchAsync(response);
      }
    }
```

一旦激活，应用服务就会有 0.5 秒的时间调用 [**ReportSuccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn706580)。 **Cortana** 使用应用提供的数据显示并说出 VCD 文件中指定的反馈。 如果应用进行该调用的时间超过 0.5 秒，**Cortana** 将插入交付屏幕，如下所示。 **Cortana** 将显示交付屏幕，直到应用程序调用 **ReportSuccessAsync** 为止，或最多显示 5 秒。 如果应用服务不调用 **ReportSuccessAsync** 或任何为 **Cortana** 提供信息的 [**VoiceCommandServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn974204) 方法，则用户将收到一条错误消息并且该应用服务将会取消。

![在后台使用 Adventure Works 应用的进度基本查询和结果屏幕](images/cortana-backgroundapp-progress-result.png)

## <span id="related_topics"> </span>相关文章


**开发人员**
* [Cortana 交互](cortana-interactions.md)
* [定义自定义识别约束](define-custom-recognition-constraints.md)
* [使用 Cortana 与后台应用交互](interact-with-a-background-app-in-cortana.md)
* [**VCD 元素和属性 v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)
* [快速入门：使用文件或图像资源](https://msdn.microsoft.com/library/windows/apps/xaml/hh965325)
* [如何使用限定符命名资源](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324)

**设计人员**
* [Cortana 设计指南](https://msdn.microsoft.com/library/windows/apps/dn974233)
* [语音设计指南](https://msdn.microsoft.com/library/windows/apps/dn596121)
* [UWP 应用的响应式设计 101](https://msdn.microsoft.com/library/windows/apps/dn958435)
* [磁贴和图标资源指南](https://msdn.microsoft.com/library/windows/apps/mt412102)

**示例**
* [Cortana 语音命令示例](http://go.microsoft.com/fwlink/p/?LinkID=619899)
 

 






<!--HONumber=Mar16_HO4-->


