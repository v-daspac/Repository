# <a name="add-ins-for-outlook-mobile"></a>Outlook Mobile 的增益集 

> **附註：**增益集可用於 iOS 版 Outlook。即將推出 Android 版 Outlook 支援。

增益集現在可在 Outlook Mobile 上運作，並使用其他 Outlook 端點可用的相同 API。如果您已建立適用於 Outlook 的增益集，很容易就能讓它在 Outlook Mobile 上運作。

所有 Office 365 Commercial 帳戶都支援 Outlook Mobile 增益集，而此支援也推至 Outlook.com 帳戶。

**iOS 版 Outlook 中的範例工作窗格**

![iOS 版 Outlook 中的工作窗格螢幕擷取畫面](../../images/outlook-mobile-addin-taskpane.png)

## <a name="whats-different-on-mobile"></a>在行動裝置上有何差異？ 

- 尺寸小和快速互動，讓設計行動裝置成為一大挑戰。為了確保我們的客戶經驗品質，我們設定了嚴格的驗證準則，而宣告行動支援的增益集必須符合該準則，才能獲准在 Office 市集中上架。
    - 增益集**必須**遵守 [UI 指導方針](./outlook-addin-design.md)。
    - 增益集的案例**必須**[在行動裝置具有意義](#what-makes-a-good-scenario-for-mobile-add-ins)。
- 此時僅支援郵件讀取。這表示 `MobileMessageReadCommandSurface` 是您應在資訊清單的 mobile 區段中宣告的唯一 [ExtensionPoint](../../reference/manifest/extensionpoint.md)。
- 行動裝置不支援 [MakeEwsRequestAsync](../../reference/outlook/Office.context.mailbox.md) API，因為行動應用程式會使用 REST API 來與伺服器通訊。如果您的應用程式後端需要連線至 Exchange Server，您可以使用回呼權杖來進行 REST API 呼叫。如需詳細資訊，請參閱[使用 Outlook 增益集中的 Outlook REST API](./use-rest-api.md)。
- 當您將增益集提交至資訊清單中具有 [MobileFormFactor](../../reference/manifest/mobileformfactor.md) 的市集時，您必須同意我們針對 iOS 上的增益集所提供的開發人員增補合約，而且您必須提交您的 Apple 開發人員 ID 驗證以供。
- 最後，您的資訊清單必須宣告 `MobileFormFactor`，並包含正確的[控制項](../../reference/manifest/control.md)類型和[圖示大小](../../reference/manifest/icon.md)。

## <a name="what-makes-a-good-scenario-for-mobile-add-ins"></a>行動裝置增益集的良好案例為何？

請記住，電話上的平均 Outlook 工作階段長度比 PC 上的平均 Outlook 工作階段長度短許多。這表示您的增益集必須很快速，而案例必須可讓使用者進出集使用其電子郵件工作流程。

以下是 Outlook Mobile 中有意義的案例範例。

- 此增益集可在 Outlook 中顯示重要的資訊，協助使用者將其電子郵件分類並適當地回應。範例︰可讓使用者查看客戶資訊及共用適當資訊的 CRM 增益集。
- 此增益集可將資訊儲存到追蹤、共同作業或類似的系統，讓使用者的電子郵件內容更有價值。範例︰此增益集可讓使用者將電子郵件變成工作項目以便進行專案追蹤，或協助處理技術支援小組的票證。

絕對還有其他很棒的案例，所以如果您認為有超越這些案例的增益集，請經由[這裡](https://aka.ms/outlookmobileaddin)與我們聯繫，以取得這是否為可接受之 Outlook Mobile 案例的意見反應。我們很高興提供相關指引，並期待您能提供更詳細的資訊。我們喜歡完美的 UI 逐步解說！

**從電子郵件郵件建立 Trello 卡的使用者互動範例**

![顯示使用者與 Outlook Mobile 增益集互動的動畫 GIF](../../images/outlook-mobile-addin-example.gif)

## <a name="testing-your-add-ins-on-mobile"></a>在行動裝置上測試您的增益集

若要在 Outlook Mobile 上測試增益集，您可以將增益集載入至 O365 或 Outlook.com 帳戶。在 Outlook Web App 中，移至設定齒輪圖，然後選擇 [管理整合] 或 [管理增益集]。按一下最上層附近的「按一下這裡以新增自訂增益集」，並上載您的資訊清單。請確定您的資訊清單已適當格式化成包含 `MobileFormFactor`，否則無法載入。

您的增益集一旦運作，請務必在不同尺寸的螢幕 (包括電話和平板) 上進行測試。您應確定它符合對比、字型大小和色彩的可及性指導方針，而且很適合搭配螢幕助讀程式使用，例如 iOS 上的 VoiceOver 或 Android 上的 TalkBack。

在行動裝置上進行疑難排解可能有所困難，因為可能沒有您慣用的工具。疑難排解的其中一個選項是[使用 Vorlon.js](../testing/debug-office-add-ins-on-ipad-and-mac.md)。或者，如果您曾經用過 Fiddler，請參閱[使用 Fiddler 搭配 iOS 裝置的教學課程](http://www.telerik.com/blogs/using-fiddler-with-apple-ios-devices)。

## <a name="next-steps"></a>後續步驟

- 了解如何[在增益集的資訊清單中新增行動支援](./manifests/add-mobile-support.md)。
- 了解如何[為您的增益集設計絕佳的行動經驗](./outlook-addin-design.md)。
- 了解如何從增益集[取得存取權杖並呼叫 Outlook REST API](./use-rest-api.md)。