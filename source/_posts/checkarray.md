title: How to Check If it is Array?
date: 2015-11-21 15:08:20
tags: JavaScript
categories: JavaScript
---

Got some instruction about how to check if a variable is Array in JavaScript from this [book](https://github.com/lxj/javascript.patterns/blob/master/chapter3.markdown).

Here I just copy and record sth to make me more sensitive for how to check Array in JavaScript.

In the environment which support ES5, just use *Array.isArrary()*. For example:

```js
Array.isArray([]); // true

// trying to fool the check
// with an array-like object
Array.isArray({
  length: 1,
  0: 1,
  slice: function () {}
}); // false
```

In somewhere that no ES5 support, just do this:

```js
if (typeof Array.isArray === undefined) {
  Array.isArray = function (arg) {
    return Object.prototype.toString.call(arg) === '[object Array]';
  };
}
```
