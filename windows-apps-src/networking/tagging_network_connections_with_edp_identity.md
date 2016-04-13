---
Description: '本主题介绍了如何在企业数据保护 (EDP) 方案中创建网络连接之前创建受保护的线程上下文。'
MS-HAID: 'dev\_networking.tagging\_network\_connections\_with\_edp\_identity'
MSHAttr: 'PreferredLib:/library/windows/apps'
Search.Product: eADQiWindows 10XVcnh
title: 使用 EDP 标识标记网络连接
---

# 使用 EDP 标识标记网络连接

__注意__ 在版本 1511（版本 10586）或更早版本的 Windows 10 上无法应用企业数据保护 (EDP) 策略。

本主题介绍了如何在企业数据保护 (EDP) 方案中创建网络连接之前创建受保护的线程上下文。 有关 EDP 如何关联到文件、数据流、剪贴板、网络、后台任务以及锁屏下的数据保护的完整开发人员蓝图，请参阅[企业数据保护 (EDP)](../enterprise/edp-hub.md)。

**注意** [企业数据保护 (EDP) 示例](http://go.microsoft.com/fwlink/p/?LinkId=620031&clcid=0x409)中包含许多 EDP 方案。



## 先决条件


-   **完成 EDP 设置**

    请参阅[设置用于 EDP 的计算机](../enterprise/edp-hub.md#set-up-your-computer-for-EDP)。

-   **致力于生成企业启发式应用**

    自发确保企业数据处于管理企业控制之下的应用称为企业启发式应用。 启发式应用比未启发式应用更强大、更智能、更灵活，也更受信赖。 你的应用可以通过声明受限的 **enterpriseDataPolicy** 功能向系统宣布它为启发式应用。 但要实现启发式的应用比要设置功能的应用多。 若要了解详细信息，请参阅[企业启发式应用](../enterprise/edp-hub.md#enterprise-enlightened-apps)。

-   **了解通用 Windows 平台 (UWP) 应用的异步编程**

    若要了解如何使用 C\# 或 Visual Basic 编写异步应用，请参阅[使用 C\# 或 Visual Basic 调用异步 API](https://msdn.microsoft.com/library/windows/apps/mt187337)。 若要了解如何使用 C++ 编写异步应用，请参阅[使用 C++ 进行异步编程](https://msdn.microsoft.com/library/windows/apps/mt187334)。

## 通过网络访问企业资源


在此方案中，启发式邮件应用将同步一组混合企业邮箱和个人邮箱的邮箱。 该应用将用户标识传递到对 [**ProtectionPolicyManager.CreateCurrentThreadNetworkContext**](https://msdn.microsoft.com/library/windows/apps/dn706025) 的调用来创建一个受保护的线程上下文。 这将标记在同一线程上使用该标识随后创建的所有网络连接，并允许访问企业策略控制访问的企业网络资源。

此处“企业”是指用户标识所属的企业。 无论策略的执行情况如何，[**CreateCurrentThreadNetworkContext**](https://msdn.microsoft.com/library/windows/apps/dn706025) 都将返回 [**ThreadNetworkContext**](https://msdn.microsoft.com/library/windows/apps/dn706029) 对象。 通常情况下，如果应用需要处理混合资源，它可以选择调用适用于所有标识的 **CreateCurrentThreadNetworkContext**。 检索网络资源后，应用会调用 **ThreadNetworkContext** 上的 **Dispose** 以从当前线程中清除任何标识标记。 用于释放上下文对象的模式将取决于你的编程语言。

如果标识未知，应用可以使用 [**ProtectionPolicyManager.GetPrimaryManagedIdentityForNetworkEndpointAsync**](https://msdn.microsoft.com/library/windows/apps/dn706027) API 从资源的网络地址查询企业策略管理的标识。

**注意** 正如你在代码示例中看到的那样，适用于 [**CreateCurrentThreadNetworkContext**](https://msdn.microsoft.com/library/windows/apps/dn706025) 的正确使用模式将保持为它在其中起作用的最小范围。 你应设置企业网络上下文、创建网络连接，然后还原上下文、使用连接并随后将其关闭。 下面的代码示例说明了详细信息。 在线程上设置企业网络上下文的同时，不要在该线程上创建文件非常重要。 执行此操作将导致文件自动加密，而无论你是否打算将该文件用作个人文件。 这是我们建议尽早还原上下文的原因之一。



```CSharp
using Windows.Data.Xml.Dom;
using Windows.Networking;
using Windows.Security.EnterpriseData;
using Windows.Storage.Streams;
using Windows.Web.Http;

...

public static async void SyncMailbox(string identity)
{
    HttpClient client = null;
    // Create a protected network context for "identity" on the current
    // thread. Network connections made after this call will be tagged
    // to "identity", and will have policy enforced. This is required
    // to access enterprise network resources for "identity".
    using (ThreadNetworkContext threadNetworkContext = 
        ProtectionPolicyManager.CreateCurrentThreadNetworkContext(identity))
    {
        // Retrieve new mail.
        client = new HttpClient();
    }

    string xmlResponse = await client.GetStringAsync
        (new Uri("https://contosomail/getnewmail?user=" + identity));

    // Now, process mail data received.
    var xmlDocument = new XmlDocument();
    xmlDocument.LoadXml(xmlResponse);
    foreach (IXmlNode mailNode in xmlDocument.GetElementsByTagName("mail"))
    {
        // Code to process text data in mail (body, recipients etc.)
        // would go here ...

        // Processes resource links in mail body.
        foreach (IXmlNode childNode in mailNode.ChildNodes)
        {
            if (childNode.NodeName == "resource")
            {
                var resourceUri = new Uri(childNode.InnerText);

                // Check if the resource is on an enterprise-managed endpoint.
                string resourceIdentity =
                    await ProtectionPolicyManager.GetPrimaryManagedIdentityForNetworkEndpointAsync
                        (new HostName(resourceUri.Host));
                if (!string.IsNullOrEmpty(resourceIdentity))
                {
                    using (ThreadNetworkContext threadNetworkContext =
                        ProtectionPolicyManager.CreateCurrentThreadNetworkContext(resourceIdentity))
                    {
                        client = new HttpClient();
                    }
                    IBuffer data = await client.GetBufferAsync(resourceUri);
                    // Code to process retrieved resource data would go here...
                }
            }
        }
    }
}
```

**注意** 本文适用于编写通用 Windows 平台 (UWP) 应用的 Windows 10 开发人员。 如果你要针对 Windows 8.x 或 Windows Phone 8.x 进行开发，请参阅[存档文档](http://go.microsoft.com/fwlink/p/?linkid=619132)。



## 相关主题


[企业数据保护 (EDP) 示例](http://go.microsoft.com/fwlink/p/?LinkId=620031&clcid=0x409)

[**Windows.Security.EnterpriseData 命名空间**](https://msdn.microsoft.com/library/windows/apps/dn279153)

 

 





<!--HONumber=Mar16_HO5-->


