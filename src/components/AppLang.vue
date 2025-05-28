<script setup lang="ts">
    const langCurrent = defineModel('langSelect')
    const radioName: string = 'lang'
</script>

<template>
    <div class="lang">
        <label for="lang-en" class="lang-btn">
            <input v-model="langCurrent" :name="radioName" id="lang-en" type="radio" value="english" checked>
            English
        </label>
        <label for="lang-ja" class="lang-btn">
            <input v-model="langCurrent" :name="radioName" id="lang-ja" type="radio" value="japanese">
            日本語
        </label>
    </div>
</template>

<style>
    .lang {
        display: grid;
        grid-template-columns: 1fr 1fr;
        margin-bottom: 4vw;
    }

    .lang-btn {
        position: relative;
        display: flex;
        justify-content: center;
        align-items: center;
        padding: 1em 1.5em;
        color: var(--white);
        cursor: pointer;
    }

    .lang-btn:has(input:checked) {
        cursor: default;
    }

    .lang-btn::before,
    .lang-btn::after {
        content: "";
        position: absolute;
        width: 100%;
        height: 100%;
        mix-blend-mode: overlay;
        top: 0;
        left: 0;
    }

    .lang-btn::before {
        background-color: #afafaf;
    }

    .lang-btn:has(input:checked)::before {
        background-color: var(--gray);
    }

    .lang-btn::after {
        box-shadow: 0 0 0 2px var(--gray) inset;
        opacity: 0;
    }

    .lang-btn:has(input:checked)::after {
        opacity: 1;
    }

    .lang-btn input {
        appearance: none;
        -webkit-appearance: none;
        -moz-appearance: none;
        margin: 0;
    }
</style>

<style media="(hover: hover) and (pointer: fine)">
    .lang-btn:hover:has(input:not(:checked))::before {
        background-color: var(--gray);
    }
</style>