# <a name="environment-class"></a>Classe Environment







Questa classe contiene informazioni di contesto sull'ambiente che ospita il framework e i suoi componenti.



## <a name="properties"></a>Proprietà

| Proprietà     | Modificatore di accesso | Tipo | Descrizione|
|:-------------|:----|:-------|:-----------|
|`applicationSessionId`     | `public` | [`Guid`](../sp-core-library/guid.md) | _Sola lettura._ Un identificatore univoco utilizzato per correlare la registrazione e altre informazioni di diagnostica. Dura quanto l'istanza dell'applicazione sul lato client, ovvero inizia con la richiesta del server che esegue il rendering della pagina e termina, ad esempio, quando la scheda del browser viene chiusa oppure quando viene premuto F5 per ricaricare la pagina. Si noti che se il router dell'applicazione supporta la navigazione sul posto (tramite l'API history.pushState()), la sessione dell'applicazione viene mantenuta in queste transizioni. |
|`pageSessionId`     | `public` | [`Guid`](../sp-core-library/guid.md) | _Sola lettura._ Un identificatore univoco utilizzato per correlare la registrazione e altre informazioni di diagnostica. Mentre applicationSessionId tiene traccia dell'intera durata dell'istanza dell'applicazione sul lato client, pageSessionId tiene traccia di una singola "pagina" di cui viene eseguito il rendering. Si supponga, ad esempio, che l'applicazione inizialmente carichi PageA, che l'utente esegua una navigazione sul posto (mediante l'API history.pushState()) per PageB, che quindi torni a PageA per chiudere, infine, la scheda del browser. Durante questa sequenza, applicationSessionId rimarrà lo stesso, anche se pageSessionId cambierà per ogni navigazione. I 3 valori di pageSessionId vengono utilizzati dalla diagnostica, ad esempio, per tenere traccia delle statistiche delle operazioni con esito positivo/negativo per PageA indipendentemente da PageB. Il concetto di "pagina" è definito dall'applicazione. |
|`type`     | `public` | [`EnvironmentType`](../sp-core-library/environmenttype.md) | _Sola lettura._ Un'enumerazione che descrive in quale tipo di ambiente viene eseguito il framework. |







