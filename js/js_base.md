# Js 基础回顾总结

## Js 基本类型 

**Js 分七种类型，Number、String、Null、undefined、Boolean、Symbol、Object.**

其中 Number、String、Null、undefined、Bollean、Symbol为基本数据类型，Object为引用类型。  

基本数据类型: 基本数据类型是放在内存栈内，所以可以直接操作变量值，并不会出现深浅贝问题。

引用变量: Object、function、Array、Date等都是引用数据类型，因为js不能直接访问，操作堆内空间，所以只能去栈内空间拿到对应的引用地址。

给引用类型变量直接赋值，只是将引用地址复制一份给新变量，因此出现浅拷贝问题。
    

> var a = { };  
var b = a;  
var c = b;
c.test = 4;  
//a = { test:4 }; b = { test:4 }; c = { test:4 }


## Js 类型判断

**js类型判断常用的为Typeof、instanceof、Object.prototype.toString.call()**

**Typeof**  

Typeof 除了Null、Array外的判断都是对的。对Null,Array都判断为Object. 
```js
    typeof 1 // 'number'  
    typeof '1' // 'string'  
    typeof true // 'boolean'  
    typeof Symbol() // 'symbol'  
    typeof a // 'undefined'  
    typeof console.log // 'function'  
    typeof [] // 'object'  
    typeof {} // 'object'
```

Object.prototype.toString.call()能够准确判断出类型，返回一个"[object 类型]" 的字符串
```js
    Object.prototype.toString.call(number),  //'[object Number]' 
    Object.prototype.toString.call(str),  //'[object String]'
    Object.prototype.toString.call(boolean),// '[object Boolean]'     
    Object.prototype.toString.call(array),//'[object Array]'
    Object.prototype.toString.call(json),// '[object Object]'    
    Object.prototype.toString.call(func),//'[object Function]' 
    Object.prototype.toString.call(und),//'[object Undefined]'
    Object.prototype.toString.call(nul),// '[object Null]'     
    Object.prototype.toString.call(date),//'[object Date]' 
    Object.prototype.toString.call(reg),//'[object RegExp]'  
    Object.prototype.toString.call(error),//'[object Error]'     
```

**instanceof 通过原型链来实现判断,所以对于直接赋值的基本数据类型无法判断，但对new出来的基本数据类型可以判断。**

```js
var test = new String('hello');
console.log(test instanceof String); // true
var test1 = 'hello';
console.log(test1 instanceof String);// false

//对于原型链继承的也能判断
function Person(){
}
function Student(){
}
Student.prototype = new Person();
var John = new Student();
console.log(John instanceof Student); // true
console.log(John instancdof Person);  // true
```


## 类型转换

Js的类型转换只有三种情况：  

* 转换布尔值 Boolean(mix)
* 转换为数字 Number(mix)、parseInt(string,radix)、parseFloat(string)
* 转换为字符串 toString(radix)、String(mix)

### 转Boolean

在条件判断是，除了 undefined , null , false , NaN , '' , 0 , -0 ,其他所有值都转为 true ,包括所有对象

### 对象转基础类型

对象转换类型时候，调用内置函数[[ToPrimitive]],主要行为：

* 判断是否是基础类型，是就不转
* 转字符串调用 obj.toString(),返回转换值。 如果不是字符串就先调用 obj.valueof()，结果不是基础类型的话再调用toString。。
* 调用obj.value(),如果转为基础类型，就返回转换的值
* 如果都没返回原始类型，就报错

## 运算

主要因为Js弱语言类型，能够使字符串转成数字后运算，但只有**加法**会出现问题，这点php就不会。

在加法时，有一方为字符串，那另一方也会转成字符串型，导致出现
```js
1 + '1' ; // '11'
```

如果一方不是数字或字符串，那就会被转成数字或者字符串。
```js
true + true // 2

4 + [1,2,3] // 41,2,3
```

## this 

简单来讲，谁调用函数，this就指向谁。但是call,apply,bind会改变this的指向

```js
function foo(){
    console.log(this.a);  
}

var a = 1;

foo();// this指向window

const obj = {
    a: 2,
    foo: foo,
}
obj.foo();// this指向obj

const c = new foo();// this指向实例化的foo对象，同时被赋值给了c
```

ES6出现后有了一个新特性  '=>' 箭头函数。箭头函数自身没有this,函数内的this指向父级函数的this.

```js
  function a(){
      return() => console.log(this);
  }
```

当this的这些规则同时出现，根据优先级来决定this的指向
* new的优先级最高
* bind,call,apply其次
* 最后就是函数调用时候指向