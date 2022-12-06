<script setup>
defineProps({
  cell_state: Boolean,
  x: Number,
  y: Number,
})

</script>

<template>
  <span class="cell" :class="{ alive: cell_state }"></span>
</template>

<style>
.cell {
  padding: 11px;
  margin: 0.8px;
  border: 1px solid #0e2a2c;
}

.alive {
  background-color: #43ce5d;
}
</style>