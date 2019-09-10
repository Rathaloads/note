# Function.prototype.bind(thisArg, arg1[,arg2[,...]])

定义:

  bind函数，bind函数会创建一个新的函数，bing被调用时，新函数的this被bind的第一个参数指定，其余的参数将作为新函数的参数供调用的时候使用

例子:

```js
var a = { name: 'Alex' }

function showName () {
  console.log(this.name)
}

var b = showName.bind(a)

b() // 打印Alex

```


# Function.prototype.call(thisArg, arg1, arg2, ...)

定义:

  call()方法使用一个指定的this值和单独给出的一个或多个参数来调用一个函数

例子:

```js
function A (name, age) {
  this.name = name
  this.age = age
}

function B () {
  A.call(this, 'Alex', 12)
  this.sex = '男'
}

var b = new B()

console.log(b.name, b.age, b.sex) // 打印: Alex, 12, 男
```

# Funtion.prototype.apply(thisArg, [argsArray])

定义:

  apply() 方法调用一个具有给定this值的函数，以及作为一个数组（或类似数组对象）提供的参数。

例子:

```js
function A (name, age) {
  this.name = name
  this.age = age
}

function B () {
  A.apply(this, ['Alex', 12])
  this.sex = '男'
}

var b = new B()

console.log(b.name, b.age, b.sex) // 打印: Alex, 12, 男
```