<script setup lang="ts">
import { ElMessage } from 'element-plus'

const value = ref('')
function handleChange(e: any) {
  ElMessage.success('加载成功')
}
</script>

<template>
  <div ma w-100 pt-12 text-center>
    <el-image class="h-60 w-60" :src="`https://api.multiavatar.com/avatar_${value}.png`" @load="handleChange">
      <template #placeholder>
        <div i-svg-spinners-bars-scale m-20 h-20 w-20 icon-btn />
      </template>
      <template #error>
        <div i-svg-spinners-bars-scale m-20 h-20 w-20 icon-btn />
      </template>
    </el-image>

    <div pt-10>
      <el-input v-model="value" size="large" placeholder="" clearable />
      <div flex="~ row" mt-2 justify="center">
        <el-tooltip content="主题色" placement="bottom">
          <div i-carbon-sun dark:i-carbon-moon mr-1 h-6 w-6 icon-btn @click="toggleDark()" />
        </el-tooltip>
      </div>
    </div>
  </div>
</template>
