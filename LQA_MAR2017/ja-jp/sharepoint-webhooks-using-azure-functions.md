# <a name="using-azure-functions-with-sharepoint-webhooks"></a>Azure Functions と SharePoint Webhooks の使用 #
[Azure Functions](https://docs.microsoft.com/en-us/azure/azure-functions/functions-overview) を使用すると、SharePoint Webhook を簡単にホストできます。ブラウザーで Webhook C# や Javascript コードを追加すると、Azure で関数のホストとスケーリングが行われます。このガイドでは、Webhook 用の Azure Functions をセットアップして使用する方法を説明します。

## <a name="create-a-an-azure-function-app"></a>Azure 関数アプリを作成する
最初に Azure 関数アプリを作成する必要があります。これは、Azure Functions をホストすることを主とした特殊な Azure Web アプリです。[https://portal.azure.com](https://portal.azure.com) に移動し、[新規] をクリックして、"関数アプリ" を検索してください。

![Azure 関数アプリの作成](../../../images/webhook-azure-function0.png)

[関数アプリ] を選択し、関数アプリの作成に必要な情報をすべて入力します。

![Azure 関数アプリの詳細をすべて入力する](../../../images/webhook-azure-function1.png)

## <a name="create-an-azure-function"></a>Azure 関数を作成する
関数をホストするためのアプリが完成したら、"新しい関数" リンクをクリックして、最初の Azure 関数を作成できます。

![Azure 関数の追加](../../../images/webhook-azure-function2.png)

ここでは、テンプレートから関数を開始できます。SharePoint Webhooks の場合、HTTP でトリガーされる関数が必要です。このサンプルでは C# コードを作成するため、**HttpTrigger-CSharp** 関数テンプレートを使用します。SharePoint Webhook サービスが匿名で呼び出し可能であることが必要だとすると、**[承認レベル]** を **[匿名]** に切り替えることは重要です。

![Azure 関数テンプレートの選択](../../../images/webhook-azure-function3.png)

>**注:**現在、**GenericWebHook** テンプレートは SharePoint Webhooks で使用できませんが、SharePoint 製品チームは問題を把握しており、取り組んでいます。

結果として、C# で記述された "既定" の Azure 関数が完成します。![既定の Azure 関数](../../../images/webhook-azure-function4.png)

今回は、この Azure 関数を SharePoint Webhook サービスとして機能させるため、C# で以下の内容を実装する必要があります。
- 呼び出しの URL パラメーターとして指定されている場合、validationtoken を返します。これが必要な理由は[ここ](./lists/create-subscription)で説明されています。SharePoint では 5 秒以内に返信が行われることを想定しています。 
- JSON Webhook 通知を処理します。以下のサンプルで、JSON をストレージ キューに格納することを選択することにより、Azure Web ジョブはその JSON を取得して、非同期的に処理できます。必要に応じて、通知を Webhook サービスで直接処理することもできますが、Webhook サービスのすべての呼び出しは 5 秒以内に完了する必要があるため、非同期モデルの使用をお勧めします。

上記の内容は、既定のコードを以下のコードに置き換えることによって実行できます (別のキュー名を使用している場合は、ストレージ アカウント接続文字列を入力して、キュー名を更新してください)。

```C#
#r "Newtonsoft.Json"
#r "Microsoft.WindowsAzure.Storage"

using System;
using System.Net;
using Newtonsoft.Json;
using Microsoft.WindowsAzure;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Queue;

public static async Task<object> Run(HttpRequestMessage req, TraceWriter log)
{
    log.Info($"Webhook was triggered!");

    // Grab the validationToken URL parameter
    string validationToken = req.GetQueryNameValuePairs()
        .FirstOrDefault(q => string.Compare(q.Key, "validationtoken", true) == 0)
        .Value;
    
    // If a validation token is present, we need to respond within 5 seconds by  
    // returning the given validation token. This only happens when a new 
    // web hook is being added
    if (validationToken != null)
    {
      log.Info($"Validation token {validationToken} received");
      var response = req.CreateResponse(HttpStatusCode.OK);
      response.Content = new StringContent(validationToken);
      return response;
    }

    log.Info($"SharePoint triggered our webhook...great :-)");
    var content = await req.Content.ReadAsStringAsync();
    log.Info($"Received following payload: {content}");

    var notifications = JsonConvert.DeserializeObject<ResponseModel<NotificationModel>>(content).Value;
    log.Info($"Found {notifications.Count} notifications");

    if (notifications.Count > 0)
    {
        log.Info($"Processing notifications...");
        foreach(var notification in notifications)
        {
            CloudStorageAccount storageAccount = CloudStorageAccount.Parse("<YOUR STORAGE ACCOUNT>");
            // Get queue... create if does not exist.
            CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
            CloudQueue queue = queueClient.GetQueueReference("sharepointlistwebhookeventazuread");
            queue.CreateIfNotExists();

            // add message to the queue
            string message = JsonConvert.SerializeObject(notification);
            log.Info($"Before adding a message to the queue. Message content: {message}");
            queue.AddMessage(new CloudQueueMessage(message));
            log.Info($"Message added :-)");
        }
    }

    // if we get here we assume the request was well received
    return new HttpResponseMessage(HttpStatusCode.OK);
}


// supporting classes
public class ResponseModel<T>
{
    [JsonProperty(PropertyName = "value")]
    public List<T> Value { get; set; }
}

public class NotificationModel
{
    [JsonProperty(PropertyName = "subscriptionId")]
    public string SubscriptionId { get; set; }

    [JsonProperty(PropertyName = "clientState")]
    public string ClientState { get; set; }

    [JsonProperty(PropertyName = "expirationDateTime")]
    public DateTime ExpirationDateTime { get; set; }

    [JsonProperty(PropertyName = "resource")]
    public string Resource { get; set; }

    [JsonProperty(PropertyName = "tenantId")]
    public string TenantId { get; set; }

    [JsonProperty(PropertyName = "siteUrl")]
    public string SiteUrl { get; set; }

    [JsonProperty(PropertyName = "webId")]
    public string WebId { get; set; }
}

public class SubscriptionModel
{
    [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
    public string Id { get; set; }

    [JsonProperty(PropertyName = "clientState", NullValueHandling = NullValueHandling.Ignore)]
    public string ClientState { get; set; }

    [JsonProperty(PropertyName = "expirationDateTime")]
    public DateTime ExpirationDateTime { get; set; }

    [JsonProperty(PropertyName = "notificationUrl")]
    public string NotificationUrl {get;set;}

    [JsonProperty(PropertyName = "resource", NullValueHandling = NullValueHandling.Ignore)]
    public string Resource { get; set; }
}
```

## <a name="configure-your-azure-function"></a>Azure 関数を構成する
適切なテンプレートを選択し、構成からの開始が間もなく完了するため、今行う必要があるのは、**[許可されている HTTP メソッド]** を **[選択されたメソッド]** に切り替え、**POST** HTTP メソッドのみを許可することです。また、**[モード]** が **[標準]** に、**[承認レベル]** が **[匿名]** に設定されていることをご確認ください。

![Azure 関数の設定](../../../images/webhook-azure-function5.png)

## <a name="test-your-azure-function"></a>Azure 関数をテストする
最初の Azure 関数をテストする準備ができました。**[開発]** 画面に移動します。**[テスト]** アイコンをクリックして、右側にテスト ペインを表示し、URL パラメーター "validationtoken" を追加して、その値としてランダムな文字列を入力します。このセットアップを使用して、新しく追加した Webhook の検証の際に、Webhook サービスを呼び出す SharePoint の動作を模倣します。**[実行]** をクリックしてテストを開始します。正常に完了すると、ログ セクションには、サービスが呼び出され、渡された値と HTTP 200 応答が返されたことが表示されます。

![Azure 関数のテスト](../../../images/webhook-azure-function6.png)

## <a name="grab-the-webhook-url-to-use-in-your-implementation"></a>実装で使用する Webhook URL の把握
どの Webhook URL を使用するかを SharePoint に認識させる必要があります。最初に Azure 関数の URL をコピーします。

![Azure 関数の承認コード](../../../images/webhook-azure-function8.png)

Azure 関数の不正使用を避けるため、呼び出し元は、関数を呼び出すときにコードを指定する必要があります。このコードは **[管理]** 画面から取得できます。

![Azure 関数の Webhook URL](../../../images/webhook-azure-function7.png)

今回の場合、使用する Webhook URL は `https://pnp-functions.azurewebsites.net/api/spwebhookfunction?code=wyx9iAxp3o7fdGFZTbnp9Kfc5o2UhlzwgSOT/XGGM6QZcdYYa/o9aw==` です。



