
<script setup>
import { ref, onUnmounted } from 'vue';

const query = ref('');
const emit = defineEmits(['search']);

function debounce(func, delay) {
    let timeout;

    function debounced(...args) {
        clearTimeout(timeout);

        timeout = setTimeout(() => {
            func(...args);
        }, delay);
    }

    debounced.cancel = () => clearTimeout(timeout);
    return debounced;
}

const debouncedSearch = debounce((value) => {
    emit('search', value);
}, 400);

function search() {
    debouncedSearch(query.value.trim());
}

onUnmounted(() => {
    debouncedSearch.cancel();
});
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
