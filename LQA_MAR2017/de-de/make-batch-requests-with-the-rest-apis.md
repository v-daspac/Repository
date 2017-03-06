# <a name="make-batch-requests-with-the-rest-apis"></a>Durchführen von Batchanforderungen mit den REST-APIs
Erfahren Sie, wie Sie die Abfrageoption `$batch` mit den REST-/OData-APIs verwenden.
 
In diesem Artikel wird beschrieben, wie Sie Vorgänge und Abfragen an die REST-/OData-API von Microsoft SharePoint Online (sowie des lokalen SharePoint 2016 und höher) und die [Unterklasse Dateien und Ordner](http://msdn.microsoft.com/en-us/office/office365/api/files-rest-operations) der Office 365 REST-APIs zusammenfassen. Mit diesem Verfahren können Sie die Leistung Ihrer Add-ins verbessern, indem Sie mehrere Vorgänge in einer einzelnen Anforderung an den Server und einer einzelnen Antwort kombinieren.

## <a name="executive-summary-of-the-batch-option"></a>Zusammenfassung der $batch-Option
SharePoint Online (sowie das lokale SharePoint 2016 und höher) und die Office 365-APIs implementieren die OData-`$batch`-Abfrageoption. Details zu ihrer Verwendung können Sie der [offiziellen Dokumentation](http://www.odata.org/documentation/odata-version-3-0/batch-processing) entnehmen. Sie können auch die Blogbeiträge von Andrew Connell zu diesem Thema lesen, beginnend bei [Part 1 - SharePoint REST API Batching](http://www.andrewconnell.com/blog/part-1-sharepoint-rest-api-batching-understanding-batching-requests) (Teil 1: Batchverarbeitung mit der SharePoint REST-API). Im Folgenden finden Sie eine Zusammenfassung der wichtigsten Punkte:
 
- Die URL der Anforderung besteht aus der Stammdienst-URL und der Option `$batch`. Beispiel: `https://fabrikam.sharepoint.com/_api/$batch` oder `https://fabrikam.office365.com/api/v1.0/me/$batch`.
    
- Der HTTP-Anforderungstext ist im MIME-Typ *multipart/mixed*.
    
- Der Text der Anforderung ist in Bereiche unterteilt, die durch eine Trennzeichenfolge voneinander getrennt sind, die im Header der Anforderung angegeben ist.
    
- Jeder Teil des Textkörpers verfügt über ein eigenes HTTP-Verb, eine eigene REST-URL sowie einen eigenen internen Textkörper, falls zutreffend.
    
- Ein Teil kann ein Lesevorgang (oder Funktionsaufruf) sein, bzw. ein ChangeSet einer oder mehrerer Schreibvorgänge (oder Funktionsaufrufe). Ein ChangeSet ist selbst ein MIME-Typ *multipart/mixed* mit Unterteilen, die Einfüge-, Aktualisierungs- oder Löschvorgänge enthalten.
    
     **Wichtig** Derzeit unterstützen SharePoint- und Office 365-APIs nicht die Funktion „alles oder nichts“ für ChangeSets mit mehr als einem Vorgang. Wenn einer der untergeordneten Vorgänge fehlschlägt, werden die anderen ausgeführt und nicht rückgängig gemacht.

## <a name="code-samples"></a>Codebeispiele
Beispiele für Code, der die Abfrageoption `$batch` für die SharePoint REST/OData-APIs verwendet: 

-  **C#:** [OfficeDev/Core.ODataBatch](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.ODataBatch)
-  **JavaScript:** [andrewconnell/sp-o365-rest](https://github.com/andrewconnell/sp-o365-rest/blob/master/SpRestBatchSample/Scripts/App.js)
    

## <a name="example-requests-and-responses"></a>Beispielanforderungen und -antworten
Das folgende Beispiel zeigt eine unformatierte HTTP-Anforderung, die zwei GET-Vorgänge zusammenfasst, die die Titel aller Elemente in zwei verschiedenen Listen abrufen.

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

Das folgende Beispiel zeigt den Textkörper einer unformatierten HTTP-Anforderung, die ein DELETE einer Liste und ein GET der SharePoint-Liste der Listen zusammenfasst.
 
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


## <a name="links-to-helpful-libraries"></a>Links zu hilfreichen Bibliotheken
Es gibt zahlreiche OData-Bibliotheken, die die OData-Batchverarbeitung für viele Sprachen unterstützen. Zwei Beispiele sind nachstehend aufgeführt. Eine vollständige Liste finden Sie unter [OData Libraries OData-Bibliotheken](http://www.odata.org/libraries/).

-  [.NET OData-Bibliothek](http://msdn.microsoft.com/en-us/office/microsoft.data.odata%28v=vs.90%29). Siehe insbesondere die **ODataBatch***-Klassen.
-  [Datajs-Bibliothek](http://datajs.codeplex.com/documentation). Siehe insbesondere [Batchvorgänge](http://datajs.codeplex.com/wikipage?title=datajs%20OData%20API&amp;referringTitle=Documentation#Batch).