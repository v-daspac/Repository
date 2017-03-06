# <a name="get-incremental-changes-to-events-in-a-calendar-view-preview"></a>Inkrementelle Änderungen an Ereignissen in einer Kalenderansicht (Vorschau) abrufen

Eine Kalenderansicht ist eine Sammlung von Ereignissen in einen Datums-/Uhrzeitbereich aus dem Standardkalender (../me/calendarview) oder aus einem anderen Kalender des Benutzers. Durch Verwendung einer Delta-Abfrage können Sie neue, aktualisierte oder gelöschte Ereignisse in einer Kalenderansicht abrufen. Die zurückgegebenen Ereignisse können Vorkommen und Ausnahmen einer Serie und einzelne Instanzen enthalten. Mit Delta-Daten können Sie einen lokalen Speicher für Ereignisse eines Benutzers pflegen und synchronisieren, ohne dass Sie jedes Mal den gesamten Ereignissatz vom Server abrufen müssen.

Die Delta-Abfrage unterstützt die vollständige Synchronisierung, die alle Ereignisse in der angegebenen Kalenderansicht abruft, und die inkrementelle Synchronisierung, die nur die Ereignisse abruft, die seit der letzten Synchronisierung in der Kalenderansicht geändert wurden. In der Regel führen Sie zunächst eine vollständige Synchronisierung durch und rufen anschließend regelmäßig inkrementelle Änderungen an dieser Kalenderansicht ab. 

## <a name="track-event-changes-in-a-calendar-view"></a>Nachverfolgen von Ereignisänderungen in einer Kalenderansicht

Delta-Abfragen für Ereignisse sind spezifisch für einen Kalender und Datums-/Uhrzeitbereich, den Sie angeben (d. h. eine Kalenderansicht). Um die Änderungen in mehreren Kalendern nachzuverfolgen, müssen Sie jeden Kalender einzeln verfolgen. 

Das Nachverfolgen von Ereignisänderungen in einer Kalenderansicht ist in der Regel eine Runde aus einer oder mehreren GET-Anforderungen mit der [Delta](../api-reference/beta/api/event_delta.md)-Funktion. Die ursprüngliche GET-Anforderung wird ähnlich wie das [Auflisten einer Kalenderansicht](https://graph.microsoft.io/en-us/docs/api-reference/beta/api/calendar_list_calendarview) durchgeführt, außer dass die folgende **delta**-Funktion eingeschlossen wird:

```
GET /me/calendarView/delta?startDateTime={start_datetime}&endDateTime={end_datetime}
```

Eine GET-Anforderung mit der **delta**-Funktion gibt Folgendes zurück:

- `nextLink` (mit einer URL mit einem **delta**-Funktionsaufruf und einem _skipToken_) oder 
- `deltaLink` (mit einer URL mit einem **delta**-Funktionsaufruf und einem _deltaToken_).

Diese Token sind [Statustoken](./delta_query_overview#state-tokens), die die Parameter _startDateTime_ und _endDateTime_ sowie alle anderen Abfrageparameter in der GET-Anforderung der ursprünglichen Delta-Abfrage codieren. 

Zustandstoken sind für den Client nicht transparent. Um mit einer Änderungsnachverfolgung fortzufahren, kopieren Sie die von der letzten GET-Anforderung zurückgegebene `nextLink`- oder `deltaLink`-URL einfach und wenden sie auf den nächsten **delta**-Funktionsaufruf für dieselbe Kalenderansicht an. `deltaLink` (in einer Antwort zurückgegeben), bedeutet, dass die aktuelle Runde der Änderungsnachverfolgung abgeschlossen ist. Sie können die `deltaLink`-URL für die nächste Runde speichern und verwenden.

Im [Beispiel](#example-to-synchronize-events-in-a-calendar-view) unten finden Sie Informationen zur Verwendung der `nextLink`- und `deltaLink`-URLs.

### <a name="use-query-parameters-in-a-delta-query-for-calendar-view"></a>Verwenden von Abfrageparametern in einer Delta-Abfrage für eine Kalenderansicht

- Schließen Sie die Parameter _startDateTime_ und _endDateTime_ ein, um einen Datums-/Uhrzeitbereich für Ihre Kalenderansicht zu definieren.
- `$select` wird nicht unterstützt.


### <a name="optional-request-header"></a>Optionaler Anforderungsheader

Jede GET-Anforderung der Delta-Abfrage gibt eine Sammlung aus einem oder mehreren Ereignissen in der Antwort zurück. Sie können optional den Anforderungsheader, _Prefer: odata.maxpagesize={x}_, angeben, um die maximale Anzahl an Ereignissen in einer Antwort festzulegen.


## <a name="example-to-synchronize-events-in-a-calendar-view"></a>Beispiel für die Synchronisierung von Ereignissen in einer Kalenderansicht

Das folgende Beispiel zeigt eine Reihe von 3 Anforderungen zum Synchronisieren des Standardkalenders eines Benutzers in einem bestimmten Zeitbereich. Es gibt 5 Ereignisse in dieser Kalenderansicht.

- [Schritt 1: Beispiel für ursprüngliche Anforderung](#step-1-sample-initial-request) und [Antwort](#sample-initial-response)
- [Schritt 2: Beispiel für zweite Anforderung](#step-2-sample-second-request) und [Antwort](#sample-second-response)
- [Schritt 3: Beispiel für dritte Anforderung](#step-3-sample-third-request) und [letzte Antwort](#sample-third-and-final-response)

Aus Platzgründen wird in den Beispielantworten nur eine Untergruppe der Eigenschaften eines Ereignisses angezeigt. In einem tatsächlichen Aufruf werden die meiste Ereigniseigenschaften zurückgegeben. 

Sehen Sie sich auch an, was Sie in der [nächsten Runde](#the-next-round-sample-first-response) tun werden.


### <a name="step-1-sample-initial-request"></a>Schritt 1: Beispiel für ursprüngliche Anforderung

In diesem Beispiel wird die angegebene Kalenderansicht zum ersten Mal synchronisiert, sodass die ursprüngliche Synchronisierungsanforderung kein Statustoken enthält. In dieser Runde werden alle Ereignisse in dieser Kalenderansicht zurückgegeben.

Die erste Anforderung gibt Folgendes an:

- Datum-/Uhrzeitwerte für die Parameter _startDateTime_ und _endDateTime_.
- Den [optionalen Anforderungsheader](#optional-request-header), _odata.maxpagesize_, der gleichzeitig 2 Ereignisse zurückgibt.

<!-- {
  "blockType": "request",
  "name": "get_calendarview_delta_1"
}-->
```http
GET https://graph.microsoft.com/beta/me/calendarview/delta?startdatetime=2016-12-01T00:00:00Z&enddatetime=2016-12-30T00:00:00Z HTTP/1.1
Prefer: odata.maxpagesize=2
```


### <a name="sample-initial-response"></a>Beispiel für ursprüngliche Antwort

Die Antwort enthält zwei Ereignisse und einen `@odata.nextLink`-Antwortheader mit einem `skipToken`. Die `nextLink`-URL gibt an, dass in der Kalenderansicht weitere Ereignisse abgerufen werden können.

<!-- {
  "blockType": "response",
  "truncated": true,
  "@odata.type": "microsoft.graph.event",
  "isCollection": true
} -->
```http
HTTP/1.1 200 OK
Content-type: application/json

{
    "@odata.context":"https://graph.microsoft.com/beta/$metadata#Collection(event)",
    "@odata.nextLink":"https://graph.microsoft.com/beta/me/calendarview/delta?$skiptoken=R0usmcCM996atia_s",
    "value":[
        {
            "@odata.type":"#microsoft.graph.event",
            "@odata.etag":"W/\"EZ9r3czxY0m2jz8c45czkwAAFXcvIQ==\"",
            "subject":"Plan shopping list",
            "body":{
                "contentType":"html",
                "content":""
            },
            "start":{
                "dateTime":"2016-12-09T20:30:00.0000000",
                "timeZone":"UTC"
            },
            "end":{
                "dateTime":"2016-12-09T22:00:00.0000000",
                "timeZone":"UTC"
            },
            "attendees":[

            ],
            "organizer":{
                "emailAddress":{
                    "name":"Fanny Downs",
                    "address":"fannyd@contoso.onmicrosoft.com"
                }
            },      
            "id":"AAMkADNVxRAAA="
        },
        {
            "@odata.type":"#microsoft.graph.event",
            "@odata.etag":"W/\"EZ9r3czxY0m2jz8c45czkwAAFXcvIg==\"",
            "subject":"Pick up car",
            "body":{
                "contentType":"html",
                "content":""
            },
            "start":{
                "dateTime":"2016-12-10T01:00:00.0000000",
                "timeZone":"UTC"
            },
            "end":{
                "dateTime":"2016-12-10T02:00:00.0000000",
                "timeZone":"UTC"
            },
            "attendees":[

            ],
            "organizer":{
                "emailAddress":{
                    "name":"Fanny Downs",
                    "address":"fannyd@contoso.onmicrosoft.com"
                }
            },
            "id":"AAMkADVxSAAA="
        }
    ]
}
```

### <a name="step-2-sample-second-request"></a>Schritt 2: Beispiel für zweite Anforderung

Die zweite Anforderung gibt die aus der vorherigen Antwort zurückgegebene `nextLink`-URL an. Beachten Sie, dass sie nicht dieselben Parameter _startDateTime_ und _endDateTime_ wie in der ursprünglichen Anforderung angeben muss, da das `skipToken` in der `nextLink`-URL diese codiert und einschließt.

<!-- {
  "blockType": "request",
  "name": "get_calendarview_delta_2"
}-->
```http
GET https://graph.microsoft.com/beta/me/calendarview/delta?$skiptoken=R0usmcCM996atia_s HTTP/1.1
Prefer: odata.maxpagesize=2
```

### <a name="sample-second-response"></a>Beispiel für zweite Antwort 

Die zweite Antwort gibt die nächsten 2 Ereignisse in der Kalenderansicht und ein weiteres `nextLink`, was bedeutet, dass weitere abzurufende Ereignisse in der Kalenderansicht vorhanden sind.

<!-- {
  "blockType": "response",
  "truncated": true,
  "@odata.type": "microsoft.graph.event",
  "isCollection": true
} -->
```http
HTTP/1.1 200 OK
Content-type: application/json

{
    "@odata.context":"https://graph.microsoft.com/beta/$metadata#Collection(event)",
    "@odata.nextLink":"https://graph.microsoft.com/beta/me/calendarview/delta?$skiptoken=R0usmci39OQxqJrxK4",
    "value":[
        {
            "@odata.type":"#microsoft.graph.event",
            "@odata.etag":"W/\"EZ9r3czxY0m2jz8c45czkwAAFXcvIw==\"",
            "subject":"Get food",
            "body":{
                "contentType":"html",
                "content":""
            },
            "start":{
                "dateTime":"2016-12-10T19:30:00.0000000",
                "timeZone":"UTC"
            },
            "end":{
                "dateTime":"2016-12-10T21:30:00.0000000",
                "timeZone":"UTC"
            },
            "attendees":[

            ],
            "organizer":{
                "emailAddress":{
                    "name":"Fanny Downs",
                    "address":"fannyd@contoso.onmicrosoft.com"
                }
            },
            "id":"AAMkADVxTAAA="
        },
        {
            "@odata.type":"#microsoft.graph.event",
            "@odata.etag":"W/\"EZ9r3czxY0m2jz8c45czkwAAFXcvJA==\"",
            "subject":"Prepare food",
            "body":{
                "contentType":"html",
                "content":""
            },
            "start":{
                "dateTime":"2016-12-10T22:00:00.0000000",
                "timeZone":"UTC"
            },
            "end":{
                "dateTime":"2016-12-11T00:00:00.0000000",
                "timeZone":"UTC"
            },
            "attendees":[

            ],
            "organizer":{
                "emailAddress":{
                    "name":"Fanny Downs",
                    "address":"fannyd@contoso.onmicrosoft.com"
                }
            },
            "id":"AAMkADVxUAAA="
        }
    ]
}
```


### <a name="step-3-sample-third-request"></a>Schritt 3: Beispiel für dritte Anforderung

Die dritte Anforderung verwendet weiterhin das neueste aus der letzten Synchronisierungsanforderung zurückgegebene `nextLink`. 
 

<!-- {
  "blockType": "request",
  "name": "get_calendarview_delta_3"
}-->
```http
GET https://graph.microsoft.com/beta/me/calendarview/delta?$skiptoken=R0usmci39OQxqJrxK4 HTTP/1.1
Prefer: odata.maxpagesize=2
```

### <a name="sample-third-and-final-response"></a>Beispiel für dritte und letzte Antwort

Die dritte Antwort gibt das einzige in dem Ordner verbleibende Ereignis in der Kalenderansicht sowie eine `deltaLink`-URL zurück, was bedeutet, dass die Synchronisierung für diese Kalenderansicht momentan abgeschlossen ist. Speichern und verwenden Sie die `deltaLink`-URL, um [diese Kalenderansicht in der nächsten Runde zu synchronisieren](#the-next-round-sample-request).


<!-- {
  "blockType": "response",
  "truncated": true,
  "@odata.type": "microsoft.graph.event",
  "isCollection": true
} -->
```http
HTTP/1.1 200 OK
Content-type: application/json

{
    "@odata.context":"https://graph.microsoft.com/beta/$metadata#Collection(event)",
    "@odata.deltaLink":"https://graph.microsoft.com/beta/me/calendarview/delta?$deltatoken=R0usmcMDNGg0J1E",
    "value":[
        {
            "@odata.type":"#microsoft.graph.event",
            "@odata.etag":"W/\"EZ9r3czxY0m2jz8c45czkwAALZu97g==\"",
            "subject":"Rest!",
            "body":{
                "contentType":"html",
                "content":""
            },
            "start":{
                "dateTime":"2016-12-12T02:00:00.0000000",
                "timeZone":"UTC"
            },
            "end":{
                "dateTime":"2016-12-12T07:30:00.0000000",
                "timeZone":"UTC"
            },
            "location":{
                "displayName":"Home"
            },
            "attendees":[

            ],
            "organizer":{
                "emailAddress":{
                    "name":"Fanny Downs",
                    "address":"fannyd@contoso.onmicrosoft.com"
                }
            },
            "id":"AAMkADj1HuAAA="
        }
    ]
}
```


### <a name="the-next-round-sample-first-request"></a>Die nächste Runde: Beispiel für erste Anforderung

Mit dem `deltaLink` aus der [letzten Anforderung](#step-3-sample-third-request) in der letzten Runde können Sie nur die Ereignisse abrufen, die sich seitdem in dieser Kalenderansicht geändert haben (durch Hinzufügung, Löschung oder Aktualisierung). Ihre erste Anforderung in der nächsten Runde sieht folgendermaßen aus, sofern Sie die gleiche maximale Seitengröße in der Antwort beibehalten möchten:

<!-- {
  "blockType": "request",
  "name": "get_calendarview_delta_next"
}-->
```http
GET https://graph.microsoft.com/beta/me/calendarview/delta?$deltatoken=R0usmcMDNGg0J1E HTTP/1.1
Prefer: odata.maxpagesize=2
```

### <a name="the-next-round-sample-first-response"></a>Die nächste Runde: Beispiel für erste Antwort

<!-- {
  "blockType": "response",
  "truncated": true,
  "@odata.type": "microsoft.graph.event",
  "isCollection": true
} -->
```http
HTTP/1.1 200 OK
Content-type: application/json

{
    "@odata.context":"https://graph.microsoft.com/beta/$metadata#Collection(event)",
    "@odata.deltaLink":"https://graph.microsoft.com/beta/me/calendarview/delta?$deltatoken=R0usmcFuQtZdtpk4",
    "value":[
        {
            "@odata.type":"#microsoft.graph.event",
            "@odata.etag":"W/\"EZ9r3czxY0m2jz8c45czkwAALZu97w==\"",
            "subject":"Attend service",
            "body":{
                "contentType":"html",
                "content":""
            },
            "start":{
                "dateTime":"2016-12-25T06:00:00.0000000",
                "timeZone":"UTC"
            },
            "end":{
                "dateTime":"2016-12-25T07:30:00.0000000",
                "timeZone":"UTC"
            },
            "location":{
                "displayName":"Chapel of Saint Ignatius",
                "address":{
                    "type":"unknown",
                    "street":"900 Broadway",
                    "city":"Seattle",
                    "state":"WA",
                    "countryOrRegion":"United States",
                    "postalCode":""
                },
                "coordinates":{
                    "latitude":47.6105,
                    "longitude":-122.321
                }
            },
            "attendees":[

            ],
            "organizer":{
                "emailAddress":{
                    "name":"Fanny Downs",
                    "address":"fannyd@contoso.onmicrosoft.com"
                }
            },
            "id":"AAMkADj1HvAAA="
        }
    ]
}
```

## <a name="see-also"></a>Siehe auch

- [Microsoft Graph-Delta-Abfrage](../Concepts/delta_query_overview.md)
- [Inkrementelle Änderungen an Nachrichten abrufen (Vorschau)](../Concepts/delta_query_messages.md)
- [Inkrementelle Änderungen an Gruppen abrufen (Vorschau)](../Concepts/delta_query_groups.md)
- [Inkrementelle Änderungen an Benutzern abrufen (Vorschau)](../Concepts/delta_query_users.md)
