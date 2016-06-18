---
layout: post
title: "less基础语法-variable&extend"
keywords: ["less ","css"]
description: "less基础语法:变量&extend详解"
category: "Frame"
tags: ["less"]
---
{% include JB/setup %}

less 是 css 的一种扩展形式，它并没有阉割 css 的功能，而是在现有的 css 语法上，添加了很多额外的功能。就语法规则而言，less 和 Sass 一样，都是使用 css 的标准语法，只是 less 的源文件的扩展名是 “`.less`”。
###变量
变量的作用就是把值定义在一个地方，然后在各处使用，这样能让代码更易维护：

    @nice-blue: #5B83AD;
    @light-blue: @nice-blue + #111;
    
    #header {
      color: @light-blue;
    }

编译为：

    #header {
      color: #6c94be;
    }

变量不仅可以用在属性值上，也可以用在选择元素名，属性名(1.6.0支持)，url 和 import 方法。

**选择元素名：**

    // Variables
    @mySelector: banner;
    // Usage
    <a href="mailto:.@{mySelector">.@{mySelector</a>} {
    font-weight: bold;
    line-height: 40px;
    margin: 0 auto;
    }

编译后为

    .banner {
    font-weight: bold;
    line-height: 40px;
    margin: 0 auto;
    }

**url：**

    // Variables
    @images: "../img";
    // 用法
    body {
    color: #444;
    background: url("@{images}/white-sand.png");
    }

编译后为

    body {
    color: #444;
    background: url("../img/white-sand.png");
    }

**@import:**

    // Variables
    @themes: "../../src/themes";
    // Usage
    @import "@{themes}/tidal-wave.less";

编译后为

    @import "../../src/themes/tidal-wave.less";

**属性名：**

    @property: color;
    .widget {
    @{property}: #0ee;
    <a href="mailto:background-@{property">background-@{property</a>}: #999;
    }

编译后为

    .widget {
    color: #0ee;
    background-color: #999;
    }

变量的变量名也可以是变量：

    @fnord:  "I am fnord.";
    @var:"fnord";
    content: @@var;

编译后为

    content: "I am fnord.";

**延迟加载：** 变量支持延迟加载，所以你可以在变量定义之前使用。如：

    .lazy-eval {
    width: @var;
    }
    @var: @a;
    @a: 9%;

或者

    .lazy-eval-scope {
    width: @var;
    @a: 9%;
    }
    @var: @a;
    @a: 100%;

都会被编译成：

    .lazy-eval-scope {
    width: 9%;
    }

问什么第二个也会被编译成上面的 css，这是因为当一个变量被定义两次时，最后一次定义的生效。就类似于 css 中，对同一个元素定义不同的 css 样式，最后定义的生效。再比如下面这个：

    @var: 0;
    .class1 {
    	@var: 1;
    	.class {
    		@var: 2;
    		three: @var;
    		@var: 3;
    	}
    	one: @var;
    }

编译后为

    .class1 .class {
    three: 3;
    }
    .class {
    one: 1;
    }


###Extend
扩展选择器是less的伪类选择器，他会复制当前选择器，定义新的样式。extend 可以附加给一个选择器，也可以放入一个规则集中。它看起来像是一个带选择器参数的伪类，也可以使用关键字 all 选择相邻的选择器。

    nav ul {
    &:extend(.inline);
    background: blue;
    }
    .inline {
    color: red;
    }

编译后为

    nav ul {
    background: blue;
    }
    .inline,
    nav ul {
    color: red;
    }

**语法：**

>     .a:extend(.b) {}
>     
>     // 上面的代码块与下面这个做一样的事情
>     .a {
>       &:extend(.b);
>     }

    .c:extend(.d all) {
      // 扩展".d"的所有实例，比如".x.d"或者".d.x"
    }
    .c:extend(.d) {
      // 扩展选择器输出为".d"的唯一实例
    }

它可以包含多个要扩展的类，使用逗号分割即可。

    .e:extend(.f) {}
    .e:extend(.g) {}
    
    // 上面的代码与下面的做一样的事情
    .e:extend(.f, .g) {}

**给选择器附加扩展**

一个选择器可以包含多个扩展分支，但是所有的扩展都必须在选择器的尾部。

- 选择器之后的扩展：`pre:hover:extend(div pre)。`
- 在选择器和扩展之间有空格是允许的：`pre:hover :extend(div pre)`.
- 也允许有多个扩展:` pre:hover:extend(div pre):extend(.bucket tr)` 注意这与 `pre:hover:extend(div pre, .bucket tr)` 一样。
- 不允许: `pre:hover:extend(div pre).nth-child(odd)`。因为扩展必须在最后。

如果一个规则集包含多个选择器，所有选择器都可以使用 extend 关键字。下面演示了一个规则集中多个带 extend 的选择器：

    .big-division,
    .big-bag:extend(.bag),
    .big-bucket:extend(.bucket) {
      // body
    }

也可以使用 `&:extend(selector)` 语法在规则集内置入 extend 。将 extend 放入规则集内是一种将它放入单个规则选择器的快捷方式。

规则内的 extend：

    pre:hover,
    .some-class {
      &:extend(div pre);
    }

与给每个选择器添加一个 extend 完全相同：

    pre:hover:extend(div pre),
    .some-class:extend(div pre) {}

**嵌套选择器：**

extend 还可以匹配嵌套选择器，比如有下面的 less：

    .bucket {
      tr { // 目标选择器中的嵌套规则
    color: blue;
      }
    }
    .some-class:extend(.bucket tr) {} // 识别嵌套规则

这会输出：

    .bucket tr,
    .some-class {
      color: blue;
    }

从本质上讲 extend 会查找编译后的 css ，而不是原始的 less。

    .bucket {
      tr & { // 目标选择器中的嵌套
    color: blue;
      }
    }
    .some-class:extend(tr .bucket) {} // 识别嵌套规则

输出：

    tr .bucket,
    .some-class {
      color: blue;
    }

**精确匹配：**

extend 默认会在选择器之间寻找精确匹配。它不管选择器是不是以星号开始；它也不管两个 nth 表达式是否具有相同的意义，它们必须以相同的形式匹配。唯一例外的是属性选择器中的引号，less 会知道它们是相同的，然后匹配它。

    .a.class,
    .class.a,
    .class > .a {
      color: blue;
    }
    .test:extend(.class) {} // 不会匹配上面的任何选择器的值

开头的 ' `*` ' 号会影响匹配结果。`*.class` 和 `.class` 是等价的，但 extend 不会匹配它们。

    *.class {
      color: blue;
    }
    .noStar:extend(.class) {} 不会匹配*.class选择器

输出：

    *.class {
      color: blue;
    }

伪类的顺序也会影响匹配结果。选择器 `link:hover:visited` 和 `link:visited:hover` 匹配相同的元素集合，但是 extend 会区别对待它们：

    link:hover:visited {
      color: blue;
    }
    .selector:extend(link:visited:hover) {}

输出：

    link:hover:visited {
      color: blue;
    }

**nth 表达式：**

nth 形式的表达式会影响匹配结果。nth 表达式 `1n+3` 和 `n+3` 是等价的，但是 extend 并不能匹配它们：

    :nth-child(1n+3) {
      color: blue;
    }
    .child:extend(n+3) {}

输出：

    :nth-child(1n+3) {
      color: blue;
    }

属性选择器中的引号类型也是有关系的。以下所有都是等价的：

    [title=identifier] {
      color: blue;
    }
    [title='identifier'] {
      color: blue;
    }
    [title="identifier"] {
      color: blue;
    }
    
    .noQuote:extend([title=identifier]) {}
    .singleQuote:extend([title='identifier']) {}
    .doubleQuote:extend([title="identifier"]) {}

输出：

    [title=identifier],
    .noQuote,
    .singleQuote,
    .doubleQuote {
      color: blue;
    }
    
    [title='identifier'],
    .noQuote,
    .singleQuote,
    .doubleQuote {
      color: blue;
    }
    
    [title="identifier"],
    .noQuote,
    .singleQuote,
    .doubleQuote {
      color: blue;
    }

**extend "all"**

    .a.b.test,
    .test.c {
      color: orange;
    }
    .test {
      &:hover {
    color: green;
      }
    }
    
    .replacement:extend(.test all) {}

输出：

    .a.b.test,
    .test.c,
    .a.b.replacement,
    .replacement.c {
      color: orange;
    }
    .test:hover,
    .replacement:hover {
      color: green;
    }

你可以认为这种操作模式就是无损搜索和替换。

**extend 中的选择器插值**

extend不能匹配变量选择器。如果选择器包含变量，extend会忽略它。

这是一个悬而未决的特性，改变它并不容易。然而，extend可以附加给插值选择器。

带变量的选择器不会匹配：

    @variable: .bucket;
    @{variable} { // 插值选择器
      color: blue;
    }
    .some-class:extend(.bucket) {} // 找不到匹配

同时在 extend 中使用目标选择器变量也什么都不匹配：

    .bucket {
      color: blue;
    }
    .some-class:extend(@{variable}) {} // 插值选择器什么也不匹配
    @variable: .bucket;

上面两个例子都会编译为：

    .bucket {
      color: blue;
    }

然而, `:extend` 附加给插值选择器是能够工作的：

    .bucket {
      color: blue;
    }
    @{variable}:extend(.bucket) {}
    @variable: .selector;

上面的例子会编译为：

    .bucket, .selector {
      color: blue;
    }

**作用域 @media 内的 extend**

编写在 media 声明内的 extend 也应该只匹配同一 media 声明内的选择器：

    @media print {
      .screenClass:extend(.selector) {} // media内的extend
      .selector { // 这个会匹配到-因为在同一的media内
    color: black;
      }
    }
    .selector { // 定义样式表中的规则 - extend会忽略它
      color: red;
    }
    @media screen {
      .selector {  // 另一个media声明内的规则 - extend也会忽略它
    color: blue;
      }
    }

最终编译为：

    @media print {
      .selector,
      .screenClass { /*  同一media内的规则扩展成功 */
    color: black;
      }
    }
    .selector { /* 定义样式表中的规则被忽略 */
      color: red;
    }
    @media screen {
      .selector { /* 其他media中的规则也被忽略 */
    color: blue;
      }
    }

编写在 media 声明内的 extend 不会匹配嵌套声明内的选择器：

    @media screen {
      .screenClass:extend(.selector) {} // media内的extend
      @media (min-width: 1023px) {
    .selector {  // 嵌套media内的规则 - extend会忽略它
      color: blue;
    }
      }
    }

编译为：

    @media screen and (min-width: 1023px) {
      .selector { /* 其他嵌套media内的规则被忽略 */
    color: blue;
      }
    }

顶级 extend 匹配一切，包括 media 嵌套内的选择器：

    @media screen {
      .selector {  /* media嵌套内的规则 - 顶级extend正常工作 */
    color: blue;
      }
      @media (min-width: 1023px) {
    .selector {  /* media嵌套内的规则 - 顶级extend正常工作 */
      color: blue;
    }
      }
    }
    
    .topLevel:extend(.selector) {} /* 顶级extend匹配一切 */

编译为：

    @media screen {
      .selector,
      .topLevel { /* media嵌套内的规则被扩展了 */
    color: blue;
      }
    }
    @media screen and (min-width: 1023px) {
      .selector,
      .topLevel { /* media嵌套内的规则被扩展了 */
    color: blue;
      }
    }

**检测重复**

现在，这里还没有检测重复。

示例：

    .alert-info,
    .widget {
      /* declarations */
    }
    
    .alert:extend(.alert-info, .widget) {}

输出：

    .alert-info,
    .widget,
    .alert,
    .alert {
      /* declarations */
    }

**extend 用例**

一、经典用例

经典用于就是避免添加基础类。比如，如果你有：

    .animal {
      background-color: black;
      color: white;
    }

如果你想有一个 animal 子类型，并且要重写背景颜色。那么你有两个选择，首先改变你的 html：

    <a class="animal bear">Bear</a>


>     .animal {
>       background-color: black;
>       color: white;
>     }
>     .bear {
>       background-color: brown;
>     }

或者简化 html，然后在你的 less 中使用 extend，比如：

    <a class="bear">Bear</a>

>     .animal {
>       background-color: black;
>       color: white;
>     }
>     .bear {
>       &:extend(.animal);
>       background-color: brown;
>     }

二、css 尺寸归并

mixins 会复制所有的属性到选择器中，这可能导致不必要的重复。因此你可以使用 extend 来代替 mixin 将你要用的属性移过去，这样就会生成更少的 css。

mixin 示例：

    .my-inline-block() {
    display: inline-block;
      font-size: 0;
    }
    .thing1 {
      .my-inline-block;
    }
    .thing2 {
      .my-inline-block;
    }

输出：

    .thing1 {
      display: inline-block;
      font-size: 0;
    }
    .thing2 {
      display: inline-block;
      font-size: 0;
    }

extend 示例：

    .my-inline-block {
      display: inline-block;
      font-size: 0;
    }
    .thing1 {
      &:extend(.my-inline-block);
    }
    .thing2 {
      &:extend(.my-inline-block);
    }

输出：

    .my-inline-block,
    .thing1,
    .thing2 {
      display: inline-block;
      font-size: 0;
    }

三、合并样式/更高级的 mixin

另一个用例可以用作 mixin 的替代 - 因为 mixin 仅仅能用于简单的选择器，如果你的 html 中有两个不同的块，但是你需要为这两个块应用相同的样式，那么你可以使用 extend 来关联这两块。

示例：

    li.list > a {
      // list styles
    }
    button.list-style {
      &:extend(li.list > a); // 使用相同的列表样式
    }








