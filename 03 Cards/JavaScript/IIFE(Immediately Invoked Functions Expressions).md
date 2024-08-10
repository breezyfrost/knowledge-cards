## 概述

IIFE，立即执行函数表达式，即用函数表达式的方式建立函数后立即执行。

## 函数的声明

在 JavaScript 中，有两种常见的函数声明方式 **function 命令** 和 **函数表达式**：

```javascript
// function 命令
function hello(name) {
    console.log('hello ' + name);
}

// 函数表达式
var hello = function (name) {
    console.log('hello ' + name);
}
```

## 函数的调用与立即执行

*函数的调用* 就是在声明函数之后，执行函数的方法

![image.png](https://cdn.jsdelivr.net/gh/breezyfrost/image-host/202304081015593.png)


而 *立即执行* 则是在声明函数的同时执行函数的方法

![image.png](https://cdn.jsdelivr.net/gh/breezyfrost/image-host/202304081017700.png)

那我们能不能在声明函数时不对函数命名呢？

![image.png](https://cdn.jsdelivr.net/gh/breezyfrost/image-host/202304081022334.png)

显然是不行的。因为 JavaScript 引擎在解析代码时，**碰到 function 开头，会认为是在声明一个函数，可是却没有找到该 function 的名称，它无法声明这个函数便会抛出这个错误。

因此我们需要使用 `(...)` 将该函数包起来，这样 **JavaScript 会将其理解为一个函数表达式，而不是在声明一个函数**，这样就不会报错了。

![image.png](https://cdn.jsdelivr.net/gh/breezyfrost/image-host/202304081029065.png)

而这种不对函数命名，并立即执行该函数的写法被称之为 IIFE(Immediately Invoked Functions Expressions)。