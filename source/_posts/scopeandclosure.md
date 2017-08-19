title: 作用域和闭包--读《你不知道的JavaScript》
date: 2016-04-26 11:58:35
tags: JavaScript
---

本文是在看完《你不知道的JavaScript》这本书之后整理而成。
本文中所有的代码可以在[这里](https://github.com/buildAll/JavaScript_Scope-Closure)找到。如果你不想读下面的文字，可以直接clone代码并运行，然后结合代码和注释，试着去理解作用域和闭包的概念。

## 词法作用域
JavaScript采用词法作用域。所谓词法作用域就是代码书写完成之后，作用域随即确定，所写既所得。

看下面的代码;

```js
    var printSpace = require('./printspace');
    var showmore = true;

    var yoho = "yoho!我在全局作用域";

    function awfulSayYoho() {
      console.log("*****一般情况*****");
      console.log(yoho);
    }

    function normalSayYoho() {
      console.log("*****局部变量*****");

      var yoho = "yoho!我在函数作用域";
      console.log(yoho);
    }

    function advanceSayYoho() {
      // var yoho;   // B行, 自动提升声明
      console.log("*****自动提升*****");
      console.log(yoho); // C行，打印的是B行的声明，既undefined

      if (showmore) {
        var yoho = "yoho!我在函数作用域"; // A行，声明yoho，并为其赋值。
        // yoho = "yoho!我在函数作用域"; 执行语句不提升，留在原地

        function sayYohoAgain() {
          console.log(yoho);
        }

        sayYohoAgain();
      }
    }

    printSpace();

    awfulSayYoho();

    printSpace();

    normalSayYoho();

    printSpace();

    advanceSayYoho();

    printSpace();
```


上面这些代码的运行结果如下:
```shell
*****一般情况*****
yoho!我在全局作用域


*****局部变量*****
yoho!我在函数作用域


*****自动提升*****
undefined
yoho!我在函数作用域
```

上面这段平淡无奇的代码，相信大多数JSer理解起来并不难。值得一提的是声明的自动提升。在advanceSayYoho函数里的A行，声明了变量yoho并为其赋值，既看上去一行代码做了两件事:声明且赋值。但是，在函数被调用时，JS引擎总是将A行这样的"声明且赋值"的语句拆分成声明和赋值两步执行，既
```js
    var  yoho = "yoho!我在函数作用域";
```


被分成了：
```js
    var yoho;
    yoho = "yoho!我在函数作用域";
```

其中声明语句
```js
    var yoho;
```

这行被自动提升到函数体的顶部，既相当于自动提升到了advanceSayYoho函数的B行。

而赋值语句
```js
    yoho = "yoho!我在函数作用域";
```

不会提升，留在原地

由此便不难理解为什么C行打印的是undefined了。


## 原始数据类型和引用数据类型
在进一步讨论作用域及作用域对象的之前，先简单的说明一下JS里的“赋值”和"引用"。

JS里有两大类数据类型: 原始数据类型和引用数据类型（及对象数据类型）。下面的代码试图说明这两者的区别:

```
    var printSpace = require('./printspace');

    // 值传递
    console.log("*****值传递*****");

    var num = 10; // num 持有了数字10
    var a = num;  //   a 持有了数字10
    var b = num;  //   b 持有了数字10

    console.log("------初始-----");
    console.log(num);
    console.log(a);
    console.log(b);

    num = 8; // num 持有的数字变为了8

    console.log("------num值改变后-----");
    console.log(num);
    console.log(a);
    console.log(b);

    printSpace();

    // 对象的引用
    console.log("*****对象的引用*****");

    var yoho = {     // yoho 引用了对象 {value: “潮流”}
      value: "潮流"
    }

    var kris = yoho; // kris 引用了对象 {value: “潮流”}
    var me = yoho;   // me 引用了对象 {value: “潮流”}

    console.log("------初始----");
    console.log(yoho.value);
    console.log(kris.value);
    console.log(me.value);

    me.value = "还是潮流"; // A行， "被me 引用的对象".value 发生了

    console.log("------me.value改变后-----");
    console.log(yoho.value);
    console.log(kris.value);
    console.log(me.value);

```

运行node reference，可以看到:
```shell
*****值传递*****
------初始-----
10
10
10
------num值改变后-----
8
10
10


*****对象的引用*****
------初始----
潮流
潮流
潮流
------me.value改变后-----
还是潮流
还是潮流
还是潮流
```

可能有些新JSer不太能理解的是：上面代码的A行只修改了me.value的值，为什么yoho.value和kris.value也发生了变化？

其实上面代码中，yoho、me、kris这三个变量是等同的，他们都指向同一块内存空间，既对象 { value: “潮流” }所在的内存空间。也就是yoho、me、kris三个变量同时引用了对象{ value: “潮流” }。

所以me.value、kris.value、yoho.value都是同一块内存空间，而那块内存里的值一开始的值是“潮流”, 然后在A行，这个值被改为了"还是潮流"。

如果还是不能理解"引用"这个概念，建议可以去看看C语言里指针的概念。

## 变量的生命周期、作用域对象及作用域链

请看下面的代码：

```js
    // JS代码由此开始运行，创建了 globalScope = {}
    // 紧接着JS引擎对整个代码内容进行检查，搜索出全局变量，并将他们的声明设置为globalScope的属性
    // 既 globalScope = {
    //     yohobuy: yohobuy,
    //     PRINTSPACE: PRINTSPACE,
    //     sayYoho: sayYoho
    // }

    // globalScope.PRINTSPACE = require('./printspace');
    var PRINTSPACE = require('./printspace');

    // globalScope.yohobuy = "YOHO!buy";
    var yohobuy = "YOHO!buy";

    // globalScope.sayYoho = function() {......}
    function sayYoho() {
      // 函数开始执行，创建了scope = {}
      // 设置scope chain, 既 scope.parentScope = globalScope
      // 提升开始: scope.yoho = undefined;

      var yoho = "yoho!"; // scope.yoho = "yoho" ， 此时scope = {yoho: "yoho"}

      console.log(yoho);  // 打印的是scope.yoho

      console.log(yohobuy); //打印的是scope.parentScope.yohobuy, 既scope.globalScope.yohobuy
    }


    PRINTSPACE();

    sayYoho();
    // 函数调用完成，在函数调用过程中创建的scope对象此时没有被任何人引用，那么这个scope对象被自动回收

    PRINTSPACE();

    console.log(yoho); // 打印的是globalScope.yoho, 而globalScope上并没有定义yoho属性
```

运行结果如你所料:
```shell
yoho!
YOHO!buy

/Users/bill/Documents/Work/closure/lifecycle.js:36
console.log(yoho); // 打印的是globalScope.yoho, 而globalScope上并没有定义yoho属性

ReferenceError: yoho is not defined
    at Object.<anonymous> (/Users/bill/Documents/Work/closure/lifecycle.js:36:13)
    at Module._compile (module.js:413:34)
    at Object.Module._extensions..js (module.js:422:10)
    at Module.load (module.js:357:32)
    at Function.Module._load (module.js:314:12)
    at Function.Module.runMain (module.js:447:10)
    at startup (node.js:140:18)
    at node.js:1001:3
```

依然是一段有些无聊的代码，但愿你看到他们的时候不要打呵欠或是关掉这个网页:)))让我们来看看这里发生的一些有趣的事情:

1. JS代码只要被运行，便会在一开始的时候就创建一个全局的作用域对象，在这里为了方便起见，我把它称为globalScope。
2. globalScope对象创建完成后，立刻去全局范围内搜索全局变量，并将变量的名字设为自己属性名，同时引用相对应的变量。
3. sayYoho函数被调用时，创建了它自己的作用域对象，我把它称为scope。
4. scope对象会设置自己的父级作用域对象，既当前函数作用域的上一个作用域，在这里就是globalScope。
5. 紧接着, scope对象开始在函数作用域内部搜索局部变量，并将其设置为自己的属性。值得一提的是，这步只完成了属性名的设置，并未对属性赋值。(这一步既是上文提到的所谓变量的提升)
6. 函数运行结束后，如果(通常就是这样)scope对象没有被引用，则其会被JS引擎回收。*(反之，如果函数运行结束后，还有变量在引用scope对象，则该对象不会被回收)*

## 闭包
很高兴你能看到这里。我们终于来到了闭包的概念。可是别激动，我还是只能提供一段最稀松平常的闭包代码，你一定看过类似下面这样的代码：

```js
    // 此处有globalScope 对象； .... 不再赘述
    var printSpace = require('./printspace');

    function outerFunc() {
      // 函数被调用时，产生了新的outerScope对象, 既outerScope = {}
      //
      // 设置scope chain，既 outerScope.parentScope = globalScope;
      //
      // 提升开始： outerScope.counter = undefined;

      // outerScope.counter = 0;
      var counter = 0;

      return function() {
        // 函数被调用的话，产生新的insideScope = {}
        //
        // 设置scope chain，既 insideScope.parentScope = outerScope;
        //
        // 此时完整的scope chain 可以想成这样的 insideScope.parentScope.parentScope
        // 既 insideScope.outerScope.globalScope
        //
        // 函数内部没有 var 定义的变量，所以insideScope没有变化

        console.log(counter); // 此处需要打印的是 insideScope.outerScope.counter。既outerScope对象被引用了！！
        counter += 1;
      }
    }

    var insideFunc = outerFunc(); // 因为outerFunc的调用，内部函数被return出来
                                  // return 出来的内部函数被insideFunc引用
                                  // 由于insideFunc 引用的函数内部引用了 outerScope上的属性counter,
                                  // 所以outerScope对象不会被回收，从效果上来讲就是局部变量counter一直存活了

    /*
     * 不妨假想成下面的代码
     *
     * var insideFunc = function() {
     *   console.log(counter);
     *   counter += 1;
     * }
     *
     * 其中的counter变量来自outerFunc被调用时所创建的作用域对象
     *
     */
    printSpace();

    insideFunc();
    insideFunc();
    insideFunc();
    insideFunc();
    insideFunc();
    insideFunc();

    printSpace();

    var anotherInsideFunc = outerFunc(); //A行， outerFunc又被调用一次，产生了新的scope chain

    anotherInsideFunc();
    anotherInsideFunc();
    anotherInsideFunc();
    anotherInsideFunc();

    printSpace();
```

运行一下node closure，看到下面的结果：

```shell
0
1
2
3
4
5

0
1
2
3
```

从结果上来讲，这段代码只是达到了将counter这个一般情况下会随着函数运行结束而被销毁的局部变量保留住了的效果。到底是怎么做到这点的呢，简单来说，过程是这样的:
1. outerFunc被调用时创造了作用域对象，outerScope，以及由这个outerScope对象做为起点的一条作用域链
2. outerFunc运行到最后时return了一个匿名函数, 而这个匿名函数使用(既引用)了outerScope对象上的属性counter。
3. 此时如果没有其他变量引用return的匿名函数，那么将不会有特别的事情发生，outerScope对象会被回收。
4. 但是，被return的匿名函数又被innerFunc这个变量引用，所以为了保证其在随后的代码中能被顺利被执行，必须让innerFunc引用的函数内的所有变量都有意义，既counter必须被保留，所以相当于outerScope对象被引用了, 那么outerScope就不会被释放。
5. A行中，outerFunc又被调用了一次，则其又产生了一个新的outerScope对象，所以后面anotherInsideFunc执行时，counter又从0开始。


## 总结
为了理解闭包，需要想通下面几点:
1. 函数运行时会创建作用域对象及以此对象为起点的作用域链。
2. 借由函数一等公民的身份，JS函数内部的函数可以被传递给别的变量，既内部函数可以被引用。
3. 一旦内部函数被引用，并且该内部函数使用了自身作用域链上某个父级作用域对象上的属性，那么该父级作用域对象即使在外部函数运行结束后扔不会被回收。
4. 外部函数每被调用一次，都会创建新的作用域链。
5. 作用域链是树形的，下面这张简陋的图也许可以帮助你理解这一概念。

![scope chain 示意图](https://raw.githubusercontent.com/buildAll/JavaScript_Scope-Closure/master/scopechain.png)

要产生闭包，关键就是:
1. 至少有一个外部函数，外部函数内至少有一个内部函数。
2. 外部函数一定要运行至少一次，以此产生作用域对象并将内部函数传递给函数外部变量。

### 联系我
对本文有任何问题和建议可以在[这里](https://github.com/buildAll/JavaScript_Scope-Closure/issues/1)一起讨论。

### TL; DR;
按照《JavaScript权威指南》里的说法，闭包并不是JS特有的，它是一种计算机术语，在计算机科学中，将函数和作用域联系起来的这种机制就是闭包，所以理论上所有的JS函数都可以被认为是闭包。但是我们平时说的闭包，更多的是指的外部函数运行时产生的作用域对象被保留的现象。

《你不知道的JavaScript》这本书在github上有英文的[开源版本](https://github.com/getify/You-Dont-Know-JS)，任何人都可以为其贡献内容，该项目已经有接近30000star。目前其中的部分章节被翻译并出版成纸质书籍，感兴趣的话可以[去看看](http://www.amazon.cn/%E4%BD%A0%E4%B8%8D%E7%9F%A5%E9%81%93%E7%9A%84JavaScript-%E7%BE%8E-%E8%BE%9B%E6%99%AE%E6%A3%AE/dp/B0153179VI?ie=UTF8&keywords=%E4%BD%A0%E4%B8%8D%E7%9F%A5%E9%81%93%E7%9A%84JavaScript&qid=1461847068&ref_=sr_1_1&sr=8-1)。


**赵彪原创，转载请注明作者和出处**
