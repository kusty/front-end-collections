### 1、使用typeof bar ===“object”来确定bar是否是一个对象时有什么潜在的缺陷？这个陷阱如何避免？

-  null在javascript中会被认为是一个对象
``` js
bar = null
console.log(typeof bar) //"object"
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
所以可以用如下代码避免
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
上述代码相当于
``` js
b = 3 // b 相当于全局变量
var a = b
```
因此在上面封闭函数之外,b作为全局变量仍在作用域内,而a则为undefined，所以代码输出如下:
``` js
a defined? false
b defined? true
```
- **注意:** 上述代码在严格模式下会报"ReferenceError",因为b未定义

## 3、下面的代码将输出到控制台的是什么？，为什么？
```js
var myObject = {
    foo: "bar",
    func: function() {
        var self = this;
        console.log("outer func:  this.foo = " + this.foo);
        console.log("outer func:  self.foo = " + self.foo);
        (function() {
            console.log("inner func:  this.foo = " + this.foo);
            console.log("inner func:  self.foo = " + self.foo);
        }());
    }
};
myObject.func();
```
在内部自执行函数中,this不再指向myObject,所以上述代码输出为:
```js
outer func:  this.foo = bar
outer func:  self.foo = bar
inner func:  this.foo = undefined
inner func:  self.foo = bar
```

## 4、考虑下面的两个函数。他们都会返回同样的值吗？为什么或者为什么不？
```js
function foo1()
{
  return {
      bar: "hello"
  };
}
 
function foo2()
{
  return
  {
      bar: "hello"
  };
}

```
javascript中遇到包含return语句的行（没有其他内容）时，会在return语句之后立即自动插入分号,所以foo2相当于:
```js
function foo2()
{
  return ;
  {
      bar: "hello"
  };
}
```
所以两个函数不会返回相同的值
```js
console.log(foo1()) // {bar:"hello"}
console.log(foo2()) // undefined
```

## 5、什么是NaN？它的类型是什么？如何可靠地测试一个值是否等于NaN?  

NaN属性表示“不是数字”的值。这个特殊值是由于一个操作数是非数字的（例如“abc”/ 4）或者因为操作的结果是非数字而无法执行的。NaN有一些令人惊讶的特征:
- 虽然NaN的意思是“不是数字”，但它的类型是数字:
```js
console.log(typeof NaN) // number
```
- NaN相比任何事情,甚至本身都是false
```js
console.log(NaN === NaN) //false
```
测试一个值是否为NaN,javascript提供了一个内置函数isNaN(),但是他不可靠:
```js
isNaN(NaN);       // true
isNaN(undefined); // true
isNaN({});        // true
isNaN(true);      // false
isNaN(null);      // false
isNaN(37);        // false
// strings
isNaN('37');      // false: "37" is converted to the number 37 which is not NaN
isNaN('37.37');   // false: "37.37" is converted to the number 37.37 which is not NaN
isNaN("37,5");    // true
isNaN('123ABC');  // true:  parseInt("123ABC") is 123 but Number("123ABC") is NaN
isNaN('');        // false: the empty string is converted to 0 which is not NaN
isNaN(' ');       // false: a string with spaces is converted to 0 which is not NaN
// dates
isNaN(new Date());                // false
isNaN(new Date().toString());     // true
```
一个比较好的做法是使用 value !== value,只有当value为NaN时上述表达式为才true,另外ES6提供了Number.isNaN()函数,他的与旧的isNaN()不同,也更加可靠

## 6、下面的代码将输出到控制台，为什么？
```js
var arr1 = "john".split('');
var arr2 = arr1.reverse();
var arr3 = "jones".split('');
arr2.push(arr3);
console.log("array 1: length=" + arr1.length + " last=" + arr1.slice(-1));
console.log("array 2: length=" + arr2.length + " last=" + arr2.slice(-1));
```
- 数组的reverse方法不仅以相反的顺序返回数组,还颠倒了数组本身的顺序
- reverse方法返回对数组本身的引用

所以上述代码实际上是这样:
```js
var arr1 = "john".split('');
arr1 = arr1.reverse();
var arr2 = arr1; //arr2其实是arr1的引用,两者指向同一对象
var arr3 = "jones".split('');
```
所以arr2 和 arr1一样,输出结果如下:
```js
"array 1: length=5 last=j,o,n,e,s"
"array 2: length=5 last=j,o,n,e,s"
```
## 7、下面的代码将输出到控制台，为什么？
```js
console.log(1 +  "2" + "2");
console.log(1 +  +"2" + "2");
console.log(1 +  -"1" + "2");
console.log(+"1" +  "1" + "2");
console.log( "A" - "B" + "2");
console.log( "A" - "B" + 2);

"122"
"32"
"02"
"112"
"NaN2"
NaN
```
- 示例2中第二个+为一元运算符,所以javascript会将"2"类型转为数字,然后将+应用于他,相当于视其为正整数2,所以1++"2",实为1+2
- 示例3原理同上,-"1",实际为负数1,1+-"1",实际为1-1
- 示例5 6中"A"-"B"的值均为NaN,后面+"2"为字符串拼接,而+2为数值相加,NaN任何数值操作均为NaN

### 8、如果数组列表太大，以下递归代码将导致堆栈溢出。你如何解决这个问题，仍然保留递归模式？
```js
var list = readHugeList();
var nextListItem = function() {
    var item = list.pop();
 
    if (item) {
        // process the list item...
        nextListItem();
    }
};
```
通过修改nextListItem函数可以避免潜在的堆栈溢出，如下所示:
```js
var list = readHugeList();
 
var nextListItem = function() {
    var item = list.pop();
 
    if (item) {
        // process the list item...
        setTimeout( nextListItem, 0);
    }
};
```
堆栈溢出被消除，因为事件循环处理递归，而不是调用堆栈。当nextListItem运行时，如果item不为null，则将超时函数（nextListItem）推送到事件队列，并且函数退出，从而使调用堆栈清零。当事件队列运行超时事件时，将处理下一个项目，并设置一个计时器以再次调用nextListItem。因此，该方法从头到尾不经过直接递归调用即可处理，因此调用堆栈保持清晰，无论迭代次数如何。