
<script setup>
import { ref } from 'vue';

const query = ref('');
const emit = defineEmits(['search']);

function search() {
    emit('search', query.value.trim());
}
</script>

<template>
    <div class="field mb-4">
        <div class="control">
            <input
                v-model="query"
                @input="search"
                class="input"
                type="text"
                placeholder="Search characters..."
            />
        </div>
    </div>
</template>
