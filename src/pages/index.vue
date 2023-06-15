<script setup lang="ts" generic="T extends any, O extends any">
import { ElMessage } from 'element-plus'

const timestamp = ref(0)
const value = ref('')
const avatar = ref('avatar_')
function handleChange(e: any) {
  ElMessage.success('加载成功')
}
function randomAvatar(len: number) {
  const time = new Date().getTime()
  if (time - timestamp.value <= 5000) {
    ElMessage.warning(`${(5 - (time - timestamp.value) / 1000).toFixed(1)}s 后再试`)
    return
  }
  const SALT = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789'
  value.value = Array.from({ length: len }, (_, index) => {
    return SALT[RandomInt(0, SALT.length)]
  }).join('')
  timestamp.value = time
  avatar.value = `avatar_${value.value}`
}
function Random(min: number, max: number) {
  return Math.random() * (max - min) + min
}
function RandomInt(min: number, max: number) {
  return Math.floor(Random(min, max))
}
</script>

<template>
  <div ma w-100 pt-12 text-center>
    <el-image class="h-60 w-60" :src="`https://api.multiavatar.com/${avatar}.png`" @load="handleChange">
      <template #placeholder>
        <div i-svg-spinners-bars-scale m-20 h-20 w-20 icon-btn />
      </template>
    </el-image>

    <div pt-10>
      <el-input v-model="value" size="large" placeholder="" clearable />
      <div flex="~ row" mt-2 justify="center">
        <el-tooltip content="下载" placement="bottom">
          <div i-carbon-download mr-1 h-6 w-6 icon-btn />
        </el-tooltip>
        <el-tooltip content="随机" placement="bottom">
          <div i-carbon-update-now mr-1 h-6 w-6 icon-btn @click="randomAvatar(16)" />
        </el-tooltip>
        <el-tooltip content="主题色" placement="bottom">
          <div i-carbon-sun dark:i-carbon-moon mr-1 h-6 w-6 icon-btn @click="toggleDark()" />
        </el-tooltip>
      </div>
    </div>
  </div>
</template>
