# 作用域

##  	1.1 RHS、LHS

​	**LHS** : **赋值操作的目标是谁**对哪个赋值就对哪个进行**LHS**引用，可以理解为赋值操作的目标。**var c 等号左边**

​	**RHS** : **谁是赋值操作的源头（retrieve his source value）**,需要获取哪个变量的值，就对哪个变量的值进行**RHS**引用，理解为赋值操作的源头。**c = 2 等号右边**

​	**异常 :** 当**RHS**在所有嵌套的作用域中遍寻不到所需的变量，引擎就会抛出**ReferenceError**异常
                  当**LHS**在所有嵌套的作用域中遍寻不到所需的变量，全局作用域会创建一个具有改名称的变量，并将其返还给引擎(**非严格模式**下)，在**严格模式**下并不创建且抛出**ReferenceError**

> ​	这两种不同的引用方式的在对没有声明的变量的处理上是不同的。而这个不同之处对于我们编写代码和分析JS引擎报的错误是很有用处的。
>
> - 当对一个变量执行RHS查询时，如果遍历该变量所在处的词法作用域未能找到这个变量，JS引擎就会抛出 ReferenceError 错误如果成功查询到了这个变量，但是对这个变量执行不合理操作，比如对一个非数组的变量执行下标取值，JS引擎就会抛出 TypeError 错误。
> - 当对一个变量执行LHS查询时，同样在遍历作用域后无法找到该变量，在非ES5的严格模式下，系统就会自动在全局作用域中创建一个同名变量，并将引用转移到该新建的全局变量中。而在ES5的严格模式下，LHS查询失败时JS引擎会抛出一个同RHS一样的 ReferenceError 错误。
>
> 因此，对LHS查询和RHS查询的仔细区分和理解无论是对JS执行过程本身的理解还是分析错误都是有所好处的。
>

``` js
function foo(a) { 
    console.log(a)
}

foo(2);
//RHS foo(2)->foo
//LHS 2->a a = 2
//RHS console
//RHS .log()
//RHS a

function foo(a){ //a = 2
    var a = b;
    return a + b;
}
var c = foo(2);

//LHS c=... ; a = 2 ; b = ....
//RHS foo(2... ; = a; ; a.. ; ..b

//第一，var c中的c需要被赋值，在赋值操作的左侧，所以对c进行LHS引用
//第二，变量c需要被赋值，他的值是foo(2),那么foo(2)的值是多少呢，需要查找foo(2)的值，在赋值操作的右侧，所以对foo(2)进行RHS引用
//第三，隐含赋值操作，将2传递给function foo(a){……}函数的参数a，a在赋值操作的左侧，对a进行LHS引用
//第四，var b=a;中，b需要被赋值，处在赋值操作的左侧，所以b进行的LHS，b的值将从a来，那么右侧的a的值从何而来呢？这就需要对赋值操作右侧的a进行RHS。
//第五，return a+b;中，需要找到a与b的值的来源，a与b都在赋值操作的右侧，才能得到a+b的值，所以对a与b都是进行RHS引用。
```

## 1.2 词法作用域

​	**词法作用域**就是定义在词法阶段的作用域

``` js
//   1.  包含着整个全局作用域，其中只有一个标识符: foo。
function foo(a){
    //   2.   包含着foo 所创建的作用域，其中有三个标识符: a、bar 和b。
    var b = a * 2;
    function bar(c){
        //   3.   包含着bar 所创建的作用域，其中只有一个标识符: C。
        console.log(a,b,c);
        // 3 END
    }
    bar(b * 3);
    // 2 END
}
foo(2); // 2,4,12;
// 1 END
```

### 1.2.1 eval()

前提为非严格模式下！

``` js
function foo(str, a){
    eval( str ); //欺诈
    console.log(a,b);
}
var b = 2;
foo("var b = 3", 1); //1, 3
```

### 1.2.2 with()

``` js
function foo(obj){
    with(obj){
        a = 2;
    }
}
var o1 = {
    a : 2
};
var o2 = {
    b : 3
};

foo(o1);
console.log( o1.a); //2

foo(o2);
console.log( o2.a); //undefined
console.log( a );  //2  -> a被泄漏到全局作用域
```

​	在with块内部，看起来只是对变量a进行简单的词法引用，实际上是一个LHS引用，并将2赋值给它

## 1.3 函数作用域和块作用域

### 1.3.1 规避冲突

``` js
function foo(){
    function bar(a){
        i = 3;   //修改for循环所属作用域中的i
        console.log(a + i);
    }
    for(var i = 0 ; i< 10; i++){
        bar( i * 2);    //无限循环
    }
}
```

解决：

- 全局命名空间
- 模块管理

### 1.3.2 函数作用域

``` js
var a = 2;
function foo(){
    var a = 3;
    console.log( a );   // 3
}
foo();
console.log(a); //2
```

此方法需声明函数foo，但foo名称污染了所在作用域，且必须显式的通过foo函数名才能调用foo函数

``` js
var a = 2;
(function foo(){
    var a = 23;
    console.log(a);  //23
})();
console.log(a); //2
```

此方法函数会被当作函数表达式而不是一个标准的函数声明来处理

> 区分函数声明和表达式最简单的方法是看function关键词出现在声明中的位置（整个声明中的位置），如果function是声明中的第一个词，那么就是一个函数声明，否则就是一个函数表达式

(function(){….})作为函数表达式意味着foo只能在…所代表的位置中被访问，外部作用域则不行。

### 	1.3.3 匿名函数

``` js
setTimeout(function(){
    console.log("hhh");
},1000);
```

函数表达式可以是匿名的，但函数声明不可以省略函数名

**匿名函数缺点**

1. 匿名函数在栈追踪中不会显示出有意义的函数名，**使得调试很困难**
2. **降低代码可读性**
3. 当函数需要引用自身时只能使用已经过期的arguments.callee引用，比如：在递归中、事件触发后事件监听器需要解绑自身

### 1.3.4 IIFE 立即执行函数表达式

**IIFE （Immediately Invoked Function Expression）————    (function foo( ) {…..} ) ( )  **

``` js
var a = 2;
(function IIFE(){
    var a = 3;
    console.log( a ); // 3
})();
console.log( a ); //2
//------------------------------------------完全相同
var a = 2;
(function IIFE(){
    var a = 3;
    console.log( a ); // 3
}());
console.log( a ); //2

var a = 2;
(function IIFE(gobal){
    var a = 3;
    console.log( a ); // 3
    console.log(gobal.a) //2
})(window);
console.log( a ); //2

//倒置代码运行顺序
var a = 2;
(function IIFE(def){
    def(window);
})(function def( global ){
    var a = 3;
    console.log( a );
    console.log( global.a );
})
```

## 1.4 块作用域

