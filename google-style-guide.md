# Google HTML/CSS 编码规范

## 通用规范

### 协议

**内嵌资源URLs的协议部分省略不写**

除非内嵌资源不合适在两种协议(http,https)都能被加载到。否则，书写指向内嵌资源文件
的URLs的时候应该省略指定具体协议的那一部分(http:,https:)。

内嵌资源URL中省略不写的协议部分，浏览器自动根据当前HTML页面的URL推测出来。避免由
于协议书写不一致，导致错误协议的资源被加载进来使用。

> DRY(Don't Repeat Yourself)原则指出，任何知识点在系统内都应该有一个唯一，明确，
> 权威的表述。
>
> 如果代码中含有重复数据是因为在两个不同的地方必须使用两个不同的表现形式，可以让
> 其中一个由另一个生成。
>
> 省略协议部分，将单个HTML页面所包含的所有内嵌资源的加载协议，统一为当前HTML页面
> 的协议，并由其统一生成其内嵌资源的加载协议。

```css
<!-- 不推荐写法 -->
<script src="http://www.google.com/js/gweb/analytics/autotrack.js">

<!-- 推荐写法 -->
<script src="/js/gweb/analytics/autotrack.js">

/* 推荐写法 */
.example {
  background: url(http://www.google.com/images/example);
}

/* 不推荐写法 */
.example {
  background: url(/images/example);
}
```
---


## 通用书写格式

### 缩进

**使用两个空格替换缩进**

避免使用制表符(tab)或者制表符和空格混用方式作为缩进。

### 字母大小写

**统一使用小写**

HTML和CSS的符号统一使用小写：包括HTML的标签和属性以及属性数值，CSS的选择符和属性
性值。

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

### 行末空格

**去掉行末不用的空格**

去掉行末的空格，可以避免`diffs`比较时候的困扰。

### EditorConfig

通过`.editorconfig`文件和编辑器插件的方式，上述书写规范可以自动化进行。

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

**EditorConfig 官网**  <http://editorconfig.org)>

---

## 通用Meta书写规则

### 编码

**使用UTF-8编码**

确保你所使用的文本编辑器使用的是UTF-8编码，而不是单纯的字节组合。
通过`<metacharset="utf-8">`指定HTML所使用的编码。

样式表可以通过`@charset "UTF-8"`或者是HTTP headers两种方式显式的指定编码。
如果样式表中的内容没有超出ASNII编码的范围，不必显示指定样式表的编码。

**编码和指定编码相关内容** <https://www.w3.org/International/tutorials/tutorial-
char-enc/#quicksummary>

### 注释

**在代码片段需要特殊说明的时候，使用注释**

使用注释说明代码：代码的功能，代码所希望达到的目标，使用的解决方案以及其优势。
(是否需要注释取决于项目需要，而不是注释每一行代码。应该视项目复杂性和代码修改频
率而定)

### 待完成项

**通过TODO来提示这是一个待完成项。**

TODO后面的括号可以用来提示联系人信息(用户名或者是邮件列表)，例：`TODO(contact)`。

TODO后面冒号是待完成的具体事项。例：`TODO: 待完成项`。

```html
<!-- TODO: 移除任意个标签 -->
<ul>
  <li>Apples</li>
  <li>Oranges</li>
</ul>
```

---

## HTML书写规范

### 文档申明

**使用HTML5的文档申明**

对于HTML文档来说更推荐使用HTML5的文档申明(<!DOCTYPE html>)。

(推荐将HTML文档的`Content-Type`设置为`text/html`，而不是`application/xhtml+xml`
。后者缺乏浏览器的支持，与HTML文档相比语法上更为僵化。)

常见的情况是，未闭合标签(<br>)在XHTML文档中是不被允许的。

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

### 语义化

**使用HTML标签的时候应该语义化**

