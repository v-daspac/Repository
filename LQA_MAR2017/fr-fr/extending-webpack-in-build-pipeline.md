# <a name="extending-webpack-in-the-sharepoint-framework-toolchain"></a>Extension de webpack dans la chaîne d’outils de l’infrastructure SharePoint

>**Remarque :** l’infrastructure SharePoint est encore en phase préliminaire et est sujette à modifications. Les composants WebPart côté client de l’infrastructure SharePoint ne sont actuellement pas pris en charge dans les environnements de production.

[webpack](https://webpack.github.io/) est un programme de regroupement de modules JavaScript qui prend vos fichiers JavaScript et leurs dépendances et génère un ou plusieurs fichiers groupés JavaScript afin que vous puissiez charger des fichiers différents selon les cas de figure.

La chaîne d’outils de l’infrastructure utilise CommonJS pour le regroupement. Cela vous permet de définir les modules et l’endroit où vous souhaitez les utiliser. La chaîne d’outils utilise également SystemJS, un chargeur de modules universel, pour charger vos modules. Cela vous aide à définir l’étendue d’application de votre composant WebPart en faisant en sorte que chaque composant WebPart soit exécuté dans son propre espace de noms.

Il est également très utile d’ajouter à l’infrastructure SharePoint une tâche permettant d’étendre la configuration de webpack avec des plug-ins et des modules de chargement personnalisés.

## <a name="using-webpack-loaders"></a>Utilisation de chargeurs webpack
Il existe de nombreux cas où vous devez importer et utiliser des ressources non JavaScript lors du développement, généralement des images ou des modèles. Un [chargeur webpack](https://webpack.github.io/docs/loaders.html) convertira la ressource en un élément pouvant être utilisé par votre application JavaScript. Par exemple, un modèle Markdown peut être compilé et converti en une chaîne de texte, tandis qu’une image peut être convertie en base64.

Il existe un certain nombre de chargeurs utiles, dont plusieurs sont déjà utilisés par la configuration webpack standard de l’infrastructure SharePoint, par exemple :

- html-loader
- json-loader
- loader-load-themed-styles

L’extension de la configuration de webpack dans l’infrastructure à l’aide de chargeurs personnalisés est un processus simple qui est décrit [dans la documentation webpack](https://webpack.github.io/docs/loaders.html#writing-a-loader).

> Vous pouvez obtenir plus d’informations sur les chargeurs dans la [documentation sur les chargeurs webpack](https://webpack.github.io/docs/loaders.html).

## <a name="example-using-the-markdown-loader-package"></a>Exemple : Utilisation du package markdown-loader
En guise d’exemple, nous allons utiliser le [chargeur markdown-loader](https://www.npmjs.com/package/markdown-loader).  Ce chargeur permet de spécifier un fichier `.md` et de le lire sous forme de chaîne au format HTML.

Vous pouvez télécharger l’exemple complet [ici](https://aka.ms/spfx-extend-webpack-sample).

### <a name="step-1---install-the-package"></a>Étape 1 : installer le package
Nous allons spécifier le chargeur markdown-loader dans notre projet.

```
npm i --save markdown-loader 
```

### <a name="step-2---configure-webpack"></a>Étape 2 : configurer webpack 
Maintenant que nous avons installé le package, nous allons configurer webpack dans l’infrastructure SharePoint afin d’inclure le chargeur markdown-loader. 

La [documentation sur markdown-loader](https://github.com/peerigon/markdown-loader) montre comment étendre la configuration webpack pour inclure le chargeur :

```JavaScript
{
  module: {
    loaders: [
      { test: /\.md$/, loader: "html!markdown" }
    ]
  }
}
```

Nous utiliserons ces informations pour le configurer dans notre projet. 

Pour ajouter ce chargeur personnalisé à la configuration de webpack dans l’infrastructure SharePoint, nous devons indiquer à la tâche de génération de configurer webpack. Les tâches de génération sont définies dans le fichier gulp (`gulpfile.js`), qui se trouve à la racine de votre projet. Étant donné que l’infrastructure SharePoint utilise [gulp](http://gulpjs.com/) en tant que programme d’exécution de tâches, c’est cet outil que nous utilisons pour définir et enregistrer des tâches personnalisées.

Modifiez la valeur `gulpfile.js` et ajoutez le code suivant juste avant `build.initialize(gulp);` :

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

Notez que nous envoyons simplement la configuration du chargeur dans la liste des chargeurs existants dans la chaîne d’outils. Il est important de s’assurer que votre fonction `additionalConfiguration` se termine par la ligne `return generatedConfiguration`, car cela garantit qu’elle renvoie la configuration du chargeur à la chaîne d’outils. 

> Bien que vous puissiez remplacer complètement la configuration de webpack par défaut de la chaîne d’outils à l’aide de cette méthode, elle n’est pas recommandée si vous souhaitez bénéficier de performances et d’une fonctionnalité optimales, sauf indication contraire dans la documentation. 

### <a name="step-3---update-your-code"></a>Étape 3 : mettre à jour votre code
Maintenant que nous avons configuré le chargeur, nous allons mettre à jour notre code et ajouter quelques fichiers pour tester le scénario. 

Créez un fichier `my-markdown.md` dans le répertoire `src` de votre dossier de projet, en y incluant du texte Markdown.

```md
#Hello Markdown

*Markdown* is a simple markup format to write content. 

You can also format text as **bold** or *italics* or ***bold italics***  
```

Lorsque vous générez le projet, le chargeur webpack markdown-loader convertit ce texte en une chaîne au format HTML. Pour utiliser cette chaîne HTML dans vos fichiers `*.ts` source, ajoutez la ligne `require()` suivante en haut du fichier, après vos instructions import, par exemple :


```TypeScript
const strMarkdownString = require("../../../../src/readme.md") as string;
```

Par défaut, webpack cherche le fichier dans le dossier `lib`, mais les fichiers `.md` ne sont pas copiés dans ce même dossier `lib`, ce qui signifie que nous devons créer un chemin d’accès relatif assez long. Nous pouvons modifier ce paramètre en définissant un fichier de configuration qui indique à la chaîne d’outils de copier les fichiers `md` vers le dossier lib. 

Créez un fichier `copy-static-assets.json` dans le répertoire `config` pour indiquer au système de génération de copier des fichiers supplémentaires de `src` vers `lib`. Par défaut, cette tâche de génération copie les fichiers dont les extensions sont comprises par la configuration de webpack par défaut de la chaîne d’outils (telles que `png` et `json`), de sorte que nous devons simplement lui demander de copier aussi les fichiers `md`.

```JSON
{
  "includeExtensions": [
    "md"
  ]
}
```

Au lieu d’utiliser le chemin d’accès relatif, vous pouvez désormais utiliser le chemin d’accès du fichier dans votre instruction `require`, par exemple :

```TypeScript
const strMarkdownString = require("../../readme.md") as string;
```
 
Vous pouvez ensuite spécifier cette chaîne dans votre code, par exemple :

``` TypeScript
public render(): void {
  this.domElement.innerHTML = strMarkdownString;
}
```

### <a name="step-4---build-and-test-your-code"></a>Étape 4 : générer et tester votre code
Pour générer et tester votre code, exécutez la commande suivante dans une console à la racine du répertoire de votre projet :

```
gulp serve
```