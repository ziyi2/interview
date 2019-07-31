# 一

## 手写Promise/Async

## Webpack实现原理

## 懒加载的Webpack配置

## Node.js的加载机制（require和module.exports）

加载机制和ES6的区别

## 擅长什么

## React和Vue的区别

## React、Vue和JQuery在什么场景下怎么选型

## 技术选型带来的好处（为什么是这样的技术选型）

## Vue的响应式原理

## 什么情况下会阻塞DOM渲染

## 有哪些异步函数

## 讲讲MVVM，并说明与MVC有什么区别


# 二

## Event Loop
## Webpack的loader和plugins的区别
## HTTP状态码206
## React高阶组件
## React和Vue的区别
## Service Worker缓存文件处理
## 跨域
## 文件上传的头信息application/x-www-form-urlencoded（文件上传的二进制是怎么处理的）
## Vue响应式原理
## Vue 2.x中的proxy做什么用
## 性能优化、首屏加载优化

# 三
## z-index
## 绝对定位
## CSS3动画
## 可视化
## 你觉得你最擅长的是什么
## flex两列布局
## 事件委托有什么优点（起码两个以上）
## ES6\ES7\ES8的特性
## target/currentTarget/relateTarget
## 业务形态
## 事件流
## 你觉得你有做过推动流程、简化流程或者改善流程的事件么，举例说明

# 四
## computed的实现
## Vue的实现原理
## TCP通讯
## Chrome插件屏蔽广告
## equl的实现，如何实现相等（基本类型和引用类型）



## watch运行原理

watch的分类：

- deep watch（深层次监听）
- user watch（用户监听）
- computed watcher（计算属性）
- sync watcher（同步监听）


$watch很详细：https://www.cnblogs.com/jasonwang2y60/p/6754594.html


- watch的初始化在data初始化之后（此时的data已经通过Object.defineProperty的设置变成了响应式）
- watch的key会在Watcher里进行值的读取，也就是立马执行get获取value（从而实现data对应的key执行getter实现对于watch的依赖收集），此时如果有immediate属性那么立马执行watch对应的回掉函数
- 当data对应的key发生变化时，触发user watch实现watch回调函数的执行


https://juejin.im/post/5d2065f2518825747817131a

## computed运行原理


- computed的属性是动态挂载到vm实例上的，和普通的响应式数据在data里声明不同
- 设置computed的getter，如果执行了computed对应的函数，由于函数会读取data属性值，因此又会触发data属性值的getter函数，在这个执行过程中就可以处理computed相对于data的依赖收集关系了
- 首次计算computed的值时，会执行vm.computed属性对应的getter函数（用户指定的computed函数，如果没有设置getter，那么将当前指定的函数赋值computed属性的getter），进行上述的依赖收集
- 如果computed的属性值又依赖了其他computed计算属性值，那么会将当前target暂存到栈中，先进行其他computed计算属性值的依赖收集，等其他计算属性依赖收集完成后，在从栈中pop出来，继续进行当前computed的依赖收集


> 由于 this.firstName 和 this.lastName 都是响应式对象，这里会触发它们的 getter，根据我们之前的分析，它们会把自身持有的 dep 添加到当前正在计算的 watcher 中，这个时候 Dep.target 就是这个 computed watcher

具体步骤如下：

- data 属性初始化 getter setter
-  computed 计算属性初始化，提供的函数将用作属性 vm.reversedMessage 的 getter
-  当首次获取 reversedMessage 计算属性的值时，Dep 开始依赖收集
-  在执行 message getter 方法时，如果 Dep 处于依赖收集状态，则判定 message 为 reversedMessage 的依赖，并建立依赖关系
-  当 message 发生变化时，根据依赖关系，触发 reverseMessage 的重新计算
-  如果计算值没有发现变化，不会触发视图更新


通过以上的分析，我们知道计算属性本质上就是一个 computed watcher，也了解了它的创建过程和被访问触发 getter 以及依赖更新的过程，其实这是最新的计算属性的实现，之所以这么设计是因为 Vue 想确保不仅仅是计算属性依赖的值发生变化，而是当计算属性最终计算的值发生变化才会触发渲染 watcher 重新渲染，本质上是一种优化。


https://segmentfault.com/a/1190000010408657

https://ustbhuangyi.github.io/vue-analysis/reactive/computed-watcher.html#computed


https://juejin.im/post/5b98c4da6fb9a05d353c5fd7


## computed和watch的区别

-  watch 选项允许我们执行异步操作（访问一个 API）或高消耗性能的操作
-  computed 是计算一个新的属性，并将该属性挂载到 vm（Vue 实例）上，而 watch 是监听已经存在且已挂载到 vm 上的数据，所以用 watch 同样可以监听 computed 计算属性的变化（其它还有 data、props）
-  computed 本质是一个惰性求值的观察者，具有缓存性，只有当依赖变化后，第一次访问 computed 属性，才会计算新的值，而 watch 则是当数据发生变化便会调用执行函数
-  从使用场景上说，computed 适用一个数据被多个数据影响，而 watch 适用一个数据影响多个数据；

- 当需要数据在异步变化或者开销较大时，执行更新，使用watch会更好一些；而computed不能进行异步操作；
- computed可以用缓存中拿数据，而watch是每次都要运行函数计算，不管变量的值是否发生变化，而computed在值没有发生变化时，可以直接读取上次的值



## computed和methods的区别


- 在模板文件中，computed属性只需要写{{reverseMessage}}，而methods需要写成{{reverseMessage()}}，最明显的区别就是methods是方法，需要执行；
- computed属性只有在依赖的data发生变化时，才会重新执行，否则会使用缓存中的值，而methods是每次进入页面都要执行的，有些需要每次进入页面都执行的方法，需要使用methods，而computed属性比较节约。

## 数据为什么频繁变化但只更新一次

Vue 异步执行 DOM 更新。只要观察到数据变化，Vue 将开启一个队列，并缓冲在同一事件循环中发生的所有数据改变。如果同一个 watcher 被多次触发，只会被推入到队列中一次。这种在缓冲时去除重复数据对于避免不必要的计算和 DOM 操作上非常重要。然后，在下一个的事件循环“tick”中，Vue 刷新队列并执行实际 (已去重的) 工作。Vue 在内部尝试对异步队列使用原生的 Promise.then 和 MessageChannel，如果执行环境不支持，会采用 setTimeout(fn, 0) 代替。


> 另外，关于waiting变量，这是很重要的一个标志位，它保证flushSchedulerQueue回调（$nextTick中执行）允许被置入callbacks一次。


因为Vue的事件机制是通过事件队列来调度执行，会等主进程执行空闲后进行调度，所以先会去等待所有的同步代码执行完成之后再去一次更新。这样的性能优势很明显，比如：

现在有这样的一种情况，mounted的时候test的值会被++循环执行1000次。 每次++时，都会根据响应式触发setter->Dep->Watcher->update->run。 如果这时候没有异步更新视图，那么每次++都会直接操作DOM更新视图，这是非常消耗性能的。 所以Vue实现了一个queue队列，在下一个Tick（或者是当前Tick的微任务阶段）的时候会统一执行queue中Watcher的run。同时，拥有相同id的Watcher不会被重复加入到该queue中去，所以不会执行1000次Watcher的run。最终更新视图只会直接将test对的DOM的0变成1000。 保证更新视图操作DOM的动作是在当前栈执行完以后下一个Tick（或者是当前Tick的微任务阶段）的时候调用，大大优化了性能。





> 具体查看https://juejin.im/post/5ae3f0956fb9a07ac90cf43e


总结


执行顺序`update -> queueWatcher -> 维护观察者队列（重复id的Watcher处理） -> waiting标志位处理（保证需要更新DOM或者Watcher视图更新的方法flushSchedulerQueue只会被推入异步执行的$nextTick回调数组一次） -> 处理$nextTick（在为微任务或者宏任务中异步更新DOM）-> ` 




- Vue是异步更新Dom的，Dom的更新放在下一个宏任务或者当前宏任务的末尾（微任务）中进行执行
> 由于VUE的数据驱动视图更新是异步的，即修改数据的当下，视图不会立刻更新，而是等同一事件循环中的所有数据变化完成之后，再统一进行视图更新。在同一事件循环中的数据变化后，DOM完成更新，立即执行nextTick(callback)内的回调。

> vue和react一样，对dom的修改都是异步的。它会在队列里记录你对dom的操作并进行diff操作，后一个操作会覆盖前一个 然后更新dom

- Vue会维护一个观察者队列，拥有相同id的观察者是不会重复push到队列中
- Vue在处理观察者队列时会有一个wating标志位，可以保证flushSchedulerQueue（更新DOM的API）只会被推入$nextTick的回调数组一次


非常好的一篇文章：
https://segmentfault.com/a/1190000013314893

## Event Loop队列存放的顺序

### Event Loop原理
浏览器（多进程）包含了Browser进程（浏览器的主进程）、第三方插件进程和GPU进程（浏览器渲染进程），其中GPU进程（多线程）和Web前端密切相关，包含以下线程：


GUI渲染线程
JS引擎线程
事件触发线程（和EventLoop密切相关）
定时触发器线程
异步HTTP请求线程


> GUI渲染线程和JS引擎线程是互斥的，为了防止DOM渲染的不一致性，其中一个线程执行时另一个线程会被挂起。


JS引擎线程和事件触发线程

浏览器页面初次渲染完毕后，JS引擎线程结合事件触发线程的工作流程如下：


（1）同步任务在JS引擎线程（主线程）上执行，形成执行栈（Execution Context Stack）。
（2）主线程之外，事件触发线程管理着一个任务队列（Task Queue）。只要异步任务有了运行结果，就在任务队列之中放置一个事件。
（3）执行栈中的同步任务执行完毕，系统就会读取任务队列，如果有异步任务需要执行，将其加到主线程的执行栈并执行相应的异步任务。


> 主线程执行当前任务,主线程的代码同步执行,并把遇到的事件和回调注册到事件表中。
当事件表中的事件被触发时,将会把对应的处理函数推送到任务队列当中。


根据macrotasks队列和microtasks队列的执行时机不同,因此需要注意异步代码的执行顺序

其原则是:

- macrotasks将进入宏任务队列,将在下一次eventloop时进行调用(先进先出)
常见的包括XHR,JSONP,setTimeout,setInterval等
- microtasks将进入微任务队列,在当前eventloop结束前进行调用
microtasks常见的有Promise.then,process.nextTick(nodejs)等


事件触发线程管理的任务队列是如何产生的呢？事实上这些任务就是从JS引擎线程本身产生的，主线程在运行时会产生执行栈，栈中的代码调用某些异步API时会在任务队列中添加事件，栈中的代码执行完毕后，就会读取任务队列中的事件，去执行事件对应的回调函数，如此循环往复，形成事件循环机制


## Grid

网格布局

https://juejin.im/post/5d3946335188255ac45f7331

http://www.ruanyifeng.com/blog/2019/03/grid-layout-tutorial.html


Flex两列和三列布局

https://blog.csdn.net/ace_arm/article/details/81044582


## 绝对定位和固定定位、z-index

### 绝对定位

- 一旦给元素加上absolute或float就相当于给元素加上了display:block
- absolute元素覆盖正常文档流内元素（不用设z-index，自然覆盖）
- 可以减少重绘和回流的开销（如absolute+ top:-9999em，或absolute + visibility:hidden，将动画效果放到absolute元素中）


### 属性介绍

- static，默认值。位置设置为static的元素，它始终会处于文档流给予的位置。
- inherit，规定应该从父元素继承 position 属性的值。但是任何的版本的 Internet Explorer （包括 IE8）都不支持属性值 “inherit”。
- fixed，生成绝对定位的元素。默认情况下，可定位于相对于浏览器窗口的指定坐标。元素的位置通过 “left”, “top”, “right” 以及 “bottom” 属性进行规定。不论窗口滚动与否，元素都会留在那个位置。但当祖先元素具有transform属性且不为none时，就会相对于祖先元素指定坐标，而不是浏览器窗口。
- absolute，生成绝对定位的元素，相对于距该元素最近的已定位的祖先元素进行定位。此元素的位置可通过 “left”、”top”、”right” 以及 “bottom” 属性来规定。
- relative，生成相对定位的元素，相对于该元素在文档中的初始位置进行定位。通过 “left”、”top”、”right” 以及 “bottom” 属性来设置此元素相对于自身位置的偏移。

> 浮动、绝对定位和固定定位会脱离文档流，相对定位不会脱离文档流，绝对定位相对于该元素最近的已定位的祖先元素，如果没有一个祖先元素设置定位，那么参照物是body层。


绝对定位相对于包含块的起始位置：

- 如果祖先元素是块级元素，包含块则设置为该元素的内边距边界。
- 如果祖先元素是行内元素，包含块则设置为该祖先元素的内容边界。


问答题：

-  定位的元素的起始位置为父包含块的内边距（不会在border里，除非使用负值，会在padding里）
-  定位的元素的margin还是能起作用的
- background属性是会显示在border里的
- z-index是有层叠层级的，需要考虑同一个层叠上下文的层叠优先级
- z-index是负值不会覆盖包含块的背景色（但是如果有内容，会被包含块的内容覆盖）
- z-index的值影响的元素是定位元素以及flex盒子
- 上面一个定位元素，下面一个正常流的元素，定位元素会覆盖在正常流元素之上，除非给z-index是负值
- 页面根元素html天生具有层叠上下文，称之为“根层叠上下文”


![enter image description here](https://img-blog.csdn.net/20160824140943500)



## HTTP状态码206


https://juejin.im/post/5b555f055188251af25700aa


## 手写Promise


## Webpack实现原理


## 什么情况下会阻塞DOM解析（注意不是页面渲染）

https://juejin.im/post/587f4afb61ff4b00651b3c18


## Webpack的loader和plugins的区别

## Service Worker缓存文件处理

- 缓存静态资源
- 离线体验
- 网页中图片是很消耗带宽资源的，用户等待网站加载，很多时候都是在等图片，而大多数放在CDN上的图片，都支持添加后缀参数获取不同分辨率照片的功能。假设我们有办法知道一个用户的网络条件的好坏（至于如何判断一个用户的网络条件，是另外一件事，可以让用户选择，也可用技术手段解决），把用户分级，暂且分为两级：网速快的和网速慢的。我们把网速级别信息放到HTTP请求的header中（或其它你想得到的合适的地方），当发起图片请求的时候，我们有机会拿到用户的网络级别，如果是网速快的用户，我们通过后缀参数返回CDN上高分辨率的图片，反之相反。



> https://juejin.im/post/5d26aec1f265da1ba56b47ea


## DOM事件流、事件委托有什么优点



## TCP通讯 


## Chrome插件屏蔽广告

什么是Chrome插件：  https://github.com/sxei/chrome-plugin-demo




## equl的实现，如何实现相等（基本类型和引用类型）


## Object.is(Object系列)

## Node.js的加载机制（和ES6加载机制的区别）


## 文件上传的二进制是怎么处理的


## ES5继承






