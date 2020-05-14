title: Web前端常见面试题——HTML/CSS
date: 2020-04-02
categories: HTML
tags:
- HTML
- CSS

---

一篇用于面试前恶补的小文章，不断补充。

<!-- more -->

## 对HTML语义化的理解
- 用正确的标签做正确的事情
- 让页面的结构化，结构更清晰，便于对浏览器、搜索引擎解析
- 即使没有CSS，页面也可以呈现一种可读的文档格式
- 搜使用引擎的爬虫也依赖于HTML标记来确定上下文和各个关键字的权重，利于SEO
- 阅读源码更容易将网站分块，便于阅读理解维护

## Doctype的作用？标准模式与兼容模式之间的区别？
告诉浏览器的解析器用什么文档标准解析文档。
标准模式的排版和js运作模式都是以该浏览器支持的最高标准运行。
兼容模式中，页面以宽松的向后兼容的方式显示，以防老式站点在老式浏览器中无法工作。

## 盒模型？标准模型？IE模型？
页面是由若干盒模型堆砌起来的，每个HTML元素都是一个盒模型。
由外到内包含外边距 margin 边框 border 内边距 padding 内容 content
IE模型的宽高包含content+padding+border

## 理解BFC？形成条件？
BFC:块格式化上下文，是一个独立的布局环境，可以理解为一个容器，在这个容器内按照一定的规则进行物品摆放，并且不会影响其他环境中的物品。

BFC的特性：
1.是一个独立的容器，容器内子元素不会影响容器外的元素
2.BFC区域不会与float的元素区域重叠
3.计算BFC高度时，浮动元素也参与计算
4.垂直方向上的距离由margin决定
5.内部的box会在垂直方向上一个接一个的摆放

形成条件：
1.浮动元素 float: left | right | inherit(none除外)
2.position: absolte或者fixed定位
3.display: inline-block | inline-flex | tebel-cell
4.overflow: hidden | auto | scroll

## CSS选择器有哪些？哪些属性可以继承？
id选择器，类选择器，标签选择器，相邻选择器，后代选择器，通配符选择器，属性选择器吗，伪类选择器

可继承的属性：font-size, font-family color ul li dl dt dd
不可继承发属性：border, padding,margin,width,height

## CSS优先级计算算法
就近原则，同权重下样式定义最近为准，载入样式以最后载入的定位为准
优先级： !important>id>class>tag

## 伪类与伪元素
伪类： 用于向某些选择器添加特殊的效果（span:first-child），不互斥可叠加使用
伪元素：用于将特殊的想过添加到某些选择器(span:befor)，在一个选择器中只能出现一次，且只能出现在末尾

## display值有哪些？
block node inline-block list-item table inherit

## box-sizing常用属性有哪些？
box-sizing: content-box; // 默认的标准(W3C)盒模型元素效果
box-sizing: border-box; // 触发怪异(IE)盒模型元素的效果
box-sizing: inherit; // 继承父元素 box-sizing 属性的值

## 什么是外边距重叠？重叠的结果是什么？
marigin-collapse
相邻的两个盒子（兄弟或父子关系）的外边距可以结合成一个单独的外边距

结果：
- 正正取最大
- 负负取绝对值最大
- 正负求和

## 隐藏元素的方法
display:nonde 元素不可见，且不占用文档空间
visibility:hidden 隐藏元素，但依然占用文档空间
opacity：0 元素完全透明
position: absolute 设置left为一个很大的负值，元素飘出可见区
transform: scale(0) 设置元素缩放为无限小，元素不可见，但位置保留

## line-height怎么理解？
是指一行字的高度，包含字间距，实际上是下一行基线到上一行基线的距离。
一个容器没有设置高度，那么撑开容器高度的就是line-height
line-height设置为height一样可以实现单行文字居中

## 行内元素有哪些？块级元素有哪些？空元素？
行内元素：a b span img input select strong
块级元素：div ul ol li dl dt dd h1-h6 p
空元素： br hr img input link meta

## position有哪些值？区别？
position: 生成绝对定位的元素，相对于值不是static的第一个父元素进行定位
relative：生成相对定位的元素，相对于正常位置进行定位
fixed：生成绝对定位的元素，相对于浏览器窗口进行定位
static：默认值，无定位，元素出现在正常流中

## HTML5的新特性
- 语义化更好的标签 header footer nav section等
- 用于媒介回放的video和audio
- canvas画布
- 拖放drag
- 表单控件，calendar，date，time，email，url，search
- 存储功能：localStorage和sessionStorage
- 新的技术webworker,websocket,Geolocation

## SVG与canvas的区别
SVG 是一种使用 XML 描述 2D 图形的语言
Canvas 通过 JavaScript 来绘制 2D 图形
SVG 基于 XML，这意味着 SVG DOM 中的每个元素都是可用的，在SVG 中，每个被绘制的图形均被视为对象
如果 SVG 对象的属性发生变化，那么浏览器能够自动重现图形
Canvas 是逐像素进行渲染的
在 canvas 中，一旦图形被绘制完成，它就不会继续得到浏览器的关注
如果其位置发生变化，那么整个场景也需要重新绘制，包括任何或许已被图形覆盖的对象

## 为什么要初始化CSS样式
浏览器兼容问题，不同浏览器对标签的默认值不同，如果不初始化会出现浏览器之间的页面显示差异，可以使用reset.css等做css初始化

## rgba（）和opacity的透明效果有什么不同？
raba()只作用于元素自身的颜色或背景色，子元素不继承透明效果
opacity作用于元素及元素内的所有内容的透明度

## cookies,sessionStorage,localStorage的区别
- cookies是往往用于存储用户身份信息，加密后存储，数据大小不超过4k，关闭浏览器等不会清除，除非手动清除cookies或者到期
- sessionStorage 不发数据给服务器，存储大小比较大，浏览器关闭后自动删除
- localStorage 不发数据给服务器，存储大小比较大，存储持久，不主动删除数据不会丢失

## 浏览器是怎么对HTML5的离线储存资源进行管理和加载的？
1）在线的情况下，浏览器发现 html 标签有 manifest 属性，它会请求 manifest 文件
2）如果是第一次访问app，那么浏览器就会根据 manifest 文件的内容下载相应的资源并且进行离线存储
3）如果已经访问过app且资源已经离线存储了，浏览器会对比新的 manifest 文件与旧的 manifest 文件，如果文件没有发生改变，就不做任何操作。如果文件改变了，那么就会重新下载文件中的资源并进行离线存储
4）离线的情况下，浏览器就直接使用离线存储的资源

## 页面导入样式时，使用 link 和 @import 有什么区别？
link属于html标签，除了加载css之外，可以用于定RSS等，页面加载时会同时加载，无兼容问题
@import只用于加载css，等页面加载完才加载，只在IE5以上被识别

## 清浮动方法
- 在最后一个浮动元素标签后，新加一个标签，设置clear:both 
优点：通俗易懂，方便
缺点：增加无意义标签，语义化差
- 父级元素增加overflow:hidden，通过触发BFC清除浮动
优点：代码简洁
缺点：内容增多的时候容易造成不会自动换行导致内容被隐藏掉，无法显示要溢出的元素
- 在父元素上使用after伪元素清除浮动（推荐使用）
优点：符合闭合浮动思想，结构语义化正确
缺点：ie6-7不支持伪元素，需使用zoon:1触发hasLayout
- 使用before和after双伪元素清除浮动
优点：代码更简洁
缺点：用zoom:1触发hasLayout

## 什么是响应式设计？响应式设计的基本原理是什么？如何兼容低版本的IE？
响应式设计就是网站能够兼容多个终端（包括PC端，手机平板终端），而不是为每个终端做一个特定的版本

基本原理是利用CSS3媒体查询，为不同尺寸的设备适配不同样式
对于低版本的IE，可采用JS获取屏幕宽度，然后通过resize方法来实现兼容

## 水平居中、垂直居中的方法有哪些？
### 水平居中
- inline-blockzi子元素实现水平居中（父级设置text-align:center）

- 块级元素可以使用width配合margin:0 auto使用 或者display:table配合margin: 0 auto

- 绝对定位实现水平居中：父元素相对定位。子元素绝对定位，上下左右都设置为0，overfolow设为auto避免溢出，子元素需要设置宽度，否则被拉伸

- flex布局（IE9以下不支持）
flex属性是flex-grow（放大比例）, flex-shrink（缩小比例） 和 flex-basis（项目占据主轴空间）的简写，默认值为0 1 auto
注意，设为 Flex 布局以后，子元素的float、clear和vertical-align属性将失效。
单个元素水平居中  伸缩容器上 display:flex; justify-content:center;
多个元素水平居中 增加width
伸缩项目上加 margin: 0 auto

- 栅格布局grid(IE10以下不支持)
容器上设置   display:grid; justify-items:center;
或网格项目中设置 justify-self: center 或 margin: 0 auto

### 垂直居中
- 新增inline-block兄弟元素，设置vertical-align

- 绝对定位配合transform位移
除了可以使用margin-top把div往上偏移之外，CSS3的transform属性也可以实现这个功能，通过设置div的transform: translateY(-50%)，意思是使得div向上平移（translate）自身高度的一半(50%)。

margin: 0 auto; /*水平居中*/ position: relative; 
top: 50%; /*偏移*/ transform: translateY(-50%);

- 使用flex弹性盒子布局display:flex
display: flex;
align-items: center; /*定义body的元素垂直居中*/
justify-content: center; /*定义body的里的元素水平居中*/

- 未知高度的块级子元素，采用table-cell + vertical-align
父元素设置为display: table，子元素设置成为display: table-cell;vertical-align: middle;

- 已知高度的块级子元素，采用绝对定位和负边距
position: absolute; top: 50%; height: 240px; margin-top: -120px;

## 你理解的flex弹性盒子布局
用于不同尺寸屏幕中创建可自动伸缩的布局，任何一个容器都可以指定为flex布局
设置里flex布局后，float,clear,vertical-align失效

容器默认存在两根轴：水平主轴和垂直的交叉轴，主轴开始位置叫main start 结束位置叫main end，交叉轴的开始位置叫cross start，结束位置脚cross end。

容器属性：
flex-direction: row | row-reverse | column | column-reverse
flex-wrap: nowrap | wrap | wrap-reverse
flex-flow: direction wrap
justify-content: flex-start | flex-end | center | space-between |space-around;
align-items: flex-start | flex-end | center | baseline | stretch;
align-content: flex-start | flex-end | center | space-between | space-around | stretch;

项目属性:
flex-grow: <number>; /* default 0 */ 放大比例
flex-shrink: <number>; /* default 1 */ 缩小比例
flex-basis: <length> | auto; /* default auto */ 项目占主轴空间
flex: none | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ] 以上三个的缩写

## 阐述px与em、rem的区别，以及你知道的其他css单位
px就是pixel像素的缩写，相对长度单位。常用于PC端的字体单位
em相对于当前父元素的font-size（并不是固定的）
rem相对于HTML根元素的font

其他css单位：
%百分比，一般来说就是相对于父元素
vw是相对视口（viewport）的宽度而定的，长度等于视口宽度的1/100
vh是相对视口（viewport）的高度而定的，长度等于视口高度的1/100
vm css3新单位，相对于视口的宽度或高度中较小的那个

### rem
所谓依赖根元素来计算的方式，就是先给予html元素一个font-size，然后我们所有的rem就根据这个font-size来计算
例如：html{ font-size:16px;}
那么我们这里的1rem就应该这么来计算：1x16=16px=1rem；浏览器默认为16px可能造成rem计算上的麻烦和多位小数，所以，我们也可以进行这种方式的初始化根元素：

html{
   font-size=62.5% //这里就是10/16x100%=62.5% 也就是默认10px的字号
}
这样初始化之后，我们来进行rem计算的时候，就会减少许多的麻烦。

### name 属性的 viewport 值（移动屏幕的缩放）

也就是可视区域。对于桌面浏览器，我们都很清楚 viewport 是什么，就是除去了所有工具栏、状态栏、滚动条等等之后用于看网页的区域，这是真正有效的区域。由于移动设备屏幕宽度不同于传统 web，因此我们需要改变 viewport 值。

实际上我们可以操作的属性有 4 个：
width – // viewport 的宽度 （范围从 200 到 10,000，默认为 980 像素）
height – // viewport 的高度 （范围从 223 到 10,000 ）
initial-scale – // 初始的缩放比例 （范围从 > 0 到 10）
minimum-scale – // 允许用户缩放到的最小比例
maximum-scale – // 允许用户缩放到的最大比例
user-scalable – // 用户是否可以手动缩放 (no，yes)
```
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no" />
```

### 移动设备上如何在不同分辨率下计算出根元素需要的font-size值？
#### 1.基于css
<img src="../../../../images/rem01.jpg" width="80%">
可以看出分辨率计算规则：
基准640宽度下屏幕对比比例为1 font-size为20px
320下 font-size 应该设置为才20/640=0.5
那么如何来根据不同分辨率定义font-size呢？当然是媒体查询了
```
@media only screen and (min-device-width: 320px)and (-webkit-min-device-pixel-ratio: 2) {
   //针对iPhone 4, 5c,5s, 所有iPhone6的放大模式，个别iPhone6的标准模式<br>　　html{<br>　　　　font-size:10px;<br>　　}
}
@media only screen and (min-device-width: 375px)and (-webkit-min-device-pixel-ratio: 2) {
　　//针对大多数iPhone6的标准模式<br>　　html{<br>　　　　font-size:12px;<br>　　}
}
   
@media only screen and (min-device-width: 375px)and (-webkit-min-device-pixel-ratio: 3) {
　　//针对所有iPhone6+的放大模式<br>　　html{<br>　　　　font-size:16px;<br>　　}
   
}
@media only screen and (min-device-width:412px) and (-webkit-min-device-pixel-ratio: 3) {
　　//针对所有iPhone6+的标准模式,414px写为412px是由于三星Nexus 6为412px，可一并处理<br>　　html{<br>　　　　font-size:20px;<br>　　}
}
```
```
@media only screen and (-webkit-device-pixel-ratio:.75){ /*低分辨率小尺寸的图片样式*/
 
}
 
@media only screen and (-webkit-device-pixel-ratio:1){ /*普通分辨率普通尺寸的图片样式*/
 
}
 
@media only screen and (-webkit-device-pixel-ratio:1.5){ /*高分辨率大尺寸的图片样式*/
 
}
```

#### 2.js
```
docEl.style.fontSize = 20 * (clientWidth / 320) + 'px';
```

rem布局需要基于根元素的基准量来做的，不同屏幕分辨率设置不同的基准量，那么对UI的还原度就会很高，但是...发现了一个rem的问题...就是如果页面设计比较看重元素间隔和高度的话...那么用rem布局就会比较难受

## 实现左边固定宽度，右边自适应（不限于一种方法）
1）浮动布局（左侧固定宽度并且左浮动，右侧使用margin-left）
2）使用定位（左侧固定宽度并且绝对定位，右侧使用margin-left）
3）overflow（左侧固定宽并且左浮动，右侧加overflow：hidden）
4）flex布局（父级元素设置display:flex，左侧设置固定宽，右侧flex:1）

## 使用css实现一个持续的动画
```
.animat{
     width:20px;
     height:20px;
     background:red;
     position:relative;
     animation:mymove 3s infinite;        
}
@keyframes mymove{
     from{left:0px;}
     to{left:200px;}
}
```

## css实现三角形
```
.triangle{ 
     width:0;
     height:0;
     border-width:20px;
     border-style:solid;
     border-color:transparent transparent red transparent;
}
```

## 移动端开发中有哪些常用的布局？
- 百分比流式布局
流动布局与固定宽度布局基本不同点就在于对网站尺寸的测量单位不同，流动布局就是使用百分比来代替px作为单位。 
优点是流动布局可以很好解决自适应需求。
缺点是不够灵活，添加元素时，需要更改其他元素的值

- flex布局
Flexbox是CSS3引入的新的布局模式，也称为弹性布局，他会根据页面的剩余宽度自动分配空间。 它决定了元素如何在页面上排列，使它们能在不同的屏幕尺寸和设备下可预测地展现出来。它能够扩展和收缩 flex 容器内的元素， 以最大限度地填充可用空间。Flexbox布局最适合应用程序的组件和小规模的布局，而网格布局更适合那些更大规模的布局。

- 媒体查询+rem布局
使用媒体查询可以根据不同的设备宽度加载不同的css样式。rem是一个相对单位，会根据根节点的字体大小来计算的，使用媒体查询和rem可以实现移动端的响应式。

- 固定宽度做法
在标签里把 viewport 加好,然后设想整个网页的宽度为 320px 即可。 其他地方根据 PC 端来布局。 
缺点:大屏手机显示网页比较宽,固定布局宽度参照永远是 320px,导致左右两 边会有空白。

- bootstrap布局
bootstrap是一个比较流行的响应式前端框架，利用bootstrap的栅格系统可以实现响应式的移动端布局。栅格系统：Bootstrap中定义了一套响应式的网格系统，其使用方式就是将一个容器划分成12列，然后通过col-xx-xx的类名控制每一列的占比， 在使用的时候，我们给相应的div设置col-lg-2 col-md-3 col-sm-4 col-xs-6，以此完成布局。

## 什么是圣杯布局和双飞翼布局，并说下实现原理
两者的功能相同，都是为了实现一个两侧宽度固定，中间宽度自适应的三栏布局。
### 圣杯
<img src="../../../../images/cup.jpg" width="80%">
1. header和footer占屏幕全部高度，高度固定
2. 中间的contaier部分是一个三栏布局
3. left和right宽度固定，middle自适应填满整个区域；高度为三栏中最大的高度；
#### 浮动布局(兼容性最好但是略显复杂)
```
<body>
        <header>header</header>
        <div class="container">
            <div class="middle">middle</div>
            <div class="left">left</div>
            <div class="right">right</div>
        </div>
        <footer>footer</footer>
</body>
```
header和footer就直接定高，width设为100%就好;container也设为100%；left和right定宽；middle宽度100%；让container下的div都向左浮动。
```
header,footer{
    height:100px;
    width:100%; 
    background-color: antiquewhite;
}
.container{
    height:200px;
}
.container>div{
    float: left;
}

.left{
    width:200px;
    height:200px;
    background-color: burlywood;
}

.right{
    width:300px;
    height:200px;
    background-color: burlywood;
}

.middle{
    width:100%;
    height:200px;
    background-color: #b0f9c2;
}
```
因为middle先渲染的且宽度为百分百，所以left和right被挤到了下面；为了让他们都在一行显示，让left左外边距向左偏移整行的宽度；让right的左外边距向左偏移right自身的宽度。
```
.left{
    ...
     margin-left: -100%;
}
.right{
    ...
    margin-left: -300px;
}
```
然而middle里的内容被left覆盖了。。我们需要给container部分设置左右padding，向中间挤压，然后将left和right设置成相对定位，将其固定到正确位置。
```
.container{
   ...
    padding-left:200px;
    padding-right:300px;
}
```
将div.left和div.right设置相对定位。并给container设置高度，其子级div高度为百分百。
```
.container>div{
    ...
    height:100%;
    position: relative;
}

.left{
    ...
    position:relative;
    left:-200px;
}

.right{
    ...
    position:relative;
    right:-300px;
}
```
#### flex布局
```
 <body>
    <header>header</header>
    <div class="container">
        <div class="left">left</div>
        <div class="middle">middle</div>
        <div class="right">right</div>
    </div>
    <footer>footer</footer>
</body>
```
flex布局非常简单，只需在container中设置flex即可。两边和上面代码一样就不贴了。
```
.container{
    display: flex;
    flex-direction: row;
}
```
#### grid布局
```
body{
    display:grid;
}
header{
    grid-row:1;
    grid-column:1/5;
    background-color: antiquewhite;
}
footer{
    grid-row:3;
    grid-column:1/5;
    background-color: antiquewhite;
}

.left{
    grid-row:2;
    grid-column:1/2;
    background-color: burlywood;
}

.right{
    grid-row:2;
    grid-column:4/5;
    background-color: burlywood;
}

.middle{
    grid-row:2;
    grid-column:2/4;
    background-color: #b0f9c2;
} 
```

### 双飞翼布局
双飞翼布局：对圣杯布局（使用相对定位对以后布局有局限性）的改进，消除相对定位
原理：主体元素上设置左右边距，预留两翼位置。左右两栏使用浮动和负边距归位，消除相对定位

## align-content与align-items的区别？
align-content:center对单行是没有效果的
而align-items:center不管是对单行还是多行都有效果
我们日常开发中用的比较多的就是align-items

## 去除inline-block出现间距的几种方法
造成空白间隙的原因是在标签和标签之间使用了空格或换行符。
- 移除空格和换行符
- 设置父元素吃的font-size为0，子元素重新设置font-size
- 利用负margin-left（不推荐，具体负margin多少取决于字体的大小）
- letter-spacing（字符边距）：负值 or word-spacing（单词边距） 负值

## title和h1,b和strong，i和em的区别
title属性没有明确意义只表示是个标题，H1则表示层次明确的标题，对页面信息的抓取有很大的影响
b是一个实体标签，展示强调内容。strong是标明重点内容，有语气加强的含义
i内容展示为斜体，em表示强调的文本

## <script>、<script defer>、<script async>三者之间区别
没有defer或async属性，浏览器会立即加载并执行相应的脚本
有了async属性，表示后续文档的加载和渲染与js脚本的加载和执行是并行进行的，即异步执行
有了defer属性，加载后续文档的过程和js脚本的加载是并行进行的（此时仅加载不执行）
defer和async在网络加载过程是一致的，都是异步执行
两者的区别在于脚本加载完成之后何时执行
如果存在多个有defer属性的脚本，那么它们是按照加载顺序执行脚本的
对于async，它的加载和执行是紧挨的，无论声明顺序如何，只要加载完成立刻执行

## data-的用法，以及他的优势
data-代表自定义属性，可以在所有的 HTML 元素中嵌入数据
自定义的数据可以让页面拥有更好的交互体验
属性名不要包含大写字母，在 data- 后必须至少有一个字符
该属性可以是任何字符串
自定义属性前缀 "data-" 会被客户端忽略

## 实现多个标签页之间通信
websocket,shareworker,localstorage,cookies

## 实现不使用border画出1px高的线
```
<div style="height:1px;overflow:hidden;background:red"></div>
```

## 怎么让Chrome支持小于12px 的文字
```
.shrink{
      -webkit-transform:scale(0.8);
       -o-transform:scale(1);
       display:inline-block;
}

```

## 一个满屏 品 字布局 如何设计?
上面的div宽100%
下面的两个div分别宽50%
然后用float或者inline使其不换行即可


## 经常遇到的浏览器的兼容性有哪些？解决方法是什么？
1）PNG24位的图片在ie6浏览器上出现背景，解决方案是做成PNG8
2）浏览器默认的margin和padding不同。解决方案：*{margin:0;padding:0;}
3）Chrome中文界面下默认会将小于12px的文本强制按照12px显示，可通过加入CSS属性-webkit-text-size-adjust:none解决
4）IE下，可以使用获取常规属性的方法来获取自定义属性，也可以使用getAttribute()获取自定义的属性
在FireFox下，只能使用getAttribute()获取自定义属性；解决方法：统一通过getAttribute()获取自定义属性
5）IE下，even对象有x,y属性，但是没有pageX,pageY属性
在Firefox下，even对象有pageX,pageY属性，但是没有x,y属性
解决方法：(条件注释)缺点是在IE浏览器下可能会增加额外的HTTP请求数

TODO

## 平移放大一个元素
```
transform: translateX(10px)
transform:scale(2)
```

## css中动画实现方式

TODO

## 你知道的CSS预处理器，以及预处理器作用

TODO

## 如何解决CSS模块化

TODO

## li与li之间有看不见的空白间隔是什么原因引起的？有什么解决办法？


## 描述一下你对渐进增强和优雅降级的理解


## 解释css sprites，如何使用
css精灵又称雪碧图
将多个小图片整个到一张大的背景图上，再利用repeat、position进行精准操作
雪碧图减轻了服务器的请求次数，提高了页面性能

## 针对移动浏览器端开发页面，不期望用户放大屏幕，且要求“视口（viewport）”宽度等于屏幕宽度，视口高度等于设备高度，如何设置？
```
<meta name = "viewport" content = "width=device-width,initial-scale = 1.0,maximum-scale = 1.0">
```

## 简述几个css hack
(1) 图片间隙 
　　在div中插入图片，图片会将div下方撑大3px。hack1：将<div>与<img>写在同一行。hack2：给<img>添加display：block；
　　dt li 中的图片间隙。hack：给<img>添加display：block；
(2) 默认高度，IE6以下版本中，部分块元素，拥有默认高度（低于18px）
　　hack1：给元素添加：font-size：0；
　　hack2：声明：overflow：hidden；
　　表单行高不一致
　　hack1：给表单添加声明：float：left；height： ；border： 0；
　　鼠标指针
　　hack：若统一某一元素鼠标指针为手型：cursor：pointer；
　　当li内的a转化块元素时，给a设置float，IE里面会出现阶梯状
　　hack1：给a加display：inline-block；
　　hack2：给li加float：left；