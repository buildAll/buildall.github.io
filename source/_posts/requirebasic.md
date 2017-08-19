title: 快速上手require.js
date: 2015-11-23 16:07:19
categories: 开发工具
tags: requirejs
---

## requirejs是什么
requriejs是一个js插件，这个插件可以在各种js运行环境里自动、异步加载js文件。
更多内容参考:
[requirejs官网](http://requirejs.org/)
[requirejs的Git仓库](https://github.com/jrburke/requirejs)

## 关于本文的示例代码
本文中的所有代码都可以在[这里](https://github.com/buildAll/dead-basic-requirejs-demo)看到或下载。

## 为什么使用requirejs
前端js代码越来越多，如果使用模块化开发，可以降低代码藕合，提高代码的可维护性。而requirejs可以比较好的支持的js模块化开发思想。
不过requirejs并不是唯一支持js模块开发的插件，类似功能的插件还有[browserify](http://browserify.org/)等。

## 如何使用requirejs

### 下载requirejs
打开[链接](http://requirejs.org/docs/release/2.1.22/comments/require.js)，复制源码到你的文件里，保存为require.js。

### 建立项目
建立如下的目录结构:

```shell
app/
  index.html
  js/
    lib/
       require.js   /*require.js源码*/
       print.js     /*自定义的(库)模块*/
    app/
       app.js       /*放置应用main方法的文件，名字可以任意起*/
       me.js        /*自定义的(应用级)模块，名字可以任意起*/
    main.js         /*调用require方法的文件，既入口文件，名字可以任意起*/

```

### 为页面引入requirejs
index.html代码如下:

```html
                <!DOCTYPE html>
                <html lang="en">
                <head>
                  <meta charset="UTF-8">
                  <title>requirejs basic </title>
                </head>
                <body>
                  <!--
                  src属性指向require.js
                  data-main属性指向main.js，注意1：可以省略.js，注意2:该文件既为调用require方法的文件
                  -->
                  <script src='js/lib/require.js' data-main='js/main'></script>
                </body>
                </html>
```

这里的script里的写法是固定的，既设置src和data-main属性，分别找到require.js的源码和我们自己页面调用require方法的入口文件。

### 配置及启动requirejs
打开main.js，输入以下代码:
```js
                require.config({
                  //baseUrl为requirejs需要加载的模块的根目录，后面的模块路径都是相对于这个目录的
                  baseUrl: './js',
                  paths: {
                  //模块名: .js文件相对于baseUrl的路径
                    me: 'app/me',
                    print: 'lib/print',
                    myApp: 'app/app'
                  }
                })

                //启动myApp模块，既运行js/app/app.js
                require(['myApp']);
```

可以看到，首先调用了require.config()去进行一些配置，这里主要配置了baseUrl和paths。
baseUrl可以看成所有模块的根目录。
paths中定义了各个模块(既各个.js文件)相对于baseUrl的路径。paths对象的格式是{模块名: .js文件相对于baseUrl的路径}。

其实可以认为所谓的模块化就是在这里发生的。既paths里通过模块名，关联到了对应的js文件。通过这种形式将js文件"设定"成了模块。

### 定义模块
看一下requirejs里怎么定义模块。打开me.js，定一个me模块：
```js
                define(function(){
                   var myName = 'Bill';
                   var myJob = 'front-end developer';
                   var mySalary = 'what?';
                   var myTarget = 'become a successful developer';

                   return {
                     getName: function() {
                       return myName;
                     },
                     getJob: function() {
                       return myJob;
                     },
                     getSalary: function() {
                       return 'salary is confidential';
                     },
                     getTarget: function() {
                       return myTarget;
                     }
                   }
                });
```

就是这么简单，调用define()方法就可以定义模块了(实际上define的英文意思就是“定义“)。向该方法传入一个匿名函数，然后在函数最后返回模块的公共接口。

同样的，我们再定一个print模块。打开print.js，定义一个模块print。输入以下代码：
```js
                define(function() {
                  return function print(msg) {
                    window.console.log && console.log(msg);
                  };
                });
```


### 使用模块
模块定义好了，怎么在其他模块中使用呢?

打开app.js，输入
```js
                //['me','print']对应require.config()方法传入参数的me和print属性。
                define(['me', 'print'], function(me, print) {
                  print(me.getTarget());
                });
```

可以看到，这个名为app的模块和之前的模块不同，这个模块在调用define方法时传入了两个参数:

* 第一个参数是一个数组，在这里是['me', 'print']。其中'me'，'print'是require.config()传入的参数中定义的模块名。
* 第二个参数是一个匿名函数，这个函数的参数和前面数组里的模块名一一对应。

### 测试代码
现在，用浏览器打开index.html，调出控制台面板(F12或者option+command+i)，就可以在控制台里看到'become a successful developer'这句话了。

## 总结
使用requirejs的步骤:
1. __在页面中的script标签里设置src属性和data-main属性。其中src对应require.js的路径，data-main属性对应入口文件路径，既本文示例中的main.js文件路径。__
2. __在main.js中通过require.confing()配置模块名和模块对应的.js文件路径。__
3. __在main.js中通过require()方法，调用页面的主函数(主模块)，在本文的示例代码中是名为myApp的模块(对应app.js文件)。__
3. __通过define()方法定义模块。__

可以看出requirejs的核心思想，就是把一个个单独的js文件“设定成”模块。然后各个模块间再通过向define()传入模块名，来互相引用。

本文只介绍了requirejs的一般使用方法，按照本文的做法已经可以在项目中去使用requirejs了。更详细的内容还请参阅[requirejs官方网站](http://requirejs.org/)。

_赵彪原创，转载请注明出处_


