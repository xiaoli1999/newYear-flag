<template>
    <div class="bubble-2023">2023</div>
    <div class="bubble-flag">
        <div class="bubble-flag-t">新年</div>
        <div class="bubble-flag-b">flag</div>
    </div>
    <div class="bubble-small"></div>
    flag测试
</template>

<script lang="ts" setup>
import { ref, onMounted } from 'vue'
import { judgePC } from '@/tools/common'
import progress from './tools/progress'
import { ElMessage } from 'element-plus'

/* 初始化进度条 */
progress.start()

let isPc = ref<boolean>(judgePC())

const getInnerSize = () => ({ w: window.innerWidth, h: window.innerHeight })
let innerSize: { w: number, h: number } = { w: 0, h: 0 }

/* 创建小泡泡 */

/* 设置小泡泡动画 */

/* 内置flag标签 */
const flagList = ['暴富', '暴瘦', '脱单', '上岸', '加薪', '漫漫', '喜乐', '平安', '早睡', '早起', '升职', '退休', '躺平', '摆烂', '摸鱼', '搞钱', '发财']

/* 创建flag泡泡 */

/* 设置flag泡泡动画 */

/* 销毁小泡泡、flag泡泡 */




onMounted(() => {
    progress.close()

    innerSize = getInnerSize()
    if (innerSize.w <= 768) isPc.value = false
    console.log(getInnerSize())
})
</script>

<style lang="less">
.bubble-2023 {
    position: absolute;
    width: 80px;
    height: 80px;
    top: 5%;
    left: 10%;
    padding: 20px;
    border-radius: 54% 46% 50% 50% / 52% 65% 35% 48% ;
    background: #ffffff20;
    backdrop-filter: blur(.5px);
    box-shadow: 0 0 16px 1px #f4f4f4aa inset;
    display: flex;
    justify-content: center;
    align-items: center;
    animation: bubble-2023 2.8s linear infinite;
    color: #f4f4f4cc;
    font-size: 28px;
    font-weight: 600;
    letter-spacing: 3px;
    z-index: 99;
}

.bubble-flag {
    position: absolute;
    width: 60px;
    height: 60px;
    top: 12%;
    right: 10%;
    padding: 20px;
    border-radius: 40% 60% 34% 66% / 60% 51% 49% 40%;
    background: #ffffff20;
    backdrop-filter: blur(.5px);
    box-shadow: -1px 0 12px 1px #f4f4f4aa inset;
    display: flex;
    flex-direction: column;
    animation: bubble-flag 2s linear infinite;
    z-index: 99;

    > div {
        height: 30px;
        display: flex;
        justify-content: center;
        align-items: center;
        color: #f4f4f4cc;
        font-size: 20px;
        font-weight: 600;
        letter-spacing: 3px;
    }
}

@keyframes bubble-2023 {
    0%, 100% {
        border-radius: 54% 46% 50% 50% / 52% 65% 35% 48% ;
    }

    33% {
        border-radius: 76% 24% 27% 73% / 52% 39% 61% 48% ;
    }

    50% {
        background: #ffffff48;
    }

    66% {
        border-radius: 39% 61% 69% 31% / 66% 56% 44% 34%  ;
    }
}


@keyframes bubble-flag {
    0%, 100% {
        border-radius: 40% 60% 34% 66% / 60% 51% 49% 40%;
    }

    33% {
        border-radius: 61% 39% 33% 67% / 60% 56% 44% 40%;
    }

    50% {
        background: #ffffff48;
    }

    66% {
        border-radius: 30% 70% 77% 23% / 19% 56% 44% 81%;
    }
}

.bubble-small {
    position: absolute;
    bottom: 120px;
    left: 140px;
    width: 20px;
    height: 20px;
    border-radius: 50%;
    z-index: 1;
    box-shadow: 0 0 4px 1px #ffffffaa inset, -1.5px -1.5px 2px 0 #ffffffaa inset;
    opacity: 1;
    animation: bubble-small 2.8s linear forwards;
}

@keyframes bubble-small {
    100% {
        opacity: .4;
    }
}
</style>
