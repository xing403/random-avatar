<script setup lang="ts">
const props = defineProps<{
  src?: string
}>()
const src = toRef(props, 'src')
</script>

<template>
  <div>
    <img :src="src" alt="avatar">
  </div>
</template>
