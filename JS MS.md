# 基础知识
## 原型 原型链

### 构造函数

``` js
function Foo(name, age){
  this.name = name;
  this.age = age;
  this.class = 'class-1';
  //return this //默认存在
}
var f = new Foo('szw', 20);
//var f1 = new Foo('list', 21)
```

- var a = {} 其实是var a = new Object()的语法糖

- var a = []其实是var a = new Arrayt()的语法糖

- function Foo(){…}其实是 var Foo = new Function(...)

- 使用instanceof判断一个函数是否是一个变量的构造函数

### 原型规则和示例

- 所有的引用类型（数组、对象、函数），都具有对象特性，即可自由扩展属性（除了“null”以外）

```js
var obj = {};
	obj.a = 100; //可扩展
var arr = [];
	arr.a = 100; //可扩展
function fn (){};
	fn.a = 100; //可扩展
```

- 所有的引用类型（数组、对象、函数），都有一个_ proto _（隐式原型）属性，属性值是一个普通的对象

``` js 
console.log(obj._proto_);
console.log(arr._proto_);
console.log(fn._proto_);
```

- 所有的函数，都有一个prototype（显试原型）属性，属性值也是一个普通的对象

``` js
console.log(fn.prototype);
```

- 所有的引用类型（数组、对象、函数），_ proto _属性值指向它的构造函数的prototype属性值

``` js
console.log(obj._proto_ === Object.prototype);
```

- 当试图得到一个对象的某个属性时，如果这个对象本身没有这个属性，那么会去它的_ proto _（即它的构造函数的prototype）中寻找

``` js
//构造函数
function Foo(name, age){
  this.name = name;
}
Foo.prototype.alertName = function(){
  alert(this.name);
}
//创建示例
var f = new Foo('szw');
f.printName = function(){
  console.log(this.name);
}

//测试
f.printName();
f.alertName();

var item;
for(item in f){
  if(f.hasOwnProperty(item)){
    console.log(item);
  }
}
```

- instanceof 用于判断引用类型属于哪个构造函数



## 作用域 闭包

### 执行上下文

### this

1. this 要在执行时才能确认值，定义时无法确认
2. 作为构造函数执行
3. 作为对象属性执行
4. 作为普通函数执行
5. call apply bind

``` js
//2
function Foo(name){
  this.name = name;
}
var f = new Foo('szw');
//3
var obj ={
  name: 'A';
  printName: function(){
  		console.log(this.name);
	}
}
obj.printName(); //'A'
//4
function fn(){
  console.log(this)  //this === windows
}
fn();
//5
function fn1(name){
  alert(name)
  console.log(this)
}
fn1.call({x:100},'szw') //this->Object {x:100}  apply相同

var fn2 = function(name){
  alert(name)
  console.log(this)
}.bind({y:200})
fn2('szw'); //this ->{y:200}
```



### 作用域

- 无块级作用域

``` js
if(true){
  var name = 'szw';
}
console.log(name);

```

- 函数和全局作用域

### 作用域链

``` js
var a = 100;
function fn(){
  var b = 200;
  //当前作用域没有定义的变量，即‘自由变量’
  console.log(a)
  console.log(b);
}
fn()
```



### 闭包

- 函数作为返回值
- 函数作为参数传递
- 主要应用于封装变量、收揽权限

``` js
function F1(){
  var a = 100;
  //返回一个函数
  return function(){
    console.log()
  }
}
//f1得到一个函数
var f1 = F1();
var a = 200;
f1(); //100

```

```js
function F1(){
  var a = 100;
  //返回一个函数
  return function(){
    console.log()
  }
}
//f1得到一个函数
var f1 = F1();
function F2(fn){
  var a = 200;
  fn();
}

F2(f1); //100


```



## 异步 单线程

### 什么是异步

#### 何时需要异步

- 在可能发生等待的情况

  定时请求： setTimeOut、setInveral

  网络请求：AJAX、动态<img>加载

  事件绑定

- 等待过程中不能像alert 一样阻塞程序运行

  img加载示例

  ```js
  console.log('star');
  var img = document.createElement('img');
  document.body.appendChild(img);
  img.onload = function(){
    console.log('loaded')
  }
  img.src = '1.jpg'
  console.log('end')
  console.log(img.src);
  ```

  ​

### 前端使用异步场景

### 异步和单线程



# JS API
## DOM

- forEach 遍历所有元素
- every 判断所有元素是否符合条件
- some 判断是否至少有一个元素符合条件
- sort 排序
- map 对元素重新组装，生成新数组
- filter 过滤符合条件的元素

## AJAX

### 跨域

- 协议、端口、域名   有一个不同就算跨域
- img、link、script 3个标签允许跨域请求
- 解决： JOSNP http-header

###  xmlHttpRequest

``` js
var xhr = new XMLHttpRequest();
xhr.open("GET", "/api", false)
xhr.onreadystatechange = function(){
  if(xhr.readyState == 4){
    //0未初始化
    //1载入
    //2载入完成
    //3交互
    //4完成
    if(xhr.status == 200){
      //2xx 完成
      //3xx 重定向
      //4xx 客户端请求错误
      //5xx 服务端请求错误
      alert(xhr.responseText);
    }
  }
}
xhr.send(null);
```



### 状态码说明



## 事件绑定

### 通用事件绑定

``` js
var btn = document.getElementById('btn1');
btn.addEventListener('click',function(event){
  console.log('clicked');
})

function bindEvent(elem,type,fn){
  elem.addEventListener(type,fn)
}
var a = document.getElementById('link1');
bindEvent(a,'click',function(e){
  e.preventDefaule();  //阻止默认行为
  alter('clicked');
})
```

### 事件冒泡

### 代理

``` js
//通用事件绑定

function bindEvent(elem, type, select, fn){
  if(fn == null){
    fn = select;
    select = null;
  }
  elem.addEventListener(type,function(e){ //添加事件
    var target;
    if(select){
      target = e.target； //返回事件元素
      if(target.matches(select)){ //target是否符合目标元素
        fn.call(target, e); //this->target
      }
    }else{
      fn(e);
    }
  })
}

//使用代理
var div1 = document.getElementById('div1');
bindEvent(div1, 'click', 'a', function(e){
  console.log(this.innerHTML)
})

//不使用代理
var a = document.getElementById('a1');
bindEvent(a, 'click', function(e){
  console.log(a.innerHTML)
})
```



# 开发环境
##版本管理
##模块化
##打包工具
# 运行环境
##页面渲染
##性能优化
#变量类型和计算

var a = 100;
var b = a;
a = 200;
console.log(b); //100

