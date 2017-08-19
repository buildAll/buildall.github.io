title: 用代码说明JavaScript里的继承
date: 2015-11-06 10:45:42
categories: JavaScript
tags: JS继承
---

JavaScript是通过prototype来实现继承的。也就是人们常说的"原型继承"。

本文的目的就是用代码来说明，到底什么是原型继承。

## 原型是什么

JavaScript里有两个“原型”:
```
  1.prototype
  2.__proto__
```

prototype，是JavaScript里由关键字"function"定义的对象的一个特有的属性，这个属性本身又是一个对象。可能大多数不熟悉原型概念的人看到这句话会有点晕，OK，让我们打开控制台，敲几行代码感受下吧。打开你的chrome浏览器，按F12或者command+option+i，并选中控制台(console)，输入以下代码(注：控制台中可以按ctrl+enter来换行):

```js
    var IamFunction = function(){};//定义一个函数
    console.log(IamFunction.prototype);//打印结果为IamFunction {}

    var IamObject = {};//定义一个对象
    console.log(IamObject.prototype);//打印结果为undefined

    var IamNumber = 0;//定义一个数字
    console.log(IamNumber.prototype);//打印结果为undefined

    var IamString = '';//定义一个字符串
    console.log(IamString.prototype);//打印结果为undefined

    var IamArray = [];//定义一个数组
    console.log(IamArray.prototype);//打印结果为undefined
```

由上面的代码可以看出，所谓prototype这个东西，只存在于某个function。(这里并没有检查JavaScript中所有的数据类型上prototype属性，但结果是一样的，你可以自己试一下，只有function类型会有prototype属性。)在上面的例子中，打印结果"IamFunction {}"就表示，prototype就是IamFunction的一个属性，它的默认值是{}。

除了Function构造的对象上prototype属性，JavaScript里还有一个地方有“原型”，既由构造函数创建的对象的{% raw %}__proto__{% endraw %}属性。关于这个{% raw %}__proto__{% endraw %}属性，请看下面的内容吧。


## 函数的调用方式

JS里的函数有两种调用方法:普通调用和作为构造函数调用。既：

```js
     function Func(){}      //定义一个函数Func
     Func();                //普通调用， 没有关键字new
     var obj = new Func();  //当作构造函数调用，有关键字new
```

可以看到两种调用方法字面上的差异是有无关键字new。下面看看两种调用方式的具体区别。

首先我们定义一个函数Animal。

```
     //定义函数Animal
     function Animal(animalName){
        console.log('function is called');

        console.log(this);
        console.log(this.eat);
        this.name = animalName;
        console.log(this.name);

        console.log('function is going to return');
     }
```

像上文说的，所有的function变量都有prototype属性，所以我们可以这样给prototype属性添加方法：

```js
     //给函数Animal的prototype属性添加eat方法
     Animal.prototype.eat = function(){
        console.log('eating');
     };
```


先来看普通调用的情况:
```js
     //以下是A行
     var dog = Animal('dog');  //调用函数Animal，未使用new  ******* A ******
                               //上面这行代码的意思可以理解为：调用Animal函数，并把返回值赋给变量dog;


     console.log(dog);         //打印结果为undefined
     /*******
     * 对于上面这行打印结果的解释:变量dog的值为undefined是因为函数Animal的定义的最后并没有写上return，既函数本身没有任何返回值，既undefined
     ******/

     console.log(window.name); //打印结果为dog。对于这个打印的解释请往下看。
```


让我们看看在上面A行调用过程中，到底发生了什么。

```js
     function Animal(animalName){

        console.log('function is called');

        console.log(this);  //打印结果为Window {external: Object, chrome: Object, document: document, dog: undefined, speechSynthesis: SpeechSynthesis…}
        /******
        *  对于以上面这行打印结果的解释： 这里打印的是对象window。window对象是由浏览器用类似下面的代码自动创建的
        *  var window = new Window();
        *
        *  因为A行是直接在全局作用域调、既window对象上调用了Animal('dog')，所以this引用的是window对象。
        *
        *  既JavaScript内部执行了类似这样的代码:
        *  this = window;
        *
        ******/

        console.log(this.eat);   //打印结果为undefined。这是由于this引用了window，而window本身并没有eat方法。

        this.name = animalName;
        console.log(this.name);  //打印结果为dog。因为上面一行将函数参数animalName的值，也就是'dog'赋给this.name，所以这里打印出'dog'。
                                 //再次注意这里的this已经引用了对象window，所以给this添加name属性既给window添加了name属性。

        console.log('function is going to return');
        // 函数体的最后并没有显性的写上return;
     }
```



再看看使用new关键字调用Animal时会发生什么：

```js
       //以下是B行
       var dog = new Animal('dog'); //在Animal的调用前使用了关键字new，则此时Animal函数成为了构造函数。  ********* B *********

       console.log(dog);            //打印结果为 Animal {name: "dog"}
       console.log(window.name)     //打印结果为undefined;

```
B行在函数Animal的调用Animal()前加上了关键字new，像这样在函数调用前加上new，此时函数就被当作构造函数使用了。这里的"构造"过程如下:

1.在JavaScript内部，new会创建一个对象{}。你可以想象成JavaScript内部执行了这样一行代码:

```js
            var newObject = {};
```

2.这个对象的{%raw%}__proto__{%endraw%}属性随即引用了Animal的prototype属性。你可以想像成JavaScript内部执行了这样的代码:
```js
           Object.setPrototypeOf(newObject, Animal.prototype);  //设置newObject的__proto__属性

           // 上面一行也可以写成newObject.__proto__ = Animal.prototype。
           // __proto__是JavaScript对象里特有的属性，它引用构造函数的prototype属性。
           // 你可以这样访问__proto__属性，Object.getPrototypeOf(newObject)，或者newObject.__proto__;
```

3.随即发生了类似这样的操作:

```js
           Animal.call(newObject, 'dog'); //这里相当于使用了函数的call方法改变了函数内部this的指向。
```


4.最后，这个由new创建的对象会被函数返回，既
```js
            function Animal('dog'){

              newObject.name = 'dog';
              return newObject;        //相当于JavaScript内部自动给我们的函数定义添加了一句return this;注意，只有当有new关键字出现时，才会自动添加这句。

            }
```


完整的看一下B行调用Animal的过程中，发生了什么：
```js
     function Animal(animalName){

        console.log('function is called');

        console.log(this);  //打印结果为 > Animal {}
        /******
        * 调用对象被new关键字设置为了newObject，所以这里打印的是{}, 前面的"Animal" 是告诉你这个{}的构造函数为Animal
        * 而由于作用域中有 this.name = animalName这行代码，所以如果你点击该行打印结果前面的箭头展开这个对象，你会看到这个{}已经具备了name属性。关于作用域，这里就不展开说了。
        ******/

        console.log(this.eat);   //打印结果为function(){console.log('eating')}
        /*****
        * 上文中的构造过程第2步中已经说明newObject的__proto__属性引用了Animal.prototype属性。
        * 这里的eat方法正是来自于__proto__属性。而JavaScript中规定，只要是__proto__上的属性，都可以省略__proto__，直接通过newObject.eat来访问。
        ***/

        this.name = animalName;
        console.log(this.name);  //打印结果为dog。


        console.log('function is going to return');
        // 函数体的最后并没有显性的写上return;
        // 但是由于用new关键字调用了函数Animal，所以这里相当于执行了 return this;
     }
```

## 典型的继承写法

先总结一下上文出现的概念:

```
1.JavaScript里的函数都有prototype属性。

2.对象都有__proto__属性。

3.对象的__proto__属性引用创建其的构造函数的prototype属性。
```

如果理解了上面的这三条概念，那么就不难理解继承了。我们来看JavaScript里典型的继承写法:
```js
        function Animal(animalName){        //定义函数Animal
           this.name = animalName;
        }


        Animal.prototype.eat = function(){  //给Animal的prototype属性添加eat方法
           console.log('eating');
        }


        function Dog(){}                    //定义函数Dog
        Dog.prototype = new Animal('dog');  //将函数Dog的prototype属性引用为函数Animal"构造"的对象，这步是重点，继承在这里发生了。

        console.log(Dog.prototype);         //打印结果为 Animal {name:'dog'}
        console.log(Dog.prototype.name);    //打印结果为'dog'
        console.log(Dog.prototype.eat());   //打印结果为'eating'

        console.log(Dog.prototype.hasOwnPropety('name'))          // 打印 true
        console.log(Dog.prototype.hasOwnPropety('eat'))           // 打印 false。上面虽然调用eat方法成功了，但是令人意外的是Dog.prototype本身并没有eat方法。
        console.log(Dog.prototype.__proto__.hasOwnPropety('eat')) // 打印 true
```


## 原型链
```js
                    function Animal(){}
                    Animal.prototype.eat = function(){console.log('I can eat');};       //给Animal的原型添加eat方法

                    function Human(){}
                    Human.prototype = new Animal();
                    //这行代码可以拆成两行理解，
                    //var animal = new Animal; 此时animal.__proto__引用自Animal.prototype，既animal.__proto__.hasOwnPropety('eat')返回true
                    //Human.prototype = animal; 此时Human.prototype.__proto__.hasOwnPropety('eat')返回true
                    //则有 Human.prototype.__proto__.eat(); 打印 I can eat
                    //而JS中可以省略__proto__直接用"."去访问__proto__上的属性，所以这时候可以像这样调用eat方法：
                    //Human.prototype.eat(); 打印 I can eat

                    //所以应该可以理解下面的代码了:
                    //var someone = new Human();
                    //上面的new构造过程包括了这样的操作: someone.__proto__ = Human.prototype;
                    //既someone.__proto__ = animal;
                    //则有:
                    //someone.__proto__.__proto__.eat(); 打印 I can eat。
                    //省略__proto__后
                    //someone.eat();  I can eat
                    //既someone对象继承了Animal上的eat方法。

                    Human.prototype.speak = function(){console.log('I can speak');};    //此时Human.prototype这个由Animal构造的对象拥有了speak方法

                    function Coder(){}
                    Coder.prototype = new Human();
                    Coder.prototype.coding = function(){console.log('I can coding');};

                    function JSer(){}
                    JSer.prototype = new Coder();
                    JSer.prototype.codingInJS = function(){console.log('I can conding in JS');};


                    var me = new JSer();

                    //原型链来了：
                    console.log(me)                                                              //打印结果为JSer {}
                    console.log(me.__proto__);                                                   //打印结果为Coder {}
                    console.log(me.__proto__.__proto__);                                         //打印结果为Human {}
                    console.log(me.__proto__.__proto__.__proto__);                               //打印结果为Animal {}, 拥有speak方法的Animal构造出的对象
                    console.log(me.__proto__.__proto__.__proto__.__proto__);                     //打印结果为Animal {}, 没有speak方法
                    console.log(me.__proto__.__proto__.__proto__.__proto__.__proto__);           //打印结果为Object {}
                    console.log(me.__proto__.__proto__.__proto__.__proto__.__proto__.__proto__); //打印结果为null，此时原型链到达尽头

                    //以下调用均省略了__proto__属性
                    console.log(me.eat());                                              //打印I can eat 。eat继承自构造函数Animal
                    console.log(me.speak());                                            //打印I can speak。speak方法继承自构造函数Human
                    console.log(me.coding());                                           //打印I can coding。 coding方法继承自构造函数Coder
                    console.log(me.codingInJS());                                       //打印I can coding in JS。 codingInJS方法继承自构造函数JSer
```

## 总结

1. JavaScript通过设置构造函数的prototype对象，从而决定了通过new构造出来的对象的{%raw%}__proto__{%endraw%}属性。
2. 因为每个对象都具备{%raw%}__proto__{%endraw%}属性，从而实现了一条原型链。
3. 又因为JavaScript可以省略{%raw%}__proto__{%endraw%}去调用{%raw%}__proto__{%endraw%}属性上的方法，所以我们就可以轻松的访问整条原型链上的属性了。


_赵彪原创，转载请注明出处_
