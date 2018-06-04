---
title: 深入理解原型原型链
date: 2018-06-05 02:27:15
tags: 
- 原型
- 原型链
- JavaScript
---

# 原型和原型链
首先要搞明白几个概念：

> 1. 函数（function）
> 2. 函数对象(function object)
> 3. 本地对象(native object)
> 4. 内置对象(build-in object)
> 5. 宿主对象(host object)

## 函数:
```
function foo(){
    
}
var foo = function(){
    
}
```
前者为函数声明，后者为函数表达式。typeof foo
的结果都是function。

<!--more-->

## 函数对象
函数就是对象,**代表函数的**对象就是函数对象
> 官方定义， 在Javascript中,每一个函数实际上都是一个函数对象.

> JavaScript代码中定义函数，或者调用Function创建函数时，最终都会以类似这样的形式调用Function函数:var newFun = new Function(funArgs, funBody)

其实也就是说，我们定义的函数，语法上，都称为函数对象，看我们如何去使用。如果我们单纯的把它当成一个函数使用，那么它就是函数，如果我们通过他来实例化出对象来使用，那么它就可以当成一个函数对象来使用，在面向对象的范畴里面，函数对象类似于类的概念。
```
var foo = new function(){
    
}
typeof foo // object

或者

function Foo (){
    
}
var foo = new Foo();

typeof foo // object
```
上面，我们所建立的对象

## 本地对象：
> ECMA-262 把本地对象（native object）定义为“独立于宿主环境的 ECMAScript 实现提供的对象”。简单来说，本地对象就是 ECMA-262 定义的类（引用类型）。它们包括：
Object,Function,Array,String,Boolean,Number
Date,RegExp,Error,EvalError,RangeError,ReferenceError,SyntaxError,TypeError,URIError.

我们不能被他们起的名字是本地对象，就把他们理解成对象（虽然是事实上，它就是一个对象，因为JS中万物皆为对象），通过
```
typeof(Object)
typeof(Array)
typeof(Date)
typeof(RegExp)
typeof(Math)

```
返回的结果都是function

也就是说其实这些本地对象（类）是通过function建立起来的，
```
function Object（）{
    
}
function Array（）{
    
}
...
```

可以看出Object原本就是一个函数，通过new Object()之后实例化后，创建对象。类似于JAVA中的类。

## 内置对象：
> ECMA-262 把内置对象（built-in object）定义为“由 ECMAScript 实现提供的、独立于宿主环境的所有对象，在 ECMAScript 程序开始执行时出现”。这意味着开发者不必明确实例化内置对象，它已被实例化了。ECMA-262 只定义了两个内置对象，即 Global 和 Math （它们也是本地对象，根据定义，每个内置对象都是本地对象）。


理清楚了这几个概念，有助于理解我们下面要讲述的原型和原型链。

## prototype

prototype属性是每一个函数都具有的属性，但是不是一个对象都具有的属性。比如
```
function Foo(){
    
}

var foo = new Foo()；
```
其中Foo中有prototype属性，而foo没有。而且foo中的隐含的__proto__属性指向Foo.prototype。

```
foo.__proto__ === Foo.prototype
```
为什么会存在prototype属性？

Javascript里面所有的数据类型都是对象，为了使JavaScript实现面向对象的思想，就必须要能够实现‘继承’使所有的对象连接起来。而如何实现继承呢？JavaScript采用了类似C++，java的方式，通过new的方式来实现实例。

举个例子，child1,child2都是Mother的孩子，且是双胞胎。（虽然不是很好，但是还是很能说明问题的）
```
function Mother(name){
    this.name = name;
    this.father = 'baba';
}
var child1 = new Mother('huahua');
var child2 = new Mother('huihui');
```

如果有一天，发现孩子的父亲其实是Baba，那么就要对孩子每一个孩子的father属性。
```
child1.father ='Baba';
console.log(child2.father) // baba
```
也就是说修改了其中一个孩子的father属性不会影响到下一个，属性的值无法共享。

正是这个原因才提出来prototype属性，把需要共享的属性放到构造函数也就是父类的实例中去。

##  __ proto __

__proto__属性是每一个对象以及函数都隐含的一个属性。对于每一个含有__proto__属性，他所指向的是创建他的构造函数的prototype。原型链就是通过这个属性构件的。

想像一下，如果一个函数对象（也成为构造函数）a的prototype是另一个函数对象b构件出的实例，a的实例就可以通过__proto__与b的原型链起来。而b的原型其实就是Object的实例，所以a的实例对象，就可以通过原型链和object的prototype链接起来。
```
function a(){
    
}
function b(){
    
}
var b1 = new b();
a.prototype = b1;
var a1 = new a();
console.log(a1.__proto__===b1);//true
console.log(a1.__proto__.__proto__===b.prototype) //true
console.log(a1.__proto__.__proto__.__proto__===Object.prototype) //true
```


如果要理清原型和原型链的关系，首先要明确一下几个概念：
1.JS中的所有东西都是对象，函数也是对象, 而且是一种特殊的对象

2.JS中所有的东西都由Object衍生而来, 即所有东西原型链的终点指向Object.prototype

3.JS对象都有一个隐藏的__proto__属性，他指向创建它的构造函数的原型，但是有一个例外，Object.prototype.__proto__指向的是null。

4.JS中构造函数和实例(对象)之间的微妙关系

构造函数通过定义prototype来约定其实例的规格, 再通过 new 来构造出实例,他们的作用就是生产对象.

```
function Foo(){
    
}
var foo = new Foo();
foo其实是通过Foo.prototype来生成实例的。

```
构造函数本身又是方法(Function)的实例, 因此也可以查到它的__proto__(原型链)
```
function Foo(){
    
}
等价于
var Foo= new Function（）；
```
而Function实际上是
```
function Function(){
    Native Code
}
也就是等价于
var Function= new Function()；
```
所以说Function是通过自己创建出来的。正常情况下对象的__proto__是指向创建它的构造函数的prototype的.所以Function的__proto__指向的Function.prototype

Object 实际上也是通过Function创建出来的
```
typeof(Object)//function
所以，
function Object(){
    Native Code
}
等价于
var Object = new Function();
```
那么Object的__proto__指向的是Function.prototype，也即是
```
Object.__proto__ === Function.prototype //true
```

下面再来看Function.prototype的__proto__指向哪里

因为JS中所有的东西都是对象，那么，Function.prototype 也是对象，既然是对象，那么Function.prototype肯定是通过Object创建出来的，所以，
```
 Function.prototype.__proto__ === Object.prototype //true
```

综上所述，Function和Object的原型以及原型链的关系可以归纳为下图。

![2016102714775006823199.jpg](http://oe3uqot64.bkt.clouddn.com/2016102714775006823199.jpg?imageView2/0/format/jpg)

对于单个的对象实例，如果通过Object创建，
```
var obj = new Object();
```

那么它的原型和原型链的关系如下图
![20161027147749990054513.jpg](http://oe3uqot64.bkt.clouddn.com/20161027147749990054513.jpg?imageView2/0/format/jpg)

如果通过函数对象来创建，
```
function Foo(){
    
}
var foo = new Foo();
```

那么它的原型和原型链的关系如下图

![20161027147750059948187.jpg](http://oe3uqot64.bkt.clouddn.com/20161027147750059948187.jpg?imageView2/0/format/jpg)

那JavaScript的整体的原型和原型链中的关系就很清晰了，如下图所示

![20161027147750055267571.jpg](http://oe3uqot64.bkt.clouddn.com/20161027147750055267571.jpg?imageView2/0/format/jpg)

