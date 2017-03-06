# <a name="extending-webpack-in-the-sharepoint-framework-toolchain"></a>SharePoint Framework 도구 체인에서 Webpack 확장

>**참고:** SharePoint Framework는 현재 미리 보기로 제공되며 변경될 수 있습니다. SharePoint Framework 클라이언트 쪽 웹 파트는 현재 프로덕션 환경에서 사용하도록 지원되지 않습니다.

[Webpack](https://webpack.github.io/)은 JavaScript 파일 및 해당 종속 파일을 사용하고 하나 이상의 JavaScript 번들을 생성하여 다른 시나리오마다 다른 번들을 로드할 수 있도록 하는 JavaScript 모듈 번들러입니다.

Framework 도구 체인은 번들화를 위해 CommonJS를 사용합니다. 이를 통해 모듈 및 사용하려는 위치를 정의할 수 있습니다. 도구 체인은 유니버설 모듈 로더인 SystemJS를 사용하여 모듈을 로드합니다. 이러한 도구 체인은 각 웹 파트가 자체 네임스페이스에 실행되도록 하여 웹 파트의 범위를 지정할 수 있도록 합니다.

SharePoint Framework 도구 체인에 추가할 수 있는 일반적인 작업은 사용자 지정 로더 및 플러그 인을 사용하여 webpack 구성을 확장하는 것입니다.

## <a name="using-webpack-loaders"></a>webpack 로더 사용
개발 중에 JavaScript 이외 리소스를 가져와 활용하려는 많은 경우가 있을 수 있으며 일반적으로 이러한 작업은 이미지 또는 템플릿을 사용하여 수행합니다. [webpack 로더](https://webpack.github.io/docs/loaders.html)는 리소스를 JavaScript 응용 프로그램에서 사용할 수 있는 형태로 변환합니다. 예를 들어 Markdown 템플릿은 텍스트 문자열로 컴파일 및 변환할 수 있지만 이미지 리소스는 Base64 이미지로 변환할 수 있습니다.

유용한 많은 로더가 있으며 다음과 같은 일부 로더는 표준 SharePoint Framework webpack 구성에서 이미 사용되고 있습니다.

- html-loader
- json-loader
- loader-load-themed-styles

사용자 지정 로더를 사용하여 Framework Webpack 구성을 확장하는 과정은 간단한 프로세스로, [이 Webpack 설명서](https://webpack.github.io/docs/loaders.html#writing-a-loader)에 자세히 설명되어 있습니다.

> 로더에 대한 자세한 내용은 [Webpack 로더 설명서](https://webpack.github.io/docs/loaders.html)를 참조하세요.

## <a name="example-using-the-markdown-loader-package"></a>예제: markdown-loader 패키지 사용
예를 들어 [markdown-loader 패키지](https://www.npmjs.com/package/markdown-loader)를 사용해보겠습니다.  `.md` 파일에서 참조하고 HTML 문자열로 출력할 수 있는 로더입니다.

전체 샘플은 [여기](https://aka.ms/spfx-extend-webpack-sample)에서 다운로드할 수 있습니다.

### <a name="step-1---install-the-package"></a>1단계 - 패키지 설치
프로젝트에서 markdown-loader를 참조하겠습니다.

```
npm i --save markdown-loader 
```

### <a name="step-2---configure-webpack"></a>2단계 - Webpack 구성 
이제 패키지를 설치했으므로 markdown-loader를 포함하도록 SharePoint Framework webpack 구성을 지정합니다. 

[markdown-loader 설명서](https://github.com/peerigon/markdown-loader)에는 이 로더를 포함하도록 webpack 구성을 확장하는 방법이 나와 있습니다.

```JavaScript
{
  module: {
    loaders: [
      { test: /\.md$/, loader: "html!markdown" }
    ]
  }
}
```

이 정보를 사용하여 프로젝트에서 해당 항목을 구성해보겠습니다. 

이 사용자 지정 로더를 SharePoint Framework webpack 구성에 추가하려면 webpack을 구성하도록 빌드 작업을 지정해야 합니다. 빌드 작업은 프로젝트의 루트에 있는 gulp 파일 `gulpfile.js`에 정의됩니다. SharePoint Framework는 [gulp](http://gulpjs.com/)를 작업 실행기로 사용하므로 이를 활용하여 사용자 지정 작업을 정의하고 Gulp 작업 실행기에 등록합니다.

`gulpfile.js`를 편집하고 `build.initialize(gulp);` 바로 앞에 다음 코드를 추가합니다.

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

도구 체인에서 기존 로더 목록에 해당 로더 구성을 밀어넣기만 하면 됩니다. `additionalConfiguration` 함수가 `return generatedConfiguration` 줄로 끝나는지 확인해야 합니다. 그래야 로더 구성이 도구 체인으로 반환됩니다. 

> 이 접근 방법을 사용하여 도구 체인의 기본 webpack 구성을 완전히 바꿀 수 있으나 최상의 성능 및 최적화를 구현하려는 경우 다른 언급이 없는 한, 이 방식은 권장되지 않습니다. 

### <a name="step-3---update-your-code"></a>3단계 - 코드 업데이트
로더를 구성했으므로 코드를 업데이트하고 몇 개의 파일을 추가하여 시나리오를 테스트해보겠습니다. 

일부 Markdown 텍스트가 있는 프로젝트 폴더의 `src` 디렉터리에 파일 `my-markdown.md`를 만듭니다.

```md
#Hello Markdown

*Markdown* is a simple markup format to write content. 

You can also format text as **bold** or *italics* or ***bold italics***  
```

프로젝트를 빌드할 때 webpack markdown-loader는 이 markdown 텍스트를 HTML 문자열로 변환합니다. 소스 `*.ts` 파일에 이 HTML 문자열을 사용하려면 가져오기 후에 파일 맨 위에 다음 `require()` 줄을 추가합니다. 예를 들면 다음과 같습니다.


```TypeScript
const strMarkdownString = require("../../../../src/readme.md") as string;
```

기본적으로 webpack은 `lib` 폴더에서 해당 파일을 찾지만 기본적으로 `.md` 파일은 `lib` 폴더로 복사되지 않습니다. 즉, 비교적 긴 상대 경로를 만들어야 합니다. 도구 체인에 `md` 파일을 lib 폴더에 복사하도록 지시하는 구성 파일을 정의하여 이 설정을 제어할 수 있습니다. 

`config` 디렉터리에 파일 `copy-static-assets.json`을 만들어 `src`에서 `lib`로 일부 추가 파일을 복사하도록 빌드 시스템에 지시합니다. 기본적으로 이 빌드 작업은 기본 도구 체인 webpack 구성이 이해하는 확장(예: `png` 및 `json`)이 있는 파일을 복사하므로 `md` 파일도 복사하도록 지시하기만 하면 됩니다.

```JSON
{
  "includeExtensions": [
    "md"
  ]
}
```

상대 경로를 사용하는 대신, `require` 문에 파일 경로를 사용할 수 있습니다. 예를 들면 다음과 같습니다.

```TypeScript
const strMarkdownString = require("../../readme.md") as string;
```
 
그런 후 코드에서 이 문자열을 참조할 수 있습니다. 예를 들면 다음과 같습니다.

``` TypeScript
public render(): void {
  this.domElement.innerHTML = strMarkdownString;
}
```

### <a name="step-4---build-and-test-your-code"></a>4단계 - 코드 빌드 및 테스트
코드를 빌드하고 테스트하려면 프로젝트 디렉터리의 루트에서 콘솔의 다음 명령을 실행합니다.

```
gulp serve
```