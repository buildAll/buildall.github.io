title: 现在开始用vim开发Web前端吧
subtitle: web前端工程师vim入门指南
date: 2015-12-09 10:39:43
categories: 开发工具
tags: vim, front-end tool
---

如果你是一名web前端工程师并且没有vim使用经验的话，那么这篇博文就是为你准备的。

## vim是什么

vim是一款文本编辑器应用程序。和一般的编辑器程序不同，vim有以下特点:

1. 运行在终端,而非桌面
2. 不需要使用鼠标进行操作，所有操作都通过键盘实现
3. Linux系统默认安装这款编辑器

## 为什么用vim

前端最流行的编辑器应该是sublime text和web storm，这两个应用程序确实也非常好用，能有效的提高前端的开发效率。那为什么我们还要使用vim来做前端开发呢，原因如下:

1. vim的命令系统使得开发者可以在编码过程中无需操作鼠标，两只手可以全程专注在敲击键盘上，这样无论对于提升开发效率还是开发者编码的体验都会有帮助。
2. vim界面十分简洁，并且能够自由、灵活的打开、切换多窗口。
3. vim和sublime一样，有着强大的插件系统，类似zen coding，多行编辑，语法检查等功能都可以在vim上实现
4. Web前端开发人员难免需要上服务器查看或修改代码，而所有的Linux服务器上都自带vim，此时如果这名前端会使用vim的话必然对工作有很大帮助。

## vim相关的概念

### 命令
vim中的操作都是通过命令来实现的，比如移动光标、复制、粘贴等，都有对应的命令。
下文会介绍一些常用的命令。

### 模式
vim的部分命令有对应的模式，如果要使用这些命令，首先要进入命令所属的模式。

各个模式的关系如下:

<!--<img src="images/frontendvim/vim-mode.jpg" alt="vim中的模式">-->

{% limg frontendvim/vim-mode.jpg  %}

对上图的解释

1. 默认模式处于最顶层，在任意模式下，按ESC进入默认模式
2. VISUAL, VISUAL BLOCK和命令行模式之间可以通过命令互相切换


## 安装vim

心动不如行动，先安装vim吧。对于安装，网上有很多教程，如：

[Windows中的vim安装教程](http://linux.chinaunix.net/techdoc/install/2009/04/20/1108200.shtml)

[Linux中的vim安装教程](http://www.cnblogs.com/lwbqqyumidi/archive/2012/08/22/2651337.html)

## vim插件管理工具

上面提到vim有很多插件，所以我们需要一个插件管理工具，我平时使用的是[vim-pathogen](https://github.com/tpope/vim-pathogen), 用了这个工具后，就可以轻松的安装各种插件了。

pathogen安装插件的通用步骤:
```shell
cd ~/.vim/bundle/
sudo git clone <所要安装插件的github代码库url>
```


删除插件的方法:
```shell
cd ~/.vim/bundle/
sudo rm -rf <需要删除的插件文件夹>
或
sudo rm <需要删除的插件文件名>
```

就是这么简单。


## vim配置文件.vimrc
vim本身有一些配置项目，这些配置都写在~/.vimrc这个文件里，通过<span style="background-color:#d0d0d0">vim ~/.vimrc</span>可以查看或者编辑这个文件。

## 前端常用插件

以下是我平时前端开发中用到的插件：
[NerdTree](https://github.com/scrooloose/nerdtree)。必装插件，实现树状文件查找。
[YouCompleteMe](https://github.com/Valloric/YouCompleteMe)。必备插件，代码自动补齐。
[emmet-vim](https://github.com/mattn/emmet-vim)。必备插件，zen-coding。
[vim-multiple-cursors](https://github.com/terryma/vim-multiple-cursors)。同时多行编辑。
[indentLine](https://github.com/Yggdroot/indentLine)。显示代码缩进。
[syntastic](https://github.com/scrooloose/syntastic)。语法检查。
[javascript-libraries-syntax.vim](https://github.com/othree/javascript-libraries-syntax.vim)。JS代码高亮插件。
[vim-javascript-syntax](https://github.com/jelera/vim-javascript-syntax)。代码折叠。
[tern_for_vim](https://github.com/ternjs/tern_for_vim)。快速跳转到变量/函数定义的地方。
[JavaScript-Indent](https://github.com/vim-scripts/JavaScript-Indent)。代码缩进。

## 开始使用vim

在命令行中通过输入<span style="background-color:#d0d0d0">vim  <文件名> </span>开始编辑文件。 如要打开index.html，则输入 <span style="background-color:#d0d0d0">vim index.html </span>。

vim打开文件后，不要急着按键，或者试图通过鼠标来做任何操作，所有的操作都是通过键盘输入来实现的。

### _以下所有命令如无特殊说明，都是按顺序输入， 如要输入命令 ctrl w l, 则先按ctrl, 然后松开ctrl，再按w，再松开w，再按l，再松开l。其中任何一个按键都不可以按住不放，整个输入过程自然连贯即可。_
### _vim中的命令都是大小写敏感的，所以需要注意各个命令的大小写。_



### 默认模式
vim打开文件后默认直接进入该模式。

进入默认模式的命令: ESC


#### 移动光标
vim中光标的的移动是通过按键实现的。

#### 一般的移动：
```shell
上移光标 k
下移光标 j
左移光标 h
右移光标 l
```

#### 快速移动：
```shell
直接移动到当前所在行的头部 0 (数字零)
直接移动到当前所在行的尾部 $
光标跳转到目标行 数字 gg  ，如跳到第74行,则输入 74gg
光标跳转到文件头部 gg
光标跳转到文件尾部 G
跳转到上一个编辑处 ctrl o
跳转到下一个编辑处 ctrl i

跳转到行内某个字母(行内查找)  f 所要查找的内容  ，如跳到当前光标所在行的a字母所在处: f a
然后按;(分号)跳转到下一个a，按,(逗号)跳转到上一个a

跳到下一个单词的开头 w
跳到前一个单词的开头 b
```

#### 窗口大小调整命令(只在打开多个窗口时生效)

```shell
增加窗口宽度 ctrl w > 或者ctrl w 数字 > 。加入数字后可以快速增加宽度
减小窗口宽度 ctrl w < 或者ctrl w 数字 < 。加入数字后可以快速减小宽度
增加窗口高度 ctrl w + 或者ctrl w 数字 + 。加入数字后可以快速增加高度
减小窗口高度 ctrl w - 或者ctrl w 数字 - 。加入数字后可以快速增加高度

跳转到上窗口 ctrl w k
跳转到下窗口 ctrl w j
跳转到左窗口 ctrl w h
跳转到右窗口 ctrl w l
```

#### 普通命令

```shell
查找 / 需要查找的内容 回车
例如要查找单词hello，则输入/hello ,再按回车键

查找当前光标所在位置的单词 gd

在查找到的结果中快速移动光标:

上移 n
下移 N

取消查找内容高亮 :noh

保存 :w
退出 :q
保存并退出 :wq
强制退出 :q!

复制当前光标所在行 yy
剪切当前光标所在行 dd
剪切多行 数字 dd （数字为需要剪切的行数）
复制当前光标选中的字符 y
剪切当前光标选中的字符 x
剪并进入INSERT模式 s

粘贴 p
undo u
re-undo ctrl r

常规删除 "_d
常规复制 "+y
常规粘贴 command v(MAC OS) 或者 ctrl v(Windows系统)

替换光标当前位置字符 r 然后输入要替换的字母。 例如，要替换"hello world"中的d为o，则先将光标移动到d，然后按r，然后再o
```

需要解释的是:

1. yy和y复制是指的yank，yank所复制的内容只能通过p粘贴，而上面的常规复制则可以将vim里的内容复制并通过ctrl/command v 复制到别处。
2. vim里除了上面的常规删除以外并没有原生的快速删除的命令(不过，你可以自定义命令)，像x,dd等命令都是自带了yank。如你想删除"hello world"里的d，光标移到d，再按x，然后你再按p的话就粘贴了d。
3. 常规删除的内容不可以通过p粘贴，只能通过常规粘贴命令进行粘贴。



### INSERT模式
以下任意命令都可以进入编辑模式。

```shell
在光标当前位置输入 i
在光标当前位置的下一格输入 a
在光标当前位置所在行的开头开始输入 o
在光标当前位置所在行的下一行开始输入 o
在光标当前位置所在行的上一行开始输入 O
```

进入编辑模式后，vim左下角出现 -- INSERT --，此时可以像在普通编辑器里一样开始写代码了。因为键盘输入直接被写在了当前的文件里，所以vim命令此刻都“失效”了。

退出编辑模式的命令 ESC


### VISUAL模式
进入该模式的命令 v
退出该模式的命令 ESC
进入编辑模式后，vim左下角出现 -- VISUAL --，一般在需要选择文本的时候进入这个模式。

在VISUAL模式下移动光标的命令和普通模式下是一样的，区别是光标移动时会选中移动路径上的文本。


选中后按y复制，按x或者d剪切，

VISUAL模式下一次性替换整块文本:

1. 选中并yank复制(按键y)马上要用来替换的内容
2. 移动光标选中需要被替换的文本
3. ctrl r
4. p



### VISUAL BLOCK模式
进入该模式的命令 ctrl v
退出该模式的命令 ESC

该模式常常用于快速注释代码，步骤如下
```shell
1. 光标移动到需要注释的代码的第一行的开头
2. ctrl v
3. j命令下移至需要注释代码的最后一行
4. I
5. 输入//
6. ESC
```

注意，第六步需要过n秒才会生效

快速缩进多行代码
```shell
1. 光标移动到需要缩进的代码的第一行的开头
2. ctrl v
3. j命令下移至需要缩进代码的最后一行
4. I
5. 按空格缩进代码
6. ESC
```

注意，第六步需要过n秒才会生效


### 命令行模式
进入该模式的命令  :(冒号)
退出该模式的命令 ESC

该模式下常用的命令:
```
在竖直方向打开新窗口 vsp <所要打开的文件路径>
在水平方向打开新窗口 sp  <所要打开的文件路径>
```


## NerdTree
NerdTree的使用小窍门:

1. o命令打开文件后可以 ctrl o回到NerdTree
2. t命令打开新tab后，可以按gt切换到下一个tab，按gT切换到前一个tab
3. VISUAL模式下按:(冒号)vsp 可以打开一个较窄的窗口，这样大小的窗口用来显示NerdTree比较合适。


## 总结
vim作为一款经典的老派编辑器能够存在这么多年，一定有它存在的理由。刚接触时可能会有一些不适应，但是如果能够坚持使用，其实并不需要多久，就能快速高效的使用vim编程了。希望本文能带你进入vim的世界。

关于vim，如果你有任何问题可以来[这里](https://github.com/buildAll/buildall.github.io/issues/1)一起讨论。

_赵彪原创，转载请注明出处_

<script>
    function removeImgBorder() {
        var imgs = document.getElementsByTagName('img');
        [].forEach.call(imgs, function(img, idx) {
            img.parentNode.style.borderBottom = 'none';
        });
    }
    window.onload = removeImgBorder;
</script>






