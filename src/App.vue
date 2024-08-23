<script setup lang="ts">
import HelloWorld from './components/Game.vue'
// import { TresCanvas, TresModel } from '@tresjs/core'
// import { OrbitControls } from '@tresjs/cientos'

// const modelUrl = 'https://raw.githubusercontent.com/KhronosGroup/glTF-Sample-Models/master/2.0/Duck/glTF/Duck.gltf'
</script>
<template>
  <div style="display: inline;">
    <!-- <h1>X vs O</h1> -->
    <img src="./assets/vue.svg" class="logo vue" alt="Girl" /><h2 style="display: inline;">Vs</h2>    
    <img src="./assets/vite.svg" class="logo vite" alt="Boy" />
    <a href="https://vuejs.org/" target="_blank">
    </a>
  </div>
  <HelloWorld />
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo.vite:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
