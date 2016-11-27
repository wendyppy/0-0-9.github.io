---
layout: post
title: "underscore（一）内部函数"
keywords: ["underscore","JavaScript","源码","1.8.3"]
description: "underscore源码解读"
category: "underscore"
tags: ["JavaScript","underscore"]
---
{% include JB/setup %}

Underscore.js 是一个 JavaScript 工具包，加上注释也不到2000行，精巧实用，比较适合源码阅读入门。[这里](http://www.css88.com/doc/underscore/#)是中文文档，[这里](http://underscorejs.org/docs/underscore.html)是英文源码及注释，参考以上内容完成本系列。

###立即执行的函数表达式

Underscore.js 的最外层是这样的：

<pre>
(function(){
    //...
}.call(this));
</pre>

严格模式下，函数调用的 this 并不会指向全局对象，但像这样做了之后，就能确保函数运行时的 this 指向。

##基本设置 baseline setup

<pre>
//创建root对象，在浏览器中表现为window，服务器中表现为exports
var root = this;
// 保存之前已经存在的同名_属性，配合后面的_.noConflict函数使用，该函数会重命名underscore对象，避免命名冲突
var previousUnderscore = root._;
//为了压缩体积而做的局部变量缓存
var ArrayProto = Array.prototype,
    ObjProto = Object.prototype,
    FuncProto = Function.prototype;
var push = ArrayProto.push,
    slice = ArrayProto.slice,
    toString = ObjProto.toString,
    hasOwnProperty = ObjProto.hasOwnProperty;
// ES5中将要用到的所有原生函数都在此声明
var nativeIsArray = Array.isArray,
    nativeKeys = Object.keys,
    nativeBind = FuncProto.bind,
    nativeCreate = Object.create;
// 将被用于给新对象添加原型，为Object.create跨浏览器实现做准备
var Ctor = function() {};
// 创建一个安全的_对象引用。_是一个函数对象，所有的api都会挂载在其上。
var _ = function(obj) {
		 // 如果obj已经是_的实例，直接返回obj
        if (obj instanceof _) return obj;
         // 如果obj不是_函数的实例，则用new关键字返回_的实例化对象
        if (!(this instanceof _)) return new _(obj);
        this._wrapped = obj;
    }
    // 针对不同的宿主环境，添加_到不同的对象
    // Node.js环境下
if (typeof exports !== 'undefined') {
    if (typeof module !== 'undefined' && module.exports) {
        exports = module.exports = _;
    }
    exports._ = _;
} else {
    // 浏览器环境下_添加到window对象
    root._ = _;
}
// 版本
_.VERSION = '1.8.3';
</pre>

###optimizeCb

<pre>
/**
 * 优化回调函数，执行函数并改变函数作用域
 * @param  {Function} func     待优化的回调函数
 * @param  {[type]} context  执行上下文，函数中this的指向
 * @param  {number} argCount 迭代回调需要的参数个数
 * @return {function(): Function}          表示当前迭代过程的函数
 */
var optimizeCb = function(func, context, argCount) {
    if (context === void 0) return func;
    switch (argCount == null ? 3 : argCount) {
        case 1:
            return function(value) {
                return func.call(context, value);
            };
        case 2:
            return function(value, other) {
                return func.call(context, value, other);
            };
        case 3:
            return function(value, index, collection) {
                return func.call(context, value, index, collection);
            };
        case 4:
            return function(accumulator, value, index, collection) {
                return func.call(context, accumulator, value, index, collection);
            };
    }
    return function() {
        return func.apply(context, arguments);
    };
};
</pre>

optimizeCb 是根据 iteratee 所需要的参数个数 argCount 来进行优化的。我们来逐句分析一下。

`if (context === void 0) return func;`如果上下文未定义，即未指明 this 指向，返回待优化函数本身。这句代码保证了下面条件判断中一定存在 context。低版本 IE 中，undefined 可以被改写。ES5以后， undefined 只是一个全局对象的只读属性，但当 undefined 作为局部变量时，它的值可能被改写（如，可以在局部作用域中这样赋值`var undefined = 5;`）。为了保证代码的严谨，underscore 中所有的 undefined 都由`void 0`代替。

optimizeCb 的优化结果是依据 iteratee 的参数个数 argCount 来决定的。

##### 1. argCount == 1

`_.times`

##### 2. argCount == 2

underscore 中未出现。

##### 3. argCount == 3

不填传入参数个数时，默认为此种情况。三个参数分别为当前元素值、迭代索引、被迭代集合。

`_.each`，`_.map`

##### 4. argCount == 4

四个参数分别为累加器、当前迭代元素、迭代索引、被迭代集合。

`_.reduce`，`_.reduceRight`

###cb

<pre>
var cb = function(value, context, argCount) {
    if (value == null) return _.identity;
    if (_.isFunction(value)) return optimizeCb(value, context, argCount);
    if (_.isObject(value)) return _.matcher(value);
    return _.property(value);
};
_.iteratee = function(value, context) {
    return cb(value, context, Infinity);
};
</pre>

cb 根据 value 的不同值创建迭代过程 iteratee。

##### 1. value == null

iteratee 将返回当前元素本身。

<pre>
var rlt = _.filter([1,"a",true]);	//1,a,true
</pre>

##### 2. value 是函数

##### 3. value 是对象

看当前迭代元素是否匹配该对象。

##### 4. value 是字面量

返回能够获取对象属性的函数。

### createAssigner

<pre>
var createAssigner = function(keysFunc, undefinedOnly) {
    return function(obj) {
        var length = arguments.length;
        if (length < 2 || obj == null) return obj;
        for (var index = 1; index < length; index++) {
            var source = arguments[index],
                keys = keysFunc(source),
                l = keys.length;
            for (var i = 0; i < l.length; i++) {
                var key = keys[i];
                if (!undefinedOnly || obj[key] === void 0) obj[key] = source[key];
            }
        }
        return obj;
    };
};
</pre>

该内部函数主要用于扩展一个对象。

应用该方法的函数有：`_extend`，`_extendOwn`，`_defaults`。这三个函数的作用很相近，大体场景就是将传入的第二个参数以后（包括第二个参数）的所有参数的 key-value 添加给第一个参数。

### baseCreate

<pre>
// Object.create 兼容版
var baseCreate = function(prototype) {
    if (!_.isObject(prototype)) return {};
    // 如果支持ES5
    if (nativeCreate) return nativeCreate(prototype);
    // 前面有 var Ctor = function() {};
    Ctor.prototype = prototype;
    var result = new Ctor;
    Ctor.prototype = null;
    return result;
};
</pre>

### 其它

<pre>
var property = function(key) {
    return function(obj) {
        return obj == null ? void 0 : obj[key];
    };
};
// Math.pow(x,y)返回x的y次幂
// Math.pow(2,53) - 1是JavaScript能精确表示的最大数
var MAX_ARRAY_INDEX = Math.pow(2, 53) - 1;
// 获取数组或者类数组元素的属性值
var getLength = property('length');
var isArrayLike = function(collection) {
    var length = getLength(collection);
    return typeof length == 'number' && length >= 0 && length <= MAX_ARRAY_INDEX;
};
</pre>