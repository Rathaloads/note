# Call和Apply的区别

## 定义：
MDN:

    call() 方法使用一个指定的 this 值和单独给出的一个或多个参数来调用一个函数。
    apply() 方法使用一个指定的 this 值和单独给出的一个或多个参数来调用一个函数。

通俗:

    call: 调用一个对象的一个方法，用另一个对象替换当前对象。例如：B.call(A, args1,args2);即A对象调用B对象的方法

    apply: 调用一个对象的一个方法，用另一个对象替换当前对象。例如：B.apply(A, arguments);即A对象应用B对象的方法。

区别:

     call() 方法接受的是一个参数列表，而 apply() 方法接受的是一个包含多个参数的数组

例子:

```js
function Product(name, price) {
    this.name = name;
    this.price = price;
}

function Food(name, price) {
    Product.call(this, name, price);
    this.category = 'food';
}

console.log(new Food('cheese', 5).name);
```