# vue
[官网](https://cn.vuejs.org/)

渐进式
JavaScript 框架
渐进式：
  框架不会将所有的 功能都 挂载到 主包上， 设计了 插件（中间件），框架预留拔插式 api 比如（app.use(插件)）, 框架很多功能定义到插件上，使用某个插件 上下文 具备某个功能


特点：
  + mvvm   model-view  view-model 
  数据改变 视图会自动刷新    视图 某些操作也会导致 数据修改
  + 组件化
  + 数据驱动开发  

# mvc


# 思想转换

一切功能 由数据驱动（数据实现 需求），而不是操作dom

state 状态（数据）

# 应用实例雏形
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  <!-- 视图上定义盒子 绑定 vue实例 -->
  <div id="app">
    <button @click="changeMsg">按钮</button>
    {{msg}}
  </div>
  <script src="./vue.global.js"></script>
  <script>
    const { createApp } = Vue;
    // 创建vue 应用程序实例  vue一切功能都是通过实例控制
    // 实例传入配置对象 config对象
    const app = createApp({
      // data 存储 实例上数据  （mvvm中 的 m）
      data(){
        return {
          msg: '你好vue'
        }
      },
      methods: {
        // 存储实例的方法  直接挂载实例上
        changeMsg(){
          this.msg = '值改变了';
        }
      },
    });
    // 渲染到 视图上 控制 视图
    app.mount('#app')

    
  </script>
</body>
</html>
```


# 模板  插值 
功能：
  将实例上 字符串数据 直接在 视图上 输出

+ js环境
+ 输出  支持表达式 （变量、值、运算、三目、短路）
+ 变量 只认识  实例上属性 
+ 全局变量  vue 模板设计 了 白名单  
```
   Infinity,undefined,NaN,isFinite,isNaN,parseFloat,parseInt,decodeURI,' +
  'decodeURIComponent,encodeURI,encodeURIComponent,Math,Number,Date,Array,' +
  'Object,Boolean,String,RegExp,Map,Set,JSON,Intl,BigInt
```

# 指令  directive
作用：
  连接了 数据 和 视图上的 状态
不同指令 可以控制 不同标签的 不同状态 比如：v-show 将 标签显示隐藏状态 和 实例上 布尔值数据绑定

语法：
  扩展了 标签 自定义属性功能
```html
<div v-指令名="值"></div>

<div v-show="isShow"></div>
```

指令的引号中是js环境，需要 给定表达式，绑定指令的值（和模板环境一样）

## 常用指令
绑定元素内容指令
+ v-text 
+ v-html  渲染富文本数据
+ v-show 将元素 显示隐藏状态绑定 布尔值上  true显示 false 元素隐藏
+ v-model 将表单 value属性 和 一个 值 进行双向绑定
  表单修饰符
    .lazy 将 双向绑定 input事件触发 改为 change触发
    .number 将 输入 value 过滤为数字，绑定为 变量 （如果开头不是 非数字字符 解析规则 同 parseFloat 否则 不解析 变量普通v-model）

+ v-bind:属性 
  针对任意标签 任意属性（自定义属性） 将 该属性变量动态属性（将这个属性的值 绑定给 实例上数据）
  注意：
    属性 html原来属性 没有破坏属性功能，<img v-bind:src="url"/>

  简写
    :属性 <img :src="url"/>

# vue中事件绑定
利用 v-on:事件名 进行 vue事件绑定
```html
<button v-on:click="fn">按钮</button>
```
简写：
  @事件名
优点：
  vue合成事件，事件函数 去实例方法查找，便于 修改数据

## 行内直接修改数据 
不建议使用： 视图中不建议 放过多业务代码， 可读性能较差， 没有复用性和可维护性
```html
      <button v-on:click="isShow = !isShow">
        {{isShow?'点击隐藏':'点击显示'}}
      </button>
    
```
## 绑定事件函数
+ 不加括号
```html
 <button v-on:click="fn">按钮</button>
```
+ 加括号
目的：事件触发传递参数
```html
<button v-on:click="fn('这是我传递的参数')">加括号绑定</button>

注意：
  这种写法 fn不再是事件函数，  会编译成 类似代码  btn.onclick = function(){ fn() }
```

## 事件对象
+ 不加括号绑定  方法就是事件函数 第一个参数 就是 事件 对象
```html
<div class="box3" @click="fn3"></div>
```
```js
{
  methods: {
    fn3(e) {
      // 第一个参数就是事件对象
    }
  }
}
```
+ 加括号绑定 vue将事件对象 存在 全局变量 $event中，直接作为实参传入即可
```html
<div class="box3" @click="fn3(3, $event)"></div>
```
```js
{
  methods: {
    fn3(n, e) {
      // 第一个参数就是事件对象
    }
  }
}
```

通过事件对象
```js
e.stopPropagation() // 取消冒泡
e.preventDefault() // 阻止默认事件
e.target // 拿到事件源
```

## 事件修饰符
.stop 取消冒泡
.prevent  阻止默认事件
.self 事件只能自己触发， 后代无法触发
.capture 捕获阶段触发
.once 只触发一次