## 移动端问题 ##
>滑动

加载jroll.js，需多包一个空div用作jroll对象。例：

```html
<div class="bupm-bg-white" id="hstWrap">
	<div>
      <!--some scroll content-->
    </div>
</div>
```

> 输入框  

如果底部有fixed定位，加载jroll-fixedinput.js。例：

```javascript
tstHstRoll.fixedinput(10);
```

![安卓](https://github.com/weixisheng/input-problem/tree/master/img/android-input.jpg)

作为软键盘收回事件的模拟方案

```javascript
(function() {
  var wHeight = window.innerHeight; //获取初始可视窗口高度
  $(window).resize(function() { //监测窗口大小的变化事件
    var hh = window.innerHeight; //当前可视窗口高度
    if (wHeight < hh) { //可以作为虚拟键盘收回事件
      $("input").blur();
    }
    wHeight = hh;
  });
}());
```
