---
title: js拷贝问题
date: 2018-03-26 16:29:30
tags:
---

# js拷贝问题

## js数据类型
1. 基本类型
	- 按值访问，可以直接操作保存在变量中的实际的值
	- Number
	- Boolean
	- String
	- NULL
	- Undefined
2. 引用类型
	- 引用类型的值是保存在内存中的对象，JavaScript 不允许直接访问内存中的位置，也就是说不能直接操作对象的内存空间。在操作对象时，实际上是在操作对象的引用而不是实际的对象。
	- Array
	- Object
	- Date
	- RegExp
	- Function
3. 实例
```
var obj = {name:"yu", age:10};
var newobj = obj;
newobj.name = "f";
console.log(obj.name);		// "f"
console.log(newobj.name);	// "f"
```

## 深拷贝 与 浅拷贝

1. 浅拷贝
	- 直接拷贝对象：
		+ 也就是拷贝应用，两个变量之间还会相互影响
		+ var obj2 = obj1;
	- 只是拷贝对象的第一层属性：
		+ 基本类型不会相互影响，但是内部的引用类型还会相互影响
		+ 如下的：
			* obj1和shallowobj对象的a可以互不影响；
			* obj1和shallowobj对象的arr指向的是同一内存地址，相互影响；
```
var obj1 = {a:1, arr:[1,2]}; 
var shallowobj = shallowCopy(obj1);
function shallowCopy(src) {
	var dst = {};
	for (var prop in src) {
		if (src.hasOwnProperty(prop)) {
			dst[prop] = src[prop];
		}
	}
	return dst;
}
```
	- 对象的浅拷贝Object：
		+ Object.assign():可以把任意多个的源对象自身的可枚举属性拷贝给目标对象
```
var x = {
  a: 1,
  b: { f: { g: 1 } },
  c: [ 1, 2, 3 ]
};
var y = Object.assign({}, x);
y.a = 2;
console.log(x.a + " " + y.a);     // 1, 2
console.log(y.b.f === x.b.f);     // true
}
```
		+ Object.getOwnPropertyNames():可拷贝不可枚举的属性
```
function shallowCopyOwnProperties( source )  
{
    var target = {} ;
    var keys = Object.getOwnPropertyNames( original ) ;
    for ( var i = 0 ; i < keys.length ; i ++ ) {
        target[ keys[ i ] ] = source[ keys[ i ] ] ;
    }
    return target ;
}

```
		+ Object.getPrototypeOf 和 Object.getOwnPropertyDescriptor 拷贝原型与描述符
			- 如果我们需要拷贝原对象的原型和描述符，我们可以使用 Object.getPrototypeOf 和 Object.getOwnPropertyDescriptor 方法分别获取原对象的原型和描述符，然后使用 Object.create 和 Object.defineProperty 方法，根据原型和属性的描述符创建新的对象和对象的属性。
```
function shallowCopy( source ) {
    // 用 source 的原型创建一个对象
    var target = Object.create( Object.getPrototypeOf( source )) ;
    // 获取对象的所有属性
    var keys = Object.getOwnPropertyNames( source ) ;
    // 循环拷贝对象的所有属性
    for ( var i = 0 ; i < keys.length ; i ++ ) {
        // 用原属性的描述符创建新的属性
        Object.defineProperty( target , keys[ i ] , Object.getOwnPropertyDescriptor( source , keys[ i ])) ;
    }
    return target ;
}

```
	- 数组的浅拷贝Array：
		+ slice()：
			* slice() 方法将一个数组被选择的部分（默认情况下是全部元素）浅拷贝到一个新数组对象，并返回这个数组对象，原始数组不会被修改。 
		+ concat()
			* concat() 方法用于合并两个或多个数组。此方法不会更改现有数组，而是返回一个新数组。
```
var array = [1, 'string', {a: 1,b: 2, obj: {c: 3}}];
// slice()
var array1 = array.slice();
// concat()
var array2 = array.concat();
// 改变原数组元素
array[1] = 'newString';
array[2].c = 4;
array1[0] = 2;
array2[0] = 3;
console.log(array[0]+' ' + array1[0] + ' ' + array2[0]);
console.log(array1[1]); // string
console.log(array1[2].c); // 4
console.log(array2[1]); // string
console.log(array2[2].c); // 4

```
	- json对象的浅拷贝：
		+ JSON对象是ES5中引入的新的类型（支持的浏览器为IE8+）
		+ JSON对象parse方法可以将JSON字符串反序列化成JS对象
		+ stringify方法可以将JS对象序列化成JSON字符串，
		+ 缺点：
			* 这种方式有一定的局限性，就是对象必须遵从JSON的格式，当遇到层级较深，且序列化对象不完全符合JSON格式时，使用JSON的方式进行深拷贝就会出现问题。
```
//例1
var source = { name:"source", child:{ name:"child" } } 
var target = JSON.parse(JSON.stringify(source));
target.name = "target";  //改变target的name属性
console.log(source.name); //source 
console.log(target.name); //target
target.child.name = "target child"; //改变target的child 
console.log(source.child.name); //child 
console.log(target.child.name); //target child
//例2
var source = { name:function(){console.log(1);}, child:{ name:"child" } } 
var target = JSON.parse(JSON.stringify(source));
console.log(target.name); //undefined
//例3
var source = { name:function(){console.log(1);}, child:new RegExp("e") }
var target = JSON.parse(JSON.stringify(source));
console.log(target.name); //undefined
console.log(target.child); //Object {}
```


2. 深拷贝
	- 将对象的属性递归的拷贝到一个新的对象上，两个对象有不同的地址，不同的引用，也包括对象里的对象属性，两个变量之间完全独立
	- 缺陷：
		+ 深拷贝的完美实现是不存在的，而且可能存在性能问题
		+ 完全可以用浅拷贝实现我们的逻辑任务，没必要完全深拷贝
	- 实现：
		+ 可以深拷贝对象或数组
```
var cloneObj = function(obj){
    var str, newobj = obj.constructor === Array ? [] : {};
    if(typeof obj !== 'object'){
        return;
    } else if(window.JSON){
        str = JSON.stringify(obj), //系列化对象
        newobj = JSON.parse(str); //还原
    } else {
        for(var i in obj){
            newobj[i] = typeof obj[i] === 'object' ? 
            cloneObj(obj[i]) : obj[i]; 
        }
    }
    return newobj;
};
```

3. 
