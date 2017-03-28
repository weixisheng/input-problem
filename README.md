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
tstHstRoll.fixedinput(10);//上移到离wrapper底部10px处
tstHstRoll.fixedinput(10,true);//scroller里面的input/textarea的tabIndex设置为-1，解决tab键切换错位bug
```
<img src="https://github.com/weixisheng/input-problem/blob/master/img/android-input.png" width="300px"><img src="https://github.com/weixisheng/input-problem/blob/master/img/ios-input.png" width="300px">

作为安卓软键盘收回事件的模拟方案

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
