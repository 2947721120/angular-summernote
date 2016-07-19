# angular-summernote - [AngularJS](http://angularjs.org/) directive to [Summernote](http://summernote.org/)

***

[![Build Status](https://travis-ci.org/summernote/angular-summernote.png?branch=master)](https://travis-ci.org/summernote/angular-summernote)
[![Dependency Status](https://gemnasium.com/summernote/angular-summernote.png)](https://gemnasium.com/summernote/angular-summernote)
[![Coverage Status](https://coveralls.io/repos/summernote/angular-summernote/badge.png)](https://coveralls.io/r/summernote/angular-summernote)

angular-summernote is just a directive to bind summmernote's all features.
You can use summernote with angular way.

**Since v0.7.x, the version of angular-summernote follows the version of summernote.
So, angular-summernote v0.7.x are compatible with summernote v0.7.x and
and angular-summernote v0.8.x will be compatible with summernote v0.8.x.
Angular-summernote will match only `major.minor` with summernote.
Therefore, angular-summernote v0.7.0 will be compatible with summernote v0.7.0, v0.7.1 and
v0.7.2. Angular-summernote will release patch update, such as v0.7.1, if only angular-summernote has changed.**

## Table of Contents

- [Demo](#demo)
- [Installation](#Installation)
- [How To Use](#how-to-use)
    - [summernote directive](summernote-directive)
    - [Options](#options)
    - [ngModel](#ngmodel)
    - [Event Listeners](#event-listeners)
    - [i18n Support](#i18n-support)
- [FAQ](#faq)
- [Change Logs](#change-logs)

## Demo

See at [JSFiddle](http://jsfiddle.net/outsider/n8dt4/322/embedded/result%2Chtml%2Cjs%2Ccss/)
or run example in projects(need to run `bower install` before run)

## Installation

angular-summernote requires all include files of [Summernote](http://summernote.org/).
see [Summernote's installation](http://summernote.org/#/features#installation).

项目文件也可以通过您最喜欢的包管理器:

* Bower: `bower install angular-summernote`

## How To Use

当您完成下载所有依赖关系和项目文件剩下的部分是在ui.bootstrap AngularJS模块添加依赖关系:

当你把所有的JS和CSS文件需要注入 `a` 进入你的角度应用:

```javascript
angular.module('myApp', ['summernote']);
```

### `summernote` Directive(指令)

你可以使用 `summernote`指令要使用summernote编辑器.
当范围被销毁时，指令将被销毁.

#### 作为元:

```html
<summernote></summernote>
```
#### 属性:

```html
<div summernote></div>
```

它将自动初始化.

如果你把涨价的指令，标记作为初始文本.

```html
<summernote><span style="font-weight: bold;">这是最初的文本.</span></summernote>
```

### Options

summernote的选项可以指定为属性.

#### height

```html
<summernote height="300"></summernote>
```

#### focus(集中)

```html
<summernote focus></summernote>
```

#### airmode(大气模式)
```html
<summernote airMode></summernote>
```

如果你使用 `removeMedia` 按钮在弹出，如下:

```
<summernote airMode config="options" on-media-delete="mediaDelete(target)"></summernote>
```

```
function DemoController($scope) {
  $scope.options = {
    popover: {
      image: [['remove', ['removeMedia']] ],
      air: [['insert', ['picture']]]
    }
  };
  $scope.mediaDelete = function(target) {
    console.log('media is delted:', target);
  }
}
```

你可以使用 'onMediaDelete` callback. The `target` 对象的DOM，删除信息:

```
{
  tagName: "IMG",
  attrs: {
    data-filename: "image-name.jpg",
    src: "http://path/to/image",
    style: "width: 100px;"
  }
}
```

#### 选择对象

你可以指定所有的选项中使用ngmodel `config` 属性.

```html
<summernote config="options"></summernote>
```

```javascript
function DemoController($scope) {
  $scope.options = {
    height: 300,
    focus: true,
    airMode: true,
    toolbar: [
            ['edit',['undo','redo']],
            ['headline', ['style']],
            ['style', ['bold', 'italic', 'underline', 'superscript', 'subscript', 'strikethrough', 'clear']],
            ['fontface', ['fontname']],
            ['textsize', ['fontsize']],
            ['fontclr', ['color']],
            ['alignment', ['ul', 'ol', 'paragraph', 'lineheight']],
            ['height', ['height']],
            ['table', ['table']],
            ['insert', ['link','picture','video','hr']],
            ['view', ['fullscreen', 'codeview']],
            ['help', ['help']]
        ]
  };
}
```

NOTE: `height` and `focus` attributes have high priority than options object.

注：自定义工具栏可以通过选项对象设置.

### ngModel

summernote's `code`, that is HTML string in summernote.
If you specify ngModel it will be 2-ways binding
to HTML string in summernote. Otherwise `angular-summernote` simply ignore it.

```html
<summernote ng-model="text"></summernote>
```

```javascript
function DemoController($scope) {
  $scope.text = "Hello World";
}
```

And you can use [ngModelOptions](https://docs.angularjs.org/api/ng/directive/ngModelOptions)
with Angular v1.3+. So, you can update ngModel when blur event emitted or with a debouncing delay
if you want.

### Event Listeners

事件侦听器可以注册为您想要的属性.

```javascript
function DemoController($scope) {
  $scope.init = function() { console.log('Summernote is launched'); }
  $scope.enter = function() { console.log('Enter/Return key pressed'); }
  $scope.focus = function(e) { console.log('Editable area is focused'); }
  $scope.blur = function(e) { console.log('Editable area loses focus'); }
  $scope.paste = function(e) { console.log('Called event paste'); }
  $scope.change = function(contents) {
    console.log('contents are changed:', contents, $scope.editable);
  };
  $scope.keyup = function(e) { console.log('Key is released:', e.keyCode); }
  $scope.keydown = function(e) { console.log('Key is pressed:', e.keyCode); }
  $scope.imageUpload = function(files) {
    console.log('image upload:', files);
    console.log('image upload\'s editable:', $scope.editable);
  }
}
```

```html
<summernote on-init="init()" on-enter="enter()" on-focus="focus(evt)"
            on-blur="blur(evt)" on-paste="paste()" on-keyup="keyup(evt)"
            on-keydown="keydown(evt)" on-change="change(contents)"
            on-image-upload="imageUpload(files)" editable="editable" editor="editor">
</summernote>
```

If you use `$editable` object in `onImageUpload` or `onChange`
(see [summernote's callback](http://summernote.org/#/features#callbacks)),
you should define `editable` attribute and use it in `$scope`.
(Because [AngularJS 1.3.x restricts access to DOM nodes from within expressions](https://docs.angularjs.org/error/$parse/isecdom))

Since summernote v0.6.4, APIs have been changed. So, If you use the verions,
`onImageUpload` is not return `editor` object anymore. If you want to user
`editor` object, you should define `editor` attribute and use it in `$scope`.
Futhermore, you can use summernote's APIs via the `editor` object.

### i18n Support

If you use i18n, you have to include language files.
See [summernote's document](http://summernote.org/#/features#i18n)
for more details.
And then you can specify language like:

```html
<summernote lang="ko-KR"></summernote>
```

## FAQ

- __How to solve compatibility problem with AngularUI Bootstrap?__

[AngularUI Bootstrap](http://angular-ui.github.io/bootstrap/) module is
written to replace the JavaScript file for bootstrap with its own
implementation (`ui-bootstrap-tpls.min.js`).

Summernote was intended to work with Bootstrap, so the coder implemented
features that rely on the `bootstrap.js` file being present.

* If you do not include `bootstrap.js`, summernote throws exceptions.
* If you do not include `ui-bootstrap-tpls.min.js`, your angular directives
  for bootstrap will not work.
* If you include both, then both JavaScript files try to listen on various
  events, and otherwise may have incompatibility issues.

If you have a drop down in the navbar, and use `data-dropdown` directive
as bootstrap says to, then two clicks are required to open
the drop down (menu) instead of the expected one click.

The solution is to not use `data-dropdown` directive. However, the
real solution is for summernote to be agnostic about which of
`bootstrap.js` or `ui-bootstrap-tpls.min.js` are loaded and make the right calls.
(see [#21](https://github.com/summernote/angular-summernote/issues/21))

## Change Logs

See [here](https://github.com/summernote/angular-summernote/blob/master/CHANGELOG.md).
