title: Vue源码学习
date: 2020-06-09
categories: VUE
tags:
- VUE

---

VUE源码分析，用于日常记录~

<!-- more -->

## 准备工作

Roolup可以作为纯js库的模块打包器。

- 生成npm项目,package.json

```
npm init -y
```

- 安装开发依赖

```
npm install @babel/preset-env @babel/core rollup rollup-plugin-babel rollup-plugin-serve cross-env -D
```

@babel/preset-env @babel/core ES6转换为ES5
rollup rolliup-plugin-babel rolliup-plugin-serve  配合rollup和babel使用

- 配置rollup.config.js

```
import babel from 'rollup-plugin-babel';
import serve from 'rollup-plugin-serve';

export default {
    input: './src/index.js',
    output: {
        format: 'umd', // 统一模块规范，默认将打包后的结果挂在到window
        file: 'dist/vue.js', //打包出的vue.js文件
        name: 'Vue', //  // 打包后的全局变量的名字
        sourcemap: true // 可以看到打包后的ES6源码，方便调试，
    },
    plugins: [
        babel( { // 配置babel 解析es6
            exclude: "node_modules/**" // 移除文件操作
        }),
        serve({ // 配置本地服务
            open: true, // 默认打开
            openPage: '/public/index.html', // 本地服务打开的页面
            port: 3000, //本地服务端口号
            contentBase: ''
        })
    ]
}
```

- 配置.babelrc

```
{
    "presets": [
        "@babel/preset-env"
    ]
}
```

- 配置启动脚本
在package.json中配置script

```
"build:dev": "rollup -c",
"serve": "cross-env ENV=development rollup -c -w"
```

运行 npm run serve

自动打开本地端口3000的服务

## VUE显示数据原理

### 入口实现
首先看一下vue是怎么使用数据绑定的呢？

```
<div id="app"></div>

const vm = new Vue({
    el: '#app',
    // 数据会被进行观测，响应式数据变化，操作数据可以更新视图
    // 对对象的所有属性，使用defineProperty进行重新定义get和set
    data() {
        return {
            msg: 'zzz'
        }
    }
})
```

可以看出，在new的时候需要传入一个对象

index.js

```
function Vue(options) {
    // 对传入的结构进行初始化操作
    this._init(options);
}

export default Vue;
```

这里的_init方法对于页面和组件都需要通用，因此可以挂载到Vue的原型上

```
Vue.protptype._init = function(options) {

}
```
这里可以采用模块化开发

抽象到一个新的init.js中

```
export function initMixin(Vue) {
    Vue.prototype._init = function() {
        // Vue内部$options就是用户传入的所有参数

        const vm = this;
        vm.$options = options;// 用户传入的参数
    }
}
```

只需要在index.js中import这个方法，就可以调用原型上的this._init方法了。

```
// 1.import
import { initMixin } from './init';

// 2.调用
initMixin(); // 给Vue原型上增加init方法
```

init方法中初始化vue状态
```
// 初始化状态
initState(vm)
```

根据不同属性进行初始化操作,initState.js
```
export function initState(vm){
    const opts = vm.$options;
    
    opts.props && initProps(vm);
    
    opts.methods && initMethod(vm);
    
    // 初始化data
    opts.data && initData(vm);
    
    opts.computed && initComputed(vm);
    
    opts.watch && initWatch(vm);
    
}
function initProps(){}
function initMethod(){}
function initData(vm){
    console.log(vm.$options.data)
}
function initComputed(){}
function initWatch(){}
```

以上，入口基本实现，入口本身就是一个Vue的构造函数。

### 初始化数据

initData()
```
let data = vm.$options.data;

//  如果传入的data是函数，则执行函数，且this需要指向vue实例的this
// vm._data就是检测后的数据

data = vm._data = typeof(data) === 'function' ? data.call(vm): data;

// 观测数据，实现响应式
observe(data);
```

### 新增observer文件夹，内部index.js内部到处导出observer方法实现数据观测和劫持

observer/index.js

```
export function observer(data) {
    // 对象就是使用defineProperty实现响应式原理
    // 如果这个不是非空对象或数组，就不需要监控
    // 此处可以抽到一个工具类方法中再导入使用 util.js中一个isObject方法
    if(typeof data === 'object' && data != null){
        return;
    }
    Observer(data)
}
```

Observer定义为一个class

```
 // 观测值
constructor(value){
    this.walk(value);
}
walk(data){ // 让对象上的所有属性依次进行观测
    let keys = Object.keys(data);
    for(let i = 0; i < keys.length; i++){
        let key = keys[i];
        let value = data[key];
        defineReactive(data,key,value);
    }
}
```

此时已经可以实现基本数据类型的观测，但是如果类型是引用值，还是观测不到内部的数据变化

在defineProperty内部先递归调用，set方法内也递归调用observer

```
observer(value);// 如果传入的值还是一个object的话，就做递归循环检测
```

** 这也是vue2监控数据变化性能不高的原因 **

### 数组监控
Observer对数组索引进行拦截，性能差且直接更改索引的概率很低

```
if(Array.isArray(data)) {
    //  函数劫持，重写数组，改变数组本身的方法可以监测到
    data._proto_ = arrayMethod; // 原型链向上查找，先找自己重写的方法
    this.observeArray(data);
}
```

```
observeArray(value){
    for(let i = 0 ; i < value.length ;i ++){
        observe(value[i]);
    }
}
```

### 函数劫持改写数组方法

```
let oldArrayMethods = Array.prototype; // 获取数组原型上的方法

// 创造一个全新的对象，可以找到数组原型上的方法，修改对象时，不会影响原数组的原型
export let arrayMethods = Object.create(oldArrayMethods);

let methods = [
    'push',
    'pop',
    'shift',
    'unshift',
    'reverse',
    'sort',
    'splice'
];
methods.forEach(method => {
    arrayMethods[method] = function (...args) {
        const result = oldArrayProtoMethods[method].apply(this, args);
        const ob = this.__ob__;
        let inserted;
        //  push unshift splice都可以新增属性，新增的属性可能是一个对象属性
        // 内部还对数组中的引用类型进行了劫持
        switch (method) {
            case 'push':
            case 'unshift':
                inserted = args;
                break;
            case 'splice':
                inserted = args.slice(2)
            default:
                break;
        }
        if (inserted) ob.observeArray(inserted); // 对新增的每一项进行观测
        return result
    }
})
```

observer方法中增加防重复监控

```
if(data._ob_ instanceof Observer) {// 防止对象被重复观测
    return;
}
```

## VUE模板解析
将template编译成render函数

vue2中模板编译可以选择性的添加。

- runtimeonly 只在运行时使用，无法解析用户传递的template属性
- runtime with compiler，可以实现模板编译的

如何将模板变成render函数 -》返回的时虚拟节点

模板引擎原理，必须用with

- 实现一个解析器，可以解析html模板，目的时把他变成ast（抽象）语法树，用一个树结构描述当前标签内容
- 虚拟节点时描述dom的，是使用对象来描述html结构的。ast是描述html语法的。

将html本身变成js语法 render函数

模板编译原理：

- 先把我们的代码转化成ast语法树 parser

```
const ncname = `[a-zA-Z_][\\-\\.0-9_a-zA-Z]*`;  // 匹配标签名
const qnameCapture = `((?:${ncname}\\:)?${ncname})`;
const startTagOpen = new RegExp(`^<${qnameCapture}`); // 标签开头的正则 捕获的内容是标签名
const endTag = new RegExp(`^<\\/${qnameCapture}[^>]*>`); // 匹配标签结尾的 </div>
const attribute = /^\s*([^\s"'<>\/=]+)(?:\s*(=)\s*(?:"([^"]*)"+|'([^']*)'+|([^\s"'=<>`]+)))?/; // 匹配属性的
const startTagClose = /^\s*(\/?)>/; // 匹配标签结束的 >
const defaultTagRE = /\{\{((?:.|\r?\n)+?)\}\}/g // {{ xxxxx  }}

// 解析html
export function parserHTML(html) {
    while(html){
        
        let textEnd = html.indexOf('<');
        // 匹配到标签< ，解析标签
        if(textEnd == 0){
            const startTagMatch = parseStartTag();
            if(startTagMatch){
                start(startTagMatch.tagName,startTagMatch.attrs);
                continue;
            }
            const endTagMatch = html.match(endTag);
            if(endTagMatch){
                advance(endTagMatch[0].length);
                end(endTagMatch[1]);
                continue;
            }
        }
        // 如果<没有，说明是文本
        let text;
        if(textEnd >= 0){
            text = html.substring(0,textEnd);
        }
        if(text){
            advance(text.length);
            chars(text);
        }
    }
    function advance(n){
        html = html.substring(n);
    }
    // 解析标签开头
    function parseStartTag(){
        const start = html.match(startTagOpen);
        if(start){
            const match = {
                tagName:start[1], // 标签名，span div
                attrs:[]
            }
            // 匹配到的部分截掉
            advance(start[0].length);

            let attr,end;
            // 匹配标签上的属性，未到标签头闭合处且匹配属性规则
            while(!(end = html.match(startTagClose)) && (attr = html.match(attribute))){

                // 匹配上的截断
                advance(attr[0].length);
                match.attrs.push({name:attr[1],value:attr[3]}); // 存入属性名和属性值
            }
            if(end){
                // 到标签头>处，截断
                advance(end[0].length);
                return match
            }
        }
    }
}
```

- 标记静态树，树得遍历标记 markup
- 通过ast产生的语法树，生成代码--》render函数 codegen




## 知识重点
- Vue中响应式数据原理

- Vue中模板编译原理
