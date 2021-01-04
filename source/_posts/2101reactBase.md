title: React入坑笔记
date: 2021-01
categories: React
tags:
- React
- Javascript

---

第一篇React学习笔记，初涉～

<!-- more -->

MVVM框架。

### 特性
- 声明式
- 组件化（侧重UI）
- 灵活（单／多页面 服务端渲染 RN-App）

### 编写Hello world

#### 基本语法
- React.render()
- React.creatElement()
- React.Conponent

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>React</title>
    <script crossorigin src="https://unpkg.com/react@16/umd/react.development.js"></script>
    <script crossorigin src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"></script>
</head>
<body>
    <div id="app"></div>
    <script>
        var hello = React.createElement('h1', {}, 'hello world');
        ReactDOM.render(hello, document.getElementById('app'));
    </script>
</body>
</html>
```

后期会使用ES6的语法糖，引入babel

```html
<script src="https://cdn.bootcss.com/babel-standalone/6.26.0/babel.js"></script>

// ...

<script type="text/babel">
    ReactDOM.render(<h1>hello world</h1>, document.getElementById('app'))
</script>

```

### JSX

### 元素渲染

