# 就随性记一下 PWA 的笔记……

[TOC]
## 移动web的痛点（开发体验好）

* 严重依赖网络环境
* 弱体验：不像原生应用那么流畅
* 弱功能：功能/API 受限严重，严重依赖浏览器
* // 无发行渠道、无变现模式、无完整生态环境

## 原生应用的痛点（用户体验好）
* 占用的资源大（安装包、内存、临时存储文件）
* 版本变更和项目维护困难，流程复杂
* 跨平台成本高

## Web App 的擅长与不擅长

* 擅长：
  - 资讯类（博客、新闻、）
  - 社交类
  - 电商类

* 不擅长：
  - 需要复杂计算与生成能力的应用（如图像视频处理，大型游戏）
  - 需要底层系统服务的领域
  - 需要后台运行的程序

## 什么是Progressive Web Apps
  
  Progressive Web Apps 不是一个新框架新架构，也不是特指的某一个新概念，而是通过一系列的经过改进后基于web的技术，使 Web App 在安全、性能和体验等方面得到提升，让用户能获得更接近于 Native App 的体验。比如说，网站使用了 HTTPS 就已经算是符合 PWA 的一项特征了。
  PWA 的这些特征可以通过一个 Progressive Web App Checklist（https://developers.google.cn/web/progressive-web-apps/checklist）来检查一个网站是否符合 PWA 的各项特征，这些特征可能会随着技术发展不断改进和增加，可以这么说，只要是渐进增强地去改进 Web App 体验的技术都算是 PWA 的一部分。

### 从用户体验的角度：
* 可靠：资源立刻加载，永远不会出现小恐龙
* 快速：快速响应用户行为、流畅的动画和切换
* 沉浸感：就像一个正经的应用程序，让人难辨真假

### 优点：
* 渐进式：渐进增强，非侵入式新技术，不需要对现有项目代码做多少更改
* App Shell：可以更进一步接近 Native App 的体验
* 离线功能：解决之前 Web App 严重依赖网络环境的痛点
* 持续更新：不需要发版本就可以更新，直接就可以让用户获得最新版
* 内容可被搜索引擎索引：Native App 就做不到这点
* 粘性：通过推送离线通知等，可以让用户回流
* 无需下载就可安装：用户可以添加常用的 Web App 到桌面，免去去应用商店下载的麻烦
* 占用的存储空间小

### 微博相关数据：



### PWA 技术角度的组成：
* 支持 PWA 的浏览器、HTTPS -- 办公大楼
* Manifest.json -- 企业 VI
* Cache Storage -- 后勤仓库
* Service Worker -- 公司员工
* 其他

#### 支持 PWA 的浏览器（定位与受众）
* 安卓5.0 + Chrome
* iOS Safari 确定在下一版本中支持 PWA


#### Manifest.json -- 企业 VI
（https://developer.mozilla.org/en-US/docs/Web/Manifest）

* icons: [{
          src: 'https://h5.sinaimg.cn/upload/696/2017/08/23/weibologo.png',
          sizes: '606x606',
          type: 'image/png'
      }]
* background_color: 启动画面的背景色
* start_url: 首屏页面的相对路径。在官方的最佳实践里建议CacheStorage缓存首屏页面，这样就永远不会出现 web app 一打开就白页面的情况。
* theme_color: app “外壳”的颜色，比如“最近任务界面”里的tab栏颜色；
* short_name: 显示在主屏幕的APP名称，状态栏名称；
* name: “添加到主屏幕”的安装横幅上显示的名称，启动画面的上显示的名称；
* display: Web App显示的形态（只有fullscreen和standalone能弹出安装横幅）：
      - fullscreen（部分支持）
      - standalone （官方推荐，仿Native App的体验）
      - minimal-ui （理论上是 browser 的阉割版，但我的几个手机表现出来的都和 browser 没区别，不知道是不是我打开的方式不对？）
      - browser（默认，同直接用浏览器打开）
* orientation：屏幕方向，安卓系统会忽略系统的屏幕方向锁（截至目前 Chrome 63 仍然有这个问题）：
      - landscape-primary 逆时针90
      - landscape-secondary 顺时针90
      - landscape = landscape-primary + landscape-secondary 横屏
      - portrait-primary 0
      - portrait-secondary 180度
      - portrait = portrait-primary + portrait-secondary 竖屏
      - natural = landscape-primary + portrait-primary
      - any = landscape + portrait 任意方向
* prefer_related_applications: 和 Native App 相关联
* related_applications: 和 Native App 相关联

> icons 的 sizes值和原始尺寸小于 48dp 的图片都会被忽略。并且在 icons 数组里面使用的图片采取最接近原则；（ dp * window.devicePixelRatio 即为实际 px 大小）
    桌面上的 icon：48dp；
    启动画面上的 logo：128dp；
    启动画面里的文字位置与 logo 图片的原始尺寸有关（和 sizes 字段无关），原始尺寸的图片宽度小于等于 80dp 时紧挨logo，否则文字置底；
    所以虽然启动画面不可以定制，但中央的logo与添加到桌面上的icon是可以长得不一样的。两者不一样带来的问题是不容易做不同分辨率下的兼容。

#### Cache Storage -- 后勤仓库
##### 再谈 Web 缓存
（《缓存的最佳实践》https://jakearchibald.com/2016/caching-best-practices/ 
《设计一个无懈可击的浏览器缓存方案》 https://zhuanlan.zhihu.com/p/28113197）

* 源服务器 CDN
* 代理服务器缓存
* HTTP缓存（强缓存、协商缓存）
* Memory Cache & Disk Cache (from Chrome)
* ++Cache Storage++
* 应用程序的花式缓存

    良好的缓存功能和缓存策略，不仅可以加快用户的访问速度，也给开发者提供了应用程序的离线能力。

##### 被废弃了的 Application Cache
（《Application Cache是个大坑》http://alistapart.com/article/application-cache-is-a-douchebag）
* 不提供删除隐式缓存元素的方法
* 更新策略难以控制，多数情况下是缓存优先
* 内置的缓存更新方法简单粗暴，不开放细化的缓存更新和 fallback 方法
* 无视重定向
* 需要后端配合开发和维护
      
##### 我们需要的离线能力
* 在线的情况下显示最新的数据（Network-first）
* 允许开发者控制哪些内容被缓存，什么时候被缓存以及怎样被缓存
* 允许我们提供给用户一些控制的能力，有可能以“保存为离线”或者“稍后阅读”按钮
* 每次对页面的访问必须提供给浏览器供离线显示需要的所有内容（Stale-while-revalidate）

##### Cache Storage 是什么？
* k-v：k 是一个 Request 对象，v 是 Respond 对象，Cache Storage是专门用于缓存 Request-Respond的对象
* window 作用域和 worker 作用域都可以访问（解决【“保存为离线”或者“稍后阅读”按钮】的需求）
* 开放针对每个 Request-Respond 的具体操作
* 增和删、在一定程度上的改和查
* 开发者可以分析每一个 Request 里的信息（比如 url、header），来决定是去 CacheStorage 里拿还是去询问服务器
* Request-Respond 默认不会更新、不会过期，除非开发者显式地去处理它们
* 当浏览器不支持 CacheStorage，甚至 worker 程序有误（比如 worker 中某些 ES6 的 API 不被支持）导致 Service Worker 注册失败时，不影响主线程的工作


#### Service Worker -- 公司员工

    Service Worker 的设计基于 Web Worker，他们执行的机制都是新开一个线程去处理一些额外的、以前不能直接处理的任务。SW 是通过与事件相关联，而不是靠文件来启动和维持的。这种设计总结了以往 Application Cache 的失败教训与 Shared Worker 的成功经验，必须让后台处理上下文的执行有限制，既要节约资源，又要确保后台上下文丢失后能重启。对于 Web Worker，我们可以使用它来进行复杂的计算，因为它并不阻塞浏览器主线程的渲染。而 Service Worker ，我们可以用它来进行本地缓存或请求转发，相当于一个浏览器端本地的代理。

* 基于 Shared Web Worker 设计
* 与事件相关联，不靠文件
* 安全性，仅信任 HTTPS 或 localhost
* 有 Scoped 作用域，仅对 Scoped 下的页面地址生效
* 纯 Promise 异步，不支持同步的特性API（例如 XHR、localStorage、Cookie。而 IndexedDB 的操作是异步的，所以可以使用）
* 相当于一个浏览器端本地的代理
    
##### 写一个简单的 PWA = manifest.json + Service Worker + CacheStorage
* 让网站支持 HTTPS
* 写一个manifest.json，放到 html 上部分
  ```<link rel="manifest" href="/pwa/manifest.json">```
* 写一个注册 service-worker 的脚本，放到  html 下部分，作用是在页面里注册 Service Worker
```
<script>
  if (navigator.serviceWorker) {
    navigator.serviceWorker.register('sw.js')
    .then(function() {
      console.log('注册成功 ');
    });
  }
</script>
```
* 写一个 ServiceWorker.js 放到一个目录（比如说网站的根路径，这个路径就是 scope）
* 测试各种浏览器的兼容性
* 上线

    
###### 基本缓存策略
《离线指南》：https://developers.google.com/web/fundamentals/instant-and-offline/offline-cookbook/
《PWA之Workbox缓存策略分析》：https://segmentfault.com/a/1190000012326428

- cacheFirst（最常用）
- cacheOnly
- networkFirst
- networkOnly
- staleWhileRevalidate（== fastest）

  1. cacheFirst：如果请求的资源与CacheStorage中的任何资源均不匹配，则从网络中获取，将其发送到页面同时添加到CacheStorage中

```
self.addEventListener('fetch', function(event) {
  event.respondWith(
    caches.open('myCacheName').then(function(cache) {
      return cache.match(event.request).then(function (response) {
        return response || fetch(event.request).then(function(response) {
          cache.put(event.request, response.clone());
          return response;
        });
      });
    })
  );
});
```

 2. networkFirst：请求会立即发出，成功的话就返回结果添加到 CacheStorage 中，可以设置超时时间，如果失败或网络超时时则返回 CacheStorage。

  略。
会生成一个 setTimeout 后被 resolve 的 CacheStorage 调用，再把它和请求放倒一个 Promise.race 中，那么请求超时后就会返回 CacheStorage。

 3.  cacheOnly：仅使用CacheStorage，若CacheStorage中没有就响应失败
```
self.addEventListener('fetch', function(event) {
  event.respondWith(
    caches.open('myCacheName').then(function(cache) {
      return cache.match(event.request);
    })
  );
});
```

 4. networkOnly：仅请求网络，完全没 CacheStorage 什么事了
```
self.addEventListener('fetch', function(event) {
  event.respondWith(
    fetch(event.request)
  );
});
```

 5. staleWhileRevalidate：优先返回 CacheStorage 中的资源，但无论 CacheStorage 中是否有缓存，它都会发出请求，请求的结果会用来更新 CacheStorage。
```
self.addEventListener('fetch', function(event) {
  event.respondWith(
    caches.open('myCacheName').then(function(cache) {
      return cache.match(event.request).then(function(response) {
        var fetchPromise = fetch(event.request).then(function(networkResponse) {
          cache.put(event.request, networkResponse.clone());
          return networkResponse;
        })
        return response || fetchPromise;
      })
    })
  );
});
```
###### 我们使用的缓存策略
* cacheFirst：webpack 打包出来的 bundles、不常更新的静态资源（例如表情图片、icon、基础样式）
* staleWhileRevalidate：页面（APP shell）
* networkFirst：与个人信息相关性较低的展示型数据API
* networkOnly：其他
* cacheOnly: 不敢用

###### 错误的缓存策略带来的后果
* 页面代码里的 bug 不能及时修复
* 用户可能无法更新页面，而开发者对此不易察觉
* 可能导致所有页面无法工作

> 注意事项
SW.js 本身也会受 CacheStorage 和  HTTP 缓存的影响，当浏览器未检测到 Service Worker 发生变化时，甚至连安装都不会被触发，所以不要对 SW.js 文件本身进行 CacheStorage 缓存，它的 HTTP 缓存过期时间也要尽可能短（max-age: 0）。

#### 生命周期
- install
- activate
- redundant
      
#### 事件
- 与生命周期有关
  - install（安装时触发，一个新的只会触发一次）
  - activate（安装完成后激活时触发，一个新的只会触发一次）
- fetch （发起请求时触发，可用于拦截请求和响应）
- push （推送。当订阅了推送服务后，可以使用推送方式唤醒 Service Worker 以响应来自系统消息传递服务的消息，即使用户已经关闭了页面。）
- notificationclick（推送时显示的卡片被点击后触发）
- sync （后台同步。注册后台同步方法后会在网络情况由坏变好的时候触发。）
- message（消息。worker特有的message通信方法。）
- 与错误监控有关
	- error（错误）
	- unhandledrejection（捕获Promise错误：未被处理的Promise错误）
	- rejectionhandled（捕获Promise错误：最初未被处理，但是稍后又得到了处理的Promise错误）

#### 对象
> 在 Worker 中常用的对象比较少，Worker无法访问 DOM ，也无法使用 window、document 对象和基于它们的方法。

- self：worker 子线程自身，指向全局 worker 对象
- clients：可以简单地认为是该 worker 能控制得到的范围的对象
- cache：CacheStorage
- navgator：基本同 window.navgator
- location：基本同 window.location
- setTimeout 和 setInterval
- importScripts：基本同 import
- indexedDB：可以用来管理 CacheStorage 里的缓存过期时间等

#### sw-precache 、sw-toolbox 和 workbox

sw-toolbox
```
toolbox.router.get(
  /^https?:\/\/h5\.sinaimg\.cn\/marvel\/(.*)\.(js|css)$/, 
  toolbox.cacheFirst
);
```
workbox
```
workboxSW.router.registerRoute('/schedule', workboxSW.strategies.cacheFirst());
```
sw-toolbox（针对动态、运行时请求的离线缓存的工具）
sw-precache（针对静态资源、App Shell 的离线预缓存的工具）
workbox ≈ sw-toolbox + sw-precache

写一个简单的 PWA
* 让网站支持 HTTPS
* 引入一个SW工具库（workbox、sw-toolbox、sw-precache……）
* 在 webpack 等构建工具里修改自己需要的个性化配置
* 测试各种浏览器的兼容性
* 上线

## Progressive Web !Sites
- 更倾向于用 App 的思路去封装和设计整个站点
  - 弱化 URL 概念，更倾向于用“模块”去区分不同功能的页面
  - 更强调数据和表现层分离，明确区分页面 Shell 和动态内容（App Shell）
  - 如果有一个 URL 对应了服务端渲染的多“种”页面的实现（类似 SSR），则需要慎重考虑对页面的缓存策略


## 更新 CacheStorage 时机的思考
  在官方和很多基础指南的里面，都是推荐在 Worker 里面写入每个 bundles 的信息，例如在 sw-precache 的帮助下，在 SW.js 的最前面输出文件路径以及文件对应的 MD5：

```
const cacheList = [
  {
    'url': 'main.js',
    'revision': '0e438282dc400829497725a6931f66e3'
  },
  {
    'url': 'main.css',
    'revision': '02ba19bb320adb687e08dded3e71408d'
  }
];
```
当在生命周期的 install 通过 cache.addAll 把所有的当前版本的文件添加到 CacheStorage。在后续的更新迭代中，SW.js 中的 MD5 因为产生了变化，浏览器认为这是一个新的 SW.js，所以会重新走一遍生命周期，把旧的 Bundles 批量删除，缓存新的 Bundles。相当于 Native APP 的安装更新。

我们是否有必要把完整的“安装包”（所有 bundles）都缓存起来，保证后续访问的所有模块都能秒开？
如果是，我们做为 web app 的优势就是快速迭代快速发版，每个小更新都需要让用户重新安装一个新的 SW.js，甚至把新 bundle 下载一遍？
如果不，我们怎么确定哪些 bundle 在何时应该被缓存？

是否存在这样一种理想情况——只要 SW.js 的代码逻辑不更新，它就可以一直工作，哪怕我们发了新版本，也能按需替换旧版本的缓存，不需要用户安装一个新的 SW.js。

* 在 fetch 事件里操作缓存，install 与 activate 事件里不做关于缓存的操作
* 稳定的模块指定 WebpackChunkName，使用文件名中的 MD5 比较
* 基础 bundles （例如 vendor.js、manifest.js 等）在 ```<head>``` 中加 preload
```<link rel="preload" href="//h5.sinaimg.cn/m/weibo-lite/js/vendor.337a6d7a.js" as="script">
  <link rel="preload" href="//h5.sinaimg.cn/m/weibo-lite/css/app.9a344f76.css" as="style">```
* 使用响应头中的 last-modified 或 e-tag 作比对（服务端需开启CORS，即响应头中有 Access-Control-Allow-Origin）

在 install 与 activate 事件里不做关于缓存的操作，每次在 Fetch 事件里做更新判断，这里的 SW.js 无法明确知道哪些包需要预缓存，所以对于第一次访问的用户来说没有预加载 prefetch 的作用。只有用户需要到这个 bundle 时，会判断更新删除旧版本缓存新版本，所以如果用户时间不访问某个模块也会存在有的 bundle 一直不更新的情况，但是不会影响访问，因为增量更新，文件名中的MD5会改变，所以旧版本永远不会与新版本冲突。

> （《学习记录 Fetch API mode 跨域模式》http://renxianglei.com/blog/2017-07-13-fetch-cors）
> 请求模式：
* same-origin：仅同域。如果一个请求是跨域的，就返回一个错误，这个模式可以确保所有的请求都是同域的
* cors（常用）：同域和跨域。通常用作跨域请求来从第三方提供的API获取数据。只有有限的 一些 headers 被暴露给Response对象，但是body是可读的。是 Request 对象的默认请求模式。
* no-cors（常用）：仅跨域。允许来自CDN的脚本、其他域的图片和其他一些跨域资源。是Fetch事件的默认请求模式。
  - 请求的 method 只能是 HEAD，GET 或者 POST。
  - 任何 ServiceWorkers 拦截了这些请求，都不能随意添加或者改写任何 headers，
  - JavaScript 不能访问 Response 中的任何属性，这保证了 Service Workers 不会导致任何跨域下的安全问题而隐私信息泄漏。
* 其他

> 响应类型：
* basic：同域的请求，包含所有的 headers（除了 Set-Cookie 和Set-Cookie2）。 
* cors： Response 从一个合法的跨域请求获得，响应头中有 Access-Control-Allow-Origin，一部分 header 可读。
* opaque：Response 从 no-cors 请求了跨域资源，我们不能读取返回的数据，也不能查看请求的状态码，这就是意味着我们将无法确定请求是成功了还是失败了。依靠Server端来做限制。
* 其他



## 离线情况行为日志的收集

* Background-Sync
* 借助本地存储来记录
* 接口批量接收日志
* 离线日志数据量太大时的抽样

## PWA 的调试
PWA 的调试依旧是借助 Chrome Dev Tool，但是由于 Service Worker 的安全性要求，只有 HTTPS 和 localhost 时可以生效，所以对于真机调试会比较麻烦。

* HTTP缓存过期时间尽可能短
* 调试阶段 SW 安装了以后直接 skipWaiting 激活
* 先在 localhost 上进行代码逻辑与规范的调试
* 真机调试使用 HTTPS：安装 SSL 证书 + 代理工具:8888 + chrome://inspect

## Service Worker in 原生 Webview
* UserAgent 失效：经过 SW 的请求无法使用客户端自定义的 UserAgent
* cookie 丢失：客户端注入的 cookie 经过 SW 的拦截后 cookie 丢失
* 客户端无法拦截请求：常规方法（shouldInterceptRequest）拦截不到经过 SW 的请求，需要安卓7.0及以上的系统api（ServiceWorkerController）对经过 SW 的请求另外进行拦截
* HTTPS调试：安卓 7.0 及以后 webview 的网络代理需要把 SSL 证书打包到客户端中，增加了真机webview调试的复杂度

目前使用UC内核的 webview 可以解决前两个bug

## 预留降级方法
* 通过 App Shell 中的某个变量控制（页面的缓存策略是 NetworkFirst）
* 页面中访问一个服务端接口控制SW 的注册和注销时机


## 商业化

