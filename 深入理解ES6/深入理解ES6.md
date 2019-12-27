## 块级作用域绑定
#### var
1. 变量提升；

```JavaScript
function getVal(condition) {
    if(condition) {
        var val = 'blue';
        console.log(val);
    }else {
        console.log('false ' + val);
    }
    
    console.log('finially ' + val);
}

getVal(true);
getVal(false);
```
2. 全局作用域的绑定，会覆盖全局变量；

```JavaScript
var name = '张三'；
console.log(window.name);
```
#### let
1. 不会变量提升；
2. 禁止重复声明；
3. 临时死区TDZ；
4. 不能覆盖全局变量，只能屏蔽它；

#### const
1. 不会变量提升；
2. 禁止重复声明；
3. 声明的常量必须进行初始化；
4. 定义的常量不可再赋值；声明的对象不允许修改绑定，但允许修改值；
5. 临时死区TDZ；
6. 不能覆盖全局变量，只能屏蔽它；

#### 循环相关
1. 循环中的块作用域绑定；
2. 循环中的函数，普通函数与立即调用函数表达式IIFE；
3. 循环中的let声明；
4. 循环中的const声明；

```JavaScript
for(const i = 0; i < 10; i++) {
    console.log(i); 
}
// 0
// 报错

var obj = {
    a: 1,
    b: 2,
    c: 3,
}

for(const value in obj) {
    console.log(value);
}
// a
// b
// c
```

#### 实践优化
默认使用const，只有在确定需要改变变量的值的时候使用let。

## 字符串和正则表达式
#### ES6新增方法
1. codePonitAt(index) 传入编码单元的位置;
2. String.fromCodePoint() 传入指定码位;
3. normalize() 传入可选字符串参数;
4. includes()/startsWith()/endsWith() 两个参数：要搜索的文本，开始搜索的位置可选;
5. repeat() 传入字符串重复次数;

#### 正则表达式新增修饰符
1. u修饰符;
2. y修饰符;
3. 正则表达式的复制;
4. flags属性;

#### 模版字面量
1. 基础语法，用反撇号替换单、双引号，且在模版字面量中，不需要转义单、双引号;
2. 多行字符串，可用反撇号或者\n直接换行;

```JavaScript
let html = `<div>
    <p>多行字符串</p>
</div>`.trim();
```

3. 字符串占位符，${JavaScript表达式}，可以嵌入变量，运算式，函数调用等;
4. 标签模板;

```JavaScript
function passthru(literals, ...substitutions) {
    let result = '';
    for(let i = 0; i < substitutions.length; i++) {
        result += literals[i];
        result += substitutions[i];
    }
    
    result += literals[literals.length - 1];
    return result;
}

let count = 10, 
    price = 0.25,
    message = passthru`${count} items cost $${(count * price).toFixed(2)}`;
    
console.log(message);
```

5. 模版字面量中使用原始值;

## 函数
#### 函数的默认值
1. 函数默认值的使用;

```JavaScript
function say(name = '张三', age = 18) {
    console.log(name, age);
}

say(); // 张三 18
say('Lay'); // Lay 18
say(undefined, 10); // 张三 10
say(null, null); // null null
```

2. 函数默认值对arguments的影响;

```JavaScript
function say(name = '张三', age = 18, hobby) {
    console.log(arguments.length);
}

say('a'); // 1
```
3. 默认参数在函数调用时求值，可以使用先定义的参数作为后定义参数的默认值;
4. 默认参数的临时死区TDZ;

#### 无命名参数
1. 不定参数（...）的使用;
2. 不定参数最多只能声明一个不定参数，且一定要放在所有参数的末尾;
3. 无论是否使用不定参数，arguments对象总是包含所有传入函数的参数;
4. 增强的Function构造函数使用默认参数和默认参数;
5. 展开运算符（...）的使用;

#### 函数的name属性
1. 正常函数返回函数名;
2. 函数表达式;

```JavaScript
var a = function b() {};
a.name; // b

var a = function () {};
a.name; // a
```

3. getter函数和setter函数，名称增加前缀'get'或'set';
4. bind()函数创建的函数，增加前缀'bound';
5. Function构造函数创建的函数，名称为'anonymous';
6. 不能使用name属性的值来获取对于函数的引用;

#### 判断函数被调用的方法
1. ES5使用instanceof;
2. ES6使用new.target;

#### 块级函数
1.ES5严格模式抛出错误;

```JavaScript
'use strict';
if(true) {
    function doSomething() {} // 报错
}
```
2. ES6严格模式下，不会报错且声明提升，执行结束销毁;

```JavaScript
'use strict';
if(true) {
    console.log(typeof doSomething); // 'function'
    function doSomething() {}
}

console.log(typeof doSomething); // undefined
```
3. ES6非严格模式下;

```JavaScript
'use strict';
console.log(typeof doSomething); // undefined
if(true) {
    console.log(typeof doSomething); // 'function'
    function doSomething() {}
}

console.log(typeof doSomething); // 'function'
```
#### 箭头函数
1. 没有this、super、arguments和new.target绑定;
2. 不能通过new关键字调用;
3. 没有原型，即用即弃;
4. 不可以通过bind，call，apply改变this的绑定;
5. 不支持arguments对象;
6. 不支持重复的命名参数;

#### 尾调用优化
1. 尾调用不访问当前栈帧的变量（不是一个闭包）;
2. 在函数内部，尾调用是最后一条语句;
3. 尾调用的结果作为函数值的返回;
   
拓展：尾递归优化的实现。

## 扩展对象的功能性
对象分为普通对象，特异对象，标准对象，内建对象。

#### 对象字面量语法扩展
1. 属性的简写;
2. 对象方法的简写;
3. 可计算属性名;
4. 重复的对象字面量属性取最后一次赋值的值;
5. 自有属性的枚举顺序;

```JavaScript
// 1. 数字优先，升序排列
// 2. 其次字符串按照加入的顺序排列
// 3. 最后Symbol按照加入顺序排列
var obj = {
    a: 1,
    0: 1,
    [Symbol('foo')]: 1,
    c: 1,
    4: 1,
    2: 1,
    b: 1,
    [Symbol('bar')]: 1,
}

obj.d = 1;
Reflect.ownKeys(obj); 
// ["0", "2", "4", "a", "c", "b", "d", Symbol(foo), Symbol(bar)]
```


#### 新增方法
1. Object.is();

```JavaScript
+0 === -0; // true
Object.is(+0, -0); // false

NaN === NaN; // false
Object.is(NaN, NaN); // true
```

2. Object.assign() 接受一个接收对象和任意数量的源对象;
3. Object.setPrototypeOf()与Object.getPrototypeOf();
4. super引用，[[homeObject]];

```JavaScript
let person = {
    say() {
        return 'hello';
    }
}

let friend = {
    say() {
        return super.say() + ' hi';
    }
}

let greeting = {
    say: function() {
        return super.say() + ' 你好！';  // 报错
    }
}

Object.setPrototypeOf(friend, person);
let f = Object.create(friend);
f.say();
```

## 解构
解构功能，将数据结构打散过程变的更加简单，可以从打散后更小的部分中获取所需的信息。

#### 对象解构
1. 解构语法形式是在一个赋值操作符左侧放置一个对象字面量;

```JavaScript
let node = {
    type: 'Identifier',
    name: 'foo',
};

let {type, name} = node;
console.log(type); // 'Identifier'
console.log(name); // 'foo'
```

2. 解构赋值，一定要用小括号包裹解构赋值语句;

```JavaScript
let node = {
    type: 'Identifier',
    name: 'foo',
},
  type, name;

({type, name} = node);
{type, name} = node; // 报错
```

3. 默认值，为指定属性不存在的属性添加默认值;
4. 为非同名局部变量赋值，同样可以添加默认值;

```JavaScript
let node = {
    type: 'Identifier',
    name: 'foo',
};

let {type: localType, name: localName, age: localAge = 18} = node;
console.log(localType);
console.log(localName);
console.log(localAge);
```

5. 嵌套对象解构;

#### 数组解构
1. 解构操作全在数组内完成;

```JavaScript
let array = ['a', 'b', 'c'];
let [,,c] = array;
console.log(c); // 'c'
```

2. 解构赋值，数组解构赋值语句不需要小括号包裹表达式;

```JavaScript
// 交换两个变量
let a = 1;
let b = 2;

[a, b] = [b, a];
console.log(a); // 2
console.log(b); // 1
```

3. 为指定位置不存在或值为undefined使用默认值;
4. 嵌套数组解构;
5. 不定元素（...）以及字符串拼接;

```JavaScript
let arr = [1, 2, 3, 4];
let [a, ...other] = arr;
console.log(other); // [2, 3, 4]
[...arr, ...other]; // [1, 2, 3, 4, 2, 3, 4]
```

#### 解构参数
1. 使用解构参数，更加清晰地了解函数预期传入的参数;

```JavaScript
function foo(name, age, {sex, hobby, nick}) {
    // ...
}

// 解构参数必须传值
foo('张三', 18); // 报错
```

2. 解构参数的默认值，使解构参数变成可选;

```JavaScript
function foo(name, age, {sex, hobby, nick} = {}) {
    // ...
}

foo('张三', 18); // 不报错

function baz(name, age, {sex = '男', hobby = '篮球', nick = '疯子'} = {sex: '男', hobby: '篮球', nick: '疯子'}) {
    // ...
}

baz('张三', 18);
// 可将默认值提取为对象
```

## Symbol
原始类型：字符串型，数字型，布尔型，null，undefined以及Symbol。

#### Symbol使用
1. 创建和使用;

```JavaScript
const sym = Symbol('first symbol');
let obj = {
    [sym]: 'symbol',
}

obj[sym]; // 'symbol'
```

2. Symbol共享体系，Symbol.for();
3. Symbol类型强制转换;

```JavaScript
const sym = Symbol.for('name');
String(sym); // 'Symbol(name)'
sym + ''; // 报错
Number(sym); // 报错
Boolean(sym); // true
```

4. 对象的Symbol属性检索Object.getOwnPropertySymbols();

#### well-konwn Symbol
1. Symbol.hasInstance;
2. Symbol.isConcatSpreadable;
3. Symbol.iterator;
4. Symbol.match, Symbol.search, Symbol.split, Symbol.replace;
5. Symbol.species;
6. Symbol.toPromitive;
7. Symbol.toStingTag;
8. Symbol.unscopables;

## Set集合和Map集合
所有对象的属性名必须是字符串类型，并且每个键名都是唯一的。

```JavaScript
let obj1 = { name: 'a' };
let obj2 = {};
let obj = Object.create(null);

obj[obj1] = 'aaa';
obj[obj2]; // 'aaa'---对象都被转化为'[object object]'
```

#### Set集合
Set类型是一种有序列表，其中含有一些相互独立的非重复值，通过Set集合可以快速访问其中的数据，更有效地追踪各种离散值。
1. 创建并添加元素；
2. 检测和移除元素；
3. forEach方法；
4. 转化为数组；

```JavaScript
let set = new Set([1, 2, 3, 4, 2]);
set.add(1);
set.add(5);
set.add('5');
set.has(1); // true
set.delete(3); // true
[...set]; // [1, 2, 4, 5, "5"]

set.forEach((value, key, ownSet) => {
    console.log(value, key);
    console.log(ownSet === set);
});

set.clear();
```

5. Weak Set集合（new WeakSet()）：
   1. 只存储对象的弱引用，不可以存储原始值；
   2. 向add()方法传入非对象参数报错，向has()和delete()传入非对象参数会返回false；
   3. 不可迭代，所以不能被用于for...of；
   4. 不暴露任何迭代器，所以无法通过程序本身来检测其中的内容；
   5. 不支持forEach()；
   6. 不支持size属性；

#### Map集合
Map类型是一种储存着许多键值对的有序列表，其中的键名和对应的值支持所有的数据类型。
1. set和get方法；
2. has，delete和clear方法；
3. Map集合的初始化；
4. forEach()方法；

```JavaScript
let map = new Map([[1, 1], ['2', 2]]);
map.set(3, 3);
map.get('2'); // 2
map.has(1); // true
map.delete('2'); // true

map.forEach((value, key, ownMap) => {
    console.log(value, key);
    console.log(ownMap === map);
});

map.clear();
```

5. Weak Map集合：
   1. 键名必须是一个非null类型的对象，否则报错；
   2. 最大的用途是保存web页面中的DOM元素；
   3. 不支持size属性，无法通过get()方法获取相应的值；
   4. 不支持键名枚举，也不支持clear()；
   5. 只支持has()和delete()方法；
   6. 私有对象数据的实现：

```JavaScript
let Person = (function() {
    let privateData = new WeakMap();
    
    function Person(name) {
        privateData.set(this, { name: name });
    }
    
    Person.prototype.getName = function() {
        return privateData.get(this).name;
    }
    
    return Person;
}());
```

思考：Set，WeakSet，Map，WeakMap分别的使用场景。

## 迭代器Iterator和生成器Generator
#### 迭代器
迭代器是一种特殊对象，它具有一些专门为迭代过程设计的专有接口，所有的迭代器对象都有一个next()方法，每次调用都要返回一个结果对象。结果对象有两个属性：value和done。

```JavaScript
function createIterator(items) {
    let i = 0;
    return {
        next() {
            let done = (i >= items.length);
            let value = !done ? items[i++] : undefined;
            
            return {
                done: done,
                value: value,
            };
        }
    };
}

var iterator = createIterator([1, 2, 3]);
iterator.next(); // {done: false, value: 1}
iterator.next(); // {done: false, value: 2}
iterator.next(); // {done: false, value: 3}
iterator.next(); // {done: true, value: undefined}
```

#### 生成器
1. 生成器是一种返回迭代器的函数，通过*来表示，函数中会用到新的关键字yield。yield只可以再生成器内部使用。

```JavaScript
function *createIterator(items) {
    yield 1;
    items.forEach(item => {
       yield item; // 报错 
    });
}
```
2. 生成器表达式写法：

```JavaScript
let createIterator = function *() {
    // ...
}
```

3. 生成器对象写法：

```JavaScript
let obj = {
    *createIterator() {
        // ...
    }
}
```

#### 可迭代对象和for...of循环
所有的集合对象（数组、Set集合和Map集合）和字符串都是可迭代对象，都有默认的迭代器。

1. 访问默认迭代器：可以通过Symbol.iterator来访问默认迭代器：

```JavaScript
let arr = [1, 2, 3];
let iterator = arr[Symbol.iterator]();
iterator.next(); // {done: false, value: 1}
iterator.next(); // {done: false, value: 2}
iterator.next(); // {done: false, value: 3}
iterator.next(); // {done: true, value: undefined}
```

2. 创建可迭代对象：给Symbol.iterator属性添加一个生成器：

```JavaScript
let collection = {
    items: [],
    *[Symbol.iterator]() {
        for(let item of this.items) {
            yield item;
        }
    }
};

collection.items.push(1);
collection.items.push(2);
collection.items.push(3);

for(let x of collection) {
    console.log(x);
}
// 1
// 2
// 3
```

3. 内建迭代器：values, keys, entries；
4. 数组和Set集合默认迭代器是values()方法，Map集合默认迭代器是entries()方法：
5. WeakSet和WeakMap无法迭代，但可以使用for...of和解构语法来获取属性；

```JavaScript
let data = new Map([['title', '标题'], ['content', '内容']]);

for(let [key, value] of data) {
    console.log(key, value);
}
// title 标题
// content 内容
```

6. 字符串迭代器：

```JavaScript
let str = 'asdasdasd';

for(let code of str) {
    console.log(code);
}
```

7. NodeList迭代器：

```JavaScript
let divs = document.getElementsByTagName('div');

for(let div of divs) {
    console.log(div);
}
```

8. 展开运算符与非数组可迭代对象：

```JavaScript
let set = new Set([1, 2, 3]);
let map = new Map([[1, 3], [2, 3]]);
[...set, [...map]];
```

#### 高级迭代器功能
1. 给迭代器传递参数，即给next()方法传参：

```JavaScript
function *createIterator() {
    let first = yield 1;
    let second = yield first + 2;
    yield second * 2;
}

let iterator = createIterator();
// 第一次next()传参无效
iterator.next(1); // {value: 1, done: false}
iterator.next(3); // {value: 5, done: false}
iterator.next(2); // {value: 4, done: false}
iterator.next(); // {value: undefined, done: true}
```

2. 抛出错误：

```JavaScript
// 方式一
function *generator() {
    yield 1;
    yield 2;
    yield 3; // 永远不会执行
}

let iterator = generator();
iterator.next(); // {value: 1, done: false}
iterator.next(); // {value: 2, done: false}
iterator.throw(new Error('错误')); // Error
iterator.next(); // {value: undefined, done: true}

// 方式二
function *generator() {
    yield 1;
    try{
        yield 2;
    }catch(ex) {
        yield 22;
    }
    yield 3; // throw不影响后续
}

let iterator = generator();
iterator.next(); // {value: 1, done: false}
iterator.next(); // {value: 2, done: false}
iterator.throw(new Error('错误')); // {value: 22, done: false}
iterator.next(); // {value: 2, done: false}
iterator.next(); // {value: undefined, done: true}
```

3. 返回语句：return语句返回值将赋给返回对象的value属性；

```JavaScript
function *generator() {
    yield 1;
    return 3;
    yield 2; // 永远不会执行
    yield 3; // 永远不会执行
}

let iterator = generator();
iterator.next(); // {value: 1, done: false}
iterator.next(); // {value: 3, done: true}
iterator.next(); // {value: undefined, done: true}
```

4. 委托生成器：

```JavaScript
function *createColorIterator() {
    yield 'red';
    yield 'blue';
}

function *createNumberIterator() {
    yield 1;
    yield 2;
}

function *createIterator() {
    yield *createColorIterator();
    yield *createNumberIterator();
}

// ...
```

#### 异步任务执行器

```JavaScript
function run(taskDef) {
    let task = taskDef;
    let result = task.next();
    
    function step() {
        if(!result.done) {
            if(typeof result.value === 'function') {
                result.value(function(err, data) {
                    if(err) {
                        result = task.throw(err);
                        return;
                    }
                    
                    result = task.next(data);
                    step();
                });
            }else {
                result = task.next(result.value);
                step();
            }
        }
    }
    
    step();
}
```

## JavaScript中的类
#### 类的声明
1. 基本类声明语法：

```JavaScript
class Person {
    constructor(name) {
        this.name = name;
    }
    
    sayName() {
        console.log(this.name)
    }
}

let person = new Person('张三');
person.sayName(); // '张三'

person instanceof Person; // true
person instanceof Object; // true

typeof Person; // 'function'
typeof Person.prototype.sayName; // 'function'
```

2. 自有属性是实例中的属性，不会出现在原型上，且只能在类的构造函数或方法中创建；
3. 类语法与自定义类型的差异：
    1. 类声明不能被提升；真正执行声明语句之前，会一直存在于临时死区中；
    2. 类声明中的所有代码自动运行在严格模式下；
    3. 在类中，所有方法都不可枚举；
    4. 每个类都有一个名为[[construct]]的内部方法；
    5. 必须使用关键字new来调用类的构造方法；
    6. 在类中修改类名会报错，但可以在外部修改类名；
    
```JavaScript
class Foo {
    constructor() {
        Foo = 'bar'; // 报错
    }
}

Foo = 'bar'; // 不报错
```
#### 类表达式
1. 类表达式（不会变量提升）:

```JavaScript
let Person = class {
    // ...
}
// 或者
let Person = class PersonClass {
    // ...
}

typeof Person; // 'function'
typeof PersonClass; // undefined
```

2. 类设计为一等公民，允许通过多种方法使用类的特性：

```JavaScript
function createObject(classDef) {
    return new classDef;
}

let obj = createObject(class {
    sayHi() {
        // ...
    }
});

obj.sayHi();
```

3. 通过立即调用类构造函数创建单例：

```JavaScript
let person = new class {
    constructor(name) {
        this.name = name;
    }
    
    sayName() {
        console.log(this.name);
    }
}('张三');

person.sayName(); // '张三'
```
#### 访问器属性
1. getter和setter：

```JavaScript
class CustomHTMLElement {
    constructor(element) {
        this.element = element;
    }
    
    get html() {
        return this.element.innerHTML;
    }
    
    set html(value) {
        this.element.innerHTML = value;
    }
}

let descriptor = Object.getOwnPropertyDescriptor(CustomHTMLElement.prototype, 'html');
'get' in descriptor; // true
'set' in descriptor; // true
```

2. 可计算成员名称，用方括号包裹一个表达式：

```JavaScript
let methodName = 'SayName';
class Person {
    constructor(name) {
        this.name = name;
    }
    
    [methodName]() {
        console.log(this.name);
    }
}

let person = new Person('张三');
person.sayName(); // '张三'
person[methodName](); // '张三'
```

3. 生成器方法：

```JavaScript
class Myclass {
    *generator() {
        yield 1;
    }
}

let myclass = new Myclass();
let iterator = myclass.generator();
iterator.next(); // {value: 1, done: false}
iterator.next(); // {value: undefined, done: true}

// better
class Collection {
    constructor() {
        this.items = [];
    }
    
    *[Symbol.iterator]() {
        yield *this.items.values();   
    }
}

let collection = new Collection();
collection.items.push(1);
collection.items.push(2);
collection.items.push(3);

for(let val of collection) {
    console.log(val);
}
```

4. 静态成员：不可在实例中访问静态成员，必须直接在类中访问；

```JavaScript
class Myclass {
    static say() {
        console.log(1);
    }
}

let myclass = new Myclass();
myclass.say(); // 报错
Myclass.say(); // 1
```

#### 继承与派生类
1. 使用extends指定类继承的函数，通过super()方法访问基类的构造函数：
    1. 只可以在派生类的构造函数中使用super()；
    2. 在构造函数中访问this之前必须调用super()；
    3. 如果不想调用super()，唯一的方法是让类的构造函数返回一个对象；

```JavaScript
class Rectangle {
    constructor(width, height) {
        this.width = width;
        this.height = height;
    }
    
    getArea() {
        return this.width * this.height;
    }
}
class Square extends Rectangle {
    constructor(width) {
        super(width, width);
    }
}

let square = new Square(3);
square.getArea(); // 9
square instanceof Square; // true
square instanceof Rectangle; // true
```

2. 类方法遮蔽：派生类中的方法总会覆盖基类中的同名方法，使用super.来调用；
3. 静态成员继承：如果基类中含有静态成员，这些静态成员也能在派生类中使用；

```JavaScript
class Rectangle {
    constructor(width, height) {
        this.width = width;
        this.height = height;
    }
    
    getArea() {
        return this.height * this.width;
    }
    
    static create(width, height) {
        return new Rectangle(width, height);
    }
}

class Square extends Rectangle {
    constructor(width) {
        super(width, width);
    }
}

let square = Square.create(3, 3);
square.getArea();
square instanceof Rectangle; // true
square instanceof Square; // false
```

4. 只要表达式可以被解析为具有[[construct]]属性和原型的函数，就能使用extends进行派生；

```JavaScript
function Rectangle(width, height) {
    this.width = width;
    this.height = height;
}

Rectangle.prototype.getArea = function() {
    return this.width * this.height;
}

function getBase() {
    return Rectangle;
}

class Square extends getBase() {
    constructor(width) {
        super(width, width);
    }
}

let square = new Square(3);
square.getArea(); // 9
square instanceof Rectangle; // true
```

5. 内建对象的派生与Symbol.species属性。原本在内建对象中的返回实例本身的方法将自动返回派生类的实例，Symbol.species用于定义返回函数的静态访问器属性；

```JavaScript
class MyArray extends Array {
    
}

let items = new MyArray(1, 2, 3, 4);
let subitems = items.slice(1, 3);
items instanceof Myarray; // true
subitems instanceof Myarray; // true

// Symbol.species
class MyClass {
    static get [Symbol.species]() {
        return this;
    }
    
    constructor(value) {
        this.value = value;
    }
    
    clone() {
        return new this.constructor[Symbol.species](this.value);
    }
}

class MyExtendsClass1 extends MyClass {
    
}

class MyExtendsClass2 extends MyClass {
    static get [Symbol.species]() {
        return MyClass;
    }
}

let myClass1 = new MyExtendsClass1('foo');
let clone1 = myClass1.clone();
let myClass2 = new MyExtendsClass2('bar');
let clone2 = myClass2.clone();

clone1 instanceof MyClass; // true
clone1 instanceof MyExtendsClass1; // true
clone2 instanceof MyClass; // true
clone2 instanceof MyExtendsClass2; // false
```

6. new.target的使用与抽象类的实现；

```JavaScript
class Absolute {
    constructor() {
        if(new.target === Absolute) {
            throw new Error('抽象类不能实例化');
        }
    }
}

let ab = new Absolute();
```

## 改进的数组功能
#### 创建数组
1. 调用Array构造函数，数组字面量；
2. Array.of():
    1. 使用原因：Array构造函数表现的与传入参数类型及数量有些不符；
    2. Array.of()方法总会创建一个包含所有参数的数组；
    3. Array.of()方法不通过Symbol.species属性来确定返回值类型，而是使用当前构造函数（也就是of()方法中的this值）；

```JavaScript
let items = Array.of(2);
items; // [2]

let items = Array.of('2');
items; // ['2']
```

3. Array.from()：
    1. 使用原因：JavaScript不支持直接将非数组对象转换为真实数组；
    2. Array.from()接受可迭代对象或类数组对象作为第一个参数；
    3. 提供一个映射函数作为Array.from()的第二个参数；
    4. 也可以给Array.form()传入第三个参数表示映射函数的this值；
    5. 用Array.from()转换可迭代对象；
    6. 如果一个对象既是类数组又是可迭代对象，Array.form()会根据迭代器来决定转换哪个值；

```JavaScript
function doSomething() {
    let args = Array.from(argements);
    // ...
}

function translate() {
    return Array.from(argument, (value) => value + 1);
}

translate(1, 2, 3); // [2, 3, 4]

// 可迭代对象
let numbers = {
    *[Symbol.iterator]() {
        yield 1;
        yield 2;
        yield 3;
    }
}

Array.from(numbers); // [1, 2, 3]
```

#### 数组新方法
1. find()和findIndex()：

```JavaScript
// 两个参数：回调函数，可选的this值
let numbers = [1, 2, 3, 4, 5];
numbers.find(val => val > 2); // 3
numbers.findIndex(val => val > 3); // 3
```

2. fill()：

```JavaScript
// 三个参数：重写内容，可选的开始索引和不包含结束索引
let numbers = [1, 2, 3, 4, 5, 6];
numbers.fill(111); // [111, 111, 111, 111, 111, 111]
numbers.fill(222, 3); // [111, 111, 111, 222, 222, 222]
numbers.fill(333, 3, 5); // [111, 111, 111, 333, 333, 222]
```

3. copyWithin()：

```JavaScript
// 三个参数：开始填充值的索引位置和开始复制值的索引位置，可选重写元素数量
let numbers = [1, 2, 3, 4, 5, 6];
numbers.copyWithin(2, 0); // [1, 2, 1, 2, 3, 4]
numbers.copyWithin(1, 0, 1); // [1, 1, 1, 2, 3, 4]
numbers.copyWithih(-2, 0 , 1); // [1, 1, 1, 2, 1, 4]
```

#### 定型数组
定型数组是一种专门处理数值类型数据的专用数组，可为JavaScript提供快速的按位运算。
1. 数值数据类型：
    1. 有符号的8位整数int8；
    2. 无符号的8位整数uint8；
    3. 有符号的16位整数int16；
    4. 无符号的16位整数uint16；
    5. 有符号的32位整数int32；
    6. 无符号的32位整数uint32；
    7. 32位浮点数float32；
    8. 64位浮点数float64；
2. 数组缓冲区：所有定型数组的根基，它是一段可以包含特定数量字节的内存地址；

```JavaScript
let buffer = new ArrayBuffer(10); // 分配10字节的内存地址
let buffer2 = buffer.slice(4, 6); // slice方法分隔
buffer.byteLength; // 10
buffer2.byteLength; // 2
```

3. 视图操作数组缓冲区：
    1. DataView类型是一种通用的数组缓冲区视图，支持所有8种数值型数据类型；
    2. 视图的几种只读属性：buffer, byteOffset, byteLength；
    3. 读取和写入数据：set和get；

```JavaScript
let buffer = new ArrayBuffer(10);
let view = new DataView(buffer, 5, 2);
view.buffer; // ArrayBuffer(10) {}
view.byteOffset; // 5
vier.byteLength; // 2
view.buffer === buffer; // true

// get方法接受两个参数：偏移的字节数量和一个可选的布尔值，确定是否小端序读取
// set方法接受三个参数：偏移的比特数量；写入的值；可选的布尔值，是否小端序
view.setInt8(0, 5);
view.setInt8(1, -1);
view.getInt8(0); // 5
view.getInt16(0); // 1535---思考为何为1535
```

4. 定型数组是视图：用于数组缓冲区特定类型的视图，可以强制使用特定的数据类型；
    1. Int8Array;
    2. Uint8Array;
    3. Unit8ClampedArray;
    4. Int16Array;
    5. Uint16Array;
    6. Int32Array;
    7. Uint32Array;
    8. Float32Array;
    9. Float64Array;
    10. Uint8Array和Uint8ClampedArray大致相同，唯一的区别是如果数组缓冲区中的值大于255或者小于0时，Uint8ClampedArray会分别将其转换为0或255；
5. 创建特定类型视图的三种方法：
   1. 传入DataView构造函数可接受的三个参数来创建；
   2. 调用构造函数时传入一个数字，这个数字是元素数量而不是字节数量；
   3. 调用构造函数时，将一个定型数组或一个可迭代对象或一个数组或一个类数组对象作为唯一参数传入；

```JavaScript
// 方法一
let buffer = new ArrayBuffer(10);
let view1 = new DataView(buffer, 5, 2);
// 方法二
let view2 = new Int8Array(2);
let view3 = new Float32Array(4);
view2.byteLength;  // 2
view2.length;  // 2
view3.byteLength; // 16
view3.length;  // 4
// 方法三
let ints1 = new Int16Array([25, 80]);
let ints2 = new Int32Array(ints1);
ints1 === ints2; // false
ints1.length === ints2.length; // true
ints1.byteLength; // 4
ints2.byteLength; // 8
// 元素大小BYTES_PER_ELEMENT，表示每个元素的字节数
Uint8Array.BYTES_PER_ELEMENT; // 1
Int32Array.BYTES_PER_ELEMENT; // 4
let ints = new Int16Array(5);
ints.BYTES_PER_ELEMENT; // 2
```

#### 定型数组与普通数组的相似之处
1. 定型数组在多数情况下可以使用普通数组的方法，如length,通过数值型索引直接访问定型数组中的元素；（length只可访问不可修改）
2. 通用方法：copy(), findIndex(), lastIndexOf(), slice(), entries(), forEach(), map(), some(), fill(), indexOf(), reduce(), sort(), filter(), join(), reduceRight(), values(), find(), keys(), reverse()；
3. 相同的迭代器，都可以使用展开运算符和for-of语句；
4. of()和from()方法；

#### 定型数组与普通数组的差别
1. 定型数组的尺寸始终不变，读取不存在数值的索引会被忽略；
2. 定型数组会检查数据类型的合法性，0被用于代替所有非法值；

```JavaScript
let ints = new Int8Array(['hi']);
ints[0]; // 0
```

3. 在定型数组中不可使用的方法：concat(), shift(), unshift(), push(), pop(), slice()；
4. 定型数组中附加的方法：set()和subarray()；

```JavaScript
// set()方法将其他数组复制到已有的定型数组
// 两个参数：数组和一个可选的偏移量
let ints = new Int16Array(4);
ints.set([25, 50]);
ints.set([2, 10], 2);
ints.toString(); // '25,50,2,10'
// subarray()方法提取已有定型数组的一部分作为新的定型数组
// 两个参数：可选的开始位置和可选的结束位置
let uints = new Uint16Array([1, 3, 4]);
uints.subarray(1, 2).toString(); // '3'
```

## Promise与异步编程
#### 基础异步编程
1. 事件模型，直到事件触发时才执行事件处理程序；
2. 回调模式，回调模式中被调用的函数是作为参数传入了；

#### Promise基础知识
1. Promise的生命周期：
    1. 未处理unsettled -> 进行中pending -> 已处理settled；
    2. Fulfilled和Rejected；
    3. then()方法和catch()方法；
    4. thenable对象：实现了then()方法的对象；
2. 创建未完成的Promise：

```JavaScript
let promise = new Promise(function(resolve, reject) {
    console.log('aaa');
    resolve();
});

promise.then(function() {
    console.log('ccc');
});

console.log('bbb');
// 'aaa'
// 'bbb'
// 'ccc'
```

3. 创建已处理的Promise：

```JavaScript
// 已完成
let promise = Promise.resolve(42);
promise.then(function(res) {
    console.log(res); // 42
});
// 已拒绝
let promise = Promise.reject(42);
promise.catch(function(err) {
    console.log(err);
});
// 传入非Promise的thenable对象
let thenable = {
    then(resolve, reject) {
        reslove(42);
    }
};

let p1 = Promise.resolve(thenable);
p1.then(function(res) {
   console.log(res); // 42
});
```

4. 执行器错误：

```JavaScript
let promise = new Promise(function(resolve, reject) {
   throw new Error('error'); 
});
promise.catch(err => console.log(err));
```

#### 全局的Promise拒绝处理
Promise特性决定了很难检测一个Promise是否被处理过。
unhandledRejection：在一个事件循环中，当Promise被拒绝，并且没有提供拒绝处理程序时，触发该事件；
rejectHandled：在一个事件循环后，当Promise被拒绝时，若拒绝处理程序被调用，触发该事件；
1. Node.js环境的拒绝处理：

```JavaScript
let possiblyUnhandledRejections = new Map();

// 如果一个拒绝没被处理，添加到Map中
process.on('unhandledRejection', function(reason, promise) {
    possiblyUnhandledRejections.set(promise, reason);
});

process.on('rejectHandled', function(promise) {
    possiblyUnhandledRejections.delete(promise);
});

setInterval(function() {
    possiblyUnhandledRejections.forEach(function(reason, promise) {
       console.log(reason.message ? reason.message : reason);
       // 处理这些拒绝
       handleRejection(promise, reason);
    });
}, 50000);
```

2. 浏览器环境的拒绝处理：

```JavaScript
let possiblyUnhandledRejections = new Map();

// 如果一个拒绝没被处理，添加到Map中
window.onunhandledRejection = function(event) {
    possiblyUnhandledRejections.set(event.promise, event.reason);
};

window.onrejectHandled = function(event) {
    possiblyUnhandledRejections.delete(event.promise);
};

setInterval(function() {
    possiblyUnhandledRejections.forEach(function(reason, promise) {
       console.log(reason.message ? reason.message : reason);
       // 处理这些拒绝
       handleRejection(promise, reason);
    });
    
    possiblyUnhandledRejections.clear();
}, 50000);
```

#### 串联Promise
1. then串联：

```JavaScript
let promise = new Promise(function(resolve, reject) {
   resolve(1); 
});

promise.then(function(res) {
   console.log(res);
}).then(function(res) {
    console.log('finished ' + res);
});
// 1
// finished undefined
```

2. 捕获错误：务必在Promise链末尾有一个拒绝处理程序。

```JavaScript
let promise = new Promise(function(resolve, reject) {
   resolve(1); 
});

promise.then(function(res) {
   console.log(res);
   throw new Error('cannot');
}).catch(function(err) {
    console.log('finished ' + err);
});
// 1
// finished Error: cannot
```

3. Promise链的返回值：

```JavaScript
let promise = new Promise(function(resolve, reject) {
   resolve(1); 
});

promise.then(function(res) {
   console.log(res);
   return res + 1
}).then(function(res) {
    console.log('finished ' + res);
});
// 1
// finished 2
```

4. Promise链中返回Promise：

```JavaScript
let promise1 = new Promise(function(resolve, reject) {
   resolve(1); 
});

let promise2 = new Promise(function(resolve, reject) {
   resolve(2); 
});

promise1.then(function(res) {
   console.log(res);
   return promise2;
}).then(function(res) {
    console.log('finished ' + res);
});
// 1
// finished 2
```

#### 响应多个Promise
1. Promise.all()，一个被拒绝就返回拒绝，全部都完成才返回完成；

```JavaScript
let promise1 = new Promise(function(resolve, reject) {
   resolve(1); 
});

let promise2 = new Promise(function(resolve, reject) {
   resolve(2);
   // reject(2);
});

let promise3 = new Promise(function(resolve, reject) {
   resolve(3); 
});

let promise4 = Promise.all([promise1, promise2, promise3]);

promise4.then(function(res) {
    console.log(res);
});
// [1, 2, 3]
// Uncaught (in promise) 2
```

2. Promise.race()，只要有一个返回完成就返回完成；

```JavaScript
let promise1 = new Promise(function(resolve, reject) {
   resolve(1); 
   // reject(1);
});

let promise2 = new Promise(function(resolve, reject) {
   resolve(2);
});

let promise3 = new Promise(function(resolve, reject) {
   resolve(3); 
});

let promise4 = Promise.race([promise1, promise2, promise3]);

promise4.then(function(res) {
    console.log(res);
});
// 1
// Uncaught (in promise) 1
```

#### 自Promise继承

```JavaScript
class MyPromise extends Promise {
    success(resolve, reject) {
        return this.then(resolve, reject);
    }
    
    failure(reject) {
        return this.catch(reject);
    }
}

let myPromise = new MyPromise(function(resolve, reject) {
   resolve(11); 
});
let myPromise2 = MyPromise.resolve(myPromise);

myPromise.success(function(res) {
   console.log(res) 
});
// 11
myPromise2.success(function(res) {
   console.log(res) 
});
// 11
myPromise2 instanceof MyPromise; // true---思考原因
```

#### 异步任务执行
用async代替*，用await代替yield来调用函数；

```JavaScript
(async function() {
   await ...; 
}); 
```

## 代理Proxy和反射Reflection
#### 代理和反射基础
1. 代理是一种可以拦截并改变底层JavaScript引擎操作的包装器，通过它暴露内部运作的对象；
2. 反射API以Reflect对象的形式出现，对象中方法的默认特性与相同的底层操作一致，而代理可以覆写这些操作，每个代理陷阱对应一个命名和参数都相同的Reflect方法；
3. 代理的创建：Proxy构造函数传入目标target和处理程序handler两个参数；

#### 用set验证属性
set陷阱来覆写设置值的默认特性，接受4个参数：
1. trapTarget 用于接收属性（代理的目标）的对象；
2. key 要写入的属性值（字符串或Symbol类型）；
3. value 要写入属性的值；
4. receiver 操作发生的对象（通常是代理）。

```JavaScript
let target = {
    name: 'target'
};
let handler = {
    set(target, key, value, receiver) {
        if(!target.hasOwnProperty(key)) {
            if(isNaN(value)) {
                throw new Error('属性的值必须是数字');
            }
        }
        
        // 添加属性
        return Reflect.set(target, key, value, receiver);
    }
};
let proxy = new Proxy(target, handler);
// 属性的值为数字
proxy.count = 1;
proxy.count; // 1
target.count; // 1
// 存在的属性
proxy.name = 'proxy';
proxy.name; // 'proxy'
target.name; // 'proxy'
// 不存在的属性且值不是数字
proxy.type = 'type'; // Error
```

#### 用get陷阱验证结构对象
无论对象中是否存在某个属性，都可以通过get陷阱来检测，3个参数：
1. target 被操作属性的源对象（代理的目标）；
2. key 要读取的属性值（字符串或Symbol）；
3. receiver 操作发生的对象（通常是代理）。

```JavaScript
let target = {
    name: 'target'
};
let handler = {
    get(target, key, receiver) {
        if(!(key in target)) {
            throw new Error(`属性${key}不存在`);    
        }
        
        return Reflect.get(target, key, receiver);
    }
}

let proxy = new Proxy(target, handler);
// 属性存在时
proxy.name; // 'target'
target.name; // 'target'
// 属性不存在时
proxy.nme; // Error
target.nme; // undefined
```

#### 用has陷阱隐藏已有属性
每当使用in操作符都会调用has陷阱，2个参数：
1. target 读取属性的对象（代理的目标）；
2. key 要检查的属性键（字符串或Symbol）。

```JavaScript
let target = {
    name: 'target',
    count: 10
};
let handler = {
    has(target, key) {
        if(key === 'name') {
            return false;
        }
        
        return Reflect.has(target, key);
    }
};

let proxy = new Proxy(target, handler);
// 隐藏对象的name属性
'name' in proxy; // false
'count' in proxy; // true
```

#### 用deleteProperty陷阱防止删除属性
每当通过delete操作符删除对象属性时，deleteProperty陷阱会被调用，2个参数：
1. target 删除属性的对象（代理的目标）；
2. key 要删除的属性键（字符串或Symbol）；

```JavaScript
let target = {
    name: 'target',
    count: 10
};
let handler = {
    deleteProperty(target, key) {
        if(key === 'name') {
            return false;
        }
        
        return Reflect.deleteProperty(target, key);
    }
};

let proxy = new Proxy(target, handler);
delete proxy.name; // false
'name' in proxy; // true
'name' in target; // true
delete proxy.count; // true
'count' in proxy; // false
'count' in target; // false
```

#### 原型代理陷阱
通过代理中的setPrototypeOf陷阱和getPrototype陷阱拦截Object.setPrototypeOf方法和Object.getPrototypeOf方法，两个参数：
1. target 接受原型设置的对象（代理的目标）；
2. proto 作为原型使用的对象。

```JavaScript
let target = {};
let handler = {
  setPrototypeOf(target, proto) {
      return Reflect.setPrototypeOf(target, proto);
  },
  getPrototypeOf(target, proto) {
      return false;
      // return Reflect.getPrototypeOf(target, proto);
  }
};

let proxy = new Proxy(target, handler);
// 允许设置原型，不允许读取原型
Object.setPrototypeOf(proxy, {}); // Proxy {}
Object.getPrototypeOf(proxy); // 报错
Object.getPrototypeOf(target); // {}
```

Object.setPrototypeOf和Reflect.setPrototypeOf，如果失败，前者报错后者返回布尔值；
Object.getPrototypeOf和Reflect.getPrototypeOf，如果传入参数不是对象，前者会先将参数强制转换为一个对象，后者报错。

#### 对象可扩展性陷阱
isExtensible检测对象是否可扩展，preventExtensions使对象不可扩展，1个参数：
1. target 可扩展性操作的对象（代理的目标）。

```JavaScript
let target = {};
let handler = {
  isExtensible(target) {
      return Reflect.isExtensible(target);
  },
  preventExtensions(target) {
      return false;
      // return Reflect.preventExtensions(target)
  }
};

let proxy = new Proxy(target, handler);
// 不允许改变对象的可扩展性
Object.isExtensible(proxy); // true
Object.preventExtensions(proxy);
Object.isExtensible(proxy); // true
```

Object.isExtensible和Reflect.isExtensible，传入非对象值时，前者返回false，后者报错；Object.preventExtensions和Reflect.preventExtensions，参数不是对象时，前者返回该参数，后者报错。

#### 属性描述符的陷阱
defineProperty陷阱拦截Object.defineProperty方法，3个参数：
1. target 要定义属性的对象（代理的目标）；
2. key 属性的键（字符串或Symbol）；
3. descriptor 属性的描述符对象。

getOwnPropertyDescriptor陷阱拦截Object.getOwnPropertyDescriptor()方法，2个参数：
1. target 要获取定义属性的对象（代理的目标）；
2. key 属性的键（字符串或Symbol）。

```JavaScript
// 默认行为
let target = {};
let handler = {
  defineProperty(target, key, descriptor) {
    console.log('defineProperty');
    return Reflect.defineProperty(target, key, descriptor);   
  },
  getOwnPropertyDescriptor(target, key) {
    console.log('getOwnPropertyDescriptor');
    return Reflect.getOwnPropertyDescriptor(target, key);
  }
};

let proxy = new Proxy(target, handler);
Object.defineProperty(proxy, 'name', {
   value: 'proxy' 
});
Object.getOwnPropertyDescriptor(proxy, 'name').value;
// defineProperty
// getOwnPropertyDescriptor
// 'proxy'
```

描述符对象限制：
1. 无论将什么对象作为第三个参数传入Object.defineProperty()方法，只有enumable、configurable、value、writable、get和set将出现传递给defineProperty陷阱的描述符对象中；
2. getOwnPropertyDescriptor陷阱的返回值必须是null、undefined或一个对象。如果返回对象，则对象的属性只能是enumable、configurable、value、writable、get和set，否则报错。

重复的描述符方法：
1. Object.defineProperty()和Reflect.defineProperty()方法只有返回值不同，前者返回第一个参数，后者返回操作的布尔值；
2. Object.getOwnPropertyDescriptor()和Reflect.getOwnPropertyDescriptor()方法，传入原始值作为第一个参数时，前者会强制转换为一个对象，后者会报错。

#### ownkeys陷阱
拦截内部方法[[ownPropertyKeys]]，接受唯一的参数是操作的目标，返回值必须是一个数组或类数组对象，否则会报错。拦截方法：Object.keys()、Object.getOwnPropertyNames()、Object.getOwnPropertySymbols()和Object.assign()。

```JavaScript
let target = {
    name: 'target',
    _name: 'private',
    [Symbol]: 'symbol'
};
let handler = {
    ownKeys(target) {
        return Reflect.ownKeys(target).filter((key) => {
           return key[0] !== '_'; 
        });
    }
}

let proxy = new Proxy(target, handler);
Object.keys(proxy); // ['name', Symbol]
Object.getOwnPropertyNames(proxy); // ['name', Symbol]
```

#### 函数代理中的apply和construct陷阱
所有代理中只有apply和construct的代理目标是一个函数。若用new则执行[[construct]]方法，否则执行[[call]]方法，执行apply陷阱。
apply接受3个参数：
1. target 被执行的函数（代理的目标）；
2. thisArg 函数被调用时内部this的值；
3. argumentsList 传递给函数的参数数组；

construct接受2个参数：
1. target 被执行的函数（代理的目标）；
2. arguments 传递给函数的参数数组；
Reflect.construct还接受第三个可选参数newTarget，用于指定函数内部new.target的值。


```JavaScript
// apply
function sum(...args) {
    return args.reduce((reduce, current) => reduce + current, 0);
}

let handler = {
    construct(target, argumentsList) {
        throw new Error('不可使用new调用');
        // return Reflect.construct(target, argumentsList)
    },
    apply(target, thisArg, argumentsList) {
        argumentsList.forEach((arg) => {
            if(typeof arg !== 'number') {
                throw new Error('参数必须为数字');
            }    
        });
        
        return Reflect.apply(target, thisArg, argumentsList);
    }
}

let sumProxy = new Proxy(sum, handler);
sumProxy(1, 2, 3); // 6
sumProxy(1, '2', 3); // Error
```

#### 可撤销代理
在创建代理后，代理不能脱离其目标。可用Proxy.revocable()方法创建可撤销的代理。

```JavaScript
let target = {
    name: 'target'
};
let handler = {};
let { proxy, revoke } = Proxy.revocable(target, handler);
proxy.name; // 'target'
revoke();
proxy.name; // 报错
```

#### 创建使用代理的类

```JavaScript
// 构造函数中返回代理
class Thing {
    constructor() {
        return new Proxy(this, {});
    }
}
```

#### 将代理用作原型
如果代理是原型，仅当默认操作（set、get和has）继续执行到原型上时才会调用代理陷阱：

```JavaScript
let target = {};
let handler = {
  defineProperty(target, key, descriptor) {
      return false;
  }  
};

let newTarget = Object.create(new Proxy(target, handler));
Object.defineProperty(newTarget, 'name', {
   value: 'newTarget' 
});

newTarget.name; // 'newTarget' （代理未生效）
```

1. 在原型上使用set陷阱；
2. 在原型上使用get陷阱；
3. 在原型上使用has陷阱。

#### 将代理用作类的原型
由于类的prototype属性不可写，因此不能直接修改类来使用代理作为类的原型，但是可以通过集成的方法来让类误以为自己可以将代理用作自己的原型。

```JavaScript
function NoSuchProperty() {};

NoSuchProperty.prototype = new Proxy({}, {
   get(target, key, receiver) {
       throw new Error(`${key} doesn't exist`);
   }
});

class Square extends NoSuchProperty {
    constructor(width, height) {
        super();
        this.width = width;
        this.height = height;
    }
    getArea() {
        return this.width * this.height;
    }
}

let shape = new Square(2, 6);
shape.width * shape.height; // 12
shape.getArea(); // 12
shape.width * shape.hight; // Uncaught Error: hight doesn't exist
```


## 用模块封装代码
模块是自动运行在严格模式下并没有办法退出运行的JavaScript代码。
脚本就是任何不是模块的JavaScript代码。
模块特点：1.在模块的顶部，this的值是undefined；2.模块不支持HTML风格的代码注释。

#### 模块的导入导出
必须在其他语句和函数之外使用。
1. 导出的基本语法：

```JavaScript
// 导出数据
export var color = 'red';
// 导出函数
export function sum(num1, num2) {
    return num1 + num2;
}
export 函数名;
// 导出类
export class Person {
    // ...
}
```

2. 导入的基本语法：

```JavaScript
// 导入单个绑定
import { sum } from './example.js';
// 导入多个绑定
import { sum, multiply } from './example.js';
// 导入整个模块
import * as example from './example.js';
// 导入的绑定列表不是解构
// 不管import语句把一个模块写了多少次，该模块只执行一次
```

3. as关键字实现重命名；
4. 模块的默认值default；
5. 重新导出一个绑定：

```JavaScript
import { sum } from './example.js';
export sum;
// 等价于
export { sum } from './example.js';
```

#### 加载模块
1. type属性值为"module"时支持加载模块：

```HTML
<script type="module" src="module.js"></script>
<!-- 或者 -->
<script type="module">
    import {sum} from './module.js';
</script>
```

2. 模块加载顺序：默认defer，一但遇到就开始下载模块文件，直到文档被完全解析模块才会执行；
3. 浏览器异步模块加载：async，脚本文件将在文件完全下载并解析后执行；
4. 将模块作为worker加载：

```JavaScript
let work = new Work('module.js', {type: 'module'});
```

5. 浏览器模块说明符：
    1. 以/开头的解析为从根目录开始；
    2. 以./开头的解析为从当前目录开始；
    3. 以../开头的解析为父目录开始；
    4. URL格式；

## ES6中较小的改动
#### 整数相关
1. 识别整数Number.isInteger;

```JavaScript
Number.isInteger(25); // true
Number.isInteger(25.0); // true
Number.isInteger(25.1); // false
```

2. 安全整数Number.isSafeInteger;

```JavaScript
let max = Number.MAX_SAFE_INTEGER;
let min = Number.MIN_SAFE_INTEGER;
Number.isSafeInteger(max); // true
Number.isSafeInteger(max + 1); // false
Number.isSafeInteger(min); // true
Number.isSafeInteger(min - 1); // false
```

3. 新的Math方法；

#### Unicode标识符

```JavaScript
// 写法均合法
var \u0061 = 'abc';
var \u{61} = 'def';
```

#### __proto__相关
1. 只能在对象字面量中指定一次__proto__，否则抛出错误；
2. 可计算形式__proto__的行为类似于普通属性，不会设置或返回当前对象的原型；
3. __proto__可直接设置对象字面量的原型；

## 了解ECMAScript7（2016）
1. 指数运算符**：

```JavaScript
let x = 2;
x ** 3; // 8
x++**3; // 8
++x**3; // 64
```

2. Array.prototype.includes()方法：

```JavaScript
let arr = [1, NaN, 3, -0];
arr.includes(NaN); // true
arr.indexOf(NaN); // -1
arr.includes(+0); // true
arr.indexOf(+0); // 3
```

3. 在参数被解构或有默认参数的函数中禁止使用'use strict'指令；