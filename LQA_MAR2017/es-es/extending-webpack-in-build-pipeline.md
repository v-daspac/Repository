# <a name="extending-webpack-in-the-sharepoint-framework-toolchain"></a>Extender Webpack en la cadena de herramientas de SharePoint Framework

>**Nota:** SharePoint Framework se encuentra en versión preliminar en este momento y está sujeto a cambios. Los elementos web del lado cliente de SharePoint Framework no se admiten actualmente para su uso en entornos de producción.

[Webpack](https://webpack.github.io/) es un módulo que instala varios programas de JavaScript que toma los archivos de JavaScript y las dependencias, y genera una o más agrupaciones JavaScript, de manera que pueda cargar agrupaciones diferentes para escenarios distintos.

La cadena de herramientas de Framework usa CommonJS para la agrupación. Esto le permite definir los módulos y el lugar en el que quiere usarlos. La cadena de herramientas también usa SystemJS, un cargador de módulos universal, para cargar sus módulos. Esto le ayuda a definir el ámbito de sus elementos web asegurándose de que cada uno se ejecuta en su propio espacio de nombres.

Una tarea común que quizás quiera agregar a la cadena de herramientas de SharePoint Framework es extender la configuración de Webpack con complementos y cargadores personalizados.

## <a name="using-webpack-loaders"></a>Usar cargadores de Webpack
Existen muchos casos en los que se querría importar y usar un recurso que no permite JavaScript durante el desarrollo, normalmente esto se realiza con imágenes o plantillas. Un [cargador de Webpack](https://webpack.github.io/docs/loaders.html) convertirá el recurso en algo que pueda usar su aplicación de JavaScript. Por ejemplo, una plantilla Markdown puede compilarse y convertirse en una cadena de texto, mientras que un recurso de imagen puede convertirse en una imagen Base64.

Existen varios cargadores útiles, algunos ya se usan en la configuración estándar de Webpack de SharePoint Framework, como:

- html-loader
- json-loader
- loader-load-themed-styles

Extender la configuración de Webpack de Framework con cargadores personalizados es un proceso sencillo que se documenta [aquí en la documentación de Webpack](https://webpack.github.io/docs/loaders.html#writing-a-loader).

> Puede obtener más información sobre los cargadores en la [documentación de cargadores de Webpack](https://webpack.github.io/docs/loaders.html)

## <a name="example-using-the-markdown-loader-package"></a>Ejemplo: Usar el paquete markdown-loader
Como ejemplo, vamos a usar el [paquete markdown-loader](https://www.npmjs.com/package/markdown-loader).  Es un cargador que le permite hacer referencia a un archivo `.md` y obtener una cadena HTML como resultado.

Puede descargar el ejemplo completo [aquí](https://aka.ms/spfx-extend-webpack-sample).

### <a name="step-1---install-the-package"></a>Paso 1: Instalar el paquete
Vamos a hacer referencia a markdown-loader en nuestro proyecto.

```
npm i --save markdown-loader 
```

### <a name="step-2---configure-webpack"></a>Paso 2: Configurar Webpack 
Ahora que tenemos instalado el paquete, vamos a configurar la configuración de Webpack de SharePoint Framework para incluir markdown-loader. 

En la [documentación de markdown-loader](https://github.com/peerigon/markdown-loader), se muestra cómo extender la configuración de Webpack para incluir el cargador:

```JavaScript
{
  module: {
    loaders: [
      { test: /\.md$/, loader: "html!markdown" }
    ]
  }
}
```

Usaremos esta información para configurarlo en nuestro proyecto. 

Para agregar este cargador personalizado en la configuración de Webpack de SharePoint Framework, necesitaremos indicar a la tarea de compilación que configure Webpack. Las tareas de compilación se definen en el archivo Gulp, `gulpfile.js`, que se encuentra en la raíz de su proyecto. SharePoint Framework usa [Gulp](http://gulpjs.com/) como su ejecutor de tareas y, por lo tanto, lo usamos para definir y registrar tareas personalizadas con el ejecutor de tareas Gulp.

Edite el archivo `gulpfile.js` y agregue el siguiente código justo antes de `build.initialize(gulp);`:

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

Observe que simplemente insertamos la configuración del cargador en la lista de cargadores existentes de la cadena de herramientas. Es importante asegurarse de que la función `additionalConfiguration` termina con la línea `return generatedConfiguration`, ya que esto garantiza que devuelve la configuración del cargador a la cadena de herramientas. 

> Aunque puede reemplazar completamente la configuración de Webpack predeterminada de la cadena de herramientas con este enfoque, para aprovechar al máximo el rendimiento y la optimización, no se recomienda hacerlo a no ser que se indique lo contrario en la documentación. 

### <a name="step-3---update-your-code"></a>Paso 3: Actualizar el código
Ahora que hemos configurado el cargador, vamos a actualizar nuestro código y a agregar algunos archivos para probar el escenario. 

Cree un archivo `my-markdown.md` en el directorio `src` de su carpeta de proyecto que incluya texto Markdown.

```md
#Hello Markdown

*Markdown* is a simple markup format to write content. 

You can also format text as **bold** or *italics* or ***bold italics***  
```

Cuando cree el proyecto, el paquete markdown-loader de Webpack convertirá este texto Markdown en una cadena HTML. Para usar esta cadena HTML en cualquiera de sus archivos `*.ts` de origen, agregue la siguiente línea `require()` en la parte superior del archivo después de las importaciones, por ejemplo:


```TypeScript
const strMarkdownString = require("../../../../src/readme.md") as string;
```

De manera predeterminada, Webpack buscará el archivo en la carpeta `lib`, pero los archivos `.md` no se copian de manera predeterminada en la carpeta `lib`, por lo que necesitamos crear una ruta de acceso relativa bastante más larga. Podemos controlar esta opción definiendo un archivo de configuración para indicar a la cadena de herramientas que copie archivos `md` en una carpeta lib. 

Cree un archivo `copy-static-assets.json` en el directorio `config` para indicar al sistema de compilación que copie algunos archivos adicionales de `src` a `lib`. De manera predeterminada, esta tarea de compilación copia archivos con extensiones que la configuración de Webpack de la cadena de herramientas predeterminada entiende (como `png` y `json`), por lo que solo tenemos que indicarle que también copie archivos `md`.

```JSON
{
  "includeExtensions": [
    "md"
  ]
}
```

Ahora, en lugar de usar la ruta de acceso relativa, puede usar la ruta del archivo en su instrucción `require`, por ejemplo:

```TypeScript
const strMarkdownString = require("../../readme.md") as string;
```
 
Después, puede hacer referencia a esta cadena en su código, por ejemplo:

``` TypeScript
public render(): void {
  this.domElement.innerHTML = strMarkdownString;
}
```

### <a name="step-4---build-and-test-your-code"></a>Paso 4: Compilar y probar el código
Para compilar y probar el código, ejecute el siguiente comando en una consola en la raíz del directorio del proyecto:

```
gulp serve
```