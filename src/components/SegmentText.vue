<script setup lang="ts">
    import { computed } from 'vue'

    const props = defineProps<{
        now: string[]
        nowType: string
        size: number
        weight: number | undefined
    }>()

    // 0～9の形を表現
    const classDisplay = (num: string): string => `seg${num}`

    // 各セグメントの位置
    const segType: string[] = ['a','b','c','d','e','f','g']
    const segPos = (type: string): string => `seg-${type}`

    // コロンを付与
    const classSymbol = (type: string, i: number): string | void => {
        if (type === 'hm' && i === 1) return 'colon'
    }

    // 文字サイズ
    const classSize = (size: number): string => {
        let className
        switch (size) {
            case 1:
                className = 'large'
                break
            case 0.5:
                className = 'medium'
                break
        }
        return `seg-${className}`
    }

    const segLayer: string[] = ['parent', 'child']
    const segProperty = computed(() => (size: number, layer: string) => {
        if (props.weight) {
            const weight: number = Math.round(props.weight * size)
            const gap: number = Math.round(weight / 4)
            const half: number = Math.round(weight / 2)

            if (layer === segLayer[0]) {
                return {
                    '--seg-margin': `${weight}px`
                }
            } else {
                return {
                    '--seg-weight': `${weight}px`,
                    '--seg-gap': `${gap}px`,
                    '--seg-half-a': `${half}px`,
                    '--seg-half-b': `calc(100% - ${half}px)`
                }
            }
        }
    })
</script>

<template>
    <div :class="classSize(props.size)" class="seg-area">
        <div v-for="(item, index) in props.now" :style="segProperty(props.size, segLayer[0])" :class="classDisplay(item), classSymbol(props.nowType, index)" class="seg">
            <span v-for="type in segType" :style="segProperty(props.size, segLayer[1])" :class="segPos(type)" class="seg-bar"></span>
        </div>
    </div>
</template>

<style>
    .seg-area {
        display: flex;
    }

    .seg {
        position: relative;
        flex: 1;
        aspect-ratio: 9 / 16;
        margin-right: var(--seg-margin);
    }

    .seg-bar {
        position: absolute;
    }

    /* セグメント（横） */
    .seg-a, .seg-d, .seg-g {
        width: calc(100% - var(--seg-weight) - var(--seg-gap));
        height: var(--seg-weight);
        clip-path: polygon(0 50%, var(--seg-half-a) 0, var(--seg-half-b) 0, 100% 50%, var(--seg-half-b) 100%, var(--seg-half-a) 100%);
    }

    .seg-a {
        top: 0;
        translate: -50% 0;
    }

    .seg-d {
        bottom: 0;
        translate: -50% 0;
    }

    .seg-g {
        top: 50%;
        translate: -50% -50%;
    }

    .seg-a, .seg-d, .seg-g {
        left: 50%;
    }

    /* セグメント（縦） */
    .seg-b, .seg-c, .seg-e, .seg-f {
        width: var(--seg-weight);
        height: calc(50% - calc(var(--seg-weight) / 2) - var(--seg-gap));
        clip-path: polygon(50% 0, 100% var(--seg-half-a), 100% var(--seg-half-b), 50% 100%, 0 var(--seg-half-b), 0 var(--seg-half-a));
    }

    .seg-b, .seg-c {
        right: 0;
    }

    .seg-e, .seg-f {
        left: 0;
    }

    .seg-b, .seg-f {
        top: calc(var(--seg-half-a) + var(--seg-gap) / 2);
    }

    .seg-c, .seg-e {
        bottom: calc(var(--seg-half-a) + var(--seg-gap) / 2);
    }

    /* 0 */
    .seg0 .seg-bar:not(.seg-g) {
        background-color: var(--white);
    }

    /* 1 */
    .seg1 :is(.seg-b, .seg-c) {
        background-color: var(--white);
    }

    /* 2 */
    .seg2 .seg-bar:not(:is(.seg-c, .seg-f)) {
        background-color: var(--white);
    }

    /* 3 */
    .seg3 .seg-bar:not(:is(.seg-e, .seg-f)) {
        background-color: var(--white);
    }

    /* 4 */
    .seg4 :is(.seg-b, .seg-c, .seg-f, .seg-g) {
        background-color: var(--white);
    }

    /* 5 */
    .seg5 .seg-bar:not(:is(.seg-b, .seg-e)) {
        background-color: var(--white);
    }

    /* 6 */
    .seg6 .seg-bar:not(.seg-b) {
        background-color: var(--white);
    }

    /* 7 */
    .seg7 :is(.seg-a, .seg-b, .seg-c) {
        background-color: var(--white);
    }

    /* 8 */
    .seg8 .seg-bar {
        background-color: var(--white);
    }

    /* 9 */
    .seg9 .seg-bar:not(.seg-e) {
        background-color: var(--white);
    }

    /* コロン */
    .colon {
        position: relative;
        margin-right: calc(var(--seg-margin) * 4);
    }

    .colon::before,
    .colon::after {
        content: "";
        position: absolute;
        width: var(--seg-margin);
        height: var(--seg-margin);
        background-color: var(--white);
        left: calc(100% + var(--seg-margin) * 2);
        translate: -50% 0;
    }

    .colon::before {
        top: calc(var(--seg-margin) * 1.5);
    }

    .colon::after {
        bottom: calc(var(--seg-margin) * 1.5);
    }
</style>