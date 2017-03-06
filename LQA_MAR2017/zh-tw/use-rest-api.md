# <a name="use-the-outlook-rest-apis-from-an-outlook-add-in"></a>使用 Outlook 增益集中的 Outlook REST API

[Office.context.mailbox.item](..\..\reference\outlook\Office.context.mailbox.item.md) 命名空間可供存取郵件和約會的許多通用欄位。不過，在某些案例中，增益集可能需要存取命名空間並未公開的資料。例如，增益集可能會仰賴外部應用程式所設定的自訂屬性，或者需要搜尋使用者信箱中來自相同寄件者的郵件。在這些案例中，[Outlook REST API](https://dev.outlook.com/restapi/reference) 是擷取資訊的建議方法。

## <a name="get-an-access-token"></a>取得存取權杖

Outlook REST API 需要 `Authorization` 標頭中的持有人權杖。應用程式通常使用 OAuth2 流程來擷取權杖。不過，增益集不需使用信箱需求集合 1.5 預覽版中引進的新 [Office.context.mailbox.getCallbackTokenAsync](https://dev.outlook.com/reference/add-ins/1.5/Office.context.mailbox.html#getCallbackTokenAsync) 方法來實作 OAuth2，即可擷取權杖。

> **注意**：因為信箱需求集合 1.5 處於預覽狀態，您無法將其指定為資訊清單中的需求。 

藉由將 `isRest` 選項設定為 `true`，您可以要求與 REST API 相容的權杖。

### <a name="add-in-permissions-and-token-scope"></a>增益集權限和權杖範圍

請務必考慮您的增益集透過 REST API 所需的存取層級。在大部分的情況下，`getCallbackTokenAsync` 傳回的權杖只會提供目前項目的唯讀權限。即使您的增益集在其資訊清單中指定 `ReadWriteItem` 權限層級，也是這種情況。

如果增益集需要使用者的信箱中目前項目或其他項目的寫入權限，增益集必須在其資訊清單中指定 `ReadWriteMailbox` 權限層級。在此情況下，傳回的權杖將包含使用者的郵件、事件和連絡人的讀取/寫入權限。

### <a name="example"></a>範例

```js
Office.context.mailbox.getCallbackTokenAsync({isRest: true}, function(result){
  if (result.status === "succeeded") {
    var accessToken = result.value;
    
    // Use the access token
    getCurrentItem(accessToken);
  } else {
    // Handle the error
  }
});
```

## <a name="get-the-item-id"></a>取得項目 ID

若要透過 REST 擷取目前項目，增益集需要已針對 REST 正確格式化的項目 ID。這取自 [Office.context.mailbox.item.itemId](../../reference/outlook/Office.context.mailbox.item.md) 屬性，但必須進行一些檢查，以確保它是 REST 格式的識別碼。

- 在 Outlook Mobile 中，`Office.context.mailbox.item.itemId` 傳回的值是 REST 格式的識別碼，可以直接使用。
- 在其他 Outlook 用戶端中，`Office.context.mailbox.item.itemId` 傳回的值是 EWS 格式的識別碼，而且必須使用 [Office.context.mailbox.convertToRestId](../../reference/outlook/Office.context.mailbox.md) 方法進行轉換。

增益集可藉由檢查 [Office.context.mailbox.diagnostics.hostName](../../reference/outlook/Office.context.mailbox.diagnostics.md) 屬性，來判斷正在載入的 Outlook 用戶端。

### <a name="example"></a>範例

```js
function getItemRestId() {
  // Currently the only Outlook Mobile version that supports add-ins
  // is Outlook for iOS.
  if (Office.context.mailbox.diagnostics.hostName === 'OutlookIOS') {
    // itemId is already REST-formatted
    return Office.context.mailbox.item.itemId;
  } else {
    // Convert to an item ID for API v2.0
    return Office.context.mailbox.convertToRestId(
      Office.context.mailbox.item.itemId,
      Office.MailboxEnums.RestVersion.v2_0
    );
  }
}
```

## <a name="get-the-rest-api-url"></a>取得 REST API URL

增益集呼叫 REST API 所需的最後一項資訊是傳送 API 要求時所應使用的主機名稱。這項資訊位於 [Office.context.mailbox.restUrl](https://dev.outlook.com/reference/add-ins/1.5/Office.context.mailbox.html#restUrl) 屬性中。

### <a name="example"></a>範例

```js
// Example: https://outlook.office.com
var restHost = Office.context.mailbox.restUrl;
```

## <a name="call-the-api"></a>呼叫 API

增益集一旦具備存取權杖、項目 ID 和 REST API URL，就可以將該資訊傳遞至呼叫 REST API 的後端服務，也可以直接使用 AJAX 進行呼叫。下列範例會呼叫 Outlook 郵件 REST API，以取得目前的郵件。

```js
function getCurrentItem(accessToken) {
  // Get the item's REST ID
  var itemId = getItemRestId();

  // Construct the REST URL to the current item
  // Details for formatting the URL can be found at 
  // https://msdn.microsoft.com/office/office365/APi/mail-rest-operations#get-a-message-rest
  var getMessageUrl = Office.context.mailbox.restUrl +
    '/api/v2.0/messages/' + itemId;

  $.ajax({
    url: getMessageUrl,
    dataType: 'json',
    headers: { 'Authorization': 'Bearer ' + accessToken }
  }).done(function(item){
    // Message is passed in `item`
    var subject = item.Subject;
    ...
  }).fail(function(error){
    // Handle error
  });
}
```

## <a name="additional-resources"></a>其他資源

如需從 Outlook 增益集呼叫 REST API 的範例，請參閱 GitHub 上的[命令示範](https://github.com/jasonjoh/command-demo)。