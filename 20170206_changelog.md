# <a name="changelog-for-microsoft-graph"></a>Änderungsprotokoll für Microsoft Graph

Dieses Änderungsprotokoll deckt alle Änderungen in Microsoft Graph ab, einschließlich der Version 1.0 und des Beta-Endpunkts von Microsoft Graph-APIs.  

## <a name="february-2017"></a>Februar 2017

### <a name="intune-apis"></a>Intune APIs

|**Änderungstyp**|**Version**|**Beschreibung**|
|:-------------|:-----------|:--------------|
|Ergänzungen|Beta|Hinzugefügte neue Entitäten: <br/>[appReportingOverviewStatus](https://graph.microsoft.io/en-us/docs/api-reference/beta/resources/intune_apps_appreportingoverviewstatus)<br/>[deviceComplianceDeviceOverview](https://graph.microsoft.io/en-us/docs//api-reference/beta/resources/intune_deviceconfig_devicecompliancedeviceoverview)<br/>[deviceConfigurationDeviceOverview](https://graph.microsoft.io/en-us/docs/api-reference/beta/resources/intune_deviceconfig_deviceconfigurationdeviceoverview)<br/>[deviceManagementExchangeOnpremisesPolicy](https://graph.microsoft.io/en-us/docs/api-reference/beta/resources/intune_onboarding_devicemanagementexchangeonpremisespolicy)<br/>[iosDeviceFeaturesConfiguration](https://graph.microsoft.io/en-us/docs/api-reference/beta/resources/intune_deviceconfig_iosdevicefeaturesconfiguration)<br/>[iosEducationDeviceConfiguration](https://graph.microsoft.io/en-us/docs/api-reference/beta/resources/intune_deviceconfig_ioseducationdeviceconfiguration)<br/>[iosLobAppProvisioningConfiguration](https://graph.microsoft.io/en-us/docs/api-reference/beta/resources/intune_apps_ioslobappprovisioningconfiguration)<br/>[onpremisesConditionalAccessSettings](https://graph.microsoft.io/en-us/docs/api-reference/beta/resources/intune_onboarding_onpremisesconditionalaccesssettings)<br/>[sharedPCConfiguration](https://graph.microsoft.io/en-us/docs/api-reference/beta/resources/intune_deviceconfig_sharedpcconfiguration)<br/>[windows10EnterpriseModernAppManagementConfiguration](https://graph.microsoft.io/en-us/docs/api-reference/beta/resources/intune_deviceconfig_windows10enterprisemodernappmanagementconfiguration)<br/>[windows10SecureAssessmentConfiguration](https://graph.microsoft.io/en-us/docs/api-reference/beta/resources/intune_deviceconfig_windows10secureassessmentconfiguration)<br/>[windows10WindowsInformationProtectionConfiguration](https://graph.microsoft.io/en-us/docs/api-reference/beta/resources/intune_deviceconfig_windows10windowsinformationprotectionconfiguration)|
|Löschung|Beta|Die folgenden komplexen Typen wurden entfernt und durch microsoft.graph.json ersetzt:<br/>managedAppDeploymentSummary <br/>managedAppSummary |
|Ändern|Beta|Der Eigenschaftentyp „appConfigComplianceStatus“ wurde in den folgenden Entitäten durch „complianceStatus“ ersetzt: <br/>[managedDeviceMobileAppConfigurationDeviceStatus](https://graph.microsoft.io/en-us/docs/api-reference/beta/resources/intune_apps_manageddevicemobileappconfigurationdevicestatus)<br/>[managedDeviceMobileAppConfigurationUserStatus](https://graph.microsoft.io/en-us/docs/api-reference/beta/resources/intune_apps_manageddevicemobileappconfigurationuserstatus)<br/><br/>Für die Ressource [managedAppStatusRaw](https://graph.microsoft.io/en-us/docs/api-reference/beta/resources/intune_mam_managedappstatusraw) wurde der Typ des Eigenschafteninhalts von „managedAppSummary“ in „json“ geändert.|

## <a name="january-2017"></a>Januar 2017

### <a name="outlook-calendar"></a>Outlook-Kalender

|**Änderungstyp**|**Version**|**Beschreibung**|
|:-------------|:-----------|:--------------|
|Ergänzungen|v1.0|Neue Aktion [findMeetingTimes](https://graph.microsoft.io/en-us/docs/api-reference/v1.0/api/user_findmeetingtimes) für die [Benutzer](https://graph.microsoft.io/en-us/docs/api-reference/v1.0/resources/user)-Ressource.|
|Ergänzungen|v1.0|Neuer komplexer Typ [attendeeBase](https://graph.microsoft.io/en-us/docs/api-reference/v1.0/resources/attendeebase), der  aus einer Typeigenschaft für den Teilnehmertyp besteht.|
|Ergänzungen|v1.0|Neue komplexe Typen:<br/>[attendeeAvailability](https://graph.microsoft.io/en-us/docs/api-reference/v1.0/resources/attendeeavailability)<br/>[locationConstraint](https://graph.microsoft.io/en-us/docs/api-reference/v1.0/resources/locationconstraint) <br/>[locationConstraintItem](https://graph.microsoft.io/en-us/docs/api-reference/v1.0/resources/locationconstraintitem)<br/>[meetingTimeSuggestion](https://graph.microsoft.io/en-us/docs/api-reference/v1.0/resources/meetingtimesuggestion)<br/>[meetingTimeSuggestionsResult](https://graph.microsoft.io/en-us/docs/api-reference/v1.0/resources/meetingtimesuggestionsresult)<br/>[timeConstraint](https://graph.microsoft.io/en-us/docs/api-reference/v1.0/resources/timeconstraint)<br/>[timeSlot](https://graph.microsoft.io/en-us/docs/api-reference/v1.0/resources/timeslot)|
|Änderung|v1.0|Der komplexe Typ des [Teilnehmers](https://graph.microsoft.io/en-us/docs/api-reference/v1.0/resources/attendee) wird jetzt von AttendeeBase abgeleitet, der wiederum vom [Empfänger](https://graph.microsoft.io/en-us/docs/api-reference/v1.0/resources/recipient) abgeleitet wird. Einschließlich die vererbten Eigenschaften besteht es aus denselben **Status**-, **Typ**- und **E-Mail-Adresse**-Eigenschaften wie zuvor.|
|Ergänzungen|Beta|hexColor zur [Kalender](https://graph.microsoft.io/en-us/docs/api-reference/beta/resources/calendar)-Ressource hinzugefügt.|

## <a name="december-2016"></a>Dezember 2016

### <a name="delta-query"></a>Delta-Abfrage

|**Änderungstyp**|**Version**|**Beschreibung**|
|:-------------|:-----------|:--------------|
|Ergänzungen|Beta|Eine neue Deltafunktion zu folgenden Elemente hinzufügen, um [Delta Abfrage](https://graph.microsoft.io/en-us/docs/concepts/delta_query_overview) auszuführen:<br/>contact<br/>contactFolder<br/>event<br/>group<br/>mailFolder<br/>message<br/>user<br/>Beispiele finden Sie unter Folgendem:<br/>[Inkrementelle Änderungen an Gruppen abrufen (Vorschau)](https://graph.microsoft.io/en-us/docs/concepts/delta_query_groups)<br/>[Inkrementelle Änderungen an Nachrichten in einem Ordner abrufen (Vorschau)](https://graph.microsoft.io/en-us/docs/concepts/delta_query_message)<br/>[Inkrementelle Änderungen an Benutzern abrufen (Vorschau)](https://graph.microsoft.io/en-us/docs/concepts/delta_query_users)|

### <a name="excel-apis"></a>Excel-APIs

|**Änderungstyp**|**Version**|**Beschreibung**|
|:-------------|:-----------|:--------------|
|Ergänzungen|v1.0|workbookPivotTable-Ressource, refresh- und refreshAll-Aktion für pivotTables, workbookRangeView-Ressource, visibleView-Aktion für den gefilterten Bereich zum Zurückgeben von workbookRangeView an den Benutzer, Abrufen der Sammlung rows und range-Ressource deaktiviert für visibleView, Funktionen columnsAfter, columnsBefore, resizedRange, rowsAbove und rowsBelow deaktiviert für range-Ressource und neue Tabelleneigenschaften hinzugefügt.|

### <a name="intune-apis"></a>Intune APIs

|**Änderungstyp**|**Version**|**Beschreibung**|
|:-------------|:-----------|:--------------|
|Ergänzungen|Beta|Ressourcen und Methode-APIs für Microsoft Intune hinzugefügt. Dies ist eine große Gruppe von Ressourcen und Methoden, um die öffentliche Vorschau von Intune auf Azure-Portal zu unterstützen. Informationen über den Dienst Intune finden Sie unter der [Intune Dokumentation](https://go.microsoft.com/fwlink/?linkid=836405). Informationen zu den Intune Ressourcen und APIs finden Sie unter [Arbeiten mit Intune in Microsoft Graph](https://graph.microsoft.io/en-us/docs/api-reference/beta/resources/intune_graph_overview).|

## <a name="october-2016"></a>Oktober 2016

### <a name="authorization-provider"></a>Autorisierungsanbieter

|**Änderungstyp**|**Version**|**Beschreibung**|
|:-------------|:-----------|:--------------|
|Ergänzungen|v1.0 und Beta|Der v2-Authentifizierungsendpunkt unterstützt jetzt die OAuth-Clientgenehmigung „_credentials“, die für [Dämone und lange ausgeführte Prozesse in Geschäftsszenarien](https://azure.microsoft.com/en-us/documentation/articles/active-directory-v2-protocols-oauth-client-creds/) verwendet werden kann.|
|Ergänzungen|v1.0 und Beta|Der v2-Authentifizierungsendpunkt unterstützt jetzt [Berechtigungsbereiche, die die Zustimmung des Administrators erfordern](http://graph.microsoft.io/en-us/docs/authorization/permission_scopes#permission-scope-details), über den [Administratorzustimmungs-Endpunkt](https://azure.microsoft.com/en-us/documentation/articles/active-directory-v2-scopes/#admin-restricted-scopes).|
|Ergänzungen|v1.0 und Beta|Der v2-Authentifizierungsendpunkt unterstützt jetzt die Administratorzustimmung für alle Benutzer in einem Mandanten über die [Administratorzustimmungs-Endpunkt](https://azure.microsoft.com/en-us/documentation/articles/active-directory-v2-scopes/#admin-restricted-scopes).|

### <a name="invitation-apis"></a>Einladungs-APIs

|**Änderungstyp**|**Version**|**Beschreibung**|
|:-------------|:-----------|:--------------|
|Ergänzungen|Beta|Die invitedUserType-Eigenschaft wurde dem Einladungsentitätstyp hinzugefügt, der den Typ von Benutzer definiert („Gast“ oder „Mitglied“), der eingeladen wird.|
|Löschung|Beta|Die invitedToGroups-Eigenschaft wird am 11.11.2016 aus dem Einladungsentitätstyp entfernt. Dies bedeutet, dass Sie mithilfe dieser API keinen eingeladenen Benutzer mehr zu einer Gruppe hinzufügen können. Verwenden von [API zum Hinzufügen von Mitglieder](https://graph.microsoft.io/en-us/docs/api-reference/v1.0/api/group_post_members) zum Hinzufügen eines Benutzers zu einer Gruppe.|

## <a name="september-2016"></a>September 2016

### <a name="azure-ad-application-proxy"></a>Azure AD-Anwendungsproxy

|**Änderungstyp**|**Version**|**Beschreibung**|
|:--------------|:-----------|:--------------|
|Ergänzungen|Beta|Azure AD-Anwendungsproxy-APIs sind jetzt im Beta-Endpunkt von Microsoft Graph verfügbar. Diese APIs ermöglichen die sichere Veröffentlichung von lokalen Anwendungen für Benutzer außerhalb des Organisationsnetzwerks unter Verwendung von Azure AD als allgemeine Zugriffssteuerebene. Mit den veröffentlichten APIs können Sie Anwendungen schreiben, die verschiedene Aspekte vom Anwendungsproxy abrufen und aktualisieren können, u. a. _Connectors_, _connectorGroups_ und _onPremisesPublishing_-Einstellungen einer Anwendung.|

### <a name="drive"></a>Laufwerk

|**Änderungstyp**|**Version**|**Beschreibung**|
|:--------------|:-----------|:--------------|
|Ergänzungen|Beta|Die _freigegebene_ Sammlung wurde hinzugefügt, um Zugriff auf freigegebene driveItems über shareId oder Freigabe-URL zu ermöglichen.|
|Ergänzungen|Beta|Die _search_-Funktion wurde zu einem Laufwerk hinzugefügt, um mehr Elemente als nur die im Stammordner des Laufwerks durchsuchen zu können.|
 

### <a name="driveitem"></a>DriveItem

|**Änderungstyp**|**Version**|**Beschreibung**|
|:--------------|:-----------|:--------------|
|Ergänzungen|Beta|Zusätzliche Unterstützung für _createUploadSession_, sodass Dateien, die größer als 4 MB sind, in OneDrive, OneDrive for Business und SharePoint-Dokumentbibliotheken hochgeladen werden können.|
|Ergänzungen|Beta|Die_sharepointIds_-Eigenschaft für driveItem wurde hinzugefügt, mit der die herkömmlichen IDs für SharePoint API für in SharePoint gespeicherte driveItems-Elemente zurückgegeben werden.|
|Ergänzungen|Beta|Zusätzliche Eigenschaften für _remoteItem_ hinzugefügt.|
|Ergänzungen|Beta|_quickXorHash_-Wert für Dateien in OneDrive for Business hinzugefügt.|
|Ergänzungen|Beta|Bereich für _createSharingLink_ hinzugefügt, mit dem Unternehmen freizugebende Links oder anonyme freizugebende Links erstellen können.|

### <a name="extended-properties"></a>Erweiterte Eigenschaften

|**Änderungstyp**|**Version**|**Beschreibung**|
|:--------------|:-----------|:--------------|
|Ergänzungen|v1.0|[Erweiterte Eigenschaften](http://graph.microsoft.io/en-us/docs/api-reference/v1.0/resources/extended-properties-overview) werden jetzt von folgenden Ressourcen unterstützt: Nachricht, mailFolder, Ereignis, Kalender, Kontakt, contactFolder, Gruppenereignis, Gruppenkalender, Gruppenbeitrag.|

### <a name="groups"></a>Gruppen

Unterstützung für dynamische Gruppenmitgliedschaft über die öffentliche Vorschau-API hinzugefügt, einschließlich der aufgelisteten Zusätze in der folgenden Tabelle:

|**Änderungstyp**|**Version**|**Beschreibung**|
|:--------------|:-----------|:--------------|
|Ergänzungen|Beta|Hinzugefügte **membershipRule**-Eigenschaft enthält Regeln, die die Mitgliedschaften für diese Gruppe steuern, wenn es sich bei der Gruppe um eine dynamische Gruppe handelt.|
|Ergänzungen|Beta|Die **membershipRuleProcessingState**-Eigenschaft wurde hinzugefügt, mit der gesteuert wird, ob die Verarbeitung dynamischer Mitgliedschaften für diese Gruppe aktiviert oder angehalten ist.|
|Ergänzungen|Beta|Legen Sie die **groupTypes**-Eigenschaft so fest, dass sie **„DynamicMembership“** enthält, um die Funktionen der dynamischen Gruppen für diese Gruppe zu erweitern|
|Ergänzungen|Beta|Die **preferredLanguage**-Eigenschaft wurde hinzugefügt, um die bevorzugte Sprache für eine Office 365-Gruppe anzugeben.|
|Ergänzungen|Beta|Die **theme**-Eigenschaft wurde hinzugefügt, um ein Farbschema für eine Office 365-Gruppe anzugeben.|

### <a name="hybrid-deployment-support"></a>Unterstützung für Hybridbereitstellung

|**Änderungstyp**|**Version**|**Beschreibung**|
|:--------------|:-----------|:--------------|
|Ergänzungen|v1.0|Apps können Version 1.0 von Outlook-Mail, -Kalender und Kontakte-APIs für den Zugriff auf lokale Postfächer in einer Hybridbereitstellung mit Exchange 2016 mit kumulativem Update 3 (KU3) verwenden. Erfahren Sie mehr über die REST-API-Unterstützung in [Hybridbereitstellungen](https://graph.microsoft.io/en-us/docs/overview/hybrid_rest_support). **Hinweis:** Wenn Sie diese API-Sets in Version 1.0 verwenden, können Sie jetzt Ihre Apps, einschließlich Produktions-Apps für lokale Postfächer verwenden, die den spezifischen Anforderungen für Hybridbereitstellung entsprechen. Diese Funktion ist derzeit in der Vorschau.|

### <a name="identityriskevents"></a>IdentityRiskEvents

|**Änderungstyp**|**Version**|**Beschreibung**|
|:--------------|:-----------|:--------------|
|Änderung|Beta|Im Rahmen der Schemaänderungen, bei denen der Typ der zwei location-Eigenschaften durch einen neuen komplexen Typ in dem identityRiskEvents-Endpunkt ersetzt wird, werden die folgenden Eigenschaften für den identityRiskEvents-Endpunkt geändert/hinzugefügt:</br>**location** – geändert von Edm.String zu ComplexType signInLocation.<br/>**previousLocation** – geändert von Edm.String zu ComplexType signInLocation.<br/>**signInLocation** – neuer ComplexType, der Ort, Bundesland, countryOrRegion und geoCoordinates-Eigenschaften enthält.<br/>**geoCoordinates** – neuer ComplexType, der Breiten- und Längengradeigenschaften enthält.|

### <a name="invitation-manager"></a>Einladungs-Manager

|**Änderungstyp**|**Version**|**Beschreibung**|
|:--------------|:-----------|:--------------|
|Ergänzungen|Beta|Einladungs-Manager-APIs sind jetzt im Beta-Endpunkt von Microsoft Graph verfügbar. Verwenden Sie Einladungs-Manager-APIs, um eine Einladung zum Hinzufügen eines externen Benutzers zu der Organisation zu erstellen. Im Rahmen der Einladung können Sie auch angeben, ob der eingeladene Benutzer zu einer Office 365-Gruppe hinzugefügt werden soll. Weitere Informationen finden Sie im [Einladungs-Manager](https://graph.microsoft.io/en-us/docs/api-reference/beta/resources/invitation).|

### <a name="onedrive"></a>OneDrive

|**Änderungstyp**|**Version**|**Beschreibung**|
|:--------------|:-----------|:--------------|
|Ergänzungen|v1.0|**CreateUploadSession**-Methode für **driveItem** hinzugefügt, die Uploads von großen Dateien und wiederaufnehmbare Uploads ermöglicht.|
|Ergänzungen|v1.0|Eigenschaften für das Nachverfolgen von SharePoint-IDs für Elemente aus SharePoint (**sharepointIds**) und eine Eigenschaft zum Identifizieren von Stammordnern (**root**) hinzugefügt.|
|Ergänzungen|v1.0|**Shares**-Stammsammlung hinzugefügt, die mit shareIds oder Freigabelinks für den Zugriff auf freigegebene Elemente in OneDrive und SharePoint verwendet werden kann. Gibt den neuen Typ sharedDriveItem zurück.|
|Ergänzungen|v1.0|**Invite**-Methode für driveItem zum Hinzufügen von Berechtigungen für Elemente hinzugefügt. |
|Ergänzungen|v1.0|**Search**-Methode für das Laufwerk, mit der Elemente im Laufwerk und freigegebene Elemente durchsucht werden können hinzugefügt. |
|Ergänzungen|v1.0|**processingMetadata**-Eigenschaft für quickXorHash-Eigenschaft mit komplexem Dateityp für Hashes mit komplexem Typ hinzugefügt. |
|Ergänzungen|v1.0|**quickXorHash**-Eigenschaft für Hashes mit komplexem Typ hinzugefügt |

### <a name="outlook-calendar"></a>Outlook-Kalender

|**Änderungstyp**|**Version**|**Beschreibung**|
|:--------------|:-----------|:--------------|
|Ergänzungen|v1.0|**onlineMeetingUrl**-Eigenschaft zur [Ereignis](http://graph.microsoft.io/en-us/docs/api-reference/v1.0/resources/event)-Ressource hinzugefügt.|
|Ergänzungen|Beta|Die [forward](http://graph.microsoft.io/en-us/docs/api-reference/beta/api/event_forward)-Aktion wurde zur Ereignisressource hinzugefügt.|
|Ergänzungen|Beta|Die folgenden Eigenschaften wurden zur [calendar](http://graph.microsoft.io/en-us/docs/api-reference/beta/resources/calendar)-Ressource zur Unterstützung der Freigabe von Kalendern hinzugefügt: **canEdit**, **canShare**, **canViewPrivateItems**, **isShared**, **isShareWithMe** und **Besitzer**.|

### <a name="outlook-mail"></a>Outlook-Mail

|**Änderungstyp**|**Version**|**Beschreibung**|
|:--------------|:-----------|:--------------|
|Ergänzungen|v1.0|[mailboxSettings](http://graph.microsoft.io/en-us/docs/api-reference/v1.0/resources/mailboxsettings) komplexer Typ, einschließlich der **automaticRepliesSetting**-, **timeZone**- und **Sprache**-Eigenschaften hinzugefügt.|
|Ergänzungen|v1.0|**mailboxSettings**-Eigenschaft zur [Benutzer](http://graph.microsoft.io/en-us/docs/api-reference/v1.0/resources/user)-Ressource hinzugefügt.|
|Ergänzungen|Beta|Unterstützung für das Erstellen, Auflisten, Abrufen und Löschen einer oder mehrerer Instanzen von [Erwähnungen](http://graph.microsoft.io/en-us/docs/api-reference/beta/resources/mention) in einer Nachricht hinzugefügt. Erwähnungen unterstützen Aufrufe, um die Aufmerksamkeit anderer Benutzer in einer Nachricht zu erhalten.|
|Ergänzungen|Beta|Unterstützung für die [getMailTips](http://graph.microsoft.io/en-us/docs/api-reference/beta/api/user_getmailtips)-Aktion, um beliebige MailTips für bestimmte Empfänger zu erhalten hinzugefügt. Folgende Ressourcen wurden hinzugefügt: automaticRepliesMailTips, mailTips, mailTipsError.|

### <a name="query-parameters"></a>Abfrageparameter

|**Änderungstyp**|**Version**|**Beschreibung**|
|:--------------|:-----------|:--------------|
|Änderung|Beta|Abfrageparameter ohne '$' Präfixe sind ab dem 26.09.2016 unterstützt. Das Präfix '$' in der Abfrageparameter ist optional. Beispiele finden Sie unter [Abfrageparameter ohne $ Präfixe in Microsoft Graph unterstützen](http://dev.office.com/queryparametersinMicrosoftGraph).|

### <a name="sharepoint"></a>SharePoint

|**Änderungstyp**|**Version**|**Beschreibung**|
|:--------------|:-----------|:--------------|
|Ergänzungen|Beta|Zugriff auf SharePoint-Websites und [-listen nach ID](http://graph.microsoft.io/en-us/docs/api-reference/beta/api/list_get) oder [Pfad/URL](http://graph.microsoft.io/en-us/docs/api-reference/beta/api/baseitem_getbyurl).|
|Ergänzungen|Beta|Unterstützung für das [Auflisten, Erstellen, Abrufen und Löschen von Instanzen des listItem-Elements](http://graph.microsoft.io/en-us/docs/api-reference/beta/resources/listitem).|

### <a name="users"></a>Benutzer

|**Änderungstyp**|**Version**|**Beschreibung**|
|:--------------|:-----------|:--------------|
|Ergänzungen|Beta|Schreibgeschützte **refreshTokensValidFromDateTime**-Eigenschaft wurde hinzugefügt, um anzugeben, ab wann die Aktualisierungs- oder Sitzungstoken gültig sind. Alle Token, die vor diesem Zeitpunkt ausgestellt wurden, sind ungültig. Bei jedem Versuch, diese Token zu verwenden, werden Benutzer dazu aufgefordert, sich erneut anzumelden.|
|Ergänzungen|Beta|Die **showInAddressList**-Eigenschaft wurde hinzugefügt, mit der gesteuert wird, ob die globale Adressliste von Outlook diesen Benutzer enthalten soll.|
|Ergänzungen|Beta|Die **invalidateAllRefreshTokens**-Dienstaktion wurde hinzugefügt, mit der alle Aktualisierungs- und Sitzungstoken des Benutzers, die für Anwendungen ausgestellt wurden, ungültig werden, indem die **refreshTokensValidFromDateTime**-Benutzereigenschaft auf aktuelles Datum und aktuelle Uhrzeit zurückgesetzt wird.|


### <a name="webhooks"></a>Webhooks

|**Änderungstyp**|**Version**|**Beschreibung**|
|:--------------|:-----------|:--------------|
|Ergänzungen|Beta|Laufwerkstammelemente zu Webhooks als Ressource hinzugefügt, die abonniert werden kann.|

## <a name="august-2016"></a>August 2016

### <a name="contacts"></a>Kontakte

|**Änderungstyp**|**Version**|**Beschreibung**|
|:--------------|:-----------|:--------------|
|Ergänzungen|Beta|Im Rahmen der Schemaänderungen, bei denen einige Eigenschaften entfernt und entsprechende Sammlungen zu Kontaktendpunkten hinzugefügt wurden, wurden die folgenden Eigenschaften zu Kontaktendpunkten hinzugefügt: _Websites – Collection(ComplexType: Website)_,_Phones – Collection (ComplexType: Phone)_, _PostalAddress – Collection(ComplexType: PhysicalAddress)_. Weitere Informationen finden Sie unter dem Blogeintrag: [Upcoming changes to Contacts and People APIs](https://blogs.msdn.microsoft.com/exchangedev/2016/06/09/upcoming-changes-to-contacts-and-people-apis/).|
|Löschung|Beta|Im Rahmen der Schemaänderungen, bei denen einige Eigenschaften entfernt und entsprechende Sammlungen zu Kontaktendpunkten hinzugefügt wurden, wurden die folgenden Eigenschaften von Kontaktendpunkten entfernt: _BusinessHomePage_,_HomePhones_, _MobilePhone1_, _BusinessPhones_, _HomeAddress_, _BusinessAddress_, _OtherAddress_. Weitere Informationen finden Sie unter dem Blogeintrag: [Upcoming changes to Contacts and People APIs](https://blogs.msdn.microsoft.com/exchangedev/2016/06/09/upcoming-changes-to-contacts-and-people-apis/).|

### <a name="excel-apis"></a>Excel-APIs

|**Änderungstyp**|**Version**|**Beschreibung**|
|:--------------|:-----------|:--------------|
|Ergänzungen|v1.0|Excel REST-API für Microsoft Graph ist nun allgemein verfügbar. Sie können jetzt umfassende und genaue Integrationen mit Excel-Arbeitsmappen in Office 365 erstellen. Weitere Informationen finden Sie unter dem Blogeintrag [Power your apps with the new Excel REST API on the Microsoft Graph](http://dev.office.com/blogs/power-your-apps-with-the-new-excel-rest-api).|

### <a name="people"></a>Kontakte

|**Änderungstyp**|**Version**|**Beschreibung**|
|:--------------|:-----------|:--------------|
|Änderung|Beta|_WebSite_-Eigenschaft wurde in _Websites_ umbenannt. Weitere Informationen finden Sie unter [Upcoming changes to Contacts and People APIs](https://blogs.msdn.microsoft.com/exchangedev/2016/06/09/upcoming-changes-to-contacts-and-people-apis/).|

### <a name="privileged-identity-management"></a>Privileged Identity Management

|**Änderungstyp**|**Version**|**Beschreibung**|
|:--------------|:-----------|:--------------|
|Ergänzungen|Beta|REST-APIs für Privileged Identity Management (PIM) sind jetzt im Beta-Endpunkt von Microsoft Graph verfügbar. [Privileged Identity Management](https://azure.microsoft.com/en-us/documentation/articles/active-directory-privileged-identity-management-configure/) ermöglicht eine Just-In-Time-Aktivierung für privilegierte Azure AD-Rollen wie globaler Administrator, Abrechnungsadministrator usw. Mit den veröffentlichten APIs können Sie Anwendungen schreiben, die privilegierte Rollenzuweisungen abrufen und aktualisieren und Benutzer in Rollen aktivieren. Weitere Informationen finden Sie unter [Microsoft Graph: Azure AD Privileged Identity Management Vorschau-APIs sind verfügbar in Beta](http://dev.office.com/blogs/microsoft-graph-azure-ad-privileged-identity-management-apis-beta) und [Azure AD Privileged Identity Management](https://graph.microsoft.io/en-us/docs/api-reference/beta/resources/privilegedidentitymanagement_root).|

## <a name="july-2016"></a>Juli 2016
  
### <a name="administrative-units"></a>Administrative Einheiten

|**Änderungstyp**|**Version**|**Beschreibung**|
|:--------------|:-----------|:--------------|
|Ergänzungen|Beta|Die neue Administrative Unites preview API wurde eingeführt. Mit administrativen Einheiten können Organisationen ihre Azure Active Directory-Umgebung aufteilen und administrative Aufgaben auf diese Unterteilungen weitergeben. Unterteilungen können Regionen, Abteilungen, Kostenstellen usw. darstellen. Dies kann jetzt über die Microsoft Graph-API verwaltet werden.|

## <a name="june-2016"></a>Juni 2016

### <a name="identityriskevents"></a>IdentityRiskEvents

|**Änderungstyp**|**Version**|**Beschreibung**|
|:--------------|:-----------|:--------------|
|Ergänzungen|Beta|Die neue  IdentityRiskEvents Vorschau-API wurde eingeführt. Diese API funktioniert in Verbindung mit Azure Active Directory Identity Protection. Sie können Abfrage-Risiko-Ereignissen abfragen, die durch Identity Protection generiert wurden. Weitere Informationen finden Sie unter [Einführung einer neuen Vorschau-API für Microsoft Graph: IdentityRiskEvents](http://dev.office.com/blogs/identityriskevents-api-preview)-Blogeintrag.

### <a name="subscriptions"></a>Abonnements

|**Änderungstyp**|**Version**|**Beschreibung**|
|:--------------|:-----------|:--------------|
|Ergänzungen|Beta|Nur App-Bereiche werden jetzt für _Mail_- und _Kontakte_-Abonnements unterstützt.|

## <a name="may-2016"></a>Mai 2016

### <a name="calendar"></a>Kalender

|**Änderungstyp**|**Version**|**Beschreibung**|
|:--------------|:-----------|:--------------|
|Änderungen|Beta|Ändert sich zur findmeetingtimes-API. Weitere Informationen finden Sie unter dem Blogeintrag [Microsoft Graph findMeetingTimes API update](http://dev.office.com/microsoft-graph-findmeetingtimes-api-update) Blogbeitrag. Diese Änderung ist seit dem 19. Mai 2016 wirksam.

### <a name="contact"></a>Kontakt

|**Änderungstyp**|**Version**|**Beschreibung**|
|:--------------|:-----------|:--------------|
|Ergänzungen|v1.0|_Erweiterungen_ hinzugefügt, die einen abstrakten Typ für die Unterstützung des offenen Typs openTypeExtension von OData v4 darstellen.|

### <a name="directory"></a>Verzeichnis

|**Änderungstyp**|**Version**|**Beschreibung**|
|:--------------|:-----------|:--------------|
|Änderungen|Beta|_settingTemplateId_ in _templateId_ umbenannt. Diese Änderung wird ab dem 19. Mai 2016 wirksam.|

### <a name="event"></a>Ereignis

|**Änderungstyp**|**Version**|**Beschreibung**|
|:--------------|:-----------|:--------------|
|Ergänzungen|v1.0|_Erweiterungen_ hinzugefügt, die einen abstrakten Typ für die Unterstützung des offenen Typs openTypeExtension von OData v4 darstellen.|

### <a name="eventmessages"></a>EventMessages

|**Änderungstyp**|**Version**|**Beschreibung**|
|:--------------|:-----------|:--------------|
|Ergänzungen|v1.0|_inferenceClassification_ und _Erweiterungen_ zu _eventMessages_ hinzugefügt.|
|Ergänzungen|Beta|_responseRequested_ zu _eventMessageRequest_ hinzugefügt.|

### <a name="messages"></a>Nachrichten

|**Änderungstyp**|**Version**|**Beschreibung**|
|:--------------|:-----------|:--------------|
|Ergänzungen|v1.0|_inferenceClassification_ und _extensions_ zu _Nachrichten_ hinzugefügt.|
|Ergänzungen|Beta|_wellknownname_ zu _contactFolder_ hinzugefügt.|Ändert sich zu _findmeetingtimes_-API. Weitere Informationen finden Sie unter dem Blogeintrag [Microsoft Graph findMeetingTimes API update](http://dev.office.com/microsoft-graph-findmeetingtimes-api-update) Blogbeitrag. Diese Änderung wird ab dem 19. Mai 2016 wirksam.|

### <a name="post"></a>Beitrag

|**Änderungstyp**|**Version**|**Beschreibung**|
|:--------------|:-----------|:--------------|
|Ergänzungen|v1.0|_Erweiterungen_ hinzugefügt, die einen abstrakten Typ für die Unterstützung des offenen Typs openTypeExtension von OData v4 darstellen.|

### <a name="user"></a>Benutzer

|**Änderungstyp**|**Version**|**Beschreibung**|
|:--------------|:-----------|:--------------|
|Ergänzungen|v1.0|_inferenceClassification_-Ressourcentyp hinzugefügt.|
|Ergänzungen|Beta|_timeZone_ zu _mailboxsettings_ hinzugefügt.|
|Ergänzungen|Beta|API _findMeetingTimesOLD _ zu _Benutzer_ hinzugefügt.|

## <a name="april-2016"></a>April 2016

### <a name="general"></a>Allgemein

|**Änderungstyp**|**Version**|**Beschreibung**|
|:--------------|:-----------|:--------------|
|Ergänzungen|v1.0 und Beta|Unterstützung für Berücksichtigung von _Accept-Encoding:gzip_ hinzugefügt.|
|Ergänzungen|v1.0|Unterstützung für Castsegment im erweiterten Pfad hinzugefügt. Beispiel: https://graph.microsoft.com/v1.0/me/messages?$expand=microsoft.graph.eventMessage/event|
|Ergänzungen|Beta|Unterstützung für PATCH-Anforderung anhand struktureller Eigenschaften hinzugefügt. Beispiel: 'PATCH /me/mailboxSettings'.|
|Ergänzungen|Beta|Azure Active Directory dient nun als Alternative für /beta/users/id/photo-Anforderungen, wenn Outlook die Anforderung nicht verarbeiten kann, beispielsweise wenn der Benutzer über keine Postfachlizenz oder der Mandant über kein Exchange Online-Abonnement verfügt. HINWEIS: Diese Alternative steht für GET und PATCH zur Verfügung.|
|Ergänzungen|Beta|Unterstützung für Castsegment im erweiterten Pfad hinzugefügt. Beispiel: https://graph.microsoft.com/v1.0/me/messages?$expand=microsoft.graph.eventMessage/event|

### <a name="onedrive"></a>OneDrive

|**Änderungstyp**|**Version**|**Beschreibung**|
|:--------------|:-----------|:--------------|
|Behebung|v1.0|Behebung eines Problems, aufgrund dessen bei createLink-Anforderungen OneDrive den 500-Fehler mit der Meldung „Nicht unterstützter Erweiterungseigenschaftentyp.“ zurückgab.|

## <a name="march-2016"></a>März 2016

### <a name="calendar"></a>Kalender

|**Änderungstyp**|**Version**|**Beschreibung**|
|:--------------|:-----------|:--------------|
|Ergänzungen|Beta|_singleValueExtendedProperties_ und _multiValueExtendedProperties_-Eigenschaften hinzugefügt.|
|Ergänzungen|Beta|_suggestionHint_-Eigenschaft zu _meetingTimeCandidate _ hinzugefügt.|
|Ergänzungen|Beta|_locationUri_-Eigenschaft zu _location_ hinzugefügt.|
|Ergänzungen|Beta|_type_ und _postOfficeBox_ zu _physicalAddress _ hinzugefügt.|
|Änderung|Beta|_findMeetingTimes_ verwendet jetzt den neuen _ReturnSuggestionHints_-Parameter.|
|Änderung|Beta|_findMeetingTimes_ gibt jetzt eine Sammlung von _meetingTimeCandidate_ zurück.|

### <a name="drive"></a>Laufwerk

|**Änderungstyp**|**Version**|**Beschreibung**|
|:--------------|:-----------|:--------------|
|Ergänzungen|v1.0 und Beta|_recent_-Funktion zum Auflisten eines Satzes von Elementen hinzugefügt, die zuletzt von dem angemeldeten Benutzer verwendet wurden. Diese Liste enthält Elemente, die sich in dem Laufwerk des Benutzers befinden sowie Elemente in anderen Laufwerken, auf die dieser Zugriff hat. Beispiel: GET /me/drive/recent.|
|Ergänzungen|v1.0 und Beta|_sharedWithMe_-Funktion zum Auflisten des Satzes von Elementen hinzugefügt, die für den aktuellen Benutzer freigegeben sind. Beispiel: GET /me/drive/sharedWithMe.|

### <a name="driveitem"></a>DriveItem

|**Änderungstyp**|**Version**|**Beschreibung**|
|:--------------|:-----------|:--------------|
|Ergänzungen|v1.0 und Beta|_remoteItem_-Typ zum Bereitstellen eines Links zu einem Element in einem anderen Laufwerk hinzugefügt.|
|Ergänzungen|v1.0 und Beta|_sharingInvitation_-Typ hinzugefügt, um Details zu verknüpften Einladungen zur Freigabe für diese Berechtigung bereitzustellen.|
|Ergänzungen|v1.0 und Beta|_delta_-Funktion zum Nachverfolgen von Änderungen an Elementen in einem Laufwerk hinzugefügt. Beispiel: GET /me/drive/items/{item-id}/delta|
|Ergänzungen|v1.0 und Beta|_copy_-Funktion zum Erstellen einer Kopie eines _driveItem_-Elements (einschließlich untergeordneter Elemente) unter einem neuen übergeordneten Element oder unter einem neuen Namen hinzugefügt. Beispiel: POST /me/drive/items/{item-id}/copy|
|Ergänzungen|v1.0 und Beta|_conflictBehavior_-Instanzattribute sind nun auf _driveItem_ anwendbar.|
|Ergänzungen|Beta|_invite_-Funktion zum Senden einer Einladung zur Freigabe für ein vorhandenes Element hinzugefügt. Mit einer Einladung zur Freigabe werden ein Freigabelink erstellt und eine E-Mail-Nachricht an den Empfänger die Einladung mit dem Freigabelink gesendet. Beispiel: POST /drive/items/{item-id}/invite

### <a name="event"></a>Ereignis

|**Änderungstyp**|**Version**|**Beschreibung**|
|:--------------|:-----------|:--------------|
|Ergänzungen|Beta|Neue _onlineMeetingUrl_-Eigenschaft und neue _cancel_-Methode hinzugefügt.|

### <a name="event-messages"></a>Ereignismeldungen

|**Änderungstyp**|**Version**|**Beschreibung**|
|:--------------|:-----------|:--------------|
|Ergänzungen|Beta|_startDateTime_, _endDateTime_, _location_, _type_, _recurrence_, _isOutOfDate_, _conversationIndex_, _unsubscribe_, _unsubscribeData_, _unsubscribeEnabled_ und _flag_-Eigenschaften zum _eventmessage_-Objekt hinzugefügt.|
|Ergänzungen|Beta|_singleValueExtendedProperties_ und _multiValueExtendedProperties_-Eigenschaften hinzugefügt.|
|Ergänzungen|Beta|Neue _unsubscribe_-Methode hinzugefügt.|

### <a name="excel"></a>Excel

|**Änderungstyp**|**Version**|**Beschreibung**|
|:--------------|:-----------|:--------------|
|Ergänzungen|Beta|Es wurden neue REST-APIs für Excel hinzugefügt, mit denen Sie Daten in einer Excel-Arbeitsmappe lesen und ändern können. Sie können nun intelligente Apps erstellen, mit denen Benutzer die in Excel-Arbeitsmappen gespeicherten Inhalte nutzen können, indem Einblicke in die Daten bereitgestellt werden. Profitieren Sie von den Vorteilen der Analysefunktionen von Excel, erstellen Sie Tabellen und Diagramme und extrahieren Sie optisch ansprechende Diagramme, und das alles von Ihrer App aus. Weitere Informationen finden Sie unter [Arbeiten mit Excel in Microsoft Graph](http://graph.microsoft.io/en-us/docs/api-reference/beta/resources/excel).|

### <a name="general"></a>Allgemein

|**Änderungstyp**|**Version**|**Beschreibung**|
|:--------------|:-----------|:--------------|
|Ergänzungen|v1.0 und Beta|Verbesserung von Fehlermeldungen beim Auflösen von Mandantenalias- und abgelehnten JWT-Token (AAD).|
|Ergänzungen|v1.0 und Beta|Der Ort für den Autorisierungsdienstendpunkt wird jetzt in dem _www-authenticate_-Header zurückgegeben, wenn eine Anforderung mit einem leeren Bearertoken empfangen wird.|
|Ergänzungen|v1.0 und Beta|Die Funktion zum Filtern nach ID-Eigenschaft einer Entität wurde behoben. Beispiel: GET https://graph.microsoft.com/v1.0/users?$filter=id+eq+'x'<br/>Bisher musste bei allen POST-Anforderungen für Dienstaktionen und Funktionen der Aktion oder dem Funktionsnamen ein Präfix mithilfe von microsoft.graph vorangestellt werden. Beispiel: POST https://graph.microsoft.com/v1.0/me/Microsoft.Graph.getMemberGroups<br/>Das Präfix ist nun nicht mehr erforderlich (obwohl es immer noch angegeben werden kann). Folgendes würde jetzt daher funktionieren: POST https://graph.microsoft.com/v1.0/me/getMemberGroups|
|Ändern|Beta|Abonnement-Eigenschaftennamen bereinigt.|
|Ergänzungen|Beta|Wir haben die Möglichkeit zum Ermitteln von (über _directorySettingTemplates_) und Außerkraftsetzen des Standardverhaltens (durch Erstellen einer _Einstellung_ aus der Vorlage) für Entitäten und die zugehörigen Funktionen hinzugefügt. Anfangs konnten die Verhaltensweisen für Office-Gruppen nur über die Vorlage gesteuert werden.|

### <a name="mail-folder"></a>Mail-Ordner

|**Änderungstyp**|**Version**|**Beschreibung**|
|:--------------|:-----------|:--------------|
|Ergänzungen|Beta|_wellKnownName_- und _userConfigurations_-Eigenschaften hinzugefügt.|
|Ergänzungen|Beta|_singleValueExtendedProperties_- und _multiValueExtendedProperties_-Eigenschaften hinzugefügt|

### <a name="messages"></a>Nachrichten

|**Änderungstyp**|**Version**|**Beschreibung**|
|:--------------|:-----------|:--------------|
|Ergänzungen|v1.0|_mobilePhone_-Eigenschaft hinzugefügt.|
|Ergänzungen|v1.0 und Beta|_internetMessageId_-Eigenschaft hinzugefügt. Die Nachrichten-ID im von [RFC2822](http://www.ietf.org/rfc/rfc2822.txt) angegebenen Format.|
|Ändern|Beta|_mobilePhone1_-Eigenschaft in _mobilePhone_ umgenannt.|
|Änderung|Beta|_createReply_ und _createReplyAll _ verwenden neue _Message_- und _comment_-Parameter.|
|Änderung|Beta|_createForward _verwendet neue _Message _, _ToRecipients_ und _comment_-Parameter.|
|Änderung|Beta|_reply_, _replyAll_und _forward_ verwenden neue _Message_-Paramater.|

### <a name="permission"></a>Berechtigung

|**Änderungstyp**|**Version**|**Beschreibung**|
|:--------------|:-----------|:--------------|
|Ergänzungen|v1.0 und Beta|_sharingInvitation_-Eigenschaft hinzugefügt, um Details zu verknüpften Einladungen zur Freigabe für diese Berechtigung bereitzustellen.|

### <a name="person"></a>Person

|**Änderungstyp**|**Version**|**Beschreibung**|
|:--------------|:-----------|:--------------|
|Ergänzungen|Beta|- Neue _birthday_-, _personNotes_-, _isFavorite_-, _phones_-, _permission_-, _postalAddresses_-,_websites_-,_yomiCompany_-, _department_-, _profession_-, _mailboxType_- und _personType_-Eigenschaften hinzugefügt.|
|Ergänzungen|Beta|Neue Enumerationstypen hinzugefügt: _physicalAddressType_, _webSite _, _phone_ und _webSiteType_.|

### <a name="reference-attachment"></a>Verweisanlage

|**Änderungstyp**|**Version**|**Beschreibung**|
|:--------------|:-----------|:--------------|
|Ergänzungen|Beta|Neue Eigenschaften hinzugefügt: _sourceUrl_, _providerType_, _thumbnailUrl_, _previewUrl_, _permission_ und _isFolder_.|
|Ergänzungen|Beta|_singleValueExtendedProperties_ und _multiValueExtendedProperties_-Eigenschaften hinzugefügt.|
|Ergänzungen|Beta|Neue enum types _referenceAttachmentProvider _und _referenceAttachmentPermission_ hinzugefügt.|

### <a name="subscriptions"></a>Abonnements

|**Änderungstyp**|**Endpunkt**|**Beschreibung**|
|:--------------|:-----------|:--------------|
|Ergänzungen|v1.0|Webhooks sind jetzt allgemein für Version 1.0 von Endpunkt über die _/Subscriptions_-Ressource verfügbar. Erstellen, Lesen, Aktualisieren und Löschen Sie Abonnements, um Benachrichtigungen zu Daten aus Outlook und Office 365-Gruppenunterhaltungen zu erhalten.|

### <a name="user"></a>Benutzer

|**Änderungstyp**|**Version**|**Beschreibung**|
|:--------------|:-----------|:--------------|
|Ergänzungen|Beta|_mailboxSettings_-Eigenschaft sowie die entsprechenden Typen hinzugefügt.|

## <a name="february-2016"></a>Februar 2016

### <a name="driveitem"></a>DriveItem

|**Änderungstyp**|**Version**|**Beschreibung**|
|:--------------|:-----------|:--------------|
|Ergänzungen|v1.0 und Beta|Neue _remoteItem_-Eigenschaft für driveItem für Microsoft-Konten.|

### <a name="general"></a>Allgemein

|**Änderungstyp**|**Version**|**Beschreibung**|
|:--------------|:-----------|:--------------|
|Änderung|v1.0 und Beta|-_/me/drive_ funktioniert nun für Microsoft-Konten und Geschäfts-, Schul- und Uni-Konten.|
|Änderung|v1.0 und Beta|Laufwerkanforderungen für Konten, deren OneDrive-Speicher bei Bedarf bereitgestellt wurde, funktionieren zuverlässig und können in mehr Szenarien verwendet werden, in denen die SharePoint-Standardwebsites des Mandanten nicht standardmäßige Namen verwenden.|
|Löschung|Beta|Verschiedene nicht implementierte Typen aus dem Betaschema zur Anpassung an 1.0 Schema entfernt.|

### <a name="subscriptions"></a>Abonnements

|**Änderungstyp**|**Version**|**Beschreibung**|
|:--------------|:-----------|:--------------|
|Ergänzungen|Beta|notificationUrl-Validierung bei Abonnement-Erstellung. Weitere Informationen finden Sie unter [Microsoft Graph WebHooks Update - January 2016](http://dev.office.com/blogs/Microsoft-Graph-WebHooks-Update-January-2016).|
|Ergänzungen|Beta|Abonnemententitäten können jetzt gelöscht werden: LÖSCHEN https://graph.microsoft.com/beta/subscriptions/|

### <a name="users"></a>Benutzer

|**Änderungstyp**|**Version**|**Beschreibung**|
|:--------------|:-----------|:--------------|
|Änderung|v1.0 und Beta|_displayName_ wird jetzt für Microsoft-Konten zurückgegeben.|

## <a name="january-2016"></a>Januar 2016

### <a name="contacts"></a>Kontakte

|**Änderungstyp**|**Version**|**Beschreibung**|
|:--------------|:-----------|:--------------|
|Ergänzungen|v1.0|mobilePhone-Eigenschaft zum Entitätssatz für persönliche Kontakte hinzugefügt.|

### <a name="directoryobjects"></a>directoryObjects

|**Änderungstyp**|**Version**|**Beschreibung**|
|:--------------|:-----------|:--------------|
|Behebung|v1.0 und Beta|Behebung eines Problems mit Aufrufaktionen, die mit directoryObjects verknüpft sind und zu dem folgenden Fehler führten: „Der Rückgabetyp des Vorgangs ist mit dem vorgegebenen Entitätssatz nicht möglich“. Dies gilt für die folgenden Aktionen: _microsoft.graph.checkMemberObjects_, _microsoft.graph.getMemberObjects_, _microsoft.graph.checkMemberGroups_, _microsoft.graph.assignLicense_, _microsoft.graph.changePassword_.|

## <a name="december-2015"></a>Dezember 2015

### <a name="contacts"></a>Kontakte

|**Änderungstyp**|**Version**|**Beschreibung**|
|:--------------|:-----------|:--------------|
|Ergänzungen|Beta|mobilePhone-Eigenschaft zum Entitätssatz für persönliche Kontakte hinzugefügt.|

### <a name="general"></a>Allgemein

|**Änderungstyp**|**Version**|**Beschreibung**|
|:--------------|:-----------|:--------------|
|Behebung|v1.0 und Beta|Behebung eines Problems mit Anforderungen mit $filter-Ausdrücken, die mehrmals die gleiche Eigenschaft angegebenen haben, was zu dem folgenden 500-Fehler führte: „Ein Element mit diesem Schlüssel wurde bereits hinzugefügt“.|
|Behebung|v1.0 und Beta|Behebung eines Problems mit der Groß-/Kleinschreibung für Aktionsparameternamen und -werte.|
|Behebung|v1.0 und Beta|Behebung eines Problems mit der Verarbeitung von Anforderungen für Nutzlasten mit Nullwerten für einige eingebettete komplexe Eigenschaften, aufgrund dessen ein Fehler mit einer Nullverweisausnahme auftrat.|
|Ergänzungen|v1.0 und Beta|Unterstützung für das Sortieren und Filtern von komplexen Type-Eigenschaften.|
|Ergänzungen|v1.0 und Beta|authorization_uri-Eigenschaft in dem www-authenticate-Header einer 401-Antwort hinzugefügt. Dieser URI kann verwendet werden, um den Tokenerwerbfluss zu starten.|
|Ergänzungen|v1.0 und Beta|Verbesserte Fehlermeldungen für Benutzer und Gruppen.|

### <a name="groups"></a>Gruppen

|**Änderungstyp**|**Version**|**Beschreibung**|
|:--------------|:-----------|:--------------|
|Behebung|v1.0 und Beta|Behebung eines Problems mit dem Aufruf folgender Gruppenaktionen: _microsoft.graph.addFavorite_, _microsoft.graph.removeFavorite_ and _microsoft.graph.resetUnseenCount_.|

### <a name="messages"></a>Nachrichten

|**Änderungstyp**|**Version**|**Beschreibung**|
|:--------------|:-----------|:--------------|
|Ergänzungen|Beta|eventMessageRequest-Untertyp der eventMessage- und startDateTime-, endDateTime-, location-, type-, recurrence- und IsOutOfDate-Eigenschaften zum eventMessag-Typ hinzugefügt.|

### <a name="users"></a>Benutzer

|**Änderungstyp**|**Version**|**Beschreibung**|
|:--------------|:-----------|:--------------|
|Behebung|v1.0 und Beta|Behebung eines Problems, aufgrund dessen bestimmte Benutzereigenschaften von anderen Benutzern nicht ausgewählt werden konnten, wenn auf den Benutzer mithilfe des Benutzerprinzipalnamens (UPN) verwiesen wurde. Beispiel: https://graph.microsoft.com/v1.0/users/anotherUser@contoso.com?$select=aboutMe|
|Behebung|v1.0 und Beta|Behebung eines Problems mit dem Aufruf der benutzergebundenen _microsoft.graph.reminderView_-Funktion, aufgrund dessen der folgende Fehler auftrat: „Eine Eigenschaft mit dem Namen 'BusinessPhones' vom Typ 'Microsoft.OutlookServices.Reminder' konnte nicht gefunden werden.“|
|Behebung|v1.0 und Beta|Behebung eines Problems mit der Erstellung und Aktualisierung von Benutzern (POST/PATCH /v1.0/users), aufgrund dessen der 400-Fehler auftrat.|
