## 节点操作

- 创建节点

```javascript
var oLi = document.createElement('li');
```

- 添加节点

```javascript
// 添加到 ul 内容的最后
oUl.appendChild(oLi);
// 添加到 ul 内容的最前
oUl.insertBefore(oLi, oUl.children[0])
```

- 删除节点

```javascript
oUl.removeChild(oLi);

oLi.remove(); // “自杀”，和上面的效果一样
```

- 克隆节点

```javascript
// 克隆 li 节点及其里面的子节点
oLi.cloneNode(true);
```

## 创建元素的三种方式

- document.write('<li></li>'); 了解

- innerHTML

```javascript
// 性能太低，不能这么写
console.time('耗时');
for(var i = 0; i < 1000; i ++) {
    document.body.innerHTML += '<div>'+i+'</div>';
}    
console.timeEnd('耗时');
```

```javascript
// 解决方案1
console.time('耗时');
var str = '';
for(var i = 0; i < 1000; i ++) {
    str += '<div>'+i+'</div>';
}   
document.body.innerHTML = str;
console.timeEnd('耗时');
```

```javascript
// 解决方案2
console.time('耗时');
var arr = [];
for(var i = 0; i < 1000; i ++) {
    arr.push('<div>'+i+'</div>');
}   
document.body.innerHTML = arr.join('');
console.timeEnd('耗时');
```

- document.createElement()

```javascript
// 这种方法很灵活，很方便的能给新创建的 div 添加样式、绑定事件...
console.time('耗时');
var arr = [];
for(var i = 0; i < 1000; i ++) {
    var oDiv = document.createElement('div');
    oDiv.innerHTML = i;
    document.body.appendChild(oDiv);
}   
console.timeEnd('耗时');
```

```javascript
console.time('耗时');
// “箱子”，文档碎片
var oFrag = document.createDocumentFragment();

for(var i = 0; i < 100000; i ++) {
    var oDiv = document.createElement('div');
    oDiv.innerHTML = i;
    // 先往“箱子”里面装
    oFrag.appendChild(oDiv);
}   
// 最终把“箱子”放到页面上面
document.body.appendChild(oFrag);
console.timeEnd('耗时');
```

## 事件高级

### 注册事件的2中方式

- on 的形式，如果给同一元素绑定同一事件执行不同的事件处理程序会发生覆盖的现象

```javascript
// 删除事件
oDiv.onclick = function() {
    console.log(1);
    oDiv.onclick = null;
};
```
- addEventListener

```javascript
// 删除事件
function fn() {}
oDiv.addEventListener('click', fn);
oDiv.removeEventListener('click', fn);
```

- 注册事件的兼容性处理，了解

```javascript
function addEventer2(ele, type, callback) {
    if(ele.addEventListener) {
        ele.addEventListener(type, callback);
    } else if(ele.attachEvent) {
        ele.attachEvent('on'+type, callback);
    } else {
        ele['on'+type] = callback
    }
}
```

### DOM事件流的3个阶段

- 事件捕获、目标阶段、事件冒泡

### 事件对象

- e.target，触发事件的对象，“点击谁就是谁”；this，绑定事件的对象，“谁绑定就是谁”

- 阻止默认事件

```javascript
oBtn.onclick = function(e) {
    // 第一种方式
    // e.preventDefault();
    // 第二种方式
    return false;
};
```

```javascript
oBtn.addEventListener('click', function(e) {
    // 必须通过 e.preventDefault(); 去阻止默认事件
    e.preventDefault();
});
```

- 阻止冒泡，e.stopPropagation()

### 事件委托/代理

- 什么是事件委托？把给每个子节点绑定事件的操作设置在其父节点上，然后利用冒泡原理影响设置每个子节点
- 原理：冒泡
- 优点：1. 性能高 2. 绑定事件的操作对新增加的元素有效


## 鼠标事件对象

- e.clientX，鼠标相对于窗口的左侧距离
- e.pageX，鼠标相对于文档的左侧距离



<!-- ......................................................... -->


## 键盘

- onkeyup
- onkeydown
- onkeypress，不识别功能键，区分大小写
- 通过 `e.keyCode` 可以判断按下了那个键

## BOM

- window.load，等DOM结构和页面资源（图片、Flash、CSS样式）加载完毕后触发
- DOMContentLoaded，等DOM结构加载完毕后触发

## 定时器

- setTimeout，隔一定时间后执行一次回调函数

```javascript
var timer = setTimeout(function() {
    // 这里的代码 1s 后才执行
}, 1000);
// 清除定时器
clearTimeout(timer);
```

```javascript
// 每隔 1s 后执行回调函数
var timer = setInterval(function() {
}, 1000);
// 清除定时器
clearInterval(timer);
```

## this 指向问题

```javascript
console.log(this); // 全局作用域下的this是window

function fn() {
    console.log(this); // this => window
}
fn(); // 相当于 window.fn();


var obj = {
    age: 18,
    showAge: function() {
        // 谁调用就是谁
        console.log(this.age);
    }
};
obj.showAge();  // 18

var temp = obj.showAge;
temp(); // undefined


setTimeout(function() {
    console.log(this); // this => window
});

var temp2 = null;
function Test() {
    temp2 = this;
    // 构造函数里面的 this 就是实例对象
}
var t = new Test();
console.log(temp2 === t); // 说明构造函数里面的 this 就是实例对象

oBtn.onclick = function() {
    // this => oBtn
};
```

## JS的执行队列

- JS 的执行分为同步任务和异步任务，当碰到同步任务时直接在执行栈中执行
- 当碰到异步任务并且条件满足时，就把异步代码添加到任务队列当中
- 当执行栈中的同步任务执行完毕之后，就去轮训任务队列当中的代码并放大执行栈中执行
- 这种反复轮训任务队列的操作就是 Event Loop





