# <a name="use-the-outlook-rest-apis-from-an-outlook-add-in"></a>Использование интерфейсов REST API Outlook из надстройки Outlook

Пространство имен [Office.context.mailbox.item](..\..\reference\outlook\Office.context.mailbox.item.md) предоставляет доступ ко множеству общих полей сообщений и встреч. Однако в некоторых случаях надстройке может потребоваться доступ к данным, недоступным из этого пространства имен. Например, надстройка может использовать настраиваемые свойства, заданные внешним приложением, или искать в почтовом ящике пользователя сообщения от одного отправителя. В таких случаях для получения сведений рекомендуется использовать [интерфейсы REST API Outlook](https://dev.outlook.com/restapi/reference).

## <a name="get-an-access-token"></a>Получение маркера доступа

Интерфейсам REST API Outlook необходим токен носителя в заголовке `Authorization`. Как правило, приложения используют потоки OAuth2 для получения маркера. Однако надстройка может получить маркер без реализации OAuth2, используя новый метод [Office.context.mailbox.getCallbackTokenAsync](https://dev.outlook.com/reference/add-ins/1.5/Office.context.mailbox.html#getCallbackTokenAsync), который появился в предварительной версии набора требований 1.5 для почтовых ящиков.

> **Примечание.** Так как набор требований версии 1.5 для почтовых ящиков предоставляется в ознакомительных целях, его невозможно указать как требование в манифесте. 

Задав для параметра `isRest` значение `true`, вы можете запросить маркер, совместимый с интерфейсами REST API.

### <a name="add-in-permissions-and-token-scope"></a>Разрешения надстроек и область маркера

Важно учитывать уровень доступа через интерфейсы REST API, который потребуется надстройке. В большинстве случаев маркер, возвращенный методом `getCallbackTokenAsync`, предоставляет доступ только для чтения и только для текущего элемента. Это верно, даже если в манифесте надстройки указан уровень разрешений `ReadWriteItem`.

Если надстройке требуется доступ на запись к текущему элементу или другим элементам в почтовом ящике пользователя, в манифесте надстройки необходимо указать уровень разрешений `ReadWriteMailbox`. В этом случае возвращаемый маркер предоставляет доступ на чтение и запись к сообщениям, событиям и контактам пользователя.

### <a name="example"></a>Пример

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

## <a name="get-the-item-id"></a>Получение идентификатора элемента

Чтобы получить текущий элемент с помощью REST, надстройке потребуется его идентификатор, правильно отформатированный для службы REST. Его можно получить из свойства [Office.context.mailbox.item.itemId](../../reference/outlook/Office.context.mailbox.item.md), но необходимо выполнить некоторые проверки, чтобы убедиться, что идентификатор отформатирован для REST.

- В Outlook Mobile свойство `Office.context.mailbox.item.itemId` возвращает идентификатор в формате REST, который можно использовать без изменений.
- В других клиентах Outlook свойство `Office.context.mailbox.item.itemId` возвращает идентификатор в формате EWS, который необходимо преобразовать с помощью метода [Office.context.mailbox.convertToRestId](../../reference/outlook/Office.context.mailbox.md).

Надстройка может определить, в каком клиенте Outlook она загружена, с помощью свойства [Office.context.mailbox.diagnostics.hostName](../../reference/outlook/Office.context.mailbox.diagnostics.md).

### <a name="example"></a>Пример

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

## <a name="get-the-rest-api-url"></a>Использование URL-адреса REST API

Последнее значение, необходимое надстройке для вызова REST API, — это имя узла, используемое для отправки запросов API. Оно содержится в свойстве [Office.context.mailbox.restUrl](https://dev.outlook.com/reference/add-ins/1.5/Office.context.mailbox.html#restUrl).

### <a name="example"></a>Пример

```js
// Example: https://outlook.office.com
var restHost = Office.context.mailbox.restUrl;
```

## <a name="call-the-api"></a>Вызов API

Когда надстройка получит маркер доступа, идентификатор элемента и URL-адрес REST API, она может передать эти сведения внутренней службе, которая вызовет REST API, или вызвать его напрямую с помощью AJAX. В приведенном ниже примере вызывается REST API Почты Outlook для получения текущего сообщения.

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

## <a name="additional-resources"></a>Дополнительные ресурсы

Пример вызова интерфейсов REST API из надстроек Outlook — [command-demo](https://github.com/jasonjoh/command-demo) на сайте GitHub.