# angular-summernote - [AngularJS](http://angularjs.org/) directive to [Summernote](http://summernote.org/)

***

[![Build Status](https://travis-ci.org/summernote/angular-summernote.png?branch=master)](https://travis-ci.org/summernote/angular-summernote)
[![Dependency Status](https://gemnasium.com/summernote/angular-summernote.png)](https://gemnasium.com/summernote/angular-summernote)
[![Coverage Status](https://coveralls.io/repos/summernote/angular-summernote/badge.png)](https://coveralls.io/r/summernote/angular-summernote)

角summernote只是一个指令绑定summmernote的所有功能。
你可以使用summernote角的方法.

**Since v0.7.x, the version of angular-summernote follows the version of summernote.
So, angular-summernote v0.7.x are compatible with summernote v0.7.x and
and angular-summernote v0.8.x will be compatible with summernote v0.8.x.
Angular-summernote will match only `major.minor` with summernote.
Therefore, angular-summernote v0.7.0 will be compatible with summernote v0.7.0, v0.7.1 and
v0.7.2. Angular-summernote will release patch update, such as v0.7.1, if only angular-summernote has changed.**

## 表的内容

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

## 演示

看 [JSFiddle](http://jsfiddle.net/outsider/n8dt4/322/embedded/result%2Chtml%2Cjs%2Ccss/)
或在项目中运行的例子（需要运行 `bower install` 在运行)

## 安装

angular-summernote 要求所有包括文件 [Summernote](http://summernote.org/).
看 [Summernote's installation](http://summernote.org/#/features#installation).

项目文件也可以通过您最喜欢的包管理器:

* Bower: `bower install angular-summernote`

## 如何使用

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

### 选项

summernote的选项可以指定为属性.

#### 高度

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

你可以使用 `onMediaDelete` 回调。这个 `target` 对象的DOM，删除信息:

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

注: `height` 和 `focus` 属性比选项具有高优先级对象.

注：自定义工具栏可以通过选项对象设置.

### ngModel

summernote's `code`, 这是summernote HTML字符串。
如果你指定ngmodel将2结合
在两个summernote HTML字符串。另有 `angular-summernote` 简单地忽略它.

```html
<summernote ng-model="text"></summernote>
```

```javascript
function DemoController($scope) {
  $scope.text = "Hello World";
}
```

你可以使用 [ngModelOptions](https://docs.angularjs.org/api/ng/directive/ngModelOptions)
角V1.3 +。所以，你可以更新ngmodel模糊事件时发出的或与去抖延时
如果你想要。

### 事件侦听器

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

如果你使用 `$editable` 对象 `onImageUpload` 或 `onChange`
(看 [summernote's callback](http://summernote.org/#/features#callbacks)),
你应该定义 `editable` 属性并使用它 `$scope`.
(Because [AngularJS 1.3.x restricts access to DOM nodes from within expressions](https://docs.angularjs.org/error/$parse/isecdom))

因为summernote v0.6.4，API已经改变。所以，如果你使用的版本，
`onImageUpload` 是没有回报的 `editor` 对象了。如果你想用户
`editor`对象，你应该定义 `editor` 属性并使用它 `$scope`.
Futhermore，你可以用summernote的API通过 `editor` 对象.如果你使用I18N，你必须包括语言文件。

看到

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
