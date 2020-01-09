# `ES6`

## 1.`ES6`模块化如何使用，开发环境如何打包？

- 模块化的基本语法
- 开发环境配置
- 关于`js`众多模块化标准



**export语法：**

```js
// util1.js
export default {
    a: 100
}
```

```js
// util2.js
export function fn1() {
    alert('fn1')
}
export function fn2() {
    alert('fn2')
}
```

**import语法**：

```js
// index.js
import util1 from './util1.js'
import {fn1,fn2} from './util2.js'
console.log(util1)
fn1()
fn2() 
```







## 2.`class`和普通构造函数的区别？

## 3.`Promise`的基本使用和原理？

## 4.总结一下`ES6`其他常用功能？

