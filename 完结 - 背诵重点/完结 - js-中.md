# `JavaScript`

## 1.`Object`对象

所有对象都继承自`Object`对象，都是`Object`对象的实例。

- 静态方法，直接定义在`Object`对象上 
- 实例方法，定义在`Object`原型对象上，`Object.prototype`上的属性和方法，被所有实例对象共享

```js
// 静态方法
Object.print = function(n) console.log(n); 
```

```js
// 实例方法
Object.prototype.print = function() console.log(this); 
var obj = new Object();
obj.print();
```

### 构造函数

```js
var obj = new Object();
var obj = {};
```

参数问题：

- 当`Object`构造函数的参数是对象时，返回该对象
- 是原始类型值时，返回该值对应的包装对象

```js
var obj = new Object(123);
obj instanceof Number // true
```

### 方法

#### `Object.keys()`

参数是一个对象，用以遍历对象==自身==键名。

#### `Object.getOwnPropertyNames()`

与`keys()`相似，区别在于：

- `Object.keys`只返回可枚举的属性名
- `Object.getOwnPropertyNames`还能返回不可枚举的属性名

```js
var a = ['西瓜', '芒果'];
Object.getOwnPropertyNames(a) // ["0","1","length"]
```

#### `Object.prototype.valueof()`

默认返回对象本身。

在自动类型转换和对象与数字相加时，默认调用这个方法。

#### `Object.prototype.toString()`

返回==对象的字符串==形式，默认返回类型字符串。

```js
var o1 = {a:1};
o1.toString() // "[object Object]"
```

对象与字符串相加时，自动调用`toString`方法。

==注意==：数组、函数、`Date`对象调用`toString`方法，并不会返回`[object Object]`，因为它们都自定义了`toString`方法，覆盖掉了原始方法。

#### `Object.prototype.toLocaleString()`

```js
var date = new Date();

date.toString() // "Tue Jan 01 2018 12:01:33 GMT+0800 (CST)"
date.toLocaleString() // "1/01/2018, 12:01:33 PM" 跟用户设定的所在地域相关
```

#### `Object.prototype.hasOwnProperty()`

实例对象==自身==是否具有该属性。

```js
var obj = {
  p: 123
};

obj.hasOwnProperty('p') // true
obj.hasOwnProperty('toString') // false
```

```js
var obj = {
    name : '123',
    age : 23,
    _proto_ : {
		lastName : "deng"
    }
}
// var in只会遍历一层原型链,即原型链的原型链会放弃遍历
for(var prop in obj) {
	if(obj.hasOwnProperty(prop)) {
        console.log(obj[prop]);
	}
}
```

#### `Object.getPrototypeOf()`

获取原型对象。

```js
var F = function () {};
var f = new F();
Object.getPrototypeOf(f) === F.prototype // true,实例对象f的原型是F.prototype
```

#### `Object.create()`

```js
// 原型对象
var A = {
  print: function () {
    console.log('hello');
  }
};

// 实例对象
var B = Object.create(A);

Object.getPrototypeOf(B) === A // true
B.print() // hello
B.print === A.print // true
```

上面代码中，`Object.create`方法以`A`对象为原型，生成了B对象，B继承了A的所有属性和方法。

`Object.create`方法的实质：新建一个空的构造函数F，然后让`F.prototype`属性指向参数对象`obj`，最后返回一个F的实例，从而实现让该实例继承`obj`的属性。

#### `Object.prototype.isPrototypeOf()`

用来判断该对象是否为参数对象的原型。

```js
var o1 = {};
var o2 = Object.create(o1);
var o3 = Object.create(o2);

o2.isPrototypeOf(o3) // true
o1.isPrototypeOf(o3) // true
```

#### `Object.prototype._proto__`

实例对象的`__proto__`属性（前后各两个下划线），返回该对象的原型，该属性可读写。

```js
var obj = {};
var p = {};

obj.__proto__ = p;
Object.getPrototypeOf(obj) === p // true
```

```js
var A = {
  name: '张三'
};
var B = {
  name: '李四'
};

var proto = {
  print: function () {
    console.log(this.name);
  }
};

A.__proto__ = proto;
B.__proto__ = proto;

A.print() // 张三
B.print() // 李四

A.print === B.print // true
A.print === proto.print // true
B.print === proto.print // true
```

总结：获取实例对象`obj`的原型对象，有三种方法：

```
obj.__proto__
obj.constructor.prototype
Object.getPrototypeOf(obj) [推荐]
```



### `Object`实例

#### `Array`

```js
var a = new Array(3); // 长度为3的空数组
var b = [undefined, undefined, undefined]; // 三成员都是undefined的数组

a.length // 3
b.length // 3

a[0] // undefined
b[0] // undefined

0 in a // false
0 in b // true
```

##### `valueOf()`

```js
var arr = [1,2,3];
arr.valueOf() // [1,2,3]
```

##### `toString()`

```js
var arr = [1, 2, 3, [4, 5, 6]];
arr.toString() // "1,2,3,4,5,6" 返回数组的字符串形式
```

##### `push()`、`pop()`

```js
var arr = [];
arr.push(1) 
arr // [1]
```

```js
var arr = ['a', 'b', 'c'];
arr.pop() // 'c'
arr // ['a', 'b']
```

##### `shift()`、`unshift()`

```js
var a = ['a', 'b', 'c'];
a.shift() // 'a' 删除数组的第一个元素并返回,改变原数组
a // ['b', 'c']
```

```js
var a = ['a', 'b', 'c'];
a.unshift('x'); // 4 在数组头部添加元素,返回新数组长度,改变原数组
a // ['x', 'a', 'b', 'c']
```

##### `join()`

```js
var a = [1,2,3,4];
a.join(' ') // '1 2 3 4'
a.join(' | ') // "1 | 2 | 3 | 4"
a.join() // "1,2,3,4"
```

##### `concat()`

```js
['hello'].concat(['world'])
// ["hello","world"]
```

##### `reverse()`

```js
var a = ['a', 'b', 'c'];
a.reverse() // ["c", "b", "a"] 颠倒数组元素,返回改变后的数组,改变原数组 
a // ["c", "b", "a"]
```

##### `slice()`

```js
var a = ['a', 'b', 'c'];

a.slice() // ['a', 'b', 'c']
a.slice(0) // ["a", "b", "c"]
a.slice(1) // ["b", "c"]
a.slice(1, 2) // ["b"]
a.slice(2, 6) // ["c"]
a.slice(-2) // ["b", "c"]
a.slice(-2, -1) // ["b"]
```

##### `splice()`

```js
var a = ['a', 'b', 'c', 'd', 'e', 'f'];

a.splice(4, 2) // ["e", "f"]
a // ["a", "b", "c", "d"]
```

```js
var a = ['a', 'b', 'c', 'd', 'e', 'f'];

a.splice(4, 2, 1, 2) // ["e", "f"]
a // ["a", "b", "c", "d", 1, 2]
```

```js
var a = ['a', 'b', 'c', 'd', 'e', 'f'];

a.splice(-4, 2) // ["c", "d"]
```

```js
var a = [1, 1, 1];

a.splice(1, 0, 2) // []
a // [1, 2, 1, 1]
```

```js
var a = [1, 2, 3, 4];

a.splice(2) // [3, 4]
a // [1, 2]
```

##### `sort()`

```js
['d','c','b','a'].sort() // ['a','b','c','d']

[4,3,2,1].sort() // [1,2,3,4]

[11,101].sort() // [101,11]

[10111,1101,111].sort(function(a, b) {
  return a - b;
})
// [111, 1101, 10111]
```

```js
[
  { name: "张三", age: 30 },
  { name: "李四", age: 24 },
  { name: "王五", age: 28  }
].sort(function(o1, o2) {
  return o1.age - o2.age;
})
// [
//   { name: "李四", age: 24 },
//   { name: "王五", age: 28  },
//   { name: "张三", age: 30 }
// ]
```

##### `map()`

```js
var numbers = [1, 2, 3];

numbers.map(function(n) {
  return n + 1;
});
// [2, 3, 4]

numbers
// [1, 2, 3]
```

```js
var arr = ['a','b','c'];

[1, 2].map(function(e) {
  return this[e];
}, arr) // 通过第二个参数,将回调函数内部的this指向arr数组
// ['b', 'c']
```

若数组有空位，`map`方法的回调函数会跳过空位，不执行。

```js
var f = function(n) {return 'a'};

[1, undefined, 2].map(f) // ["a", "a", "a"]
[1, null, 2].map(f) // ["a", "a", "a"]
[1, , 2].map(f) // ["a", , "a"]
```

##### `forEach()`

```js
function log(element, index, array) {
  console.log('[' + index + '] = ' + element);
}

[2, 5, 9].forEach(log);
// [0] = 2
// [1] = 5
// [2] = 9
```

```js
var out = [];

[1, 2, 3].forEach(function(elem) {
  this.push(elem * elem);
}, out);

out // [1, 4, 9]
```

注意：`forEach`方法无法中断执行，总是会将所有成员遍历完。若希望符合某种条件时中断遍历，则需使用for循环，且也会跳过数组的空位。

**总结**：若遍历数组的目的是为了得到返回值，则使用`map`方法，否则使用`forEach`方法。

##### `filter()`

```js
[1, 2, 3, 4, 5].filter(function(elem) {
  return (elem > 3);
})
// [4, 5]
```

```js
var obj = {MAX: 3};
var myFilter = function(item) {
  if(item > this.MAX) return true;
};

var arr = [2, 8, 3, 4, 1, 3, 2, 9];
arr.filter(myFilter, obj) // [8, 4, 9] 第二个参数用来绑定参数函数内部的`this`变量 
```

##### `some()`、`every()`

`some`方法，只要有一个成员的返回值是`true`，整个返回值就是`true`。

`every`方法，所有成员的返回值必须都为`true`，方法才返回`true`。

```js
var arr = [1, 2, 3, 4, 5];

arr.some(function(elem, index, arr) {
  return elem >= 3;
});
// true
```

```js
var arr = [1, 2, 3, 4, 5];

arr.every(function(elem, index, arr) {
  return elem >= 3;
});
// false
```

##### `reduce()`、`reduceRight()`

依次处理数组的每个成员，最终累计为一个值。

区别：

- `reduce`从左到右进行处理
- `reduceRight`从右到左进行处理

```js
[1, 2, 3, 4, 5].reduce(function(a, b) {
  console.log(a, b);
  return a + b;
})
// 1 2
// 3 3
// 6 4
// 10 5
// 最后结果: 15
```

```js
[1, 2, 3, 4, 5].reduce(function(a, b) {
  return a + b;
}, 10); // 指定参数a的初值为10,即数组从10开始累加,b则为1
// 25
```

```js
// 找出字符长度最长的数组成员
function findLongest(entries) {
  return entries.reduce(function(longest, entry) {
    return entry.length > longest.length ? entry : longest;
  }, '');
}

findLongest(['aaa', 'bb', 'c']) // "aaa"
```

##### `indexOf()`、`lastIndexOf()`

```js
var a = ['a', 'b', 'c'];

a.indexOf('b') // 1
a.indexOf('y') // -1
```

```js
['a', 'b', 'c'].indexOf('a', 1) 
// -1
```

#### `Math`

##### `Math.abs()`

```js
Math.abs(-1) // 1
```

##### `Math.ceil()`

```js
Math.ceil(3.2) // 4
Math.ceil(-3.2) // -3
```

##### `Math.floor()` 

```js
Math.floor(3.2) // 3
Math.floor(-3.2) // -4
```

##### `Math.max()`、`Math.min()` 

```js
Math.max(2, -1, 5) // 5
Math.min(2, -1, 5) // -1
```

##### `Math.round()` 

```js
Math.round(0.1) // 0
Math.round(0.5) // 1
Math.round(-1.1) // -1
Math.round(-1.5) // -1 特殊
Math.round(-1.6) // -2
```

##### `Math.random()` 

随机返回`[0，1）`之间的一个数。

#### `Date`

##### `Date.now()`

```js
Date.now() // 1364026285194
```

##### `Date.prototype.toLocaleDateString()`

```js
var d = new Date(2013, 0, 1);

d.toLocaleDateString()
// 中文版浏览器为"2013年1月1日"
// 英文版浏览器为"1/1/2013"
```

##### `Date.prototype.toLocaleTimeString()`

```js
var d = new Date(2013, 0, 1);

d.toLocaleTimeString()
// 中文版浏览器为"上午12:00:00"
// 英文版浏览器为"12:00:00 AM"
```

##### `getDay()`

返回星期几，星期日为 0，星期一为 1，以此类推。

##### `getHours()`

返回小时（0 - 23）。

```js
var d = new Date('January 6, 2013');

d.getDate() // 6
d.getMonth() // 0
d.getYear() // 113
d.getFullYear() // 2013
```

#### `RegExp`

```js
// test必须出现在开始位置
/^test/.test('test123') // true

// test必须出现在结束位置
/test$/.test('new test') // true

// 从开始位置到结束位置只有test
/^test$/.test('test') // true
/^test$/.test('test test') // false
```

#### `JSON`

##### `JSON.stringify()`

将值转为`JSON`字符串，该字符串符合`JSON`格式，并且可以被`JSON.parse`方法还原。

##### `JSON.parse()`

将`JSON`字符串转换成对应的值。



## 2.`Number`对象

```js
var n = new Number(1);
typeof n // "object"
```

```js
Number(true) // 1
```

### 方法

#### `Number.prototype.toString()`

将一个数值转为字符串形式。

#### `Number.prototype.toFixed()`

将数转为指定位数的小数，返回字符串形式。

```js
10.005.toFixed(2) // "10.01"
```



## 3.`String`对象

```js
new String('abc') // 类似数组的对象 
// String {0: "a", 1: "b", 2: "c", length: 3}
```

```js
var s1 = new String('abc')
typeof s1 // 'object'
s1.valueOf() // 'abc'
```

```js
String(true) // "true"
```

### 方法

#### `String.prototype.length`

```js
'abc'.length // 3
```

#### `String.prototype.charAt()`

```js
var s = new String('abc');
s.charAt(1) // "b"
```

```js
'abc'.charAt(-1) // ""
'abc'.charAt(3) // ""
```

#### `String.prototype.concat()`

返回新字符串。

```js
var s1 = 'abc';
var s2 = 'def';

s1.concat(s2) // "abcdef"
s1 // "abc"
```

若参数不是字符串，会先转为字符串，再拼接。

#### `String.prototype.slice()`

不改变原字符串。

```js
'JavaScript'.slice(0, 4) // "Java"
'JavaScript'.slice(4) // "Script",一直截切到最后
'JavaScript'.slice(-2, -1) // "p"
'JavaScript'.slice(2, 1) // ""
```

```js
/pinglun?name=sxw&msg=hello

split('?') ==> name=sxw&msg=hello
split('&') ==> name=sxw	msg=hello
forEach()
name=sxw.split('=')

0 key
1 value
```

```js
Array.prototype.mySlice = function() {
	var start = 0;
	var end = this.length;
	if(arguments.length === 1) {
		start = arguments[0]
	}else if(arguments.length === 2) {
		start = arguments[0]
		end = arguments[1]
	}
	var tmp = []
	for(var i = start;i < end;i ++) {
		tmp.push(this[i])
	}
	return tmp 
}

var fakeArr = {
	a: 'abs'
	b: 'efg'
	length: 2
}
[].mySlice.call(fakeArr);
```

#### `String.prototype.substring()`

```js
'JavaScript'.substring(0, 4) // "Java"
```

```js
'JavaScript'.substring(10, 4) // "Script",自动更换参数位置
```

```js
'Javascript'.substring(-3) // "JavaScript",参数为负数,即为0
```

#### `String.prototype.indexOf()`

#### `String.prototype.lastIndexOf()`

```js
'hello world'.indexOf('o') // 4
```

```js
'sxw'.indexOf('a') // -1
```

```js
'hello world'.indexOf('o', 6) // 7,第二个参数表示从第几位开始往后匹配
```

#### `String.prototype.trim()`

```js
'  hello world  '.trim()
// "hello world",除空格
```

#### `String.prototype.toUpperCase()`

#### `String.prototype.toLowerCase()`

```js
'Hello World'.toUpperCase()
// "HELLO WORLD"
```

#### `String.prototype.search()`

```js
'cat, bat, sat, fat'.search('at') // 1,返回值为第一个匹配的位置索引
```

#### `String.prototype.replace()`

```js
'aaa'.replace('a', 'b') // "baa"
'aaa'.replace(/a/, 'b') // "baa"
'aaa'.replace(/a/g, 'b') // "bbb"
```

```js
var str = '  #id div.class  ';

str.replace(/^\s+|\s+$/g, '') // 消除字符串首尾两端的空格
// "#id div.class"
```

#### `String.prototype.split()`

```js
'a|b|c'.split('|') // ["a", "b", "c"]
```

```js
'a|b|c'.split('') // ["a", "|", "b", "|", "c"]
```

```js
'a|b|c'.split() // ["a|b|c"]
```

#### `String.prototype.padStart`

#### `String.prototype.padEnd`

字符串补全长度，可在头部或尾部进行补全。

```js
'x'.padStart(5, 'ab') // 'ababx'
'x'.padEnd(5, 'ab') // 'xabab'
```

```js
'12'.padStart(10, 'YYYY-MM-DD') // "YYYY-MM-12"
'09-12'.padStart(10, 'YYYY-MM-DD') // "YYYY-09-12"
```



## 4.`Boolean`对象

```js
Boolean(null) // false
Boolean(undefined) // false
Boolean(false) // false
Boolean(0) // false
Boolean('') // false
Boolean(NaN) // false
```

使用双重否运算符`!`也可将任意值转为对应的布尔值：

```js
!!null // false
!!undefined // false
!!0 // false
!!'' // false
!!NaN // false
```



## 5.`debugger`

```js
for(var i = 0; i < 5; i++){
  console.log(i);
  if (i === 2) debugger;
}
// 上面代码打印出0,1,2以后,就会暂停,自动打开源码界面,等待进一步处理;
```



## 6.面向对象

#### 继承

让`Rectangle`构造函数继承`Shape`：

```js
// 第一步，子类继承父类的实例
function Rectangle() {
  Shape.call(this); // 调用父类构造函数
}
// 另一种写法
function Rectangle() {
  this.base = Shape;
  this.base();
}

// 第二步，子类继承父类的原型
Rectangle.prototype = Object.create(Shape.prototype);
Rectangle.prototype.constructor = Rectangle;
```

采用这样的写法后，`instanceof`运算符会对子类和父类的构造函数，都返回`true`。

```js
var rect = new Rectangle();
rect.move(1, 1) // 'Shape moved.'

rect instanceof Rectangle  // true
rect instanceof Shape  // true
```

以上是整体继承！

有时只需要单个方法的继承，这时可以采用下面的写法。

```javascript
ClassB.prototype.print = function() {
  ClassA.prototype.print.call(this);
  // some code
}
```

上面代码，子类`B`的`print`方法先调用父类`A`的`print`方法，再部署自己的代码。

#### 多重继承

```js
function M1() {
  this.hello = 'hello';
}

function M2() {
  this.world = 'world';
}

function S() {
  M1.call(this);
  M2.call(this);
}

// 继承 M1
S.prototype = Object.create(M1.prototype);
// 继承链上加入 M2
Object.assign(S.prototype, M2.prototype);
// 指定构造函数
S.prototype.constructor = S;

var s = new S();
s.hello // 'hello：'
s.world // 'world'
```

