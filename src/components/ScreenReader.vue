<script setup lang="ts">
    const props = defineProps<{
        time: string[]
        day: number
        days: string[][]
        date: string
        lang: string
    }>()

    // 時・分
    const readTime = (): string => {
        const hm: string = props.time.join('')
        switch (props.lang) {
            case 'japanese':
                return hm.slice(0, 2) + '時' + hm.slice(2, 4) + '分'
            default:
                return hm.slice(0, 2) + ':' + hm.slice(2, 4)
        }
    }

    // 曜日
    const readDay = (): string => {
        let i: number = 0
        let end: string = ''
        switch (props.lang) {
            case 'japanese':
                i = 1
                end = '曜日'
                break
        }
        return props.days[i][props.day] + end
    }

    // 年・月・日
    const readDate = (): string => {
        const ymd = props.date
        switch (props.lang) {
            case 'japanese':
                return ymd.slice(0, 4) + '年' + ymd.slice(5, 7) + '月' + ymd.slice(8, 10) + '日'
            default:
                return ymd.slice(0, 2) + '-' + ymd.slice(3, 5) + '-' + ymd.slice(6, 10)
        }
    }

    // 読み上げるテキストを出力
    const read = (): string => {
        switch (props.lang) {
            case 'japanese':
                return readTime() + readDate() + readDay()
            default:
                return readTime() + ',' + readDay() + ',' + readDate()
        }
    }
</script>

<template>
    <span class="read-text">{{ read() }}</span>
</template>

<style>
    .read-text {
        position: absolute;
        scale: 0 0;
        bottom: 0;
        left: 50%;
        translate: -50% 0;
    }
</style>