# <a name="get-incremental-changes-to-events-in-a-calendar-view-preview"></a>カレンダー ビューのイベントへの増分の変更を取得する (プレビュー)

カレンダー ビューは、既定のカレンダー (../me/calendarview) またはユーザーの他のカレンダーからの日付/時刻範囲内にあるイベントのコレクションです。デルタ クエリを使用すると、カレンダー ビューで新規、更新済み、または削除済みのイベントを取得できます。返されるイベントには、定期的なアイテムの発生と例外、および単一のインスタンスが含まれている場合があります。デルタ データを使用すると、毎回サーバーからユーザーのイベントのセット全体をフェッチせずに、ユーザーのイベントのローカル ストアの保守と同期が行えます。

デルタ クエリでは、指定したカレンダー ビュー内のすべてのイベントを取得する完全な同期と、最後の同期以降にカレンダー ビューで変更されたそれらのイベントを取得する増分同期の両方がサポートされています。通常は、最初の完全同期を実行して、その後、そのカレンダー ビューへの増分の変更を定期的に取得します。 

## <a name="track-event-changes-in-a-calendar-view"></a>カレンダー ビューのイベントの変更を追跡する

イベントのデルタ クエリは、指定するカレンダーと日付/時刻範囲 (つまり、カレンダー ビュー) に固有のものです。複数のカレンダー ビューの変更を追跡するには、各カレンダーを個別に追跡する必要があります。 

通常、カレンダー ビュー内のイベント変更の追跡は、[デルタ](../api-reference/beta/api/event_delta.md)関数を使用した、1 つ以上の GET 要求のラウンドです。最初の GET 要求は、**デルタ**関数を含めることを除いて、[calendarView の一覧表示](https://graph.microsoft.io/en-us/docs/api-reference/beta/api/calendar_list_calendarview)方法とほぼ同じです。

```
GET /me/calendarView/delta?startDateTime={start_datetime}&endDateTime={end_datetime}
```

**デルタ**関数を使用した GET 要求は、次のいずれかを返します。

- `nextLink` (**デルタ**関数の呼び出しと _skipToken_ を使用した URL を含む)、または 
- `deltaLink` (**デルタ**関数の呼び出しと _deltaToken_ を使用した URL を含む)。

これらのトークンは、_startDateTime_ パラメーターと _endDateTime_ パラメーター、および最初のデルタ クエリの GET 要求内の他のすべてのクエリ パラメーターをエンコードする [state tokens](./delta_query_overview#state-tokens) です。 

状態トークンは、クライアントに対して完全に不透明です。変更追跡のラウンドを続行する手順は、最後の GET 要求から返された `nextLink` または `deltaLink` の URL を、その同じカレンダー ビューの次の**デルタ**関数呼び出しにコピーして適用するだけです。応答で返される `deltaLink` は、変更追跡の現在のラウンドが完了したことを示します。`deltaLink` URL を保存して、次のラウンドを開始する際に使用することができます。

これらの `nextLink` と `deltaLink` の URL を使用する方法については、以下の[例](#example-to-synchronize-events-in-a-calendar-view)を参照してください。

### <a name="use-query-parameters-in-a-delta-query-for-calendar-view"></a>カレンダー ビューのデルタ クエリでクエリ パラメーターを使用する

- _startDateTime_ パラメーターと _endDateTime_ パラメーターを含めて、カレンダー ビューの日付/時刻範囲を定義します。
- `$select` はサポートされていません。


### <a name="optional-request-header"></a>オプションの要求ヘッダー

各デルタ クエリの GET 要求は、応答で 1 つ以上のイベントのコレクションを返します。必要に応じて、要求ヘッダー _Prefer: odata.maxpagesize={x}_ を指定して、応答内の最大イベント数を設定できます。


## <a name="example-to-synchronize-events-in-a-calendar-view"></a>カレンダー ビューでのイベント同期の例

次の例では、特定の時間範囲にあるユーザーの既定のカレンダーを同期するための一連の 3 つの要求を示します。そのカレンダー ビューには 5 つのイベントがあります。

- [手順 1: サンプルの最初の要求](#step-1-sample-initial-request)と[応答](#sample-initial-response)
- [手順 2: サンプルの 2 番目の要求](#step-2-sample-second-request)と[応答](#sample-second-response)
- [手順 3: サンプルの 3 番目の要求](#step-3-sample-third-request)と[最後の応答](#sample-third-and-final-response)

簡潔にするため、サンプルの応答にはイベントのプロパティのサブセットのみを表示します。実際の呼び出しでは、ほとんどのイベント プロパティが返されます。 

[次のラウンド](#the-next-round-sample-first-response)で行う操作も参照してください。


### <a name="step-1-sample-initial-request"></a>手順 1: 最初の要求のサンプル

この例では、指定されたカレンダー ビューは初めて同期されるため、最初の同期要求には状態トークンは含まれません。このラウンドは、そのカレンダー ビュー内のすべてのイベントを返します。

最初の要求では、次を指定します。

- _startDateTime_ パラメーターと _endDateTime_ パラメーターの日付/時刻の値。
- [オプションの要求ヘッダー](#optional-request-header)である _odata.maxpagesize_ が一度に 2 つのイベントを返します。

<!-- {
  "blockType": "request",
  "name": "get_calendarview_delta_1"
}-->
```http
GET https://graph.microsoft.com/beta/me/calendarview/delta?startdatetime=2016-12-01T00:00:00Z&enddatetime=2016-12-30T00:00:00Z HTTP/1.1
Prefer: odata.maxpagesize=2
```


### <a name="sample-initial-response"></a>最初の応答のサンプル

応答には、2 つのイベントと `skipToken` のある `@odata.nextLink` 応答ヘッダーが含まれています。`nextLink` URL は、取得するカレンダー ビューにさらにイベントがあることを示します。

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

### <a name="step-2-sample-second-request"></a>手順 2: 2 番目の要求のサンプル

2 番目の要求では、前の応答で返された `nextLink` URL を指定します。最初の要求にあるような同じ _startDateTime_ パラメーターと _endDateTime_ パラメーターを指定する必要はなくなりましたのでご注意ください。これは、`nextLink` URL の `skipToken` によってこれらのパラメーターがエンコードされて含まれるためです。

<!-- {
  "blockType": "request",
  "name": "get_calendarview_delta_2"
}-->
```http
GET https://graph.microsoft.com/beta/me/calendarview/delta?$skiptoken=R0usmcCM996atia_s HTTP/1.1
Prefer: odata.maxpagesize=2
```

### <a name="sample-second-response"></a>2 番目の応答のサンプル 

2 番目の応答は、カレンダー ビューから取得するイベントがまだあることを示す、カレンダー ビュー内の次の 2 つのイベントと別の `nextLink` を返します。

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


### <a name="step-3-sample-third-request"></a>手順 3: 3 番目の要求のサンプル

3 番目の要求は、最後の同期要求から返された最新の `nextLink` を引き続き使用します。 
 

<!-- {
  "blockType": "request",
  "name": "get_calendarview_delta_3"
}-->
```http
GET https://graph.microsoft.com/beta/me/calendarview/delta?$skiptoken=R0usmci39OQxqJrxK4 HTTP/1.1
Prefer: odata.maxpagesize=2
```

### <a name="sample-third-and-final-response"></a>3 番目と最後の応答のサンプル

3 番目の応答では、カレンダー ビューに残っている 1 つだけのイベントと、このカレンダー ビューの同期が完了したことを示す `deltaLink` URL が返されます。`deltaLink` URL を保存して、[次のラウンドでそのカレンダー ビューを同期するために使用します](#the-next-round-sample-request)。


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


### <a name="the-next-round-sample-first-request"></a>次のラウンド: 最初の要求のサンプル

最後のラウンドの[最後の応答](#step-3-sample-third-request)からの `deltaLink` を使用すると、それ以降にそのカレンダー ビューで (追加、削除、または更新によって) 変更されたこれらのイベントのみを取得できます。次のラウンドの最初の要求は、同じ最大ページ サイズを維持することを前提とすると、次のようになります。

<!-- {
  "blockType": "request",
  "name": "get_calendarview_delta_next"
}-->
```http
GET https://graph.microsoft.com/beta/me/calendarview/delta?$deltatoken=R0usmcMDNGg0J1E HTTP/1.1
Prefer: odata.maxpagesize=2
```

### <a name="the-next-round-sample-first-response"></a>次のラウンド: 最初の応答のサンプル

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

## <a name="see-also"></a>関連項目

- [Microsoft Graph デルタ クエリ](../Concepts/delta_query_overview.md)
- [メッセージへの増分の変更を取得する (プレビュー)](../Concepts/delta_query_messages.md)
- [グループへの増分の変更を取得する (プレビュー)](../Concepts/delta_query_groups.md)
- [ユーザーへの増分の変更を取得する (プレビュー)](../Concepts/delta_query_users.md)
