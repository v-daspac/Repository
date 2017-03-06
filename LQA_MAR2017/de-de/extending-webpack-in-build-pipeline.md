# <a name="extending-webpack-in-the-sharepoint-framework-toolchain"></a>Erweitern von Webpack in der SharePoint Framework-Toolkette

>**Hinweis:** Das SharePoint Framework befindet sich derzeit in der Vorschauphase. Änderungen sind vorbehalten. Die Verwendung von clientseitigen SharePoint Framework-Webparts in Produktionsumgebungen wird derzeit nicht unterstützt.

[Webpack](https://webpack.github.io/) ist ein JavaScript-Modulbundler, der aus Ihren JavaScript-Dateien und deren Abhängigkeiten ein oder mehrere JavaScript-Bündel generiert, damit Sie unterschiedliche Bündel für verschiedene Szenarien laden können.

Die Framework-Toolkette verwendet CommonJS zum Bündeln. Auf diese Weise können Sie Module und Verwendungsmöglichkeiten definieren. Die Toolkette verwendet auch SystemJS, ein universelles Modulladeprogramm, um Ihre Module zu laden. Auf diese Weise können Sie den Bereich für Ihre Webparts festlegen, indem Sie sicherstellen, dass jedes Webpart in seinem eigenen Namespace ausgeführt wird.

Eine häufige Aufgabe, die Sie ggf. der SharePoint Framework-Toolchain hinzufügen möchten, ist die Erweiterung der Webpack-Konfiguration mit benutzerdefinierten Ladeprogrammen und Plug-Ins.

## <a name="using-webpack-loaders"></a>Verwenden von Webpack-Ladeprogrammen
Es kommt häufig vor, dass eine Ressource ohne JavaScript während der Entwicklung importiert und verwendet werden soll. Dies erfolgt in der Regel mit Bildern oder Vorlagen. [Webpack-Ladeprogramme](https://webpack.github.io/docs/loaders.html) konvertieren die Ressource so, dass sie von der JavaScript-Anwendung genutzt werden kann. So werden Markdown-Vorlagen z. B. in eine Textzeichenfolge kompiliert und konvertiert, während eine Abbildungsressource in ein Base64-Bild konvertiert werden kann.

Es gibt eine Reihe von nützlichen Ladeprogrammen, von denen einige bereits durch die standardmäßige SharePoint Framework Webpack-Konfiguration verwendet werden, z. B.:

- HTML-Ladeprogramm
- JSON-Ladeprogramm
- loader-load-themed-styles

Das Erweitern der Framework-Webpack-Konfiguration mit benutzerdefinierten Ladeprogrammen ist ein einfacher Prozess, der in der [Webpack-Dokumentation](https://webpack.github.io/docs/loaders.html#writing-a-loader) beschrieben ist.

> Weitere Details zu Ladeprogrammen finden Sie in der [Dokumentation zu Webpack-Ladeprogrammen](https://webpack.github.io/docs/loaders.html)

## <a name="example-using-the-markdown-loader-package"></a>Beispiel: Verwenden des Pakets mit dem Markdown-Ladeprogramm
Verwenden wir als Beispiel das Paket mit dem [Markdown-Ladeprogramm](https://www.npmjs.com/package/markdown-loader).  Dies ist ein Ladeprogramm zum Verweisen auf eine `.md`-Datei und zur Ausgabe dieser als HTML-Zeichenfolge.

Sie können das vollständige Beispiel [hier](https://aka.ms/spfx-extend-webpack-sample) herunterladen.

### <a name="step-1---install-the-package"></a>Schritt 1 – Installieren des Pakets
Wir möchten in unserem Projekt auf das Markdown-Ladeprogramm verweisen.

```
npm i --save markdown-loader 
```

### <a name="step-2---configure-webpack"></a>Schritt 2 – Konfigurieren von Webpack 
Nun, da wir das Paket installiert haben, können wir die SharePoint Framework Webpack-Konfiguration so konfigurieren, dass sie das Markdown-Ladeprogramm einschließt. 

In der [Dokumentation zum Markdown-Ladeprogramm](https://github.com/peerigon/markdown-loader) wird gezeigt, wie die Webpack-Konfiguration erweitert wird, sodass sie das Ladeprogramm einschließt:

```JavaScript
{
  module: {
    loaders: [
      { test: /\.md$/, loader: "html!markdown" }
    ]
  }
}
```

Wir verwenden diese Informationen für die Konfiguration in unserem Projekt. 

Um dieses benutzerdefinierte Ladeprogramm zur SharePoint Framework-Webpack-Konfiguration hinzuzufügen, muss die Buildaufgabe zur Konfiguration von Webpack angewiesen werden. Die Buildaufgaben werden in der Gulp-Datei `gulpfile.js` definiert, die sich im Stammverzeichnis des Projekts befindet. SharePoint Framework verwendet [gulp](http://gulpjs.com/) zur Ausführung von Aufgaben. Wir werden es daher zum Definieren und Registrieren benutzerdefinierter Aufgaben verwenden.

Bearbeiten Sie `gulpfile.js` und fügen Sie folgenden Code direkt vor `build.initialize(gulp);` ein:

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

Beachten Sie, dass die Ladeprogramm-Konfiguration einfach auf die Liste mit den vorhandenen Ladeprogrammen in der Toolkette gesetzt wird. Achten Sie darauf, dass die Funktion `additionalConfiguration` mit der Zeile `return generatedConfiguration` endet: So wird sichergestellt, dass die Konfiguration des Ladeprogramms an die Toolkette zurückgegeben wird. 

> Zwar können Sie mit diesem Ansatz die standardmäßige Webpack-Konfiguration der Toolkette vollständig ersetzen, doch wird dies (sofern nicht anders in der Dokumentation angegeben) nicht empfohlen, wenn Sie von maximaler Leistung und Optimierung profitieren möchten. 

### <a name="step-3---update-your-code"></a>Schritt 3 – Aktualisieren des Code
Nun, da wir das Ladeprogramm konfiguriert haben, können wir den Code aktualisieren und einige Dateien hinzufügen, um das Szenario zu testen. 

Erstellen Sie die Datei `my-markdown.md` im Verzeichnis `src` des Projektordners mit etwas Markdown-Text darin.

```md
#Hello Markdown

*Markdown* is a simple markup format to write content. 

You can also format text as **bold** or *italics* or ***bold italics***  
```

Wenn Sie das Projekt erstellen, konvertiert das Webpack-Markdown-Ladeprogramm den Markdown-Text in eine HTML-Zeichenfolge. Um diese HTML-Zeichenfolge in einer Ihrer `*.ts`-Quelldateien zu verwenden, fügen Sie die folgende Zeile `require()` nach dem Importieren oben in der Datei hinzu. Beispiel:


```TypeScript
const strMarkdownString = require("../../../../src/readme.md") as string;
```

Webpack sucht standardmäßig im `lib`-Ordner nach der Datei. `.md`-Dateien werden jedoch nicht standardmäßig in den `lib`-Ordner kopiert. Dies bedeutet, dass ein ziemlich langer relativer Pfad erstellt werden muss. Diese Einstellung lässt sich durch Definieren einer Config-Datei steuern, wodurch die Toolkette angewiesen wird, `md`-Dateien in den lib-Ordner zu kopieren. 

Erstellen Sie die Datei `copy-static-assets.json` im Verzeichnis `config`. So wird das Buildsystem angewiesen, einige zusätzliche Dateien von `src` zu `lib` zu kopieren. Von dieser Buildaufgabe werden Dateien mit Erweiterungen, die von der Webpack-Standardkonfiguration der Toolkette verstanden werden, standardmäßig kopiert (wie z. B. `png` und `json`). Sie muss also nur angewiesen werden, auch `md`-Dateien zu kopieren.

```JSON
{
  "includeExtensions": [
    "md"
  ]
}
```

Anstelle des relativen Pfads können Sie nun den Dateipfad in Ihrer `require`-Anweisung verwenden, z. B.:

```TypeScript
const strMarkdownString = require("../../readme.md") as string;
```
 
Dann können Sie in Ihrem Code auf diese Zeichenfolge verweisen, z. B.:

``` TypeScript
public render(): void {
  this.domElement.innerHTML = strMarkdownString;
}
```

### <a name="step-4---build-and-test-your-code"></a>Schritt 4: Erstellen und Testen des Codes
Führen Sie zum Erstellen und Testen des Codes den folgenden Befehl in einer Konsole im Stammverzeichnis des Projekts aus:

```
gulp serve
```