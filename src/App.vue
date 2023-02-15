<template>
    <div class="bubble-2023">2023</div>
    <div class="bubble-flag">
        <div class="bubble-flag-t">新年</div>
        <div class="bubble-flag-b">flag</div>
    </div>
</template>

<script lang="ts" setup>
import { ref, onMounted, nextTick, onBeforeUnmount } from 'vue'
import { judgePC } from '@/tools/common'
import progress from './tools/progress'
import { ElMessage } from 'element-plus'

/* 初始化进度条 */
progress.start()

let isPc = ref<boolean>(judgePC())

const getInnerSize = () => ({ w: window.innerWidth, h: window.innerHeight })
let innerSize: { w: number, h: number } = { w: 0, h: 0 }


/**
 * @function createWaterBubble 生成随机小水泡
 * @description 生成随机小水泡及参数
 * @returns {{waterElement: HTMLDivElement, emoteParams: { left: number, bottom: number, opacity: number, fs: number }}}
 */
const createWaterBubble = () => {
    const sizeRange = isPc.value ? [16, 10] : [12, 8]
    const size = sizeRange[0] + Math.round(Math.random() * sizeRange[1])
    const left = Math.round(Math.random() * ((innerSize.w - (size / 2)) - (size / 2)))
    const bottom = -size
    const opacity = Number(((Math.random() * 36 + 72) / 100).toFixed(2))

    const waterElement: HTMLElement = document.createElement('div')
    waterElement.className = 'water'
    waterElement.style.position = 'absolute'
    waterElement.style.width = `${ size }px`
    waterElement.style.height = `${ size }px`
    waterElement.style.bottom = `${ bottom }px`
    waterElement.style.left = `${ left }px`
    waterElement.style.borderRadius = '50%'
    waterElement.style.boxShadow = `0 0 ${ size / 5 }px 1px #ffffffaa inset, -1.5px -1.5px ${ size / 10 }px 0 #ffffff80 inset`
    waterElement.style.zIndex = '1'

    return { waterElement, waterParams: { size, left, bottom, opacity } }
}

/* 设置小泡泡动画 */
const setWaterBubbleAnimate = () => {
    const { waterElement, waterParams: { size, left  } } = createWaterBubble()

    document.body.append(waterElement)

    const leftRange = isPc.value ? [-80, 80] : [-40, 40]
    const endLeft = left + leftRange[Math.round(Math.random())]
    // console.log(waterElement.style.height)
    const endBottom = innerSize.h + size + Math.round(Math.random() * 10)
    // console.log(endBottom)
    const duration = (isPc.value ? 8400 : 9600) + Math.round(Math.random() * 4000)

    waterElement.animate([{ bottom: `${ endBottom }px`, left: `${ endLeft }px`, opacity: '0.24' }], { easing: 'linear', duration, fill: 'forwards', iterations: 1 })

    setTimeout(() => waterElement.remove(), duration + 400)
}

/* 内置flag标签 */
const flagList = ['暴富', '暴瘦', '脱单', '上岸', '加薪', '漫漫', '喜乐', '平安', '早睡', '早起', '升职', '退休', '躺平', '摆烂', '摸鱼', '搞钱', '发财', '温柔', '安康']
const selectFlagList = ref<string[]>(['暴富', '脱单', '上岸', '加薪', '漫漫', '喜乐', '升职', '搞钱'])
/* 创建flag泡泡 */
/**
 * @function createWaterBubble 生成随机小水泡
 * @description 生成随机小水泡及参数
 * @returns {{waterElement: HTMLDivElement, emoteParams: { left: number, bottom: number, opacity: number, fs: number }}}
 */
const createFlagBubble = () => {
    const sizeRange = isPc.value ? [16, 10] : [12, 8]
    const size = sizeRange[0] + Math.round(Math.random() * sizeRange[1])
    const left = Math.round(Math.random() * ((innerSize.w - (size / 2)) - (size / 2)))
    const bottom = -size
    const opacity = Number(((Math.random() * 36 + 72) / 100).toFixed(2))

    const flagElement: HTMLElement = document.createElement('div')
    flagElement.className = 'water'
    flagElement.style.position = 'absolute'
    flagElement.style.width = `${ size }px`
    flagElement.style.height = `${ size }px`
    flagElement.style.bottom = `${ bottom }px`
    flagElement.style.left = `${ left }px`
    flagElement.style.borderRadius = '50%'
    flagElement.style.boxShadow = `0 0 ${ size / 5 }px 1px #ffffffaa inset, -1.5px -1.5px ${ size / 10 }px 0 #ffffff80 inset`
    flagElement.style.zIndex = '1'

    return { flagElement, flagParams: { size, left, bottom, opacity } }
}

/* 设置小泡泡动画 */
const setflagBubbleAnimate = () => {
    const { flagElement, flagParams: { size, left  } } = createFlagBubble()

    document.body.append(flagElement)

    const leftRange = isPc.value ? [-80, 80] : [-40, 40]
    const endLeft = left + leftRange[Math.round(Math.random())]
    // console.log(waterElement.style.height)
    const endBottom = innerSize.h + size + Math.round(Math.random() * 10)
    // console.log(endBottom)
    const duration = (isPc.value ? 8400 : 9600) + Math.round(Math.random() * 4000)

    flagElement.animate([{ bottom: `${ endBottom }px`, left: `${ endLeft }px`, opacity: '0.24' }], { easing: 'linear', duration, fill: 'forwards', iterations: 1 })

    setTimeout(() => flagElement.remove(), duration + 400)
}
/* 设置flag泡泡动画 */

let timer
const start = () => {
    let n = 0
    timer = setInterval(() => {
        setWaterBubbleAnimate()
        setflagBubbleAnimate()
        if (n % 5 === 0) innerSize = getInnerSize()
        n++
    }, isPc.value ? 420 : 840)
}

onMounted(() => {
    progress.close()

    innerSize = getInnerSize()
    if (innerSize.w <= 768) isPc.value = false

    nextTick(() => start())
})

onBeforeUnmount(() => clearInterval(timer))
</script>

<style lang="less">
.bubble-2023 {
    position: absolute;
    top: 5%;
    left: 10%;
    z-index: 99;
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 20px;
    width: 80px;
    height: 80px;
    font-size: 28px;
    color: #f4f4f4cc;
    background: #ffffff20;
    border-radius: 54% 46% 50% 50% / 52% 65% 35% 48%;
    box-shadow: 0 0 16px 1px #f4f4f4aa inset;
    backdrop-filter: blur(0.5px);
    animation: bubble-2023 2.8s linear infinite;
    font-weight: 600;
    letter-spacing: 3px;
}

.bubble-flag {
    position: absolute;
    top: 12%;
    right: 10%;
    z-index: 99;
    display: flex;
    padding: 20px;
    width: 60px;
    height: 60px;
    background: #ffffff20;
    border-radius: 40% 60% 34% 66% / 60% 51% 49% 40%;
    box-shadow: -1px 0 12px 1px #f4f4f4aa inset;
    backdrop-filter: blur(0.5px);
    flex-direction: column;
    animation: bubble-flag 2s linear infinite;

    > div {
        display: flex;
        justify-content: center;
        align-items: center;
        height: 30px;
        font-size: 20px;
        color: #f4f4f4cc;
        font-weight: 600;
        letter-spacing: 3px;
    }
}

@keyframes bubble-2023 {
    0%,
    100% {
        border-radius: 54% 46% 50% 50% / 52% 65% 35% 48%;
    }

    33% {
        border-radius: 76% 24% 27% 73% / 52% 39% 61% 48%;
    }

    50% {
        background: #ffffff48;
    }

    66% {
        border-radius: 39% 61% 69% 31% / 66% 56% 44% 34%;
    }
}

@keyframes bubble-flag {
    0%,
    100% {
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
</style>
