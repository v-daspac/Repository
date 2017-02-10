# <a name="changelog-for-microsoft-graph"></a>Microsoft Graph の変更ログ

この変更ログでは、Microsoft Graph と、v1.0 およびベータ版のエンドポイント Microsoft Graph API の変更内容について説明します。  

## <a name="february-2017"></a>2017 年 2 月

### <a name="intune-apis"></a>Intune API

|**変更の種類**|**バージョン**|**説明**|
|:-------------|:-----------|:--------------|
|追加|ベータ版|新しいエンティティを追加しました。 <br/>[appReportingOverviewStatus](https://graph.microsoft.io/en-us/docs/api-reference/beta/resources/intune_apps_appreportingoverviewstatus)<br/>[deviceComplianceDeviceOverview](https://graph.microsoft.io/en-us/docs//api-reference/beta/resources/intune_deviceconfig_devicecompliancedeviceoverview)<br/>[deviceConfigurationDeviceOverview](https://graph.microsoft.io/en-us/docs/api-reference/beta/resources/intune_deviceconfig_deviceconfigurationdeviceoverview)<br/>[deviceManagementExchangeOnpremisesPolicy](https://graph.microsoft.io/en-us/docs/api-reference/beta/resources/intune_onboarding_devicemanagementexchangeonpremisespolicy)<br/>[iosDeviceFeaturesConfiguration](https://graph.microsoft.io/en-us/docs/api-reference/beta/resources/intune_deviceconfig_iosdevicefeaturesconfiguration)<br/>[iosEducationDeviceConfiguration](https://graph.microsoft.io/en-us/docs/api-reference/beta/resources/intune_deviceconfig_ioseducationdeviceconfiguration)<br/>[iosLobAppProvisioningConfiguration](https://graph.microsoft.io/en-us/docs/api-reference/beta/resources/intune_apps_ioslobappprovisioningconfiguration)<br/>[onpremisesConditionalAccessSettings](https://graph.microsoft.io/en-us/docs/api-reference/beta/resources/intune_onboarding_onpremisesconditionalaccesssettings)<br/>[sharedPCConfiguration](https://graph.microsoft.io/en-us/docs/api-reference/beta/resources/intune_deviceconfig_sharedpcconfiguration)<br/>[windows10EnterpriseModernAppManagementConfiguration](https://graph.microsoft.io/en-us/docs/api-reference/beta/resources/intune_deviceconfig_windows10enterprisemodernappmanagementconfiguration)<br/>[windows10SecureAssessmentConfiguration](https://graph.microsoft.io/en-us/docs/api-reference/beta/resources/intune_deviceconfig_windows10secureassessmentconfiguration)<br/>[windows10WindowsInformationProtectionConfiguration](https://graph.microsoft.io/en-us/docs/api-reference/beta/resources/intune_deviceconfig_windows10windowsinformationprotectionconfiguration)|
|削除|ベータ版|次の複合型を削除し、microsoft.graph.json に置き換えました。<br/>managedAppDeploymentSummary <br/>managedAppSummary |
|変更|ベータ版|次のエンティティで、プロパティの種類 appConfigComplianceStatus を complianceStatus に置き換えました。 <br/>[managedDeviceMobileAppConfigurationDeviceStatus](https://graph.microsoft.io/en-us/docs/api-reference/beta/resources/intune_apps_manageddevicemobileappconfigurationdevicestatus)<br/>[managedDeviceMobileAppConfigurationUserStatus](https://graph.microsoft.io/en-us/docs/api-reference/beta/resources/intune_apps_manageddevicemobileappconfigurationuserstatus)<br/><br/>リソース [managedAppStatusRaw](https://graph.microsoft.io/en-us/docs/api-reference/beta/resources/intune_mam_managedappstatusraw) について、プロパティ コンテンツの種類を managedAppSummary から json に変更しました。|

## <a name="january-2017"></a>2017 年 1 月

### <a name="outlook-calendar"></a>Outlook カレンダー

|**変更の種類**|**バージョン**|**説明**|
|:-------------|:-----------|:--------------|
|追加|v1.0|[user](https://graph.microsoft.io/en-us/docs/api-reference/v1.0/resources/user) リソースの新しいアクション [findMeetingTimes](https://graph.microsoft.io/en-us/docs/api-reference/v1.0/api/user_findmeetingtimes)。|
|追加|v1.0|新しい複合型 [attendeeBase](https://graph.microsoft.io/en-us/docs/api-reference/v1.0/resources/attendeebase)。attendee 型の型プロパティで構成されます。|
|追加|v1.0|新しい複合型:<br/>[attendeeAvailability](https://graph.microsoft.io/en-us/docs/api-reference/v1.0/resources/attendeeavailability)<br/>[locationConstraint](https://graph.microsoft.io/en-us/docs/api-reference/v1.0/resources/locationconstraint) <br/>[locationConstraintItem](https://graph.microsoft.io/en-us/docs/api-reference/v1.0/resources/locationconstraintitem)<br/>[meetingTimeSuggestion](https://graph.microsoft.io/en-us/docs/api-reference/v1.0/resources/meetingtimesuggestion)<br/>[meetingTimeSuggestionsResult](https://graph.microsoft.io/en-us/docs/api-reference/v1.0/resources/meetingtimesuggestionsresult)<br/>[timeConstraint](https://graph.microsoft.io/en-us/docs/api-reference/v1.0/resources/timeconstraint)<br/>[timeSlot](https://graph.microsoft.io/en-us/docs/api-reference/v1.0/resources/timeslot)|
|変更|v1.0|[attendee](https://graph.microsoft.io/en-us/docs/api-reference/v1.0/resources/attendee) 複合型は、[recipient](https://graph.microsoft.io/en-us/docs/api-reference/v1.0/resources/recipient) から派生する attendeeBase から派生するようになりました。継承されたプロパティを含めて、以前と同じ **status**、**type**、**emailAddress** プロパティで構成されます。|
|追加|ベータ版|hexColor が、[calendar](https://graph.microsoft.io/en-us/docs/api-reference/beta/resources/calendar) リソースに追加されました。|

## <a name="december-2016"></a>2016 年 12 月

### <a name="delta-query"></a>デルタ クエリ

|**変更の種類**|**バージョン**|**説明**|
|:-------------|:-----------|:--------------|
|追加|ベータ版|[デルタ クエリ](https://graph.microsoft.io/en-us/docs/concepts/delta_query_overview)を実行するため、以下のエンティティに新しいデルタ関数が追加されました。<br/>contact<br/>contactFolder<br/>event<br/>group<br/>mailFolder<br/>message<br/>user<br/>例については、以下をご覧ください。<br/>[グループへの増分の変更を取得する (プレビュー)](https://graph.microsoft.io/en-us/docs/concepts/delta_query_groups)<br/>[フォルダー内のメッセージへの増分の変更を取得する (プレビュー)](https://graph.microsoft.io/en-us/docs/concepts/delta_query_message)<br/>[ユーザーへの増分の変更を取得する (プレビュー)](https://graph.microsoft.io/en-us/docs/concepts/delta_query_users)|

### <a name="excel-apis"></a>Excel API

|**変更の種類**|**バージョン**|**説明**|
|:-------------|:-----------|:--------------|
|追加|v1.0|workbookPivotTable リソース、pivotTables の refresh および refreshAll アクション、workbookRangeView リソース、フィルターされた範囲に対して実行して workbookRangeView をユーザーに返す visibleView アクション、visibleView からの行コレクションと範囲リソースの取得、範囲リソースからの columnsAfter、columnsBefore、resizedRange、rowsAbove、rowsBelow 関数、および新しいテーブル プロパティが追加されました。|

### <a name="intune-apis"></a>Intune API

|**変更の種類**|**バージョン**|**説明**|
|:-------------|:-----------|:--------------|
|追加|ベータ版|Microsoft Intune に、リソースとメソッド API が追加されました。これには、Azure ポータルでの Intune のパブリック プレビューをサポートする多数のリソースとメソッドのセットが含まれます。Intune サービスの詳細については、[Intune のドキュメント](https://go.microsoft.com/fwlink/?linkid=836405) をご覧ください。Intune のリソースと API の詳細については、「[Microsoft Graph での Intune の使用](https://graph.microsoft.io/en-us/docs/api-reference/beta/resources/intune_graph_overview)」をご覧ください。|

## <a name="october-2016"></a>2016 年 10 月

### <a name="authorization-provider"></a>認証プロバイダー

|**変更の種類**|**バージョン**|**説明**|
|:-------------|:-----------|:--------------|
|追加|v1.0 およびベータ版|v2.0 認証エンドポイントでは、[ビジネス シナリオでのデーモン プロセスおよび長時間実行プロセス](https://azure.microsoft.com/en-us/documentation/articles/active-directory-v2-protocols-oauth-client-creds/)で使える client_credentials OAuth 許可がサポートされるようになりました。|
|追加|v1.0 およびベータ版|v2.0 認証エンドポイントでは、[管理者の同意エンドポイント](https://azure.microsoft.com/en-us/documentation/articles/active-directory-v2-scopes/#admin-restricted-scopes)経由で、[管理者の同意を必要とするアクセス許可のスコープ](http://graph.microsoft.io/en-us/docs/authorization/permission_scopes#permission-scope-details)がサポートされるようになりました。|
|追加|v1.0 およびベータ版|v2.0 認証エンドポイントでは、[管理者の同意エンドポイント](https://azure.microsoft.com/en-us/documentation/articles/active-directory-v2-scopes/#admin-restricted-scopes)経由で、テナント内のすべてのユーザーに対する管理者の同意がサポートされるようになりました。|

### <a name="invitation-apis"></a>招待 API

|**変更の種類**|**バージョン**|**説明**|
|:-------------|:-----------|:--------------|
|追加|ベータ版|招待エンティティの型に、招待するユーザー (“ゲスト” または “メンバー”) の種類を定義する invitedUserType プロパティが追加されました。|
|削除|ベータ版|2016 年 11 月 11 日付けで、招待のエンティティ型から invitedToGroups プロパティが削除されます。このため、この API を使用して、招待したユーザーをグループに追加することができなくなります。代わりに、[メンバー追加 API](https://graph.microsoft.io/en-us/docs/api-reference/v1.0/api/group_post_members) を使ってユーザーをグループに追加することになります。|

## <a name="september-2016"></a>2016 年 9 月

### <a name="azure-ad-application-proxy"></a>Azure AD アプリケーション プロキシ

|**変更の種類**|**バージョン**|**説明**|
|:--------------|:-----------|:--------------|
|追加|ベータ版|Azure AD アプリケーション プロキシ API が、Microsoft Graph ベータ エンドポイントで利用可能になりました。これらの API では、アクセスのための共通のコントロール プレーンとして Azure AD を使用し、企業ネットワーク外のユーザーにオンプレミス アプリケーションをセキュアに発行できます。発行された API を使用すると、アプリケーションの _connectors_、_connectorGroups_、_onPremisesPublishing_ の設定など、アプリケーション プロキシのさまざまな側面を取得、更新するアプリケーションを作成できます。|

### <a name="drive"></a>ドライブ

|**変更の種類**|**バージョン**|**説明**|
|:--------------|:-----------|:--------------|
|追加|ベータ版|shareId または共有 URL により共有されている driveItems にアクセスできる _shared_ コレクションが追加されました。|
|追加|ベータ版|ドライブに _search_ 関数が追加され、ドライブのルート フォルダー内の項目だけを検索するよりも多くの項目を検索できるようになりました。|
 

### <a name="driveitem"></a>DriveItem

|**変更の種類**|**バージョン**|**説明**|
|:--------------|:-----------|:--------------|
|追加|ベータ版|_createUploadSession_ のサポートが追加され、OneDrive、OneDrive for Business、SharePoint のドキュメント ライブラリに 4 MB を超えるファイルをアップロードできるようになりました。|
|追加|ベータ版|SharePoint に保存されている driveItems の従来の SharePoint API 識別子を返す、driveItem に _sharepointIds_ プロパティが追加されました。|
|追加|ベータ版|_remoteItem_ に他のプロパティが追加されました。|
|追加|ベータ版|OneDrive for Business のファイルに対して _quickXorHash_ 値が追加されました。|
|追加|ベータ版|_createSharingLink_ にスコープが追加され、会社の共有可能なリンクまたは匿名の共有リンクが作成できるようになりました。|

### <a name="extended-properties"></a>拡張プロパティ

|**変更の種類**|**バージョン**|**説明**|
|:--------------|:-----------|:--------------|
|追加|v1.0|[拡張プロパティ](http://graph.microsoft.io/en-us/docs/api-reference/v1.0/resources/extended-properties-overview)が、次のリソースでサポートされるようになりました: message, mailFolder, event, calendar, contact, contactFolder, group event, group calendar, group post。|

### <a name="groups"></a>グループ

パブリック プレビューの API により、動的グループ メンバーシップのサポートが追加されました。追加された内容の一部を、次の表に記載します。

|**変更の種類**|**バージョン**|**説明**|
|:--------------|:-----------|:--------------|
|追加|ベータ版|グループが動的グループの場合に、このグループのメンバーシップを制御する規則を含む、**membershipRule** プロパティが追加されました。|
|追加|ベータ版|このグループに対する動的メンバーシップの処理が実行中または一時停止中であるかどうかを制御するための **membershipRuleProcessingState** プロパティが追加されました。|
|追加|ベータ版|**"DynamicMembership"** を含むように **groupTypes** プロパティを設定して、このグループの動的グループ機能を強化できます。|
|追加|ベータ版|Office 365 グループの優先言語を示すための **preferredLanguage** プロパティが追加されました。|
|追加|ベータ版|Office 365 グループの色のテーマを指定するための **theme** プロパティが追加されました。|

### <a name="hybrid-deployment-support"></a>ハイブリッド展開のサポート

|**変更の種類**|**バージョン**|**説明**|
|:--------------|:-----------|:--------------|
|追加|v1.0|アプリで v1.0 Outlook メール、カレンダー、連絡先の API を使用して、Exchange 2016 累積的な更新プログラム 3 (CU3) を使用したハイブリッド展開のオンプレミスのメールボックスにアクセスできます。REST API サポートの詳細については、特定の[ハイブリッド展開](https://graph.microsoft.io/en-us/docs/overview/hybrid_rest_support)をご覧ください。**注:**v1.0 のこれらの API セットを使用している場合、特定のハイブリッド展開の要件を満たすオンプレミスのメールボックスで機能する、運用アプリを含むアプリを検出できるようになりました。この機能はプレビューでのみ使用できます。|

### <a name="identityriskevents"></a>IdentityRiskEvents

|**変更の種類**|**バージョン**|**説明**|
|:--------------|:-----------|:--------------|
|変更|ベータ版|2 つの場所のプロパティの型が identityRiskEvents エンドポイントの新しい複合型で置き換えられるスキーマ変更の一環として、次のプロパティが identityRiskEvents エンドポイントで変更/追加されました。</br>**location** – Edm.String から ComplexType signInLocation に変更されました。<br/>**previousLocation** – Edm.String から ComplexType signInLocation に変更されました。<br/>**signInLocation** – city、state、countryOrRegion、geoCoordinates プロパティを含む新しい ComplexType です。<br/>**geoCoordinates** – latitude と longitude プロパティを含む新しい ComplexType です。|

### <a name="invitation-manager"></a>招待マネージャー

|**変更の種類**|**バージョン**|**説明**|
|:--------------|:-----------|:--------------|
|追加|ベータ版|招待マネージャー API が、Microsoft Graph ベータ エンドポイントで利用可能になりました。招待マネージャー API を使用して、組織に外部ユーザーを追加するための招待状を作成します。招待の一環として、招待されたユーザーを Office 365 グループに追加することも選択できます。詳細については、[招待マネージャー](https://graph.microsoft.io/en-us/docs/api-reference/beta/resources/invitation)をご覧ください。|

### <a name="onedrive"></a>OneDrive

|**変更の種類**|**バージョン**|**説明**|
|:--------------|:-----------|:--------------|
|追加|v1.0|**driveItem** の **CreateUploadSession** メソッドが追加され、サイズの大きなファイルの再開可能なアップロードができるようになります。|
|追加|v1.0|SharePoint から項目の SharePoint ID を追跡する複数のプロパティ (**sharepointIds**) と、ルート フォルダーを識別する 1 つのプロパティ (**root**) が追加されました。|
|追加|v1.0|**Shares** ルート コレクションが追加されました。shareIds または共有リンクとともに使用して、OneDrive と SharePoint の共有項目にアクセスできます。新しい型 sharedDriveItem を返します。|
|追加|v1.0|driveItem の **Invite** メソッドが追加されました。項目へのアクセス許可を追加できます。 |
|追加|v1.0|ドライブの **Search** メソッドが追加されました。ドライブ内の項目と共有された項目を対象とした検索が可能です。 |
|追加|v1.0|ファイル複合型の **processingMetadata** プロパティ、ハッシュ複合型の quickXorHash プロパティが追加されました。 |
|追加|v1.0|ハッシュ複合型の **quickXorHash** プロパティが追加されました。 |

### <a name="outlook-calendar"></a>Outlook カレンダー

|**変更の種類**|**バージョン**|**説明**|
|:--------------|:-----------|:--------------|
|追加|v1.0|**onlineMeetingUrl** プロパティが、[event](http://graph.microsoft.io/en-us/docs/api-reference/v1.0/resources/event) リソースに追加されました。|
|追加|ベータ版|event リソースに [forward](http://graph.microsoft.io/en-us/docs/api-reference/beta/api/event_forward) アクションが追加されました。|
|追加|ベータ版|カレンダーの共有をサポートする次のプロパティが、[calendar](http://graph.microsoft.io/en-us/docs/api-reference/beta/resources/calendar) リソースに追加されました: **canEdit**、**canShare**、**canViewPrivateItems**、**isShared**、**isShareWithMe**、**owner**。|

### <a name="outlook-mail"></a>Outlook メール

|**変更の種類**|**バージョン**|**説明**|
|:--------------|:-----------|:--------------|
|追加|v1.0|[mailboxSettings](http://graph.microsoft.io/en-us/docs/api-reference/v1.0/resources/mailboxsettings) 複合型が追加されました。これには **automaticRepliesSetting**、**timeZone**、**language** プロパティが含まれています。|
|追加|v1.0|**mailboxSettings** プロパティが [user](http://graph.microsoft.io/en-us/docs/api-reference/v1.0/resources/user) リソースに追加されました。|
|追加|ベータ版|メッセージに含まれる[参照投稿](http://graph.microsoft.io/en-us/docs/api-reference/beta/resources/mention)の 1 つ以上のインスタンスを作成、一覧表示、取得、削除する機能のサポートが追加されました。参照投稿は、他のユーザーの注意を引きつけるためのメッセージ内のコールアウトをサポートしています。|
|追加|ベータ版|[getMailTips](http://graph.microsoft.io/en-us/docs/api-reference/beta/api/user_getmailtips) アクションのサポートが追加され、特定の受信者のすべてのメール ヒントを取得できるようになりました。次のリソースが追加されました: automaticRepliesMailTips、mailTips、mailTipsError。|

### <a name="query-parameters"></a>クエリ パラメーター

|**変更の種類**|**バージョン**|**説明**|
|:--------------|:-----------|:--------------|
|変更|ベータ版|2016 年 9 月 26 日現在、'$' プレフィックスなしのクエリ パラメーターがサポートされています。クエリ パラメーターの '$' プレフィックスは、オプションです。詳細については、ブログ投稿「[Microsoft Graph における $ プレフィックスのないクエリ パラメーターのサポート](http://dev.office.com/queryparametersinMicrosoftGraph)」をご覧ください。|

### <a name="sharepoint"></a>SharePoint

|**変更の種類**|**バージョン**|**説明**|
|:--------------|:-----------|:--------------|
|追加|ベータ版|SharePoint サイトへのアクセスと、[ID ごとの一覧表示](http://graph.microsoft.io/en-us/docs/api-reference/beta/api/list_get) または [パス/URL](http://graph.microsoft.io/en-us/docs/api-reference/beta/api/baseitem_getbyurl) ごとの一覧表示が可能になりました。|
|追加|ベータ版|[listItem のインスタンスを一覧表示、作成、取得、削除](http://graph.microsoft.io/en-us/docs/api-reference/beta/resources/listitem)できるようになりました。|

### <a name="users"></a>ユーザー

|**変更の種類**|**バージョン**|**説明**|
|:--------------|:-----------|:--------------|
|追加|ベータ版|更新トークンまたはセッション トークンの有効期間の開始時期を示す、読み取り専用の **refreshTokensValidFromDateTime** プロパティが追加されました。この日時より前に発行されたすべてのトークンは無効になります。また、これらのトークンを使用しようとすると、ユーザーは新たにサインインを強制されます。|
|追加|ベータ版|Outlook のグローバル アドレス一覧にこのユーザーを含める必要があるかどうかを制御するための **showInAddressList** プロパティが追加されました。|
|追加|ベータ版|**invalidateAllRefreshTokens** サービス アクションが追加されました。これを使って **refreshTokensValidFromDateTime** ユーザー プロパティを現在の日時にリセットすることで、アプリケーションに発行されたすべての更新トークンとセッション トークンが無効になります。|


### <a name="webhooks"></a>Webhooks

|**変更の種類**|**バージョン**|**説明**|
|:--------------|:-----------|:--------------|
|追加|ベータ版|購読可能なリソースとしてドライブのルート項目が Webhooks に追加されました。|

## <a name="august-2016"></a>2016 年 8 月

### <a name="contacts"></a>連絡先

|**変更の種類**|**バージョン**|**説明**|
|:--------------|:-----------|:--------------|
|追加|ベータ版|いくつかのプロパティが削除され、対応するコレクションが連絡先エンドポイントに追加されたスキーマ変更の一環として、次のプロパティが連絡先エンドポイントに追加されました。_Websites – Collection(ComplexType:Website)_、_Phones – Collection (ComplexType:Phone)_、_PostalAddress – Collection(ComplexType:PhysicalAddress)_。詳細については、ブログ投稿「[連絡先および People API で今後予定されている変更](https://blogs.msdn.microsoft.com/exchangedev/2016/06/09/upcoming-changes-to-contacts-and-people-apis/)」をご覧ください。|
|削除|ベータ版|いくつかのプロパティが削除され、対応するコレクションが連絡先エンドポイントに追加されたスキーマ変更の一環として、次のプロパティが連絡先エンドポイントから削除されました。_BusinessHomePage_、_HomePhones_、_MobilePhone1_、_BusinessPhones_、_HomeAddress_、_BusinessAddress_、_OtherAddress_。詳細については、ブログ投稿「[連絡先および People API で今後予定されている変更](https://blogs.msdn.microsoft.com/exchangedev/2016/06/09/upcoming-changes-to-contacts-and-people-apis/)」をご覧ください。|

### <a name="excel-apis"></a>Excel API

|**変更の種類**|**バージョン**|**説明**|
|:--------------|:-----------|:--------------|
|追加|v1.0|Microsoft Graph の Excel REST API は一般公開されています。Office 365 の Excel ブックとの充実した高度な統合を構築できるようになりました。詳細については、ブログ投稿「[Microsoft Graph の新しい Excel REST API を使ってアプリをパワーアップする](http://dev.office.com/blogs/power-your-apps-with-the-new-excel-rest-api)」をご覧ください。|

### <a name="people"></a>複数のユーザー

|**変更の種類**|**バージョン**|**説明**|
|:--------------|:-----------|:--------------|
|変更|ベータ版|_WebSite_ プロパティの名前が _Websites_ に変更されました。詳細については、「[連絡先および People API で今後予定されている変更](https://blogs.msdn.microsoft.com/exchangedev/2016/06/09/upcoming-changes-to-contacts-and-people-apis/)」をご覧ください。|

### <a name="privileged-identity-management"></a>Privileged Identity Management

|**変更の種類**|**バージョン**|**説明**|
|:--------------|:-----------|:--------------|
|追加|ベータ版|Privileged Identity Management (PIM) REST API が Microsoft Graph ベータ エンドポイントで利用可能になりました。[Privileged Identity Management](https://azure.microsoft.com/en-us/documentation/articles/active-directory-privileged-identity-management-configure/) では、グローバル管理者、課金管理者など、Azure AD の組織内でのロールの「ジャスト イン タイム」アクティベーションが提供されます。発行された API を使用すると、特権ロールの割り当てを取得、更新して、ユーザーをロールにアクティブ化するアプリケーションを作成できます。詳細については、「[Microsoft Graph:ベータ版で利用可能な Azure AD Privileged Identity Management Preview API](http://dev.office.com/blogs/microsoft-graph-azure-ad-privileged-identity-management-apis-beta)」と「[Azure AD Privileged Identity Management](https://graph.microsoft.io/en-us/docs/api-reference/beta/resources/privilegedidentitymanagement_root)」をご覧ください。|

## <a name="july-2016"></a>2016 年 7 月
  
### <a name="administrative-units"></a>管理単位

|**変更の種類**|**バージョン**|**説明**|
|:--------------|:-----------|:--------------|
|追加|ベータ版|新しい管理単位のプレビュー API が導入されました。管理単位を使用すると、組織は Azure Active Directory を分割して、サブ部門に管理職務を委任できます。サブ部門は、地域、部署、コスト センターなどを表すことができます。これを Microsoft Graph API から管理できるようになりました。|

## <a name="june-2016"></a>2016 年 6 月

### <a name="identityriskevents"></a>IdentityRiskEvents

|**変更の種類**|**バージョン**|**説明**|
|:--------------|:-----------|:--------------|
|追加|ベータ版|新しい IdentityRiskEvents プレビュー API が導入されました。この API は、Azure Active Directory Identity Protection と連携して動作します。この API を使うと、Identity Protection によって生成されたリスク イベントに対してクエリを実行できます。詳細については、ブログ投稿「[Microsoft Graph の新しいプレビュー API の紹介:IdentityRiskEvents](http://dev.office.com/blogs/identityriskevents-api-preview)」をご覧ください。

### <a name="subscriptions"></a>サブスクリプション

|**変更の種類**|**バージョン**|**説明**|
|:--------------|:-----------|:--------------|
|追加|ベータ版|_メール_と_連絡先_のサブスクリプションに対して、アプリ専用スコープがサポートされるようになりました。|

## <a name="may-2016"></a>2016 年 5 月

### <a name="calendar"></a>カレンダー

|**変更の種類**|**バージョン**|**説明**|
|:--------------|:-----------|:--------------|
|重大な変更|ベータ版|findMeetingTimes API に対する変更です。詳細については、ブログ投稿「[Microsoft Graph findMeetingTimes API の更新](http://dev.office.com/microsoft-graph-findmeetingtimes-api-update)」をご覧ください。この変更は、2016 年 5 月 19 日に有効になりました。

### <a name="contact"></a>連絡先

|**変更の種類**|**バージョン**|**説明**|
|:--------------|:-----------|:--------------|
|追加|v1.0|_extensions_ が追加されました。これは OData v4 のオープン型 openTypeExtension をサポートする抽象型です。|

### <a name="directory"></a>ディレクトリ

|**変更の種類**|**バージョン**|**説明**|
|:--------------|:-----------|:--------------|
|重大な変更|ベータ版|_settingTemplateId_ は _templateId_ に名前が変更されます。この変更は、2016 年 5 月 19 日に有効になります。|

### <a name="event"></a>イベント

|**変更の種類**|**バージョン**|**説明**|
|:--------------|:-----------|:--------------|
|追加|v1.0|_extensions_ が追加されました。これは OData v4 のオープン型 openTypeExtension をサポートする抽象型です。|

### <a name="eventmessages"></a>EventMessages

|**変更の種類**|**バージョン**|**説明**|
|:--------------|:-----------|:--------------|
|追加|v1.0|_inferenceClassification_ と _extensions_ が _eventMessages_ に追加されました。|
|追加|ベータ版|_responseRequested_ が _eventMessageRequest_ に追加されました。|

### <a name="messages"></a>メッセージ

|**変更の種類**|**バージョン**|**説明**|
|:--------------|:-----------|:--------------|
|追加|v1.0|_inferenceClassification_ と _extensions_ が _messages_ に追加されました。|
|追加|ベータ版|_wellknownname_ が _contactFolder_ に追加されました。|_findMeetingTimes_ API に対する変更です。詳細については、ブログ投稿「[Microsoft Graph findMeetingTimes API の更新](http://dev.office.com/microsoft-graph-findmeetingtimes-api-update)」をご覧ください。この変更は、2016 年 5 月 19 日に有効になります。|

### <a name="post"></a>投稿

|**変更の種類**|**バージョン**|**説明**|
|:--------------|:-----------|:--------------|
|追加|v1.0|_extensions_ が追加されました。これは OData v4 のオープン型 openTypeExtension をサポートする抽象型です。|

### <a name="user"></a>ユーザー

|**変更の種類**|**バージョン**|**説明**|
|:--------------|:-----------|:--------------|
|追加|v1.0|_inferenceClassification_ リソースの種類が追加されました。|
|追加|ベータ版|_timeZone_ が _mailboxsettings_ に追加されました。|
|追加|ベータ版|API _findMeetingTimesOLD _ が _user_ に追加されました。|

## <a name="april-2016"></a>2016 年 4 月

### <a name="general"></a>全般

|**変更の種類**|**バージョン**|**説明**|
|:--------------|:-----------|:--------------|
|追加|v1.0 およびベータ版|_Accept-Encoding:gzip_ の使用のサポートが追加されました。|
|追加|v1.0|拡張パスのキャスト セグメントのサポートが追加されました。例: 'https://graph.microsoft.com/v1.0/me/messages?$expand=microsoft.graph.eventMessage/event'。|
|追加|ベータ版|構造プロパティに対する PATCH 要求のサポートが追加されました。例:'PATCH /me/mailboxSettings'。|
|追加|ベータ版|たとえば、ユーザーがメールボックスのライセンスを持っていない場合、またはテナントに Exchange Online のサブスクリプションがない場合など、Outlook が要求を処理できないときに、Azure Active Directory が /beta/users/id/photo 要求のフォールバックとして使用されるようになりました。注: このフォールバックは GET と PATCH の両方に使用できます。|
|追加|ベータ版|拡張パスのキャスト セグメントのサポートが追加されました。例: 'https://graph.microsoft.com/v1.0/me/messages?$expand=microsoft.graph.eventMessage/event'。|

### <a name="onedrive"></a>OneDrive

|**変更の種類**|**バージョン**|**説明**|
|:--------------|:-----------|:--------------|
|修正|v1.0|500 エラーと「拡張プロパティの型がサポートされていません」というエラーで OneDrive の createLink 要求が失敗する問題が修正されました。|

## <a name="march-2016"></a>2016 年 3 月

### <a name="calendar"></a>カレンダー

|**変更の種類**|**バージョン**|**説明**|
|:--------------|:-----------|:--------------|
|追加|ベータ版|_singleValueExtendedProperties_ プロパティと _multiValueExtendedProperties_ プロパティが追加されました。|
|追加|ベータ版|_suggestionHint_ プロパティが _meetingTimeCandidate_ に追加されました。|
|追加|ベータ版|_locationUri_ プロパティが _location_ に追加されました。|
|追加|ベータ版|_type_ と _postOfficeBox_ が _physicalAddress_ に追加されました。|
|変更|ベータ版|_findMeetingTimes_ で新しいパラメーター _ReturnSuggestionHints_ を使用するようになりました。|
|変更|ベータ版|_findMeetingTimes_ が _meetingTimeCandidate_ のコレクションを返すようになりました。|

### <a name="drive"></a>ドライブ

|**変更の種類**|**バージョン**|**説明**|
|:--------------|:-----------|:--------------|
|追加|v1.0 およびベータ版|サインインしたユーザーによって最近使用された項目のセットを一覧表示する _recent_ 関数が追加されました。この一覧には、ユーザーのドライブにある項目と、他のドライブとの間でアクセス可能な項目が含まれています。例: GET /me/drive/recent。|
|追加|v1.0 およびベータ版|現在のユーザーと共有されている項目のセットを一覧表示する _sharedWithMe_ 関数が追加されました。例: GET /me/drive/sharedWithMe。|

### <a name="driveitem"></a>DriveItem

|**変更の種類**|**バージョン**|**説明**|
|:--------------|:-----------|:--------------|
|追加|v1.0 およびベータ版|別のドライブ内の項目へのリンクを提供する _remoteItem_ 型が追加されました。|
|追加|v1.0 およびベータ版|このアクセス許可に関連付けられた共有の招待に関する詳細情報を提供する _sharingInvitation_ 型が追加されました。|
|追加|v1.0 およびベータ版|ドライブ内の項目に対する変更を追跡する _delta_ 関数が追加されました。例: GET /me/drive/items/{item-id}/delta|
|追加|v1.0 およびベータ版|新しい親の下に、または新しい名前を指定して、_driveItem_ (すべての子を含む) のコピーを作成する _copy_ が追加されました。例: POST /me/drive/items/{item-id}/copy。|
|追加|v1.0 およびベータ版|_conflictBehavior_ インスタンス属性が _driveItem_ に適用されるようになりました。|
|追加|ベータ版|既存の項目に共有の招待を送信する _invite_ 関数が追加されました。共有の招待では、一意の共有リンクが作成され、共有リンクを記載した電子メールが招待状の受信者に送信されます。例: POST /drive/items/{item-id}/invite。

### <a name="event"></a>イベント

|**変更の種類**|**バージョン**|**説明**|
|:--------------|:-----------|:--------------|
|追加|ベータ版|新しいプロパティ _onlineMeetingUrl_ と、新しいメソッド _cancel_ が追加されました。|

### <a name="event-messages"></a>イベント メッセージ

|**変更の種類**|**バージョン**|**説明**|
|:--------------|:-----------|:--------------|
|追加|ベータ版|_startDateTime_、_endDateTime_、_location_、_type_、_recurrence_、_isOutOfDate_、_conversationIndex_、_unsubscribe_、_unsubscribeData_、_unsubscribeEnabled_、_flag_ プロパティが、_eventmessage_ オブジェクトに追加されました。|
|追加|ベータ版|_singleValueExtendedProperties_ プロパティと _multiValueExtendedProperties_ プロパティが追加されました。|
|追加|ベータ版|新しいメソッド _unsubscribe_ が追加されました。|

### <a name="excel"></a>Excel

|**変更の種類**|**バージョン**|**説明**|
|:--------------|:-----------|:--------------|
|追加|ベータ版|現在、Excel ブックのデータの読み取りと変更が可能な新しい Excel REST API を追加しているところです。データへのインサイトを提供することで、ユーザーが Excel ブックに保存されているコンテンツから価値を取得できるスマート アプリを構築できるようになりました。Excel の分析機能の活用、表とグラフの作成、および視覚に訴えるグラフ イメージの抽出などを、アプリ内からすべて実行できます。詳細については、「[Microsoft Graph での Excel の操作](http://graph.microsoft.io/en-us/docs/api-reference/beta/resources/excel)」をご覧ください。|

### <a name="general"></a>全般

|**変更の種類**|**バージョン**|**説明**|
|:--------------|:-----------|:--------------|
|追加|v1.0 およびベータ版|テナント エイリアスと拒否された JWT (AAD) トークンを解決するときのエラー メッセージを改善しました。|
|追加|v1.0 およびベータ版|空のベアラー トークンで要求を受信した場合に、承認サービス エンドポイントの場所が _www-authenticate_ ヘッダー内に返されるようになりました。|
追加|v1.0 およびベータ版|エンティティの ID プロパティでのフィルター機能が修正されました。例: GET https://graph.microsoft.com/v1.0/users?$filter=id+eq+'x'<br/>以前は、サービスのアクションと関数に対する POST 要求で、アクション名または関数名に microsoft.graph のプレフィックスを付ける必要がありました。例:POST https://graph.microsoft.com/v1.0/me/Microsoft.Graph.getMemberGroups。<br/>プレフィックスは不要になりました (ただし、引き続き指定できます)。そのため、次のような指定でも機能するようになりました。POST https://graph.microsoft.com/v1.0/me/getMemberGroups。|
|変更|ベータ版|サブスクリプションのプロパティ名がクリーンアップされました。|
|追加|ベータ版|エンティティとその関連機能の既定の動作を (_directorySettingTemplates_ 経由で) 検出し、(テンプレートから _setting_ を作成することにより) 上書きする機能が追加されました。最初に提供されたこの唯一のテンプレートは、Office グループ上での動作を制御するためのものです。|

### <a name="mail-folder"></a>メール フォルダー

|**変更の種類**|**バージョン**|**説明**|
|:--------------|:-----------|:--------------|
|追加|ベータ版|_wellKnownName_ プロパティと _userConfigurations_ プロパティが追加されました。|
|追加|ベータ版|_singleValueExtendedProperties_ プロパティと _multiValueExtendedProperties_ プロパティが追加されました|

### <a name="messages"></a>メッセージ

|**変更の種類**|**バージョン**|**説明**|
|:--------------|:-----------|:--------------|
|追加|v1.0|_mobilePhone_ プロパティが追加されました。|
|追加|v1.0 およびベータ版|_internetMessageId_ プロパティが追加されました。メッセージ ID は、[RFC2822](http://www.ietf.org/rfc/rfc2822.txt) によって指定された形式です。|
|変更|ベータ版|_mobilePhone1_ プロパティは _mobilePhone_ に名前が変更されました。|
|変更|ベータ版|_createReply_ と _createReplyAll_ は、新しいパラメーター _Message_ および _comment_ を使用します。|
|変更|ベータ版|_createForward_ は、新しいパラメーター _Message_、_ToRecipients_、_comment_ を使用します。|
|変更|ベータ版|_reply_、_replyAll_、_forward_ は、新しいパラメーター _Message_ を使用します。|

### <a name="permission"></a>アクセス許可

|**変更の種類**|**バージョン**|**説明**|
|:--------------|:-----------|:--------------|
|追加|v1.0 およびベータ版|このアクセス許可に関連付けられた共有の招待の詳細情報を提供する _sharingInvitation_ プロパティが追加されました。|

### <a name="person"></a>人物

|**変更の種類**|**バージョン**|**説明**|
|:--------------|:-----------|:--------------|
|追加|ベータ版|新しいプロパティ _birthday_、_personNotes_、_isFavorite_、_phones_、_permission_、_postalAddresses_、_websites_、_yomiCompany_、_department_、_profession_、_mailboxType_、_personType_ が追加されました。|
|追加|ベータ版|新しい列挙型 _physicalAddressType_、_webSite_、_phone_、_webSiteType_ が追加されました。|

### <a name="reference-attachment"></a>参照添付ファイル

|**変更の種類**|**バージョン**|**説明**|
|:--------------|:-----------|:--------------|
|追加|ベータ版|新しいプロパティ _sourceUrl_、_providerType_、_thumbnailUrl_、_previewUrl_、_permission_、_isFolder_ が追加されました。|
|追加|ベータ版|_singleValueExtendedProperties_ プロパティと _multiValueExtendedProperties_ プロパティが追加されました。|
|追加|ベータ版|新しい列挙型 _referenceAttachmentProvider_ と _referenceAttachmentPermission_ が追加されました。|

### <a name="subscriptions"></a>サブスクリプション

|**変更の種類**|**エンドポイント**|**説明**|
|:--------------|:-----------|:--------------|
|追加|v1.0|Webhooks が、_/Subscriptions_ リソースから V1.0 エンドポイントで一般公開されるようになりました。Outlook と Office 365 のグループ会話からデータに関する通知を受信するためのサブスクリプションを作成、読み取り、更新、削除します。|

### <a name="user"></a>ユーザー

|**変更の種類**|**バージョン**|**説明**|
|:--------------|:-----------|:--------------|
|追加|ベータ版|_mailboxSettings_ プロパティおよび対応する型が追加されました。|

## <a name="february-2016"></a>2016 年 2 月

### <a name="driveitem"></a>DriveItem

|**変更の種類**|**バージョン**|**説明**|
|:--------------|:-----------|:--------------|
|追加|v1.0 およびベータ版|Microsoft アカウントに対する driveItem の新しい _remoteItem_ プロパティ。|

### <a name="general"></a>全般

|**変更の種類**|**バージョン**|**説明**|
|:--------------|:-----------|:--------------|
|変更|v1.0 およびベータ版|-_/me/drive_ が、Microsoft アカウントおよび職場と学校のアカウントの両方で機能するようになりました。|
|変更|v1.0 およびベータ版|OneDrive ストレージがオンデマンドでプロビジョニングされたアカウントの Drive 要求は、動作の信頼性がより高くなり、テナントの既定の SharePoint サイトで非標準の名前が使用されるような、より多くのシナリオで動作します。|
|削除|ベータ版|1.0 スキーマにより厳密に一致するように、実装されていないさまざまな型がベータ スキーマから削除されました。|

### <a name="subscriptions"></a>サブスクリプション

|**変更の種類**|**バージョン**|**説明**|
|:--------------|:-----------|:--------------|
|追加|ベータ版|サブスクリプション作成時の notificationUrl 検証。詳細については、「[Microsoft Graph の WebHooks の更新 - 2016 年 1 月](http://dev.office.com/blogs/Microsoft-Graph-WebHooks-Update-January-2016)」をご覧ください。|
|追加|ベータ版|サブスクリプション エンティティを削除できるようになりました。DELETE https://graph.microsoft.com/beta/subscriptions/|

### <a name="users"></a>ユーザー

|**変更の種類**|**バージョン**|**説明**|
|:--------------|:-----------|:--------------|
|変更|v1.0 およびベータ版|Microsoft アカウントに対して _displayName_ が返されるようになりました。|

## <a name="january-2016"></a>2016 年 1 月

### <a name="contacts"></a>連絡先

|**変更の種類**|**バージョン**|**説明**|
|:--------------|:-----------|:--------------|
|追加|v1.0|mobilePhone プロパティが個人用連絡先エンティティ セットに追加されました。|

### <a name="directoryobjects"></a>directoryObjects

|**変更の種類**|**バージョン**|**説明**|
|:--------------|:-----------|:--------------|
|修正|v1.0 およびベータ版|directoryObjects にバインドされている呼び出しアクションが修正されました。このアクションは次のエラーで失敗していました。「操作からの戻り値の型は、指定したエンティティ セットで使用できません。」これは、次のアクションに適用されます: _microsoft.graph.checkMemberObjects_、_microsoft.graph.getMemberObjects_、_microsoft.graph.checkMemberGroups_、_microsoft.graph.assignLicense_、_microsoft.graph.changePassword_。|

## <a name="december-2015"></a>2015 年 12 月

### <a name="contacts"></a>連絡先

|**変更の種類**|**バージョン**|**説明**|
|:--------------|:-----------|:--------------|
|追加|ベータ版|mobilePhone プロパティが個人用連絡先エンティティ セットに追加されました。|

### <a name="general"></a>全般

|**変更の種類**|**バージョン**|**説明**|
|:--------------|:-----------|:--------------|
|修正|v1.0 およびベータ版|同じプロパティを複数回指定した $filter 式を使用する要求が修正されました。この要求は次の 500 エラーで失敗していました。「同じキーを持つ項目が既に追加されています。」|
|修正プログラム|v1.0 およびベータ版|アクション パラメーターの名前と値で大文字と小文字が区別されない問題を修正しました。|
|修正|v1.0 およびベータ版|一部の埋め込み複合プロパティに null 値を含むペイロードの要求処理を修正しました。この要求は null 参照の例外で失敗していました。|
|追加|v1.0 およびベータ版|複合型プロパティの並べ替えとフィルター処理のサポートが追加されました。|
|追加|v1.0 およびベータ版|401 応答の www-authenticate ヘッダーに authorization_uri プロパティが追加されました。この URI は、トークンの取得フローを開始するために使用できます。|
|追加|v1.0 およびベータ版|ユーザーとグループ全体でエラー メッセージが改善されました。|

### <a name="groups"></a>グループ

|**変更の種類**|**バージョン**|**説明**|
|:--------------|:-----------|:--------------|
|修正|v1.0 およびベータ版|次のグループ アクションの呼び出しを修正しました: _microsoft.graph.addFavorite_、_microsoft.graph.removeFavorite_、_microsoft.graph.resetUnseenCount_。|

### <a name="messages"></a>メッセージ

|**変更の種類**|**バージョン**|**説明**|
|:--------------|:-----------|:--------------|
|追加|ベータ版|eventMessage の eventMessageRequest サブタイプと、startDateTime、endDateTime、location、type、recurrence、isOutOfDate プロパティが、eventMessage 型に追加されました。|

### <a name="users"></a>ユーザー

|**変更の種類**|**バージョン**|**説明**|
|:--------------|:-----------|:--------------|
|修正|v1.0 およびベータ版|ユーザー プリンシパル名 (UPN) でユーザーを参照する場合に、他のユーザーで特定のユーザー プロパティを選択できてしまう問題を修正しました。例: https://graph.microsoft.com/v1.0/users/anotherUser@contoso.com?$select=aboutMe|
|修正プログラム|v1.0 およびベータ版|ユーザーにバインドされた _microsoft.graph.reminderView_ 関数の呼び出しを修正しました。この呼び出しは次のエラーで失敗していました。「‘Microsoft.OutlookServices.Reminder’ 型で ‘businessPhones’ という名前のプロパティは見つかりませんでした。」|
|修正プログラム|v1.0 およびベータ版|400 エラーで失敗していた、ユーザーの作成と更新 (POST/PATCH /v1.0/users) を修正しました。|
