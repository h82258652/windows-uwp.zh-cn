# 对已转换的桌面应用进行签名

本文介绍如何对已转换为通用 Windows 平台 (UWP) 的桌面应用进行签名。 你必须先使用证书对 .appx 程序包进行签名，然后才能部署它。

## 使用桌面应用转换器 (DAC) 自动签名

运行 DAC 自动对 .appx 程序包进行签名时，请使用 ```-Sign``` 标志。 有关详细信息，请参阅[桌面应用转换器预览](desktop-to-uwp-run-desktop-app-converter.md)。

## 使用 SignTool.exe 手动签名

首先，使用 MakeCert.exe 创建一个证书。 如果系统要求你输入密码，请选择“无”。 

```cmd
C:\> MakeCert.exe -r -h 0 -n "CN=<publisher_name>" -eku 1.3.6.1.5.5.7.3.3 -pe -sv <my.pvk> <my.cer>
```

接下来，使用 pvk2pfx.exe 将你的公钥和私钥信息复制到证书中。 

```cmd
C:\> pvk2pfx.exe -pvk <my.pvk> -spc <my.cer> -pfx <my.pfx>
```
最后，使用 SignTool.exe 通过证书对 .appx 进行签名。

```cmd
C:\> signtool.exe sign -f <my.pfx> -fd SHA256 -v .\<outputAppX>.appx
``` 

有关其他详细信息，请参阅[如何使用 SignTool 对应用包进行签名](https://msdn.microsoft.com/library/windows/desktop/jj835835.aspx)。 

所有三个工具都随附在 Microsoft Windows 10 SDK 中。 若要直接调用它们，请从命令提示符调用 ```C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\Tools\VsDevCmd.bat``` 脚本。

## 常见错误

### 发布者和证书不匹配导致 Signtool 错误“错误: SignerSign() 失败”(-2147024885/0x8007000b)

Appx 清单中的发布者条目必须与要签名的证书的使用者匹配。  可使用以下任一方法查看证书的使用者。 

**选项 1：Powershell**

运行以下 PowerShell 命令。 可以将 .cer 或 .pfx 用作证书文件，因为它们具有相同的发布者信息。

```ps
(Get-PfxCertificate <cert_file>).Subject
```

**选项 2：文件资源管理器**

在文件资源管理器中双击证书、选择“详细信息”**选项卡，然后在列表中选择“使用者”**字段。 接着，就可以复制内容。 

**选项 3：CertUtil**

从命令行对 PFX 文件运行 **certutil**，然后从输出中复制“使用者”**字段。 

```cmd
certutil -dump <cert_file.pfx>
```

### 已损坏或格式不正确的验证码签名

本部分包含有关如何在可能包含已损坏或格式不正确的验证码签名的 AppX 程序包中标识可移植可执行 (PE) 文件的问题。 PE 文件中的无效验证码签名可能采用任何二进制格式（如 .exe、.dll、.chm 等），并且会阻止程序包正确签名，从而使其无法从 AppX 程序包部署。 

PE 文件的验证码签名的位置由可选头数据目录中的证书表项和关联的属性证书表指定。 在签名验证期间，这些结构中指定的信息用于找到 PE 文件上的签名。 如果这些值损坏，则文件可能看起来无法有效进行签名。 

若要使验证码签名正确，验证码签名必须符合以下情况：

- PE 文件中 **WIN_CERTIFICATE** 项的开头不得超过可执行文件的末尾
- **WIN_CERTIFCATE** 项应位于映像的末尾
- **WIN_CERTIFICATE** 项的大小必须为正数
- 对于 32 位可执行文件，**WIN_CERTIFICATE** 项必须在 **IMAGE_NT_HEADERS32** 结构之后开始，对于 64 位可执行文件，必须在 IMAGE_NT_HEADERS64 结构之后开始

有关更多详细信息，请参考[验证码门户可执行文件规范](http://download.microsoft.com/download/9/c/5/9c5b2167-8017-4bae-9fde-d599bac8184a/Authenticode_PE.docx)和 [PE 文件格式规范](https://msdn.microsoft.com/windows/hardware/gg463119.aspx)。 

请注意，当尝试对 AppX 程序包进行签名时，SignTool.exe 可输出已损坏或格式不正确的二进制文件列表。 若要执行此操作，请通过将环境变量 APPXSIP_LOG 设置为 1（如 ```set APPXSIP_LOG=1```）来启用详细日志记录并重新运行 SignTool.exe。

若要修复这些格式不正确的二进制文件，请确保它们符合上述要求。

## 另请参阅

- [SignTool](https://msdn.microsoft.com/library/windows/desktop/aa387764.aspx)
- [SignTool.exe（签名工具）](https://msdn.microsoft.com/library/8s9b9yaz.aspx)
- [如何使用 SignTool 对应用包进行签名](https://msdn.microsoft.com/library/windows/desktop/jj835835.aspx)

<!--HONumber=Sep16_HO2-->


