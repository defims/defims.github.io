# 函数型JS与面向对象JS的一些见解

## 大家都在讨论JS的面向对象模拟

### [ES5](http://www.w3.org/html/ig/zh/wiki/ES5)

    function Shape() {
        this.x = 0;
        this.y = 0;
    }
    Shape.prototype.move = function(x ,y) {
        this.x += x;
        this.y += y;
        console.log(this.x ,this.y);
    }
    function Rectangle() {
        Person.apply(this ,arguments);
    }
    Rectangle.prototype = Object.create(Person.prototype);//继承
    Rectangle.prototype.move = function(){//重载
        Shape.prototype.move.apply(this ,arguments);//super
        console.log("重载")；
    }

    var rect = new Rectangle;

    rect instanceof Rectangle //true
    rect instanceof Shape //true

    rect.move(1, 1); //打印 "1 1" "重载"

### [class.js](https://github.com/rauschma/class-js)

    var Shape = Class.extend({
        constructor: function () {
            this.x = 0;
            this.y = 0;
        }
        , move: function() {
            this.x += x;
            this.y += y;
            console.log(this.x ,this.y);
        }
    });
    var Rectangle = Person.extend({
        constructor: function () {
            Worker.super.constructor.apply(this, arguments);
        },
        move: function () {
            Worker.super.move.apply(this ,arguments)
            console.log("重载")；
        }
    });

    var rect = new Rectangle;

    rect instanceof Rectangle //true
    rect instanceof Shape //true

    rect.move(1, 1); //打印 "1 1" "重载"


### [coffeescript](http://coffeescript.org/)

    class Shape
        constructor: () ->
            @x = 0
            @y = 0
        move: (x ,y) ->
            @x += x
            @y += y
            console.log this.x ,this.y

    class Rectangle extends Shape
        move: ->
            super
            console.log "重载"

    rect = new Rectangle

    rect instanceof Rectangle #true
    rect instanceof Shape #true

    rect.move(1, 1); #打印 "重载"


### [ES6](http://wiki.ecmascript.org/doku.php?id=harmony:classes)

    class Shape {
        constructor(x, y) {
            this.x = x;
            this.y = y;
        }
        move() {
            console.log( this.x ,this.y );
        }
    }
    class Rectangle extends Shape {
        constructor() {
            super();
        }
        move() {
            super();
            console.log("重载");
        }
    }

    var rect = new Rectangle;
    rect.move(1, 1); //打印 "重载"

## 我们再看下JS的函数型特性

### 无副作用

#### 副作用简单来说就是状态的修改

记得刚接触c的变量时候的困惑吗？为什么`a = a + 1;`直到我们了解了计算机原理才明白，并且接受。当程序变得复杂，调试就变得复杂，我们必须断点知道当某条语句运行时，各个变量值是什么。

    var result = 0;
    function add(a ,b) {
        result = a + b;
    }
    add();

add函数依赖result，并且会修改result的值。当result被修改过，那add()的结果就不可知了。

#### 无副作用让状态稳定

当我确定知道一个函数相同的输入必然有相同输出时候，这个函数对于我就是个稳定的黑盒。稳定的程序是程序员的爱人。

    function add(a ,b) {
        return a + b;
    }
    var result = add(1 ,1);

我确定知道add是一个将两数相加的函数，并且不管什么时候什么环境下执行，add(1 ,1)结果都是2。于是我们不需要知道add怎么做到将俩数相加了。

#### 无副作用是模块化的好基友

    ;(function(){//module
    })();

记得这样的代码吗？一个立即执行函数用来包裹一个模块所需的内部变量，防止外部串改。

    var a = 1;
    ;(function(){//module
        console.log(a);
        a += 1;
        console.log(a);
    })();

假如模块使用了外部变量，这就会带来副作用。如果其他地方改变了a怎么办？

    ;(function(a){//module
        function add(a) {
            return a + 1;
        }
        console.log(add(a));
    })(1);

等效代码，但这个模块非常稳定。

### 并行

script a:

    var a = 1;

script b:

    var a = 2;

script c:

    console.log(a);

a->b->c //2
b->a->c //1
a->c->b //1
c->b->a //error
c->a->b //error
b->c->a //2

不同的执行顺序有不同的结果，这段代码非常依赖执行顺序。

script a:

    function a1() {
        return 1;
    }

script b:

    function a2() {
        return 2;
    }

script c:

    console.log(a1() ,a2());

并行执行a，b，得到一个map结果，然后在c进行reduce，这也是Google的mapreduce并行计算的最基本思想。

### 数学

### jQuery

## 思想

### 一切皆对象

### 函数第一性

## 适用

### 面向对象适合GUI，访问数据库

### 函数型适合计算

## 参考

* [函数型 VS 面向对象型的javascript程序设计](http://wenku.baidu.com/view/ed1660d726fff705cc170a3f.html)
* [why functional programming](https://www.byvoid.com/blog/why-functional-programming)
* [为什么函数式编程没有](http://www.infoq.com/cn/news/2009/03/fp-doesnt-catchon)
* [javascript mapreduce](http://jcla1.com/blog/javascript-mapreduce/)
* [javascript mapreduce](http://www.diguage.com/archives/75.html)
