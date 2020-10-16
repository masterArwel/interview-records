# interview-records
## 每天弄懂一道面试题， 每天提升一点点
## 暂时只是对面试中被问到的技术问题进行整理, 后续会一步一步完善答案



### 1. 浏览器缓存问题
> 浏览器缓存分为两种：强缓存和协商缓存
> 
#### ***强缓存*** 
> 不需要请求服务器， 直接使用客户端本地缓存数据. 主要是根据Expires和Cache-Control来控制
> 
> Expires: 该时间是一个绝对时间, 其值直接是绝对时间
>
> Cache-Control: 该时间是一个相对时间, 值为：
> - max-age：即最大有效时间
> - no-cache：表示没有缓存，即告诉浏览器该资源并没有设置缓存
> - s-maxage：同max-age，但是仅用于共享缓存，如CDN缓存
> - public：多用户共享缓存，默认设置
> - private：不能够多用户共享，HTTP认证之后，字段会自动转换成private。
比如：Cache-Control: max-age=2592000
> 
> 缺点：如果上线的一个图片设置的强制缓存，图片更新了部署到服务器，但是用户那边始终读到的缓存里的原图，你没法要求用户清空浏览器缓存。
#### ***协商缓存*** 
> 向服务器发送请求，由服务器判断请求文件是否发生改变。若未发生改变，则返回304状态码，通知客户端直接使用本地缓存；如果发生改变，则直接返回请求文件。根据Last-Modified和If-Modified-Since、Etag和If-None-Match字段来控制
> 
> ***Last-Modified和If-Modified-Since：***
> 
> 服务器会将If-Modified-Since的值与Last-Modified字段进行对比，如果相等，则表示未修改，响应304；反之，则表示修改了，响应200状态码，返回数据。
>
> ***缺点：***
> 
> 如果资源更新的速度是秒以下单位，那么该缓存是不能被使用的，因为它的时间单位最低是秒。如果文件是通过服务器动态生成的，那么该方法的更新时间永远是生成的时间，尽管文件可能没有变化，所以起不到缓存的作用。
>
> ***Etag和If-None-Match：***
>
> 服务器存储着文件的Etag字段，可以在与每次客户端传送If-no-match的字段进行比较，如果相等，则表示未修改，响应304；反之，则表示已修改，响应200状态码，返回数据。

### 2. 数据传递中的跨域问题

### 3. jsBridge 通信原理

### 4. 实现 MVVM 的思路

### 5. vue 组件通信的方式

### 6. 预检请求触发的条件

### 7. css 伪类 (:before, :after) 是所有元素都可以使用吗


### 1. 给定一个数组, length 为 n 随机取, 尽可能的平均分为 m 个小数组, 最后整合成为一个二维数组.

### 2. 取链接的所有参数的 api 

### 3. 前端可以不可以操作cookie

### 4. 详细说一下 vue 的实现原理

### 5. 关于组件设计有什么看法

### 6. 埋点监控相关

### 7. 前端微服务? 微前端?

### 8. 性能优化

### 9. 最近在看什么书



### 1. html5 有哪些新的标签

### 2. 平常使用 canvas 是用来实现什么功能的

### 3. flex 布局

### 4. css3 的动画

### 5. position 的值都有哪些, 都什么作用

### 6. vuex

### 7. vue 组件通信

### 8. vue-router 的生命周期


### 1. vue 的生命周期, 以及作用

### 2. 详细介绍下 vue 的执行过程

### 3. new Vue() 后都做了什么

### 4. diff 算法

### 5. 为什么 会有 computed 和 methods 区别在哪 为什么用 computed 不用 methods

### 6. computed 需要传参 怎么办



### 1. loader 和 plugin 的执行顺序

### 2. vue 的响应式原理

### 3. 聊一聊缓存策略

### 4. vue diff 算法有哪些策略

### 5. vue 路由的实现方式, 分别使用哪些 API

### 6. history 模式下 会出现 404 是什么原因导致的

### 7. webpack 2 或者 3 跟 4 有哪些不同

### 8. call apply bind 有什么区别

### 9. obj.bind().bind().bind() 结果

### 10. obj.call().call().call() 结果

### 11. 浏览器的  event loop

### 12. 最近做的一个项目, 在里面承担什么样的职责

### 13. 为什么想要离职



### 1. 两栏布局

### 2. 浏览器加载阻塞

### 3. call apply bind

### 4. 浏览器缓存策略

### 5. cdn 缓存

### 6. 列表滚动加载多次后导致页面卡顿的解决方案

### 7. js 事件机制  事件委托

### 8. 浏览器event loop

### 9. var let const 区别  let 先使用后定义为什么会报错

### 10. 箭头函数和普通函数的区别 箭头函数内部可以使用 call apply 进行 this 指向的改变吗

### 11. 有哪些种类的 js 需要放在html 的 head 里面

### 12. css 文件的 prefetch 和 preload 的作用

### 13. 首页白屏可能出现的原因以及相对应的解决方案

### 14. rem 的实现原理

### 15. Vue 响应式原理  追问 computed 里面的结果怎么实现的响应式 

### 16. vuex 使用场景

### 17. xss 攻击如何防范, 前端如何判断页面被劫持

### 18. 最近做的最有成就感的项目, 以及项目中比较困难的逻辑实现


### 1. 给定一个数组 arr, 求出数组中第 k 大和第 m 大之和 需考虑数量

### 2. 将数组中相邻项按照一定条件合并

### 3. 实现一个 EatMan 
```javascript
 EatMan('Hank')  => Hi! This is Hank
 EatMan('Hank').eat('dinner')  => Hi! This is Hank  Eat dinner~
 EatMan('Hank').eat('dinner').eatFirst('lunch')  => Eat lunch~ Hi! This is Hank  Eat dinner~
``` 

### 4. event loop
> 浏览器: 宏任务 (setTimeout, setInterval 之类), 微任务(promise 之类), 宏任务 -> 微任务 -> 宏任务 -> 微任务
>
> node: timer -> pending -> idea -> poll -> check -> close; 顺序执行 超出或者一定时间到达下一个阶段
### 5. promise

### 6. 水平垂直居中
> 1. flex
> 2. margin
> 3. position + transform
> 4. text-align + line-height
### 7. flex 布局

### 8. webpack 原理

### 9. loader 和 plugin

## 记录

### 1. 实现一个一位数组转化为树形结构

### 2. promise 入参一个 promise 返回最后一个 promise 的执行结果

### 3. http 常见 code 
> 203: 
> 
> 301: 永久重定向; 302: 临时重定向; 304: 使用缓存
> 
> 401: 无权限; 403: 禁止访问
