# 1

## CSS选择器以及优先级

- !important
- 内联样式（1000）
- ID选择器（0100）
- 类选择器/属性选择器/伪类选择器（0010）
- 元素选择器/关系选择器/伪元素选择器（0001）
- 通配符选择器（0000）


##  BFC

### 什么是BFC
BFC 全称为 块级格式化上下文 (Block Formatting Context) 。BFC是 W3C CSS 2.1 规范中的一个概念，它决定了元素如何对其内容进行定位，以及与其他元素的关系和相互作用，当涉及到可视化布局的时候，Block Formatting Context提供了一个环境，HTML元素在这个环境中按照一定规则进行布局。一个环境中的元素不会影响到其它环境中的布局。比如浮动元素会形成BFC，浮动元素内部子元素的主要受该浮动元素影响，两个浮动元素之间是互不影响的。这里有点类似一个BFC就是一个独立的行政单位的意思。


自我理解：

也可以说BFC就是一个作用范围。可以把它理解成是一个独立的容器，并且这个容器的里box的布局，与这个容器外的毫不相干。

这么多性质有点难以理解，但可以作如下推理来帮助理解：html的根元素就是html，而根元素会创建一个BFC，创建一个新的BFC时就相当于在这个元素内部创建一个新的html，子元素的定位就如同在一个新html页面中那样，而这个新旧html页面之间时不会相互影响的。



### 触发BFC的条件

- 根元素或其它包含它的元素
- 浮动元素 (元素的 float 不是 none)
- 绝对定位元素 (元素具有 position 为 absolute 或 fixed)
- 内联块 (元素具有 display: inline-block)
- 表格单元格 (元素具有 display: table-cell，HTML表格单元格默认属性)
- 表格标题 (元素具有 display: table-caption, HTML表格标题默认属性)
- 具有overflow 且值不是 visible 的块元素
- 弹性盒（flex或inline-flex）
- display: flow-root
- column-span: all 应当总是会创建一个新的格式化上下文，即便具有 column-span: all 的元素并不被包裹在一个多列容器中。


### BFC的约束规则

- 内部的盒会在垂直方向一个接一个排列（可以看作BFC中有一个的常规流）；
- 处于同一个BFC中的元素相互影响，可能会发生margin collapse；
- 每个元素的margin box的左边，与容器块border box的左边相接触(对于从左往右的格式化，否则相反)。即使存在浮动也是如此；
- BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素，反之亦然；
- 计算BFC的高度时，考虑BFC所包含的所有元素，连浮动元素也参与计算；
- 浮动盒区域不叠加到BFC上；



### BFC可以解决的问题

- 垂直外边距重叠问题
- 去除浮动
- 自适用两列布局（float + overflow）

### 参考链接

https://juejin.im/post/5a4dbe026fb9a0452207ebe6
https://juejin.im/post/59b73d5bf265da064618731d

###  盒模型

包括**内容区域**、**内边距区域**、**边框区域**和**外边距区域**。

`box-sizing: content-box`（W3C盒子模型）：元素的宽高大小表现为`内容`的大小。
`box-sizing: border-box`（IE盒子模型）：元素的宽高表现为`内容 + 内边距 + 边框`的大小。背景会延伸到边框的外沿。


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

### 跨域行为

- 同源策略限制、安全性考虑
- 协议、IP和端口不一致都是跨域行为

###  JSONP

Web前端事先定义一个用于获取跨域响应数据的回调函数，并通过没有同源策略限制的script标签发起一个请求（将回调函数的名称放到这个请求的query参数里），然后服务端返回这个回调函数的执行，并将需要响应的数据放到回调函数的参数里，前端的script标签请求到这个执行的回调函数后会立马执行，于是就拿到了执行的响应数据。


缺点： JSONP只能发起GET请求


> 空的iframe+form实现POST请求。

### 如何实现一个JSONP

https://segmentfault.com/a/1190000015597029
https://zhangguixu.github.io/2016/12/02/jsonp/
https://www.cnblogs.com/iovec/p/5312464.html



### JSONP安全性问题

#### 安全性问题1（伪造请JSONP接口请求，CSRF攻击）

 前端构造一个恶意页面，请求JSONP接口，收集服务端的敏感信息。如果JSONP接口还涉及一些敏感操作或信息（比如登录、删除等操作），那就更不安全了。

> 解决方法：验证JSONP的调用来源（Referer），服务端判断Referer是否是白名单。或者部署随机Token来防御。https://www.jianshu.com/p/14f569b13dcc

#### 安全性问题2（XSS漏洞）

``` javascript 
不严谨的 content-type 导致的 XSS 漏洞

想象一下 jsonp 就是你请求 http://youdomain.com?callback=douniwan, 然后返回 douniwan({ data })

那假如请求 http://youdomain.com?callback=<script>alert(1)</script> 不就返回 <script>alert(1)</script>({ data }) 了吗，如果没有严格定义好 Content-Type（ Content-Type: application/json ），再加上没有过滤 callback 参数，直接当 html 解析了，就是一个赤裸裸的 XSS 了。
```

> 解决方法：严格定义 Content-Type: application/json，然后严格过滤 callback 后的参数并且限制长度（进行字符转义，例如<换成&lt，>换成&gt）等，这样返回的脚本内容会变成文本格式，脚本将不会执行。

#### 安全问题3（服务器被黑，返回一串恶意执行的代码）

可以将执行的代码转发到服务端进行JSONP内容是否合法，再返回结果。






### CORS（跨域资款共享）

#### 什么是CORS

cors（跨域资源共享 Cross-origin resource sharing），它允许浏览器向跨域服务器发出XMLHttpRequest请求，从而克服跨域问题，它需要浏览器和服务器的同时支持。

- 浏览器端会自动向请求头添加origin字段，表明当前请求来源。
- 浏览器端需要设置响应头的Access-Control-Allow-Methods，Access-Control-Allow-Headers，Access-Control-Allow-Origin等字段，指定允许的方法，头部，源等信息。
- 请求分为简单请求和非简单请求，非简单请求会先进行一次OPTION方法进行预检，看是否允许当前跨域请求。


#### 简单请求和非简单请求

- 简单请求

1) 请求方法是以下三种方法之一：

HEAD
GET
POST

2）HTTP的头信息不超出以下几种字段：

Accept
Accept-Language
Content-Language
Last-Event-ID
Content-Type：只限于三个值application/x-www-form-urlencoded、multipart/form-data、text/plain


- 非简单请求

非简单请求是那种对服务器有特殊要求的请求，比如请求方法是PUT或DELETE，或者Content-Type字段的类型是application/json。




#### 后端设置响应头信息

简单请求：

Access-Control-Allow-Origin：该字段是必须的。它的值要么是请求时Origin字段的值，要么是一个*，表示接受任意域名的请求。

Access-Control-Allow-Credentials：该字段可选。它的值是一个布尔值，表示是否允许发送Cookie。

Access-Control-Expose-Headers：该字段可选。CORS请求时，XMLHttpRequest对象的getResponseHeader()方法只能拿到6个基本字段：Cache-Control、Content-Language、Content-Type、Expires、Last-Modified、Pragma。如果想拿到其他字段，就必须在Access-Control-Expose-Headers里面指定。

非简单请求：

非简单请求的CORS请求，会在正式通信之前，增加一次HTTP查询请求，称为"预检"请求（preflight）。

Access-Control-Request-Method：该字段是必须的，用来列出浏览器的CORS请求会用到哪些HTTP方法，上例是PUT。

Access-Control-Request-Headers：该字段是一个逗号分隔的字符串，指定浏览器CORS请求会额外发送的头信息字段，上例是X-Custom-Header。


如果浏览器否定了"预检"请求，会返回一个正常的HTTP回应，但是没有任何CORS相关的头信息字段。这时，浏览器就会认定，服务器不同意预检请求，因此触发一个错误，被XMLHttpRequest对象的onerror回调函数捕获。

#### JSONP和CORS的对比

- CORS与JSONP的使用目的相同，但是比JSONP更强大。
- JSONP只支持GET请求，CORS支持所有类型的HTTP请求。JSONP的优势在于支持老式浏览器，以及可以向不支持CORS的网站请求数据。

#### 参考链接

https://github.com/HolyZheng/holyZheng-blog/issues/15
http://www.ruanyifeng.com/blog/2016/04/cors.html

### 其他

- Nginx反向代理
> 他的原理就是，服务器之间的通信是没有同源限制的，让本地服务器充当一个代理服务器的角色，帮助我们向目标服务器获取数据。因为我们在本地开发的时候，我们的代码运行在本地服务器之上，所以我们向本地服务器发送请求是不跨域的，我们就可以实现跨域了。

- postMessage
- document.domain



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

强缓存不过服务器，协商缓存需要过服务器，协商缓存返回的状态码是304。

两类缓存机制可以同时存在，强制缓存的优先级高于协商缓存，当执行强制缓存时，如若缓存命中，则直接使用缓存数据库数据，不在进行缓存协商。


### 强缓存

对于强制缓存，服务器响应的header中会用两个字段来表明——Expires和Cache-Control。

- Expires(HTTP1.0)

Exprires的值为服务端返回的数据到期时间。当再次请求时的请求时间小于返回的此时间，则直接使用缓存数据。
但由于服务端时间和客户端时间可能有误差，这也将导致缓存命中的误差，另一方面，Expires是HTTP1.0的产物，故现在大多数使用Cache-Control替代。

> 缺点：使用的是绝对时间，如果服务端和客户端的时间产生偏差，那么会导致命中缓存产生偏差。

- Pragma(HTTP1.0)

HTTP1.0时的遗留字段，当值为"no-cache"时强制验证缓存,

Pragma禁用缓存，如果又给Expires定义一个还未到期的时间，那么Pragma字段的优先级会更高。


> 服务端响应添加'Pragma': 'no-cache'，浏览器表现行为和刷新(F5)类似。

- Cache-Control(HTTP1.1)

Cache-Control有很多属性，不同的属性代表的意义也不同。
private：客户端可以缓存
public：客户端和代理服务器都可以缓存
max-age=t：缓存内容将在t秒后失效
no-cache：需要使用协商缓存来验证缓存数据
no-store：所有内容都不会缓存。


> 请注意no-cache指令很多人误以为是不缓存，这是不准确的，no-cache的意思是可以缓存，但每次用应该去想服务器验证缓存是否可用。no-store才是不缓存内容。

> 当在首部字段Cache-Control 有指定 max-age 指令时，比起首部字段 Expires，会优先处理 max-age 指令

> 命中强缓存的表现形式：Firefox浏览器表现为一个灰色的200状态码。Chrome浏览器状态码表现为200 (from disk cache)或是200 OK (from memory cache)


### 协商缓存

协商缓存需要进行对比判断是否可以使用缓存。浏览器第一次请求数据时，服务器会将缓存标识与数据一起响应给客户端，客户端将它们备份至缓存中。再次请求时，客户端会将缓存中的标识发送给服务器，服务器根据此标识判断。若未失效，返回304状态码，浏览器拿到此状态码就可以直接使用缓存数据了。

Last-Modified

服务器在响应请求时，会告诉浏览器资源的最后修改时间。

if-Modified-Since：浏览器再次请求服务器的时候，请求头会包含此字段，后面跟着在缓存中获得的最后修改时间。服务端收到此请求头发现有if-Modified-Since，则与被请求资源的最后修改时间进行对比，如果一致则返回304和响应报文头，浏览器只需要从缓存中获取信息即可。

- 如果真的被修改：那么开始传输响应一个整体，服务器返回：200 OK
- 如果没有被修改：那么只需传输响应header，服务器返回：304 Not Modified

if-Unmodified-Since: 从字面上看, 就是说: 从某个时间点算起, 是否文件没有被修改

- 如果没有被修改:则开始`继续'传送文件: 服务器返回: 200 OK
- 如果文件被修改:则不传输,服务器返回: 412 Precondition failed (预处理错误)

这两个的区别是一个是修改了才下载一个是没修改才下载。
Last-Modified 说好却也不是特别好，因为如果在服务器上，一个资源被修改了，但其实际内容根本没发生改变，会因为Last-Modified时间匹配不上而返回了整个实体给客户端（即使客户端缓存里有个一模一样的资源）。为了解决这个问题，HTTP1.1推出了Etag。


> 使用的是相对时间，不需要关心客户端和服务端的时间偏差。

- Etag

服务器响应请求时，通过此字段告诉浏览器当前资源在服务器生成的唯一标识（生成规则由服务器决定）


If-Match:

条件请求，携带上一次请求中资源的ETag，服务器根据这个字段判断文件是否有新的修改


If-None-Match： 

再次请求服务器时，浏览器的请求报文头部会包含此字段，后面的值为在缓存中获取的标识。服务器接收到次报文后发现If-None-Match则与被请求资源的唯一标识进行对比。


- 不同，说明资源被改动过，则响应整个资源内容，返回状态码200。
- 相同，说明资源无心修改，则响应header，浏览器直接从缓存中获取数据信息。返回状态码304.


但是实际应用中由于Etag的计算是使用算法来得出的，而算法会占用服务端计算的资源，所有服务端的资源都是宝贵的，所以就很少使用Etag了。


- 浏览器地址栏中写入URL，回车浏览器发现缓存中有这个文件了，不用继续请求了，直接去缓存拿。（最快）
- F5就是告诉浏览器，别偷懒，好歹去服务器看看这个文件是否有过期了。于是浏览器就胆胆襟襟的发送一个请求带上If-Modify-since。
- Ctrl+F5告诉浏览器，你先把你缓存中的这个文件给我删了，然后再去服务器请求个完整的资源文件下来。于是客户端就完成了强行更新的操作.



### 缓存场景

对于大部分的场景都可以使用强缓存配合协商缓存解决，但是在一些特殊的地方可能需要选择特殊的缓存策略

- 对于某些不需要缓存的资源，可以使用 Cache-control: no-store ，表示该资源不需要缓存
- 对于频繁变动的资源，可以使用 Cache-Control: no-cache 并配合 ETag 使用，表示该资源已被缓存，但是每次都会发送请求询问资源是否更新。
- 对于代码文件来说，通常使用 Cache-Control: max-age=31536000 并配合策略缓存使用，然后对文件进行指纹处理，一旦文件名变动就会立刻下载新的文件。



## 首屏加载优化

- Vue-Router路由懒加载（利用Webpack的代码切割）
- 使用CDN加速，将通用的库从vendor进行抽离
- Nginx的gzip压缩
- Vue异步组件
- 服务端渲染SSR
- 如果使用了一些UI库，采用按需加载
- webpack开启gzip压缩
- 如果首屏为登录页，可以做成多入口
- Service Worker缓存文件处理
- 使用link标签的rel属性设置   prefetch（这段资源将会在未来某个导航或者功能要用到，但是本资源的下载顺序权重比较低。也就是说prefetch通常用于加速下一次导航）、preload（preload将会把资源得下载顺序权重提高，使得关键数据提前下载好，优化页面打开速度）。





## Node alias

- 全局变量 
- 环境变量
- 自己HACK一个@符号，指向特定的路径
- HACK `require`方法


https://segmentfault.com/a/1190000010998044
http://chashaobao.net/2017/09/03/alias-require-hack/
https://www.zhihu.com/question/26621212

##作用域链

了解作用域链之前我们要知道一下几个概念：

- 函数的生命周期
- 变量和函数的声明
- Activetion Object（AO）、Variable Object（VO）

函数的生命周期：

创建：JS解析引擎进行预解析，会将函数声明提前，同时将该函数放到全局作用域中或当前函数的上一级函数的局部作用域中。

执行：JS引擎会将当前函数的局部变量和内部函数进行声明提前，然后再执行业务代码，当函数执行完退出时，释放该函数的执行上下文，并注销该函数的局部变量。

变量和函数的声明：

如果变量名和函数名声明时相同，函数优先声明。


Activetion Object（AO）、Variable Object（VO）：

AO：Activetion Object（活动对象）
VO：Variable Object（变量对象）


VO对应的是函数创建阶段，JS解析引擎进行预解析时，所有的变量和函数的声明，统称为Variable Object。该变量与执行上下文相关，知道自己的数据存储在哪里，并且知道如何访问。VO是一个与执行上下文相关的特殊对象，它存储着在上下文中声明的以下内容：

变量 (var, 变量声明);
函数声明 (FunctionDeclaration, 缩写为FD);
函数的形参


AO对应的是函数执行阶段，当函数被调用执行时，会建立一个执行上下文，该执行上下文包含了函数所需的所有变量，该变量共同组成了一个新的对象就是Activetion Object。该对象包含了：

函数的所有局部变量
函数的所有命名参数
函数的参数集合
函数的this指向


作用域链

当代码在一个环境中创建时，会创建变量对象的一个作用域链（scope chain）来保证对执行环境有权访问的变量和函数。作用域第一个对象始终是当前执行代码所在环境的变量对象（VO）。

如果是函数执行阶段，那么将其activation object（AO）作为作用域链第一个对象，第二个对象是上级函数的执行上下文AO，下一个对象依次类推。

在《JavaScript深入之变量对象》中讲到，当查找变量的时候，会先从当前上下文的变量对象中查找，如果没有找到，就会从父级(词法层面上的父级)执行上下文的变量对象中查找，一直找到全局上下文的变量对象，也就是全局对象。这样由多个执行上下文的变量对象构成的链表就叫做作用域链。


https://github.com/mqyqingfeng/Blog/issues/6
https://juejin.im/entry/57f5d492bf22ec006475238f


## null和undefined的区别

## 闭包


## VUE响应式原理



## Event Loop

事件触发线程管理的任务队列是如何产生的呢？事实上这些任务就是从JS引擎线程本身产生的，主线程在运行时会产生执行栈，栈中的代码调用某些异步API时会在任务队列中添加事件，栈中的代码执行完毕后，就会读取任务队列中的事件，去执行事件对应的回调函数，如此循环往复，形成事件循环机制。

JS中有两种任务类型：微任务（microtask）和宏任务（macrotask），在ES6中，microtask称为 jobs，macrotask称为 task。

宏任务： script （主代码块）、setTimeout 、setInterval 、setImmediate 、I/O 、UI rendering
微任务：process.nextTick（Nodejs） 、promise 、Object.observe 、MutationObserver


node.js的Event Loop和浏览器的Event Loop有什么区别

``` javascript
┌───────────────────────┐
┌─>│        timers         │<————— 执行 setTimeout()、setInterval() 的回调
│  └──────────┬────────────┘
|             |<-- 执行所有 Next Tick Queue 以及 MicroTask Queue 的回调
│  ┌──────────┴────────────┐
│  │     pending callbacks │<————— 执行由上一个 Tick 延迟下来的 I/O 回调（待完善，可忽略）
│  └──────────┬────────────┘
|             |<-- 执行所有 Next Tick Queue 以及 MicroTask Queue 的回调
│  ┌──────────┴────────────┐
│  │     idle, prepare     │<————— 内部调用（可忽略）
│  └──────────┬────────────┘     
|             |<-- 执行所有 Next Tick Queue 以及 MicroTask Queue 的回调
|             |                   ┌───────────────┐
│  ┌──────────┴────────────┐      │   incoming:   │ - (执行几乎所有的回调，除了 close callbacks 以及 timers 调度的回调和 setImmediate() 调度的回调，在恰当的时机将会阻塞在此阶段)
│  │         poll          │<─────┤  connections, │ 
│  └──────────┬────────────┘      │   data, etc.  │ 
│             |                   |               | 
|             |                   └───────────────┘
|             |<-- 执行所有 Next Tick Queue 以及 MicroTask Queue 的回调
|  ┌──────────┴────────────┐      
│  │        check          │<————— setImmediate() 的回调将会在这个阶段执行
│  └──────────┬────────────┘
|             |<-- 执行所有 Next Tick Queue 以及 MicroTask Queue 的回调
│  ┌──────────┴────────────┐
└──┤    close callbacks    │<————— socket.on('close', ...)
   └───────────────────────┘
```

Node.js的宏任务分了好几种，并且这几种放在了不同的task queue里，不同的task queue又有顺序的区别，而微任务又放在了每个task queue的末尾。

setTimeout/setInterval 属于 timers 类型；
setImmediate 属于 check 类型；
socket 的 close 事件属于 close callbacks 类型；
其他 MacroTask 都属于 poll 类型。
process.nextTick 本质上属于 MicroTask，但是它先于所有其他 MicroTask 执行；
所有 MicroTask 的执行时机，是不同类型 MacroTask 切换的时候。idle/prepare 仅供内部调用，我们可以忽略。
pending callbacks 不太常见，我们也可以忽略。





https://www.jianshu.com/p/deedcbf68880

https://www.jianshu.com/p/3ffd610c5fe4



## 回流和重绘


- 浏览器使用流式布局模型 (Flow Based Layout)。
- 浏览器会把HTML解析成DOM，把CSS解析成CSSOM，DOM和CSSOM合并就产生了Render Tree。
- 有了RenderTree，我们就知道了所有节点的样式，然后计算他们在页面上的大小和位置，最后把节点绘制到页面上。
- 由于浏览器使用流式布局，对Render Tree的计算通常只需要遍历一次就可以完成，但table及其内部元素除外，他们可能需要多次计算，通常要花3倍于同等元素的时间，这也是为什么要避免使用table布局的原因之一。


浏览器渲染过程如下：


- 解析HTML，生成DOM树，解析CSS，生成CSSOM树
- 将DOM树和CSSOM树结合，生成渲染树(Render Tree)
- Layout(回流):根据生成的渲染树，进行回流(Layout)，得到节点的几何信息（位置，大小）
- Painting(重绘):根据渲染树以及回流得到的几何信息，得到节点的绝对像素
- Display:将像素发送给GPU，展示在页面上。（这一步其实还有很多内容，比如会在GPU将多个合成层合并为同一个层，并展示在页面中。而css3硬件加速的原理则是新建合成层，这里我们不展开，之后有机会会写一篇博客）


何时发生回流：

- 添加或删除可见的DOM元素
- 元素的位置发生变化
- 元素的尺寸发生变化（包括外边距、内边框、边框大小、高度和宽度等）
- 内容发生变化，比如文本变化或图片被另一个不同尺寸的图片所替代。
- 页面一开始渲染的时候（这肯定避免不了）
- 浏览器的窗口尺寸变化（因为回流是根据视口的大小来计算元素的位置和大小的）

> 回流一定会触发重绘，而重绘不一定会回流，回流比重绘的代价要更高。

何时发生重绘：

当页面中元素样式的改变并不影响它在文档流中的位置时（例如：color、background-color、visibility等），浏览器会将新样式赋予给元素并重新绘制它，这个过程称为重绘。


有时即使仅仅回流一个单一的元素，它的父元素以及任何跟随它的元素也会产生回流。
现代浏览器会对频繁的回流或重绘操作进行优化：
浏览器会维护一个队列，把所有引起回流和重绘的操作放入队列中，如果队列中的任务数量或者时间间隔达到一个阈值的，浏览器就会将队列清空，进行一次批处理，这样可以把多次回流和重绘变成一次。


你访问以下属性或方法时，浏览器会立刻清空队列：


clientWidth、clientHeight、clientTop、clientLeft


offsetWidth、offsetHeight、offsetTop、offsetLeft


scrollWidth、scrollHeight、scrollTop、scrollLeft


width、height


getComputedStyle()


getBoundingClientRect()


以上属性和方法都需要返回最新的布局信息，因此浏览器不得不清空队列，触发回流重绘来返回正确的值。因此，我们在修改样式的时候，**最好避免使用上面列出的属性，他们都会刷新渲染队列。**如果要使用它们，最好将值缓存起来。


如何避免：


CSS 


- 避免使用table布局。
- 尽可能在DOM树的最末端改变class。
- 避免设置多层内联样式。
- 将动画效果应用到position属性为absolute或fixed的元素上。
- 避免使用CSS表达式（例如：calc()）。
- css3硬件加速（GPU加速）

JavaScript


- 避免频繁操作样式，最好一次性重写style属性，或者将样式列表定义为class并一次性更改class属性。
- 避免频繁操作DOM，创建一个documentFragment，在它上面应用所有DOM操作，最后再把它添加到文档中。
- 也可以先为元素设置display: none，操作结束后再把它显示出来。因为在display属性为none的元素上进行的DOM操作不会引发回流和重绘。
- 避免频繁读取会引发回流/重绘的属性，如果确实需要多次使用，就用一个变量缓存起来。
- 对具有复杂动画的元素使用绝对定位，使它脱离文档流，否则会引起父元素及后续元素频繁回流。


https://juejin.im/post/5c6cb7b4f265da2dae511a3d
https://juejin.im/post/5a9923e9518825558251c96a

# 2

1、1 1 2 3 5 8 计算第n个数的值

2、1块、4块、5块，求总数N块的最小硬币数






