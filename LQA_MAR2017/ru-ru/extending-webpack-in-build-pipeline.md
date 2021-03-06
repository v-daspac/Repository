# <a name="extending-webpack-in-the-sharepoint-framework-toolchain"></a>Расширение возможностей webpack в цепочке инструментов SharePoint Framework

>**Примечание.** Платформа SharePoint Framework находится на этапе бета-тестирования и может меняться. Сейчас клиентские веб-части SharePoint Framework не поддерживаются в производственной среде.

Инструмент [webpack](https://webpack.github.io/) — это средство увязки модулей JavaScript в пакеты, которое создает один или несколько пакетов JavaScript из файлов и зависимостей JavaScript. Это позволяет загружать различные пакеты для разных сценариев.

Цепочка инструментов платформы использует CommonJS для увязки в пакеты. Это позволяет определять модули и их назначение. Цепочка инструментов также использует SystemJS, универсальный загрузчик модулей. Это позволяет определять области для веб-частей, так как каждая веб-часть выполняется в отдельном пространстве имен.

Одна из распространенных задач, добавляемых в цепочку инструментов SharePoint Framework, — расширение возможностей конфигурации webpack с помощью пользовательских загрузчиков и подключаемых модулей.

## <a name="using-webpack-loaders"></a>Использование загрузчиков webpack
Во время разработки часто требуется импортировать и использовать ресурсы, не относящиеся к JavaScript, например изображения или шаблоны. [Загрузчик webpack](https://webpack.github.io/docs/loaders.html) преобразует ресурс так, чтобы его можно было использовать в приложении JavaScript. Например, шаблон Markdown можно скомпилировать и преобразовать в текстовую строку, а ресурс изображения — в изображение с кодировкой Base64.

Существует ряд полезных загрузчиков, некоторые из которых уже используются в стандартной конфигурации webpack SharePoint Framework, например:

- html-loader;
- json-loader;
- loader-load-themed-styles.

Расширить конфигурацию webpack платформы, добавив пользовательские загрузчики, можно довольно просто. Этот процесс описан в [документации по webpack](https://webpack.github.io/docs/loaders.html#writing-a-loader).

> Дополнительные сведения о загрузчиках см. в [документации по загрузчикам webpack](https://webpack.github.io/docs/loaders.html).

## <a name="example-using-the-markdown-loader-package"></a>Пример: использование пакета markdown-loader
В качестве примера рассмотрим [пакет markdown-loader](https://www.npmjs.com/package/markdown-loader).  Это загрузчик, с помощью которого можно сослаться на файл `.md` и вывести его в качестве строки HTML.

Готовый пример можно скачать [здесь](https://aka.ms/spfx-extend-webpack-sample).

### <a name="step-1---install-the-package"></a>Этап 1. Установка пакета
Добавим в проект ссылку на пакет markdown-loader.

```
npm i --save markdown-loader 
```

### <a name="step-2---configure-webpack"></a>Этап 2. Настройка webpack 
Теперь, когда мы установили пакет, настроим конфигурацию webpack SharePoint Framework, добавив в нее пакет markdown-loader. 

В [документации по markdown-loader](https://github.com/peerigon/markdown-loader) показано, как расширить возможности конфигурации webpack, включив в нее загрузчик:

```JavaScript
{
  module: {
    loaders: [
      { test: /\.md$/, loader: "html!markdown" }
    ]
  }
}
```

Мы будем использовать эти сведения для настройки проекта. 

Чтобы добавить этот пользовательский загрузчик в конфигурацию webpack SharePoint Framework, необходимо поручить задаче сборки настройку webpack. Задачи сборки определяются в файле gulp (`gulpfile.js`), который находится в корневой папке проекта. SharePoint Framework использует [gulp](http://gulpjs.com/) для запуска задач, поэтому мы будем определять и регистрировать настраиваемые задачи с помощью этого средства.

Отредактируйте `gulpfile.js`, добавив следующий код перед строкой `build.initialize(gulp);`:

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

Обратите внимание, что мы просто добавляем конфигурацию загрузчика в список имеющихся загрузчиков в цепочке инструментов. Очень важно убедиться, что функция `additionalConfiguration` заканчивается строкой `return generatedConfiguration`. Это гарантирует, что функция возвращает конфигурацию загрузчика в цепочку инструментов. 

> С помощью этого подхода вы можете полностью заменить конфигурацию webpack по умолчанию в цепочке инструментов, но для обеспечения максимальной производительности и оптимизации не рекомендуем этого делать, если обратное не указано в документации. 

### <a name="step-3---update-your-code"></a>Этап 3. Обновление кода
Теперь, когда мы настроили загрузчик, давайте обновим код и добавим несколько файлов, чтобы протестировать сценарий. 

Создайте файл `my-markdown.md` с текстом Markdown в каталоге `src` папки проекта.

```md
#Hello Markdown

*Markdown* is a simple markup format to write content. 

You can also format text as **bold** or *italics* or ***bold italics***  
```

При сборке проекта загрузчик markdown-loader преобразует этот текст в строку HTML. Чтобы использовать эту строку HTML в каком-либо из исходных файлов `*.ts`, добавьте следующую строку `require()` в начале кода каждого файла после импорта:


```TypeScript
const strMarkdownString = require("../../../../src/readme.md") as string;
```

По умолчанию webpack будет искать файл в папке `lib`, но файлы `.md` не копируются в папку `lib` по умолчанию, поэтому необходимо составлять довольно длинный относительный путь. Мы можем изменить эту настройку, определив файл конфигурации, чтобы цепочка инструментов копировала файлы `md` в папку lib. 

Создайте файл `copy-static-assets.json` в каталоге `config`, чтобы система сборки копировала некоторые дополнительные файлы из `src` в `lib`. По умолчанию эта задача сборки копирует файлы с расширениями, которые распознает стандартная конфигурация webpack цепочки инструментов (например, `png` и `json`), поэтому мы просто поручим ей также копировать файлы `md`.

```JSON
{
  "includeExtensions": [
    "md"
  ]
}
```

Теперь в операторе `require` вместо относительного пути можно использовать путь к файлу, например:

```TypeScript
const strMarkdownString = require("../../readme.md") as string;
```
 
Вы можете ссылаться на эту строку в коде, например:

``` TypeScript
public render(): void {
  this.domElement.innerHTML = strMarkdownString;
}
```

### <a name="step-4---build-and-test-your-code"></a>Этап 4. Сборка и тестирование кода
Чтобы собрать и протестировать код, выполните в консоли следующую команду для корневого каталога проекта:

```
gulp serve
```