# 封装SVG的icon组件

基于vue-cli3

依赖 `svg-sprite-loader`

> 创建 svg-icon.vue

```html

<template>
  <svg
    :style="`font-size: ${size}; fill: ${color}`"
    class="svg-icon"
    aria-hidden="true"
  >
    <use :xlink:href="iconName" />
  </svg>
</template>

<script>
export default {
  name: 'SvgIcon',

  props: {
    name: {
      type: String,
      default: 'file'
    },
    size: {
      type: String,
      default: '1.4rem'
    },
    color: {
      type: String,
      default: 'currentColor'
    }
  },

  computed: {
    iconName () {
      return `#icon-${this.name}`
    }
  }
}
</script>
<style lang="stylus" scoped>
  .svg-icon
    width 1em
    height 1em
    vertical-align -0.15em
    overflow hidden
</style>

```

> 注册全局组件

```js
import Vue from 'vue'
import SvgIcon from '@/components/svg-icon'// svg组件

// register globally
Vue.component('svg-icon', SvgIcon)

// 引入所有的.svg文件
const req = require.context('./svg', false, /\.svg$/)
const requireAll = requireContext => requireContext.keys().map(requireContext)
requireAll(req)

```

> 配置 vue.config.js

```js

const path = require('path')

module.exports = {
  chainWebpack: config => {
    config.module.rules.delete('svg') // 重点:删除默认配置中处理svg
    config.module
      .rule('svg-sprite-loader')
      .test(/\.svg$/)
      .include
      .add(path.resolve(__dirname, 'src/assets/icons/svg')) // 处理svg目录
      .end()
      .use('svg-sprite-loader')
      .loader('svg-sprite-loader')
      .options({
        symbolId: 'icon-[name]'
      })
  }
}

```