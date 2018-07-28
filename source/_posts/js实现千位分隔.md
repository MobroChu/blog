---
title: js实现千位分隔
date: 2018-07-28 18:06:12
tags: [js, 千位分隔]
description: 最近一个项目中使用到了千位分隔这个功能，在网上也看见一些例子，但是实现起来总觉有些复杂。因此，自己实现了一个千位分隔，留给后来的我们。
---

最近一个项目中使用到了千位分隔这个功能，在网上也看见一些例子，但是实现起来总觉有些复杂。因此，自己实现了一个千位分隔，留给后来的我们。

先上源码吧。  
该方法支持传入的是一个数字字符串，数字。第二个参数为保留小数的位数，默认保留两位小数；
```js
function splitThousands(num, fixed) {
	var reg = /\B(?=(\d{3})+$)/g;
	num = num.toString().split(".");
	
	num[1] = num[1] ? num[1].substr(0, fixed || 2) : "00000000000000000".substr(0, fixed || 2);
	
	return num.join(".");
}
```
这个方法实现非常简单，就是一个正则的问题。在该实现方法中，难点还是这个正则。  
才陋学书，简单看看这个正则：  
\B 同 \D, \W 等一样，取 \b 的反方向。 \b 单词分隔符，那么很明显，\B 就是非单词分隔符。 eg：   

```js
var str = "hello world, hello MobroZhu, not Mobro Zhu";
var reg = [/Mobro\b/g, /Mobro/g];
console.log(reg[0], reg[0].exec(str) , "\n" + reg[1], reg[1].exec(str));
``` 
打印日志如下：    

```js
/Mobro\b/g ["Mobro", index: 33, input: "hello world, hello MobroZhu, not Mobro Zhu"] "
/Mobro/g" ["Mobro", index: 19, input: "hello world, hello MobroZhu, not Mobro Zhu"]
```

很明显可以看出来，两次找到的 Mobro 不是同一个。没有 \b 的话，表示只要匹配到 Mobro 就算数。所以，/Mobro/g 会匹配到两个 Mobro，而 /Mobro\b/g 只会匹配到后面一个 Mobro. 

----
他想，他的心，就像这个坍塌的洞，昏暗而闷臭，没有人适合住进来；   
而他的感情，在人们的眼里，就像刚刚那只老鼠，却在黑暗和污水里，长得膘肥体壮，凶狠的能吃掉一只猫，可依旧见不得光，人人喊打。  

   一一 俗念亲 《每次跳楼，都看见那厮在铺救生气垫》
