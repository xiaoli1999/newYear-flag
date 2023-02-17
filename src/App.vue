<template>
    <div class="bubble-2023">2023</div>
    <div class="bubble-flag">
        <div class="bubble-flag-t">新年</div>
        <div class="bubble-flag-b">flag</div>
    </div>
    <div class="test">
        <span>暴富</span>
        <div></div>
    </div>
    <div class="flag-btn"><span>🌸</span></div>
</template>

<script lang="ts" setup>
import { ref, onMounted, nextTick, onBeforeUnmount } from 'vue'
import progress from './tools/progress'

/* 初始化进度条 */
progress.start()

/**
 * @function judgePC 判断是当前浏览器信息是否为pc
 * @return { Boolean } 返回是否是pc
 */
const judgePC = () => {
    let userAgent
    if (window && window.navigator) {
        userAgent = window.navigator.userAgent
    } else {
        return true
    }

    const agents = ['Android', 'iPhone', 'SymbianOS', 'Windows Phone', 'iPod', 'iPad']
    for (let i = 0; i < agents.length; i++) {
        if (userAgent.indexOf(agents[i]) >= 0) return false
    }
    return true
}

let isPc = ref<boolean>(judgePC())

const getInnerSize = () => ({ w: window.innerWidth, h: window.innerHeight })
let innerSize: { w: number, h: number } = { w: 0, h: 0 }


/**
 * @function createWaterBubble 生成随机小水泡
 * @description 生成随机小水泡及参数
 * @returns {{waterElement: HTMLDivElement, emoteParams: { left: number, bottom: number, opacity: number, fs: number }}}
 */
const createWaterBubble = () => {
    const sizeRange = isPc.value ? [14, 6] : [10, 6]
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

/**
 * @function setWaterBubbleAnimate 设置小水泡动画
 * @description 设置小水泡动画并延时销毁
 */
const setWaterBubbleAnimate = () => {
    const { waterElement, waterParams: { size, left  } } = createWaterBubble()

    document.body.append(waterElement)

    const leftRange = isPc.value ? [-80, 80] : [-40, 40]
    const endLeft = left + leftRange[Math.round(Math.random())]
    const endBottom = innerSize.h + size + Math.round(Math.random() * 10)
    const duration = (isPc.value ? 10000 : 12000) + Math.round(Math.random() * 4000)

    waterElement.animate([{ bottom: `${ endBottom }px`, left: `${ endLeft }px`, opacity: '0.24' }], { easing: 'linear', duration, fill: 'forwards', iterations: 1 })

    setTimeout(() => waterElement.remove(), duration + 400)
}

/* 内置flag标签 */
const flagList = ['暴富', '暴瘦', '脱单', '上岸', '加薪', '漫漫', '喜乐', '平安', '早睡', '早起', '升职', '退休', '躺平', '摸鱼', '搞钱', '发财', '温柔']
const selectFlagList = ref<string[]>(['暴富', '脱单', '上岸', '加薪', '漫漫', '喜乐', '升职', '搞钱'])
const flagColorList = ['#e093d3', '#f36b9b', '#f4f4f4ee', '#37c0fe', '#dd059c', '#f9edd5', '#2ae39d', '#aeadb1', '#84f9a6', '#f9ab1a', '#d49c7d', '#5baf70']

/**
 * @function createFlagBubble 生成随机flag泡泡
 * @description 生成随机flag泡泡及参数
 * @returns {{flagElement: HTMLDivElement, flagParams: { size: number, bottom: number, left: number }}}
 */
const createFlagBubble = () => {
    const fsRange = isPc.value ? [16, 20] : [12, 10]
    const fontSize = fsRange[0] + Math.round(Math.random() * fsRange[1])
    const size = fontSize * 3
    const left = Math.round(Math.random() * ((innerSize.w - (size / 2)) - (size / 2)))
    const bottom = -size
    const color = flagColorList[Math.round(Math.random() * (flagColorList.length - 1))]
    const offsetList = ['-1px', '0', '1px']
    const offsetX = offsetList[Math.round(Math.random() * (offsetList.length - 1))]
    const offsetY = offsetList[Math.round(Math.random() * (offsetList.length - 1))]
    const text = flagList[Math.round(Math.random() * (flagList.length - 1))]

    const flagElement: HTMLElement = document.createElement('div')
    flagElement.className = 'flag'
    flagElement.style.position = 'absolute'
    flagElement.style.width = `${ size }px`
    flagElement.style.height = `${ size }px`
    flagElement.style.bottom = `${ bottom }px`
    flagElement.style.left = `${ left }px`
    flagElement.style.fontSize = `${ fontSize }px`
    flagElement.style.color = `${ color }`
    flagElement.style.fontWeight = '700'
    flagElement.style.textAlign = 'center'
    flagElement.style.lineHeight = `${ size }px`
    flagElement.style.borderRadius = '50%'
    flagElement.style.boxShadow = `${ offsetX } ${ offsetY } ${ size / 3.5 }px 1px ${ color } inset`
    flagElement.style.zIndex = '100'
    flagElement.innerText = text

    return { flagElement, flagParams: { size, left, bottom } }
}

/**
 * @function setFlagBubbleAnimate 设置flag泡泡动画
 * @description 设置flag泡泡动画并延时销毁
 */
const setFlagBubbleAnimate = () => {
    const { flagElement, flagParams: { size, left  } } = createFlagBubble()

    document.body.append(flagElement)

    const leftRange = isPc.value ? [-80, 80] : [-40, 40]
    const endLeft = left + leftRange[Math.round(Math.random())]
    const endBottom = innerSize.h + size
    const duration = (isPc.value ? 8400 : 9600) + Math.round(Math.random() * 8000)

    flagElement.animate([{ bottom: `${ endBottom }px`, left: `${ endLeft }px` }], { easing: 'linear', duration, fill: 'forwards', iterations: 1 })

    setTimeout(() => flagElement.remove(), duration + 400)
}

const adjustDevice = () => {
    innerSize = getInnerSize()
    isPc.value = innerSize.w > 768
}

let timer
const start = () => {
    let n = 0
    clearInterval(timer)
    timer = setInterval(() => {
        setWaterBubbleAnimate()
        setFlagBubbleAnimate()
        if (n % 5 === 0) adjustDevice()
        n++
    }, isPc.value ? 420 : 840)
}

onMounted(() => {
    progress.close()

    adjustDevice()

    // nextTick(() => start())
})

onBeforeUnmount(() => clearInterval(timer))
</script>

<style lang="less">
.test {
    position: relative;
    width: 80px;
    height: 80px;
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 24px;
    font-weight: 600;
    margin: 480px auto 0;
    border-radius: 50%;
    color: #f4f4f4;

    > span {
        position: absolute;
        display: inline-block;
        width: 100%;
        height: 100%;
        display: flex;
        justify-content: center;
        align-items: center;
        left: 0;
        top: 0;
        right: 0;
        bottom: 0;
        margin: auto;
        //animation: test 2.4s linear infinite;
    }

    > div {
        position: absolute;
        width: 100%;
        height: 100%;
        border-radius: 50%;
        box-shadow: -2px -2px 16px 1px #f4f4f4 inset;
        animation: test-text 2s linear infinite;
        opacity: 1;
    }
}



@keyframes test {
    0%, 100% {
        opacity: 1;
    }

    50% {
        opacity: .6;

    }
}

@keyframes test-text {
    50% {
        //border-radius: 24%;
    }
    100% {
        transform: rotateZ(1turn);
    }
}
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
    font-size: 36px;
    color: #f4f4f4cc;
    background: #ffffff20;
    border-radius: 54% 46% 50% 50% / 52% 65% 35% 48%;
    box-shadow: 0 0 16px 1px #f4f4f4aa inset;
    backdrop-filter: blur(0.5px);
    font-weight: 600;
    letter-spacing: 3px;
    transition: all 0.24s;
    animation: bubble-2023 2.8s linear infinite;
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
    transition: all 0.24s;
    animation: bubble-flag 2s linear infinite;

    > div {
        display: flex;
        justify-content: center;
        align-items: center;
        height: 30px;
        font-size: 26px;
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

.flag-btn {
    position: fixed;
    right: -10px;
    bottom: -10px;
    z-index: 101;
    width: 80px;
    height: 80px;
    border-radius: 100% 0 0;
    box-shadow: 1px 1px 8px 1px #f7f8facc inset;
    backdrop-filter: blur(1px);
    cursor: pointer;
    animation: flag-btn 6s linear infinite;

    > span {
        position: relative;
        top: 42%;
        left: 36%;
        display: inline-block;
        padding: 1px 0 1.5px 0.5px;
        font-size: 26px;
        text-align: center;
        transition: font-size 0.24s;
        transform-origin: center;
        animation: flag-btn-text 2.4s linear infinite;
    }

    &::after {
        position: absolute;
        right: 0;
        bottom: 0;
        z-index: -1;
        display: block;
        width: 100%;
        height: 100%;
        border-radius: 100% 0 0;
        box-shadow: 1px 1px 8px 1px #f4f4f4ee inset;
        content: "";
        animation: flag-btn-wrap 1.5s linear infinite;
        backdrop-filter: blur(1px);
        cursor: default;
    }
}

@keyframes flag-btn {
    0%,
    100% {
        box-shadow: 1px 1px 8px 1px #f4f4f4ee inset;
    }

    16% {
        box-shadow: 1px 1px 8px 1px #37c0fe inset;
    }

    32% {
        box-shadow: 1px 1px 8px 1px #dd059c inset;
    }

    50% {
        box-shadow: 1px 1px 8px 1px #f9edd5 inset;
    }

    68% {
        box-shadow: 1px 1px 8px 1px #d49c7d inset;
    }

    84% {
        box-shadow: 1px 1px 8px 1px #aeadb1 inset;
    }
}

@keyframes flag-btn-wrap {
    100% {
        width: 160%;
        height: 160%;
        opacity: 0;
    }
}

@keyframes flag-btn-text {
    100% {
        transform: rotateZ(1turn);
    }
}

@media only screen and (max-width: 768px) {
    .bubble-2023 {
        top: 2%;
        left: 8%;
        transform: scale(0.6);
    }

    .bubble-flag {
        top: 8%;
        right: 8%;
        transform: scale(0.6);
    }

    .flag-btn {
        transform: scale(0.8);
    }
}
</style>
