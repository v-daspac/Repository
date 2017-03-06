# <a name="extending-webpack-in-the-sharepoint-framework-toolchain"></a>Estendendo webpack no toolchain da estrutura do SharePoint

>**Observação:** O SharePoint Framework está atualmente em visualização e está sujeito a alterações. No momento, as Web Parts do lado do cliente do SharePoint Framework não têm suporte para uso em ambientes de produção.

O [Webpack](https://webpack.github.io/) é um empacotador de módulo JavaScript que considera seus arquivos JavaScript como dependências e gera um ou mais conjuntos de JavaScript para que você possa carregar diferentes pacotes para diferentes cenários.

A cadeia de ferramentas de estrutura usa CommonJS para empacotamento. Isso permite que você defina módulos e onde deseja usá-los. A cadeia de ferramentas também usa SystemJS, um carregador de módulos universal, para carregar seus módulos. Isso ajuda a criar um escopo como Web Parts, garantindo que cada Web Part seja executada em seu próprio namespace.

Uma tarefa comum que você pode desejar adicionar ao toolchain da estrutura do SharePoint é estender a configuração do webpack com carregadores e plug-ins personalizados.

## <a name="using-webpack-loaders"></a>Usando carregadores webpack
Há muitos casos onde uma pessoa gostaria de importar e usar um recurso não JavaScript durante o desenvolvimento, isso normalmente é feito com imagens ou modelos. Um [carregador webpack](https://webpack.github.io/docs/loaders.html) vai converter o recurso em algo que pode ser usado pelo seu aplicativo JavaScript. Por exemplo, um modelo de redução poderá ser compilado e convertido em uma cadeia de caracteres de texto, enquanto um recurso de imagem poderá ser convertido em uma imagem Base64.

Há diversos carregadores úteis, vários dos quais já estão sendo usados pela configuração webpack da estrutura do SharePoint padrão, como:

- html-loader
- json-loader
- loader-load-themed-styles

Estender a configuração de webpack da estrutura com carregadores padrão é um processo simples que está documentado [aqui na documentação do webpack](https://webpack.github.io/docs/loaders.html#writing-a-loader).

> Você pode encontrar mais detalhes sobre os carregadores na [documentação dos carregadores webpack](https://webpack.github.io/docs/loaders.html)

## <a name="example-using-the-markdown-loader-package"></a>Exemplo: Usando o pacote de carregador de redução
Como exemplo, vamos usar o [pacote de carregador de redução](https://www.npmjs.com/package/markdown-loader).  É um carregador que permite que você faça referência a um arquivo `.md` e o emita como uma cadeia de caracteres HTML.

Você pode baixar o exemplo completo [aqui](https://aka.ms/spfx-extend-webpack-sample).

### <a name="step-1---install-the-package"></a>Etapa 1 - instalar o pacote
Vamos fazer referência ao carregador de redução em nosso projeto.

```
npm i --save markdown-loader 
```

### <a name="step-2---configure-webpack"></a>Etapa 2 - configurar Webpack 
Agora que temos o pacote instalado, vamos definir a configuração do webpack da estrutura do SharePoint para incluir o carregador de redução. 

Na [documentação do carregador de redução](https://github.com/peerigon/markdown-loader), ele mostra como estender a configuração do webpack para incluir o carregador:

```JavaScript
{
  module: {
    loaders: [
      { test: /\.md$/, loader: "html!markdown" }
    ]
  }
}
```

Usaremos essa informação configurá-lo em nosso projeto. 

Para adicionar esse carregador personalizado à configuração de webpack da estrutura do SharePoint, precisaremos instruir a tarefa de criação para configurar o webpack. As tarefas de criação são definidas no arquivo gulp - `gulpfile.js` - que está localizado na raiz de seu projeto. A estrutura do SharePoint usa [gulp](http://gulpjs.com/) como executor de tarefas e, por isso, o usamos para definir e registrar tarefas personalizadas com o executor de tarefas gulp.

Edite o `gulpfile.js` e adicione o seguinte código imediatamente antes de `build.initialize(gulp);`:

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

Observe que podemos simplesmente colocar a config carregador na lista dos carregadores existentes na toolchain. É importante garantir que sua função `additionalConfiguration` termine com a linha `return generatedConfiguration`, já que isso assegura que ela retorne a configuração de carregador para o toolchain. 

> Enquanto você for capaz de substituir completamente a configuração webpack padrão do toolchain usando essa abordagem, para obter o benefício máximo com o desempenho e a otimização, não é recomendável para fazê-lo, a menos que indicado na documentação de outra forma. 

### <a name="step-3---update-your-code"></a>Etapa 3: atualizar seu código
Agora que configuramos o carregador, vamos atualizar nosso código e adicionar alguns arquivos para testar o cenário. 

Crie um arquivo `my-markdown.md` no diretório `src` de sua pasta de projeto com algum texto de redução nele.

```md
#Hello Markdown

*Markdown* is a simple markup format to write content. 

You can also format text as **bold** or *italics* or ***bold italics***  
```

Quando você cria um projeto, o carregador de redução webpack converterá esse texto de redução em uma cadeia de caracteres HTML. Para usar essa cadeia de caracteres HTML em qualquer um de seus arquivos de origem `*.ts`, adicione a seguinte linha `require()` na parte superior do arquivo após as importações, por exemplo:


```TypeScript
const strMarkdownString = require("../../../../src/readme.md") as string;
```

Webpack por padrão procurará na pasta `lib` pelo arquivo mas, por padrão os arquivos `.md` não são copiados para a pasta `lib`, o que significa que precisamos criar um caminho relativo bastante extenso. Podemos controlar essa configuração definindo um arquivo de configuração para dizer à cadeia de ferramentas para copiar arquivos `md` para a pasta da biblioteca. 

Criar um arquivo `copy-static-assets.json` no diretório `config` para dizer ao sistema de criação para copiar alguns arquivos adicionais de `src` para `lib`. Por padrão, essa tarefa de criação copia arquivos com extensões que a configuração webpack de toolchain entende (como `png` e `json`), então precisamos apenas dizer a ele que também copie os arquivos `md`.

```JSON
{
  "includeExtensions": [
    "md"
  ]
}
```

Agora, em vez de usar o caminho relativo, você pode usar o caminho do arquivo na sua instrução `require`, por exemplo:

```TypeScript
const strMarkdownString = require("../../readme.md") as string;
```
 
Você então pode fazer referência a essa cadeia de caracteres em seu código, por exemplo:

``` TypeScript
public render(): void {
  this.domElement.innerHTML = strMarkdownString;
}
```

### <a name="step-4---build-and-test-your-code"></a>Etapa 4: criar e testar seu código
Para criar e testar seu código, execute o seguinte comando em um console na raiz do diretório do projeto:

```
gulp serve
```