### 常用字符
```html
.     匹配除换行符以外的任意字符                                                       *     重复零次或更多次   
\w    匹配字母或数字或下划线或汉字    \W   匹配任意不是字母，数字，下划线，汉字的字符      +     重复一次或更多次
\s    匹配任意的空白符               \S   匹配任意不是空白符的字符                      ?     重复零次或一次
\d    匹配数字                      \D   匹配任意非数字的字符                         {n}     重复n次
\b    匹配单词的开始或结束           \B   匹配不是单词开头或结束的位置                  {n,}    重复n次或更多次
^     匹配字符串的开始               [^x] 匹配除了x以外的任意字符                      {n,m}   重复n到m次
$     匹配字符串的结束
```
### 后向引用
使用小括号指定一个子表达式后，匹配这个子表达式的文本(也就是此分组捕获的内容)可以在表达式或其它程序中作进一步的处理。默认情况下，每个分组会自动拥有一个组号，
规则是：从左向右，以分组的左括号为标志，第一个出现的分组的组号为1，第二个为2，以此类推。分组0对应整个正则表达式
```javascript
"go go".match(/\b(\w+)\b\s+\1\b/g) //==>["go go"]
```
你也可以自己指定子表达式的组名，要指定一个子表达式的组名，请使用这样的语法：<code>(?&lt;test&gt;\w+)</code>【或者把尖括号换成'也行，即<code>(?'test'\w+)</code>】
```javascript
\b(?<Word>\w+)\b\s+\k<Word>\b
```
### 零宽断言
```javascript
    "<div>hello</div>".match(/(?<=<(\w+)>).*(?=<\/\1>)/g) //==>["hello"]
  ```
- 零宽度正预测先行断言 <code>(?=exp)</code>
  断言自身出现的位置的后面能匹配表达式exp.
  ```javascript
    "coding boy".match(/\b\w+(?=ing\b)/g) //==>["cod"]
  ```
 - 零宽度正回顾后发断言 <code>(?<=exp)</code>
   断言自身出现的位置的前面能匹配表达式exp.
   ```javascript
    "interesting".match(/(?<=\bin)\w+\b/g) //==>["teresting"]
   ```
 ### 负向零宽断言
 - 零宽度负预测先行断言 <code>(?!exp)</code>
   断言此位置的后面不能匹配表达式exp.
   ```javascript
   //匹配三位数字，而且这三位数字的后面不能是数字
    "135A".match(/\d{3}(?!\d)/g) //==>["135"]
   ```
 - 零宽度负回顾后发断言 <code>(?<!exp)</code>
   断言此位置的前面不能匹配表达式exp.
   ```javascript
   //匹配前面不是小写字母的七位数字
    "A1357900".match(/(?<![a-z])\d{7}/g) //==>["1357900"]
   ```
 ### 贪婪&懒惰
 
 | 代码/语法   |  说明  |
 |:------:  |------|
 |*?   | 重复任意次，但尽可能少重复|  
 |+?   | 重复1次或更多次，但尽可能少重复|
 |??   | 重复0次或1次，但尽可能少重复|
 |{n,m}? | 重复n到m次，但尽可能少重复|
 |{n,}?   | 重复n次以上，但尽可能少重复|
 
 ```javascript
 //贪婪匹配
 'aabab'.match(/a.*b/g) //==> ["aabab"]
 
 //懒惰匹配
 'aabab'.match(/a.*?b/g) //==> ["aab", "ab"].最先开始的匹配拥有最高的优先权，因此第一个是aab而不是ab
 ```

### 应用实例
- 获取地址栏参数
```javascript
function getParam(url) {
	return s.match(/([^?&=]+)(=[^=&]+)/g).reduce((acc,val)=>(acc[val.split('=')[0]] = val.split('=')[1],acc),{})
}
let s = 'http://www.test.com/setting?type=add&id=3&code=addrole'
// getParam(s) => {code: "addrole",id: "3",type: "add"}
```
