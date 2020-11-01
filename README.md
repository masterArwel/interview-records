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
> 首先跨域 就是我们常说的违反浏览器同源策略的资源请求。
>
> ***什么是同源策略***
>
> 同源策略/SOP（Same origin policy）是一种约定，由Netscape公司1995年引入浏览器，它是浏览器最核心也最基本的安全功能，如果缺少了同源策略，浏览器很容易受到XSS、CSFR等攻击。所谓同源是指"协议+域名+端口"三者相同，即便两个不同的域名指向同一个ip地址，也非同源。
>
> **解决跨域的一些方案**
>
> - 通过jsonp
> 
> 原理：通过动态创建script，再请求一个带参网址实现跨域通信
> 
> 缺点：只能支持 get 请求的形式
> - document.domain + iframe
> 
> 原理：通过js强制设置document.domain为基础主域
>
> 缺点：1. 仅限主域相同，子域不同的跨域应用场景
> - location.hash + iframe
>
> 原理：不同域之前的相互通信，通过中间页来实现。 三个页面，不同域之间利用iframe的location.hash传值，相同域之间直接js访问来通信。
> - window.name + iframe
> 
> 原理：通过iframe的src属性由外域转向本地域，跨域数据由iframe的window.name从外域传递到本地域。
> - postMessage
> 
> postMessage是HTML5 XMLHttpRequest Level 2中的API, 使用方法在[MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/postMessage) 上有详细介绍
> - CORS
> 
> CORS 跨域有两种方式， 一种是需要cookie的一种是不需要的。不需要cookie 的方式，仅仅服务端的同学设置Access-Control-Allow-Origin即可，前端同学不需任何设置；带有cookie时，需要前端同学设置 ```xhr.withCredentials=true```
> - nginx代理
> 
> 通过nginx配置一个代理服务器（域名与domain1相同，端口不同）做跳板机，反向代理访问domain2接口，并且可以顺便修改cookie中domain信息，方便当前域cookie写入，实现跨域登录的问题
> - nodejs中间件代理
> 
> node中间件实现跨域代理，原理大致与nginx相同，都是通过启一个代理服务器，实现数据的转发。
> - WebSocket协议
> 
> [WebSocket protocol](https://developer.mozilla.org/zh-CN/docs/Web/API/WebSocket) 是HTML5一种新的协议。它实现了浏览器与服务器全双工通信，同时允许跨域通讯，是server push技术的一种很好的实现

### 3. jsBridge 通信原理
> 待完善
### 4. 实现 MVVM 的思路
> 这个问题一般在考察框架原理的时候经常被问到。回答一般以 发布-订阅者模式和脏值检查模式为切入点，详细聊一下 **1.修改View层，Model对应数据发生变化。2.Model数据变化，不需要查找DOM，直接更新View。** 的完整流程。
>
> 思路如下：
> 1. 实现一个数据监听器Observer，能够对数据对象的所有属性进行监听，如有变动可拿到最新值并通知订阅者
> 2. 实现一个指令解析器Compile，对每个元素节点的指令进行扫描和解析，根据指令模板替换数据，以及绑定相应的更新函数
> 3. 实现一个Watcher，作为连接Observer和Compile的桥梁，能够订阅并收到每个属性变动的通知，执行指令绑定的相应回调函数，从而更新视图
> 4. 实现入口函数，整合以上三者。
>
> ![原理图](/images/mvvm.png)
### 5. vue 组件通信的方式
>
> 1. props 和 $emit。使用场景最广泛，这里不做说明
> 2. $parent 和 $children。通过这两个api 可以让父组件访问到子组件的实例列表，也可以让子组件访问到父组件的实例，从而进行相互通信。使用这种通信方式的时候，不建议父组件与孙子或更深层次的组件进行通信
> 3. $attrs / $listeners。 ($listeners 在 vue3.x 已废弃)
> 4. provide / inject。在生命周期范围内， 父组件定义provide 后，可被子组件，子组件的子组件... 无限延展都可通过inject 来获取。
> 5. ref
> 6. EventBus
> 7. Vuex
> 8. localStorage / sessionStorage

### 6. 预检请求触发的条件
> OPTIONS请求即预检请求，可用于检测服务器允许的http方法。当发起跨域请求时，由于安全原因，触发一定条件时浏览器会在正式请求之前自动先发起OPTIONS请求，服务器若接受该跨域请求，浏览器才继续发起正式请求，以下三种情况下会触发预检请求
> 
> 1. 使用 PUT/DELETE/CONNECT/OPTIONS/TRACE/PATCH 中的一种请求方法
> 2. 请求头设置  除了 Accept/Accept-Language/Content-Language/Content-Type/DPR/Downlink/Save-Data/Viewport-Width/Width 这些字段之外的字段
> 3. Content-Type 的值不属于 application/x-www-form-urlencoded、multipart/form-data、text/plain
### 7. css 伪类 (:before, :after) 是所有元素都可以使用吗
> 待完善

### 9. 给定一个数组, length 为 n 随机取, 尽可能的平均分为 m 个小数组, 最后整合成为一个二维数组.
> 待完善
### 10. 取链接的所有参数的 api 
> window.location.search:  返回以 ? 开头的参数字符串
### 11. 前端可以不可以操作cookie
> 可以操作
> 1. 写入cookie：document.cookie = ‘key=value；expires=过期时间戳’；
> 2. 读取cookie：console.log(document.cookie);
> 3. 删除cookie：document.cookie = ‘key=value;expires=当前时间戳+1’；
### 12. 关于组件设计有什么看法
> 待完善
### 13. 埋点监控相关
> 待完善
### 14. 前端微服务? 微前端?
> 待完善
### 15. 性能优化
> ![性能优化](/images/optimize.png)


### 16. html5 有哪些新的标签
> canvas header footer session ...
### 17. 平常使用 canvas 是用来实现什么功能的
> 对于C 端来说 一般情况下都是用来画图， 也有有一些小游戏使用
### 18. flex 布局
> 待完善
### 19. css3 的动画
> 待完善
### 20. position 的值都有哪些, 都什么作用
> 1. relative: 生成相对定位的元素，相对于其正常位置进行定位
> 2. absolute: 生成绝对定bai位的元素，相对于 static 定位以外的第一个父元素进行定位
> 3. fixed: 生成绝对定位的元素，相对于浏览器窗口进行定位
> 4. static: 默认值。没有定位，元素出现在正常的流中
> 5. sticky: 元素根据正常文档流进行定位
### 21. vuex
> 待完善
### 22. vue-router 的生命周期
> 两种情况
> 1. 全局路由钩子：2个 (beforeEach、afterEach)
>
> 2. 组件路由钩子：3个 (beforeRouteEnter、beforeRouteUpdate、beforeRouteLeave)


### 23. vue 的生命周期, 以及作用
> 这里贴一张官网的地址 自行查看吧
> [Vue声明周期](https://cn.vuejs.org/v2/api/#%E9%80%89%E9%A1%B9-%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E9%92%A9%E5%AD%90)
### 25. new Vue() 后都做了什么
> 阅读源码位置 src/core/instance/init.js
>
> Vue 初始化主要就干了几件事情，合并配置，初始化生命周期，初始化事件中心，初始化渲染，初始化 data、props、computed、watcher 等等，最后初始化完成检测到如果有 el 属性，则调用 vm.$mount 方法挂载 vm，挂载的目标就是把模板渲染成最终的 DOM；此过程我们重点关注initState以及 最后执行的mount。
### 27. computed 需要传参 怎么办
> 利用闭包的方式：
>```javascript 
> computed: {
>   total(price){
>     return (price) => {
>       this.num * price
>     }
>   }
> }
>```

### 28. loader 和 plugin 的执行顺序
> 待完善
### 29. vue diff 算法有哪些策略
> 
### 30. vue 路由的实现方式, 分别使用哪些 API
> Vue路由有两种实现方式
> 1. hash 模式
> 
> 表现为在页面链接上会有 ```#``` 符号，路由改变页面更新是使用了 ```hashChange``` 事件, ```#``` 后面的内容改变不会刷新页面
>
> 2. history 模式
> 主要用到的api 是 pushState、replaceState 进行页面更新
### 31. history 模式下 会出现 404 是什么原因导致的
> 单页应用通常只有一个浏览器可以访问的index.html。但是如果使用history模式，用户直接访问或者刷新非index.html时，web服务器会绕过index.html，会去当前刷新的位置找相应的页面，这样就会导致404
### 32. webpack 2 或者 3 跟 4 有哪些不同
> 待完善
### 33. call apply bind 有什么区别
> 首先， 三种方式都是用来改变 this 的指向的，call 和apply 的原理是一样的，区别就是传参的不同， call 方法接收的参数为 （target, ...args）apply 是（target, [arg1,arg2,...]）;bind 方法的实现原理是基于call 返回了一个函数
### 34. obj.bind().bind().bind() 结果
> 只有第一个bind 生效
### 35. obj.call().call().call() 结果
> 最后一个生效


### 36. 两栏布局
> 待完善
### 37. 浏览器加载阻塞
> 待完善
### 38. cdn 缓存
> 待完善
### 39. 列表滚动加载多次后导致页面卡顿的解决方案
> 待完善
### 40. js 事件机制  事件委托
> 待完善
### 41. var let const 区别  let 先使用后定义为什么会报错
> 待完善
### 42. 箭头函数和普通函数的区别 箭头函数内部可以使用 call apply 进行 this 指向的改变吗
> 待完善
### 43. 有哪些种类的 js 需要放在html 的 head 里面
> 待完善
### 44. css 文件的 prefetch 和 preload 的作用
> 待完善
### 45. 首页白屏可能出现的原因以及相对应的解决方案
> 待完善
### 46. rem 的实现原理
> 待完善
### 47. Vue 响应式原理  追问 computed 里面的结果怎么实现的响应式 
> 待完善
### 48. xss 攻击如何防范, 前端如何判断页面被劫持
> 待完善

### 49. 给定一个数组 arr, 求出数组中第 k 大和第 m 大之和 需考虑数量
> 待完善
### 50. 将数组中相邻项按照一定条件合并

### 51. 实现一个 EatMan 
```javascript
 EatMan('Hank')  => Hi! This is Hank
 EatMan('Hank').eat('dinner')  => Hi! This is Hank  Eat dinner~
 EatMan('Hank').eat('dinner').eatFirst('lunch')  => Eat lunch~ Hi! This is Hank  Eat dinner~
``` 
> 待完善

### 52. event loop
> 浏览器: 宏任务 (setTimeout, setInterval 之类), 微任务(promise 之类), 宏任务 -> 微任务 -> 宏任务 -> 微任务
>
> node: timer -> pending -> idea -> poll -> check -> close; 顺序执行 超出或者一定时间到达下一个阶段
### 53. promise
> 待完善
### 54. 水平垂直居中
> 1. flex
> 2. margin
> 3. position + transform
> 4. text-align + line-height
### 55. webpack 原理
> 待完善
### 56. loader 和 plugin
> loader: 主要是对文件的转换 比如 .less => .css
>
> plugin: 主要是在webpack 打包过程中每个阶段暴露出的回调补充处理方式

### 57. 实现一个一位数组转化为树形结构
> ```javascript
>function toTree(data) {
>   let result = []
>   if(!Array.isArray(data)) {
>      return result
>   }
>    data.forEach(item => {
>       delete item.children;
>     });
>     let map = {};
>     data.forEach(item => {
>        map[item.id] = item;
>      });
 >     data.forEach(item => {
>         let parent = map[item.pid];
>         if(parent) {
>            (parent.children || (parent.children = [])).push(item);
>          } else {
>            result.push(item);
>           }
>       });
>      return result;
> }
>```
### 58. promise 入参一个 promise 返回最后一个 promise 的执行结果
> 待完善
### 59. http 常见 code 
> 203: 
> 
> 301: 永久重定向; 302: 临时重定向; 304: 使用缓存
> 
> 401: 无权限; 403: 禁止访问

### 60. 实现深拷贝
> 深拷贝的方式有很多。这里实现以下最容易理解的一种，就是利用递归
>
> ```javascript
> // 定义一个深拷贝函数  接收目标target参数
> function deepClone(target) {
>     // 定义一个变量
>     let result;
>    // 如果当前需要深拷贝的是一个对象的话
>    if (typeof target === 'object') {
>    // 如果是一个数组的话
>        if (Array.isArray(target)) {
>            result = []; // 将result赋值为一个数组，并且执行遍历
>            for (let i in target) {
>                // 递归克隆数组中的每一项
>                result.push(deepClone(target[i]))
>            }
>         // 判断如果当前的值是null的话；直接赋值为null
>        } else if(target===null) {
>            result = null;
>         // 判断如果当前的值是一个RegExp对象的话，直接赋值    
>        } else if(target.constructor===RegExp){
>            result = target;
>        }else {
>         // 否则是普通对象，直接for in循环，递归赋值对象的所有值
>            result = {};
>            for (let i in target) {
>                result[i] = deepClone(target[i]);
>            }
>        }
>     // 如果不是对象的话，就是基本数据类型，那么直接赋值
>    } else {
>        result = target;
>    }
>     // 返回最终结果
>    return result;
>}
> ```

61.  页面主要内容也是canvas 由于数据量大  准备一个 canvas的loading， 主内容渲染时 会导致loading canvas 卡死， 为什么 怎么解决

62. 实现灰度部署的原理

