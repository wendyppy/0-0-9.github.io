---
layout: post
title: "[转] less使用实例总结"
keywords: ["less ","css"]
description: "less使用实例总结"
category: "Frame"
tags: ["less"]
---
{% include JB/setup %}

**定义：**

less 将 css 赋予了动态语言的特性，如变量、继承、运算、函数等，less 可以在客户端运行，也可以在服务端运行。

引入：

less 应用的样式文件后缀名为 `.less`，引入样式表,然后再引入 `less.js`，然后通过 localhost 打开页面，即可看到效果：

    <link rel="stylesheet/less" type="text/css" href="style.less" />
    
    <script type="text/javascript" src="less.js"></script>
	<!----text/javascript 次序不能反---------->
<br><br>
**定义全局设置：**


在 `less.js` 引入前加入一段 js 语句定义 less 全局编译设置：

    <script type="text/javascript">
    
    less = {
    
    env: "development", // or "production"
    
    async: false,   // load imports async
    
    fileAsync: false,   // load imports async when in a page under a file protocol
    
    poll: 1000, // when in watch mode, time in ms between polls
    
    functions: {},  // user functions, keyed by name
    
    dumpLineNumbers: "comments", // or "mediaQuery" or "all"
    
    relativeUrls: false,// whether to adjust url's to be relative
    
    // if false, url's are already relative to the entry less file
    
    rootpath: ":/a.com/"// a path to add on to the start of every url resource
    
    };
    
    </script>
<br><br>
**定义函数：**

官方文档上称之为类，我觉得更像一个函数定义的方式。

    .default(@color){
    
      background-color:@color;
    
      border:1px solid @color;
    
    }

这样就定义了一个函数 default，前面的"`.`"必须加上，使用这个函数就直接在要使用这个样式的元素后调用就可以。

    .con1{default(red)};

这样就给 class 为 con1 的元素使用了函数 default 定义的一组样式，background-color 和 border 的颜色为红色。

函数的参数可以设定一个默认值，调用的时候不传值，就使用默认值：

    .default(@color：red){
    
      background-color:@color;
    
      border:1px solid @color;
    
    }

还可以定义一个变量，然后在函数定义的时候直接传变量：

    @re:red;    

    @ye:yellow;
       


    .con1{default(@ye)}; 
 
    .con2{default(@re)};

函数体内定义的变量作用域也只是函数体内。

    @yu:pink;
    
    @col:yellow;
    
    .default(@color){
    
      @yu:red;
    
      background-color:@color;
    
      border:1px solid @yu;
    
    }
    
    .default2(@color){
    
      @yu:yellow;
    
      background-color:@color;
    
      border:1px solid @yu;
    
    }
    
     
    
    .con1{.default(@col)};
    
    .con2{.default2(@yu)};

还可以使用 @arguments 来引用所有传入的变量。

    .border(@a,@b,@color:blue){
    
      border:@arguments;
    
    }
    
     
    
    .con2{.border(1px,solid)}

输出为：`.con2{1px solid blue}`

还可以使用参数的控制位来控制相同的类输出不同的样式：

    .border(cool,@color){
    
      border:2px solid @color;
    
    }
    
    .border(hot,@color){
    
      border:1px solid @color
    
    }
    
    .border(@_,@ye){
    
      color:@ye;
    
    }

调用：

    .con2{.border(hot,red)}
    
    .con1{.border(cool,blue)}

输出为：

    .con2(border:1px solid red;color:red)
    
      .con1{border:2px solid blue;color:blue}

或者使用参数个数来控制输出那个类：

    .border(@a){….}
    
    .border(@a,@b){...}

如果传入1个参数则调用第一个定义，传入2个则调用第二个定义。

<br><br>
**层级嵌套定义：**

比如 class 为 con1 的 div 下的 ul 下的 li 项中的 a 链接 hover 的 color 为 red ，不加下划线。

    div.con1{
    
     ul{
    
       li{
    
     a{text-decoration: none;
    
       &:hover{color:red};
    
     	}
    
      }
    
     }
    
    }

<br><br>
**条件语句判断：**

可以在类函数定义时候使用条件判断。

    .border(@a) when (@a>10),(@a<3){
    
      border:@a solid blue;
    
    }
    
    .con1{.border(5px)}

这里的条件是大于10或者小于3，所以调用不成立。

    .border(@a) when (@a>10) and (@a<15){
    
      border:@a solid blue;
    
    }
    
    .con1{.border(12px)}

这里的条件是大于10并且小于15 ，调用成立，条件语句中那个 px 可以加也可以不加，判断都通过。

还可以使用内置函数 unit 来增加或者取出单位：

    .border(@a) {
    
      border:unit(@a,px) solid red;
    
    }
    
     .con2{.border(5)}

输出` .con2{border:5px solid red;}`，如果写成 `unit(5px) ` 则去掉单位输出5。

<br><br>
**&可以用于代表父选择器**


    p { font-size: 12px;
    
    a { text-decoration: none;
    
      &:hover { border-width: 1px }
    
    }

代表了：

    p {
    
      font-size: 12px;
    
    }
    
    p a {
    
      text-decoration: none;
    
    }
    
    p a:hover {
    
      border-width: 1px;
    
    }

<br><br>

**less 内置函数：**


除了上面介绍的 unit 外，


>     ceil(@number);   // 向上取整
>     
>     floor(@number);  // 向下取整
>     
>     percentage(@number); // 将浮点数转换为百分比，例如 0.5 -50%
>     
>     round(number, [places: 0]);  // 四舍五入取整
>     
>     saturate(@color, 10%);   // 饱和度增加 10%
>     
>     desaturate(@color, 10%); // 饱和度降低 10%
>     
>     lighten(@color, 10%);// 亮度增加 10%
>     
>     darken(@color, 10%); // 亮度降低 10%
>     
>     fadein(@color, 10%); // 透明度增加 10%
>     
>     fadeout(@color, 10%);// 透明度降低 10%

<br><br>
其他的详见 less 中国官网： [http://www.lesscss.net/](http://www.lesscss.net/)























