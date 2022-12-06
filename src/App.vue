<script setup>
import { ref } from 'vue'
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://vuejs.org/api/sfc-script-setup.html#script-setup
// import grid_size from './components/GridSize.vue'
// import cell_slider from './components/CellProbability.vue'
import grid from './components/Grid.vue'


const size = ref(null)

</script>

<template>
  <div>
    <h1>Conway's Game of Lifes </h1>
    <grid/>
  </div>
</template>
