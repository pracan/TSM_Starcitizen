<script setup lang="ts">
import HelloWorld from './components/HelloWorld.vue';
import TSS from './components/Traveling_Salesman_Starcitizen.vue';
import data_writing from './components/data_writing.vue';
import TSM_Trigo from './components/TSM_trigo.vue';
import TSM_Haversine from './components/TSM_haversine.vue';
import cards_traject from './components/cards_traject.vue';
</script>

<template>
  <div>
    <a href="https://vitejs.dev" target="_blank">
      <img src="/vite.svg" class="logo" alt="Vite logo" />
    </a>
    <a href="https://vuejs.org/" target="_blank">
      <img src="./assets/vue.svg" class="logo vue" alt="Vue logo" />
    </a>
  </div>
  <!-- <HelloWorld msg="Vite + Vue" /> -->
  <!-- <data_writing /> -->
  <TSM_Trigo />
  <TSM_Haversine />
  <TSS />
  <cards_traject />
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
