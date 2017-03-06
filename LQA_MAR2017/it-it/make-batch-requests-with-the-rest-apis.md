# <a name="make-batch-requests-with-the-rest-apis"></a>Effettuare richieste di batch con le API REST
Informazioni su come utilizzare l'opzione di query `$batch` con le API REST/OData.
 
In questo articolo viene descritto come è possibile effettuare operazioni e richieste in batch nell'API REST/OData di Microsoft SharePoint Online (e SharePoint 2016 locale e versioni successive) e nel [subset di file e cartelle](http://msdn.microsoft.com/en-us/office/office365/api/files-rest-operations) delle API REST di Office 365. Con questa tecnica è possibile migliorare le prestazioni del componente aggiuntivo, combinando molte operazioni in una singola richiesta per il server e in una singola risposta.

## <a name="executive-summary-of-the-batch-option"></a>Riepilogo generale dell'opzione $batch
SharePoint Online (e SharePoint 2016 locale e versioni successive) e le API di Office 365 implementano l'opzione di query `$batch` di OData, quindi è possibile fare riferimento alla [documentazione ufficiale](http://www.odata.org/documentation/odata-version-3-0/batch-processing) per informazioni dettagliate sul suo utilizzo. In alternativa, è possibile consultare i post del blog di Andrew Connell sull'argomento iniziando da [Part 1 - SharePoint REST API Batching](http://www.andrewconnell.com/blog/part-1-sharepoint-rest-api-batching-understanding-batching-requests) (Parte 1 - Invio in batch di API REST di SharePoint). Di seguito sono riportati solo alcuni dei punti più importanti:
 
- L'URL di richiesta è costituito dall'URL del servizio radice e l'opzione `$batch`; per esempio, `https://fabrikam.sharepoint.com/_api/$batch` o `https://fabrikam.office365.com/api/v1.0/me/$batch`.
    
- Il corpo della richiesta HTTP è tipo MIME *multipart/mixed*.
    
- Il corpo della richiesta è suddiviso in parti che sono separate l'una dall'altra da una stringa limite specificata nell'intestazione della richiesta.
    
- Ogni parte del corpo dispone di un verbo HTTP e un URL REST, oltre al corpo interno quando applicabile.
    
- Una parte può essere un'operazione di lettura (o chiamata di funzione) o un insieme di modifiche di una o più operazioni di scrittura (o chiamate di funzione). Un insieme di modifiche è un tipo MIME *multipart/mixed* con sottoparti contenenti operazioni di inserimento, aggiornamento o eliminazione.
    
     **Importante**  Al momento, le API di SharePoint e Office 365 non supportano le funzionalità "all or nothing" per insiemi di modifiche contenenti più di una operazione. Se una delle operazioni figlio ha esito negativo, le altre non vengono completate e non ne viene eseguito il rollback.

## <a name="code-samples"></a>Esempi di codice
Esempi di codice che utilizzano l'opzione di query `$batch` rispetto alle API REST/OData di SharePoint: 

-  **C#:** [OfficeDev/Core.ODataBatch](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.ODataBatch)
-  **JavaScript:** [andrewconnell/sp-o365-rest](https://github.com/andrewconnell/sp-o365-rest/blob/master/SpRestBatchSample/Scripts/App.js)
    

## <a name="example-requests-and-responses"></a>Richieste e risposte di esempio
Di seguito viene riportato l'esempio di una richiesta HTTP base che invia in batch due operazioni GET che consentono di recuperare i titoli di tutti gli elementi in due elenchi diversi.

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

Di seguito viene riportato un esempio del corpo di una richiesta HTTP base che invia in batch un'operazione DELETE di un elenco e un'operazione GET della lista di elenchi di SharePoint.
 
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


## <a name="links-to-helpful-libraries"></a>Collegamenti a raccolte utili
Esistono raccolte OData che supportano l'invio in batch di OData per più lingue. Di seguito vengono forniti due esempi. Per un elenco più completo, vedere [Raccolte di OData](http://www.odata.org/libraries/).

-  [Raccolta di OData .NET](http://msdn.microsoft.com/en-us/office/microsoft.data.odata%28v=vs.90%29). Vedere in particolare le classi **ODataBatch***.
-  [Raccolta di Datajs](http://datajs.codeplex.com/documentation). Vedere in particolare [Operazioni in batch](http://datajs.codeplex.com/wikipage?title=datajs%20OData%20API&amp;referringTitle=Documentation#Batch).