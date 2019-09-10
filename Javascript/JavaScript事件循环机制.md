# JavaScript事件循环机制
```js
var num = 2;
function pow(num) {
    return num * num;
}
pow(num);
setTimeout(callback, 10000);
function callback(){
    console.log('hello timer!');
}
```

![JS引擎原理](../images/JS引擎原理.png)

> 术语解析

+ CALL STACK [调用栈]
+ GLOBAL EX.CONTEXT [全局执行上下文]
+ GLOBAL MEMORY [全局内存，也叫做堆]
+ EVENT LOOP [事件循环]
+ CALLBACK QUEUE [回调队列]
+ BROWSER APIs [浏览器API]
+ 函数内部有一个**本地执行上下文**


> 原理解析

+ 全局内存

    JS引擎在解析JS的时候，会将所有的全局变量和函数的引用放到**全局内存堆**里面

+ 全局执行上下文

    函数在被调用的时候，JS引擎会创建一个**全局执行上下文**，所有代码都是放在这个执行上下文的环境中运行,它是JS在解析和运行时环境的一个抽象概念。它主要包括三个周期: **创建--运行--销毁**

    > 创建:
    
    + 创建变量对象，首先初始化函数的arguments对象，提升函数声明和变量声明，从近到远查找函数运行所需要的变量。

    + 创建作用域链，作用域就是一个独立的地盘，让变量不会相互干扰，当前作用域没有定义的变量，这成为 自由变量。自由变量会向上一直寻找，要到创建这个函数的那个作用域中取值——是“创建”，而不是“调用”，如果最终没有就为undefined。这种层层之间就构成了作用域链。

    + 确定this指向，this、apply、call的指向

    ```js
    var goo = "Hello";
    foo(goo);

    function foo(str) {
        console.log("function1:", str);
        var str = "Yes";
        console.log("function2:", str);
    }
    console.log("全局:", goo)

    // function1: Hello
    // function2: Yes
    // 全局: Hello
    ```

    等价于

    ```js
      function foo(str) {
        var str = str;
        console.log("function1:", str);
        str = "Yes";
        console.log("function:", str);
      }
      var goo;
      goo = "Hello";
      foo(goo)
      console.log("全局:", goo)

      function A(str) {
        var str;
        console.log('log1:', str)
        var str = 'Alex';
        console.log('log2:', str)
        str = 'Perry';
        console.log('log3:', str)
      }
      A('Loly')

    ```

    [参考](https://www.cnblogs.com/yuanzhiguo/p/10626352.html)

+ 调用栈

    在函数被调用的时候，会创建一个**调用栈**来管理**全局执行上下文**,调用栈的数据结构是**栈**的形式存储函数的调用,例子。

    ```js
    function A (a) {
        return a + 10
    }

    function B(b) {
        return A(b+5)
    }

    console.log('Hello')

    console.log(B(5))

    // Hello
    // 20
    // undefined
    ```
    
    在以上代码被调用的时候，**console.log('Hello')**会被首先压入栈，因为是直接输出，所以直接就被执行弹出。然后**console.log(B(5))**执行，**console.log(B(5))**被压入栈作为第一栈帧，**B()**被调用，B函数被压入栈作为第二栈帧,B函数调用**A()**,A函数被调用，压入第三栈帧。A函数执行完毕弹出，紧跟着B执行完毕弹出，最后到console.log(B(5))执行完毕弹出。此时调用栈为空，输出一个undefined[为什么会出现一个undefined？我个人理解是此刻调用栈已全部执行完毕]

    每一个栈帧包含着这个函数的参数以及函数内部的局部变量，同时也包含着这个函数内部的一个**本地执行上下文**

+ 回调队列

    异步代码在JS引擎中执行的时候，会被放到回调队列里面。**每个异步函数在被放入调用堆栈之前必须通过回调队列**,而且需要**等到调用栈空闲的时候，异步函数才会从回调队列进入到调用栈**。

+ 事件循环

    用来检查调用栈是否为空，并且将回调队列里的异步函数放到调用栈中执行。

### 总结

JS在运行的时候,创建一个**全局执行上下文**的环境，脚本的执行都在这个概念环境里面，然后将函数的引用和变量的引用放进一个叫做**全局内存堆**里，等待使用。然后解析函数的调用，将同步的函数按照栈的结构塞进**调用栈**里面，将异步执行的函数按照一定的顺序放进**回调队列**里面,**事件循环机制**不断检测调用栈和回调队列，一旦调用栈被使用完(即调用栈为空的时候)，就将回调队列里面待执行的函数放进调用栈里面执行。

[参考链接](https://github.com/qq449245884/xiaozhi/issues/68)