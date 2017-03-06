# <a name="fetchurlconfigurationoptions"></a>fetch(url,configuration,options)
**Nota:** al momento, SharePoint Framework è in versione di anteprima e potrebbe essere soggetto a modifiche. Le web part sul lato client di SharePoint Framework attualmente non sono supportate in ambienti di produzione.



Generalmente, i parametri e la semantica di SPHttpClient.fetch() sono praticamente uguali allo standard API WHATWG documentato qui: https://fetch.spec.whatwg.org/ La sottoclasse SPHttpClient aggiunge alcuni comportamenti che sono utili quando si lavora con le API ODATA di SharePoint (che possono essere evitate utilizzando in alternativa HttpClient): - Le intestazioni predefinite "Accept" e "Content-Type" vengono aggiunte se non specificato esplicitamente. - Per le operazioni di scrittura, un'intestazione "X-RequestDigest" viene aggiunta automaticamente - Il token digest della richiesta viene recuperato automaticamente e memorizzato in una cache, con il supporto di precaricamento Per un'operazione di scrittura, SPHttpClient aggiungerà automaticamente l'intestazione "X-RequestDigest", che potrebbe essere ottenuta inviando una richiesta separata come ad esempio "https://example.com/sites/sample/_api/contextinfo". In genere l'URL SPWeb appropriato può essere ipotizzato cercando un segmento URL riservato, ad esempio "_api" nell'URL originale passato a fetch(); in caso contrario, utilizzare ISPHttpClientOptions.webUrl per specificarlo in modo esplicito.

**Firma:** _@override public fetch(url: string, configurazione: [SPHttpClientConfiguration](../sp-http/sphttpclientconfiguration.md), opzioni: [ISPHttpClientOptions](../sp-http/isphttpclientoptions.md)): Promise<[SPHttpClientResponse](../sp-http/sphttpclientresponse.md)>;_

**Restituisce**: `Promise<SPHttpClientResponse>`



una promessa che verrà restituito il risultato

#### <a name="parameters"></a>Parametri


| Parametro    | Tipo    | Descrizione |
|:-------------|:---------------|:------------|
| `url`    | `string` | l'URL per recuperare |
| `configuration`    | [`SPHttpClientConfiguration`](../sp-http/sphttpclientconfiguration.md) | determina il comportamento predefinito di SPHttpClient; in genere dovrebbe essere il numero di versione più recente da SPHttpClientClientConfigurations |
| `options`    | [`ISPHttpClientOptions`](../sp-http/isphttpclientoptions.md) | altre opzioni che riguardano la richiesta |


