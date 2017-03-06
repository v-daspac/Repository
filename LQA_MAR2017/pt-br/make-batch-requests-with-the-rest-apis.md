# <a name="make-batch-requests-with-the-rest-apis"></a>Fazer solicitações em lote com as APIs REST
Saiba como usar a opção de consulta `$batch` com as APIs REST/OData.
 
Este arquivo descreve como você pode fazer consultas e operações em lote na API REST/OData API do Microsoft SharePoint Online (e SharePoint 2016 no local e posterior) e [Subconjunto de arquivos e pastas](http://msdn.microsoft.com/en-us/office/office365/api/files-rest-operations) da API REST do Office 365. Com essa técnica, você pode melhorar o desempenho de seu suplemento combinando várias opções em uma única solicitação para o servidor e uma única resposta.

## <a name="executive-summary-of-the-batch-option"></a>Resumo executivo da opção $batch
As APIs do SharePoint Online (e o SharePoint 2016 e posterior no local) e do Office 365 implementam a opção de consulta OData `$batch` para que você possa contar com a [documentação oficial](http://www.odata.org/documentation/odata-version-3-0/batch-processing) para obter detalhes sobre como usá-la. (Outra opção é consultar os posts no blog de Andrew Connell sobre o assunto, começando em [Parte 1 - Lote da API REST do SharePoint](http://www.andrewconnell.com/blog/part-1-sharepoint-rest-api-batching-understanding-batching-requests).) A seguir encontra-se apenas um lembrete dos pontos principais:
 
- A URL da solicitação consiste em URL de serviço raiz e a opção `$batch`, por exemplo `https://fabrikam.sharepoint.com/_api/$batch` ou `https://fabrikam.office365.com/api/v1.0/me/$batch`.
    
- O corpo da solicitação HTTP é do tipo MIME *várias partes/misto*.
    
- O corpo da solicitação é dividido em partes separadas umas das outras por uma cadeia de caracteres limite que é especificada no cabeçalho da solicitação.
    
- Cada parte do corpo tem seu próprio verbo HTTP e URL REST e seu próprio corpo interno quando aplicável.
    
- Uma parte pode ser uma operação de leitura (ou chamada de função) ou um ChangeSet de uma ou mais operações de gravação (ou chamadas de função). Um ChangeSet em si é um tipo MIME *multipart/mixed* com subpartes que contém operações inserir, atualizar ou excluir.
    
     **Importante** Nesse momento, as APIs do SharePoint e do Office 365 não oferecem suporte a funcionalidades "tudo ou nada" para ChangeSets que tem mais de uma operação nelas. Se qualquer uma das operações filho falhar, as outras ainda são concluídas e não são revertidas.

## <a name="code-samples"></a>Code samples
Exemplos de código que usam a opção de consulta `$batch` em APIs REST/OData do SharePoint: 

-  **C#:** [OfficeDev/Core.ODataBatch](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.ODataBatch)
-  **JavaScript:** [andrewconnell/sp-o365-rest](https://github.com/andrewconnell/sp-o365-rest/blob/master/SpRestBatchSample/Scripts/App.js)
    

## <a name="example-requests-and-responses"></a>Exemplo de solicitações e respostas
A seguir está um exemplo de uma solicitação HTTP bruta que faz duas operações GET em lote que recupera os títulos de todos os itens em duas listas diferentes.

```
POST https://fabrikam.sharepoint.com/_api/$batch HTTP/1.1
Authorization: Bearer <access token omitted>
Content-Type: multipart/mixed; boundary=batch_e3b6819b-13c3-43bb-85b2-24b14122fed1
Host: fabrikam.sharepoint.com
Content-Length: 527
Expect: 100-continue

--batch_e3b6819b-13c3-43bb-85b2-24b14122fed1
Content-Type: application/http
Content-Transfer-Encoding: binary

GET https://fabrikam.sharepoint.com/_api/Web/lists/getbytitle('Composed%20Looks')/items?$select=Title HTTP/1.1

--batch_e3b6819b-13c3-43bb-85b2-24b14122fed1
Content-Type: application/http
Content-Transfer-Encoding: binary

GET https://fabrikam.sharepoint.com/_api/Web/lists/getbytitle('User%20Information%20List')/items?$select=Title HTTP/1.1

--batch_e3b6819b-13c3-43bb-85b2-24b14122fed1--

```

A seguir está um exemplo do corpo de uma solicitação HTTP bruta que faz operações DELETE de uma lista e GET da lista das listas do SharePoint.
 
```
POST https://fabrikam.sharepoint.com/_api/$batch HTTP/1.1
Authorization: Bearer <access token omitted>
Content-Type: multipart/mixed; boundary=batch_7ba8d60b-efce-4a2f-b719-60c27cc0e70e
Host: fabrikam.sharepoint.com
Content-Length: 647
Expect: 100-continue

--batch_7ba8d60b-efce-4a2f-b719-60c27cc0e70e
Content-Type: multipart/mixed; boundary=changeset_efb6b37c-a5cd-45cb-8f5f-4d648006e65d

--changeset_efb6b37c-a5cd-45cb-8f5f-4d648006e65d
Content-Type: application/http
Content-Transfer-Encoding: binary

DELETE https://fabrikam.sharepoint.com/_api/Web/lists/getbytitle('OldList') HTTP/1.1
If-Match: "1"

--changeset_efb6b37c-a5cd-45cb-8f5f-4d648006e65d--
--batch_7ba8d60b-efce-4a2f-b719-60c27cc0e70e
Content-Type: application/http
Content-Transfer-Encoding: binary

GET https://fabrikam.sharepoint.com/_api/Web/lists HTTP/1.1

--batch_7ba8d60b-efce-4a2f-b719-60c27cc0e70e--
```


## <a name="links-to-helpful-libraries"></a>Links para bibliotecas úteis
Há bibliotecas OData que oferecem suporte a lotes de OData para vários idiomas. Dois exemplos estão abaixo. Para obter uma lista mais completa, consulte [Bibliotecas OData](http://www.odata.org/libraries/).

-  [Biblioteca .NET OData](http://msdn.microsoft.com/en-us/office/microsoft.data.odata%28v=vs.90%29). Consulte especialmente as classes **ODataBatch***.
-  [Biblioteca Datajs](http://datajs.codeplex.com/documentation). Consulte especialmente [Operações em lote](http://datajs.codeplex.com/wikipage?title=datajs%20OData%20API&amp;referringTitle=Documentation#Batch).