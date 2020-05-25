title: javascript基础
date: 2020-05-16
categories: Javascript
tags:
- Javascript

---

关于Javascript的那些事，日常温习~

<!-- more -->

### 本质
- 解释性语言，比变异性语言稍慢，但是优点是可以跨平台
- 单线程，轮短时间片，把每一个任务分解成小的时间片段，送到队列中执行
- ES标准
- 三大部分 ES BOM DOM

### 变量
- 声明
- 赋值

### 数据类型
- 原始值 stack栈 先进后出，栈内存之间赋值是copy
Number, Boolean, String, undefined, null
- 引用值 heap堆 在栈内存中放出堆内存的地址，赋值后copy前一个值，即对应的堆的地址，操作同一个引用会有连锁反应。新赋值会打开关联
array  [], object, function, ...data, RegExp

### 运算符
- 比较运算符
- 逻辑运算符

### 条件语句

### 循环语句
forEach，for in， for of区别
- forEach
forEach专门用来循环数组，可以直接取到元素，同时也可以取到index值
问题：存在局限性，不能continue跳过或者break终止循环，没有返回值，不能return
- for...in 
一般循环遍历的都是对象的属性，遍历对象本身的所有可枚举属性，以及对象从其构造函数原型中继承的属性
问题：key会变成字符串类型
- for of
for of是ES6新引入的特性。修复了ES5中for in的不足
允许遍历 Arrays（数组）、Strings（字符串）、Maps（映射）、Sets（集合）等可迭代的数据结构
for of 支持return, 只能遍历数组不能遍历对象（遍历对象需要通过和Object.keys()搭配使用）

### typeof类型
typeof()  未定义的值不包错，返回字符串型的'undefined'
- number
- string
- object:  对象 数组 null（历史遗留问题，以前浏览器认为Null表示空对象）
- undefined
- function

#### 显式类型转换
- Number()类型转换为数字，undefined转为NaN，123AB转为NaN，即使转换为NaN类型也是number
- parseInt （目标，目标进制）舍小数位转换为整数，Null，undefined，a，都是NaN，123abc转为122，目标进制转十进制
- parseFloat 
- String() 所有东西都转为字符串
- Boolean()
- a.toString(目标进制) undefined和Null不能用，十进制转目标进制


#### 隐式类型转换
- isNaN() 先Number再和NAN比较
- ++  --     - 转数字，Number
- +两侧有一个string则调用string
- +-/* Number
- <> 数字优先.都是字符串转为ascall码
- == != 
- undefined == null != 0
- NaN != NaN
