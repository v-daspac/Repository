# <a name="integrate-gulp-tasks-in-sharepoint-framework-toolchain"></a>Integrare le attività di gulp nella toolchain di SharePoint Framework

>**Nota:** al momento, SharePoint Framework è in versione di anteprima e potrebbe essere soggetto a modifiche. Le web part sul lato client di SharePoint Framework attualmente non sono supportate in ambienti di produzione.

Gli strumenti di sviluppo lato client di SharePoint utilizzano [gulp](http://gulpjs.com/) come lo strumento di esecuzione attività del processo di creazione per:

* Aggregare e minimizzare file JavaScript e CSS.
* Eseguire gli strumenti per chiamare le attività di aggregazione e minimizzazione prima di ogni compilazione.
* Compilare i file SASS in CSS.
* Compilare i file TypeScript in JavaScript.

L'integrazione delle attività di gulp nella pipeline di compilazione rappresenta una delle attività comuni che l'utente potrebbe voler aggiungere alla toolchain di SharePoint Framework.

## <a name="gulp-tasks"></a>Attività di gulp
In genere, le attività di gulp vengono definite in `gulpfile.js` come:

```js
gulp.task('somename', function() {
  // Do stuff
});
```

Quando si utilizza la toolchain di SharePoint Framework, è necessario definire le attività nella pipeline di compilazione del framework. Una volta definita e registrata nella pipeline, l'attività verrà aggiunta alla toolchain.

SharePoint Framework utilizza una [toolchain di compilazione comune](sharepoint-framework-toolchain.md#common-build-tools-packages), vale a dire, un set di pacchetti npm che condividono attività di compilazione comuni. Di conseguenza, le attività preimpostate vengono definite nel pacchetto comune come opposte rispetto al `gulpfile.js` del progetto sul lato client. Per visualizzare le attività disponibili, è possibile eseguire il comando seguente in una console disponibile all'interno della directory di progetto:

```
gulp --tasks
```

Il comando riportato in precedenza conterrà un elenco di tutte le attività disponibili.

![Attività di gulp disponibili](../../images/gulp-tasks-available.png)

## <a name="custom-gulp-tasks"></a>Attività di gulp personalizzate
Per aggiungere le attività personalizzate, è necessario definirle in `gulpfile.js`. Aprire `gulpfile.js` nell'editor di codice. Il codice predefinito inizializza la toolchain di SharePoint Framework e l'istanza `gulp` globale per la toolchain. Tutte le attività personalizzate aggiunte devono essere definite prima di inizializzare l'istanza `gulp` globale.

### <a name="add-your-custom-task"></a>Aggiungere attività personalizzate
Per aggiungere un'attività di gulp personalizzata, è necessario aggiungere una nuova attività secondaria alla pipeline di compilazione di SharePoint Framework tramite la funzionalità [`build.subTask`](https://github.com/Microsoft/gulp-core-build#defining-a-custom-task):

```js
let helloWorldSubtask = build.subTask('log-hello-world-subtask', function(gulp, buildOptions, done) {
  this.log('Hello, World!');   
  // use functions from gulp task here  
});
```

Nel caso di un flusso, l'utente restituisce il flusso:

```js
let helloWorldSubtask = build.subTask('log-hello-world-subtask', function(gulp, buildOptions, done) {
  return gulp.src('images/*.png')
             .pipe(gulp.dest('public'));
});
```

### <a name="register-your-task-with-gulp-command-line"></a>Registrare l'attività con la riga di comando del gulp
Una volta definita l'attività personalizzata, è possibile registrarla con la riga di comando del gulp utilizzando la funzionalità `build.task`:

```js
// Register the task with gulp command line
let helloWorldTask = build.task('hello-world', helloWorldSubtask);
```

>Nota: Tutte le attività personalizzate aggiunte devono essere definite prima di inizializzare l'istanza `gulp` globale, vale a dire, prima della riga di codice seguente: `build.initialize(gulp);`

Ora è possibile eseguire il comando personalizzato nella riga di comando, così come riportato di seguito:

```js
gulp hello-world
```

>Nota: Non è possibile eseguire l'attività secondaria registrata utilizzando la funzionalità `build.subTask` direttamente dalla riga di comando. È possibile eseguire soltanto l'attività registrata utilizzando la funzionalità `build.task`:

### <a name="execute-your-custom-task-before-or-after-available-tasks"></a>Eseguire l'attività personalizzata prima o dopo le attività disponibili
È anche possibile aggiungere questa attività personalizzata in modo che sia eseguita prima o dopo determinate attività di gulp disponibili. L'attività di gulp seguente permette di inserire l'attività personalizzata prima o dopo l'attività:

- Attività di compilazione generica (include tutte le attività disponibili)
- Attività typescript

Le attività di SharePoint Framework sono disponibili negli strumenti di compilazione predefiniti. Gli strumenti di compilazione consistono in una serie di attività definite per uno scopo preciso. In questo caso, la compilazione di pacchetti lato client. È possibile accedere a questi strumenti predefiniti utilizzando l'oggetto `build.rig` e ottenere le funzionalità da usare prima e dopo l'attività:
 
```js
// execute after TypeScript task
build.rig.addPostTypescriptTask(helloWorldTask);

//execute before TypeScript task
build.rig.addBuildTasks(helloWorldTask);

//execute after all tasks
build.rig.addPostBuildTask(helloWorldTask);

//execute before all tasks
build.rig.addPreBuildTask(helloWorldTask);
```

## <a name="example-custom-image-resize-task"></a>Esempio: Attività di ridimensionamento di un'immagine personalizzata
Ad esempio, utilizzare l'[attività di gulp per il ridimensionamento di un'immagine](https://www.npmjs.com/package/gulp-image-resize).  Si tratta di un'attività semplice che consente di ridimensionare le immagini.

È possibile scaricare l'esempio completo [qui](https://aka.ms/spfx-extend-gulp-sample).

Nella [documentazione relativa all'attività di ridimensionamento delle immagini](https://www.npmjs.com/package/gulp-image-resize#example), viene mostrato come usare questa attività personalizzata:

```js
var gulp = require('gulp');
var imageResize = require('gulp-image-resize');
 
gulp.task('default', function () {
  gulp.src('test.png')
    .pipe(imageResize({
      width : 100,
      height : 100,
      crop : true,
      upscale : false
    }))
    .pipe(gulp.dest('dist'));
});
```

Queste informazioni vengono usate per aggiungere l'attività nel progetto utilizzando le funzionalità `build.subTask` e `build.task`:

```js
var imageResize = require('gulp-image-resize');

let imageResizeSubTask = build.subTask('image-resize-subtask', function(gulp, buildOptions, done){
    return gulp.src('images/*.jpg')
               .pipe(imageResize({
                   width: 100,
                   height: 100,
                   crop: false                   
               }))
               .pipe(gulp.dest(path.join(buildOptions.libFolder, 'images')))
});

let imageResizeTask = build.task('resize-images', imageResizeSubTask);
```

Dal momento che si sta definendo un flusso, il flusso della funzionalità `build.subTask` viene restituito nella pipeline di compilazione. In seguito, la pipeline di compilazione esegue in modo asincrono questo flusso di gulp. 

A questo punto, è possibile eseguire l'attività dalla riga di comando di gulp nel modo seguente:

```js
gulp resize-images
```

![image-resize-task](../../images/gulp-extend-image-resize-task.png)

L'attività `resize-images` verrà visualizzata nell'elenco di attività disponibili per il progetto quando si esegue `gulp --tasks`:

![image-resize-task con attività disponibili](../../images/gulp-extend-image-resize-available-tasks.png)




