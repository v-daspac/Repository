# <a name="integrate-gulp-tasks-in-sharepoint-framework-toolchain"></a>SharePoint Framework 도구 체인에 gulp 작업 통합

>**참고:** SharePoint Framework는 현재 미리 보기로 제공되며 변경될 수 있습니다. SharePoint Framework 클라이언트 쪽 웹 파트는 현재 프로덕션 환경에서 사용하도록 지원되지 않습니다.

SharePoint 클라이언트 쪽 개발 도구는 빌드 프로세스 작업 실행기로 [gulp](http://gulpjs.com/)를 사용하여 다음을 수행합니다.

* JavaScript 및 CSS 파일을 번들로 묶고 축소합니다.
* 각 빌드 전에 번들화 및 축소 작업을 호출하는 도구를 실행합니다.
* ESS 또는 SASS 파일을 CSS로 컴파일합니다.
* TypeScript 파일을 JavaScript로 컴파일합니다.

SharePoint Framework 도구 체인에 추가하려는 일반적인 작업은 빌드 파이프라인에 사용자 지정 gulp 작업을 통합하는 것입니다.

## <a name="gulp-tasks"></a>Gulp 작업
일반적으로 Gulp 작업은 `gulpfile.js`에 다음과 같이 정의됩니다.

```js
gulp.task('somename', function() {
  // Do stuff
});
```

SharePoint Framework 도구 체인으로 작업할 때 프레임워크의 빌드 파이프라인에서 작업을 정의해야 합니다. 작업은 정의되고 파이프라인에 등록된 후에 도구 체인에 추가됩니다.

SharePoint Framework는 일반적인 빌드 작업을 공유하는 npm 패키지 집합으로 구성되는 [공용 빌드 도구 체인](sharepoint-framework-toolchain.md#common-build-tools-packages)을 사용합니다. 따라서 기본 작업은 클라이언트 쪽 프로젝트의 `gulpfile.js`와 달리 일반적인 패키지에 정의됩니다. 사용할 수 있는 작업을 보려면 프로젝트 디렉터리 내의 콘솔에서 다음 명령을 실행할 수 있습니다.

```
gulp --tasks
```

위의 명령을 실행하면 사용 가능한 모든 작업이 표시됩니다.

![사용할 수 있는 gulp 작업](../../images/gulp-tasks-available.png)

## <a name="custom-gulp-tasks"></a>사용자 지정 gulp 작업
사용자 지정 작업을 추가하려면 `gulpfile.js`에서 사용자 지정 작업을 정의합니다. 코드 편집기에서 `gulpfile.js`를 엽니다. 기본 코드는 SharePoint Framework 도구 체인 및 해당 도구 체인에 대한 전역 `gulp` 인스턴스를 초기화합니다. 추가된 모든 사용자 지정 작업은 전역 `gulp` 인스턴스를 초기화하기 전에 정의해야 합니다.

### <a name="add-your-custom-task"></a>사용자 지정 작업 추가
사용자 지정 gulp 작업을 추가하려면 [`build.subTask`](https://github.com/Microsoft/gulp-core-build#defining-a-custom-task) 함수를 사용하여 SharePoint Framework 빌드 파이프라인에 새 하위 작업을 추가합니다.

```js
let helloWorldSubtask = build.subTask('log-hello-world-subtask', function(gulp, buildOptions, done) {
  this.log('Hello, World!');   
  // use functions from gulp task here  
});
```

스트림의 경우 스트림을 반환합니다.

```js
let helloWorldSubtask = build.subTask('log-hello-world-subtask', function(gulp, buildOptions, done) {
  return gulp.src('images/*.png')
             .pipe(gulp.dest('public'));
});
```

### <a name="register-your-task-with-gulp-command-line"></a>gulp 명령줄에 작업 등록
사용자 지정 작업이 정의된 후에는 `build.task` 함수를 사용하여 이 사용자 지정 작업을 gulp 명령줄에 등록할 수 있습니다.

```js
// Register the task with gulp command line
let helloWorldTask = build.task('hello-world', helloWorldSubtask);
```

>참고: 추가된 모든 사용자 지정 작업은 전역 `gulp` 인스턴스를 초기화하기 전에 `build.initialize(gulp);` 코드 줄 앞에 정의해야 합니다.

이제 다음과 같이 명령줄에서 사용자 지정 명령을 실행할 수 있습니다.

```js
gulp hello-world
```

>참고: 명령줄에서 직접 `build.subTask` 함수를 사용하여 등록된 하위 작업을 실행할 수는 없습니다. `build.task` 함수를 사용해야만 등록된 작업을 실행할 수 있습니다.

### <a name="execute-your-custom-task-before-or-after-available-tasks"></a>사용 가능한 작업 전후에 사용자 지정 작업 실행
또한 사용할 수 있는 특정 gulp 작업 전후에 실행되도록 사용자 지정 작업을 추가할 수도 있습니다. 다음 gulp 작업을 사용하여 작업 전후에 사용자 지정 작업 삽입할 수 있습니다.

- 일반적인 빌드 작업(사용 가능한 모든 작업으로 구성)
- TypeScript 작업

SharePoint Framework 작업은 기본 빌드 Rig에서 사용할 수 있습니다. 빌드 Rig는 특정 용도로 정의된 작업의 컬렉션입니다. 이 경우에는 클라이언트 쪽 패키지 빌드가 작업의 용도입니다. `build.rig` 개체를 사용하여 이 기본 Rig에 액세스하고 사전 및 사후 작업 함수에 대한 액세스 권한을 얻을 수 있습니다.
 
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

## <a name="example-custom-image-resize-task"></a>예제: 사용자 지정 이미지 크기 조정 작업
예를 들어 [이미지 크기 조정 gulp 작업](https://www.npmjs.com/package/gulp-image-resize)을 사용해 보겠습니다.  이 작업은 이미지 크기를 조정할 수 있는 간단한 작업입니다.

전체 샘플은 [여기](https://aka.ms/spfx-extend-gulp-sample)에서 다운로드할 수 있습니다.

[이미지 크기 조정 작업 설명서](https://www.npmjs.com/package/gulp-image-resize#example)에는 이 사용자 지정 작업을 사용하는 방법이 나와 있습니다.

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

이 정보는 `build.subTask` 및 `build.task` 함수를 사용하여 프로젝트에 이 작업을 추가하는 데 사용됩니다.

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

스트림을 정의해야 하므로 `build.subTask` 함수의 스트림을 빌드 파이프라인으로 반환합니다. 그러면 빌드 파이프라인은 이 gulp 스트림을 비동기적으로 실행합니다. 

이제 다음과 같이 gulp 명령줄에서 이 작업을 실행할 수 있습니다.

```js
gulp resize-images
```

![image-resize-task](../../images/gulp-extend-image-resize-task.png)

`gulp --tasks`를 실행할 때 프로젝트에 대한 사용 가능한 작업에서 `resize-images` 작업도 표시됩니다.

![사용 가능한 작업이 있는 image-resize-task](../../images/gulp-extend-image-resize-available-tasks.png)




