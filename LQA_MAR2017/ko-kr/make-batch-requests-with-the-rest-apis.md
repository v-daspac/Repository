# <a name="make-batch-requests-with-the-rest-apis"></a>REST API를 사용하여 일괄 처리 요청 수행
REST/OData API에서 `$batch` 쿼리 옵션을 사용하는 방법을 알아봅니다.
 
이 문서에서는 Microsoft SharePoint Online(및 온-프레미스 SharePoint 2016 이상)의 REST/OData API에 대해 쿼리 및 작업을 일괄 처리하는 방법과 Office 365 REST API의 [파일 및 폴더 하위 집합](http://msdn.microsoft.com/en-us/office/office365/api/files-rest-operations)에 대해 설명합니다. 이 기술을 사용하여 많은 작업을 서버에 대한 단일 요청과 단일 응답으로 조합하여 추가 기능의 성능을 개선할 수 있습니다.

## <a name="executive-summary-of-the-batch-option"></a>$batch 옵션의 주요 내용 요약
SharePoint Online(및 온-프레미스 SharePoint 2016 이상) 및 Office 365 API는 OData `$batch` 쿼리 옵션을 구현하므로 [공식 문서](http://www.odata.org/documentation/odata-version-3-0/batch-processing)에서 사용 방법을 확인할 수 있습니다. (또는 Andrew Connell의 블로그 게시물 중에서 [1부 - SharePoint REST API 일괄 처리](http://www.andrewconnell.com/blog/part-1-sharepoint-rest-api-batching-understanding-batching-requests)로 시작하는 항목을 참조할 수도 있습니다.) 다음은 주요 내용을 한 번 더 강조해서 설명한 것입니다.
 
- 요청 URL은 루트 서비스 URL과 `$batch` 옵션(예: `https://fabrikam.sharepoint.com/_api/$batch` 또는 `https://fabrikam.office365.com/api/v1.0/me/$batch`)으로 구성됩니다.
    
- HTTP 요청 본문은 MIME 형식 *multipart/mixed*입니다.
    
- 요청의 본문의 각 부분은 요청 헤더에 지정된 경계 문자열을 통해 구분됩니다.
    
- 본문의 각 부분에는 자체 HTTP 동사 및 REST URL과 자체 내부 본문(해당되는 경우)이 있습니다.
    
- 한 부분은 읽기 작업(또는 함수 호출)이거나, 하나 이상의 쓰기 작업(또는 함수 호출)에 대한 ChangeSet일 수 있습니다. ChangeSet은 삽입, 업데이트 또는 삭제 작업을 포함하는 하위 부분이 있는 MIME 형식 *multipart/mixed* 자체입니다.
    
     **중요** 현재, SharePoint와 Office 365 API는 둘 이상의 작업을 포함하는 ChangeSet에 대해 "양자택일" 기능을 지원하지 않습니다. 자식 작업 중 하나가 실패해도 다른 작업은 여전히 완료되며 롤백되지 않습니다.

## <a name="code-samples"></a>코드 샘플
SharePoint REST/OData API에 대해 `$batch` 쿼리 옵션을 사용하는 코드 샘플: 

-  **C#:** [OfficeDev/Core.ODataBatch](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.ODataBatch)
-  **JavaScript:** [andrewconnell/sp-o365-rest](https://github.com/andrewconnell/sp-o365-rest/blob/master/SpRestBatchSample/Scripts/App.js)
    

## <a name="example-requests-and-responses"></a>예제 요청 및 응답
다음은 두 개의 다른 목록에 있는 모든 항목의 제목을 검색하는 두 가지 GET 작업을 일괄 처리하는 원시 HTTP 요청의 예입니다.

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

다음은 목록의 DELETE와 SharePoint 목록의 GET을 일괄로 처리하는 원시 HTTP 요청의 본문 예입니다.
 
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


## <a name="links-to-helpful-libraries"></a>유용한 라이브러리에 대한 링크
많은 언어에 대해 OData 일괄 처리를 지원하는 OData 라이브러리가 있습니다. 다음은 두 가지 예입니다. 전체 목록을 보려면 [OData 라이브러리](http://www.odata.org/libraries/)를 참조하세요.

-  [.NET OData 라이브러리](http://msdn.microsoft.com/en-us/office/microsoft.data.odata%28v=vs.90%29). 특히 **ODataBatch*** 클래스를 참조하세요.
-  [Datajs 라이브러리](http://datajs.codeplex.com/documentation). 특히 [작업 일괄 처리](http://datajs.codeplex.com/wikipage?title=datajs%20OData%20API&amp;referringTitle=Documentation#Batch)을 참조하세요.