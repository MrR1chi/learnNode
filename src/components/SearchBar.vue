
<script setup>
import { ref } from 'vue';

const query = ref('');
const emit = defineEmits(['search']);

function search() {
    emit('search', query.value.trim());
}
</script>

<template>
    <form @submit.prevent="search">
        <input
            v-model="query"
            type="text"
            placeholder="Search characters..."
            class="input"
        />

        <button type="submit" class="button is-primary">
            Search
        </button>
    </form>
</template>
