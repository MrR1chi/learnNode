<script setup>
import { ref } from 'vue';

const selectedColor = ref('#ffffff');
const errorMessage = ref('');

async function pickColor() {
    errorMessage.value = '';

    if (!('EyeDropper' in window)) {
        errorMessage.value = 'EyeDropper API is not supported in this browser.';
        return;
    }

    try {
        const eyeDropper = new window.EyeDropper();
        const result = await eyeDropper.open();

        selectedColor.value = result.sRGBHex;
    } catch (error) {
        if (error.name === 'AbortError') {
            errorMessage.value = 'Color selection was cancelled.';
        } else {
            errorMessage.value = 'Something went wrong.';
            console.error(error);
        }
    }
}
</script>

<template>
    <div class="container">
        <h1 class="title">EyeDropper API Demo</h1>

        <p class="mb-4">
            Click the button and select any color from your screen.
        </p>

        <button class="button is-primary mb-4" @click="pickColor">
            Pick a color
        </button>

        <div class="box" :style="{ backgroundColor: selectedColor, height: '100px' }"></div>

        <p class="has-text-weight-bold">
            Selected color: {{ selectedColor }}
        </p>

        <p v-if="errorMessage" class="has-text-danger">
            {{ errorMessage }}
        </p>
    </div>
</template>
