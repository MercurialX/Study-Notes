## 初识JavaScript
#### 起源
JavaScript语言的前身是LiveScript，由美国Netscape公司的布瑞登·艾克为在1995年发布的Navigator2.0浏览器的应用而开发的脚本语言。

#### 主要特点
1. 解释性；
2. 基于对象；
3. 事件驱动；
4. 跨平台；
5. 安全性；

#### 基本语法
1. 按照在HTML文件中出现的顺序逐行执行；
2. 大小写敏感；
3. 每行结尾的分号可有可无；

## JavaScript基础
#### 数据结构
1. 标识符：就是一个名称，用来命名变量和函数，或者用作某些循环的标签。首字母不允许为数字，不能和JavaScript关键字同名；
2. 关键字：在JavaScript语言中有特定含义，成为JavaScript语法一部分的那些字；
3. 常量：值不能改变的量；
4. 变量：程序中一个已经命名的存储单元，为数据操作提供存放信息的容器；

#### 数据类型
1. 数字型；
2. 字符串型；
3. 布尔型；
4. undefined；
5. null；  
 
思考：数据类型的转换。

#### 运算符
1. 算术运算符；
2. 比较运算符；
3. 赋值运算符；
4. 逻辑运算符；
5. 条件运算符；
6. 其他运算符：位操作运算符，typeof，new；  

思考：各运算符的优先级。

#### 表达式
表达式是一个语句集合，像一个组一样，计算结果是个单一值，然后该结果被JavaScript归入下列数据类型之一：boolean，number，string，function或者object。

## 流程控制
1. 赋值语句；
2. 条件判断语句：if语句，if...else语句，switch语句；
3. 循环控制语句：while语句，do...while语句，for循环；
4. 跳转语句：continue语句，break语句；

## 函数
#### 函数的定义
1. 函数是由关键字function，函数名加一组参数以及置于大括号中需要执行的一段代码定义的；

```JavaScript
function functionName(parameter1, parameter2) {
    statements;
    [return expression];
}
```

2. Function构造函数；
3. 函数直接量；


#### 函数的调用
1. 直接调用；
2. 事件响应中调用；
3. 通过链接调用；

#### 函数参数
1. 形参：定义函数时指定的参数；
2. 实参：调用函数时实际传递的值；

#### 函数的返回值
在函数中使用return将需要返回的值赋予到变量，最后将此变量返回；

#### 嵌套函数
在函数内部再定义一个函数；

#### 递归函数
函数在自身的函数体内调用自身；

#### 内置函数
由JavaScript语言自身提供的函数：eval()，isFinite()，isNaN()，parseInt()，parseFloat()，encodeURI()，encodeURIComponent()，decodeURI()，decodeURIComponent()。

## JavaScript对象与数组
#### JavaScript内部对象
1. Object对象
   1. 创建：new Object([value])；
   2. 属性：prototype，constructor；
   3. 方法：toLocalString()，toString()，valueOf()；
2. String对象
   1. 创建：new String(String Text)；
   2. 属性：length，constructor，prototype；
   3. 方法：anchor()，big()，small()，fontsize()，bold()，italics()，link()，strike()，blink()，fixed()，charAt()，charCodeAt()，concat()，fontcolor()，fromCharCode()，indexOf()，lastIndexOf()，localCompare()，match()，replace()，search()，split()，sunstr()，sunstring()，slice()，sub()，sup()，toLocalLowerCase()，toLocalUpperCase()，toLowerCase()，toUpperCase()，toSource()，valueOf()；
3. Date对象
   1. 创建：new Date()或new Date(dateVal)或new Date(year, month, day...)；
   2. 属性：constructor，prototype；
   3. 方法：Date()，getDate()，getDay()，getMonth()，gerFullYear()，getYear()，getHours()，getMinutes()，getSeconds()，getMilliseconds()，getTime()，getTimezoneOffset()，getUTCDate()，getUTCDay()，getUTCMonth()，getUTCFullYear()，getUTCHours()，getUTCMinutes()，getUTCSeconds()，getUTCMilliseconds()，parse()，setDate()，setMonth()，setFullYear()，setYear()，setHours()，setMinutes()，setSeconds()，setMilliseconds()，setTime()，setUTCDate()，setYTCMonth()，setUTCFullYear()，setUTCHours()，setUTCMinutes()，setUTCSeconds()，setUTCMilliseconds()，toSource()，toString()，toTimeString()，toDateString()，toGMTString()，toUTCString()，toLocaleString()，toLocaleTimeString()，toLocaleDateString()，UTC()，valueOf()；
4. Event对象
   1. 创建：event或window.event；
   2. 属性：altLeft，ctrlLeft，shiftLeft，button，clientX，clientY，X，Y，srcElement，
5. FileSystemObject对象
   1. 创建：new ActiveXObject("Scripting.FileSystemObject")；
   2. 方法：GetAbsolutePathName()，GetBaseName()，GetDriveName()，GetDrive()，GetExtensionName()，GetFileName()，GetParentFolderName()，GetTempName()；
6. Drive对象
   1. 属性：FreeSpace，IsReady，TotalSize，DriveType，SerialNumber，AvailableSpace，FileSystem，Path，RootFolder，ShareName，VolumeName；
7. File对象
   1. 属性：Attributes，DateCreated，DateLastAccessed，DateLastModified，Name，Size，Type，ShortName，parentFolder，Path；
   2. 方法：Copy()，Delete()，Move()，OpenAsTextStream()；
8. Folder对象
   1. 属性：Files，isRootFolder；

#### 对象访问语句
1. for...in语句；
2. with语句；

#### JavaScript中的数组
1. 数组的创建，输入/输出；
2. Array对象的属性：length和prototype；
3. Array对象的方法：concat()，pop()，push()，shift()，unshift()，reverse()，sort()，slice()，toSource()，toString()，toLocalString()，join()，valueOf()；

## 字符串和数值处理对象
#### 字符串对象方法
1. match()；
2. search()；
3. replace()；
4. split()；

#### Math对象
1. 属性：E，LN2，LN10，SQRT2；
2. 方法：abs(x)，acos(x)，asin(x)，atan(x)，atan2(x, y)，ceil(x)，cos(x)，exp(x)，floor(x)，log(x)，max(x, y)，min(x, y)，pow(x, y)，randow()，round(x)，sin(x)，sqrt(x)，tan(x)；

#### Number对象
1. 创建：new Number(value)；
2. 属性：MAX_VALUE，MIN_VALUE，NEGTIVE_INFINITY，POSITIVE_INFINITY；
3. 方法：toString()，toLocalString()，toFixed()，toExponential()，toPrecision()；

#### Boolean对象
1. 创建：new Boolean([boolValue])；
2. 属性：constructor，prototype；
3. 方法：toString()，valueOf()；

## 正则表达式
#### 基础
1. 基本结构：由普通字符以及特殊字符（称为元字符）组成的文字模式；
2. 作用：用于匹配和替换的强有力的工具；

#### 语法
正则表达式的语法主要就是从各个元字符功能的描述。
1. 模式匹配符：\，^，$，*，+，?，.，(x)，x|y，{n}，{n,}，{n, m}，[x, y, z]，[^xyz]，[\b]，\b，\B，\cX，\d，\D，\f，\n，\r，\s，\S，\v，\t，\w，\W，\n，\ooctal和\xhex；
2. 定位符和原义字符：
   1. 用"^"匹配目标字符串的开始位置；
   2. 用"$"匹配目标字符串的结尾位置；
   3. 用"\b"匹配一个字边界；
   4. 用"\B"匹配非字边界；
3. 限定符：
   1. 用"+"限定必须出现一次或连续多次；
   2. 用"*"限定可以出现的次数；
   3. 用"?"限定最多出现一次；
   4. 用"{n}"限定连续出现的次数；
   5. 用"{n,}"限定至少出现的次数；
   6. 用"{n, m}"限定最少和最多出现的次数；
4. 贪婪匹配与非贪婪匹配：默认情况下，正则表达式使用最长匹配原则，如果当字符"?"紧跟任何其他限定符之后时，匹配模式变成使用最短匹配原则；
5. 选择匹配符：选择匹配符"|"，用于选择匹配两个选项之中的任意一个，其两个选项是"|"字符两边尽可能最大的表达式；
6. 特殊字符：
   1. "\n"中的n是一个一位的八进制数（0～7）；
   2. "\nm"中的n和m是一个一位的八进制数（0～7）；
   3. "\nml"中，当n是八进制数（0～3），m和l是八进制数（0～7）时，匹配ASCII码值等于八进制的nml；
   4. "\un"匹配Unicode编码等于n的字符；
   5. "\xn"匹配ASCII码值等于n的字符；
   6. "\cx"匹配由x指定的控制字符；
7. 字符匹配符：
   1. "[...]"匹配方括号中包含的字符集中的任意一个字符；
   2. "[^...]"匹配方括号中未包含的任意字符；
   3. "[a-z]"匹配指定范围内的任意字符；
   4. "[^a-z]"匹配不在指定范围内的任意字符；
   5. "\w"匹配任何单字字符；
   6. "\W"匹配任何非单字字符；
   7. "\s"匹配任何空白字符；
   8. "\S"匹配任何非空白字符；
   9. "\d"匹配任何一个数字字符；
   10. "\D"匹配任何一个非数字字符；
   11. "."匹配除"\n"之外的任何单个字符；
   12. "()"标记一个子表达式的开始和结束位置；
8. 分组组合与反向引用符：
   1. 分组符号"(pattern)"；
   2. 反向引用"\num"；
   3. 非捕获匹配"(?:pattern)"；
   4. 正向“预测先行”匹配"(?=pattern)"；
   5. 反向“预测先行”匹配"(?!pattern)"；  

思考：正则表达式的实际应用。

#### RegExp对象
1. 创建：new RegExp("pattern"[, "flags"])或/pattern/[flags]；
2. 属性：
   1. 静态属性：index, input, lastIndex, lastMatch, lastParen, leftContext, rightContext以及从$1到$9；
   2. 实例属性：global, ignoreCase, multiline, source；
3. 方法：exex()，test()，match()，search()，replace()，split()；

## 程序调试和错误处理
#### 处理异常
1. 方式：onerror和try...catch...finilly；
2. 异常类型：语法异常，运行时异常和逻辑异常；

#### 调试技巧
1. alert()；
2. write()语句调试；
3. 使用抛出自定义异常消息；

## 事件处理
#### JavaScript常用事件
1. 鼠标键盘事件：onclick, ondbclick, onmousedown, onmouseup, onmouseover, onmousemove, onmouseout, onkeypress, onkeydown, onkeyup；
2. 页面相关事件：onabort, onbeforeunload, onerror, onload, onresize, onunload；
3. 表单相关事件：onblur, onchange, onfocus, onreset, onsubmit；
4. 滚动字幕事件：onbounce, onfinish, onstart；
5. 编辑事件：onbeforecopy, onbeforecut, onbeforeeditfocus, onbeforepaste, onbeforeupdate, oncontextmenu, oncopy, oncut, ondrag, ondragend, ondragenter, ondragleave, ondragover, ondragstart, ondrop, onlosecapture, onpaste, onselect, onselectstart；
6. 数据绑定事件：onafterupdate, oncellchange, ondateavailable, ondatasetchanged, ondatasetcomplete, onerrorupdate, onrowenter, onrowexit, onrowsdelete, onrowsinserted；
7. 外部事件：onafterprint, onbeforeprint, onfilterchange, onhelp, onpropertychange, onreadystatechange；

#### DOM事件模型
1. 事件流：事件在元素节点与根节点之间的路径传播，路径所经过的节点都会收到该事件；
2. 事件模型：冒泡型事件，捕获型事件，DOM标准事件模型；
3. 注册与移除事件监听器；

```JavaScript
// IE下
element.attachEvent('onclick', observer);
element.detachEvent('onclick', observer);

// DOM标准
element.addEventListen('click', observer, useCapture);
element.removeEventListen('click', observer, useCapture);
```

4. 阻止冒泡：stopPropagation()，取消默认：preventDefault()；

## document对象
文档对象代表浏览器窗口中的文档，该对象是window对象的子对象。

#### document对象的属性
alinkColor, all[], anchors[], bgColor, cookie, fgColor, forms[], fileCreatedDate, fileModifiedDate, fileSize, lastModified, images[], linkColor, links[], vlinkColor, title, body, readyState, URL

#### document对象的方法
close, open, write, writeln, createElement, getElementById

#### document对象的事件
onabort, onblur, onchange, onclick, ondbclick, onerror, onfocus, onkeydown, onkeypress, onkeyup, onload, onmousedown, onmousemove, onmouseout, onmouseover, onmouseup, onreset, onresize, onscroll, onselect, onsubmit, onunload

## 文档对象模型（DOM对象）
#### DOM分层
每一个对象都可以称为一个节点。
1. 根节点：最顶端的<html>节点；
2. 父节点：一个节点之上的节点是该节点的父节点；
3. 子节点；
4. 兄弟节点；
5. 后代：一个节点的子节点的结合；
6. 叶子节点：在树形结构最底层的节点；

文档模型中节点的3种类型：
1. 元素节点；
2. 文本节点；
3. 属性节点；

#### DOM对象节点属性
nodeName, nodeValue, nodeType, parentNode, childNodes, firstChild, lastChild, previousSibling, nextSibling, attributes

思考：遍历文档树通过parentNode, firstChild, lastChild, previousSibling和nextSibling来实现。

#### 节点
1. 创建节点：

```JavaScript
// 添加到当前节点的末尾
obj.appendChild(newChild);

let p = document.createElement('p');
let text = document.createTextNode('hello');
p.appendChild(text);
document.body.appendChild(p);

// createDocumentFragment()
let cdf = document.createDocumentFragment();
```

2. 插入节点：

```JavaScript
// 新的子节点new插入到ref节点末尾
obj.insertBefore(new, ref);
```

3. 复制节点：

```JavaScript
// true为深度复制
obj.cloneNode(deep);
```

4. 删除节点：

```JavaScript
// obj为父节点
obj.removeChild(oldChild);
```

5. 替换节点：

```JavaScript
obj.replaceChild(new, old);
```

#### 获取文档中指定元素
1. 通过id获取：document.getElementById()；
2. 通过name获取：document.getElementByName()

#### DHTML对应的DOM
属性：innerHTML, innerText, outerHTML, outerText

## window对象
#### 属性
document, frames, location, name, status, defaultstatus, top, parent, opener, closed, self, screen, navigator

#### 方法
alert(), confirm(), prompt(), open(), close(), focus(), scrollTo(x, y), scrollBy(offsetx, offsety), setTimeout(timer), setInterval(interval), moveTo(x, y), moveBy(offsetx, offsety), resizeTo(x, y), resizeBy(offsetx, offsety), print(), navigator(URL), status(), defaultstatus()

#### 窗口事件
1. 通用窗口事件：onfocus, onblur, onload, onunload, onresize, onerror；
2. 扩展窗口事件：onafterprint, onbeforeprint, onbeforeunload, ondragdrop, onhelp, onresizeend, onresizestart, onscroll；

#### IE浏览器窗口扩展
1. 模式窗口：window.showModalDialog(对话框URL, 参数，特征)；
2. 无模式窗口：window.showModellessDialog(对话框URL, 参数，特征)；
3. 弹出窗口：window.showModalDialog(对话框URL, 参数，特征)；

## style对象
#### 属性
dom.style.属性

#### 方法
getAttribute, getAttributeNode, getExpression, normalize, removeAttribute, removeAttributeNode, removeExpression, setAttribute, setAttributeNode, setExpression；

## 表单和表单元素
#### 表单标记<form>
1. 处理程序action属性；
2. 表单名称name属性；
3. 提交方式method属性；
4. 编码方式enctype属性；
5. 目标显示方式target属性；

#### 输入标记<input>
1. 文字域text；
2. 密码域password；
3. 单选按钮radio；
4. 复选框checkbox；
5. 提交按钮submit；
6. 普通按钮button；
7. 重置按钮reset；
8. 图像域image；
9. 隐藏域hidden；
10. 文本域file；

#### 文本域标记<textarea>
#### 菜单和列表标记<select>, <option>

## 应用
#### 页面打印

```
<object id ="web" classid="CISID:8856F961-340A-11D0-A96B-00C04Fd705A2" width=0 height=0></object>

WebBrowser.ExecWB(id, opt[, pvaIn][, pvaOut]);

```

#### Cookie
1. 形式：name=value；
2. 属性：name, expires, path, domain和secure；
3. 优点：持久性，操作简单；
4. 缺点：安全性较低，可以被禁用，可以被删除，不可共享；
5. 写入：document.cookie = cookie；
6. 读取：document.cookie；
7. 删除：实质上是使Cookie值过期；

#### Image对象
1. 属性：border, height, hspace, lowsrc, name, src, vspace, width, alt；
2. 预装载：

```JavaScript
let img = new Image();
img.src = 'url';
```

#### 浏览器检测
1. 属性： appName, appVersion, userAgent, appCodeName, platform；
2. 子对象：MimeType, Plugin对象；

#### MIME类型
1. 文本文件text；
2. 多类型multipart；
3. 不同类型的消息message；
4. 应用类型application；
5. 图像image；
6. 声音audio；
7. 影像video；

#### JavaScript的安全
1. 同源策略：协议相同，端口相同，域名相同；
2. 屏蔽部分按键；

```JavaScript
function keydown() {
    // 8退格键，13回车键，116刷新键...
    if(event.keyCode === 8) {
        event.keyCode = 0;
        event.returnValue = false;
    }
}
```

3. 屏蔽鼠标右键，event.button === 2；
4. 禁止网页另存为，noscript；
5. 禁止复制网页内容，onselectstart='return false;'；
6. 信息-摘要算法MD5：将一个字符串或汉字等，通过函数转换为另一个新的字符串；

#### Ajax
1. 初始化：new XMLHttpRequest()；
2. 常用方法：
   1. open('method', 'URL'[, asyncFlag, userName, password])；
   2. send(content)；
   3. setRequestHeader('header', 'value')；
   4. absort()；
   5. getResponseHeader('headerLabel')；
3. 常用属性：
   1. onreadystatechange；
   2. readyState：等于4是完成；
   3. responseText；
   4. status：200代表成功；

```JavaScript
var xmlhttp = false;
if(window.ActiveXObject) {
    xmlhttp = new ActiveXObject('Microsoft.XMLHttp');
}else if(window.XMLHttpRequest) {
    xml = new XMLHttpRequest();
}

xmlhttp.open('POST', url, true);
xmlhttp.setRequestHeader('Content-Type', 'application/x-www-form-urlencodeed');
xmlhttp.onreadystatechange = function() {
    if(xmlhttp.readyState === 4 && xmlhttp.stauts === 200) {
        // 成功代码
    }
};
xmlhttp.send(url参数);
```
#### Jquery
1. 选择器：
   1. ID选择器（#ID）；
   2. 元素选择器（element）；
   3. 类名选择器（.class）；
   4. 复合选择器（selector1, selector2, selector3）；
   5. 通配符选择器（*）；
   6. 层级选择器：祖孙（an de），父子（parent > child），紧接着（prev + next），匹配之后所有siblings（prev ~ siblings）；
   7. 过滤选择器；
   8. 属性选择器；
   9. 表单选择器；
2. 元素内容操作：text(), text(val)；
3. HTML内容操作：html(), html(val)；
4. 对元素值操作：val(), val(val)；
5. 对DOM节点操作：append(), appendTo(), prepend(), prependTo(), after(), insertAfter(), before(), insertBefore(), empty(), remove(), clone(), replaceAll(), replaceWith()；
6. 对元素属性操作：attr(), removeAttr()；
7. 对元素的CSS样式操作：addClass(), removeClass(), toggleClass(), css()；
8. 页面加载响应事件：$(document).ready()；
9. Jquery事件：blur(), change(), click(), dbclick(), error(), focus(), keydown(), keyup(), keypress(), load(), mousedown(), mouseover(), mouseout(), mousemove(), mouseup(), resize(), scroll(), select(), submit(), unload()；
9. 事件绑定：bind(), unbind(), one()；
10. 模拟用户操作：triggerHandler(), trigger(), hover(), toggle()；
11. 基本动画效果：hide(), show(), toggle(), fadeIn(), fadeOut(), fadeTo(), slideDown(), slideUp(), slideToggle();
12. 自定义动画效果：animate(), stop()；