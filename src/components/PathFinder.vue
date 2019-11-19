<template>
  <div class="pathfinder-wrapper">
    <template v-for="(row, rowIndex) in grid">
      <div :key="rowIndex">
        <template v-for="({
          row,
          col,
          isStart,
          isFinish,
          isWall,
          mouseIsPressed
        }, nodeIndex) in row">
          <Node
            :key="nodeIndex"
            :row="row"
            :col="col"
            :is-start="isStart"
            :is-finish="isFinish"
            :is-wall="isWall"
            :on-mouse-down="handleOnMouseDown"
            :on-mouse-enter="handleOnMouseEnter"
            :on-mouse-up="handleOnMouseUp"
          />
        </template>
      </div>
    </template>
  </div>
</template>

<script>
import Node from './Node'

const START_NODE_ROW = 10
const START_NODE_COL = 15
const FINISH_NODE_ROW = 10
const FINISH_NODE_COL = 35

export default {
  name: 'PathFinder',

  components: {
    Node
  },

  props: {
    msg: String
  },

  data: () => ({
    grid: []
  }),

  mounted() {
    // generate grid
    this.grid = this.getGrid()
  },

  methods: {
    getGrid() {
      const totalRows = 25
      const totalCols = 50
      const grid = []
      for (let row = 0; row < totalRows; row++) {
        const currentRow = []
        for (let col = 0; col < totalCols; col++) {
          currentRow.push(this.getNode(row, col))
        }
        grid.push(currentRow)
      }
      return grid
    },
    getNode(row, col) {
      return {
        row,
        col,
        prevNode: null,
        distance: Infinity,
        isStart: row === START_NODE_ROW && col === START_NODE_COL,
        isFinish: row === FINISH_NODE_ROW && col === FINISH_NODE_COL,
        isVisited: false,
        isWall: false,
        mouseIsPressed: false
      }
    },
    getNewGridWithWallToggled(grid, row, col) {

      const newGrid = grid.slice()
      // get clicked on node
      const node = newGrid[row][col]
      const newNode = {
        ...node,
        isWall: !node.isWall
      }
      
      // update clicked on node
      newGrid[row][col] = newNode

      return newGrid
    },
    handleOnMouseDown(row, col) {
      // get new grid will walls
      this.grid = this.getNewGridWithWallToggled(this.grid, row, col)
      this.mouseIsPressed = true
    },
    handleOnMouseEnter(row, col) {
      if (!this.mouseIsPressed) {
        return
      }
      this.grid = this.getNewGridWithWallToggled(this.grid, row, col)
    },
    handleOnMouseUp() {
      this.mouseIsPressed = false
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
.pathfinder-wrapper {}
</style>
