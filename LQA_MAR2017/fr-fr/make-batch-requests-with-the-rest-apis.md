# <a name="make-batch-requests-with-the-rest-apis"></a>Créer des requêtes de lots avec l’API REST
Découvrez comment utiliser l’option de requête `$batch` avec les API REST/OData.
 
Cet article explique comment regrouper des requêtes et des opérations dans l’API REST/OData de Microsoft SharePoint Online (et des instances locales de SharePoint 2016 et versions ultérieures) et le [sous-ensemble dédié aux fichiers et dossiers](http://msdn.microsoft.com/en-us/office/office365/api/files-rest-operations) de l’API REST d’Office 365. Cette technique vous permet d’améliorer les performances de votre complément en combinant plusieurs opérations dans une seule requête au serveur et en obtenant une seule réponse.

## <a name="executive-summary-of-the-batch-option"></a>Récapitulatif sur l’option $batch
SharePoint Online (et la version locale de SharePoint 2016 et versions ultérieures) et les API d’Office 365 utilisent l’option de requête `$batch` OData, et vous pouvez consulter la [documentation officielle](http://www.odata.org/documentation/odata-version-3-0/batch-processing) pour savoir comment l’utiliser (vous pouvez aussi consulter les billets de blog d’Andrew Connell sur le sujet, en commençant par la [partie 1, relative au regroupement par lots pour les API REST SharePoint](http://www.andrewconnell.com/blog/part-1-sharepoint-rest-api-batching-understanding-batching-requests)). Cet article est uniquement un rappel des points principaux :
 
- L’URL de requête se compose de l’URL du service racine et de l’option `$batch` ; par exemple, `https://fabrikam.sharepoint.com/_api/$batch` ou `https://fabrikam.office365.com/api/v1.0/me/$batch`.
    
- Le corps de la requête HTTP est de type MIME *mixte/multipartie*.
    
- Le corps de la requête est divisé en plusieurs parties, séparées par une chaîne de limitation spécifiée dans l’en-tête de la requête.
    
- Chaque partie possède son propre verbe HTTP, sa propre URL REST et son propre corps interne, le cas échéant.
    
- Une partie peut être une opération de lecture (ou un appel de fonction), ou un ensemble de modifications d’une ou plusieurs opérations d’écriture (ou appels de fonction). Un ensemble de modifications est lui-même un type MIME *mixte/multipartie* dont les sous-parties contiennent des opérations d’insertion, de mise à jour ou de suppression.
    
     **Important**  Pour l’instant, les API SharePoint et Office 365 ne prennent pas en charge la fonctionnalité « tout ou rien » pour les ensembles de modifications qui contiennent plusieurs opérations. Si l’une des opérations enfant échoue, les autres sont quand même exécutées et ne sont pas annulées.

## <a name="code-samples"></a>Exemples de code
Exemples de code qui utilisent l’option de requête `$batch` sur les API REST/OData SharePoint : 

-  **C# :** [OfficeDev/Core.ODataBatch](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.ODataBatch)
-  **JavaScript :** [andrewconnell/sp-o365-rest](https://github.com/andrewconnell/sp-o365-rest/blob/master/SpRestBatchSample/Scripts/App.js)
    

## <a name="example-requests-and-responses"></a>Exemples de requêtes et de réponses
Voici un exemple de requête HTTP brute effectuant un traitement par lots de deux opérations GET qui extraient les titres de tous les éléments de deux listes différentes.

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

Voici un exemple de corps d’une demande HTTP brute qui effectue le traitement par lots d’une opération DELETE sur une liste et d’une opération GET sur la liste des listes SharePoint.
 
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


## <a name="links-to-helpful-libraries"></a>Liens vers des bibliothèques utiles
Des bibliothèques OData prennent en charge le regroupement OData pour de nombreux langages. Nous vous en fournissons deux exemples en dessous. Pour une liste plus complète, voir [Bibliothèques OData](http://www.odata.org/libraries/).

-  [Bibliothèque OData .NET](http://msdn.microsoft.com/en-us/office/microsoft.data.odata%28v=vs.90%29). Voir en particulier les classes **ODataBatch***.
-  [Bibliothèque datajs](http://datajs.codeplex.com/documentation). Voir en particulier les [opérations par lots](http://datajs.codeplex.com/wikipage?title=datajs%20OData%20API&amp;referringTitle=Documentation#Batch).