# 深浅拷贝

## 浅拷贝

仅是引用值（内存地址）被拷贝了。

注意：在定义一个对象或数组时，变量存放的往往只是一个地址，当我们使用对象拷贝时，如果属性是对象或数组时，这时候我们传递的也只是一个地址，因此子对象在访问该属性时，会根据地址回溯到父对象指向的堆内存中，即父子对象发生了关联，两者的属性值会指向同一内存空间。

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

```js
const object1 = {
  a: 1,
  b: 2,
  c: 3
};
const object2 = Object.assign({c: 4, d: 5}, object1);
console.log(object2); //Object2 = { c: 3, d: 5, a: 1, b: 2 }
// 浅拷贝返回的不是一个新对象,而是把一个或多个源对象添加到目标对象;
```

```js
let object1 = {
  a: 1,
  b: 2,
  c: 3
};
let object2 = {...object1};
object1.a=11;
console.log(object2); // Object2 = { a: 11, b: 2, c: 3 } 浅拷贝
```

```js
var obj = {x: 1,y : {x: 2}}
var obj1 = {}
for(var i in obj) {
	obj[i] = obj[i];
}
```

## 深拷贝

在实际编码中，我们不希望父子对象之间产生关联，那么这时候可以用到深拷贝。既然属性值类型是数组和对象时只会传址，那么我们就用递归来解决这个问题，把父对象中所有属于对象的属性类型都遍历赋给子对象即可。

```js
// JSON.stringify()和JSON.parse()
// 先把对象使用JSON.stringify()转为字符串赋值给另外一个变量,然后使用JSON.parse()转回来;
let object1 = {
  a: 1,
  b: 2,
  c: 3
};
let object2 = JSON.parse(JSON.stringify(object1));
object2.a = 11;
console.log(object1,object2);
// Object1 = { a: 1, b: 2, c: 3 }; 
// Object2 = { a: 11, b: 2, c: 3 };
```

```js
var obj = {
    name : "abc",
    age : 123,
    card : ['visa', 'master'],
    wife : {
        name : "bcd",
        son : {
            name : aaa
        }
    }
}
var obj1 = {
    name : obj.name, // 原始值直接拷贝就行;
    age : 123,
    card : [obj.card[0], obj.card[1]],
    wife : {
        name : "bcd", // 是原始值,直接拿过来
        son : {
            
        }
    }
}
```

属性判断：判断是原始值还是引用值，引用值需要特殊拷贝，原始值直接拷贝。引用值的话考虑是数组还是对象。

**深度克隆思路步骤**：

1. 判断是不是原始值，`typeof()`，结果如果是`object`则是引用值，如果不是基本上都是原始值，先不考略`null`
2. 引用值的话，判断是数组还是对象，3种判断方法，建议使用`toString()`
3. 建立相应的数组或对象

```js
function deepClone(origin, target) {
    var target = target || {}; // target有就用target,没有就使用||后面的;
    	toStr = Object.prototype.toString,
    	arrStr = "[object Array]"; // 拿过来,等一下方便比对
    for(var prop in origin) {
        if(origin.hasOwnProperty(prop)) { // 判断去除原型上的属性
        	if(origin[prop] !== "null" && typeof(origin[prop]) == 'object') { 
                // 判断是不是原始值
                // 判断引用值是数组的引用值还是对象的引用值
                if(toStr.call(origin[prop]) == arrStr) { // 是数组
                    target.[prop] = [];
                }else {
                    // 对象
                    target[prop] = {};
                }
                deepClone(origin[prop], target[prop]); // 递归,再次循环上面的过程
        	}else {
                target[prop] = origin[prop]; // 搞定原始值
        	}
            
        }
    }
    return target;
}
```

使用三目运算符，简化上述深度克隆：

```js
function deepClone(origin, target) {
    var target = target || {};  
    	toStr = Object.prototype.toString,
    	arrStr = "[object Array]"; 
    for(var prop in origin) {
        if(origin.hasOwnProperty(prop)) {  
        	if(origin[prop] !== "null" && typeof(origin[prop]) == 'object') {  
                target[prop] = toStr.call(origin[prop]) == arrStr ? [] : {};
                deepClone(origin[prop], target[prop]); 
        	}else {
                target[prop] = origin[prop]; 
        	}
        }
    }
    return target;
}
```

```js
// 递归实现深克隆
function clone(obj) {
	var obj1 = {};
	for(var i in obj) {
		if(typeof obj[i] == "object") {
			obj1[i] = clone(obj[i])
		}else {
			obj1[i] = obj[i];
		}
	}
	return obj1
}
```

```js
// 在原型链上操作实现深克隆
function clone1(obj) {
	function Temp() {}
	Temp.prototype = obj;
	return new Temp();
}
```







