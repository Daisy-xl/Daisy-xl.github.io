title: ES2020可选链
date: 2020-11
categories: ES2020
tags:
- ES
- ES2020

---

关于ES2020新增方法——可选链的解读。

<!-- more -->

在ES6还没有读熟读透的时候，ES已经更新到2020了，刷掘金的时候发现了其中的一个新特性——可选链，对于苦于针对于业务中接口的不确定性无休无止的判断多年的前端er，简直是代码简洁和保障神器。

### 什么是可选链？

可选链 ?. 是一种访问嵌套对象属性的安全的方式。即使中间的属性不存在，也不会出现错误。

### 可选链解决了什么问题？

简而言之，对于一个层级很深的对象userInfo，三层以下有某个属性——父亲的手机号（tel）,结构如下：

```
userInfo = {
    name: 'zhangsan',
    family: {
        father: {
            name: 'laowang',
            tel: '137****4321'
        }
    }
}
```

当我们去取父亲的手机号时，需要用到userInfo.family.father.tel，但是我们发现，用户根本没有提供父亲的信息

```
let userInfo = {
    name: 'zhangsan'，
    family: {
        mother：{
            name: 'laowang',
            tel: '137****4321'
        }
    }
}; // 一个没有father属性的obj

alert(userInfo.family.father.tel) // Error!

```

这是预期的结果。JavaScript 的工作原理就是这样的。因为 userInfo.family.father 为 undefined，尝试读取 userInfo.family.father.tel 会失败，并收到一个错误。

对于无法掌控的后端接口，甚至在用户没有提供家庭成员信息的时候，family节点都会消失。

```
let userInfo = {
    name: 'zhangsan'
}; // 一个没有family属性的obj

alert(userInfo.family.father.tel) // Error!

```
同样的，userInfo.family为undefined， 那么接着去找undefined内部的属性，也必然会报错。

还有另一个例子。在 Web 开发中，我们可以使用特殊的方法调用（例如 document.querySelector('.elem')）以对象的形式获取一个网页元素，如果没有这种对象，则返回 null。

```

// 如果 document.querySelector('.elem') 的结果为 null，则这里不存在这个元素
let html = document.querySelector('.elem').innerHTML; // 如果 document.querySelector('.elem') 的结果为 null，则会出现错误
复制代码

```
同样，如果该元素不存在，则访问 null 的 .innerHTML 时会出错。在某些情况下，当元素的缺失是没问题的时候，我们希望避免出现这种错误，而是接受 html = null 作为结果。

那么，可选链就是为这样的问题提供了一种快速，方便，友好的解决方案。

### 其他解决方案

那么在可选链出现之前，我们是怎么规避或者解决这样的问题的呢？

#### 万能 if 判断 && 条件语句

通过if层层判断或者通过条件运算符 ？：，显然可以解决这个问题，像下面这样：

```
if (userInfo) {
    ...
    if (userInfo.family) {
        ...
        if (userInfo.family.father) {
            ...
        }
    }
}
```

或是

```
userInfo ? userInfo.family ? userInfo.family.father ? userInfo.family.father.tel : '' : undefined : undefined : undefined
```

这显然是及其不优雅的，并且当你既需要判断父亲，又需要判断母亲的时候，你的代码对于其他人来说，简直是一个迷。

这就是为什么增加了可选链这个解决方案。

### 可选链，详解

如果可选链 ?. 前面的部分是 undefined 或者 null，它会停止运算并返回该部分。

为了简明起见，在本文接下来的内容中，我们会说如果一个属性既不是 null 也不是 undefined，那么它就“存在”。

换句话说，例如 value?.prop：

- 如果 value 存在，则结果与 value.prop 相同，
- 否则（当 value 为 undefined/null 时）则返回 undefined。

那么上面这个例子，我们就可以这样优雅的写；

```
userInfo？.family？.father？.tel 
```

这样，某一个节点不存在，也只会返回undefined，而不是报出异常。

### 说明

#### 不要过度使用可选链：

我们应该只将 ?. 使用在一些东西可以不存在的地方。
例如，如果根据我们的代码逻辑，user 对象必须存在，但 address 是可选的，那么我们应该这样写 user.address?.street，而不是这样 user?.address?.street。
所以，如果 user 恰巧因为失误变为 undefined，我们会看到一个编程错误并修复它。否则，代码中的错误在不恰当的地方被消除了，这会导致调试更加困难。

#### 可选链 ?. 前的变量必须已声明

如果未声明变量 userInfo，那么 userInfo?.anything 会触发一个错误：

```
// ReferenceError: userInfo is not defined
userInfo?.family;
```

#### 存在短路效应

正如前面所说的，如果 ?. 左边部分不存在，就会立即停止运算（“短路效应”）。

所以，如果后面有任何函数调用或者副作用，它们均不会执行。

```
let user = null;
let x = 0;

user?.sayHi(x++); // 没有 "sayHi"，因此代码执行没有触达 x++

alert(x); // 0，值没有增加
```

#### 其他应用

可选链 ?. 不是一个运算符，而是一个特殊的语法结构。它还可以与函数和方括号一起使用。

例如，将 ?.() 用于调用一个可能不存在的函数。

如果我们想使用方括号 [] 而不是点符号 . 来访问属性，语法 ?.[] 也可以使用。跟前面的例子类似，它允许从一个可能不存在的对象上安全地读取属性。

用法和点符号都一样，这里就不举例子了。

#### 可以将 ?. 跟 delete 一起使用

```
delete userInfo?.name; // 如果 userInfo 存在，则删除 userInfo.name
```

### 总结

可选链 ?. 语法有三种形式：

- obj?.prop —— 如果 obj 存在则返回 obj.prop，否则返回 undefined。
- obj?.[prop] —— 如果 obj 存在则返回 obj[prop]，否则返回 undefined。
- obj.method?.() —— 如果 obj.method 存在则调用 obj.method()，否则返回 undefined。

正如我们所看到的，这些语法形式用起来都很简单直接。?. 检查左边部分是否为 null/undefined，如果不是则继续运算。
?. 链使我们能够安全地访问嵌套属性。
但是，我们应该谨慎地使用 ?.，仅在当左边部分不存在也没问题的情况下使用为宜。以保证在代码中有编程上的错误出现时，也不会对我们隐藏。