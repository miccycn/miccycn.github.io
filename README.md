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
- display的可选项：fullscreen（部分支持）、standalone、minimal-ui（不支持）、browser（默认），只有fullscreen和standalone能弹出安装横幅；

## 开发阶段如何多机型调试 PWA

## webpack 如何配合 cache Storage 缓存策略
