## 移动端

- touchstart、touchmove、touchend

- 事件对象

```javascript
oDiv.addEventListener('touchstart', function(e) {
    // e.touches          // 当前屏幕上的手指信息列表
    // e.targetTouches[0].pageY // 触发当前元素的手指信息列表
    // e.changedTouches   // 手指信息状态发生变化时的信息列表
});
```

- 拖拽 div

```javascript



var oDiv = document.querySelector('div');
var startX = 0;
var startY = 0;
var startPageX = 0;
var startPageY = 0;
oDiv.ontouchstart = function(e) {
    // div 到屏幕左侧的距离
    startX = this.offsetLeft;
    startY = this.offsetTop;
    // 手指到屏幕左侧的距离
    startPageX = e.targetTouches[0].pageX;
    startPageY = e.targetTouches[0].pageY;
};

oDiv.ontouchmove = function(e) {
    // moveX 是移动的距离
    var moveX = e.targetTouches[0].pageX - startPageX;
    var moveY = e.targetTouches[0].pageY - startPageY;
    // 用按下时记录的div 到屏幕左侧的距离 加上 移动的距离
    this.style.left = startX + moveX + 'px';
    this.style.top = startY + moveY + 'px';

    return false;
};
```

```javascript
var oDiv = document.querySelector('div');
var disX = 0;
var disY = 0;
oDiv.ontouchstart = function(e) {
    // 记录手指到盒子的距离
    disX = e.targetTouches[0].pageX - this.offsetLeft;
    disY = e.targetTouches[0].pageY - this.offsetTop
};

oDiv.ontouchmove = function(e) {
    // 用当前手指距离屏幕的距离减去 按下时记录的手指到盒子的距离
    this.style.left = e.targetTouches[0].pageX - disX + 'px';
    this.style.top = e.targetTouches[0].pageY - disY + 'px';

    return false;
};
```

## 关于插件

- fastclick.js，解决移动端300ms延迟的现象
- swiper.js，轮播
- touchSlide.js
- zy.media.js，视频播放插件

### 插件的使用步骤

- 引入公共的 CSS 文件      swiper.min.css
- 引入针对当前效果的 CSS
- 引入 HTML 结构
- 引入公共的 JS 文件       swiper.min.js
- 引入针对当前效果的 JS
- 把 CSS（覆盖源代码） 和 JS（修改参数） 修改成自己想要的

## 移动端轮播图

0. 让 ul 自动播放，在定期器里面让 index++，然后 index * 一张图片的宽度
1. 判断 index 临界值，注意在 transitionend 里面去判断
3. 拖动 ul 位移
4. touchend 时让 ul 自动移动到对应的位置
5. 优化

## classList 掌握
