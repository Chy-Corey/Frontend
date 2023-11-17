<template>
  <div>
    {{ num }}
  </div>
</template>

<script setup>
  import {ref} from 'vue'
  const num = ref(0)

  defineExpose({
    num
  })
</script>