<script setup>
import SearchBar from '../components/SearchBar.vue';
import axios from 'axios';
import { ref } from 'vue';
import CharacterCard from '../components/CharacterCard.vue';
let characters = ref([]);
let info = ref({
    count: 0,
    pages: 0,
    next: null,
    prev: null,
});

await getCharacters('https://rickandmortyapi.com/api/character');

async function getCharacters(url) {
    try {
        const res = await axios.get(url);
        characters.value = res.data.results;
        info.value = res.data.info;
    } catch (error) {
        if (error.response?.status === 404) {
            characters.value = [];
            info.value = {
                count: 0,
                pages: 0,
                next: null,
                prev: null
            };
        } else {
            console.error(error);
        }
    }
}

async function next() {
    await getCharacters(info.value.next);
}

async function prev() {
    await getCharacters(info.value.prev);
}

async function searchCharacters(query) {
    const url = query
        ? `https://rickandmortyapi.com/api/character?name=${encodeURIComponent(query)}&page=1`
        : 'https://rickandmortyapi.com/api/character?page=1';

    await getCharacters(url);
}

</script>
<template>
    <SearchBar @search="searchCharacters" />
    <div class="is-flex is-justify-content-space-between mb-2">
        <button class="button is-primary" :disabled="!info.prev" @click="prev">Prev</button>
        <button class="button is-primary" :disabled="!info.next" @click="next">Next</button>
    </div>
    
<p v-if="characters.length === 0">
    No characters found
</p>

<div v-else class="columns is-multiline">
    <div
        class="column is-3"
        v-for="character in characters"
        :key="character.id"
    >
        <CharacterCard :character="character" />
    </div>
</div>
</template>