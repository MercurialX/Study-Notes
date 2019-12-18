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
1. 允许在块级作用域内声明变量；
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
## Object .is()

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

# Symbol
## Symbol概述
1. 一种新的原始数据类型Symbol，表示独一无二的值；七种数据类型分别是Undefined、Null、String、Number、Array、Object和Symbol；

```JavaScript
let s = Symbol();
typeof s; // 'Symbol'
```
2. 接受一个参数，表示对Symbol的描述，如果参数为对象就调用对象toString方法；

```JavaScript
var s1 = Symbol('foo');
var s2 = Symbol('foo');
s1; // Symbol(foo)
s1.toString(); // 'Symbol(foo)'
s1 === s2; // false
```

3. Symbol值不能参加运算；

```JavaScript
var s1 = Symbol('foo');
'Symbol is ' + s1;
`Symbol is $(s1)`;
```

4. Symbol值能转为字符串或布尔值，但不能转为数值；

```JavaScript
var s = Symbol('foo');
String(s); // 'Symbol(foo)'
Boolean(s); // true
Number(s); // 报错
!s; // false
```
## 作为属性名的Symbol
1. 写法；

```JavaScript
var mySymbol = Symbol();
// 写法一
var o = {};
o[mySymbol] = 1;

// 写法二
var o = {
    [mySymbol]: 1
}

// 写法三
var o = {};
o.defineProperty(o, [Symbol], {
    value: 1
})

// 使用点运算符？
o.mySymbol = 1;

let obj = {
    [mySymbol]() {...}
}
```
2. 消除魔术字符串，即在代码中多次出现、与代码形成强耦合的某一个具体的字符串或数值

```JavaScript
const shapeType = {
    triangle: Symbol()
}

function getArea(shape, options) {
    var area = 0;
    switch(shape) {
        case shapeType.triangle:
          area = .5 * options.width * options.height;
          break;
    }
    return area;
}

getArea(shapeType.triangle, {width: 100, height: 100});
```
3. 属性名的遍历，Object.getOwnPropertySymbols()和Reflects.ownKeys()；

```JavaScript
var a = Symbol('a');
var b = Symbol('b');
var obj = {
    [a]: 1,
    [b]: 2,
    c: 3
}

Object.getOwnPropertySymbols(obj);
Reflect.ownkeys(obj);
// 思考
for(let k in obj) {
    console.log(k);
}

Object.getOwnPropertyNames(obj);
```

## Symbol.for()、Symbol.keyFor()

```JavaScript
var s1 = Symbol.for('foo');
var s2 = Symbol.for('foo');
var s3 = Symbol('foo');
s1 === s2; // true
s2 === s3; // false
Symbol.keyFor(s1); // 'foo'
Symbol.keyFor(s3); // undefined
```
## 模块的Singleton模式
## 内置的Symbol值
1. instanceof和Symbol.hasInstance;

```JavaScript
class Myclass {
    [Symbol.hasInstance](foo) {
        return foo instanceof Array;
    }
}
```

2. Symbol.isConcatSpreadable;

```JavaScript
let obj = {length: 2, 0: 'e', 1: 'f'};
['a', 'b'].concat(obj, 'c', 'd');
obj[Symbol.isConcatSpreadable] = true;
['a', 'b'].concat(obj, 'c', 'd');
```

3. instanceof和Symbol.species;

```JavaScript
class MyArray extends Array {
    static get [Symbol.species]() {
        return Array;
    }
}

var a = new MyArray(1, 2, 3);
var maped = a.map(x => x * x);
maped instanceof MyArray; // false
maped instanceof Array; // true
```

4. match和Symbol.match;

```JavaScript
String.prototype.match(regexp);
regexp[Symbol.match](this);
```

5. replace和Symbol.replace;

```JavaScript
String.prototype.replace(searchValue, replaceValue);
searchValue[Symbol.replace](this, replaceValue);
```

6. search和Symbol.search;

```JavaScript
String.prototype.search(regexp);
regexp[Symbol.search](this);
```

7. Symbol.split;

```JavaScript
String.prototype.split(separator, limit);
separator[Symbol.split](this, limit);
```

8. 默认遍历器的方法Symbol.iterator;

```JavaScript
var myIterator = {};
myIterator[Symbol.iterator] = function* () {
    yield 1;
    yield 2;
    yield 3;
}
[...myIterator]; // [1, 2, 3]
```

9. 对象被转为原始类型的值时返回该对象对应的原始类型值Symbol.toPrimitive;

```JavaScript
let obj = {
    [Symbol.toPrimitive](hint) {
        switch (hint) {
            case 'number':
                return 123;
            case 'string':
                return 'str';
            case 'default':
                return 'default'
            default:
                throw new Error();
        }
    }
};

2 * obj; // '246'
2 + obj; // '2default'
obj == 'default'; // true
String(obj); // 'str'
```

10. 返回值会出现再toString的字符串中Symbol.toStringTag;

```JavaScript
({[Symbol.toStringTag]: 'Foo'}).toString(); // "[object Foo]"
```

11. 使用with时哪些属性会被with环境排除：Symbol.unscopables;

```JavaScript
Object.keys(Array.prototype[Symbol.unscopables]; 
// ["copyWithin", "entries", "fill", "find", "findIndex", "includes", "keys", "values"]
```

# Set和Map数据结构
## Set
1. 基本用法；

```JavaScript
// 声明和赋值
const s = new Set();
[1, 2, 3, 1, 1].forEach(x => s.add(x));
const set = new Set([1, 2, 3, 1, 1]);
[...s]; // [1, 2, 3]
[...set]; // [1, 2, 3]

// 向Set传值不会发生类型转换
let s = new Set();
let a = NaN;
let b = NaN;
s.add(a);
s.add(b);
[...s]; // [NaN]

// 两个对象总是不相等
let set = new Set();
set.add({});
set.size;
set.add({});
set.size;
```

2. Set实例的属性和方法；

```JavaScript
var s = new Set();
s.add(1).add(2); // Set(2) {1, 2}
s.delete(1); // true
s.has(1); // true
s.clear(); // undefined
s; // Set(0) {}

// 使用Array.from将Set转为数组，去重
Array.from(new Set([1, 2, 3, 1, 2, 1])); // [1, 2, 3]
```

3. Set遍历；

```JavaScript
var set = new Set(['a', 'b', 'c']);
[...set];

for(let k of set.keys()) {
    console.log(k);
}

for(let v of set.values()) {
    console.log(v)
}

for(let item of set.entries()) {
    console.log(item);
}

set.forEach((k, v) => console.log(k));
[...set].map(x => console.log(x));
[...set].filter(x => x === 'a');

// 交集、并集、差集
let a = new Set([1, 2, 3]);
let b = new Set([1, 4, 5]);
[...a].filter(x => b.has(x));
new Set([...a, ...b]);
[...a].filter(x => !b.has(x));
```

4. WeakSet, 成员只能是对象，成员是弱引用，不计入垃圾回收机制，无法遍历没有size;

```JavaScript
var ws = new WeakSet();
ws.add(1);
ws.add([1]);
ws.add([2]);
ws.delete([1]);
ws.clear();
const a = [[1], [2]];
const b = [1, 2];
const ws = new WeakSet(a);
const ws = new WeakSet(b);
```
## Map：为了解决Object只能用字符串作为键,值-值
1. Map的含义与基本用法；

```JavaScript
const map = new Map();
const o = {say: 'hello'};
map.set(o, 'content');
map.get(o);
map.has(o);
map.delete(o);
map.has(o);

const m = new Map(['name', '张三'], ['age', 18]);
m.size;
// 多次赋值、读取未知的键
const map = new Map();
map.set(1, 'aaa').set(1, 'bbb');
map.get('dddd');
// 与内存地址的联系
const map = new Map();
map.set(['a'], 111).set(['a'], 222);
map.get(['a']); // undefined
map.size;
// 键是简单类型的值
const map = new Map();
map.set(+0, 'aaa');
map.get(-0);
map.set(true, 1).set(true, 2);
map.set(undefined, 1).set(NaN, 2).set(null, 3).set(null, 4);
map.clear();
```

2. Map的属性与方法:set、get、size、delete、has、clear()；
3. Map的遍历：keys()、values()、entries()、forEach((k, v, m) => ...)；
4. Map与其他类型的转换；

```JavaScript
// Map与数组相互转换
const map = new Map([[1, 2], [3, 4]]);
[...map];
// Map与对象相互转换
// 键为字符串的Map
function mapToObj(map) {
    let obj = Object.create(null);
    for(let [k, v] of map) {
        obj[k] = v;
    }
    
    return obj;
}

function objToMap(object) {
    let map = new Map();
    for(let k of Object.keys(object)) {
        map.set(k, object[k]);
    }
    
    return map;
}
// Map与Json相互转换
// map键为string转为字符串JSON
function mapToStrJSON(map) {
    return JSON.stringify(mapToObj(map));
}
// map键含其他类型转为数组JSON
function mapToJSON(map) {
    return JSON.parse([...map]);
}
// JSON转map
function strJsontoMap(json) {
    return objToMap(JSON.parse(json));
}
// JSON成员本身又具有两个成员的数值转map
function jsontoMap(json) {
    return new Map(JSON.parse(json))
}
```

5. WeakMap：只接受对象作为键名，键名所指向的对象不计入垃圾回收机制，不是键值

```JavaScript
// 以DOM节点作为键名
let elememt = document.getElementById('demo');
let wm = new WeakMap();
wm.set(element, {times: 0});
element.addEventListener('click', function() {
    let times = wm.get(element);
    times.times ++;
}, false)
// 注册监听事件的listener对象为键名
const listener = new WeakMap();
listener.set(ele1, handler1);
listener.set(ele2, handler2);
ele1.addEventListener('click', listener.get(ele1), false);
ele2.addEventListener('click', listener.get(ele2), false);
// 部署私有属性
const _counter = new WeakMap();
const _action = new WeakMap();

class Countdown {
    constructor(counter, action) {
        _counter.set(this, counter);
        _action.set(this, action);
    }
    
    dec() {
        let counter = _counter.get(this);
        console.log(counter);
        if(counter < 1) {
            _action.get(this)();
            return;
        }
        
        counter --;
        _counter.set(this, counter);
    }
}

const c = new Countdown(2, () => console.log('DONE'));
c.dec();
c.dec();
```
# Proxy
## 概述
1. 用于修改某些操作的默认行为，属于一种“元编程”，即对编程语言进行编程；
```JavaScript
var proxy = new Proxy(target, handler);
// target：所要代理的目标对象；handler：配置对象，提供对应的处理函数
```
2. 要使Proxy起作用必须针对Proxy实例进行操作，而不是针对目标对象；
3. 没有设置handler，等同于直接通向原对象；

```JavaScript
var target = {};
var handler = {};
var proxy = new Proxy(target, handler);
proxy.a = '111';
target.a;
```
4. 将Proxy设置到object.proxy属性，从而可以在object上调用；

```JavaScript
var object = {proxy: new Proxy(target, handler)};
// Proxy实例作为其他对象原型
var proxy = new Proxy({}, {
    get: (target, property) => 35
})
let obj = Object.create(proxy);
obj.time;
```
5. 同一个拦截器设置多个操作；
## Proxy实例的方法
1.get(object, name, receiver);

```JavaScript
var person = {
    name: '张三'
}
var handler = {
    get: (target, property) => {
        if(property in target) {
            return target[property];
        }else {
            throw new Error('没有属性');
        }
    }
}
var proxy = new Proxy(person, handler);
proxy.name;
proxy.age;

// get方法继承
var obj = Object.create(proxy);
obj.name;
obj.age;

// 实现属性的链式操作
var pipe = (function() {
   return function(value) {
       var funcStack = [];
       var proxy = new Proxy({}, {
           get: (target, property) => {
               if(property === 'get') {
                   return funcStack.reduce((val, fn) => fn(val), value);
               }
               funcStack.push(window[property]);
               return proxy;
           }
       });
       
       return proxy;
   } 
}());

var double = x => x * 2;
var pow = x => x * x;
var reverseInt = x => x.toString().split('').reverse().join('') | 0;

pipe(3).double.pow.reverseInt.get;

// 实现一个生成各种dom节点的通用函数dom
const dom = new Proxy({}, {
    get(target, property) {
        return (attrs = {}, ...children) => {
            const el = document.createElement(property);
            for(let prop of Object.keys(attrs)) {
                el.setAttribute(prop, attrs[prop]);
            }
            for(let child of children) {
                if(typeof child === 'string') {
                    child = document.createTextNode(child);
                }
                el.appendChild(child);
            }
            return el;
        }
    }
});

const el = dom.div({}, 
    'Hello, my name is',
    dom.a({href: '/'}, 'Mark'),
    ',I like:',
    dom.ul({},
        dom.li({}, 'eat'),
        dom.li({}, 'music'),
        dom.li({}, '...')
    )
)
document.body.appendChild(el);

// 如果一个属性不可配置或不可写，使用get()会报错
const obj = Object.defineProperties({}, {
    name: {
        value: '张三',
        configurable: false,
        writable: false
    }
});
const proxy = new Proxy(obj, {
    get(terget, property) {
        return 'aaa';
    }
})
proxy.name;
```
2. set(target, key, value);
3. apply(target, ctx, args);

```JavaScript
var p = () => 'function';
var proxy = new Proxy(p, {
    apply() {
        return 'proxy';
    }
});
proxy();

// 两个数的和再翻倍
var sum = (a, b) => a + b;
var twice = {
    apply(target, ctx, args) {
        return Reflect.apply(...arguments) * 2;
    }
}
var proxy = new Proxy(sum, twice);
proxy(1, 2);
proxy.call(null, 2, 3);
proxy.apply(null, [5, 5]);
Reflect.apply(proxy, null, [9, 10]);
```

4. has(target, key);

```JavaScript
var obj = {
    name: '张三',
    _name: 'aaa'
};
var proxy = new Proxy(obj, {
  has(target, key) {
      if(key[0] === '_') {
          return false;
      }
      
      return target[key];
  }  
});

'name' in proxy;
'_name' in proxy;

// 原对象是不可配置或禁止扩展，has拦截会报错
var obj = { name: 1 };
Object.preventExtensions(obj);

// for...in依旧有效
for(let k in proxy) {
    console.log(k);
}
```
5. construct(target, args);

```JavaScript
var proxy = new Proxy(function() {}, {
    construct(target, args) {
        return {value: [...args]};
    }
});
(new proxy(1, 2)).value;

// construct必须返回对象否则报错
var p = new Proxy(function() {}, {
    construct(target, args) {
        return 1;
    }
})
new p();
```

6. deleteProperty(target, key); // 目标对象设置了不可配置的属性不能使用
7. defineProperty(target, key, descriptor); // extensible报错
8. getOwnerPropertyDescriptor(target, key);
9. getPrototypeOf(target); // 返回值必须是对象或null，extensible返回目标对象原型
10. setPropertyOf(target, proto); // 返回值必须是布尔值，extensible不能改变目标对象原型
11. isExtensible(target); // 返回值必须是布尔值且与isExtensible一致
12. preventExtensions(target); // 返回值是布尔值且目标对象isExtensible才能返回true
13. ownKeys(target);

```JavaScript
var obj = {
    a: '1',
    _a: '111',
    b: '2',
    _b: '222'
}
var proxy = new Proxy(obj, {
    ownKeys(target) {
        return Reflect.ownKeys(target).filter(key => key[0] === '_'); 
    }
})

for(let key of Object.keys(proxy)) {
    console.log(obj[key]);
}
// 自动过滤：不存在属性、属性名为Symbol值、不可遍历（enumable）的属性
let obj = {
    a: 'aa',
    b: 'bb',
    [Symbol.for('secret')]: 'cc'
}
Object.defineProperty(obj, 'c', {
    enumable: false,
    writable: true,
    configurable: true,
    value: 'cc'
})
var proxy = new Proxy(obj, {
    ownKeys(target) {
        return Reflect.ownKeys(target);
    }
})
Object.keys(proxy); // ['a', 'b']
Object.getOwnPropertyNames(proxy); // ['a', 'b', 'c']

// 返回的数值成员只能是字符串或Symbol，否则报错
Object.getOwnPropertyNames(new Proxy({}, {
    ownKeys(target) {
        return [0, null, undefined];
    }
}));

// 目标对象的不可配置属性必须被返回
var obj = {a: 'aaa'};
Object.defineProperty(obj, 'key', {
    configurable: false,
    enumerable: true,
    value: 'bbb'
});
Object.getOwnPropertyNames(new Proxy(obj, {
    ownKeys(target) {
        return ['a'];
    }
}));

// 目标对象是不可扩展的（non-extensible），必须返回所有属性且不能包含多余属性
var obj = {a: 1};
Object.preventExtensions(obj);
Object.getOwnPropertyNames(new Proxy(obj, {
    ownKeys(target) {
        return ['a', 'n'];
    }
}));
```
## Proxy.revocable()
返回：一个可取消的Proxy实例；
使用场景：目标对象不允许直接访问，必须通过代理访问，一旦访问结束，就收回代理权，不允许再次访问；

```JavaScript
let target = {};
let handler = {};
let {proxy, revoke} = Proxy.revocable(target, handler);
proxy.foo = 111;
proxy.foo;
revoke();
proxy.foo;
```
## this问题

```JavaScript
// 在Proxy代理的情况下，目标对象内部的this会指向Proxy
const target = {
    m() {
        console.log(this === proxy)
    }
}
const handler = {};
const proxy = new Proxy(target, handler);

target.m();
proxy.m();

const _name = new WeakMap();
class Person {
    constructor(name) {
        _name.set(this, name);
    }
    get name() {
        return _name.get(this);
    }
}

const jane = new Person('jane');
jane.name;
const proxy = new Proxy(jane, {});
proxy.name;

// 有些原生对象的内部属性只有通过this才能获取，因此不能使用Proxy代理
const target = new Date();
const handler = {};
const proxy = new Proxy(target, handler);
proxy.getDate();

const p = new Proxy(target, {
    get(target, property) {
        if(property === 'getDate') {
            return target.property.bind(target);
        }
        return Reflect.get(target, property);
    }
})
```
## web客户端

```JavaScript
function createWebService(baseUrl) {
    return new Proxy({}, {
        get(target, prop, receiver) {
            return () => httpGet(baseUrl + '/' + prop);
        }
    })
}
```
# Reflect
## 概述
1. 将Object对象上一些明显属于语言内部的方法（如Object.defineProperty）放到Reflect上；
2. 修改某些Object方法的返回结果，让其更加合理，比如Object.defineProperty在无法定义时抛出错误，而Reflect.defineProperty则会返回false；
3. 让Object操作变为函数行为，如in变成Reflect.has()；
4. Reflect对象的方法与Proxy方法一一对应。
## 静态方法
1. Reflect.get(target, name, receiver);

```JavaScript
var obj = {
    foo: 1,
    bar: 2,
    get baz() {
        return this.foo + this.bar
    }
}

Reflect.get(obj, 'baz');
Reflect.get(obj, 'baz', {
    foo: 10,
    bar: 1
});

// 第一个参数不是对象报错
Reflect.get(1, 'a');
```

2. Reflect.set(target, name, value, receiver);

```JavaScript
var obj = {
    foo: 1,
    set bar(value) {
        return this.foo = value;
    }
}
var receiver = {
    foo: 2
}
obj.foo;
Reflect.set(obj, 'bar', 3);
Reflect.set(obj, 'bar', 4, receiver);
obj.foo;
receiver.foo;

// 第一个参数不是对象报错
Reflect.set(1, 'a', {});

// Proxy.set会触发Proxy.defineProperty拦截
```
3. Reflect.has(target, key);

```JavaScript
var obj = {name: '张三'};
// 旧写法
name in obj;
// 新写法
Reflect.has(obj, name);
```

4. Reflect.deleteProperty(target, key);

```JavaScript
var obj = {name: '张三'};
// 旧写法
delete obj.name;
// 新写法
Reflect.deleteProperty(obj, name);
```
5. Reflect.construct(target, args);

```JavaScript
function Person(name) {
    this.name = name;
}
// new
const p = new Person('张三');
// 不用new调用构造函数
const person = Reflect.construct(Person, ['张三']);
```
6.Reflect.getPrototypeOf(target);

```JavaScript
const obj = new Person('张三');
// 旧写法
Object.getPrototypeOf(obj) === Person.prototype;
// 新写法
Reflect.getPrototypeOf(obj) === Person.prototype;
// 区别：参数不是对象前者转为对象再运行，后者报错
Object.getPrototypeOf(1);
Reflect.getPrototypeOf(1);
```
7. Reflect.setPrototypeOf(target, newproto);

```JavaScript
const obj = new Person();
// 旧写法
Object.setPrototypeOf(obj, OtherPerson);
// 新写法
Object.setPrototypeOf(obj, OtherPerson);
// 区别：第一个参数不是对象前者会返回参数本身，后者会报错。
Object.setPrototypeOf(1, {});
Reflect.setPrototypeOf(1, {});
//第一个参数是undefined或null，两者都会报错
Object.setPrototypeOf(null, {});
Reflect.setPrototypeOf(null, {});
```

8. Reflect.apply(func, thisArg, args);

```JavaScript
const ages = [11, 33, 42, 1, 2, 92];
// 旧写法
const youngest = Math.min.apply(Math, ages);
const type = Object.prototype.toString.call(youngest);
// 新写法
const youngest = Reflect.apply(Math.min, Math, ages);
const type = Reflect.apply(Object.prototype.toString, youngest, []);
```

9. Reflect.defineProperty(target, key, descriptor);
10. Reflect.getOwnPropertyDescriptor(target, key);
11. Reflect.isExtensible(target);
12. Reflect.preventExtensions(target);
13. Reflect.ownKeys(target);

```JavaScript
// 等于Object.getOwnPropertyNames与Object.getOwnPropertySymbols之和
var obj = {
    a: '1',
    b: '2',
    [Symbol.for('foo')]: '3',
    [Symbol.for('baz')]: '4'
}

Object.getOwnPropertyNames(obj);
Object.getOwnPropertySymbols(obj);
Reflect.ownKeys(obj);
```
## 观察者模式

```JavaScript
const queuedObservers = new Set();
const observe = fn => queuedObservers.add(fn);
const observable = obj => new Proxy(obj, {set});

function set(target, key, value, receiver) {
    const result = Reflect.set(target, key, value, receiver);
    queuedObservers.forEach(observer => observer());
    return result;
}

const person = {
    name: '张三',
    age: 18
}

function print() {
    console.log`${person.name}, ${person.age}`;
}

observe(print);
person.name = '李四';
```
# Promise对象
## Promise含义
1. 所谓Promise，简单来说就是一个容器，保存着某个未来才会结束的事件的结果；
2. 对象的状态不受外界影响；
3. 一旦状态改变就不会再变，任何时候都可以得到这个结果；
4. 缺点是无法取消Promise，当处于Pending状态时，无法得知具体进展；

## 基本用法

```JavaScript
// Promise立即执行
let promise = new Promise(function(resolve, reject) {
    console.log('Promise');
    resolve();
});
promise.then(function() {
    console.log('Resolve');
});
console.log('hi');

// Promise作为resolve的参数
let p1 = new Promise(function(resolve, rejcet) {
   console.log('p1');
   resolve();
});
let p2 = new Promise(function(resolve, reject) {
    console.log('p2');
    resolve(p1);
});
p2
    .then(res => console.log(res))
    .catch(err => console.log(err));

// 调用resolve或reject不会终结Promise的执行
new Promise(function(resolve, reject){
    resolve(1);
    console.log(2);
}).then(res => {
    console.log(res);
});
```

## Promise.prototype.then()

```JavaScript
// then返回的是一个新的Promise实例，不是原来那个，所以可以使用链式写法
getJSON(url).then(
    res => res,
    err => err
).then(
    res => res,
    err => err
);
```
## Promise.prototype.catch()

```JavaScript
// catch捕获异常
getJSON(url).then(function(posts) {
    ...
}).catch(function(error) {
    ...
})

// 如果Promise状态时Resolved，再抛出异常无效
var promise = new Promise(function(resolve, reject) {
    resolve('ok');
    throw new Error('错误');
});
promise.then(val => console.log(val)).catch(err => console.log(err));

// Promise错误具有“冒泡”特性，应使用catch来处理异常
// catch返回的是一个新的Promise对象，可以使用链式写法
// catch方法抛出的错误，如果后面没有catch将导致错误无法捕获，也无法传递到外层
```
## Promise.all()

```JavaScript
// 将多个Promise包装成一个Promise
var p = Promise.all([p1, p2, p3]);

// 所有子Promis都是Fulfilled，状态才是Fulfilled，否则返回第一个rejected子Promise

// 子Promise自身定义了catch方法，那么它被rejected时并不会触发Promise.all()的catch方法
var p1 = new Promise(function(resolve, reject) {
   resolve('p1 ok'); 
})
.then(res => res)
.catch(err => err);
var p2 = new Promise(function(resolve, reject) {
    throw new Error('error');
})
.then(res => res)
.catch(err => err);

Promise.all([p1, p2]).then(res => console.log(res)).catch(err => console.log(err));

// 子Promise没有自己的catch就会调用Promise.all()的catch
```
## Promise.race()
同样是将多个Promise实例包装成一个新的Promise实例；子Promise只要有一个率先改变状态，Promise.race的状态就跟着改变，率先改变的示例的返回值就会传递给Promise.race的回调函数。
```JavaScript
var promise = Promise.race([p1, p2, p3]);
```
## Promise.resolve()
将对象转为Promise对象：

```JavaScript
// 参数是一个Promise实例，直接返回这个实例

// 参数是一个thenable对象，会将这个对象转为Promise，执行then
let obj = {
    then(resolve, reject) {
        resolve('ok');
    }
}
let p = Promise.resolve(obj);
p.then(val => console.log(val));

// 参数不具有thenable方法的对象或根本不是对象，返回新的Promise，状态为resolver
let p = Promise.resolve("hello");
p.then(s => console.log(s));

// 不带任何参数，返回一个Resolved状态的Promise
let p = Promise.resolve();
setTimeout(() => console.log(1), 0); // 下轮事件执行
p.then(res => console.log(2)); // 本轮事件执行
console.log(3); // 立即执行
```
## Promise.reject()
返回一个新的Promise实例，状态为rejected
## 附加方法
1. done();

```JavaScript
Promise.prototype.done = function(onFilfilled, onRejected) {
    this.then(onFilfilled, onRejected)
    .catch(function(reason) {
        setTimeout(() => throw reason, 0);
    })
}
asyncFunc().then(f1).then(f2).done();
// 位于回调链的尾端，保证抛出任何可能出现的错误
```

2. finally();

```JavaScript
Promise.proto.finally = function(callback) {
    let p = this.constructor;
    return this.then(
        value => P.resolve(callback()).then(() => value),
        reason => P.resolve(callback()).then(() => {throw reason})
    );
};
```
## Promise.try()（提案）

```JavaScript
// 同步函数会在本轮事件循环的末尾执行
const f = () => console.log('now');
Promise.resolve().then(f);
console.log('later');

// 使用async函数
const f = () => console.log('now');
(async () => f())();
console.log('later');
// async会吃掉f()抛出的异常，需要重新捕获
(async () => f())().then(...).catch(...);

// 使用new Promise()
const f = () => console.log('now');
(
    () => new Promise(
        resolve => resolve(f())
    )
)()
console.log('later');

// try
const f = () => console.log('now');
Promise.try(f);
console.log('later');
```

# 遍历器Itreator和for...of循环
## Iterator概念
1. js表示“集合”的数据结构有：数组、对象、Map、Set；
2. 遍历器：一种接口，为各种不同的数据结构提供统一的访问机制；
3. Iterator作用：a.为各种数据结构提供一个统一的、简便的访问接口；b.使数据结构的成员能够按照某种次序排序；c.供for...of消费；
4. Iterator遍历过程：a.创建一个指针对象，指向数据结构的起始位置；b.不断调用next方法，直到它指向数据结构的结束位置；

```JavaScript
var it = idMaker();
it.next().value;
it.next().value;
it.next().value;

function idMaker() {
    var index = 0;
    return {
        next() {
            return {value: index++, done: false};
        }
    }
}
```
## 默认Iterator接口
1. 默认的Iterator接口部署在数据结构的Symbol.iterator属性；

```JavaScript
const obj = {
    [Symbol.iterator]: function() {
        return {
            next() {
                return {value: 1, done: false}
            }
        }
    }
}
```

2. 原生具有Iterator接口的数据结构：Array、Map、Set、String、TypedArray、函数的arguments对象、NodeList对象；

```JavaScript
let arr = ['a', 'b', 'c'];
let iter = arr[Symbol.iterator]();
iter.next();
iter.next();
iter.next();
iter.next();
```

3. 类部署Iterator；

```JavaScript
class RangeIterator {
    constructor(start, stop) {
        this.value = start;
        this.stop = stop;
    }
    
    [Symbol.iterator]() {
        return this;
    }
    
    next() {
        let value = this.value;
        if(value < this.stop) {
            this.value++;
            return {value: value, done: false};
        }
        return {value: undefined, done: true};
    }
}

function range(start, stop) {
    return new RangeIterator(start, stop);
}

for(let v of range(0, 3)) {
    console.log(v);
}
```

4. 遍历器实现指针结构；

```JavaScript
function Obj(value) {
    this.value = value;
    this.next = null;
}

Obj.prototype[Symbol.iterator] = function() {
    var iterator = {next: next};
    var current = this;
    function next() {
        if(current) {
            var value = current.value;
            current = current.next;
            return {done: false, value: value};
        }else {
            return {done: true};
        }
    }
    return iterator;
}

var one = new Obj(1);
var two = new Obj(2);
var three = new Obj(3);

one.next() = two;
two.next() = three;

for(let i of one) {
    console.log(i);
}
```

5. 对象添加Iterator接口；

```JavaScript
let obj = {
    data: ['hello', 'world'],
    [Symbol.iterator]() {
        const self = this;
        let index = 0;
        return {
            next() {
                if(index < self.data.length) {
                    return {
                        value: self.data[index++],
                        done: false
                    };
                }
                return {value: undefined, done: true};
            }
        }
    }
}
```

6. 类似数组（存在数值键名和length属性）部署Iterator接口；

```JavaScript
let iterable = {
    length: 2,
    0: 'aaa',
    1: 'bbb',
    [Symbol.iterator]: Array.prototype[Symbol.iterator]
}
for(let v of iterable) {
    console.log(v);
}
```
## 调用Iterator接口的场合
1. 解构赋值；

```JavaScript
let s = new Set().add('a').add('b').add('c');
let [x, y] = s;
let [a, ...b] = s;
```

2. 扩展运算符；

```JavaScript
var str = 'hello';
[...str];
var arr = ['a', 'b'];
['c', ...arr, 'd'];
```

3. yield*；
4. 任何接受数组作为参数的场合：for...of、Array.form()、Map()、Set()、WeakMap()、WeakSet()、Promise.all()、Promise.race()；
## 字符串的Iterator接口

```JavaScript
var str = 'hello';
typeof str[Symbol.iterator];

var it = str[Symbol.iterator]();
it.next();

// 重写Symbol.iterator
str[Symbol.iterator] = function() {
    return {
        next() {
            if(this._first) {
                this._first = false;
                return {value: 'bye', done: false};
            }
            return {done: true};
        },
        _first: true
    };
};

[...str];
str;
```
## Iterator接口与Generator函数

```JavaScript
let obj = {
    *[Symbol.iterator] () {
        yield 'hello';
        yield 'world';
    }
};
[...obj];
```
## 遍历器对象的return()、throw()
next方法必须部署，return和throw方法可选部署
## for...of循环

```JavaScript
// 数组
var arr = ['a', 'b', 'c'];
for(let k of arr) {
    console.log(k);
}

// Set和Map结构
var s = new Set().add('a').add('b').add('c');
var m = new Map().set('a', 1).set('b', 2).set('c', 3);
for(let k of s) {
    console.log(s);
}
for(let map of m) {
    console.log(map);
}

// entries()、keys()、values()
var arr = ['a', 'b', 'c'];
for(let k of arr.entries()) {
    console.log(k);
}

// 类似数组
let arrayLike = {
    length: 2,
    0: 'a',
    1: 'b'
};
for(let k of arrayLike) {
    console.log(k); // 报错
}
for(let k of Array.from(arrayLike)) {
    console.log(k);
}

// 普通对象
let obj = {
    name: '张三',
    age: 18
};
for(let k of Object.keys(obj)) {
    console.log(obj[k]);
}

```
遍历方法有:  
for -- 麻烦  
forEach -- 无法break或return  
for...in -- 数组键名是数字，循环使用的是字符串，会遍历其他键名，顺序不定  
for...of  

# Generator函数
## 简介

```JavaScript
function* helloWorldGenerator() {
    console.log('1');
    yield 'hello';
    console.log('2');
    yield 'world';
    console.log('3');
    return 'ending';
    console.log('4');
}

var hw = helloWorldGenerator();
hw.next(); // 1 {value: 'hello', done: false}
hw.next(); // 2 {value: 'world', done: false}
hw.next(); // 3 {value: 'ending', done: true}
hw.next(); // {value: undefined, done: true}

hw[Symbol.iterator]() === hw; // true

// 惰性求值
function* sum() {
    yield 222 + 333;
}
sum(); // 不求值
sum().next(); // {value: 555, done: false}

// yield只能用在Generator函数中
var arr = [1, 2, 3, 4, 5];
var flat = function* (a) {
    a.forEach(item => {
        yield* flat(item);
    });
};

[...flat(arr)]; // 报错

// yield用在另一个表达式中，必须放在圆括号内
function* demo() {
    console.log('hello' + yield);
    console.log('hello' + (yield));
}

// yield用作函数参数或放在赋值表达式的右边可以不加括号
function* demo() {
    foo(yield 'a', yield 'b');
    let input = yield;
}

// 与Iterator关系
var obj = {};
obj[Symbol.iterator] = function* () {
  yield 1;
  yield 2;
  yield 3;
};

[...obj];

```
## next方法的参数
next方法的参数表示上一条yield语句的返回值，所以第一次使用next传参无效。
```JavaScript
function* foo(x) {
    var y = 2 * (yield (x + 1));
    var z = yield(y / 3);
    return (x + y + z);
}

var a = foo(5);
a.next(); // {value: 6, done: false}
a.next(); // {value: NaN, done: false}
a.next(); // {value: NaN, done: true}

var b = foo(5);
b.next(); // {value: 6, done: false}
b.next(12); // {value: 8, done: false}
b.next(9); // {value: 38, done: false}
```
## for...of

```JavaScript
function* foo() {
    yield 1;
    yield 2;
    yield 3;
    return 4;
}
for(let v of foo) {
    console.log(v); // 1, 2, 3
}
[...foo()]; // [1, 2, 3]
Array.from(foo()); // [1, 2, 3]
let [x, y] = foo();
```
## Generator.prototype.throw()

```JavaScript
function* g() {
    try{
        yield 1;
    }catch(e) {
        console.log('内部捕获' + e);
    }
};

var i = g();
i.next();
try{
    i.throw('a');
    i.throw('b');
    i.throw('c');
}catch(e) {
    console.log('外部捕获' + e);
}
// 内部捕获a
// 外部捕获b

i.throw(new Error('出错了'));

// 如果Generator函数部署了try...catch，那么遍历器throw方法抛出错误不影响下一次遍历
function *gen() {
    yield 1;
    yield 2;
}

var f = gen();
f.next(); // {value: 1, done: false}
f.throw(); // Uncaught undefined
f.next(); // {value: undefined, done: true}
```
## Generator.prototype.return()
返回给定的值，终结Generator函数遍历。

```JavaScript
function* f() {
    yield 1;
    yield 2;
    yield 3;
}
var g = f();
g.next(); // {value: 1, done: false}
g.return(); // {value: undefined, done: true}
g.next(); // {value: undefined, done: true}

// 结合finally, return会推迟到finally之后执行
function* foo() {
    try{
        yield 1;
        yield 2;
    } finally {
        yield 3;
        yield 4;
    }
    
}
var f = foo();
f.next(); // {value: 1, done: false}
f.return('a'); // {value: 3, done: false}
f.next(); // {value: 4, done: false}
f.next(); // {value: 'a', done: true}
```
## yield*表达式
Generator函数内部调用另一个Generator函数

```JavaScript
function* bar() {
    yield 1;
    yield 2;
}
function* foo() {
    yield 1;
    bar();
    yield bar();
    yield* bar();
    yield 2;
}
[...foo()];

// 只要是有Iterator接口的数据结构，就能被yield*遍历
function* foo() {
    yield 'aaa';
    yield* 'bbb';
}
[...foo()]; // ['aaa', 'b', 'b', 'b']

// 被代理的Generator使用return，向代理它的Generator函数返回数据
function* bar() {
    yield 1;
    yield 2;
    return 'aaa';
}
function* foo() {
    yield 1;
    var str = yield* bar();
    yield 2;
    console.log(str);
}
[...foo()];

// 取出嵌套数组的所有成员
function* iterTree(tree) {
    if(Array.isArray(tree)) {
        for(let i = 0; i < tree.length; i++) {
            yield* iterTree(tree[i]);
        }
    }else {
        yield tree;
    }
}

const tree = ['a', ['b', 'c'], ['d', 'e', ['f']]];
[...iterTree(tree)]; // ['a', 'b', 'c', 'd', 'e', 'f']

// 遍历完全二叉树
function Tree(left, label, right) {
    this.left = left;
    this.label = label;
    this.right = right;
}

function* inorder(t) {
    if(t) {
        yield* inorder(t.left);
        yield t.label;
        yield* inorder(t.right);
    }
}

function make(tree) {
    if(tree.length === 1) {
        return new Tree(null, tree[0], null);
    }
    return new Tree(make(tree[0]), tree[1], make(tree[2]));
}

let tree = make([['a'], 'b', ['c'], [['d']], ['e']]);
var result = [];
for(let v of inorder(tree)) {
    result.push(v);
}
result;
```
## 作为对象属性的Generator函数

```JavaScript
var obj = {
    *g() {...}
};
```
## Generator函数与this

```JavaScript
// Generator函数总是返回遍历器对象，而不是this
function* f() {
    this.a = 11;
}
let f = f();
f.a; // undefined

// Generator函数不能使用new
function* F() {
    this.x = 2;
}
new F();

// 改造，使其能获取this属性，也能new
function* f() {
    this.a = 1;
    yield this.b = 2;
    yield this.c = 3;
}

function F() {
    return f.call(f.prototype);
}

var f = new F();
f.next(); // {value: 2, done: false}
f.next(); // {value: 3, done: false}
f.next(); // {value: undefined, done: true}
f.a; // 1
f.b; // 2
f.c; // 3
```
## Generator状态机

```JavaScript
function* clock() {
    while(true) {
        console.log('Tick!');
        yield;
        console.log('Tock!');
        yield;
    }
}
```
## 应用
1. 异步操作的同步化表达；
2. 控制流管理；
3. 部署Iterator接口；
4. 所为数据结构；

# Generator函数的异步应用
## 传统方法
1. 回调函数；
2. 事件监听；
3. 发布/订阅；
4. Promise对象；
## Thunk函数
1. 求值策略：函数的参数到底应该在何时求值，分为传值调用，传名调用；
2. Thunk函数：传名调用时将参数放到一个临时函数之中，再将这个临时函数传入函数体；

```JavaScript
function f(m) {
    return m + 2;
}

f(x + 5);
// 等同于
var thunk = function() {
    return x + 5;
}

f(thunk) {
    return thunk() + 2;
}
```

3. Thunkify模块;
4. Thunk函数的自动流程管理；

```JavaScript
function run(fn) {
    var gen = fn();
    function next(err, data) {
        var result = gen.next(data);
        if(result.done) return;
        result.value(next);
    }
    
    next();
}

function* g() {...};

run(g);
```
## [co模块](github.com/tj/co)
用于Generator函数的自动执行。

# async函数
## 含义
1. async是什么？用一句话来说，它就是Generator的语法糖；
2. async函数就是将Generator函数的星号替换为async，yield替换为await；
3. 优点：a.自带执行器；b.更好的语义；c.更广的适用性；d.返回值是Promise；
## 用法
1. async函数返回的是一个Promise对象，可以使用then方法添加回调函数；

```JavaScript
function async print(value, ms) {
    await timeout(ms);
    console.log(value);
}

function timeout(ms) {
    return new Promise(resolve => {
        setTimeout(resolve, ms);
    })
}

print('hello async', 5000);
```

2. async函数多种使用形式；

```JavaScript
// 函数声明
async function foo() {};

// 函数表达式
const foo = async function() {};

// 对象的方法
let obj = {
    async foo() {}
};
obj.foo().then(...);

// class方法
class Storage {
    constructor() {
        this.cachePromise = cache.open('avatars');
    }
    
    async getAvatar(name) {
        const cache = await this.cachePromise;
        return cache.match(`/avatars/${name}.jpg`);
    }
}

const storage = new Storage();
stroage.getAvatar('jack').then(...);

// 箭头函数
const foo = async () => {};
```
## 语法
1. 返回Promise对象；

```JavaScript
// return值成为then回调参数
async function foo() {
    return 'aaa';
}
foo().then(v => console.log(v));

// throw捕获
async function foo() {
    throw new Error('出错了');
}

foo().then(
    v => console.log(v), 
    e => console.log(e)
);
```

2. Promise对象的状态变化：只有async函数内部的异步操作执行完，才会执行then；
## await命令
1. await后面是一个Promis对象，如果不是，会被转为一个立即resolve的Promise对象；

```JavaScript
async function f() {
    return await 111;
}
f().then(res => console.log(res));
```

2. await后面的Promis对象变为reject状态，则reject的参数会被catch方法的回调函数接收；

```JavaScript
async function foo() {
    await Promise.reject('出错了');
    await Promise.resolve('aaa'); // 不会执行
}
foo().then(v => console.log(v)).catch(e => console.log(e));

// 使异步操作不影响下一步执行
async function f() {
    try {
        await Promise.reject('出错了');
    }catch(e) {
        
    }
    return await Promise.resolve('hello');
}
f().then(v => console.log(v)).catch(e => console.log(e));

async function f() {
    await Promise.reject('出错了').catch(e => console.log(e));
    return await Promise.resolve('hello');
}
f().then(v => console.log(v)).catch(e => console.log(e));
```
3. 将多条await命令，统一放在try...catch结构中；
## 使用注意点
1. await命令后面的Promise对象运行结果可能是reject，所以最好把await命令都放在try...catch结构中；
2. 多条await命令后面的异步操作如果不存在继发关系，最好让它们同时触发；

```JavaScript
// 写法一
let [foo, bar] = await Promise.all([getFoo(), getBar()]);
// 写法二
let fooPromise = getFoo();
let barPromise = getBar();
let foo = await fooPromise;
let bar = await barPromise;
```

3. await命令只能在async函数中，否则会报错；
## async函数的实现原理
原理是将Generator函数和自动执行器包装在一个函数里；

```JavaScript
async function fn(args) {
    ...
}
// 等同于
function fn(args) {
    return spawn(function* () {
        ...
    });
}

function spawn(genF) {
    return new Promise(function(resolve, reject) {
        var gen = genF();
        function step(nextF) {
            try {
                var next = nextF();
            }catch(e) {
                return reject(e);
            }
            if(next.done) {
                return resolve(next.value);
            }
            Promise.resolve(next.value).then(function(v) {
               step(function() {
                   return gen.next(v);
               });
            }, function(e) {
                step(function() {
                    return gen.throw(e);
                });
            });
        }
        step(function () {
            return gen.next(undefined);
        });
    });
}
```
## async函数的顺序执行和并发执行

```JavaScript
// 顺序执行动画
async function chainAnimationsAsync(elem, animations) {
    let ret = null;
    try{
        for(let ani of animations) {
            ret = await ani(elem);
        }
    }catch(e) {
        // 忽略错误
    }
    return ret;
}
// 顺序读取一组url
async function logInOrder(urls) {
    for(let url of urls) {
        const response = await fetch(url);
        console.log(response.text())
    }
}
// 并发读取一组url
async function loginInOrder(urls) {
    const textPromises = urls.map(async url => {
        const response = await fetch(url);
        return response.text();
    });
    
    for(let text of textPromises) {
        console.log(await textPromises);
    }
}
```
## 异步遍历器（提案）
语法特点：遍历器的next方法返回的是一个Promise对象
1. for await...of

```JavaScript
async function f() {
    for await (const x of createAsyncIterator(['a', 'b'])) {
        console.log(x);
    }
}

(async function () {
    for await (let x of ['a', 'b']) {
        console.log(x);
    }
})();
```
2. 异步Generator函数

```JavaScript
async function* f() {
    yield 'hello';
}
const f = f();
f.next().then(res => console.log(res));
```

3. yield*语句与一个异步遍历器一同使用

```JavaScript
async function* f() {
    yield 'a';
    yield 'b';
    return 'c';
}

async function* gen() {
    const result = yield* f();
}
```
# Class的基本用法
## 简介
1. 类完全可以看作构造函数的另一种写法；

```JavaScript
class Point {
    ...
}
typeof Point; // 'function'
Point === Point.prototype.constructor; // true
new Point()
```

2. 类的所有方法都定义在prototype上；
3. Object.assign一次向类添加多个方法；

```JavaScript
class Point {
    constructor() {
        ...
    }    
}
Object.assign(Point.prototype, {
   toString() {};
   toValue() {};
});
```

4. 和ES5不同，类内部的方法不可枚举；
5. 类的属性名可以采用表达式；

```JavaScript
let methodName = 'getArea';
class Square {
    constructor() {
        ...
    }
    
    [methodName]() {
        ...
    }
}
```
## 严格模式
类和模块内部默认使用严格模式；
## constructor方法
1. 如果没有显示定义，会默认生成；
2. 默认返回实例对象this，不过可以指定返回另一个对象；

```JavaScript
class Foo {
    constructor() {
        return Object.create(null);
    }
}
new Foo() instanceof Foo; // false
```

3. 必须使用new，否则报错，普通构造函数不报错；
## 类的实例对象
1. 实例的属性除非显式定义在this上，否则都是定义在原型上；
2. 类的所有实例共享一个原型对象；
## class表达式
1. 使用表达式的形式定义类；

```JavaScript
const myclass = class Foo {
    getClassName() {
        return Foo.name;
    }
} // myclass是类的名字，Foo只在class内部使用，指代当前类
```

2. 立即执行的Class;

```JavaScript
let person = new class {
    ...
}(param)
```
## 类不存在变量提升
## 私有方法的实现
1. 命名上约定，如_foo；
2. 将私有方法移出模块；

```JavaScript
class widget {
    foo(baz) {
        bar.call(this, baz);
    }
}

function bar(baz) {
    ...    
}
```

3. Symbol命名；

```JavaScript
const bar = Symbol('bar');
const baz = Symbol('baz');
class Point {
    // 公有方法
    foo(name) {
        this[bar](name);
    }
    // 私有方法
    [bar](name) {
        return this[baz] = name;
    }
}
```
## 私有属性（#）提案
## this指向
将方法提取出来单独使用，this会指向该方法运行时所在的环境，解决方法：
1. 构造函数中绑定this；

```JavaScript
class Foo {
    constructor() {
        this.sayName = this.sayName.bind(this);
    }
}
```

2. 使用箭头函数；

```JavaScript
class Foo {
    constructor() {
        this.sayName = (name = 'there') => {
            this.print(`Hello${name}`);
        }
    }
}
```

3. 使用Proxy，在获取方法绑定this；
## name总是返回紧跟在class后面的类名；
## getter和setter
## 静态方法
1. 在一个方法前面加上static，表示该方法不会被实例继承，直接通过类调用：

```JavaScript
class Foo {
    static say() {
        console.log('hello');
    }
}
const f = new Foo();
f.say();
Foo.say();
```
2. 静态方法可以被子类继承；

```JavaScript
class Foo {
    static say() {
        console.log('hello');
    }
}

class Bar extends Foo {};

Bar.say();
```

3. 从super对象调用；
## class静态属性和实例属性
1. 当前静态属性定义；

```JavaScript
class Foo {}
Foo.prop = 1;
Foo.prop; // 1
```
2. 提案；
## new.target属性
用于确定构造韩式是怎么调用的，只能在构造函数内部使用；  
不能独立使用，只能继承后才能使用的类；

```JavaScript
class Shape {
    constructor() {
        if(new.target === Shape) {
            throw new Error('本类只能继承使用');
        }
    }
}

class Rectangle extends Shape {
    constructor() {
        super();
    }
}

new Shape();
new Rectangle();
```
# class的继承
## 简介
class使用extends实现继承；子类没有自己的this对象，因此必须在constructor中调用super方法；继承的实质是先创建父类的实例对象this，然后再用子类的构造函数修改this。

```JavaScript
class Point {
    constructor(x, y) {
        this.x = x;
        this.y = y;
    }
}

class ColorPoint {
    constructor(x, y, color) {
        this.color = color; // 报错
        super(x, y);
        this.color = color; // 不报错
    }
}

let cp = new ColorPoint(23, 4, 'red');
cp instanceof Point; // true
cp instanceof ColorPoint; // true
```
## Object.getPrototypeOf()
从子类上获取父类

```JavaScript
Object.getPrototypeOf(ColorPoint) === Point; // true
```
## super关键字
1. super作为函数调用时代表父类的构造函数，只能用在子类构造函数中，否则报错；

```JavaScript
class A {
    constructor() {
        console.log(new.target.name);
    }
}

class B extends A {
    constructor() {
        super();
    }
    m() {
        super(); // 报错
    }
}

new A(); // A
new B(); // B
```

2. super作为对象时再普通方法中指向父类的原型对象；

```JavaScript
class A {
    constructor() {
        this.q = 21;
    }
    p() {
        return 2;
    }
}
A.prototype.x = 22;

class B extends A {
    constructor() {
        super();
        console.log(super.x);
        console.log(super.p());
    }
    
    get m() {
        return super.q;
    }
}

let b = new B(); // 2
b.m; // undefined
// super指向父类的原型对象，定义在父亲实例上的方法或属性无法通过super调用

```
3. super调用父类的方法时，super会绑定子类的this;

```JavaScript
class A {
    constructor() {
        this.x = 1;
    }
    print() {
        console.log(this.x);
    }
}

class B extends A {
    constructor() {
        super();
        this.x = 2;
    }
    m() {
        super.print();
    }
}

let b = new B();
b.m(); // 2
```
3. 通过super对某个属性赋值，这时super就是this，赋值的属性会变成子类实例的属性；

```JavaScript
class A {
    constructor() {
        this.x = 1;
    }
    print() {
        console.log(this.x);
    }
}

class B extends A {
    constructor() {
        super();
        this.x = 2;
        super.x = 3;
    }
    m() {
        super.print();
    }
}

let b = new B();
b.m();  // 3
```
4. super作为对象用在静态方法中，super指向父类而不是父类的原型对象；

```JavaScript
class A {
    static method(name) {
        console.log('static' + name);
    }
    
    method(name) {
        console.log('instance' + name);
    }
}

class B extends A {
    static method(name) {
        super.method(name);
    }
    
    method(name) {
        super.method(name);
    }
}

B.method('张三'); // 'static张三'
let b = new B();
b.method('张三')；// 'instance张三'
```
## 类的prototype属性和__proto__属性
1. 两条继承链：子类的__proto__属性表示构造函数的继承，总是指向父类；子类prototype属性的__proto__属性表示方法的继承，总是指向父类的prototype属性；

```JavaScript
class A {}
class B extends A {}

B.__proto__ === A; // true
B.prototype.__proto__ === A.prototype; // true
// 作为一个对象，子类的原型是父类；作为一个构造函数，
Object.setPrototypeOf(B.prototype, A.prototype);
// 等同于
B.prototype.__proto__ = A.prototype;

Object.setPrototypeOf(B, A);
// 等同于
B.__proto__ = A;

Object.create(A.prototype);
// 等同于
B.prototype.__proto__ = A.prototype;
```
2. 子类继承多种类型的值；

```JavaScript
// 子类继承对象
class A extends Object {}

A.__proto__ === Object;
A.prototype.__proto__ === Object.prototype;

// 不存在任何继承
class A {}

A.__proto__ === Function.prototype;
A.prototype.__proto__ === Object.prototype;

// 子类继承null
class A extends null {}

A.__proto__ === Function.prototype;
A.prototype.__proto__ === undefined;
```
3. 子类的原型的原型是父类的原型（__proto__.__proto__）；
## 原生构造函数的继承
原生构造函数：Boolean()、Number()、String()、Array()、Date()、Function()、RegExp()、Error()、Object()；  
继承Object子类的差异行为；
## mix模式的实现

```JavaScript
function mix(...mixins) {
    class Mix();
    
    for(let mixin of mixins) {
        copyProperties(Mix, mixin);
        copyProperties(Mix.prototype, mixin.prototype);
    }
    
    return Mix;
}

function copyProperties(target, source) {
    for(let key of Reflect.ownKeys(source)) {
        if(key !== 'constructor' && key !== 'prototype' && key !== 'name') {
            let desc = Object.getOwnPropertyDescroptor(source, key);
            Object.defineProperty(target, key, desc);
        }
    }
}

// 将多个对象合并为一个类
class DistributedEdit extends mix(A, B) {
    
}
```
# 修饰器
## 类的修饰

```JavaScript
@descriptor
class A {
    
}
// 等同于
class A {}
A = descriptor(A) || A;
// 第一个参数就是所要修饰的目标类
function descriptor(target) {
    // ...
}
// 多个参数
function method(a, b, c) {
    return function(target) {}
}
```
## 方法的修饰

```JavaScript
class P {
    @readonly
    name() {
        return '张三';
    }
}

// 修饰器函数接受三个参数
function readonly(target, name, descriptor) {
    descriptor.writable = false;
    return descriptor;
}
readonly(P.prototype, 'name', descriptor);
// 类似于
Object.defineProperty(P.prototype, 'name', descriptor);
```
多个修饰器，先从外到内进入修饰器，然后由内向外执行

```JavaScript
function dec(id) {
    console.log('evaluated', id);
    return (target, name, descriptor) => console.log('ex', id);
}

class Example {
    @dec(1)
    @dec(2)
    method() {
        
    }
}
// evaluated 1
// evaluated 2
// ex 2
// ex 1
```
函数不能使用修饰器，原因是存在函数提升
## 常见修饰器[core-decorators.js](https://github.com/jayphelps/core-decorators)
## 自动发布事件[Postal.js](https://github.com/postaljs/postal.js)
## 混入[Trait](https://github.com/CocktailJs/traits-decorator)
# Module的语法
## 概述
1. ES6之前模块加载方案有CommonJS和AMD，前者用于服务器，后者用于浏览器，都在运行时确定模块的依赖关系，以及输入和输出的变量；
2. ES6的设计思想是尽量静态化，在编译时就能确定模块的依赖关系，以及输入和输出的变量；
## 模块自动采用严格模式
1. 变量必须声明后使用；
2. 函数的参数不能有同名参数，否则报错；
3. 不能使用with语句；
4. 不能对只读属性赋值，否则报错；
5. 不能使用前缀0表示八进制数，否则报错；
6. 不能删除不可删除的属性，否则报错；
7. 不能删除变量delete prop，会报错，只能删除属性delete global[prop]；
8. eval不会在它的外层作用域引入变量；
9. eval和arguments不能被重新赋值；
10. arguments不会自动反映函数参数的变化；
11. 不能使用arguments.callee；
12. 不能使用arguments.caller；
13. 禁止this指向全局变量；
14. 不能使用fn.caller和fn.arguments获取函数调用的堆栈；
15. 增加了保留字（protected、static和interface）。

## export命令
用于规定模块的对外接口。

```JavaScript
// 输出变量，函数或类
var name = '张三';
var age = 12;
function sayName() {
    console.log(name);
}

export {name, age, sayName};

// as 重命名
export {sayName as xxx};

// export规定的是对外的接口，必须与模块内部的变量一一对应
// 报错
export 1; 
// 报错
var m = 1;
export m;
// 报错
function f() {}
export f;
// 写法一
export var m = 1;
// 写法二
var m = 1;
export {m};
// 写法三
var m = 1;
export { m as n };

// export是动态绑定关系，可以取到模块内部实时的值
export var foo = 'baz';
setTimeour({} => foo = 'bar', 500);
```
## import命令
用于输入其他模块提供的功能。
```JavaScript
// 变量名必须与被导入模块对外接口的名称相同
import {a, b, c} from '...';

// as重新命名
import {a as aa} from '...';

// import命令具有提升效果，会提升到头部首先执行
foo();
import {foo} from '...';

// import是静态执行，所以不能使用表达式和变量或if结构
import {'f' + 'oo'} from '...';

var name = 'foo';
import {name} from '...';

if(true) {
    import {foo} from '...';
}
```
## 模块的整体加载

```JavaScript
import {a, b, c} from '...';
// 等价于
import * as foo from '...';
foo.a;
foo.b;
foo.c;
// 不允许运行时改变
foo.a = 'aaa'; // 不允许
```
## export default命令
为模块指定默认输出

```JavaScript
export default function foo() {}
// 等价于
export default function() {}

// 默认输出
export default function a() {}
import a from '...';
// 正常输出(带大括号)
export function a() {};
import {a} from '...';

// export default输出一个叫default的变量，所以后面不能跟变量声明语句
export default var a = 1; // 错误
export default 42; // 正确

// 一条import语句同时输入默认方法和其他接口
import _, {each} from '...'

// 输出类
export default class {...}
```
## export和import复合写法

```JavaScript
export {foo, baz} from '...';
// 等价于
import {foo, baz} from '...';
export {foo, baz};

// 接口改名
export {foo as myFoo} from '...';

// 整体输出
export * from '...';

// 默认接口
export {default} from '...';

// 具名改默认
export {es6 as default} from '...';

// 默认改具名
export {default as es6} form '...';
```
## 跨模块常量
const声明的常量只在当前的代码块内有效。

```JavaScript
// a.js
export const A = 1;
export const B = 2;
// b.js
import * as constants from 'a.js';
constants.A;
constants.B;
// c.js
import {A, B} from 'a.js';
A;
B;
```
## import()提案，实现运行时加载
1. 按需加载；
2. 条件加载；
3. 动态的模块路径；

```JavaScript
import(url).then((a,b) => {...});
```
# Module的加载实现
## 浏览器加载
1. defer和async异步加载js脚本；
2. type="module"渲染完再执行模块脚本，等同于defer；

```JavaScript
// 外部加载
<script type="module" src="url"></script>
// 内嵌
<script type="module">
    import util from url;
    ...
</script>
```

## ES6模块与CommonJS的差异
1. CommonJS模块输出的是一个值的复制，ES6模块输出的是值的引用；
2. CommonJS是运行时加载，ES6模块是编译时输出接口；
## Node加载
ES6模块和CommonJS采用各自的加载方案。

```JavaScript
// 不输出任何接口，但是希望被Node认为是ES6模块
export {};
```
ES6模块中顶层的this指向undefined，CommonJS顶层this指向当前模块。
1. import命令加载CommonJS模块；
2. require命令加载ES6模块
3. CommonJS模块的循环加载；
4. ES6模块的循环加载；

## ES6模块的转码
1. [ES6 module transpiler](https://github.com/esnext/es6-module-transpiler)
2. [System.JS](https://github.com/systemjs/systemjs)

# 编程风格
## 块级作用域
1. let取代var；
2. const优于let；

## 字符串
1. 静态字符串一律使用单引号或反引号；
2. 动态字符串使用反引号；

## 解构赋值
1. 使用数组成员对变量赋值，优先使用解构赋值；
2. 函数的参数是对象的成员，优先使用解构赋值；
3. 函数返回多个值，优先使用对象的解构赋值，而不是数组的解构赋值；

## 对象
1. 单行定义的对象，最后一个成员不以逗号结尾，多行定义的对象，最后一个成员以逗号结尾；
2. 对象尽量静态化，一旦定义不得随意添加新的属性；
3. 如果对象的属性名是动态的，可以在创造对象的时候使用属性表达式定义；
4. 对象的属性和方法尽量采用简洁表达法；

## 数组
1. 使用扩展运算符（...）复制数组；
2. 使用Array.from方法将类似数组的对象转为数组；

## 函数
1. 立即执行的函数写成箭头函数的形式；
2. 简单的、单行的、不会复用的函数，尽量使用箭头函数；
3. 不要再函数体内使用arguments变量，尽量使用rest运算符(...)代替；
4. 使用默认值语法设置函数参数的默认值；

## Map结构
注意区分Object和Map，只有模拟实体对象时才使用Object。

## Class
1. 总是用class取代需要prototype的操作；
2. 使用extends实现继承，简单，不破坏instanceof预算；

## Module
1. 使用import取代require的代码；
2. 使用export取代module.exports；
3. 模块只有一个输出值使用export default，不同时使用default和普通export；
4. 模块默认输出一个函数，函数首字母小写；
5. 模块默认输出一个对象，对象名首字母大写；

## ESLint使用
保证代码语法正确、风格统一。

# ECMAScript规格
规格文件是计算机语言的官方标准，详细描述了语法规则和实现方法。
[官方网站](http://www.ecma-international.org/ecma-262/)

# ArrayBuffer
## ArrayBuffer对象
1. ArrayBuffer对象代表存储二进制数据的一段内存，不能直接读/写；
2. ArrayBuffer也是一个构造函数，分配一段可以存放数据的连续内存区域；

```JavaScript
// 参数为内存大小
var buf = new ArrayBuffer(32); 
var dataView = new DataView(buf);
dataView.getUint8(0); // 0
```

3. DataView是为了读/写这段内存的视图，数组成员都是同一个数据类型；
4. 一个内存上的多个视图会互相影响；
5. TypeArray视图是一组构造函数，可以接受普通数组作为参数；

```JavaScript
var ta = new Uint8Array([0, 1, 2]);
ta.length; // 3
ta[0] = 5; // 0
ta; [5, 1, 2]
```

6. byteLength属性返回分配内存区域的字节长度，分配内存的时候可能分配失败；

```JavaScript
var bf = new ArrayBuffer(32);
bf.byteLength;
```
7. slice方法复制ArrayBuffer对象，两个参数分别是开始和截止的字节序号；
8. isView方法判断参数是否为ArrayBuffer的视图实例；
## TypedArray视图
1. 数组成员可以是不同类型，9种视图类型；
2. TypedArray数组只是一层视图，本身不存储数据；
3. TypedArray数组提供9种构造函数，用于生成数组实例，参数可以是成员数量，另一个TypedArray实例或者普通数组；
4. 普通数组的操作方法和属性对TypedArray完全使用，除了concat；
5. 字节序指的是数值在内存中的表示方式，分为小端字节序和大端字节序；
6. BYTES_PER_ELEMENT属性表示该数据类型占据的字节数；
7. ArrayBuffer与字符串的互相转换；

```JavaScript
// ArrayBuffer转字符串
String.fromCharCode.apply(null, new Uint16Array(buf));

// 字符串转ArrayBuffer
let buf = new ArrayBuffer(str.length * 2); // 每个字符占用2个字节
let bufView = new Uint16Array(buf);
for(let i = 0, strLen = str.length; i < strLen; i ++) {
    bufView[i] = str.charCodeAt(i);
}
return buf;
```

8. TypedArray溢出分为正向溢出和负向溢出；
9. TypedArray实例的buffer属性返回内存对应的ArrayBuffer对象，只读；
10. byteLength属性返回内存长度，单位为字节，byteOffset属性返回该TypedArray数组从底层ArrayBuffer对象的哪个字节开始，只读；
11. length返回数组含有多少个成员；
12. set方法用于复制数组；
13. subarray方法用于复制数组的一部分建立一个新的视图；
14. slice方法返回一个指定位置的新的TypedArray实例；
15. of方法将参数转为一个TypedArray实例；
16. from方法接受一个可遍历的数组结构作为参数，返回一个基于此结构的TypedArray实例；
## 复合视图
同一段内存中可以依次存放不同类型的数据。
## DataView视图
用于操作多种类型的数据。

```JavaScript
var buffer = new ArrayBuffer(24);
var dv = new DataView(buffer);
```
属性：  
buffer：返回队友ArrayBuffer对象；  
byteLength：返回占据的内存字节长度；  
buteOffset：返回视图从对应的ArrayBuffer对象的哪个字节开始。  
  
8个读取内存的方法：getInt8、getUint8、getInt16、getUint16、getInt32、getUint32、getFloat32、getFloat64
  
get方法默认大端字节序解读数据，指定第二个参数true使用小端字节序。

```JavaScript
var v1 = dv.get(1, true); // 小端字节序
var v2 = dv.get(3, false); // 大端字节序
var v3 = dv.get(5); // 大端字节序
```

8个写入内存的方法：setInt8、setUint8、setInt16、setUint16、setInt32、setUint32、setFloat32、setFloat64  
   
set方法默认大端字节序解读数据，指定第三个参数true使用小端字节序，第一个参数表示从哪个字节写入，第二个参数为写入的数据。

```JavaScript
dv.setInt32(1, 25, true); // 小端字节序
dv.setInt32(3, 22, false); // 大端字节序
dv.setInt32(5, 30); // 大端字节序
```
## 二进制数组的应用
1. AJAX；
2. Canvas；
3. WebSocket；
4. Fetch API；
5. File API；
## sharedArrayBuffer
允许Worker线程与主线程共享同一块内存。
```JavaScript
// 新建1kb的共享内存
var shareBuffer = new sharedArrayBuffer(1024);
// 主线程
var w = new Work('mywork.js');
// 主线程将共享内存的地址发送出去
w.postMessage(shareBuffer);
// 在内存上建立视图，供写入数据
const sharedArray = new Int32Array(shareBuffer);
w.onmessage = function(ev) {
    console.log(ev.data); // 主线程共享的数据
}
```
## Atomics
保证所有共享内存的操作都是“原子性”的，并且可以在所有线程内同步；

```JavaScript
// 主线程
var sab = new SharedArrayBuffer(Int32Array.BYTES_PER_ELEMENT * 100000);
var ia = new Int32Array(sab);

for(let i = 0; i< ia.length; i++) {
    ia[i] = primes.next(); // 存入质数
}

// worker线程
ia[112] ++; //错误
Atomics.add(ia, 112, 1); // 正确
```
方法：
1. 读取数据：Atomics.load(array, index);
2. 写入数据：Atomics.store(array, index, value);
3. 锁内存：Atomics.wait(sharedArray, index, value, item);
4. 解除锁：Atomics.wake(sharedArray, index, count);
5. 运算方法：   
        添加：Atomics.add(sharedArray, index, value);
        减去：Atomics.sub(sharedArray, index, value);
   位运算and：Atomics.and(sharedArray, index, value);
    位运算or：Atmoics.or(sharedArray, index, value);
   位运算xor：Atmoics.xor(sharedArray, index, value);
6. 替换：Atomics.compareExchange(sharedArray, index, oldval, newval);
7. 设置指定下标的值：Atomics.exchange(sharedArray, index, value);
8. 返回对象是否可以处理某个size的内存锁定：Atomics.isLockFree(size);