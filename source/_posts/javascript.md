title: javascript基础
date: 2020-05-16
categories: Javascript
tags:
- Javascript

---

关于Javascript的那些事，日常温习~
原创文章，转载请注明出处。

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

## 函数
### 函数声明

```
//  函数声明
function test() {

}

// 函数表达式
// test打印出整个函数体
var a = function() {

}
// a打印函数体
var b = funtion test() {

}
// b打印函数体 test打印报错，函数表达式中的函数名无意义

a.name // a
b.name // test
```

### 形参、实参
arguments-函数内的实参列表
函数名.length 形参梳数量
arguments和形参不是一个值，但是是互相绑定的，改变一个则另一个也改变

```
function sum(a, b, c) {
    a = 2; 
    arguments[0] = 3;
    // a = argument[2] ===2

    c = 4;
    arguments[2] // undefined
}

sum(1, 2);
```

实参列表赋值的时候有几个，arguments就有几个，长度对应才会映射。


递归的代码是最简单的，但是是效率最低的。（阶乘 菲薄纳西数列）
- 找规律
- 找出口


### 预编译
js：单线程，解释性语言
- 语法分析，通篇扫描
- 预编译
- 解释执行

函数声明整体提升，函数会被提升到逻辑的最前面。

变量，声明提升，赋值不提升。

函数和变量重名无法解决。

未经声明的变量会变成全局变量。
全局的任何变量即使声明了，也归window所有。

window就是全局的域。

```
function test() {
    var a = b = 123;
    // 先把123赋值给未经声明的b，未经声明的b挂载到全局的window上
    // 再声明一个a，把b的值赋给a
}

test();

a // undefined
b // 123
```

预编译发生在函数执行的前一刻:
- 创建AO对象（执行期上下文）
- 找形参和变量声明，将变量和形参名作为AO属性名，值为undefined
- 形参和实参统一
- 在函数体里面找函数声明，值赋予函数体

```
/**
AO {
    a: function a () {}
    b: undefined
    d: function d () {}
}
**/

function fn(a) {
    console.log(a); // function
    
    var a = 123;

    console.log(a); // 123
    
    function a () {} // 函数声明已经提升了，不再执行

    console.log(a); // 123

    var b = function() {}

    console.log(b); // function

    function d () {}
}

fn(1);
```

全局预编译
- 生成一个GO对象 === window

### 作用域，作用域链

```
function test() {
    
}

test.name
test.[[scope]] // 隐式属性，仅供引擎存取，就是作用域，存储的是执行期上下文的集合
```
 
作用域链：[[scope]]中所存储的执行期上下文对象的集合，这个集合呈链式链接，这种链式链接即作用域链。

```
function a() {
    
}
var glob = 100;
a();

// a defined a.[[scope]] --> 0: GO { }
// a doing   a.[[scope]] --> 0: AO { } 1: GO { }
```
从函数作用域链的顶端依次向下查找。

### 立即执行函数，闭包

```
function a() {
    var num = 100;
    funttion b() {
        num++;
        console.log(num);
    }
    return b;
}

var demo = a(); // 执行完a销毁了执行上下文，b保留着aAO

demo(); // 101
demo(); // 102
```

a产生了GO和aAO

b拿到a的成果AO

b每次执行都在基础上执行新的bAO，但是a的AO是不丢掉的，所以num在aAO上叠加。

闭包：党内部函数被保存到外部时，将会生成闭包。闭包会导致原有作用域链不释放，造成内存泄漏。

 - 实现共有变量：
 利用闭包实现计数器
 ```
 function add() {
     var count = 0;
     function demo () {
         count ++;
         console.log(count);
     }
     return demo();
 }
 
 var counter = add();
 counter(); // 1
 counter(); // 2
 ```
 
 - 可以做缓存
 ```
 function test() {
    var num = 100;
    function a() {
        num ++;
    }
    function b() {
        num --;
    }
    return [a, b]; 
 }
 
 var myArr = test();
 myArr[0](); // 101
 myArr[1](); // 100
 ```
 
 ```
 function eater() {
    var food = ''

    var obj = {
        eat: function() {
            food = ''; 
        },
        push: function(myFood) {
            food = myFood;
        }
    }
    return obj
};
var eater1 = eater();
eater1.push('banan');
eater1.eat();
 ```
 
 - 实现私有化
 - 模块化开发
 
### 立即执行函数
针对初始化功能的函数，执行后立即释放空间

```
var num = (function(形参) {
    var a = 123;
    var b = 234;
    return a;
}(实参))

abc() // 报错
```
能被执行符号执行的表达式，忽略表达式的名字

```
+ function test() {
   console.log('a');
}()

test(); // 报错
```

### 闭包

```
function test() {
    var arr = [];
    for(var i = 0; i < 10; i++) {
        arr[i] = function() {
            doocument.write(i + '');
        } // 赋值语句，函数体内先不执行，只是一个函数引用
    }
    return arr;
}

var myArr = test();

for(var j = 0; j < 10; j++) {
    myArr[j](); // 执行的时候才执行上面的function，这个时候i已经变成了10，并被保存到外部
}
```

打印结果 10个10

解决方案：
- 立即执行函数（无法控制执行时机）

```
arr[i] = function() {
            doocument.write(i + '');
        }()
```

- 闭包+ 立即执行函数解决

```
function test() {
    var arr = [];
    for(var i = 0; i < 10; i++) {
        (function(j) {
            arr[j] = function() {
                document.write(j + '');
            }
        }(i)) // 会形成10个函数，i立即变现
    }
    return arr;
}

var myArr = test();

for(var j = 0; j < 10; j++) {
    myArr[j](); 
}
```

**闭包**：两个或多个函数互相嵌套，把内部的函数保存到外面，导致原有作用域链不被释放，造成内存泄漏。

#### eg: 给一个li列表添加onclick方法

```
<ul>
    <li>a</li>
    <li>a</li>
    <li>a</li>
    <li>a</li>
</ul>

function test() {
    var liCollection = document.getElementByTagName('li');
    for(var i=0; i< liCollection.length; i++) {
        /*liCollection[i].onclick = function() {
            console.log(i + ''); // 点击会打印全部是4
        }*/
        (function(j) {
            liCollection[j].onclick = function() {
            
            }
        }(i))
    }
}

test();
```

### 对象，包装类
#### 对象
属性和方法的集合

```
var obj = {
    name: 'daisy',
    age: 27,
    health: 100,
    drink: function() {
        this.health ++;
        // obj.health ++;
    }
}
```

#### 增,查，删，改

```
obj.wife = 'ooo'; // 增
 
obj.name; // 查

obj.age = 28; // 改

delete obj.name; // 删
```

#### 创建方法

```
var obj = {} // 对象字面量

var obj = new Object(); //系统自带的构造函数

// 自定义构造函数
function Person() {
    this.name = 'sss'
}

ver person = new Person(); // 函数通过new
```

由于函数和构造函数很相似，因此构造函数命名规则是大驼峰式。

#### 构造函数内部原理
- new操作符调用函数
- new会在函数体内部最顶端会隐式生成一个var this = {} 空对象
- new会在函数末尾隐式加一个return this

```
function Person(name, height) {
    // var this = {};
    this.name = name;
    this.height = height;
    this.say = function() {
        console.log(this.say);
    }
    // return this;
}

console.log(new Person('xiaowang', 160).name)
```

**如果显示的return一个对象，则new获取的是显示返回的object，但是如果显示返回一个原始值，则会返回this。**

## 包装类

```
var num = 123;
var num1 = new Number(123); // 变成对象
num1 * 2 // 246 运算后变回原始值 

new String('abc') 
new Boolean('true')
```

原始值没有属性和方法，对象可以有。

原始值没有属性和方法，但是添加属性会先创建一个对象。

```
// 包装类
var num = 4;
num.len = 3;
//new Number(4).len = 3; delete
console.log(num.len); // 再new  Number(4).len 但是是新的，返回undefined

var str = 'abcd';
str.length = 2 ;
// new String('abcd').length; delete
console.log(str.length); // 4
```

### 原型，原型链
原型是function对象的一个属性，定义了构造函数制造出的对象的公共祖先，通过该构造函数产生的对象，可以继承原型的属性和方法，原型也是对象。

```
// Person.prototype--原型 = {} 是祖先
Person.prototype.name = 'hehe';
Person.prototype.say = function() {
    console.log('hehe')
}
function Person() {
    
}

var person = new Person();
person.name // 'hehe'
person.say(); // 'hehe'
```

- 利用原型的嗯对象和方法可以提取对象的共有属性
- delete可以删除对象自身的属性，但是不能删除原型上的属性
- 查看原型--》隐式属性_proto_
- 查看对象的构造函数-->constructor 隐式，可修改

```
Person.prototype.name = 'abc'

function Person() {
    //var this = { _proto_ = Person.prototype}
}

var obj = {
    name: 'sunny'
}

var person = new Person();

Person.prototype.name = 'cherry' // 可以修改person的name
Person.prototype = {
    name : 'cherry'
} // 不能修改name 修改了原型，_proto_指向原空间
```

#### 原型链

```
Grand.prototype.lastName = 'Gao';
function Grand() {
    
}

var grand = new Grand();

Father.prototype = grand;
function Father() {
    this.name = 'aa';
    this.fortune = {
        car1: 'visa'
    }
}

var father = new Father();

Son.prototype = father;
function Son() {
    this.hobbit = 'music';
}

var son = new Son();

son.lastName // Gao
son.toString // 所有原型的最终原型是object.prototype

son.fortune.car1 = 'jsk'; // 引用值，调用修改
```

后代无法增删改查父代的属性。

例外：引用值

```
// a.sayName() sayName里面的this指向时，谁调用的这个方法,this指向谁

Person.prototype = {
    name:  'a',
    sayName: function() {
        console.log(this.name)
    }
}

function Person() {
    this.name = 'b'
}

var person = new Person();

person.sayName() // b
person.prototype.sayName(); // a
```

- 绝大多数的对象最终都会继承自Object,prototype  object.create(null) 没有_proto_
- object.create(原型)
- undefined null没有原型，没有toString

#### 插播：浮点数计算精度偏差怎么处理？
*100后取整。

原因：浮点数转换为二进制计算，例如0.1会转换为无限循环的二进制，计算机会自动截断为52位

解决方案：
计算出使浮点数都变成整数的最小整数倍，先放大进行计算，结果再缩小。

面试有时候会问这个问题，真的是闲的蛋疼，为什么前端要进行数据计算呢？可靠吗？

<span id="callapply"></span>
### call/apply（必问题）
- 作用：改变this指向
- 区别：后面传参形式不同


```
function Person(name. age) {
    this.name = name;
    this.age = age;
}

var person = new Person('a', 100);

var obj = {};

Person.call() // Person
Person.call(obj, 'b', 300);// this = obj

obj.name // b
```
this指向call的第一个参数，其他传参顺次放在call的后面几个参数中。

```
function Person(name, age, sex) {
    this.name = name;
    this.age = age;
    this.sex = sex;
}

function Student(name, age, sex, grade, tel) {
    // var this = {};
    Person.call(this, name, age, sex);
    Person.apply(this,[name, age, sex]);
    // this = {name, age, sex} 
    this.grade = grade;
    this.tel = tel;
}

var student = new Student('a', 10, 'female', 3, 139);
```

call要把实参按照形参的个数穿进去
apply传的是一个实参列表的数组

### 继承
- 原型链

  过多的继承了无用属性
  
- 借用构造函数call／apply
   
  不能继承借用构造函数的原型
  
  每次构造函数都要多走一个函数

- 共享原型
   
  不能随便改变自己的原型

```
Father.prototype.lastName = 'G';
function Father() {
    
}

function Son() {
    
}

// Son.prototype = Father.prototype;

/**
问题是，父子互相影响，指向同一个位置
function inherit(Target, Origin) {
    Target.prototype = Origin.Prototype;
}

inherit(Son, Father);

var son = new Son();
**/

//function F() {}
//F.prototype = Father.prototype;
//Son.prototype = new F();

// 面试常见问题：你能实现一个继承吗？
// 圣杯模式
function inherit(Target, Origin) {
    function F() {}
    F.prototype = Origin.prototype;
    Target.prototype = new F();
    Target.prototype.constructor = Target; //解决son._proto_ --> new F()._proto_-->Father.prototype 作用域链紊乱
}

// 雅虎的解决方案
var inherit = (function() {
    var F = function() {} // 私有化变量
    return function (Target, Origin) {
        F.prototype = Origin.prototype;
        Target.prototype = new F();
        Target.prototype.constructor = Target;
        Target.prototype.uber = Father.prototype;
    }
    // 闭包的第三点作用，实现封装，属性私有化
}());
```

#### 命名空间
管理变量，防止全局污染，适用于模块开发。


#### 实现链式调用

```
var a = {
    smock: function() {
        console.log('1111')
        return this;
    },
    drink: function() {
        console.log('1111')
        return this;
    },
    eat: function() {
        console.log('1111')
        return this;
    }
}

a.smock().drink();
```

#### 属性名
obj.name ===== obj['name']


#### 对象遍历
for...in... 遍历属性的key值
- hasOwnProperty()
- in
- A instanceof B A对象是不是B构造函数构造出来的，看A的原型链上有没有B的原型

应用：
- typeOf判断时数组和对象返回的都是'object'
- 可以用xx instanceof Array换原来的this,深度拷贝的时候可以用来区分数组和对象

```
var obj = {
    name: '123',
    age: '123',
    sex: 'male',
    _proto_: {
        lastname: 'deng'
    }
}

for(var prop in obj) {
    console.log(obj.prop); // undefined  相当于 obj['prop']
    console.log(obj[prop]); // 会遍历出原型上的属性，系统自带的不会打印

    if(obj.hasOwnProperty(prop)) {
        console.log(obj[prop]); // 不会遍历出原型上的属性
    }
}

[] instanceof Array //true
[] instanceof Object // true
```

## this

```
var x = 1, y = z = 0;
function add(n) {
    return n = n + 1;
}

y = add(x); // 4

function add(n) {
    return n = n + 3;
}

z = add(x); // 4
```
预编译，后一个同名函数体会覆盖前面函数。

```
function foo(x) {
    console.log(arguments);
    return x;
}

foo(1,2,3,4,5) // 打印1,2,3,4,5

function foo(x) {
    console.log(arguments);
    return x;
}(1,2,3,4,5) // undefined

(function foo(x) {
    console.log(arguments)
    reutrn x;
})(1,2,3,4,5) // 1,2,3,4,5

fucntion foo() { 
    bar.apply(null, arguments);
}
function bar() {
    console.log(arguments);
}
foo(1,2,3,4,5) // 1,2,3,4,5
```

```
{} == {} // false

isNaN('123') // NaN
```

### this
- 函数预编译过程 this-->window
- 全局作用域里 this-->window
- <a href="#callapply">call/apply改变函数运行时的this指向</a>
- obj.func()；func()里面的this指向obj

```
fucntion test(c) {
    var a = 123;
    function b() {}
}

AO {
    arguments: [1],
    this: window,
    c: 1,
    a: undefined,
    b: function() {}
}

test(1)

new test(); // var this = Object.create(test.prototype)
```

```
var name  = '222'

var a = {
    name : '111',
    say: function() {
        console.log(this.name);
    }
}
var fun = a.say; // function函数引用

fun(); // 222，全局范围内执行 this指向window

a.say(); // 111， this指向a

var b = {
    name: '333',
    say: function(fun) {
        fun(); // fun（）执行指向预编译，不是this.fun()
    }
}
b.say(a.say); // 222， fun（）执行指向预编译
b.say = a.say;
b.say(); // 333
```

```
var foo = 123;
function print() {
    this.foo = 234;
    console.log(foo)
}

print(); // 234
new print(); // 123
```

```
var a = 5;
function test() {
    a = 0;
    alert(a);
    alert(this.a);
    var a;
    alert(a);
}

test(); // 0 5 0

new test(); // 0 undefined 0 
```

```
var b = {a: '002'};
function print() {
    bar.a = 'a';
    Object.prototype.b = 'b';
    return function inner() {
        console.log(bar.a); // a
        console.log(bar.b); // b
    }
}

print()();
```

#### arguments
- arguments.callee 指向函数的引用

```
// 立即执行函数无法递归调用
var num = (fucntion(n){
    if(n == 1) {
        return 1;
    }

    return n = n * arguments.callee(n-1);
}(100))
```

- func.caller 谁调用的，没什么用

## 克隆

- 原始值克隆（浅拷贝）

```
var obj = {
    name: 'abc',
    age: 123,
    sex: 'female'
};

var obj1 = {};

function clone(origin, target) {
    var target = target || {};
    for(var prop in origin) {
        target[prop] = origin[prop];
    }

    return target;
}

clone(obj, obj1);
```

- 深拷贝

```
function deepClone(origin, target) {
    var target = target || {};

    var toStr = Object.prototype.toString;

    arrStr = "[object Array]";

    for(var prop in origin) {
        if(origin.hasOwnProperty(prop)) {
            if (typeof(origin[prop]) == 'object') {
                
                if(toStr.call(origin[prop] == arrStr) {
                    target[prop] = [];
                } else {
                    target[prop] = {};
                }
                
                deepClone(origin[prop], target[prop]);

            } else {
                target[prop] = origin[prop];
            }
        }
    }
    return target;
}
```

```
Object.prototype.toString.call(undefined);
"[object Undefined]"

Object.prototype.toString.call(null);
"[object Null]"

Object.prototype.toString.call('');
"[object String]"

Object.prototype.toString.call({});
"[object Object]"

Object.prototype.toString.call(function(){});
"[object Function]"

Object.prototype.toString.call(123);
"[object Number]"
```

## 数组和数组方法

```
var arr = [];
var arr1 = new Array();
```

- 改变原数组

push pop shift unshift sort reverse

splice

- 不改变原数组

concat join

split  toString  slice

```
// 实现一个push
var arr = [1, 2]；
Array.prototype.push = function() {
    for(var i = 0; i< arguments.length; i++) {
        this[this.length] = arguments[i];
    }

    return this.length;
}

// 实现一个pop
Array.prototype.pop = function() {
    this.length = this.length - 1;

    return this.length
}
```

- shift()方法：移除数组中的第一项并返回该项

```
Array.prototype.shift = function() {
    var newArr = [];
    var len = this. length;
    
    if (len === 0) {
        return
    }
    
    for(var i=1; i < len; i++) {
        newArr[i-1] = this[i];
    }
    
    var value = this[0];
    this.length = 0;
    
    for(var i = 0; i < len-1; i++) {
        this[i] = newArr[i];
    }
    
    return value;
}
```

- unshift()方法：在数组的前端添加项

```
Array.prototype.unshift = function() {
    var len = this.length;
    if (len == 0) {
        return [];
    }
    var newArr = [];
    for(var i = 0; i < arguemnts.length; i++) {
        newArr[i] = arguments[i];
    }
    for(var j = 0; j < len; j++) {
        newArr[arguments.length + j] = this.[j];
    }
    
    this.length = 0;
    for(var k = 0; k < newArr.length; k++) {
        this[k] = newArr[k];
    }
    
    return this;
}
```

- reverse() 数组逆转顺序

```
Array.prototype.reverse = function() {
    var len = this.length;
    
    if(len === 0) {
        return [];
    }
    
    var newArr = [];
    for(var i = 0; i < len; i++) {
        newArr[i] = this[len-i];
    }
    ...
}
```

- slice() 方法返回一个从开始到结束（不包括结束）选择的数组的一部分浅拷贝到一个新数组对像，原始数组不会改变。

- splice()方法通过删除现有元素和/或添加新元素来更改一个数组的内容。

区别：

splice(截取开始位置，截取长度，插入数据) 方法会直接对数组进行修改。

slice()不对数组进行修改，原数组不会改变。

splice() 返回值为被操作的值。

slice()以新的字符串返回被提取的部分。

- sort() 无序数组排序，按ascll排序

可通过function来自定义
1.必须传两个形参
2.规则是看返回值，返回负数，前面的数放在前面，返回正数，后面的数在前，为零不动

```
array.sort(function(a, b) {
  return b - a; //正数，b>a，b在前，则是倒序排序
})

// 有序数组，乱序
array.sort(function)
```

### 类数组

```
var obj = {
    '0': 'a',
    '1': 'b',
    '2': 'c',
    'length': 3,
    'push': Array.prototype.push
}

obj.push(d) // 对象插入'3':'d'，且length变了
```

- 索引属性
- 有length
- 最好加上push

原理：

```
Array.prototype.push = function(target) {
    this[this.length] = target;
    length ++;
}
```

### 编程题-数组去重，要求在原型链上编程

```
Array.prototype.unique = function() {
    if(this.length === 0) {
        return [];
    }
    var arrMap = {};
    for(var i = 0; i < this.length; i++) {
        arrMap[this[i]] = i;
    }
    this.length = 0;
    let j = 0;
    for(var prop in arrMap) {
        this[j] = prop;
        j++;
    }
    return this;
}
```

### 实现精准类型判断

```
fucntion type(target) {
    var template = {
        "[object Array]": 'array',
        "[object String]": 'string - object',
        "[object Boolean]": 'boolean - object',
        "[object Object]": 'object',
        "[object Number]": 'number - object'
    }
    if(target === null) {
        return null;
    }
    
    if (typeof(target === 'object')){
        // 数组，对象，包装类
        var toStr = Object.prototype.toString.call(target);
        
        return tmplate[toStr];
    }
    
    return typeof(target)
}
```

### try catch
```
try{

} catch(e) {

}
```
捕获未知错误。

