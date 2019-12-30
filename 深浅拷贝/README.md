# 深浅拷贝

## 浅拷贝

仅是引用值（内存地址）被拷贝了。

### 数组的浅拷贝

```js
let arr = [1,2,3]
let newArr = arr.concat()
```

```js
let arr = [1,2,3]
let newArr = arr.slice()
```

```js
let arr = [1,2,3]
let newArr = [...arr]
```

```js
let arr = [1,2,3]
let newArr = Array.from(arr)
```

### 对象的浅拷贝

```js
let obj1 = {
    a: 1,
    b: 2
}
let obj2 = {
    ...obj1
}
```

```js
let obj1 = {
    a: 1,
    b: 2
}
let obj2 = Object.assign({}, obj1)
```







