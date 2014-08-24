# 简单的函数式Javascript
## 什么是函数型语言
函数式编程是一种编程模型，他将计算机运算看做是数学中函数的计算，并且避免了状态以及变量的概念．
## 函数型语言特点
### lambda(λ)表达式
lambda好陌生对吧，那看看下面的代码：

    function ajax(url ,callback) {
        var xhr = new XMLHttpRequest;
        xhr.onreadystatechange = function(){
            if(this.readyState == 4 && this.status == 200) {
                callback(this.responseText);
            }
        }
        xhr.open("GET", url, true);
        xhr.send();
    }
    //用法
    ajax("http://www.youmi.net" ,function(data) {
        console.log(data)
    })

熟悉吗？  
这里将一个匿名函数作为callback传入作为ajax参数，这里就用到lambda表达式啦  
lambda表达式就是匿名函数
### 高阶函数
高阶函数又是什么？看看下面代码：

    function fn(callback){
        return function() {
            console.log(callback)
        }
    }
    function fn2() {
        console.log("fn2")
    }
    fn(fn2)

这有什么稀奇？无非就是一个函数作为参数或者返回值么？这就是高阶函数  
高阶函数是可以接受一个函数作为参数或者返回值的函数  
### 柯里化(curry)
又一个陌生的名词，再看看下面代码：

    function add(a ,b) {
        return a + b
    }
    add(1 ,2);                  //3


    function add(a){
        return function(a) {
            return a + b
        }
    }
    add(1)(2);                  //3
    ((add)(1))(2);              //3
    var add1 = add(1); add1(2); //3

同样实现两个数的加法，add2使用分步参数设定，先给第一个参数再给第二个参数。这种将接受多个参数的函数变成接受单一参数的函数就是柯里化。柯里化又称部分求值。  
柯里化就是将普通函数变成只接受一个参数，并且返回接受余下的参数而且返回结果的新函数的技术。  
### 偏函数

    function add(a ,b ,c ,d) {
        return a + b + c + d
    }
    var add12 = fn.bind(this ,1 ,2);
    add12(3 ,4);                        //10
    add12(5 ,6);                        //12

实现四个数加法，假设我们多次调用都用到相同的几个参数，那么就可以创立一个新函数固定参数，这里使用了Function.prototype.bind实现  
偏函数就是固定函数的某几个参数值。

#### 偏函数和柯里化的区别

* 偏函数应用是找一个函数，固定其中的几个参数值，从而得到一个新的函数。
* 函数柯里化是一种使用匿名单参数函数来实现多参数函数的方法。
* 函数柯里化化能够让你轻松的实现某些偏函数应用。
* 有些语言(例如 Haskell, OCaml)所有的多参函数都是在内部通过函数柯里化实现的。

### 反柯里化(uncurry)
这个并不是curry的反转版本，它会拆分第一个已得参数，形式为`uncurry(a,b...)==f(a)(b...)`，看代码

#### 普通做法

    var  arr = []
        ,push = Array.prototype.push
        ;
    push.call(arr ,'first');                    //['first']
    push.call(arr ,'second');                   //['first' ,'second']

#### 反柯里化

    Function.prototype.uncurry = function() {
        var  that = this;

        return function() {
            return Function.prototype.call.apply(that ,arguments)
        }
    }
    var  arr = []
        ,push = Array.prototype.push.uncurry()
        ;
    push(arr ,'first');                         //['first']
    push(arr ,'second');                        //['first' ,'second']


### 惰性求值（延迟求值）
这个似乎有点看懂意思了，介绍一个js里面惰性求值的简单例子

    function InfiniteArray() {
        var i = 0;
        this.next = function() {
            return i++;
        };
    }
    var a = new InfiniteArray();
    a.next();
    a.next();

上面代码定义了一个无穷数组，并且可以不断地取下一个值，这里我们只在取值时候才求值．  
惰性求值就是表达式不在它被绑定到变量之后就立即求值，而是在该值被取用的时候求值。
### 模式匹配

    function fib(n) {
        if(n == 0) return 1;
        if(n == 1) return 1;
        return fib(n - 2) + fib(n - 1);
    }

让我们用js衍生出的假想语言来支持模式匹配：

    function fib(0) {
        return 1;
    }
    function fib(1) {
        return 1;
    }
    function fib(n) {
        return fib(n - 2) + fib(n - 1);
    }

当然上面这段代码只是伪代码，模式匹配需要模拟  
模式匹配在js更多的是正则表达式，这里就不做进一步介绍了  
模式匹配就是依次检查每个模式，以确定输入数据是否与模式兼容，如果找到了匹配项，则执行结果表达式，否则测试下一个模式规则．
### 尾递归
[SO](http://stackoverflow.com/questions/33923/what-is-tail-recursion)有个精彩的回答，我改写成js如下:
#### 循环

    var result = 0;
    for(var i=1; i<=5; i++) {
        result += i;
    }
    console.log(result);

#### 递归

    function recsum(x) {
        if (x == 1) {
            return x
        }
        else {
            return x + recsum(x - 1)
        }
    }
    console.log(recsum(5));

    /*以下解释递归原理*
    recsum(5)
    5 + recsum(4)
    5 + (4 + recsum(3))
    5 + (4 + (3 + recsum(2)))
    5 + (4 + (3 + (2 + recsum(1))))
    5 + (4 + (3 + (2 + 1)))
    15
    /**/

#### 尾递归

    function tailrecsum(x, running_total) {
        running_total = running_total || 0;
        if (x == 0) {
            return running_total
        }
        else {
            return tailrecsum(x - 1, running_total + x)
        }
    }

    /*以下解释尾递归原理*
    tailrecsum(5, 0)
    tailrecsum(4, 5)
    tailrecsum(3, 9)
    tailrecsum(2, 12)
    tailrecsum(1, 14)
    tailrecsum(0, 15)
    15
    /**/

递归每次调用都需要保留上下文栈，代价比较大，但通过尾递归，我们可以在每次函数调用后释放上下文栈，避免栈溢出．  
尾递归就是指一个函数里的最后一个动作是该函数自身的调用。
### 闭包
这个，深入js骨子了了吧!

    function f1(name){
        var n=99;
        return function f2() {
            console.log(n);     //99
        }
    }
    var f2 = f1();
    f2();                       //99

闭包就是函数和相关引用环境组合的实体．这里f2引用了n，所以在调用f2时，n也同时可用．

## 优美的js

### 找出Tickets价格小于1000中，最高的价格

    var tickets = ["a" ,"b" ,"c"];
    function getPrice(ticket) {
        return {a: 1100 ,b: 900 ,c: 800}[ticket]
    }
    function less1000(price) {
        return price < 1000
    }
    function pickHighPriced(price1 ,price2) {
        return price1 > price2 ? price1 : price2
    }

#### 命令式

    var prices = [];
    for(var i=0,len=tickets.length; i<len; i++) {    //抓出所有价格
        prices.push(getPrice(tickets[i]))
    }
    for(var i=0,len=prices.length; i<len; i++) {    //去除大于1000的
        if(!less1000(prices[i])) {
            prices.splice(i ,1);
        }
    }
    var highestPrice = 0;
    for(var i=0,len=prices.length; i<len; i++) {    //找出最高价格
        highestPrice = pickHighPriced(highestPrice ,prices[i]);
    }

#### 函数式

    tickets                                         //["a" ,"b" ,"c"]
        .map(getPrice)                              //[1100 ,900 ,800]
        .filter(less1000)                           //[900 ,800]
        .reduce(pickHighPriced)                     //900

### dom选择器

#### 命令式

    var dom = document.getElementsByTagName("div");
    dom.style.width = "100px";
    dom.style.height = "100px";
    dom.innerHTML = "hello world!";

#### jquery函数式

    $("div")
        .width("100px")
        .height("100px")
        .html("hello world!")

## 为什么要了解函数型javascript?

* 我们希望用偏向于人的语言来编码．
* 我们希望关注结果不是过程．
* 我们希望适应并发时代．
* 我们更喜欢数学模型．
* 我们喜欢细分任务.
* 我们讨厌不可控．

## 参考资料

* [Functional Javascript](http://www.hunlock.com/blogs/Functional_Javascript)
* [js functional](https://www.ibm.com/developerworks/cn/web/1006_qiujt_jsfunctional/)
* [tail call optimization](http://www.paulbarry.com/articles/2009/08/30/tail-call-optimization)
* [Into the Land of Functional Programming with JavaScript](http://smthngsmwhr.wordpress.com/2012/10/22/higher-order-javascript-short-journey-into-the-land-of-functional-programming/#lazy_evaluation)
* [Functional Programming in JavaScript](http://dailyjs.com/2012/09/14/functional-programming/)
