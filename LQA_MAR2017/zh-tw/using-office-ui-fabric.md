
#<a name="use-office-ui-fabric-261-in-office-add-ins"></a>在 Office 增益集中使用 Office UI Fabric 2.6.1

如果您要建置 Office 增益集，我們鼓勵您使用 [Office UI Fabric](https://github.com/OfficeDev/Office-UI-Fabric)來建立使用者經驗。下列步驟會引導您了解使用 Fabric 的基本知識。  

##<a name="1-set-up-fabric"></a>1.設定 Fabric
將下列命令行加入您的 HTML 標題區段，以從 CDN 參考 Fabric。

     <link rel="stylesheet" href="https://appsforoffice.microsoft.com/fabric/1.0/fabric.min.css">
     <link rel="stylesheet" href="https://appsforoffice.microsoft.com/fabric/1.0/fabric.components.min.css">


##<a name="2-use-fabric-icons-and-fonts"></a>2.使用 Fabric 圖示和字型
使用圖示很簡單。只需使用 "i" 項目並參考適當的類別即可。只要變更字型大小，即可控制圖示的大小。

    <i class="ms-Icon ms-Icon--group" style="font-size:xx-large" aria-hidden="true"></i>


##<a name="3-use-styles-for-simple-components"></a>3.使用簡單元件的樣式
Fabric 隨附不同 UI 項目的樣式，例如按鈕和核取方塊。只需參考適當的類別即可加入對應的樣式，如下列範例所示。

    <button class="ms-Button" id="get-data-from-selection">
    <span class="ms-Button-icon"><i class="ms-Icon ms-Icon--plus"></i></span>
    <span class="ms-Button-label">Get Data from selection</span>
    <span class="ms-Button-description">Get Data from the document selection</span>
    </button>

##<a name="4-use-components-with-sample-behavior"></a>4.使用範例行為的元件
Fabric 包含的某些元件支援行為 (例如，按一下會發生的情況)。為了引導您開始使用，**Fabric 2.6.1** 包含一些您可以使用的 JQuery UI 外掛程式形式的**範例程式碼**。您也可以使用任何其他您想用的架構。如果您選擇使用範例，請注意其程式碼不會隨 CDN 而散發，所以您必須從 **2.6.1 版** [Fabric GitHub 專案](https://github.com/OfficeDev/office-ui-fabric-core/tree/release/2.6.1)下載、加以參考，然後在您的程式碼中予以初始化。 

例如，若要使用 SearchBox 元件：

1. 請從 [GitHub](https://github.com/OfficeDev/office-ui-fabric-core/tree/release/2.6.1/src/components/SearchBox) 下載 SearchBox 元件。
2. 將下列參考加入您的程式碼中：`<script src="SearchBox/Jquery.SearchBox.js"></script>`
3. 確認載入您的網頁時會執行這一行，以初始化元件：`$(".ms-SearchBox").SearchBox();`。建議您將這一行加入增益集的 `Office.Initialize` 區塊中。     

**附註：**如果您不想使用 Fabric 的所有元件，則可改為選擇裝載每個元件的個別 CSS 檔案，以減少下載資源的大小。您可從 [Fabric 2.6.1 GitHub 存放庫](https://github.com/OfficeDev/office-ui-fabric-core/tree/release/2.6.1)的元件資料夾取得 CSS 檔案。 


##<a name="next-steps"></a>後續步驟
如果您想尋找示範如何使用 Fabric 的端對端範例，我們也提供相關資訊。請參閱 [Office 增益集 Fabric UI 範例](https://github.com/OfficeDev/Office-Add-in-Fabric-UI-Sample)。您也可以瀏覽互動式 [Office UI Fabric](https://github.com/OfficeDev/Office-UI-Fabric)網站。

