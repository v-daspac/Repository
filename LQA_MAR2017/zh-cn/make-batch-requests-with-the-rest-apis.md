# <a name="make-batch-requests-with-the-rest-apis"></a>使用 REST API 进行批处理请求
了解如何将 `$batch` 查询选项与 REST/OData API 配合使用。
 
本文介绍如何对 Microsoft Office SharePoint Online（和本地 SharePoint 2016 及更高版本）的 REST/OData API 和 Office 365 REST API 的[文件和文件夹子集](http://msdn.microsoft.com/en-us/office/office365/api/files-rest-operations)执行批处理查询和操作。借助此技术，可通过将多个操作合并到对服务器的单个请求和单个响应，提高外接程序的性能。

## <a name="executive-summary-of-the-batch-option"></a>$batch 选项的执行摘要
Microsoft SharePoint Online（和本地 SharePoint 2016 及更高版本）与 Office 365 API 可实现 OData `$batch` 查询选项，可参阅[官方文档](http://www.odata.org/documentation/odata-version-3-0/batch-processing)，了解有关其使用方式的详细信息。（也可查看 Andrew Connell 关于该主题的博客文章，开始部分是 [Part 1 - SharePoint REST API Batching](http://www.andrewconnell.com/blog/part-1-sharepoint-rest-api-batching-understanding-batching-requests)（第 1 部分 - SharePoint REST API 批处理）。）以下只是有关要点提示：
 
- 请求 URL 包含根服务 URL 和 `$batch` 选项；例如，`https://fabrikam.sharepoint.com/_api/$batch` 或 `https://fabrikam.office365.com/api/v1.0/me/$batch`。
    
- HTTP 请求正文属于 MIME 类型：*多部分/混合*。
    
- 请求的正文分成几个部分，各个部分之间用请求标头中指定的边界字符串分开。
    
- 正文的每部分都有自己的 HTTP 谓词和 REST URL，以及自己的内部正文（若适用）。
    
- 部分可以是读取操作（或函数调用）或者一个或多个写入操作的变更集（或函数调用）。变更集本身属于 MIME 类型（*多部分/混合*），具有包含插入、更新或删除操作的子部分。
    
     **重要提示**：目前，SharePoint 和 Office 365 API 不支持包含多个操作的变更集的“全有或全无”功能。如果任何子操作失败，其他操作仍将完成且不会回退。

## <a name="code-samples"></a>代码示例
针对 SharePoint REST/OData API 使用 `$batch` 查询选项的代码示例： 

-  **C#:**[OfficeDev/Core.ODataBatch](https://github.com/OfficeDev/PnP/tree/master/Samples/Core.ODataBatch)
-  **JavaScript：** [andrewconnell/sp-o365-rest](https://github.com/andrewconnell/sp-o365-rest/blob/master/SpRestBatchSample/Scripts/App.js)
    

## <a name="example-requests-and-responses"></a>示例请求和响应
下面是一个原始 HTTP 请求的示例，该请求对两个 GET 操作进行批处理，以检索两个不同列表中所有项目的标题。

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

下面是一个原始 HTTP 请求的正文示例，该请求对列表的 DELETE 和 SharePoint 列表的列表的 GET 进行批处理。
 
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


## <a name="links-to-helpful-libraries"></a>有用的库的链接
存在支持多种语言的 OData 批处理的 OData 库。以下是两个示例。有关更加完整的列表，请参阅 [OData 库](http://www.odata.org/libraries/)。

-  [.NET OData 库](http://msdn.microsoft.com/en-us/office/microsoft.data.odata%28v=vs.90%29)。请着重参阅 **ODataBatch*** 类。
-  [Datajs 库](http://datajs.codeplex.com/documentation)请着重参阅[批处理操作](http://datajs.codeplex.com/wikipage?title=datajs%20OData%20API&amp;referringTitle=Documentation#Batch)。