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
1. 参数变量是默认声明的，不能用let或const再次声明；
2. 函数不能有同名参数；
3. 惰性求值：参数默认值不是传值的，而是每次都重新计算默认值表达式的值；
4. 与解构赋值的默认值结合使用；

```JavaScript
function f({x, y = 2}) {
    console.log(x, y);
}
f({}); // undefined, 5
f();  // TypeError
f({x: 1}); // 1, 2
f({x: 1, y: 3}); // 1, 3

function f1({x = 0, y = 0} = {}) {
    return [x, y];
}
function f2({x, y} = {x: 0, y: 0}) {
    return [x, y];
}
f1(); // [0, 0]
f2(); // [0, 0]
f1({}); // [0 , 0]
f2({}); // [undefined, undefined]
```

5. 无法只省略有默认值的参数，而不省略其后的参数，除非显示输入undefined；

```JavaScript
function f(x, y = 2, z) {
    return [x, y, z];
}
f(); // [undefined, 2 , undefined]
f(1); // [1, 2, undefined]
f(1,,2); // 报错
f(1, undefined, 2); // [1, 2, 2]
```

6. 指定了函数的默认值，函数的length将返回没有指定默认值的参数个数，如果设置了默认值不是尾参数，后面的参数也不计入length；

```JavaScript
(function f1(a, b = 2, c) {}).length; // 1
```

7. 设置了函数默认值，声明初始化的时候，参数会形成一个单独的作用域，直到初始化完成；

```JavaScript
let x = 1;
function f(y = x) {
    let x = 2;
    console.log(y);
}
f(); // 1

var a = 1;
(function f(a = a) {})(); // 报错

var x = 3;
function f(x, y = function() { x = 4 }) {
    var x = 5;
    y();
    console.log(x);
}
f(); // 5
console.log(x); // 3

var x = 3;
function f(x, y = function() { x = 4 }) {
    x = 5;
    y();
    console.log(x);
}
f(); // 4
console.log(x); // 3
```

8. 应用；

```JavaScript
// 指定一个参数不能省略
function throwIfMissing() {
    throw new Error('参数不能省略');
}

function foo(mustBeProvided = throwIfMissing()) {
    return mustBeProvided;
}

foo(); // Error: 参数不能省略

// 参数设为undefined表示可以省略
function foo(optional = undefined) {
    return optional;
}

foo(); // undefined;
```
## rest参数
1. ES6引入rest参数（形式为...变量名），用于获取函数多余参数，替代arguments；

```JavaScript
function f(...numbers) {
    let sum = 0;
    for(let value of numbers) {
        sum += value;
    }
    
    return sum;
}

f(1, 2, 3); // 6
```
2. rest参数中的变量代表数组，所以数组的方法都能使用；
3. rest参数之后不能有其他参数，否则会报错；
4. rest参数不参与length。

## 严格模式
只要参数设置了默认值，解构赋值，扩展运算符，就不能显示指定严格模式，规避：

```JavaScript
// 设定全局的严格模式
'use strict';

function f(a = 1) {...}

// 把函数包在一个无参数的立即执行函数里
const doSomething = (function(){
    'use strict';
    
    return function(value = 1) {
        return value;
    }
})();
```
## name属性
1. 使用name属性返回该函数的函数名；
2. 匿名函数赋值给变量，ES5返回为空，ES返回变量名；

```JavaScript
var f = function() {...};
//ES5
f.name; // "";
//ES6
f.name; // "f" 
```

3. Function构造函数的name属性为anonymous；

```JavaScript
(new Function).name; // "anonymous"
```

4. bind返回的函数，name属性会加上bound前缀；

```JavaScript
function foo() {};
(foo.bind({})).name; // "bound foo"
```
## 箭头函数
1. 使用箭头（=>）定义函数，圆括号"（）"代表参数，多条语句使用{}；

```JavaScript
var foo = (a, b) => {
    let result = a  + b;
    return result;
}
```

2. 与解构赋值结合使用；

```JavaScript
var foo = ({a, b}) => a + '' + b;
foo({a: 'aa', b: 'bb'}); // "aabb"
```

3. 简化回调函数；

```JavaScript
[1, 2, 3].map(x => x * x);

const numbers = (...nums) => nums;
numbers(1, 2, 3, 4);
```

4. 箭头函数this对象就是定义时所在的对象，而不是使用时所在的对象；

```JavaScript
function Timer() {
    this.s1 = 0;
    this.s2 = 0;
    setInterval(() => this.s1 ++, 1000)
    setTinterval(function() {
        this.s2 ++;
    })
}

var timer = new Timer();
setTimeout(() => console.log(timer.s1), 3100);
setTimeout(() => console.log(timer.s2), 3100);
```

5. 箭头函数不能当做构造函数，即不能new；
6. 箭头函数不能使用arguments对象和yield命令；
7. 箭头函数绑定this，双冒号（::） -- 提案；
8. 尾调用不一定出现再函数尾部，只要在最后一步操作即可；

```JavaScript
function f(x) {
    return g(x);
}

function f(x) {
    return g(x) + 1;
}

function f(x) {
    let y = g(x);
    return y;
}

function f(x) {
    g(x);
}

function f(x) {
    if(x > 0) {
        return m(x);
    }
    return g(x);
}
```

9. 尾调用优化，尾递归优化，柯里化，严格模式，蹦床函数；

```JavaScript
function factorial(n) {
    if(n === 1) {
        return 1;
    }
    
    return n * factorial(n - 1);
}

factorial(5); // 120 (复杂度为O(n));

function factorial(n, total = 1) {
    if(n === 1) {
        return total;
    }
    
    return factorial(n - 1, n * total);
}

factorial(5); // 120 (复杂度为O(1))
```

# 数组的扩展
1. 扩展运算符（...），将一个数组变为参数序列；

```JavaScript
function sum(x, y) {
    return x + y;
}

let numbers = [1, 2];
sum(...numbers); // 3
// 替代apply方法
Math.max.apply(null, [1, 2, 3]);

Math.max(...[1, 2, 3]);

Array.prototype.push.apply([1, 2, 3], [3, 4, 5]);
[1, 2, 3].push(...[3, 4, 5]);
```

2. 扩展运算符的应用；

```JavaScript
// 合并数组
var arr1 = [1, 2, 3];
var arr2 = [4, 4, 5];
var arr3 = [7, 2];
[...arr1, ...arr2, ...arr3];

// 与解构赋值想结合
const [first, ...last] = [1, 2, 3, 4];
first;
last;

// 将字符串转为真正的数组
[...'string'];

// 将实现了Iterator接口的对象，转为数组
var nodelist = document.querySelectorAll('div');
[...nodelist];

// 实现了Iterator接口，如Map、Set结构和Generator函数，转为数组
var map = new Map([
    [1, 'one'],
    [2, 'two'],
    [3, 'three'],
])

[...map.keys()];
[...map.values()];

var go = function*() {
    yield 1;
    yield 2;
    yield 3;
};

[...go()];
```

3. Array.from()将类似数组的对象和可遍历对象转为数组；

```JavaScript
let arrayLike = {
    '0': 'a',
    '1': 'b',
    '2': 'c',
    length: 3
};

[].slice.call(arrayLike);
Array.from(arrayLike);

Array.from('string');
Array.from({ length: 3 });

Array.from({length: 2}, () => 'hello');
```

4. Array.of()将一组值转为数组；

```JavaScript
Array.of(1, 2, 3);
```

5. coprWithin(target, start = 0, end = this.length)

```JavaScript
[1, 2, 3, 4, 5].copyWithin(0, 3);
```

6. find()和findIndex();

```JavaScript
[1, 2, 3, 5].find((value, index, array) => value > 2); // 3
[1, 2, 3, 5].findIndex((value, index, array) => value > 2); // 2
```

7. fill();

```JavaScript
[1, 2, 3].fill(7); // [7, 7, 7]
[1, 2, 3, 4].fill(7, 1, 3); // [1, 7, 7, 4]
```

8. 数组遍历：entries()、keys()、values()；

```JavaScript
for(let key of [1, 2, 3].keys()) {
    console.log(key);
}

for(let val of [1, 2, 3].values()) {
    console.log(val);
}

for(let [a, b] of [1, 2, 3].entries()) {
    console.log([a, b]);
}
```

9. includes()与indexOf
10. 数组的空位；

```JavaScript
0 in [undefined, undefined]; // true
0 in [,,]; // false
Array.from([0,,1]); // [0, undefined, 1]
[...[0,,1]]; // [0, undefined, 1]
```
# 对象的扩展
## 属性的简洁表示法
1. 直接写入变量和函数作为对象的属性和方法；

```JavaScript
var str = "hello";
var obj = {str};
obj;
```

2. 对象中只写属性名，不写属性值；

```JavaScript
function f(x, y) {
    return {x, y};
}

f(1, 2);
```

3. 方法的简写；

```JavaScript
var o = {
    name: '张三',
    birth,
    method() {...} // 等价于'method': function() {...}
}
```

4. CommomJS模块输出变量的简洁写法；
5. 某个方法的值是一个Generator函数；

```JavaScript
var obj = {
    *g() {
        yield 1;
    }
}
```
## 属性名表达式

```JavaScript
var str1 = 'foo';
var obj1 = {x: 1};
var obj2 = {
    [str1]: 1,
    [obj1]: 2
}
obj2; // {foo: 1, [object, object]: 2}
var baz = {[str1]};
baz; // 报错
```
## 方法的name属性

```JavaScript
const obj = {
    get f() {},
    set f(x) {},
    foo() {}
}

obj.f.name; // 报错
obj.foo.name; // 'foo'

const descriptor = Object.getOwnPropertyDescriptor(obj, 'f');
descriptor.get.name; // 'get f'
descriptor.set.name; // 'set f'
obj.foo.bind().name // 'bound foo'
(new Function()).name; // 'anonymous'

const key1 = Symbol('description');
const key2 = Symbol();
let obj = {
    [key1]() {},
    [key2]() {}
}
obj[key1].name; // '[description]'
obj[key2].name; // ''
```
## Object.is()

```JavaScript
Object.is('1', '1');
Object.is({}, {});
-0 === +0;
Object.is(-0, +0);
NaN === NaN;
Object.is(NaN, NaN);
```
## Object.assign()
1. 基本用法；

```JavaScript
Object.assign({a: 1, b: 2}, {c: 3}, {d: 4});
Object.assign({a: 1, b: 2}, {b: 3}, {a: 4});
Object.assign({a: 1, b: 2});
Object.assign('abc');
Object.assign(null);
Object.assign({a: 1, b: 2}, null);
Object.assign({}, 'abc', 10, true);
Object.assign({a: 1}, 'abc', Object.defineProperty({}, 'say', {
    enumerable: false,
    value: 'hello'
}));
Object.assign({a: 1}, {[Symbol('b')]: 2});
```

2. 注意点；

```JavaScript
// 浅复制，复制得到的是对象的引用
var obj1 = {a: {b: 1}};
var obj2 = Object.assign({}, obj1);
obj2.a.b = 2;
obj2.a.b;

// 同名属性替换
Object.assign({a: 1, b: 2}, {b: 3}, {a: 4});

// 处理数组
Object.assign([1, 2, 3], [4, 5]);
```

3. 常见用途；

```JavaScript
// 对象添加属性
class Point {
    constructor(x, y) {
        Object.assign(this, {x, y});
    }
}

// 对象添加方法
Object.assign(someClass.prototype, {
    someMethod() {},
    anotherMethod() {}
})

// 克隆对象
function clone(origin) {
    let pt = Object.getPrototypeOf(origin);
    return Object.assign(Object.creat(pt), origin);
}

// 合并多个对象
const merge = ...sources => Object.assign({}, ...sources);

// 为属性添加默认值
const DEFAULT = {
    version: 1,
    type: 'css'
}

function(options) {
    options = Object.assign({}, DEFAULT, options);
}
```
## 属性的描述对象和可枚举性

```JavaScript
const obj = {foo: 123};
Object.getOwnPropertyDescriptor(obj, 'foo');
//{
//    value: '123',
//    writable: true,
//    enumerable: true,
//    configurable: true
//}

// 所有class原型的方法都是不可枚举的
Object.getOwnPropertyDescriptor(class{foo() {}}.prototype, 'foo').enumerable;
```
## 属性的遍历
1. for...in;
2. Object.keys(obj);
3. Object.getOwnPropertyNames(obj);
4. Object.getOwnPropertySymbols(obj);
5. Reflect.ownKeys(obj); // 返回数组，按数值（数字），字符串（时间），symbol（时间）排序
## 对象属性相关操作
1. 内部属性，__proto__，不推荐使用;
2. Object.setPrototypeOf(object, prototype);

```JavaScript
var obj = {x: 1, y: 2};
var proto = {};
Object.setPrototypeOf(obj, proto);
proto.z = 3;
obj.z;

Object.setPrototypeOf(1, {}) === 1;
Object.setPrototypeOf("foo", {}) === "foo";
Object.setPrototypeOf(true, {}) === true;
Object.setPrototypeOf(undefined, {});
Object.setPrototypeOf(null, {});
```

3. Object.getPrototypeOf(object);

```JavaScript
Object.getPrototypeOf(1);
Object.getPrototypeOf("foo");
Object.getPrototypeOf(true);
Object.getPrototypeOf(undefined);
Object.getPrototypeOf(null);
```

4. Object.keys()、Object.values()、Object.entries()

```JavaScript
var obj = Object.create({}, {p: {value: 1}});
Object.values(obj); // []
Object.values({[Symbol()]: 123, foo: 'aaa'}); // ['aaa']
Object.values('foo'); // ['f', 'o', 'o']

var obj = {x: 1, y: 2, z: 3};
for(let [k, v] of Object.entries(obj)) {
    console.log`${JSON.stringify(k)}:${JSON.stringify(v)}`;
}

// 将对象转为Map
var obj = {x: 1, y: 2};
var map = new Map(Object.entries(obj));
map;
```

## 对象的扩展运算符

```JavaScript
// 解构赋值
let {x, y, ...z} = {x: 1, y: 2, a: 1, b: 2};
x;
y;
z;

let {x, ...y} = undefined;
let {x, ...y} = null;
let {x, ...y, z} = obj;

let obj = {a: {b: 1}};
let {x, ...y} = {x: 1, obj};
obj.a.b = 2;
y.obj.a.b;

let a = {a: 1};
let b = {b: 2};
Object.setPrototypeOf(b, a);
let {...c} = b;
c.a;
c.b;

var o = Object.create({x: 1, y: 2});
o.z = 3;
var {x, ...{y, z}} = o;
x;
y;
z;

// 扩展运算符
let z = {a: 1, b: 2};
let c = {...z};
c; // 只复制对象实例的属性
// 等同于 Object.assign({}, a);
let d = Object.assign(Object.create(Object.getPrototypeOf(z)), a); // 复制对象实例的属性和对象原型的属性

let e = {...z, ...c, a: 11, b: 22};
{...{}, a: 1};
{...null, ...undefined};

let runtimeError = {
    ...a,
    ...{
        get x() {
            throw new Error('throw error');
        }
    }
} // 参数对象之中有取值函数get，这个函数将会执行
```
## 正确复制get和set属性，Object.getOwnPropertyDescriptors()

```JavaScript
const obj = {
    name: 'hello',
    get bar() {return 'abc'}
};
Object.getOwnPropertyDescriptor(obj, 'name');
Object.getOwnPropertyDescriptors(obj);

// 配合Object.defineProperties方法正确复制
const clone = (to, source) => Object.defineProperties(to, Object.getOwnPropertyDescriptors(source));

// 配合Object.create将对象属性克隆到另一个对象
const clone = obj => Object.create(Object.getPropertyOf(obj), Object.getOwnPropertyDescriptors(obj));
```
## 对象的继承方法

```JavaScript
const obj = {
    name: 'Ray',
    set sayHello() { console.log('hello') }
}
// Object.create
let obj1 = Object.create(obj);
// Object.assign
let obj2 = Object.assign({}, obj);
// Object.getOwnPropertyDescriptors
let obj3 = Object.getOwnPropertyDescriptors(obj);
```
## Null传导运算符（提案）

```JavaScript
const name = a && a.b && a.b.c && a.b.c.d || 'default';
// 可以写成
const name = a?.b?.c?.d || 'default';
```
