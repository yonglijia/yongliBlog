---
title: JS 数组方法总结
disqusId: jiayongli
date: 2018-06-07 00:22:09
tags: 
- JavaScript
categories: technology
---

作为最常用的类型，JavaScript中的数组还是和其他语言中有很大的区别的。
主要体现在两点：
- 数组中的每一项都可以保存任何类型的数据

- 数组的大小可以动态调整

<!--more-->

首先来介绍创建数组的两种方法
1. 第一种方式

```javascript
var arr1 = new Array();

var arr2 = new Array(3);

var arr3 = new Array('jerry');
```
可以看到这种方式建立数组，arr1是一个空数组，arr2是一个长度为3的数组，arr3是一个包含‘**jerry**’一个元素的数组。同时通过这种方式创建的数组，new操作符可以省略。
2. 第二种方式称为数组字面量表示法。

```javascript
var a = [];
var arr = ['tom','jack']
```
数组的长度是可动态调整，导致我们直接就可以设置它的长度

```javascript
var a = [123,423];
a.length = 10;
a[9]='123';
console.log(a[8])//undefined

a[10] = '123'
console.log(a.length)//10
```

从上面的代码中我们可以看出：

- 如果我们设置的长度大于原来的数组的长度的时候， 数组后面的元素自动设置为undefined。

- 如果我们对大于当前数组长度的位置赋值的时候，那么就会导致数组的长度自动变为你所赋值位置+1.

![2016103149463arr_function.png](http://oe3uqot64.bkt.clouddn.com/2016103149463arr_function.png)

## 改变数组的方法
### 栈方法 

pop和push很简单，也很容易理解。pop就是从数组的末尾删除一个元素并返回。push是在数组的末尾添加一个元素。

```javascript
var arr = [1,3,4];
arr.pop();
console.log(arr);//[1,3]

arr.push(5);
console.log(arr);//[1,3,5]
```

### 队列方法

shift和unshift是和栈方法是相对的，它俩是从数组的头部进行操作。shift是从头部删除一个元素，unshift是从同步加入一个元素。

```javascript
var arr = [1,3,4];
arr.shift();
console.log(arr);//[3,4]

arr.unshift(5);
console.log(arr);//[5,3,4]
```

### 重排序方法

reverse是对数组进行翻转。

```javascript
var arr = [1,3,4];
arr.reverse();
console.log(arr);//[4,3,1]
```

sort是对数组进行排序。

```javascript
var arr = [1,3,5,4];
arr.sort();
console.log(arr);//[1,3,4,5];
```

sort默认的对数组进行升序排序。sort可以接收一个自定义的比较函数，自定义排序规则。

sort方法会调用每个元素的toString()方法，从而通过字符串进行比较大小。即使是数值，依然要变换成字符串，从而就会带来一些问题。比如


```javascript
var arr = [1,3,15,4];
arr.sort()
console.log(arr);//[1,15,3,4];
```

转换为字符串之后，‘15’是排在‘3’，‘4’的前面的。这就带来了问题，所以在进行数值数组的排序，必须进行自定义排序规则。

```javascript
var arr = [1,3,15,4];
function compare(v1,v2){
    if(v1 > v2)
        return 1;
    if(v1 < v2)
        return -1;
    return 0;
}
arr.sort(compare)
console.log(arr);//[1,3,4,15]
```

### splice方法

splice方法可以说是数组中功能最强大的方法，集多项功能于一身。主要的用途就是用来向数组的中部插入元素。

splice方法主要有三种用法。

splice的返回值为删除的元素组成的数组。如果删除的元素为空，返回空数组。

- 删除元素

splice（index,count）,index表示删除的位置，count表示删除的项数。

```javascript
var arr = [1,3,4];
console.log(arr.splice(2,1));//[4]
//删除元素
console.log(arr);[1,3];
```

- 插入元素

splice(index,0,element,....)
index 表示要插入的位置，0代表删除0个元素，element要插入的元素,如果要插入多个元素，可以继续添加。

```javascript
var arr = [1,3,4];
console.log(arr.splice(2,0,'tom'));//[ ]

console.log(arr);//[1,3,'tom',4]
```

如果index的值大于数组本身的长度，那么就在最后位置添加。且数组的长度只会加1.

```javascript
var arr = [1,3,4];
console.log(arr.splice(5,0,'tom'));//[ ]

console.log(arr);//[1,3,4,'tom']
console.log(arr.length);//4
```

如果index的值为负数,那么就从（arr.length+index）位置开始插入，如果（arr.length+index）的值小于0，那么就从数组的开始位置进行插入。

```javascript
var arr = [1,3,4,4,7,6];
console.log(arr.splice(-1,0,'tom'));//[ ]

console.log(arr);//[1,3,4,4,7,'tom',6]
console.log(arr.length);//7

console.log(arr.splice(-7,0,'tom'));//[ ]

console.log(arr);//['tom',1,3,4,4,7,'tom',6]
console.log(arr.length);//8

console.log(arr.splice(-10,0,'jack'));//[ ]

console.log(arr);//['jack','tom',1,3,4,4,7,'tom',6]
console.log(arr.length);//9
```

- 替换元素

splice（index,count,element,....）.index代表替换开始的位置，count > 0,element表示要替换成的元素。其实替换过程包含两个过程:1.删除. 2插入.也就是上面的两个过程的融合。

```javascript
var arr = [1,3,4];
console.log(arr.splice(1,1,'tom'));//[3]

console.log(arr);//[1,'tom',4]
```
如果index大于数组的长度，或者小于0，处理的结果同上面插入元素处理的方式一样。

## 不改变数组的方法

### 转换方法
join方法主要是用来将数组的元素通过规定的方式连接成字符串。

```javascript
var arr = [1,3,4,5];
console.log(arr.join(','))//1,3,4,5
console.log(arr.join('+'))//1+3+4+5
console.log(arr.join('?'))//1?3?4?5
console.log(arr)//[1,3,4,5]
```

### 操作方法
slice和concat方法。
slice方法主要用来返回指定位置的数组的子数组。slice(start,end)。end省略，返回的是从开始位置到数组的末尾。end不省略，返回的是从start到end之间的子数组，包括start位置但不包括end位置的数组。

```javascript
var arr = [1,3,4,5];

console.log(arr.slice(1));//[3,4,5]
console.log(arr.slice(1,2));//[3]
```
如果slice方法的参数中有一个负数，则用数组长度加上该数来确定相应的位置。例如在一个长度为5的数组上调用slice(-2,-1)与调用slice(3,4)得到的结果相同。如果结束位置小于起始位置，则返回空数组。

concat 方法，主要是连接多个数组。

```javascript
var arr = [1,3,4,5];
var testArr = [1,23,4];
console.log(arr.concat(testArr));//[1,3,4,5,1,23,4]
console.log(arr.concat('tom'));//[1,3,4,5,'tom']
```
### 迭代方法
ES5新增加的迭代方法主要包括如下几种

> map 
> every 
> some 
> fliter 
> forEach 

这几个方法有一下共同点，都接收两个参数，一个是要在数组上每一项运行的函数，一个是运行该函数作用域的对象，改变this的指向（可选）。其中函数需要传入三个参数，一个是每个元素的值，每个元素的index，数组本身。

```javascript
function(value,index,array)
{
}
```

下面一个一个的来介绍
- map

map返回数组中每一个数组元素经过传入的函数处理后组成的新数组

```javascript
var arr = [1,3,4];
var newArr = arr.map(function(value,index,array){
	return value*2;
})
console.log(newArr);//[2,6,8]
console.log(arr);//[1,3,4]
```
- some和every

some和every比较相像。some是对每一个数组中的元素运行传入的函数，如果有一个返回true，那么就返回true；every是对每一个数组中的元素运行传入的函数，如果所有的都返回true，那么就返回true。

```javascript
var arr = [1,3,4];
var result1 = arr.some(function(value,index,array){
	return value > 2;
})

var result2 = arr.every(function(value,index,array){
	return value > 2;
})
console.log(result1);// true
console.log(result2);// false
```
- filter

从名字可以看出，这是一个过滤的方法，返回的一个数组，这个数组是满足传入的参数的函数的元素所组成的。

```javascript
var arr = [1,3,4];
var result = arr.filter(function(value,index,array){
	return value > 2;
})
console.log(result);// [3,4]
```
- forEach

forEach主要用来遍历，遍历数组中每一个元素，对其进行操作。该方法没有返回值。

```javascript
var arr = [1,3,4];
arr.forEach(function(value,index,array){
	console.log('arr['+index+']='+value);
})
// 结果
arr[0]=1
arr[1]=3
arr[2]=4
```

### 缩小方法
reduce和reduceRight.这两个方法接收两个参数，一个是每项都运行的函数，一个是缩小基础的初始值（可选）。reduce和reduceRight返回的是一个值。其中每项都运行的函数包含四个参数，

```javascript
funciton(prev,cur,index,array){
}
```
下面通过一个例子就可以说明这个函数是干嘛的。

```javascript
var arr = [1,3,4];
var result = arr.reduce(function(prev,cur,index,array){
	return prev+cur;
},10);
console.log(result)//18
var result1 = arr.reduce(function(prev,cur,index,array){
	return prev+cur;
});
console.log(result1)//8
```
reduceRight和reduce一样，无非他开始的位置是从数组的后面。

## 其他方法
- indexOf()
- lastIndexOf()

这两个主要是用来判断元素在数组中的位置,未找到返回-1，接收两个参数，indexOf(searchElement[, fromIndex]),lastIndexOf(searchElement[, fromIndex])。fromIndex可选。其中formIndex也可以指定字符串。

```javascript
var arr = [1,3,4,4,1,5,1];
var value = arr.indexOf(1)
console.log(value)//0
value = arr.indexOf(1,4)
console.log(value)//4
value = arr.indexOf(1,5)
console.log(value)//6

value = arr.lastIndexOf(1)
console.log(value)//6

value = arr.lastIndexOf(1,3)
console.log(value)//0
```
- toString()
- toLocalString()
- valueOf()

这三个方法是所有对象都具有的方法。

toString()返回的是一个字符串，toLocaleString同它类似。valueOf()返回的是一个数组

```javascript
var arr= [1,3,4]
console.log(arr.toString());//1,3,4
console.log(arr.valueOf());//[1,3,4]
console.log(arr.toLocaleString());//1,3,4
```
可以复写toString(),toLocaleString()返回不同的结果。