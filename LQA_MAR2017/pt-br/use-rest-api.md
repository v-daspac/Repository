# <a name="use-the-outlook-rest-apis-from-an-outlook-add-in"></a>Use as APIs REST do Outlook de um suplemento do Outlook

O namespace [Office.context.mailbox.item](..\..\reference\outlook\Office.context.mailbox.item.md) fornece acesso a vários dos campos comuns das mensagens e dos compromissos. No entanto, em alguns cenários um suplemento talvez precise acessar os dados que não são expostos pelo namespace. Por exemplo, o suplemento pode depender de propriedades personalizadas definidas por um aplicativo externo ou ela precisa pesquisar na caixa de correio do usuário pelas mensagens do mesmo remetente. Nessas situações, as [APIs REST do Outlook](https://dev.outlook.com/restapi/reference) é o método recomendado para recuperar as informações.

## <a name="get-an-access-token"></a>Obter um token de acesso

As APIs REST exigem um token portador no cabeçalho `Authorization`. Normalmente, os aplicativos usam fluxos de OAuth2 para recuperar um token. No entanto, os suplementos podem recuperar um token sem implementar o OAuth2 usando o novo método [Office.context.mailbox.getCallbackTokenAsync](https://dev.outlook.com/reference/add-ins/1.5/Office.context.mailbox.html#getCallbackTokenAsync) introduzido no requisito de Caixa de Correio para definir visualização 1.5.

> **Observação**: como o conjunto de requisitos 1.5 da Caixa de Correio está na versão de visualização, não será possível especificá-lo como um requisito no manifesto. 

Ao definir a opção `isRest` como `true`, você pode solicitar um token compatível com APIs REST.

### <a name="add-in-permissions-and-token-scope"></a>Permissões de suplementos e escopo do token

É importante levar em consideração o nível de acesso que seu suplemento precisará com as APIs REST. Na maioria dos casos, o token retornado por `getCallbackTokenAsync` fornecerá acesso somente leitura ao item atual. Isso é verdadeiro mesmo que seu suplemento especifique o nível de permissão `ReadWriteItem` em seu manifesto.

Se seu suplemento exigirá acesso de gravação para o item atual ou outros itens na caixa de correio do usuário, o suplemento precisará especificar o nível de permissão `ReadWriteMailbox` em seu manifesto. Nesse caso, o token retornado conterá acesso de leitura/gravação às mensagens, aos eventos e aos contatos do usuário.

### <a name="example"></a>Exemplo

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

## <a name="get-the-item-id"></a>Obter a ID do item

Para recuperar o item atual pela REST, o suplemento precisará da ID do item, formatada corretamente para REST. Isto é obtido pela propriedade [Office.context.mailbox.item.itemId](../../reference/outlook/Office.context.mailbox.item.md), mas algumas verificações devem ser feitas para garantir que seja uma ID formatada para REST.

- No Outlook Mobile, o valor retornado por `Office.context.mailbox.item.itemId` é uma ID formatada para REST e pode ser usado como está.
- Em outros clientes do Outlook, o valor retornado por `Office.context.mailbox.item.itemId` é uma ID formatada para EWS e deve ser convertida usando o método [Office.context.mailbox.convertToRestId](../../reference/outlook/Office.context.mailbox.md).

O suplemento pode determinar em qual cliente do Outlook ele estará será carregado verificando a propriedade [Office.context.mailbox.diagnostics.hostName](../../reference/outlook/Office.context.mailbox.diagnostics.md).

### <a name="example"></a>Exemplo

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

## <a name="get-the-rest-api-url"></a>Obter a URL da API REST

A informação final que seu suplemento precisa para chamar a API REST é o nome do host que deve usar para enviar solicitações de API. Estas informações estão na propriedade [Office.context.mailbox.restUrl](https://dev.outlook.com/reference/add-ins/1.5/Office.context.mailbox.html#restUrl).

### <a name="example"></a>Exemplo

```js
// Example: https://outlook.office.com
var restHost = Office.context.mailbox.restUrl;
```

## <a name="call-the-api"></a>Chamar a API

Depois que seu suplemento tiver o token de acesso, a ID do item e a URL da API REST, ele poderá passar essas informações para um serviço de back-end que chama a API REST ou pode chamá-la diretamente usando o AJAX. O exemplo a seguir chama a API REST do Outlook Mail para obter a mensagem atual.

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

## <a name="additional-resources"></a>Recursos adicionais

Para obter um exemplo que chama as APIs REST de um suplemento do Outlook, consulte [command-demo](https://github.com/jasonjoh/command-demo) no GitHub.