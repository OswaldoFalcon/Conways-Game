<script setup>
import { ref } from 'vue'
const props = defineProps(['state'])
const size = ref(10)
const emit = defineEmits(['grid-size'])
emit('grid-size', size)


</script>

<template>
  <strong> Grid Size: </strong>
  <br>
  <select v-model="size" :disabled="state">
    <option disabled value="">Please select the grid size</option>
    <option :value=10 class="op">10 x 10</option>
    <option :value=20 class="op">20 x 20</option>
    <option :value=50 class="op">50 x 50</option>
    <option :value=100 class="op">100 x 100</option>
  </select>
</template>
<style>
.op {
  text-align: center;
}
</style>
