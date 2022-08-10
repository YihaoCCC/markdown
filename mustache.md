## 📝 踩坑记录:vue的template模板中mustache语法无法获取localStorage

### 1. ❓只是猜测
在这之前，想一想，什么时候会用到`vue`中`mustache`语法，`{{ }}`想取值，或者进行简单计算时，

取谁的值？

vue当前组件的数据  `vue data`

### 2. 👀 测试一下
这样就很容易理解了，`localStorage` 属于 `windows`中的属性，vue当前组件的<font color>最高访问权</font>是 `this` 对象，看下this的结构 <font color=Coral size=2 > 这里用的是 `vue3` </font>
```javascript
<script setup lang='ts'>
    import { onMounted, getCurrentInstance } from 'vue';
    onMounted(() => {
        let vue = getCurrentInstance()
        console.log(vue);

        let test = window.localStorage.getItem('name')
        console.log(test);
        
        console.log(window);
    })
</script>
```
这里打印出来的是当前的vue组件对象，也就是 `vue2` 中的 `this`
又因为mustache语法只能访问到vue组件内部对象，`localStorage` 属于 `window` 对象，所以访问不到就很正常

### 疑问1💬：为什么vuex能访问到
看上面分析，`vuex` 是绑定到<font size=5 color=SandyBrown>全局vue</font>对象上的，通过`this.$store` this 能访问到，所以mustache当然也适用。

### 疑问2💬：为什么`react`中的`{}`可以正常访问`localStroage`
react是`jsx`语法，本身就是原生`script` 并不属于react框架中的功能
```jsx
export function test {
  return (
    <>
      <h3>
        {localStorage.getItem('test')}
      </h3>
    </>
  )
}
```
这里就能拿到全局的`localStorage`对象，所以能访问属性,同时也解释了在`vue`中只能在`script`标签中使用`localStorage`的原因。

<font size=6 color=LightSeaGreen>  总结一下</font>：`vue` 封装  `react` 原生处理能力强



