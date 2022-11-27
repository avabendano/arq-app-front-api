<script setup>
import { onMounted, ref } from "vue";

const characters = ref([]);
const page = ref(1);

const fetchCharacters = async (page=1) => {
    let endpoint = `http://localhost:3001/api/characters`;

    endpoint += page > 1 ? `/${page}` : "";

    console.log(endpoint);
    try {
        const res = await fetch(endpoint, {
            mode: 'cors',
            headers: {
                'Content-Type': 'application/json',
                'Access-Control-Allow-Origin': 'cors'
            }
        });

        return await res.json();
    } catch (error) {
        console.log(error)
    }

    return null;
}

const loadMoreCharacters = async () => {
    const newCharacters = await fetchCharacters(page.value);
    characters.value = characters.value.concat(newCharacters);
    page.value += 1;
}

onMounted(async () => {
    const newCharacters = await fetchCharacters();
    characters.value = newCharacters;
    page.value += 1;
});

</script>

<template>
    <ul class="character-list">
        <li v-for="character in characters" :key="character.id">
            <div class="card character-card">
                <img :src="character.image" :alt="`Imagen de ${character.name}`">
                <div class="character-body">
                    <h2>{{ character.name }}</h2>
                    <p class="specie-type">{{ character.species == 'Human' ? "🙎🏻" : "👽" }}</p>
                </div>
            </div>
        </li>
    </ul>
    <p class="loading-text" v-if="characters.length == 0">Cargando...</p>
    <button v-if="characters.length != 0" @click="loadMoreCharacters">Cargar mas</button>
</template>

<style scoped>
.character-list {
    list-style: none;
}

.character-card {
    display: flex;
    flex-direction: row;
    height: 100px;
    width: min-content(500px, 80%);
    padding: 0;
    margin-top: 1rem;
}

.character-card:hover {
    box-shadow: 4px 3px 5px 0px rgba(0, 0, 0, 0.3);
    -webkit-box-shadow: 4px 3px 5px 0px rgba(0, 0, 0, 0.3);
    -moz-box-shadow: 4px 3px 5px 0px rgba(0, 0, 0, 0.3);
}

.character-card img {
    border-radius: 8px 0 0 8px;
}

.character-body {
    width: 100%;
    padding: 1rem 2rem;
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: space-between;
}

.character-body h2 {
    font-size: 1.3rem;
    margin-right: 1rem;
}

.character-body p {
    font-size: 1.5rem;
}
</style>