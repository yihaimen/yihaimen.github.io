# 普通函数与箭头函数有什么区别

## 0x00 引言
大家好，我是以卖码为生的海门。今天想和大家探讨一下普通函数与箭头函数有什么区别？

大家也许在面试的时候被问及过这个问题，而我是在一次和同事的交谈中被问及的。我当时的回答是：普通函数可以在它定义之前的位置使用它，即与位置无关，而箭头函数不行，且箭头函数没有 this。这么回答很显然是说服不了任何人的。

## 0x01 书写形式
简单罗列几种使用过程中的书写形式。主要介绍定义方式，有参无参，返回一条语句或一个对象的情况。相信大家很容易区分书写方式的异同，在实际开发过程中或多或少写这么写过，书写形式方面的区别就不做过多的赘述了，一句话总结就是箭头函数更加简洁明了。

* 箭头函数
```
const doSomething = () => {
  return a + b;
}

const doSomething1 = (a, b) => a + b;

const doSomething2 = c => ({name: c}); 
```
* 普通函数
```
const doSomething = function() {
  return a + b;
}

const doSomething1 = function(a, b) {
  return a + b;
}

const doSomething2 = function(c) {
  return {name: c};
}
```

## 0x02 关于 this
想必大家都有被 this 指针搞晕的情况，我最近还遇到了。


