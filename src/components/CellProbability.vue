<script setup>
import { ref } from 'vue'
const props = defineProps(['state'])
const sliderValue = ref(30)
const emit = defineEmits(['cell_prob'])
emit('cell_prob', sliderValue)
</script>

<template>
  <div class="slidecontainer">
    <strong> Cell Probability  {{ sliderValue }}% </strong>
    <br>
    <input v-model="sliderValue" type="range" min="10" max="100" step="10" class="slider" v-if="!state">
  </div>
</template>
<style>

</style>
