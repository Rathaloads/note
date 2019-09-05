## VUE源码解读

### 组件的本质

组件的本质就是一个函数，给出相应的数据，渲染对应的HTML模板

```js
import { template } from 'lodash'

// defind
const MyCompoent = prop => {
    const compiler = MyComponent.cache || (MyComponent.cache = template('<h1><%= title %></h1>'))

    return complier(prop)
}

MyComponent.cache = null

// use
document.getElementById('app').innerHTML = MyComponent({ title: 'MyComponent' })
```
模板引擎的时代，组件的输入与产出如下：

    数据 + 模板 = HTML

在NG， React， Vue的时代，组件的产出从HTML变成了Virtual DOM：

    数据 + 模板 = Virtual DOM

### 设计VNODE

### 渲染器


