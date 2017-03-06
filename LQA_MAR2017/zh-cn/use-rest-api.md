# <a name="use-the-outlook-rest-apis-from-an-outlook-add-in"></a>从 Outlook 外接程序使用 Outlook REST API

[Office.context.mailbox.item](..\..\reference\outlook\Office.context.mailbox.item.md) 命名空间提供访问许多邮件和约会的公用字段的权限。但是，在某些方案中，外接程序可能需要访问命名空间未公开的数据。例如，外接程序可能依赖于外部应用设置的自定义属性，或需要搜索用户邮箱中来自同一发件人的邮件。在这些方案中，[Outlook REST API](https://dev.outlook.com/restapi/reference) 是推荐的检索信息的方法。

## <a name="get-an-access-token"></a>获取访问令牌

Outlook REST API 需要 `Authorization` 标头中的持有者令牌。通常情况下，应用使用 OAuth2 流来检索令牌。但是，外接程序可以检索令牌，而无需使用邮箱要求集 1.5 预览中介绍的新 [Office.context.mailbox.getCallbackTokenAsync](https://dev.outlook.com/reference/add-ins/1.5/Office.context.mailbox.html#getCallbackTokenAsync) 方法来实现 OAuth2。

> **注意**：因为邮箱要求集 1.5 处于预览阶段，所以无法将其指定为清单中的要求。 

通过将 `isRest` 选项设置为 `true`，可以请求与 REST API 兼容的令牌。

### <a name="add-in-permissions-and-token-scope"></a>外接程序权限和令牌范围

请务必考虑外接程序通过 REST API 所需要的访问级别。在大多数情况下，由 `getCallbackTokenAsync` 返回的令牌将仅提供对当前项的只读访问权限。即使外接程序在其清单中指定了 `ReadWriteItem` 权限级别也是如此。

如果外接程序需要当前项目或用户邮箱中的其他项目的写权限，则外接程序必须在其清单中指定 `ReadWriteMailbox` 权限级别。在这种情况下，所返回的令牌将包含用户的邮件、事件和联系人的读取/写入访问权限。

### <a name="example"></a>示例

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

## <a name="get-the-item-id"></a>获取项目 ID

为了通过 REST 检索当前项目，外接程序将需要项目的 ID，并针对 REST 设置了正确的格式。这可从 [Office.context.mailbox.item.itemId](../../reference/outlook/Office.context.mailbox.item.md) 属性获取，但应进行一些检查，以确保它是适用于 REST 格式的 ID。

- 在 Outlook Mobile 中，由 `Office.context.mailbox.item.itemId` 返回的值是适用于 REST 格式的 ID 并可按原样使用。
- 在其他 Outlook 客户端中，由 `Office.context.mailbox.item.itemId` 返回的值是适用于 EWS 格式的 ID，且必须使用 [Office.context.mailbox.convertToRestId](../../reference/outlook/Office.context.mailbox.md) 方法进行转换。

通过查看 [Office.context.mailbox.diagnostics.hostName](../../reference/outlook/Office.context.mailbox.diagnostics.md) 属性，外接程序可以确定它所加载的是哪个 Outlook 客户端。

### <a name="example"></a>示例

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

## <a name="get-the-rest-api-url"></a>获取 REST API URL

外接程序调用 REST API 所需的最后一部分信息是其发送 API 请求应使用的主机名。此信息在 [Office.context.mailbox.restUrl](https://dev.outlook.com/reference/add-ins/1.5/Office.context.mailbox.html#restUrl) 属性中。

### <a name="example"></a>示例

```js
// Example: https://outlook.office.com
var restHost = Office.context.mailbox.restUrl;
```

## <a name="call-the-api"></a>调用 API

外接程序具有访问令牌、项目 ID 和 REST API URL 的权限后，它可以将这些信息传递到调用 REST API 的后端服务，或者可以使用 AJAX 直接对其进行调用。以下示例调用 Outlook Mail REST API 以获取当前消息。

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

## <a name="additional-resources"></a>其他资源

有关从 Outlook 外接程序调用 REST API 的示例，请参阅 GitHub 上的 [command-demo](https://github.com/jasonjoh/command-demo)。