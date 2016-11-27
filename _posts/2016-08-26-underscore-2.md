---
layout: post
title: "underscore（二）"
keywords: ["underscore","JavaScript","源码","1.8.3"]
description: "underscore源码解读"
category: "underscore"
tags: ["JavaScript","underscore"]
---
{% include JB/setup %}

## 集合相关函数 Collection Functions

### _.each

ES5 的 forEach 方法，只支持对数组进行迭代，而 underscore 既支持数组的迭代也支持对象的遍历。