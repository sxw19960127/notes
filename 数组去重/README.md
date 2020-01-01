# 数组去重

## 原型链上编程

### `hash`方式去重

借用对象的同一属性名不可能出现两次来实现去重效果。

```js
Array.prototype.unique = function() {
    var temp = {},
        arr = [], // 返回的去重后的数组
    	len = this.length; // 减少代码计算次数
    for(var i = 0;i < len;i ++) {
  // a.数组中的属性值拿出来当做对象的属性名并与对象进行比对,如果对象上该属性名已经存在属性值,则忽略;
  // b.在对象上找不到该属性名对应的属性值,那么这个属性名即数组中对应的属性值就是我们想要的筛选结果;
  // c.我们随便给筛选出的我们想要的属性名赋予一个属性值进行标记,然后push出该属性名到新数组;
  // d.循环上面操作;
    	//if(temp[this[i]]) {
    	//	啥也不干
    	//}else {
    	//	取到 undefined 的时候,才是我们的操作
    	//}
    	
    	//简化代码
        if(!temp[this[i]]) { 
			//!undefined --> true,执行下面代码块
            temp[this[i]] = "abc";
            arr.push(this[i]);
        }
    }
    return arr;
}
```

### for循环嵌套,利用splice去重

```js
function newArr(arr){
    for(var i=0;i<arr.length;i++){
        for(var j=i+1;j<arr.length;j++)
            if(arr[i]==arr[j]){ 
            //如果第一个等于第二个，splice方法删除第二个
            arr.splice(j,1);
            j--;
            }
        }
    }
    return arr;
}
var arr = [1,1,2,5,6,3,5,5,6,8,9,8];
console.log(newArr(arr))
```

### 建新数组,利用indexOf去重

```js
function newArr(array){ 
    //一个新的数组 
    var arrs = []; 
    //遍历当前数组 
    for(var i = 0; i < array.length; i++){ 
        //如果临时数组里没有当前数组的当前值，则把当前值push到新数组里面 
        if (arrs.indexOf(array[i]) == -1){ 
            arrs.push(array[i])
        }; 
    } 
    return arrs; 
}
var arr = [1,1,2,5,5,6,8,9,8];
console.log(newArr(arr))
```

### ES6中利用Set去重

```js
function newArr(arr){
    return Array.from(new Set(arr))
}
var arr = [1,1,2,9,6,9,6,3,1,4,5];
console.log(newArr(arr))
```

