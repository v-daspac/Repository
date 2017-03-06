# <a name="extending-webpack-in-the-sharepoint-framework-toolchain"></a>在 SharePoint Framework 工具链中扩展 Webpack

>**注意：**SharePoint Framework 目前处于预览模式，并可能会发生更改。当前不支持在生产环境中使用 SharePoint Framework 客户端 Web 部件。

[Webpack](https://webpack.github.io/) 是一个 JavaScript 模块程序包，它采用 JavaScript 文件及其依赖项，并生成一个或多个 JavaScript 捆绑，以便为不同的方案加载不同的程序包。

Framework 工具链使用 CommonJS 进行构建。这便于你对模块及其使用位置进行定义。工具链还使用通用的模块加载器 SystemJS 来加载模块。这有助于通过确保每个 Web 部件在其各自的命名空间中执行，确定 Web 部件的范围。

可能需要向 SharePoint Framework 工具链添加的一个常见任务是使用自定义加载程序和插件扩展 webpack 配置。

## <a name="using-webpack-loaders"></a>使用 webpack 加载程序
在许多情况下，需要在开发过程中导入和利用非 JavaScript 资源，这通常通过图像或模板实现。[webpack 加载程序](https://webpack.github.io/docs/loaders.html)将资源转化成一种 JavaScript 应用程序可用的资源。例如，可将 Markdown 模板编译和转换为文本字符串，将图像资源转换为 Base64 图像。

存在大量有用的加载程序，其中多个已由标准 SharePoint Framework webpack 配置使用，如：

- html-loader
- json-loader
- loader-load-themed-styles

使用自定义加载程序扩展 Framework webpack 配置是一个简单的过程，此过程记录在[此处的 webpack 文档](https://webpack.github.io/docs/loaders.html#writing-a-loader)中。

> 可在 [webpack 加载文档](https://webpack.github.io/docs/loaders.html)中找到关于加载程序的更多详细信息。

## <a name="example-using-the-markdown-loader-package"></a>示例：使用 markdown-loader 包
举一个例子，我们将使用 [markdown-loader 包](https://www.npmjs.com/package/markdown-loader)。通过此加载程序，可引用 `.md` 文件并将其输出为 HTML 字符串。

可在[此处](https://aka.ms/spfx-extend-webpack-sample)下载完整的示例。

### <a name="step-1---install-the-package"></a>步骤 1 - 安装包
在项目中引用 markdown-loader。

```
npm i --save markdown-loader 
```

### <a name="step-2---configure-webpack"></a>步骤 2 - 配置 Webpack 
安装包后，配置 SharePoint Framework webpack 配置以将 markdown-loader 包含在内。 

在 [markdown-loader 文档](https://github.com/peerigon/markdown-loader)中，介绍了如何扩展 webpack 配置来包括该加载程序：

```JavaScript
{
  module: {
    loaders: [
      { test: /\.md$/, loader: "html!markdown" }
    ]
  }
}
```

我们将使用此信息在项目中对其进行配置。 

若要将此自定义加载程序添加到 SharePoint Framework webpack 配置，需要指示生成任务配置 webpack。gulp 文件 `gulpfile.js` 中定义生成任务，此文件位于项目的根目录中。SharePoint Framework 将 [gulp](http://gulpjs.com/) 用作其任务运行程序，因此可用它来定义自定义任务，并向 gulp 任务运行程序注册自定义任务。

编辑 `gulpfile.js` 并在 `build.initialize(gulp);` 前直接添加以下代码：

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

请注意，只需将加载程序配置推送到工具链中的现有加载程序列表。请务必确保 `additionalConfiguration` 函数以 `return generatedConfiguration` 行结尾，因为这样可确保向工具链返回加载程序配置。 

> 尽管可用此方法彻底替换工具链的默认 webpack 配置，但为了实现最佳性能和优化，除非文档中另有规定，否则不建议这样做。 

### <a name="step-3---update-your-code"></a>步骤 3 - 更新代码
配置加载程序后，更新代码并添加一些文件来测试方案。 

在项目文件夹的 `src` 目录中创建包含 Markdown 文本的 `my-markdown.md` 文件。

```md
#Hello Markdown

*Markdown* is a simple markup format to write content. 

You can also format text as **bold** or *italics* or ***bold italics***  
```

生成项目时，webpack markdown-loader 会将此 Markdown 文本转换为 HTML 字符串。若要在任何源 `*.ts` 文件中使用此 HTML 字符串，请导入后在文件顶部添加以下 `require()` 行，例如：


```TypeScript
const strMarkdownString = require("../../../../src/readme.md") as string;
```

默认情况下，Webpack 将在 `lib` 文件夹中查找文件，但 `.md` 文件不会默认复制到 `lib` 文件夹，这意味着需要创建一个很长的相对路径。可以通过定义配置文件控制此设置，指示工具链将 `md` 文件复制到 lib 文件夹。 

在 `config` 目录中创建 `copy-static-assets.json` 文件，指示生成系统将一些其他文件从 `src` 复制到 `lib`。默认情况下，此生成任务将复制带有默认工具链 webpack 配置可识别的扩展名（如 `png` 和 `json`）的文件，因此只需指示其也复制 `md` 文件。

```JSON
{
  "includeExtensions": [
    "md"
  ]
}
```

现在，可以在 `require` 语句中使用该文件路径，而非相对路径，例如：

```TypeScript
const strMarkdownString = require("../../readme.md") as string;
```
 
然后可在代码中引用此字符串，例如：

``` TypeScript
public render(): void {
  this.domElement.innerHTML = strMarkdownString;
}
```

### <a name="step-4---build-and-test-your-code"></a>步骤 4 - 生成和测试代码
若要生成并测试代码，请在控制台中项目目录的根目录中执行以下命令：

```
gulp serve
```