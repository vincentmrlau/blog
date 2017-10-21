---
title:  "vue组件封装之v-model的支持"
date:   2017-08-16 17:46:18
tags:
- front-end
- vue
excerpt: 聊一聊如何使vue组件支持v-model绑定数据
published: true
---
![](https://raw.githubusercontent.com/vincentmrlau/remote-image-store/master/blog-vue-v-model.png)

## 问题
1. 在封装组件的时候，经常需要做表单的封装
2. 为了是封装之后的组件用起来和原生组件一样的feel，那么就要支持`v-model`绑定属性了

## v-model做了什么
> `v-model`其实是一个语法糖

1. 接收一个`value`苏醒
2. 触发`input`事件的时候改变`value`（可以用修饰符`.lazy`指定到`change`事件）

```javaScript
<input v-model="myValue">
```
等价于

```javaScript
<input 
	v-bind:value="myValue"
	v-on:input="myValue = arguments[0]"
>
```
## 那么要使用v-model的组件应该怎么封装
假设已经封装了一个支持使用`v-model`的组件`selector`

```javaScript
<selector v-model="myValue">
</selector>
```
由上面的例子，这个用法等价于

```javaScript
<selector 
	v-bind:value="myValue"
	v-on:input="myValue = arguments[0]"
>
</selector>
```
由此，我们只需要再组件内将`emit`一个 `input`事件，并且将要改变的`value`作为第一个参数传进来就可以了

## 完整例子：
```javaScript
<template>
  <div class="selector">
    <p v-for="option, index in computedOptions" @click="change(option.value)" :class="option.selected?'selected':''">
      <slot :name="option.value"></slot>{{option.name}}
    </p>
  </div>
</template>

<script>
  export default {
    props: {
      options: {
        type: Array
      },
      value: {
        type: [String, Number]
      }
    },
    computed: {
      computedOptions: function () {
        return this.options.map((x) => {
          if (x.value === this.value) {
            x.selected = true
          } else {
            x.selected = false
          }
          return x
        })
      }
    },
    methods: {
      change: function (value) {
        this.$emit('input', value)
      }
    }
  }
</script>
```

