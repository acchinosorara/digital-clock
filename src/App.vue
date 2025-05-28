<script setup lang="ts">
  import { ref, computed, watch, useTemplateRef } from 'vue'
  import { useNow, useElementSize } from '@vueuse/core'
  import Segment from './components/SegmentText.vue'
  import Day from './components/DayText.vue'
  import Lang from './components/AppLang.vue'
  import Read from './components/ScreenReader.vue'

  // 選択中の言語
  const lang = ref('')

  const now = useNow()
  const nowType: string[] = ['hm', 's']
  const str = (num: number): string => num.toString()

  // 数字が1桁の場合、先頭に0を付与
  const zero = (num: number): string => str(num).padStart(2, '0')

  // 時・分
  const nowTime = computed(() =>
    zero(now.value.getHours()) +
    zero(now.value.getMinutes())
  )
  const time = computed(() => nowTime.value.split(''))

  // 秒
  const nowSecond = computed(() => zero(now.value.getSeconds()))
  const second = computed(() => nowSecond.value.split(''))

  // 年・月・日
  const date = computed(() => {
    switch (lang.value) {
      case 'japanese':
        return str(now.value.getFullYear()) + '/' + zero(now.value.getMonth() + 1) + '/' + zero(now.value.getDate())
      default:
        return zero(now.value.getMonth() + 1) + '/' + zero(now.value.getDate()) + '/' + str(now.value.getFullYear())
    }
  })

  // 曜日
  const day = computed(() => now.value.getDay())
  const dayCharacters: string[][] = [
    ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'],
    ['日', '月', '火', '水', '木', '金', '土']
  ]

  // リサイズ対応のセグメント幅
  const segWeight = ref<number>()
  const segArea = useTemplateRef('segArea')
  const { width } = useElementSize(segArea)
  watch(width, (newVal) => {
    segWeight.value = Math.round(newVal / 24)
  })

  // セグメント文字サイズ
  interface size {
    l: number,
    m: number
  }
  const textSize: size = {
    l: 1,
    m: 0.5
  }
</script>

<template>
  <article class="wrapper">
    <div class="h-m-s">
      <Segment ref="segArea" :now="time" :nowType="nowType[0]" :size="textSize.l" :weight="segWeight"/>
      <Segment :now="second" :nowType="nowType[1]" :size="textSize.m" :weight="segWeight"/>
    </div>
    <div class="y-m-d" aria-hidden="true">
      <Day :date="date" :day="day" :days="dayCharacters" :lang="lang"/>
    </div>
  </article>
  <Lang v-model:langSelect="lang" />
  <Read :time="time" :day="day" :days="dayCharacters" :date="date" :lang="lang" />
</template>