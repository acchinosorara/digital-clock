<script setup lang="ts">
import { computed } from 'vue'

const props = defineProps<{
    date: string
    day: number
    days: string[][]
    lang: string
}>()

// 現在の曜日を出力
const daysEn: string[] = props.days[0].map((day) => day.slice(0, 3).toUpperCase())
const dayText = computed(() => (i: number) => {
    if (!props.lang) return daysEn[i]
    switch(props.lang) {
        case 'english':
            return daysEn[i]
        case 'japanese':
            return props.days[1][i]
    }
})
</script>

<template>
    <p>{{ props.date }}</p>
    <p>{{ dayText(props.day) }}</p>
</template>

<style>
    p {
        font-family: "Zen Maru Gothic", sans-serif;
        color: var(--white);
        font-size: clamp(16px, 6vw, 40px);
        letter-spacing: 0.1em;
        line-height: 1;
        margin: 0;
    }
</style>