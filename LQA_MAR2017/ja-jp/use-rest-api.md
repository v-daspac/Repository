# <a name="use-the-outlook-rest-apis-from-an-outlook-add-in"></a>Outlook アドインからの Outlook REST API の使用

[Office.context.mailbox.item](..\..\reference\outlook\Office.context.mailbox.item.md) 名前空間は、メッセージや予定の多くの共通フィールドへのアクセスを提供します。ただし、シナリオによっては、名前空間によって公開されないデータにアドインがアクセスする必要が生じる可能性があります。たとえば、アドインは外部アプリによって設定されるカスタム プロパティを使用する場合があります。あるいは、同じ送信者からのメッセージをユーザーのメールボックスから検索する必要があります。これらのシナリオでは、[Outlook REST API](https://dev.outlook.com/restapi/reference) を使用して情報を取得する方法が推奨されています。

## <a name="get-an-access-token"></a>アクセス トークンを取得する

Outlook REST API では、`Authorization` ヘッダーにベアラー トークンが必要です。通常、アプリは OAuth2 フローを使用してトークンを取得します。ただし、アドインは、メールボックス要件セット 1.5 プレビュー版で導入されている新しい [Office.context.mailbox.getCallbackTokenAsync](https://dev.outlook.com/reference/add-ins/1.5/Office.context.mailbox.html#getCallbackTokenAsync) メソッドを使用することにより、OAuth2 を実装せずにトークンを取得できます。

> **注**:メールボックス要件セット 1.5 はプレビュー版なので、マニフェストで要件として指定することはできません。 

`isRest` オプションを `true` に設定することにより、REST API と互換性があるトークンを要求できます。

### <a name="add-in-permissions-and-token-scope"></a>アドインのアクセス許可とトークンの範囲

REST API を経由してアドインが必要とするアクセスのレベルを考慮することが重要です。ほとんどの場合、`getCallbackTokenAsync` によって返されるトークンは、現在の項目への読み取り専用のアクセスのみを提供します。このことは、アドインがそのマニフェストに `ReadWriteItem` アクセス許可レベルを指定する場合にも当てはまります。

現在の項目またはユーザーのメールボックス内のその他の項目への書き込みアクセスがアドインに必要な場合、アドインがそのマニフェストに `ReadWriteMailbox` アクセス許可レベルを指定する必要があります。その場合、返されるトークンに、ユーザーのメッセージ、イベント、および連絡先に対する読み取り/書き込みアクセス権限が含まれます。

### <a name="example"></a>例

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

## <a name="get-the-item-id"></a>項目 ID を取得する

REST を経由して現在の項目を取得するには、REST 用に正しく書式設定された項目の ID がアドインに必要です。これは [Office.context.mailbox.item.itemId](../../reference/outlook/Office.context.mailbox.item.md) プロパティから取得されますが、REST 用に書式設定された ID であることを確認するためのいくつかの確認が必要です。

- Outlook Mobile の場合、`Office.context.mailbox.item.itemId` によって返される値が REST 用に形式設定された ID であり、そのまま使用できます。
- その他の Outlook クライアントの場合、`Office.context.mailbox.item.itemId` によって返される値が EWS 用に設定された ID であり、[Office.context.mailbox.convertToRestId](../../reference/outlook/Office.context.mailbox.md) メソッドを使用して変換する必要があります。

[Office.context.mailbox.diagnostics.hostName](../../reference/outlook/Office.context.mailbox.diagnostics.md) プロパティを確認することにより、アドインは読み込まれる Outlook クライアントを判別できます。

### <a name="example"></a>例

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

## <a name="get-the-rest-api-url"></a>REST API URL を取得する

REST API を呼び出すためにアドインで必要な情報の最終部分は、API 要求の送信に使用するホスト名です。この情報は [Office.context.mailbox.restUrl](https://dev.outlook.com/reference/add-ins/1.5/Office.context.mailbox.html#restUrl) プロパティにあります。

### <a name="example"></a>例

```js
// Example: https://outlook.office.com
var restHost = Office.context.mailbox.restUrl;
```

## <a name="call-the-api"></a>API を呼び出す

アドインがアクセス トークン、アイテム ID、および REST API URL を取得すると、REST API を呼び出すバックエンド サービスにその情報を渡すか、AJAX を使用して直接呼び出すことができるようになります。次の例は、Outlook Mail REST API を呼び出して現在のメッセージを取得します。

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

## <a name="additional-resources"></a>その他のリソース

Outlook アドインから REST API を呼び出す例については、GitHub の [コマンド デモ](https://github.com/jasonjoh/command-demo)を参照してください。