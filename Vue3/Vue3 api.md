## Vue 3 api

[API 参考 | Vue.js (vuejs.org)](https://cn.vuejs.org/api/)

记录了一些常用的 api。

### 一、全局 api

#### 1. nextTick()

等待下一次 DOM 更新刷新的工具方法。

- **类型**

  ts

  ```
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



### 二、组合式 api

