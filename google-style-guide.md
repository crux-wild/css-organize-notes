# Google HTML/CSS 编码规范

## 通用规范

### 协议

**内嵌资源URLs的协议部分省略不写**

除非内嵌资源不合适在两种协议(`http`，`https`)都能被加载到。否则，书写指向内嵌资
源文件的URLs的时候应该省略指定具体协议的那一部分(`http:`，`https:`)。

内嵌资源URL中省略不写的协议部分，浏览器自动根据当前HTML页面的URL推测出来。避免由
于协议书写不一致，导致错误协议的资源被加载进来使用。

> DRY(Don't Repeat Yourself)原则指出，任何知识点在系统内都应该有一个唯一，明确，
> 权威的表述。
> 如果代码中含有重复数据是因为在两个不同的地方必须使用两个不同的表现形式，可以让
> 其中一个由另一个生成。
> 省略协议部分，将当前`HTML`页面所包含的所有内嵌资源的加载协议的概念，统一为当前
> HTML页面的协议，并由其统一生成其所包含内嵌资源的加载协议。

```css
<!-- 不推荐写法 -->
<script src="http://www.google.com/js/gweb/analytics/autotrack.js">

<!-- 推荐写法 -->
<script src="//www.google.com/js/gweb/analytics/autotrack.js">

/* 推荐写法 */
.example {
  background: url(http://www.google.com/images/example);
}

/* 不推荐写法 */
.example {
  background: url(//www.google.com/images/example);
}
```
---


## 通用书写格式

### 缩进

**使用两个空格替换缩进**

避免使用`tab`或者`tab`和`space`混用方式作为缩进。

```css
<!-- 不推荐写法 -->
<A HREF="/">HOME</A>

<!-- 推荐写法 -->
<img src="google.png" alt="Google">

/** 不推荐写法 **/
color: #E5E5E5;

/** 推荐写法 **/
color: #e5e5e5;
```

### 字母大小写

**统一使用小写**

HTML和CSS的符号统一使用小写：包括`HTML`的元素和属性以及属性数值，`CSS`的选择符和
属性性值。

### 行末空格

**去掉行末不用的空格**

去掉行末的空格，可以避免`diffs`比较时候的困扰。

### EditorConfig

**通用书写规范可以通过`.editorconfig`配置文件和文本编辑插件，自动完成**

`.editorconfig`配置文件如下：

```.editorconfig
# http://editorconfig.org

root = true

[*.{css, html}]
charset = utf-8
end_of_line = lf
indent_size = 2
indent_style = space
insert_final_newline = true
max_line_length = 80
trim_trailing_whitespace = true
```

**EditorConfig 官网**  <http://editorconfig.org>

### gulp-htmlhint

**与HTML内容有关的书写规范，可以使用`gulp-htmlhint`自动完成**

`gulpfile.js`配置文件如下：

```javascript
var gulp = require('gulp');
var htmlhint = require('gulp-htmlhint');

var htmlWatcher;

gulp.task('htmlhint', function() {
  gulp.src('./src/*.html')
    .pipe(htmlhint())
    .pipe(htmlhint.reporter());
});

htmlWatcher = gulp.watch('src/*.html', ['htmlhint']);
htmlWatcher.on('change', function(event) {
  console.log('File' + event.path + ' was' + event.type + ', running tasks ' +
    'htmlhint');
});
```

`.htmlhintrc`配置文件如下：

```javascript
{
  // 元素和属性使用统一使用小写
  tagname-lowercase: true,
  attr-lowercase: true,
}
```

**HTMLHint Github**  <https://github.com/yaniswang/HTMLHint>

---


## 通用Meta书写规则

### 编码

**使用UTF-8编码**

确保你所使用的文本编辑器使用的是`UTF-8`编码，而不是单纯的字节组合。
通过`<metacharset="utf-8">`指定HTML所使用的编码。

样式表可以通过`@charset "UTF-8"`或者是HTTP headers两种方式显式的指定编码。
如果样式表中的内容没有超出ASNII编码的范围，不必显示指定样式表的编码。

> 中文环境下，很多时候都会使用含有中文的内容，建议统一添加`@charset="utf-8"`。

**编码和指定编码相关内容** <https://www.w3.org/International/tutorials/tutorial-
char-enc/#quicksummary>

### 注释

**在代码片段需要特殊说明的时候，使用注释**

使用注释说明：

  - 代码的功能
  - 代码想要达到的效果
  - 代码所使用的解决方案及其优势

(是否需要注释取决于项目需要，而不是通篇都是注释。应该视项目复杂性和代码改动频率
而定。)

### 待完成项

**通过`TODO`来提示这是一个待完成项。**

`TODO`后面的括号可以用来提示联系人信息(用户名或者是邮件列表)，例：`TODO(contact)`。

`TODO`后面冒号是待完成的具体事项。例：`TODO: 待完成项`。

```html
<!-- TODO: 移除任意个元素 -->
<ul>
  <li>Apples</li>
  <li>Oranges</li>
</ul>
```

---

## HTML书写规范

### 文档申明

**使用HTML5的文档申明**

对于HTML文档来说更推荐使用HTML5的文档申明(`<!DOCTYPE html>`)。

(推荐将HTML文档的`Content-Type`设置为`text/html`，而不是`application/xhtml+xml`
。后者缺乏浏览器的支持，与HTML文档相比语法上更为僵化。)

常见的情况是，未闭合元素(`<br>`)在XHTML文档中是不被允许的。

`.htmlhintrc`配置文件如下：

```javascript
{
  // 使用html5风格的文档申明
  doctype-html5: true
}
```

### HTML内容检验

**HTML内容尽量合乎语法**

语法不正确的HTML文档，解析的时候会需要额外的容错处理，会降低解析过程的效率。

HTML内容是否存在语法错误，是评价HTML代码质量的重要标注。语法正确代码内容可以避免
一些意象不到错误。

> HTML遵循的是"对输入宽容"的原则，即使接受的HTML文档不是很规范，HTML解析器也可以
> 尽量理解其中的意义。但是，HTML的容错算法对绝大多数用户来说，是不透明的。带来问
> 题是经过容错处理后的HTML解析结果很难被我们所理解。进行HTML内容校验正是为了避免
> 类似问题的发生。

```html
<!-- 不推荐写法 -->
<title>Test</title>
<article>This is only a test.

<!-- 推荐写法 -->
<!DOCTYPE html>
<meta charset="utf-8">
<title>Test</title>
<article>This is only a test.</article>
```
**W3C HTML校验地址** <https://validator.w3.org/nu/#textarea>

### gulp-w3cjs

**校验HTML的语法错误，可以通过`gulp-w3cjs`的方式自动化完成**

gulpfile.js配置如下：

```javascript
var w3cjs = require('gulp-w3cjs');

gulp.task('w3cjs', function() {
  gulp.src('src/*.html')
    .pipe(w3cjs())
    .pipe(w3cjs.repoter());
});
```

### 语义化

**HTML元素应该语义化**

使用HTML标签(经常被错误的表述为标签)应该根据所包含内容的语义来选择。例如标题相关
的内容使用标题元素(`h1`~`h6`)，文本段落相关的内容使用`p`，需要使用页面跳转功能使
用`a`等等。

这样做一个重要的目的便是提高代码的易读性, 复用性，表现力。

```html
<!-- 不推荐写法 -->
<div onclick="goToRecommenddations();">All recommendations</div>

<!-- 推荐写法 -->
<a href="recommendations/">All recommendations</a>
```

### 多媒体替代文本

**为所有的多媒体内容提供一个替代文本**

诸如图片，视频，画布(`canvas`)，应该提供一个替代读写方案。如果是图片的话那就是一
段有意义的替换文本(`alt`)，对于视频和音频来说的话就是一段说明字幕。

提供替代内容，可以帮助脚本或者是搜索引擎更好去理解多媒体中的内容。

*如果一些图片仅仅是作为装饰作用而没有具体内容，可以没有`alt`属性或者属性为空(`al
t=''`)。*

```html
<!-- 不推荐写法 -->
<img src="spreadsheet.png">

<!-- 推荐写法 -->
<img src="spreadsheet.png" alt="Spreadsheet screenshot.">
```

`.htmlhintrc`配置文件如下：

```javascript
{
  'alt-require': true
}
```

### 职责分离

**`HTML`代码中避免与表现和行为相关的逻辑**

前端中`html`负责结构，`css`负责表现，`javascript`负责行为。

确保`HTML`文档和模板中包含与结构相关`HTML`代码。将样式相关逻辑放到`css`中，行为
相关逻辑放到`javascript`中去。

在`HTML`中处理样式和行为的逻辑需要更高的维护成本，职责分离的一个重要目的便是提高
代码的可维护性。

```html
<!-- 不推荐写法 -->
<!DOCTYPE html>
<title>HTML sucks</title>
<link rel="stylesheet" href="base.css" media="screen">
<link ref="stylesheet" href="grid.css" media="screen">
<link rel="stylesheet" href="print.css" media="print">
<h1 style="font-size: 1em;">HTML sucks</h1>
<p>I've read about this on a few sites but now I'm sure:
  <u>HTML is stupid!!</u>
<center>I can't believe there's no way to control the styling of
  my website without doing everything all over again!</center>

<!-- 推荐写法 -->
<!DOCTYPE html>
<title>My first Css-only redesign</title>
<link rel="stylesheet" href="default.css">
<h1>My first Css-only redesign</h1>
<p>I've read about this on a few sites but today I'm actucally
  doing it: separating concerns and avoiding anythig in the HTML of
  my website that is presentational.
</p>
```

`.htmlhintrc`配置文件如下：

```javascript
{
  // 禁止在HTML嵌入`javascript`或者`css`代码
  'head-script-disabled': true,
  'inline-style-disabled': true,
  'inline-script-disabled': true
}
```

### 字符实体

**避免使用字符实体**

如果只是为了表示(`UTF-8`)中非常用字符，例如：(`&mdash;`，`&rdquo;`，`&#x2663a;`。
应该避免使用字符实体。)

除了是`HTML`中的所必须的转义字符，(例如： `<`，`&`和空格)，可以使用字符实体。

```html
<!-- 不推荐写法 -->
The currency symbol for the Euro is &ldquo;&eur;&rdquo;.

<!-- 推荐写法 -->
The currency symbol for the Euro is “€”.
```

> 从示例可以看出，使用字符实体在易读和易写方面表现都比较差。而且，语法上也容易出
> 错。

`.htmlhintrc`配置文件如：

```javascript
{
  // 提示需要转义的字符
  'spec-char-escape': true
}
```

### 省略可选标签(不推荐)

```html
<!-- 示例 -->
<!DOCTYPE html>
<title>Saving money, saving bytes</title>
<p>Qed.
```

**可选标签列表：** <https://www.w3.org/International/tutorials/tutorial-char-enc
/>

> 根据清晰原则，"清晰胜于技巧"。省略可选标签虽然是`HTML5`规范所允许的。权衡省略
> 写法所带来的网络上的细微提升和由其造成可维护性降低的事实。这样的优化是不值得的
> 。

### 资源类型属性

**`javascript`和`css`资源类型省略不写**

样式表和脚本在`HTML5`标准中把`text/css`和`text/javascript`作为二者属性的默认值。
这个规则在旧版本浏览器中也成立。

```html
<!-- 不推荐写法 -->
<link rel="stylesheet" href="//www.google.com/css/maia.css" type="text/css">

<!-- 推荐写法 -->
<link rel="stylesheet" href="//www.google.com/css/maia.css">

<!-- 不推荐写法 -->
<script src="//www.google.com/js/gweb/analytics/autotrack.js"
  type="text/javascript"></script>

<!-- 推荐写法 -->
<script src="//www.google.com/js/gweb/analytics/autotrack.js"></script>
```
---


## HTML格式规则

### 通用格式规则

**独立`blockquote`，`ul`，`table`元素之间使用空行作为分隔。子元素相对其父元素应
该有两个空格的缩进**

```html
<blockquote>
  <p><em>Space</em>, the final frontier</p>
</blockquote>

<ul>
  <li>Moe</li>
  <li>Larry</li>
  <li>Curly</li>
</ul>

<table>
  <thead>
    <tr>
      <th scope="col">Income</th>
      <th scope="col">Taxes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>$ 5.00</td>
      <td>$ 4.50</td>
    </tr>
  </tbody>
</table>
```

### HTML标签的引号

**标签中的属性统一使用双引号(`""`)**

```html
<!-- 不推荐写法 -->
<a class='maia-button maia-button-secondary'>Sign in</a>

<!-- 推荐写法 -->
<a class="maia-button maia-button-secondary">Sign in</a>
```

`.htmlhintrc`配置文件如下:

```javascript
{
  'attr-value-double-quotes': true
}
```

---


## CSS 书写规则

### CSS内容校验

**对`CSS`进行内容校验**

剔除为了解决特定bugs和必须使用特殊语法两种情况。其余`css`内容都应该确保其能通过
语法校验。

能够通过语法校验是衡量`css`代码质量的重要标注。语法错误的`css`一般不会产生任何样
式效果并被解析器移除，语法正确能够确保定义样式生效。

**W3C CSS校验地址** <http://jigsaw.w3.org/css-validator/>

### gulp-w3c-css

**校验CSS内容的语法错误可以使用`gulp-w3c-css`自动化的完成**

`gulpfile.js`的配置文件如下：

```javascript
var w3c_css = require('gulp-w3c-css');

gulp.task('w3c-css', function() {
  gulp.src('src/*.css')
    .pipe(w3c_css())
    .pipe(gulp.dest('./build'));
});
```

> 暂时没有发现一个支持`reporter`方式输出的插件。

### ID和CLASS的命名规则

**使用表意和通用的`id`和`class`名称**

表意命名更能反映`id`和`class`的用途，优点是易于理解，缺点是不易于复用。

通用命名通常没有特殊的含义，可以被一组相似的元素所复用。通用命名多数时候不会单独
出现，而是组合使用补充说明该元素的通用特征。

```css
/** 不推荐写法：不表意 */
#yee-1901

/** 不推荐写法：复用性差 */
.button-green {}
.clear {}

/** 推荐写法：表意清晰 */
#galler {}
#login {}
.video {}

/** 推荐写法：命名通用 */
.aux {}
.alt {}
```

### 元素选择符

**避免在使用标签选择符**

除了需要使用标签选择符隔离作用域等情况，不要把元素选择与`id`选择符或者`class`选
择符链接使用。

```css
/** 不推荐 */
ul #exmaple {}
div .error {}

/** 推荐写法 */
#example {}
.error {}
```

### gulp-csslint

**`CSS`相关的书写规范校验可以通过，`gulp-csslint`自动化的完成**

`gulpfile.js`配置文件如下：

```javascript
var gulp = require('gulp');
var csslint = require('gulp-csslint');

var cssWatcher;

gulp.task('csslint', function() {
  gulp.src('./src/*.css')
    .pipe(csslint())
    .pipe(csslint.reporter());
});

cssWatcher = gulp.watch('src/*.css', ['csslint']);
cssWatcher.on('change', function(event) {
  console.log('File' + event.path + ' was' + event.type + ', running tasks ' +
    'csslint');
});
```

`.csslintrc`配置文件如下：

```javascript
{
  'overqualified-elements': true
}
```

### 简写属性值

**尽可能使用简写属性值**

推荐使用`css`提供的类似`font`属性的简写方式，即使可能只需要设置单一属性值。

使用属性值的简写方式可以使代码更简洁，更易读。

```css
/** 不推荐写法 */
border-top-style: none;
font-family: palatino, georgia, serif;
font-size: 100%;
line-height: 1.6;
padding-bottom: 2em;
padding-left: 1em;
padding-right: 1em;
padding-top: 0;

/** 推荐写法 */
border-top: 0;
font: 100%/1.6 palatino, georgia, serif;
padding: 0 1em 2em;
```

`.csslintrc`配置文件如下：
```javascript
{
  'shorthand': true
}
```

### 数值0

**省略数值为`0`属性值的单位**

```css
margin: 0;
padding: 0;
```

`.csslintrc`配置文件如下：

```javascript
{
  'zero-units': true
}
```
