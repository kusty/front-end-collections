### 1、使用typeof bar ===“object”来确定bar是否是一个对象时有什么潜在的缺陷？这个陷阱如何避免？

> null在javascript中会被认为是一个对象
``` js
bar = null
console.log(typeof bar) //"object"
```
> 数组也会被认为是一个对象
``` js
bar = []
console.log(typeof bar) //"object"
```
> 函数不会被认为是一个对象
``` js
bar = function (){}
console.log(typeof bar) //"function"
```
> 所以可以用如下代码避免
``` js
console.log((bar !== null) && (typeof bar === "object") && (toString.call(bar) !== "[object Array]"));

//ES6写法
console.log((bar !== null) && (typeof bar === "object") && !Array.isArray(bar));
```

### 2、下面的代码将输出到控制台的是什么，为什么？
``` js
(function(){
  var a = b = 3;
})();
 
console.log("a defined? " + (typeof a !== 'undefined'));
console.log("b defined? " + (typeof b !== 'undefined'));
```