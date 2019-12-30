# 循环遍历数组

## `for...in`

`ES5`标准，用于遍历对象的属性名。

当其遍历数组的时候，得到的是对应项的索引。

```js
const obj = {
    name: 'sxw',
    age: 18
}
for(let prop in obj) {
    console.log(prop) // name age
    console.log(obj[prop]) // 'sxw' 18
}
```

```js
let arr = ['青莲地心火','三千炎焱火','虚无吞炎']
for(let prop in arr) {
    console.log(typeof(prop)) // String
    console.log(prop) // 0 1 2
    console.log(arr[prop]) // '青莲地心火' '三千炎焱火' '虚无吞炎'
}
```

==注意==：

- `prop`为字符串类型数字，不能直接进行几何运算。
- 遍历顺序有可能不是按照实际数组内部顺序进行的。
- 会遍历出数组所有的可枚举属性，包括原型

```js
Array.prototype.func = function() {
    console.log('在原型上定义方式...')
}
let arr = [1,2,3]
for(let prop in arr) {
    console.log(prop) // 0 1 2 func
}
```

## `for...of`

`ES6`标准，提供了统一的遍历机制。

只要是实现了`[Symbol.iterator]`接口的对象都可以用其进行遍历：

- 数组
- `set`结构
- `map`结构
- 类似数组的对象 `arguments`、`DOM NodeList`、字符串
- `Generator`对象
- 字符串 

```js
let arr = ['青莲地心火','三千炎焱火','虚无吞炎']
for(item of arr) {
    console.log(item) // '青莲地心火' '三千炎焱火' '虚无吞炎'
}
```

==注意==：没有`for...in`的缺点，且不同于`forEach`，可以与`break`、`continue`、`return`配合使用。

```js
let arrLike = {
    0: 'a',
    1: 'b',
    length: 2
}
// Array.from将伪数组对象或可迭代对象转为数组
for(let item of Array.from(arrLike)) {
    console.log(item) // a b
}
```

```html
<body>
   <p>1</p>
   <p>2</p>
   <p>3</p>
   <script>
      let paras = document.getElementsByTagName('p')
      for(let item of paras) {
         console.log(item.innerText) // 1 2 3
      }
</script>
</body>
```

```js
let str = 'shuxiaowei'
for(item of str) {
    console.log(item)
}
```

```js
// 对于普通对象不可使用for...of进行遍历
// 但可以通过Object.keys()返回一个可枚举属性组成的数组
let obj = {
    name: 'sxw',
    age: 18,
    sex: 'male'
}
for(let key of Object.keys(obj)) {
    console.log(key) // name age sex
    console.log(obj[key]) // sxw 18 male
}
```

## `forEach`

数组自带的方法，无需获取数组长度，对数组中的每一项给定运行函数，没有返回值且无法终止循环。

一循环必然会执行完毕，不能灵活运用于多种情况。

```js
let arr = [1,2,3]
arr.forEach(function(item,index,arr) {
    console.log(item) // 1 2 3
    console.log(index) // 0 1 2 
    console.log(arr) // [1,2,3]
})
```

## 三个高阶函数

### `filter`

### `map`

### `reduce`

```js
// 三个需求
// 1.取出数组中所有小于100的数字
let arr = [10,20,111,222,444,40,50]
let newArr = []
for(let item of arr) {
    if(item < 100) {
        newArr.push(item)
    }
}

// 2.将上述满足条件的结果中的所有数字 * 3
let newArr2 = []
for(let item of newArr) {
    newArr2.push(item * 3)
}

// 3.将上述所得结果数组中的所有数字相加
let total = null
for(let item of newArr2) {
    total += item
}
```

使用高阶函数优化上述需求：

```js
// 优化需求1
// filter筛选遍历数组中的每一项,将每一项传到回调函数中
// filter内部会将回调函数结果为true的item值进行保存为一个新数组并返回
let arr = [10,20,111,222,444,40,50]
let newArr = arr.filter(function(item) {
    return item < 100
})

// 优化需求2
// map仅对数组中的每一项进行操作
let newArr2 = newArr.map(function(item) {
    return item * 3
})

// 优化需求3
// reduce对数组进行汇总处理,接收两个参数(回调函数和设定汇总初始值)
// 回调函数也接收两个参数(初始汇总值,循环数组中的每一项)
let total = newArr2.reduce(function(preValue, item) {
    return preValue + item
}, 0)
```

```js
// 代码简化
let arr = [10,20,111,222,444,40,50]
let total = arr.filter(function(item) {
    return item < 100
}).map(function(item) {
    return item * 3
}).reduce(function(preValue, item) {
    return preValue + item
}, 0)
```

```js
// 最终版
let arr = [10,20,111,222,444,40,50]
let total = arr.filter(item => item < 100).map(item => item * 3).reduce((preValue, item) => preValue + item)
```

## `every`

返回布尔值。

循环遍历数组每一项，将值传到回调函数中，当且仅当所有项都返回`true`时，返回`true`。

```js
let arr = [1,2,3]
let result = arr.every(function(item) {
    return item > 1 // false
})
```

## `some`

与`every`相反。

```js
let arr = [1,2,3]
let result = arr.some(function(item) {
    return item > 1 // true
})
```

## `find`

返回最早符合条件的那一项，若不存在符合条件的项则返回`-1`。

```js
let arr = [1,2,3]
let result = arr.find(function(item) {
    return item > 1 // 2
})
```

## `findIndex`

返回最早符合条件的那一项的索引。

```js
let arr = [1,2,3]
let result = arr.findIndex(function(item) {
    return item > 1 // 1
})
```

## 普通`for`循环

```js
let arr = [1,2,3]
let len = arr.length
for(let i = 0;i < len;i ++) {
    console.log(arr[i]) // 1 2 3
}
```

## `entries`

返回一个新的`Array Iterator`对象，该对象包含数组中每个索引的键/值对。

`arr.entries()`一个新的数组迭代器对象，它的原型（`__proto__:Array Iterator`）上有一个`next`方法，可用来遍历迭代器取得原数组的`[key,value]`。

```js
let arr = ['a','b','c']
let iterator = arr.entries()
console.log(iterator.next().value) // [0, 'a']
console.log(iterator.next().value) // [1, 'b']
```

```js
// 结合for...of进行循环
let arr = ['a','b','c']
let iterator = arr.entries()
for(let item of iterator) {
    console.log(item)
}
// [0, 'a']
// [1, 'b']
// [2, 'c']
```

## `keys`

返回一个包含数组中每个索引键的`Array Iterator`对象。

```js
let arr = ['a','b','c']
let iterator = arr.keys()
for(let key of iterator) {
    console.log(key) // 0 1 2
}
```

## `values`

返回一个包含数组中每个索引的值的`Array Iterator`对象。

```js
let arr = ['a','b','c']
let iterator = arr.values()
for(let key of iterator) {
    console.log(key) // a b c
}
```



