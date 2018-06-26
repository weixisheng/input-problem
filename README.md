## 移动端问题 ##
> meta标签

```html
<meta name="viewport" content="width=device-width, initial-scale=1, user-scalable=0, minimum-scale=1.0, maximum-scale=1.0" />
<meta name="apple-mobile-web-app-capable" content="yes" />
<meta name="format-detection" content="telephone=no" />
<meta name="format-detection" content="email=no" />
<meta name="apple-mobile-web-app-capable" content="yes" /><!-- ios7.0版本以后，safari上已看不到效果 -->
<meta name="apple-mobile-web-app-status-bar-style" content="black" /><!--将网站添加到主屏幕快速启动方式，仅针对ios的safari顶端状态条的样式。 可选default、black、black-translucent -->
```

> flex布局

```css
/*flex容器*/
.flex{
  display: -webkit-box; /* 老版本语法: Safari, iOS, Android browser, older WebKit browsers. */
    display: -moz-box; /* 老版本语法: Firefox (buggy) */
    display: -ms-flexbox; /* 混合版本语法: IE 10 */
    display: -webkit-flex; /* 新版本语法: Chrome 21+ */
    display: flex; /* 新版本语法: Opera 12.1, Firefox 22+ */
}
/*主轴对齐*/
.justify{
  -webkit-box-pack: start | center | end | justify;
    -moz-justify-content: center;
    -webkit-justify-content: center;
    justify-content: flex-start | center | flex-end | space-between | space-around;
}
/*侧轴对齐*/
.align{
   -webkit-box-align: start | end | center | baseline | stretch;
    -moz-align-items: center;
    -webkit-align-items: center;
    align-items: flex-start | flex-end | center | baseline | stretch;
}
/*子元素显示方向*/
/*左到右*/
.direction{
  -webkit-box-direction: normal;
    -webkit-box-orient: horizontal;
  
    -moz-flex-direction: row;
    -webkit-flex-direction: row;
    flex-direction: row;
}
/*右到左*/
.direction{
  -webkit-box-pack: end;/*box-pack 改变对齐方式*/
    -webkit-box-direction: reverse;
    -webkit-box-orient: horizontal;
  
    -moz-flex-direction: row-reverse;
    -webkit-flex-direction: row-reverse;
    flex-direction: row-reverse;
}
/*上到下*/
.direction{
  -webkit-box-direction: normal;
    -webkit-box-orient: vertical;
  
    -moz-flex-direction: column;
    -webkit-flex-direction: column;
    flex-direction: column;
}
/*下到上*/
.direction{
  -webkit-box-pack: end;
    -webkit-box-direction: reverse;
    -webkit-box-orient: vertical;
  
    -moz-flex-direction: column-reverse;
    -webkit-flex-direction: column-reverse;
    flex-direction: column-reverse;
}
/*子元素换行*/
.wrap{
   -moz-box-lines: single | multiple; /*Firefox*/
    -webkit-box-lines: single | multiple; /*Safari,Opera,Chrome*/
    box-lines: single | multiple;
  	-webkit-flex-wrap:nowrap | wrap;
  	flex-wrap: nowrap | wrap
}
/*子元素缩放*/
.item{
    box-flex: <value>;
    /*伸缩：<一个浮点数，默认为0.0，即表示不可伸缩，大于0的值可伸缩，柔性相对>*/
 
    flex-grow: <number>; /* default 0 */
    /*放大：默认0（即如果有剩余空间也不放大，值为1则放大，2是1的双倍大小，以此类推）*/
 
    flex-shrink: <number>; /* default 1 */
    /*缩小：默认1（如果空间不足则会缩小，值为0不缩小）*/
  	flex:0 1 auto;/*flex-grow, flex-shrink 和 flex-basis的简写,默认0 1 auto; auto (1 1 auto) 和 none (0 0 auto)*/
}
/*子元素顺序*/
.order{
   -webkit-box-ordinal-group: 1;
    -moz-order: 1;
    -webkit-order: 1;
    order: 1;
}
```

> 显示省略号

```css
/*单行文字省略号*/
.ellipse1{
   white-space: nowrap;
   overflow: hidden;
   text-overflow: ellipsis;
}
/*多行文字省略号*/
.ellipse2{
  overflow: hidden;
  display: -webkit-box;
  -webkit-line-clamp: 2;//正整数
  -webkit-box-orient: vertical;
  text-overflow: ellipsis;
}
```

> 滑动

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

> 快速tab

```html
<div class="tonic-tabs">
  <div class="tab-item active">tab1</div>
  <div class="tab-item">tab2</div>
  <div class="underline"></div>
</div>
<div class="tab-content">
  <div class="tc-1"></div>
  <div class="tc-2 hidden"></div>
</div>
```

```javascript
var tabs = $(".tonic-tabs>.tab-item"),
    panels = $(".tab-content>div"),
    underline = $(".underline");
tabs.each(function(index) {
        $(this).data("tab", index).click(function() {
            var target = panels.eq($(this).data("tab"));
	    $(this).addClass("active").siblings("div").removeClass("active");
	    target.removeClass("hidden").siblings("div").addClass("hidden");
          var l = this.offsetLeft;
          underline.css({
            "-webkit-transform": "translateX(" + l + "px)",
            "transform": "translateX(" + l + "px)"
          });
        });
});

var tabCon = document.querySelector(".tab-content"),
    tabItems = document.querySelectorAll(".tab-item");
var startX, moveX, differ;
tabCon.addEventListener('touchstart', function(e) {
  var touch = e.touches[0];
  startX = touch.pageX;
}, false);
tabCon.addEventListener('touchmove', function(e) {
  var touch = e.touches[0];
  moveX = touch.pageX;
  differ = moveX - startX;
}, false);
tabCon.addEventListener('touchend', function(e) {
  //left-->right
  if (differ > 0 && differ > 50) {
    if (tabItems[1].classList.contains('active')) {
      tabItems[0].click();
    }
  }
  //right-->left
  else if (differ < 0 && differ < -50) {
    if (tabItems[0].classList.contains('active')) {
      tabItems[1].click();
    }
  }
}, false);
```

> 浏览器打开app问题(**微信浏览器不行**)

```javascript
var u = navigator.userAgent;
var isAndroid = u.indexOf('Android') > -1 || u.indexOf('Linux') > -1; //android终端或者uc浏览器
var isiOS = !!u.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/); //ios终端

// wechat
if (/micromessenger/i.test(u)) {
  $(".weixin-layer").removeClass('hidden');
}
// app
else {
  if (isAndroid) {
    window.location.href = "xxx://"; /***打开app的协议，由安卓同事提供***/
    setTimeout(function() {
      window.location.href = "http://app.qq.com/#id=detail&appid=xxx"; /***下载安卓app的地址***/
    }, 1000);
  } else {
    var ifr = document.createElement("iframe");
    ifr.src = "xxx://"; /***打开app的协议，由ios同事提供***/
    ifr.style.display = "none";
    document.body.appendChild(ifr);
    setTimeout(function() {
      document.body.removeChild(ifr);
      window.location.href = "itms-apps://itunes.apple.com/tc/app/xxx?mt=8";
    }, 1000);
  }
}
```

> 判断是否空对象

```javascript
function isEmptyObject(o){
  for(var i in o){
    /*或
    	if(o.hasOwnProperty(i))
    */ 
  	return !1;
  }
  return !0;
}
```

