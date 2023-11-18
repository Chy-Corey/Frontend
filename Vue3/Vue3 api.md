# Vue 3 api

[API 参考 | Vue.js (vuejs.org)](https://cn.vuejs.org/api/)

记录了一些常用的 api。

## 一、全局 api

#### 1. nextTick()

等待下一次 DOM 更新刷新的工具方法。

- **类型**

  ```ts
  function nextTick(callback?: () => void): Promise<void>
  ```
  
- **详细信息**

  当你在 Vue 中更改响应式状态时，最终的 DOM 更新并不是同步生效的，而是由 Vue 将它们缓存在一个队列中，直到下一个“tick”才一起执行。这样是为了确保每个组件无论发生多少状态改变，都仅执行一次更新。

  `nextTick()` 可以会状态更新后立即调用，以等待 DOM 更新完成。你可以传递一个回调函数作为参数，或者 await 返回的 Promise。

  当操作 Dom 时，由于 Dom 的更新是异步的，所以需要使用 `nextTick`

- **示例**

  ```vue
  <script setup>
  import { ref, nextTick } from 'vue'
  
  const count = ref(0)
  
  async function increment() {
    count.value++
  
    // DOM 还未更新
    console.log(document.getElementById('counter').textContent) // 0
  
    await nextTick()
    // DOM 此时已经更新
    console.log(document.getElementById('counter').textContent) // 1
  }
  </script>
  
  <template>
    <button id="counter" @click="increment">{{ count }}</button>
  </template>
  ```



#### 2. app.version

提供当前应用所使用的 Vue 版本号。这在[插件](https://cn.vuejs.org/guide/reusability/plugins.html)中很有用，因为可能需要根据不同的 Vue 版本执行不同的逻辑。

- **类型**

  ts

  ```
  interface App {
    version: string
  }
  ```

- **示例**

  在一个插件中对版本作判断：

  js

  ```
  export default {
    install(app) {
      const version = Number(app.version.split('.')[0])
      if (version < 3) {
        console.warn('This plugin requires Vue 3')
      }
    }
  }
  ```

#### 3. app.config

每个应用实例都会暴露一个 `config` 对象，其中包含了对这个应用的配置设定。你可以在挂载应用前更改这些属性 (下面列举了每个属性的对应文档)。

```js
import { createApp } from 'vue'

const app = createApp(/* ... */)

console.log(app.config)
```

#### 4. app.config.errorHandler

用于为应用内抛出的未捕获错误指定一个全局处理函数。

- **示例**

  ```js
  app.config.errorHandler = (err, instance, info) => {
    // 处理错误，例如：报告给一个服务
  }
  ```

- **详细信息**

  错误处理器接收三个参数：错误对象、触发该错误的组件实例和一个指出错误来源类型信息的字符串。

  它可以从下面这些来源中捕获错误：

  - 组件渲染器
  - 事件处理器
  - 生命周期钩子
  - `setup()` 函数
  - 侦听器
  - 自定义指令钩子
  - 过渡 (Transition) 钩子

#### 5. app.config.warnHandler

用于为 Vue 的运行时警告指定一个自定义处理函数。

- **详细信息**

  警告处理器将接受警告信息作为其第一个参数，来源组件实例为第二个参数，以及组件追踪字符串作为第三个参数。

  这可以用户过滤筛选特定的警告信息，降低控制台输出的冗余。所有的 Vue 警告都需要在开发阶段得到解决，因此仅建议在调试期间选取部分特定警告，并且应该在调试完成之后立刻移除。

  > 警告仅会在开发阶段显示，因此在生产环境中，这条配置将被忽略。

- **示例**

  ```js
  app.config.warnHandler = (msg, instance, trace) => {
    // `trace` 是组件层级结构的追踪
  }
  ```

#### 6. app.config.globalProperties

向任何组件实例中添加可全局访问的属性。

```js
// 在vue2中: 
vue.prototype.$http = () => {}
// 在vue3中: 
app.config.globalproperties.$http = () => {}
```

```js
//在main.js中创建实例
import { createApp } from 'vue'
import App from './App.vue'
const app = createApp(App);
//挂载方法到实例上
app.config.globalProperties.$axios = axios

//全局调用
import { getCurrentInstance } from "vue";
const { proxy }: any = getCurrentInstance();
proxy.$axios()
```



## 二、组合式 api

### 1. `setup()`

`setup()` 钩子是在组件中使用组合式 API 的入口，通常只在以下情况下使用：

1. 需要在非单文件组件中使用组合式 API 时。
2. 需要在基于选项式 API 的组件中集成基于组合式 API 的代码时。

我们可以使用[响应式 API](https://cn.vuejs.org/api/reactivity-core.html) 来声明响应式的状态，在 `setup()` 函数中返回的对象会暴露给模板和组件实例。其他的选项也可以通过组件实例来获取 `setup()` 暴露的属性：

```vue
<script>
import { ref } from 'vue'

export default {
  setup() {
    const count = ref(0)

    // 返回值会暴露给模板和其他的选项式 API 钩子
    return {
      count
    }
  },

  mounted() {
    console.log(this.count) // 0
  }
}
</script>

<template>
  <button @click="count++">{{ count }}</button>
</template>
```

在模板中访问从 `setup` 返回的 [ref](https://cn.vuejs.org/api/reactivity-core.html#ref) 时，它会[自动浅层解包](https://cn.vuejs.org/guide/essentials/reactivity-fundamentals.html#deep-reactivity)，因此你无须再在模板中为它写 `.value`。当通过 `this` 访问时也会同样如此解包。

`setup()` 自身并不含对组件实例的访问权，即在 `setup()` 中访问 `this` 会是 `undefined`。你可以在选项式 API 中访问组合式 API 暴露的值，但反过来则不行。

`setup()` 应该*同步地*返回一个对象。唯一可以使用 `async setup()` 的情况是，该组件是 [Suspense](https://cn.vuejs.org/guide/built-ins/suspense.html) 组件的后裔。

#### 访问 Props

`setup` 函数的第一个参数是组件的 `props`。和标准的组件一致，一个 `setup` 函数的 `props` 是响应式的，并且会在传入新的 props 时同步更新。

```js
export default {
  props: {
    title: String
  },
  setup(props) {
    console.log(props.title)
  }
}
```

请注意如果你解构了 `props` 对象，解构出的变量将会丢失响应性。因此我们推荐通过 `props.xxx` 的形式来使用其中的 props。

如果你确实需要解构 `props` 对象，或者需要将某个 prop 传到一个外部函数中并保持响应性，那么你可以使用 [toRefs()](https://cn.vuejs.org/api/reactivity-utilities.html#torefs) 和 [toRef()](https://cn.vuejs.org/api/reactivity-utilities.html#toref) 这两个工具函数：

```js
import { toRefs, toRef } from 'vue'

export default {
  setup(props) {
    // 将 `props` 转为一个其中全是 ref 的对象，然后解构
    const { title } = toRefs(props)
    // `title` 是一个追踪着 `props.title` 的 ref
    console.log(title.value)

    // 或者，将 `props` 的单个属性转为一个 ref
    const title = toRef(props, 'title')
  }
}
```

#### Setup 上下文

传入 `setup` 函数的第二个参数是一个 **Setup 上下文**对象。上下文对象暴露了其他一些在 `setup` 中可能会用到的值：

```js
export default {
  setup(props, context) {
    // 透传 Attributes（非响应式的对象，等价于 $attrs）
    console.log(context.attrs)

    // 插槽（非响应式的对象，等价于 $slots）
    console.log(context.slots)

    // 触发事件（函数，等价于 $emit）
    console.log(context.emit)

    // 暴露公共属性（函数）
    console.log(context.expose)
  }
}
```

该上下文对象是非响应式的，可以安全地解构：

```js
export default {
  setup(props, { attrs, slots, emit, expose }) {
    ...
  }
}
```

`attrs` 和 `slots` 都是有状态的对象，它们总是会随着组件自身的更新而更新。这意味着你应当避免解构它们，并始终通过 `attrs.x` 或 `slots.x` 的形式使用其中的属性。此外还需注意，和 `props` 不同，`attrs` 和 `slots` 的属性都**不是**响应式的。如果你想要基于 `attrs` 或 `slots` 的改变来执行副作用，那么你应该在 `onBeforeUpdate` 生命周期钩子中编写相关逻辑。

#### 暴露公共属性

`expose` 函数用于显式地限制该组件暴露出的属性，当父组件通过[模板引用](https://cn.vuejs.org/guide/essentials/template-refs.html#ref-on-component)访问该组件的实例时，将仅能访问 `expose` 函数暴露出的内容：

```js
export default {
  setup(props, { expose }) {
    // 让组件实例处于 “关闭状态”
    // 即不向父组件暴露任何东西
    expose()

    const publicCount = ref(0)
    const privateCount = ref(0)
    // 有选择地暴露局部状态
    expose({ count: publicCount })
  }
}
```

#### 与渲染函数一起使用

`setup` 也可以返回一个[渲染函数](https://cn.vuejs.org/guide/extras/render-function.html)，此时在渲染函数中可以直接使用在同一作用域下声明的响应式状态：

```js
import { h, ref } from 'vue'

export default {
  setup() {
    const count = ref(0)
    return () => h('div', count.value)
  }
}
```

返回一个渲染函数将会阻止我们返回其他东西。对于组件内部来说，这样没有问题，但如果我们想通过模板引用将这个组件的方法暴露给父组件，那就有问题了。

我们可以通过调用 [`expose()`](https://cn.vuejs.org/api/composition-api-setup.html#exposing-public-properties) 解决这个问题：

```js
import { h, ref } from 'vue'

export default {
  setup(props, { expose }) {
    const count = ref(0)
    const increment = () => ++count.value

    expose({
      increment
    })

    return () => h('div', count.value)
  }
}
```

此时父组件可以通过模板引用来访问这个 `increment` 方法。

#### 在语法糖中暴露公共属性

由于是否暴露某个属性，是在编译阶段决定的，所以不需要调用某个函数，使用 `defineExpose` 编译宏即可：

```js
<script setup>
import { ref } from 'vue'

const num = ref(0)

defineExpose({
    num,
})
</script>
```

#### 在父组件中使用

子组件中暴露以后，如果要在父组件中调用，一定要在挂在以后，因为只有等子组件挂载了，才能拿到子组件暴露出的属性：

```js
<template>
  <Child ref="child" />
</template>

<script setup>
import { ref, onMounted } from 'vue'
import Child from './Child.vue'

const child = ref(null)

onMounted(() => {
  // child.value 是 <Child /> 组件的实例
  child.value.a	// 假设暴露了属性 a
})
</script>
```



### 2. 响应式核心

#### `ref()`

接受一个内部值，返回一个响应式的、可更改的 ref 对象，此对象只有一个指向其内部值的属性 `.value`。

- **详细信息**

  ref 对象是可更改的，也就是说你可以为 `.value` 赋予新的值。它也是响应式的，即所有对 `.value` 的操作都将被追踪，并且写操作会触发与之相关的副作用。

  如果将一个对象赋值给 ref，那么这个对象将通过 [reactive()](https://cn.vuejs.org/api/reactivity-core.html#reactive) 转为具有深层次响应式的对象。这也意味着如果对象中包含了嵌套的 ref，它们将被深层地解包。

  若要避免这种深层次的转换，请使用 [`shallowRef()`](https://cn.vuejs.org/api/reactivity-advanced.html#shallowref) 来替代。

- **示例**

  ```js
  const count = ref(0)
  console.log(count.value) // 0
  
  count.value++
  console.log(count.value) // 1
  ```

#### `shallowRef()`

[`ref()`](https://cn.vuejs.org/api/reactivity-core.html#ref) 的浅层作用形式。

- **详细信息**

  和 `ref()` 不同，浅层 ref 的内部值将会原样存储和暴露，并且不会被深层递归地转为响应式。只有对 `.value` 的访问是响应式的。

  `shallowRef()` 常常用于对大型数据结构的性能优化或是与外部的状态管理系统集成。

- **示例**

  ```js
  const state = shallowRef({ count: 1 })
  
  // 不会触发更改
  state.value.count = 2
  
  // 会触发更改
  state.value = { count: 2 }
  ```

#### `reactive()`

返回一个对象的响应式代理。

- **详细信息**

  响应式转换是“深层”的：它会影响到所有嵌套的属性。一个响应式对象也将深层地解包任何 [ref](https://cn.vuejs.org/api/reactivity-core.html#ref) 属性，同时保持响应性。

  值得注意的是，当访问到某个响应式数组或 `Map` 这样的原生集合类型中的 ref 元素时，不会执行 ref 的解包。

  若要避免深层响应式转换，只想保留对这个对象顶层次访问的响应性，请使用 [shallowReactive()](https://cn.vuejs.org/api/reactivity-advanced.html#shallowreactive) 作替代。

  返回的对象以及其中嵌套的对象都会通过 [ES Proxy](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy) 包裹，因此**不等于**源对象，建议只使用响应式代理，避免使用原始对象。

- **示例**

  创建一个响应式对象：

  ```js
  const obj = reactive({ count: 0 })
  obj.count++
  ```

  ref 的解包：

  ```ts
  const count = ref(1)
  const obj = reactive({ count })
  
  // ref 会被解包
  console.log(obj.count === count.value) // true
  
  // 会更新 `obj.count`
  count.value++
  console.log(count.value) // 2
  console.log(obj.count) // 2
  
  // 也会更新 `count` ref
  obj.count++
  console.log(obj.count) // 3
  console.log(count.value) // 3
  ```

  注意当访问到某个响应式数组或 `Map` 这样的原生集合类型中的 ref 元素时，**不会**执行 ref 的解包：

  ```js
  const books = reactive([ref('Vue 3 Guide')])
  // 这里需要 .value
  console.log(books[0].value)
  
  const map = reactive(new Map([['count', ref(0)]]))
  // 这里需要 .value
  console.log(map.get('count').value)
  ```

  将一个 [ref](https://cn.vuejs.org/api/reactivity-core.html#ref) 赋值给一个 `reactive` 属性时，该 ref 会被自动解包：

  ```ts
  const count = ref(1)
  const obj = reactive({})
  
  obj.count = count
  
  console.log(obj.count) // 1
  console.log(obj.count === count.value) // true
  ```

#### `reactive()` 的局限性

`reactive()` API 有一些局限性：

1. **有限的值类型**：它只能用于对象类型 (对象、数组和如 `Map`、`Set` 这样的[集合类型](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects#keyed_collections))。它不能持有如 `string`、`number` 或 `boolean` 这样的[原始类型](https://developer.mozilla.org/en-US/docs/Glossary/Primitive)。

2. **不能替换整个对象**：由于 Vue 的响应式跟踪是通过属性访问实现的，因此我们必须始终保持对响应式对象的相同引用。这意味着我们不能轻易地“替换”响应式对象，因为这样的话与第一个引用的响应性连接将丢失：

   ```js
   let state = reactive({ count: 0 })
   
   // 上面的 ({ count: 0 }) 引用将不再被追踪
   // (响应性连接已丢失！)
   state = reactive({ count: 1 })
   ```

3. **对解构操作不友好**：当我们将响应式对象的原始类型属性解构为本地变量时，或者将该属性传递给函数时，我们将丢失响应性连接：

   ```js
   const state = reactive({ count: 0 })
   
   // 当解构时，count 已经与 state.count 断开连接
   let { count } = state
   // 不会影响原始的 state
   count++
   
   // 该函数接收到的是一个普通的数字
   // 并且无法追踪 state.count 的变化
   // 我们必须传入整个对象以保持响应性
   callSomeFunction(state.count)
   ```

由于这些限制，我们建议使用 `ref()` 作为声明响应式状态的主要 API。

#### `ref()` VS `reactive()`

`ref()` 和 `reactive()` 用于跟踪其参数的更改。当使用它们初始化变量时，是向 Vue 提供信息：“嘿，每次这些变量发生更改时，请重新构建或重新运行依赖于它们的所有内容”。

在下面的示例中，单击按钮后 `personRef` 和 `personReactive` 都会修改 `name` 属性，随后便会在 UI 上得到响应，但对普通 JS 对象 `person` 来说就不会。

```vue
<template>
  {{ person.name }} <!-- 不会变为 Amy -->
  {{ personRef.name }} <!-- 会变为 Amy -->
  {{ personReactive.name }} <!-- 会变为 Amy -->
  <button @click="changeName('Amy')">Change Name</button>
</template>

<script setup lang="ts">
  import { ref, reactive } from 'vue'

  const person = { name: 'John' }
  const personRef = ref({ name: 'John' })
  const personReactive = reactive({ name: 'John' })

  const changeName = (name: string) => {
    person.name = name
    personRef.value.name = name
    personReactive.name = name
  }
</script>
```

##### 不同之处

`ref()` 与 `reactive()` 主要有三个区别：

- `ref()` 函数可以接受[原始类型](https://link.juejin.cn/?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fen-US%2Fdocs%2FGlossary%2FPrimitive)（最常见的是布尔值、字符串和数字）以及对象作为参数，而 `reactive()` 函数只能接受对象作为参数。

```javascript
// 无效
const x = reactive(true)

// 有效
const x = ref(true)
```

对于对象，这两种语法都是有效的：

```javascript
// 有效
const x = reactive({ name: 'John'})

// 有效
const x = ref({ name: 'John'})
```

- `ref()` 有一个 `.value` 属性，你必须使用 `.value` 属性获取内容，但是使用 `reactive()` 的话可以直接访问：

```javascript
// 有效
const x = reactive({ name: 'John'})
x.name = 'Amy'
// x => { name: 'Ammy' }

// 有效
const x = ref({ name: 'John'})
x.value.name = 'Amy'
// x.value => { name: 'Ammy' }
```

> 注意：`ref` 传递到模板（`<template>`）时，会被解包，因此你不需要在那里写 `.value`。

- 使用 `ref()` 函数可以替换整个对象实例，但是在使用 `reactive()` 函数时就不行：

```javascript
// 无效 - x 的更改不会被 Vue 记录
let x = reactive({name: 'John'})
x = reactive({todo: true})

// 有效
const x = ref({name: 'John'})
x.value = {todo: true}
```

##### 何时使用 `reactive()`

使用 `reactive()` 的一个优点是避免样板代码。当使用 `ref`s 时，你可能会因为在代码中到处写 `.value` 而感到烦恼。但是，如果需要原始值，则无法避免 `ref()` 的使用……除非将它们放入对象中！

事实证明，在想要将整个状态放入一个变量中时，`reactive()` 非常方便，就像在选项 API（`data()`）中所做的那样。对比 `ref`，`reactive()` 会跟踪每个属性的变动。

**这意味着在 `reactive()` 中的每个属性都会被视为独立的 `ref()`**, Vue 会据此确定是否需要更新某些依赖于这些属性的内容。

**如果想将 `ref()` 用作状态容器，每当一个属性更新时，使用该状态的任何地方都将被更新。这会触发不必要的重新渲染并减慢应用程序的速度。**

以下是使用一个状态变量示例：

```javascript
const state = reactive({
  person: {name: 'John'},
  isLoading: false,
  updated: true,
  ...
})
```

**结论**：你可以将 `reactive()` 看作未包装的 `ref()` 容器。如果你想要一个单状态变量，可以在组件内部使用它。

#### `readonly()`

接受一个对象 (不论是响应式还是普通的) 或是一个 [ref](https://cn.vuejs.org/api/reactivity-core.html#ref)，返回一个原值的只读代理。

- **详细信息**

  只读代理是深层的：对任何嵌套属性的访问都将是只读的。它的 ref 解包行为与 `reactive()` 相同，但解包得到的值是只读的。

  要避免深层级的转换行为，请使用 [shallowReadonly()](https://cn.vuejs.org/api/reactivity-advanced.html#shallowreadonly) 作替代。

- **示例**

  ```js
  const original = reactive({ count: 0 })
  
  const copy = readonly(original)
  
  watchEffect(() => {
    // 用来做响应性追踪
    console.log(copy.count)
  })
  
  // 更改源属性会触发其依赖的侦听器
  original.count++
  
  // 更改该只读副本将会失败，并会得到一个警告
  copy.count++ // warning!
  ```

### 3. 响应式工具

#### `isRef`

检查某个值是否为 ref。

请注意，返回值是一个[类型判定](https://www.typescriptlang.org/docs/handbook/2/narrowing.html#using-type-predicates) (type predicate)，这意味着 `isRef` 可以被用作类型守卫：

```ts
let foo: unknown
if (isRef(foo)) {
  // foo 的类型被收窄为了 Ref<unknown>
  foo.value
}
```

#### `unref()`

如果参数是 ref，则返回内部值，否则返回参数本身。这是 `val = isRef(val) ? val.value : val` 计算的一个语法糖。

- **示例**

  ```ts
  function useFoo(x: number | Ref<number>) {
    const unwrapped = unref(x)
    // unwrapped 现在保证为 number 类型
  }
  ```

#### `toRef()`

可以将值、refs 或 getters 规范化为 refs (3.3+)。

也可以基于响应式对象上的一个属性，创建一个对应的 ref。这样创建的 ref 与其源属性保持同步：改变源属性的值将更新 ref 的值，反之亦然。

- **示例**

  规范化签名 (3.3+)：

  ```js
  // 按原样返回现有的 ref
  toRef(existingRef)
  
  // 创建一个只读的 ref，当访问 .value 时会调用此 getter 函数
  toRef(() => props.foo)
  
  // 从非函数的值中创建普通的 ref
  // 等同于 ref(1)
  toRef(1)
  ```

  对象属性签名：

  ```js
  const state = reactive({
    foo: 1,
    bar: 2
  })
  
  // 双向 ref，会与源属性同步
  const fooRef = toRef(state, 'foo')
  
  // 更改该 ref 会更新源属性
  fooRef.value++
  console.log(state.foo) // 2
  
  // 更改源属性也会更新该 ref
  state.foo++
  console.log(fooRef.value) // 3
  ```

  **请注意，这不同于：**

  ```js
  const fooRef = ref(state.foo)
  ```

  上面这个 ref **不会**和 `state.foo` 保持同步，因为这个 `ref()` 接收到的是一个纯数值。

  `toRef()` 这个函数在你想把一个 prop 的 ref 传递给一个组合式函数时会很有用：

  ```vue
  <script setup>
  import { toRef } from 'vue'
  
  const props = defineProps(/* ... */)
  
  // 将 `props.foo` 转换为 ref，然后传入
  // 一个组合式函数
  useSomeFeature(toRef(props, 'foo'))
  
  // getter 语法——推荐在 3.3+ 版本使用
  useSomeFeature(toRef(() => props.foo))
  </script>
  ```

  当 `toRef` 与组件 props 结合使用时，关于禁止对 props 做出更改的限制依然有效。尝试将新的值传递给 ref 等效于尝试直接更改 props，这是不允许的。在这种场景下，你可能可以考虑使用带有 `get` 和 `set` 的 [`computed`](https://cn.vuejs.org/api/reactivity-core.html#computed) 替代。详情请见[在组件上使用 `v-model`](https://cn.vuejs.org/guide/components/v-model.html) 指南。

  当使用对象属性签名时，即使源属性当前不存在，`toRef()` 也会返回一个可用的 ref。这让它在处理可选 props 的时候格外实用，相比之下 [`toRefs`](https://cn.vuejs.org/api/reactivity-utilities.html#torefs) 就不会为可选 props 创建对应的 refs。

#### `toValue()` 

将值、refs 或 getters 规范化为值。这与 [unref()](https://cn.vuejs.org/api/reactivity-utilities.html#unref) 类似，不同的是此函数也会规范化 getter 函数。如果参数是一个 getter，它将会被调用并且返回它的返回值。

这可以在[组合式函数](https://cn.vuejs.org/guide/reusability/composables.html)中使用，用来规范化一个可以是值、ref 或 getter 的参数。

- **示例**

  ```js
  toValue(1) //       --> 1
  toValue(ref(1)) //  --> 1
  toValue(() => 1) // --> 1
  ```

  在组合式函数中规范化参数：

#### `toRefs()`

将一个响应式对象转换为一个普通对象，这个普通对象的每个属性都是指向源对象相应属性的 ref。每个单独的 ref 都是使用 [`toRef()`](https://cn.vuejs.org/api/reactivity-utilities.html#toref) 创建的。

- **示例**

  ```js
  const state = reactive({
    foo: 1,
    bar: 2
  })
  
  const stateAsRefs = toRefs(state)
  /*
  stateAsRefs 的类型：{
    foo: Ref<number>,
    bar: Ref<number>
  }
  */
  
  // 这个 ref 和源属性已经“链接上了”
  state.foo++
  console.log(stateAsRefs.foo.value) // 2
  
  stateAsRefs.foo.value++
  console.log(state.foo) // 3
  ```

  当从组合式函数中返回响应式对象时，`toRefs` 相当有用。使用它，消费者组件可以解构/展开返回的对象而不会失去响应性（**`reactive` 解构会丢失响应性**）：

  ```js
  function useFeatureX() {
    const state = reactive({
      foo: 1,
      bar: 2
    })
  
    // ...基于状态的操作逻辑
  
    // 在返回时都转为 ref
    return toRefs(state)
  }
  
  // 可以解构而不会失去响应性
  const { foo, bar } = useFeatureX()
  ```

  `toRefs` 在调用时只会为源对象上可以枚举的属性创建 ref。如果要为可能还不存在的属性创建 ref，请改用 [`toRef`](https://cn.vuejs.org/api/reactivity-utilities.html#toref)。

#### `isProxy()`

检查一个对象是否是由 [`reactive()`](https://cn.vuejs.org/api/reactivity-core.html#reactive)、[`readonly()`](https://cn.vuejs.org/api/reactivity-core.html#readonly)、[`shallowReactive()`](https://cn.vuejs.org/api/reactivity-advanced.html#shallowreactive) 或 [`shallowReadonly()`](https://cn.vuejs.org/api/reactivity-advanced.html#shallowreadonly) 创建的代理。

#### `isReactive()`

检查一个对象是否是由 [`reactive()`](https://cn.vuejs.org/api/reactivity-core.html#reactive) 或 [`shallowReactive()`](https://cn.vuejs.org/api/reactivity-advanced.html#shallowreactive) 创建的代理。

#### `isReadonly()`

检查传入的值是否为只读对象。只读对象的属性可以更改，但他们不能通过传入的对象直接赋值。

通过 [`readonly()`](https://cn.vuejs.org/api/reactivity-core.html#readonly) 和 [`shallowReadonly()`](https://cn.vuejs.org/api/reactivity-advanced.html#shallowreadonly) 创建的代理都是只读的，因为他们是没有 `set` 函数的 [`computed()`](https://cn.vuejs.org/api/reactivity-core.html#computed) ref。

### 4. 响应式进阶

#### `shallowRef()`

[`ref()`](https://cn.vuejs.org/api/reactivity-core.html#ref) 的浅层作用形式。

- **详细信息**

  和 `ref()` 不同，浅层 ref 的内部值将会原样存储和暴露，并且不会被深层递归地转为响应式。只有对 `.value` 的访问是响应式的。

  `shallowRef()` 常常用于对大型数据结构的性能优化或是与外部的状态管理系统集成。

- **示例**

  ```js
  const state = shallowRef({ count: 1 })
  
  // 不会触发更改
  state.value.count = 2
  
  // 会触发更改
  state.value = { count: 2 }
  ```

#### `triggerRef()`

强制触发依赖于一个[浅层 ref](https://cn.vuejs.org/api/reactivity-advanced.html#shallowref) 的副作用，这通常在对浅引用的内部值进行深度变更后使用。

配合shallowref()一起使用，强制触发响应，手动更新视图。

- **示例**

  ```js
  const shallow = shallowRef({
    greet: 'Hello, world'
  })
  
  // 触发该副作用第一次应该会打印 "Hello, world"
  watchEffect(() => {
    console.log(shallow.value.greet)
  })
  
  // 这次变更不应触发副作用，因为这个 ref 是浅层的
  shallow.value.greet = 'Hello, universe'
  
  // 打印 "Hello, universe"
  triggerRef(shallow)
  ```

#### `customRef()`

创建一个自定义的 ref，显式声明对其依赖追踪和更新触发的控制方式。

- **详细信息**

  `customRef()` 预期接收一个工厂函数作为参数，这个工厂函数接受 `track` 和 `trigger` 两个函数作为参数，并返回一个带有 `get` 和 `set` 方法的对象。

  一般来说，`track()` 应该在 `get()` 方法中调用，而 `trigger()` 应该在 `set()` 中调用。然而事实上，你对何时调用、是否应该调用他们有完全的控制权。

- **示例**

  创建一个防抖 ref，即只在最近一次 set 调用后的一段固定间隔后再调用：

  ```js
  import { customRef } from 'vue'
  
  export function useDebouncedRef(value, delay = 200) {
    let timeout
    return customRef((track, trigger) => {
      return {
        get() {
          track()
          return value
        },
        set(newValue) {
          clearTimeout(timeout)	// 取消上一次的 setTimeout
          timeout = setTimeout(() => {
            value = newValue
            trigger()
          }, delay)
        }
      }
    })
  }
  ```

  在组件中使用：

  ```vue
  <script setup>
  import { useDebouncedRef } from './debouncedRef'
  const text = useDebouncedRef('hello')
  </script>
  
  <template>
    <input v-model="text" />
  </template>
  ```

#### `shallowReactive()`

[`reactive()`](https://cn.vuejs.org/api/reactivity-core.html#reactive) 的浅层作用形式。

- **详细信息**

  和 `reactive()` 不同，这里没有深层级的转换：一个浅层响应式对象里只有根级别的属性是响应式的。属性的值会被原样存储和暴露，这也意味着值为 ref 的属性**不会**被自动解包了。

  谨慎使用

  浅层数据结构应该只用于组件中的根级状态。请避免将其嵌套在深层次的响应式对象中，因为它创建的树具有不一致的响应行为，这可能很难理解和调试。

- **示例**

  ```js
  const state = shallowReactive({
    foo: 1,
    nested: {
      bar: 2
    }
  })
  
  // 更改状态自身的属性是响应式的
  state.foo++
  
  // ...但下层嵌套对象不会被转为响应式
  isReactive(state.nested) // false
  
  // 不是响应式的
  state.nested.bar++
  ```

#### `shallowReadonly()`

[`readonly()`](https://cn.vuejs.org/api/reactivity-core.html#readonly) 的浅层作用形式

- **详细信息**

  和 `readonly()` 不同，这里没有深层级的转换：只有根层级的属性变为了只读。属性的值都会被原样存储和暴露，这也意味着值为 ref 的属性**不会**被自动解包了。

  谨慎使用

  浅层数据结构应该只用于组件中的根级状态。请避免将其嵌套在深层次的响应式对象中，因为它创建的树具有不一致的响应行为，这可能很难理解和调试。

- **示例**

  ```js
  const state = shallowReadonly({
    foo: 1,
    nested: {
      bar: 2
    }
  })
  
  // 更改状态自身的属性会失败
  state.foo++
  
  // ...但可以更改下层嵌套对象
  isReadonly(state.nested) // false
  
  // 这是可以通过的
  state.nested.bar++
  ```

#### `toRaw()`

根据一个 Vue 创建的代理返回其原始对象。

- **详细信息**

  `toRaw()` 可以返回由 [`reactive()`](https://cn.vuejs.org/api/reactivity-core.html#reactive)、[`readonly()`](https://cn.vuejs.org/api/reactivity-core.html#readonly)、[`shallowReactive()`](https://cn.vuejs.org/api/reactivity-advanced.html#shallowreactive) 或者 [`shallowReadonly()`](https://cn.vuejs.org/api/reactivity-advanced.html#shallowreadonly) 创建的代理对应的原始对象。

  这是一个可以用于临时读取而不引起代理访问/跟踪开销，或是写入而不触发更改的特殊方法。不建议保存对原始对象的持久引用，请谨慎使用。

- **示例**

  ```js
  const foo = {}
  const reactiveFoo = reactive(foo)
  
  console.log(toRaw(reactiveFoo) === foo) // true
  ```

#### `markRaw()`

将一个对象标记为不可被转为代理。返回该对象本身。

- **示例**

  ```js
  const foo = markRaw({})
  console.log(isReactive(reactive(foo))) // false
  
  // 也适用于嵌套在其他响应性对象
  const bar = reactive({ foo })
  console.log(isReactive(bar.foo)) // false
  ```

- 谨慎使用

- `markRaw()` 和类似 `shallowReactive()` 这样的浅层式 API 使你可以有选择地避开默认的深度响应/只读转换，并在状态关系谱中嵌入原始的、非代理的对象。它们可能出于各种各样的原因被使用：

  - 有些值不应该是响应式的，例如复杂的第三方类实例或 Vue 组件对象。

  - 当呈现带有不可变数据源的大型列表时，跳过代理转换可以提高性能。

  这应该是一种进阶需求，因为只在根层访问能到原始值，所以如果把一个嵌套的、没有标记的原始对象设置成一个响应式对象，然后再次访问它，你获取到的是代理的版本。这可能会导致**对象身份风险**，即执行一个依赖于对象身份的操作，但却同时使用了同一对象的原始版本和代理版本：

```js	
const foo = markRaw({
  nested: {}
})

const bar = reactive({
  // 尽管 `foo` 被标记为了原始对象，但 foo.nested 却没有
  nested: foo.nested
})

console.log(foo.nested === bar.nested) // false
```

识别风险一般是很罕见的。然而，要正确使用这些 API，同时安全地避免这样的风险，需要你对响应性系统的工作方式有充分的了解。

#### `effectScope()`

创建一个 effect 作用域，可以捕获其中所创建的响应式副作用 (即计算属性和侦听器)，这样捕获到的副作用可以一起处理。对于该 API 的使用细节，请查阅对应的 [RFC](https://github.com/vuejs/rfcs/blob/master/active-rfcs/0041-reactivity-effect-scope.md)。

- **示例**

  ```js
  const scope = effectScope()
  
  scope.run(() => {
    const doubled = computed(() => counter.value * 2)
  
    watch(doubled, () => console.log(doubled.value))
  
    watchEffect(() => console.log('Count: ', doubled.value))
  })
  
  // 处理掉当前作用域内的所有 effect
  scope.stop()
  ```

在Vue的setup中，响应会在开始初始化的时候被收集，在实例被卸载的时候，响应就会自动的被取消追踪了，这时一个很方便的特性。但是，当我们在组件外使用或者编写一个独立的包时，这会变得非常麻烦。当在单独的文件中，我们该如何停止computed & watch的响应式依赖呢？

实际上EffectScope按我的理解就是副作用生效的作用域。

vue3对响应式的监听是通过effect实现的，当我们的组件销毁的时候vue会自动取消该组件的effect。

那么如果我们想要自己控制effect生效与否呢？ 比如我只想在莫种特定情况下才监听摸个ref，其他情况下不想监听该怎么做？可以使用 `effectScope()` 。

##### 示例

```js
const scope = effectScope()
let counter = ref(0)
setInterval(() => {
  counter.value++
}, 1000);
scope.run(() => {
  watchEffect(() =>console.log(`counter: ${counter.value}`))
})
/*log:
counter: 0
counter: 1
counter: 2
counter: 3
counter: 4
counter: 5
*/
```

```js
const scope = effectScope()
let counter = ref(0)
setInterval(() => {
  counter.value++
}, 1000);
scope.run(() => {
  watchEffect(() =>console.log(`counter: ${counter.value}`))
})
scope.stop()
/*log:
counter: 0
*/
```

##### 示例：Shared Composable

一些 composables 会设置全局副作用，例如如下的 useMouse() function:

```javascript
function useMouse() {
	const x = ref(0)
  const y = ref(0)
  
  window.addEventListener('mousemove', handler)
  
  function handler(e) {
  	x.value = e.x
    y.value = e.y
  }
  
  onUnmounted(() => {
  	window.removeEventListener('mousemove', handler)
  })
  
  return {x,y}
  
}
```

如果在多个组件中调用 useMouse () ，则每个组件将附加一个 mouseemove 监听器，并创建自己的 x 和 y refs 副本。我们应该能够通过在多个组件之间共享相同的侦听器集和 refs 来提高效率，但是我们做不到，因为每个 onUnmounted 调用都耦合到一个组件实例。

我们可以使用分离作用域和 onScopeDispose 来实现这一点, 首先，我们需要用 onScopeDispose 替换 onUnmounted

```scss
- onUnmounted(() => {
+ onScopeDispose(() => {
  window.removeEventListener('mousemove', handler)
})
```

这仍然有效，因为 Vue 组件现在也在作用域内运行其 setup () ，该作用域将在组件卸载时释放。

然后，我们可以创建一个工具函数来管理父范围订阅:

```scss
function createSharedComposable(composable) {
  let subscribers = 0
  let state, scope

  const dispose = () => {
    if (scope && --subscribers <= 0) {
      scope.stop()
      state = scope = null
    }
  }
	
 	// 这里只有在第一次运行的时候创建一个state, 后面所有的组件就不会再创建新的state，而是共用一个state
  return (...args) => {
    subscribers++
    if (!state) {
      scope = effectScope(true)
      state = scope.run(() => composable(...args))
    }
    onScopeDispose(dispose)
    return state
  }
}
```

现在我们就可以使用这个 shared 版本的 useMouse

```ini
const useSharedMouse = createSharedComposable(useMouse)
```

#### `getCurrentScope()`

如果有的话，返回当前活跃的 [effect 作用域](https://cn.vuejs.org/api/reactivity-advanced.html#effectscope)。

#### `onScopeDispose()`

在当前活跃的 [effect 作用域](https://cn.vuejs.org/api/reactivity-advanced.html#effectscope)上注册一个处理回调函数。当相关的 effect 作用域停止时会调用这个回调函数。

这个方法可以作为可复用的组合式函数中 `onUnmounted` 的替代品，它并不与组件耦合，因为每一个 Vue 组件的 `setup()` 函数也是在一个 effect 作用域中调用的。

```js
import { onScopeDispose } from 'vue'

const scope = effectScope()

scope.run(() => {
  onScopeDispose(() => {
    console.log('cleaned!')
  })
})

scope.stop() // logs 'cleaned!'
```



