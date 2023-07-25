# 回顾
```
const { createApp } = Vue;
const app = createApp({
  data(){
    return {
      msg: '这是实例数据'
    }
  },
  methods: {
    fn(){
      this.msg = 'wdwdww';
    }
  }
})
app.mount('#app')


{{}}

指令：
  v-text  v-html  v-model(.lazy .number .trim) v-show 
  v-bind:属性 简写 :属性
  v-on:事件  @事件名
  .stop .self .prevent .capture .once
```

# 条件渲染
+ 单分支  v-if

面试题？
  v-show和 v-if区别？
相同点：  将元素显示隐藏状态 和 布尔值绑定 true显示 false 隐藏
不同点：
  v-if false时，将 节点 移除 dom树   
  v-show控制 display: none
使用场景：  
  v-show 有更高的 状态切花性能
  v-if 初始不渲染时， 有更高的 初始渲染性能

  v-show适用于 频繁切换状态的场景，如  选项卡
  v-if适用于  初始不渲染，切换不频繁场景  提高 网页 首屏打开速度

+ v-else 配合 v-if使用
注意： v-else绑定元素 必须是 v-if绑定标签的 下一个兄弟
+ v-else-if
三个指令绑定的元素 必须是 连续的兄弟

# 列表渲染  循环渲染

+ 声明 值得变量
```html
      <ul>
        <li v-for="item in arr">
          {{item}}
        </li>
      </ul>
```
+ 声明值和下标 变量
```html
      <ul>
        <li v-for="(item, index) in arr">
          {{item}}
          --->
          {{index}}
        </li>
      </ul>
```

注意：
1 循环时 指令中声明变量 只能在 绑定标签 内部使用
2 循环少了一个属性 没讲
3 复杂循环，出现循环嵌套时， 内外层声明的变量作用域（优先找 距离最近 v-for声明变量）
  内层想要使用外层的变量 内外层 声明时  变量不能重名

# v-bind:class的扩展
vue针对 标签 class可以是多个值的特点， 允许 将 v-bind:class绑定的值 的类型 字符串、数组、对象

+ 绑定数组
数组每个值 都会编译成一个类
```html
 <div :class="[a, 'box2']">22</div>
 <!-- 编译成 <div class="box box2"></div> -->
```
```js
    const app = createApp({
        data() {
          return {
            a: 'box',
            isActive: true
          };
        },
      });
```
+ 绑定对象
对象 每一个属性 作为 一个类， 是否有这个类 取决于 属性的值 是否为 true
```html
<div :class="{a: 1===1, b: false, active:isActive}">33</div>
<!-- 编译成 <div class="a active"></div> -->
```
```js
    const app = createApp({
        data() {
          return {
            a: 'box',
            isActive: true
          };
        },
      });
```
+ 数组嵌套对象
```html
<div :class="[a, 'box2', {a: 1===1, b: false, active:isActive}]">44</div>
<!-- 编译成 <div class="box box2 a active"></div> -->
```
```js
 const app = createApp({
        data() {
          return {
            a: 'box',
            isActive: true
          };
        },
      });
```