<script setup lang="ts" generic="T extends any, O extends any">
import { invoke, until, useClipboard } from '@vueuse/core'
import { ElMessage, ElNotification } from 'element-plus'

const timestamp = ref(0)
const value = ref('')
const avatar = ref('')
function handleChangeAvatar(e: any) {
  ElMessage.success('加载成功')
}
const { text, copy, isSupported } = useClipboard()
function randomAvatar(len: number) {
  const time = new Date().getTime()
  if (time - timestamp.value <= 5000) {
    ElMessage.info(`${(5 - (time - timestamp.value) / 1000).toFixed(1)}s 后再试`)
    return
  }
  const SALT = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789'
  value.value = Array.from({ length: len }, (_, index) => {
    return SALT[RandomInt(0, SALT.length)]
  }).join('')
  timestamp.value = time
  avatar.value = `https://api.multiavatar.com/${value.value}.png`
}
function Random(min: number, max: number) {
  return Math.random() * (max - min) + min
}
function RandomInt(min: number, max: number) {
  return Math.floor(Random(min, max))
}
function handleChangeInput(_avatar: string) {
  timestamp.value = new Date().getTime()
  value.value = _avatar
  avatar.value = `https://api.multiavatar.com/${_avatar}.png`
}
function handleDownloadAvatar() {
  fetch(avatar.value)
    .then(response => response.blob())
    .then((blob) => {
      const url = URL.createObjectURL(blob)
      const link = document.createElement('a')
      link.href = url
      link.download = `${value.value}.png`
      link.click()
    })
}
randomAvatar(16)
invoke(async () => {
  await until(text).changed()

  ElNotification.success({
    title: '链接复制成功',
    message: `${avatar.value}`,
    duration: 2000,
  })
})
</script>

<template>
  <div ma w-100 pt-12 text-center>
    <el-image class="h-60 w-60" :src="avatar" :alt="value" :title="value" @load="handleChangeAvatar">
      <template #placeholder>
        <div i-svg-spinners-bars-scale m-20 h-20 w-20 icon-btn />
      </template>
    </el-image>

    <div pt-10>
      <el-input v-model="value" size="large" placeholder="" clearable @input="handleChangeInput" />
      <div flex="~ row" mt-5 justify="center">
        <el-tooltip content="下载" placement="bottom">
          <button i-carbon-download mr-2 h-6 w-6 icon-btn @click="handleDownloadAvatar" />
        </el-tooltip>
        <el-tooltip v-if="isSupported" content="复制链接" placement="bottom">
          <button i-carbon-copy-file mr-2 h-6 w-6 icon-btn @click="copy(avatar)" />
        </el-tooltip>
        <el-tooltip content="随机刷新" placement="bottom">
          <button i-carbon-update-now mr-2 h-6 w-6 icon-btn @click="randomAvatar(16)" />
        </el-tooltip>
        <el-tooltip content="主题色" placement="bottom">
          <button i-carbon-sun dark:i-carbon-moon mr-2 h-6 w-6 icon-btn @click="toggleDark()" />
        </el-tooltip>
      </div>
    </div>
  </div>
</template>
