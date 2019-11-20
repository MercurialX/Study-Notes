# ECMAScript 6简介
1. ECMAScript和JavaScript的关系是，前者是后者的规格，后者是前者的一种实现。这两个词是可以互换的；  
2. ES6既是一个历史名词，也是一个泛指，含义是5.1版本以后的JavaScript的下一代标准，涵盖了ES2015、ES2016、ES2017等，而ES2015则是正式名称，特指当年发布的正式版本的语言标准；
3. [ES-Checker](https://github.com/ruanyf/es-checker)
4. [ES6转码器-Babel](https://babeljs.io)：.babelrc文件、babel-cli/babel-node、babel-register、babel-core、babel-polyfill
5. [ES6转码器-Traceur](https://github.com/google/traceur-compiler)：插入网页、在线转换、命令行转换、Node环境
# let和const命令
## let
1. 声明的变量仅在块级作用域内有效：

```JavaScript
var a = [];
for(var i = 0; i < 10; i ++) {
    a[i] = function() {
        console.log(i);
    };
}

a[6](); // 10

---
var a = [];
for(let i = 0; i < 10; i ++) {
    a[i] = function() {
        console.log(i);
    };
}

a[6](); // 6
```

2. 设置循环变量的那部分是一个父作用域，而循环体内部是一个单独的子作用域：

```JavaScript
for(let i = 0; i < 3; i ++) {
    let i = 'hello';
    console.log(i);
}
// hello
// hello
// hello
```

3. 不存在变量提升，即声明的变量一定要在声明后使用：

```JavaScript
console.log(a); // undefined
console.log(b); // ReferenceError报错
var a = 1;
let b = 2;
```

4. 暂时性死区，即只要块级作用域内存在let命令，它就不再受外部的影响：
```JavaScript
var temp = 123;
if(true) {
 typeof temp; // ReferenceError报错
 temp = 'abc'; // ReferenceError报错
 let temp;
 console.log(temp); // undefined
 temp = 'abc';
 console.log(temp); // abc
}
// 隐蔽的死区
function bar(x = y, y = 2) {
    return [x, y];
}

bar(); // RefrenceError, y is not defined

function bar(x = 2, y = x) {
    return [x, y];
}

bar(); // [2, 2]

var x = x; // undefined
let y = y; // ReferenceError报错
```
5. 不允许重复声明；

```JavaScript
function func(arg) {
    let arg = 1;
}

func('a'); // Syntax报错
```

## 块级作用域
#### 本质
块级作用域是一个语句，将多个操作封装在一起，没有返回值。
#### 使用原因
1. 内层变量可能会覆盖外层变量：

```JavaScript
var tmp = new Date();
function f() {
    console.log(tmp);
    if(false) {
        var tmp = 'abc';
    }
}

f(); // undefined
```

2. 用来计数的循环变量泄漏为全局变量；
#### ES6场景
1. ES6允许块级作用域任意嵌套；
2. 内层作用域可以定义外层作用域的同名变量；
3. 可以取代立即执行匿名函数（IIFE）；

```JavaScript
// IIFE
(function() {
    var tmp = 'abc';
    ...
})()
// 块级作用域
{
    let tmp = '123';
    ...
}
```
#### 块级作用域与函数声明
1. 允许再块级作用域内声明变量；
2. 函数声明类似于var，即会提升到全局作用域或函数作用域的头部；
3. 函数声明还会提升到所在的块级作用域的头部；
4. 环境导致的差异过大，避免在块级作用域内声明函数。如果声明，应使用函数表达式形式；
5. ES6允许声明函数的规则只在使用大括号的情况下成立，否则会报错；
## const
1. 声明一个只读的常量，一旦声明，值就不能改变；
2. 一旦声明常量，必须立即初始化，否则会报错；
3. 和let命令相同：只在声明所在的块级作用域内有效，存在暂时性死区，不可重复声明；
4. const实际上保证的并不是变量的值不得改动，而是变量指向的那个内存地址不得改变；
# 变量的解构赋值
1. 含义：ES6允许按照一定模式从数组和对象中提取值，然后对变量进行赋值； 

```JavaScript
let [foo, [[bar], baz]] = [1, [[2], 3]];
foo // 1
bar // 2
baz // 3

let [head, ...tail] = [1, 2, 3, 4];
tail // [2, 3, 4]

let [x, y, ...z] = ['a'];
x // "a"
y // undefined
z // []

let [a, [b], d] = [1, [2, 3], 4];
a // 1
b // 2
d // 4
```
2. 解构赋值指定默认值；

```JavaScript
function f() {
    console.log('aaa');
}

let [x = f()] = [1];
x // 1

let [x = 1, y = x] = []; // x = 1; y = 1
let [x = 1, y = x] = [2]; // x = 2; y = 2
let [x = 1, y = x] = [1, 2]; // x = 1; y = 2
let [x = y, y = x] = []; // ReferenceError报错
```

3. 对象的解构赋值，数组的元素按次序排列，变量的取值由它的位置决定，对象的元素没有次序，变量必须与属性同名才能取值；

```JavaScript
let {foo, baz} = {baz: 'a', foo: 'b'};
foo // "b"
baz // "a"

let {foo: baz} = {foo: 'aaa', bar: 'bbb'}
foo // error
baz // "aaa"
```

4. 对象的解构默认值生效的条件是，对象的属性值严格等于undefined

```JavaScript
var {x = 3} = {x: undefined};
var {y = 3} = {y : null};
x // 3
y // null
```

5. 变量赋值；

```JavaScript
let x;
{x} = {x: 1}; // 报错

let x;
({x} = {x: 2}); // 2
```

6. 特殊写法；

```JavaScript
({} = [true, false]);

let {log, sin, cos} = Math;

let arr = [1, 2, 3];
let {0: first, [arr.length - 1]: last} = arr;
first // 1
last // 3
```

7. 字符串解构赋值；

```JavaScript
const [a, b, c, d, e] = 'hello';
a // h
e // o

let {length: len} = 'hello';
len // 5
```

8. 数值和布尔值的解构赋值，如果等号右边是数值和布尔值，则会先转化为对象；

```JavaScript
let {toString: s} = 123;
let {toString: str} = true;
s === Number.prototype.toString // true
str === Boolean.prototype.toString // true
```

9. 函数参数的解构赋值；
10. 圆括号，不能使用的场景和可以使用的场景；
11. 使用场景：

```JavaScript
// 场景一：交换变量的值
let x = 1;
let y = 2;
[x, y] = [y, x];

// 场景二：从函数返回多个值
function example() {
    return [1, 2, 3];
}

let [a, b, c] = example();

function example() {
    return {x: 1, y: 2};
}

let {x, y} = example();

// 场景三：函数参数的定义
function f(x, y, z) {...}
f([1, 2, 3]);

function f({x, y, z}) {...}
f({z: 3, y: 2, x: 1}); // 传入无序参数

// 场景四：提取JSON数据
let jsonData = {id: 1, status: 'ok', data: [1, 2]};
let {id, status, data: number} = jsonData;
id // 1
status // "ok"
number // [1, 2]

// 场景五：函数参数的默认值
function f({a = 1, b = 2}) {...}

// 场景六：遍历Map结构(任何部署了Iterator都能使用for of)
// 扩展---array, map, set
var map = new Map();
map.set("first", "1");
map.set("second", "2");
for(let [key, value] of map) {
    console.log(key + " is " + value);
}

for(let [key] of map) {...}
for(let [, value] of map) {...}

// 场景七：输入模块的指定方法
const {SourceMapConsumer, SourceNode} = require("source-map");
```
# 字符串的扩展
## 字符的Unicode表示法
1. JavaScript允许采用\uxxxx的形式表示一个字符，其中xxxx表示字符的Unicode码点；
2. 表示的范围在\u0000~\uFFFF之间，超过了必须使用2个双字节的形式表达；
3. ES6中将码点放入大括号，就能正确解读该字符；
4. 字符的6种表示方法。

```JavaScript
'\z' === 'z';
'\172' === 'z';
'\x7A' === 'z';
'\u007A' === 'z';
'\u{7A}' === 'z';
```
## codePointAt()
测试一个字符是由2个字节还是4个字节组成：

```JavaScript
function is32Bit(c) {
    return c.codePointAt(0) > 0xFFFF;
}
```
## 从码点返回字符String.formCharCode() --> String.fromCodePoint()

```JavaScript
String.fromCodePoint(0x20BB7); // "𠮷"
```

## 字符串遍历for --> for of

```JavaScript
for(let char of "foo") {
    console.log(char);
}
```
## 返回字符串给定位置的字符charAt() --> at()（提案）

```JavaScript
'𠮷'.at(0); // "𠮷"
```

## Unicode正规化normalize()
```JavaScript
'\u01D1' === '\u004F\u030C'; // false
'\u01D1'.normalize() === '\u004F\u030C'.normalize(); // true
```
## 字符串包含indexOf() --> includes()、startsWith()、endsWith()

```JavaScript
var s = "hello world!";
s.includes('world', 6); // true 
s.startsWith('world', 5); // true 
s.endsWith('world', 5); // false 
```
## 将字符串重复x次 repeat();

```JavaScript
'hello'.repeat(2); // "hellohello"
'x'.repeat(2.9); // "xx"
'x'.repeat(-1); // RangeError报错
'x'.repeat(Infinity); // RangeError报错
'x'.repeat(-0.9); // ""
'x'.repeat(NaN); // ""
'x'.repeat("3"); // "xxx"
```
## 字符串补全长度 padStart()、padEnd()

```JavaScript
'xxx'.padStart(6, 'ab'); // "abaxxx"
'xxx'.padEnd(6, 'ab'); // "xxxaba"
'xxx'.padStart(2, 'ab'); // "xxx"
'xxx'.padStart(6, 'abcdefg'); // "abcxxx"
'xxx'.padStart(6); // "   xxx"
'09-12'.padStart(10, 'YYYY-MM-DD'); // YYYY-09-12
```
## 模版字符串

```JavaScript
var name = "Xia", age = 18;
`My name is ${name}, 
my age is ${age}`;

var a = 1, b = 2;
`a + b = ${a + b}`; // "a + b = c"

var obj = {a: 1, b: 2};
`${obj.a + obj.b}`; // "3"

function fn() {
    return "xia"
}

`hello ${fn()}!`; // "hello xia!"
```
## 标签模版

```JavaScript
function tag(s, v1, v2) {
    console.log(s[0]);
    console.log(s[1]);
    console.log(s[2]);
    console.log(v1);
    console.log(v2);
    return "OK";
}

let a = 1, b = 2;
tag`Hello ${a + b} World ${a * b}`; // 等价tag(["Hello ", " World ", ""], 3, 2)
// "Hello "
// " World "
// ""
// 3
// 2
// "OK"
```
## String.raw()

```JavaScript
String.raw`test\n${2 + 3}`; // "test\n5"
String.raw({raw: 'test'}, 1, 2, 3, 4, 5); // "t1e2s3t"
```
# 数值的扩展
#### 二进制和八进制
二进制前缀0b(或0B)，八进制前缀0o（或0O），使用Number转为十进制。
#### Number.isFinite()、Number.isNaN()
传统方法先调用Number()转为数值再判断，新方法只对数值有效。
#### Number.parseInt、Number.parseFloat()
#### 判断整数Number.isInteger()

```JavaScript
Number.isInterger(3); // true
Number.isInterger(3.0); // true
```
#### 误差常量Number.EPSILON

```
function withinErrorMargin(left, right) {
    return Math.abs(left - right) < Number.EPSILON;
}

withinErrorMargin(0.1 + 0.2, 0.3); // true
withinErrorMargin(0.2 + 0.2, 0.3); // false
```
#### Number.MAX_SAFE_INTEGER、 Number.MIN_SAFE_INTEGER
#### Math对象的扩展
1. 取整：Math.trunc()

```JavaScript
Math.trunc = Math.trunc || function(x) {
    return x < 0 ? Math.ceil(x) : Math.floor(x);
}
```

2. 判断正负数或0：Math.sign()

```JavaScript
Math.sign = Math.sign || function(x) {
    x = + x;
    if(x === 0 || Number.isNaN(x)) {
        return x;
    }
    return x > 0 ? 1 : -1;
}
```

3. 立方根：Math.cbrt()

```JavaScript
Math.cbrt = Math.cbrt || function(x) {
    let y = Math.pow(Math.abs(x), 1/3);
    return x < 0 ? -y : y;
}
```

4. 返回整数的前导0数量：Math.clz32()

```JavaScript
Math.clz32(true); // 31
```

5. 带符号32位整数形式相乘：Math.imul()
6. 单精度浮点数形式：Math.fround()

```JavaScript
Math.fround = Math.fround || function(x) {
    return new Float32Array([x])[0];
}
```

7. 返回所有参数平方和的平方根：Math.hypot()
8. 对数方法：Math.expm1()、Math.log1p()、Math.log10()、Math.log2()
9. 指数运算符（\*\*、\*\*=）

```JavaScript
2 ** 2; // 4
2 ** 3; // 8
let a = 2;
a **= 2; // 等同于a = a * a
a **= 3; // 等同于a = a * a * a
```

# 函数的扩展
## 参数的默认值
