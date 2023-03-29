<!--
 * @Author: yulw yulw@anchnet.com
 * @Date: 2023-03-29 17:30:08
 * @LastEditors: yulw yulw@anchnet.com
 * @LastEditTime: 2023-03-29 20:59:15
 * @FilePath: \vue3-demo\README.md
 * @Description: 这是默认设置,请设置`customMade`, 打开koroFileHeader查看配置 进行设置: https://github.com/OBKoro1/koro1FileHeader/wiki/%E9%85%8D%E7%BD%AE
-->
# vue3-demo
## Vue3官网通读
### API 风格（组合式与选项式）
> 选项式即为vue2中data，methods, mounted方法形式，组合式API通常与setup一起使用告诉Vue需要在编译时进行一些处理，作用为 可以在script中使用import，定义顶层变量和函数
 ```
 <script setup>
import { ref, onMounted } from 'vue'

// 响应式状态
const count = ref(0)

// 用来修改状态、触发更新的函数
function increment() {
  count.value++
}

// 生命周期钩子
onMounted(() => {
  console.log(`The initial count is ${count.value}.`)
})
</script>

<template>
  <button @click="increment">Count is: {{ count }}</button>
</template>
```
**两种API风格都能实现大部分业务场景，选项式API是在组合式API基础上实现的。生产中，低复杂度项目推荐使用选项式API，高复杂度使用组合式API+单文件组件**
***
### 起手式
```
  import { createApp } from 'Vue';
  import APP from 'xx/app.vue';
  const app = createApp(APP);
  app.mount('#app');
```
```
// index.html
<div id="d1"><div>
```
**比较vue2，new Vue({}).$mount('#app'), vue3改变为createApp(APP).mount('#app')**
> app.mount('#app'); 中应用到了app根组件的mount方法，还有config（config.errorHandler捕获所有子组件的错误并操作）、component（注册全局子组件）等方法，后续阅读补充

***
### 多个应用实例
```javascript
const app1 = createApp({
  /* ... */
})
app1.mount('#container-1')

const app2 = createApp({
  /* ... */
})
app2.mount('#container-2')
```
> 应用实例并不只限于一个。createApp API 允许你在同一个页面中创建多个共存的 Vue 应用，而且每个应用都拥有自己的用于配置和全局资源的作用域。如果你正在使用 Vue 来增强**服务端**渲染 HTML，并且只想要 Vue 去控制一个大型页面中特殊的一小部分，应避免将一个单独的 Vue 应用实例挂载到整个页面上，而是应该创建多个小的应用实例，将它们分别挂载到所需的元素上去.
***
### 文本插值（{{}}） 即v-text
```template
<span>Message: {{ msg }}</span>
```

### html作为参数(v-html)
```template
<p>Using text interpolation: {{ rawHtml }}</p>
<p>Using v-html directive: <span v-html="rawHtml"></span></p>
```
> 注意，不能使用v-html来拼接组合模板，数据绑定会失效，因为Vue不是一个基于字符串的模板引擎，应使用组件作为UI重用和组合的基本单元。
### 属性绑定(v-bind) ---> :id="dynamicId"
```template
<div v-bind:id="dynamicId"></div>
```
> 双大括号不能在 HTML attributes 中使用。想要响应式地绑定一个 attribute，应该使用 v-bind

```template
<button :disabled="isButtonDisabled">Button</button>
```
> **当 isButtonDisabled 为真值或<u> 一个空字符串 (即 \<button disabled=""\>) 时 </u>，元素会包含这个 disabled attribute。而当其为其他假值时 attribute 将被忽略。**

### 动态绑定多个属性
```javascript
const objectOfAttrs = {
  id: 'container',
  class: 'wrapper'
}
```
```template
<div v-bind="objectOfAttrs"></div>
```
> 如果你有像这样的一个包含多个 attribute 的 JavaScript 对象，可以通过不带参数的 v-bind，你可以将它们绑定到单个元素上

### 动态属性
```template
  <div :[attr]="attrbute"></div>
  <div @[attr]="attrbute"></div>
```
> attr应为一个变量，值为字符串或null，为null时取消该属性，attribute为变量。在html文件中，attr不能包含大写，因为会被转为小写，如属性名为attrName->>attrname,则你的attrName不生效。

***

### 修饰符
