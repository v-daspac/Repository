# <a name="iwebpartdata-interface"></a>Interfaccia IWebPartData







Questa struttura rappresenta lo stato serializzato di una web part. Quando l'API serialize() viene chiamata su una web part, l'output dovrebbe avere questa struttura. La struttura del campo "properties" è di proprietà della web part ed è specifico per la web part. Ogni web part può decidere il set di proprietà da serializzare.




## <a name="properties"></a>Proprietà

| Proprietà     | Tipo   | Descrizione|
|:-------------|:-------|:-----------|
|`description`      | `string` | Definizione: descrizione web part. Utilizzo: visualizzare la descrizione della web part. Obbligatoria: nessuna Tipo: stringa Valori supportati: stringa con descrizione. Esempio: "Text" |
|`id`      | `string` | Definizione: ID Type web part universalmente univoco. Utilizzo: identificare in modo univoco una web part. Obbligatoria: sì Tipo: GUID Valori supportati: qualsiasi GUID Esempio: "dbef608d-3ad5-4f8f-b139-d916f2f0a294" |
|`instanceId`      | `string` | Definizione: ID istanza universalmente univoco della web part. Una web part può avere più istanze in una pagina. Questo id dovrebbe essere univoco oltre i limiti di tempo e di pagina. Utilizzo: utilizzato da framework per identificare un'istanza di una web part in modo univoco. Obbligatorio: sì Tipo: valori stringa supportati: una stringa univoca. Potrebbe essere GUID o altro formati identificabile in modo univoco. Esempio: ["dbef608d-3ad5-4f8f-b139-d916f2f0a294"] sperimentale: sì |
|`title`      | `string` | Definizione: titolo della web part. Utilizzo: visualizzare il nome della web part nella casella degli strumenti, nella raccolta di web part e nella pagina. Obbligatoria: sì Tipo: stringa Valori supportati: stringa con meno di 100 caratteri Esempio: "Text" |






