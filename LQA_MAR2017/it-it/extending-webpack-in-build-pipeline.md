# <a name="extending-webpack-in-the-sharepoint-framework-toolchain"></a>Estendere webpack nella toolchain di SharePoint Framework

>**Nota:** al momento, SharePoint Framework è in versione di anteprima e potrebbe essere soggetto a modifiche. Le web part sul lato client di SharePoint Framework attualmente non sono supportate in ambienti di produzione.

[Webpack](https://webpack.github.io/) è un bundler di modulo JavaScript che utilizza file e dipendenze JavaScript e genera uno o più pacchetti di JavaScript per poter caricare diversi bundle per diversi scenari.

La toolchain del framework utilizza CommonJS per la creazione di bundle. In questo modo è possibile definire moduli e dove si desidera utilizzarli. La toolchain utilizza anche SystemJS, un caricatore del modulo universale, per caricare i moduli. In questo modo è possibile definire l'ambito delle web part assicurandosi che ogni web part venga eseguita nel proprio spazio dei nomi.

Un’attività che comunemente si desidera aggiungere alla toolchain di SharePoint Framework consiste nell'estendere la configurazione di webpack con programmi di caricamento e plug-in personalizzati.

## <a name="using-webpack-loaders"></a>Utilizzo di programmi di caricamento per webpack
In molti casi si può desiderare di importare e utilizzare una risorsa non JavaScript durante lo sviluppo, in genere con immagini o modelli. Un [programma di caricamento di webpack](https://webpack.github.io/docs/loaders.html) consentirà di convertire la risorsa in un oggetto da utilizzare dall'applicazione JavaScript. Ad esempio, un modello markdown potrebbe essere compilato e convertito in stringa di testo, mentre la risorsa di un’immagine potrebbe essere convertita in immagine Base64.

Esistono diversi programmi di caricamento utili, alcuni dei quali sono già utilizzati per la configurazione di webpack per SharePoint Framework standard, ad esempio:

- html-loader
- json-loader
- loader-load-themed-styles

Estendere la configurazione di webpack per framework con programmi di caricamento personalizzati è un'operazione semplice riportata [di seguito nella documentazione webpack](https://webpack.github.io/docs/loaders.html#writing-a-loader).

> Ulteriori dettagli sui programmi di caricamento sono disponibili nella [documentazione relativa ai programmi di caricamento di webpack](https://webpack.github.io/docs/loaders.html)

## <a name="example-using-the-markdown-loader-package"></a>Esempio: Utilizzo del pacchetto markdown-loader
Ad esempio, è possibile utilizzare il [pacchetto markdown-loader](https://www.npmjs.com/package/markdown-loader).  Si tratta di un programma di caricamento che consente di fare riferimento a un file `.md` e di visualizzarlo come stringa HTML.

È possibile scaricare l'esempio completo [qui](https://aka.ms/spfx-extend-webpack-sample).

### <a name="step-1---install-the-package"></a>Passaggio 1 - Installare il pacchetto
Si farà riferimento a un markdown-loader del progetto.

```
npm i --save markdown-loader 
```

### <a name="step-2---configure-webpack"></a>Passaggio 2 - Configurare webpack 
Dopo aver installato il pacchetto, è possibile configurare la configurazione webpack per SharePoint Framework affinché includa il markdown-loader. 

Nella [documentazione relativa al markdown-loader](https://github.com/peerigon/markdown-loader), viene illustrato come estendere la configurazione webpack affinché includa il programma di caricamento:

```JavaScript
{
  module: {
    loaders: [
      { test: /\.md$/, loader: "html!markdown" }
    ]
  }
}
```

Si utilizzeranno queste informazioni per configurarlo nel progetto. 

Per aggiungere questo programma di caricamento personalizzato nella configurazione webpack di SharePoint Framework, sarà necessario indicare all'attività di compilazione di configurare il webpack. Le attività di compilazione sono definite nel file di gulp - `gulpfile.js` - che si trova alla radice del progetto. SharePoint Framework utilizza [gulp](http://gulpjs.com/) come strumento di esecuzione attività, pertanto si usa per definire e registrare attività personalizzate con lo strumento di esecuzione attività gulp.

Modificare `gulpfile.js` e aggiungere il codice seguente subito prima di `build.initialize(gulp);`:

```JavaScript 
build.configureWebpack.mergeConfig({ 
  additionalConfiguration: (generatedConfiguration) => { 
    generatedConfiguration.module.loaders.push([ 
      { test: /\.md$/, loader: "html!markdown" } 
    ]); 

    return generatedConfiguration; 
  } 
});
```

Si noti che l’operazione consiste semplicemente nell’inserire la configurazione del programma di caricamento nell’elenco dei programmi di caricamento esistenti nella toolchain. È importante verificare che la funzione `additionalConfiguration` termini con la riga `return generatedConfiguration`, poiché in questo modo viene restituita la configurazione di caricamento per la toolchain. 

> Sebbene sia possibile sostituire completamente la configurazione webpack predefinita della toolchain con questo approccio per ottenere il massimo vantaggio con prestazioni e ottimizzazione, non è consigliabile farlo se non viene diversamente specificato nella documentazione. 

### <a name="step-3---update-your-code"></a>Passaggio 3 - Aggiornare il codice
Dopo aver configurato il programma di caricamento, è possibile aggiornare il codice e aggiungere alcuni file per verificare lo scenario. 

Creare un file `my-markdown.md` nella directory `src` della cartella del progetto contenente testo relativo al markdown.

```md
#Hello Markdown

*Markdown* is a simple markup format to write content. 

You can also format text as **bold** or *italics* or ***bold italics***  
```

Quando si compila il progetto, il programma di caricamento webpack di markdown convertirà il testo relativo al markdown in una stringa HTML. Per utilizzare questa stringa HTML in uno dei file `*.ts` di origine, aggiungere la seguente riga `require()` nella parte superiore del file dopo gli oggetti importati, ad esempio:


```TypeScript
const strMarkdownString = require("../../../../src/readme.md") as string;
```

Per impostazione predefinita, l’oggetto webpack cercherà il file nella cartella `lib`, ma, per impostazione predefinita, i file `.md` non vengono copiati nella cartella `lib`, perciò è necessario creare un percorso relativo piuttosto lungo. Possiamo controllare questa impostazione definendo un file di configurazione per indicare alla toolchain di copiare file `md` nella cartella lib. 

Creare un file `copy-static-assets.json` nella directory `config` per indicare al sistema di compilazione di copiare alcuni file aggiuntivi da `src` a `lib`. Per impostazione predefinita, questa attività di compilazione consente di copiare i file con estensioni riconosciuti dalla configurazione webpack della toolchain predefinita (ad esempio `png` e `json`), pertanto occorre semplicemente indicare di copiare anche i file `md`.

```JSON
{
  "includeExtensions": [
    "md"
  ]
}
```

Anziché utilizzare il percorso relativo, è possibile utilizzare il percorso del file nella dichiarazione `require`, ad esempio:

```TypeScript
const strMarkdownString = require("../../readme.md") as string;
```
 
È quindi possibile fare riferimento a questa stringa del codice, ad esempio:

``` TypeScript
public render(): void {
  this.domElement.innerHTML = strMarkdownString;
}
```

### <a name="step-4---build-and-test-your-code"></a>Passaggio 4 - Creare e testare il codice
Per creare e testare il codice, eseguire il seguente comando in una console nella radice della directory del progetto:

```
gulp serve
```