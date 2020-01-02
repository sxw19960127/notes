# 内置类型

`JS`中分为七种内置类型，七种内置类型又分为两大类型：

- 基本类型：`number`、`string`、`boolean`、`null`、`undefined`、`symbol`
- 对象：`object`

其中，`JS`的数字类型是浮点类型的，没有整型。并且浮点类型基于`IEEE754`标准实现，在使用中会遇到某些`bug`。

`NaN`也属于`number`类型，并且`NaN`不等于自身。

```js
let a = 111; // 这只是字面量,不是number类型
a.toString(); // 在使用的时候才会转换为对象类型

对于基本类型来说,如果使用字面量的方式,那么这个变量只是一个字面量,只有在必要的时候才会转换为对应的类型。
```

```js
let a = { name: 'sxw' }
let b = a
b.name = 'iu'
console.log(a.name) // iu

对象是引用类型,在使用过程中会遇到深拷贝和浅拷贝的问题。
```



# `Typeof`

`typeof`对于基本类型，除了`null`其他都可以正确显示类型。

```js
typeof 1 // 'number'
typeof '1' // 'string'
typeof true // 'boolean'
typeof undefined // 'undefined'
typeof Symbol() // 'symbol'
typeof a // a并没有声明,但还是会返回undefined
```

```js
typeof [] // 'object'
typeof {} // 'object'
typeof console.log // 'function'

在对象中,除了函数其他都会显示object
```

```js
typeof null // 'object'

对于null,虽然它是基本类型,但还是会返回object,这是一个存在了很久的bug。在JS的最初版本中,使用的是32位系统,为了性能考略,使用了低位存储变量的类型信息。`000`开头代表对象,然而`null`表示为全零,所以将它错误的判断为`object`。虽然现在的内部类型判断代码已经改变了,但是对于这个bug却一直流传了下来。
```

```js
// 我们也可以这样来判断undefined
let a
a === undefined // 但是undefined不是保留字,能够在低版本浏览器中被赋值
let undefined = 1 // 这样判断就会出错

// 推荐使用void后面随便跟上一个组成表达式,返回的就是undefined
a === void 0 // 返回undefined
```

如果我们想获得一个变量的正确类型，可通过`Object.prototype.toString.call()`方法，返回`[object Type]`类型字符串。



# 类型转换

## 转`Boolean`

在条件判断的时候，除了`undefined`、`null`、`NaN`、`''`、`false`、`0`、`-0`，其他所有值都转为`true`，包括所有对象。

## 对象转基本类型

对象转基本类型的时候，会先调用`valueOf`方法，然后调用`toString`方法。并且这两个方法都可以被重写。

```js
let a = {
    valueOf() {
        return 0
    }
}
```

```js
// 当然也可以重写`Symbol.toPrimitive`,该方法在转基本类型的时候,调用的优先级最高。
let a = {
    [Symbol.toPrimitive]() {
        return 2
    },
    valueOf() {
        return 0
    },
    toString() {
        return '1'
    }
}
1 + a // 3
'1' + a // '12'
```

## 四则运算符

只有加法运算的时候，当其中一方是字符串类型，另一方也会被转为字符串类型。

其他运算，只要其中一方是数字，那么另一方就会被转为数字。

并且加法运算会触发三种类型转换：值转为原始值、数字、字符串。

```js
1 + '1' // '11'
2 * '2' // 4
[1,2] + [2,1] // '1,22,1'
// [1,2].toString() -> '1,2'
// [2,1].toString() -> '2,1'
// '1,2' + '2,1' = '1,22,1'
```

```js
// 对于加号需要注意这个表达式`'a'+ + 'b'`
'a' +  + 'b' // 'aNaN'
// 因为 + 'b' -> NaN
// 也许你在一些代码中看到过 + '1' -> 1
```

## `==`操作符

![==操作符](D:\notes\前端进阶之道\img\==操作符.jpg)

上图中的`toPrimitive`就是对象转基本类型。

```js
[] -- ![] // true

// []转为true,然后取反变为false
[] == false
// 根据第8条得出
[] == ToNumber(false)
[] == 0
// 根据第10条得出
ToPrimitive([]) == 0
// [].toString() -> ''
'' == 0
// 根据第6条得出
0 == 0 // true
```

## 比较运算符

- 如果是对象，就通过`toPrimitive`转换对象
- 如果是字符串，就通过`unicode`字符索引来比较



# 原型

![原型](D:\notes\前端进阶之道\img\原型.jpg)

每个函数都有`prototype`属性，除了`Function.prototype.bind()`，该属性指向原型。

每个对象都有`__proto__`属性，指向了创建该对象的构造函数的原型。其实这个属性指向了`[[prototype]]`，但是`[[prototype]]`是内部属性，我们并不能访问到，所以使用`__proto__`来访问。

对象可以通过`__proto__`来寻找不属于该对象的属性，`__proto__`将对象连接起来组成了原型链。

[深入解析原型中的各个难点](https://github.com/KieSun/Dream/issues/2)



# `new`

在调用`new`的过程中会发生以下四件事情：

1. 新生成一个对象
2. 链接到原型
3. 绑定`this`
4. 返回新对象

```js
// 自己实现一个new
function create() {
    // 创建一个空的对象
    let obj = new Object()
    // 获得构造函数
    let Con = [].shift.call(arguments)
    // 链接到原型
    obj.__proto__ = Con.prototype
    // 绑定this,执行构造函数
    let result = Con.apply(obj, arguments)
    // 确保new出来的是个对象
    return typeof result === 'object' ? result : obj
}
```

对于实例对象来说，都是通过`new`产生的，无论是`function foo()`还是`let a = { b: 1 }`。

对于创建一个对象来说，更推荐使用字面量的方式创建对象（无论性能上还是可读性）。因为你使用`new Object()`的方式创建对象需要通过作用域链一层层找到`Object`，但是你使用字面量的方式就没有这个问题。

```js
function Foo() {}
// function就是一个语法糖,内部等价于new Function()
let a = { b: 1 }
// 这个字面量内部也是使用了new Object()
```

```js
// 使用new使用需要注意运算符的优先级
function Foo() {
    return this;
}
Foo.getName = function() {
    console.log('1')
}
Foo.prototype.getName = function() {
    console.log('2')
}
new Foo.getName() //1
new Foo().getName() //2

// 从下图可以看出,`new Foo()`的优先级大于`new Foo`,所以上述代码的执行顺序为
new (Foo.getName()); // 先执行了`Foo.getName()`，结果为1
(new Foo()).getName(); // 先执行了`new Foo()`生成一个实例,然后通过原型链找到了`Foo`上的`getName`函数,结果为2
```

![new](D:\notes\前端进阶之道\img\new.jpg)



# `instanceof`

可以正确的判断对象的类型，因为内部机制是通过判断对象的原型链中是不是能找到类型的`prototype`。

```js
// 实现instanceof
function instanceof(left,right) {
    // 获得类型的原型
    let prototype = right.prototype
    // 获得对象的原型
    left = left.__proto__
    // 判断对象的类型是否等于类型的原型
    while(true) {
        if(left === null) {
            return false
        }
        if(prototype === left) {
            return true
        }
        left = left.__proto__
    }
}
```



# `this`

```js
function foo() {
    console.log(this.a)
}
var a = 1
foo()

var obj = {
    a: 2,
    foo: foo
}
obj.foo()

// 以上两种情况,this只依赖于调用函数前的对象,优先级是第二个情况大于第一个情况
// 以下情况是优先级最高的,this只会绑定在`c`上,不会被任何方式所修改`this`指向
var c = new foo()
c.a = 3
console.log(c.a)

// 还有一种是利用call,apply,bind改变this,这个优先级仅次于new
```

```js
function a() {
    return () => {
        return () => {
            console.log(this)
        }
    }
}
console.log(a()()())
// 箭头函数其实是没有this的,这个函数中的this只取决于他外面的第一个不是箭头函数的函数的this。上面栗子中,因为调用a符合前面代码中的第一个情况,所以this是window，并且this一旦绑定了上下文,就不会被任何代码所改变了。
```



# 执行上下文

当执行`JS`代码的时候，会产生三种执行上下文：

- 全局执行上下文
- 函数执行上下文
- `eval`执行上下文

每个执行上下文中都有三个重要的属性：

- 变量对象`VO`，包含变量、函数声明、函数的形参，该属性只能在全局上下文中访问
- 作用域链（`JS`采用词法作用域，也就是说变量的作用域是在定义时候就决定了）
- `this`

```js
var a = 10
function foo(i) {
    var b = 20
}
foo()
上述代码,执行栈中有两个上下文: 全局上下文和函数`foo`上下文。
```

```js
stack = [
    globalContext,
    fooContext
]
```

```js
// 对于全局上下文来说,`VO`大概是这样的
globalContext.VO === globe
globalContext.VO = {
    a: undefined,
    foo: <Function>
}
```

```js
// 对于函数`foo`来说,`VO`不能访问,只能访问到活动对象`AO`
fooContext.VO === foo.AO
fooContext.AO {
    i: undefined,
    b: undefined,
    arguments: <>
}
// arguments是函数独有的对象(箭头函数没有)
// 该对象是一个伪数组,有length属性且可以通过下标访问元素,对象中的callee属性代表函数本身,caller属性代表函数的调用者
```

```js
// 对于作用域链,可以把它理解成包含自身变量对象和上级变量对象的列表,通过`[[Scope]]`属性查找上级变量。
fooContext.[[Scope]] = [
    globalContext.VO
]
fooContext.Scope = fooContext.[[Scope]] + fooContext.VO
fooContext.Scope = [
    fooContext.VO,
    globalContext.VO
]
```

```js
// 老生常谈的问题,在生成执行上下文时,会有两个阶段:
// 第一个阶段是创建的阶段(具体步骤是创建 VO),JS 解释器会找出需要提升的变量和函数,并且给他们提前在内存中开辟好空间,函数的话会将整个函数存入内存中,变量只声明并且赋值为undefined。
// 第二个阶段,也就是代码执行阶段,我们可以直接提前使用。
b() // call b
console.log(a) // undefined

var a = 'Hello world'

function b() {
	console.log('call b')
}
```

```js
// 在提升的过程中,相同的函数会覆盖上一个函数,并且函数优先于变量提升
b() // call b second
function b() {
	console.log('call b fist')
}
function b() {
	console.log('call b second')
}
var b = 'Hello world'
```

`ES6`中引入了`let`。`let`不能在声明前使用，但是这并不是常说的 `let` 不会提升，`let` 提升了声明但没有赋值，因为临时死区导致了并不能在声明前使用。

```js
// 非匿名的立即执行函数,当JS解释器在遇到非匿名的立即执行函数时,会创建一个辅助的特定对象,然后将函数名称作为这个对象的属性,因此函数内部才可以访问到 foo,但是这个值又是只读的,所以对它的赋值并不生效,所以打印的结果还是这个函数,并且外部的值也没有发生更改。
var foo = 1
(function foo() {
    foo = 10
    console.log(foo)
}()) // -> ƒ foo() { foo = 10 ; console.log(foo) }
```

```js
specialObject = {};
Scope = specialObject + Scope;
foo = new FunctionExpression;
foo.[[Scope]] = Scope;
specialObject.foo = foo; // {DontDelete}, {ReadOnly}
delete Scope[0]; // remove specialObject from the front of scope chain
```



# 闭包

函数 A 返回了一个函数 B，并且函数 B 中使用了函数 A 的变量，函数 B 就被称为闭包。

```js
function A() {
  let a = 1
  function B() {
      console.log(a)
  }
  return B
}
```

你是否会疑惑，为什么函数 A 已经弹出调用栈了，为什么函数 B 还能引用到函数 A 中的变量。因为函数 A 中的变量这时候是存储在堆上的。现在的 JS 引擎可以通过逃逸分析辨别出哪些变量需要存储在堆上，哪些需要存储在栈上。

```js
//经典面试题，循环中使用闭包解决 var 定义函数的问题
//首先因为 setTimeout 是个异步函数，所有会先把循环全部执行完毕，这时候 i 就是 6 了，所以会输出一堆 6。
for ( var i=1; i<=5; i++) {
	setTimeout( function timer() {
		console.log( i );
	}, i*1000 );
}
```

```js
// 解决办法两种，第一种使用闭包
for (var i = 1; i <= 5; i++) {
  (function(j) {
    setTimeout(function timer() {
      console.log(j);
    }, j * 1000);
  })(i);
}
```

```js
//第二种就是使用 setTimeout 的第三个参数
for ( var i=1; i<=5; i++) {
	setTimeout( function timer(j) {
		console.log( j );
	}, i*1000, i);
}
```

```js
// 第三种就是使用 let 定义 i 了
for ( let i=1; i<=5; i++) {
	setTimeout( function timer() {
		console.log( i );
	}, i*1000 );
}
```

```js
// 因为对于 let 来说，他会创建一个块级作用域，相当于
{ // 形成块级作用域
  let i = 0
  {
    let ii = i
    setTimeout( function timer() {
        console.log( ii );
    }, i*1000 );
  }
  i++
  {
    let ii = i
  }
  i++
  {
    let ii = i
  }
  ...
}
```



# 深浅拷贝

```js
let a = {
    age: 1
}
let b = a
a.age = 2
console.log(b.age) // 2
// 从上述例子中我们可以发现,如果给一个变量赋值一个对象,那么两者的值会是同一个引用,其中一方改变,另一方也会相应改变。
// 通常在开发中我们不希望出现这样的问题,我们可以使用浅拷贝来解决这个问题。
```

## 浅拷贝

```js
// 通过 Object.assign 来解决这个问题。
let a = {
    age: 1
}
let b = Object.assign({}, a)
a.age = 2
console.log(b.age) // 1
```

```js
// 通过展开运算符（…）来解决
let a = {
    age: 1
}
let b = {...a}
a.age = 2
console.log(b.age) // 1
```

```js
// 通常浅拷贝就能解决大部分问题了，但是当我们遇到如下情况就需要使用到深拷贝了
// 浅拷贝只解决了第一层的问题，如果接下去的值中还有对象的话，那么就又回到刚开始的话题了，两者享有相同的引用。要解决这个问题，我们需要引入深拷贝。
let a = {
    age: 1,
    jobs: {
        first: 'FE'
    }
}
let b = {...a}
a.jobs.first = 'native'
console.log(b.jobs.first) // native
```

## 深拷贝

```js
// 通过 JSON.parse(JSON.stringify(object)) 来解决。
let a = {
    age: 1,
    jobs: {
        first: 'FE'
    }
}
let b = JSON.parse(JSON.stringify(a))
a.jobs.first = 'native'
console.log(b.jobs.first) // FE
```

但是该方法也是有局限性的：

- 会忽略 `undefined`
- 会忽略 `symbol`
- 不能序列化函数
- 不能解决循环引用的对象

```js
// 如果你有这么一个循环引用对象，你会发现你不能通过该方法深拷贝
let obj = {
  a: 1,
  b: {
    c: 2,
    d: 3,
  },
}
obj.c = obj.b
obj.e = obj.a
obj.b.c = obj.c
obj.b.d = obj.b
obj.b.e = obj.b.c
let newObj = JSON.parse(JSON.stringify(obj))
console.log(newObj)
```

```js
// 在遇到函数、 undefined 或者 symbol 的时候，该对象也不能正常的序列化
let a = {
    age: undefined,
    sex: Symbol('male'),
    jobs: function() {},
    name: 'yck'
}
let b = JSON.parse(JSON.stringify(a))
console.log(b) // {name: "yck"}

// 你会发现在上述情况中，该方法会忽略掉函数和 undefined 
```

但是在通常情况下，复杂数据都是可以序列化的，所以这个函数可以解决大部分问题，并且该函数是内置函数中处理深拷贝性能最快的。当然如果你的数据中含有以上三种情况下，可以使用 [lodash 的深拷贝函数](https://lodash.com/docs##cloneDeep)。

```js
// 如果你所需拷贝的对象含有内置类型并且不包含函数，可以使用 MessageChannel
function structuralClone(obj) {
  return new Promise(resolve => {
    const {port1, port2} = new MessageChannel();
    port2.onmessage = ev => resolve(ev.data);
    port1.postMessage(obj);
  });
}

var obj = {a: 1, b: {
    c: b
}}
// 注意该方法是异步的
// 可以处理 undefined 和循环引用对象
(async () => {
  const clone = await structuralClone(obj)
})()
```



# 模块化

```js
// file a.js
export function a() {}
export function b() {}
// file b.js
export default function() {}

import {a, b} from './a.js'
import XXX from './b.js'
```

## `CommonJS`

是 Node 独有的规范，浏览器中使用就需要用到 `Browserify` 解析了。

```js
// a.js
module.exports = {
    a: 1
}
// or
exports.a = 1

// b.js
var module = require('./a.js')
module.a // -> log 1
```

上述代码中，`module.exports` 和 `exports` 容易混淆，让我们来看看大致内部实现。

```js
var module = require('./a.js')
module.a
// 这里其实就是包装了一层立即执行函数，这样就不会污染全局变量了，
// 重要的是 module 这里，module 是 Node 独有的一个变量
module.exports = {
    a: 1
}
// 基本实现
var module = {
  exports: {} // exports 就是个空对象
}
// 这个是为什么 exports 和 module.exports 用法相似的原因
var exports = module.exports
var load = function (module) {
    // 导出的东西
    var a = 1
    module.exports = a
    return module.exports
};
```

再来说说 `module.exports` 和 `exports`，用法其实是相似的，但是不能对 `exports` 直接赋值，不会有任何效果。

对于 `CommonJS` 和 ES6 中的模块化的两者区别是：

- 前者支持动态导入，也就是 `require(${path}/xx.js)`，后者目前不支持，但是已有提案
- 前者是同步导入，因为用于服务端，文件都在本地，同步导入即使卡住主线程影响也不大。而后者是异步导入，因为用于浏览器，需要下载文件，如果也采用同步导入会对渲染有很大影响
- 前者在导出时都是值拷贝，就算导出的值变了，导入的值也不会改变，所以如果想更新值，必须重新导入一次。但是后者采用实时绑定的方式，导入导出的值都指向同一个内存地址，所以导入值会跟随导出值变化
- 后者会编译成 `require/exports` 来执行的

## `AMD`

由 `RequireJS` 提出的

```js
// AMD
define(['./a', './b'], function(a, b) {
    a.do()
    b.do()
})
define(function(require, exports, module) {
    var a = require('./a')
    a.doSomething()
    var b = require('./b')
    b.doSomething()
})
```



# 防抖

你是否遇到一个问题，在滚动事件中需要做个复杂计算或者实现一个按钮的防二次点击操作。

这些需求都可以通过函数防抖动来实现。尤其是第一个需求，如果在频繁的事件回调中做复杂计算，很有可能导致页面卡顿，不如将多次计算合并为一次计算，只在一个精确点做操作。

PS：防抖和节流的作用都是防止函数多次调用。区别在于，假设一个用户一直触发这个函数，且每次触发函数的间隔小于wait，防抖的情况下只会调用一次，而节流的情况会每隔一定时间（参数wait）调用函数。

```js
// func是用户传入需要防抖的函数
// wait是等待时间
const debounce = (func, wait = 50) => {
  // 缓存一个定时器id
  let timer = 0
  // 这里返回的函数是每次用户实际调用的防抖函数
  // 如果已经设定过定时器了就清空上一次的定时器
  // 开始一个新的定时器，延迟执行用户传入的方法
  return function(...args) {
    if (timer) clearTimeout(timer)
    timer = setTimeout(() => {
      func.apply(this, args)
    }, wait)
  }
}
// 不难看出如果用户调用该函数的间隔小于wait的情况下，上一次的时间还未到就被清除了，并不会执行函数
```

这是一个简单版的防抖，但是有缺陷，这个防抖只能在最后调用。

一般的防抖会有immediate选项，表示是否立即调用。这两者的区别，举个栗子来说：

- 例如在搜索引擎搜索问题的时候，我们当然是希望用户输入完最后一个字才调用查询接口，这个时候适用延迟执行的防抖函数，它总是在一连串（间隔小于wait的）函数触发之后调用。
- 例如用户给`interviewMap`点star的时候，我们希望用户点第一下的时候就去调用接口，并且成功之后改变star按钮的样子，用户就可以立马得到反馈是否star成功了，这个情况适用立即执行的防抖函数，它总是在第一次调用，并且下一次调用必须与前一次调用的时间间隔大于wait才会触发。

下面我们来实现一个带有立即执行选项的防抖函数：

```js
// 这个是用来获取当前时间戳的
function now() {
  return +new Date()
}
/**
 * 防抖函数，返回函数连续调用时，空闲时间必须大于或等于 wait，func 才会执行
 *
 * @param  {function} func        回调函数
 * @param  {number}   wait        表示时间窗口的间隔
 * @param  {boolean}  immediate   设置为ture时，是否立即调用函数
 * @return {function}             返回客户调用函数
 */
function debounce (func, wait = 50, immediate = true) {
  let timer, context, args

  // 延迟执行函数
  const later = () => setTimeout(() => {
    // 延迟函数执行完毕，清空缓存的定时器序号
    timer = null
    // 延迟执行的情况下，函数会在延迟函数中执行
    // 使用到之前缓存的参数和上下文
    if (!immediate) {
      func.apply(context, args)
      context = args = null
    }
  }, wait)

  // 这里返回的函数是每次实际调用的函数
  return function(...params) {
    // 如果没有创建延迟执行函数（later），就创建一个
    if (!timer) {
      timer = later()
      // 如果是立即执行，调用函数
      // 否则缓存参数和调用上下文
      if (immediate) {
        func.apply(this, params)
      } else {
        context = this
        args = params
      }
    // 如果已有延迟执行函数（later），调用的时候清除原来的并重新设定一个
    // 这样做延迟函数会重新计时
    } else {
      clearTimeout(timer)
      timer = later()
    }
  }
}
```

总结：

- 对于按钮防点击来说：如果函数是立即执行的，就立即调用，如果函数是延迟执行的，就缓存上下文和参数，放到延迟函数中去执行。一旦我开始一个定时器，只要我定时器还在，你每次点击我都重新计时。一旦你点累了，定时器时间到，定时器重置为 `null`，就可以再次点击了。
- 对于延时执行函数来说的实现：清除定时器ID，如果是延迟调用就调用函数



# 节流

防抖动和节流本质是不一样的。

防抖动是将多次执行变为最后一次执行，节流是将多次执行变成每隔一段时间执行。

```js
/**
 * underscore 节流函数，返回函数连续调用时，func 执行频率限定为 次 / wait
 *
 * @param  {function}   func      回调函数
 * @param  {number}     wait      表示时间窗口的间隔
 * @param  {object}     options   如果想忽略开始函数的的调用，传入{leading: false}。
 *                                如果想忽略结尾函数的调用，传入{trailing: false}
 *                                两者不能共存，否则函数不能执行
 * @return {function}             返回客户调用函数
 */
_.throttle = function(func, wait, options) {
    var context, args, result;
    var timeout = null;
    // 之前的时间戳
    var previous = 0;
    // 如果 options 没传则设为空对象
    if (!options) options = {};
    // 定时器回调函数
    var later = function() {
      // 如果设置了 leading，就将 previous 设为 0
      // 用于下面函数的第一个 if 判断
      previous = options.leading === false ? 0 : _.now();
      // 置空一是为了防止内存泄漏，二是为了下面的定时器判断
      timeout = null;
      result = func.apply(context, args);
      if (!timeout) context = args = null;
    };
    return function() {
      // 获得当前时间戳
      var now = _.now();
      // 首次进入前者肯定为 true
	  // 如果需要第一次不执行函数
	  // 就将上次时间戳设为当前的
      // 这样在接下来计算 remaining 的值时会大于0
      if (!previous && options.leading === false) previous = now;
      // 计算剩余时间
      var remaining = wait - (now - previous);
      context = this;
      args = arguments;
      // 如果当前调用已经大于上次调用时间 + wait
      // 或者用户手动调了时间
 	  // 如果设置了 trailing，只会进入这个条件
	  // 如果没有设置 leading，那么第一次会进入这个条件
	  // 还有一点，你可能会觉得开启了定时器那么应该不会进入这个 if 条件了
	  // 其实还是会进入的，因为定时器的延时
	  // 并不是准确的时间，很可能你设置了2秒
	  // 但是他需要2.2秒才触发，这时候就会进入这个条件
      if (remaining <= 0 || remaining > wait) {
        // 如果存在定时器就清理掉否则会调用二次回调
        if (timeout) {
          clearTimeout(timeout);
          timeout = null;
        }
        previous = now;
        result = func.apply(context, args);
        if (!timeout) context = args = null;
      } else if (!timeout && options.trailing !== false) {
        // 判断是否设置了定时器和 trailing
	    // 没有的话就开启一个定时器
        // 并且不能不能同时设置 leading 和 trailing
        timeout = setTimeout(later, remaining);
      }
      return result;
    };
  };
```



# 继承

```js
function Super() {}
Super.prototype.getNumber = function() {
  return 1
}

function Sub() {}
let s = new Sub()
Sub.prototype = Object.create(Super.prototype, {
  constructor: {
    value: Sub,
    enumerable: false,
    writable: true,
    configurable: true
  }
})
```

以上继承实现的思路就是将子类的原型设置为父类的原型。

`ES6`中，可以通过 `class` 语法轻松解决这个问题：

```js
class MyDate extends Date {
  test() {
    return this.getTime()
  }
}
let myDate = new MyDate()
myDate.test()
```

但是 ES6 不是所有浏览器都兼容，所以我们需要使用 Babel 来编译这段代码。

如果你使用编译过得代码调用 `myDate.test()` 你会惊奇地发现出现了报错。因为在 JS 底层有限制，如果不是由 `Date` 构造出来的实例的话，是不能调用 `Date` 里的函数的。这也侧面的说明了：**ES6 中的 `class` 继承与 ES5 中的一般继承写法是不同的**。

既然底层限制了实例必须由 `Date` 构造出来，那么我们可以改变下思路实现继承：

```js
function MyData() {

}
MyData.prototype.test = function () {
  return this.getTime()
}
let d = new Date()
Object.setPrototypeOf(d, MyData.prototype)
Object.setPrototypeOf(MyData.prototype, Date.prototype)
```

以上继承实现思路：**先创建父类实例** => 改变实例原先的 `_proto__` 转而连接到子类的 `prototype` => 子类的 `prototype` 的 `__proto__` 改为父类的 `prototype`。



# `call,apply,bind`

首先说下前两者的区别，`call` 和 `apply` 都是为了解决改变 `this` 的指向。作用都是相同的，只是传参的方式不同。

除了第一个参数外，`call` 可以接收一个参数列表，`apply` 只接受一个参数数组。

```js
let a = {
    value: 1
}
function getValue(name, age) {
    console.log(name)
    console.log(age)
    console.log(this.value)
}
getValue.call(a, 'yck', '24')
getValue.apply(a, ['yck', '24'])
```

## 模拟实现`call`和`apply`

```js
Function.prototype.myCall = function (context) {
  var context = context || window
  // 给 context 添加一个属性
  // getValue.call(a, 'yck', '24') => a.fn = getValue
  context.fn = this
  // 将 context 后面的参数取出来
  var args = [...arguments].slice(1)
  // getValue.call(a, 'yck', '24') => a.fn('yck', '24')
  var result = context.fn(...args)
  // 删除 fn
  delete context.fn
  return result
}
```

以上就是 `call` 的思路，`apply` 的实现也类似：

```js
Function.prototype.myApply = function (context) {
  var context = context || window
  context.fn = this

  var result
  // 需要判断是否存储第二个参数
  // 如果存在，就将第二个参数展开
  if (arguments[1]) {
    result = context.fn(...arguments[1])
  } else {
    result = context.fn()
  }

  delete context.fn
  return result
}
```

`bind` 和其他两个方法作用也是一致的，只是该方法会返回一个函数，并且我们可以通过 `bind` 实现柯里化。

同样的，也来模拟实现下 `bind`：

```js
Function.prototype.myBind = function (context) {
  if (typeof this !== 'function') {
    throw new TypeError('Error')
  }
  var _this = this
  var args = [...arguments].slice(1)
  // 返回一个函数
  return function F() {
    // 因为返回了一个函数，我们可以 new F()，所以需要判断
    if (this instanceof F) {
      return new _this(...args, ...arguments)
    }
    return _this.apply(context, args.concat(...arguments))
  }
}
```



# `Promise`

`Promise`解决了回调地狱的问题。

可以把 Promise 看成一个状态机。初始是 `pending` 状态，可以通过函数 `resolve` 和 `reject` ，将状态转变为 `resolved` 或者 `rejected` 状态，状态一旦改变就不能再次变化。

`then` 函数会返回一个 Promise 实例，并且该返回值是一个新的实例而不是之前的实例。因为 Promise 规范规定除了 `pending` 状态，其他状态是不可以改变的，如果返回的是一个相同实例的话，多个 `then` 调用就失去意义了。

对于 `then` 来说，本质上可以把它看成是 `flatMap`：

```js
// 三种状态
const PENDING = "pending";
const RESOLVED = "resolved";
const REJECTED = "rejected";
// promise 接收一个函数参数，该函数会立即执行
function MyPromise(fn) {
  let _this = this;
  _this.currentState = PENDING;
  _this.value = undefined;
  // 用于保存 then 中的回调，只有当 promise
  // 状态为 pending 时才会缓存，并且每个实例至多缓存一个
  _this.resolvedCallbacks = [];
  _this.rejectedCallbacks = [];

  _this.resolve = function (value) {
    if (value instanceof MyPromise) {
      // 如果 value 是个 Promise，递归执行
      return value.then(_this.resolve, _this.reject)
    }
    setTimeout(() => { // 异步执行，保证执行顺序
      if (_this.currentState === PENDING) {
        _this.currentState = RESOLVED;
        _this.value = value;
        _this.resolvedCallbacks.forEach(cb => cb());
      }
    })
  };

  _this.reject = function (reason) {
    setTimeout(() => { // 异步执行，保证执行顺序
      if (_this.currentState === PENDING) {
        _this.currentState = REJECTED;
        _this.value = reason;
        _this.rejectedCallbacks.forEach(cb => cb());
      }
    })
  }
  // 用于解决以下问题
  // new Promise(() => throw Error('error))
  try {
    fn(_this.resolve, _this.reject);
  } catch (e) {
    _this.reject(e);
  }
}

MyPromise.prototype.then = function (onResolved, onRejected) {
  var self = this;
  // 规范 2.2.7，then 必须返回一个新的 promise
  var promise2;
  // 规范 2.2.onResolved 和 onRejected 都为可选参数
  // 如果类型不是函数需要忽略，同时也实现了透传
  // Promise.resolve(4).then().then((value) => console.log(value))
  onResolved = typeof onResolved === 'function' ? onResolved : v => v;
  onRejected = typeof onRejected === 'function' ? onRejected : r => throw r;

  if (self.currentState === RESOLVED) {
    return (promise2 = new MyPromise(function (resolve, reject) {
      // 规范 2.2.4，保证 onFulfilled，onRjected 异步执行
      // 所以用了 setTimeout 包裹下
      setTimeout(function () {
        try {
          var x = onResolved(self.value);
          resolutionProcedure(promise2, x, resolve, reject);
        } catch (reason) {
          reject(reason);
        }
      });
    }));
  }

  if (self.currentState === REJECTED) {
    return (promise2 = new MyPromise(function (resolve, reject) {
      setTimeout(function () {
        // 异步执行onRejected
        try {
          var x = onRejected(self.value);
          resolutionProcedure(promise2, x, resolve, reject);
        } catch (reason) {
          reject(reason);
        }
      });
    }));
  }

  if (self.currentState === PENDING) {
    return (promise2 = new MyPromise(function (resolve, reject) {
      self.resolvedCallbacks.push(function () {
        // 考虑到可能会有报错，所以使用 try/catch 包裹
        try {
          var x = onResolved(self.value);
          resolutionProcedure(promise2, x, resolve, reject);
        } catch (r) {
          reject(r);
        }
      });

      self.rejectedCallbacks.push(function () {
        try {
          var x = onRejected(self.value);
          resolutionProcedure(promise2, x, resolve, reject);
        } catch (r) {
          reject(r);
        }
      });
    }));
  }
};
// 规范 2.3
function resolutionProcedure(promise2, x, resolve, reject) {
  // 规范 2.3.1，x 不能和 promise2 相同，避免循环引用
  if (promise2 === x) {
    return reject(new TypeError("Error"));
  }
  // 规范 2.3.2
  // 如果 x 为 Promise，状态为 pending 需要继续等待否则执行
  if (x instanceof MyPromise) {
    if (x.currentState === PENDING) {
      x.then(function (value) {
        // 再次调用该函数是为了确认 x resolve 的
        // 参数是什么类型，如果是基本类型就再次 resolve
        // 把值传给下个 then
        resolutionProcedure(promise2, value, resolve, reject);
      }, reject);
    } else {
      x.then(resolve, reject);
    }
    return;
  }
  // 规范 2.3.3.3.3
  // reject 或者 resolve 其中一个执行过得话，忽略其他的
  let called = false;
  // 规范 2.3.3，判断 x 是否为对象或者函数
  if (x !== null && (typeof x === "object" || typeof x === "function")) {
    // 规范 2.3.3.2，如果不能取出 then，就 reject
    try {
      // 规范 2.3.3.1
      let then = x.then;
      // 如果 then 是函数，调用 x.then
      if (typeof then === "function") {
        // 规范 2.3.3.3
        then.call(
          x,
          y => {
            if (called) return;
            called = true;
            // 规范 2.3.3.3.1
            resolutionProcedure(promise2, y, resolve, reject);
          },
          e => {
            if (called) return;
            called = true;
            reject(e);
          }
        );
      } else {
        // 规范 2.3.3.4
        resolve(x);
      }
    } catch (e) {
      if (called) return;
      called = true;
      reject(e);
    }
  } else {
    // 规范 2.3.4，x 为基本类型
    resolve(x);
  }
}
```



# `Generator`实现

是`ES6`中新增的语法，和`Promise`一样，都可以用来异步编程。它能够中断执行代码的特性，可以帮助我们控制异步代码的执行顺序。

```js
// 使用 * 表示这是一个 Generator 函数
// 内部可以通过 yield 暂停代码
// 通过调用 next 恢复执行
function* test() {
  let a = 1 + 2;
  yield 2;
  yield 3;
}
let b = test();
console.log(b.next()); // >  { value: 2, done: false }
console.log(b.next()); // >  { value: 3, done: false }
console.log(b.next()); // >  { value: undefined, done: true }
```

从以上代码可以发现，加上 `*` 的函数执行后拥有了 `next` 函数，也就是说函数执行后返回了一个对象。每次调用 `next` 函数可以继续执行被暂停的代码。

以下是 Generator 函数的简单实现：

```js
// cb 也就是编译过的 test 函数
function generator(cb) {
  return (function() {
    var object = {
      next: 0,
      stop: function() {}
    };

    return {
      next: function() {
        var ret = cb(object);
        if (ret === undefined) return { value: undefined, done: true };
        return {
          value: ret,
          done: false
        };
      }
    };
  })();
}
// 如果你使用 babel 编译后可以发现 test 函数变成了这样
function test() {
  var a;
  return generator(function(_context) {
    while (1) {
      switch ((_context.prev = _context.next)) {
        // 可以发现通过 yield 将代码分割成几块
        // 每次执行 next 函数就执行一块代码
        // 并且表明下次需要执行哪块代码
        case 0:
          a = 1 + 2;
          _context.next = 4;
          return 2;
        case 4:
          _context.next = 6;
          return 3;
		// 执行完毕
        case 6:
        case "end":
          return _context.stop();
      }
    }
  });
}
```



# `Map`、`Reduce`、`sort`

`Map` 作用是生成一个新数组，遍历原数组，将每个元素拿出来做一些变换然后 `append` 到新的数组中。

```js
[1, 2, 3].map((v) => v + 1)
// -> [2, 3, 4]
```

`Map` 有三个参数，分别是当前索引元素，索引，原数组

```js
['1','2','3'].map(parseInt)
//  parseInt('1', 0) -> 1
//  parseInt('2', 1) -> NaN
//  parseInt('3', 2) -> NaN
```

`Reduce` 作用是数组中的值组合起来，最终得到一个值。

```js
function a() {
    console.log(1);
}

function b() {
    console.log(2);
}

[a, b].reduce((a, b) => a(b()))
// -> 2 1
```

`sort`排序算法

```js
// 看上去正常的结果:
['Google', 'Apple', 'Microsoft'].sort(); // ['Apple', 'Google', 'Microsoft'];

// apple排在了最后:
['Google', 'apple', 'Microsoft'].sort(); // ['Google', 'Microsoft", 'apple']

// 无法理解的结果:
[10, 20, 1, 2].sort(); // [1, 10, 2, 20]
```

第二个排序把`apple`排在了最后，是因为字符串根据ASCII码进行排序，而小写字母a的ASCII码在大写字母之后。

第三个排序是因为`Array`的`sort()`方法默认把所有元素先转换为`String`再排序，结果`'10'`排在了`'2'`前面，因为字符1比字符2的ASCII码小。

```js
'use strict';
var arr = [10, 20, 1, 2];
    arr.sort(function (x, y) {
        if (x < y) {
            return -1;
        }
        if (x > y) {
            return 1;
        }
        return 0;
    });
    console.log(arr); // [1, 2, 10, 20]
```

`sort()`方法会直接对`Array`进行修改，它返回的结果仍是当前`Array`，一个栗子：

```js
var a1 = ['B', 'A', 'C'];
var a2 = a1.sort();
    a1; // ['A', 'B', 'C']
    a2; // ['A', 'B', 'C']
    a1 === a2; // true, a1和a2是同一对象
```



# `async`和`await`

`async`函数是`Generator`函数的语法糖。使用关键字`async`来表示，在函数内部使用`await`来表示异步。

比较于`Generator`，`Async`函数有以下4点优势：

- **内置执行器**。Generator 函数的执行必须依靠执行器，而 `Aysnc` 函数自带执行器，调用方式跟普通函数的调用一样
- **更好的语义**。`async` 和 `await` 相较于 `*` 和 `yield` 更加语义化
- **更广的适用性**。`co` 模块约定，`yield` 命令后面只能是`Thunk`函数或`Promise`对象。而 `async` 函数的 `await` 命令后面则可以是 Promise 或者 原始类型的值（Number，string，boolean，但这时等同于同步操作）
- **返回值是 Promise**。`async` 函数返回值是 Promise 对象，比 Generator 函数返回的 Iterator 对象方便，可以直接使用 `then()` 方法进行调用

定义`Fetch`方法，获取`github user`的信息：

```js
function fetchUser() { 
    return new Promise((resolve, reject) => {
        fetch('https://api.github.com/users/superman66')
        .then((data) => {
            resolve(data.json());
        }, (error) => {
            reject(error);
        })
    });
}
```

`Promise`方式：充满了`then()`方法，语义化不明显，代码流程不能很好的表示执行流程。

```js
function getUserByPromise() {
    fetchUser()
        .then((data) => {
            console.log(data);
        }, (error) => {
            console.log(error);
        })
}
getUserByPromise();
```

`Generator`方式：函数的执行需要依靠执行器，每次都需要通过 `g.next()` 的方式去执行。

```js
function* fetchUserByGenerator() {
    const user = yield fetchUser();
    return user;
}

const g = fetchUserByGenerator();
const result = g.next().value;
result.then((v) => {
    console.log(v);
}, (error) => {
    console.log(error);
})
```

`async` 函数完美的解决了上面两种方式的问题。流程清晰，直观、语义明显。操作异步流程就如同操作同步流程。同时 `async` 函数自带执行器，执行的时候无需手动加载。

## 语法

`async`函数返回一个`Promise`对象。

`async`函数内部`return`返回的值，会成为`then`方法回调函数的参数。 

```js
async function  f() {
    return 'hello world'
};
f().then( (v) => console.log(v)) // hello world
```

如果 `async` 函数内部抛出异常，则会导致返回的 Promise 对象状态变为 `reject` 状态。抛出的错误而会被 `catch` 方法回调函数接收到。

```js
async function e(){
    throw new Error('error');
}
e().then(v => console.log(v))
.catch( e => console.log(e));
```

`async`函数返回的`Promise`对象，必须等到内部所有的`await`命令的`Promise`对象执行完，才会发生状态改变。

也就是说，只有当 `async` 函数内部的异步操作都执行完，才会执行 `then` 方法的回调。

```js
const delay = timeout => new Promise(resolve=> setTimeout(resolve, timeout));
async function f(){
    await delay(1000);
    await delay(2000);
    await delay(3000);
    return 'done';
}

f().then(v => console.log(v)); // 等待6s后才输出 'done'
```

正常情况下，`await`命令后面跟着的是`Promise`，如果不是的话，也会被转换成一个立即`resolve`的`Promise`。

```js
async function  f() {
    return await 1
};
f().then( (v) => console.log(v)) // 1
```

如果返回的是 reject 的状态，则会被 `catch` 方法捕获。

## `Async`函数的错误处理

```js
let a;
async function f() {
    await Promise.reject('error');
    a = await 1; // 这段 await 并没有执行
}
f().then(v => console.log(a));
```

如上所示，当 `async` 函数中只要一个 `await` 出现 reject 状态，则后面的 `await` 都不会被执行。
解决办法：添加 `try/catch`。

```js
// 正确的写法
let a;
async function correct() {
    try {
        await Promise.reject('error')
    } catch (error) {
        console.log(error);
    }
    a = await 1;
    return a;
}

correct().then(v => console.log(a)); // 1
```

## 在项目中使用

依然是通过 `babel` 来使用。
只需要设置 `presets` 为 `stage-3` 即可。
安装依赖：

```shell
npm install babel-preset-es2015 babel-preset-stage-3 babel-runtime babel-plugin-transform-runtime
```

修改`.babelrc`：

```js
"presets": ["es2015", "stage-3"],
"plugins": ["transform-runtime"]
```

## 使用规则

- 凡是在前面添加了`async`的函数在执行后都会自动返回一个Promise对象 

```js
async function test() {
    
}

let result = test()
console.log(result)  //即便代码里test函数什么都没返回，我们依然打出了Promise对象
```

- `await`必须在`async`函数里使用，不能单独使用

```js
// 错误
function test() {
    let result = await Promise.resolve('success')
    console.log(result)
}

test()   //执行以后会报错
```

```js
// 正确
async test() {
    let result = await Promise.resolve('success')
    console.log(result)
}
test()
```

- `await`后面需要跟`Promise`对象，不然就没有意义，而且`await`后面的`Promise`对象不必写`then`，因为`await`的作用之一就是获取后面`Promise`对象成功状态传递出来的参数 

```js
function fn() {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve('success')
        })
    })
}

async test() {
    let result = await fn() //因为fn会返回一个Promise对象
    console.log(result)    //这里会打出Promise成功后传递过来的'success'
}

test()
```

## 错误处理方式

通过给包裹`await`的`async`函数添加`then`/`catch`方法来解决。

```js
let promiseDemo = new Promise((resolve, reject) => {
    setTimeout(() => {
        let random = Math.random()
        if (random >= 0.5) {
            resolve('success')
        } else {
            reject('failed')
        }   
    }, 1000)
})

async function test() {
    let result = await promiseDemo
    return result  //这里的result是promiseDemo成功状态的值，如果失败了，代码就直接跳到下面的catch了
}

test().then(response => {
    console.log(response) 
}).catch(error => {
    console.log(error)
})
```

上面代码需要注意两个地方：一是`async`函数需要主动`return`一下，如果`Promise`的状态是成功的，那么return的这个值就会被下面的then方法捕捉到；二是，如果`async`函数有任何错误，都被catch捕捉到！

## 同步与异步

在`async`函数中使用`await`，那么`await`这里的代码就会变成同步的了，意思就是说只有等await后面的Promise执行完成得到结果才会继续下去，await就是等待，这样虽然避免了异步，但是它也会阻塞代码，所以使用的时候要考虑周全。

```js
function fn(name) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve(`${name}成功`)
        }, 1000)
    })
}

async function test() {
    let p1 = await fn('小红')
    let p2 = await fn('小明')
    let p3 = await fn('小华')
    return [p1, p2, p3]
}

test().then(result => {
    console.log(result)
}).catch(result => {
    console.log(result)
})
```

这样写虽然是可以的，但是这里`await`会阻塞代码，每个`await`都必须等后面的`fn()`执行完成才会执行下一行代码，所以`test`函数执行需要3秒。如果不是遇到特定的场景，最好还是不要这样用。

## 小练习

```js
console.log(1)
let promiseDemo = new Promise((resolve, reject) => {
    console.log(2)
    setTimeout(() => {
        let random = Math.random()
        if (random >= 0.2) {
            resolve('success')
            console.log(3)
        } else {
            reject('failed')
            console.log(3)
        }   
    }, 1000)
})

async function test() {
    console.log(4)
    let result = await promiseDemo
    return result
}

test().then(result => {
    console.log(5)
}).catch((result) => {
    console.log(5)
})

console.log(6)
```

打出的数字顺序是：1 2 4 6 3 5

## 适合使用`async/await`的业务场景

当我们需要发送多个请求，而**后面请求的发送总是需要依赖上一个请求返回的数据**。对于这个问题，我们既可以用的Promise的链式调用来解决，也可以用async/await来解决，然而后者会更简洁些。

使用Promise链式调用来处理：

```js
function request(time) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve(time)
        }, time)
    })
}

request(500).then(result => {
    return request(result + 1000)
}).then(result => {
    return request(result + 1000)
}).then(result => {
    console.log(result)
}).catch(error => {
    console.log(error)
}) 
```

使用`async/await`的来处理：

```js
function request(time) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve(time)
        }, time)
    })
}

async function getResult() {
    let p1 = await request(500)
    let p2 = await request(p1 + 1000)
    let p3 = await request(p2 + 1000)
    return p3
}

getResult().then(result => {
    console.log(result)
}).catch(error => {
    console.log(error)
})
```

## 在循环中使用`await`

==注意==：必须在`async`函数中使用。

在`for...of`中使用`await`：

```js
let request = (time) => {
    return new Promise((resolve) => {
        setTimeout(() => {
            resolve(time)
        }, time)
    })
}

let times = [1000, 500, 2000]
async function test() {
    let result = []
    for (let item of times) {
        let temp = await request(item)
        result.push(temp)
    }
    return result
}

test().then(result => {
    console.log(result)
}).catch(error => {
    console.log(error)
})
```

一个函数如果加上 `async` ，那么该函数就会返回一个 `Promise`。

```js
async function test() {
  return "1";
}
console.log(test()); // -> Promise {<resolved>: "1"}
```

可以把 `async` 看成将函数返回值使用 `Promise.resolve()` 包裹了下。

`await` 只能在 `async` 函数中使用。

```js
function sleep() {
  return new Promise(resolve => {
    setTimeout(() => {
      console.log('finish')
      resolve("sleep");
    }, 2000);
  });
}
async function test() {
  let value = await sleep();
  console.log("object");
}
test()
```

上面代码会先打印 `finish` 然后再打印 `object` 。因为 `await` 会等待 `sleep` 函数 `resolve` ，所以即使后面是同步代码，也不会先去执行同步代码再来执行异步代码。

`async`和`await` 相比直接使用 `Promise` 来说，优势在于处理 `then` 的调用链，能够更清晰准确的写出代码。缺点在于滥用 `await` 可能会导致性能问题，因为 `await` 会阻塞代码，也许之后的异步代码并不依赖于前者，但仍然需要等待前者完成，导致代码失去了并发性。

下面来看一个使用 `await` 的代码。

```js
var a = 0
var b = async () => {
  a = a + await 10
  console.log('2', a) // -> '2' 10
  a = (await 10) + a
  console.log('3', a) // -> '3' 20
}
b()
a++
console.log('1', a) // -> '1' 1
```

对于以上代码你可能会有疑惑，这里说明下原理

- 首先函数 `b` 先执行，在执行到 `await 10` 之前变量 `a` 还是 0，因为在 `await` 内部实现了 `generators` ，`generators` 会保留堆栈中东西，所以这时候 `a = 0` 被保存了下来
- 因为 `await` 是异步操作，遇到`await`就会立即返回一个`pending`状态的`Promise`对象，暂时返回执行代码的控制权，使得函数外的代码得以继续执行，所以会先执行 `console.log('1', a)`
- 这时候同步代码执行完毕，开始执行异步代码，将保存下来的值拿出来使用，这时候 `a = 10`
- 然后后面就是常规执行代码了



# `Proxy`

用途：运算符重载、对象模拟、简洁而灵活的`API`创建、对象变化事件、`Vue3`内部响应系统提供动力。

`Proxy`用于修改某些操作的默认行为。也可理解为在目标对象之前架设一层拦截，外部所有的访问都必须先通过这层拦截，因此提供了一种机制，可对外部的访问进行过滤和修改。

`ES6`提供了`Proxy`构造函数，用来生成`Proxy`实例。

```js
var proxy = new Proxy(target, handler);
// 其中`new Proxy`用来生成`Proxy`实例,`target`是表示所要拦截的对象,`handle`是用来定制拦截行为的对象。
```

下面是`Proxy`的一个例子，是一个有陷阱的代理，一个`get`陷阱，总是返回`42`：

```js
let target = {
  x: 10,
  y: 20
}

let hanler = {
  get: (obj, prop) => 42
}

target = new Proxy(target, hanler)

target.x //42
target.y //42
target.x // 42
```

对象将任何属性的访问操作都返回42，包括`target.x`，`target['x']`，`Reflect.get(target, 'x')`等。

## 用例

`JavaScript`中未设置属性的默认值是`undefined`，但`Proxy`可以改变这种情况。

```js
const withZeroValue = (target, zeroValue) => new Proxy(target, {
  get: (obj, prop) => (prop in obj) ? obj[prop] : zeroValue
})
```

函数`withZeroValue`用来包装目标对象，如果设置了属性，则返回属性值。否则，返回一个默认的零值。

## 基本语法

```js
let p = new Proxy(target, handler);
// target是你要代理的对象,可以是JavaScript中的任何合法对象,如: (数组, 对象, 函数等)
// handler自定义操作方法的一个集合
// p是一个被代理后的新对象,它拥有target的一切属性和方法.只不过其行为和结果是在handler中自定义的.
```

```js
let obj = {
  a: 1,
  b: 2,
}

const p = new Proxy(obj, {
  get(target, key, value) {
    if (key === 'c') {
      return '我是自定义的一个结果';
    } else {
      return target[key];
    }
  },

  set(target, key, value) {
    if (value === 4) {
      target[key] = '我是自定义的一个结果';
    } else {
      target[key] = value;
    }
  }
})

console.log(obj.a) // 1
console.log(obj.c) // undefined
console.log(p.a) // 1
console.log(p.c) // 我是自定义的一个结果

obj.name = '李白';
console.log(obj.name); // 李白
obj.age = 4;
console.log(obj.age); // 4

p.name = '李白';
console.log(p.name); // 李白
p.age = 4;
console.log(p.age); // 我是自定义的一个结果
```

## `Proxy`所能代理的范围--`handler`

构造一个代理对象时所传的第二个参数`handler`对象是由`get`和`set`两个函数方法组成的。

这两个方法会在一个对象被`get`和`set`时被调用执行,以代替原生对象上的操作。

为什么在`handler`中定义`get`和`set`两个函数名之后就代理对象上的`get`和`set`操作了呢?

实际上`handler`本身就是`ES6`所新设计的一个对象，用来自定义代理对象的各种可代理操作。它本身共有13种方法，每种方法都可以代理一种操作：

```js
handler.getPrototypeOf()

// 在读取代理对象的原型时触发该操作，比如在执行 Object.getPrototypeOf(proxy) 时。

handler.setPrototypeOf()

// 在设置代理对象的原型时触发该操作，比如在执行 Object.setPrototypeOf(proxy, null) 时。

handler.isExtensible()

// 在判断一个代理对象是否是可扩展时触发该操作，比如在执行 Object.isExtensible(proxy) 时。

handler.preventExtensions()

// 在让一个代理对象不可扩展时触发该操作，比如在执行 Object.preventExtensions(proxy) 时。

handler.getOwnPropertyDescriptor()

// 在获取代理对象某个属性的属性描述时触发该操作，比如在执行 Object.getOwnPropertyDescriptor(proxy, "foo") 时。

handler.defineProperty()

// 在定义代理对象某个属性时的属性描述时触发该操作，比如在执行 Object.defineProperty(proxy, "foo", {}) 时。

handler.has()

// 在判断代理对象是否拥有某个属性时触发该操作，比如在执行 "foo" in proxy 时。

handler.get()

// 在读取代理对象的某个属性时触发该操作，比如在执行 proxy.foo 时。

handler.set()

// 在给代理对象的某个属性赋值时触发该操作，比如在执行 proxy.foo = 1 时。

handler.deleteProperty()

// 在删除代理对象的某个属性时触发该操作，比如在执行 delete proxy.foo 时。

handler.ownKeys()

// 在获取代理对象的所有属性键时触发该操作，比如在执行 Object.getOwnPropertyNames(proxy) 时。

handler.apply()

// 在调用一个目标对象为函数的代理对象时触发该操作，比如在执行 proxy() 时。

handler.construct()

// 在给一个目标对象为构造函数的代理对象构造实例时触发该操作，比如在执行new proxy() 时。
```

## 作用

代理模式`Proxy`的作用主要体现在三个方面：

- 拦截和监视外部对对象的访问

- 降低函数或类的复杂度

- 在复杂操作前对操作进行校验或对所需资源进行管理

[ES6-Proxy使用场景](https://www.w3cplus.com/javascript/use-cases-for-es6-proxies.html)

## `definePropety`

`ES5`提供了`Object.defineProperty`方法，该方法可在一个对象上定义一个新属性，或者修改一个对象的现有属性，并返回这个对象。

```js
Object.defineProperty(obj, prop, descriptor)
// obj: 要在其上定义属性的对象。
// prop: 要定义或修改的属性的名称。
// descriptor: 将被定义或修改的属性的描述符。
```

```js
var obj = {};
Object.defineProperty(obj, "num", {
    value : 1,
    writable : true, // 能否被赋值运算符所改变
    enumerable : true, // 是否可枚举
    configurable : true // 属性描述符能否被改变
});
// 对象obj拥有属性num,值为1
```

```js
var value = 1;
Object.defineProperty({}, "num", {
    get : function(){
      return value;
    },
    set : function(newValue){
      value = newValue;
    },
    enumerable : true,
    configurable : true
});
```

### 存取器属性

讲到`defineProperty`，是因为我们要使用存取描述符中的`get`和`set`，这两个方法又被称为`getter`和`setter`。

由`getter`和`setter`定义的属性称做存取器属性。

当程序查询存取器属性的值时，`JavaScript`调用`getter`方法，这个方法的返回值就是属性存取表达式的值。

当程序设置一个存取器属性的值时，`JavaScript`调用`setter`方法，将赋值表达式右侧的值当做参数传入`setter`。从某种意义上讲，这个方法负责设置属性值。可忽略`setter`方法的返回值。

```js
var obj = {}, value = null;
Object.defineProperty(obj, "num", {
    get: function(){
        console.log('执行了 get 操作')
        return value;
    },
    set: function(newValue) {
        console.log('执行了 set 操作')
        value = newValue;
    }
})

obj.num = 1 // 执行了 set 操作

console.log(obj.num); // 执行了 get 操作 // 1
```

这不就是我们要的监控数据改变的方法吗？我们再来封装一下：

```js
function Archiver() {
    var value = null;
    // archive n. 档案
    var archive = [];

    Object.defineProperty(this, 'num', {
        get: function() {
            console.log('执行了 get 操作')
            return value;
        },
        set: function(value) {
            console.log('执行了 set 操作')
            value = value;
            archive.push({ val: value });
        }
    });
    this.getArchive = function() { return archive; };
}

var arc = new Archiver();
arc.num; // 执行了 get 操作
arc.num = 11; // 执行了 set 操作
arc.num = 13; // 执行了 set 操作
console.log(arc.getArchive()); // [{ val: 11 }, { val: 13 }]
```

## `watch API`

需求：点击按钮，`span`标签里的值加1。

```html
<span id="container">1</span>
<button id="button">点击加 1</button>
```

```js
// 传统做法
document.getElementById('button').addEventListener("click", function(){
    var container = document.getElementById("container");
    container.innerHTML = Number(container.innerHTML) + 1;
});
```

```js
// 使用了defineProperty
var obj = {
    value: 1
}

// 储存 obj.value 的值
var value = 1;

Object.defineProperty(obj, "value", {
    get: function() {
        return value;
    },
    set: function(newValue) {
        value = newValue;
        document.getElementById('container').innerHTML = newValue;
    }
});

document.getElementById('button').addEventListener("click", function() {
    obj.value += 1;
});
```

使用`watch`函数简化上述代码：

```js
var obj = {
    value: 1
}

watch(obj, "value", function(newvalue){
    document.getElementById('container').innerHTML = newvalue;
})

document.getElementById('button').addEventListener("click", function(){
    obj.value += 1
});
```

```js
(function(){
    var root = this;
    function watch(obj, name, func){
        var value = obj[name];

        Object.defineProperty(obj, name, {
            get: function() {
                return value;
            },
            set: function(newValue) {
                value = newValue;
                func(value)
            }
        });

        if (value) obj[name] = value
    }

    this.watch = watch;
})()
```

现在我们可以监控对象属性值的改变，并且可以根据属性值的改变，添加回调函数。

## `Proxy`

```js
var proxy = new Proxy({}, {
    get: function(obj, prop) {
        console.log('设置 get 操作')
        return obj[prop];
    },
    set: function(obj, prop, value) {
        console.log('设置 set 操作')
        obj[prop] = value;
    }
});

proxy.time = 35; // 设置 set 操作

console.log(proxy.time); // 设置 get 操作 // 35
```

```js
// 使用has方法隐藏某些属性,不被in运算符发现
var handler = {
  has (target, key) {
    if (key[0] === '_') {
      return false;
    }
    return key in target;
  }
};
var target = { _prop: 'foo', prop: 'foo' };
var proxy = new Proxy(target, handler);
console.log('_prop' in proxy); // false
```

```js
let p = new Proxy(target, handler);
// `target` 代表需要添加代理的对象
// `handler` 用来自定义对象中的操作
```

使用`Proxy`实现数据的绑定和监听

```js
let onWatch = (obj, setBind, getLogger) => {
  let handler = {
    get(target, property, receiver) {
      getLogger(target, property)
      return Reflect.get(target, property, receiver);
    },
    set(target, property, value, receiver) {
      setBind(value);
      return Reflect.set(target, property, value);
    }
  };
  return new Proxy(obj, handler);
};

let obj = { a: 1 }
let value
let p = onWatch(obj, (v) => {
  value = v
}, (target, property) => {
  console.log(`Get '${property}' = ${target[property]}`);
})
p.a = 2 // bind `value` to `2`
p.a // -> Get 'a' = 2
```



# 0.1 + 0.2 != 0.3

因为`JS`采用了`IEEE 754`双精度版本（64位），并且只要采用`IEEE 754`的语言都有该问题。

计算机表示十进制是采用二进制表示的，因为`0.1`和`0.2`都是无限循环的二进制，所以在小数位末尾处需要判断是否进位（就和十进制的四舍五入一样）。最后把这两个二进制加起来会得出的数转为十进制就是`0.30000000000000004`。

原生解决办法：

```js
parseFloat((0.1 + 0.2).toFixed(10))
```



# 正则表达式

概述：为了字符串模式匹配，从而实现搜索和替换功能。

正则表达式的基本组成元素可分为：字符和元字符。

- 字符就是基础的计算机字符编码，通常正则表达式里面使用的就是数字、英文字母。

- 元字符（特殊字符）是一些用来表示特殊语义的字符。利用这些元字符，才能构造出强大的表达式模式。

## 单个字符

一一对应的关系。

`'apple'`这个单词里找到`‘a'`这个字符，使用`/a/`正则。

`/\*/`使用转义字符`\`，让`*`失去其本来的意义。

![正则](D:\notes\前端进阶之道\img\正则.jpg)

## 多个字符

如`/[123]/`这个正则就能同时匹配`1,2,3`三个字符。

使用元字符`-`就可以用来表示区间范围，利用`/[0-9]/`就能匹配所有的数字, `/[a-z]/`则可以匹配所有的英文小写字母。

在正则表达式里衍生了一批用来同时匹配多个字符的简便正则表达式：

![正则2](D:\notes\前端进阶之道\img\正则2.jpg)

## 循环与重复

同时匹配多个字符，只要多次循环，重复使用之前的正则规则就可以了。

根据循环次数的多与少，可分为0次，1次，多次，特定次。

![循环与重复](D:\notes\前端进阶之道\img\循环与重复.jpg)

## 特定次数

匹配特定的重复次数，元字符`{`和`}`用来给重复匹配设置精确的区间范围。

如`'a'`匹配3次，使用`/a{3}/`正则；`'a'`匹配至少两次，用`/a{2,}/`这个正则。

```
- {x}: x次

- {min, max}: 介于min次到max次之间

- {min, }: 至少min次

- {0, max}: 至多max次
```

![特定次数2](D:\notes\前端进阶之道\img\特定次数2.jpg)

## 位置边界

### 单词边界

```
The cat scattered his food all over the room.
```

找`cat`这个单词，如果只是使用`/cat/`这个正则，就会同时匹配到`cat`和`scattered`这两处文本，这时需要使用边界正则表达式`\b`。

上面的例子改写成`/\bcat\b/`这样就能匹配到`cat`这个单词了。

### 字符串边界

- 元字符`^`用来匹配字符串的开头。

- 元字符`$`用来匹配字符串的末尾。

注意在长文本里，如果要排除换行符的干扰，我们要使用多行模式，试着匹配`I am scq000`这个句子：

```
I am scq000.
I am scq000.
I am scq000.
```

使用`/^I am scq000\.$/m`正则表达式。

正则里面的模式除了`m`外比较常用的还有`i`和`g`。前者是忽略大小写，后者是找到所有符合的匹配。

总结：

![边界](D:\notes\前端进阶之道\img\边界.jpg)

## 子表达式

### 分组

分组体现在：所有以`(`和`)`元字符所包含的正则表达式被分为一组，每一个分组都是一个子表达式，它也是构成高级正则表达式的基础。

如果只是使用简单的`(regex)`匹配语法本质上和不分组是一样的，如果要发挥它强大的作用，往往要结合回溯引用的方式。

### 回溯引用

回溯引用指的是模式的后面部分引用前面已经匹配到的子字符串。

可以把它想象成是变量，回溯引用的语法像`\1`,`\2`,....，其中`\1`表示引用的第一个子表达式，`\2`表示引用的第二个子表达式，以此类推。而`\0`则表示整个表达式。

```
// 匹配两个连续相同的单词
Hello what what is the first thing, and I am am scq000.
`\b(\w+)\s\1`正则
```

回溯引用在替换字符串中十分常用，用`$1`,`$2`...来引用要被替换的字符串：

```js
var str = 'abc abc 123';
str.replace(/(ab)c/g,'$1g');
// 得到结果 'abg abg 123'
```

```js
// 如果不想子表达式被引用,可以使用非捕获正则(?:regex)这样就可以避免浪费内存。
var str = 'scq000'.
str.replace(/(scq00)(?:0)/, '$1,$2')
// 返回scq00,$2
// 由于使用了非捕获正则，所以第二个引用没有值，这里直接替换为$2
```

当需要限制回溯引用的适用范围，可通过前向查找和后向查找达到目的。

#### 前向查找

前向查找是用来限制后缀的。

凡是以`(?=regex)`包含的子表达式在匹配过程中都会用来限制前面的表达式的匹配。

例如`happy happily`这两个单词，我想获得以`happ`开头的副词，那么就可以使用`happ(?=ily)`来匹配。

如果我想过滤所有以`happ`开头的副词，那么也可以采用负前向查找的正则`happ(?!ily)`，就会匹配到`happy`单词的`happ`前缀。

#### 后向查找

前向查找的反向操作：后向查找。

后向查找是通过指定一个子表达式，然后从符合这个子表达式的位置出发开始查找符合规则的字串。

例子：`apple`和`people`都包含`ple`这个后缀，如果我只想找到`apple`的`ple`，可通过限制`app`这个前缀，就能唯一确定`ple`这个单词了。

```
/(?<=app)ple/
```

其中`(?<=regex)`的语法就是后向查找。

`regex`指代子表达式会作为限制项进行匹配，匹配到这个子表达式后，就会继续向后查找。

另外一种限制匹配是利用`(?<!regex)`语法，这里称为负后向查找。与正前向查找不同的是，被指定的子表达式不能被匹配到，上面的例子中，如果想要查找`apple`的`ple`也可以这么写成`/(?<!peo)ple`。

注意：不是每种正则实现都支持后向查找，在`javascript`中是不支持的。

所以如果有用到后向查找的情况，思路是将字符串进行翻转，然后再使用前向查找，作完处理后再翻转回来。

例子：

```js
// 替换apple的ple为ply
var str = 'apple people';
str.split('').reverse().join('').replace(/elp(?=pa)/, 'ylp').split('').reverse().join('');
```


![后向查找](D:\notes\前端进阶之道\img\后向查找.jpg)

### 逻辑处理

三种逻辑关系，与或非。

在正则里，默认的正则规则都是与的关系，所以这里不讨论。

而非关系，分为两种情况：

- 字符匹配
- 子表达式匹配

在字符匹配的时候，需要使用`^`这个元字符。

只在`[`和`]`内部使用的`^`才表示非的关系，子表达式匹配的非关系就要用到前面介绍的前向负查找子表达式`(?!regex)`或后向负查找子表达式`(?<!regex)`。

或关系，通常给子表达式进行归类使用。如：同时匹配a,b两种情况就可以使用`(a|b)`子表达式。

![逻辑处理](D:\notes\前端进阶之道\img\逻辑处理.jpg)

面试题：正则处理数字千分位，将`12345`替换成`12,345`。

```js
"123456".replace(/(?!^)(?=(\d{3})+$)/g, ",");
```

## 图解

### 元字符

![正则表达式](D:\notes\前端进阶之道\img\正则表达式.jpg)

### 修饰语

![修饰语](D:\notes\前端进阶之道\img\修饰语.jpg)

### 字符简写

![字符简写](D:\notes\前端进阶之道\img\字符简写.jpg)

[深入学习正则](https://juejin.im/post/5965943ff265da6c30653879)



# `V8`下的垃圾回收机制

## `NodeJS`执行环境

在`chrome`浏览器中，`JavaScript`是在`V8`上解释执行的。

`NodeJS`是基于`JavaScript`语言和环境所编写的，它的执行环境也离不开`V8`。

`Chrome`浏览器和`NodeJS`执行环境的对比：

![浏览器和Node.js执行环境的对比](D:\notes\前端进阶之道\img\浏览器和Node.js执行环境的对比.jpg)

- 在浏览器中，因为要渲染`UI`界面（HTML，CSS），所以多了浏览器的`WebKit`（内核）以及底层也会调用显卡来显示渲染界面。
- `NodeJS`使用场景多是服务器端，没有`UI`界面需要渲染，也就不需要`WebKit`内核。底层相应的也不会调用显卡驱动。
- `NodeJS`的中间层`libuv`是其支持跨平台运行的关键所在。
- 在`Chrome`浏览器和`NodeJS`中，`JavaScript`的执行环境都是`V8`虚拟机。

## `V8`的内存限制

在其它后端语言中，如`Java`在使用内存时是没有限制的（可用充分使用物理内存）。

但在`JavaScript`中，内存的使用受到了`V8`虚拟机的限制。

因为`V8`是为`Chrome`浏览器研发生产的，在浏览器环境内并不存在使用大量内存的情况，所以`V8`在内存使用上对不同的平台做了相对应的限制：

- 32位操作系统约为`0.7GB`
- 64位操作系统约为`1.4GB`

当我们操作大文件（超过该内存限制的文件）时，无法将文件全部读取到内存中来，相对应的计算机的内存资源也就无法得到充分的使用。因此，对于大内存的操作需谨慎否则超出`V8`内存限制将会造成进程退出。

## `V8`对象分配（堆内存）

了解`JVM`的人都知道，`Java`中的对象是分配在堆内存中的。

`V8`也不例外，当我们在代码中声明变量并赋值时，所使用对象的内存就分配在堆中。如果已申请的堆空闲内存不够分配新对象时，将继续申请堆内存，直到堆的大小超过`V8`限制为止。

查看堆内存使用情况：

```shell
$ node
> process.memoryUsage();
{ rss: 20774912,
  heapTotal: 7258112,
  heapUsed: 4225360,
  external: 9327 }
```

- `rss`是驻留集大小，是给这个进程分配了多少物理内存（占总分配内存的一部分），这些物理内存中包含堆、代码段、以及栈。

- `heapTotal`堆中总共申请到的内存量。
- `heapUsed`堆中目前用到的内存量，判断内存泄漏我们主要以这个字段为准。 

- `external`引擎内部`C++`对象占用的内存。

以上内存限制在`Node`中也是可被打破的，即在启动项目时指定堆内存大小，如下所示：

```shell
node --max-old-space-size=2048 index.js // 设置老龄代堆内存大小,单位MB
// 或者
node --max-new-space-size=1024 index.js // 设置新生代堆内存大小,单位MB
```

以上参数在`V8`初始化时生效，一旦生效就不能再动态修改。

## `V8`的垃圾回收机制

概述：垃圾回收是指回收那些在应用程序中不再引用的对象。当一个对象无法从根节点访问时，就会做为垃圾回收的候选对象。这里的根对象可以为全局对象、局部变量，无法从根节点访问指的也就是不会在被任何其它活动对象所引用。

`V8`实现了准确式`GC`，垃圾回收策略主要基于分代垃圾回收机制。

在分代回收的基础上，针对**新生代**和**老龄代**在其内部又做了相对应的回收算法：

- 新生代内使用的是复制算法
- 老龄代内使用的是（标记－清除算法）以及（标记－合并算法）

### 内存堆内存分配－分代算法

![内存堆内存分配－分代算法](D:\notes\前端进阶之道\img\内存堆内存分配－分代算法.jpg)

![v8引擎的垃圾回收算法](D:\notes\前端进阶之道\img\v8引擎的垃圾回收算法.jpg)

`V8`的堆内存分代算法：将整个堆内存分为新生代和老龄代，新生代内保存的是最新创建且存活周期短的对象。老龄代内保存的是较为活跃且存活周期长的对象。

### 复制算法（新生代内存）

新生代中的对象一般存活时间较短，使用`Scavenge GC`算法。

该算法将内存一分为二，每一部分空间称为`semispace`。

在这两个`semispace`空间中，只有一个处于使用中，另一个处于闲置状态。

正在使用的`semispace`空间我们称之为`From`空间，处于闲置的`semispace`空间我们称之为`To`空间。

当我们分配对象时，先是在`From`空间进行分配。当`From`空间被占满时，新生代`GC`就会启动了。算法会检查`From`空间中的存活对象，这些存活对象将被复制到`To`空间中，而非存活对象占用的空间将被释放。完成复制后，`From`空间和`To`空间的角色发生互换，`From`空间变为`To`空间，`To`空间变为`From`空间。

简而言之，在新生代的垃圾回收过程中，就是通过将存活对象在两个`semispace`空间之间进行复制。如下图所示：

![新生代内存空间](D:\notes\前端进阶之道\img\新生代内存空间.jpg)

![复制算法](D:\notes\前端进阶之道\img\复制算法.jpg)

![Scavenge算法](D:\notes\前端进阶之道\img\Scavenge算法.jpg)

复制算法的缺点是：只能使用新生代内存中一半内存空间大小。

但复制算法由于只复制存活的对象，并且对于生命周期短的场景存活对象只占少部分，所以它在时间效率上有优异的表现。

由于复制算法是典型的牺牲空间换取时间的算法，所以无法大规模应用在所有的垃圾回收中。但可以发现，复制算法非常适合应用在新生代中，因为新生代中对象的生命周期较短，恰恰适合这个算法。

上图也可以看出，实际在`V8`中处于使用的内存是新生代一半的内存加上老龄代内存，有一半的新生代内存处于闲置状态。

我们了解了新生代中的内存空间使用，那什么样的对象会从新生代中晋升到老龄代内存中呢？

- 当一个对象经过多次复制依然存活时，它将被认为是生命周期较长的对象。这种较长生命周期的对象随后会被移动到老龄代内存中，采用老龄代中新的算法进行管理。

- 对象从新生代晋升到老龄代的条件主要有两个：一个是对象是否经历过复制算法回收，一个是`To`空间的内存占用比超过一定的限制25%。

### 标记－清除算法（老龄代内存）

老龄代中的对象一般存活时间较长且数量也多，使用了两个算法，分别是标记清除算法和标记合并算法。

以下情况会先启动标记清除算法：

- 某一个空间没有分块 
- 空间中被对象超过一定限制
- 空间不能保证新生代中的对象移动到老生代中

在这个阶段中，会遍历堆中所有的对象，然后标记活的对象，在标记完成后，销毁所有没有被标记的对象。

标记清除算法分为两个阶段，一阶段是标记，另一阶段是清除。

与复制算法相比，标记－清除算法并不会将内存空间划分为两半，所以不存在浪费一半空间的行为。

与复制算法不同的是，标记－清除算法在标记阶段遍历堆中的所有对象，并标记活着的对象，在随后的清除阶段中，只清除没有被标记的对象。

可以看出复制算法只复制活着的对象，而标记－清除算法只清理死亡的对象。这是因为活对象在新生代中占较小部分，死对象在老龄代中占较小部分，这也是两种回收方式能高效工作的原因。

标记－清除算法的工作图解如下：

![标记-清除算法](D:\notes\前端进阶之道\img\标记-清除算法.jpg)

![标记清除](D:\notes\前端进阶之道\img\标记清除.jpg)

![Mark-Sweep算法](D:\notes\前端进阶之道\img\Mark-Sweep算法.jpg)

标记－清除算法最大的问题是在进行一次标记清除回收后，内存空间会出现不连续的状态。这些内存碎片会对后续的内存分配造成问题，因为很可能出现需要分配一个大内存对象的情况，这时无法分配的情况下就会再次触发垃圾回收，而这次的回收是不必要的，这时就该下面的回收算法登场了：标记－合并算法。

清除对象后会造成堆内存出现碎片的情况，当碎片超过一定限制后会启动合并算法。在压缩过程中，将活的对象向一端移动，直到所有对象都移动完成然后清理掉不需要的内存。

### 标记－合并算法（老龄代内存）

上面也说了标记－清除算法的弊端，标记－合并算法就是为了解决这些弊端设计演变出来的。

它们的差别在于对象在标记为死亡后，在整理的过程中，将活着的对象往一端移动，在移动完成后，直接清理掉另一端死亡的对象。完成移动并清理完另一端死亡对象的内存后，老龄代内存空间就是连续的未使用和以使用了，这样就可以进行大内存对象的分配了。

![标记合并](D:\notes\前端进阶之道\img\标记合并.jpg)

### 垃圾回收算法比较

![垃圾回收算法的比较](D:\notes\前端进阶之道\img\垃圾回收算法的比较.jpg)

### 增量标记

为了避免出现`JavaScript`应用逻辑与垃圾回收器看到的情况不一致，垃圾回收时应用逻辑会停下来，这种行为被成为全停顿，对老生代影响较大。
增量标记就是拆分为许多小的步进，每次做完一步进，就让`JavaScript`执行一会，垃圾回收与应用逻辑交替执行。
 采用增量标记后，`GC`的最大停顿时间减少到原来的1 / 6左右。

# 内存泄漏

概述：内存泄漏是指程序中已动态分配的堆内存由于某种原因程序未释放或无法释放，造成系统内存的浪费，导致程序运行速度减慢甚至系统崩溃等严重后果。

## 全局变量

未声明的变量或挂在全局`global`下的变量不会自动回收，将会常驻内存直到进程退出才会被释放，除非通过`delete`或重新赋值为`undefined/null`解决之间的引用关系，才会被回收。 

## 闭包

概念：同一个作用域生成的闭包对象是被该作用域中所有下一级作用域共同持有的。 

闭包会引用父级函数中的变量，如果闭包得不到释放，闭包引用的父级变量也不会释放从而导致内存泄漏。

## 将内存做为缓存

缓存中存储的键越多，长期存活的对象也就越多，垃圾回收时将会对这些对象做无用功。

## 模块私有变量内存永驻

在加载一个模块代码之前，`Node.js`会使用一个如下的函数封装器将其封装，保证了顶层的变量（var、const、let）在模块范围内，而不是全局对象。

这个时候就会形成一个闭包，在`require`时会被加载一次，将`exports`对象保存于内存中，直到进程退出才会回收，这个将会导致的是内存常驻，所以避免一些没必要的模块加载，否则也会造成内存增加。

```js
(function(exports, require, module, __filename, __dirname) {
    // 模块的代码实际上在这里
});
```

```js
const a = require('a.js') // 推荐

function test() { 
    a.run()
}
```

```js
function test(){ // 不推荐
  require('a.js').run()
}
```

## 事件重复监听

## `setInterval`定时器

在使用定时器`setInterval`时记的使用对应的`clearInterval`进行清除，因为`setInterval`执行完之后会返回一个值且不会自动释放。

## 数组的操作

`map`、`filter`等对数组进行操作，每次操作之后都会创建一个新的数组，会占用内存。

如果单纯的遍历例如`map`可以使用`forEach`代替等。