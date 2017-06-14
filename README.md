# 就随性记一下 PWA 的笔记……

## Manifest.json
- icons 里面 sizes值和原始尺寸小于 48dp 的图片都会被忽略。并且在 icons 里面使用的图片采取最接近原则；（ dp * window.devicePixelRatio 即为实际 px 大小）
- 桌面上的 icon：48dp；
- 启动画面上的 logo：128dp；
- 启动画面里的文字位置与 logo 图片的原始尺寸有关（和 sizes 字段无关），原始尺寸的 width 小于等于 80dp 时紧挨logo，否则文字置底；
- background_color定义的是APP启动画面的背景色；
- theme_color定义的是 app “外壳”的颜色，比如“最近任务界面”里的tab栏颜色；
- short_name：显示在主屏幕的APP名称，状态栏名称；
- name：“添加到主屏幕”的安装横幅上显示的名称，启动画面的上显示的名称；
- display的可选项：fullscreen（部分支持）、standalone、minimal-ui（不支持，或者是我打开的方式不对？）、browser（默认）；只有fullscreen和standalone能弹出安装横幅；

## 开发阶段如何多终端调试 PWA
这个应该是刚开始开发 PWA 都比较纠结的一个问题了。
因为 PWA 必须在 https 或 localhost 下才能正常运行，那对于在除本机之外的其他终端上调试就没办法 localhost 了。
- 所以只能走 https：
https 调试起来就麻烦多了。如果是自己本地临时生成的 ssl 证书还需要手动添加证书之类的，证书这个问题在多终端调试上就显得尤其麻烦。
- 正经申请一个 ssl 证书：
略
- 用别人的证书：
github pages：简单粗暴，这也是谷歌开发者教程上推荐的方法之一；缺点是你在调试阶段就要把项目开源出去，比较适合个人的野路子项目；
ngrok：ngrok 会生成一个临时的 ngrok.io 的二级域名，在 https 协议下也能访问，能把二级域名转发到本地 localhost 的其中一个端口，甚至公网上也能访问你本机；缺点是 ngrok 速度比较慢，而且会在你的调试页面里面植入广告= =；
以上，都会带来跨域问题。
- 跨域问题的常见解决方法：
略

## webpack 如何配合 cache Storage 缓存策略
