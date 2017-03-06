# <a name="make-batch-requests-with-the-rest-apis"></a>Отправка пакетных запросов с помощью интерфейсов REST API
Узнайте, как использовать параметр запроса `$batch` с интерфейсами REST API и OData API.
 
В этой статье описана отправка пакетных запросов и операций в случае REST API или OData API для Microsoft SharePoint Online (а также локальной среды SharePoint 2016 и более поздних версий), а также в случае [подмножества файлов и папок](http://msdn.microsoft.com/en-us/office/office365/api/files-rest-operations) REST API для Office 365. С помощью этой методики вы можете повысить производительность надстройки, совместив множество операций в одном запросе к серверу и одном отклике.

## <a name="executive-summary-of-the-batch-option"></a>Аннотация к параметру $batch
В SharePoint Online (и локальной среде SharePoint 2016 и более поздних версий), а также API Office 365 реализован параметр запроса OData `$batch`, поэтому вы можете следовать указаниям по его использованию из [официальной документации](http://www.odata.org/documentation/odata-version-3-0/batch-processing). Кроме того, вы можете ознакомиться с этой темой в блоге Эндрю Коннела (Andrew Connell). Начните с записи [Часть 1. Пакетные запросы REST API в SharePoint](http://www.andrewconnell.com/blog/part-1-sharepoint-rest-api-batching-understanding-batching-requests). Напомним о некоторых важных факторах:
 
- URL-адрес запроса состоит из корневого URL-адреса службы и параметра `$batch`. Пример: `https://fabrikam.sharepoint.com/_api/$batch` или `https://fabrikam.office365.com/api/v1.0/me/$batch`.
    
- Тело HTTP-запроса относится к типу MIME *multipart/mixed*.
    
- Текст запроса делится на части, отделенные друг от друга граничной строкой, которая указывается в заголовке запроса.
    
- У каждой части этого тела запроса есть команда HTTP и URL-адрес REST, а также внутреннее тело, если это применимо.
    
- Такая часть может быть операцией чтения (либо вызовом функции) или объектом ChangeSet из одной или нескольких операций записи (либо вызовов функций). Объект ChangeSet относится к типу MIME *multipart/mixed* с дочерними частями, содержащими операции вставки, обновления или удаления.
    
     **Важно!** В настоящее время интерфейсы API для SharePoint и Office 365 не поддерживают функции "все или ничего" объектов ChangeSet, содержащих несколько операций. В случае сбоя какой-либо из дочерних операций остальные операции выполняются без отката.

## <a name="code-samples"></a>Примеры кода
Примеры кода, в котором используется параметр запроса `$batch` для интерфейсов REST API или OData API в SharePoint: 

-  **C#:** [OfficeDev/Core.ODataBatch](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.ODataBatch)
-  **JavaScript:** [andrewconnell/sp-o365-rest](https://github.com/andrewconnell/sp-o365-rest/blob/master/SpRestBatchSample/Scripts/App.js)
    

## <a name="example-requests-and-responses"></a>Примеры запросов и откликов
Ниже приведен пример необработанного HTTP-запроса, который пакетно обрабатывает две операции GET, которые извлекают названия всех элементов в двух разных списках.

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

Ниже приведен пример текста необработанного HTTP-запроса, который пакетно обрабатывает операцию DELETE для списка и операцию GET для списка списков SharePoint.
 
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


## <a name="links-to-helpful-libraries"></a>Ссылки на полезные библиотеки
Существуют библиотеки OData, поддерживающие пакетные запросы OData для множества языков. Ниже представлены два примера. Более полный список см. на странице [Библиотеки OData](http://www.odata.org/libraries/).

-  [Библиотека OData .NET](http://msdn.microsoft.com/en-us/office/microsoft.data.odata%28v=vs.90%29). Обратите особое внимание на классы **ODataBatch***.
-  [Библиотека Data.js](http://datajs.codeplex.com/documentation). Обратите особое внимание на [пакетные операции](http://datajs.codeplex.com/wikipage?title=datajs%20OData%20API&amp;referringTitle=Documentation#Batch).