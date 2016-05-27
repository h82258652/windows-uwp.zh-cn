---
author: mcleanbyron
ms.assetid: 4BF9EF21-E9F0-49DB-81E4-062D6E68C8B1
description: 使用 Windows 应用商店分析 API，针对已注册到你的或组织的 Windows 开发人员中心帐户的应用以编程方式检索分析数据。
title: 使用 Windows 应用商店服务访问分析数据
---

# 使用 Windows 应用商店服务访问分析数据


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


使用 *Windows 应用商店分析 API*，针对已注册到你的或组织的 Windows 开发人员中心帐户的应用以编程方式检索分析数据。 此 API 使你可以针对应用和 IAP 购置、错误、应用评分和评价检索数据。 此 API 使用 Azure Active Directory (Azure AD) 验证来自应用或服务的调用。

## 使用 Windows 应用商店分析 API 的先决条件


-   你（或你的组织）必须具有一个 Azure AD 目录。 如果你已使用 Office 365 或 Microsoft 的其他业务服务，则表示你已经具有 Azure AD 目录。 如果没有，你可以[免费获取一个](http://go.microsoft.com/fwlink/p/?LinkId=703757)。
-   必须在你想要与 Windows 开发人员中心帐户相关联的 Azure AD 目录中拥有一个[用户帐户](https://azure.microsoft.com/documentation/articles/active-directory-create-users/)。

## 使用 Windows 应用商店分析 API


在使用 Windows 应用商店分析 API 之前，必须将 Azure AD 应用程序与你的开发人员中心帐户相关联并获取 Azure AD 访问令牌。 Azure AD 应用程序是指你想要从中调用 Windows 应用商店分析 API 的应用或服务。 获得访问令牌后，可以从应用或服务调用 Windows 应用商店分析 API。

以下步骤介绍端到端过程：

1.  [将 Azure AD 应用程序与你的 Windows 开发人员中心帐户相关联](#associate-an-azure-ad-application-with-your-windows-dev-center-account)。
2.  [获取 Azure AD 访问令牌](#obtain-an-azure-ad-access-token)。
3.  [调用 Windows 应用商店分析 API](#call-the-windows-store-analytics-api)。


### 将 Azure AD 应用程序与你的 Windows 开发人员中心帐户相关联

1.  在开发人员中心中，转到**“帐户设置”**、单击**“管理用户”**，然后将你组织的开发人员中心帐户与 Azure AD 目录相关联。 有关详细说明，请参阅[管理帐户用户](https://msdn.microsoft.com/library/windows/apps/mt489008)。 可选择从你的组织的 Azure AD 目录添加其他用户，因此他们同样也可以访问开发人员中心帐户。

    **注意** 仅一个开发人员中心帐户可与 Azure Active Directory 相关联。 同样地，仅一个 Azure Active Directory 可与开发人员中心帐户相关联。 建立此关联后，你将无法在不联系支持人员的情况下删除它。

     

2.  在**“管理用户”**页面上，单击**“添加 Azure AD 应用程序”**、添加将用于访问你的开发人员中心帐户的分析数据的 Azure AD 应用程序，然后为其分配**“管理者”**角色。 如果此应用程序已存在于你的 Azure AD 目录中，你可以在**“添加 Azure AD 应用程序”**上选择它，以将其添加到你的开发人员中心帐户。 如果没有此应用程序，你可以在**“添加 Azure AD 应用程序”**页面创建新的 Azure AD 应用程序。 有关详细信息，请参阅[管理帐户用户](https://msdn.microsoft.com/library/windows/apps/mt489008)中有关管理 Azure AD 应用程序的部分。

3.  返回到**“管理用户”**页面、单击 Azure AD 应用程序的名称以转到应用程序设置，然后单击**“添加新密钥”**。 在接下来的屏幕上，记下**“客户端 ID”**和**“密钥”**值。 有关详细信息，请参阅[管理帐户用户](https://msdn.microsoft.com/library/windows/apps/mt489008)中有关管理 Azure AD 应用程序的部分。 你需要这些客户端 ID 和密钥来获取 Azure AD 访问令牌，以供在调用 Windows 应用商店分析 API 时使用。 在离开此页面后，你将无法再访问该信息。


### 获取 Azure AD 访问令牌

在将 Azure AD 应用程序与你的开发人员中心帐户关联并对该应用程序检索客户端 ID 和密钥后，可使用该信息来获取 Azure AD 访问令牌。 在调用 Windows 应用商店分析 API 中的任何方法之前，你需要一个访问令牌。

若要获取访问令牌，请按照[使用客户端凭据的服务到服务调用](https://msdn.microsoft.com/library/azure/dn645543.aspx)中的说明将 HTTP POST 发送到以下 Azure AD 终结点。

```syntax
https://login.microsoftonline.com/<tenant id>/oauth2/token
```

-   若要获取租户 ID，请登录[“Azure 管理门户”](http://manage.windowsazure.com/)、导航到**“Active Directory”**，然后单击已链接到开发人员中心帐户的目录。 此目录的租户 ID 已嵌入此页面的 URL 中，如以下示例中的 *your\_tenant\_ID* 字符串所示。

  ```syntax
  https://manage.windowsazure.com/@<your_tenant_name>#Workspaces/ActiveDirectoryExtension/Directory/<your_tenant_ID>/directoryQuickStart
  ```

-   对于 *client\_id* 和 *client\_secret* 参数，请为你之前从开发人员中心检索的应用程序指定客户端 ID 和密钥。
-   对于 *resource* 参数，请指定以下 URI：```https://manage.devcenter.microsoft.com```。


### 调用 Windows 应用商店分析 API

获取 Azure AD 访问令牌后，可以随时调用 Windows 应用商店分析 API。 有关每个方法的语法信息，请参阅以下文章。 必须将访问令牌传递到每个方法的 **Authorization** 标头。

-   [获取应用购置](get-app-acquisitions.md)
-   [获取 IAP 购置](get-in-app-acquisitions.md)
-   [获取错误报告数据](get-error-reporting-data.md)
-   [获取应用评分](get-app-ratings.md)
-   [获取应用评价](get-app-reviews.md)

## 代码示例


以下代码示例演示了如何获取 Azure AD 访问令牌以及如何从 C# 控制台应用调用 Windows 应用商店分析 API。 若要使用此代码示例，请将 *tenantId*、*clientId*、*clientSecret* 和 *appID* 变量分配给你的方案的相应值。 此示例需要 Newtonsoft 中的 [Json.NET 程序包](http://www.newtonsoft.com/json)，以便反序列化 Windows 应用商店分析 API 返回的 JSON 数据。

```CSharp
using Newtonsoft.Json;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Text;
using System.Threading.Tasks;

namespace TestAnalyticsAPI
{
    class Program
    {
        static void Main(string[] args)
        {
            string tenantId = "<your tenant ID>";
            string clientId = "<your client ID>";
            string clientSecret = "<your secret>";

            string scope = "https://manage.devcenter.microsoft.com";

            // Retrieve an Azure AD access token
            string accessToken = GetClientCredentialAccessToken(
                    tenantId,
                    clientId,
                    clientSecret,
                    scope).Result;

            // This is your app's product ID. This ID is embedded in the app's listing link
            // on the App identity page of the Dev Center dashboard.
            string appID = "<your app's product ID>";

            DateTime startDate = DateTime.Parse("08-01-2015");
            DateTime endDate = DateTime.Parse("11-01-2015");
            int pageSize = 1000;
            int startPageIndex = 0;

            // Call the Windows Store analytics API
            CallAnalyticsAPI(accessToken, appID, startDate, endDate, pageSize, startPageIndex);

            Console.Read();
        }

        private static void CallAnalyticsAPI(string accessToken, string appID, DateTime startDate, DateTime endDate, int top, int skip)
        {
            string requestURI;

            // Get app acquisitions
            requestURI = string.Format(
                "https://manage.devcenter.microsoft.com/v1.0/my/analytics/appacquisitions?applicationId={0}&startDate={1}&endDate={2}&top={3}&skip={4}",
                appID, startDate, endDate, top, skip);

            //// Get IAP acquisitions
            //requestURI = string.Format(
            //    "https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappacquisitions?applicationId={0}&startDate={1}&endDate={2}&top={3}&skip={4}",
            //    appID, startDate, endDate, top, skip);

            //// Get app failures
            //requestURI = string.Format(
            //    "https://manage.devcenter.microsoft.com/v1.0/my/analytics/failurehits?applicationId={0}&startDate={1}&endDate={2}&top={3}&skip={4}",
            //    appID, startDate, endDate, top, skip);

            //// Get app ratings
            //requestURI = string.Format(
            //    "https://manage.devcenter.microsoft.com/v1.0/my/analytics/ratings?applicationId={0}&startDate={1}&endDate={2}top={3}&skip={4}",
            //    appID, startDate, endDate, top, skip);

            //// Get app reviews
            //requestURI = string.Format(
            //    "https://manage.devcenter.microsoft.com/v1.0/my/analytics/reviews?applicationId={0}&startDate={1}&endDate={2}&top={3}&skip={4}",
            //    appID, startDate, endDate, top, skip);

            HttpRequestMessage requestMessage = new HttpRequestMessage(HttpMethod.Get, requestURI);
            requestMessage.Headers.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);

            WebRequestHandler handler = new WebRequestHandler();
            HttpClient httpClient = new HttpClient(handler);

            HttpResponseMessage response = httpClient.SendAsync(requestMessage).Result;

            Console.WriteLine(response);
            Console.WriteLine(response.Content.ReadAsStringAsync().Result);

            response.Dispose();
        }

        public static async Task<string> GetClientCredentialAccessToken(string tenantId, string clientId, string clientSecret, string scope)
        {
            string tokenEndpointFormat = "https://login.microsoftonline.com/{0}/oauth2/token";
            string tokenEndpoint = string.Format(tokenEndpointFormat, tenantId);

            dynamic result;
            using (HttpClient client = new HttpClient())
            {
                string tokenUrl = tokenEndpoint;
                using (
                    HttpRequestMessage request = new HttpRequestMessage(
                        HttpMethod.Post,
                        tokenUrl))
                {
                    string content =
                        string.Format(
                            "grant_type=client_credentials&client_id={0}&client_secret={1}&resource={2}",
                            clientId,
                            clientSecret,
                            scope);

                    request.Content = new StringContent(content, Encoding.UTF8, "application/x-www-form-urlencoded");

                    using (HttpResponseMessage response = await client.SendAsync(request))
                    {
                        string responseContent = await response.Content.ReadAsStringAsync();
                        result = JsonConvert.DeserializeObject(responseContent);
                    }
                }
            }

            return result.access_token;
        }
    }
}
```

## 错误响应


Windows 应用商店分析 API 会在 JSON 对象中返回含有错误代码和消息的错误响应。 以下示例演示了由无效参数引起的错误响应。

```json
{
    "code":"BadRequest",
    "data":[],
    "details":[],
    "innererror":{
        "code":"InvalidQueryParameters",
        "data":[
            "top parameter cannot be more than 10000"
        ],
        "details":[],
        "message":"One or More Query Parameters has invalid values.",
        "source":"AnalyticsAPI"
    },
    "message":"The calling client sent a bad request to the service.",
    "source":"AnalyticsAPI"
}
```

## 相关主题

* [获取应用购置](get-app-acquisitions.md)
* [获取 IAP 购置](get-in-app-acquisitions.md)
* [获取错误报告数据](get-error-reporting-data.md)
* [获取应用评分](get-app-ratings.md)
* [获取应用评价](get-app-reviews.md)
 


<!--HONumber=May16_HO2-->


