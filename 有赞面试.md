## CSS选择器以及优先级

- !important
- 内联样式（1000）
- ID选择器（0100）
- 类选择器/属性选择器/伪类选择器（0010）
- 元素选择器/关系选择器/伪元素选择器（0001）
- 通配符选择器（0000）


##  BFC

BFC 全称为 块格式化上下文 (Block Formatting Context) 。

触发BFC的条件：

- 根元素或其它包含它的元素
- 浮动元素 (元素的 float 不是 none)
- 绝对定位元素 (元素具有 position 为 absolute 或 fixed)
- 内联块 (元素具有 display: inline-block)
- 表格单元格 (元素具有 display: table-cell，HTML表格单元格默认属性)
- 表格标题 (元素具有 display: table-caption, HTML表格标题默认属性)
- 具有overflow 且值不是 visible 的块元素，
- display: flow-root
- column-span: all 应当总是会创建一个新的格式化上下文，即便具有 column-span: all 的元素并不被包裹在一个多列容器中。



###  盒模型

包括**内容区域**、**内边距区域**、**边框区域**和**外边距区域**。

`box-sizing: content-box`（W3C盒子模型）：元素的宽高表现为大小为`内容`的大小。
`box-sizing: border-box`（IE盒子模型）：元素的宽高表表现为`内容 + 内边距 + 边框`的大小。背景会延伸到边框的外沿。


IE5.x和IE6在怪异模式中使用非标准的盒子模型，这些浏览器的`width`属性不是**内容**的宽度，而是**内容**、**内边距**和**边框**的宽度的总和。


## 左侧固定宽度，右侧自适应的布局


dom结构

``` html
<div class="box">
  <div class="box-left"></div>
  <div class="box-right"></div>
</div>
```

- 利用`float + margin`实现

``` javascript
.box {
 height: 200px;
}

.box > div {
  height: 100%;
}

.box-left {
  width: 200px;
  float: left;
  background-color: blue;
}

.box-right {
  margin-left: 200px;
  background-color: red;
}
```

- 利用`calc`计算宽度

``` javascript
.box {
 height: 200px;
}

.box > div {
  height: 100%;
}

.box-left {
  width: 200px;
  float: left;
  background-color: blue;
}

.box-right {
  width: calc(100% - 200px);
  float: right;
  background-color: red;
}
```
- 利用`float + overflow`实现

``` javascript
.box {
 height: 200px;
}

.box > div {
  height: 100%;
}

.box-left {
  width: 200px;
  float: left;
  background-color: blue;
}

.box-right {
  overflow: hidden;
  background-color: red;
}
```

- 利用`flex`实现

``` javascript
.box {
  height: 200px;
  display: flex;
}

.box > div {
  height: 100%;
}

.box-left {
  width: 200px;
  background-color: blue;
}

.box-right {
  flex: 1; // 设置flex-grow属性为1，默认为0
  overflow: hidden;
  background-color: red;
}
```


## 跨域





## HTTP2和HTTP1的区别


相对于HTTP1.0，HTTP1.1的优化：

- 缓存处理：多了Entity tag，If-Unmodified-Since, If-Match, If-None-Match等缓存信息（HTTTP1.0 If-Modified-Since,Expires）
- 带宽优化及网络连接的使用
- 错误通知的管理
- Host头处理
- 长连接： HTTP1.1中默认开启Connection： keep-alive，一定程度上弥补了HTTP1.0每次请求都要创建连接的缺点。

相对于HTTP1.1，HTTP2的优化：

- HTTP2支持二进制传送（实现方便且健壮），HTTP1.x是字符串传送
- HTTP2支持多路复用
- HTTP2采用HPACK压缩算法压缩头部，减小了传输的体积
- HTTP2支持服务端推送


HTTP和HTTPS的区别：

HTTP:  HTTP - TCP
HTTPS:  HTTP - SSL/TLS - TCP
SPDY：HTTP - SPDY - SSL - TCP




## 缓存

强缓存和协商缓存。

强缓存不过服务器，协议缓存需要过服务器，协商缓存返回的状态码是304。


## 首屏加载优化

- Vue-Router路由懒加载（利用Webpack的代码切割）
- 使用CDN加速，将通用的库从vendor进行抽离
- Nginx的gzip压缩
- Vue异步组件
- 服务端渲染SSR
- 如果使用了一些UI库，采用按需加载
- webpack开启gzip压缩
- 如果首屏为登录页，可以做成多入口



## Node alias

- 全局变量 
- 环境变量
- 自己HACK一个@符号，指向特定的路径
- HACK `require`方法


https://segmentfault.com/a/1190000010998044
http://chashaobao.net/2017/09/03/alias-require-hack/
https://www.zhihu.com/question/26621212

##作用域链



https://github.com/mqyqingfeng/Blog/issues/6
https://juejin.im/entry/57f5d492bf22ec006475238f


## null和undefined的区别

## 闭包


## VUE响应式原理
## Event Loop
node.js的Event Loop和浏览器的Event Loop有什么区别
