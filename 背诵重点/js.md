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

- `break` 立即跳出代码块或循环
- `continue` 立即终止本轮循环，返回到循环结构的头部，开始下一轮循环

## 3.数据类型

基础类型：`number`、`string`、`boolean`

引用类型：`object`

特殊类型：`null`、`undefined`

`ES6`新增：`symbol`

### 判断数据类型

`typeof` 运算符

`instanceof` 运算符

`Object.prototype.toString` 方法

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
        "[object Array]": "array",
        "[object Object]": "object",
        "[object Number]" : "number - object",
        "[object Boolean]" : "boolean - object",
        "[object String]" : "string - object"
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

返回对象的类型字符串，可用做判断值的类型。

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

