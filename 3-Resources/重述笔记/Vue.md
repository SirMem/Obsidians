---
Project: "[[web前端]]"
Status: 
tags:
  - Resources
Deadline: 
CreateTime: 2024-10-25
Connected: 
sr-due: 2024-10-12
sr-interval: 3
sr-ease: 250
date created: 星期三, 九月 4日 2024, 2:50:24 下午
date modified: 星期二, 十月 15日 2024, 2:51:11 下午
---
# Vue
#review 
## 环境配置
https://xiaoman.blog.csdn.net/article/details/122769982
### nvm安装
[官方文档](https://github.com/nvm-sh/nvm)
### vite安装
[官方文档](https://vitejs.cn/guide/#overview)

**文件索引未更新**：IDE 可能没有索引你的项目文件，导致无法识别文件结构并提供自动补全。你可以尝试**重启编辑器**，或者在 VS Code 中按 `Cmd + Shift + P`（Mac）或 `Ctrl + Shift + P`（Windows），然后选择 `Reload Window` 以刷新文件索引。

优势:
 - 冷服务   默认的构建目标浏览器是能 [在 script 标签上支持原生 ESM](https://caniuse.com/es6-module "在 script 标签上支持原生 ESM") 和 [原生 ESM 动态导入](https://caniuse.com/es6-module-dynamic-import "原生 ESM 动态导入")
- HMR  速度快到惊人的 [模块热更新（HMR）](https://vitejs.cn/guide/features.html#hot-module-replacement "模块热更新（HMR）")
- [Rollup](https://so.csdn.net/so/search?q=Rollup&spm=1001.2101.3001.7020)打包  它使用 [Rollup](https://rollupjs.org/ "Rollup") 打包你的代码，并且它是预配置的 并且支持大部分rollup插件

## Vite 目录 & npm脚本
- public 下面的不会被编译 可以存放静态资源

- assets 下面可以存放可编译的静态资源

- components 下面用来存放我们的组件

- App.vue 是全局组件

- main ts 全局的ts文件

- index.html 非常重要的入口文件 （webpack，rollup 他们的入口文件都是enrty input 是一个js文件 而Vite 的入口文件是一个html文件，他刚开始不会编译这些js文件 只有当你用到的时候 如script src="xxxxx.js" 会发起一个请求被vite拦截这时候才会解析js文件）

- vite config ts 这是vite的配置文件具体配置项

`npm` 脚本用于简化常见的开发、构建和部署任务。可以通过 `package.json` 文件中的 `scripts` 字段定义并运行这些脚本。
- `npm run dev` 启动开发服务器
- `npm run build` 打包项目
- `npm run preview` 预览生产环境

## Vue3 指令

Vue 指令（Directives）是 Vue.js 的一项核心功能，它们可以在 HTML 模板中以 v- 开头的特殊属性形式使用，用于将响应式数据绑定到 DOM 元素上或在 DOM 元素上进行一些操作。

Vue 指令是带有前缀 v- 的特殊 HTML 属性，它赋予 HTML 标签额外的功能。

与传统的 JavaScript 方法相比，使用 Vue 创建响应式页面要容易得多，并且需要的代码更少。

以下是几个常用的 Vue 指令：

| 指令        | 描述                                              |
| --------- | ----------------------------------------------- |
| `v-bind`  | 用于将 Vue 实例的数据绑定到 HTML 元素的属性上。简写 :               |
| `v-if`    | 用于根据表达式的值来条件性地渲染元素或组件。                          |
| `v-show`  | v-show 是 Vue.js 提供的一种指令，用于根据表达式的值来条件性地显示或隐藏元素。  |
| `v-for`   | 用于根据数组或对象的属性值来循环渲染元素或组件。                        |
| `v-on`    | 用于在 HTML 元素上绑定事件监听器，使其能够触发 Vue 实例中的*方法或函数。* 简写@ |
| `v-model` | 用于在表单控件和 Vue 实例的数据之间创建双向数据绑定。                   |

[相关博客的案例](https://xiaoman.blog.csdn.net/article/details/122773486)

### v-for key的作用
`key` 是一个特殊的属性，用于帮助 Vue 更高效地更新和渲染列表。它的作用是为每个渲染的元素提供唯一标识，使 Vue 能够区分这些元素。没有 `key` 的情况下，Vue 可能会尝试尽量重用 DOM 元素，从而导致渲染问题。

#### `key` 的使用规则：

- `key` 必须是一个唯一值。它可以是数组元素的某个唯一属性，如 `id` 或者 `index`。
- 通常建议使用每个元素的唯一 `id` 作为 `key`，避免使用 `index`，因为 `index` 可能会因为插入或删除操作导致元素顺序发生变化，破坏 `key` 的唯一性。

[底层原理](https://xiaoman.blog.csdn.net/article/details/122778560)
## ref、reactive
`ref` 和 `reactive` 是 Vue 3 中两个核心的响应式 API，用于管理和追踪状态的变化。使用它们需要导入相应的包
`import {ref reactive} from 'vue'`

ref在`<template>`标签中自动解包

[相关博客](https://xiaoman.blog.csdn.net/article/details/122780637)
//ref shallowRef
//ref深层次 shallowRef浅层次的响应
//ref 和 shallowRef是不能一块写的不然会影响shallowRef造成视图的更新

https://xiaoman.blog.csdn.net/article/details/122784094
/reactive proxy不能直接赋值，否则破坏响应式对象，无法响应
/解决方案数组可以使用push加解构添加一个对象把数组作为一个属性去解决

### ref
接受一个内部值并返回一个响应式且可变的 ref 对象。ref 对象仅有一个 `.value` property，指向该内部值。
#### 特点：

- 适用于原始数据类型（`number`, `string`, `boolean` 等）。
- 当 `ref` 包裹复杂类型（如对象、数组）时，仍然需要通过 `.value` 来访问。
- 在模板中，Vue 会自动解包 `ref` 对象，因此无需在模板中使用 `.value`。

<template>
  <div>
    <p>{{ count }}</p> <!-- 自动解包，无需写成 count.value -->
    <button @click="increment">Increment</button>
  </div>
</template>
<template>
  <div>
    <p>{{ count }}</p> <!-- 自动解包，无需写成 count.value -->
    <button @click="increment">Increment</button>
  </div>
</template>
<template>
  <div>
    <p>{{ count }}</p> <!-- 自动解包，无需写成 count.value -->
    <button @click="increment">Increment</button>
  </div>
</template>
<template>
  <div>
    <p>{{ count }}</p> <!-- 自动解包，无需写成 count.value -->
    <button @click="increment">Increment</button>
  </div>
</template>
<template>
  <div>
    <p>{{ count }}</p> <!-- 自动解包，无需写成 count.value -->
    <button @click="increment">Increment</button>
  </div>
</template>
<template>
  <div>
    <p>{{ count }}</p> <!-- 自动解包，无需写成 count.value -->
    <button @click="increment">Increment</button>
  </div>
</template>
<template>
  <div>
    <p>{{ count }}</p> <!-- 自动解包，无需写成 count.value -->
    <button @click="increment">Increment</button>
  </div>
</template>
<template>
  <div>
    <p>{{ count }}</p> <!-- 自动解包，无需写成 count.value -->
    <button @click="increment">Increment</button>
  </div>
</template>
<template>
  <div>
    <p>{{ count }}</p> <!-- 自动解包，无需写成 count.value -->
    <button @click="increment">Increment</button>
  </div>
</template>
<template>
  <div>
    <p>{{ count }}</p> <!-- 自动解包，无需写成 count.value -->
    <button @click="increment">Increment</button>
  </div>
</template>
<template>
  <div>
    <p>{{ count }}</p> <!-- 自动解包，无需写成 count.value -->
    <button @click="increment">Increment</button>
  </div>
</template>
```vue
<template>
  <div>
    <p>{{ count }}</p> <!-- 自动解包，无需写成 count.value -->
    <button @click="increment">Increment</button>
  </div>
</template>
```
### reactive
`reactive` 是 Vue 3 中用来创建**深层次响应式对象**的方法，通常用于处理复杂类型的数据，如对象或数组。`reactive` 会将整个对象及其内部所有属性都转换为响应式对象，任何嵌套的属性改变都会触发视图更新。

reactive 源码约束了类型。相较于ref，reactive用来绑定复杂的数据类型 例如 对象 数组。用ref去绑定对象 或者 数组 等复杂的数据类型，源码里面其实也是 去调用reactive

#### 特点：

- 适用于对象、数组等复杂类型。
- `reactive` 创建的是**深度响应式**对象，内部所有的嵌套属性都会被代理。
- 无需 `.value`，直接通过对象的属性访问和修改数据。
- 
### 选择 `ref` 还是 `reactive`？

- 如果只处理**简单的原始类型数据**（如字符串、数字），或者不需要深层次的响应式行为，使用 `ref` 是比较合适的。
- 如果需要对一个**复杂的对象**进行操作，并且想让所有嵌套属性都具备响应式能力，使用 `reactive` 会更方便。

## to(Ref, Refs, Raw)
toRef, toRefs用作对reactive对象解构
https://xiaoman.blog.csdn.net/article/details/122791665
### toRef

`toRef` 是一个用于从 `reactive` 对象中**创建单个属性的 ref** 的函数。它将 `reactive` 对象的某个属性包装成 `ref`，允许你对该属性进行单独的响应式操作，并保持与原对象的同步。通常用作对reactive对象的解构操作

```vue
<template>
  <div>
    <p>{{ count }}</p>
    <button @click="increment">Increment</button>
  </div>
</template>

<script setup>
import { reactive, toRef } from 'vue';

const state = reactive({
  count: 0,
  user: {
    name: 'Vue 3'
  }
});

// 使用 toRef 将 state.count 转化为 ref
const count = toRef(state, 'count');

function increment() {
  count.value++; // 操作的是 ref，但会同步更新到 state.count
}
</script>

```

使用 toRef 将 state.count 转化为 ref，完成解构
### toRefs
`toRefs` 是一个将整个 `reactive` 对象中的所有属性**转换为 ref** 的工具函数。它将对象的每个属性都转化为独立的 `ref`，从而可以解构这些属性而不会失去响应性。

```js
const state = reactive({ count: 0, user: { name: 'Vue 3' } }); 
// 使用 toRefs 将 state 对象的每个属性转化为 ref 
const { count, user } = toRefs(state);
```

### toRaw
`toRaw` 用于从 `reactive` 对象中**获取原始的非响应式对象**。

```js
const rawState = toRaw(state); // 获取非响应式的原始对象
```

## computed
`computed` 是 Vue 3 中的一种计算属性，它用于**基于已有数据计算出新的数据**，并且具备缓存机制，只有依赖的数据发生变化时才会重新计算。

https://xiaoman.blog.csdn.net/article/details/122792620
案例
计算属性本质上是基于响应式数据派生出的数据，它们可以根据其他的响应式数据自动更新。
### 对象式(带有 getter 和 setter)

```vue
<template>
  <div>
    <p>Count: {{ count }}</p>
    <p>Double Count: {{ doubleCount }}</p>
    <button @click="increment">Increment</button>
    <button @click="setDoubleCount">Set Double Count</button>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';

const count = ref(0);

// 对象式 computed 定义，具有 getter 和 setter
const doubleCount = computed({
  get() {
    return count.value * 2;
  },
  set(newValue) {
    count.value = newValue / 2; // 修改 doubleCount 实际是修改 count, 进行了反向推导
  }
});

function increment() {
  count.value++;
}

function setDoubleCount() {
  doubleCount.value = 10; // 会自动调整 count 为 5
}
</script>

```

#### 特点：

- **可读写**：对象式计算属性可以定义 `getter` 和 `setter`，允许修改计算属性时影响其依赖的数据。
- **更灵活**：适合需要双向绑定或反向推导数据的场景。
### 函数式
函数式的 `computed` 是最常见的定义方式，适用于**只读**的计算属性。通过传递一个函数，Vue 会根据这个函数的返回值创建一个计算属性。

```vue
import { computed, reactive, ref } from 'vue'
let price = ref(0)//$0
 
let m = computed<string>(()=>{
   return `$` + price.value
})
 
price.value = 500
```

#### 特点：

- **简单易用**：只需提供一个函数，函数返回的值就是计算属性的结果。
- **只读**：函数式计算属性不能直接修改，专门用于从已有数据派生新值。

## watch & watchEffect

https://xiaoman.blog.csdn.net/article/details/122797990
https://xiaoman.blog.csdn.net/article/details/122802130
### 副作用
**副作用**通常指的是在响应式数据发生变化时，执行的一些额外逻辑或操作。函数式编程中，理想的函数是**纯函数**，即同样的输入总是产生同样的输出，没有任何副作用。副作用指的就是不属于“纯函数”范畴的行为，具体表现为函数执行时，除了返回值之外还对外部环境产生了影响。

**常见的副作用包括：**

- **数据的持久化**：将数据存储到数据库或浏览器的本地存储中。
- **日志输出**：将信息打印到控制台（如 `console.log`）。
- **网络请求**：向服务器发送数据或获取数据。
- **直接操作 DOM**：修改页面上的元素。
- **定时器、事件监听**：如 `setTimeout` 或 `addEventListener`。
### watch
`watch` 是 Vue 提供的一个用于**精确监听**特定响应式数据变化的工具。可以指定一个或多个响应式数据作为监听目标，并在它们发生变化时执行相应的副作用操作。

watch第一个参数监听源

watch第二个参数回调函数cb（newVal,oldVal）

watch第三个参数一个options配置项是一个对象
{
immediate:true //是否立即调用一次
deep:true //是否开启深度监听
}

```js
import { ref, watch } from 'vue'
 
let message = ref({
    nav:{
        bar:{
            name:""
        }
    }
})
 
 
watch(message, (newVal, oldVal) => {
    console.log('新的值----', newVal);
    console.log('旧的值----', oldVal);
},{
    immediate:true,
    deep:true
})
```

监听多个ref 第一个参数变成数组

```js
import { ref, watch ,reactive} from 'vue'
 
let message = ref('')
let message2 = ref('')
 
watch([message,message2], (newVal, oldVal) => {
    console.log('新的值----', newVal);
    console.log('旧的值----', oldVal);
})
```

使用reactive监听深层对象开启和不开启deep 效果一样

### watchEffect
`watchEffect` 是 Vue 3 中的一个自动追踪依赖的工具。它会在依赖的响应式数据变化时，自动执行副作用逻辑。与 `watch` 不同的是，**不需要明确指定依赖的目标**，Vue 会在执行副作用函数时自动追踪依赖的数据。

触发监听之前会调用一个函数可以处理你的逻辑

停止跟踪 watchEffect 返回一个函数 调用之后将停止更新

副作用刷新时机 flush 一般使用post

| pre  | sync        | post       |             |
| ---- | ----------- | ---------- | ----------- |
| 更新时机 | 组件**更新前**执行 | 强制效果始终同步触发 | 组件**更新后**执行 |
|      |             |            |             |

## 组件基础
https://xiaoman.blog.csdn.net/article/details/122811060
每一个.vue 文件呢都可以充当组件来使用
每一个组件都可以复用

## 组件标签
`<suspence>`
`<transition>`
`<teleport>`
`<keep-alive>`
## 父子组件传参
https://xiaoman.blog.csdn.net/article/details/122850170
### 父传子
子组件: defineProps

ts withDefault
@Input
@Output

### 子传父
emit 
defineEmits定义事件传参

父组件在template的子组件中绑定与子组件同样的事件，并通过绑定的方法传递参数
### 子组件暴露属性方法
子组件: defineExpose
从父组件通过ref获取子组件实例

## 全局组件 & 局部组件 & 递归组件
## 动态组件
component组件
属性 is 

## 异步组件
await 异步组件
defineAsyncComponent 父组件
import函数模式
suspence

### 代码分包
通过defineAsyncComponent导入的包，在打包过程中分包处理。这样每个文件只在用户实际访问到相应的组件时才会加载。这减少了初始加载的资源大小，提高了首屏渲染速度。
**异步组件**是指在 Vue 中，可以延迟加载某些组件，只有在需要时才加载。这对于优化应用性能非常有帮助，特别是在构建大型应用时，使用异步组件可以减少初次加载时的资源占用，加快页面渲染速度。

### 异步组件的作用

- **按需加载**：通常应用会把所有组件一次性加载，这可能导致较大的打包文件和缓慢的页面加载时间。异步组件则允许你将组件的加载延迟到真正需要使用时才进行，从而减少初始加载的体积。
- **优化性能**：在路由切换或组件显示时才加载这些组件，可以有效优化性能。

Vue 3 提供了一个新的函数 `defineAsyncComponent` 来创建异步组件。你可以在需要加载组件时调用该函数，并通过动态 `import()` 来懒加载组件。

```vue
<script setup lang="ts">
import { defineAsyncComponent } from 'vue'

// 定义异步组件
const AsyncComponent = defineAsyncComponent(() =>
  import('./MyComponent.vue') // 使用动态导入加载组件
)
</script>

<template>
  <div>
    <!-- 只有在渲染到这里时才会加载异步组件 -->
    <AsyncComponent />
  </div>
</template>

```

顶层 await
使用 `Suspense` 来包裹异步组件，并在等待数据加载时显示骨架屏幕。

```vue
<template>
  <div>
    <h1>Blog Post Comments</h1>
    <Suspense>
      <!-- 加载异步组件 -->
      <template #default>
        <BlogComment />
      </template>
      <!-- 加载骨架屏幕，直到数据加载完成 -->
      <template #fallback>
        <SkeletonLoader />
      </template>
    </Suspense>
  </div>
</template>

<script setup lang="ts">
import BlogComment from './BlogComment.vue'
import SkeletonLoader from './SkeletonLoader.vue'
</script>

```

## 传送组件

disable
to

`Teleport` 是 Vue 3 中引入的一个强大的内置组件，用于将子组件或元素 "传送" 到 DOM 中的任意位置，而不是被限制在父组件的 DOM 层次结构中。

### 使用场景

- **模态框**：模态框通常希望直接挂载在 `body` 下，而不是在组件树的内部。
- **通知消息**：通知一般希望在整个页面上固定显示，不受当前组件的影响。
- **工具提示**：工具提示通常需要精确控制其在 DOM 中的位置。

```vue
<template>
  <div>
    <h1>这是一个普通的页面</h1>

    <!-- 使用 Teleport 将模态框传送到 body 下 -->
    <Teleport to="body">
      <div class="modal">
        <h2>模态框内容</h2>
        <p>这个内容会被传送到 body 下，而不是嵌套在页面结构中。</p>
      </div>
    </Teleport>
  </div>
</template>

<style scoped>
.modal {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background: white;
  padding: 20px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}
</style>

```

### `Teleport` 的关键点

1. **`to` 属性**：`to` 指定了目标元素的选择器，Vue 会将 `Teleport` 的内容传送到这个选择器指向的元素内。在上面的例子中，`to="body"` 表示把模态框传送到 `body` 中。
    
2. **保留组件上下文**：即使 `Teleport` 的内容被移动到别的地方，它仍然保持在原组件树的上下文中。这意味着模态框仍然可以访问父组件的数据和方法。
## keep-alive缓存组件
https://xiaoman.blog.csdn.net/article/details/122953072
`keep-alive` 是 Vue 中的一个内置组件，用于缓存动态组件或路由组件，防止它们在切换时被销毁。它可以让组件在被切换时保持其状态，而不是每次重新渲染和初始化，提升性能和用户体验。
**`<keep-alive>`**：用来缓存动态切换的组件。组件切换时不会被销毁，而是保持其状态。

```vue
<template>
  <div>
    <button @click="currentComponent = 'ComponentA'">显示 A 组件</button>
    <button @click="currentComponent = 'ComponentB'">显示 B 组件</button>

    <!-- 使用 keep-alive 缓存组件 -->
    <keep-alive>
      <component :is="currentComponent"></component>
    </keep-alive>
  </div>
</template>

<script setup>
import { ref } from 'vue'

// 动态组件名
const currentComponent = ref('ComponentA')

// 示例组件
const ComponentA = {
  template: '<div>我是 A 组件</div>'
}

const ComponentB = {
  template: '<div>我是 B 组件</div>'
}
</script>

```

参数 include exclude max

## transition动画组件
leave-active-class enter-active-class
参数属性
duration
基本过渡类名(Vue提供)
自定义过渡类名
animate.css 动画库
transition钩子函数
el: element
gsap 动画库
appear属性 首次渲染执行动画
transition group 参数class tags
transition group列表过渡
平滑过渡
状态过渡 watch监听，生成副作用

## 插槽slot
slot
dialog
**插槽（slot）** 是 Vue 中用于实现组件内容分发的机制，它允许父组件向子组件传递 HTML 结构或其他内容。
### 匿名插槽(默认)
默认插槽用于插入未命名的内容。在子组件中使用 `<slot></slot>`，父组件在该组件的标签中传入的内容会自动替换 `<slot>` 标签。

```vue
<!-- 子组件 ChildComponent.vue -->
<template>
  <div class="box">
    <slot></slot>  <!-- 这里是插槽 -->
  </div>
</template>

<script setup>
// 无需特别定义插槽，<slot> 标签自动处理
</script>

<style scoped>
.box {
  border: 1px solid black;
  padding: 10px;
}
</style>

```

```vue
<!-- 父组件 ParentComponent.vue -->
<template>
  <ChildComponent>
    <p>This is some content inserted into the slot!</p>
  </ChildComponent>
</template>

<script setup>
import ChildComponent from './ChildComponent.vue'
</script>

```

`<slot>` 是默认插槽，父组件 `ParentComponent` 传入的 `<p>` 标签内容会插入到子组件 `ChildComponent` 的 `<slot>` 位置。

### 具名插槽
具名插槽允许你为不同的插槽定义不同的名字，以便在子组件的多个位置插入内容。

```vue
<!-- 子组件 ChildComponent.vue -->
<template>
  <div class="header">
    <slot name="header"></slot>  <!-- 具名插槽 -->
  </div>
  <div class="content">
    <slot></slot>  <!-- 默认插槽 -->
  </div>
  <div class="footer">
    <slot name="footer"></slot>  <!-- 具名插槽 -->
  </div>
</template>

```

```vue
<!-- 父组件 ParentComponent.vue -->
<template>
  <ChildComponent>
    <template #header>
      <h1>This is the header content</h1>
    </template>

    <p>This is the default slot content</p>

    <template #footer>
      <p>This is the footer content</p>
    </template>
  </ChildComponent>
</template>

<script setup>
import ChildComponent from './ChildComponent.vue'
</script>

```

在这个例子中：

- `#header` 传入具名为 `header` 的插槽内容。
- 没有具名的内容会传入默认插槽（即 `slot`）。
- `#footer` 传入具名为 `footer` 的插槽内容。


### 作用域插槽
作用域插槽（Scoped Slots）允许子组件向父组件传递数据，然后父组件可以使用这些数据来动态生成内容。作用域插槽通常用于当子组件需要将数据暴露给插槽内容时，父组件可以根据这些数据来渲染内容。

```vue
<!-- 子组件 ChildComponent.vue -->
<template>
  <ul>
    <li v-for="item in items" :key="item.id">
      <slot :item1="item"></slot>  <!-- 传递 item 给父组件 -->
    </li>
  </ul>
</template>

<script setup>
const items = [
  { id: 1, name: 'Apple' },
  { id: 2, name: 'Banana' },
  { id: 3, name: 'Orange' }
]
</script>

```

```vue
<!-- 父组件 ParentComponent.vue -->
<template>
  <ChildComponent v-slot="{ item1 }">
    <span>{{ item1.name }}</span>  <!-- 使用传递过来的 item -->
  </ChildComponent>
</template>

<script setup>
import ChildComponent from './ChildComponent.vue'
</script>

```

在这个例子中：

- 子组件通过 `<slot :item1="item">` 将 `item1` 暴露给父组件。
- 父组件使用 `v-slot` 获取到子组件传递的 `item1`，并根据它渲染内容。
### 简写方式

## 生命周期
https://xiaoman.blog.csdn.net/article/details/122811060

## sass使用
https://xiaoman.blog.csdn.net/article/details/122832888

### sass

### 1. **什么是 Sass?**

**Sass**（Syntactically Awesome Stylesheets）是一个**CSS 预处理器**，它扩展了 CSS 的功能，允许使用**变量、嵌套规则、混合（mixins）、继承（inheritance）等高级功能** ，简化了 CSS 的编写和管理。最终，它会被编译成标准的 CSS

### 安装sass

```bash
npm install -D sass
pnpm i sass
```

### 配置sass

可以通过 `vite.config.js` 文件中的 `css.preprocessorOptions` 配置全局的 Sass 变量、函数等。这样就不需要在每个组件中重复引入 Sass 资源。

```js
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
 
// https://vitejs.dev/config/
export default defineConfig({
    plugins: [vue()],
    css: {
        preprocessorOptions: {
            scss: {
                additionalData: "@import './src/bem.scss';"
            }
        }
    }
})
```

### scoped
Vue 提供了一种叫做**作用域样式（Scoped CSS）** 的功能，使用 `scoped` 关键字可以确保样式只在当前组件中生效，不会污染全局样式。

```vue
<style scoped>
.example {
  color: blue;
}
</style>

```

加上 `scoped` 后，Vue 会自动为每个元素添加独特的属性标识符，使样式只影响当前组件中的元素。

**注意：** Scoped 样式不能影响子组件中的元素。
### bem架构

**BEM**（Block, Element, Modifier）是一种 CSS 命名规范，旨在使类名的命名更加结构化、可读和易于维护。它将样式分为三类：

- **Block**（块）：代表组件的外层容器。
- **Element**（元素）：属于块的子元素，不能单独存在。
- **Modifier**（修饰符）：用于描述块或元素的不同状态或外观。

### 使用

```sas
$block-sel: "-" !default;
$element-sel: "__" !default;
$modifier-sel: "--" !default;
$namespace:'xm' !default;
@mixin bfc {
    height: 100%;
    overflow: hidden;
}
 
//混入
@mixin b($block) {
   $B: $namespace + $block-sel + $block; //变量
   .#{$B}{ //插值语法#{}
     @content; //内容替换
   }
}
 
@mixin flex {
    display: flex;
}
 
@mixin e($element) {
    $selector:&;
    @at-root {
        #{$selector + $element-sel + $element} {
            @content;
        }
    }
}
 
@mixin m($modifier) {
    $selector:&;
    @at-root {
        #{$selector + $modifier-sel + $modifier} {
            @content;
        }
    }
}
```

```vue
<template>
    <div class="xm-wraps">
         <div>
            <Menu></Menu>
         </div>
         <div class="xm-wraps__right">
            <Header></Header>
            <Content></Content>
         </div>
    </div>
</template>
 
<script lang="ts" setup>
import { ref, reactive } from "vue"
import Menu from './Menu/index.vue'
import Content from './Content/index.vue'
import Header from './Header/index.vue'
</script>
 
<style lang="scss" scoped>
@include b('wraps'){
    @include bfc;
    @include flex;
    @include e(right){
        flex:1;
        display: flex;
        flex-direction: column;
    }
}
</style>
```

## 依赖注入
https://xiaoman.blog.csdn.net/article/details/123143981

在 Vue 3 中，`Provide` 和 `Inject` 是一对用于在组件树中进行依赖注入的 API。它们允许祖先组件（通常是高层级组件）将数据或函数提供给后代组件（不一定是直接的子组件），而无需通过一层一层的 `props` 传递。

### `provide` 和 `inject` 的使用场景
`provide` 和 `inject` 的主要应用场景是当你有全局状态或方法时，想在某个祖先组件中提供，并在多级嵌套的子组件中使用。这避免了繁琐的 `props` 层层传递。

### 基本用法

- **`provide`**：祖先组件通过 `provide` 提供数据。
- **`inject`**：后代组件通过 `inject` 获取数据。

#### 示例 1：基础用法

```vue
<!-- 父组件 Parent.vue -->
<template>
  <div>
    <h1>父组件</h1>
    <Child />
  </div>
</template>

<script setup>
import { provide } from 'vue'
const message = 'Hello from Parent!'
provide('sharedMessage', message) // 提供数据
</script>

<!-- 子组件 Child.vue -->
<template>
  <div>
    <h2>子组件</h2>
    <p>{{ message }}</p>
  </div>
</template>

<script setup>
import { inject } from 'vue'
const message = inject('sharedMessage') // 获取数据
</script>
```

#### 解释：
1. 在父组件 `Parent.vue` 中，使用 `provide` 提供了 `sharedMessage` 数据，它的值是 `'Hello from Parent!'`。
2. 在子组件 `Child.vue` 中，使用 `inject` 来注入 `sharedMessage`，并可以在模板中使用这个变量。

这样，子组件可以直接使用来自祖先组件的值，而不需要通过 `props` 逐级传递。

### 动态提供和注入

`provide` 不仅可以传递静态值，还可以传递动态的对象或函数。

#### 示例 2：传递动态数据

```vue
<!-- 父组件 Parent.vue -->
<template>
  <div>
    <h1>父组件</h1>
    <button @click="increment">增加计数</button>
    <Child />
  </div>
</template>

<script setup>
import { ref, provide } from 'vue'

const count = ref(0)
const increment = () => count.value++

provide('count', count) // 提供动态数据
provide('increment', increment) // 提供函数
</script>

<!-- 子组件 Child.vue -->
<template>
  <div>
    <h2>子组件</h2>
    <p>计数：{{ count }}</p>
    <button @click="increment">子组件增加计数</button>
  </div>
</template>

<script setup>
import { inject } from 'vue'

const count = inject('count') // 获取动态数据
const increment = inject('increment') // 获取函数
</script>
```

#### 解释：
1. `Parent.vue` 提供了一个可响应的 `count` 和一个 `increment` 函数。
2. `Child.vue` 使用 `inject` 获取 `count` 和 `increment`，可以直接在子组件中操作这些值，且它们与父组件是响应式共享的。

### 提供和注入的作用范围

- `provide` 和 `inject` 的作用范围是**组件树**中，意味着祖先组件提供的值可以传递给任何后代组件，而不仅限于直接子组件。
- 如果某个组件之间有较深的嵌套，`provide` 和 `inject` 可以帮助减少重复的 `props` 传递。

### 使用场景

1. **跨层级状态共享**：当需要在多个层级的组件之间共享状态时，而不想通过 `props` 一层一层传递。
2. **插件开发**：很多 Vue 插件使用 `provide` 和 `inject` 来在应用中提供全局的服务（例如 Vue Router、Vuex）。
3. **依赖注入**：提供功能或工具方法给整个组件树中需要的组件。

### `provide` 和 `inject` 的限制

- **非响应式数据**：`provide` 传递的数据本身不是响应式的，除非你传递的是 `ref`、`reactive` 或其他响应式数据。
- **单向传递**：`provide` 是单向的，意味着数据只能从祖先组件流向后代组件，后代组件无法直接修改祖先组件提供的数据。

### 响应式 `provide`/`inject`

当需要保持响应式时，传递的值应该是 Vue 的响应式对象，如 `ref` 或 `reactive`。

#### 示例 3：响应式 `inject`

```vue
<!-- 父组件 Parent.vue -->
<template>
  <div>
    <h1>父组件</h1>
    <button @click="increment">增加计数</button>
    <Child />
  </div>
</template>

<script setup>
import { ref, provide } from 'vue'

const count = ref(0)
const increment = () => count.value++

provide('count', count)
</script>

<!-- 子组件 Child.vue -->
<template>
  <div>
    <h2>子组件</h2>
    <p>计数：{{ count }}</p>
  </div>
</template>

<script setup>
import { inject } from 'vue'

const count = inject('count') // 获取响应式数据
</script>
```

在这个例子中，`count` 是响应式的，因此子组件中的数据也会随之更新。

### 总结

- `provide` 和 `inject` 是用于跨组件树传递数据的工具，尤其适合在多层嵌套的组件中避免 `props` 的繁琐传递。
- 它们适用于全局状态或服务的提供，可以让组件更加灵活地共享数据。
- 如果需要响应式，记得传递 `ref` 或 `reactive` 数据。




## 兄弟组件传参 & BUS

在 Vue 中，兄弟组件之间的传参指的是在两个同级（兄弟）组件之间共享数据或传递消息。因为 Vue 的组件系统是基于单向数据流的，通常父组件会通过 `props` 传递数据给子组件，或者子组件通过 `emit` 事件通知父组件。但兄弟组件之间没有直接的 `props` 或事件通信机制，因此需要使用一些其他的方法。

### 兄弟组件传参的方式

#### 1. 通过父组件作为中介

兄弟组件之间通常是通过父组件作为桥梁来实现通信。父组件接收一个子组件的事件，然后将数据传递给另一个子组件。

**示例**：

```vue
<!-- 父组件 Parent.vue -->
<template>
  <div>
    <ChildA @sendMessage="receiveMessage" />
    <ChildB :message="messageFromA" />
  </div>
</template>

<script setup>
import { ref } from 'vue'
import ChildA from './ChildA.vue'
import ChildB from './ChildB.vue'

const messageFromA = ref('')

const receiveMessage = (message) => {
  messageFromA.value = message
}
</script>

<!-- 子组件 ChildA.vue -->
<template>
  <button @click="sendToB">发送消息到 B</button>
</template>

<script setup>
const emit = defineEmits(['sendMessage'])

const sendToB = () => {
  emit('sendMessage', 'Hello from Child A')
}
</script>

<!-- 子组件 ChildB.vue -->
<template>
  <p>从 A 传递的消息: {{ message }}</p>
</template>

<script setup>
const props = defineProps({
  message: String
})
</script>
```

在这个例子中，`ChildA` 通过触发自定义事件将数据发送到父组件，父组件接收到数据后再通过 `props` 传递给 `ChildB`。这种方式虽然有效，但有时会比较繁琐，特别是当组件结构层次较深时。

#### 2. 使用事件总线（Event Bus）

**事件总线（Event Bus）** 是 Vue 2 中常用的一种用于兄弟组件通信的模式。它是一种发布-订阅模式，通过一个全局的事件总线来进行跨组件通信。尽管 Vue 3 取消了对事件总线的官方支持，但仍然可以使用一些简单的方式实现。

**创建一个事件总线**：

```ts
// eventBus.js
import { reactive } from 'vue'

const eventBus = reactive({
  listeners: {},
  emit(event, data) {
    if (this.listeners[event]) {
      this.listeners[event].forEach(callback => callback(data))
    }
  },
  on(event, callback) {
    if (!this.listeners[event]) {
      this.listeners[event] = []
    }
    this.listeners[event].push(callback)
  },
  off(event, callback) {
    if (this.listeners[event]) {
      this.listeners[event] = this.listeners[event].filter(cb => cb !== callback)
    }
  }
})

export default eventBus
```

**使用事件总线进行通信**：

```vue
<!-- 子组件 ChildA.vue -->
<template>
  <button @click="sendMessageToB">发送消息到 B</button>
</template>

<script setup>
import eventBus from './eventBus'

const sendMessageToB = () => {
  eventBus.emit('messageFromA', 'Hello from Child A')
}
</script>

<!-- 子组件 ChildB.vue -->
<template>
  <p>从 A 传递的消息: {{ message }}</p>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'
import eventBus from './eventBus'

const message = ref('')

const receiveMessage = (data) => {
  message.value = data
}

onMounted(() => {
  eventBus.on('messageFromA', receiveMessage)
})

onBeforeUnmount(() => {
  eventBus.off('messageFromA', receiveMessage)
})
</script>
```

#### 解释：
1. `eventBus.js` 文件中定义了一个简单的事件总线，包含 `emit`、`on` 和 `off` 方法，用于发布和监听事件。
2. `ChildA` 通过 `eventBus.emit` 发送消息。
3. `ChildB` 通过 `eventBus.on` 监听消息，并在接收到消息时更新显示。

这种方式不需要父组件作为中介，兄弟组件可以直接通信。但请注意，Vue 3 中更推荐使用 `Vuex` 或者 `Pinia` 这种状态管理工具来处理复杂的跨组件通信和共享状态。

#### 3. 使用状态管理工具（如 Vuex、Pinia）

对于复杂应用中的状态共享，使用状态管理工具如 Vuex 或 Pinia 更加方便和可维护。它们能够全局管理应用状态，任何组件都可以访问和修改状态，不论它们在组件树中的位置如何。

**Pinia 示例**：

1. 安装 Pinia：

   ```bash
   npm install pinia
   ```

2. 创建一个全局状态管理：

   ```ts
   // store.js
   import { defineStore } from 'pinia'

   export const useStore = defineStore('main', {
     state: () => ({
       messageFromA: ''
     }),
     actions: {
       setMessage(message) {
         this.messageFromA = message
       }
     }
   })
   ```

3. 使用 Pinia 在兄弟组件之间共享数据：

   ```vue
   <!-- 子组件 ChildA.vue -->
   <template>
     <button @click="sendMessageToB">发送消息到 B</button>
   </template>

   <script setup>
   import { useStore } from './store'

   const store = useStore()

   const sendMessageToB = () => {
     store.setMessage('Hello from Child A')
   }
   </script>

   <!-- 子组件 ChildB.vue -->
   <template>
     <p>从 A 传递的消息: {{ store.messageFromA }}</p>
   </template>

   <script setup>
   import { useStore } from './store'

   const store = useStore()
   </script>
   ```

在这个例子中，`Pinia` 用来在两个兄弟组件之间共享状态，使用起来比事件总线更直观，特别是在管理复杂的应用状态时。

### 总结

- **父组件中介**：通过父组件的 `props` 和 `emit` 机制让兄弟组件通信，是最常见的方式，但不适合太多层次的组件通信。
- **事件总线（Event Bus）**：允许组件跨层级、跨兄弟直接通信，但不推荐在 Vue 3 中使用，适合较小的项目。
- **状态管理工具**：如 `Pinia` 或 `Vuex`，更适合处理全局状态和复杂应用中的状态共享。

兄弟组件通信的方式多种多样，选择合适的方式取决于应用的复杂度和需求。

## Mitt
**Mitt** 是一个非常轻量级的事件库，它可以作为事件总线（Event Bus）来实现组件之间的通信，特别是兄弟组件之间的消息传递。Mitt 本身提供了类似于发布/订阅模式的功能，这使得它非常适合用于跨组件通信。

由于兄弟组件之间没有直接的 `props` 和 `emit` 机制，因此 Mitt 可以作为中间工具，帮助它们相互通信。与传统的事件总线相比，Mitt 的 API 更加简单，体积小，非常适合轻量级的项目中使用。

### 如何使用 Mitt 实现兄弟组件之间的交互

**1. 安装 Mitt**

如果你使用的是 Vue 3 项目，可以通过 npm 安装 Mitt：

```bash
npm install mitt
```

**2. 创建一个 Mitt 实例**

首先，你需要创建一个全局的 Mitt 实例，通常你会将它放在一个独立的文件中：

```js
// eventBus.js
import mitt from 'mitt'

const emitter = mitt()

export default emitter
```

**3. 在兄弟组件中使用 Mitt 进行通信**

**ChildA.vue**：发送消息的组件

```vue
<template>
  <button @click="sendMessage">发送消息到 B</button>
</template>

<script setup>
import emitter from './eventBus'

const sendMessage = () => {
  emitter.emit('message', 'Hello from Child A')
}
</script>
```

**ChildB.vue**：接收消息的组件

```vue
<template>
  <p>从 A 传递的消息: {{ message }}</p>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'
import emitter from './eventBus'

const message = ref('')

const receiveMessage = (msg) => {
  message.value = msg
}

onMounted(() => {
  emitter.on('message', receiveMessage)
})

onBeforeUnmount(() => {
  emitter.off('message', receiveMessage)
})
</script>
```

### 解释
- **`emitter.emit('message', data)`**：`ChildA` 使用 `emit` 来发送消息，事件名称为 `'message'`。
- **`emitter.on('message', callback)`**：`ChildB` 监听 `'message'` 事件，并接收 `ChildA` 发送的消息。
- **`emitter.off('message', callback)`**：在组件卸载时，我们取消事件监听，避免内存泄漏。

### Mitt 的特点
- **简单轻量**：Mitt 的代码非常小巧，仅有几行代码，适合小型应用或简单的兄弟组件通信。
- **事件总线功能**：它为兄弟组件、甚至是任意层级的组件提供了一种松耦合的通信方式。
- **API 简单**：Mitt 提供的 API 非常简单，仅有 `on`、`off`、`emit` 等几个方法，使用起来非常容易。

### 总结
- **Mitt** 和兄弟组件交互密切相关，它通过事件发布和订阅机制，使兄弟组件之间可以轻松地传递数据。
- 通过使用 **Mitt**，你可以避免通过父组件传递数据，从而简化组件的交互逻辑。

## TSX
**TSX** 是 **TypeScript JSX** 的缩写，主要是指在 TypeScript 中使用类似 JSX 的语法（用于在 JavaScript/TypeScript 中直接编写 HTML 结构）。在 Vue 3 中，你也可以使用 TSX 来编写组件，这种风格的写法与 React 的 JSX 类似。

在 Vue 3 中使用 TSX，可以实现类似于 React 的组件结构和写法。TSX 允许你使用 JavaScript/TypeScript 的表达式插入到组件的模板中，使组件逻辑和渲染更加紧密结合。

## 自定义指令
https://xiaoman.blog.csdn.net/article/details/123228132

在 Vue 的自定义指令中，`el` 和 `binding` 是指令钩子函数中的两个常用参数，它们分别代表：

1. **`el`**（Element）：
    
    - 这是指令绑定的原生 DOM 元素，意味着你可以直接操作它。比如修改它的样式、属性或添加事件监听器等。
    - `el` 参数在自定义指令的钩子函数中始终会被传递，表示当前指令所应用的 DOM 元素。
2. **`binding`**（Binding）：
    
    - `binding` 是一个对象，包含了指令的值、参数、修饰符等信息。它帮助你获取指令传递的额外数据。

`binding` 对象是由 Vue 在解析自定义指令时自动生成的
el
binding

### 自定义指令内部参数

1. **`el`**：指令绑定的原生 DOM 元素，允许你对它进行操作。
    
2. **`binding`**：包含了指令的相关信息，如绑定的值、参数和修饰符等。
    
    - **`value`**：当前绑定的值，即指令的绑定值。
    - **`oldValue`**：上一次绑定的值，仅在 `updated` 或 `beforeUpdate` 钩子中可用。
    - **`arg`**：通过 `:` 传递的参数，用于传递额外信息。
    - **`modifiers`**：修饰符对象，是一个布尔值，表示是否使用了某个修饰符。
    - **`instance`**：当前组件的实例，即指令所在的组件实例。
3. **`vnode`**：Vue 编译生成的虚拟 DOM 节点。
    
4. **`prevNode`**：上一个虚拟 DOM 节点，仅在 `updated` 或 `beforeUpdate` 钩子中可用。

```vue
<template>
  <div v-color:background.once="color">改变背景颜色</div>
  <button @click="changeColor">改变颜色</button>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

// 定义颜色
const color = ref('red')

// 改变颜色的方法
const changeColor = () => {
  color.value = color.value === 'red' ? 'blue' : 'red';
}

// 自定义指令
const colorDirective = {
  // 元素插入 DOM 时调用
  mounted(el: HTMLElement, binding: any) {
    console.log(binding)
    // 输出 binding 对象的内容
    // {
    //   value: 'red',
    //   oldValue: undefined,
    //   arg: 'background',
    //   modifiers: { once: true },
    //   instance: 当前组件实例
    // }
    
    if (binding.arg === 'background') {
      el.style.backgroundColor = binding.value;  // 使用 binding.value 获取传递的颜色
    }
  },
  // 元素更新时调用
  updated(el: HTMLElement, binding: any) {
    // 仅当没有 .once 修饰符时才更新颜色
    if (!binding.modifiers.once) {
      el.style.backgroundColor = binding.value;
    }
  }
};

export default {
  directives: {
    color: colorDirective
  }
};
</script>

<style scoped>
div {
  padding: 20px;
  color: white;
}
</style>

```

- **`v-color:background.once="color"`**：
    
    - `value`：这里的 `color` 是传递的值，初始为 `'red'`，改变按钮会修改它。
    - `arg`：传递的参数是 `background`，告诉指令需要操作背景颜色。
    - `modifiers`：修饰符是 `once`，表示只在第一次更新时应用。
- **`binding.value`**：当前的颜色值，指令通过它来改变背景颜色。
    
- **`binding.oldValue`**：上一次的颜色值，仅在 `updated` 钩子中有用，用来对比颜色是否发生变化。
    
- **`binding.arg`**：传递的参数，这里是 `background`，表示指令操作的 DOM 属性。
    
- **`binding.modifiers`**：修饰符对象，`once` 为 `true`，表示指令只在首次绑定时生效。


图片懒加载
静态加载
IntersectionObserver
虚拟列表
虚拟滚动
## 自定义hook
Mixins
useAttr
declare
https://xiaoman.blog.csdn.net/article/details/123271189
案例监听元素变化
pnpm init
tsc --init
vite.config.ts
pnpm i vue -D
pnpm i vite -D
MutationObserver
ResizeObserver 主要监听元素宽高的变化

vite打包

npm发布
## 自定义插件
插件支持两种格式，对象格式，函数格式，install
install
vnode
## UI库
Element


## 全局函数和变量
由于Vue3 没有Prototype 属性 使用 app.config.globalProperties 代替 然后去定义变量和函数

## 样式穿透
https://xiaoman.blog.csdn.net/article/details/123319462

## CSS完整新特性
https://xiaoman.blog.csdn.net/article/details/124754590

## tailwind
https://xiaoman.blog.csdn.net/article/details/124951311

## 事件循环
https://www.bilibili.com/video/BV1dS4y1y7vd?p=48&spm_id_from=pageDriver&vd_source=8b450300cfa6415cb0312754cf65ba30

## nextTick
Vue更新DOM是异步的，数据更新是同步的
异步具有传染性，后面调send的方法全都变成异步了，考下大家，如何消除send的异步传染性
操作dom的时候发现数据读取的是上次的，就需要使用nextTick

## 移动端开发
ionic
底层cordova，capacitor

h5
//px固定的单位不会随着屏幕的变化而变化
//rem r root 1rem html font-size 16px 1rem 16px
//375屏幕 适合多少html-font size 引入淘宝的flexible.js
/vw-vh 相对于视口宽高进行控制的 375屏・1vw=3.75px
//百分比是相对于父元素的viewport相对于视口

postCSS Babel

## unoCss原子化
https://xiaoman.blog.csdn.net/article/details/125650172

## electron
进程传参法
## 函数式编程,h函数
//1.temp1ate模板书写风格2.tsx编写风格3.函数式组件h函数
//h函数的源码createVnode
//h函数的优势
跳过了模板的编译
/parser -ast -transform ->js api -> generate -> render

## 编译宏
PropType
Vue3.3 新增了对defineProps泛型的支持
defineOptions
defineSlots 约束Slot所传的信息

## docker配合vue3
https://xiaoman.blog.csdn.net/article/details/126375948

## 环境变量
配置用户代码片段
环境变量是在一个import存在的
系统环境变量
import.meta.dev
process.dev

自定义环境变量

## webpack
打包器
vue-loader

## 性能优化

## Web Components

## 跨域
jsonp请求
cors 后端设置
proxy代理 常用

## Pinia
### defineStore
第一个参数为唯一标识符区分不同的Store
### 枚举类型初始化文件
将 `enum`（枚举）用于 `defineStore` 中的第一个参数（即 store 名称）有几个好处：

 1. **减少硬编码，增强可维护性**：
   直接使用字符串作为 store 名称容易出错，特别是在代码中多次引用时，如果名称发生变化，你需要手动更改所有相关代码。而使用枚举可以减少硬编码，统一管理所有 store 名称。如果需要修改名称，你只需要在枚举中修改一次即可。

   **好处**：
   - 避免拼写错误，尤其在重构时更安全。
   - 修改方便，只需更改枚举值，无需到处找和改动字符串。

   ```typescript
   // names.ts
   export const enum Names {
      Test = 'TEST'
   }

   // store/index.ts
   import { defineStore } from 'pinia';
   import { Names } from './names'; // 导入 Names 枚举

   export const useTestStore = defineStore(Names.Test, {
     state: () => ({
       count: 0,
     }),
     actions: {
       increment() {
         this.count++;
       },
     },
   });
   ```

2. **增加类型安全**：
   使用枚举可以带来额外的类型检查。`TypeScript` 在编译时会检查你是否使用了正确的枚举值，减少使用时拼写错误的风险。如果使用普通字符串，TypeScript 无法对这些字符串进行检查。

   **好处**：
   - 提高代码的类型安全性，在编译时就能发现潜在的错误。
   - 自动补全功能：使用枚举时，IDE 能够提供自动补全，减少手动输入的错误几率。

3. **代码一致性**：
   枚举为你提供了统一的名称管理，代码更一致和可读。通过将常用的 store 名称集中在一起，开发者可以在不同的模块或组件中保持一致的名称使用风格，增加代码的可维护性。

4. **便于扩展**：
   当项目变大时，你可能会有多个 store。使用枚举可以集中管理 store 的名称，便于未来扩展。比如，你可以为不同的模块或功能区分 store 名称，而不需要在不同文件里手动输入字符串。

#### 例子：

#### names.ts
```typescript
export const enum Names {
  Test = 'TEST',
  User = 'USER',
  Product = 'PRODUCT'
}
```

#### store/index.ts
```typescript
import { defineStore } from 'pinia';
import { Names } from './names';

export const useTestStore = defineStore(Names.Test, {
  state: () => ({
    count: 0,
  }),
  actions: {
    increment() {
      this.count++;
    },
  },
});

export const useUserStore = defineStore(Names.User, {
  state: () => ({
    user: null,
  }),
  actions: {
    setUser(user: any) {
      this.user = user;
    },
  },
});
```

## State修改值
1 直接调用修改
```ts
<template>
     <div>
         <button @click="Add">+</button>
          <div>
             {{Test.current}}
          </div>
     </div>
</template>
 
<script setup lang='ts'>
import {useTestStore} from './store'
const Test = useTestStore()
const Add = () => {
    Test.current++
}
 
</script>
 
<style>
 
</style>
```

2 通过patch批量进行修改State的值
```ts
import {useTestStore} from './store'
const Test = useTestStore()
const Add = () => {
    Test.$patch({
       current:200,
       age:300
    })
}
 

```

3 通过patch批量修改的函数形式（推荐）
```ts
import {useTestStore} from './store'
const Test = useTestStore()
const Add = () => {
    Test.$patch((state) => {
	    state.current = 200,
	    state.age = 300
    })
}
```
### action(支持同步异步)
1 同步直接调用
```ts
import { defineStore } from 'pinia'
import { Names } from './store-naspace'
export const useTestStore = defineStore(Names.TEST, {
    state: () => ({
        counter: 0,
    }),
    actions: {
        increment() {
            this.counter++
        },
        randomizeCounter() {
            this.counter = Math.round(100 * Math.random())
        },
    },
})

// App.vue
Test.increment()直接调用
```

2 异步 可以结合async await 进行修饰

### getter和action的区别
在 Vue 的状态管理工具（如 Vuex 或 Pinia）中，`getter` 和 `action` 都是用来处理和获取状态数据的工具，但它们的作用和使用场景有所不同。这里是它们的区别：

### 1. **getter**
- **作用**：类似于 Vue 组件中的计算属性，`getter` 用于**计算和获取派生状态**（即从现有状态派生出新的值），并返回数据。`getter` 是**同步**的，只做纯计算，不进行异步操作。
- **使用场景**：当你需要根据状态计算出一些值，或者对状态进行简单的处理来返回派生数据时，使用 `getter`。
- **特点**：
  - 返回值是基于状态的计算结果。
  - 适合用于状态的派生计算和缓存。
  - 通常是无副作用的，依赖输入值，输入相同则输出结果不变。

#### 示例：
```javascript
getters: {
  newCurrent (): number | string {
    return ++this.current + this.newName;
  },
  newName (): string {
    return `$-${this.name}`;
  }
}
```
在这个例子中，`newCurrent` 和 `newName` 是 `getter`，它们根据现有的 `current` 和 `name` 状态计算出一个新值并返回。

### 2. **action**
- **作用**：`action` 用于**处理异步操作**或复杂的业务逻辑，并且可以**提交状态变更**。与 `getter` 不同，`action` 是可以**异步**的，它通常被用于从外部 API 获取数据、执行异步任务，然后通过提交 `mutation` 或直接修改状态来更新应用的状态。
- **使用场景**：当你需要处理异步逻辑（如 API 请求）或包含复杂业务逻辑时，使用 `action`。
- **特点**：
  - 通常用于执行异步操作，如向后端发起请求、定时任务等。
  - 通过 `dispatch` 来调用 `action`。
  - `action` 本身并不直接修改状态，而是通过提交 `mutation` 或修改 `state` 来更新状态。
  
#### 示例：
```javascript
actions: {
  async fetchData() {
    const data = await fetch('/api/data');
    this.data = await data.json();
  }
}
```
在这个例子中，`fetchData` 是一个 `action`，它异步请求数据并在获取到数据后修改状态 `data`。

### 总结区别：
- **getter**：用于计算或获取派生状态，是**同步**的，仅进行状态的读取和计算，不直接改变状态。
- **action**：用于处理异步操作或复杂逻辑，可以是**异步**的，通过提交 `mutation` 或直接修改 `state` 来改变状态。

因此，`getter` 主要用来派生状态，而 `action` 用来执行复杂或异步任务。

### API
https://xiaoman.blog.csdn.net/article/details/123402377

### 插件
## Router
### 基本路由

```App.vue
<Template>
	<router-view><router-view/>
<Template/>
```

```route/index.ts
import {createRouter, createWebHistory} from 'vue-router'

const routes: Array<RouteRecordRaw> = [{
    path: '/:',
    component: () => import('../components/Testing.vue')
},{
    path: '/register',
    component: () => import('../components/Headers.vue')
}]

const route = createRouter({
    history: createWebHistory(),
    routes
})

export default route
```

```main.ts

import App from './App.vue'
import { createApp } from 'vue'
import route from './route/index.ts'

const app = create(App)
app.use(route)
app.mount('#app')
```

### Router-link
https://xiaoman.blog.csdn.net/article/details/123589648
除了 path 之外，你还可以为任何路由提供 name。这有以下优点：
- **避免硬编码 URL**：使用 `name` 可以避免在代码中直接写死 URL 路径，当路由地址改变时，不需要到处修改跳转路径，只需要修改路由的 `name`。
- **自动编码/解码参数**：当你在路由中传递 `params`（例如动态参数）时，Vue 会自动处理 URL 中的编码和解码工作，减少出错的几率。
- **防止 URL 打字错误**：直接使用路由的 `name` 属性可以避免拼写 URL 路径时的手误。
- **绕过路径排序**：在某些情况下，Vue 会根据路径的复杂程度对路由进行排序，通过 `name` 可以避免排序问题，确保路由的优先级。
```
<router-link :to="{name:'Login'}">Login</router-link>
<router-link :to="{name:'Reg'}">Reg</router-link>
```

### 编程式导航
除了使用 `<router-link>` 创建 a 标签来定义导航链接，我们还可以借助 router 的[实例方法](https://so.csdn.net/so/search?q=%E5%AE%9E%E4%BE%8B%E6%96%B9%E6%B3%95&spm=1001.2101.3001.7020)，通过编写代码来实现。

编程式初始化
```ts
import {useRouter} from 'vue-router'

const router = useRouter()
```

函数
```
import { useRouter } from 'vue-router'
const router = useRouter()
 
const toPage = () => {
  router.push('/reg')
}
```
对象
```
import { useRouter } from 'vue-router'
const router = useRouter()
 
const toPage = () => {
  router.push({
    path: '/reg'
  })
}
```
命名式
```
import { useRouter } from 'vue-router'
const router = useRouter()
 
const toPage = () => {
  router.push({
    name: 'Reg'
  })
}
```


在执行 `toPage` 函数后，浏览器会导航到 `/reg` 路由，并显示与 `/reg` 路由关联的组件。

### 历史记录
https://xiaoman.blog.csdn.net/article/details/123590884
在标签Route-view中使用replace，当页面重新刷新后，不会保留历史记录
`<route-view replace to = '/:'>`

编程式
```ts
const toPage = (url: string) => {
	router.replace(url)
}
```

```ts
const toPage = () => {
	router.push() //导航
}

const toPage = () => {
	router.back() //历史退后
}

const toPage = () => {
	router.go() //历史前进
}
```

### 路由传参
query 对应 path去传参
params 对应 name去传参

```ts
const toDetail = (item: Item) => {
	router.push{
		path: /reg
		query: item
	}
}

const toDetail = (item: Item) => {
	router.push{
		name: reg
		params: item
	}
}

//使用 
import {useRoute} from 'vue-router'

const route = useRoute()

<template> 
	  {{route.query.name}}
	  {{route.params.id}}
</template>
```

### 嵌套路由
https://xiaoman.blog.csdn.net/article/details/123618719
```ts
const routes: Array<RouteRecordRaw> = [
    {
        path: "/user",
        component: () => import('../components/footer.vue'),
        children: [
            {
                path: "",
                name: "Login",
                component: () => import('../components/login.vue')
            },
            {
                path: "reg",
                name: "Reg",
                component: () => import('../components/reg.vue')
            }
        ]
    },
 
]
```

父路由组件
```vue
    <div>
        <router-view></router-view> //子路由组件界面
        <div>
	        //父路由界面
            <router-link to="/">login</router-link> //修改<router-view>标签内容
            <router-link style="margin-left:10px;" to="/user/reg">reg</router-link>
        </div>
    </div>
```

### 命令路由
```ts
import { createRouter, createWebHistory, RouteRecordRaw } from 'vue-router'
 
 
const routes: Array<RouteRecordRaw> = [
    {
        path: "/",
        components: {
            default: () => import('../components/layout/menu.vue'),
            header: () => import('../components/layout/header.vue'),
            content: () => import('../components/layout/content.vue'),
        }
    },{
	    path: "/user",
        components: {
            header: () => import('../components/layout/header.vue'),
            content: () => import('../components/layout/content.vue'),
    
    }
]
 
const router = createRouter({
    history: createWebHistory(),
    routes
})
 
export default router
```

```vue
    <div>
        <router-view></router-view>
        <router-view name="header"></router-view>
        <router-view name="content"></router-view>
        <router-link to='/user:'></router-link> //显示多组件
    </div>
```

### 重定向别名
https://xiaoman.blog.csdn.net/article/details/123697904
```ts
const routes: Array<RouteRecordRaw> = [
    {
        path:'/',
        component:()=> import('../components/root.vue'),
        redirect:'/user1', //字符串形式
        redirect: { path: '/user1'}, //对象形式
        redirect: (to) => {  //函数形式
	        return {
		        path: '/user1',
		        query: to.query
	        }
        }
        children:[
            {
                path:'/user1',
                components:{
                    default:()=> import('../components/A.vue')
                }
            },
            {
                path:'/user2',
                components:{
                    bbb:()=> import('../components/B.vue'),
                    ccc:()=> import('../components/C.vue')
                }
            }
        ]
    }
]
```

alias别名
```ts
const routes: Array<RouteRecordRaw> = [
    {
        path: '/',
        component: () => import('../components/root.vue'),
        alias:["/root","/root2","/root3"], //访问/root, /root2 , /root3 也是访问/ 路径
        children: [
            {
                path: 'user1',
                components: {
                    default: () => import('../components/A.vue')
                }
            },
            {
                path: 'user2',
                components: {
                    bbb: () => import('../components/B.vue'),
                    ccc: () => import('../components/C.vue')
                }
            }
        ]
    }
]
```

### 导航守卫

## 特殊的$event对象
在 Vue 中，`$event` 是一个特殊的内置变量，通常用于在模板内传递事件对象。它代表的是事件处理函数所接收的原生 DOM 事件对象，类似于普通 JavaScript 中的 `event` 对象。

### 1. **`$event` 的使用场景**
当你在 Vue 模板中绑定一个事件处理方法时，有时需要将事件对象传递给方法。这时可以使用 `$event` 变量，它代表当前事件的 `event` 对象。

例如：

```vue
<template>
  <button @click="handleClick($event)">Click me</button>
</template>

<script setup>
function handleClick(event) {
  console.log(event); // 输出原生 DOM 事件对象
}
</script>
```

在这个例子中，`$event` 被用来传递点击事件对象给 `handleClick` 方法。

### 2. **典型的用法**
- **传递额外的参数**：你可以同时传递事件对象和其他参数给方法。  
  例如：

  ```vue
  <button @click="handleClick('Hello', $event)">Click me</button>
  ```

  然后在处理函数中接收这些参数：

  ```javascript
  function handleClick(message, event) {
    console.log(message); // 输出 'Hello'
    console.log(event); // 输出原生 DOM 事件对象
  }
  ```

- **结合修饰符使用**：Vue 中的事件修饰符（例如 `.stop`、`.prevent` 等）会自动处理事件对象的默认行为和冒泡过程。如果你需要手动处理事件时，仍然可以使用 `$event`。
  
  例如，阻止默认行为但仍然获取事件对象：

  ```vue
  <form @submit.prevent="handleSubmit($event)">
    <input type="text" />
    <button type="submit">Submit</button>
  </form>
  ```

  ```javascript
  function handleSubmit(event) {
    console.log(event); // 输出提交事件对象
  }
  ```

### 3. **与原生 `event` 对象的区别**
在 Vue 模板中，`$event` 是直接提供给开发者使用的变量，表示事件处理器的事件对象。但它本质上与 JavaScript 中的 `event` 对象是相同的。唯一的区别是 `$event` 是 Vue 在模板中的语法糖，用于方便地在事件绑定中引用，而在方法内部你仍然会获取到标准的 DOM 事件对象。

### 总结
- `$event` 是 Vue 模板中的特殊变量，用来代表原生 DOM 事件对象。
- 它允许你在模板中将事件对象传递给方法，同时可以与其他参数一同传递。
- 虽然它是 Vue 的模板语法，但最终 `$event` 表示的仍是标准的 DOM 事件对象。


## 配置项
在 Vue 3 中，**配置项（options）** 是用于定义组件行为、状态和功能的一种方式。配置项通常是对象形式，包含了组件的各种选项，如数据、方法、生命周期钩子、计算属性等。配置项一般结合 `export default` 一起使用，用于导出组件定义。

### 1. **配置项的作用**
Vue 组件本质上是一个对象，通过配置项来声明组件的属性和行为。常见的配置项包括：

- `data`：用于声明组件的状态。
- `methods`：定义组件中的方法。
- `computed`：定义计算属性。
- `props`：声明父组件传递给当前组件的属性。
- `watch`：用于监听数据的变化。
- `emits`：声明组件可以发射的事件。
- `mounted`、`created` 等：生命周期钩子。

这些配置项提供了组件的核心逻辑和数据管理方式。

### 2. **结合 `export default` 使用**
Vue 组件通常通过 `export default` 语法导出一个配置项对象。`export default` 用于将该对象导出为组件，以便在其他地方引用和使用。

例如：

```javascript
export default {
  name: 'MyComponent', // 组件名称
  data() {
    return {
      message: 'Hello, Vue 3!'
    };
  },
  methods: {
    greet() {
      console.log(this.message);
    }
  },
  mounted() {
    this.greet(); // 生命周期钩子
  }
};
```

在这个例子中，`export default` 导出的是一个包含 `data`、`methods` 和 `mounted` 配置项的对象，用来定义一个 Vue 组件。

### 3. **配置项的分类**
Vue 3 的配置项大致可以分为以下几类：
- **数据相关**：`data`、`props`、`computed`、`methods`。
- **生命周期**：`created`、`mounted`、`updated`、`beforeDestroy` 等。
- **事件相关**：`emits`、`watch`。
- **其它**：`components`、`directives`、`filters`。

### 4. **Vue 3 的 Composition API**
在 Vue 3 中，除了传统的配置项（Options API），还引入了 Composition API，通过 `setup` 函数来更灵活地组织组件逻辑。因此，不再必须依赖配置项对象，而可以直接使用函数式的组织方式。不过，传统的 `export default` 方式依然是可用的，尤其对于已有的 Vue 2 项目过渡到 Vue 3 时非常有用。

### 总结
- **配置项** 是 Vue 组件的核心，通过配置项声明组件的属性、状态和行为。
- 它们通常结合 `export default` 使用，以便导出组件并在其他地方使用。
- Vue 3 新增了 Composition API，但传统的配置项方式仍然是主流和常用的。

## script setup

`<script setup>` 是在单文件组件 (SFC) 中使用组合式 API 的编译时语法糖。当同时使用 SFC 与组合式 API 时该语法是默认推荐。相比于普通的 `<script>` 语法，它具有更多优势：

- 更少的样板内容，更简洁的代码。
- 能够使用纯 TypeScript 声明 props 和自定义事件。
- 更好的运行时性能 (其模板会被编译成同一作用域内的渲染函数，避免了渲染上下文代理对象)。
- 更好的 IDE 类型推导性能 (减少了语言服务器从代码中抽取类型的工作)。

## watch
对对象数据进行监听，数据发生变化执行方法
## 深层侦听器
响应式对象

回调
## `watchEffect()`
## 一次性侦听器

## 副作用清理[​](https://cn.vuejs.org/guide/essentials/watchers.html#side-effect-cleanup)

有时我们可能会在侦听器中执行副作用，例如异步请求


## 模板引用
模板引用是指html标签的内容，可以被script的函数引用，从而函数可以实现类似与html标签内容的效果吗

是的，在 Vue 3 中，模板引用 (`ref`) 是指通过 `ref` 特性绑定到模板中的 DOM 元素或组件实例，允许你在 JavaScript 代码中通过 `ref` 引用这些元素或组件，从而操作它们。

在你的示例中，你使用了 `ref="my-input"` 将 `<input>` 元素与模板引用 `my-input` 绑定。然后在 `<script setup>` 中，通过 `useTemplateRef` 来获取对这个 DOM 元素的引用，从而可以在 `onMounted` 生命周期钩子中调用 `input.value.focus()`，使输入框在组件挂载后自动获取焦点。

### 具体流程解释：
1. 在 `<template>` 中，通过 `ref="my-input"` 给 `<input>` 元素绑定引用。
2. 在 `<script setup>` 中使用 `useTemplateRef('my-input')` 来获取与这个引用绑定的 DOM 元素。
3. 当组件挂载后，`onMounted` 钩子会触发，此时你可以通过 `input.value` 访问到真实的 DOM 元素，并调用 `focus()` 方法让输入框获取焦点。

不过，Vue 3 的标准做法是使用 `ref()` 进行模板引用的定义，而不是 `useTemplateRef`。你可以改为以下方式：

```vue
<script setup>
import { ref, onMounted } from 'vue'

const input = ref(null)

onMounted(() => {
  input.value.focus()
})
</script>

<template>
  <input ref="input" />
</template>
```

在这个版本中，`ref(null)` 定义了一个引用容器，`ref="input"` 将 DOM 元素和 JavaScript 中的 `input` 变量绑定，`input.value` 就是对 DOM 元素的访问。

useTemplateRef(): 返回一个浅层 ref，其值将与模板中的具有匹配 ref attribute 的元素或组件同步。

## emit

基于函数实现组件之间的交互

## slot 
基于html代码实现组件之间的交互

## 动态组件

## pinia
状态记录