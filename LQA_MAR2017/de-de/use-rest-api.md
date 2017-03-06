# <a name="use-the-outlook-rest-apis-from-an-outlook-add-in"></a>Verwenden von Outlook-REST-APIs in Outlook-Add-Ins

Der [Office.context.mailbox.item](..\..\reference\outlook\Office.context.mailbox.item.md)-Namespace ermöglicht den Zugriff auf viele allgemeinen Felder von Nachrichten und Terminen. In einigen Fällen muss ein Add-In möglicherweise auf Daten zugreifen, die nicht von dem Namespace zur Verfügung gestellt werden. Das Add-In basiert möglicherweise auf benutzerdefinierten Eigenschaften außerhalb der App oder muss im Postfach des Benutzers nach Nachrichten von einem bestimmten Absender suchen. In diesen Fällen wird empfohlen, die [Outlook REST APIs](https://dev.outlook.com/restapi/reference)-Methode zum Abrufen der Informationen zu verwenden.

## <a name="get-an-access-token"></a>Abrufen eines Zugriffstokens

Die Outlook-REST-APIs erfordern ein Bearertoken im `Authorization`-Header. In der Regel verwenden Sie OAuth2-Flüsse zum Abrufen eines Tokens. Add-Ins können allerdings ein Token ohne die Implementierung von OAuth2 abrufen, indem Sie die neue [Office.context.mailbox.getCallbackTokenAsync](https://dev.outlook.com/reference/add-ins/1.5/Office.context.mailbox.html#getCallbackTokenAsync)-Methode verwenden, die in der Vorschauversion des Postfachanforderungssatzes 1.5 hinzugefügt wurde.

> **Hinweis**: Da der Postfachanforderungssatz 1.5 in der Vorschau verfügbar ist, können Sie diesen nicht als Anforderung im Manifest festlegen. 

Durch Festlegen der `isRest`-Option auf `true` können Sie ein Token anfordern, das mit den REST-APIs kompatibel ist.

### <a name="add-in-permissions-and-token-scope"></a>Add-In-Berechtigungen und Tokenumfang

Es ist wichtig, die Zugriffsebene zu berücksichtigen, die das Add-In über die REST-APIs benötigt. In den meisten Fällen stellt das über `getCallbackTokenAsync` zurückgegebene Token nur schreibgeschützten Zugriff auf das aktuelle Element bereit. Dies gilt auch, wenn das Add-In die `ReadWriteItem`-Berechtigungsstufe im Manifest angibt.

Wenn das Add-In Schreibzugriff auf das aktuelle Element oder anderen Elemente im Postfach des Benutzers benötigt, muss das Add-In die `ReadWriteMailbox`-Berechtigungsstufe im Manifest angeben. In diesem Fall enthält das zurückgegebene Token Lese-/Schreibzugriff auf Nachrichten, Ereignisse und Kontakte des Benutzers.

### <a name="example"></a>Beispiel

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

## <a name="get-the-item-id"></a>Abrufen der Element-ID

Um das aktuelle Element über REST abzurufen, benötigt das Add-In die ordnungsgemäß für REST formatierte Element-ID. Diese wird über die [Office.context.mailbox.item.itemId](../../reference/outlook/Office.context.mailbox.item.md)-Eigenschaft abgerufen, es sind jedoch einige Überprüfungen erforderlich, um sicherzustellen, dass es sich dabei um eine für REST formatierte ID handelt.

- In Outlook Mobile ist der von `Office.context.mailbox.item.itemId` zurückgegebene Wert ist eine ID im REST-Format und kann ohne Weiteres verwendet werden.
- In anderen Outlook-Clients ist der von `Office.context.mailbox.item.itemId` zurückgegebene Wert eine ID im EWS-Format und muss erst mithilfe der [Office.context.mailbox.convertToRestId](../../reference/outlook/Office.context.mailbox.md)-Methode konvertiert werden.

Das Add-In kann bestimmen, welcher Outlook-Client geladen wird, indem die [Office.context.mailbox.diagnostics.hostName](../../reference/outlook/Office.context.mailbox.diagnostics.md)-Eigenschaft überprüft wird.

### <a name="example"></a>Beispiel

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

## <a name="get-the-rest-api-url"></a>Abrufen der REST-API-URL

Die letzte Angaben, die das Add-In zum Aufrufen der REST-API benötigt, ist der Hostname, der zum Senden von API-Anforderungen verwendet werden soll. Diese Informationen sind in der [Office.context.mailbox.restUrl](https://dev.outlook.com/reference/add-ins/1.5/Office.context.mailbox.html#restUrl)-Eigenschaft enthalten.

### <a name="example"></a>Beispiel

```js
// Example: https://outlook.office.com
var restHost = Office.context.mailbox.restUrl;
```

## <a name="call-the-api"></a>Aufrufen der API

Sobald das Add-In über das Zugriffstoken, die Element-ID und die REST-API-URL verfügt, kann es diese Informationen entweder an einen Back-End-Dienst übergeben, der die REST-API aufruft, oder sie direkt mithilfe von AJAX verwenden. Im folgende Beispiel wird die Outlook-E-Mail-REST-API aufgerufen, um die aktuelle Nachricht abzurufen.

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

## <a name="additional-resources"></a>Zusätzliche Ressourcen

Ein Beispiel, in dem die REST-APIs im Outlook-Add-In aufgerufen werden, finden Sie unter [command-demo](https://github.com/jasonjoh/command-demo) auf GitHub.