## Vue 3 api

[API 参考 | Vue.js (vuejs.org)](https://cn.vuejs.org/api/)

记录了一些常用的 api。

### 一、全局 api

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

