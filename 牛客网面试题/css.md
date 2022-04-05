1. css flex 的各个属性值
   
2. background
背景颜色  背景图片地址 背景平铺  背景图像滚动 背景图片位置


3. css 动画 animation 各个时间值含义；
animation-name
animation-duration
animation-timing-function
animation-delay
animation-iteration-count
animation-direction
animation-fill-mode
4. css 如何实现让一个元素旋转并横向移动，只用一个 css 属性
```css
transform: translateX(1000px) rotate(360deg);
```

5.  解决 img 图片自带边距的问题
6.  介绍一下你对浏览器内核的理解
7.  CSS3有哪些新特性？
    1. CSS3实现圆角（border-radius），阴影（box-shadow），
    2. 对文字加特效（text-shadow、），线性渐变（gradient），旋转（transform）
    3. transform:rotate(9deg) scale(0.85,0.90) translate(0px,-30px) skew(-9deg,0deg);// 旋转,缩放,定位,倾斜
    4. 增加了更多的CSS选择器 多背景 rgba
    5. 在CSS3中唯一引入的伪元素是 ::selection.
    6. 媒体查询，多栏布局
    7. border-image

8.  css 中可以让文字在垂直和水平方向上重叠的两个属性是什么？
　　垂直方向：line-height
　　水平方向：letter-spacing (letter-spacing 属性增加或减少字符间的空白)
　　那么问题来了，关于letter-spacing的妙用知道有哪些么？
　　答案:可以用于消除inline-block元素间的换行符空格间隙问题。

9. rgba() 和 opacity 的透明效果有什么不同？
rgba()和opacity都能实现透明效果，但最大的不同是opacity作用于元素，以及元素内的所有内容的透明度，
　　而rgba()只作用于元素的颜色或其背景色。（设置rgba透明的元素的子元素不会继承透明效果！）

10. 什么是外边距重叠？重叠的结果是什么？
11. CSS 伪类和伪元素的区别
12. 什么是响应式设计？响应式设计的基本原理是什么？如何兼容低版本的IE？

响应式网站设计(Responsive Web design)是一个网站能够兼容多个终端，而不是为每一个终端做一个特定的版本。

基本原理是通过媒体查询检测不同的设备屏幕尺寸做处理。

页面头部必须有meta声明的viewport。

13. 说一下 css 盒模型，border-box 和 content-box 区别
14. 说说 BFC
15. 移动端响应式布局怎么实现的；
16. 说一说 flex 布局，有了解 Grid 么
17. 有兼容 retina 屏幕的经历吗？如何在移动端实现 1 px 的线;
18. grid布局