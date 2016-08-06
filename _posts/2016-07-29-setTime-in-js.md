---
layout: post
title: "JavaScript中的定时器"
keywords: ["JavaScript","setTimeout","js","setTimeInterval"]
description: "详细解读JavaScript中的定时器用法：setTimeout()、setTimeInterval()"
category: "JavaScript"
tags: ["JavaScript"]
---
{% include JB/setup %}

JavaScript 是单线程语言，可以设置超时值和间歇时间值来调度代码在特定时间执行。

##超时调用 setTimeout()

###语法

<pre>
setTimeout(codeStr|func,delay)
</pre>

其中，第一个参数可以是一个包含 JavaScript 代码的字符串，也可以是一个函数；

要时刻记住，JavaScript 是单线程的，所以同一时间只能执行一个任务，后续任务必须排队，等到前面任务执行之后才去执行。第二个参数告诉 JavaScript 再过多少毫秒把当前任务添加到任务队列中。如果队列是空的，添加的代码会立即执行，如果队列非空，要等前面代码执行之后再执行。

不推荐 ↓↓
<pre>
setTimeout("alert('Hey,girls!')",1000);
</pre>

推荐 ↓↓
<pre>
setTimeout(function(){
    alert('Hey,girls!');
},1000);
</pre>

虽然这两种调用方法都没有问题，但传递字符串会造成性能损失，因此不建议字符串作为第一个参数。

###取消超时调用 clearTimeout()

调用 setTimeout() 后，该方法会返回一个数值ID，表示超时调用，这个超时调用ID是计划执行代码的唯一标识符，可以通过它来取消超时调用。

<pre>
function showGirl(){
    alert("Selina ❤️ ");
}
var timeoutId = setTimeout(showGirl,1000);
clearTimeout(timeoutId);
</pre>

上面的代码在指定的时间尚未过去之前调用了 clearTimeout()，完全取消了超时调用，结果就跟什么都没发生一样。

###this

超时调用的代码都是在全局作用域中执行的，函数中的 this 指向 window。

<pre>
window.girl = "Selina";
var obj = {
    girl: "Hebe",
    showGirl: function(){
        setTimeout(function(){
            alert(this.girl);
        },500);
    }
}
obj.showGirl();	//"Selina"
</pre>

绑定当前作用域中的 this 值，解决方案之一是使用 bind()。

<pre>
window.girl = "Selina";
var obj = {
    girl: "Hebe",
    showGirl: function(){
        setTimeout(function(){
            alert(this.girl);
        }.bind(this),500);
    }
}
obj.showGirl();	//"Hebe"
</pre>

##间歇调用 setInterval()

###语法

<pre>
setInterval(codeStr|func,delay)
</pre>

参数解释同`setTimeout()`。

不推荐 ↓↓
<pre>
setInterval("alert('Hey,girls!')",1000);
</pre>

推荐 ↓↓
<pre>
setInterval(function(){
    alert('Hey,girls!');
},1000);
</pre>

###描述

间歇调用与超时调用类似，只不过它会按照指定的时间间隔重复执行代码，直至间歇调用被取消或者页面被卸载。

###取消间歇调用 clearInterval()

调用 setInterval() 会返回一个间歇调用ID，该ID可以用于在将来某个时刻取消间歇调用。

<pre>
clearInterval(间歇调用ID)
</pre>

取消间歇调用的重要性远远高于取消超时调用，因为不加感是的情况下，间歇调用将会一直执行到页面卸载。

举个 🌰：

<pre>
var num = 0;
var max = 3;
var intervalId = null;

function incrementNumber(){
    num++;
    //当执行次数达到max，取消后续调用
    if(num === max){
        clearInterval(intervalId);
        alert("num: "+ num + ",Done!");
    }
}
intervalId = setInterval(incrementNumber,500);
</pre>

上面的代码也可以用超时调用这样改写：

<pre>
var num = 0;
var max = 3;

function incrementNumber(){
    num++;
    //当执行次数没有达到max，设置另一次超时调用
    if(num < max){
        setTimeout(incrementNumber,500);
    }else{
        alert("num: "+ num + ",Done!");
    }
}
setTimeout(incrementNumber,500);
</pre>

开发环境下，很少真正使用间歇调用，因为后一个间歇调用可能会在前一个结束之前启动。类似情境下，最好用超时调用代替。

###this

同`setTimeout()`。

##测试

看本文中代码的演示效果，请移步[这里](http://blog.ilanyy.com/example/setTime/)。