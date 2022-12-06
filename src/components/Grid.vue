<script>
import Cell from "./Cell.vue";
import Grid_size from './GridSize.vue';
import Cell_probability from './CellProbability.vue';
import { watch } from "vue";
export default {
  components: {
    Cell,
    Grid_size,
    Cell_probability,
  },
  data() {
    return {
      grid_size: 10,
      interval: null,
      run_state: false,
      prob: 10,
      grid: [],
    };
  },
  watch:
  {
    grid_size(newSize, oldSize) {
      this.setSize(newSize)
    }
  },
  methods: {
    randomizeGrid() {
      if (!this.run_state) {
        this.grid = [];
        for (let row = 0; row < Number(this.grid_size); row++) {
          this.grid[row] = [];
          for (let cell = 0; cell < Number(this.grid_size); cell++) {
            this.grid[row][cell] = Math.random() < this.prob * 0.01;
          }
        }
      }
    },
    toggleCellState(x, y) {
      this.grid[y][x] = !this.grid[y][x];
    },
    resetGrid() {
      this.grid = [];
      for (let row = 0; row < this.grid_size; row++) {
        this.grid[row] = [];
        for (let cell = 0; cell < this.grid_size; cell++) {
          this.grid[row][cell] = false;
        }
      }
    },
    nearAliveCells(x, y) {
      let count = 0;
      for (let row = y - 1; row <= y + 1; row++) {
        for (let col = x - 1; col <= x + 1; col++) {
          if (row >= 0 && row < Number(this.grid_size) && col >= 0 && col < Number(this.grid_size))
            if ((x != col || y != row) && this.grid[row][col]) {
              count++;
            }
        }
      }
      return count;
    },
    nextGrid() {
      let next_grid = [];
      for (let row = 0; row < Number(this.grid_size); row++) {
        next_grid[row] = [];
        for (let col = 0; col < Number(this.grid_size); col++) {
          let alive_n = this.nearAliveCells(col, row);
          if (this.grid[row][col] && (alive_n == 2 || alive_n == 3)) {
            next_grid[row][col] = true;
          } else if (!this.grid[row][col] && alive_n == 3) {
            next_grid[row][col] = true;
          } else {
            next_grid[row][col] = false;
          }
        }
      }
      this.grid = next_grid;
    },
    gameState() {
      if (this.run_state) {
        clearInterval(this.interval);
        this.run_state = false;
      } else {
        this.interval = setInterval(this.nextGrid, 500);
        this.run_state = true;
      }
    },
    setSize(new_size) {
      this.grid_size = new_size;
      this.resetGrid();
    },
  },
  created() {
    this.resetGrid();
  },
};

</script>
<template>
  <div class="game-container">
    <div class="control">
      <div>
        <Grid_size @grid-size="(msg) => grid_size = msg" :state="run_state" />
      </div>
      <Cell_probability @cell_prob="(msg) => prob = msg" :state="run_state" />
    </div>
    <div class="control">
      <button @click="gameState">
        {{ run_state ? "Stop" : "Play" }}
      </button>
      <button @click="randomizeGrid" :disabled="run_state">
        Random Grid
      </button>
    </div>

    <div class="grid-container">
      <div v-for="(row, y) in grid" :key="y" class="row">
        <Cell v-for="(cell, x) in row" :x="x" :y="y" :cell_state="cell" @click="toggleCellState(x, y)" />
      </div>
    </div>
  </div>


</template>
<style>
.row {
  display: flex;
  flex-direction: row;
  justify-content: center;
}

.control {
  display: flex;
  justify-content: space-around;
  padding-top: 10px;
  padding-bottom: 10px;
}
</style>
