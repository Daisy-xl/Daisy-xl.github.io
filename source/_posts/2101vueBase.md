title: Vue知识体系
date: 2020-08-19
categories: VUE
tags:
- VUE

---

VUE知识体系～

<!-- more -->

## 基础原理
### Vnode虚拟DOM

Vnode是虚拟节点，是对真实节点的抽象。产生的原因是过去通过innerHTML修改DOM修改视图，每次更新DOM节点都要重绘整个页面，项目一旦庞大起来，维护就成了一个难题。那换个角度想如果把真实Dom树抽象成为一棵以JS语法构建的抽象，然后通过修改抽象树的结构来转换成真实的Dom来重新渲染到视图。

Vue.js将DOM抽象成一个以JavaScript对象为节点的虚拟DOM树，以VNode节点模拟真实DOM，可以对这颗抽象树进行创建节点、删除节点以及修改节点等操作，在这过程中都不需要操作真实DOM，只需要操作JavaScript对象后只对差异修改，相对于整块的innerHTML的粗暴式修改，大大提升了性能。修改以后经过diff算法得出一些需要修改的最小单位，再将这些小单位的视图进行更新。这样做减少了很多不需要的DOM操作，大大提高了性能。

Vue是通过数据绑定来修改视图，当某个数据被修改的时候set方法会让闭包中的Dep调用notify通知所有订阅者Watcher，Watcher通过get方法执行vm._update(vm._render(), hydrating)。

updage方法的第一个参数是Vnode，在内部会将新老Vnode进行patch，这里使用的就是diff算法，对比较结果进行最小单位的修改。

diff算法是通过同层的树节点进行比较而非对树进行逐层搜索遍历的方式，所以时间复杂度只有O(n)，是一种相当高效的算法。

当oldVnode与vnode在sameVnode的时候才会进行patchVnode，也就是新旧VNode节点判定为同一节点的时候才会进行patchVnode这个过程，否则就是创建新的DOM，移除旧的DOM。

