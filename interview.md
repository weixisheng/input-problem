#### 柯里化函数

```javascript
const currying = function (targetFn, targetFnLength = targetFn.length) {
    return function () {
        const args = [].slice.call(arguments);
        if (targetFnLength <= args.length) {
            return targetFn.apply(null, args);
        }
        return currying(function () {
            const followingArgs = [].slice.call(arguments);
            return targetFn.apply(null, args.concat(followingArgs));
        }, targetFnLength - args.length);
    }
}

const curry = function (fn) {
    return currying(fn);
};
const plus = curry((x, y, z, m) => {
    return x + y + z + m;
});
console.log(plus(1, 2, 3, 4));
console.log(plus(1, 2, 3)(4));
console.log(plus(1, 2)(3)(4));
console.log(plus(1)(2)(3)(4));
```

#### 容易忽视的面试题

```javascript
function Foo() {
    this.getName = function(){
        console.log(3);
        return {getName:getName};
    };
    getName = function () { console.log (1); };
    return this;
}
Foo.getName = function () { console.log (2);};
Foo.prototype.getName = function () { console.log (6);};
var getName = function () { console.log (4);};
function getName() { console.log (5);}
 
//请写出以下输出结果：
Foo.getName();//2
getName();//4
Foo().getName();//1
getName();//1
new Foo.getName();//==>new (Foo.getName)() 2
new Foo().getName();//==>(new Foo()).getName() 3
new Foo().getName().getName();//3 1
new new Foo().getName();//==>new (new Foo().getName)() 3
```

#### 事件循环机制

```javascript
//宏任务(script,[setTimeout,setInterval,setImmediate第二轮事件循环])
//==>微任务(process.nextTick,promise)
console.log('golb1');//1

setTimeout(function() {
    console.log('timeout1');//8
    process.nextTick(function() {
        console.log('timeout1_nextTick');//12
    })
    new Promise(function(resolve) {
        console.log('timeout1_promise');//9
        resolve();
    }).then(function() {
        console.log('timeout1_then')//14
    })
})

setImmediate(function() {
    console.log('immediate1');//16
    process.nextTick(function() {
        console.log('immediate1_nextTick');//20
    })
    new Promise(function(resolve) {
        console.log('immediate1_promise');//17
        resolve();
    }).then(function() {
        console.log('immediate1_then')//22
    })
})
process.nextTick(function() {
    console.log('glob1_nextTick');//4
})
new Promise(function(resolve) {
    console.log('glob1_promise');//2
    resolve();
}).then(function() {
    console.log('glob1_then')//6
})

setTimeout(function() {
    console.log('timeout2');//10
    process.nextTick(function() {
        console.log('timeout2_nextTick');//13
    })
    new Promise(function(resolve) {
        console.log('timeout2_promise');//11
        resolve();
    }).then(function() {
        console.log('timeout2_then')//15
    })
})

process.nextTick(function() {
    console.log('glob2_nextTick');//5
})
new Promise(function(resolve) {
    console.log('glob2_promise');//3
    resolve();
}).then(function() {
    console.log('glob2_then')//7
})

setImmediate(function() {
    console.log('immediate2');//18
    process.nextTick(function() {
        console.log('immediate2_nextTick');//21
    })
    new Promise(function(resolve) {
        console.log('immediate2_promise');//19
        resolve();
    }).then(function() {
        console.log('immediate2_then')//23
    })
})
```

#### 每隔1s输出0,1,2,3,4,5

```javascript
//闭包
for (var i = 0; i < 5; i++) {
    (function(j) {
        setTimeout(function() {
            console.log(j);
        }, 1000 * j);  
    })(i);
}

setTimeout(function() { 
    console.log(i);
}, 1000 * i);
```

```javascript
//Promise
let tasks = [];
const output = (i)=> new Promise((resolve)=>{
    setTimeout(()=>{console.log(i);resolve();}, 1000*i)
});
for(var i=0;i<5;i++){
    tasks.push(output(i));
}
Promise.all(tasks).then(()=>{
    setTimeout(()=>{console.log(i)}, 1000); //循环结束后i=5,注意只需1s
});
```

```javascript
//async
const sleep = (t) => new Promise(resolve => {
    setTimeout(resolve,t);
}); 
async function output(n) {
    let i = 0;
    for(;i<n;i++){
        console.log(i);
        await sleep(1000);
    }
    return i;
}
output(5).then(console.log);
```

#### Vue的computed工作流程

```html
1、data属性初始化，使用Object.defineProperty转化为getter和setter
2、computed属性初始化，每个属性提供的函数作为属性的 getter
3、Object.defineProperty getter 依赖收集，用于依赖发生变化时，触发属性重新计算
4、computed 计算属性嵌套其他 computed 计算属性时，先进行其他的依赖收集
```

#### new操作符

```javascript
var obj  = {}; //创建一个新对象；
obj.__proto__ = F.prototype; //将构造函数的作用域赋给新对象（因此this就指向了这个对象）；
F.call(obj); //执行构造函数中的代码（为这个新对象添加属性）；
return obj; //返回新对象。
```

#### 一个页面从输入URL到页面加载显示

```bash
1、首先，在浏览器地址栏中输入url
2、浏览器先查看浏览器缓存-系统缓存-路由器缓存，如果缓存中有，会直接在屏幕中显示页面内容。若没有，则继续。
3、在发送http请求前，需要域名解析(DNS解析)，解析获取相应的IP地址。
4、浏览器向服务器发起tcp连接，与服务器建立tcp三次握手。
5、握手成功后，浏览器向服务器发送http请求，请求数据包。
6、服务器处理收到的请求，将数据返回至浏览器
7、浏览器收到HTTP响应
8、读取页面内容，浏览器渲染，解析html源码
9、生成Dom树、解析css样式、构建渲染树
10、js解析执行
11、显示页面
```

