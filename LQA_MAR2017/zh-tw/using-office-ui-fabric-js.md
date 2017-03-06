-
#<a name="use-office-ui-fabric-in-office-add-ins"></a>在 Office 增益集中使用 Office UI Fabric

如果您要建置 Office 增益集，我們鼓勵您使用 [Office UI Fabric](https://dev.office.com/fabric)來建立使用者經驗。 

Office UI Fabric 是用於建置 Office 與 Office 365 之使用者體驗的 JavaScript 前端架構。Fabric 提供以視覺效果為主的元件，您可用於擴充、重新作業，以及用於您的 Office 增益集。由於 Fabric 使用了 Office 設計語言，所以 Fabric 的 UX 元件看起來與 Office 一般擴充項目無異。

Fabric 由數個專案所組成︰

- **Fabric JS (建議選項)** - 僅使用 JavaScript 來實作 UX 元件。若您不希望從 React 架構中取得相依性，我們建議您使用此版本的 Fabric。  
- **Fabric React** - 使用 React 架構來實作 UX 元件。
- **Fabric Core** - 包含設計語言 (例如：圖示、色彩、類型及格線)的核心元素。Fabric JS 及 Fabric React 皆使用 Fabric Core。 

下列步驟會引導您了解使用 Fabric JS 的基本知識。  

##<a name="1-add-the-fabric-cdn-references"></a>1.新增 Fabric CDN 參考
若要從 CDN 參考 Fabric，新增下列 HTML 程式碼到您的頁面。

    <link rel="stylesheet" href="https://static2.sharepointonline.com/files/fabric/office-ui-fabric-js/1.2.0/css/fabric.min.css">
    <link rel="stylesheet" href="https://static2.sharepointonline.com/files/fabric/office-ui-fabric-js/1.2.0/css/fabric.components.min.css">
    <script src="https://static2.sharepointonline.com/files/fabric/office-ui-fabric-js/1.2.0/js/fabric.min.js"></script>

僅以上步驟。現在您已準備好可在增益集中使用 Fabric。 

##<a name="2-use-fabric-icons-and-fonts"></a>2.使用 Fabric 圖示和字型
使用圖示很簡單。只需使用 "i" 項目並參考適當的類別即可。只要變更字型大小，即可控制圖示的大小。例如，下列程式碼示範如何建立使用 themePrimary (#0078d7) 色彩的特大表格圖示。 
   
    <i class="ms-Icon ms-font-xl ms-Icon--Table ms-fontColor-themePrimary"></i>

若要尋找 Office UI Fabric 中更多可用的圖示，請使用[圖示](https://dev.office.com/fabric#/styles/icons)頁面上的 [搜尋] 功能。當您找到要用於增益集內的圖示時，請務必在圖示名稱前面加上 `ms-Icon--`。 

如需 Office UI Fabric 內可用字型大小及色彩的相關資訊，請參閱[印刷樣式](https://dev.office.com/fabric#/styles/typography)和[色彩](https://dev.office.com/fabric#/styles/colors)。

##<a name="3-use-fabric-js-ux-components"></a>3.使用 Fabric JS UX 元件

Fabric 提供了多種您可用於增益集中的 UX 元件，例如：按鈕或核取方塊。以下是建議用於增益集中的 Fabric JS UX 元件清單。若要在增益集中使用任一 Fabric 元件，請前往連結以取得 Fabric 文件，接著依照**使用此元件**的指示進行。

> **附註：**我們將陸續新增其他元件。 

- [階層連結](https://github.com/OfficeDev/office-ui-fabric-js/blob/master/ghdocs/components/Breadcrumb.md)
- [按鈕](https://github.com/OfficeDev/office-ui-fabric-js/blob/master/ghdocs/components/Button.md)
- [核取方塊](https://github.com/OfficeDev/office-ui-fabric-js/blob/master/ghdocs/components/CheckBox.md)
- [ChoiceFieldGroup](https://github.com/OfficeDev/office-ui-fabric-js/blob/master/ghdocs/components/ChoiceFieldGroup.md)
- [日期選擇器](https://github.com/OfficeDev/office-ui-fabric-js/blob/master/ghdocs/components/DatePicker.md) (如需示範如何在增益集中實作日期選擇器的範例，請參閱 [Excel 銷售追蹤器](https://github.com/OfficeDev/Excel-Add-in-JavaScript-SalesTracker) 程式碼範例)。
- [下拉式清單](https://github.com/OfficeDev/office-ui-fabric-js/blob/master/ghdocs/components/Dropdown.md)
- [標籤](https://github.com/OfficeDev/office-ui-fabric-js/blob/master/ghdocs/components/Label.md)
- [連結](https://github.com/OfficeDev/office-ui-fabric-js/blob/master/ghdocs/components/Link.md)
- [清單](https://github.com/OfficeDev/office-ui-fabric-js/blob/master/ghdocs/components/List.md) (考慮在 CSS 中變更元件的預設樣式)。
- [MessageBanner](https://github.com/OfficeDev/office-ui-fabric-js/blob/master/ghdocs/components/MessageBanner.md)
- [MessageBar](https://github.com/OfficeDev/office-ui-fabric-js/blob/master/ghdocs/components/MessageBar.md)
- [重疊](https://github.com/OfficeDev/office-ui-fabric-js/blob/master/ghdocs/components/Overlay.md)
- [面板](https://github.com/OfficeDev/office-ui-fabric-js/blob/master/ghdocs/components/Panel.md)
- [樞紐](https://github.com/OfficeDev/office-ui-fabric-js/blob/master/ghdocs/components/Pivot.md)
- [ProgressIndicator](https://github.com/OfficeDev/office-ui-fabric-js/blob/master/ghdocs/components/ProgressIndicator.md)
- [Searchbox](https://github.com/OfficeDev/office-ui-fabric-js/blob/master/ghdocs/components/SearchBox.md)
- [載入狀態圓環](https://github.com/OfficeDev/office-ui-fabric-js/blob/master/ghdocs/components/Spinner.md)
- [表格](https://github.com/OfficeDev/office-ui-fabric-js/blob/master/ghdocs/components/Table.md)
- [TextField](https://github.com/OfficeDev/office-ui-fabric-js/blob/master/ghdocs/components/TextField.md)
- [切換](https://github.com/OfficeDev/office-ui-fabric-js/blob/master/ghdocs/components/Toggle.md)
   
## <a name="updating-your-add-in-to-use-fabric-js"></a>更新您的增益集以使用 Fabric JS
若您已在使用舊版 Office UI Fabric 且想改為使用 Fabric JS，請確保會深入了解、整合及測試增益集內的新元件。請記住以下幾點，對於規劃更新時會有所助益：

- 使用 Fabric JS 進行元件初始化會更為簡單。針對舊版的 Fabric，您可以將 Fabric 元件的 JavaScript 檔案加入增益集專案 (其中已包含該檔案的 `<Script>` 參考) 中，接著再初始化元件。在Fabric JS 中，已不再需要加入 Fabric 元件的 JavaScript 檔案和相關的 `<Script>` 參考。您只需要初始化 Fabric 元件。   
- 目前有多種元件已提供可控制 UX 元件行為的函式。例如，核取方塊控制具有 `toggle` 函式，其可在核取及未核取狀態之間切換。 
- 而部分圖示類別名稱及樣式已經更新。
- 最值得注意的變更，便是在多項元件中使用 `<label>` 元素。`<label>` 元素可控制元件的樣式。您可能需要更新您的 UX 程式碼，以便使用 `<label>` 元素。例如，在 Fabric JS 核取方塊上變更 `<input>` 元素已核取屬性的值，不會在核取方塊上產生作用。反之，您可使用 `check`、`unCheck` 或 `toggle` 函式。   

##<a name="next-steps"></a>後續步驟
如果您想尋找示範如何使用 Fabric JS 的端對端程式碼範例，我們也提供了相關資訊。請參閱以下資源：

- [Excel 銷售追蹤器](https://github.com/OfficeDev/Excel-Add-in-JavaScript-SalesTracker) 

##<a name="related-resources"></a>相關資源
如果您正在尋找舊版 Fabric 的程式碼範例或說明文件，請參閱下列項目︰

- [UX 設計模式 (使用 Fabric 2.6.1)](https://github.com/OfficeDev/Office-Add-in-UX-Design-Patterns-Code) 
- [Office 增益集 Fabric UI 範例 (使用 Fabric 1.0)](https://github.com/OfficeDev/Office-Add-in-Fabric-UI-Sample) 
- [在 Office 增益集中使用 Fabric 2.6.1](https://dev.office.com/docs/add-ins/design/ui-elements/using-office-ui-fabric)
 

