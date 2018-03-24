## 值类型

```javascript
var a = 100;
var b = a;
a = 200;
console.log(b); //100
```
## 引用类型——对象、数组、函数 

变量相当于一个指针

```javascript
var a = {age: 20};
var b = a;
b.age = 21;
console.log(a.age);  //21
```

## typeof——6种类型

```javascript
typeof undefined //undefined
typeof 'abc' //string
typeof 123 //number
typeof true //boolean
//typeof 只能区分上面这4种类型
typeof {} //object
typeof [] //object
typeof null //object  引用类型
typeof console.log() //function
```

## 变量计算——强制类型转换

### 字符串拼接

```js
var a = 100 + 10; //110
var b = 100 + '10';	//'10010'
```



### ==运算符

```js
100 == '100' //true   转化为字符串
0 == '' //true   转化为false
null == undefined //true   转化为false
```



### if 语句

```js
var a = ture;
if(a){
  //->
}
var b = 100;
if(b){
  //转化为boolean true 运行
}
var c = '';
if(c){
  //转化为boolean false
}
```



### 逻辑运算

```Js
console.log(10 && 0); //0  10->true
console.log('' || 'abc'); //'abc' ''->false
console.log(!windows.abc); //ture windows.abc->undefined

//如何检测被强制转换成什么——>加！！
var a = 100;
console.log(!!a);

```

## 何时使用==与===

```js
if(obj.a == NULL){
  //相当于obj.a === null || obj.a ===undefined 的简写形式
}
```

## js内置函数——数据封装类对象

``` js
object
Array
Boolean
Number
String
Function
Date
RegExp
Error
```

## js按存储方式区分变量类型

``` js
//值类型
var a = {age: 20};
var b = a;
b.age = 21;
console.log(a.age);  //21
//引用类型
var a = {age: 20};
var b = a;
b.age = 21;
console.log(a.age);  //21 
```

## json只是js的一个内置对象



