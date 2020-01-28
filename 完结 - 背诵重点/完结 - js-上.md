# `JavaScript`

## 1.语言特点

- 解释性
- 弱类型
- 宿主语言
- 轻量级
- 不提供`I/O`相关`API`
- 事件驱动和非阻塞式设计
- 函数式编程
- 面向对象编程



## 2.基础语法

### 变量声明提升

### 流程控制

```js
function countDown(n) {
    while(n-- > 0) console.log(n)
}
countDown(3)

// 2
// 1
// 0
```

```js
function countDown(n) {
    while(--n > 0) console.log(n)
}
countDown(3)

// 2
// 1
```

### `switch`结构

```js
switch('apple') {
    case 'apple':
        console.log('苹果');
        break;
    case 'pear':
        console.log('梨');
        break;
    default:
        console.log('什么也不是')
}
```

### 三元表达式

```js
var max = function(a,b,c) {
    return a > b ? (a > c ? a : c) : (b > c ? b : c)
}
```

### `for`循环

```js
var f = function() {
    var flag = true
    for(var i = 0;flag && i < 10;i ++) {
        for(var j = 0;j < 10;j ++) {
            if(j === 2) {
                console.log(i + "=" + j)
                flag = false
                break
            }
        }
    }
}
f() // 0 = 2
```

### `break`和`continue`

- `break` 立即跳出当前代码块或循环
- `continue` 立即终止本轮循环，返回到循环结构的头部，开始下一轮循环



## 3.数据类型

基础类型：`number`、`string`、`boolean`

引用类型：`object`

特殊类型：`null`、`undefined`

`ES6`新增：`symbol`

### 判断数据类型

#### `typeof`

以==字符串==的形式，返回指定变量的数据类型。

缺点：无法准确辨别以下两种类型。

```js
typeof [] // 'object'
typeof null // 'object'
```

```js
// 在没有块级作用域之前,typeof不报错
typeof a // 'undefined'
```

封装`typeof`：

```js
function type(target) {
    var template = {
        "[object Number]" : "number - object",
        "[object String]" : "string - object",
        "[object Boolean]" : "boolean - object",
        "[object Array]": "array",
        "[object Object]": "object"
    }
    if(target === null) {
        return 'null'
    }else if(typeof(target) == 'object') {
        var str = object.prototype.toString.call(target);
        return template[str]
    }else {
        return typeof(target)
    }
}
```

#### `instanceof`

验证某个对象是否为指定构造函数的实例。

```js
var obj = {}
obj instanceof Object // true
```

```js
// 若参数为原始值,Object会将其转为对应包装对象的实例

// 数字
var obj = Object(1);
obj instanceof Object // true
obj instanceof Number // true

// 字符串
var obj = Object('foo');
obj instanceof Object // true
obj instanceof String // true

// 布尔值
var obj = Object(true);
obj instanceof Object // true
obj instanceof Boolean // true
```

`Object()`的作用：判断某一变量是否为对象

```js
// 若参数为 (空 / undefined / null) , Object()会返回一个空对象
// 若参数为对象,返回自身

// 数组
var arr = [];
var obj = Object(arr); 
obj === arr // true

// 对象
var value = {};
var obj = Object(value) 
obj === value // true

// 函数
var fn = function() {};
var obj = Object(fn); 
obj === fn // true

// 判断某一变量是否为对象
function isObject(value) {
	return value === Object(value);
}
```

#### `Object.prototype.toString` 推荐

返回对象的类型字符串。

```js
var obj = {};
obj.toString() // "[object Object]" 第二个Object表示该值的构造函数
```

`Object.prototype.toString.call(target)`判断类型的函数：

```
数值: [object Number]
字符串: [object String]
布尔值: [object Boolean]
null: [object Null]
undefined: [object Undefined]
数组: [object Array]
arguments对象: [object Arguments]
函数: [object Function]
Error对象: [object Error]
Date对象: [object Date]
RegExp对象: [object RegExp]
其他对象: [object Object]
```

#### `constructor`

```js
[].constructor // Array 
```

==注意==：`null`、`undefined`无法使用！



## 4.数值

`js`底层根本就没有整数，所有数字都是以64位浮点数的形式进行存储的。

而当某些运算只有整数才能完成的时候，`js`会自动将64位浮点数转为32位整数进行计算。

由于浮点数计算不精确，便会产生误差。

```js
0.1 + 0.2 = 0.30000000000000004
0.3 - 0.2 = 0.09999999999999998
```

```js
0 / 0 // NaN
typeof NaN // "number"
NaN === NaN // false
Infinity * 0  // NaN
```

总结：

- `NaN`与任何数（包括自己）的运算结果都是`NaN`
- `Infinity`与`NaN`比较的结果总是返回`false`
- `Infinity`与`undefined`计算结果返回`NaN`

### `parseInt`

将字符串转为整数。

总结：

- 当`parseInt`后面的参数不是字符串时，会先转为字符串
- 字符串转整数是逐字符依次转换，直到不能进行下去，并返回已经转换成功的部分
- `parseInt`返回值只有两种情况：十进制整数或者`NaN`

### `parseFloat`

将字符串转为浮点数。

```js
parseFloat('3.14') // 3.14
parseFloat('314e-2') // 3.14
parseFloat('0.0314E+2') // 3.14
```

### `isNaN`

判断值是否为`NaN`。

若传入其他类型的值，会先转为数值再进行判断。

```js
function isNaN(value) {
  return value !== value; // 利用NaN是唯一不等于自身的特点进行判断 
}
isNaN(NaN) // true
```

### `isFinite`

返回布尔值，用以表示某个值是否为正常的数值。

除`Infinity`、`-Infinity`、`NaN`、`undefined`外，其他值都返回`true`。



## 5.字符串

### `Base`64转码

```js
btoa() // 将任意值转成Base64编码
atob() // 将Base64编码还原为原来的值
```



## 6.对象

### 引用

当不同的变量名指向同一对象时，它们都是该对象的引用（指向同一内存地址）。当修改其中任一变量的属性时，会影响到其他所有的变量。

若两个变量指向同一原始类型值，那么变量都是值的拷贝（深拷贝）。

### 表达式还是语句

```js
{foo: 123} // 代码块
({foo: 123}) // 对象

eval('{foo: 123}') // 123
eval('({foo: 123})') // {foo: 123}
```

### 读取数据

```js
var obj = {
	p: 'Hello World'
};

obj.p // "Hello World" 
obj['p'] // "Hello World"
```

```js
// 当对象的键名是数字时,不能使用点运算符,只能使用方括号运算符读取数据 

var obj = {
	0.7: 'Hello World'
};

obj['0.7'] // "Hello World",推荐使用这种方式去读取数据
obj[0.7] // "Hello World"
```

### 属性的增删改查

```js
var obj = {}

obj.foo = 'hello'
obj['bar'] = 'world'
```

1. `Object.keys()` 遍历对象自身的所有属性名 

```js
var obj = {p: 1}
Object.keys(obj) // ["p"]
```

2. `delete` 删除对象自身的属性，返回布尔值

- 删除一个不存在的属性，`delete`也返回`true`。所以不能根据`delete`命令所返回的结果，判断某个属性是否存在
- 只在一种情况下，`delete`才返回`false`，当属性存在且限制了不能被删除的情况
- 即使`delete`返回`true`，属性依然可能读取到值，因为`delete`无法删除继承的属性

```js
var obj = {p: 1}
delete obj.p // true
```

```js
var obj = Object.defineProperty({}, 'p', {
  value: 123,
  configurable: false
});

obj.p // 123
delete obj.p // false
```

```js
var obj = {};
delete obj.toString // true
obj.toString // function toString() { [native code] }
```

### 遍历对象

#### `in`运算符

检查对象是否包含某个属性名（无法识别属性是自身还是继承而来），结果返回布尔值。

```js
var obj = {p: 1};
'p' in obj // true

var obj = {};
'toString' in obj // true
```

#### `for...in`

遍历对象身上所有可遍历的属性（自身 + 继承），跳过不可遍历的属性。

```js
var obj = {a: 1, b: 2, c: 3};

for(var i in obj) {
  console.log(obj[i]);
}

// 1
// 2
// 3
```

对象默认都继承`toString`属性，但`for...in`循环不会遍历到。因为默认`toString`是不可遍历的。

```js
var obj = {};
obj.toString // toString() { [native code] }

for(var p in obj) {
  console.log(p); // 没输出
} 
```

一般情况下，我们都只是遍历对象自身的属性。所以使用`for...in`时，应该结合`hasOwnProperty`方法，在循环内部判断一下某个属性是否为对象自身的属性。

```js
var person = {name: '老张'};

for(var key in person) {
  if(person.hasOwnProperty(key)) {
    console.log(key); // name
  }
}
```



## 7.数组

```js
var arr = new Array(3); // 长度为3的稀松数组,默认3为数组的长度 (, , )
```

数组是特殊的对象。

```js
var arr = ['a', 'b', 'c'];

Object.keys(arr) // 遍历对象键名
// ["0", "1", "2"]
```

### `length`属性

```js
var arr = ['a','b','c'];

arr.length = 2;
arr // ["a","b"]
```

```js
var arr = ['a', 'b', 'c'];

arr.length = 0;
arr // []
```

```js
var a = ['a'];

a.length = 3;
a[1] // undefined,新增位置都是undefined
```

### `in`运算符

检查某个==键名==是否存在。

```js
var arr = ['a','b','c'];

2 in arr  // true
'2' in arr // true
4 in arr // false
```

```js
var arr = [];
arr[3] = 'a'; // [empty, empty, empty, "a"]

3 in arr // true
1 in arr // false,数组中某个位置是空位empty时,也返回false
```

### `delete`

```js
var a = [1, 2, 3];

delete a[1];
a // [1,empty,3]
a[1] // undefined
```

### 遍历数组

#### `for...in`不推荐

```js
var a = [1, 2, 3];

for(var i in a) {
  console.log(a[i]);
}
// 1
// 2
// 3
```

```js
var a = [ , , , ];

for(var i in a) {
  console.log(i); // 不产生任何输出
}
```

```js
var a = [1, 2, 3];

a.foo = true;
for(var key in a) {
  console.log(key);
}
// 0
// 1
// 2
// foo
```

#### `Object.keys()`不推荐

```js
var a = [ , , , ];

Object.keys(a) // []
```

#### `for`循环

```js
var a = [1, 2, 3];

for(var i = 0; i < a.length;i ++) {
  console.log(a[i]);
}
```

#### `forEach`循环

```js
var a = [undefined, undefined, undefined];

a.forEach(function(value, index) {
  console.log('索引是:' + index + '值是:' + value);
});
// 0. undefined
// 1. undefined
// 2. undefined
```



## 8.类数组

要求对象所有的键名必须是正整数或零，并且存在`length`属性。

```js
// push原理
Array.prototype.push = function(target) {
    obj[obj.length] = target;
    obj.length ++; // 动态增长length属性
}
```

```js
var obj = {
    "o" : 'a',
    "1" : 'b',
    "2" : 'c',
    "length" : 3,
    "push" : Array.prototype.push,
    "splice" : Array.prototype.splice // 一旦给对象加上splice之后,这个对象长的就像数组了
}
```

### 三大典型类数组

#### `arguments`对象

```js
function args() {
    retrun arguments
}
var arrLike = args('a','b')

arrLike[0] // 'a'
arrLike.length // 2
arrLike instanceof Array // false
```

#### `DOM`元素集

```js
var elts = document.getElementsByTagName('h3');

elts.length // 3
elts instanceof Array // false
```

#### 字符串

```js
'abc'[1] // 'b'
'abc'.length // 3
'abc' instanceof Array // false
```

### 类数组转为数组

#### 数组`slice`方法

```js
function args() {
    return arguments
}
var arrayLike = args('a', 'b');
var arr = Array.prototype.slice.call(arrayLike);
```

```js
function args() {return arguments}
var arrayLike = args('a', 'b'); 

// 将数组原型链上的所有方法放到类数组上去,实现转换
Array.prototype.forEach.call(arrayLike, function(value, index) {
	console.log(index + ' : ' + value);
});

// 0 : a
// 1 : b
```

```js
// 一般我们都是将类数组转为数组,再调用数组的forEach方法实现遍历

var arr = Array.prototype.slice.call('abc');
arr.forEach(function(value) {
  console.log(value);
});

// a
// b
// c
```



## 9.函数

### 递归

函数调用自身实现递归。

- 函数自己调用自己 
- 有终止条件

```js
// 斐波那契数列
function fib(num) {
  if (num === 0) return 0;
  if (num === 1) return 1; // 设定出口
  return fib(num - 2) + fib(num - 1); // 逐步递减到出口
}
fib(6) // 8
```

```js
function f(n) { // 实现n的阶乘 n!
  	if(n === 0) {
        return 1;
  	}  
  	return n * f(n-1);
};
```

### `length`返回形参个数

### 作用域问题

函数执行时的作用域，是定义时的作用域，而不是调用时的作用域。

```js
var x = function () {
  console.log(a);
};

function y(f) {
  var a = 2;
  f();
}

y(x) // ReferenceError: a is not defined
```

### 闭包

读取函数内部的变量，让变量始终存留在内存中。

```js
function foo() {
  var x = 1;
  function bar() {
    console.log(x); 
  }
  return bar; // 将函数foo内的变量x连同bar函数一起被保存到了外部
}

var x = 2;
var f = foo();
f() // 1
```

#### 作用

##### 计数器

```js
function createIncrementor(start) {
  return function () {
    console.log(start++);
  };
}

var inc = createIncrementor(5);

inc() // 5
inc() // 6
inc() // 7 
```

##### 私有化变量

```js
function Person(name) {
  var _age;
  
  function setAge(n) {
    _age = n;
  }
  
  function getAge() { // 形成闭包
    return _age;
  }

  return {
    name: name,
    getAge: getAge,
    setAge: setAge
  };
}

var p1 = Person('张三');
p1.setAge(25);
p1.getAge() // 25

// 函数Person的内部变量_age,通过闭包变成了返回对象p1的私有变量;
// 注意: 外层函数每次调用,赋值,都会生成一个新的闭包,而这个闭包又会保留外层函数的内部变量,内存消耗大;
```

##### 做存储结构

```js
function eater() {
    var food = "";
    var obj = {
        eat : function() {
            console.log("food is" + " " + food);
            console.log("I am eating" + " " + food);
            food = "";
            console.log("now,food is" + " " + food);
        },
        push : function(myFood) {
            food = myFood;
        }
    }
    return obj;
}
var eater1 = eater();
```

```js
function test() {
    var num = 100;
    function a() {
        num ++;
        console.log(num); // 因为a里面没有定义num,所以就会去上一层去找num属性并且修改并且保存
    }
    function b() {
        num --;
        console.log(num);
    }
    return [a,b]; // a和b使用的是父级的同一个AO,他们都是test的劳动成果,所以操作的是同一个对象
}
var myArr = test();
myArr[0](); // 101
myArr[1](); // 100
```

```js
function a() {
    var num = 100;
    function b() { 
        // b是拿得到a里面的变量的,b刚刚被定义的时候拿的是a的劳动成果,死死地绑在了自己的身上,b每次执行的时候都在自己的执行上下文上形成一个新的AO,执行完成之后把自己的AO扔掉,等下次执行的时候再创建一个新的自己的AO,
        num ++; // 去a的AO里面拿num,变成101
        console.log(num);
    }
    return b;
}
var demo = a();
demo(); // 101 相当于执行一次b
demo(); // 102 由于b的AO里面没有所要打印的num,所以我们还是去a的AO里面找num,进行操作
```

总结：不能滥用闭包，否则会造成原有作用域链不释放，造成内存泄露，网页的性能问题。

### `arguments`对象

```js
var f = function() {
  console.log(arguments[0]);
  console.log(arguments[1]);
  console.log(arguments[2]);
}

f(1, 2, 3)
// 1
// 2
// 3
```

#### 将`arguments`对象转为数组

##### a.`slice`+`call`

```js
var args = Array.prototype.slice.call(arguments);
```

##### b.`for`循环

```js
var args = [];
for (var i = 0; i < arguments.length;i ++) {
  args.push(arguments[i]);
}
```

##### c.`callee`严格模式下禁用

### 立即执行函数`IIFE`

```js
(function() {}())
(function() {})()
```

作用：

- 不必为函数命名，避免污染全局变量
- `IIFE`内部形成了一个单独的作用域，可封装一些外部无法读取的私有变量

### `eval`

将字符串直接当作语句来执行，常用来解析`JSON`数据字符串，不过还是推荐使用浏览器提供的`JSON.parse`方法。

```js
eval('var a = 1;');
a // 1
```



## 10.运算

所有运算一律转为数值后进行计算，除加法外其他运算符都不会发生重载。

### 对象相加

- 先调用对象的`valueOf`方法，返回对象自身
- 再调用对象的`toString`方法，默认返回`"[object Object]"`，将其转为字符串

```js
// 自定义valueof()

var obj = {
  valueOf: function() {
    return 1;
  }
};

obj + 2 // 3
```

```js
// 自定义toString()

var obj = {
  toString: function() {
    return 'hello';
  }
};

obj + 2 // "hello2"
```

==特例==：当对象是`Date`对象的实例并且自定义了`valueOf`和`toString`方法，结果会优先执行`toString`方法而忽略`valueOf`方法。

```js
var obj = new Date();
obj.valueOf = function () { return 1 };
obj.toString = function () { return 'hello' };

obj + 2 // "hello2"
```

### 字符串的比较

按照字典顺序进行比较，比较首字母的`Unicode`码点。

非字符串的比较：

- 除了相等运算符 == 和严格相等运算符 === 外，其他比较运算符都是先转成数值再进行比较

### 对象的比较

```js
[2] > [11] // true

// [2].valueOf().toString() > [11].valueOf().toString()
// 即 '2' > '11'
```

### 复合类型的比较

复合类型数据的比较，比较它们是否指向同一地址。

对象的比较，严格相等运算符比较的是地址，而大于或小于运算符比较的是值。

```js
var v1 = {};
var v2 = v1;
v1 === v2 // true
```

```js
new Date() > new Date() // false
```

### `null`和`undefined`的比较

`null`是一个空对象，转为数值时始终为`0`。

`undefined`转为数值时为`NaN`。

`undefined`和`null`与其他类型值比较时为`false`。互相比较时，为`true`。

```js
undefined == null // true
undefined === null // false
```

```js
'true' == true // false

// 等同于 Number('true') === Number(true)
// 等同于 NaN === 1
```

```js
'' == 0 // true

// 等同于 Number('') == 0
// 等同于 0 == 0
```



## 11.数据类型转换

### 强制类型转换

#### `Number()`

- 可被解析为数值的，转为对应数值
- 不可解析为数值的，返回`NaN`
- 空字符串，`null`转为`0`

转换原理：

- 先调用对象自身的`valueOf`方法，若返回原始类型数值，则直接对该值使用`Number`函数，不再进行后续步骤
- 若`valueOf`方法返回的还是对象，则改为调用对象自身的`toString`方法，如果`toString`方法返回原始类型的值，则对该值使用`Number`函数，不再进行后续步骤；如果`toString`方法返回的是对象，报错

默认情况下，对象的`valueOf`方法返回对象本身，所以总是会调用`toString`方法。而`toString`方法返回对象的类型字符串，如：`[object Object]`。

```js
var obj = {x: 1};
Number(obj) // NaN
// 等同于
if(typeof obj.valueOf() === 'object') {
  Number(obj.toString());
}else {
  Number(obj.valueOf());
}
```

```js
// 自定义valueOf和toString方法
Number({
  valueOf: function () {
    return 2;
  },
  toString: function () {
    return 3;
  }
})
```

**关于`Number`函数和`parseInt`函数的比较**：

相同：`parseInt`和`Number`函数都会自动过滤掉一个字符串，前导和后缀的空格。

不同：

- `parseInt`('42 cats') // 42 逐个解析字符
- `Number`('42 cats') // `NaN` 整体转换字符

总结：`Number`将字符串转为数值比`parseInt`函数严格很多，只要有一个字符无法转换，直接`NaN`。

#### `String()`

原始类型值：

```
数值,字符串,布尔值,null,undefined,都转为对应字符串形式
```

对象：

```js
String({a: 1}) // "[object Object]"
```

```js
// 数组,返回该数组的字符串形式;
String([1, 2, 3]) // "1,2,3"
```

原理：与`Number`方法相似，只是互换了`valueOf`和`toString`方法的执行顺序。

- 先调用对象自身的`toString`方法，如果返回原始类型的值，则对该值使用`String`函数，不再进行以下步骤
- 如果`toString`方法还是返回对象，再调用原对象的`valueOf`方法，如果`valueOf`方法返回原始类型的值，对该值使用`String`函数，不再进行以下步骤，如果`valueOf`返回对象，报错 

```js
String({
  toString: function() {
    return 3;
  }
})
// "3"

String({
  valueOf: function() {
    return 2;
  }
})
// "[object Object]"

String({
  valueOf: function() {
    return 2;
  },
  toString: function() {
    return 3;
  }
})
// "3"

// 第一个对象返回toString方法的值,数值3;
// 第二个对象返回的还是toString方法的值,[object Object];
// 第三个对象表示toString方法先于valueOf方法执行;
```

#### `Boolean()`

除以下六个值转换结果为`false`外，其余都为`true`。

```
false
+0 / -0
NaN
''
null
undefined
```

### 自动转换类型

```js
'5' + {} // "5[object Object]"
'5' + function() {} // "5function() {}"
'5' + undefined // "5undefined"
'5' + null // "5null"
'5' + [] // "5"
'5' * []    // 0
undefined + 1 // NaN
```



## 12.错误处理机制

### `Error`实例对象

所有抛出的错误都是这个构造函数的实例。

```js
var err = new Error('出错了');
err.message // "出错了"
```

### 错误类型

`SyntaxError`对象：语法解析错误。

`ReferenceError`对象：引用一个不存在的变量时发生的错误。

`RangeError`对象：数值超出有效范围时发生的错误。

`TypeError`对象：变量或参数不是预期类型时发生的错误。

### 自定义错误

```js
// 自定义函数,构建错误对象
function UserError(message) {
  this.message = message || '默认信息';
  this.name = 'UserError';
}

// 1.指定新建的错误对象函数的原型继承自Error对象
UserError.prototype = new Error();
// 2.
UserError.prototype.constructor = UserError;

new UserError('这是自定义的错误！');
```

### `throw`语句

手动中断程序执行并抛出错误。

```js
if(x < 0) {
  throw new Error('x要大于0'); // 程序中断
  console.log('我执行了吗')
}
```

### `try...catch`

允许对错误进行处理，可选择是否往下执行。

```js
try{
  throw new Error('出错了!');
  console.log('这里不会执行')
}catch(e) {
  console.log(e.name + ": " + e.message);
  console.log(e.stack);
}

// Error: 出错了!
//   at <anonymous>:3:9
//   ...

// 总结: 当try代码块抛出错误,JavaScript引擎立即把代码的执行转到catch代码块,被catch代码块捕获了;catch接受一个参数,表示try代码块抛出的值;
```

### `finally`

`try...catch`结构允许在最后添加一个`finally`代码块。表示不管是否出现错误，都必须运行的语句。

```js
function cleansUp() {
  try{
    throw new Error('出错了……');
    console.log('此行不会执行');
  }finally {
    console.log('完成清理工作');
  }
}

cleansUp()

// 完成清理工作
// Error: 出错了……

// 由于没有catch语句块,所以错误没有被捕获
// 执行finally代码块以后,程序就中断在错误抛出的地方
```

```js
function idle(x) {
  try{
    console.log(x);
    return 'result';
  }finally {
    console.log("FINALLY");
  }
}

idle('hello')
// hello
// FINALLY
// "result"

// try代码块没有发生错误,而且里面还包括return语句,但是finally代码块依然会执行;注意: 只在其执行完毕后才会显示return语句的值。
```

`return`语句的执行是排在`finally`代码之前的，只是等`finally`代码执行完毕后才返回。

```js
function f() {
  try{
    console.log(0);
    throw 'bug';
  }catch(e) {
    console.log(1);
    return true; // 这句原本会延迟到finally代码块结束再执行
    console.log(2); // 不会运行
  }finally {
    console.log(3);
    return false; // 这句会覆盖掉前面那句return
    console.log(4); // 不会运行
  }
  console.log(5); // 不会运行
}

var result = f();
// 0
// 1
// 3
```



## 13.作用域

### 运行期上下文

当函数执行前一刻，会创建一个称为执行期上下文的内部对象。

执行期上下文定义了该函数执行时的环境，函数每次执行时对应的执行期上下文都是独一无二的。

多次调用同一个函数会创建多个执行上下文，当函数执行完毕，它所产生的执行上下文立即被销毁。

### 查找变量

在当前函数，从作用域链的顶端依次向下查找。

一个函数刚刚出生的时候，[[scope]] 作用域里面就已经存了GO，当函数执行的时候又会产生执行期上下文AO，
并且将AO放在作用域链的最顶端。

### 作用域链

每一个函数都有一个执行期上下文的集合，叫作用域链。

我们真正在这个函数里面去访问某一变量的话，要遵循这个函数的作用域链，按顺序去访问。

一个函数执行完之后是要销毁自身的执行期上下文的，b只会干掉自己本身所产生的AO。

但是a不一样，当a执行结束后去除作用域链`scope chain`和自己本身的AO的联系线的时候，因为a里面的AO是包含b函数的`b : function`，所以b就会永远没有了，a重新回归未定义状态，等待下一次被执行。

### 隐式属性

仅供`javascript`引擎存取，`[[scope]]`就是其中之一。

`[[scope]]`指的就是我们所说的作用域，其中存储了运行期上下文的集合。

```js
test.[[scope]] // [[scope]]表示由这个函数所产生的作用域
// [[scope]]中存储的是执行期上下文对象集合,这个集合呈链式链接,我们把这种链式链接叫做作用域链。
```

```js
function a() { // a刚出生,是在全局下去看世界
    function b() { 
        // 由于a执行产生b定义,所以b刚出生时的执行期上下文是a的最终结果;
		// b是站在a的肩膀上去看世界,b里面产生的类似a产生的AO其实质就是a自身所产生的AO;
        var b = 234;
    }
    var a = 123;
    b(); // 当b执行的时候,产生自身的AO又会跑到执行期上下文的最上面
}
var glob = 100;
a();

------------------------------------------------

图解:
function a() {
    function b() {
        function c() {
            
        }
        c();
    }
    b();
}
a();
-----------------------------------
            作用域        执行期上下文
a defined a.[[scope]] --> 0 : GO
a doing   a.[[scope]] --> 0 : aAO
						  1 : GO

b defined b.[[scope]] --> 0 : aAO
						  1 : GO
b doing   b.[[scope]] --> 0 : bAO
						  1 : aAO
						  2 : GO

c defined c.[[scope]] --> 0 : bAO
						  1 : aAO
						  2 : GO
c doing   c.[[scope]] --> 0 : cAO
						  1 : bAO
						  2 : aAO
						  3 : GO
```



## 14.预编译

```js
function fn(a) {
    console.log(a); // function a() {}
    var a = 123; // 预编译提升声明变量,但是赋值还是需要看的,将a = 123代替AO对象里a属性的值
    console.log(a); // 123
    function a() {} // 预编译已经将其提升,所以不用再看
    console.log(a); // 123
    var b = function() {} // var b不用看了,修改AO里的b,值为function() {}
    console.log(b); // function() {}
    function d() {}
    console.log(d); // function d() {}
}
fn(1);
```

```js
function test(a,b) {
    console.log(a); // 1
    c = 0;
    var c;
    a = 3;
    b = 2;
    console.log(b); // 2
    function b() {}
    function d() {}
    console.log(b); // 2
    console.log(d); // function d() {}
}
test(1);
```

步骤：

- 创建AO对象，执行期上下文 

```
AO{

}
```

- 找形参和变量声明，AO找的是局部范围里的，GO找的是全局范围里的。将形参和变量名作为AO属性名，值为 undefined 

```js
AO{
    a : undefined,
    b : undefined,
}
```

- 将实参值和形参相统一

```
AO{
    a : 1,
    b : undefined,
}
```

- 在函数体里面找函数声明，值赋予函数体

```
AO{
    a : function a() {},
    b : undefined,
    d : function d() {},
}
```

---

```js
function test(a,b) {
    console.log(a); // function a() {}
    console.log(b); // undefined
    var b = 234;
    console.log(b); // 234
    a = 123;
    console.log(a); // 123
    function a() {}
    var a;
    var b = function() {}
    console.log(a); // 123
    console.log(b); // function() {}
}
test(1);
---------------------------------------------------------------------------------
1.函数声明整体提升: 不管是在函数声明之前调用函数还是在函数声明之后调用函数,其本质都是在函数声明之后
调用函数,他始终会把函数声明提升到逻辑的最前面;
    console.log(a); // function a() {}
    var a = 123;
    function a() {};
2.变量 声明提升: 只是将定义变量部分提升,赋值部分不提升;
    console.log(a); // undefined
    var a = 123;
3.GO全局状态下,预解析没有第三步,并且第一步是生成一个GO对象; GO === window
    var a = 123;
    function a() {};
    console.log(a); //123
4.
function test() {
    var a = b = 123;
    console.log(window.a); //undefined GO里面没有a定义
    console.log(window.b); //123
    console.log(a); //123
    console.log(b); //123 AO里面没有的话往上找GO
}
test();
-----------------------------------------------------------------
GO{
    b : undefined,123
}
AO{
    a : undefined,123
}
-----------------------------------------
5.
console.log(test); // function test() {} 和下面打印出来的fun指的不一样
function test(test) {
    console.log(test); // function test() {}
    var test = 234;
    console.log(test); // 234
    function test() {}
}
test(1);
-------------------------------------------
GO{
    test : function test() {} 
}
AO{
    test ; undefined,1,function test() {},234
}
----------------------------------------------------------
6.
var global = 100;
function fn() {
    console.log(global); //100 先去AO里面找,AO里面没有,再去GO上找
}
fn();
------------------------------------------------------------------
GO{
    global : undefined,100
    fn : function fn() {},
}
AO{
    
}
----------------------------------------------------
7.
global = 100;
function fn() {
    console.log(global); //undefined
    global = 200; //可以改变AO里对应属性的值
    console.log(global); //200
    var global = 300;
}
fn();
console.log(global); //100
var global;
----------------------------------------------------
GO{
    global : undefined,100
    fn : function fn() {}
}
AO{
    global : undefined,200,300
}
-------------------------------------------
8.
function test() {
    console.log(b); //undefined
    if(a) { //a = undefined,所以b = 100不执行
        var b = 100; //AO 提升
    }
    console.log(b); //undefined
    c = 234;
    console.log(c); //234
}
var a;
test();
console.log(a); //undefined
a = 10;
console.log(a); //10
console.log(c); //234
-------------------------
GO{
    a : undefined,10
    c : 234,
    test : function test() {}
}
AO{
	b : undefined
}
-----------------------------------------
9.
function bar() {
    return foo; //function foo() {} 在完成预编译第四步后进来走程序直接return,必然返回function
    foo = 10;
    function foo() {}
    var foo = 11;
}
console.log(bar());
----------------------------------
10.
console.log(bar());
function bar() {
	console.log(foo); //function foo() {}
    foo = 10;
    console.log(foo); //10
    function foo() {}
    var foo = 11;
    return foo; //11 
}
----------------------------------------------
11.
a = 100;
function demo(e) {
    function e() {}
    arguments[0] = 2; //实参列表,传参与传参的形参位相映射即 e = 2
    console.log(e); //2
    if(a) { //a = undefined,所以if里面的语句不执行
        var b = 123;
        function c() {
        
        }
    }
    var c;
    a = 10;
    var a;
    console.log(b); //undefined
    f = 123;
    console.log(c); //理想状态下是function,因为规定里刚刚新添if语句里面不能定义函数
    console,log(a); //10
}
var a;
demo(1);
console.log(a); //100
console.log(f); //123
----------------------------------------------------------------------
GO{
    a : undefined,100
    demo : function demo() {}
    f : 123
}
AO{
    e : undefined,1,function e() {},2
    b : undefined
    c : undefined,function c() {}
    a : undefined,10
}
```



## 15.构造器

### 构造函数实现原理

1. 在函数体最前面隐式的加上`this = {}`。
2. 执行`this.xxx = xxx`。
3. 隐式的返回`this`。

```js
function Student(name, age, sex) {
    	// var this = {
        //   name : "",
        //   age : "",
        //   sex : "",
    	// };
    	this.name = name;
    	this.age = age;
    	this.sex = sex;
    	this.grade = 2017;
    	// return this;
}
console.log(new Student('sxw', 24, male).name); // 这样也能调用对象
```



## 16.包装类

```js
new String();
new Boolean();
new Number();
```

- 数字123是原始值，原始值是不能有属性和方法的，属性和方法只有对象有，是对象独有的特性 
- 正常的数字有原始值类型和对象类型，原始值类型数字是没有属性和方法的，对象类型有 
- 正常的字符串有原始值类型和对象类型
- 正常的布尔值有原始值类型和对象类型
- 数字对象能够参与运算，但是运算的结果就不是对象了，又变回数字了 

==注意==：`undefined`和`null`是不能设置属性的。

`undefined`和`null`不能调用`toString`方法，`undefined.toString()`报错 / `null.toString()`报错。

```js
var num = 4; 
num.len = 3; // 经历了包装类

// new Number(4).len = 3; 然后delete 
// 电脑会新建一个数字对象,让这个数字对象的len等于3,来弥补我们操作的不足,完成之后又会自动去销毁;
// 当我们下面要去访问num.len的时候,电脑又会满足我们的要求,再去new一个新的number,然后把4放进去,再去访问len,因为一个对象是没有属性的,所以返回值为undefined;
console,log(num.len); // undefined
```

```js
var str = "abcd";
str.length = 2;
// new String('abcd').length = 2; --> delete
console.log(str); // "abcd"
console.log(str.length); // 4
```

```js
Number.prototype.toString = function() {} // 原型链终端
// Number.prototype._proto_ = Object.prototype
// Object.prototype.toString = function() {}
```



## 17.原型

- 查看原型：隐式属性`_proto_`
- 查看原型：显式属性`prototype`
- 查看对象的构造函数：`constructor`

一开始系统给我们生成原型的时候，就自带了一个属性叫构造器`constructor`，目的就是让构造函数构造出来的对象想找对应的构造函数的时候能够找到。

`prototype`是函数一定义就会有的，在没有给她定义的时候是一个空对象。

```js
// Person.prototype = {}  原型
Person.prototype.name = "hehe";
function Person() {};
var person = new Person(); 
```

```js
function Person() {
    // var this ={
    //       _proto_ : Person.prototype;
    // 当我们要找对象上的属性时,如果对象上没有我们要找的属性,就会通过_proto_指向的索引,到_proto_后面的值对应的身上去找有没有我们需要的属性。相当于连接的关系,把原型和自己通过_proto_连接到了一起。_proto_后面存的是当前对象的原型,_proto_就是一个指向,但是我们可以改变这个指向,间接的去改变对象的原型;
    // };
}
```

```js
Person.prototype.name = 'sunny';
function Person() {
    // var this = {_proto_ : Person.prototype}
}
var person = new Person();
Person.prototype = { // 把原型给修改了,相当于换了个新对象
    name : 'sxw'
}
// 原型上的name是sunny
```

```js
function Person() {
    // var this = {_proto_ : Person.prototype}
}
Person.prototype.name = 'sunny';
var person = new Person(); // 这个相当于在new之后修改的原型
Person.prototype = {
    name : 'cherry';
}
```

### `Object.create()`

绝大多数对象最终都会继承`Object.prototype`，但是`Object.create()`例外。

```js
// 一个对象的原型只能是对象 / Null 
Object.create(原型); // 括号里只能填 Object / null 
Object.create(null); // 这样构造出来的对象没有原型,没有.toString();方法,但他是一个对象,这个对象就这么奇怪的没有原型;
```

对象是通过`_proto_`索引找到原型的：

```js
Grand.prototype._proto_ = Object.prototype // 原型链的终端
Grand.prototype.lastName = "Deng";

function Grand() {
}
var grand = new Grand();

Father.prototype = grand;
function Father() {
    this.name = 'xuming';
}
var father = new Father();

son.prototype = father;
function Son() {
    this.hobbit = "smoke";
}
var son = new Son();

// 原型链上的原型只能通过自身去删除/修改属性,想要通过子孙后代去修改是实现不了的;
function Father() {
    this.name = 'xuming';
    this.fortune = {
        card1 : 'visa'
    };
    this.age = 100; 
}
var father = new Father();
Son.prototype = father;

function Son() {
    this.hobbit = "smoke";
}
var son = new Son();
// 虽然通过后代无法改变原型链上的属性,但可以通过给引用值加东西去达到修改的目的;
// son.fortune.name,这种是引用值自身的修改,这不算赋值的修改;
```

```js
var num = 123;
num.toString(); 
// 调用的是Number的toString,并不是调用原型上面的toString;
// 这是为啥呢?因为当我们跳过自身直接去访问原型链上面的时候:
Object.prototype.toString.call(123); 
// 返回[object Number],并不能返回对应数字的字符串,所以系统内部给我们重新定义了一个。
```

```js
var obj = Object.create(null);
obj.toString = function() {
    return '我是sxw';
}
document.write(obj); // 打印的是 我是sxw;
document.write(obj.toString()); // 但是如果我们上面说过采用Object.create(原型);实现对象没有原型,那么就不能调用toString方法了,除非我们人为又去添加一个toString方法
```

