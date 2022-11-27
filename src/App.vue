<script setup>
import HelloWorld from './components/HelloWorld.vue'
import CharacterListVue from './components/CharacterList.vue';

</script>

<template>
  <HelloWorld msg="Personajes de Rick and Morty" />

  <CharacterListVue />
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
}
</style>
