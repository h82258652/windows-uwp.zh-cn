---
title: "创建 Microsoft Passport 登录应用"
description: "这是有关如何创建 Windows 10 UWP（通用 Windows 平台）应用的完整演练中的第 1 部分，将使用 Microsoft Passport 作为传统用户名和密码身份验证系统的替代项。"
ms.assetid: A9E11694-A7F5-4E27-95EC-889307E0C0EF
author: awkoren
translationtype: Human Translation
ms.sourcegitcommit: af8ae79f67d77195d5ed4801d040b2f1aafe8a97
ms.openlocfilehash: d28f0b9ea08d35a220cdb58367f03af95966e282

---

# 创建 Microsoft Passport 登录应用


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 的文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


\[有些信息与可能在商业发行之前就经过实质性修改的预发布产品相关。 Microsoft 不对此处提供的信息作任何明示或默示的担保。\]

这是有关如何创建 Windows 10 UWP（通用 Windows 平台）应用的完整演练中的第 1 部分，将使用 Microsoft Passport 作为传统用户名和密码身份验证系统的替代项。 该应用将用户名用于登录并为每个帐户创建 Passport 密钥。 这些帐户在配置 Microsoft Passport 时将受在 Windows 设置中设置的 PIN 保护。

本演练分为两个部分：生成应用和连接后端服务。 当你完成此文章时，请继续执行第 2 部分：[Microsoft Passport 登录服务](microsoft-passport-login-auth-service.md)。

在开始之前，你应阅读 [Microsoft Passport 和 Windows Hello](microsoft-passport.md) 概述，以便大致了解 Microsoft Passport 的工作原理。

## 入门


为了生成此项目，你需要具有 C# 和 XAML 方面的一些经验。 你还需要在 Windows 10 计算机上使用 Visual Studio 2015（社区版或更高版本）。

-   打开 Visual Studio 2015，然后依次选择“文件”&gt;“新建”&gt;“项目”。
-   这将打开一个“新建项目”窗口。 导航到“模板”&gt;“Visual C#”。
-   选择“空白应用（通用 Windows）”并将你的应用程序命名为“PassportLogin”。
-   生成并运行新的应用程序 (F5)，然后你应该看到屏幕上显示的空白窗口。 关闭该应用程序。

![](images/passport-login-1.png)

## 练习 1：使用 Microsoft Passport 登录


在本练习中，你将了解如何检查 Microsoft Passport 是否在计算机上设置，以及如何使用 Microsoft Passport 登录到某一帐户。

-   在新项目中，在解决方案中创建一个名为“Views”的新文件夹。 在本示例中，此文件夹包含将导航到的页面。 在解决方案资源管理器中右键单击该项目、依次选择“添加”&gt;“新文件夹”，然后将该文件夹重命名为 Views。

    ![](images/passport-login-2.png)

-   右键单击新的 Views 文件夹、依次选择“添加”&gt;“新项目”，然后选择“空白页”。 将此页面命名为“Login.xaml”。

    ![](images/passport-login-3.png)

-   若要为新的登录页定义用户界面，请添加以下 XAML。 此 XAML 定义 StackPanel 以便对齐以下子元素：

    -   将包含标题的 TextBlock。
    -   用于错误消息的 TextBlock。
    -   用于要输入的用户名的文本框。
    -   用于导航至注册页的按钮。
    -   包含 Microsoft Passport 状态的 TextBlock。
    -   用于解释登录页的 TextBlock（当没有任何后端或已配置的用户时）。

    ```xml
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
      <StackPanel Orientation="Vertical">
        <TextBlock Text="Login" FontSize="36" Margin="4" TextAlignment="Center"/>
        <TextBlock x:Name="ErrorMessage" Text="" FontSize="20" Margin="4" Foreground="Red" TextAlignment="Center"/>
        <TextBlock Text="Enter your username below" Margin="0,0,0,20"
                   TextWrapping="Wrap" Width="300"
                   TextAlignment="Center" VerticalAlignment="Center" FontSize="16"/>
        <TextBox x:Name="UsernameTextBox" Margin="4" Width="250"/>
        <Button x:Name="PassportSignInButton" Content="Login" Background="DodgerBlue" Foreground="White"
            Click="PassportSignInButton_Click" Width="80" HorizontalAlignment="Center" Margin="0,20"/>
        <TextBlock Text="Don't have an account?"
                    TextAlignment="Center" VerticalAlignment="Center" FontSize="16"/>
        <TextBlock x:Name="RegisterButtonTextBlock" Text="Register now"
                   PointerPressed="RegisterButtonTextBlock_OnPointerPressed"
                   Foreground="DodgerBlue"
                   TextAlignment="Center" VerticalAlignment="Center" FontSize="16"/>
        <Border x:Name="PassportStatus" Background="#22B14C"
                   Margin="0,20" Height="100" >
          <TextBlock x:Name="PassportStatusText" Text="Microsoft Passport is ready to use!"
                 Margin="4" TextAlignment="Center" VerticalAlignment="Center" FontSize="20"/>
        </Border>
        <TextBlock x:Name="LoginExplaination" FontSize="24" TextAlignment="Center" TextWrapping="Wrap" 
            Text="Please Note: To demonstrate a login, validation will only occur using the default username 'sampleUsername'"/>
      </StackPanel>
    </Grid>
    ```

-   需要将几个方法添加到代码隐藏，以便生成解决方案。 按 F7，或者使用“解决方案资源管理器”来访问 Login.xaml.cs。 添加以下两个事件方法，以处理登录和注册事件。 现在，这些方法将 ErrorMessage.Text 设置为空字符串。

    ```cs
    namespace PassportLogin.Views
    {
        public sealed partial class Login : Page
        {
            public Login()
            {
                this.InitializeComponent();
            }
     
            private void PassportSignInButton_Click(object sender, RoutedEventArgs e)
            {
                ErrorMessage.Text = "";
            }
            private void RegisterButtonTextBlock_OnPointerPressed(object sender, PointerRoutedEventArgs e)
            {
                ErrorMessage.Text = "";
            }
        }
    }
    ```

-   若要呈现登录页，可在加载 MainPage 时编辑 MainPage 代码以导航到登录页。 打开 MainPage.xaml.cs 文件。 在“解决方案资源管理器”中，双击 MainPage.xaml.cs。 如果你找不到此文件，请单击 MainPage.xaml 旁边的小箭头以显示代码隐藏。 创建将导航到登录页的已加载事件处理程序方法。 你将需要添加对 Views 命名空间的引用。

    ```cs
    using PassportLogin.Views;
     
    namespace PassportLogin
    {
        public sealed partial class MainPage : Page
        {
            public MainPage()
            {
                this.InitializeComponent();
                Loaded += MainPage_Loaded;
            }
     
            private void MainPage_Loaded(object sender, RoutedEventArgs e)
            {
                Frame.Navigate(typeof(Login));
            }
        }
    }
    ```

-   在登录页中，你需要处理 OnNavigatedTo 事件来验证 Microsoft Passport 在此计算机上是否可用。 在 Login.xaml.cs 中，实现以下项。 你会注意到 MicrosoftPassportHelper 对象标记一个错误。 这是因为我们尚未实现它。

    ```cs
    public sealed partial class Login : Page
    {
        public Login()
        {
            this.InitializeComponent();
        }
     
        protected override async void OnNavigatedTo(NavigationEventArgs e)
        {
            // Check Microsoft Passport is setup and available on this machine
            if (await MicrosoftPassportHelper.MicrosoftPassportAvailableCheckAsync())
            {
            }
            else
            {
                // Microsoft Passport is not setup so inform the user
                PassportStatus.Background = new SolidColorBrush(Windows.UI.Color.FromArgb(255, 50, 170, 207));
                PassportStatusText.Text = "Microsoft Passport is not setup!\n" + 
                    "Please go to Windows Settings and set up a PIN to use it.";
                PassportSignInButton.IsEnabled = false;
            }
        }
    }
    ```

-   若要创建 MicrosoftPassportHelper 类，请右键单击解决方案 PassportLogin（通用 Windows），然后依次单击“添加”&gt;“新文件夹”。 将此文件夹命名为 Utils。

    ![](images/passport-login-5.png)

-   右键单击 Utils 文件夹，然后依次单击“添加”&gt;“类”。 将此类命名为“MicrosoftPassportHelper.cs”。
-   将 MicrosoftPassportHelper 的类定义更改为公共静态，然后添加以下方法以通知用户 Microsoft Passport 是否可供使用。 你将需要添加所需的命名空间。

    ```cs
    using System;
    using System.Diagnostics;
    using System.Threading.Tasks;
    using Windows.Security.Credentials;
     
    namespace PassportLogin.Utils
    {
        public static class MicrosoftPassportHelper
        {
            /// <summary>
            /// Checks to see if Passport is ready to be used.
            /// 
            /// Passport has dependencies on:
            ///     1. Having a connected Microsoft Account
            ///     2. Having a Windows PIN set up for that _account on the local machine
            /// </summary>
            public static async Task<bool> MicrosoftPassportAvailableCheckAsync()
            {
                bool keyCredentialAvailable = await KeyCredentialManager.IsSupportedAsync();
                if (keyCredentialAvailable == false)
                {
                    // Key credential is not enabled yet as user 
                    // needs to connect to a Microsoft Account and select a PIN in the connecting flow.
                    Debug.WriteLine("Microsoft Passport is not setup!\nPlease go to Windows Settings and set up a PIN to use it.");
                    return false;
                }
     
                return true;
            }
        }
    }
    ```

-   在 Login.xaml.cs 中，添加对 Utils 命名空间的引用。 这将解决 OnNavigatedTo 方法中的错误。

    ```cs
    using PassportLogin.Utils;
    ```

-   生成并运行应用程序 (F5)。 你将导航到登录页，Microsoft Passport 横幅将指示 Passport 是否可供使用。 你应该看到绿色或蓝色横幅，这指示你的计算机上的 Microsoft Passport 状态。

    ![](images/passport-login-6.png)

    ![](images/passport-login-7.png)

-   接下来你需要执行的操作是生成用于登录的逻辑。 创建名为“Models”的新文件夹。
-   在 Models 文件夹中，创建名为“Account.cs”的新类。 此类将用作你的帐户模型。 由于这是一个示例，因此它将仅包含一个用户名。 将类定义更改为公共，并添加 Username 属性。
    
    ```cs
    namespace PassportLogin.Models
    {
        public class Account
        {
            public string Username { get; set; }
        }
    }
    ```

-   你将需要一种帐户处理方法。 对于本动手实验，由于没有服务器或数据库，因此将本地保存和加载用户列表。 右键单击 Utils 文件夹，然后添加名为“AccountHelper.cs”的新类。 将类定义更改为公共静态。 AccountHelper 为静态类，其中包含本地保存和加载帐户列表的所有必需方法。 使用 XmlSerializer 即可进行保存和加载。 你还需要记住保存的文件及其保存位置。 需要其他命名空间以供引用。
    
    ```cs
    using System.IO;
    using System.Xml.Serialization;
    using Windows.Storage;
    using PassportLogin.Models;

    namespace PassportLogin.Utils
    {
        public static class AccountHelper
        {
            // In the real world this would not be needed as there would be a server implemented that would host a user account database.
            // For this tutorial we will just be storing accounts locally.
            private const string USER_ACCOUNT_LIST_FILE_NAME = "accountlist.txt";
            private static string _accountListPath = Path.Combine(ApplicationData.Current.LocalFolder.Path, USER_ACCOUNT_LIST_FILE_NAME);
            public static List<Account> AccountList = new List<Account>();
     
            /// <summary>
            /// Create and save a useraccount list file. (Updating the old one)
            /// </summary>
            private static async void SaveAccountListAsync()
            {
                string accountsXml = SerializeAccountListToXml();
     
                if (File.Exists(_accountListPath))
                {
                    StorageFile accountsFile = await StorageFile.GetFileFromPathAsync(_accountListPath);
                    await FileIO.WriteTextAsync(accountsFile, accountsXml);
                }
                else
                {
                    StorageFile accountsFile = await ApplicationData.Current.LocalFolder.CreateFileAsync(USER_ACCOUNT_LIST_FILE_NAME);
                    await FileIO.WriteTextAsync(accountsFile, accountsXml);
                }
            }
     
            /// <summary>
            /// Gets the useraccount list file and deserializes it from XML to a list of useraccount objects.
            /// </summary>
            /// <returns>List of useraccount objects</returns>
            public static async Task<List<Account>> LoadAccountListAsync()
            {
                if (File.Exists(_accountListPath))
                {
                    StorageFile accountsFile = await StorageFile.GetFileFromPathAsync(_accountListPath);
     
                    string accountsXml = await FileIO.ReadTextAsync(accountsFile);
                    DeserializeXmlToAccountList(accountsXml);
                }
     
                return AccountList;
            }
     
            /// <summary>
            /// Uses the local list of accounts and returns an XML formatted string representing the list
            /// </summary>
            /// <returns>XML formatted list of accounts</returns>
            public static string SerializeAccountListToXml()
            {
                XmlSerializer xmlizer = new XmlSerializer(typeof(List<Account>));
                StringWriter writer = new StringWriter();
                xmlizer.Serialize(writer, AccountList);
     
                return writer.ToString();
            }
     
            /// <summary>
            /// Takes an XML formatted string representing a list of accounts and returns a list object of accounts
            /// </summary>
            /// <param name="listAsXml">XML formatted list of accounts</param>
            /// <returns>List object of accounts</returns>
            public static List<Account> DeserializeXmlToAccountList(string listAsXml)
            {
                XmlSerializer xmlizer = new XmlSerializer(typeof(List<Account>));
                TextReader textreader = new StreamReader(new MemoryStream(Encoding.UTF8.GetBytes(listAsXml)));
     
                return AccountList = (xmlizer.Deserialize(textreader)) as List<Account>;
            }
        }
    }
    ```

-   接下来，实现一种可从帐户的本地列表添加和删除帐户的方法。 每次执行这些操作都将保存该列表。 你将需要对本动手实验执行的最终方法是验证方法。 因为没有用户的身份验证服务器或数据库，因此这将对硬编码的单个用户进行验证。 这些方法应添加到 AccountHelper 类。
    
    ```cs
    public static Account AddAccount(string username)
            {
                // Create a new account with the username
                Account account = new Account() { Username = username };
                // Add it to the local list of accounts
                AccountList.Add(account);
                // SaveAccountList and return the account
                SaveAccountListAsync();
                return account;
            }
     
            public static void RemoveAccount(Account account)
            {
                // Remove the account from the accounts list
                AccountList.Remove(account);
                // Re save the updated list
                SaveAccountListAsync();
            }
     
            public static bool ValidateAccountCredentials(string username)
            {
                // In the real world, this method would call the server to authenticate that the account exists and is valid.
                // For this tutorial however we will just have a existing sample user that is just "sampleUsername"
                // If the username is null or does not match "sampleUsername" it will fail validation. In which case the user should register a new passport user
     
                if (string.IsNullOrEmpty(username))
                {
                    return false;
                }
     
                if (!string.Equals(username, "sampleUsername"))
                {
                    return false;
                }
     
                return true;
            }<
    ```

-   接下来你需要执行的操作是处理来自用户的登录请求。 在 Login.xaml.cs 中，创建一个将保存当前正在登录的帐户的新私有变量。 然后，添加一个名为 SignInPassport 的新方法。 这将使用 AccountHelper.ValidateAccountCredentials 方法验证帐户凭据。 如果输入的用户名与你在上一步中设置的硬编码字符串值相同，此方法将返回一个布尔值。 在本示例中，硬编码的值是“sampleUsername”。

    ```cs
    using PassportLogin.Models;
    using PassportLogin.Utils;
    using System.Diagnostics;
     
    namespace PassportLogin.Views
    {
        public sealed partial class Login : Page
        {
            private Account _account;
     
            public Login()
            {
                this.InitializeComponent();
            }
     
            protected override async void OnNavigatedTo(NavigationEventArgs e)
            {
                // Check Microsoft Passport is setup and available on this machine
                if (await MicrosoftPassportHelper.MicrosoftPassportAvailableCheckAsync())
                {
                }
                else
                {
                    // Microsoft Passport is not setup so inform the user
                    PassportStatus.Background = new SolidColorBrush(Windows.UI.Color.FromArgb(255, 50, 170, 207));
                    PassportStatusText.Text = "Microsoft Passport is not setup!\nPlease go to Windows Settings and set up a PIN to use it.";
                    PassportSignInButton.IsEnabled = false;
                }
            }
     
            private void PassportSignInButton_Click(object sender, RoutedEventArgs e)
            {
                ErrorMessage.Text = "";
                SignInPassport();
            }
     
            private void RegisterButtonTextBlock_OnPointerPressed(object sender, PointerRoutedEventArgs e)
            {
                ErrorMessage.Text = "";
            }
     
            private async void SignInPassport()
            {
                if (AccountHelper.ValidateAccountCredentials(UsernameTextBox.Text))
                {
                    // Create and add a new local account
                    _account = AccountHelper.AddAccount(UsernameTextBox.Text);
                    Debug.WriteLine("Successfully signed in with traditional credentials and created local account instance!");
     
                    //if (await MicrosoftPassportHelper.CreatePassportKeyAsync(UsernameTextBox.Text))
                    //{
                    //    Debug.WriteLine("Successfully signed in with Microsoft Passport!");
                    //}
                }
                else
                {
                    ErrorMessage.Text = "Invalid Credentials";
                }
            }
        }
    }
    ```

-   你可能已经注意到，注释代码引用了 MicrosoftPassportHelper 中的方法。 在 MicrosoftPassportHelper.cs 中，添加名为 CreatePassportKeyAsync 的新方法。 此方法使用 [**KeyCredentialManager**](https://msdn.microsoft.com/library/windows/apps/dn973043) 中的 Microsoft Passport API。 调用 [**RequestCreateAsync**](https://msdn.microsoft.com/library/windows/apps/dn973048) 将创建特定于 *accountId* 和本地计算机的 Passport 密钥。 如果你对在真实应用场景中进行实现感兴趣，请注意 switch 语句中的注释。

    ```cs
    /// <summary>
    /// Creates a Passport key on the machine using the _account id passed.
    /// </summary>
    /// <param name="accountId">The _account id associated with the _account that we are enrolling into Passport</param>
    /// <returns>Boolean representing if creating the Passport key succeeded</returns>
    public static async Task<bool> CreatePassportKeyAsync(string accountId)
    {
        KeyCredentialRetrievalResult keyCreationResult = await KeyCredentialManager.RequestCreateAsync(accountId, KeyCredentialCreationOption.ReplaceExisting);

        switch (keyCreationResult.Status)
        {
            case KeyCredentialStatus.Success:
                Debug.WriteLine("Successfully made key");

                // In the real world authentication would take place on a server.
                // So every time a user migrates or creates a new Microsoft Passport account Passport details should be pushed to the server.
                // The details that would be pushed to the server include:
                // The public key, keyAttesation if available, 
                // certificate chain for attestation endorsement key if available,  
                // status code of key attestation result: keyAttestationIncluded or 
                // keyAttestationCanBeRetrievedLater and keyAttestationRetryType
                // As this sample has no concept of a server it will be skipped for now
                // for information on how to do this refer to the second Passport sample

                //For this sample just return true
                return true;
            case KeyCredentialStatus.UserCanceled:
                Debug.WriteLine("User cancelled sign-in process.");
                break;
            case KeyCredentialStatus.NotFound:
                // User needs to setup Microsoft Passport
                Debug.WriteLine("Microsoft Passport is not setup!\nPlease go to Windows Settings and set up a PIN to use it.");
                break;
            default:
                break;
        }

        return false;
    }
    ```

-   现在你已创建 CreatePassportKeyAsync 方法，请返回到 Login.xaml.cs 文件，并取消对 SignInPassport 方法内代码的注释。

    ```cs
    private async void SignInPassport()
    {
        if (AccountHelper.ValidateAccountCredentials(UsernameTextBox.Text))
        {
            //Create and add a new local account
            _account = AccountHelper.AddAccount(UsernameTextBox.Text);
            Debug.WriteLine("Successfully signed in with traditional credentials and created local account instance!");

            if (await MicrosoftPassportHelper.CreatePassportKeyAsync(UsernameTextBox.Text))
            {
                Debug.WriteLine("Successfully signed in with Microsoft Passport!");
            }
        }
        else
        {
            ErrorMessage.Text = "Invalid Credentials";
        }
    }
    ```

-   生成并运行应用程序。 你将转到登录页。 键入“sampleUsername”，然后单击“登录”。 系统将通过一条 Microsoft Passport 提示信息提示你输入 PIN。 在正确输入 PIN 之后，CreatePassportKeyAsync 方法将能够创建 Passport 密钥。 监视输出窗口，以查看是否显示指示已成功的消息。

    ![](images/passport-login-8.png)

## 练习 2：欢迎页和用户选择页


在本练习中，你将从上一练习继续操作。 当用户成功登录时，他们应该进入可从中注销或删除其帐户的欢迎页。 当 Passport 为每台计算机创建密钥时，将创建一个用户选择屏幕，该屏幕上将显示已登录这台计算机的所有用户。 然后，用户可以选择这些帐户之一并直接转至欢迎屏幕，而无需重新输入密码，因为已针对这些用户进行身份验证，以便访问该计算机。

-   在 Views 文件夹中，添加名为“Welcome.xaml”的新空白页。 添加以下 XAML 来完成用户界面。 这将显示一个标题、登录的用户名以及两个按钮。 其中一个按钮将向后导航到用户列表（将在稍后创建），另一个按钮将处理忘记此用户的情况。

    ```xml
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
      <StackPanel Orientation="Vertical">
        <TextBlock x:Name="Title" Text="Welcome" FontSize="40" TextAlignment="Center"/>
        <TextBlock x:Name="UserNameText" FontSize="28" TextAlignment="Center" Foreground="Black"/>

        <Button x:Name="BackToUserListButton" Content="Back to User List" Click="Button_Restart_Click"
                HorizontalAlignment="Center" Margin="0,20" Foreground="White" Background="DodgerBlue"/>

        <Button x:Name="ForgetButton" Content="Forget Me" Click="Button_Forget_User_Click"
                Foreground="White"
                Background="Gray"
                HorizontalAlignment="Center"/>
      </StackPanel>
    </Grid>
    ```

-   在 Welcome.xaml.cs 代码隐藏文件中，添加用于保留已登录的帐户的新私有变量。 你将需要实现某种方法来替代 OnNavigateTo 事件，以便存储传递至欢迎页的帐户。 还需要针对 XAML 中定义的两个按钮实现单击事件。 你将需要一个对 Models 和 Utils 文件夹的引用。

    ```cs
    using PassportLogin.Models;
    using PassportLogin.Utils;
    using System.Diagnostics;
     
    namespace PassportLogin.Views
    {
        public sealed partial class Welcome : Page
        {
            private Account _activeAccount;
     
            public Welcome()
            {
                InitializeComponent();
            }
     
            protected override void OnNavigatedTo(NavigationEventArgs e)
            {
                _activeAccount = (Account)e.Parameter;
                if (_activeAccount != null)
                {
                    UserNameText.Text = _activeAccount.Username;
                }
            }
     
            private void Button_Restart_Click(object sender, RoutedEventArgs e)
            {
            }
     
            private void Button_Forget_User_Click(object sender, RoutedEventArgs e)
            {
                // Remove it from Microsoft Passport
                // MicrosoftPassportHelper.RemovePassportAccountAsync(_activeAccount);
     
                // Remove it from the local accounts list and resave the updated list
                AccountHelper.RemoveAccount(_activeAccount);
     
                Debug.WriteLine("User " + _activeAccount.Username + " deleted.");
            }
        }
    }
    ```

-   你可能已注意到，某行在忘记用户单击事件中已有注释。 该帐户将从你的本地列表中删除，不过当前没有从 Passport 删除的方法。 你需要在 MicrosoftPassportHelper.cs 中实现一个新方法，以便处理 Passport 用户的删除操作。 此方法将使用其他 Microsoft Passport API 来打开和删除帐户。 实际上，在删除帐户后，应通知服务器或数据库，以便用户数据库保持有效。 你将需要一个对 Models 文件夹的引用。

    ```cs
    using PassportLogin.Models;

    /// <summary>
    /// Function to be called when user requests deleting their account.
    /// Checks the KeyCredentialManager to see if there is a Passport for the current user
    /// Then deletes the local key associated with the Passport.
    /// </summary>
    public static async void RemovePassportAccountAsync(Account account)
    {
        // Open the account with Passport
        KeyCredentialRetrievalResult keyOpenResult = await KeyCredentialManager.OpenAsync(account.Username);

        if (keyOpenResult.Status == KeyCredentialStatus.Success)
        {
            // In the real world you would send key information to server to unregister
            //e.g. RemovePassportAccountOnServer(account);
        }

        // Then delete the account from the machines list of Passport Accounts
        await KeyCredentialManager.DeleteAsync(account.Username);
    }
    ```

-   返回到 Welcome.xaml.cs，并取消用于调用 RemovePassportAccountAsync 的行的注释。

    ```cs
    private void Button_Forget_User_Click(object sender, RoutedEventArgs e)
    {
        // Remove it from Microsoft Passport
        MicrosoftPassportHelper.RemovePassportAccountAsync(_activeAccount);
     
        // Remove it from the local accounts list and resave the updated list
        AccountHelper.RemoveAccount(_activeAccount);
     
        Debug.WriteLine("User " + _activeAccount.Username + " deleted.");
    }
    ```

-   在 SignInPassport 方法（属于 Login.xaml.cs）中，在CreatePassportKeyAsync 成功创建后，应导航到欢迎屏幕并传递该帐户。

    ```cs
    private async void SignInPassport()
    {
        if (AccountHelper.ValidateAccountCredentials(UsernameTextBox.Text))
        {
            // Create and add a new local account
            _account = AccountHelper.AddAccount(UsernameTextBox.Text);
            Debug.WriteLine("Successfully signed in with traditional credentials and created local account instance!");

            if (await MicrosoftPassportHelper.CreatePassportKeyAsync(UsernameTextBox.Text))
            {
                Debug.WriteLine("Successfully signed in with Microsoft Passport!");
                Frame.Navigate(typeof(Welcome), _account);
            }
        }
        else
        {
            ErrorMessage.Text = "Invalid Credentials";
        }
    }
    ```

-   生成并运行应用程序。 使用“sampleUsername”登录，然后单击“登录”。 输入你的 PIN，如果成功，你应导航到欢迎屏幕。 尝试单击“忘记用户”，并监视输出窗口以查看是否已删除该用户。 请注意，在用户删除后，你将仍位于欢迎页上。 你需要创建应用可以导航到的用户选择页。

    ![](images/passport-login-9.png)

-   在 Views 文件夹中，创建名为“UserSelection.xaml”的新空白页，然后添加以下 XAML 来定义用户界面。 此页面将包含一个用于在本地帐户列表中显示所有用户的 [**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878)，以及一个用于导航到登录页的 Button，从而让用户可以添加其他帐户。

    ```xml
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
      <StackPanel Orientation="Vertical">
        <TextBlock x:Name="Title" Text="Select a User" FontSize="36" Margin="4" TextAlignment="Center" HorizontalAlignment="Center"/>

        <ListView x:Name="UserListView" Margin="4" MaxHeight="200" MinWidth="250" Width="250" HorizontalAlignment="Center">
          <ListView.ItemTemplate>
            <DataTemplate>
              <Grid Background="DodgerBlue" Height="50" Width="250" HorizontalAlignment="Stretch" VerticalAlignment="Stretch">
                <TextBlock Text="{Binding Username}" HorizontalAlignment="Center" TextAlignment="Center" VerticalAlignment="Center" Foreground="White"/>
              </Grid>
            </DataTemplate>
          </ListView.ItemTemplate>
        </ListView>

        <Button x:Name="AddUserButton" Content="+" FontSize="36" Width="60" Click="AddUserButton_Click" HorizontalAlignment="Center"/>
      </StackPanel>
    </Grid><
    ```

-   在 UserSelection.xaml.cs 中，如果本地列表中不存在任何帐户，则实现将导航到登录页的加载方法。 此外，针对 ListView 实现 SelectionChanged 事件，针对 Button 实现单击事件。

    ```cs
    using System.Diagnostics;
    using PassportLogin.Models;
    using PassportLogin.Utils;

    namespace PassportLogin.Views
    {
        public sealed partial class UserSelection : Page
        {
            public UserSelection()
            {
                InitializeComponent();
                Loaded += UserSelection_Loaded;
            }

            private void UserSelection_Loaded(object sender, RoutedEventArgs e)
            {
                if (AccountHelper.AccountList.Count == 0)
                {
                    //If there are no accounts navigate to the LoginPage
                    Frame.Navigate(typeof(Login));
                }


                UserListView.ItemsSource = AccountHelper.AccountList;
                UserListView.SelectionChanged += UserSelectionChanged;
            }

            /// <summary>
            /// Function called when an account is selected in the list of accounts
            /// Navigates to the Login page and passes the chosen account
            /// </summary>
            private void UserSelectionChanged(object sender, RoutedEventArgs e)
            {
                if (((ListView)sender).SelectedValue != null)
                {
                    Account account = (Account)((ListView)sender).SelectedValue;
                    if (account != null)
                    {
                        Debug.WriteLine("Account " + account.Username + " selected!");
                    }
                    Frame.Navigate(typeof(Login), account);
                }
            }

            /// <summary>
            /// Function called when the "+" button is clicked to add a new user.
            /// Navigates to the Login page with nothing filled out
            /// </summary>
            private void AddUserButton_Click(object sender, RoutedEventArgs e)
            {
                Frame.Navigate(typeof(Login));
            }
        }
    }
    ```

<!-- -->

-   存在你想要在其中导航到 UserSelection 页的应用中的多个位置。 在 MainPage.xaml.cs 中，你应导航到 UserSelection 页而不是登录页。 当你在 MainPage 中执行加载事件时，你需要加载帐户列表，以便 UserSelection 页可以检查是否存在任何帐户。 此操作不仅需要更改要异步执行的加载方法，还需要添加对 Utils 文件夹的引用。

    ```cs
    using PassportLogin.Utils;

    private async void MainPage_Loaded(object sender, RoutedEventArgs e)
    {
        // Load the local Accounts List before navigating to the UserSelection page
        await AccountHelper.LoadAccountListAsync();
        Frame.Navigate(typeof(UserSelection));
    }
    ```

-   接下来，需要从欢迎页导航到 UserSelection 页。 在这两个单击事件中，你应向后导航到 UserSelection 页。

    ```cs
    private void Button_Restart_Click(object sender, RoutedEventArgs e)
    {
        Frame.Navigate(typeof(UserSelection));
    }

    private void Button_Forget_User_Click(object sender, RoutedEventArgs e)
    {
        // Remove it from Microsoft Passport
        MicrosoftPassportHelper.RemovePassportAccountAsync(_activeAccount);

        // Remove it from the local accounts list and resave the updated list
        AccountHelper.RemoveAccount(_activeAccount);

        Debug.WriteLine("User " + _activeAccount.Username + " deleted.");

        // Navigate back to UserSelection page.
        Frame.Navigate(typeof(UserSelection));
    }
    ```

-   在登录页中，你需要代码来登录到从 UserSelection 页的列表中选择的帐户。 在 OnNavigatedTo 事件中，存储已传递给导航的帐户。 首先，添加新的私有变量，用于标识帐户是否为现有帐户。 然后，处理 OnNavigatedTo 事件。

    ```cs
    namespace PassportLogin.Views
    {
        public sealed partial class Login : Page
        {
            private Account _account;
            private bool _isExistingAccount;

            public Login()
            {
                InitializeComponent();
            }

            /// <summary>
            /// Function called when this frame is navigated to.
            /// Checks to see if Microsoft Passport is available and if an account was passed in.
            /// If an account was passed in set the "_isExistingAccount" flag to true and set the _account
            /// </summary>
            protected override async void OnNavigatedTo(NavigationEventArgs e)
            {
                // Check Microsoft Passport is setup and available on this machine
                if (await MicrosoftPassportHelper.MicrosoftPassportAvailableCheckAsync())
                {
                    if (e.Parameter != null)
                    {
                        _isExistingAccount = true;
                        // Set the account to the existing account being passed in
                        _account = (Account)e.Parameter;
                        UsernameTextBox.Text = _account.Username;
                        SignInPassport();
                    }
                }
                else
                {
                    // Microsoft Passport is not setup so inform the user
                    PassportStatus.Background = new SolidColorBrush(Windows.UI.Color.FromArgb(255, 50, 170, 207));
                    PassportStatusText.Text = "Microsoft Passport is not setup!\n" + 
                        "Please go to Windows Settings and set up a PIN to use it.";
                    PassportSignInButton.IsEnabled = false;
                }
            }
        }
    }
    ```

-   SignInPassport 方法将需要进行更新，以便登录到所选的帐户。 MicrosoftPassportHelper 将需要另一种方法来使用 Passport 打开帐户，因为帐户已具有一个为其创建的 Passport 密钥。 在 MicrosoftPassportHelper.cs 中实现新方法，以便现有用户可以使用 Passport 登录。 有关代码的每个部分的信息，请阅读整个代码注释。

    ```cs
    /// <summary>
    /// Attempts to sign a message using the Passport key on the system for the accountId passed.
    /// </summary>
    /// <returns>Boolean representing if creating the Passport authentication message succeeded</returns>
    public static async Task<bool> GetPassportAuthenticationMessageAsync(Account account)
    {
        KeyCredentialRetrievalResult openKeyResult = await KeyCredentialManager.OpenAsync(account.Username);
        // Calling OpenAsync will allow the user access to what is available in the app and will not require user credentials again.
        // If you wanted to force the user to sign in again you can use the following:
        // var consentResult = await Windows.Security.Credentials.UI.UserConsentVerifier.RequestVerificationAsync(account.Username);
        // This will ask for the either the password of the currently signed in Microsoft Account or the PIN used for Microsoft Passport.

        if (openKeyResult.Status == KeyCredentialStatus.Success)
        {
            // If OpenAsync has succeeded, the next thing to think about is whether the client application requires access to backend services.
            // If it does here you would Request a challenge from the Server. The client would sign this challenge and the server
            // would check the signed challenge. If it is correct it would allow the user access to the backend.
            // You would likely make a new method called RequestSignAsync to handle all this
            // e.g. RequestSignAsync(openKeyResult);
            // Refer to the second Microsoft Passport sample for information on how to do this.

            // For this sample there is not concept of a server implemented so just return true.
            return true;
        }
        else if (openKeyResult.Status == KeyCredentialStatus.NotFound)
        {
            // If the _account is not found at this stage. It could be one of two errors. 
            // 1. Microsoft Passport has been disabled
            // 2. Microsoft Passport has been disabled and re-enabled cause the Microsoft Passport Key to change.
            // Calling CreatePassportKey and passing through the account will attempt to replace the existing Microsoft Passport Key for that account.
            // If the error really is that Microsoft Passport is disabled then the CreatePassportKey method will output that error.
            if (await CreatePassportKeyAsync(account.Username))
            {
                // If the Passport Key was again successfully created, Microsoft Passport has just been reset.
                // Now that the Passport Key has been reset for the _account retry sign in.
                return await GetPassportAuthenticationMessageAsync(account);
            }
        }

        // Can't use Passport right now, try again later
        return false;
    }
    ```

-   在 Login.xaml.cs 中更新 SignInPassport 方法以处理现有帐户。 这将在 MicrosoftPassportHelper.cs 中使用新方法。 如果成功，将登录该帐户，并且用户将导航到欢迎屏幕。

    ```cs
    private async void SignInPassport()
    {
        if (_isExistingAccount)
        {
            if (await MicrosoftPassportHelper.GetPassportAuthenticationMessageAsync(_account))
            {
                Frame.Navigate(typeof(Welcome), _account);
            }
        }
        else if (AccountHelper.ValidateAccountCredentials(UsernameTextBox.Text))
        {
            //Create and add a new local account
            _account = AccountHelper.AddAccount(UsernameTextBox.Text);
            Debug.WriteLine("Successfully signed in with traditional credentials and created local account instance!");

            if (await MicrosoftPassportHelper.CreatePassportKeyAsync(UsernameTextBox.Text))
            {
                Debug.WriteLine("Successfully signed in with Microsoft Passport!");
                Frame.Navigate(typeof(Welcome), _account);
            }
        }
        else
        {
            ErrorMessage.Text = "Invalid Credentials";
        }
    }
    ```

-   生成并运行应用程序。 使用“sampleUsername”登录。 键入你的 PIN，如果成功，你将导航到欢迎屏幕。 通过单击返回到用户列表。 你现在应该可以在列表中看到用户。 通过单击此 Passport，你可以重新登录，而无需重新输入任何密码等。

    ![](images/passport-login-10.png)

## 练习 3：注册新的 Passport 用户


在本练习中，将为你创建一个新页面，将在该页面中使用 Passport 创建一个新帐户。 这将与登录页的工作原理类似。 登录页将针对迁移的现有用户进行实现，以便使用 Passport。 PassportRegister 页将为新用户创建 Passport 注册。

-   在 Views 文件夹中，创建名为“PassportRegister.xaml”的新空白页。 在 XAML 中，添加以下内容来设置用户界面。 此处的界面类似于登录页。

    ```xml
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
      <StackPanel Orientation="Vertical">
        <TextBlock x:Name="Title" Text="Register New Passport User" FontSize="24" Margin="4" TextAlignment="Center"/>

        <TextBlock x:Name="ErrorMessage" Text="" FontSize="20" Margin="4" Foreground="Red" TextAlignment="Center"/>

        <TextBlock Text="Enter your new username below" Margin="0,0,0,20"
                   TextWrapping="Wrap" Width="300"
                   TextAlignment="Center" VerticalAlignment="Center" FontSize="16"/>

        <TextBox x:Name="UsernameTextBox" Margin="4" Width="250"/>

        <Button x:Name="PassportRegisterButton" Content="Register" Background="DodgerBlue" Foreground="White"
            Click="RegisterButton_Click_Async" Width="80" HorizontalAlignment="Center" Margin="0,20"/>

        <Border x:Name="PassportStatus" Background="#22B14C"
                   Margin="4" Height="100">
          <TextBlock x:Name="PassportStatusText" Text="Microsoft Passport is ready to use!" FontSize="20"
                 Margin="4" TextAlignment="Center" VerticalAlignment="Center"/>
        </Border>
      </StackPanel>
    </Grid>
    ```

-   在 PassportRegister.xaml.cs 代码隐藏文件中，实现私有 Account 变量，并针对注册 Button 实现单击事件。 这将添加一个新的本地帐户，并创建 Passport 密钥。

    ```cs
    using PassportLogin.Models;
    using PassportLogin.Utils;

    namespace PassportLogin.Views
    {
        public sealed partial class PassportRegister : Page
        {
            private Account _account;

            public PassportRegister()
            {
                InitializeComponent();
            }

            private async void RegisterButton_Click_Async(object sender, RoutedEventArgs e)
            {
                ErrorMessage.Text = "";

                //In the real world you would normally validate the entered credentials and information before 
                //allowing a user to register a new account. 
                //For this sample though we will skip that step and just register an account if username is not null.

                if (!string.IsNullOrEmpty(UsernameTextBox.Text))
                {
                    //Register a new account
                    _account = AccountHelper.AddAccount(UsernameTextBox.Text);
                    //Register new account with Microsoft Passport
                    await MicrosoftPassportHelper.CreatePassportKeyAsync(_account.Username);
                    //Navigate to the Welcome Screen. 
                    Frame.Navigate(typeof(Welcome), _account);
                }
                else
                {
                    ErrorMessage.Text = "Please enter a username";
                }
            }
        }
    }
    ```

-   当单击注册时，需要从登录页导航至该页。

    ```cs
    private void RegisterButtonTextBlock_OnPointerPressed(object sender, PointerRoutedEventArgs e)
    {
        ErrorMessage.Text = "";
        Frame.Navigate(typeof(PassportRegister));
    }
    ```

-   生成并运行应用程序。 尝试注册新用户。 然后返回到用户列表，并验证你是否可以选择该用户并登录。

    ![](images/passport-login-11.png)

在本实验中，你已经掌握了使用新 Microsoft Passport API 对现有用户进行身份验证和为新用户创建帐户时所需的基本技能。 借助此新知识，你的用户现在不再需要记住你的应用程序密码，不过请放心，你的应用程序仍然受用户身份验证保护。 Windows 10 使用 Passport 技术来支持 Windows Hello 的生物识别登录。 如果你使用的计算机支持 Windows Hello，你将看到这组练习已支持 Windows Hello。

作为一名开发人员，你无需执行任何额外的工作，即可在实现对 Microsoft Passport 的支持之后支持 Windows Hello。

## 相关主题

* [Microsoft Passport 和 Windows Hello](microsoft-passport.md)
* [Microsoft Passport 登录服务](microsoft-passport-login-auth-service.md)


<!--HONumber=Jun16_HO4-->


