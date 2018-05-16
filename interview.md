### 1、使用typeof bar ===“object”来确定bar是否是一个对象时有什么潜在的缺陷？这个陷阱如何避免？

-  null在javascript中会被认为是一个对象
``` js
bar = null
console.log(typeof bar) //"object"
```
- 数组也会被认为是一个对象
``` js
bar = []
console.log(typeof bar) //"object"
```
- 函数不会被认为是一个对象
``` js
bar = function (){}
console.log(typeof bar) //"function"
```
- 所以可以用如下代码避免
``` js
//ES5
console.log((bar !== null) && (typeof bar === "object") && (toString.call(bar) !== "[object Array]"))

//ES6
console.log((bar !== null) && (typeof bar === "object") && !Array.isArray(bar))
```

## 2、下面的代码将输出到控制台的是什么，为什么？
``` js
(function(){
  var a = b = 3;
})();
 
console.log("a defined? " + (typeof a !== 'undefined'));
console.log("b defined? " + (typeof b !== 'undefined'));
```
- 上述代码相当于
``` js
b=3;// b 相当于全局变量
var a=b;
```
- 因此在上面封闭函数之外,b作为全局变量仍在作用域内,而a则为undefined，所以代码输出如下:
``` js
a defined? false
b defined? true
```
- **注意:** 上述代码在严格模式下会报"ReferenceError",因为b未定义