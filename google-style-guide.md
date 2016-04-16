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
> 的协议，并由其统一生成内嵌资源的加载协议。

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

去掉行末的空格，可以避免diff比较时候的困扰。

### EditorConfig

上述书写规范通过.editorconfig文件和编辑器插件的方式自动化的完成。

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

[EditorConfig 官网](http://editorconfig.org)

---


