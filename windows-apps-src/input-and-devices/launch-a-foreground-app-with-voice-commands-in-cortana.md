---
author: Karl-Bridge-Microsoft
Description: 除了在 Cortana 内使用语音命令访问系统功能外，你还可以通过 Cortana 使用语音命令启动前台应用并在应用内指定某个要执行的操作或命令。
title: 在 Cortana 中使用语音命令启动前台应用
ms.assetid: 8D3D1F66-7D17-4DD1-B426-DCCBD534EF00
label: Cortana-Launch a foreground app
template: detail.hbs
---

# 通过 Cortana 使用语音命令激活前台应用

除了在 **Cortana** 内使用语音命令访问系统功能外，你还可以通过应用中的特性和功能扩展 **Cortana**。 使用语音命令，你的应用可以激活到前台以及在应用内执行的某个操作或命令。 

**重要的 API**

-   [**Windows.ApplicationModel.VoiceCommands**](https://msdn.microsoft.com/library/windows/apps/dn706594)
-   [**VCD 元素和属性 v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)


当应用在前台处理语音命令时，它将获得焦点，并且将取消 Cortana。 如果你愿意，你可以激活应用并作为后台任务执行命令。 在此情况下，Cortana 会保留焦点，并且你的应用通过 **Cortana** 画布和 **Cortana** 语音返回所有反馈和结果。

需要其他上下文或用户输入（例如将消息发送给特定联系人）的语音命令最好通过前台应用处理，而基本命令（如列出即将到来的旅行）可以通过后台应用使用 **Cortana** 处理。

如果你希望使用语音命令在后台激活应用，请参阅[通过 Cortana 使用语音命令激活后台应用](launch-a-background-app-with-voice-commands-in-cortana.md)

> **注意**  
> 语音命令是在语音命令定义 (VCD) 文件中定义的具有特定意图的单个发音，通过 **Cortana** 指向一个已安装的应用

> VCD 文件定义一个或多个语音命令，每个语音命令都具有一种特殊意图。

> 语音命令定义的复杂程度可能有所不同。 它可以支持任何语音命令，从单个受限发音到一组更灵活的自然语言发音，所有发音都表示相同的意图。

为了演示前台应用功能，我们将使用 [Cortana 语音命令示例](http://go.microsoft.com/fwlink/p/?LinkID=619899)中的名为 **Adventure Works** 的旅行规划和管理应用

若要在没有 **Cortana** 的情况下创建一个新的 **Adventure Works** 旅行，用户将启动该应用并导航到“新旅行”****页面。 若要查看现有旅行，用户将启动该应用、导航到**“新旅行”**页面，然后选择该旅行。

通过 **Cortana** 使用语音命令，用户只需改说“Adventure Works 添加旅行”或“在 Adventure Works 上添加旅行”，即可启动该应用并导航到**“新旅行”**页面。 反过来，说“Adventure Works，显示我的伦敦之旅”将启动该应用并导航到**“旅行”**详细信息页，如此处所示。

![Cortana 启动前台应用](images/cortana-foreground-with-adventureworks.png)

以下是使用语音或键盘输入添加语音命令功能并将 Cortana 与你的应用集成的基本步骤：

1.  创建 VCD 文件。 这是一个 XML 文档，可以定义在激活应用时用户可说出以启动操作或调用命令的所有语音命令。 请参阅 [**VCD elements and attributes v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)（VCD 元素和属性 v1.2）
2.  当启动应用时，在 VCD 文件中注册命令集。
3.  处理通过语音命令激活、应用内导航和命令执行。

**先决条件：  **

如果你还不熟悉通用 Windows 平台 (UWP) 应用开发，请仔细阅读这些主题来熟悉此处讨论的技术。

-   [创建你的第一个应用](https://msdn.microsoft.com/library/windows/apps/bg124288)
-   借助[事件和路由事件概述](https://msdn.microsoft.com/library/windows/apps/mt185584)了解事件

**用户体验指南：  **

有关如何将你的应用与 **Cortana** 集成的信息，请参阅 [Cortana 设计指南](https://msdn.microsoft.com/library/windows/apps/dn974233)；有关设计出既实用又有吸引力且支持语音的应用的有用提示，请参阅[语音设计指南](https://msdn.microsoft.com/library/windows/apps/dn596121)。

## <span id="Create_a_new_solution_with_project_in_Visual_Studio"></span><span id="create_a_new_solution_with__project_in_visual_studio"></span><span id="CREATE_A_NEW_SOLUTION_WITH__PROJECT_IN_VISUAL_STUDIO"></span>使用 Visual Studio 中的项目创建一个新解决方案


1.  启动 Microsoft Visual Studio 2015。

    将出现 Visual Studio 2015 起始页。

2.  在“文件”****菜单上，依次选择“新建”**** > “项目”****

    会出现“新建项目”****对话框。 可以在对话框的左侧窗格中选择要显示模板的类型。

3.  在左侧窗格中，依次展开**“已安装”>“模板”>“Visual C\#”>“Windows”**，然后选取**“通用”**模板组。 对话框的中心窗格会显示适用于通用 Windows 平台 (UWP) 应用的项目模板的列表。
4.  在中心窗格中，选择“空白应用(通用 Windows)”****模板。

    “空白应用”****模板会创建一个最基本的 UWP 应用，该应用可以编译和运行，但不包含任何用户界面控件或数据。 本教程将指导你向该应用添加控件。

5.  在**“名称”**文本框中，键入项目名称。 在此示例中，我们使用“AdventureWorks”。
6.  单击“确定”****以创建项目。

    Microsoft Visual Studio 将创建项目并在“解决方案资源管理器”****中显示该项目

## <span id="Add_image_assets_to_project_and_specify_them_in_the_app_manifest"></span><span id="add_image_assets_to_project_and_specify_them_in_the_app_manifest"></span><span id="ADD_IMAGE_ASSETS_TO_PROJECT_AND_SPECIFY_THEM_IN_THE_APP_MANIFEST"></span>将图像资源添加到项目，并在应用清单中指定它们
      
UWP 应用可以基于特定设置和设备功能（高对比度、有效像素、区域设置等）自动选择最合适的图像。 你只需提供图像，并确保在不同资源版本的应用项目中使用相应的命名约定和文件夹组织。 如果未能提供推荐的资源版本，辅助功能、本地化和图像质量将受到影响，具体取决于用户首选项、功能、设备类型和位置。

有关高对比度和比例系数的图像资源的更多详细信息，请参阅[磁贴和图标资源指南](https://msdn.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-app-assets)

使用限定符命名资源。 资源限定符是一种文件夹和文件名修饰符，可标识应使用一种特定资源版本的上下文。

标准命名约定是 `foldername/qualifiername-value[_qualifiername-value]/filename.qualifiername-value[_qualifiername-value].ext`。 例如，`images/en-US/logo.scale-100_contrast-white.png`，可以在代码中仅使用根文件夹和文件名指代：`images/logo.png`。 请参阅[如何使用限定符命名资源](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324.aspx)

我们建议在字符串资源文件（如 `en-US\resources.resw`）上标记默认语言，在图像（如 `logo.scale-100.png`）上标记默认比例系数，即使你当前不计划提供本地化或多种分辨率的资源也是如此。 但是，我们建议你至少为 100、200 和 400 比例系数提供资源。

> *重要提示

> 在 **Cortana** 画布的标题区域使用的应用图标是在“Package.appxmanifest”文件中指定的 44x44 方形徽标图标。 
    
## <span id="Create_a_VCD_file"></span><span id="create_a_vcd_file"></span><span id="CREATE_A_VCD_FILE"></span>创建 VCD 文件

1. 在 Visual Studio 中，右键单击你的主项目名称，然后依次选择“添加”>“新建项目”****。 添加 **XML 文件**
2. 为 [**VCD**](https://msdn.microsoft.com/library/windows/apps/dn706593) 文件键入名称（在本示例中为“AdventureWorksCommands.xml”），然后单击“添加”。 
3. 在“解决方案资源管理器”****中，选择 [**VCD**](https://msdn.microsoft.com/library/windows/apps/dn706593) 文件。
4.  在“属性”****窗口中，将“生成操作”****设置为“内容”****，然后将“复制到输出目录”****设置为“如果较新则复制”****

## <span id="Edit_the_VCD_file"></span><span id="edit_the_vcd_file"></span><span id="EDIT_THE_VCD_FILE"></span>编辑 VCD 文件


添加带有 **xmlns** 属性的 **VoiceCommands** 元素，该属性指向

2. 针对你的应用支持的每种语言，创建一个包含你的应用支持的语音命令的 [**CommandSet**](https://msdn.microsoft.com/library/windows/apps/dn722331) 元素。

  你可以声明多个 [**CommandSet**](https://msdn.microsoft.com/library/windows/apps/dn722331) 元素，每个都带有不同的 [**xml:lang**](https://msdn.microsoft.com/library/windows/apps/dn722331) 属性以使你的应用可用于不同的市场。 例如，用于美国的应用可能有一个英语版本的 [**CommandSet**](https://msdn.microsoft.com/library/windows/apps/dn722331) 和一个西班牙语版本的 [**CommandSet**](https://msdn.microsoft.com/library/windows/apps/dn722331)。

  >  **注意**  
  为了激活应用并使用语音命令启动操作，该应用必须注册一个 VCD 文件，此文件包含带有与用户为其设备选择的语音语言相匹配的语言的 [**CommandSet**](https://msdn.microsoft.com/library/windows/apps/dn722331)。 语音语言位于“设置”>“系统”>“语音”>“语音语言”****中

3. 为要支持的每个命令添加 **Command** 元素。

  [
            **VCD**](https://msdn.microsoft.com/library/windows/apps/dn706593) 文件中声明的每个 **Command** 都必须包含以下信息：

  - 你的应用程序用于在运行时标识语音命令的 **Name** 属性。 
  - **Example** 元素，其中包含一个描述用户可以如何调用命令的短语。 当用户说“我能说什么？”、“帮助”或当他们点击“查看详细信息”****时，**Cortana** 会显示此示例    
  -   **ListenFor** 元素，其中包含你的应用识别为命令的字词或短语。 每个 **ListenFor** 元素都可以包含对一个或多个包含与该命令相关的特定字词的 **PhraseList** 元素的引用。
  > **注意**  
  **ListenFor** 元素无法以编程方式修改。 但是，与 **ListenFor** 元素相关联的 **PhraseList** 元素可以以编程方式修改。 应用程序应该基于用户在使用应用时所生成的数据集在运行时修改 **PhraseList** 的内容。 请参阅[动态修改语音命令定义 (VCD) 短语列表](dynamically-modify-voice-command-definition--vcd--phrase-lists.md)

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

## <span id="Install_the_VCD_commands"></span><span id="install_the_vcd_commands"></span><span id="INSTALL_THE_VCD_COMMANDS"></span>安装 VCD 命令


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

  该 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 对象随后将传递给 [**InstallCommandDefinitionsFromStorageFileAsync**](https://msdn.microsoft.com/library/windows/apps/dn708205)    
```csharp
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
}
```

## <span id="Handle_activation_and_execute_voice_commands"></span><span id="handle_activation_and_execute_voice_commands"></span><span id="HANDLE_ACTIVATION_AND_EXECUTE_VOICE_COMMANDS"></span>处理激活并执行语音命令

指定你的应用如何响应后续语音命令激活（至少启动一次应用并安装语音命令集之后）。

1.  确认你的应用已通过语音命令激活。

    替代 [**Application.OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330) 事件并检查 [**IActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224727).[**Kind**](https://msdn.microsoft.com/library/windows/apps/br224728) 是否是 [**VoiceCommand**](https://msdn.microsoft.com/library/windows/apps/br224693)

2.  确定命令名称和所说的内容。

    从 [**IActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224727) 中获取对 [**VoiceCommandActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn609755) 对象的引用并查询 [**SpeechRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/dn631432) 对象的 [**Result**](https://msdn.microsoft.com/library/windows/apps/dn609758) 属性。

    若要确定用户说出的内容，请在 [**SpeechRecognitionSemanticInterpretation**](https://msdn.microsoft.com/library/windows/apps/dn631443) 字典中检查 [**Text**](https://msdn.microsoft.com/library/windows/apps/dn631441) 的值或已识别短语的语义式属性。

3.  在应用中执行相应的操作，如导航到所需页面。

对于此示例，我们可重新参考“步骤 3：编辑 VCD 文件”中的 VCD。

一旦我们得到语音命令对应的语音识别结果，我们会从 [**RulePath**](https://msdn.microsoft.com/library/windows/apps/dn631438) 数组中的第一个值获取命令名称。 由于 VCD 文件定义了多个可能的语音命令，因此我们需要将该值与 VCD 中的命令名称进行比较，并执行相应的操作。

应用程序可执行的最常见操作是导航到带有与该语音命令的上下文相关的内容的页面。 对于此示例，我们导航到“TripPage”****页面并提交语音命令的值、输入命令的方式以及已识别的“目标”短语（如果适用）。 此外，应用可在导航到该页面时将导航参数发送到 [**SpeechRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/dn631432)。

你可以查明实际是否已说出已启动应用的语音命令，或者是否已使用 **commandMode** 注册表项从 [**SpeechRecognitionSemanticInterpretation.Properties**](https://msdn.microsoft.com/library/windows/apps/dn631445) 字典中以文本形式将其键入。 该注册表项的值将为“voice”或“text”。 如果该注册表项的值为“voice”，请考虑在应用中使用语音合成 ([**Windows.Media.SpeechSynthesis**](https://msdn.microsoft.com/library/windows/apps/dn278951)) 向用户提供语音反馈。

使用 [**SpeechRecognitionSemanticInterpretation.Properties**](https://msdn.microsoft.com/library/windows/apps/dn631445) 找出在 **ListenFor** 元素的 **PhraseList** 或 **PhraseTopic** 约束中说出的内容。 该字典密钥是 **PhraseList** 或 **PhraseTopic** 元素的 **Label** 属性的值。 下面我们介绍如何访问 **{destination}** 短语的值。

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


