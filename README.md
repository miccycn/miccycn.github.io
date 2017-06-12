# 就随性记一下 PWA 的笔记……

## Manifest.json
- icons 里面 sizes值和原始尺寸小于 48dp 的图片都会被忽略。并且在 icons 里面使用的图片采取的原则为取最接近原则；（ N dp * window.devicePixelRatio 即为实际 px 大小）
- 桌面上的 icon：48dp；
- 启动画面上的 logo：128dp；
- background_color定义的是APP启动画面的背景色；
- theme_color定义的是 app “外壳”的颜色，比如“最近任务界面”里的tab栏颜色；
- short_name：主屏幕APP名称，状态栏名称；
- name：“添加到主屏幕”的横幅上显示的名称，启动画面的上显示的名称；
- 启动画面里的 icon 与文字的距离与 icon 图片的原始尺寸有关（和 sizes 字段无关），原始尺寸越大距离越大；
- display的可选项：fullscreen、standalone、minimal-ui、browser
