<template>
    flag测试
</template>

<script lang="ts" setup>
import { ref, onMounted } from 'vue'
import { judgePC } from '@/tools/common'
import progress from './tools/progress'
import { ElMessage } from 'element-plus'


/* 初始化进度条 */
progress.start()

const isPc = ref<boolean>(judgePC())


onMounted(() => {
    progress.close()
})
</script>

<style lang="less" scoped>
</style>
