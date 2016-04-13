---
title: 在应用之间共享证书
description: 要求用户名和密码组合以上的安全身份认证的通用 Windows 平台 (UWP) 应用可以使用证书进行身份验证。
ms.assetid: 159BA284-9FD4-441A-BB45-A00E36A386F9
author: awkoren
---

# 在应用之间共享证书


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


要求用户名和密码组合以上的安全身份认证的通用 Windows 平台 (UWP) 应用可以使用证书进行身份验证。 对用户进行身份验证时，证书身份验证将提供高级别的信任。 在某些情况下，一组服务将要针对多个应用对用户进行身份验证。 本文介绍了如何使用同一个证书对多个应用进行身份验证，以及如何提供方便代码，用户可使用此代码导入提供的证书以访问安全的 Web 服务。

应用可使用证书对 Web 服务进行身份验证，并且多个应用可使用来自证书存储的单个证书对相同的用户进行身份验证。 如果存储中不存在证书，可将代码添加到应用以从 PFX 文件导入证书。

## 启用 Microsoft Internet 信息服务 (IIS) 和客户端证书映射


本文以 Microsoft Internet 信息服务 (IIS) 为例。 默认情况下不启用 IIS。 可通过使用控制面板来启用 IIS。

1.  打开“控制面板”，选择“程序”****。
2.  选择“打开或关闭 Windows 功能”****。
3.  展开**“Internet 信息服务”**，然后展开**“万维网服务”**。 展开**“应用开发功能”**并选择**“ASP.NET 3.5”**和**“ASP.NET 4.5”**。 做出这些选择将自动启用 **Internet 信息服务**。
4.  单击**“确定”**以应用更改。

## 创建并发布安全的 Web 服务


1.  作为管理员运行 Microsoft Visual Studio 并从起始页选择“新建项目”****。 将 Web 服务发布到 IIS 服务器需要管理员访问权限。 在“新建项目”对话框中，将框架更改为**“.NET Framework 3.5”**。 选择 **Visual C#** -> **Web** -> **Visual Studio** - > **ASP.NET Web 服务应用程序**。 将应用程序命名为“FirstContosoBank”。 单击**“确定”**以创建项目。
2.  在 **Service1.asmx.cs** 文件中，用以下“登录”方法替换 **HelloWorld** Web 方法。
    ```cs
            [WebMethod]
            public string Login()
            {
                // Verify certificate with CA
                var cert = new System.Security.Cryptography.X509Certificates.X509Certificate2(
                    this.Context.Request.ClientCertificate.Certificate);
                bool test = cert.Verify();
                return test.ToString();
            }
    ```

3.  保存 **Service1.asmx.cs** 文件。
4.  在**解决方案资源管理器**中，右键单击“FirstContosoBank”应用并选择**“发布”**。
5.  在**“发布 Web”**对话框中，创建新的配置文件并将其命名为“ContosoProfile”。 单击**“下一步”**。
6.  在下一个页面上，为你的 IIS 服务器输入服务器名并指定“默认 Web 站点/FirstContosoBank”的站点名。 单击**“发布”**以发布你的 Web 服务。

## 配置你的 Web 服务以使用客户端认证身份验证。


1.  运行“Internet 信息服务 (IIS) 管理器”****。
2.  展开你的 IIS 服务器的站点。 在**“默认 Web 站点”**下，选择新的“FirstContosoBank”Web 服务。 在**“操作”**部分中，选择**“高级设置...”**。
3.  将**“应用程序池”**设置为**“.NET v2.0”**并单击**“确定”**。
4.  在**“Internet 信息服务 (IIS) 管理器”**中，选择你的 IIS 服务器，然后双击**“服务器证书”**。 在**“操作”**部分中，选择**“创建自签名证书...”**。 输入“ContosoBank”作为证书的昵称并单击**“确定”**。 这将创建一个新的证书供 IIS 服务器以“<server-name>.<domain-name>”格式使用。
5.  在**“Internet 信息服务 (IIS) 管理器”**中，选择默认网站。 在**“操作”**部分中，选择**“绑定”**，然后单击**“添加...”**。 选择“https”作为类型，将端口设置为“443”，然后输入 IIS 服务器的完整主机名（“<server-name>.<domain-name>”）。 将 SSL 证书设置为“ContosoBank”。 单击**“确定”**。 单击**“站点绑定”**窗口中的**“关闭”**。
6.  在**“Internet 信息服务 (IIS) 管理器”**中，选择“FirstContosoBank”Web 服务。 双击**“SSL 设置”**。 选中**“要求 SSL”**。 在**“客户端证书”**下，选择**“要求”**。 在**“操作”**部分中，单击**“应用”**。
7.  你可以通过打开 Web 浏览器并输入以下 Web 地址来验证 Web 服务是否正确配置：“https://<server-name>.<domain-name>/FirstContosoBank/Service1.asmx”。 例如，“https://myserver.example.com/FirstContosoBank/Service1.asmx”。 如果你的 Web 服务已正确配置，将提示你选择一个客户端证书以访问该 Web 服务。

你可以重复之前的步骤以创建多个可使用相同客户端证书访问的 Web 服务。

## 创建使用证书身份验证的 Windows 应用商店应用


现在你已拥有一个或多个安全 Web 服务，应用可使用证书来验证这些 Web 服务。 在使用 [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) 对象发出对经过身份验证的 Web 服务的请求时，初始请求将不包含客户端证书。 经过身份验证的 Web 服务将以对客户端身份验证的请求进行响应。 当此情况发生时，Windows 客户端将自动查询证书存储以获取可用的客户端证书。 用户可以从这些证书中选择以对 Web 服务进行身份验证。 一些证书受密码保护，所以你将需要向用户提供输入证书密码的方法。

如果没有可用的客户端证书，则用户将需要将证书添加到证书存储。 可将代码包括在 Windows 应用商店应用中，此应用使用户能够选择包含客户端证书的 PFX 文件，然后将该证书导入到客户端证书存储中。

**提示** 你可以使用 makecert.exe 创建 PFX 文件以用于此快速入门。 有关使用 makecert.exe 的信息，请参阅 [MakeCert](https://msdn.microsoft.com/library/windows/desktop/aa386968)。

 

1.  打开 Visual Studio，然后在起始页创建新的项目。 将此新项目命名为“FirstContosoBankApp”。 单击**“确定”**创建新项目。
2.  在 MainPage.xaml 文件中，将以下 XAML 添加到默认 **Grid** 元素中。 此 XAML 包括一个用于浏览要导入的 PFX 文件的按钮、一个用于输入受密码保护的 PFX 文件的密码的文本框、一个用于导入选中的 PFX 文件的按钮、一个用于登录安全 Web 服务的按钮以及一个用于显示当前操作状况的文本块。
    ```xaml
    <Button x:Name="Import" Content="Import Certificate (PFX file)" HorizontalAlignment="Left" Margin="352,305,0,0" VerticalAlignment="Top" Height="77" Width="260" Click="Import_Click" FontSize="16"/>
    <Button x:Name="Login" Content="Login" HorizontalAlignment="Left" Margin="611,305,0,0" VerticalAlignment="Top" Height="75" Width="240" Click="Login_Click" FontSize="16"/>
    <TextBlock x:Name="Result" HorizontalAlignment="Left" Margin="355,398,0,0" TextWrapping="Wrap" VerticalAlignment="Top" Height="153" Width="560"/>
    <PasswordBox x:Name="PfxPassword" HorizontalAlignment="Left" Margin="483,271,0,0" VerticalAlignment="Top" Width="229"/>
    <TextBlock HorizontalAlignment="Left" Margin="355,271,0,0" TextWrapping="Wrap" Text="PFX password" VerticalAlignment="Top" FontSize="18" Height="32" Width="123"/>
    <Button x:Name="Browse" Content="Browse for PFX file" HorizontalAlignment="Left" Margin="352,189,0,0" VerticalAlignment="Top" Click="Browse_Click" Width="499" Height="68" FontSize="16"/>
    <TextBlock HorizontalAlignment="Left" Margin="717,271,0,0" TextWrapping="Wrap" Text="(Optional)" VerticalAlignment="Top" Height="32" Width="83" FontSize="16"/>
    ```
    
3.  保存 MainPage.xaml 文件。
4.  在 MainPage.xaml.cs 文件中，添加以下 using 语句。
    ```cs
    using Windows.Web.Http;
    using System.Text;
    using Windows.Security.Cryptography.Certificates;
    using Windows.Storage.Pickers;
    using Windows.Storage;
    using Windows.Storage.Streams;
    ```

5.  在 MainPage.xaml.cs 文件中，将以下变量添加到 **MainPage** 类中。 它们指定“FirstContosoBank”服务的“登录”方法的地址以及承载要导入到证书存储中的 PFX 证书的全局变量。 将 <server-name> 更新为 Microsoft Internet Information Server (IIS) 服务器的完全限定的服务器名。
    ```cs
    private Uri requestUri = new Uri("https://&lt;server-name&gt;/FirstContosoBank/Service1.asmx?op=Login");
    private string pfxCert = null;
    ```

6.  在 MainPage.xaml.cs 文件中，添加登录按钮的单击处理程序和访问安全 Web 服务的方法。
    ```cs
    private void Login_Click(object sender, RoutedEventArgs e)
    {
        MakeHttpsCall();
    }

    private async void MakeHttpsCall()
    {

        StringBuilder result = new StringBuilder("Login ");
        HttpResponseMessage response;
        try
        {
            Windows.Web.Http.HttpClient httpClient = new Windows.Web.Http.HttpClient();
            response = await httpClient.GetAsync(requestUri);
            if (response.StatusCode == HttpStatusCode.Ok)
            {
                result.Append("successful");
            }
            else
            {
                result = result.Append("failed with ");
                result = result.Append(response.StatusCode);
            }
        }
        catch (Exception ex)
        {
            result = result.Append("failed with ");
            result = result.Append(ex.Message);
        }

        Result.Text = result.ToString();
    }
    ```

7.  在 MainPage.xaml.cs 文件中，添加以下用于浏览 PFX 文件的按钮和用于将选中的 PFX 文件导入到证书存储中的按钮的单击处理程序。
    ```cs
    private async void Import_Click(object sender, RoutedEventArgs e)
    {
        try
        {
            Result.Text = "Importing selected certificate into user certificate store....";
            await CertificateEnrollmentManager.UserCertificateEnrollmentManager.ImportPfxDataAsync(
                pfxCert,
                PfxPassword.Password,
                ExportOption.Exportable,
                KeyProtectionLevel.NoConsent,
                InstallOptions.DeleteExpired,
                "Import Pfx");

            Result.Text = "Certificate import succeded";
        }
        catch (Exception ex)
        {
            Result.Text = "Certificate import failed with " + ex.Message;
        }
    }

    private async void Browse_Click(object sender, RoutedEventArgs e)
    {

        StringBuilder result = new StringBuilder("Pfx file selection ");
        FileOpenPicker pfxFilePicker = new FileOpenPicker();
        pfxFilePicker.FileTypeFilter.Add(".pfx");
        pfxFilePicker.CommitButtonText = "Open";
        try
        {
            StorageFile pfxFile = await pfxFilePicker.PickSingleFileAsync();
            if (pfxFile != null)
            {
                IBuffer buffer = await FileIO.ReadBufferAsync(pfxFile);
                using (DataReader dataReader = DataReader.FromBuffer(buffer))
                {
                    byte[] bytes = new byte[buffer.Length];
                    dataReader.ReadBytes(bytes);
                    pfxCert = System.Convert.ToBase64String(bytes);
                    PfxPassword.Password = string.Empty;
                    result.Append("succeeded");
                }
            }
            else
            {
                result.Append("failed");
            }
        }
        catch (Exception ex)
        {
            result.Append("failed with ");
            result.Append(ex.Message); ;
        }

        Result.Text = result.ToString();
    }
    ```

8.  运行应用并登录到安全 Web 服务以及将 PFX 文件导入到本地证书存储中。

可使用这些步骤创建多个应用，这些应用使用同一个用户证书访问相同或不同的安全 Web 服务。

<!--HONumber=Mar16_HO5-->


