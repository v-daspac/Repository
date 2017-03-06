# <a name="extending-webpack-in-the-sharepoint-framework-toolchain"></a>SharePoint フレームワーク ツールチェーンでの webpack の拡張

>**注:**SharePoint フレームワークは、現在プレビュー段階で、変更される可能性があります。SharePoint フレームワーククライアント側の Web パーツは、運用環境での使用は現在サポートされていません。

[webpack](https://webpack.github.io/) は、JavaScript ファイルとその依存関係を処理して 1 つまたは複数の JavaScript バンドルを生成する JavaScript モジュール バンドラーです。さまざまなシナリオについて異なるバンドルを読み込むことができます。

フレームワーク ツールチェーンは、CommonJS を使ってバンドルします。これにより、モジュールを定義し、それらのモジュールを使う場所を定義することができます。また、ツールチェーンは、ユニバーサル モジュール ローダーである SystemJS を使ってモジュールを読み込みます。これにより、各 Web パーツが独自の名前空間で実行されるようになるため、Web パーツのスコープを設定するために役立ちます。

SharePoint フレームワークのツールチェーンに追加できる一般的なタスクの 1 つは、カスタム ローダーとプラグインを使って webpack 構成を拡張することです。

## <a name="using-webpack-loaders"></a>webpack ローダーの使用
開発時に JavaScript 以外のリソースをインポートして利用する機会は、数多くあります。通常、これは、イメージまたはテンプレートを使って行います。[webpack ローダー](https://webpack.github.io/docs/loaders.html)により、リソースは JavaScript アプリケーションで利用できる形式に変換されます。たとえば、マークダウン テンプレートはコンパイルされてテキスト ストリングに変換され、イメージ リソースは Base64 イメージに変換されます。

いくつかの便利なローダーが用意されており、その中のいくつかは標準の SharePoint フレームワーク webpack 構成で既に使用されています。たとえば、次のローダーがあります。

- html-loader
- json-loader
- loader-load-themed-styles

カスタム ローダーを使ってフレームワークの webpack 構成を拡張する作業は、簡単なプロセスです。[webpack のドキュメント](https://webpack.github.io/docs/loaders.html#writing-a-loader)に説明があります。

> ローダーの詳細については、[webpack ローダーのドキュメント](https://webpack.github.io/docs/loaders.html)をご覧ください。

## <a name="example-using-the-markdown-loader-package"></a>例:markdown-loader パッケージの使い方
一例として、[markdown-loader パッケージ](https://www.npmjs.com/package/markdown-loader)を使ってみましょう。このローダーでは、`.md` ファイルを参照し、それを HTML 文字列として出力します。

完成したサンプルは、[こちら](https://aka.ms/spfx-extend-webpack-sample)からダウンロードすることができます。

### <a name="step-1---install-the-package"></a>手順 1 - パッケージをインストールする
プロジェクトで、markdown-loader ローダーを参照します。

```
npm i --save markdown-loader 
```

### <a name="step-2---configure-webpack"></a>手順 2 - webpack を構成する 
これでパッケージをインストールできましたので、次に、SharePoint フレームワーク webpack の構成に markdown-loader を組み込みます。 

[markdown-loader のドキュメント](https://github.com/peerigon/markdown-loader)には、webpack 構成を拡張してローダーを組み込む方法が記載されています。

```JavaScript
{
  module: {
    loaders: [
      { test: /\.md$/, loader: "html!markdown" }
    ]
  }
}
```

ここでは、この情報を使ってプロジェクト内で webpack 構成を設定します。 

SharePoint フレームワーク webpack 構成にこのカスタム ローダーを追加するには、webpack を構成するようにビルド タスクに指示を与える必要があります。ビルド タスクは、gulp ファイル (`gulpfile.js`) で定義します。このファイルは、プロジェクトのルートにあります。SharePoint フレームワークでは、タスク ランナーとして [gulp](http://gulpjs.com/) を使います。したがって、gulp を使ってカスタム タスクを定義し、gulp タスク ランナーに登録します。

`gulpfile.js` を編集し、次のコードを `build.initialize(gulp);` の直前に追加します。

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

ローダーの構成を、ツールチェーンにある既存のローダーのリストに単にプッシュしているだけであることに注目してください。ここで、`additionalConfiguration` 関数の最後に `return generatedConfiguration` の行を置くことが重要です。これにより、ローダー構成がツールチェーンに返されるからです。 

> この方法を使ってツールチェーンの既定の webpack 構成を完全に置き換えることもできますが、パフォーマンスと最適化のメリットを最大限に活用するために、ドキュメントに明記されていない限り、そのようにすることはお勧めしません。 

### <a name="step-3---update-your-code"></a>手順 3 - コードを更新する
これでローダーの構成が終わりましたので、次に、コードを更新し、シナリオをテストするためにいくつかのファイルを追加しましょう。 

プロジェクト フォルダーの `src` ディレクトリに `my-markdown.md` ファイルを作成し、そのファイルに何らかのマークダウン テキストを追加します。

```md
#Hello Markdown

*Markdown* is a simple markup format to write content. 

You can also format text as **bold** or *italics* or ***bold italics***  
```

プロジェクトをビルドすると、webpack markdown-loader によってこのマークダウン テキストが HTML 文字列に変換されます。ソース `*.ts` ファイルのいずれかでこの HTML 文字列を使うには、次の `require()` 行を、ファイル先頭の imports の後に追加します。たとえば、次のとおりです。


```TypeScript
const strMarkdownString = require("../../../../src/readme.md") as string;
```

webpack は既定では `lib` フォルダーからファイルを探しますが、既定では `.md` ファイルが `lib` フォルダーにコピーされません。したがって、かなり長い相対パスを作成する必要があります。この設定を制御するには、ツールチェーンが `md` ファイルを lib フォルダーにコピーするように構成ファイルを定義します。 

ファイル `copy-static-assets.json` を `config` ディレクトリに作成し、いくつかの追加ファイルを `src` から `lib` にコピーするようにビルド システムに指示を与えます。既定では、このビルド タスクは、既定のツールチェーン webpack 構成に認識される拡張子 (`png` や `json` など) を持つファイルをコピーします。したがって、`md` ファイルもコピーするようにビルドタスクに指示を与える必要があります。

```JSON
{
  "includeExtensions": [
    "md"
  ]
}
```

これで、`require` ステートメントで、相対パスを使う代わりに、ファイル パスを使用できるようになります。たとえば、次のとおりです。

```TypeScript
const strMarkdownString = require("../../readme.md") as string;
```
 
その後、この文字列をコードで参照することができます。たとえば、次のとおりです。

``` TypeScript
public render(): void {
  this.domElement.innerHTML = strMarkdownString;
}
```

### <a name="step-4---build-and-test-your-code"></a>手順 4 - コードをビルドしてテストする
コードをビルドしてテストするために、コンソールから、プロジェクト ディレクトリのルートで、次のコマンドを実行します。

```
gulp serve
```