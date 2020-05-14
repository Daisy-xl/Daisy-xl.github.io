title: HTML/CSS基础
date: 2020-05-11
categories: HTML、CSS
tags:
- HTML
- CSS

---

一篇用于面试前恶补的小文章，不断补充。

<!-- more -->


行内元素有哪儿些？块级元素有哪儿些？空元素（void）有哪儿些？
行内： a b span input button img select textarea
块级：div ul ol li dt h1-h6 p
空： br hr link meta

### html标签
HTML超文本标记语言
h1-h6  ol li div span
meta： name+content 关键字，描述  charset字符集
ul li：导航栏类结构，父子结构好，通用型，可维护
&nbsp空格  &lt小于号   &gt大于号
```
<img src="" alt="图片占位符，出错时展示" title="鼠标划过提示">
```

```
<a href="指向的目标地址" target="_blank"></a>
```
1. 超文本引用 内容任意 target在新标签页打开
2.a href=“#id“可作为锚点，跳回页面指定元素位置，可做置顶，双击置顶也可以
3.打电话 ```<a href="tel: 17610603269"></a>``` mailto:发邮件
4.协议限定符：```<a href="javascript:while(1){alert('让你手欠')"></a>```

form表单:发送数据

```
<form method="get/post" action="发送后端地址">
    <p>username: <input type="text" name="userName" placeholder="默认展示的文案"></p>
    <p>password: <input type="password" name="password"></p>
    你喜欢的人：
    a<input type="radio" name="a1" value="a"  checked="checked">
    b<input type="radio" name="a1" value="b">
    c<input type="radio"name="a1" value="c">
    <input type="submit" value="登陆">
    你喜欢的人：
    a<input type="checkbox" name="a1" value="a">
    b<input type="checkbox" name="a1" value="b">
    c<input type="checkbox"name="a1" value="c">

    <select name="province"> 
        <option>beijing</option>
        <option>shanghai</option>
        <option>tianjin</option>
    </select>
</form>
```

提交后name绑定的名称和值都会拼接到url后面，密码用md5加密

### 浏览器及其内核
IE      triden
Google  Chrome  webkit-->blint
Safari  webkit
FireFox Cecko
Opera   Presto

### Css层叠样式表
- 内联样式
- 页面级样式
- 外部链接文件
```
<link ref="stylesheet" type="text/css" href="">
```

#### 选择器及其权重
- !important Infinity
- 行间样式 1000
- id 100
- class 10
- 标签 1
- 通配符 0

#### 复杂选择器
- 父子选择器 div span {} 祖先也可以，不限制直接父子
- 直接父子 div > span
- 并列选择器 div.demo 不加空格，表示同一个标签下的某个class
- 分组选择器 div,span 公用一个代码块


浏览器处理父子选择器的顺序：自右向左。

不管什么关系的选择器，权重都直接相加。相同权重的相同样式后面覆盖前面。

#### font-size
设置的字体的高，从而设置字体大小

#### font-weight
设置元素的粗细，bold,,lighter,normal,bolder或者数字
bolder是否有效取决于字体包可否设置成更粗的样式

#### font-style
设置斜体 italic，normal

#### font-family
字体，默认宋体

#### color
设置字体颜色
- 英文单词
- 颜色代码 r()g()b() 每两位代表一个16进制的颜色 00-ff
- 颜色函数 rgb(0-255，0-255，0-255)

#### border:粗细，类型，颜色
border-width
border-style
border-color
画三角形利用border-left-color:transparent

#### text-
text-align对齐方式
text-indent：2em 首行缩进
line-height=1.2倍的font-size丢对应的height
text-decoration: underline,overline,line-through

### 行级元素与块级元素
#### 行级元素 span a strong em del 
- 内容决定元素所占位置
- 不可以通过css改变宽高

#### 块级元素 div p input ul ol li form address
- 独占一行
- 可以通过css改变宽高

#### 行级块元素 img
- 内容决定大小
- 可以改宽高

