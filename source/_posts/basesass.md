title: 用于生成基础样式的base.sass文件
date: 2015-11-16 23:38:02
categories: CSS
tags: sass
---

我写了一个base.sass文件用于生成项目中的base.css。你可以在这里看到源码

[https://github.com/buildAll/base.sass-for-base.css/blob/master/base.sass](https://github.com/buildAll/base.sass-for-base.css/blob/master/base.sass)


下面贴出base.sass的代码:

```css
/*
 * @about: base.sass is used to output the common and basic style
 * @author: bill zhao
 * @repo: https://github.com/buildAll/sass_for_base.css
 * @desciption: You can use this code for free, please keep this comments in the file head
 */


body, div,dl,dt,dd,ul,ol,li,h1,h2,h3,h4,h5,h6,pre,form,fieldset,input,textarea,p,blockquote,th,td
  margin: 0
  padding: 0

table
  border-collapse: collapse
  border-spacing: 0

fieldset,img
  border: 0

address,caption, cite,code,dfn,em,strong,th,var
  font-style: normal
  font-weight: normal

ol,ul
  list-style: none

capation,th
  text-align: left

h1,h2,h3,h4,h5,h6
  font-size: 100%
  font-weight: normal

q:before, q:after
  content: ' '

abbr,acronym
  border: 0


@each $size in 12 13 14 15 16 20 25 30
  .f#{$size}
    font-size: #{$size}px

.fb
  font-weight: bold

.fn
  font-weight: normal

@each $indent in 2 3 4 5
  .t#{$indent}
    text-indent: #{$indent}em

@each $lh, $var in (lh150:150%, lh180:180%,lh200:200%)
  .#{$lh}
    line-height: #{$var}

.unl
  text-decoration: underline

.no_unl
  text-decoration: none

@each $ta, $var in (tl:left, tc:center, tr:right)
  .#{$ta}
    text-align: #{$var}

.bc
  margin-left: auto
  margin-right: auto

@each $floatname, $var in (fl:left, fr:right)
  .#{$floatname}
    float: $var
    display: inline

@each $clearname, $var in (cb:both, cl:left, cr:right)
  .#{$clearname}
    clear: $var

.clearfix:after
  content: '.'
  display: block
  height: 0
  clear: both
  visibility: hidden


.clearfix
  display: inline-block

html .clearfix
  height: 1%

.vm
  vertical-align: center

.pr
  position: relative

.abs-right
  position: absolute
  right: 0

.zoom
  zoom: 1

.hidden
  visibility: hidden

.none
  display: none

@each $width in 10 20 30 40 50 60 70 80 90 100 200 300 400 500 600 700 800
  .w#{$width}
    width: #{$width}px

.w
  width: 100%

@each $height in 50 80 100 200
  .h#{$height}
    height: #{$height}px

.h
  height: 100%

$list: 5 10 15 20 25 30 50 100

@each $m in $list
  .m#{$m}
    margin: #{$m}px

@each $mt in $list
  .mt#{$mt}
    margin-top: #{$mt}px

@each $mb in $list
  .mb#{$mb}
    margin-bottom: #{$mb}px

@each $ml in $list
  .ml#{$ml}
    margin-left: #{$ml}px

@each $mr in $list
  .mr#{$mr}
    margin-right: #{$mr}px

@each $p in $list
  .p#{$p}
    padding-top: #{$p}px

@each $pt in $list
  .pt#{$pt}
    padding-top: #{$pt}px

@each $pb in $list
  .pb#{$pb}
    padding-bottom: #{$pb}px

@each $pl in $list
  .pl#{$pl}
    padding-left: #{$pl}px

@each $pr in $list
  .pr#{$pr}
    padding-right: #{$pr}px

```

_赵彪原创，转载请注明出处_


